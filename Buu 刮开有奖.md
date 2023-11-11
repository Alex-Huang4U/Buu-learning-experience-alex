姓名：黄子權

ID：Severus Alex

邮箱：2482771965@qq.com

# Buu 刮开有奖

首先我们打开程序看下有什么线索

![](G:\IDA\buu截图\屏幕截图 2023-11-09 164553.png)

发现也没啥玄机，就只有四个字啥用没有

丢进查壳器

![](G:\IDA\buu截图\屏幕截图 2023-11-09 164739.png)

32位无壳，丢进ida

![](G:\IDA\buu截图\屏幕截图 2023-11-09 164916.png)

发现一个dialogboxparaA，上面有一个dialogfunc看名字是个函数，点进跟进

![](G:\IDA\buu截图\屏幕截图 2023-11-09 164932.png)

这么一看应该就是主程序了，反汇编后看伪代码如下

或者查看字符，发现一个可疑的玩意，跟进一下也可以到主程序

![](G:\IDA\buu截图\屏幕截图 2023-11-09 192415.png)

反汇编后看伪代码如下

看到输入函数知道flag长度为8

```
INT_PTR __stdcall DialogFunc(HWND hDlg, UINT a2, WPARAM a3, LPARAM a4)
{
  const char *v4; // esi
  const char *v5; // edi
  int v7[2]; // [esp+8h] [ebp-20030h] BYREF
  int v8; // [esp+10h] [ebp-20028h]
  int v9; // [esp+14h] [ebp-20024h]
  int v10; // [esp+18h] [ebp-20020h]
  int v11; // [esp+1Ch] [ebp-2001Ch]
  int v12; // [esp+20h] [ebp-20018h]
  int v13; // [esp+24h] [ebp-20014h]
  int v14; // [esp+28h] [ebp-20010h]
  int v15; // [esp+2Ch] [ebp-2000Ch]
  int v16; // [esp+30h] [ebp-20008h]
  CHAR String[65536]; // [esp+34h] [ebp-20004h] BYREF
  char v18[65536]; // [esp+10034h] [ebp-10004h] BYREF

  if ( a2 == 272 )
    return 1;
  if ( a2 != 273 )
    return 0;
  if ( (_WORD)a3 == 1001 )
  {
    memset(String, 0, 0xFFFFu);
    GetDlgItemTextA(hDlg, 1000, String, 0xFFFF);
    if ( strlen(String) == 8 )          //这个应该是标明flag的输入长度为8
    {
      v7[0] = 90;
      v7[1] = 74;
      v8 = 83;
      v9 = 69;
      v10 = 67;
      v11 = 97;
      v12 = 78;
      v13 = 72;
      v14 = 51;
      v15 = 110;
      v16 = 103;
      sub_4010F0(v7, 0, 10);
      memset(v18, 0, 0xFFFFu);
      v18[0] = String[5];
      v18[2] = String[7];
      v18[1] = String[6];
      v4 = (const char *)sub_401000(v18, strlen(v18));
      memset(v18, 0, 0xFFFFu);
      v18[1] = String[3];
      v18[0] = String[2];
      v18[2] = String[4];
      v5 = (const char *)sub_401000(v18, strlen(v18));
      if ( String[0] == v7[0] + 34
        && String[1] == v10
        && 4 * String[2] - 141 == 3 * v8
        && String[3] / 4 == 2 * (v13 / 9)
        && !strcmp(v4, "ak1w")
        && !strcmp(v5, "V1Ax") )
      {
        MessageBoxA(hDlg, "U g3t 1T!", "@_@", 0);
      }
    }
    return 0;
  }
  if ( (_WORD)a3 != 1 && (_WORD)a3 != 2 )
    return 0;
  EndDialog(hDlg, (unsigned __int16)a3);
  return 1;
}
```

看到最后的条件判断if中有六个条件，根据这六个条件应该就能逆向出flag

往上看其中的参数是否被加密

```
int v7[2]; // [esp+8h] [ebp-20030h] BYREF
  int v8; // [esp+10h] [ebp-20028h]
  int v9; // [esp+14h] [ebp-20024h]
  int v10; // [esp+18h] [ebp-20020h]
  int v11; // [esp+1Ch] [ebp-2001Ch]
  int v12; // [esp+20h] [ebp-20018h]
  int v13; // [esp+24h] [ebp-20014h]
  int v14; // [esp+28h] [ebp-20010h]
  int v15; // [esp+2Ch] [ebp-2000Ch]
  int v16; // [esp+30h] [ebp-20008h]
```

首先是这一堆连续的v几，因为地址是连续的所以是数组，而在 sub_4010F0函数输入参数时使用数组的第一个元素的地址作为索引也符合数组的用法

因此我们可以将其定义为一个字符数组str

```
      v7[0] = 90;
      v7[1] = 74;
      v8 = 83;
      v9 = 69;
      v10 = 67;
      v11 = 97;
      v12 = 78;
      v13 = 72;
      v14 = 51;
      v15 = 110;
      v16 = 103;
```

将下面所赋值的数转换为对应ASCII字符后，放入字符数组可得到

```
 char str[] = "ZJSECaNH3ng";
```

通过刚才的观察我们知道有两个未知函数

sub_4010F0以及sub_401000

先跟进sub_4010F0

![](G:\IDA\buu截图\屏幕截图 2023-11-09 180050.png)

