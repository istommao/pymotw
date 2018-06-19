# getpass 安全密码提示

## 例子

```python
# getpass_defaults.py
import getpass

try:
    p = getpass.getpass()
except Exception as err:
    print('ERROR:', err)
else:
    print('You entered:', p)
```

```python
# getpass_prompt.py
import getpass

p = getpass.getpass(prompt='What is your favorite color? ')
if p.lower() == 'blue':
    print('Right.  Off you go.')
else:
    print('Auuuuugh!')
```

```python
# getpass_stream.py
import getpass
import sys

p = getpass.getpass(stream=sys.stderr)
print('You entered:', p)
```

## Using getpass Without a Terminal

```python
# getpass_noterminal.py
import getpass
import sys

if sys.stdin.isatty():
    p = getpass.getpass('Using getpass: ')
else:
    print('Using readline')
    p = sys.stdin.readline().rstrip()

print('Read: ', p)
```
