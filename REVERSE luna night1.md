姓名：黄子權

昵称：teach_me_re

学号：2023280288

邮箱：2482771965@qq.com

## **REVERSE**

### [easy]luna night1| 卡住了，以及复现

先查壳

![](G:\IDA\wp截图\屏幕截图 2023-10-29 114111.png)

64位，丢进ida

shift 12查看字符

![](G:\IDA\wp截图\屏幕截图 2023-10-29 114239.png)

看到似乎是判断结束的语句，点进去后F5查看伪代码

![](G:\IDA\wp截图\屏幕截图 2023-10-29 112655.png)

输入一个长度为36的字符串，然后丢进一个循环进行判断

![](G:\IDA\wp截图\屏幕截图 2023-10-29 112712.png)

瞪眼一看这个switch中的四种选择就像是某种移动方式，同时下面这个数组是一维数组的二维表示方式，基本可以确定就是迷宫题，数组信息也说明这是一个7*7=49的迷宫，那么现在就开始找到迷宫的字符打印出来走迷宫就行了

![](G:\IDA\wp截图\屏幕截图 2023-10-29 113925.png)

把这个玩意按照迷宫格式打印

```
map = [i for i in "xxxxxxxx...xxxxxx.xxxxx..xxxxx.xxxxxx.xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx..xxxxxx.xxx....xxxxxxxxxxxxxxxxx...xxxx.x.xxxx.x.xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.xxxxxx..xxxxxx...xxxxxxxxxxxxxxxx#....xxxxxx.xxxxxx.xxxxxx.xxxxxx.xxxxxxxx"]
for i in range(0,len(map)-1):
    if i%7 == 0 :
        print(f"\n{map[i]}",end='')
    else:
        print(map[i],end='')
```

得到

![](G:\IDA\wp截图\屏幕截图 2023-10-29 113853.png)

然后走迷宫就可以得到

flag为Aurora{ddssassxdddwwaxwwaassxsdsddxwwwwaaaa}