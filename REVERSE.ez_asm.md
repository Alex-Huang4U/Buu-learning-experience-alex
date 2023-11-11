姓名：黄子權

昵称：teach_me_re

学号：2023280288

邮箱：2482771965@qq.com

## **REVERSE**

### [easy]ez_asm | 卡住了，以及复现

![image-20231027152901592](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231027152901592.png)

这是便学习汇编边翻译出来的解释

大概就是把列表中位次小于35的数都与0EC进行一次异或，加一个0x10

再文本的最下面也给了相应的数组

![image-20231027153211140](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231027153211140.png)

根据这个和之前的汇编就可以写出加密的脚本

```
flag = [0xbd, 0xa9, 0xae, 0x93, 0xae, 0x9d, 0xa7, 0x93, 0x94, 0xc3, 
        0xa5, 0xec, 0xa9, 0xc3, 0x97, 0x92, 0xb3, 0xab, 0xc3, 0xab, 
        0x94, 0x9d, 0xa8, 0xc3, 0xed, 0xaf, 0xc3, 0x9d, 0xaf, 0x91,
        0xdd, 0xdd, 0xdd, 0xa1]
for i in range(0,34) :
    flag[i] -= 0x10
    flag[i] ^= 0xEC

    print(chr(flag[i]),end='')
```

![image-20231027155101131](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231027155101131.png)

也就获得答案啦

Aurora{oh_y0u_knOw_what_1s_asm!!!}