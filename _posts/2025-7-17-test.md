---
layout: post
title: MCPE 0.14客户端制作
date: 2025-7-17
author: 芒果
tags: [Minecraft,MCPE,0.14]
comments: true
toc: true
---

## 目录
- [准备](#准备)
- [so修改](#so修改)
  - [UN管理器如何使用](#un管理器如何使用)
  - [伤害无抖动](#伤害无抖动)
  - [皮肤解锁](#皮肤解锁)
  - [单人生存模式不掉血](#单人生存模式不掉血)
  - [仿JE血量饥饿条](#仿je血量饥饿条)
  - [Win10UI](#win10ui)
  - [隐身效果失效](#隐身效果失效)
- [APK修改](#apk修改)
  - [流量进服](#流量进服)
  - [添加光影](#添加光影)
  - [修改GUI](#修改gui)
  - [修改按键](#修改按键)

<a name="准备"></a>
# 准备
制作MCPE0.14客户端需要以下工具  
so修改：UN管理器  
修改安装包：[MT管理器](https://mt2.cn)或[NP管理器](http://normalplayer.top/)  
如果你要修改so，请把安装包中的so提取到一个显眼的目录，使用UN管理器修改后，把so复制进去即可

<a name="so修改"></a>
# so修改
<a name="un管理器如何使用"></a>
## UN管理器如何使用
打开UN管理器，点击底栏的第二个按键，点击so编辑，选择你已经提取出来的so，假如第二次不想这么麻烦，可以在历史记录快速进入  
过滤函数：点击右上角三个点，点击过滤，粘贴在下面复制的函数名，点击确定，等待过滤完成，过滤完成后点击即可  
修改so：找到函数后，照着内容去找代码，找到代码后，要再三确认，比如这行代码的下面是不是这个等，确认好后再照着修改，修改后要记得点击顶栏的保存图标，保存后就可以把修改后的so替换了
<a name="伤害无抖动"></a>
## 伤害无抖动
反编译libminecraftpe.so  
过滤函数_ZN13LevelRenderer7bobHurtER6Matrixf  
找到EBE7  
原  
ble <@this+0x20> ;#0x46a374  
改  
b <@this+0x20> ;#0x46a374  
> 后面说的so都指libminecraftpe.so，除了明确说明
> 

<a name="皮肤解锁"></a>
## 皮肤解锁
反编译so 找到函数  
_ZNK4Skin13premiumLockedEv  
原  
eor r0 r0 ＃1  
改  
eor r0 r0 ＃0
<a name="单人生存模式不掉血"></a>
## 单人生存模式不掉血
反编译so 找到函数  
_ZN11LocalPlayer4hurtERK18EntityDamageSourcei  
原：  
ldr.w r3, [r0, #0xcd8]  
cbz r3, <@this+0xa> ;#0x3641ea  
修改为：  
mov.w r3, #1  
nop  
这样生存模式也不会掉血了
<a name="仿je血量饥饿条"></a>
## 仿JE血量饥饿条
反编译so 找到函数  
_ZNK11AppPlatform14useCenteredGUIEv  
原  
movs r0, #0  
bx lr  
修改为  
movs r0, #1  
bx lr
<a name="win10ui"></a>
## Win10UI
反编译so 找到函数  
_ZNK19AppPlatform_android13getScreenTypeEv  
原  
movs r0, #1  
bx lr  
修改为  
movs r0, #0  
bx lr
<a name="隐身效果失效"></a>
## 隐身效果失效
反编译so 找到函数  
_ZN3Mob24updateInvisibilityStatusEv  
偏移：0x4d256a  
原值：51D0  
修改：51D1
<a name="apk修改"></a>
# APK修改
<a name="流量进服"></a>
## 流量进服
用mt管理器或np管理器打开安装包，点击classes.dex文件，使用dex++编辑器（dex plus编辑器），修改路径为com/mojang/minecraftpe/MainActivity下的类。  
在MainActivity类中，找到isNetworkEnabled函数。  
原  
    .line 626  
    .restart local v1  # "info":Landroid/net/NetworkInfo;  
    :cond_1d  
    const/4 v2, 0x0  
改  
    .line 626  
    .restart local v1  # "info":Landroid/net/NetworkInfo;  
    :cond_1d  
    const/4 v2, 0x1
<a name="添加光影"></a>
## 添加光影
这里我以Moonlight光影修复版为例  
[下载光影（github镜像）](https://bgithub.xyz/EatMango233/blog/raw/refs/heads/master/file/Moonlight%E5%85%89%E5%BD%B1%E4%BF%AE%E5%A4%8D%E7%89%88.zip) [下载光影（github）](https://github.com/EatMango233/blog/raw/refs/heads/master/file/Moonlight%E5%85%89%E5%BD%B1%E4%BF%AE%E5%A4%8D%E7%89%88.zip)  
我们打开moonlight光影后发现只有shader文件夹，而安装包里并没有shader文件夹，我们就需要点开assets文件夹就可以把moonlight的shader文件夹给替换过去了，假如其它的光影不止有一个shader文件夹，或者已经有了assets文件夹，都可以全部替换
<a name="修改gui"></a>
## 修改GUI
如果你觉得默认的GUI太过于难看，可以根据自己的需要提取出文件  
移动、暂停、聊天、物品栏：/assets/images/gui/gui.png  
准心、血量饥饿条、经验条、网络状态：/assets/images/gui/icons.png  
设置开关、语言按钮：/assets/images/gui/touchgui.png  
设置的三个图标：/assets/images/gui/touchgui2.png  
头盔、护甲、护腿、靴子、图标：assets/images/gui/newgui/目录搜索empty即可搜到（文件名太长了）  
语言图标：assets/images/gui/newgui/中的language18.png和language16.png  
提取出来之后就可以使用绘画软件进行修改了，绘画时可以对照游戏内的按键进行绘画，绘画后替换安装包内的图片即可
<a name="修改按键"></a>
## 修改按键
按键和gui分开是因为按键图片太多了，甚至不同的ui有不同的按键，如果你想修改，可以根据需要提取出来绘画  
未知按键（有可能是背包按键）：/assets/images/gui/spritesheet.png  
> 接下来你就可以点进newgui目录了，剩下的按键都是在newgui里
> 

未按下的按键：buttonNew.png  
未按下的按键1：classic-button.png  
未按下的按键2：classic-button-hover.png  
按下后的按键：classic-button-pressed.png  
绘画后替换原图片即可
