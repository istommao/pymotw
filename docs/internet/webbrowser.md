# webbrowser 显示网页

## 简单示例

```python
# webbrowser_open.py
import webbrowser

webbrowser.open(
    'https://docs.python.org/3/library/webbrowser.html'
)
```

## Windows vs. Tabs

```python
# webbrowser_open_new.py
import webbrowser

webbrowser.open_new(
    'https://docs.python.org/3/library/webbrowser.html'
)
```

## Using a specific browser

```python
# webbrowser_get.py
import webbrowser

b = webbrowser.get('lynx')
b.open('https://docs.python.org/3/library/webbrowser.html')
```
