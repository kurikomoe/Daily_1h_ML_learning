<!--
 * @Author: sunyz
 * @Date: 2019-07-30 00:06:26
 * @github: https://github.com/sunyz
 * @LastEditors: sunyz
 * @LastEditTime: 2019-07-30 01:24:22
 * @Description: content
 -->

# Day 4： Python基础补充-3

今天从**模块**章节继续开始看

- 每一个包目录下面都会有一个__init__.py的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。\_\_init__.py可以是空文件，也可以有Python代码，因为__init__.py本身就是一个模块
- 任何模块代码的第一个字符串都被视为模块的文档注释
- 类似__xxx__这样的变量是特殊变量，可以被直接引用，但是有特殊用途，比如上面的__author__，__name__就是特殊变量，hello模块定义的文档注释也可以用特殊变量__doc__访问
- 类似_xxx和__xxx这样的函数或变量就是非公开的（private），不应该被直接引用，比如_abc，__abc等
- __init__方法的第一个参数永远是self
- 和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量self，并且，调用时，不用传递该参数。除此之外，类的方法和普通函数没有什么区别
- 如果要让内部属性不被外部访问，可以把属性的名称前加上两个下划线__
- 变量名类似__xxx__的，也就是以双下划线开头，并且以双下划线结尾的，是特殊变量，特殊变量是可以直接访问的，不是private变量
- 总是优先使用isinstance()判断类型，可以将指定类型及其子类“一网打尽”，栗子：

  ```py
  isinstance('a', str)
  isinstance([1, 2, 3], (list, tuple))
  ```

- Python允许在定义class的时候，定义一个特殊的__slots__变量，来限制该class实例能添加的属性，栗子：

  ```py
  class Student(object):
      __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
  ```

- 把一个getter方法变成属性，只需要加上@property就可以了，此时，@property本身又创建了另一个装饰器@score.setter，负责把一个setter方法变成属性赋值
- 只定义getter方法，不定义setter方法就是一个只读属性
- 利用完全动态的__getattr__，我们可以写出一个链式调用

  ```py
  class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__

    >>> Chain().status.user.timeline.list
    '/status/user/timeline/list'
  ```

- @unique装饰器可以帮助我们检查保证没有重复值
- *metaclass* 暂时用不到
