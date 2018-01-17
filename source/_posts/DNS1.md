---
title: Distributed System之DNS
date: 2016-02-14 23:45:39
tags:
  - Distributed System
  - Network
categories:
  - Distributed System
---


### 引入DNS
按照国际惯例，首先谈谈什么是DNS？我们用电话拨号可以打一个比喻。我们都知道电话之间通信在用户层面，就是通过电话号码来连接。当你准备给Jason Chan打电话时，你掏出你的手机，按键的时候你会一个个按数字吗？在这个智能机横行的时代，大多数时候你不会。你会记住你叔叔的、你表嫂的、你妹妹的闺蜜的电话吗？如果你是一个正常人，你记不住。但是你会在通讯录中找到目标人物的名字，然后点击“Call”按钮，这样手机就会拨通对方的电话。
<!--more-->

同样的道理，当一个客户（Client）打算访问Google， Youtube, Baidu(Servers)的时候，你会在浏览器地址栏去敲打IP地址吗？不会，因为记不住。。。我们只知道输入www.google.com或者www.google.hk...

这样的话，说明作为一个正常的人，你不需要记住网站的IP地址，只需要记住他的“别名”。但是同时，需要有一种机制，帮助你可以通过网站域名，自动映射到IP地址。这时你就需要Domain Name System (DNS)。

### 域名命名的问题
在DNS出现之前，所有的映射关系都存在一个host.txt的文件中。以Linux为例，所有网站的映射都存在 /etc/hosts 中。机器会定期地通过FTP来更新host.txt。显然，尽管对于域名命名方式没有任何讲究，但是这种单机集中式处理方式显得十分笨拙。难道不是吗？

同时，任意命名的一个问题在于无法保证域名的独特性(Uniqueness)。比如，USC到底是Universitiy of SiChuan,还是University of South California？ MIT到底指的是Massachusetts Institute of Technology，还是Mexican Institute of Technology？谁知道啊！

DNS后来在众人期待中就闪亮出场了，DNS很好地解决了这系列的问题。首先它使用了分布式的数据库，保证了所有的网站域名是可以可扩展Scalable的。同时，这种命名体系是一个树状的命名结构，而且是倒置。这个意思是全球的顶级域名.com, 中国域名.cn, 日本域名.jp,或者是教育域名后缀.edu在树的第一层；下一层就是二级域名(如果有的话)；再下一层就是子域名，代表了独一无二的公司或者组织，比如google,yahoo,baidu等；再往下一层就是主机，一般就是www, news, mail，ftp等，代表了不同的业务。

![树状命名](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150708-2.jpg)

树的根由一个国际组织ICANN控制，树的根实际上是总共有13个root Servers。全球用的是同一棵树，那么这意味着每一个节点以及所代表的地址，是全球独一无二的。你在美国用www.baidu.com和在中国用www.baidu.com得到的结果是一样的。（当然，你在中国使用www.google.com估计只有得到404了，跟墙外是不一样的，这个不在讨论范围中。）

同时，刚才提到了一个地域划分，美国域名.us, 中国域名.cn, 日本域名.jp, 英国.uk等等，每个区域都有一个管理员(Administrator)，这样的话，一个当地的管理员不需要储存所有DNS名字，只需要负责其所属的小区域就可以了，实际上也是“分而治之”的思想体现。当然系统中可能有冗余来保证DNS系统的健壮性（robustness）。

### DNS过程
DNS过程稍微有点繁琐。具体大致如下：
<待填坑>

### DNS所存在的风险

* Denial Of Service
* DNS Hijack

### Reference:
Prof.Bruce Maggs，CS512的课件
