---
title: 算法1
date: 2020-04-08 16:35:52
tags: 
- (宽)字符集操作函数 
- unicode编码 
- 字符转换 
- 最小圆覆盖算法 
- 算法
categories: c/c++
---

c字符转换和最小覆盖圆算法

<!-- more -->



# 算法1

## C语言:宽字符集操作函数(unicode编码)

### 字符分类： 

宽字符函数普通C函数描述   
iswalnum() isalnum() 测试字符是否为数字或字母   
iswalpha() isalpha() 测试字符是否是字母   
iswcntrl() iscntrl() 测试字符是否是控制符   
iswdigit() isdigit() 测试字符是否为数字   
iswgraph() isgraph() 测试字符是否是可见字符   
iswlower() islower() 测试字符是否是小写字符   
iswprint() isprint() 测试字符是否是可打印字符   
iswpunct() ispunct() 测试字符是否是标点符号   
iswspace() isspace() 测试字符是否是空白符号   
iswupper() isupper() 测试字符是否是大写字符   
iswxdigit() isxdigit()测试字符是否是十六进制的数字   

### 大小写转换：   

宽字符函数 普通C函数描述   
towlower() tolower() 把字符转换为小写   
towupper() toupper() 把字符转换为大写   

### 字符比较： 

宽字符函数普通C函数描述   
wcscoll() strcoll() 比较字符串   

### 日期和时间转换：   

宽字符函数描述   
strftime() 根据指定的字符串格式和locale设置格式化日期和时间   
wcsftime() 根据指定的字符串格式和locale设置格式化日期和时间， 并返回宽字符串   
strptime() 根据指定格式把字符串转换为时间值， 是strftime的反过程  

### 打印和扫描字符串：   

宽字符函数描述   
fprintf()/fwprintf() 使用vararg参量的格式化输出   
fscanf()/fwscanf() 格式化读入   
printf() 使用vararg参量的格式化输出到标准输出   
scanf() 从标准输入的格式化读入   
sprintf()/swprintf() 根据vararg参量表格式化成字符串   
sscanf() 以字符串作格式化读入   
vfprintf()/vfwprintf() 使用stdarg参量表格式化输出到文件   
vprintf() 使用stdarg参量表格式化输出到标准输出   
vsprintf()/vswprintf() 格式化stdarg参量表并写到字符串   

### 数字转换：   

宽字符函数 普通C函数描述   
wcstod() strtod() 把宽字符的初始部分转换为双精度浮点数   
wcstol() strtol() 把宽字符的初始部分转换为长整数   
wcstoul() strtoul() 把宽字符的初始部分转换为无符号长整数   

### 多字节字符和宽字符转换及操作：   

宽字符函数描述   
mblen() 根据locale的设置确定字符的字节数   
mbstowcs() 把多字节字符串转换为宽字符串   
mbtowc()/btowc() 把多字节字符转换为宽字符   
wcstombs() 把宽字符串转换为多字节字符串   
wctomb()/wctob() 把宽字符转换为多字节字符  

### 输入和输出：   

宽字符函数 普通C函数描述   
fgetwc() fgetc() 从流中读入一个字符并转换为宽字符   
fgetws() fgets() 从流中读入一个字符串并转换为宽字符串   
fputwc() fputc() 把宽字符转换为多字节字符并且输出到标准输出   
fputws() fputs() 把宽字符串转换为多字节字符并且输出到标准输出串   
getwc() getc() 从标准输入中读取字符， 并且转换为宽字符   
getwchar() getchar() 从标准输入中读取字符， 并且转换为宽字符   
None gets() 使用fgetws()   
putwc() putc() 把宽字符转换成多字节字符并且写到标准输出   
putwchar() putchar() 把宽字符转换成多字节字符并且写到标准输出   
None puts() 使用fputws()   
ungetwc() ungetc() 把一个宽字符放回到输入流中   

###　字符串操作：   

宽字符函数 普通C函数描述   
wcscat() strcat() 把一个字符串接到另一个字符串的尾部   
wcsncat() strncat() 类似于wcscat()， 而且指定粘接字符串的粘接长度.   
wcschr() strchr() 查找子字符串的第一个位置   
wcsrchr() strrchr() 从尾部开始查找子字符串出现的第一个位置   
wcspbrk() strpbrk() 从一字符字符串中查找另一字符串中任何一个字符第一次出现的位置  
wcswcs()/wcsstr() strchr() 在一字符串中查找另一字符串第一次出现的位置   
wcscspn() strcspn() 返回不包含第二个字符串的的初始数目   
wcsspn() strspn() 返回包含第二个字符串的初始数目   
wcscpy() strcpy() 拷贝字符串   
wcsncpy() strncpy() 类似于wcscpy()， 同时指定拷贝的数目   
wcscmp() strcmp() 比较两个宽字符串   
wcsncmp() strncmp() 类似于wcscmp()， 还要指定比较字符字符串的数目   
wcslen() strlen() 获得宽字符串的数目   
wcstok() strtok() 根据标示符把宽字符串分解成一系列字符串   
wcswidth() None 获得宽字符串的宽度   
wcwidth() None 获得宽字符的宽度   
另外还有对应于memory操作的 wmemcpy()， wmemchr()， wmemcmp()， wmemmove()， wmemset()

