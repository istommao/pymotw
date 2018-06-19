# cgitb Detailed Traceback Reports

## Standard Traceback Dumps

```python
# cgitb_basic_traceback.py
def func2(a, divisor):
    return a / divisor


def func1(a, b):
    c = b - 5
    return func2(a, c)

func1(1, 5)
```

## Enabling Detailed Tracebacks

```python
# cgitb_local_vars.py
import cgitb
cgitb.enable(format='text')
```

## Local Variables in Tracebacks

```python
# cgitb_with_classes.py
import cgitb
cgitb.enable(format='text', context=12)


class BrokenClass:
    """This class has an error.
    """

    def __init__(self, a, b):
        """Be careful passing arguments in here.
        """
        self.a = a
        self.b = b
        self.c = self.a * self.b
        # Really
        # long
        # comment
        # goes
        # here.
        self.d = self.a / self.b
        return

o = BrokenClass(1, 0)
```

## Exception Properties

```python
# cgitb_exception_properties.py
import cgitb
cgitb.enable(format='text')


class MyException(Exception):
    """Add extra properties to a special exception
    """

    def __init__(self, message, bad_value):
        self.bad_value = bad_value
        Exception.__init__(self, message)
        return

raise MyException('Normal message', bad_value=99)
```

## HTML Output

## Logging Tracebacks

```python
# cgitb_log_exception.py
import cgitb
import os

LOGDIR = os.path.join(os.path.dirname(__file__), 'LOGS')

if not os.path.exists(LOGDIR):
    os.makedirs(LOGDIR)

cgitb.enable(
    logdir=LOGDIR,
    display=False,
    format='text',
)


def func(a, divisor):
    return a / divisor

func(1, 0)
```
