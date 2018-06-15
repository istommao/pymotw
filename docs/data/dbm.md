# dbm Unix健值数据库

## 数据库类型


### dbm.gnu

### dbm.ndbm

### dbm.dumb


## 创建一个新的数据库

```python
# dbm_new.py
import dbm

with dbm.open('/tmp/example.db', 'n') as db:
    db['key'] = 'value'
    db['today'] = 'Sunday'
    db['author'] = 'Doug'
```

```python
# dbm_whichdb.py
import dbm

print(dbm.whichdb('/tmp/example.db'))
```


## 打开一个存在的数据库

```python
# dbm_existing.py
import dbm

with dbm.open('/tmp/example.db', 'r') as db:
    print('keys():', db.keys())
    for k in db.keys():
        print('iterating:', k, db[k])
    print('db["author"] =', db['author'])
```

## 错误情况

```python
# dbm_intkeys.py
import dbm

with dbm.open('/tmp/example.db', 'w') as db:
    try:
        db[1] = 'one'
    except TypeError as err:
        print(err)
```

```python
# dbm_intvalue.py
import dbm

with dbm.open('/tmp/example.db', 'w') as db:
    try:
        db['one'] = 1
    except TypeError as err:
        print(err)
```
