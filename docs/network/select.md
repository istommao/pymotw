# select Wait for I/O Efficiently

## 使用select()

```python
# select_echo_server.py
import select
import socket
import sys
import queue

# Create a TCP/IP socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setblocking(0)

# Bind the socket to the port
server_address = ('localhost', 10000)
print('starting up on {} port {}'.format(*server_address),
      file=sys.stderr)
server.bind(server_address)

# Listen for incoming connections
server.listen(5)

# Sockets from which we expect to read
inputs = [server]

# Sockets to which we expect to write
outputs = []

# Outgoing message queues (socket:Queue)
message_queues = {}

while inputs:

    # Wait for at least one of the sockets to be
    # ready for processing
    print('waiting for the next event', file=sys.stderr)
    readable, writable, exceptional = select.select(inputs,
                                                    outputs,
                                                    inputs)

    # Handle inputs
    for s in readable:

        if s is server:
            # A "readable" socket is ready to accept a connection
            connection, client_address = s.accept()
            print('  connection from', client_address,
                  file=sys.stderr)
            connection.setblocking(0)
            inputs.append(connection)

            # Give the connection a queue for data
            # we want to send
            message_queues[connection] = queue.Queue()
        else:
            data = s.recv(1024)
            if data:
                # A readable client socket has data
                print('  received {!r} from {}'.format(
                    data, s.getpeername()), file=sys.stderr,
                )
                message_queues[s].put(data)
                # Add output channel for response
                if s not in outputs:
                    outputs.append(s)
            else:
                # Interpret empty result as closed connection
                print('  closing', client_address,
                      file=sys.stderr)
                # Stop listening for input on the connection
                if s in outputs:
                    outputs.remove(s)
                inputs.remove(s)
                s.close()

                # Remove message queue
                del message_queues[s]

    # Handle outputs
    for s in writable:
        try:
            next_msg = message_queues[s].get_nowait()
        except queue.Empty:
            # No messages waiting so stop checking
            # for writability.
            print('  ', s.getpeername(), 'queue empty',
                  file=sys.stderr)
            outputs.remove(s)
        else:
            print('  sending {!r} to {}'.format(next_msg,
                                                s.getpeername()),
                  file=sys.stderr)
            s.send(next_msg)
    # Handle "exceptional conditions"
    for s in exceptional:
        print('exception condition on', s.getpeername(),
              file=sys.stderr)
        # Stop listening for input on the connection
        inputs.remove(s)
        if s in outputs:
            outputs.remove(s)
        s.close()

        # Remove message queue
        del message_queues[s]

```

```python
# select_echo_multiclient.py
import socket
import sys

messages = [
    'This is the message. ',
    'It will be sent ',
    'in parts.',
]
server_address = ('localhost', 10000)

# Create a TCP/IP socket
socks = [
    socket.socket(socket.AF_INET, socket.SOCK_STREAM),
    socket.socket(socket.AF_INET, socket.SOCK_STREAM),
]

# Connect the socket to the port where the server is listening
print('connecting to {} port {}'.format(*server_address),
      file=sys.stderr)
for s in socks:
    s.connect(server_address)

for message in messages:
    outgoing_data = message.encode()

    # Send messages on both sockets
    for s in socks:
        print('{}: sending {!r}'.format(s.getsockname(),
                                        outgoing_data),
              file=sys.stderr)
        s.send(outgoing_data)

    # Read responses on both sockets
    for s in socks:
        data = s.recv(1024)
        print('{}: received {!r}'.format(s.getsockname(),
                                         data),
              file=sys.stderr)
        if not data:
            print('closing socket', s.getsockname(),
                  file=sys.stderr)
            s.close()
```

## 非阻塞IO超时

```python
select_echo_slow_client.py
import socket
import sys
import time

# Create a TCP/IP socket
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect the socket to the port where the server is listening
server_address = ('localhost', 10000)
print('connecting to {} port {}'.format(*server_address),
      file=sys.stderr)
sock.connect(server_address)

time.sleep(1)

messages = [
    'Part one of the message.',
    'Part two of the message.',
]
amount_expected = len(''.join(messages))

try:

    # Send data
    for message in messages:
        data = message.encode()
        print('sending {!r}'.format(data), file=sys.stderr)
        sock.sendall(data)
        time.sleep(1.5)

    # Look for the response
    amount_received = 0

    while amount_received < amount_expected:
        data = sock.recv(16)
        amount_received += len(data)
        print('received {!r}'.format(data), file=sys.stderr)

finally:
    print('closing socket', file=sys.stderr)
    sock.close()
```

## 使用 poll()

```python
# select_poll_echo_server.py
import select
import socket
import sys
import queue

# Create a TCP/IP socket
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.setblocking(0)

# Bind the socket to the port
server_address = ('localhost', 10000)
print('starting up on {} port {}'.format(*server_address),
      file=sys.stderr)
server.bind(server_address)

# Listen for incoming connections
server.listen(5)

# Keep up with the queues of outgoing messages
message_queues = {}

# Do not block forever (milliseconds)
TIMEOUT = 1000

# Commonly used flag sets
READ_ONLY = (
    select.POLLIN |
    select.POLLPRI |
    select.POLLHUP |
    select.POLLERR
)
READ_WRITE = READ_ONLY | select.POLLOUT

# Set up the poller
poller = select.poll()
poller.register(server, READ_ONLY)

# Map file descriptors to socket objects
fd_to_socket = {
    server.fileno(): server,
}

while True:

    # Wait for at least one of the sockets to be
    # ready for processing
    print('waiting for the next event', file=sys.stderr)
    events = poller.poll(TIMEOUT)

    for fd, flag in events:

        # Retrieve the actual socket from its file descriptor
        s = fd_to_socket[fd]

        # Handle inputs
        if flag & (select.POLLIN | select.POLLPRI):

            if s is server:
                # A readable socket is ready
                # to accept a connection
                connection, client_address = s.accept()
                print('  connection', client_address,
                      file=sys.stderr)
                connection.setblocking(0)
                fd_to_socket[connection.fileno()] = connection
                poller.register(connection, READ_ONLY)

                # Give the connection a queue for data to send
                message_queues[connection] = queue.Queue()

            else:
                data = s.recv(1024)

                if data:
                    # A readable client socket has data
                    print('  received {!r} from {}'.format(
                        data, s.getpeername()), file=sys.stderr,
                    )
                    message_queues[s].put(data)
                    # Add output channel for response
                    poller.modify(s, READ_WRITE)
                else:
                    # Interpret empty result as closed connection
                    print('  closing', client_address,
                          file=sys.stderr)
                    # Stop listening for input on the connection
                    poller.unregister(s)
                    s.close()

                    # Remove message queue
                    del message_queues[s]
        elif flag & select.POLLHUP:
            # Client hung up
            print('  closing', client_address, '(HUP)',
                  file=sys.stderr)
            # Stop listening for input on the connection
            poller.unregister(s)
            s.close()
        elif flag & select.POLLOUT:
            # Socket is ready to send data,
            # if there is any to send.
            try:
                next_msg = message_queues[s].get_nowait()
            except queue.Empty:
                # No messages waiting so stop checking
                print(s.getpeername(), 'queue empty',
                      file=sys.stderr)
                poller.modify(s, READ_ONLY)
            else:
                print('  sending {!r} to {}'.format(
                    next_msg, s.getpeername()), file=sys.stderr,
                )
                s.send(next_msg)
        elif flag & select.POLLERR:
            print('  exception on', s.getpeername(),
                  file=sys.stderr)
            # Stop listening for input on the connection
            poller.unregister(s)
            s.close()

            # Remove message queue
            del message_queues[s]

