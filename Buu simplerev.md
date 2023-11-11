# Buu simplerev

先丢进查壳器

![](G:\IDA\wp截图\simplerev.png)

看到是64位后丢进ida64

F12查看关键字符，直接进入检验正确后庆祝语的位置

![](G:\IDA\wp截图\sp1.png)

然后进入后分析，由于输入与对比对象都是字符，所以将十六进制都转换为对应ASCII的字符

![](G:\IDA\wp截图\spp.png)

在这里我们可以看到两个字符串

分别为text和key，对应值也在图中标出了

这里学习了一个函数strcat是将两个字符串尾连接

![](G:\IDA\wp截图\小端序.png)

这里要注意src和v9是小端序，所以在读取的时候要反过来读(看到有点奇怪的英文都可以去看看汇编码看看地址确认读取方式)

![](E:\OneDrive\图片\Screenshots\大端序.png)

正常的大端序地址高位数低地址存放的如上图

相关内容在[大端序和小端序_大端序和小端序的区别-CSDN博客](https://blog.csdn.net/Casuall/article/details/98481469)可以看到

然后再将key全部转为小写

![](G:\IDA\wp截图\simp3.png)

然后可以看到这个最后的检验，首先key[v3%v5]这种写法其实也是遍历数组而已，一种看起来复杂的写法

意思就是对满足条件的字符丢进一个包含key等式之中使运算后等式等于text，将等式逆向解出v1即可，

在这里我选择了硬解，求余我是用法逆运算发现有点问题

```
str2 = [i for i in "adsfkndcls"]
KEY = [i for i in "killshadow"]
flag = ''
temp = 0
for i in range(0,len(KEY)) :
        for v1 in range(0,128) :
                if chr(v1).isalpha():
                        temp = ((v1 - 39 - ord(str2[i]) + 97) % 26 + 97)
                        if KEY[i] == chr(temp) :
                                flag += chr(v1)
                                break
print(flag)


```

这里首先是发现这种硬解的写法

然后使查了一个检验字符的操作 if chr(v1).isalpha():可以有效防止输出乱码

解出flag{KLDQCUDFZO}