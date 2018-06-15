# glob 文件名模式匹配

## 通配符

```python
# glob_asterisk.py
import glob
for name in sorted(glob.glob('dir/*')):
    print(name)
```

```python
# glob_subdir.py
import glob

print('Named explicitly:')
for name in sorted(glob.glob('dir/subdir/*')):
    print('  {}'.format(name))

print('Named with wildcard:')
for name in sorted(glob.glob('dir/*/*')):
    print('  {}'.format(name))
```

## 单个字符通配符

```python
# glob_question.py
import glob

for name in sorted(glob.glob('dir/file?.txt')):
    print(name)
```

## 字符范围

```python
# glob_charrange.py
import glob
for name in sorted(glob.glob('dir/*[0-9].*')):
    print(name)
```

## 转义元字符

```python
# glob_escape.py
import glob

specials = '?*['

for char in specials:
    pattern = 'dir/*' + glob.escape(char) + '.txt'
    print('Searching for: {!r}'.format(pattern))
    for name in sorted(glob.glob(pattern)):
        print(name)
    print()
```

