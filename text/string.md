# string — 文字常量和模板

> 目的: 包含处理文本的常量和类

The string module dates from the earliest versions of Python. Many of the functions previously implemented in this module have been moved to methods of str objects. The string module retains several useful constants and classes for working with str objects. This discussion will concentrate on them.

## 函数
`capwords() 函数使字符串所有字首字母变成大写`
```python
import string

s = 'The quick brown fox jumped over the lazy dog.'

print(s)
print(string.capwords(s))
```

## 模版

```python
# string_template.py
import string

values = {'var': 'foo'}

t = string.Template("""
Variable        : $var
Escape          : $$
Variable in text: ${var}iable
""")

print('TEMPLATE:', t.substitute(values))

s = """
Variable        : %(var)s
Escape          : %%
Variable in text: %(var)siable
"""

print('INTERPOLATION:', s % values)

s = """
Variable        : {var}
Escape          : {{}}
Variable in text: {var}iable
"""

print('FORMAT:', s.format(**values))
```

```python
# string_template_missing.py
import string

values = {'var': 'foo'}

t = string.Template("$var is here but $missing is not provided")

try:
    print('substitute()     :', t.substitute(values))
except KeyError as err:
    print('ERROR:', str(err))

print('safe_substitute():', t.safe_substitute(values))
```

## 高级模版
```python
import string


class MyTemplate(string.Template):
    delimiter = '%'
    idpattern = '[a-z]+_[a-z]+'


template_text = '''
  Delimiter : %%
  Replaced  : %with_underscore
  Ignored   : %notunderscored
'''

d = {
    'with_underscore': 'replaced',
    'notunderscored': 'not replaced',
}

t = MyTemplate(template_text)
print('Modified ID pattern:')
print(t.safe_substitute(d))
```

```python
# string_template_defaultpattern.py
import string

t = string.Template('$var')
print(t.pattern.pattern)
```

```python
# string_template_newsyntax.py

import re
import string


class MyTemplate(string.Template):
    delimiter = '{{'
    pattern = r'''
    \{\{(?:
    (?P<escaped>\{\{)|
    (?P<named>[_a-z][_a-z0-9]*)\}\}|
    (?P<braced>[_a-z][_a-z0-9]*)\}\}|
    (?P<invalid>)
    )
    '''


t = MyTemplate('''
{{{{
{{var}}
''')

print('MATCHES:', t.pattern.findall(t.template))
print('SUBSTITUTED:', t.safe_substitute(var='replacement'))
```

## 格式化

## 常量

`strting模块包含一些与ASCII和数字相关的字符集常量`

```python
import inspect
import string


def is_str(value):
    return isinstance(value, str)


for name, value in inspect.getmembers(string, is_str):
    if name.startswith('_'):
        continue
    print('%s=%r\n' % (name, value))
```

```python

ascii_letters = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'

ascii_lowercase = 'abcdefghijklmnopqrstuvwxyz'

ascii_uppercase = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'

digits = '0123456789'

hexdigits = '0123456789abcdefABCDEF'

octdigits = '01234567'

printable = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~ \t\n\r\x0b\x0c'

punctuation = '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'

whitespace = ' \t\n\r\x0b\x0c'
```
