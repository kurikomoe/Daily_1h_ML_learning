<!--
 * @Author: sunyz
 * @Date: 2019-07-25 23:53:02
 * @github: https://github.com/sunyz
 * @LastEditors: sunyz
 * @LastEditTime: 2019-07-26 02:25:48
 * @Description: content
 -->

# Day 2： Python基础补充

虽然对Python有一定程度的掌握，也写过很多次爬虫，但确实没有系统的学习一遍python，心感仍有
不足。所以趁此机会把一些python基础的东西过一遍，以廖雪峰的python博客
<https://www.liaoxuefeng.com/wiki/1016959663602400> 作为参考。

以下为看到哪记到哪的笔记，不系统，仅为个人学习记录。

- 一行写不开的换行用\
- 可以用-1索引list最后一个元素
- list的删除是pop
- tuple是()
- 只有1个元素的tuple定义时必须加一个逗号,，来消除歧义，如

    ```python
    >>> t = (1,)
    >>> t
    (1,)
    ```

- input()返回的数据类型是str
- range(i) = [0, i-1], 0 起始，i-1 结束
- dict是{}
- set是([ ]),操作是add()， remove()
- str是**不变对象**，而list是可变对象,所以对字符串的操作需要返回值，原值不改
- 数据类型检查可以用内置函数isinstance()实现
- 函数可以同时返回多个值，但其实就是一个tuple
- 函数有默认参数时，必选参数在前，默认参数在后，否则Python的解释器会报错
- 定义默认参数要牢记一点：默认参数必须指向不变对象
    举个栗子

    ```python
    def add_end(L=[]):
        L.append('END')
        return L

    >>> add_end()
    ['END', 'END']
    >>> add_end()
    ['END', 'END', 'END']

    # 正确
    def add_end(L=None):
        if L is None:
            L = []
        L.append('END')
        return L
    ```

- 在参数前面加了一个*号表示可变参数
- 关键字参数并不像指针，不会改变原始值
- 关键字参数的栗子：

  ```py
  def person(name, age, **kw):
    if 'city' in kw:
        # 有city参数
        pass
    if 'job' in kw:
        # 有job参数
        pass
    print('name:', name, 'age:', age, 'other:', kw)
   ```

- 和关键字参数\*\*kw不同，命名关键字参数需要一个特殊分隔符*，*后面的参数被视为命名关键字参数
  栗子：

  `def person(name, age, *, city, job):`

- 参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数
