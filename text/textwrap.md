# textwrap - 格式化文本段落

## 示例数据

```python
# textwrap_example.py

sample_text = '''
    The textwrap module can be used to format text for output in
    situations where pretty-printing is desired.  It offers
    programmatic functionality similar to the paragraph wrapping
    or filling features found in many text editors.
    '''
```

## 填充段落

```python
import textwrap

sample_text = '''
    The textwrap module can be used to format text for output in
    situations where pretty-printing is desired.  It offers
    programmatic functionality similar to the paragraph wrapping
    or filling features found in many text editors.
    '''

print(textwrap.fill(sample_text, width=50))
```