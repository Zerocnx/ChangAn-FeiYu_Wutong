# ChangAn-Wutong
# 关于长安车机飞鱼系统解除系统安装限制以及过程

## 前提说明：

#### 文章原作者：@17710，因作者无吾爱账号，所以代为发表。已经获得作者授权！以下内容为作者原内容！

#### 	此教程仅针对飞鱼系统代表车型UNI-V 锐程puls 等 至于梧桐系统(代表车型 UNI-T) 请见文章结尾思路

####	(本人是纯小白 可能走了很多弯路 大佬勿喷)

## 思路

> **首先感谢VIKING大佬的思路(csdn搜索长安梧桐车机)** @VIKING-01
>
> 至于二代75p的教程 可以看@Streamform的帖子，详细的介绍了如何绕过辰为加密直接安装(帖子地址百度)

```SHELL
#从大佬教程中 我们可以找到飞鱼车机的第一道拦截apk并进行删除 system/app/VecentekAPP/ VecentekAPP.apk

#然后使用 pm install -r 继续安装软件会进行报错
```

如图：

![image-20230517105953753](D:\cydesk\Desktop\车机文档\image\1.png)

#### 一、根据Viking的思路之后的流程

##### 1、找到车机中的Services.jar并拷贝出来，并开始修改

> 根据报错提示以及大佬思路 可以找到/system/framework/services.jar ,然后使用adb pull拷贝出来

```shell
#adb工具命令如下：

adb pull /system/framework/services.jar system '拷贝jar文件到工具箱下面的system目录中

#根据报错提示以及大佬思路，将这个文件用MT管理器打开，然后进行关键字搜索，可以得到如下结果。
```

如图：

![image-20230517110602750](D:\cydesk\Desktop\车机文档\image\2.png)

> 将这部分代码转换成java代码 得到如下所示

![image-20230517110822326](D:\cydesk\Desktop\车机文档\image\3.png)

> 然后将整段代码喂个GTP（不要问 问就是看不懂 纯小白） 得到以下结果

![image-20230517110844867](D:\cydesk\Desktop\车机文档\image\4.png)

![image-20230517110907718](D:\cydesk\Desktop\车机文档\image\5.png)

> 发现是通过这行代码来判断应用是否通过认证 继续找gtp 

![image-20230517110939230](D:\cydesk\Desktop\车机文档\image\6.png)

> 按照GTP给的修改教程 改完之后发现我转不回dex里的smali代码了（问就是菜）…
>
> 只得作罢 从一开始的smali代码动手 
>
> 找到刚刚用MT搜索到的对应文件里面的对应内容

![image-20230517111029977](D:\cydesk\Desktop\车机文档\image\7.png)

>可以发现 java代码里的判断 对应的就这图上这几行，老规矩 找gtp，得到如下结果

![image-20230517111256896](D:\cydesk\Desktop\车机文档\image\8.png)

> 然后让gpt 动手修改

![image-20230517111330508](D:\cydesk\Desktop\车机文档\image\12.png)

> 按照教程修改（一共有两个） 有两处install failed，我们需要修改两处，修改完后用MT反编译回去即可

如图所示：

![image-20230517111425569](D:\cydesk\Desktop\车机文档\image\9.png)

![image-20230517111435763](D:\cydesk\Desktop\车机文档\image\10.png)

##### 2、开始测试流程

温馨提示：

```shell
# 进入adb 替换掉车机原文件（提前做好备份）执行”stop” ”sync” ”start” 三个命令进行重启 (不用reboot重启是因为reboot重启需要重连adb  而这三个命令重启不会 万一文件修改的有问题 无法开机 可以在adb里面替换回去)
#成功开机 push个apk，然后使用 pm install -r 安装试试 发现没有报错 只有段提示(没截图 不重要)   为了车机安装软件 直接安装软件 直接推了个via到system/priv-app下  reboot重启 打开via 随意下载个安装包 点击安装
```

安装成功的话如图所示：

![image-20230517111653227](D:\cydesk\Desktop\车机文档\image\11.png)

至此，流程结束。你的车机已经可以随便安装软件到data了。

#### 二、其它车型，如UNIT等梧桐车机系统

```shell
#	Unit等其他梧桐系统 的思路 
#	梧桐系统的services文件经过dex2oat优化过，保存在system/framework/oat/arm64/下
#	理论上来讲 将优化过得文件反编译成java代码或者smali代码 就能按照上述方法进行修改
```

####  三、如果无需安装到data(车机system分区空间足够的话，可以看上一篇帖子)
传送门：https://www.52pojie.cn/thread-1783719-1-1.html

#### 四、工具箱使用说明

```shell
#请详细阅读以下使用说明    可能会影响保修以及小概率导致车机变砖 请酌情使用
1、工具箱使用请准备好笔记本以及双公头usb线 有c口的电脑可以使用普通的数据线 并确保adb驱动以安装 (win7需要安装 w10 w11自带） 并查看文件内的车机准备  不同车型进入工程模式可能不同 具体自行百度 或者加群讨论
2、什么车型能用  2代55p unik univ 锐程puls （具体车型要查看你的services.jar文件 不确定的可以加群讨论） 也可以使用工具箱进行试验
因为services.jar文件的通用性 所以他可能在多种车型上运行 工具箱内自带的jar文件是使用uinv文件修改的 仅测试过univ 锐程plus 其余车型如果点击完美破解后 如果卡在loge开机界面  请点击
还原未破解状态 来恢复你的原车文件（在次期间请务保证adb连接正常）
3、破解详细说明请查看查看工具箱左侧说明
4、完美破解仅用来解除软件安装限制 不能对system分区进行修改 如果需要替换原厂高德 停用系统更新 卸载原车自带软件 请使用普通工具箱
5、目前最新款二代55p已经屏蔽了adb的root权限 不能使用此工具箱 其他车型完美破解成功后 建议屏蔽更新
6、部分车型破解安装软件后没有图标 可以下载小白点使用 已经有大佬在研究这个bug 后期可能会修复

```

工具箱截图：

![image-20230517142019653](D:\cydesk\Desktop\车机文档\image\13.png)

下载地址：https://wwba.lanzoum.com/iHmOE0web3zg    密码:17710
