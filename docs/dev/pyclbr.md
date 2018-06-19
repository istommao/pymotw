# pyclbr Class Browser

```python
# pyclbr_example.py
"""Example source for pyclbr.
"""


class Base:
    """This is the base class.
    """

    def method1(self):
        return


class Sub1(Base):
    """This is the first subclass.
    """


class Sub2(Base):
    """This is the second subclass.
    """


class Mixin:
    """A mixin class.
    """

    def method2(self):
        return


class MixinUser(Sub2, Mixin):
    """Overrides method1 and method2
    """

    def method1(self):
        return

    def method2(self):
        return

    def method3(self):
        return


def my_function():
    """Stand-alone function.
    """
    return
```

```python
# pyclbr_readmodule.py
import pyclbr
import os
from operator import itemgetter


def show_class(name, class_data):
    print('Class:', name)
    filename = os.path.basename(class_data.file)
    print('  File: {0} [{1}]'.format(
        filename, class_data.lineno))
    show_super_classes(name, class_data)
    show_methods(name, class_data)
    print()


def show_methods(class_name, class_data):
    for name, lineno in sorted(class_data.methods.items(),
                               key=itemgetter(1)):
        print('  Method: {0} [{1}]'.format(name, lineno))


def show_super_classes(name, class_data):
    super_class_names = []
    for super_class in class_data.super:
        if super_class == 'object':
            continue
        if isinstance(super_class, str):
            super_class_names.append(super_class)
        else:
            super_class_names.append(super_class.name)
    if super_class_names:
        print('  Super classes:', super_class_names)


example_data = pyclbr.readmodule('pyclbr_example')

for name, class_data in sorted(example_data.items(),
                               key=lambda x: x[1].lineno):
    show_class(name, class_data)
```

## Scanning for Functions

```python
# pyclbr_readmodule_ex.py
import pyclbr
import os
from operator import itemgetter

example_data = pyclbr.readmodule_ex('pyclbr_example')

for name, data in sorted(example_data.items(),
                         key=lambda x: x[1].lineno):
    if isinstance(data, pyclbr.Function):
        print('Function: {0} [{1}]'.format(name, data.lineno))
```
