# pdb 交互式调试器

## 开始调试

```python
# pdb_script.py
#!/usr/bin/env python3
# encoding: utf-8
#
# Copyright (c) 2010 Doug Hellmann.  All rights reserved.
#


class MyObj:

    def __init__(self, num_loops):
        self.count = num_loops

    def go(self):
        for i in range(self.count):
            print(i)
        return

if __name__ == '__main__':
    MyObj(5).go()
```

### From Within a Program

```python
# pdb_set_trace.py
#!/usr/bin/env python3
# encoding: utf-8
#
# Copyright (c) 2010 Doug Hellmann.  All rights reserved.
#

import pdb


class MyObj:

    def __init__(self, num_loops):
        self.count = num_loops

    def go(self):
        for i in range(self.count):
            pdb.set_trace()
            print(i)
        return

if __name__ == '__main__':
    MyObj(5).go()
```

### After a Failure

```python
# pdb_post_mortem.py
#!/usr/bin/env python3
# encoding: utf-8
#
# Copyright (c) 2010 Doug Hellmann.  All rights reserved.
#


class MyObj:

    def __init__(self, num_loops):
        self.count = num_loops

    def go(self):
        for i in range(self.num_loops):
            print(i)
        return
```

## Controlling the Debugger

```python
# pdb_function_arguments.py
#!/usr/bin/env python3
# encoding: utf-8
#
# Copyright (c) 2010 Doug Hellmann.  All rights reserved.
#

import pdb


def recursive_function(n=5, output='to be printed'):
    if n > 0:
        recursive_function(n - 1)
    else:
        pdb.set_trace()
        print(output)
    return

if __name__ == '__main__':
    recursive_function()
```

