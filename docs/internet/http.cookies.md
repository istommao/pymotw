# http.cookies HTTP Cookies

## 创建和设置Cookie

```python
# http_cookies_setheaders.py
from http import cookies


c = cookies.SimpleCookie()
c['mycookie'] = 'cookie_value'
print(c)
```

## Morsels

```python
# http_cookies_Morsel.py
from http import cookies
import datetime


def show_cookie(c):
    print(c)
    for key, morsel in c.items():
        print()
        print('key =', morsel.key)
        print('  value =', morsel.value)
        print('  coded_value =', morsel.coded_value)
        for name in morsel.keys():
            if morsel[name]:
                print('  {} = {}'.format(name, morsel[name]))


c = cookies.SimpleCookie()

# A cookie with a value that has to be encoded
# to fit into the header
c['encoded_value_cookie'] = '"cookie,value;"'
c['encoded_value_cookie']['comment'] = 'Has escaped punctuation'

# A cookie that only applies to part of a site
c['restricted_cookie'] = 'cookie_value'
c['restricted_cookie']['path'] = '/sub/path'
c['restricted_cookie']['domain'] = 'PyMOTW'
c['restricted_cookie']['secure'] = True

# A cookie that expires in 5 minutes
c['with_max_age'] = 'expires in 5 minutes'
c['with_max_age']['max-age'] = 300  # seconds

# A cookie that expires at a specific time
c['expires_at_time'] = 'cookie_value'
time_to_live = datetime.timedelta(hours=1)
expires = (datetime.datetime(2009, 2, 14, 18, 30, 14) +
           time_to_live)

# Date format: Wdy, DD-Mon-YY HH:MM:SS GMT
expires_at_time = expires.strftime('%a, %d %b %Y %H:%M:%S')
c['expires_at_time']['expires'] = expires_at_time

show_cookie(c)
```

## Encoded Values

```python
# http_cookies_coded_value.py
from http import cookies


c = cookies.SimpleCookie()
c['integer'] = 5
c['with_quotes'] = 'He said, "Hello, World!"'

for name in ['integer', 'with_quotes']:
    print(c[name].key)
    print('  {}'.format(c[name]))
    print('  value={!r}'.format(c[name].value))
    print('  coded_value={!r}'.format(c[name].coded_value))
    print()
```

## 接收和解析cookie头

```python
# http_cookies_parse.py
from http import cookies


HTTP_COOKIE = '; '.join([
    r'integer=5',
    r'with_quotes="He said, \"Hello, World!\""',
])

print('From constructor:')
c = cookies.SimpleCookie(HTTP_COOKIE)
print(c)

print()
print('From load():')
c = cookies.SimpleCookie()
c.load(HTTP_COOKIE)
print(c)
```

## 选择输出格式

```python
# http_cookies_js_output.py
from http import cookies
import textwrap


c = cookies.SimpleCookie()
c['mycookie'] = 'cookie_value'
c['another_cookie'] = 'second value'
js_text = c.js_output()
print(textwrap.dedent(js_text).lstrip())
```

