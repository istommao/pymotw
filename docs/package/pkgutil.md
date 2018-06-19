# pkgutil Package Utilities

## Package Import Paths

```python
# demopkg1/__init__.py
import pkgutil
import pprint

print('demopkg1.__path__ before:')
pprint.pprint(__path__)
print()

__path__ = pkgutil.extend_path(__path__, __name__)

print('demopkg1.__path__ after:')
pprint.pprint(__path__)
print()
```

```python
# pkgutil_extend_path.py
import demopkg1
print('demopkg1           :', demopkg1.__file__)

try:
    import demopkg1.shared
except Exception as err:
    print('demopkg1.shared    : Not found ({})'.format(err))
else:
    print('demopkg1.shared    :', demopkg1.shared.__file__)

try:
    import demopkg1.not_shared
except Exception as err:
    print('demopkg1.not_shared: Not found ({})'.format(err))
else:
    print('demopkg1.not_shared:', demopkg1.not_shared.__file__)
```


## Development Versions of Packages


```python
# demopkg2/overloaded.py

def func():
    print('This is the installed version of func().')
```


```python
# demopkg2/__init__.py
import pkgutil

__path__ = pkgutil.extend_path(__path__, __name__)
__path__.reverse()
```

```python
# pkgutil_devel.py
import demopkg2
print('demopkg2           :', demopkg2.__file__)

import demopkg2.overloaded
print('demopkg2.overloaded:', demopkg2.overloaded.__file__)

print()
demopkg2.overloaded.func()
```

```python
# develop/demopkg2/overloaded.py

def func():
    print('This is the development version of func().')
```

## Managing Paths with PKG Files

```python
# pkgutil_os_specific.py
import demopkg1
print('demopkg1:', demopkg1.__file__)

import demopkg1.shared
print('demopkg1.shared:', demopkg1.shared.__file__)

import demopkg1.not_shared
print('demopkg1.not_shared:', demopkg1.not_shared.__file__)
```

```shell
# with_os.sh
#!/bin/sh

export PYTHONPATH=os_${1}
echo "PYTHONPATH=$PYTHONPATH"
echo

python3 pkgutil_os_specific.py
```

## Nested Packages


```python
# nested/__init__.py
import pkgutil

__path__ = pkgutil.extend_path(__path__, __name__)
__path__.reverse()
```


```python
# pkgutil_nested.py
import nested

import nested.shallow
print('nested.shallow:', nested.shallow.__file__)
nested.shallow.func()

print()
import nested.second.deep
print('nested.second.deep:', nested.second.deep.__file__)
nested.second.deep.func()
```

## Package Data

```html
# pkgwithdata/templates/base.html
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML//EN">
<html> <head>
<title>PyMOTW Template</title>
</head>

<body>
<h1>Example Template</h1>

<p>This is a sample data file.</p>

</body>
</html>
```

```python
# pkgutil_get_data.py
import pkgutil

template = pkgutil.get_data('pkgwithdata', 'templates/base.html')
print(template.decode('utf-8'))
```

```python
# pkgutil_get_data_zip.py
import pkgutil
import zipfile
import sys

# Create a ZIP file with code from the current directory
# and the template using a name that does not appear on the
# local filesystem.
with zipfile.PyZipFile('pkgwithdatainzip.zip', mode='w') as zf:
    zf.writepy('.')
    zf.write('pkgwithdata/templates/base.html',
             'pkgwithdata/templates/fromzip.html',
             )

# Add the ZIP file to the import path.
sys.path.insert(0, 'pkgwithdatainzip.zip')

# Import pkgwithdata to show that it comes from the ZIP archive.
import pkgwithdata
print('Loading pkgwithdata from', pkgwithdata.__file__)

# Print the template body
print('\nTemplate:')
data = pkgutil.get_data('pkgwithdata', 'templates/fromzip.html')
print(data.decode('utf-8'))
```
