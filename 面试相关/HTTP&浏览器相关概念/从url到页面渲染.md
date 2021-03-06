这个过程大致分为六个部分
1. DNS域名解析系统：将域名地址(url)转化为对应的ip地址
  - 浏览器DNS缓存
  - 系统DNS缓存
  - 路由器DNS缓存
  - 网络运营商DNS缓存
  - 递归搜索(下面分析)

  - 域名：通俗理解就是网址 格式一般为：`www.baidu.com.` 注意最后的那个点
  在一般情况下，为了输入方便，那个点我们省略了，但是它至关重要
  - IP地址：表示一台主机的地址，就好像人的住址一样。如果想找到我，不能只知道我的名字(某某某)，
  还要知道我的住址。访问网站也是一样，如果想打开百度，不能只知道它叫做百度(域名)，还要知道它真实的地址(ip)

  通过域名查找IP地址，就是DNS的用途。

  一个域名`www.baidu.com.`由4部分组成
    - 第一部分：`.`代表根服务器
    - 第二部分：`.com.`代表顶级域名服务器
    - 第三部分：`baidu.com.`代表域名所有者服务器
    - 第四部分：`www.baidu.com.`代表主机域名

  域名体系是一个分级的过程，最高级是根服务器，最低级是本地服务器(如127.0.0.1)
  域名的查询需要逐级递归查询
  过程：
    - 浏览器向本地DNS发送查询请求
    - 本地DNS再向根服务器`.` 查询`.com.`的服务器ip地址
    - 查询到了后，本地DNS就向`.com.`服务器查询`baidu.com.`的ip地址
    - 本地DNS向`baidu.com.`服务器中查询`www.baidu.com.`的ip地址
    - 最后将查询到的`www.baidu.com.`ip地址发给浏览器

2. TCP连接，三次握手
  - 第一次握手：由浏览器发起，告诉服务器我要发送请求了
  - 第二次握手，由服务器发起，告诉浏览器我准备好了，你可以发送了
  - 第三次握手，由浏览器发起，告诉服务器我马上就要发了，你准备接收吧
  为什么需要三次？
  只发送第一次，当服务器正忙没空处理当前请求，或者服务器未开启。浏览器把数据发送过去，服务器无法接收到
  只发送两次：服务器已准备好（打开了一定的空间，和接口...），接收请求的数据，但当这时浏览器关闭或不发送请求，则服务器需要干等，浪费资源
  发送三次：这样可以保证连接最准确

3. 发送请求
  - 浏览器和服务器握手成功，已建立连接。
  - 浏览器发送请求报文：HTTP协议的通信内容

4. 接受响应
 - 服务器发送响应报文

5. 渲染页面
  - 遇见HTML标记，浏览器就调用HTML解析器解析成Token并构建成DOM树
  - 遇见CSS标记，就调用CSS解析器，处理css标记构建成CSSOM树
  - 遇见JS标记，就调用JS解析引擎，处理script代码(绑定事件，修改dom树/cssom树)
  - 将DOM树和CSSOM树合并成一棵渲染树
  - 根据渲染树来计算布局，计算节点的几何大小等等...
  - 将节点颜色等样式绘制到屏幕中(渲染)

  注意：
    这几个步骤不一定按照顺序来执行，如果dom树和cssom树被修改了，可能会执行多次布局和渲染(重绘重排(回流))
    往往实际页面会执行多次重绘重排

6. 断开连接：TCP四次挥手
  第一次挥手：由浏览器发起，发给服务器，我东西已经发送完了(请求报文)，你准备关闭吧
  第二次挥手：由服务器发起，告诉浏览器，我东西接收完了(请求报文)，我准备关闭了，你也准备吧
  第三次挥手：由服务器发起，告诉浏览器，我东西已经发送完了(响应报文)，你准备关闭吧
  第四次挥手：由浏览器发起，告诉服务器，我东西已经接收完毕(响应报文)，我准备关闭了，你也准备吧