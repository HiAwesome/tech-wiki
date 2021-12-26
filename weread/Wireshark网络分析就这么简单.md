## Wireshark网络分析就这么简单

 **林沛满**


### 前言

* Wireshark 是最流行的网络嗅探器之一，能在多种平台上抓取和分析网络包，比如Windows、Linux和Mac等。它的图形界面非常友好，但如果你觉得鼠标操作不够有腔调，也可以使用它的命令行形式——TShark。

* 对于缺乏网络基础的Wireshark用户，建议先阅读一本较成系统的教材，个人推荐Richard Stevens的《TCP/IP详解卷1：协议》。搭上《颈椎病防治一本通》也许还能免运费，前一本有助于你更快地学会 Wireshark，后一本则能在学会Wireshark之后治疗职业病。



### 你一定会喜欢的技巧

* 经常有人在论坛上问，Wireshark 是按照什么过滤出一个 TCP/UDP Stream的？答案就是：两端的IP加port。单击Wireshark的Statistics-->Conversations，再单击TCP或者UDP标签就可以看到所有的Stream（见图11）。

* 总体来说，过滤是 Wireshark 中最有趣，最难，也是最有价值之处，值得我们用心学习。

* 与很多软件一样，Wireshark 也可以通过“Ctrl+F”搜索关键字。假如我们怀疑包里含有“error”一词，就可以按下“Ctrl+F”之后选中“String”单选按钮，然后在Filter中输入“error”进行搜索（见图19）。很多应用层的错误都可以靠这个方法锁定问题包


### Patrick的故事

* 这时我不禁想起他自谦过的一句话“Everybody has his expertise”。可是有什么技术领域不是你的expertise? 我很想当面问问这位素未谋面的老师。

