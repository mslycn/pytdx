
https://github.com/rainx/pytdx/issues/133

http://down.tdx.com.cn:8001/fin/gpcw20160930.zip
数据解析
~~~
004757B3  |.  8D5424 20     ||lea     edx, dword ptr [esp+0x20]
004757B7  |.  51            ||push    ecx
004757B8  |.  57            ||push    edi
004757B9  |.  68 4023A600   ||push    00A62340                                   ;  C:\Program Files\tdx\vipdoc\
004757BE  |.  68 F8968B00   ||push    008B96F8                                   ;  %scw\gpcw%04d%04d.dat
004757C3  |.  52            ||push    edx
004757C4  |.  C74424 3C 000>||mov     dword ptr [esp+0x3C], 0x0
004757CC  |.  E8 ED3B3B00   ||call    <jmp.&MFC42.#CString::Format_2818>
004757D1  |.  83C4 14       ||add     esp, 0x14
004757D4  |.  8D4C24 38     ||lea     ecx, dword ptr [esp+0x38]
004757D8  |.  E8 673D3B00   ||call    <jmp.&MFC42.#CFile::CFile_354>
004757DD  |.  8B4424 20     ||mov     eax, dword ptr [esp+0x20]
004757E1  |.  6A 00         ||push    0x0
004757E3  |.  6A 40         ||push    0x40
004757E5  |.  50            ||push    eax
004757E6  |.  8D4C24 44     ||lea     ecx, dword ptr [esp+0x44]
004757EA  |.  C64424 70 01  ||mov     byte ptr [esp+0x70], 0x1
004757EF  |.  E8 443D3B00   ||call    <jmp.&MFC42.#CFile::Open_5186>             ;  打开财报文件
004757F4  |.  85C0          ||test    eax, eax
004757F6  |.  0F84 50010000 ||je      0047594C
004757FC  |.  8D4C24 48     ||lea     ecx, dword ptr [esp+0x48]
00475800  |.  6A 14         ||push    0x14
00475802  |.  51            ||push    ecx
00475803  |.  8D4C24 40     ||lea     ecx, dword ptr [esp+0x40]
00475807  |.  E8 263D3B00   ||call    <jmp.&MFC42.#CFile::Read_5442>             ;  读取文件
0047580C  |.  8B4424 4E     ||mov     eax, dword ptr [esp+0x4E]
00475810  |.  85C0          ||test    eax, eax
00475812  |.  0F8E 2B010000 ||jle     00475943
00475818  |.  8D1480        ||lea     edx, dword ptr [eax+eax*4]
0047581B  |.  8D0450        ||lea     eax, dword ptr [eax+edx*2]
0047581E  |.  50            ||push    eax
0047581F  |.  E8 CC393B00   ||call    <jmp.&MFC42.#operator new_823>             ;  申请2b66大小的缓存
00475824  |.  8BF8          ||mov     edi, eax                                   ;  12B443C0
00475826  |.  8B4424 52     ||mov     eax, dword ptr [esp+0x52]
0047582A  |.  83C4 04       ||add     esp, 0x4
0047582D  |.  897C24 14     ||mov     dword ptr [esp+0x14], edi
00475831  |.  8D0C80        ||lea     ecx, dword ptr [eax+eax*4]
00475834  |.  8D1448        ||lea     edx, dword ptr [eax+ecx*2]
00475837  |.  8D4C24 38     ||lea     ecx, dword ptr [esp+0x38]
0047583B  |.  52            ||push    edx
0047583C  |.  57            ||push    edi
0047583D  |.  E8 F03C3B00   ||call    <jmp.&MFC42.#CFile::Read_5442>             ;  从财宝中读取2B66大小的内容，存放到刚才申请的缓存中
00475842  |.  8B4424 4E     ||mov     eax, dword ptr [esp+0x4E]
00475846  |.  33F6          ||xor     esi, esi
00475848  |.  85C0          ||test    eax, eax
0047584A  |.  0F8E E2000000 ||jle     00475932
00475850  |>  57            ||/push    edi                                       ; /s
00475851  |.  C647 06 00    |||mov     byte ptr [edi+0x6], 0x0                   ; |在股票代码末尾设置\0防止字符串解析错误
00475855  |.  FF15 98638500 |||call    dword ptr [<&MSVCRT.atol>]                ; \将股票代码转long类型
0047585B  |.  8B4C24 78     |||mov     ecx, dword ptr [esp+0x78]
0047585F  |.  83C4 04       |||add     esp, 0x4
00475862  |.  3BC1          |||cmp     eax, ecx
00475864  |.  74 11         |||je      short 00475877                            ;  查找指定的股票
00475866  |.  8B4424 4E     |||mov     eax, dword ptr [esp+0x4E]
0047586A  |.  46            |||inc     esi                                       ;  累加器统计目标股票代码的偏移数量
0047586B  |.  83C7 0B       |||add     edi, 0xB
0047586E  |.  3BF0          |||cmp     esi, eax
00475870  |.^ 7C DE         ||\jl      short 00475850
00475872  |.  E9 B7000000   ||jmp     0047592E                                   ;  如果没找到就跳走，如果找到则获取其财报信息
00475877  |>  8B4C24 14     ||mov     ecx, dword ptr [esp+0x14]                  ;  得到STOCK_ITEM的基地址
0047587B  |.  8D04B6        ||lea     eax, dword ptr [esi+esi*4]                 ;  指定股票的偏移+偏移*4
0047587E  |.  03CE          ||add     ecx, esi
00475880  |.  6A 00         ||push    0x0
00475882  |.  8B5441 07     ||mov     edx, dword ptr [ecx+eax*2+0x7]             ;  得到每只股票代码的下一个成员(offset)
00475886  |.  8B4424 28     ||mov     eax, dword ptr [esp+0x28]                  ;  得到要获取元素的偏移。
0047588A  |.  8D4C82 FC     ||lea     ecx, dword ptr [edx+eax*4-0x4]
0047588E  |.  51            ||push    ecx
0047588F  |.  8D4C24 40     ||lea     ecx, dword ptr [esp+0x40]
00475893  |.  E8 C6433B00   ||call    <jmp.&MFC42.#CFile::Seek_5773>             ;  计算其财报信息每股净收益的偏移
00475898  |.  8D5424 28     ||lea     edx, dword ptr [esp+0x28]
0047589C  |.  6A 04         ||push    0x4
0047589E  |.  52            ||push    edx
0047589F  |.  8D4C24 40     ||lea     ecx, dword ptr [esp+0x40]
004758A3  |.  E8 8A3C3B00   ||call    <jmp.&MFC42.#CFile::Read_5442>             ;  得到需要的元素
004758A8  |.  8B83 F8140000 ||mov     eax, dword ptr [ebx+0x14F8]

