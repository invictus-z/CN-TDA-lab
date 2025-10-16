# Ⅰ. HTTP

## （一）基本HTTP响应/请求交互

### 实验过程及截图

![screenshot1](assets\screenshot1.png)

### 问题与回答

（以下问题ans同时标注在附件1-基本HTTP响应请求交互.pdf上）

#### Q1 HTTP版本

浏览器：http v1.1  服务器：http v1.1

#### Q2 语言

请求报文首部行：Accept-Language: zh-CN,zh;q=0.9,en;q=0.8,en-GB;q=0.7,en-US;q=0.6

（语言;q=权重）按请求次序分别是中文（简体）；中文（通用）；英文（通用）；英文（英国）；英文（美国）

#### Q3 IP地址

主机：192.168.3.128（内网地址）  服务器地址：128.119.245.12

#### Q4 返回状态码

200 OK

#### Q5 Last-Modified

响应报文首部行：Last-Modified: Tue, 30 Sep 2025 05:59:01 GMT

#### Q6 内容长度

响应报文首部行：Content-Length: 128

#### Q7 包列表窗口未显示的首部

有，比如Last-Modified

## （二）HTTP条件响应/请求交互

### 实验过程及截图

访问对应网址再刷新即可

![image-20251001150040981](assets\screenshot2.png)

### 问题与回答

#### Q8 第一个请求报文是否有“IF-MODIFIED-SINCE"头？

不包含；

#### Q9 第一个响应报文信息

直接返文件内容；在返回报文实体体中可见；

#### Q10 第二个请求报文是否有“IF-MODIFIED-SINCE"头？信息？

包含； If-Modified-Since: Wed, 01 Oct 2025 05:59:01 GMT\r\n

#### Q11 第二个响应报文信息

状态码：304 Not Modified；

不返回文件内容；返回报文实体体为空；

## （三）获取长文件

### 实验过程及截图

![image-20251001190608816](assets\screenshot3.png)

#### 解析：

311 340 342 三次握手建立对服务器端口59241的TCP连接

313 339 341 三次握手建立对服务器端口59242的TCP连接（浏览器优化 并行处理？）

343 浏览器请求报文

359 服务器TCP响应

360 361 363服务器TCP分段传输 （reassembled to HTTP=>364） 

362 365 401 浏览器TCP确认收到

366 384 请求响应/favicon.ico

### 问题与回答

#### Q12 请求报文数量与序号

1；序号343；

#### Q13 携带状态码的TCP分段

![image-20251001194044103](assets\screenshot4.png)

序号：360；

#### Q14 响应报文状态码和响应短语

200 OK；

#### Q15 TCP分段传输数量

3；对应序号360 361 363；

## （四）嵌入对象的HTML文档

### 实验过程及截图

![image-20251001205053948](assets\screenshot5.png)

### 问题与回答

#### Q16 请求报文数量和地址

3（除去请求/favicon.ico）； 

地址：128.119.245.12（gaia.cs.umass.edu）、178.79.137.164（kurose.cslash.net）；

#### Q17 串行or并行？

串行

## （五）HTTP认证

### 实验过程及截图

![image-20251001210904802](assets\screenshot6.png)

### 问题与回答

#### Q18 初始响应

401 Unauthorized；

#### Q19 第二次请求报文的新增字段

Authorization首部行："basic"+"wireshark-students:network".base64encode

# Ⅱ. DNS

## （一）nslookup

### 实验过程及截图

#### Q1-Q2

分别执行对应的nslookup命令。

![image-20251002114603480](assets\screenshot7.png)

#### Q3

执行对应命令。显示 Query refused 。故换用梯子（tun模式）重试，成功。

![image-20251002114651568](assets\screenshot8.png)

### 问题与回答

#### Q1 亚洲网页

执行命令：nslookup baidu.com

```shell
服务器:  UnKnown
Address:  192.168.3.1

非权威应答:
名称:    baidu.com
Addresses:  39.156.70.37
            220.181.7.203
```

#### Q2 欧洲大学DNS服务器

执行命令：nslookup -type=NS imperial.ac.uk

```shell
服务器:  UnKnown
Address:  192.168.3.1

非权威应答:
imperial.ac.uk  nameserver = auth0.dns.cam.ac.uk
imperial.ac.uk  nameserver = ns0.ic.ac.uk
imperial.ac.uk  nameserver = ns1.ic.ac.uk
imperial.ac.uk  nameserver = ns2.ic.ac.uk
```

（非常神奇 返回的第一项是剑桥的域名解析服务器

#### Q3 用Q2解析雅虎邮件服务器

执行命令：nslookup -type=MX yahoo.com auth0.dns.cam.ac.uk

```shell
服务器:  auth0.dns.cam.ac.uk
Address:  131.111.8.37

非权威应答:
yahoo.com       MX preference = 1, mail exchanger = mta7.am0.yahoodns.net
yahoo.com       MX preference = 1, mail exchanger = mta5.am0.yahoodns.net
yahoo.com       MX preference = 1, mail exchanger = mta6.am0.yahoodns.net
```

选择第一个服务器，继续查询。

执行命令：nslookup mta7.am0.yahoodns.net auth0.dns.cam.ac.uk

```shell
服务器:  auth0.dns.cam.ac.uk
Address:  131.111.8.37

非权威应答:
名称:    mta7.am0.yahoodns.net
Addresses:  98.136.96.76
          67.195.228.106
          67.195.228.94
          67.195.228.111
          67.195.204.72
          67.195.204.73
          98.136.96.74
          67.195.228.109
```

