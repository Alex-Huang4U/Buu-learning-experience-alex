姓名：黄子權

昵称：teach_me_re

学号：2023280288

邮箱：2482771965@qq.com

## **REVERSE**

### [easy]148464 | 卡住了，以及复现

先将文件丢进ExeinfoPe查壳

![](G:\IDA\wp截图\屏幕截图 2023-10-23 124932.png)

64位，丢进ida，shift f12查看字符

![](G:\IDA\wp截图\屏幕截图 2023-10-23 125138.png)

发现与flag相关的字符，同时发现最底下有个字符串，一眼顶针是base64的表

同时在图片所选字符的下面一行的字符看起来应该就是需要我们解密的字符了

接下来点进去查看

![](G:\IDA\wp截图\屏幕截图 2023-10-23 125229.png)

直接f5看伪代码

这一看确实是经过了一段加密，流程为：

输入一段字符，经过加密表加密后赋值到s1，与硬编码 "zMXHz9T6sdfZx8LtxZn0x8jHC8vFnJr3"进行比较，一致即为flag。

其中关键为函数 sub_1234()和sub_12F0

 sub_1234()应该是对加密表做出了改动

![image-20231023130710617](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231023130710617.png)

sub_12F0则是对输入的s进行了操作

![image-20231023130729915](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231023130729915.png)

结果由于 sub_1234()函数分析有点问题

![](G:\IDA\wp截图\屏幕截图 2023-10-26 172712.png)

没分析出sub_1169是swap函数

```c伪代码
_BYTE *__fastcall sub_1169(__int64 a1, __int64 a2)
{
  _BYTE *result; // rax//定义一个指针变量
  __int64 savedregs; // [rsp+0h] [rbp+0h]//声明一个64位整数

  *(&savedregs - 3) = a1;//将a1放进这个地址
  *(&savedregs - 4) = a2;//将a2放进这个地址
  *((_BYTE *)&savedregs - 1) = *(_BYTE *)*(&savedregs - 3);//将a1放进一个临时地址
  *(_BYTE *)*(&savedregs - 3) = *(_BYTE *)*(&savedregs - 4);//将a2放进a1原本的地址
  result = (_BYTE *)*(&savedregs - 4);//将a2的值赋值给result
  *result = *((_BYTE *)&savedregs - 1);//将a1的值赋值给result指向的值
  return result;
}
```

后来问了一下大佬们，发现是我打开IAD的时候没有选择生成栈上的变量导致反汇编成了这样（少了一个将放在中间变量中的a1的值赋值给a2的语句）

![](G:\IDA\wp截图\微信图片_20231029105512.png)

正确打开后

![](G:\IDA\wp截图\微信图片_20231029105528.png)

可以很明显看出就是个交换函数

再换回原本的窗口分析，就可以写出把表换回来的脚本（这里用的python）



![image-20231023130710617](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231023130710617.png)

```python3
word = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/"
key = [i for i in word]
def swap(a,b) :
    return b,a


for j in range(0,26) :
    key[j] , key[j + 26] = swap(key[j],key[j + 26])
for k in range(0,6) :
    key[k + 52], key[k + 58] = swap(key[k + 52], key[k + 58])

for i in key :
    print(i,end= '')
```

然后将输出和硬编码丢进cyberchef解码即可获得flag

![image-20231027144846089](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231027144846089.png)

也就获得正确的flag啦

### [easy]148464 | 已复现