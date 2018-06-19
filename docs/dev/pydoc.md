# pydoc 在线帮助模块

## 纯文本的帮助

```shell
Help on built-in module atexit:

NAME
    atexit - allow programmer to define multiple exit functions
to be executed upon normal program termination.

DESCRIPTION
    Two public functions, register and unregister, are defined.

FUNCTIONS
    register(...)
        register(func, *args, **kwargs) -> func

        Register a function to be executed upon normal program
termination

            func - function to be called at exit
            args - optional arguments to pass to func
            kwargs - optional keyword arguments to pass to func

            func is returned to facilitate usage as a decorator.

    unregister(...)
        unregister(func) -> None

        Unregister an exit function which was previously
registered using
        atexit.register

            func - function to be unregistered

FILE
    (built-in)
```


## HTML 帮助

```shell
$ pydoc -p 5000
Server ready at http://localhost:5000/
Server commands: [b]rowser, [q]uit
server> q
Server stopped
```


## 交互式帮助

```shell
Python 3.5.2 (v3.5.2:4def2a2901a5, Jun 26 2016, 10:47:25)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more
information.
>>> help('atexit')
Help on module atexit:

NAME
    atexit - allow programmer to define multiple exit functions
to be executed upon normal program termination.

...
```

