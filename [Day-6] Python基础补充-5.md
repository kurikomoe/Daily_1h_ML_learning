<!--
 * @Author: sunyz
 * @Date: 2019-07-31 23:27:49
 * @github: https://github.com/sunyz
 * @LastEditors: sunyz
 * @LastEditTime: 2019-08-01 01:00:55
 * @Description: content
 -->

# Day 5： Python基础补充-4

- 在正则表达式中，用\d可以匹配一个数字，\w可以匹配一个字母或数字
- 在正则表达式中，用*表示任意个字符（包括0个），用+表示至少一个字符，用?表示0个或1个字符，用{n}表示n个字符，用{n,m}表示n-m个字符
- \[ ] 表示范围,^表示行的开头,$表示行的结束
- 使用Python的r前缀，就不用考虑转义的问题
- \s可以匹配一个空格（也包括Tab等空白符），所以\s+表示至少有一个空格
- 用正则表达式切分字符串比用固定的字符更灵活
- 用()表示的就是要提取的分组（Group）,栗子：

  ```py
  >>> m = re.match(r'^(\d{3})-(\d{3,8})$', '010-12345')
  >>> m
  <_sre.SRE_Match object; span=(0, 9), match='010-12345'>
  >>> m.group(0)
  '010-12345'
  >>> m.group(1)
  '010'
  >>> m.group(2)
  '12345'
  ```

- 加个?就可以让\d+采用非贪婪匹配
- 如果一个正则表达式要重复使用几千次，出于效率的考虑，我们可以预编译该正则表达式，接下来重复使用时就不需要编译这个步骤了，直接匹配：

  ```py
  >>> import re
  # 编译:
  >>> re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
  # 使用：
  >>> re_telephone.match('010-12345').groups()
  ('010', '12345')
  >>> re_telephone.match('010-8086').groups()
  ('010', '8086')
  ```

- timestamp()方法可以换算出从1970年开始的秒数
- str转datetime需要按照字符串'%Y-%m-%d %H:%M:%S'规定的日期和时间部分的格式
- namedtuple('名称', \[属性list])用来创建一个自定义的tuple对象，并且规定了tuple元素的个数
- deque是为了高效实现插入和删除操作的双向列表，适合用于队列和栈
- 如果要保持Key的顺序，可以用OrderedDict按照**插入的顺序**排列
- Base64适用于小段内容的编码，比如数字证书签名、Cookie的内容等
- struct的pack函数把任意数据类型变成bytes,栗子：

  ```py
  >>> import struct
  # >表示字节顺序是big-endian，也就是网络序，I表示4字节无符号整数
  >>> struct.pack('>I', 10240099)
  b'\x00\x9c@c'
  ```

- 摘要算法就是通过摘要函数f()对任意长度的数据data计算出固定长度的摘要digest，目的是为了发现原始数据是否被人篡改过...(其实就是hash)
- MD5是最常见的摘要算法，速度很快，生成结果是固定的128 bit字节，通常用一个32位的16进制字符串表示
- SHA1的结果是160 bit字节，通常用一个40位的16进制字符串表示
- 要确保存储的用户口令不是那些已经被计算出来的常用口令的MD5，这一方法通过对原始口令加一个复杂字符串来实现，俗称“加盐”
- 如果假定用户无法修改登录名，就可以通过把登录名作为Salt的一部分来计算MD5，从而实现相同口令的用户也存储不同的MD5
- Hmac算法：Keyed-Hashing for Message Authentication。它通过一个标准算法，在计算哈希的过程中，把key混入计算过程中
- 模拟一个微博登录，先读取登录的邮箱和口令，然后按照weibo.cn的登录页的格式以username=xxx&password=xxx的编码传入：

  ```py
  from urllib import request, parse

  print('Login to weibo.cn...')
  email = input('Email: ')
  passwd = input('Password: ')
  login_data = parse.urlencode([
      ('username', email),
      ('password', passwd),
      ('entry', 'mweibo'),
      ('client_id', ''),
      ('savestate', '1'),
      ('ec', ''),
      ('pagerefer', 'https://passport.weibo.cn/signin/welcome?entry=mweibo&r=http%3A%2F%2Fm.weibo.cn%2F')
  ])

  req = request.Request('https://passport.weibo.cn/sso/login')
  req.add_header('Origin', 'https://passport.weibo.cn')
  req.add_header('User-Agent', 'Mozilla/6.0 (iPhone; CPU iPhone OS 8_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/8.0 Mobile/10A5376e Safari/8536.25')
  req.add_header('Referer', 'https://passport.weibo.cn/signin/login?entry=mweibo&res=wel&wm=3349&r=http%3A%2F%2Fm.weibo.cn%2F')

  with request.urlopen(req, data=login_data.encode('utf-8')) as f:
      print('Status:', f.status, f.reason)
      for k, v in f.getheaders():
          print('%s: %s' % (k, v))
      print('Data:', f.read().decode('utf-8'))
  ```

- proxy控制

  ```py
  proxy_handler = urllib.request.ProxyHandler({'http': 'http://www.example.com:3128/'})
  proxy_auth_handler = urllib.request.ProxyBasicAuthHandler()
  proxy_auth_handler.add_password('realm', 'host', 'username', 'password')
  opener = urllib.request.build_opener(proxy_handler, proxy_auth_handler)
  with opener.open('http://www.example.com/login.html') as f:
      pass
  ```
