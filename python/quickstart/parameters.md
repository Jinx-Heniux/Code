# Parameters



## [Python不定长参数(\*args、\*\*kwargs含义) - 知乎](https://zhuanlan.zhihu.com/p/70649428)



```python
def register(name, email, **kwargs):
    print('name:%s, age:%s, others:%s', (name, email, kwargs))

d = {"addr":"shanghai"}
register("demon", "1@1.com", **d)

d = {"name":"yrr", "email":"1@1.com", "addr":"shanghai"}
register(**d)

d = {"email":"yrr", "name":"1@1.com", "addr":"shanghai"}
register(**d)

"""
name:%s, age:%s, others:%s ('demon', '1@1.com', {'addr': 'shanghai'})
name:%s, age:%s, others:%s ('yrr', '1@1.com', {'addr': 'shanghai'})
name:%s, age:%s, others:%s ('1@1.com', 'yrr', {'addr': 'shanghai'})
"""


```



```python
def test1(a, b, c=0, *args, **kwargs):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kwargs)

def test2(a, b, c=0, *args, d, **kwargs):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'args=', args, 'kw =', kwargs)

# 定义一个元组和字典用作参数传入
args = (1, 2, 3, 4)
kw = {'d': 99, 'x': '#'}

test1(*args, **kw)
# a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
test2(*args, **kw)
# a = 1 b = 2 c = 3 d = 99 args= (4,) kw = {'x': '#'}

"""
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
a = 1 b = 2 c = 3 d = 99 args= (4,) kw = {'x': '#'}
"""

```

