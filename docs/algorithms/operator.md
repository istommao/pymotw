# operator 内置操作符

## 逻辑运算符

```python
import operator

a = -1
b = 5

print('a =', a)
print('b =', b)
print()

print('not_(a)     :', operator.not_(a))
print('truth(a)    :', operator.truth(a))
print('is_(a, b)   :', operator.is_(a, b))
print('is_not(a, b):', operator.is_not(a, b))
```

## 比较运算符

```python
import operator

a = 1
b = 5.0

print('a =', a)
print('b =', b)
for func in (operator.lt, operator.le, operator.eq, operator.ne, operator.ge, operator.gt):
    print('{}(a, b): {}'.format(func.__name__, func(a, b)))
```

## 算术运算符

```python
import operator

a = -1
b = 5.0
c = 2
d = 6

print('a =', a)
print('b =', b)
print('c =', c)
print('d =', d)

print('\nPositive/Negative:')
print('abs(a):', operator.abs(a))
print('neg(a):', operator.neg(a))
print('neg(b):', operator.neg(b))
print('pos(a):', operator.pos(a))
print('pos(b):', operator.pos(b))

print('\nArithmetic:')
print('add(a, b)     :', operator.add(a, b))
print('floordiv(a, b):', operator.floordiv(a, b))
print('floordiv(d, c):', operator.floordiv(d, c))
print('mod(a, b)     :', operator.mod(a, b))
print('mul(a, b)     :', operator.mul(a, b))
print('pow(c, d)     :', operator.pow(c, d))
print('sub(b, a)     :', operator.sub(b, a))
print('truediv(a, b) :', operator.truediv(a, b))
print('truediv(d, c) :', operator.truediv(d, c))

print('\nBitwise:')
print('and_(c, d)  :', operator.and_(c, d))
print('invert(c)   :', operator.invert(c))
print('lshift(c, d):', operator.lshift(c, d))
print('or_(c, d)   :', operator.or_(c, d))
print('rshift(d, c):', operator.rshift(d, c))
print('xor(c, d)   :', operator.xor(c, d))
```

## 序列操作符

```python
import operator

a = [1, 2, 3]
b = ['a', 'b', 'c']

print('a =', a)
print('b =', b)

print('\nConstructive:')
print('  concat(a, b):', operator.concat(a, b))

print('\nSearching:')
print('  contains(a, 1)  :', operator.contains(a, 1))
print('  contains(b, "d"):', operator.contains(b, "d"))
print('  countOf(a, 1)   :', operator.countOf(a, 1))
print('  countOf(b, "d") :', operator.countOf(b, "d"))
print('  indexOf(a, 5)   :', operator.indexOf(a, 1))

print('\nAccess Items:')
print('  getitem(b, 1)                  :', operator.getitem(b, 1))
print('  getitem(b, slice(1, 3))        :', operator.getitem(b, slice(1, 3)))
print('  setitem(b, 1, "d")             :', end=' ')
operator.setitem(b, 1, "d")
print(b)
print('  setitem(a, slice(1, 3), [4, 5]):', end=' ')
operator.setitem(a, slice(1, 3), [4, 5])
print(a)

print('\nDestructive:')
print('  delitem(b, 1)          :', end=' ')
operator.delitem(b, 1)
print(b)
print('  delitem(a, slice(1, 3)):', end=' ')
operator.delitem(a, slice(1, 3))
print(a)
```

## In-place Operators 原地操作符

```python
import operator

a = -1
b = 5.0
c = [1, 2, 3]
d = ['a', 'b', 'c']
print('a =', a)
print('b =', b)
print('c =', c)
print('d =', d)
print()

a = operator.iadd(a, b)
print('a = iadd(a, b) =>', a)
print()

c = operator.iconcat(c, d)
print('c = iconcat(c, d) =>', c)
```

## Attribute and Item “Getters”

```python
import operator


class MyObj:
    """example class for attrgetter"""

    def __init__(self, arg):
        super().__init__()
        self.arg = arg

    def __repr__(self):
        return 'MyObj({})'.format(self.arg)


l = [MyObj(i) for i in range(5)]
print('objects   :', l)

# Extract the 'arg' value from each object
g = operator.attrgetter('arg')
vals = [g(i) for i in l]
print('arg values:', vals)

# Sort using arg
l.reverse()
print('reversed  :', l)
print('sorted    :', sorted(l, key=g))
```

```python
import operator

l = [dict(val=-1 * i) for i in range(4)]
print('Dictionaries:')
print(' original:', l)
g = operator.itemgetter('val')
vals = [g(i) for i in l]
print('   values:', vals)
print('   sorted:', sorted(l, key=g))

print()
l = [(i, i * -2) for i in range(4)]
print('\nTuples:')
print(' original:', l)
g = operator.itemgetter(1)
vals = [g(i) for i in l]
print('   values:', vals)
print('   sorted:', sorted(l, key=g))
```

## 组合操作符与自定义类

```python
import operator


class MyObj:
    """Example for operator overloading"""

    def __init__(self, val):
        super(MyObj, self).__init__()
        self.val = val

    def __str__(self):
        return 'MyObj({})'.format(self.val)

    def __lt__(self, other):
        """compare for less-than"""
        print('Testing {} < {}'.format(self, other))
        return self.val < other.val

    def __add__(self, other):
        """add values"""
        print('Adding {} + {}'.format(self, other))
        return MyObj(self.val + other.val)


a = MyObj(1)
b = MyObj(2)

print('Comparison:')
print(operator.lt(a, b))

print('\nArithmetic:')
print(operator.add(a, b))
```


