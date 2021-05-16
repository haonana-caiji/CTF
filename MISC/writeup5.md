**窥屏发现typaro可以自动上传图片，以后有图片啦~，收回这句话，图床坏了，原因未知，挂着vpn都上传不了图片，折腾了半天我放弃了。**

#### 1.zips

没找到线索，放一边爆破下，得到密码723456解压得到另一个压缩包，解压只得到一个flag.zip，丢010里看看，发现是伪加密，把09改为00，解压成功，得到.sh文件，打开发现一句命令，属性查看该文件是‎2019‎年‎5‎月‎17‎日，‏‎8:25:28写入的，写了个脚本跑了半天失败了，直接上答案，好家伙python2的加密，精度为2，我怎么也跑不出呀，python3就没低过7位数，而且看答案还不是属性那天的密码，绝绝子，差评！只确定前两位为15的脚本爆破密码（假装自己电脑跑了一天），得到**flag{fkjabPqnLawhvuikfhgzyffj}**

#### 2.千层套路

爆破了下压缩包，还是嵌套压缩包，看了一下密码，发现密码，就是压缩包的数字，写个py脚本爆破一下，跑了一会得到qr.txt，一堆坐标点，应该是要画二维码，一共4000行，那就是200*200的二维码，（255，255，255）为黑，（0，0，0）为白，写个py脚本把qr.txt转换下，然后化成二维码，思路如下：

```
f = open('./qr.txt','r')
f2 = open('./qr2.txt','w')
for y in range(200):
  for x in range(200):
​    if(f.readline() == '(255, 255, 255)\n'):
​      f2.write(str(x) + ',' + str(y) + '\n')

import matplotlib.pyplot as plt
import numpy as np
x,y = np.loadtxt('./qr2.txt',delimiter = ',',unpack=True)
plt.plot(x,y,'.')
plt.show()
```

扫下二维码得到**flag{ta01uyout1nreet1n0usandtimes}**

#### 3.爬

文件丢010里，发现是pdf，改下后缀，打开后提示flag在图片后，神器Ad直接编辑，第一张图片拖开可见一串十六进制，ocr不是很准，改了一会，得到丢010转16进制得**flag{th1s_1s_@_pdf_and_y0u_can_use_phot0sh0p}**

#### 4.girlfriend

解压得waf，听一下，感觉是以前手机打电话时，各个按键的声音，直接答案找工具，（dtmf2num.exe），通过工具得到

DTMF numbers:  

999 666 88 2 777 33 6 999 4 444 777 555 333 777 444 33 66 3 7777

对照下表，

![picture](http://dyf.ink/crypto/classical/figure/mobile.jpg)

得到**flag{youaremygirlfriends}**

#### 5.Cyber Punk

改下系统时间，打开程序可得**flag{We1cOm3_70_cyber_security}**

#### 6.二维码

sb题目！！！！！）截图拼了一些，扫不出来，可能没截好，上ps，扣了半天，我没抠出来，是我菜了，直接答案了，**flag{7bf116c8ec2545708781fd4a0dda44e5}**

还是那句话sb题目!!!!!

#### 7.虚假的压缩包

虚假的压缩包是伪加密，直接把09改成00即可解压得key.txt，不知道是什么密码，第二个压缩包010看完全局标志00以也是伪加密，结果不是，有点疑惑了。。。直接答案，rsa加密，。。学下。。写个脚本解下，得到：**答案是5**，拿来解压压缩包，得到一张jpg和一个文件，jpg丢010里，发现是png，改下后缀，后，crc32校验发现不对，改下高度，打开图片看到里多出了**^5**。

![image-20210516214359246](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210516214359246.png)

那应该就是异或加密，那就写个py文件再异或一下就行，得到一堆16进制，乍一看还以为是压缩包，实际上是doc文件，改下后缀后，打开，flag颜色改变隐藏了起来，但是可以搜索得到，改下颜色，得到**flag{_th2_7ru8_2iP_}**



#### 8.USB

解压提示，压缩包里的png头缺失，头大不知道怎么修复，ftm文件也打不开，丢010里查看感觉是usb数据，但不知道怎么处理，直接答案了，rar处理，做的时候没对比仔细，陷入了思维盲区，一直以为是png本身文件头丢失，然后还经过了压缩，傻啦吧唧的，那样也能解压的，靠，我是傻逼。

rar文件格式不是很熟悉

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200509211403238.png)这里的7A改成74，解压得到图片，用stegsolve查看blue0通道有一张二维码

