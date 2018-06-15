# uuid 全局唯一标识符

## UUID 1 - IEEE 802 MAC Address

```python
# uuid_getnode.py
import uuid

print(hex(uuid.getnode()))
```

```python
# uuid_uuid1.py
import uuid

u = uuid.uuid1()

print(u)
print(type(u))
print('bytes   :', repr(u.bytes))
print('hex     :', u.hex)
print('int     :', u.int)
print('urn     :', u.urn)
print('variant :', u.variant)
print('version :', u.version)
print('fields  :', u.fields)
print('  time_low            : ', u.time_low)
print('  time_mid            : ', u.time_mid)
print('  time_hi_version     : ', u.time_hi_version)
print('  clock_seq_hi_variant: ', u.clock_seq_hi_variant)
print('  clock_seq_low       : ', u.clock_seq_low)
print('  node                : ', u.node)
print('  time                : ', u.time)
print('  clock_seq           : ', u.clock_seq)
```

```python
# uuid_uuid1_repeat.py
import uuid

for i in range(3):
    print(uuid.uuid1())
```

```python
# uuid_uuid1_othermac.py
import uuid

for node in [0x1ec200d9e0, 0x1e5274040e]:
    print(uuid.uuid1(node), hex(node))
```

## UUID 3 and 5 - Name-Based Values

```python
# uuid_uuid3_uuid5.py
import uuid

hostnames = ['www.doughellmann.com', 'blog.doughellmann.com']

for name in hostnames:
    print(name)
    print('  MD5   :', uuid.uuid3(uuid.NAMESPACE_DNS, name))
    print('  SHA-1 :', uuid.uuid5(uuid.NAMESPACE_DNS, name))
    print()
```

```python
# uuid_uuid3_repeat.py
import uuid

namespace_types = sorted(
    n
    for n in dir(uuid)
    if n.startswith('NAMESPACE_')
)
name = 'www.doughellmann.com'

for namespace_type in namespace_types:
    print(namespace_type)
    namespace_uuid = getattr(uuid, namespace_type)
    print(' ', uuid.uuid3(namespace_uuid, name))
    print(' ', uuid.uuid3(namespace_uuid, name))
    print()
```

## UUID 4 - Random Values

```python
# uuid_uuid4.py
import uuid

for i in range(3):
    print(uuid.uuid4())
```

## 处理UUID对象

```python
# uuid_uuid_objects.py
import uuid


def show(msg, l):
    print(msg)
    for v in l:
        print(' ', v)
    print()


input_values = [
    'urn:uuid:f2f84497-b3bf-493a-bba9-7c68e6def80b',
    '{417a5ebb-01f7-4ed5-aeac-3d56cd5037b0}',
    '2115773a-5bf1-11dd-ab48-001ec200d9e0',
]

show('input_values', input_values)

uuids = [uuid.UUID(s) for s in input_values]
show('converted to uuids', uuids)

uuids.sort()
show('sorted', uuids)
```
