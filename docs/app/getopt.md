# getopt 命令行选项解析

## 函数参数

## Short Form Options

```python
# getopt_short.py
import getopt

opts, args = getopt.getopt(['-a', '-bval', '-c', 'val'], 'ab:c:')

for opt in opts:
    print(opt)
```

## Long Form Options

```python
# getopt_long.py
import getopt

opts, args = getopt.getopt(
    ['--noarg',
     '--witharg', 'val',
     '--witharg2=another'],
    '',
    ['noarg', 'witharg=', 'witharg2='],
)
for opt in opts:
    print(opt)
```

## 完整例子

```python
# getopt_example.py
import getopt
import sys

version = '1.0'
verbose = False
output_filename = 'default.out'

print('ARGV      :', sys.argv[1:])

try:
    options, remainder = getopt.getopt(
        sys.argv[1:],
        'o:v',
        ['output=',
         'verbose',
         'version=',
         ])
except getopt.GetoptError as err:
    print('ERROR:', err)
    sys.exit(1)

print('OPTIONS   :', options)

for opt, arg in options:
    if opt in ('-o', '--output'):
        output_filename = arg
    elif opt in ('-v', '--verbose'):
        verbose = True
    elif opt == '--version':
        version = arg

print('VERSION   :', version)
print('VERBOSE   :', verbose)
print('OUTPUT    :', output_filename)
print('REMAINING :', remainder)
```

## Abbreviating Long Form Options

## 类GNU的选项解析

```python
# getopt_gnu.py
import getopt
import sys

version = '1.0'
verbose = False
output_filename = 'default.out'

print('ARGV      :', sys.argv[1:])

try:
    options, remainder = getopt.gnu_getopt(
        sys.argv[1:],
        'o:v',
        ['output=',
         'verbose',
         'version=',
         ])
except getopt.GetoptError as err:
    print('ERROR:', err)
    sys.exit(1)

print('OPTIONS   :', options)

for opt, arg in options:
    if opt in ('-o', '--output'):
        output_filename = arg
    elif opt in ('-v', '--verbose'):
        verbose = True
    elif opt == '--version':
        version = arg

print('VERSION   :', version)
print('VERBOSE   :', verbose)
print('OUTPUT    :', output_filename)
print('REMAINING :', remainder)
```

## Ending Argument Processing

> If getopt() encounters “--” in the input arguments,
> it stops processing the remaining arguments as options.
> This feature can be used to pass argument values that look like options,
> such as filenames that start with a dash (“-“).

