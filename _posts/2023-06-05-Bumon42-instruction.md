---
layout: post
title:  "键盘|Bumon42使用说明"
date:   2023-06-05 17:01:58 +0800
categories: keyboards
tags: keyboards, instructions, goose
author: Bumon
mathjax: true
---

* content
{:toc}

Bumon42是我设计的基于ZMK固件的蓝牙5.0双模40%键盘,有低功耗，长续航，便携的特点。带有热插拔轴座、编程轴灯及编码器。





# 使用说明

## 首次开机及蓝牙连接

往前方拨动键盘左侧的开关，空格中间的状态灯会闪两次红光后开机，蓝牙寻找到 ⌨️Bumon42 来连接。此时默认的是第一台设备，对应Bt0。如果需要连接新设备，需要 `fn` + `s` 切换到Bt1， `fn` + `d` 切换到Bt2以此类推， `a` ,  `s` , `d` , `f` , `g` 依次对应Bt0，Bt1，Bt2，Bt3，Bt4这五台设备，最多可同时使用5台设备。如果需要重新配对对应设备，则通过 `fn` +  `tab` 来重新配置当前蓝牙设备号。

## 输入数字

按住 `左空格` , `Q`  `W`  `E` 这一行对应12345的数字键，也就是60佩列的数字键，如果需要输入数字键的符号，则需要再按上 `shift` 键才能输入。

## 输入符号

按住 `右空格`，红色的字符为按住时输入的字符。具体请看键位图

## 灯光

### 开启和关闭

按住`左空格`  `右空格` + `右CTRL` 或者旋钮按下，可开启或关闭灯光。

### 更改颜色

按住`左空格` 和 `右空格` ， `A` `Z` 为修改色相， `S` `X` 为修改饱和度， `D` `C` 为修改亮度+-。注意，因为保险丝限流为500ma，所以在白光等耗电比较大的灯光时，较高的亮度可能导致保险丝熔断后自恢复，具体表现为灯光突然变暗，此为正常现象。

### 更改模式

按住`左空格` 和 `右空格` ， `F` `V`为修改速度， `G` `B` 为修改颜色预设模式。ZMK固件颜色变换比较少，但也够用了hhhh

## 输出设备

按住 `Fn` ， `Z` 为使用USB输出， `X` 为使用蓝牙输出， `C` 为切换输出模式，即原本为USB则切换为蓝牙，原本为蓝牙则切换到USB。ZMK固件是插上TypeC线的时候默认为USB输出，所以在你想一边充电一边使用蓝牙，或者插线给电脑用的同时想使用平板等移动设备，则可以使用本条来切换输出设备。

# 更改键位
---
✅2023-04-16更新

zmk进行了一次大更新后，需要修改来的文件结构等操作才可以继续依托github来构建固件，忙里偷闲更新了下相关的代码，并借此机会小小更新一下文档😆

目前zmk已有可视化修改代码的网页支持，但仍然需要github的action来编译构建固件，不论如何对于小白的改键难度和成本降低了不少，感谢开源。相关说明已经补充，相关网站在此👇

https://nickcoutsos.github.io/keymap-editor/
---
## 前言

