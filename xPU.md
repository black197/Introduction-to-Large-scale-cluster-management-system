# Introduction-to-some-hardware
*Technical reports about Storage, Network, xPU & Memory*

吴幸融 516030910253 xPU

叶东諴 516030910020 Network

吴怜颐 516030910252 Memory

张宇航 516030910273 Stroage


# xPU

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