~~~


Python通达信数据接口
========

概述
----
使用纯Python类似TradeX的获取通达信行情接口的实现

因为之前TradeX的接口是使用Python扩展的方式调用C++代码实现的，功能上有诸多的限制，如只支持32位的Python， 不支持MacOS, Linux等， 
无奈我自己使用的电脑是Mac系统, 服务器又是基于Linux的，所以只能自己重新实现一份。
 
 声明
---
次代码用于个人对网络协议的研究和习作，不对外提供服务，任何人使用本代码遇到问题请自行解决，也可以在github提issue给我，但是我不保证能即时处理。
由于我们连接的是既有的通达信兼容行情服务器，机构请不要使用次代码，对此造成的任何问题本人概不负责。

兼容性
---
目前已经验证在如下的Python版本下成功运行，并在Windows,Mac,Linux各个系统下进行了测试

```bash
Python2.7+
Python3.6+
```

安装
--

```bash
> pip install pytdx

或者

> pip install git+https://github.com/rainx/pytdx
```

命令行
---
我提供了一个命令行工具来实现简单的交互和功能演示，在安装之后，应该可以直接使用 ```hqget``` 命令调用， hqget分为交互模式和单命令模式，
您可以随时使用 hqget --help 获取接口的使用规则。

### 交互模式

直接输入 ```hqget``` 即可进入交互模式，进入之后，先选择要连接的服务器类型，然后选择要执行的功能，选择菜单里面最后一项退出交互模式。

选择服务器
```bash
-->rainx@JingdeMacBook-Pro:~/dev/pytdx [master]$ hqget
连接中....
请选择服务器
--------------------
[1] :招商证券深圳行情 (119.147.212.81:7709)
[2] :华泰证券(南京电信) (221.231.141.60:7709)
[3] :华泰证券(上海电信) (101.227.73.20:7709)
[4] :华泰证券(上海电信二) (101.227.77.254:7709)
[5] :华泰证券(深圳电信) (14.215.128.18:7709)
[6] :华泰证券(武汉电信) (59.173.18.140:7709)
[7] :华泰证券(天津联通) (60.28.23.80:7709)
[8] :华泰证券(沈阳联通) (218.60.29.136:7709)
[9] :华泰证券(南京联通) (122.192.35.44:7709)
[10] :华泰证券(南京联通) (122.192.35.44:7709)
--------------------
请输入序号  [1]:
```
 
 选择功能
 
