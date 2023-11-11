# Buu java逆向解密

先丢进查壳器

![](G:\IDA\wp截图\Java1.png)

class文件Java写的，丢进ida好像只能看到汇编，所以丢进idax_gui

![](G:\IDA\wp截图\java.png)

真舒服，这道题应该就是叫你看懂Java

先加@的ASCII值

然后和32异或

写一个逆向脚本

```
list = [180, 136, 137, 147, 191, 137, 147, 191, 148, 136, 133, 191, 134, 140, 129, 135, 191, 65]
flag = ''
for i in list :
    i ^= 32
    i -= ord('@')
    flag += chr(i)
print(flag)
```

就可以看到flag{This_is_the_flag_!}