~~ZMK改键暂时没有像 `VIA` 之类的实时改键的软件，官方说后续会支持，目前先静待更新吧😉~~。虽然ZMK的改键还是没有实时改键，但已有可视化修改keymap的网页可用，相比于原来纯文本的操作，GUI还是让人赏心悦目的。ZMK的改键整体来说还是比较方便快捷的，[官方指南](https://zmk.dev/docs/customization)在此。让我们期待ZMK越来越好用hhhh

## 教程

### 注册一个GitHub账号

在[GitHub](https://github.com/)上注册一个账号，如果已有的话可跳过此步。国内可能网络状况不太好，会出现偶尔上不了的情况，如果遇到了多刷新试试或者使用代理软件及github访问加速之类的服务。

![github图](http://tva1.sinaimg.cn/large/007c24l8gy1hfk44gh87cj31e90efk4i.jpg)

### 克隆此键盘的仓库

去到我的首页寻找仓库[DAydDaY](https://github.com/DAydDaY)/**[zmk-config](https://github.com/DAydDaY/zmk-config)**，点击 `Fork` 把仓库fork到自己的账号里，fork完应该会回到自己仓库里，再选择对应的键盘分支。

![2.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk45z4gdgj31et0dkdnn.jpg)


### 点击Actions启动该功能

这里我早就启动了，所以没有截图，点开点个确定就行了。

![3.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk46bwnjlj31xg0zftnr.jpg)


### 在线编辑器修改

❗**一定要在自己的仓库里修改**❗

直接找到对应的文件，进入之后点击编辑

![4.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asgi27j31880igjyt.jpg)


以 `bumon42` 为例子，选好`bumon42` 的分支之后，进入到下列目录，主要是修改2个文件： `Bumon42.conf` 和 `Bumon42.keymap` 

目录：z**mk-config**/config/boards/shields/**Bumon42**/

 `键盘名.conf` 开关RGB，OLED，编码器等

 `键盘名.keymap` 键位

---

#### .conf 配置项

这个其实是python写的， `#` 是注释符号， `#` 后面还有空格，取消注释的时候不要漏了空格。对应的开关功能见代码上的注释（如下所示）

![5.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asf6evj319p0hcqa2.jpg)


编辑完之后，往下滚，输入commit信息提交，当然不输入也没关系。

![6.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4at0j1fj317y0cd0yi.jpg)


#### .keymap 键码

修改方式一样，keymap内有各个层，改键需要参照[官方手册](https://zmk.dev/docs/codes/)，内容太多我就不翻译出来了，浏览器翻译一下不太影响可读性，直接让我说不太好针对说出什么，这一部分等大家群里问的时候再逐步完善吧！

> 先说一下怎么查阅[手册](https://zmk.dev/docs/codes/)
> 

![7.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asepdfj315h0ofgzl.jpg)


描述下面有and的，如 `a and A` 指的是是否按住 `shift` 时按下的操作

> 怎样修改
> 

这里列举一些可能常用的，日后有需要再慢慢补充，详细keymap介绍可[参阅文档](https://zmk.dev/docs/features/keymaps)

**一般按键 `&kp`**

`&kp` 后接 Names，如按键 `A`  为`&kp  A` 

**长按切层按键  `&lt`** 

`&lt`  后接前面define的层和Names，如短按为windows键,长按为FN层为`&lt FN RGUI` 

![8.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asdv5aj309204qmxv.jpg)


须知：`&lt`  只能顺着箭头方向“往上提升”，如果想从5层 `&lt`  到3层是不可行的。

**永久切层按键 `&to`**

这个我用来切换层，比如一层默认打字键位，一层打游戏键位，一层敲代码键位之类的，可用这个前缀来实现。

`&to` 后接定义的层，如去到游戏层 `GAM` 为`&to  GAM` 

#### 改蓝牙名称

如下图箭头所指双引号中的内容就是键盘名称，名称不能太长，中文行不行我还没试过。

![9.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4ase41uj31e50mz456.jpg)


---

### 去Actions选项卡下下载固件

如果没有语法错误的话，成功编译大概会花费2分钟的时间，在 `Actions` 选项卡下找到最新的工作任务，点进去下载固件吧！

![10.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4ase3huj31av0k511g.jpg)


![11.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asokz1j315r0khq77.jpg)


下载是一个压缩包，请解压它获取`zmk.uf2`!

### 烧录固件

ZMK的烧录固件真的值得一夸，用usb连接电脑， `fn` + `/?` 键，就会弹出一个可移动设备，把前面得到的 `zmk.uf2` 文件直接拖进去即可完成烧录！如果`fn` + `/?` 无效或者键码暂时失效，请通过6颗螺丝拆下pcb（注意连接的电线），然后在pcb背面下方有一个小的实体的reset按钮，可通过双击此按钮来进入烧录模式（这一步应该不用配图了吧）

![12.jpg](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asekkvj30xc0xc0tg.jpg)


# keymap-editor使用教程

![13.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asmhiaj30ol0gwq7w.jpg)


通过[keymap-editor](https://nickcoutsos.github.io/keymap-editor/)的网站，绑定自己的github账号后，我们可以在左上角的下拉框中选择自己的对应的仓库和分支，即可通过页面来修改keymap值。

【特别注意】👇

之前已经fork过仓库的朋友们，可通过[该文章](https://zhuanlan.zhihu.com/p/291845721)内描述的方法来update一下自己的仓库！

![14.png](http://tva1.sinaimg.cn/large/007c24l8gy1hfk4asekm8j30cj05174l.jpg)


修改完成后，和github一样需要commit，commit之后可通过右方蓝色的按钮直达固件下载页，等待固件完成编译后，即可通过上述同样的步骤完成键盘新固件的烧录。到此，享受你的键盘吧！😆