```bash
连接成功！
--------------------
功能列表：
1 : 获取股票行情
2 : 获取k线
3 : 获取市场股票数量
4 : 获取股票列表
5 : 获取指数k线
6 : 查询分时行情
7 : 查询历史分时行情
8 : 查询分笔成交
9 : 查询历史分笔成交
10 : 查询公司信息目录
11 : 读取公司信息详情
12 : 读取除权除息信息
13 : 读取财务信息
14 : 退出断开连接
--------------------
请输入要使用的功能:
```

输入参数并获取结果

```bash

参数：市场代码， 股票代码， 如： 0,000001 或 1,600300
请输入参数  [0,000001]:
--------------------
   market    code  active1  price  last_close  open  high   low  \
0       0  000001     2801   9.18        9.25  9.23  9.27  9.16

       reversed_bytes0  reversed_bytes1   ...     ask5  bid_vol5  ask_vol5  \
0  [178, 174, 231, 12]             -918   ...     9.23      4171      6140

   reversed_bytes4  reversed_bytes5  reversed_bytes6  reversed_bytes7  \
0             5689                1               17               82

   reversed_bytes8  reversed_bytes9  active2
0               21            65526     2801

[1 rows x 44 columns]
```

输出结果默认会使用pandas Dataframe格式输出，在内容较多时会省略部分列或行的记录，这个时候可以使用 ```--no-df``` 参数，让其用原始数据格式输出。

如启动时

```bash
> hqget --no-df
```
然后进行之前的操作，结果为：

```python
参数：市场代码， 股票代码， 如： 0,000001 或 1,600300
请输入参数  [0,000001]:
--------------------
[OrderedDict([('market', 0),
              ('code', '000001'),
              ('active1', 2864),
              ('price', 9.19),
              ('last_close', 9.25),
              ('open', 9.23),
              ('high', 9.27),
              ('low', 9.16),
              ('reversed_bytes0', bytearray(b'\xbd\xc9\xec\x0c')),
              ('reversed_bytes1', -919),
              ('vol', 428899),
              ('cur_vol', 30),
              ('amount', 395218880.0),
              ('s_vol', 284703),
              ('b_vol', 144196),
              ('reversed_bytes2', 1),
              ('reversed_bytes3', 698),
              ('bid1', 9.18),
              ('ask1', 9.19),
              ('bid_vol1', 1078),
              ('ask_vol1', 5236),
              ('bid2', 9.17),
              ('ask2', 9.2),
              ('bid_vol2', 8591),
              ('ask_vol2', 3027),
              ('bid3', 9.16),
              ('ask3', 9.21),
              ('bid_vol3', 12638),
              ('ask_vol3', 3557),
              ('bid4', 9.15),
              ('ask4', 9.22),
              ('bid_vol4', 13234),
              ('ask_vol4', 2615),
              ('bid5', 9.14),
              ('ask5', 9.23),
              ('bid_vol5', 5377),
              ('ask_vol5', 6033),
              ('reversed_bytes4', 5768),
              ('reversed_bytes5', 1),
              ('reversed_bytes6', 16),
              ('reversed_bytes7', 83),
              ('reversed_bytes8', 20),
              ('reversed_bytes9', 0),
              ('active2', 2864)])]
```

### 单命令模式

脚本也可以使用命令模式进行， 这个时候，需要通过输入 ```-f/--function``` 参数来选择要执行的命令

如：

```bash
> hqget -f 1 
```

### 保存文件
在但命令模式下，可以通过设定 ```-o/--output``` 参数来选择将命令结果保存到文件中，这个时候根据 ```--df/--no-df``` 参数的结果不同，会保存为不同的格式，
如果没有设置或者设置为 ```--df```, 则通过pandas Dataframe保存为csv格式，如果选择了 ```--no-df``` 则把结果保存为Python Pickle序列化的格式。

### 默认连接服务器
我们可以通过设定选项 ```-s/--server```来指定其默认连接的服务器，格式是 [ip]:[port], 如：

```bash
> hqget -f 1 -s 119.147.212.81:7709
```


接口API
---
下面是如何在程序里面调用本接口

首先需要引入

```python
from pytdx.hq import TdxHq_API
```

然后，创建对象

```python
api = TdxHq_API()
```

之后，通常是如下的格式

```python
if api.connect('119.147.212.81', 7709):
    # ... same codes...
    api.disconnect()

```

当然，我们也支持with 语法,可以省略```disconnect()```语句

```python
with api.connect('119.147.212.81', 7709):
    # some codes
```

我们的数据获取届接口一般返回list结构，如果需要转化为pandas Dataframe接口，可以使用 ```api.to_df``` 进行转化
如：
```python
data = api.get_security_bars(9, 0, '000001', 0, 10) #返回普通list
data = api.to_df(api.get_security_bars(9, 0, '000001', 0, 10)) # 返回DataFrame
```

