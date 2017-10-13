---
title: collections模块使用
date: 2017-10-13 13:59:01
tags:
---

# `collections.namedtuple`
### collections.nametuple是一个工厂函数，主要用来产生可以使用名称来访问元素的数据对象，通常用来增强代码的可读性， 在访问一些tuple类型的数据时尤其好用。  
用 `namedtuple` 构建的类的实例所消耗的内存跟元组是一样的。
``` python
# -*- coding: utf-8 -*-
"""
比如我们用户拥有一个这样的数据结构，每一个对象是拥有三个元素的tuple。
使用namedtuple方法就可以方便的通过tuple来生成可读性更高也更好用的数据结构。
"""
from collections import namedtuple
websites = [
    ('Sohu', 'http://www.google.com/', u'张朝阳'),
    ('Sina', 'http://www.sina.com.cn/', u'王志东'),
    ('163', 'http://www.163.com/', u'丁磊')
]
Website = namedtuple('Website', ['name', 'url', 'founder'])
for website in websites:
    website = Website._make(website)
    print website
# Result:
Website(name='Sohu', url='http://www.google.com/', founder=u'\u5f20\u671d\u9633')
Website(name='Sina', url='http://www.sina.com.cn/', founder=u'\u738b\u5fd7\u4e1c')
Website(name='163', url='http://www.163.com/', founder=u'\u4e01\u78ca')
```

+ `_fields` 属性是一个包含这个类所有字段名称的元组。
```python
Website._fields
#Result:('name', 'url', 'founder')
```

+ `_make()` 通过接受一个可迭代对象来生成这个类的一个实例
```python
a = ('Sohu', 'http://www.google.com/', u'张朝阳')
w1 = Website._make(a)
#out Website(name='Sohu', url='http://www.google.com/', founder='张朝阳')

# 跟 Website(*a)是一样的
```

+ `_asdict()` 把具名元组以collections.OrderdDict形式返回
```python
w1._asdict()
# Out:
OrderedDict([('name', 'Sohu'),
             ('url', 'http://www.google.com/'),
             ('founder', '张朝阳')])
```

# `双向队列` *collections.deque*
### deque其实是 double-ended queue 的缩写，翻译过来就是双端队列，它最大的好处就是实现了从队列 头部快速增加和取出对象: .popleft(), .appendleft()    

双向队列从中间删除元素的操作会慢一些，因为它只对在头尾的操作进行了优化。

`append` && `popleft` 都是原子操作
 
``` python
# -*- coding: utf-8 -*-
"""
下面这个是一个有趣的例子，主要使用了deque的rotate方法来实现了一个无限循环
的加载动画

deque.rotate 接受一个参数n，当n > 0 时，队列的最右边的n个元素会被移动到队列的左边,
    当 n < 0 时，最左边的 n 个元素会被移动到右边。
"""
import sys
import time
from collections import deque
fancy_loading = deque('>--------------------')
while True:
    print '\r%s' % ''.join(fancy_loading),
    fancy_loading.rotate(1)
    sys.stdout.flush()
    time.sleep(0.08)
# Result:
# 一个无尽循环的跑马灯
------------->-------
```

```python
# 通过指定队列的大小，可以实现队列用来存放“最近用到的几个元素”功能
from collections import deque
dq = deque(range(10), maxlen=10)
dq.appendleft(-1)
dq.extend([11, 22, 33])
dq.extendleft([10, 20, 30, 40])
```

# Counter
``` python
# -*- coding: utf-8 -*-
"""
下面这个例子就是使用Counter模块统计一段句子里面所有字符出现次数
"""
from collections import Counter
s = '''A Counter is a dict subclass for counting hashable objects. It is an unordered collection where elements are stored as dictionary keys and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or negative counts. The Counter class is similar to bags or multisets in other languages.'''.lower()
c = Counter(s)
# 获取出现频率最高的5个字符
print c.most_common(5)
# Result:
[(' ', 54), ('e', 32), ('s', 25), ('a', 24), ('t', 24)]
```

# OrderedDict 

### OrderedDict 会按key 进行排序
``` python
# -*- coding: utf-8 -*-
from collections import OrderedDict
items = (
    ('A', 1),
    ('B', 2),
    ('C', 3)
)
regular_dict = dict(items)
ordered_dict = OrderedDict(items)
print 'Regular Dict:'
for k, v in regular_dict.items():
    print k, v
print 'Ordered Dict:'
for k, v in ordered_dict.items():
    print k, v
# Result:
Regular Dict:
A 1
C 3
B 2
Ordered Dict:
A 1
B 2
C 3
```


