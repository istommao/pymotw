# gettext 信息目录

## Creating Message Catalogs from Source Code

```python
# gettext_example.py
import gettext

# Set up message catalog access
t = gettext.translation(
    'example_domain', 'locale',
    fallback=True,
)
_ = t.gettext

print(_('This message is in the script.'))
```

## Finding Message Catalogs at Runtime

```python
# gettext_find.py
import gettext

catalogs = gettext.find('example', 'locale', all=True)
print('Catalogs:', catalogs)
```

## Plural Values

```python
# gettext_plural.py
from gettext import translation
import sys

t = translation('plural', 'locale', fallback=False)
num = int(sys.argv[1])
msg = t.ngettext('{num} means singular.',
                 '{num} means plural.',
                 num)

# Still need to add the values to the message ourself.
print(msg.format(num=num))
```

## Application vs. Module Localization

```python
# gettext_app_builtin.py
import gettext

gettext.install(
    'example',
    'locale',
    names=['ngettext'],
)

print(_('This message is in the script.'))
```

## Module Localization

```python
# gettext_module_global.py
import gettext

t = gettext.translation(
    'example',
    'locale',
    fallback=False,
)
_ = t.gettext
ngettext = t.ngettext

print(_('This message is in the script.'))
```
