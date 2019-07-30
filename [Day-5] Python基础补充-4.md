<!--
 * @Author: sunyz
 * @Date: 2019-07-30 22:52:44
 * @github: https://github.com/sunyz
 * @LastEditors: sunyz
 * @LastEditTime: 2019-07-30 23:56:42
 * @Description: content
 -->

# Day 5： Python基础补充-4

- 常见的错误类型和继承关系<https://docs.python.org/3/library/exceptions.html#exception-hierarchy>
- 启动Python解释器时可以用-O参数来关闭assert
- logging的好处，它允许你指定记录信息的级别，有debug，info，warning，error等几个级别，当我们指定level=INFO时，logging.debug就不起作用了。同理，指定level=WARNING后，debug和info就不起作用了。这样一来，你可以放心地输出不同级别的信息，也不用删除，最后统一控制输出哪个级别的信息。栗子：

  ```py
  import logging
  logging.basicConfig(level=logging.INFO)

  s = '0'
  n = int(s)
  logging.info('n = %d' % n)
  print(10 / n)
  ```

- 标准写法的文档测试，栗子：

  ```py
  def abs(n):
    '''
    Function to get absolute value of number.

    Example:

    >>> abs(1)
    1
    >>> abs(-1)
    1
    >>> abs(0)
    0
    '''
    return n if n >= 0 else (-n)
    ```

- Python引入了with语句来自动帮我们调用close()方法
- 只有调用close()方法时，操作系统才保证把没有写入的数据全部写入磁盘。忘记调用close()的后果是数据可能只写了一部分到磁盘，剩下的丢失了。所以，还是用with语句来得保险

  ```py
  with open('/Users/michael/test.txt', 'w') as f:
      f.write('Hello, world!')
  ```

- 操作文件和目录

  ```py
  # 查看当前目录的绝对路径:
  >>> os.path.abspath('.')
  '/Users/michael'
  # 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
  >>> os.path.join('/Users/michael', 'testdir')
  '/Users/michael/testdir'
  # 然后创建一个目录:
  >>> os.mkdir('/Users/michael/testdir')
  # 删掉一个目录:
  >>> os.rmdir('/Users/michael/testdir')

  >>> os.path.split('/Users/michael/testdir/file.txt')
  ('/Users/michael/testdir', 'file.txt')

  >>> os.path.splitext('/path/to/file.txt')
  ('/path/to/file', '.txt')

  #列出当前目录下的所有目录
  >>> [x for x in os.listdir('.') if os.path.isdir(x)]
  ['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim', 'Applications', 'Desktop', ...]

  #列出所有的.py文件
  >>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
  ['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py', 'urls.py', 'wsgiapp.py']
  ```

- shutil模块提供了copyfile()的函数
- 把变量从内存中变成可存储或传输的过程称之为序列化，在Python中叫pickling，在其他语言中也被称之为serialization，marshalling，flattening等等
- Python提供了pickle模块来实现序列化，栗子：

  ```py
  >>> import pickle
  >>> d = dict(name='Bob', age=20, score=88)
  >>> pickle.dumps(d)
  b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'

  #或者用pickle.dump()直接把对象序列化后写入一个file-like Object
  >>> f = open('dump.txt', 'wb')
  >>> pickle.dump(d, f)
  >>> f.close()

  #可以直接用pickle.load()方法从一个file-like Object中直接反序列化出对象
  >>> f = open('dump.txt', 'rb')
  >>> d = pickle.load(f)
  >>> f.close()
  >>> d
  {'age': 20, 'score': 88, 'name': 'Bob'}
  ```

- json模块的dumps()和loads()函数是定义得非常好的接口的典范
- 如果要启动大量的子进程，可以用进程池的方式批量创建子进程,栗子：

  ```py
  from multiprocessing import Pool
  import os, time, random

  def long_time_task(name):
      print('Run task %s (%s)...' % (name, os.getpid()))
      start = time.time()
      time.sleep(random.random() * 3)
      end = time.time()
      print('Task %s runs %0.2f seconds.' % (name, (end - start)))

  if __name__=='__main__':
      print('Parent process %s.' % os.getpid())
      p = Pool(4)
      for i in range(5):
          p.apply_async(long_time_task, args=(i,))
      print('Waiting for all subprocesses done...')
      p.close()
      p.join()
      print('All subprocesses done.')
  ```

- Pool的默认大小是CPU的核数
- 进程间通信是通过Queue、Pipes等实现的
- 线程使用

  ```py
  import time, threading

  # 新线程执行的代码:
  def loop():
      print('thread %s is running...' % threading.current_thread().name)
      n = 0
      while n < 5:
          n = n + 1
          print('thread %s >>> %s' % (threading.current_thread().name, n))
          time.sleep(1)
      print('thread %s ended.' % threading.current_thread().name)

  print('thread %s is running...' % threading.current_thread().name)
  t = threading.Thread(target=loop, name='LoopThread')
  t.start()
  t.join()
  print('thread %s ended.' % threading.current_thread().name)

  #执行结果
  thread MainThread is running...
  thread LoopThread is running...
  thread LoopThread >>> 1
  thread LoopThread >>> 2
  thread LoopThread >>> 3
  thread LoopThread >>> 4
  thread LoopThread >>> 5
  thread LoopThread ended.
  thread MainThread ended.
  ```

- lock,注意：**用try...finally来确保锁一定会被释放**

  ```py
  balance = 0
  lock = threading.Lock()

  def run_thread(n):
      for i in range(100000):
          # 先要获取锁:
          lock.acquire()
          try:
              # 放心地改吧:
              change_it(n)
          finally:
              # 改完了一定要释放锁:
              lock.release()
  ```

- 在Python中，可以使用多线程，但不要指望能有效利用多核，由于GIL的历史遗留问题
- 一个ThreadLocal变量虽然是全局变量，但每个线程都只能读写自己线程的独立副本，互不干扰。ThreadLocal解决了参数在一个线程中各个函数之间互相传递的问题
- **分布式进程**<https://www.liaoxuefeng.com/wiki/1016959663602400/1017631559645600>(以后用到细看)
