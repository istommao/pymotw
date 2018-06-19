# platform System Version Information

## Interpreter

```python
# platform_python.py
import platform

print('Version      :', platform.python_version())
print('Version tuple:', platform.python_version_tuple())
print('Compiler     :', platform.python_compiler())
print('Build        :', platform.python_build())
```


`OS X:`

> $ python3 platform_python.py
>
> Version      : 3.6.4
> Version tuple: ('3', '6', '4')
> Compiler     : GCC 4.2.1 (Apple Inc. build 5666) (dot 3)
> Build        : ('v3.6.4:d48ecebad5', 'Dec 18 2017 21:07:28')

`Linux:`

> $ python3 platform_python.py
>
> Version      : 3.5.2
> Version tuple: ('3', '5', '2')
> Compiler     : GCC 4.8.4
> Build        : ('default', 'Jul 17 2016 00:00:00')

`Windows:`
> C:\>Desktop\platform_python.py
>
> Version      : 3.5.1
> Version tuple: ('3', '5', '1')
> Compiler     : MSC v.1900 64 bit (AMD64)
> Build        : ('v3.5.1:37a07cee5969', 'Dec  6 2015 01:54:25')

## Platform

```python
# platform_platform.py
import platform

print('Normal :', platform.platform())
print('Aliased:', platform.platform(aliased=True))
print('Terse  :', platform.platform(terse=True))
```

## Operating System and Hardware Info

```python
# platform_os_info.py
import platform

print('uname:', platform.uname())

print()
print('system   :', platform.system())
print('node     :', platform.node())
print('release  :', platform.release())
print('version  :', platform.version())
print('machine  :', platform.machine())
print('processor:', platform.processor())
```

## Executable Architecture

```python
# platform_architecture.py
import platform

print('interpreter:', platform.architecture())
print('/bin/ls    :', platform.architecture('/bin/ls'))
```
