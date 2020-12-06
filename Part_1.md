# Part One
## 一、认识Linux
### 1. 操作系统演化历史
- 从纸带机开始，操作系统就在配合cpu的道路上撒丫子狂奔。从人工操作到分布式和批处理，操作系统发展出了多道程序同时运行、同一cpu多用户使用的各种花招。
- UNIX：起源于MULTICS，可是后来，MULTICS项目开始迷失，目标过于庞大，功能过于复杂，研发的人们越来越不明白这个项目将会如何走下去。最终隶属 AT&T 公司的贝尔实验室（Bell Labs）退出了这个项目，在这样的背景下，UNIX以游戏系统+c语言的形式诞生。因为反垄断制裁，UNIX真正意义上的开源过一段时间，但是开源社区的开发促进了UNIX发展与风靡，一系列商业竞争后成为价格昂贵的产品，催生对便宜操作系统的追求
- GNU：UNIX商业闭源后，RMS模仿 Unix 的界面和使用方式，从头做一个开源的版本，做出编辑器 Emacs 和编译器 GCC。GNU 是一个计划或者叫运动。在这个旗帜下成立了 FSF，起草了 GPL 等。开源社区在 GNU 计划下做了很多的工作和项目，基本实现了当初的计划。包括核心的 gcc 和 glibc。但是 GNU 系统缺少操作系统内核。原定的内核叫 HURD，一直完不成。同时 BSD（一种 UNIX 发行版）陷入版权纠纷，x86 平台开发暂停。
- Linux：学生时代的Linus啃着本《操作系统：设计与实现》模仿MINIX（类UNIX,微内核）开发粗linux（宏内核，也只是内核）,制作的系统启动之后使用的仍然是 gcc 和 bash 等软件。Linus 在发布 Linux 的时候选择了 GPL，因此符合 GNU 的宗旨，被不满意HURD内核的GNU相中，拼粗了GNU/Linux系统，
- 参考资料：

    [《GNU 是什么，和 Linux 是什么关系？》](https://www.zhihu.com/question/319783573/answer/656033035)

    [《Unix、Windows、Mac OS、Linux系统故事》](https://zhuanlan.zhihu.com/p/48834280)

    [《为何 Linus 一个人就能写出这么强的系统，中国却做不出来？》](https://www.zhihu.com/question/63187737/answer/1415937231)
### 2. 为什么使用Linux
#### (1). Linux的特性
- 免费（超重点）
- 宏内核，易用性更好（为了兼容东拼西凑的pc零件）；微内核的提升也不很大
- 完全兼容POSIX1.0标准，可以在Linux下通过相应的模拟器运行常见的DOS、Windows的程序
- 支持多用户，各用户之间互不影响；支持多任务，多个程序可以同时并独立地运行；支持多平台，Linux可以在多种硬件平台上运行
- 是一种嵌入式操作系统，能够根据用户需求灵活裁剪软硬件模块
- 支持多处理器技术，系统性能较高
- 支持良好的图形界面

#### (2). Linux的优点
- 开源免费，成本低，有良好的社区生态
- 嵌入式特性：易于裁剪、开发嵌入式作品
- 定制性强，从桌面环境到开发环境皆可定制
- 安全，社区集众人之力找bug,能被利用的漏洞相对较少
- 具有良好的编程环境
- 培养初学者（比如我）/半吊子的能力
- 轻巧，除了嵌入式操作外还可以充分利用老旧手机/计算机
- 更新/安装方便

****
## 二、接触linux
### 1. 安装
#### (1). 想法：
- 想要arch,喜欢其操作方式、更新速度以及详细的wiki

- 不想手动装arch,7月份装了一中午才弄好

- manjaro是arch的衍生版，相似度极高，更新速度有所不如

#### (2). 解决方案：manjaro换arch源更新，省时省力（其实是我懒）
#### (3). 操作
- 第一步：图形化界面安装manjaro

![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/09.jpg)

**Virtual Box配置**

**设定启动顺序为光盘早于硬盘**    

**挂载ISO镜像“manjaro-kde-20.1.1-201001-linux58.iso”**    

**挂载增强扩展包VBoxGuestAdditions_6.0.24.ISO**     

**启动进入安装界面，操作如下**


![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/01.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/02.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/03.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/04.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/05.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/06.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/07.jpg)
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/08.jpg)

（  因为还没准备好图床，就只能先用我的github仓库凑活一下  ）

关闭虚拟机，删除先前的两个光盘，启动顺序中只留下硬盘

重启，打开Konsole，敲命令

```
#安装我的生存必需品
sudo pacman -S vim
sudo pacman -S yay

#删除发行版信息
sudo rm -f /etc/lsb-release

#换源-1
sudo vim /etc/pacman.d/mirrorlist

#更改为
## aliyun
Server = http://mirrors.aliyun.com/archlinux/$repo/os/$arch

## 163
Server = http://mirrors.163.com/archlinux/$repo/os/$arch

## 清华大学
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch

#换源-2
sudo vim /etc/pacman.conf

#添加
[archlinuxcn]
# The Chinese Arch Linux communities packages.
# SigLevel = Optional TrustedOnly
SigLevel = Optional TrustAll

# 163源

Server = http://mirrors.163.com/archlinux-cn/$arch

#换源-3
yay --aururl “https://aur.tuna.tsinghua.edu.cn” --save

#更新，注入灵魂
sudo pacman -Syyu

##安装中文输入法
sudo pacman -S fcitx5-im
sudo pacman -S fcitx5-chinese-addons

#配置变量
vim ~/.pam_environment

#写入
INPUT_METHOD  DEFAULT=fcitx5
GTK_IM_MODULE DEFAULT=fcitx5
QT_IM_MODULE  DEFAULT=fcitx5
XMODIFIERS    DEFAULT=\@im=fcitx5

```
因为之前将fcitx设为开机自启莫名其妙翻车用不了，所以我决定将他添加到面板（任务栏），毕竟这个虚拟机才是我的主力机（捂脸）（因为要用ms office、qq和微信才保留了WIN10）

然后我就高兴地重启，进入应用管理器（姑且这么叫吧）安装软件了(〃∇〃)，因为懒得去查找需要些啥

直接进应用管理器搜wps、vscode（用来写.md文件 (〃∇〃) ）、网抑云

****
### 2. 美化

(1). 外观：整体选择Breath2的浅色样式；字体选择nNoto Serif CJK TC Medium，字号整体调大4号

(2). 工作空间行为：动画速度设置为最快，桌面特效更改“模糊”和“透明度”

(3). 桌面面板配置：将任务图标全部收入“状态和通知”，面板自动隐藏，应用程序面板配置为全屏

(4). 注：因为个人喜好的原因，其实改动并不大。与主机的对比如下：

![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/12.jpg)

(5). 以下为配置效果
![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/11.jpg)


****
## 三、使用Linux
### 运行一段c代码(源码如下)
```
#include <math.h>
#include <stdio.h>

int main(void)
{
    int x,y;
    for(y=0;y>=-6;y--)
    {
        for(x=0;x<=6;x++)
        if(fabs(x-3)+fabs(y+3)<=3)
        printf("*");
        else
        printf(" ");
        printf("\n");
    }
}
```

运行截图如下

![](https://raw.githubusercontent.com/NeiitDoor/Neiit-sNotes4LinuxLearning/main/imgs/part1-imgs/13.jpg)

