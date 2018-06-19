# inspect Inspect Live Objects

## Example Module

```python
# example.py
# This comment appears first
# and spans 2 lines.

# This comment does not show up in the output of getcomments().

"""Sample file to serve as the basis for inspect examples.
"""


def module_level_function(arg1, arg2='default', *args, **kwargs):
    """This function is declared in the module."""
    local_variable = arg1 * 2
    return local_variable


class A(object):
    """The A class."""

    def __init__(self, name):
        self.name = name

    def get_name(self):
        "Returns the name of the instance."
        return self.name


instance_of_a = A('sample_instance')


class B(A):
    """This is the B class.
    It is derived from A.
    """

    # This method is not part of A.
    def do_something(self):
        """Does some work"""

    def get_name(self):
        "Overrides version from A"
        return 'B(' + self.name + ')'
```

## Inspecting Modules

```python
# inspect_getmembers_module.py
import inspect

import example

for name, data in inspect.getmembers(example):
    if name.startswith('__'):
        continue
    print('{} : {!r}'.format(name, data))
```


## Inspecting Classes

```python
# inspect_getmembers_class.py
import inspect
from pprint import pprint

import example

pprint(inspect.getmembers(example.A), width=65)
```


```python
# inspect_getmembers_class_methods.py
import inspect
from pprint import pprint

import example

pprint(inspect.getmembers(example.A, inspect.isfunction))
```

```python
# inspect_getmembers_class_methods_b.py
import inspect
from pprint import pprint

import example

pprint(inspect.getmembers(example.B, inspect.isfunction))
```

## Inspecting Instances

```python
inspect_getmembers_instance.py
import inspect
from pprint import pprint

import example

a = example.A(name='inspect_getmembers')
pprint(inspect.getmembers(a, inspect.ismethod))
```

## Documentation Strings

```python
# inspect_getdoc.py
import inspect
import example

print('B.__doc__:')
print(example.B.__doc__)
print()
print('getdoc(B):')
print(inspect.getdoc(example.B))
```

```python
# inspect_getcomments_method.py
import inspect
import example

print(inspect.getcomments(example.B.do_something))
```

```python
# inspect_getcomments_module.py
import inspect
import example

print(inspect.getcomments(example))
```

```python
# inspect_getsource_class.py
import inspect
import example

print(inspect.getsource(example.A))
```

```python
# inspect_getsource_method.py
import inspect
import example

print(inspect.getsource(example.A.get_name))
```

```python
# inspect_getsourcelines_method.py
import inspect
import pprint
import example

pprint.pprint(inspect.getsourcelines(example.A.get_name))
```

## Method and Function Signatures

```python
# inspect_signature_function.py
import inspect
import example

sig = inspect.signature(example.module_level_function)
print('module_level_function{}'.format(sig))

print('\nParameter details:')
for name, param in sig.parameters.items():
    if param.kind == inspect.Parameter.POSITIONAL_ONLY:
        print('  {} (positional-only)'.format(name))
    elif param.kind == inspect.Parameter.POSITIONAL_OR_KEYWORD:
        if param.default != inspect.Parameter.empty:
            print('  {}={!r}'.format(name, param.default))
        else:
            print('  {}'.format(name))
    elif param.kind == inspect.Parameter.VAR_POSITIONAL:
        print('  *{}'.format(name))
    elif param.kind == inspect.Parameter.KEYWORD_ONLY:
        if param.default != inspect.Parameter.empty:
            print('  {}={!r} (keyword-only)'.format(
                name, param.default))
        else:
            print('  {} (keyword-only)'.format(name))
    elif param.kind == inspect.Parameter.VAR_KEYWORD:
        print('  **{}'.format(name))
```

```python
# inspect_signature_bind.py
import inspect
import example

sig = inspect.signature(example.module_level_function)

bound = sig.bind(
    'this is arg1',
    'this is arg2',
    'this is an extra positional argument',
    extra_named_arg='value',
)

print('Arguments:')
for name, value in bound.arguments.items():
    print('{} = {!r}'.format(name, value))

print('\nCalling:')
print(example.module_level_function(*bound.args, **bound.kwargs))
```

```python
# inspect_signature_bind_partial.py
import inspect
import example

sig = inspect.signature(example.module_level_function)

partial = sig.bind_partial(
    'this is arg1',
)

print('Without defaults:')
for name, value in partial.arguments.items():
    print('{} = {!r}'.format(name, value))

print('\nWith defaults:')
partial.apply_defaults()
for name, value in partial.arguments.items():
    print('{} = {!r}'.format(name, value))
```

## Class Hierarchies

```python
# inspect_getclasstree.py
import inspect
import example


class C(example.B):
    pass


class D(C, example.A):
    pass


def print_class_tree(tree, indent=-1):
    if isinstance(tree, list):
        for node in tree:
            print_class_tree(node, indent + 1)
    else:
        print('  ' * indent, tree[0].__name__)
    return


if __name__ == '__main__':
    print('A, B, C, D:')
    print_class_tree(inspect.getclasstree(
        [example.A, example.B, C, D])
    )
```

```python
# inspect_getclasstree_unique.py
import inspect
import example
from inspect_getclasstree import *

print_class_tree(inspect.getclasstree(
    [example.A, example.B, C, D],
    unique=True,
))
```

## Method Resolution Order

```python
# inspect_getmro.py
import inspect
import example


class C(object):
    pass


class C_First(C, example.B):
    pass


class B_First(example.B, C):
    pass


print('B_First:')
for c in inspect.getmro(B_First):
    print('  {}'.format(c.__name__))
print()
print('C_First:')
for c in inspect.getmro(C_First):
    print('  {}'.format(c.__name__))
```

## The Stack and Frames

```python
# inspect_currentframe.py
import inspect
import pprint


def recurse(limit, keyword='default', *, kwonly='must be named'):
    local_variable = '.' * limit
    keyword = 'changed value of argument'
    frame = inspect.currentframe()
    print('line {} of {}'.format(frame.f_lineno,
                                 frame.f_code.co_filename))
    print('locals:')
    pprint.pprint(frame.f_locals)
    print()
    if limit <= 0:
        return
    recurse(limit - 1)
    return local_variable

if __name__ == '__main__':
    recurse(2)
```


```python
# inspect_stack.py
import inspect
import pprint


def show_stack():
    for level in inspect.stack():
        print('{}[{}]\n  -> {}'.format(
            level.frame.f_code.co_filename,
            level.lineno,
            level.code_context[level.index].strip(),
        ))
        pprint.pprint(level.frame.f_locals)
        print()


def recurse(limit):
    local_variable = '.' * limit
    if limit <= 0:
        show_stack()
        return
    recurse(limit - 1)
    return local_variable


if __name__ == '__main__':
    recurse(2)
```