## 最小圆覆盖算法

### 算法目的：

最小圆覆盖算法可以在线性时间复杂度内求出覆盖n个点的最小圆

### 算法步骤： 

1. 首先现将所有点随机排列
2. 按顺序把点一个一个的加入（一步一步的求前i个点的最小覆盖圆），每加入一个点就进入3
3. 如果发现当前i号点在当前的最小圆的外面，那么说明点i一定在前i个点的最小覆盖圆边界上，我们转到4来进一步确定这个圆，否则前i个点的最小覆盖圆与前i-1个点的最小覆盖圆是一样的，则不需要更新，返回2
4. 此时已经确认点i一定在前i个点的最小覆盖圆的边界上了，那么我们可以把当前圆的圆心设为第i个点，半径为0，然后重新把前i-1个点加入这个圆中（类似上面的步骤，只不过这次我们提前确定了点i在圆上，目的是一步一步求出包含点i的前j个点的最小覆盖圆），每加入一个点就进入5
5. 如果发现当前j号点在当前的最小圆的外面，那么说明点j也一定在前j个点（包括i）的最小覆盖圆边界上，我们转到⑥来再进一步确定这个圆，否则前j个点（包括i）的最小覆盖圆与前i-1个点（包括i）的最小覆盖圆是一样的，则不需要更新，返回4
6. 此时已经确认点i，j一定在前j个点（包括i）的最小覆盖圆的边界上了，那么我们可以把当前圆的圆心设为第i个点与第j的点连线的中点，半径为到这两个点的距离（就是找一个覆盖这两个点的最小圆），然后重新把前j-1个点加入这个圆中（还是类似上面的步骤，只不过这次我们提前确定了两个点在圆上，目的是求出包含点i，j的前k个点的最小覆盖圆），每加入一个点就进入7
7. 如果发现当前k号点在当前的最小圆的外面，那么说明点k也一定在前k个点（包括i，j）的最小覆盖圆边界上，我们不需要再进一步确定这个圆了（因为三个点能确定一个圆！），直接求出这三点共圆，否则前k个点（包括i，j）的最小覆盖圆与前k-1个点（包括i，j）的最小覆盖圆是一样的，则不需要更新。

时间复杂度：O(N)

空间复杂度：O(N)

复杂度证明：空间复杂度显然，下面来证时间复杂度

首先，因为我们已经将点打乱顺序，所以我们可以认为点是随机生成的

我们知道，当点完全随机时，第i个点在前i-1个点的最小覆盖圆外的几率是3/i  
证明：我们先随机生成i个点，求出他们的最小覆盖圆，我们可以认为这个圆是由现在在边界上的那三个点来确定的，也就是说当随机生成的最后一个点不是那三个点时，新生成的点就在圆内，否则在圆外，所以第i个点在前i-1个点的最小覆盖圆外的概率只有3/i

```c
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cmath>
#define N 100010
using namespace std;
struct node{double x,y;}b[N];
node O;
double R;
double sqr(double x){return x*x;}
double dis(node x,node y)
{
    return sqrt(sqr(x.x-y.x)+sqr(x.y-y.y));
}
bool incircle(node x)
{
    if(dis(O,x)<=R) return true;
    return false;
}
node solve(double a,double b,double c,double d,double e,double f)
{
    double y=(f*a-c*d)/(b*d-e*a);
    double x=(f*b-c*e)/(a*e-b*d);
    return (node){x,y};
}
int main()
{
    int n;
    scanf("%d",&n);
    int i,j,k;
    for(i=1;i<=n;i++)
    scanf("%lf%lf",&b[i].x,&b[i].y);
    random_shuffle(b+1,b+n+1);
    R=0;
    for(i=1;i<=n;i++)
    if(!incircle(b[i]))
    {
        O.x=b[i].x;O.y=b[i].y;R=0;
        for(j=1;j<i;j++)
        if(!incircle(b[j]))
        {
            O.x=(b[i].x+b[j].x)/2;
            O.y=(b[i].y+b[j].y)/2;
            R=dis(O,b[i]);
            for(k=1;k<j;k++)
            if(!incircle(b[k]))
            {
                O=solve(
                b[i].x-b[j].x,b[i].y-b[j].y,(sqr(b[j].x)+sqr(b[j].y)-sqr(b[i].x)-sqr(b[i].y))/2,
                b[i].x-b[k].x,b[i].y-b[k].y,(sqr(b[k].x)+sqr(b[k].y)-sqr(b[i].x)-sqr(b[i].y))/2 
                );
                R=dis(b[i],O);
            }
        }
    }
    printf("%.10lf\n%.10lf %.10lf",R,O.x,O.y);
}
```