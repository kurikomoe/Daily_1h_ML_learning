<!--
 * @Author: sunyz
 * @Date: 2019-07-26 14:29:10
 * @LastEditors: sunyz
 * @LastEditTime: 2019-07-26 17:48:28
 * @Description: content
 -->

# Day 2： Python基础补充-2

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

- *生成式*感觉作用不大，之后用到再补吧
- 