有点复杂，直接分析时有一点困难了，所以干脆直接翻译成c运行下看结果

```
#include<stdio.h>
#include<string.h>
int main()
{
    char str[] = "ZJSECaNH3ng";
    sub_4010F0(str, 0, 10);
    printf("%s", str);
    return 0;
}

int sub_4010F0(char* a1, int a2, int a3)
{
    int result; // eax
    int i; // esi
    int v5; // ecx
    int v6; // edx

    result = a3;
    for (i = a2; i <= a3; a2 = i)
    {
        v5 = i;
        v6 = a1[i];
        if (a2 < result && i < result)
        {
            do
            {
                if (v6 >(a1[result]))
                {
                    if (i >= result)
                        break;
                    ++i;
                    a1[v5] = a1[result];
                    if (i >= result)
                        break;
                    while (a1[i] <= v6)
                    {
                        if (++i >= result)
                            goto LABEL_13;
                    }
                    if (i >= result)
                        break;
                    v5 = i;
                    a1[result] = a1[i];
                }
                --result;
            } while (i < result);
        }
    LABEL_13:
        a1[result] = v6;
        sub_4010F0(a1, a2, i - 1);
        result = a3;
        ++i;
    }
    return result;
}
```

类似于 *(_DWORD *)(4 * i + a1)这种表达，实际上是一种数组的汇编表达

4指相应数据类型长度，所以4*i就是指数组往下数第几个值（偏移量），a1就是数组名，在这里表示数组第一个元素的初始位置

综上就可以将该形式改成a1[i]这样的C语言表达，根据这个原理进行修改，再结合前面得出的字符数组，得到上述代码，得出

![](G:\IDA\buu截图\屏幕截图 2023-11-09 181011.png)

加密后的字符数组为3CEHJNSZagn

![](G:\IDA\buu截图\屏幕截图 2023-11-09 181324.png)

接下来就看看下一个加密函数sub_401000

可以看出就是对丢进去的第345加密赋值给v5，对678加密赋值给v4(数组下标加一)

看看加密函数

```
_BYTE *__cdecl sub_401000(int a1, int a2)
{
  int v2; // eax
  int v3; // esi
  size_t v4; // ebx
  _BYTE *v5; // eax
  _BYTE *v6; // edi
  int v7; // eax
  _BYTE *v8; // ebx
  int v9; // edi
  int v10; // edx
  int v11; // edi
  int v12; // eax
  int i; // esi
  _BYTE *result; // eax
  _BYTE *v15; // [esp+Ch] [ebp-10h]
  _BYTE *v16; // [esp+10h] [ebp-Ch]
  int v17; // [esp+14h] [ebp-8h]
  int v18; // [esp+18h] [ebp-4h]

  v2 = a2 / 3;
  v3 = 0;
  if ( a2 % 3 > 0 )
    ++v2;
  v4 = 4 * v2 + 1;
  v5 = malloc(v4);
  v6 = v5;
  v15 = v5;
  if ( !v5 )
    exit(0);
  memset(v5, 0, v4);
  v7 = a2;
  v8 = v6;
  v16 = v6;
  if ( a2 > 0 )
  {
    while ( 1 )
    {
      v9 = 0;
      v10 = 0;
      v18 = 0;
      do
      {
        if ( v3 >= v7 )
          break;
        ++v10;
        v9 = *(unsigned __int8 *)(v3 + a1) | (v9 << 8);
        ++v3;
      }
      while ( v10 < 3 );
      v11 = v9 << (8 * (3 - v10));
      v12 = 0;
      v17 = v3;
      for ( i = 18; i > -6; i -= 6 )
      {
        if ( v10 >= v12 )
        {
          *((_BYTE *)&v18 + v12) = (v11 >> i) & 0x3F;
          v8 = v16;
        }
        else
        {
          *((_BYTE *)&v18 + v12) = 64;
        }
        *v8++ = byte_407830[*((char *)&v18 + v12++)];
        v16 = v8;
      }
      v3 = v17;
      if ( v17 >= a2 )
        break;
      v7 = a2;
    }
    v6 = v15;
  }
  result = v6;
  *v8 = 0;
  return result;
}
```

又特么好离谱，不过经过一会儿的摸索，找到了byte_407830这个玩意的值

![](G:\IDA\buu截图\屏幕截图 2023-11-09 183558.png)

看这个表一眼base64加密

在观察下面得到对比对象

![](G:\IDA\buu截图\屏幕截图 2023-11-09 183807.png)

分别是ak1w和V1Ax

直接放进cyberchef解密

![](G:\IDA\buu截图\屏幕截图 2023-11-09 183957.png)

![](G:\IDA\buu截图\屏幕截图 2023-11-09 183929.png)

得到第输入得flag的第3到第8位为WP1jMp

接下来继续看最后的条件判断

![](G:\IDA\buu截图\屏幕截图 2023-11-09 184302.png)

对于第一个字符string[0]我们进行运算

在加密后v7对应3CEHJNSZagn中的3，因为是字符串将3转化成对应的ASCII值51

得51 + 34 = 85 为U

对于第二个字符string[1]我们看到他就为加密后的v10

所以v10为3CEHJNSZagn中的J（因为v7占两位）

结合上面得出的3至8位WP1jMp

得出flag{UJWP1jMp}