可以使用的api方法有下列的几个。

### api方法列表

#### 参数一般性约定

一般来说，股票代码和文件名称使用字符串类型，其它参数都使用数值类型


#### 1 : 获取股票行情
可以获取**多**只股票的行情信息

需要传入一个列表，每个列表由一个市场代码， 一个股票代码构成的元祖构成
```[ (市场代码1， 股票代码1)，(市场代码2， 股票代码2) ... (市场代码n， 股票代码n) ]```

如：
```python
api.get_security_quotes([(0, '000001'), (1, '600300')])
```

#### 2 : 获取k线

* category-> 
```
K线种类
0 5分钟K线 1 15分钟K线 2 30分钟K线 3 1小时K线 4 日K线
5 周K线
6 月K线
7 1分钟
8 1分钟K线 9 日K线
10 季K线
11 年K线
```
* market -> 市场代码 0:深圳，1:上海
* stockcode -> 证券代码;
* start -> 指定的范围开始位置;
* count -> 用户要请求的 K 线数目，最大值为 800

如： 

```python
api.get_security_bars(9,0, '000001', 4, 3)
```

#### 3 : 获取市场股票数量
0 - 深圳， 1 - 上海
```python
api.get_security_count(0)
```
#### 4 : 获取股票列表
参数：市场代码, 起始位置， 数量  如： 0,0 或 1,100

```python
api.get_security_list(1, 0)
```

#### 5 : 获取指数k线
* category-> 
```
K线种类
0 5分钟K线 1 15分钟K线 2 30分钟K线 3 1小时K线 4 日K线
5 周K线
6 月K线
7 1分钟
8 1分钟K线 9 日K线
10 季K线
11 年K线
```
* market -> 市场代码 0:深圳，1:上海
* stockcode -> 证券代码;
* start -> 指定的范围开始位置;
* count -> 用户要请求的 K 线数目，最大值为 800

如： 

```python
api.get_index_bars(9,1, '000001', 1, 2)
```
#### 6 : 查询分时行情
参数：市场代码， 股票代码， 如： 0,000001 或 1,600300
```python
api.get_minute_time_data(1, '600300')
```
#### 7 : 查询历史分时行情
参数：市场代码， 股票代码，时间 如： 0,000001,20161209 或 1,600300,20161209
```python
api.get_history_minute_time_data(TDXParams.MARKET_SH, '600300', 20161209)
```

注意，在引入 TDXParams 之后， （```from pytdx.params import TDXParams```）
我们可以使用 TDXParams.MARKET_SH , TDXParams.MARKET_SZ 常量来代替 1 和 0 作为参数

#### 8 : 查询分笔成交

参数：市场代码， 股票代码，起始位置， 数量 如： 0,000001,0,10
```python
api.get_transaction_data(TDXParams.MARKET_SZ, '000001', 0, 30)
```

#### 9 : 查询历史分笔成交

参数：市场代码， 股票代码，起始位置，日期 数量 如： 0,000001,0,10,20170209

```python
api.get_history_transaction_data(TDXParams.MARKET_SZ, '000001', 0, 10, 20170209)
```
#### 10 : 查询公司信息目录
参数：市场代码， 股票代码， 如： 0,000001 或 1,600300
```python
api.get_company_info_category(TDXParams.MARKET_SZ, '000001')
```

#### 11 : 读取公司信息详情

参数：市场代码， 股票代码, 文件名, 起始位置， 数量, 如：0,000001,000001.txt,2054363,9221
```python
api.get_company_info_content(0, '000001', '000001.txt', 0, 100)
```

注意这里的 起始位置， 数量 参考上面接口的返回结果。

#### 12 : 读取除权除息信息
参数：市场代码， 股票代码， 如： 0,000001 或 1,600300
```python
api.get_xdxr_info(1, '600300')
```

#### 13 : 读取财务信息
参数：市场代码， 股票代码， 如： 0,000001 或 1,600300
```python
api.get_finance_info(0, '000001')
```

### 多线程支持

由于Python的特性，一般情况下，不太建议使用多线程代码，如果需要并发访问，建议使用多进程来实现，如果要使用多线程版本，请在初始化时设置multithread参数为True

```python
api = TdxHq_API(multithread=True)
```

### 调试模式

如果您需要调试本代码，监控传输过程中的数据包传输情况，可以使用调试模式，使用方法是设定环境变量 TDX_DEBUG 为 1 如

```bash
> TDX_DEBUG=1 hqget -f 1 
```

## 其它

欢迎对量化交易感兴趣的朋友互相交流，可以来我们的智矿社区看看 http://zhikuang.org
