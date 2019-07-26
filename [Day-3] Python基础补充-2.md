<!--
 * @Author: sunyz
 * @Date: 2019-07-26 14:29:10
 * @LastEditors: sunyz
 * @LastEditTime: 2019-07-27 00:14:50
 * @Description: content
 -->

# Day 3： Python基础补充-2

今天从**高级特性**章节继续开始看

- L[0:3]表示，从索引0开始取，直到索引3为止，但**不包括索引3**。即索引0，1，2，正好是3个元素
- 切片后10个数：L[-10:]
- 前10个数，每两个取一个： L[:10:2]
- 只写[:]就可以原样复制一个list
- dict的存储不是按照list的方式顺序排列，所以，迭代出的结果顺序很可能不一样
- 默认情况下，dict迭代的是key。如果要迭代value，可以用

  `for value in d.values()`，

  如果要同时迭代key和value，可以用

  `for k, v in d.items()`
- Python内置的`enumerate`函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身
- 列表生成式可以快速生成期望的list，栗子：

  `[x * x for x in range(1, 11) if x % 2 == 0]`

  和

  `[d for d in os.listdir('.')] # os.listdir可以列出文件和目录`

- *生成式* 感觉作用不大，之后用到再补吧
- 编写高阶函数，就是让函数的参数能够接收别的函数
- map()函数接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回

  ```py
  >>> def f(x):
  ...     return x * x
  ...
  >>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
  >>> list(r)
  [1, 4, 9, 16, 25, 36, 49, 64, 81]
  ```

- reduce把一个函数作用在一个序列[x1, x2, x3, ...]上，这个函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算，其效果就是

  `reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)`

- filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
- 实现忽略大小写的排序：

  `sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)`

- *闭包* 暂时没感受到明确用途，用到再回看
- 关键字lambda表示匿名函数，冒号前面的x表示函数参数，只能有一个表达式，不用写return，返回值就是该表达式的结果
- 装饰器， 栗子

  ```py
  def log(text):
      def decorator(func):
          def wrapper(*args, **kw):
              print('%s %s():' % (text, func.__name__))
              return func(*args, **kw)
          return wrapper
      return decorator

  @log('execute')
  def now():
      print('2015-3-25')
      >>> now()
  # 执行
  execute now():
  2015-3-25
  ```

- functools.partial的作用就是，把一个函数的某些参数给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数会更简单，栗子：

  `int2 = functools.partial(int, base=2)`
