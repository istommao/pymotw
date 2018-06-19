# warnings Non-fatal Alerts

## Generating Warnings

```python
# warnings_warn.py
import warnings

print('Before the warning')
warnings.warn('This is a warning message')
print('After the warning')
```


```python
# warnings_warn_raise.py
import warnings

warnings.simplefilter('error', UserWarning)

print('Before the warning')
warnings.warn('This is a warning message')
print('After the warning')
```

## Filtering with Patterns

```python
# warnings_filterwarnings_message.py
import warnings

warnings.filterwarnings('ignore', '.*do not.*',)

warnings.warn('Show this message')
warnings.warn('Do not show this message')
```

```python
# warnings_filter.py
import warnings

warnings.warn('Show this message')
warnings.warn('Do not show this message')
```

```python
# warnings_filterwarnings_module.py
import warnings

warnings.filterwarnings(
    'ignore',
    '.*',
    UserWarning,
    'warnings_filter',
)

import warnings_filter
```

```python
# warnings_filterwarnings_lineno.py
import warnings

warnings.filterwarnings(
    'ignore',
    '.*',
    UserWarning,
    'warnings_filter',
    13,
)

import warnings_filter
```

## Repeated Warnings

```python
# warnings_repeated.py
import warnings


def function_with_warning():
    warnings.warn('This is a warning!')


function_with_warning()
function_with_warning()
function_with_warning()
```

```python
# warnings_once.py
import warnings

warnings.simplefilter('once', UserWarning)

warnings.warn('This is a warning!')
warnings.warn('This is a warning!')
warnings.warn('This is a warning!')
```

## Alternate Message Delivery Functions

```python
# warnings_showwarning.py
import warnings
import logging


def send_warnings_to_log(message, category, filename, lineno,
                         file=None, line=None):
    logging.warning(
        '%s:%s: %s:%s',
        filename, lineno,
        category.__name__, message,
    )


logging.basicConfig(level=logging.INFO)

old_showwarning = warnings.showwarning
warnings.showwarning = send_warnings_to_log

warnings.warn('message')
```

## Formatting

```python
# warnings_formatwarning.py
import warnings


def warning_on_one_line(message, category, filename, lineno,
                        file=None, line=None):
    return '-> {}:{}: {}:{}'.format(
        filename, lineno, category.__name__, message)


warnings.warn('Warning message, before')
warnings.formatwarning = warning_on_one_line
warnings.warn('Warning message, after')
```

## Stack Level in Warnings

```python
# warnings_warn_stacklevel.py
#!/usr/bin/env python3
# encoding: utf-8

import warnings


def old_function():
    warnings.warn(
        'old_function() is deprecated, use new_function()',
        stacklevel=2)


def caller_of_old_function():
    old_function()


caller_of_old_function()
```
