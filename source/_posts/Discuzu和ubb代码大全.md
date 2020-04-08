# Discuzu和UBB代码

## UBB代码

直接拨号
> [call]手机号码[/call]

换行
> [br]

空格
> [tab]

链接
> [url=URL地址]链接标题[/url]

注意
> 如果链接站内地址，请在"URL地址"后面加上"&sid=[sid]"，以防手机掉线情况发生。

显示站内信息条数
> [m e s s a g e]

自动显隐站内信息
> [automsg]

图片
> [img]图片URL地址[/img]

居左
> [left]

居中
> [center]

居右
> [right]

加粗
> [b]要加粗字符[/b]

斜体
> [i]要斜体字符[/i]

下划线
> [u]要下划线字符[/u]

当前系统日期和时间
> [now]

显示年份
> [year]

显示当前月份
> [month]

显示当天数
> [day]

显示时钟数
> [hour]

显示分钟数
> [minute]

显示秒钟数
> [second]

显示当前系统日期
> [date]

显示当前系统时间
> [time]

显示当日星期
> [weekday]

显示农历日期
> [ttcc]

显示问候语
> [hello]

显示飞翔效果
> [fly]八神欢迎你[/fly]

方框效果
> [input=21]空格效果[/input]

显示字体
> [font=幼圆]八神欢迎你[/font]

显示字体尺寸
> [size=6]字体尺寸[/size]

### 显示标题链接

语法
> [功能模块标识=N_M_X]

N为栏目ID，为0时表示此功能模块所有内容

M为返回条数，最多10条

X为0为随机，1为最新，2热点，3精华(论坛，友链，商店，约会)，4不自动换行

文章功能
> [article=N_M_X]

论坛功能
> [bbs=N_M_X]

论坛专题
> [bbstopic=N_M_X]

下载功能
> [download=N_M_X]

图片功能
> [picture=N_M_X]

视频功能
> [video=N_M_X]

铃声功能
> [ring=N_M_X]

商店功能
> [shop=N_M_X]

约会功能
> [yuehui=N_M_X]

留言板功能
> [guessbook=N_M_X]

友链功能
> [link=N_M_X]

动态友链功能
> [linkactive=N_M_X]

WML自写功能
> [wml=N_M_X]

例如您新增了一个栏目指向了文章功能，且这个栏目你已经增加了内容，那么输入[article=12_3_0]，12为你的栏目ID（即classid），在界面上将显示随机显示3条标题链接。NM输入错误返回空。

### 显示下拉框

下拉链接
> [sel name=英文名称][option=链接地址]中文名称[/option][/sel]

例
以下为显示天气预报下拉链接

> [sel name=city][option=utility/weather/show.asp?id=020&siteid=[siteid]&sid=[sid]]广州[/option][option=utility/weather/show.asp?id=010&siteid=[siteid]&sid=[sid]]北京[/option][/sel]

此代码任何地方不能有换行符或回车！

###　显示搜索框

输入框
> [input=长度大小]说明[/input]

例
> [input=15]输入关键字[/input]

搜索按钮
> [ancho=功能模块标识]说明[/ancho]

目前16个功能模块搜索按钮如下

```ubb
[ancho=article]文章搜索[/ancho]

[ancho=bbs]论坛搜索[/ancho]

[ancho=picture]图片搜索[/a

ncho]

[ancho=ring]铃声搜索[/ancho]

[ancho=video]视频搜索[/ancho]

[ancho=download]下载搜索[/ancho]

[ancho=album]相册搜索[/ancho]

[ancho=link]友链搜索[/ancho]

[ancho=yuehui]约会搜索[/ancho]

[ancho=shop]商店搜索[/ancho]

[ancho=airplane]航班搜索[/ancho]

[ancho=hotel]酒店搜索[/ancho]

[ancho=guessbook]留言搜索[/ancho]

[ancho=paimai]拍卖搜索[/ancho]

[ancho=wml]自写代码搜索[/ancho]

[ancho=search]用户搜索[/ancho]

[ancho=card]名片搜索[/ancho]

[ancho=class]栏目搜索[/ancho]
```

高级使用
> 取得输入框值[urlancho=地址_参数]说明[/urlancho]

例1.百度搜索原地址
`http://wap.baidu.com/baidu?word=XXXXX[input=15]美女[/input][urlancho=http://wap.baidu.com/baidu_word]百度搜索[/urlancho]`

例2.儒豹搜索原地址
`http://wap.roboo.com/auto.jsp?q=XXX&back=123.com&f=123[input=15]美女[/input][urlancho=http://wap.roboo.com/auto.jsp?back=123.com&f=123_q]儒豹搜索[/urlancho]`

