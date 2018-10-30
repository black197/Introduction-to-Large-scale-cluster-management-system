# Introduction to some hardware
Technical reports about [Memory](#memory), [Storage](#storage)(Ceph), [Network](#network) and [xPU](#xpu).

# xPU
By 吴幸融 516030910253

## CPU
CPU( Central Processing Unit, 中央处理器)就是机器的“大脑”，也是布局谋略、发号施令、控制行动的“总司令官”。

### Features

#### Architecture

CPU的结构主要包括运算器（ALU, Arithmetic and Logic Unit）、控制单元（CU, Control Unit）、寄存器（Register）、高速缓存器（Cache）和它们之间通讯的数据、控制及状态的总线。

![avatar](http://5b0988e595225.cdn.sohucs.com/images/20171028/e664ed39638648469655f0fefe19f731.png)

**计算单元**主要执行算术运算、移位等操作以及地址运算和转换；**存储单元**主要用于保存运算中产生的数据以及指令等；**控制单元**则对指令译码，并且发出为完成每条指令所要执行的各个操作的控制信号。

CPU遵循的是**冯诺依曼架构**，其核心就是：**存储程序，顺序执行**。

### Pros & Cons

因为CPU的架构中需要大量的空间去放置存储单元（橙色部分）和控制单元（黄色部分），相比之下计算单元（绿色部分）只占据了很小的一部分，所以它在**大规模并行计算能力**上极受限制，而更擅长于**逻辑控制**。

### Key Indicators

**主频**，也就是CPU的时钟频率，简单地说也就是CPU的工作频率。

**总线速度**，即外频。

**工作电压**，也就是CPU正常工作所需的电压。

**协处理器**，主要的功能就是负责浮点运算，对多媒体指令进行优化。。

**流水技术**。

**超线程**，把一颗CPU模拟成两颗CPU使用，在同时间内更有效地利用资源来提高性能。

**制程技术**，制程越小发热量越小，这样就可以集成更多的晶体管，CPU效率也就更高。

## GPU

GPU全称为Graphics Processing Unit，中文为图形处理器，就如它的名字一样，GPU最初是用在个人电脑、工作站、游戏机和一些移动设备（如平板电脑、智能手机等）上运行绘图运算工作的微处理器。

### Features

#### Architecture

GPU的构成相对简单，有**数量众多的计算单元**和**超长的流水线**，特别适合处理大量的类型统一的数据。

![avatar](http://5b0988e595225.cdn.sohucs.com/images/20171028/e7ac26fc862a469983022f5078cf4bbb.png)

### Pros & Cons

GPU不仅可以在图像处理领域大显身手，它还被用来科学计算、密码破解、数值分析，海量数据处理（排序，Map-Reduce等），金融分析等需要**大规模并行计算**的领域。

GPU的工作大部分都计算量大，但没什么技术含量，而且要重复很多很多次。

但GPU**无法单独工作**，必须由CPU进行控制调用才能工作。

### Key Indicators

体现GPU**计算能力**的两个重要特征：

1)CUDA核的个数； 

2)存储器大小。 

描述GPU**性能**的两个重要指标： 

1)计算性能峰值； 

2)存储器带宽。

## TPU

TPU（Tensor Processing Unit, 张量处理器）就是谷歌专门为加速深层神经网络运算能力而研发的一款芯片，其实也是一款ASIC（专用集成电路）。

### Features

#### Architecture

TPU在芯片上使用了高达24MB的**局部内存**，6MB的累加器内存以及用于与主控处理器进行对接的内存，总共占芯片面积的37%（图中蓝色部分）。

