# importlib Python’s Import Mechanism

## Example Package

```python
# example/__init__.py
print('Importing example package')
```

```python
# example/submodule.py
print('Importing submodule')
```

## Module Types

```python
# importlib_suffixes.py
import importlib.machinery

SUFFIXES = [
    ('Source:', importlib.machinery.SOURCE_SUFFIXES),
    ('Debug:',
     importlib.machinery.DEBUG_BYTECODE_SUFFIXES),
    ('Optimized:',
     importlib.machinery.OPTIMIZED_BYTECODE_SUFFIXES),
    ('Bytecode:', importlib.machinery.BYTECODE_SUFFIXES),
    ('Extension:', importlib.machinery.EXTENSION_SUFFIXES),
]


def main():
    tmpl = '{:<10}  {}'
    for name, value in SUFFIXES:
        print(tmpl.format(name, value))


if __name__ == '__main__':
    main()
```


## Importing Modules

```python
# importlib_import_module.py
import importlib


m1 = importlib.import_module('example.submodule')
print(m1)

m2 = importlib.import_module('.submodule', package='example')
print(m2)

print(m1 is m2)
```

```python
# importlib_import_module_error.py
import importlib


try:
    importlib.import_module('example.nosuchmodule')
except ImportError as err:
    print('Error:', err)
```


```python
# importlib_reload.py
import importlib


m1 = importlib.import_module('example.submodule')
print(m1)

m2 = importlib.reload(m1)
print(m1 is m2)
```

## Loaders

```python
# importlib_find_loader.py
import importlib

loader = importlib.find_loader('example')
print('Loader:', loader)

m = loader.load_module()
print('Module:', m)
```

```python
# importlib_submodule.py
import importlib

pkg_loader = importlib.find_loader('example')
pkg = pkg_loader.load_module()

loader = importlib.find_loader('submodule', pkg.__path__)
print('Loader:', loader)

m = loader.load_module()
print('Module:', m)
```
