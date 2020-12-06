# Linux方向-第二期
## 一、一切皆文件
### Linux的文件系统
<br />

#### I. Linux文件系统层次


>![](https://img-blog.csdnimg.cn/20190529154719250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dpdGh1Yl8zNzg4MjgzNw==,size_16,color_FFFFFF,t_70)

>![](https://img-blog.csdnimg.cn/20190529155953264.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2dpdGh1Yl8zNzg4MjgzNw==,size_16,color_FFFFFF,t_70)

如上图所示

- （底层）硬件系统

   即CPU、RAM、磁盘、网口等   

- 设备驱动程序

   在Linux系统中，对不同硬件所提供的驱动模块一般都存放在内核目录树 /drivers/ata 中，而对于一般通用的硬件驱动，也许会直接被编译到内核中，而不会以模块的方式出现。

- 内核  

  - 通用串行总线层（win10是这么叫的）

     将不同的 Input/Output 接口抽象为统一的对外接口，便于管理。因为是通用的，所以这一层的任何改动都会直接影响到所有文件系统。

  - 文件系统

     即管理文件的系统。

  - 虚拟文件系统（VFS）与系统调用（SCI）

     文件系统相对较为复杂，各有各的API接口，其复杂性与用户的操作需求不对标，因此通过VFS对不同的文件系统做一个抽象，提供统一的API访问接口，再经过系统调用的包装，让用户可以经过SCI的系统调用来操作不同的文件系统。


- 用户级程序（用户直接接触的应用）

   图形用户界面、服务器、命令行等。


参考资料：
[《Linux文件系统详解》](https://blog.csdn.net/github_37882837/article/details/90672881)
[《Linux操作系统的层次与组成》](https://blog.csdn.net/lilygg/article/details/89309461)<br /><br /><br />

#### II. Linux的存储介质



 闪存（Flash Memory）是一种长寿命的非易失性（在断电情况下仍能保持所存储的数据信息）的存储器，数据删除不是以单个的字节为单位而是以固定的区块为单位（注意：NOR Flash 为字节存储。），区块大小一般为256KB到20MB。闪存是电子可擦除只读存储器（EEPROM）的变种，EEPROM与闪存不同的是，它能在字节水平上进行删除和重写而不是整个芯片擦写，这样闪存就比EEPROM的更新速度快。由于其断电时仍能保存数据，闪存通常被用来保存设置信息，如在电脑的BIOS（基本输入输出程序）、PDA（个人数字助理）、数码相机中保存资料等。

  外存通常是磁性介质或光盘，像硬盘，软盘，磁带，CD等，能长期保存信息，并且不依赖于电来保存信息，但是由机械部件带动，速度与CPU相比就显得慢的多。内存指的就是主板上的存储部件，是CPU直接与之沟通，并用其存储数据的部件，存放当前正在使用的（即执行中）的数据和程序，它的物理实质就是一组或多组具备数据输入输出和数据存储功能的集成电路，内存只用于暂时存放程序和数据，一旦关闭电源或发生断电，其中的程序和数据就会丢失。

  RAM又分为动态的和静态。。静态被用作cache，动态的常用作内存。。网上说闪存不能代替DRAM是因为闪存不像RAM（随机存取存储器）一样以字节为单位改写数据，因此不能取代RAM。这个以后可以了解下硬件的知识再来辨别。
 
 参考资料：[《Linux文件系统详解》](https://blog.csdn.net/github_37882837/article/details/90672881)<br /><br /><br />

#### III. Linux主要文件系统分类


**1. ext文件系统**

即第一代扩展文件系统，是一种文件系统，于1992年4月发表，是为linux核心所做的第一个文件系统。它采用Unix文件系统（UFS）的元数据结构，同时也是是在linux上第一个利用虚拟文件系统实现出的文件系统，克服了MINIX文件系统性能不佳的问题。

**其特点如下：**

- 采用名为索引节点的系统来存放虚拟目录中所存储文件的信息。
- 索引节点系统在每个物理设备中创建一个单独的表(称为索引节点表)来存储这些文件的信息。
- 存储在虚拟目录中的每一个文件在索引节点表中都有一个条目


**同时也有一定的局限性：**
- 文件大小不得超过2 GB
- 存储数据用的块很容易分散在整个设备中(称作碎片化,fragmentation) 数据块的碎片化会降低文件系统的性能。  


>![](https://img-blog.csdn.net/20170809005817126?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc2NTMxNDQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

图解（图中灰色为incode块，蓝色为block块）<br /><br />

**2. ext2文件系统**

Ext2于1993年1月加入Linux到核心支持之中，Ext2文件系统是ext文件系统基本功能的一个扩展,但保持了同样的结构。Ext2文件系统扩展了索引节点表的格式来保存系统上每个文件的更多信息。为了代替Ext1，其单一文件大小与文件系统本身的容量上限以及文件系统本身的簇大小有关。一般的X86架构电脑来说簇最大为4KB，则单一文件理论大小上限为2TB，而文件系统的容量上限为16TB。Ext2主要有如下几个特点：

- 当创建Ext2文件系统时，系统管理员可以根据预期的文件平均长度来选择最佳的块大小（从1024B-4096B）。
- 当创建Ext2文件系统时，系统管理员可以根据在给定大小的分区上预计存放的文件数来选择给该分区分配多少索引节点，以有效地利用磁盘空间。
- ext2的索引节点表为文件添加了创建时间值、修改时间值和最后访问时间值来帮助系统管理员追踪文件的访问情况。
- 文件系统把磁盘块分为组，形成多个块组，每个块组都有独立的inode/block/super block系统。
- 在磁盘数据块被实际使用之前，文件系统就把这些块预分配给普通文件。因此当文件的大小增加时，因为物理上相邻的几个块已被保留，这就减少了文件碎片。
- 支持快速符号链接。如果符号链接表示一个短路径名（小于或等于60个字符），就把它存放在索引节点中而不用通过由一个数据块进行转换。


**其主要结构如下图所示：**

![](https://img-blog.csdn.net/20170809010221881?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMzc2NTMxNDQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

**缺点** 

- ext2文件系统容易在系统崩溃或断电时损坏
- 即使文件数据正常保存到了物理设备上,如果索引节点表记录没完成更新的话,ext2文件系统甚至都不知道那个文件存在
- 非日志文件系统 
- data block方面

  - Ext2文件系统中block主要为1KB、2KB、4KB三种大小。除此之外，data block大小还有如下限制：

  - 原则上，block大小与数量在格式化完就不能再改变了，除非重新格式化；

  - 每个block内最多只能够放置一个文件的数据；

  - 承上，如果文件大于block的大小，则一个文件会占用多个block数量；

  - 承上，若文件小于block，则该block的剩余空间也不能再被使用了（因此磁盘空间会被浪费）。

  (所以，虽然块容量越大单一文件容量和系统文件容量的上限也越大，但不根据文件长度预期而盲目地选择大容量块可能会浪费大量存储空间)<br /><br />

**3. ext3文件系统**

ext3文件系统是一个日志文件系统，是由ext2发展而来并完全兼容前者，ext2可直接更新到ext3。相较于其前身，ext3具有以下优点：
- 高可用性：系统使用了ext3文件系统后，即使在非正常关机后，系统也不需要检查文件系统。
- 数据的完整性：避免了意外宕机对文件系统的破坏，且恢复耗时极短。
- 文件系统的速度：因为ext3的日志功能对磁盘的驱动器读写头进行了优化。所以，文件系统的读写性能较之Ext2文件系统并来说，性能并没有降低。
- 数据转换 ：由ext2文件系统转换成ext3文件系统非常容易。
- 具有多种日志模式，可以跟踪记录文件系统的变化,并将变化内容写入日志,写操作首先是对日志记录文件进行操作。
- ext3文件系统用有序模式的日志功能——只将索引节点信息写入日志文件,直到数据块都被成功写入存储设备才删除。

**ext3文件系统的缺点**

- ext3文件系统无法恢复误删的文件。
- 它没有任何内建的数据压缩功能(虽然有个需单独安装的补丁支持这个功能)
- 不支持加密文件。<br /><br />

**4. ext4文件系统**

是Linux系统下的日志文件系统，是ext3文件系统的后继版本。Ext4是由Ext3的维护者Theodore Tso领导的开发团队实现的，并在引入到Linux2.6.19内核中，现在已是大多数流行的Linux发行版采用的默认文件系统。

优化：
- 延迟分配
- 快速fsck
- 日志校验
- “无日志”（No Journaling）模式
- 在线碎片整理
- inode 相关特性：较之Ext3默认的inode大小128字节，ext4默认inode大小为256字节
- 与Ext3兼容：执行若干条命令，就能从Ext3在线迁移到Ext4，而无须重新格式化磁盘或重新安装系统。
- 支持更大的文件系统和更大的文件：较之Ext3目前所支持的最大16TB文件系统和最大2TB文件，Ext4分别支持1EB（1,048,576TB，1EB=1024PB，1PB=1024TB）的文件系统，以及16TB 的文件。
- 支持无限数量的子目录：Ext3目前只支持32,000个子目录，而Ext4支持无限数量的子目录。
- Extents：Ext4引入了现代文件系统中流行的extents概念，每个 extent 为一组连续的数据块，相比Ext3采用间接块映射，提高了不少效率。
- 块分配：Ext4 的多块分配器“multiblock allocator”（mballoc） 支持一次调用分配多个数据块。
- 引入了块预分配技术(block preallocation)如果你想在存储设备上给一个你知道要变大的文件预留空间,ext4文件系统可以为文件分配所有需要用到的块,而不仅仅是那些现在已经用到的块。ext4文件系统用0填满预留的数据块,不会将它们分配给其他文件 

格式化为Ext4格式之后硬盘布局大概是这样：

![](https://img-blog.csdn.net/20170103131027729?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjUxMTUxOA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

<br /><br />

**5. btrfs文件系统**

很多有关Ext4的文档不约而同地谈及了Btrfs文件系统,并认为ext文件系统是一个过渡文件系统，Btrfs文件系统将成为下一代文件系统的标准。相较于Ext系列文件系统，Btrfs文件系统新增了以下特性：
- 支持容量的动态扩展和缩减
- 支持快照功能
- 支持对快照做快照，以实现增量更新机制
- 支持COW(Copy On Write，写时复制)
- 支持对单个文件的快照功能。
- 多物理卷支持：一个文件系统可由多个底层物理卷组成；支持RAID,以联机“增加”“移除”“修改”
- 支持子卷：sub_volume
- 透明压缩----无需用户参与<br /><br />

**6. reiserFS**

 reiserFS是Linux环境下最稳定的日志文件系统之一，使用快速的平衡二叉树（binary tree）算法来查找磁盘上的自由空间和已有的文件，其搜索速度高于ext2，reiserFS能够像其他大多数文件系统一样，可动态的分配索引节，而无须在文件系统中创建固定的索引节。有助于文件系统更灵活的适应各种存储需要。<br /><br />

**7. VFAT**

VFAT主要用于处理长文件的一种文件名系统，它运行在保护模式下并使用VCACHE进行缓存，并具有和Windows系列文件系统和Linux文件系统兼容的特性。因此VFAT可以作为Windows和Linux交换文件的分区。

参考资料：[《ext文件系统》](https://blog.csdn.net/xiangjie256/article/details/84907006?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160403892119724822522687%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160403892119724822522687&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-84907006.first_rank_ecpm_v3_pc_rank_v2&utm_term=ext%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F&spm=1018.2118.3001.4449)[《linux之ext、ext1、ext2、ext3、ext4文件系统的区别及常用命令》](https://blog.csdn.net/weixin_43972437/article/details/102911403?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160403915419724838546186%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160403915419724838546186&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v28-1-102911403.first_rank_ecpm_v3_pc_rank_v2&utm_term=ext1%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F&spm=1018.2118.3001.4449)[《Linux之btrfs详解2015082901》](https://blog.csdn.net/weixin_33796205/article/details/93051695)<br /><br />

#### IV. Linux的储存结构

- **linux下并不存在C/D/E/F盘，所有的文件及目录都是以树形结构划分的，并且每个文件都规定了自己的作用范围。**

![](https://img-blog.csdn.net/20171115190726397?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluOTcwNTA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- **每个目录的具体作用：**
![](https://img-blog.csdn.net/20171115190847203?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvamluOTcwNTA1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### V. Linux文件系统的inode

**1. inode是什么**

硬盘上文件存储的最小单位为扇区（sector），每个扇区储存512字节。 操作系统读取硬盘上数据时，一次性读取一个block（最常见的是1 block == 8 sector）。文件储存在block中，就得有一个相应的区域储存文件的创建者、创建日期和文件大小等信息。该区域即为inode，中文名叫索引节点。硬盘上每一个文件都有对应的inode。<br /><br />

**2. inode的内容**

inode包含文件的源信息，总体来说有以下内容：
- Access 文件的读、写和执行权限
- Size 文件的字节数
- Uid 文件拥有者的USER ID
- Gid 文件的Group ID
- 文件的时间戳，共有三个：
  - Change：inode上次变动的时间
  - Modify：文件内容上次变动的时间
  - Access：文件上次打开的时间
- Links 链接数，即有多少个文件名指向这个inode
- inode 文件数据block的位置信息
- Blocks block数
- IO Blocks block大小
- Device 设备号码

注：没有文件名的

查看命令：

```
stat example.txt
```

例如：

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-000.jpg)<br /><br />

**3. inode的大小**

inode也会消耗硬盘空间，所以硬盘格式化的时候，操作系统自动将硬盘分为两个区域。一个是数据区，存放文件数据；另一个是inode区（inode table），存放inode所包含的信息。

每个inode节点的大小，一般是128字节或256字节。inode节点的总数，在格式化时就给定，一般是每1KB或每2KB就设置一个inode。假定在一块1GB的硬盘中，每个inode节点的大小为128字节，每1KB就设置一个inode，那么inode table的大小就会达到128MB，占整块硬盘的12.8%。

查看每个硬盘分区的inode总数和已经使用的数量，可以使用df命令。

```
df -i
```

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-001.jpg)

查看每个inode节点的大小，可用如下命令

```
sudo dumpe2fs -h /dev/hda | grep "Inode size"
```

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-002.png)

(懒得再去找一个文件了   ("▔㉨▔)汗    )

于每个文件都必须有一个inode，因此有可能发生inode已经用光，但是硬盘还未存满的情况。这时，就无法在硬盘上创建新文件。<br /><br />

**4. inode号码**

每个inode都有一个号码，操作系统用inode号码来识别不同的文件。

**Unix/Linux系统内部不使用文件名，而使用inode号码来识别文件。对于系统来说，文件名只是inode号码便于识别的别称或者绰号。**

表面上，用户通过文件名，打开文件。实际上，系统内部这个过程分成三步：首先，系统找到这个文件名对应的inode号码；其次，通过inode号码，获取inode信息；最后，根据inode信息，找到文件数据所在的block，读出数据。

使用ls -i命令，可以看到文件名对应的inode号码：

```
ls -i example.txt
```

如图：

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-003.jpg)

<br /><br />

**5. 目录文件**

**Unix/Linux系统中，目录（directory）也是一种文件。打开目录，实际上就是打开目录文件。**

目录文件的结构非常简单，就是一系列目录项（dirent）的列表。每个目录项，由两部分组成：所包含文件的文件名，以及该文件名对应的inode号码。

ls命令只列出目录文件中的所有文件名，`ls -i`命令列出整个目录文件，即文件名和inode号码：

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-004.jpg)

**如果要查看文件的详细信息，就必须根据inode号码，访问inode节点读取信息。目录文件的读权限（r）和写权限（w），都是针对目录文件本身。由于目录文件内只有文件名和inode号码，所以如果只有读权限，只能获取文件名，无法获取其他信息，因为其他信息都储存在inode节点中，而读取inode节点内的信息需要目录文件的执行权限（x）。**
<br /><br />

**6. 硬链接**

一般情况下，文件名和inode号码是"一一对应"关系，每个inode号码对应一个文件名。但是，Unix/Linux系统允许，多个文件名指向同一个inode号码。

这意味着，可以用不同的文件名访问同样的内容；对文件内容进行修改，会影响到所有文件名；但是，删除一个文件名，不影响另一个文件名的访问。这种情况就被称为"硬链接"（hard link）。

ln命令可以创建硬链接：

```
ln 源文件 目标文件
```

![](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTgwMzA3MTY0NTI5MzQz?x-oss-process=image/format,png)

**《关于我因为害怕搞坏虚拟机而从网上刮图这件事》**

运行上面这条命令以后，源文件与目标文件的inode号码相同，都指向同一个inode。inode信息中有一项叫做"链接数"，记录指向该inode的文件名总数，这时就会增加1。

反过来，删除一个文件名，就会使得inode节点中的"链接数"减1。当这个值减到0，表明没有文件名指向这个inode，系统就会回收这个inode号码，以及其所对应block区域。

这里顺便说一下目录文件的"链接数"。创建目录时，默认会生成两个目录项：".“和”…"。前者的inode号码就是当前目录的inode号码，等同于当前目录的"硬链接"；后者的inode号码就是当前目录的父目录的inode号码，等同于父目录的"硬链接"。所以，任何一个目录的"硬链接"总数，总是等于2加上它的子目录总数（含隐藏目录）。
<br /><br />

**7. 软链接**

除了硬链接以外，还有一种特殊情况。

文件A和文件B的inode号码虽然不一样，但是文件A的内容是文件B的路径。读取文件A时，系统会自动将访问者导向文件B。因此，无论打开哪一个文件，最终读取的都是文件B。这时，文件A就称为文件B的"软链接"（soft link）或者"符号链接（symbolic link）。

这意味着，文件A依赖于文件B而存在，如果删除了文件B，打开文件A就会报错：“No such file or directory”。这是软链接与硬链接最大的不同：文件A指向文件B的文件名，而不是文件B的inode号码，文件B的inode"链接数"不会因此发生变化。

ln -s命令可以创建软链接：

```
ln -s 源文文件或目录 目标文件或目录
```

![](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTgwMzA3MTY0NjMyNzI4?x-oss-process=image/format,png)

<br /><br />

**8. inode的特殊作用**

由于inode号码与文件名分离，这种机制导致了一些Unix/Linux系统特有的现象。

有时，文件名包含特殊字符，无法正常删除。这时，直接删除inode节点，就能起到删除文件的作用。
移动文件或重命名文件，只是改变文件名，不影响inode号码。
打开一个文件以后，系统就以inode号码来识别这个文件，不再考虑文件名。因此，通常来说，系统无法从inode号码得知文件名。
第3点使得软件更新变得简单，可以在不关闭软件的情况下进行更新，不需要重启。因为系统通过inode号码，识别运行中的文件，不通过文件名。更新的时候，新版文件以同样的文件名，生成一个新的inode，不会影响到运行中的文件。等到下一次运行这个软件的时候，文件名就自动指向新版文件，旧版文件的inode则被回收。
<br /><br />

**9. 实操**

- 超多文件数量文件夹寻找

新建`scan.sh`文件，保存如下内容：

```
florder=$1
dir=$(ls -l $florder |awk '/^d/ {print $NF}')
for i in $dir
do
    if [ "$i" != 'home' -a "$i" != 'proc' ];then
    f=$i
    if [ $florder != '/' ];then
        f=$florder/$i
    fi
        rs=$(ls -lR $f|grep "^-"| wc -l)
    echo $f 文件以及子文件个数 $rs
    fi
done

```

主目录运行->子目录运行->…，没有特别好的办法，挨个文件夹执行，会遍历当前目录下文件夹的文件数量。

- 删除文件夹中的内容

```
find /dir/ -type f -name '*' -print0 | xargs -0 rm &
```
执行后，时间较长，可以用htop查看find进程

参考资料：
>[《Linux的inode的理解》](https://blog.csdn.net/xuz0917/article/details/79473562?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160490061419724838547363%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160490061419724838547363&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-79473562.pc_first_rank_v2_rank_v28&utm_term=linux+inode&spm=1018.2118.3001.4449)

### 练习题目
#### 1. Linux各个分区的作用和内容

- **磁盘分区**

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/15.jpg)

- **tmpfs**

tmpfs是一种是基于内存虚拟文件系统，而不是块设备，创建时不需要使用mkfs等初始化。

它最大的特点就是它的存储空间在VM(virtual memory)，VM是由linux内核里面的vm子系统管理的。LINUX中可以把一些程序的临时文件放置在tmpfs中，利用tmpfs比硬盘速度快的特点提升系统性能。 

linux下面VM的大小由RM(Real Memory)和swap组成,RM的大小就是物理内存的大小，而Swap的大小是由自己决定的。

Swap是通过硬盘虚拟出来的内存空间，因此它的读写速度相对RM(Real Memory）要慢许多，当一个进程申请一定数量的内存时，如内核的vm子系统发现没有足够的RM时，就会把RM里面的一些不常用的数据交换到Swap里面，如果需要重新使用这些数据再把它们从Swap交换到RM里面。如果有足够大的物理内存，可以不划分Swap分区。(termux安装系统的时候好像也没有分区这一步 ("▔㉨▔)汗 ) 

- **磁盘分区**

分为基本分区，扩充分区和逻辑分区，其中扩充分区包含逻辑分区。

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/14.jpg)

这是我手机上manjaro-arm的文件目录/，各文件夹的作用如下
```
/bin 
/boot
/dev
/etc
/home
/lib
/mnt
/opt
/proc
/root
/run
/sbin
/srv
/sys
/tmp
/usr
/var

```

## 二、Linux的基本命令

### 命令

<br />
我日常使用manjaro和Sparky（基于Debian），所以包管理器有关的命令得会apt、apt-get、dpkg以及pacman等打头的东西

**系统信息** 

|命令     |作用     |
|-----------------|-----------------|
|lsb_release-a| 显示系统的发行版信息 |
|arch| 显示机器的处理器架构 |
|uname -m| 显示机器的处理器架构 |
|uname -r| 显示正在使用的内核版本 |
|dmidecode -q| 显示硬件系统部件 - (SMBIOS / DMI) |
|hdparm -i /dev/hda| 罗列一个磁盘的架构特性 |
|hdparm -tT /dev/sda| 在磁盘上执行测试性读取操作 |
|cat /proc/cpuinfo| 显示CPU info的信息 |
|cat /proc/interrupts| 显示中断 |
|cat /proc/meminfo| 校验内存使用 |
|cat /proc/swaps| 显示哪些swap被使用 |
|cat /proc/version| 显示内核的版本 |
|cat /proc/net/dev| 显示网络适配器及统计 |
|cat /proc/mounts| 显示已加载的文件系统 |
|lspci -tv| 罗列 PCI 设备 |
|lsusb -tv| 显示 USB 设备 |
|date| 显示系统日期 |
|cal 20xx| 显示20xx年的日历表，不加后缀就显示当前月历 |
|date 0412170020xx.00| 设置日期和时间 - 月日时分年.秒 |
|date "+%Y %m %d %H %M %S"| 打印系统时间（年月日时分秒），中间连接的符号随意 |
|clock -w| 将时间修改保存到 BIOS |
-----
<br />

**系统的关机、重启以及登出**

|命令     |作用     |
|-----------------|-----------------|
|shutdown -h now| 关闭系统 |
|init 0| 关闭系统 |
|telinit 0| 关闭系统 |
|shutdown -h hours:minutes &| 按预定时间关闭系统 |
|shutdown -c| 取消按预定时间关闭系统 |
|shutdown -r now| 重启 |
|reboot| 重启 |
|logout| 注销 |
-----
<br />

**文件和目录**

|命令     |作用     |
|-------------|-----------------|
|cd /home| 进入 '/ home' 目录' |
|cd ..| 返回上一级目录 |
|cd ../..| 返回上两级目录 |
|cd| 进入个人的主目录 |
|cd ~user1| 进入个人的主目录 |
|cd -| 返回上次所在的目录 |
|pwd| 显示工作路径 ||
|ls| 查看目录中的文件 |
|ls -F| 查看目录中的文件 |
|ls -l| 显示文件和目录的详细资料 |
|ls -a| 显示隐藏文件 |
|ls *[0-9]*| 显示包含数字的文件名和目录名 |
|tree| 显示文件和目录由根目录开始的树形结构|
|lstree| 显示文件和目录由根目录开始的树形结构|
|mkdir dir1| 创建一个叫做 'dir1' 的目录' |
|mkdir dir1 dir2| 同时创建两个目录 |
|mkdir -p /tmp/dir1/dir2| 创建一个目录树 |
|rm -f file1| 删除一个叫做 'file1' 的文件' |
|rmdir dir1| 删除一个叫做 'dir1' 的目录' |
|rm -rf dir1| 删除一个叫做 'dir1' 的目录并同时删除其内容 |
|rm -rf dir1 dir2| 同时删除两个目录及它们的内容 |
|mv dir1 new_dir| 重命名/移动 一个目录 |
|cp file1 file2| 复制一个文件 |
|cp dir/* .| 复制一个目录下的所有文件到当前工作目录 |
|cp -a /tmp/dir1 .| 复制一个目录到当前工作目录 |
|cp -a dir1 dir2| 复制一个目录 |
|ln -s file1 lnk1| 创建一个指向文件或目录的软链接 |
|ln file1 lnk1| 创建一个指向文件或目录的物理链接 |
|touch -t 0712250000 file1| 修改一个文件或目录的时间戳 - (YYMMDDhhmm) |
|file file1| 查看文件类型 |
|iconv -l| 列出已知的编码 |
|iconv -f fromEncoding -t toEncoding inputFile| 转换文件并将输出打印在屏幕上 |

-----------
<br />

**文件搜索** 

|命令     |作用     |
|-------------|-----------------|
|find / -name file1| 从 '/' 开始进入根文件系统搜索文件和目录 |
|find / -user user1| 搜索属于用户 'user1' 的文件和目录 |
|find /home/user1 -name \*.bin| 在目录 '/ home/user1' 中搜索带有'.bin' 结尾的文件 |
|find /usr/bin -type f -atime +100| 搜索在过去100天内未被使用过的执行文件 |
|find /usr/bin -type f -mtime -10| 搜索在10天内被创建或者修改过的文件 |
|find / -name \*.deb -exec chmod 755 '{}' \;| 搜索以 '.deb' 结尾的文件并定义其权限 |
|find / -xdev -name \*.deb| 搜索以 '.deb' 结尾的文件，忽略光驱、捷盘等可移动设备 |
|locate \*.ps| 寻找以 '.ps' 结尾的文件 - 先运行 'updatedb' 命令 |
|whereis halt| 显示一个二进制文件、源码或man的位置 |
|which halt| 显示一个二进制文件或可执行文件的完整路径 |
------ 
<br />

**挂载一个文件系统**

|命令     |作用     |
|-------------|-----------------|
|mount /dev/hda2 /mnt/hda2| 挂载一个叫做hda2的盘 - 确定目录 '/ mnt/hda2' 已经存在 |
|umount /dev/hda2| 卸载一个叫做hda2的盘 - 先从挂载点 '/ mnt/hda2' 退出 |
|fuser -km /mnt/hda2| 当设备繁忙时强制卸载 |
|umount -n /mnt/hda2| 运行卸载操作而不写入 /etc/mtab 文件- 当文件为只读或当磁盘写满时非常有用 |
|mount /dev/fd0 /mnt/floppy| 挂载一个软盘 |
|mount /dev/cdrom /mnt/cdrom| 挂载一个cdrom或dvdrom |
|mount /dev/hdc /mnt/cdrecorder| 挂载一个cdrw或dvdrom |
|mount /dev/hdb /mnt/cdrecorder| 挂载一个cdrw或dvdrom |
|mount -o loop file.iso /mnt/cdrom| 挂载一个文件或ISO镜像文件 |
|mount -t vfat /dev/hda5 /mnt/hda5| 挂载一个Windows FAT32文件系统 |
|mount /dev/sda1 /mnt/usbdisk| 挂载一个usb 捷盘或闪存设备 |
|mount -t smbfs -o username=user,password=pass //WinClient/share /mnt/share| 挂载一个windows网络共享 |
-----
放弃制表 （X﹏X）
<br />



**磁盘空间**

```
df -h 显示已挂载的分区列表
ls -lSr |more 以尺寸大小排列文件和目录
du -sh dir1 估算目录“dir1”已经使用的磁盘空间
du -sk*|sort -m 以容量大小为依据依次显示文件和目录的大小
dpkg-query -W -f='${installed-Size;10}t${Package}n'|sort -k1,1n 以大小为依据依次显示已安装的rpm包所用的空间
```
<br />

**用户和群组**

```
groupadd group_name 创建一个新用户组 
groupdel group_name 删除一个用户组 
groupmod -n new_group_name old_group_name 重命名一个用户组 
useradd -c "Name Surname " -g admin -d /home/user1 -s /bin/bash user1 创建一个属于 "admin" 用户组的用户 
useradd user1 创建一个新用户 
userdel -r user1 删除一个用户 ( '-r' 排除主目录) 
usermod -c "User FTP" -g system -d /ftp/user1 -s /bin/nologin user1 修改用户属性 
passwd 修改口令 
passwd user1 修改一个用户的口令 (只允许root执行) 
chage -E 2005-12-31 user1 设置用户口令的失效期限 
pwck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的用户 
grpck 检查 '/etc/passwd' 的文件格式和语法修正以及存在的群组 
newgrp group_name 登陆进一个新的群组以改变新创建文件的预设群组 
```
<br />

**文件的权限** 

(使用 "+" 设置权限，使用 "-" 用于取消) 
```
ls -lh 显示权限 
ls /tmp | pr -T5 -W$COLUMNS 将终端划分成5栏显示 
chmod ugo+rwx directory1 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r ）、写(w)和执行(x)的权限 
chmod go-rwx directory1 删除群组(g)与其他人(o)对目录的读写执行权限 
chown user1 file1 改变一个文件的所有人属性 
chown -R user1 directory1 改变一个目录的所有人属性并同时改变改目录下所有文件的属性 
chgrp group1 file1 改变文件的群组 
chown user1:group1 file1 改变一个文件的所有人和群组属性 
find / -perm -u+s 罗列一个系统中所有使用了SUID控制的文件 
chmod u+s /bin/file1 设置一个二进制文件的 SUID 位 - 运行该文件的用户也被赋予和所有者同样的权限 
chmod u-s /bin/file1 禁用一个二进制文件的 SUID位 
chmod g+s /home/public 设置一个目录的SGID 位 - 类似SUID ，不过这是针对目录的 
chmod g-s /home/public 禁用一个目录的 SGID 位 
chmod o+t /home/public 设置一个文件的 STIKY 位 - 只允许合法所有人删除文件 
chmod o-t /home/public 禁用一个目录的 STIKY 位 
```
<br />

**文件的特殊属性**
(使用 "+" 设置权限，使用 "-" 用于取消)
```
chattr +a file1 只允许以追加方式读写文件 
chattr +c file1 允许这个文件能被内核自动压缩/解压 
chattr +d file1 在进行文件系统备份时，dump程序将忽略这个文件 
chattr +i file1 设置成不可变的文件，不能被删除、修改、重命名或者链接 
chattr +s file1 允许一个文件被安全地删除 
chattr +S file1 一旦应用程序对这个文件执行了写操作，使系统立刻把修改的结果写到磁盘 
chattr +u file1 若文件被删除，系统会允许你在以后恢复这个被删除的文件 
lsattr 显示特殊的属性 
```
<br />

**打包和压缩文件** 

```
bunzip2 file1.bz2 解压一个叫做 'file1.bz2'的文件 
bzip2 file1 压缩一个叫做 'file1' 的文件 
gunzip file1.gz 解压一个叫做 'file1.gz'的文件 
gzip file1 压缩一个叫做 'file1'的文件 
gzip -9 file1 最大程度压缩 
rar a file1.rar test_file 创建一个叫做 'file1.rar' 的包 
rar a file1.rar file1 file2 dir1 同时压缩 'file1', 'file2' 以及目录 'dir1' 
rar x file1.rar 解压rar包 
unrar x file1.rar 解压rar包 
tar -cvf archive.tar file1 创建一个非压缩的 tarball 
tar -cvf archive.tar file1 file2 dir1 创建一个包含了 'file1', 'file2' 以及 'dir1'的档案文件 
tar -tf archive.tar 显示一个包中的内容 
tar -xvf archive.tar 释放一个包 
tar -xvf archive.tar -C /tmp 将压缩包释放到 /tmp目录下 
tar -cvfj archive.tar.bz2 dir1 创建一个bzip2格式的压缩包 
tar -jxvf archive.tar.bz2 解压一个bzip2格式的压缩包 
tar -cvfz archive.tar.gz dir1 创建一个gzip格式的压缩包 
tar -zxvf archive.tar.gz 解压一个gzip格式的压缩包 
zip file1.zip file1 创建一个zip格式的压缩包 
zip -r file1.zip file1 file2 dir1 将几个文件和目录同时压缩成一个zip格式的压缩包 
unzip file1.zip 解压一个zip格式压缩包 
```
<br />

**DEB 包**
```
dpkg -i package.deb 安装/更新一个 deb 包 
dpkg -r package_name 从系统删除一个 deb 包 
dpkg -l 显示系统中所有已经安装的 deb 包 
dpkg -l | grep httpd 显示所有名称中包含 "httpd" 字样的deb包 
dpkg -s package_name 获得已经安装在系统中一个特殊包的信息 
dpkg -L package_name 显示系统中已经安装的一个deb包所提供的文件列表 
dpkg --contents package.deb 显示尚未安装的一个包所提供的文件列表 
dpkg -S /bin/ping 确认所给的文件由哪个deb包提供 
dpkg --force-all --purge packagename 有些软件很难卸载，而且还阻止了别的软件的应用，就能够用这个，但是有点冒险。 
```
<br />

**APT 软件工具**
```
apt-get install package_name 安装/更新一个 deb 包 
apt-cdrom install package_name 从光盘安装/更新一个 deb 包
apt-get update  更新源
apt-get -f install 修复安装 
apt-get install package --reinstall 重新安装包
apt-get update 升级列表中的软件包 
apt-get upgrade 升级所有已安装的软件 
apt-get dist-upgrade 将系统升级到新版本
apt-get source package  下载该包的源代码
apt-get build-dep package 安装相关的编译环境
apt-get remove package_name 从系统删除一个deb包 
apt-get check 确认依赖的软件仓库正确 
apt-get clean 从下载的软件包中清理缓存 
apt-cache search searched-package 返回包含所要搜索字符串的软件包名称 
apt-get autoremove --purage <xxx> 卸载软件
apt-get --purge remove packagename 卸载一个已安装的软件包（删除配置文档） 
sudo apt-get clean && sudo apt-get autoclean 清理无用的包
apt-cache depends package 了解使用该包依赖那些包
apt-cache rdepends package 查看该包被哪些包依赖
apt-cache search package 搜索软件包
apt-cache show package  获取包的相关信息，如说明、大小、版本等
apt update 更新软件包列表
apt upgrade从本地仓库中对比系统中所有已安装的软件，如果有新版本的话则进行升级
apt full-upgrade在升级软件包时自动处理依赖关系
apt list查看本地仓库中所有的软件包名和简要信息
apt list package查看package包名和简要信息（支持通配符）
apt show package 查看包的具体信息（依赖关系）
apt search package寻找package包
apt install package 安装package包，并同时安装其依赖的其它包
apt remove package卸载package包，但保留相关的配置文件（支持通配符）
apt purge remove package卸载package包，删除相关的配置文件（支持通配符）
apt autoremove卸载因安装软件自动安装的依赖，而现在又不需要的依赖包
apt clean删除所有已下载的软件包的备份
apt autoclean删除（无用、过期、已经卸载包的）备份
```
<br />

**PACMAN 软件工具**
```
pacman -S package_name 安装软件包
pacman -R package_name 删除软件包
pacman -Rs package_name 顺便删除软件包相关依赖（递归）
pacman -Rn package_name 在删除软件包时要同时删除相应的配置文件
pacman -Rsn package_name 删除一个软件包、它的配置文件以及所有不再需要的依赖(Pacman不会删除软件包安装后才创建的配置文件。可以从的home文件夹中手动删除)
pacman -Su 升级系统中所有已安装的包
pacman -Syu 升级系统中的所有包(将升级系统和同步仓库数据合成为一条指令)
pacman -Ss package 查询软件包
pacman -Qs package 查询已安装的包
pacman -Qi package 显示查找的包的信息(-Si也成)
pacman -Ql package 显示你要找的包的文件都安装的位置
pacman -Sw package 下载但不安装包
pacman -U /path/package.pkg.tar.gz 安装本地包
pacman -Qdl 罗列所有不再作为依赖的软件包(孤立orphans)
pacman -Q -help Pacman使用-Q参数来查询本地软件包数据库
pacman -S -help Pacman使用-S参数来查询远程软件包数据库
pacman -Sc 清理当前未被安装软件包的缓存(/var/cache/pacman/pkg)
pacman -Scc 清理包缓存，下载的包会在/var/cache 这个目录（仅在确定不需要做任何软件包降级工作时才这样做）
pacman -Sf pacman 重新安装包
pacman -Sw package_name 下载包而不安装它
pacman -U http://url/package_name-version.pkg.tar.gz 网络安装软件（不从源里）


```
<br />

**查看文件内容**
``` 
cat file1 从第一个字节开始正向查看文件的内容 
tac file1 从最后一行开始反向查看一个文件的内容 
more file1 查看一个长文件的内容 
less file1 类似于 'more' 命令，但是它允许在文件中和正向操作一样的反向操作 
head -2 file1 查看一个文件的前两行 
tail -2 file1 查看一个文件的最后两行 
tail -f /var/log/messages 实时查看被添加到一个文件中的内容 
```
<br />

**文本处理** 
```
cat file1 file2 ... | command <> file1_in.txt_or_file1_out.txt general syntax for text manipulation using PIPE, STDIN and STDOUT 
cat file1 | command( sed, grep, awk, grep, etc...) > result.txt 合并一个文件的详细说明文本，并将简介写入一个新文件中 
cat file1 | command( sed, grep, awk, grep, etc...) >> result.txt 合并一个文件的详细说明文本，并将简介写入一个已有的文件中 
grep Aug /var/log/messages 在文件 '/var/log/messages'中查找关键词"Aug" 
grep ^Aug /var/log/messages 在文件 '/var/log/messages'中查找以"Aug"开始的词汇 
grep [0-9] /var/log/messages 选择 '/var/log/messages' 文件中所有包含数字的行 
grep Aug -R /var/log/* 在目录 '/var/log' 及随后的目录中搜索字符串"Aug" 
sed 's/stringa1/stringa2/g' example.txt 将example.txt文件中的 "string1" 替换成 "string2" 
sed '/^$/d' example.txt 从example.txt文件中删除所有空白行 
sed '/ *#/d; /^$/d' example.txt 从example.txt文件中删除所有注释和空白行 
echo 'esempio' | tr '[:lower:]' '[:upper:]' 合并上下单元格内容 
sed -e '1d' result.txt 从文件example.txt 中排除第一行 
sed -n '/stringa1/p' 查看只包含词汇 "string1"的行 
sed -e 's/ *$//' example.txt 删除每一行最后的空白字符 
sed -e 's/stringa1//g' example.txt 从文档中只删除词汇 "string1" 并保留剩余全部 
sed -n '1,5p;5q' example.txt 查看从第一行到第5行内容 
sed -n '5p;5q' example.txt 查看第5行 
sed -e 's/00*/0/g' example.txt 用单个零替换多个零 
cat -n file1 标示文件的行数 
cat example.txt | awk 'NR%2==1' 删除example.txt文件中的所有偶数行 
echo a b c | awk '{print $1}' 查看一行第一栏 
echo a b c | awk '{print $1,$3}' 查看一行的第一和第三栏
echo -n 不输出换行，意思就是echo完不换行(echo默认换行)
echo -e 处理特殊字符
echo -E 不处理特殊字符，默认选项，和-e相反

#如果加上-e选项，遇到下面的特殊字符，会特殊处理

echo -e遇到 \\ 插入\(反斜线)字符，第一个\是转义字符
echo -e遇到 \a 发出警告声
echo -e遇到 \b 退格（删除前一个字符）
echo -e遇到 \c 最后不加上换行符号，一般写在字符串末尾(其实和-n功能差不多)
echo -e遇到 \e 逃逸（回车后不显示命令行)
echo -e遇到 \f 换页
echo -e遇到 \n 换行
echo -e遇到 \r 回车，和换行有区别
echo -e遇到 \t 横向制表符
echo -e遇到 \v 纵向制表符
paste file1 file2 合并两个文件或两栏的内容 
paste -d '+' file1 file2 合并两个文件或两栏的内容，中间用"+"区分 
sort file1 file2 排序两个文件的内容 
sort file1 file2 | uniq 取出两个文件的并集(重复的行只保留一份) 
sort file1 file2 | uniq -u 删除交集，留下其他的行 
sort file1 file2 | uniq -d 取出两个文件的交集(只留下同时存在于两个文件中的文件) 
comm -1 file1 file2 比较两个文件的内容只删除 'file1' 所包含的内容 
comm -2 file1 file2 比较两个文件的内容只删除 'file2' 所包含的内容 
comm -3 file1 file2 比较两个文件的内容只删除两个文件共有的部分 
```
<br />

**字符设置和文件格式转换** 
```
dos2unix filedos.txt fileunix.txt 将一个文本文件的格式从MSDOS转换成UNIX 
unix2dos fileunix.txt filedos.txt 将一个文本文件的格式从UNIX转换成MSDOS 
recode ..HTML < page.txt > page.html 将一个文本文件转换成html 
recode -l | more 显示所有允许的转换格式 
```
<br />

**文件系统分析**
```
badblocks -v /dev/hda1 检查磁盘hda1上的坏磁块 
fsck /dev/hda1 修复/检查hda1磁盘上linux文件系统的完整性 
fsck.ext2 /dev/hda1 修复/检查hda1磁盘上ext2文件系统的完整性 
e2fsck /dev/hda1 修复/检查hda1磁盘上ext2文件系统的完整性 
e2fsck -j /dev/hda1 修复/检查hda1磁盘上ext3文件系统的完整性 
fsck.ext3 /dev/hda1 修复/检查hda1磁盘上ext3文件系统的完整性 
fsck.vfat /dev/hda1 修复/检查hda1磁盘上fat文件系统的完整性 
fsck.msdos /dev/hda1 修复/检查hda1磁盘上dos文件系统的完整性 
dosfsck /dev/hda1 修复/检查hda1磁盘上dos文件系统的完整性 
```

**初始化一个文件系统**
``` 
mkfs /dev/hda1 在hda1分区创建一个文件系统 
mke2fs /dev/hda1 在hda1分区创建一个linux ext2的文件系统 
mke2fs -j /dev/hda1 在hda1分区创建一个linux ext3(日志型)的文件系统 
mkfs -t vfat 32 -F /dev/hda1 创建一个 FAT32 文件系统 
fdformat -n /dev/fd0 格式化一个软盘 
mkswap /dev/hda3 创建一个swap文件系统 
```
<br />

**SWAP文件系统**
```
mkswap /dev/hda3 创建一个swap文件系统 
swapon /dev/hda3 启用一个新的swap文件系统 
swapon /dev/hda2 /dev/hdb3 启用两个swap分区 
```
<br />

**备份** 
```
dump -0aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的完整备份 
dump -1aj -f /tmp/home0.bak /home 制作一个 '/home' 目录的交互式备份 
restore -if /tmp/home0.bak 还原一个交互式备份 
rsync -rogpav --delete /home /tmp 同步两边的目录 
rsync -rogpav -e ssh --delete /home ip_address:/tmp 通过SSH通道rsync 
rsync -az -e ssh --delete ip_addr:/home/public /home/local 通过ssh和压缩将一个远程目录同步到本地目录 
rsync -az -e ssh --delete /home/local ip_addr:/home/public 通过ssh和压缩将本地目录同步到远程目录 
dd bs=1M if=/dev/hda | gzip | ssh user@ip_addr 'dd of=hda.gz' 通过ssh在远程主机上执行一次备份本地磁盘的操作 
dd if=/dev/sda of=/tmp/file1 备份磁盘内容到一个文件 
tar -Puf backup.tar /home/user 执行一次对 '/home/user' 目录的交互式备份操作 
( cd /tmp/local/ && tar c . ) | ssh -C user@ip_addr 'cd /home/share/ && tar x -p' 通过ssh在远程目录中复制一个目录内容 
( tar c /home ) | ssh -C user@ip_addr 'cd /home/backup-home && tar x -p' 通过ssh在远程目录中复制一个本地目录 
tar cf - . | (cd /tmp/backup ; tar xf - ) 本地将一个目录复制到另一个地方，保留原有权限及链接 
find /home/user1 -name '*.txt' | xargs cp -av --target-directory=/home/backup/ --parents 从一个目录查找并复制所有以 '.txt' 结尾的文件到另一个目录 
find /var/log -name '*.log' | tar cv --files-from=- | bzip2 > log.tar.bz2 查找所有以 '.log' 结尾的文件并做成一个bzip包 
dd if=/dev/hda of=/dev/fd0 bs=512 count=1 做一个将 MBR (Master Boot Record)内容复制到软盘的动作 
dd if=/dev/fd0 of=/dev/hda bs=512 count=1 从已经保存到软盘的备份中恢复MBR内容 
```
<br />

**光盘**
```
cdrecord -v gracetime=2 dev=/dev/cdrom -eject blank=fast -force 清空一个可复写的光盘内容 
mkisofs /dev/cdrom > cd.iso 在磁盘上创建一个光盘的iso镜像文件 
mkisofs /dev/cdrom | gzip > cd_iso.gz 在磁盘上创建一个压缩了的光盘iso镜像文件 
mkisofs -J -allow-leading-dots -R -V "Label CD" -iso-level 4 -o ./cd.iso data_cd 创建一个目录的iso镜像文件 
cdrecord -v dev=/dev/cdrom cd.iso 刻录一个ISO镜像文件 
gzip -dc cd_iso.gz | cdrecord dev=/dev/cdrom - 刻录一个压缩了的ISO镜像文件 
mount -o loop cd.iso /mnt/iso 挂载一个ISO镜像文件 
cd-paranoia -B 从一个CD光盘转录音轨到 wav 文件中 
cd-paranoia -- "-3" 从一个CD光盘转录音轨到 wav 文件中（参数-3） 
cdrecord --scanbus 扫描总线以识别scsi通道 
dd if=/dev/hdc | md5sum 校验一个设备的md5sum编码，例如一张 CD 
```
<br />

**网络 - （以太网和WIFI无线）** 
```
ifconfig eth0 显示一个以太网卡的配置 
ifup eth0 启用一个 'eth0' 网络设备 
ifdown eth0 禁用一个 'eth0' 网络设备 
ifconfig eth0 192.168.1.1 netmask 255.255.255.0 控制IP地址 
ifconfig eth0 promisc 设置 'eth0' 成混杂模式以嗅探数据包 (sniffing) 
dhclient eth0 以dhcp模式启用 'eth0' 
route -n 不解析域名
route add -net 0/0 gw IP_Gateway 配置默认网关
route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1 配置静态线路固定访问该ip
route del 0/0 gw IP_gateway 移除静态网路
echo "1" > /proc/sys/net/ipv4/ip_forward activate ip routing 
hostname 显示设备的hostname
host www.example.com 查找hostname以找到对应ip并访问
ip link show 显示所有接口的链接状态 
mii-tool eth0 显示 "eth0" 的链接状态
ethtool eth0 显示网卡"eth0"的统计信息 
netstat -tup 显示所有活动网络连接及其 PID
netstat -tupl 显示所有网络服务侦听系统及其 PID
tcpdump tcp port 80 显示所有 HTTP 流量
iwlist scan 显示无线网络
iwconfig eth1 显示无线网卡的配置主机名显示主机名
nslookup www.example.com 查找主机名以将名称解析为ip地址和副信息
whois www.example.com 在whois的数据库中查找该网站 
```
<br />

参考资料：[《Linux常用命令大全（非常全！！！）》](https://blog.csdn.net/qq_36197776/article/details/102854810)[《Ubuntu下apt-get命令详解》](https://blog.csdn.net/bjzhaoxiao/article/details/81224550?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160681440019725222446134%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160681440019725222446134&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-3-81224550.first_rank_v2_rank_v28p2&utm_term=apt-get%E5%91%BD%E4%BB%A4&spm=1018.2118.3001.4449)[《apt-get命令》](https://blog.csdn.net/hdutigerkin/article/details/6604580?biz_id=102&utm_term=apt-get%E5%91%BD%E4%BB%A4&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-6604580&spm=1018.2118.3001.4449)[《linux基础 apt命令》](https://blog.csdn.net/K_Actor/article/details/82903915?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160681433819724813250972%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160681433819724813250972&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-82903915.first_rank_v2_rank_v28p2&utm_term=apt%E5%91%BD%E4%BB%A4&spm=1018.2118.3001.4449)[《archlinux pacman命令详解》](https://blog.csdn.net/iteye_13152/article/details/82237121)[《pacman常用命令》](https://blog.csdn.net/qq_38157516/article/details/89031269?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522160681491219726885843328%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=160681491219726885843328&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-89031269.first_rank_v2_rank_v28p2&utm_term=pacman%20%E5%91%BD%E4%BB%A4&spm=1018.2118.3001.4449)[《Linux下echo命令详解》](https://blog.csdn.net/wangzengdi/article/details/30557003?biz_id=102&utm_term=linux%20echo%E5%91%BD%E4%BB%A4&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-30557003&spm=1018.2118.3001.4449)


### 练习题目

**按照2016-01-13 18:06:07的格式输出时间**

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-005.png)

**删除/tmp目录下面所有A开头的文件**

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-006.jpg)

**查看一个文件中的头三行**

![](https://raw.githubusercontent.com/NeiitDoor/Notes4LinuxLearning/main/Images/part2-imgs/part2-007.jpg)

**重启Linux**

```
reboot
```















