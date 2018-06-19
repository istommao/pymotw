# compileall Byte-compile Source Files

## Compiling One Directory

```python
# compileall_compile_dir.py
import compileall
import glob


def show(title):
    print(title)
    for filename in glob.glob('examples/**',
                              recursive=True):
        print('  {}'.format(filename))
    print()


show('Before')

compileall.compile_dir('examples')

show('\nAfter')
```

## Ignoring Files

```python
# compileall_exclude_dirs.py
import compileall
import re

compileall.compile_dir(
    'examples',
    rx=re.compile(r'/subdir'),
)
```

```python
# compileall_recursion_depth.py
import compileall
import re

compileall.compile_dir(
    'examples',
    maxlevels=0,
)
```

## Compiling sys.path

```python
# compileall_path.py
import compileall
import sys

sys.path[:] = ['examples', 'notthere']
print('sys.path =', sys.path)
compileall.compile_path()
```

## Compiling Individual Files

```python
# compileall_compile_file.py
import compileall
import glob


def show(title):
    print(title)
    for filename in glob.glob('examples/**',
                              recursive=True):
        print('  {}'.format(filename))
    print()


show('Before')

compileall.compile_file('examples/a.py')

show('\nAfter')
```
