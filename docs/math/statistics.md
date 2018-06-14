# statistics 统计计算

## 平均值

```python
# statistics_mean.py
from statistics import *

data = [1, 2, 2, 5, 10, 12]

print('{:0.2f}'.format(mean(data)))
```

```python
# statistics_mode.py
from statistics import *

data = [1, 2, 2, 5, 10, 12]

print(mode(data))
```

```python
# statistics_median.py
from statistics import *

data = [1, 2, 2, 5, 10, 12]

print('median     : {:0.2f}'.format(median(data)))
print('low        : {:0.2f}'.format(median_low(data)))
print('high       : {:0.2f}'.format(median_high(data)))
```

```python
# statistics_median_grouped.py
from statistics import *

data = [10, 20, 30, 40]

print('1: {:0.2f}'.format(median_grouped(data, interval=1)))
print('2: {:0.2f}'.format(median_grouped(data, interval=2)))
print('3: {:0.2f}'.format(median_grouped(data, interval=3)))
```

## 方差

```python
# statistics_variance.py
from statistics import *
import subprocess


def get_line_lengths():
    cmd = 'wc -l ../[a-z]*/*.py'
    out = subprocess.check_output(
        cmd, shell=True).decode('utf-8')
    for line in out.splitlines():
        parts = line.split()
        if parts[1].strip().lower() == 'total':
            break
        nlines = int(parts[0].strip())
        if not nlines:
            continue  # skip empty files
        yield (nlines, parts[1].strip())


data = list(get_line_lengths())

lengths = [d[0] for d in data]
sample = lengths[::2]

print('Basic statistics:')
print('  count     : {:3d}'.format(len(lengths)))
print('  min       : {:6.2f}'.format(min(lengths)))
print('  max       : {:6.2f}'.format(max(lengths)))
print('  mean      : {:6.2f}'.format(mean(lengths)))

print('\nPopulation variance:')
print('  pstdev    : {:6.2f}'.format(pstdev(lengths)))
print('  pvariance : {:6.2f}'.format(pvariance(lengths)))

print('\nEstimated variance for sample:')
print('  count     : {:3d}'.format(len(sample)))
print('  stdev     : {:6.2f}'.format(stdev(sample)))
print('  variance  : {:6.2f}'.format(variance(sample)))
```
