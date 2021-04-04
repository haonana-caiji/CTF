1. ### 金三胖

一闪而过的画面，丢ps里看到夹层，得到flag{he11ohongke}

2. ### 二维码

扫码得到，secret is here，010打开，翻到最后发现zip文件头50 4B 03 04，复制出来，发现一个加密过的zip文件，没有任何提示，怀疑是伪加密，改了发现报错，疑惑？？？，得好好学下zip的格式了

那就看文件名4number，尝试写脚本爆破，还真出了，解压打开4number.txt得到flag：

flag{vjpw_wnoei}

3. ### N种解决方案

Exe文件，双击打开失败，老规矩丢010里看

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image002.png)

Base64加密，写个py解一下，打开发现是png，改下后缀，出了二维码

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image003.png)

扫一下得到flag，KEY{dca57f966e4e4e31fd5b15417da63269}

4. ### 大白

打开一看，感觉是半身图，直接010改身高试试，还真出来了。。。

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image005.png)

5. ### 你竟然赶我走

一张图，010打开前后看看，在最后，完。flag{stego_is_s0_bor1ing}

6. ### 基础破解

提示是4位数字密码，rar文件，拿前面的脚本改一下，结果改了半天，莫名发现rar的pwd不能加encode，疑惑+1，得到密码2563，打开看到ZmxhZ3s3MDM1NDMwMGE1MTAwYmE3ODA2ODgwNTY2MWI5M2E1Y30=，有等于号试试base64

得到flag{70354300a5100ba78068805661b93a5c}

7. ### 乌镇峰会种图

老规矩010走一波，又在最底.。。。flag{97314e7864a8f62627b26f3f998c37f1}

8. ### LSB

提示LSB，直接丢stegsolve中，翻到这个

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image006.png)

翻了个遍，发现alpha位没有，设置下

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image008.png)

发现为png文件，保存下，打开发现为二维码，直接扫码得到flag

9. ### 文件中的秘密

还是010打开，看到exif，属性查看，得到flag{870c5a72806115cb5439345d8b014396}。

10. ### Wireshark

用Wireshark打开流量包，直接搜索字符“flag”，看到email=flag,把后面的password试一下，还真是，flag{ ffb7567a1d4f4abdffdb54e022f8facd}。

11. ### Rar

本来打算脚本走一波，但是报错了，现在怀疑是rar版本问题，一打开直接弹窗显示有密码，有空尝试解决下

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image010.png)

Unrar让我头大，迟早重写一个。。。

用工具跑一下

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image011.png)

得到密码8795，不得不说还是python快

打开得到flag.txt，得到flag{1773c5da790bd3caff38e3decd180eb7}

12. ### Qr

算不上题目，扫一下就有，签个到吧，Flag{878865ce73370a4ce607d21ca01b5e59}

13. ### Zip伪加密

题如其名，那就010改一下，09改成00，直接打开了，得到flag{Adm1N-B2G-kU-SZIP}

14. ### Ningen

看提示，还要解压缩，那直接把图片丢010里，发现是jpg，找下文件尾FFD9，发现后面很多东西，看了一下504B开头，应该是zip，复制出来，用脚本搞一下，得到flag.txt，打开得到flag{b025fc9ca797a67d2103bfbc407a6d5f}

15. ### 镜子里面的世界

一张图片，名称为steg，那直接丢进steg里，左右翻看，0号位直接黑，alpha位不变化，直接lsb走一波

![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\msohtmlclip1\01\clip_image013.png)

得到flag{st3g0_saurus_wr3cks}

16. ### 被嗅探的流量

Wireshark打开直接查找flag字符，直接找到flag{da73d88936010da1eeeb36e945ec4b97}

17. ### 小明的保险箱

010打开，翻到结尾发现rar，复制出来爆破下，得到

flag{75a3d68bf071ee188c418ea6cf0bb043}

18. ### 爱因斯坦

丢010里，jpg文件，最后又是一个rar，搞出来爆破一下，爆破失败，重新找线索，尝试伪加密，失败，重新翻看010，发现属性有隐藏信息，看一下，发现备注：this_is_not_password，作为密码尝试下。。。。。这就是此地无银三百两？？？得到

flag{dd22a92bf2cceb6c0cd0d6b83ff51606}

19. ### Easycap

流量包，丢wireshark里看看，看了一眼tcp流量，得到

FLAG:385b87afc8671dee07550290d16a8071

玄学做题。。。

20. ### 另一个世界

010看，发现jpg文件没有结尾FFD9，结尾还有奇怪的二进制码，试试把二进制码拿出来，写个脚本，转成ascii码试试，得到koekj3s

小总结一下：

1. 题目不难，甚至简单得离谱了，但毕竟也是由易到难，慢慢学吧。

2. 有几个知识点需要整理一下

3. 分别是jpg，png，rar，zip文件格式，和写脚本解压缩时出现的情况

4. Base64编码原理是什么http://blog.xiayf.cn/2016/01/24/base64-encoding/

5. 找时间整理一下，写过的脚本，变成工具集。

6. 写python时的一些小知识int(,base)，base参数填写要转换的数据原类型，字符串相加时的顺序，记得加break！还有环境问题，项目方式打开相对路径才起作用。

7. 有时间一定要好好整理！！！！