![image-20210514223655036](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210514223655036.png)

扫码得到：**ci{v3erf_0tygidv2_fc0}**，ftm文件根据文件名直接搜索key，发现key.pcap，前后查找到zip文件头，将zip文件搞出来，解压得到pcap，wireshark看下，是USB数据，在kali里用tshark取出数据

**tshark -r key.pcap -T fields -e usb.capdata > usbdata.txt**，用UsbKeyboardDataHacker解得**key{xinan}**，维吉尼亚解密得**fa{i3eei_0llgvgn2_sc0}**，再进行栅栏解密得**flag{vig3ne2e_is_c001}**

#### 9.通行证

b64解一下，得到一串像移位密码的东西，但是又不太像，因为其中没有出现过flag，可能进行了别的加密，直接答案，答案显示使用了栅栏密码，栅栏密码应该是**kanbbrgghjl{zb____}vtlaln**这段字符串的开头到**{**有12位，是3的倍数，所以尝试了栅栏密码，栅栏后我尝试了凯撒密码穷举一下**kzna**，得到bias = 13，从而得到**flag{oyay_now_you_get_it}**

#### 10.蜘蛛侠呀

看了眼tcp流，猜测传输了一个zip文件，还有很多icmp协议数据，百度学习下，主要看数据，但没什么线索，感觉是base64加密过的数据，ssh也提示是encryed，丢kali里用tshark把icmp包搞出来，由于有重复，写个脚本去下重,去重后以字符的形式查看，去掉开头结尾，作为zip保存起来，解压得到gif，时间隐写，kali下用**identify -format "%T" flag.gif**，20置0，50置1，转为字节流后md5加密下得到**flag{f0f1003afe4ae8ce4aa8e8487a8ab3b6}**

#### 11.draw

打开没思路，直接答案了，好家伙writeup直接丢网址，一头雾水，应该是geogle出来的，用 logo 解释器得到**flag{RCTF_HeyLogo}**

详见： https://www.calormen.com/jslogo/

#### 12.[MRCTF2020]不眠之夜

解压看到一堆图片，感觉是拼图得flag，ps试试，好家伙，拼到死，绝了，得到**flag{Why_4re_U_5o_ShuL1an??}**，拼图真的无语。。。看了下别人的wp可知，先用montage，再用gaps

#### 13.[UTCTF2020]docx

打开是一篇英文的文档，word设置了下查看隐藏字符，没有线索，直接答案，可知，修改后缀为rar解压可得一张图片里有**flag{unz1p_3v3ryth1ng}**

#### 14.[安洵杯 2019]easy misc

解压得到一张图片和29个txt，一个提示和一个压缩包；图片丢010发现结尾有一张图片，搞出来，发现两张图片看起来一样，但大小不一样，压缩包里有注释**FLAG IN ((√2524921X85÷5+2)÷15-1794)+NNULLULL,**，可能是密码算出来是**7+NNULLULL**，（好家伙这个开方试了我好几次。。。。）不知道是啥玩意，没思路直接看wp。分理出的图片blue plane确实挺奇怪的，这就是水印隐写的特征？

![image-20210516111650563](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210516111650563.png)

用？？工具分离

提示为压缩包密码格式**7+NNULLULL,**，用掩码爆破的方式得到密码：**2019456NNULLULL,**，解压得到txt文件，字频隐写，写个脚本得到**QW8obWdIWT9pMkFSQWtRQjVfXiE/WSFTajBtcw==**，base64解密得**Ao(mgHY?i2ARAkQB5_^!?Y!Sj0ms**，base85解密得**flag{have_a_good_day1}**

一道题学了一堆知识点

#### 15.[MRCTF2020]Hello_ misc

png文件名要求我们进行图片恢复，010打开，发现图片格式并没有损坏，直接打开图片![try to restore it](E:\Downloads\BUU\[MRCTF2020]Hello_ misc\hello\try to restore it.png)

这题有点熟悉的感觉，但没解决，直接答案了,stegsolve的red通道隐藏着图片，![在这里插入图片描述](https://i.loli.net/2021/05/16/MFBrPn2lovO3GQs.png)

保存下来得到密码：**!@#$%67*()-+**，用foremost分离得到zip，解压得到一堆数字，转二进制发现只有八位二进制的前两位不同，用Python将前两位提取提取出来，并以四个两位二进制一组，转为十进制，再转为字符（好家伙，这一套把我整懵了）得到rar密码，解压发现是docx，标色后，出现字符串，base64解密下，得到二进制数，将1置为空格，可看出**flag{He1Lo_mi5c~}**

这题难到起飞！

#### 16.[MRCTF2020]Unravel!!

jpg丢010里，底部有一张png和zip文件，搞出来，png打开是一串字符，

![image-20210513090117681](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513090117681.png)

zip文件损坏，丢010里，头部错得离谱，后面内容应该是正确的，改一下，处理到最后发现没有数据区，凉凉，打开音频，这吉它真的弹得稀碎，痛苦面具，提示看文件尾部，直接丢010拉到结尾，得到**key=U2FsdGVkX1/nSQN+hoHL8OwV9iJB/mSdKk5dmusulz4=**

不是很会密码，结尾有=猜一下base64，丢网页里插件提示openssl加密

![image-20210513090254806](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513090254806.png)

解不了，直接wp，AES加密，又是没学过的密码，下周学密码去了。。。我是傻逼，突然想起压缩包打开图片名已经提示我是AES了，血亏，解密得到**CCGandGulu**，文件名ending，010末尾没找到啥，wp上写新工具silentEye的wav隐写工具，得到**flag{Th1s_is_the_3nd1n9}**



#### 17.[BSidesSF2019]zippy

流量包很小，只有两个tcp流，一个是命令，一个是zip包，搞出来，直接拿命令里的密码：**supercomplexpassword**解压，得到**flag{this_flag_is_your_flag}**

#### 18.[UTCTF2020]file header

png打不开，丢010里，头部改一下，打开图片得到**flag{3lit3_h4ck3r}**

#### 19.寂静之城

笑死，把小说看完了，差点忘了要做题，先来一句祥瑞玉兔。然后直接答案，这题F12翻了翻，页面翻了翻没啥思路，看了眼提示，社工，试试，先翻了会评论，没什么线索，登录后看下点赞和转发的

![image-20210513160451620](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513160451620.png)

笑死，成功发现出题人（bushi

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513160627293.png" alt="image-20210513160627293" style="zoom:200%;" />

![image-20210513160823658](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513160823658.png)

![image-20210513160846184](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210513160846184.png)

离谱，这题能做？？账号都锁住了。。。我是守法好公民，手上没裤子。直接看wp，好家伙，最后还要查开房记录，告辞。

#### 20.[ACTF新生赛2020]明文攻击

一个压缩包和一张图片，题目明文攻击，图片直接010，底部有zip包，搞一下，对比两个压缩包，发现flag.txt大小不一样，但是crc32一致，上明文爆破，得到**flag{3te9_nbb_ahh8}**

#### 21.粽子的来历

四个word都打不开，丢010里看到开头的

![image-20210514154803601](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210514154803601.png)

四张图片都有不同的，试了下直接弄出来md5，不对，又试了直接删去，也不对，搞了个格式相同的文档对比，发现要全改为FF，成功打开文档，诗句一致，但BCD文件与A文件大小都不一致，应该是存在隐写，隐藏文字显示开着，啥也没有，试了下隐写，啥也没发现，直接答案。

这题因为c文档最初是I come from dbappah ，提示该文档为正确答案，然后，观察可知每段行距不同（好眼！），然后，设1.5倍为1，单倍行距为0，得到100100100001，计算md5值为flag，即**flag{d473ee3def34bd022f8e5233036b3345}**

#### 22.[UTCTF2020]basic-forensics

打不开jpeg文件，直接丢010里看，发现是text文本，然后搜索flag，得到**flag{fil3_ext3nsi0ns_4r3nt_r34l}**，没什么意思的签到题。