## （二）ipconfig

无实验

## （三）wireshark追踪DNS

### 实验过程与截图

#### Q4-Q10

清空DNS及浏览器缓存，访问指定网站。

![image-20251003113651943](assets\screenshot9.png)

使用`ipconfig /all`命令检查本地DNS服务器地址。

![image-20251002125957248](assets\screenshot10.png)

#### Q11-Q15

执行命令`nslookup www.mit.edu`：

![image-20251003122238077](assets\screenshot12.png)

wireshark获取视图如下：

![image-20251003122134345](assets\screenshot13.png)

#### Q16-Q19

执行命令`nslookup -type=NS mit.edu`：

![image-20251003130039207](assets\screenshot14.png)

wireshark获取视图如下：

![image-20251003130445834](assets\screenshot15.png)

#### Q20-Q23

一般情况会显示连接超时，以下实验通过tun模式抓虚拟网卡（singbox_tun）上的包实现：

执行命令`nslookup www.aiit.or.kr bitsy.mit.edu`：

![image-20251003133141678](assets\screenshot16.png)

wireshark获取视图如下：

![image-20251003133913728](assets\screenshot17.png)

### 问题与回答

#### Q4 DNS请求报文响应报文协议？

UDP

#### Q5 请求报文目的端口/响应报文源端口

query Dst Port：53；

response Src Port：53；

#### Q6 请求报文目的地址/本地DNS服务器地址

均为192.168.3.1

#### Q7 请求报文Type&answers？

Type: A；

不包含answers块；

#### Q8 响应报文answers数量及内容

共两条，如下：

- www.ietf.org: type A, class IN, addr 104.16.44.99

- www.ietf.org: type A, class IN, addr 104.16.45.99

#### Q9 TCP SYN包DES IP以及是否与DNS返回IP相符？

![image-20251003114002318](assets\screenshot11.png)

需要注意的是，由于analytics.ietf.org解析晚于ietf.org且两者ip相同，故wireshark维护的IPtoName解析表中104.16.44.99对应的域名被覆盖为analytics.ietf.org；

但不管怎么说，该tcp SYN包的des ip为104.16.44.99，与之前dns解析ietf.org的answers是相符的。

#### Q10 请求每次图片前发出新DNS请求？

并没有每次都发出请求，但请求ietf.svg时包括对static.ietf.org的dns解析请求，最后还有对

analytics.ietf.org的dns解析请求。

------

#### Q11 请求报文目的端口/响应报文源端口

query Dst Port：53；

response Src Port：53；

#### Q12 响应报文目的地址/本地DNS服务器地址

均为192.168.3.1

#### Q13 请求报文Type&answers？

Type: A  / AAAA（两个请求，分别是ipv4&ipv6）；

不包含answers块；

#### Q14 响应报文answers数量及内容

针对Type A的响应报文共三条ansewers：

- www.mit.edu: type CNAME, class IN, cname www.mit.edu.edgekey.net

- www.mit.edu.edgekey.net: type CNAME, class IN, cname e9566.dscb.akamaiedge.net

- e9566.dscb.akamaiedge.net: type A, class IN, addr 23.48.184.61

针对Type AAAA的响应报文共四条ansewers：

- www.mit.edu: type CNAME, class IN, cname www.mit.edu.edgekey.net

- www.mit.edu.edgekey.net: type CNAME, class IN, cname e9566.dscb.akamaiedge.net

- e9566.dscb.akamaiedge.net: type AAAA, class IN, addr 2600:140b:2:199::255e
- e9566.dscb.akamaiedge.net: type AAAA, class IN, addr 2600:140b:2:189::255e

#### Q15 截图

见上“实验过程与截图”模块Q11-Q15

------

#### Q16 请求报文目的地址/本地DNS服务器地址

均为192.168.3.1

#### Q17 请求报文Type&answers？

Type: NS；

不包含answers块；

#### Q18 响应报文answers数量及内容

共八条：

- mit.edu: type NS, class IN, ns asia1.akam.net
- mit.edu: type NS, class IN, ns use5.akam.net
- mit.edu: type NS, class IN, ns use2.akam.net
- mit.edu: type NS, class IN, ns asia2.akam.net
- mit.edu: type NS, class IN, ns usw2.akam.net
- mit.edu: type NS, class IN, ns eur5.akam.net
- mit.edu: type NS, class IN, ns ns1-37.akam.net
- mit.edu: type NS, class IN, ns ns1-173.akam.net

#### Q19 截图

见上“实验过程与截图”模块Q16-Q19

------

#### Q20 请求报文目的地址/本地DNS服务器地址

请求报文发向18.0.72.3，与该网卡对应的本地DNS服务器172.18.0.2不相符，对应的是bitsy.mit.edu的地址

#### Q21 请求报文Type&answers？

Type: A  / AAAA（两个请求，分别是ipv4&ipv6）；

不包含answers块；

#### Q22 响应报文answers数量及内容

针对Type A的响应报文共两条ansewers：

- www.aiit.or.kr: type A, class IN, addr 172.67.152.120

- www.aiit.or.kr: type A, class IN, addr 104.21.74.8

针对Type AAAA的响应报文共四条ansewers：

- www.aiit.or.kr: type AAAA, class IN, addr 2606:4700:3036::6815:4a08

- www.aiit.or.kr: type AAAA, class IN, addr 2606:4700:3031::ac43:9878

#### Q23 截图

见上“实验过程与截图”模块Q20-Q23