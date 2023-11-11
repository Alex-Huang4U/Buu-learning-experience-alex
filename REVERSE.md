姓名：黄子權

昵称：teach_me_re

学号：2023280288

邮箱：2482771965@qq.com

## **REVERSE**

### [easy]消失的原神 | 已完成

先将文件丢进ExeinfoPe查壳

![image-20231023124305687](C:\Users\86136\AppData\Roaming\Typora\typora-user-images\image-20231023124305687.png)

可以看到为64位无壳



然后我们将其丢进ida，shift f12查看字符串

![](G:\IDA\wp截图\屏幕截图 2023-10-23 123037.png)

可以直接看到个格式特别想flag的玩意（其实下一行有个flag，被骗了一次）

点进去查看

![](G:\IDA\wp截图\屏幕截图 2023-10-23 123116.png)

长这样确实就是flag无疑了

那么flag自然就是Aurora{D0_u_P1a_G3nsh1n_iMpact}

### [easy]消失的原神 | 已复现