![avatar](http://5b0988e595225.cdn.sohucs.com/images/20171028/cc49dbed99cf40238ba6f5b66dcdfeb4.jpeg)

谷歌充分意识到了片外内存访问是GPU能效比低的罪魁祸首，因此不惜成本的在芯片上放了巨大的内存。

TPU的高性能还来源于**对于低运算精度的容忍**。研究结果表明，低精度运算带来的算法准确率损失很小，但是在硬件实现上却可以带来巨大的便利，包括功耗更低、速度更快、占芯片面积更小的运算单元、更小的内存带宽需求等...TPU采用了8比特的低精度运算。

### Pros & Cons

TPU与同期的CPU和GPU相比，可以提供15-30倍的性能提升，以及30-80倍的效率（性能/瓦特）提升。初代的TPU只能做推理，要依靠Google云来实时收集数据并产生结果，而训练过程还需要额外的资源；而第二代TPU既可以用于**训练神经网络**，又可以用于**推理**。

TPU的**生产成本非常高**。

### Key Indicators

**性能**：

每核心时钟（IPC）

响应时间

**吞吐量**：

最大批量任务

**成本**：

制造

能效比

## Comment

从CPU到GPU再到TPU，xPU新生代从通用转向专一。万能工具的效率永远比不上专用工具。但万能工具和专用工具只有相互配合，各司其职，才能以最高的效率和最低的成本最好地完成任务。

“结构决定性质”。xPU家族中的每款硬件都具有不同的架构，这也从本质上决定了他们各自不同的特性，不同的长处与短处。所以每当研制新的xPU时，也总是从调整架构上入手。

“时代造就英雄，需求推动生产”。新的xPU的产生源自于新的领域，新的运算需求。谷歌提供的很多服务，包括谷歌图像搜索、谷歌照片、谷歌云视觉API、谷歌翻译等产品和服务都需要用到深度神经网络，因此谷歌投入了大量的人力物力研发了擅长训练神经网络的TPU。

## xPUs

APU -- Accelerated Processing Unit, 加速处理器，AMD公司推出加速图像处理芯片产品。

BPU -- Brain Processing Unit, 地平线公司主导的嵌入式处理器架构。

CPU -- Central Processing Unit 中央处理器， 目前PC core的主流产品。

DPU -- Deep learning Processing Unit, 深度学习处理器，最早由国内深鉴科技提出；另说有Dataflow Processing Unit 数据流处理器， Wave Computing 公司提出的AI架构；Data storage Processing Unit，深圳大普微的智能固态硬盘处理器。

FPU -- Floating Processing Unit 浮点计算单元，通用处理器中的浮点运算模块。

GPU -- Graphics Processing Unit, 图形处理器，采用多线程SIMD架构，为图形处理而生。

HPU -- Holographics Processing Unit 全息图像处理器， 微软出品的全息计算芯片与设备。

IPU -- Intelligence Processing Unit， Deep Mind投资的Graphcore公司出品的AI处理器产品。

MPU/MCU -- Microprocessor/Micro controller Unit， 微处理器/微控制器，一般用于低计算应用的RISC计算机体系架构产品，如ARM-M系列处理器。

NPU -- Neural Network Processing Unit，神经网络处理器，是基于神经网络算法与加速的新型处理器总称，如中科院计算所/寒武纪公司出品的diannao系列。

RPU -- Radio Processing Unit, 无线电处理器， Imagination Technologies 公司推出的集合集Wifi/蓝牙/FM/处理器为单片的处理器。

TPU -- Tensor Processing Unit 张量处理器， Google 公司推出的加速人工智能算法的专用处理器。目前一代TPU面向Inference，二代面向训练。

VPU -- Vector Processing Unit 矢量处理器，Intel收购的Movidius公司推出的图像处理与人工智能的专用芯片的加速计算核心。

WPU -- Wearable Processing Unit， 可穿戴处理器，Ineda Systems公司推出的可穿戴片上系统产品，包含GPU/MIPS CPU等IP。

XPU -- 百度与Xilinx公司在2017年Hotchips大会上发布的FPGA智能云加速，含256核。

ZPU -- Zylin Processing Unit, 由挪威Zylin 公司推出的一款32位开源处理器。

# Memory
By 吴怜颐 516030910252
## RAM(Random Access Memory，随机存取存储器)
- 既可以从中读取数据，也可以写入数据。
- 当存储器中的数据被读取或写入时，所需要的时间与这段信息所在的位置或所写入的位置无关。
- 当机器电源关闭时，存于其中的数据就会丢失。
- 主要用来存放操作系统、各种应用程序、数据等。
- 存取速度快。
- 主要种类：
    1. DRAM（Dynamic RAM，动态随机存取存储器）
        - 每几微秒就要刷新一次，否则数据会丢失。
        - 存取速度快。
        - 成本便宜。
        - 通常都用作计算机内的主存储器。
    2. SRAM（Static RAM，静态随机存取存储器）
        - 数据可以长驻其中而不需要随时进行存取。
        - 比一般的动态随机处理内存处理速度更快更稳定。
        - 往往用来做高速缓存。
    3. VRAM（Video RAM，视频内存）
        - 将显卡的视频数据输出到数模转换器中。
        - 降低绘图显示芯片的负担。
        - 价格昂贵。
        - 多用于高级显卡中的高档内存。
    4. FPM DRAM（Fast Page Mode DRAM，快速页切换模式动态随机存取存储器）
        - 在触发了行地址后，如果CPU需要的地址在同一行内，则可以连续输出列地址而不必输出行地址。
        - 大大提高读取速度。
    5. EDO DRAM（Extended Data Out DRAM，延伸数据输出动态随机存取存储器）
        - 不需要在存取每一BIT数据时输出行地址和列地址并使其稳定一段时间，然后才能读写有效的数据。
        - 存取速度比FPM DRAM快15%。
    6. BEDO DRAM（Burst Extended Data Out DRAM，爆发式延伸数据输出动态随机存取存储器）
        - 突发式的读取方式。
        - 一次可以存取多组数据。
        - 存取速度比EDO快。
        - 支持的主板非常少。
    7. MDRAM（Multi-Bank DRAM，多插槽动态随机存取存储器）
        - 其内部分成数个类别不同的小储存库 (BANK)，也即由数个属立的小单位矩阵所构成，每个储存库之间以高于外部的资料速度相互连接。
        - 一般应用于高速显示卡或加速卡中，也有少数主机板用于L2高速缓存中。
    8. WRAM（Window RAM，窗口随机存取存储器）
        - VRAM内存的改良版。
        - 有一、二十组的输入/输出控制器
        - 采用EDO的资料存取模式。
        - 存取速度快。
        - 提供了区块搬移功能（BitBlt）。
        - 可应用于专业绘图工作。
    9. RDRAM（Rambus DRAM，高频动态随机存取存储器）
        - 速度一般可以达到500~530MB/s。
        - 使用该内存后内存控制器需要作相当大的改变。
        - 一般应用于专业的图形加速适配卡或者电视游戏机的视频内存中。
    10. SDRAM（Synchronous DRAM，同步动态随机存取存储器）
        - 内存能够与CPU同步存取资料。
        - 取消等待周期。
        - 减少数据传输的延迟。
    11. SGRAM（Synchronous Graphics RAM，同步绘图随机存取存储器）
        - SDRAM的改良版。
        - 以区块Block为基本存取单位，个别地取回或修改存取的数据。
        - 减少内存整体读写的次数。
        - 增加了绘图控制器。
        - 提供区块搬移功能（BitBlt）。
    12. SB SRAM（Synchronous Burst SRAM，同步爆发式静态随机存取存储器）
        - 工作时脉与系统同步。
        - 更好地适应CPU速度。
    13. PB SRAM（Pipeline Burst SRAM，管线爆发式静态随机存取存储器）
        - 有效地延长存取时脉。
        - 访问速度快。
    14. DDR SDRAM（Double Data Rate二倍速率同步动态随机存取存储器）
        - 速度比SDRAM有一倍的提高。
        - 采用了DLL（Delay Locked Loop，延时锁定回路）提供一个数据滤波信号。
        - 是目前内存市场上的主流模式。
    15. SLDRAM （Synchronize Link，同步链环动态随机存取存储器）
        - 扩展型SDRAM结构内存。
        - 增加了更先进同步电路。
        - 改进了逻辑控制电路。
        - 难以投入实用。
    16. CDRAM（CACHED DRAM，同步缓存动态随机存取存储器）
        - 在DRAM芯片的外部插针和内部DRAM之间插入一个SRAM作为二级CACHE使用。
        - 极大地提高CPU效率。
    17. DDRII（Double Data Rate Synchronous DRAM，第二代同步双倍速率动态随机存取存储器）
        - 采用了在时钟的上升/下降沿同时进行数据传输的基本方式。
        - 拥有两倍于上一代DDR内存预读取能力。
    18. DRDRAM （Direct Rambus DRAM）
        - 将所有的接脚都连结到一个共同的Bus。
        - 控制器的体积小。
        - 数据传送的效率高。
    19. DDRIII（Double Data Rate Synchronous DRAM，第三代同步双倍速率动态随机存取存储器）
        - 功耗和发热量较小。
        - 工作频率更高。
        - 成本低。
        - 兼容性好。
## ROM(READ Only Memory，只读存储器)
- 一旦写入只能读出，不能修改。
- 当机器电源关闭时，存于其中数据不会丢失。
- 常用于存储各种固定程序和数据。
- 存取速度慢。
- 主要种类：
    1. MASK ROM（掩模型只读存储器）
        - 有原始数据的ROM或EPROM样本。
        - 数据永远无法做修改。
        - 成本比较低。
    2. PROM（Programmable ROM，可编程只读存储器）
        - 可以用刻录机将数据写入，但只能写入一次。
    3. EPROM（Erasable Programmable，可擦可编程只读存储器）
        - 具有可擦除功能，擦除后即可进行再编程。
        - 遭到阳光直射后数据即被清除。
    4. EEPROM（Electrically Erasable Programmable，电可擦可编程只读存储器）
        - 以约20V的电压来清除数据。
        - 可以用电信号进行数据写入。
        - 多应用于即插即用（PnP）接口。
    5. Flash Memory（快闪存储器）
        - 可以直接在主机板上修改内容而不需要将IC拔下。
        - 在写入新的数据时必须先将旧数据清除。
        - 写入速度慢。
## Key Indicators
1. 内存主频
    - 内存所能达到的最高工作频率。
    - 决定着该内存最高能在什么样的频率正常工作。
    - 内存无法决定自身的工作频率，其实际工作频率是由主板来决定的。
2. 内存容量
    - 内存可以存储的字节数。
    - 在其他配置相同的条件下内存越大机器性能也就越高。
3. 内存时序
    - 内存完成一项工作所需要的时间周期。
    - 时间越长，则表示执行效率越低。
## Comment
由于目前系统处理的数据量都是相当巨大的，而几乎每一步CPU操作都得经过内存，这也导致内存可以说是整个系统中工作最为频繁的部件。如此一来，内存的性能就在一定程度上决定了这个系统的表现。

随着CPU速度的不断提升，内存频率的提升也成为了提升计算机性能必不可少的一个因素。而内存的工作频率主要依靠于主板的工作频率，如果主板最高只支持3000MHz频率，而安装一块4000MHz频率的内存显然就是没有意义的。并且内存主频受到制作工艺的限制，不可能在短时间内成倍提高。

内存容量作为影响内存带宽的因素之一，确实在相同情况下，内存容量越大，计算机的性能越高，但是目前的内存容量已经基本足够大，能够满足人们的使用，一味的追求内存容量的增加，对于性能的提升或许并不明显。

而一般来讲时序越低内存条性能越强，但如果过于降低时序又会增加硬件不稳定的风险。

而在提高内存本身的性能的之外，通过提高总线宽度和数据包个数，来提高内存带宽的方式也是现在的发展趋势。

# Storage
By 张宇航 516030910273
## Vendors or types or technologies
### Features
* Ceph delivers unified storage, supporting File, Block and Object. 
* By shedding design assumptions like allocation lists found in nearly all existing systems, we maximally separate data from metadata management, allowing them to scale independently. 
* Ceph does striping of individual files across multiple nodes to achieve higher throughput, similar to how RAID0 stripes partitions across multiple hard drives. Adaptive load balancing is supported whereby frequently accessed objects are replicated over more nodes.
### Pros
* The obvious point of File, Block and Object in the same wrapper. It might be an obvious point, but it’s a pretty damn important one.
* Better transfer speed and lower latency, because traffic to and from the Swift cluster goes through proxy servers which slow it down.
* Swift can have further latency problems as replicas are not necessarily updated at the same time, so requesters retrieving data can access old wrong/outdated versions.
### Cons
* Ceph’s multi-region support – usually touted as an advantage, is in a master slave configuration, but as replication is only possible from master to slave, in a deployment with 2+ regions you can get uneven load distribution. 
* There can also be a security issue as RADOS clients on the Cloud compute node communicate directly with the RADOS servers over the same network Ceph uses for unencrypted replication traffic. So, potentially, if Ceph client node is compromised, the attacker can see all traffic on the storage network.
## Key indicators
### What, how to measure, scope
* OSD Throughput,Write Latency,Data Distribution and Scalability
* Metadata Update Latency,Metadata Read Latency, Metadata Scaling
## Your own comment
To be honest, Ceph isn’t perfect for everyone. It’s not the most efficient at using flash or CPU (but it’s getting better), the file storage feature isn’t fully mature yet, and it is missing key efficiency features like deduplication and compression. And some customers just aren’t comfortable with open-source or software-defined storage of any kind. But every release of Ceph adds new features and improved performance, while system integrators build turnkey Ceph appliances that make it easy to deploy and come with integrated hardware and software support.
