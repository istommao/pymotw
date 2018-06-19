# atexit 程序关闭回调

## 注册退出回调

```python
# atexit_simple.py
import atexit


def all_done():
    print('all_done()')


print('Registering')
atexit.register(all_done)
print('Registered')
```

```python
# atexit_multiple.py
import atexit


def my_cleanup(name):
    print('my_cleanup({})'.format(name))


atexit.register(my_cleanup, 'first')
atexit.register(my_cleanup, 'second')
atexit.register(my_cleanup, 'third')
```

## 装饰器语法

```python
# atexit_decorator.py
import atexit


@atexit.register
def all_done():
    print('all_done()')


print('starting main program')
```

## 取消回调

```python
# atexit_unregister.py
import atexit


def my_cleanup(name):
    print('my_cleanup({})'.format(name))


atexit.register(my_cleanup, 'first')
atexit.register(my_cleanup, 'second')
atexit.register(my_cleanup, 'third')

atexit.unregister(my_cleanup)
```

## When Are atexit Callbacks Not Called?

```python
# atexit_signal_parent.py
import os
import signal
import subprocess
import time

proc = subprocess.Popen('./atexit_signal_child.py')
print('PARENT: Pausing before sending signal...')
time.sleep(1)
print('PARENT: Signaling child')
os.kill(proc.pid, signal.SIGTERM)
```

```python
# atexit_signal_child.py
import atexit
import time
import sys


def not_called():
    print('CHILD: atexit handler should not have been called')


print('CHILD: Registering atexit handler')
sys.stdout.flush()
atexit.register(not_called)

print('CHILD: Pausing to wait for signal')
sys.stdout.flush()
time.sleep(5)
```

```python
# atexit_os_exit.py
import atexit
import os


def not_called():
    print('This should not be called')


print('Registering')
atexit.register(not_called)
print('Registered')

print('Exiting...')
os._exit(0)
```

```python
# atexit_sys_exit.py
import atexit
import sys


def all_done():
    print('all_done()')


print('Registering')
atexit.register(all_done)
print('Registered')

print('Exiting...')
sys.exit()
```

## 处理异常

```python
# atexit_exception.py
import atexit


def exit_with_exception(message):
    raise RuntimeError(message)


atexit.register(exit_with_exception, 'Registered first')
atexit.register(exit_with_exception, 'Registered second')
```