提示
> 一个页面建议只使用一个输入框！

### 显示栏目特别按钮

搜索
> [search=功能模块标识_栏目ID]说明[/search]

最新
> [*=功能模块标识_栏目ID]说明[/*]

热点
> [hot=功能模块标识_栏目ID]说明[/hot]

精华
> [good=功能模块标识_栏目ID]说明[/good]

N天前
> [days=功能模块标识_栏目ID_天数]说明[/days]

例如你的一个论坛栏目ID为988，以上应用为

```ubb
[search=bbs_988]XX栏目搜索[/search]

[*=bbs_988]XX栏目最新[/*]

[hot=bbs_988]XX栏目人气[/hot]

[good=bbs_988]XX栏目精华[/good]

[days=bbs_988_1]XX栏目今天发表[/days]
```

(其中栏目ID可以为上一级的，最顶级的为0，部分功能模块无此应用)

### 显示特殊内容

登录可见内容
> [login]输入内容[/login]

手机可见内容
> [mobi]输入内容[/mobi]

金钱可见内容
> [coin=金钱数]输入内容[/coin]

等级可见内容
> [grade=等级]输入内容[/grade]

以下只适用于论坛内容帖处录入

付费可见内容
> [buy=金钱数]输入内容[/buy]

回复可见内容
> [reply]输入内容[/reply]

### 访问量统计API

第几位访问者
> [vtotal]

昨日访问量
> [vyestaday]

今日访问量
> [vtoday]

本周访问量
> [vweek]

本月访问量
> [vmonth]

总访问量
> [vtotal]

当前在线人数
> [online]

所有网站流量
> [valltotal]

聊天室在线数
> [chat=栏目ID]

### 高级API

取流水号
> [fid]

取网站ID
> [siteid]

取当前页栏目ID
> [classid]

(以下用户登录后有效)

取用户ID
> [userid]

取用户名
> [username]

取用昵称
> [nickname]

取用户MD5密码
> [password]

(终端不支持Cookies时有效)

取用户登录信息串
> [sid]

例
> URL.asp?siteid=[siteid]

&classid=[classid]

&sid=[sid]

以上为HTTP接口，二次开发通过以上接口可以开

发出功能强大模块。

怎么发彩色字体

`[forecolor=#00FF00]顶[/forecolor]`去掉星号就可以了，而FF0000就是颜色代码，

附颜色代码
白色FFFFFF红色FF0000绿色00FF00蓝色0000FF青色00FFFF黄 色FFFF00黑色000000海蓝70DB93巧克力5C3317蓝柴色9F5F9F黄铜色B5A642亮金色D7D719棕色A67D3D青铜色8C7853二青铜色A67D3D士官服蓝5F9F9F冷铜色珊瑚红FF7FOO铜色B87333紫蓝色42426F深棕色5C4033深绿色2F4F2F深铜绿4A766E深橄榄绿4F4F2F深兰花9932CD深紫色871F78深石板蓝6B238F深铅灰色2F4F4F深棕褐色97694F深绿松石色7093DB暗木色855E42淡灰色545454火砖色BE2323森林绿238E23长石色D19275金色CD7F32鲜黄 色DBDB70灰色C0C0C0铜绿色527F76青黄 色93DB70借人绿215E21印度红4E2F2F土黄 色9F9F5F浅灰色A8A8A8浅钢蓝色8F8FBD浅木色F9C2A6石灰绿色32CD32桔黄 色E47833褐红色8E236B中海蓝32CD99中森林绿6B8E23中鲜黄 色EAEAAE中兰花色9370DB中海绿色426F42中石板蓝7F00FF中春绿7FFF00中绿松色70DBDB中紫红色DB7093深藏青2F2F4F海军蓝23238E霓虹蓝4D4DFF霓虹粉FF6EC7

## Discuz代码大全

大家在论坛发表主题或回帖时，经常要用到DISCUZ代码，本文就常用的DISCUZ代码作一个介绍，大家不妨试一试：

Discuz! 代码是一个 HTML 代码的简化版本，来简化对帖子显示格式的控制。

1、字体加粗
> [ b ]字体加粗[ /b ]

2、斜体文字
> [ i ]斜体文字[ /i ]

3、下划线文字
> [ u ]下划线文字[ /u ]

4、字体颜色
> [ color=red ]字体颜色[ /color ]
说明："color=red"等号后面的是颜色的英文单词，类似的还有蓝色：blue、绿色：green、粉色：pink、灰色：gray等，同样也可以使用颜色的16进制代码，比如"FF6600"

5、字体大小
> [ size=3 ]字体大小为3[ /size ]
说明：本论坛字体大小范围是从"1"——"7"。

6、改变字体
> [ font=黑体 ]字体为黑体[ /font ]
说明："font=黑体"等号后面的为字体名字，字体必需使用论坛已用的字体库，否则无法辨认。论坛的中文字体默认为宋体，英文字母默认字体为Tahoma。各支持字体效果如下： QUOTE: 宋体黑体 Arial Book Antiqua Century Gothic Courier New Georgia Impact Tahoma Times New Roman Verdana
7、对齐格式
> [ align=center ]居中对齐[ /align ]

说明："align="等号后面是对齐格式，左对齐为left，居中为center，右对齐为right。

8、飞行效果
> [ fly ]飞行效果[ /fly ]

9、超链接
> `[ url ]http://www.anhuinews.com[ /url ]`

10、文字超链接
> `[ url=http://www.anhuinews.com/ ]中安在线[ /url ]`

11、电子邮件链接
> [ email ][email=gswow@hoa]gswow@hotmail.com[/email][ /email ]

12、文字电子邮件链接
> [ [email=email=gswow@hotmail.com]email=[email=gswow@hoa]gswow@hotmail.com[/email][/email]]管理员的邮箱[ /email ]

13、引用文字
> [ quote ]被引用文字[ /quote ]

14、列表
> [ list ] 列表项目1 列表项目2 列表项目3 [ /list ]

15、项目符号
> [ * ]

16、链接图片
> [ img ]图片地址[ /img ] 说明：可以对链接的图片进行

尺寸限制
> [ img=176,62 ]图片地址[ /img ]

17、链接Flash
> [ swf ]flash地址[ /swf ]

18、链接音乐
> `[ wma ]http://www.verywd.com/0/404/zhuguli/sound/2006930211321985.wma[ /wma ]`
说明：由于插件是WMP播放器，所有只能支持WMP默认能播放的音频。另外，本论坛还支持RM格式的音视频。

19、链接视频
> `[ wmv ]http://www.verywd.com/FamilyFile/0/76/janfans/Video/200608291628307422326.wmv[ /wmv ]`

20、隐藏代码(回复可见)
> [hide][/hide]

21.[FLIPH]左右颠倒文字[/FLIPH]

22.[FLIPV]上下颠倒文字[/FLIPV]

23.[BLUR=文字宽度,方向,浓度]模糊文字[/BLUR]

1：新增标签：wmv

替换内容:

```Discuz
<object id='mPlayer1' width=468 height=350

classid='CLSID:6BF52A52-394A-11D3-B153-00C04F79FAA6'

type=application standby="Loading Windows Media Player components..." />

<param name='url' value='{1}'

<param name='rate' value='1'>

<param name='balance' value='0'>

<param name='currentPosition' value='0'>

<param name='defaultFrame' value=''>

<param name='playCount' value='100'>

<param name='autoStart' value='-1'>

<param name='currentMarker' value='0'>

<param name='invokeURLs' value='-1'>

<param name='baseURL' value=''>

<param name='volume' value='50'>

<param name='mute' value='0'>

<param name='uiMode' value='full'>

<param name='stretchToFit' value='0'>

<param name='windowlessVideo' value='0'>

<param name='enabled' value='-1'>")

<param name='enableContextMenu' value='0'>

<param name='fullScreen' value='0'>

<param name='SAMIStyle' value=''>

<param name='SAMILang' value=''>

<param name='SAMIFilename' value=''>

<param name='captioningID' value=''>

</object>
```

例子:

> [wmv][/wmv]

解释:

wmv

参数个数:

1

嵌套次数:

1

2：新增标签：rm

替换内容

```Discuz
<OBJECT ID=xinhua-mtv CLASSID="clsid:CFCDAA03-8BE4-11cf-B84B-0020AFBBCCFA" HEIGHT=240 WIDTH=352 NAME="controls" VALUE="ImageWindow"><PARAM NAME="console" VALUE="xinhua-audio-mtv"><PARAM NAME="autostart" VALUE="-1"><PARAM NAME="src" VALUE="{1}"><param name="_ExtentX" value="9313"><param name="_ExtentY" value="6350"><param name="SHUFFLE" value="0"><param name="PREFETCH" value="0"><param name="NOLABELS" value="0"><param name="LOOP" value="0"><param name="NUMLOOP" value="0"><param name="CENTER" value="0"><param name="MAINTAINASPECT" value="0"><param name="BACKGROUNDCOLOR" value="#000000"><EMBED SRC="{1}" type="audio/x-pn-realaudio-plugin" C C HEIGHT=288 WIDTH=352 AUTOSTART=ture></OBJECT><br><OBJECT ID=xinhua-mtv CLASSID="clsid:CFCDAA03-8BE4-11cf-B84B-0020AFBBCCFA" HEIGHT=90 WIDTH=352><PARAM NAME="controls" VALUE="all"><PARAM NAME="console" VALUE="xinhua-audio-mtv"><param name="_ExtentX" value="9313"><param name="_ExtentY" value="2381"><param name="AUTOSTART" value="0"><param name="SHUFFLE" value="0"><param name="PREFETCH" value="0"><param name="NOLABELS" value="0"><param name="LOOP" value="0"><param name="NUMLOOP" value="0"><param name="CENTER" value="0"><param name="MAINTAINASPECT" value="0"><param name="BACKGROUNDCOLOR" value="#000000"><EMBED type="audio/x-pn-realaudio-plugin" C C HEIGHT=90 WIDTH=275 AUTOSTART=false></EMBED></OBJECT>
```

例子:

> [rm][/rm]

解释:

rm

参数个数:

1

嵌套次数:

1

如果想要自动播放,请改一下参数

AutoStart设置成1就行了

3：新增标签：mp3

内容替换

> `<embed src="{1} " type="audio/midi" hidden="false" autostart="true" loop="true" height="20" width="200"></embed>`

例子:

[mp3][/mp3]

解释:

mp3

参数个数:

1

嵌套次数:

1

4：新增标签：mid

替换内容

> `<embed src="{1} " type="audio/midi" hidden="false" autostart="true" loop="true" height="20" width="200"></embed>`

例子:

> [mid][/mid]

解释:

mid

参数个数:

1

嵌套次数:

1

--------

5.新增标签：fly

替换内容

> `<marquee direction="left" scrollamount="3" >{1}</marquee>`

例子:

飞行文字

解释:

飞行文字

参数个数:

1

嵌套次数:

1

6：新增标签：typewriter

例子:

> [typewriter]打字机文字[/typewriter]

解释:

打字机文字

参数个数:

1

嵌套次数:

1

--------

7：新增标签：crfont

例子:

> [crfont]彩虹文字[/crfont]

解释:

彩虹文字

参数个数:

1

嵌套次数:

1

--------

8：新增标签：qq

替换内容

> `<a href="http://wpa.qq.com/msgrd?V=1&Uin={1}&Site=[Discuz!]&Menu=yes" target="_blank"><img src="http://wpa.qq.com/pa?p=1:{1}:1" border="0"></a>`

解释:

Show online status of specified QQ UIN and chat with him/her simply by clicking the icon

参数个数:

1

嵌套次数:

1

--------

9：新增标签：box

内容替换

> `<blockquote style="margin-left:20px; margin-right:20px; border:#DDE3EC dashed 1px; padding:5px; background-color:#FFFFFF;b`

ackground-color: {1} ;">{2}</blockquote>

例子:

> [box=color]Text[/box]

解释:

文字底色

参数个数:

2

嵌套次数:

3

10：新增标签：dshadow

替换内容:

> `<a style="width:100%; filter:Shadow(color=#999999, direction=145)"><font color="blue">{1}</font></font></a>`

例子:

> [dshadow]阴影文字2[/dshadow]

解释:

阴影文字2

参数个数:

1

嵌套次数:

1

--------

11：新增标签：blur

替换内容:

> `<SPAN style="FILTER: Blur(Add=1, Direction=90, Strength=80); WIDTH: 730px">{1}</SPAN>`

例子:

> [blur]动感文字[/blur]

解释:

动感文字

参数个数:

1

嵌套次数:

1

--------

12：新增标签：dao

内容替换

> `<span style="width:400; font-family: Arial ; position: relative; filter: flipv">{1}</SPAN>`

例子:

> [dao]倒转文字[/dao]

解释:

倒转文字

参数个数:

1

嵌套次数:

1

14：新增标签：Shadow

替换内容:

> `<a style="width:100%; filter:Shadow(color=#999999, direction=145)"><font color="red">{1}</font></font></a>`

例子:

> [shadow]阴影文字[/shadow]

解释:

阴影文字

参数个数:

1

嵌套次数:

1

--------

15：新增标签：strike

替换内容:

> `<strike>{1}</strike>`

例子:

> [strike]文字加上刪除线[/strike]

解释:

文字加上刪除线

[strike]123456[/strike]