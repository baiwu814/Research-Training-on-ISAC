# 通感一体化(ISAC)

ISAC：

雷达通信一体化（DFRC）

通信感知一体化（ISAC）



## 来源与方向

雷达与通信的[频谱共享](https://zhida.zhihu.com/search?content_id=207482042&content_type=Article&match_order=1&q=频谱共享&zhida_source=entity)（Radar and communication spectrum sharing, RCSS）

衍生出

### 雷达通信一体化（Dual-functional radar and communication, DFRC）

#### 1.1、主要任务：

寄希望通过硬件平台、资源分配、信号处理等一体化的设计，同时实现通信和雷达的双功能

#### 1.2、过渡：

雷达通信一体化，现在叫通信感知一体化（Integrated sensing and communication, ISAC）逐渐成为了众多热点研究之一

过渡中的方向变化：主要是雷达朝着感知（sensing）的过渡，将传统的信道估计或者WiFi检测纳入sensing的范畴

#### 1.3、演化的ISAC的动机：

雷达和通信频谱共享

雷达与通信在硬件架构、信号处理等方面均有共通之处，这就为通信网络中融入感知提供了可能，这种一体化的设计能够获得“[集成增益](https://zhida.zhihu.com/search?content_id=207482042&content_type=Article&match_order=1&q=集成增益&zhida_source=entity)”；

通信与感知系统能够互惠互利获得一体化系统的“[协作增益](https://zhida.zhihu.com/search?content_id=207482042&content_type=Article&match_order=1&q=协作增益&zhida_source=entity)”

多种应用

![感知赋能](https://pic3.zhimg.com/v2-b51cf2e9c5f3fba1e5491644ba761bf8_1440w.jpg)

### 通信频谱共存（Radar-communication coexistence, RCC）：物理层通信中的[认知无线电](https://zhida.zhihu.com/search?content_id=207482042&content_type=Article&match_order=1&q=认知无线电&zhida_source=entity)（Cognitive radio，CR）

#### 2.1、主要任务

围绕雷达和通信系统的干扰管理，即在满足通信SINR约束的前提下，来优化雷达探测的性能





## ISAC的性能

要求系统在同一套硬件和频谱资源上，既能实现**高速通信**，又能实现**高精度感知**。

通信和感知两个大方面的性能指标（Performance criteria）

### ISAC的背景

 更 高 的 **频 段**（ 毫 米 波 乃 至 太 赫兹）、更宽的**带宽**、更**大规模天线阵列**使高精度、高分辨 感知成为可能，从而可以在一个系统中实现通信感知一体化

### 通信与感知

#### 感知（“网络即传感器”（Network as  a Sensor））

##### 原理

通信网络可以作为一 个巨大的传感器，网元发送和接收无线信号，利用无线电波的 传输、反射和散射，可以更好地感知和理解物理世界

##### 应用

提供高精度定位、 手势捕捉、动作识别、无源对象的检测和追踪、成像及环境重 构

#### 通信（感知辅助通信 (Sensing-assisted Communication)）

##### 原理

感知所提供的高精度定位、成像和 环境重构能力可以帮助提升通信性能，例如波束赋形更准确、 波束失败恢复更迅速、终端信道状态信息（Channel State  Information，CSI）追踪的开销更低

##### 应用

超高精度定位追踪、同步成像、地图构建、 人类感官增强等（详情参见表 1）



### 通信指标

#### 一般指标

* **通信速率 (Data Rate)：** 单位时间内传输的数据量（如峰值速率、用户体验速率）。

* **传输时延 (Latency)：** 数据从发送端到接收端的时间，尤其是在车联网（V2X）等 ISAC 应用场景下，对超低时延有极高要求。

* 误码率 (Bit Error Rate, BER)

​	**误码率** 是指在传输过程中，出错的比特数占总传输比特数的比例。

* CSI：通信系统信道估计里的信道状态信息（Channel statement information）

* SNR、SINR

  **SNR** (Signal-to-Noise Ratio) —— 信噪比

  SINR (Signal-to-Interference-plus-Noise Ratio) —— 信干噪比

  **SINR** 比 SNR 多考虑了一个变量——**干扰（Interference）**。

  * **公式逻辑**：

  $$
  SINR = \frac{信号}{干扰 + 噪声}
  $$

  

补充：关于CSI见下面链接





#### 频谱与资源效率

- **频谱效率 (Spectral Efficiency, SE)：** 单位频带宽度内的传输速率（bps/Hz）。ISAC 的核心优势就在于通过信号共享提升总体的频谱效率。
- **能量效率 (Energy Efficiency, EE)：** 每消耗单位能量所能传输的比特数。

### 感知中雷达探测的性能指标：

检测（Detection）和估计（Estimation）

#### 检测

检测：正确检测概率、错误检测概率和虚警（False alarm）概率

#### 估计

参数估计：性能通常可以用均方误差（Mean Squared Error, MSE）和克拉美-罗下界（Cramér–Rao Lower Bound，CRLB）进行刻画。

一些经典的检测器[[1\]](https://zhuanlan.zhihu.com/p/537067898#ref_1)、CRLB[[2\]](https://zhuanlan.zhihu.com/p/537067898#ref_2)的推导可以参考这2篇paper：

> [1] Fishler E, Haimovich A, Blum R S, et al. Spatial diversity in radars—Models and detection performance[J]. IEEE Transactions on signal processing, 2006, 54(3): 823-838. 
>
> [2] Bekkerman I, Tabrikian J. Target detection and localization using MIMO radars and sonars[J]. IEEE Transactions on Signal Processing, 2006, 54(10): 3873-3883.

有关底层估计理论

> [3] S. M. Kay, “Fundamentals of statistical signal processing,” in Estimation Theory. Englewood Cliffs, NJ, USA: Prentice Hall, 1998.

**有关雷达的部分**可以见以下文件链接

> [雷达学术入门--脉冲雷达信号处理概述](https://mp.weixin.qq.com/s/vaofUxurFaxSBInlvc_6Rw)

这里进对其做一个总结

#### 脉冲雷达信号处理概述

##### 1. 脉冲雷达的基本工作原理

文章首先介绍了脉冲雷达的核心工作循环：

- **发射与接收：** 雷达周期性地发射探测脉冲，信号碰到目标后反射回雷达，回波中携带了目标的距离、速度和角度等参数。
- **关键周期参数：** * **脉冲重复周期 (PRI)：** 指相邻两次发射-接收循环的时间间隔。
  - **脉冲重复频率 (PRF)：** 指一秒钟内发射脉冲的个数。
- **数学建模：** 文章给出了针对远场点目标的雷达回波信号模型，回波信号由**双程路径损耗、目标雷达散射截面积 (RCS)、多普勒频率、天线阵列方向矢量、双程时延以及加性高斯白噪声**共同决定。
- **干扰与杂波：** 提到雷达除了接收感兴趣的目标信号外，还会收到来自环境的干扰，即“杂波 (Clutter)”，通常需要进行抑制处理。

##### 2. 目标检测问题 (Detection)

检测是判断“目标是否存在”的过程，文章将其建模为**二元假设检验**：

- **假设模型：**
  - $H_0$（零假设）：仅收到噪声。
  - $H_1$：同时收到目标回波和噪声。
- **检测器设计：** 需要设定一个门限值，将回波信号映射后的数值与该门限对比来做出判决。常用的检测器包括：似然比检验 (LRT)、广义似然比检验 (GLRT) 等。
- **评价指标：** * **检测概率 ($P_D$)：** 目标存在且判断正确的概率。
  - **虚警概率 ($P_{FA}$)：** 目标不存在但判断存在的概率。文章特别强调，**虚警对雷达资源的浪费伤害巨大**。
- **准则：** 常用**奈曼-皮尔逊准则 (Neyman-Pearson Criterion)**，即在限定虚警概率的条件下，尽可能提高检测概率。

##### 3. 参数估计问题 (Estimation)

在确认目标存在后，需要通过估计器提取目标的具体参数：

- **距离估计：** 采用**脉冲压缩**技术，利用匹配滤波器估计信号的双程时延，这属于“快时间 (Fast-Time)”信号处理。
- **速度估计：** 利用多个脉冲之间的多普勒相移，通过**快速傅里叶变换 (FFT)** 提取多普勒频率，这属于“慢时间 (Slow-Time)”信号处理。
- **角度估计：** 采用经典的子空间算法，如 **MUSIC** 或 **ESPRIT**。
- **性能评估：** 常用**均方误差 (MSE)** 来衡量。由于 MSE 闭式解难求，学术上常使用**克拉美-罗下界 (CRLB)** 作为性能基准，它是所有无偏估计器能达到的方差下界。

##### 4. 总结与背景

文章总结了雷达信号处理的两个核心：**检测**与**估计**。同时提到，随着技术发展，雷达与通信领域（如 MIMO、OFDM）正不断融合影响，这正是作者所深耕的**感知通信一体化 (ISAC)** 领域的基础。

该文章是刘凡博士在 IEEE Communications Letters 发表的系列约稿中的第一篇，作为该领域的科普和学术入门具有很高的参考价值。



### ISAC 特有的通信权衡指标

在通感一体化中，通信和感知往往存在资源竞争。为了衡量两者的平衡，引入了一些特殊指标：

- **通信中断概率 (Outage Probability)：** 当感知功率增强时，通信信号质量下降导致连接中断的概率。
- **信道容量 (Channel Capacity)：** 在存在感知干扰或多径效应下的最大理论传输速率。

基本的性能指标、更多相关的基础内容、ISAC研究的基本理论和场景框架，可以参考Fan Liu老师等[[4\]](https://zhuanlan.zhihu.com/p/537067898#ref_4)、Yuanhao Cui博士等[[5\]](https://zhuanlan.zhihu.com/p/537067898#ref_5)、An Liu老师等[[6\]](https://zhuanlan.zhihu.com/p/537067898#ref_6)的ISAC综述。

> [4] Liu F, Cui Y, Masouros C, et al. Integrated sensing and communications: Towards dual-functional wireless networks for 6G and beyond[J]. IEEE Journal on Selected Areas in Communications, 2022. 
>
> [5] Cui Y, Liu F, Jing X, et al. Integrating Sensing and Communications for Ubiquitous IoT: Applications, Trends, and Challenges[J]. IEEE Network, 2021, 35(5): 158-167. 
>
> [6] Liu A, Huang Z, Li M, et al. A survey on fundamental limits of integrated sensing and communication[J]. IEEE Communications Surveys & Tutorials, 2022.

如果是对WiFi感知、大规模感知网络、信号处理技术等相关工作感兴趣的同学可以follow一下北京大学Daqing Zhang老师课题组、悉尼科技大学J. Andrew Zhang老师课题组、北京邮电大学Zhiyong Feng老师课题组等等的综述和相关工作，这里就不再一一列举了。

## ISAC的场景应用

####  四大应用分类 (Use Cases)

文章详细罗列了 ISAC 将催生的新服务，这些都是潜在的**课题选题方向**：

- **高精度定位与跟踪**：实现厘米级甚至毫米级的设备/非设备定位。
- **成像、建图与环境重构**：利用超大带宽（太赫兹）进行非视距（NLOS）成像，比如“透视”墙后物体。
- **增强型人类感知**：如无接触式的健康监测（心率、呼吸）、污染监测等。
- **手势与动作识别**：实现新型的人机交互方案。



#### 两个案例研究 (Case Studies)

- **案例 1：SAPE 方案**：展示了如何结合“基于设备的”和“无设备的”感知来极大提升定位精度。
- **案例 2：太赫兹成像**：展示了利用太赫兹波在移动设备上实现毫米级分辨率成像的原型机实验结果。

#### 技术挑战与难点 (Research Challenges) —— **这是你最需要的“避坑”与“创新点”指引**：

1. **波形设计 (Waveform Design)**：如何在高通信速率要求下，保证感知信号的低旁瓣特性？目前的 OFDM 是否足够？（文章提到仍需探索更有竞争力的方案）。
2. **硬件共担 (Hardware Co-design)**：感知对硬件非理想性（如相位噪声、采样抖动）极度敏感，如何在廉价的商用硬件上实现高性能感知？
3. **自干扰抑制 (Self-interference Cancellation)**：单站感知时，强大的发射信号会淹没微弱的回波信号。
4. **协作感知同步 (Synchronization)**：多基站协作感知时，时间同步精度需要达到纳秒甚至亚纳秒级，这是目前最大的工程难点之一。

## ISAC研究方向

集中在通感一体的信息论、波形设计、性能折中方案、通感互惠、通信与感知共存、通感算一体化以及通感一体有机结合新兴技术（如RIS、NOMA、UAV，预测会有WPT与SWIPT，其实也提了很多年了）等方面的研究

### 传统通信技术与ISAC

感知和通信的关系更趋同于是“共存”，二者甚至还需要竞争资源，例如功率、频谱、时序调度等。

在某个通信为中心得场景中，如何合理分配有限的资源，来实现通信和感知性能之间的折中（tradeoff）

#### 物理层安全

原理：考虑用一体化的波形，即感知和通信共用一个X发出去，这个X的回波能用来做雷达检测与估计，也可以用来和多用户通信

问题：是X合并探测目标或者波束方向有窃听者与X分开性能损失干扰的矛盾

要求：利用一体化的波形，在物理层做到安全通信

信息论基础[[7\]](https://zhuanlan.zhihu.com/p/537067898#ref_7)

> Poor H V, Schaefer R F. Wireless physical layer security[J]. Proceedings of the National Academy of Sciences, 2017, 114(1): 19-26.

相关论文：

具体工作Nanchi Su等[[9\]](https://zhuanlan.zhihu.com/p/537067898#ref_9)、Zhongxiang Wei等[[10\]](https://zhuanlan.zhihu.com/p/537067898#ref_10)的工作。

> 1. [^](https://zhuanlan.zhihu.com/p/537067898#ref_9_0)Su N, Liu F, Wei Z, et al. Secure dual-functional radar-communication transmission: Exploiting interference for resilience against target eavesdropping[J]. IEEE Transactions on Wireless Communications, 2022.
> 2. [^](https://zhuanlan.zhihu.com/p/537067898#ref_10_0)Wei Z, Liu F, Masouros C, et al. Toward Multi-Functional 6G Wireless Networks: Integrating Sensing, Communication, and Security[J]. IEEE Communications Magazine, 2022, 60(4): 65-71.

#### 多用户多播信道的容量域/多用户通信的公平性

[[8\]](https://zhuanlan.zhihu.com/p/537067898#ref_8)

> Ren Z, Song X, Fang Y, et al. Fundamental CRB-Rate Tradeoff in Multi-antenna Multicast Channel with ISAC[J]. arXiv preprint arXiv:2205.15615, 2022.



#### RC：物理层关心的就是次级链路用户对主链路用户通信的干扰需要低于某个阈值

##### 1. 角色定义：谁是“主”，谁是“次”？

- **主链路用户 (Primary User, PU)：** 拥有该频段的“合法执照”或绝对优先权。在 ISAC 场景下，这通常是**基站的通信用户**，或者是**专门的雷达站**。
- **次级链路用户 (Secondary User, SU)：** “借用”或“共用”该频段进行额外任务的用户。例如，在通信频段里进行**目标感知的雷达功能**，或者在雷达频段里工作的临时通信节点。

##### 2. 核心逻辑：不干扰是底线

物理层关心的这个“阈值”，本质上是为了确保**主链路的通信质量（QoS）不受毁灭性影响**。

- **干扰功率约束：** 次级链路在发射信号进行感知（或通信）时，其产生的电磁波会作为噪声叠加上主链路的接收机上。

- **阈值（Threshold）：** 通常设定为一个具体的功率值 $I_{th}$。数学上表达为：

  $$I_{SU \to PU} \le I_{th}$$

  或者要求主用户的**信噪比（SINR）**下降不得超过某个百分比。

##### 3. 这在 ISAC 中具体意味着什么？

在刘凡博士的研究（如你提到的系列文章）中，这通常涉及**联合波形设计**的优化问题：

- **牺牲感知保通信：** 算法在设计发射波形时，会给感知波束加一个“紧箍咒”。即便感知任务需要巨大的发射功率来探测远方小目标，但如果这股能量会干扰到主链路用户的正常通话/上网，算法必须**强行压低**该方向的副瓣电平。
- **空间投影（Null-steering）：** 在物理层，这常通过预编码（Precoding）技术实现。次级链路会将信号投影到主链路信道的**零空间（Null-space）**中。这样，感知信号在物理上就不会对主用户产生任何电动势，从而让干扰几乎为零。

#### 完善CSI/非完善CSI：非完善硬件引入的信号噪声等等各种鲁棒性设计

### 新兴的通信技术与ISAC

#### RIS/IRS assisted/enabled ISAC

这块研究其实蛮多的，最新一期的WCM也快出来了，相信会有更多优秀的RIS/IRS结合ISAC的工作。

北理工Xinyi Wang等[[11\]](https://zhuanlan.zhihu.com/p/537067898#ref_11)做的波形设计

> Wang X, Fei Z, Zheng Z, et al. Joint waveform design and passive beamforming for RIS-assisted dual-functional radar-communication system[J]. IEEE Transactions on Vehicular Technology, 2021, 70(5): 5131-5136.

大连理工Rang Liu等[[12\]](https://zhuanlan.zhihu.com/p/537067898#ref_12)[[13\]](https://zhuanlan.zhihu.com/p/537067898#ref_13)做的符号级预编码

> 1. [^](https://zhuanlan.zhihu.com/p/537067898#ref_12_0)Liu R, Li M, Liu Q, et al. Dual-functional radar-communication waveform design: A symbol-level precoding approach[J]. IEEE Journal of Selected Topics in Signal Processing, 2021, 15(6): 1316-1331.
> 2. [^](https://zhuanlan.zhihu.com/p/537067898#ref_13_0)Liu R, Li M, Liu Y, et al. Joint transmit waveform and passive beamforming design for RIS-aided DFRC systems[J]. IEEE Journal of Selected Topics in Signal Processing, 2022.

港中深Xianxin Song等[[14\]](https://zhuanlan.zhihu.com/p/537067898#ref_14)做的非视距下DOA估计的CRB。

> [^](https://zhuanlan.zhihu.com/p/537067898#ref_14_0)Song X, Xu J, Liu F, et al. Intelligent Reflecting Surface Enabled Sensing: Cram\'er-Rao Lower Bound Optimization[J]. arXiv preprint arXiv:2204.11071, 2022.

澳门大学Qingqing Wu老师团队

浙江大学Xiaoming Chen老师团队



#### NOMA （Semi-ISAC）

雷达感知这里性能指标上的选择比较偏向于Communication-friendly

比如SCNR/SINR、Beampattern matching error以及Sensing rate等

QMUL Yuanwei Liu老师的课题组[[15\]](https://zhuanlan.zhihu.com/p/537067898#ref_15)

> [^](https://zhuanlan.zhihu.com/p/537067898#ref_15_0)Mu X, Wang Z, Liu Y. NOMA for Integrating Sensing and Communications towards 6G: A Multiple Access Perspective[J]. arXiv preprint arXiv:2206.00377, 2022.

偏向于是Communication-centric，对于感知部分的促进可能有限

在资源分配和波形设计时，**通信指标**（如 SINR、传输速率）被设为“目标函数”，而**感知指标**（如距离/速度分辨率）往往只是“约束条件”或者是附带的功能。

**?所以这里感知部分能不能作为目标函数？为什么不考虑这个**

AI解释：**权衡（Trade-off）问题**。

#### 为什么这句话是对的？

说一个系统是 **Communication-centric（以通信为中心）**，意味着在资源分配和波形设计时，**通信指标**（如 SINR、传输速率）被设为“目标函数”，而**感知指标**（如距离/速度分辨率）往往只是“约束条件”或者是附带的功能。

原因如下：

- **波形不匹配**：通信信号（如正交频分复用 OFDM）设计初衷是为了传输数据，具有随机性。而理想的雷达感知信号通常需要特定的自相关特性（如线性频扫 Chirp 信号）。强行用通信波形做感知，会导致感知精度受限，产生旁瓣干扰。
- **资源倾斜**：如果大部分功率和时频资源都用于承载通信比特（Bit），那么用于探测目标的有效能量和频谱带宽就会被压缩。
- **处理机制差异**：通信关注的是**“消除多径”**以提高速率，而感知关注的是**“利用多径/反射”**来获取目标特征。以通信为核心的处理架构可能会过滤掉对感知至关重要的弱反射信号。

------

#### 这句话的“局限性”或“错误”在哪？

虽然这句话在逻辑上没错，但如果放在 **6G 最新研究进展**中，它可能显得有些**片面**或**过时**，错误主要在于忽视了“协同效应”：

1. **高频段的天然优势**：在毫米波或太赫兹频段，通信为了实现超高速率必须使用极宽的带宽。根据雷达原理，带宽越大，距离分辨率越好。这意味着即使是“以通信为中心”的设计，在客观上也极大地提升了感知性能。
2. **导频信号的复用**：通信系统中为了估计信道，会发送**导频（Pilot）**或**前导（Preamble）**序列。这些信号是已知的、确定性的，非常类似于雷达脉冲。通过优化这些现有的通信开销，可以实现不牺牲通信速率的高性能感知。
3. **互信息（Mutual Information）的统一**：从信息论角度看，通信是传递信息，感知是提取环境信息。最新的研究表明，两者在数学本质上是可以统一优化的。设计得当的话，通信性能的提升可以与感知精度的增强成正比，而不再是简单的“此消彼长”。

#### UAV-enabled ISAC

#### 问：所以目前UAV情况如何？

UAV应该是16年左右大火，我18年读研一那会，能够感觉很多工作慢慢开始推不动了。一是UAV一出口太容易被攻击了，各种不实际、过于理想等帽子，搞得人有口难辨；二是很多运营商、大厂，似乎也不太感兴趣，UAV通信的出圈，还要看翼龙救灾，真正的民用通信、灵活部署，我想阻力还是很大的。

主要动机，其实和UAV通信差不多，利用UVA的灵活部署、广域覆盖、视距（Line of sight, LoS）通信

LoS通信，这一点实际上和雷达感知是一致的，在单站场景下，是不希望目标被遮挡的。（注：双站/多站场景中可能会有所区别，即便是NLoS路径，如果是经过目标的，对于感知也还是有利的）

工作：

论文：UAV-Enabled Integrated Sensing and Communication: Opportunities and Challenges

如前提及的安全通信，或者是热点区域卸载、空中感知（大多数的研究还是集中在UAV作为空中基站，执行通信/感知任务）澳门大学的Kaitao Meng博士总结了很多[[16\]](https://zhuanlan.zhihu.com/p/537067898#ref_16)、干扰管理、

> ^[a](https://zhuanlan.zhihu.com/p/537067898#ref_16_0)[b](https://zhuanlan.zhihu.com/p/537067898#ref_16_1)https://arxiv.org/pdf/2206.03408.pdf

跟踪（UAV通信中的作用，即基站作为雷达，来跟踪UAV，对UAV执行控制功能等等：Integrated Scheduling of Sensing, Communication, and Control for mmWave/THz Communications in Cellular Connected UAV Networks），

#### 蜂窝联接的毫米波/太赫兹无人机通信网络中的通信-感知-控制融合调度

##### 1. 核心背景与动机

- **需求矛盾：** 无人机网络向蜂窝化演进，需要毫米波/太赫兹提供 Tbps 级的回传速率，但高频段波束极窄，极易因无人机运动产生**波束对准误差**。
- **三者耦合：** * **感知**：基站像雷达一样扫描无人机的位置、速度。
  - **控制**：基站基于感知信息，向无人机发送运动控制指令以对准波束。
  - **通信**：良好的对准才能保障高数据速率传输。
- **调度难题：** 感知和控制任务会占用时间资源，导致通信速率下降；反之，若只顾着通信而减少感知频率，无人机位置更新不及时，波束偏离会导致速率骤降甚至中断。

------

##### 2. 论文核心逻辑与模型

文章构建了一个**“闭环”**的融合调度框架，其运作流程如下：

###### A. 建立数学关联

文章分析并得出了两个关键的单调关系：

1. **运动控制误差 vs 波束对准误差：** 波束对准误差随无人机运动状态（位置、方向等）更新误差的增大而**线性增长**。
2. **传输速率损失 vs 运动误差：** 运动误差越大，波束偏离越远，通信速率损失就越严重。

###### B. 引入创新指标：状态噪声比 (State-to-Noise Ratio)

这是本文的一个亮点。作者从运动控制角度提出了这一新指标，用于描述**控制状态更新方程中“状态项”与“物理环境噪声”之间的比值**。通过这个指标，可以将复杂的物理运动规律与通信传输速率建立起闭式的数学联系。

###### C. 融合调度策略

基站内嵌入了一个远程控制器，根据实时状况决定何时切换模式：

- **策略标准：** 以**基于波束对准的传输速率门限**作为调度标准。
- **激活逻辑：** 当速率由于波束偏移跌破门限时，激活感知-控制环节进行状态更新；当状态恢复（状态噪声比达标）且波束对准后，全力保障通信任务。

------

##### 3. 主要研究贡献

1. **新型框架：** 首次提出了这种将感知、通信和控制三者融合设计的调度方法，解决了高频无人机波束追踪对准的痛点。
2. **解析策略：** 给出了基于“状态噪声比”的解析调度策略，能够证明在保障通信需求的同时，维持良好的控制稳定性。
3. **性能验证：** 仿真结果证明（见图3、图4），该方法在**控制精度（误差小）**和**通信吞吐量（速率稳）**之间达到了优秀的平衡。

------

##### 4. 总结：文章在干嘛？

这篇文章实际上是在解决**“如何让高速移动的无人机在高频网络中不‘掉线’”**的问题。它通过一种精细的调度算法，让基站聪明地在“盯着无人机看（感知）”、“告诉它怎么飞（控制）”和“收发海量数据（通信）”之间进行时分复用和性能折中。







多UAV协作定位、感知辅助多UAV组网防止碰撞等，都是感知和UAV比较好的课题。







### ISAC信息论

ISAC信息论的工作其实和信道估计紧密相关

Dongning Guo等提出的著名的I-MMSE等式[[17\]](https://zhuanlan.zhihu.com/p/537067898#ref_17)

> [^](https://zhuanlan.zhihu.com/p/537067898#ref_17_0)Guo D, Shamai S, Verdú S. Mutual information and minimum mean-square error in Gaussian channels[J]. IEEE transactions on information theory, 2005, 51(4): 1261-1282.

MMSE指的是获得接收Y，对于输入X的推断的误差

**感知更希望获得带有目标信息/环境参数的H，而不是发射符号本身X，**从这个意义上说，感知和信道估计的关系其实很紧密的。这么去想的话，Kobayashi等关于capacity-distortion (C-D)的工作[[18\]](https://zhuanlan.zhihu.com/p/537067898#ref_18)[[19\]](https://zhuanlan.zhihu.com/p/537067898#ref_19)，核心思想就是在于允许对H估计的一定失真，来最大化容量

> 1. [^](https://zhuanlan.zhihu.com/p/537067898#ref_18_0)Kobayashi M, Hamad H, Kramer G, et al. Joint state sensing and communication over memoryless multiple access channels[C]//2019 IEEE International Symposium on Information Theory (ISIT). IEEE, 2019: 270-274.
> 2. [^](https://zhuanlan.zhihu.com/p/537067898#ref_19_0)Kobayashi M, Caire G, Kramer G. Joint state sensing and communication: Optimal tradeoff for a memoryless case[C]//2018 IEEE International Symposium on Information Theory (ISIT). IEEE, 2018: 111-115.

浙大An Liu老师课题组[[20\]](https://zhuanlan.zhihu.com/p/537067898#ref_20)[[21\]](https://zhuanlan.zhihu.com/p/537067898#ref_21)

> 1. [^](https://zhuanlan.zhihu.com/p/537067898#ref_20_0)Liu Y, Li M, Liu A, et al. Information-Theoretic Limits of Integrated Sensing and Communication with Correlated Sensing and Channel States for Vehicular Networks[J]. IEEE Transactions on Vehicular Technology, 2022.
> 2. [^](https://zhuanlan.zhihu.com/p/537067898#ref_21_0)Liu Y, Li M, Liu A, et al. Generalized Modeling and Fundamental Limits for Multiple-Access Integrated Sensing and Communication Systems[J]. arXiv preprint arXiv:2205.05328, 2022.



（基于原理？）

### 总结与本质

天然存在一种折中（tradeoff）的问题讨论，也是很多研究的最偏爱的讨论。框架性的研究问题一般有三种，即

雷达为中心（Radar-centric）

通信为中心（Communication-centric）

联合设计（Joint-design）。





MIMO





## 视角帮助



我觉得一开始肯定需要从综述文章开始看起，起码你会知道后续你要阅读的文献他们都是基于什么样的出发点去进行系统建模的，以及为什么这么做，这个很重要，不搞清楚motivation的话，我觉得再靓的工作可能仅仅是mathematical exercise。

我作为即将硕士毕业的学生来看，**有少数的同学是属于放养以后照样可以出很好的成果，学术天才型。**如果你是十分有学术追求的，又没有绝世idea，那你一定要找一个十分nice的老师，起码能懂你在做什么，能分辨你的工作是否trivial，能在关键的时候帮你搞出东西来的。而不是跟大多数硕士一样，乱撞。。。









# 综述

# 综述文献整理

## 从概念到实践



Integrated Sensing and Communication  (ISAC) — From Concept to Practice



如何使用 6G ISAC 提高定位精度以及用便携式终端进行太 赫兹毫米级成像

### 论文深度分析：通信感知一体化（ISAC）——从概念到实践

#### 一、 核心背景与愿景

1. **6G 愿景**：6G 将连接物理世界与数字世界，构建“万物智联”时代。网络感知（Sensing）与原生 AI 是 6G 的两大核心新场景。
2. **ISAC 定义**：将感知与通信能力融入同一套系统，利用无线电波实现“网络即传感器”，在虚拟世界中构建物理世界的数字孪生。
3. **核心优势**：通过共享频谱、硬件和信号处理链路，提高资源效率，降低功耗和设备尺寸。

------

#### 二、 ISAC 的演进等级（三个阶段）

文章将 ISAC 的集成程度划分为三个互补的等级：

- **第一级：业务/光谱/硬件共享**：感知和通信在同一硬件平台运行，共享频谱，但信号处理相对独立。
- **第二级：跨功能融合（融合设计）**：波形和信号处理的一体化，利用同一套波形（如 OFDM 的变体）同时实现数据传输和目标探测。
- **第三级：完全一体化（互惠共生）**：感知信息辅助通信（如感知波束跟踪），通信信息辅助感知，实现系统性能的质变。

------

#### 三、 四大应用场景及 KPI 要求

文章详细定义了 ISAC 的应用版图及严格的性能指标（KPI）：

| **应用大类**         | **具体实例**                                | **关键 KPI 要求**            |
| -------------------- | ------------------------------------------- | ---------------------------- |
| **高精度定位与追踪** | 机器人手术、无人机对接、自动仓储            | 毫米至厘米级精度、高刷新率   |
| **成像与地图构建**   | 室内 3D 重建、SLAM（即时定位与地图构建）    | 高分辨率、低延迟             |
| **人类感官增强**     | 穿墙探测、包裹扫描、食物/污染检测（谱识别） | 太赫兹频段、极高带宽、穿透力 |
| **动作与手势识别**   | 医疗康复监护、无接触家电控制、虚拟乐器      | 微动检测精度、低虚警率       |

------

#### 四、 核心方案分析（一）：基于多径的厘米级定位（SAPE）

这是文中重点介绍的第一个技术突破，旨在解决传统 5G 定位精度受限的问题。

1. **挑战分析**：
   - **多径干扰**：传统方法视多径为噪声，ISAC 将多径视为“有用信息”。
   - **关联难题**：如何将观测到的多径参数准确关联到环境中的反射面（虚拟传输点 vTP）。
   - **精度瓶颈**：受带宽限制，难以区分非常接近的路径。
2. **SAPE 方案（环境感知辅助定位估计）步骤**：
   - **步骤 1（环境探测）**：基站扫描环境，利用反向散射获取环境射频地图（RF Map），识别静态反射面（vTP）。
   - **步骤 2（特定感知）**：终端（UE）利用 SAGE 算法进行超分辨率参数估计（角度、时延）。
   - **步骤 3（参数关联）**：提出一种基于“差分距离”的关联算法，无需 UE 位置先验信息即可匹配多径。
3. **性能结论**：在室内和室外测试中，SAPE 方案比 3GPP R16 的精度提升了 10 倍以上，达到厘米级。

------

#### 五、 核心方案分析（二）：太赫兹（THz）毫米级成像

探讨利用 140GHz 及以上频段在便携终端上实现高分辨率成像。

1. **核心技术：虚拟孔径（Synthetic Aperture）**：
   - **背景**：手机大小无法容纳巨大天线阵。
   - **实现**：利用用户手部自然移动产生的轨迹，在时间维度上构建一个巨大的“虚拟阵列孔径”。
2. **原型样机指标**：
   - **频率**：140 GHz。
   - **带宽**：8 GHz（提供极高的距离分辨率）。
   - **配置**：4T16R。
3. **算法挑战**：必须实时补偿手部抖动带来的相位误差，通过运动补偿和稀疏采样算法恢复图像。

------

#### 六、 详细挑战总结（自查补充完整版）

文章在最后部分和各技术章节详细列出了 ISAC 走向商用的重重阻碍：

### 1. 基础理论与算法挑战

- **多参数估计的非凸性**：感知涉及距离、角度、速度多维解算，计算复杂度极高。
- **多径关联模糊**：在复杂室内环境（如商场），成百上千个反射点会导致关联算法崩溃。
- **杂波抑制**：如何从复杂的背景杂波（如树叶晃动）中提取微小的目标信号。

### 2. 硬件与射频链路挑战

- **全双工/自干扰**：感知需要同时发收，发射信号对接收端的漏泄（Self-Interference）是限制感知距离的核心瓶颈。
- **太赫兹射频损伤**：高频下极易产生相位噪声和 IQ 不平衡，直接导致成像图像畸变。
- **天线阵列限制**：如何在终端有限的空间内布置能支持感知扫描的多天线。

### 3. 架构与标准挑战

- **资源权衡（Trade-off）**：通信带宽与感知精度的博弈，如何动态分配时频资源。
- **隐私与安全**：感知能力可能导致隐私泄露（如穿墙检测），需要新的安全框架。
- **协作效率**：多基站协作感知（Networked Sensing）对同步的要求极高（纳秒级）。

------

#### 七、 结论与前瞻

- **结论**：ISAC 是 6G 的关键使能技术，通过 SAPE 和 THz 成像方案，证明了其在厘米级定位和毫米级成像上的潜力。
- **未来方向**：研究将转向更复杂的动态环境感知、更低功耗的硬件实现以及感知辅助的 AI 网络演进。



## 面向6G的双功能网络

**文章名称**：*Integrated Sensing and Communication: Toward Dual-Functional Wireless Networks for 6G and Beyond*

**作者**：Fan Liu, Yuanhao Cui, et al.



### 1. 核心概念：两大增益

文章开篇定义了 ISAC 技术的两大核心优势：

- **集成增益 (Integration Gain)：** 指通过硬件共享、频谱共享，降低系统的体积、功耗和成本，提高资源利用率。
- **协作增益 (Coordination Gain)：** 指通信与感知互助。例如，感知到的位置信息可以帮助通信快速对准波束（感知辅助通信）；而通信反馈的信道信息也可以辅助感知建模（通信辅助感知）。

### 2. 宏大蓝图：7 大场景与 38 个用例

这篇文章最出名的部分之一是它细致地规划了 6G 通感的应用场景，包括：

- **智慧城市：** 城市高精度定位、侵入检测。
- **智能交通（ITS）：** 车联网、协同自动驾驶。
- **低空经济：** 无人机监管与集群通信。
- **工业 4.0：** 工厂机器人精准导航。
- **环境监测：** 利用无线信号感知降雨、空气质量等。

### 3. 技术核心：性能折衷与边界

文章在技术章节深入探讨了通信与感知在**共用无线资源**时的竞争关系：

- **性能权衡 (Trade-off)：** 如果波束非常窄，对通信增益好，但对搜索目标（感知）不利。
- **性能边界 (Performance Boundaries)：** 揭示了通信速率与感知精度（CRB）之间的理论极限。

### 4. 关键技术：波形与信号处理

- **一体化波形设计：** 综述了如何设计出既能携带高速数据，又能像雷达波一样具备良好自相关特性的信号。
- **接收机处理：** 如何在接收端把微弱的感知回波从强烈的通信干扰中分离出来。
- **协作技术：** 详细讨论了感知如何减少通信的导频开销，以及通信如何为感知提供辅助信息。

### 5. 与新兴技术的有机结合

文章前瞻性地讨论了 ISAC 如何与以下技术“联姻”：

- **RIS（可重构智能表面）：** 绕过遮挡，实现无死角感知。
- **边缘计算/边缘智能：** 在网络边缘处理海量的感知数据。
- **太赫兹/毫米波：** 利用超大带宽实现厘米级甚至毫米级的探测精度。



## ISAC动因应用要求挑战和未来



Integrated Sensing and Communication in 6G: Motivations, Use  Cases, Requirements, Challenges and Future Directions  

Danny Kai Pin Tan, Jia He, Yanchun Li, Alireza Bayesteh, Yan Chen, Peiying Zhu, and Wen Tong  Huawei Technologies Co., Ltd. 

### 6G 集成传感与通信 (ISAC)

#### 1. 核心定义：从“共存”到“互惠”

- **定义**：6G ISAC 不仅仅是硬件共享，而是波形、信号处理、协议栈以及资源（时/频/空）的全方位深度融合。
- **双向增益**：
  - **通信为感知提供基础**：利用广泛覆盖的基站网络实现全城范围的感知。
  - **感知辅助通信 (Sensing-assisted Comms)**：这是我之前漏掉的要点。通过感知环境，通信系统可以预知障碍物、优化波束追踪、减少信道估计开销。

------

#### 2. 细分场景与服务（包含遗漏的“感知辅助”部分）

##### 类别 A：感知作为新服务 (Sensing as a Service)

1. **高精度定位与跟踪**：
   - **垂直行业**：AGV 机器人协作（2cm 精度）、无人机自动对接（5mm 相对精度）。
   - **语义定位**：将物理坐标转化为“把物体放在桌上”这类 AI 可理解的任务。
2. **同步建图与定位 (SLAM)**：
   - 通过多径信息（反射、衍射）重建室内外环境的 3D 模型，即使在非视距（NLOS）环境下也能通过虚拟锚点实现高精度定位。
3. **增强人类感知 (Enhanced Human Sensing)**：
   - **太赫兹成像**：实现毫米级分辨率，看清黑暗或浓雾中的物体。
   - **材料识别**：通过光谱分析探测物质成分（如检测污染物、识别液体种类）。
4. **手势与活动识别**：
   - **无接触交互**：虚拟钢琴（3mm 精度）、手势控制。
   - **健康监测**：非侵入式监测呼吸、心跳及跌倒报警。

##### 类别 B：感知辅助通信 (Sensing-assisted Comms) —— **重点补充**

1. **波束成形优化**：利用感知获取的终端位置和速度，直接进行预测性波束成形，显著降低波束扫描延迟。
2. **信道状态信息 (CSI) 压缩**：感知到的环境地图可以作为“先验知识”，减少通信所需的训练序列开销。
3. **故障检测与切换**：预判物体遮挡导致的链路中断，提前规划切换路径。

------

#### 3. 关键性能指标 (KPI)

除了传统的通信指标（吞吐量、延迟），文章定义了感知的核心指标：

- **感知准确度 (Accuracy)**：估计值与真值的偏差。
- **分辨率 (Resolution)**：区分两个相邻目标的能力（距离分辨率、角度分辨率）。
- **覆盖范围 (Coverage)**：感知能达到的最大探测距离（通常受限于信噪比）。
- **更新速率 (Update Rate)**：感知信息的实时性。

------

#### 4. 技术挑战：全方位梳理

##### A. 基本权衡与界限

- **率失真权衡 (Rate-Distortion Tradeoff)**：系统需在“传输多少数据”和“感知多准”之间寻找最优平衡点。

##### B. 波形设计挑战

- **CP-OFDM**：通信好，但雷达性能下由于 **PAPR（高峰均比）** 和 **范围-多普勒解耦** 问题受限。
- **OTFS（正交时频空间波形）**：文章提到的新兴波形，在高速移动环境下感知性能优于 OFDM。

##### C. 硬件限制

- **全双工 (Full Duplex)**：自干扰抑制是关键。感知需要同时收发，但发送信号的泄露会淹没微弱的回波。
- **相位噪声与抖动**：高频段（毫米波/太赫兹）下硬件的不完美严重干扰相干信号处理。

##### D. 信道建模：从统计到确定性

- 必须从传统的**随机模型**向基于环境特征的**确定性模型（射线追踪）\**或\**混合模型**转变。

------

#### 5. 语义通信与未来方向

- **语义语义交互**：通信不再仅是比特传输，而是基于感知的环境理解，实现“意图驱动”的连接。
- **智能环境感知**：利用 AI 提取特征，让网络具备“认知”能力。

即

**AI 与学习算法**：利用信念传播和融合算法增强多节点协作感知的性能 。

**语义通信辅助**：利用感知获得的环境知识实现“环境感知通信”，通过减少重复的信道估计过程显著降低开销 。

**协作 ISAC**：在空间和时间维度上共享节点信息，提升整体感知精度 。



# 问题总结

我们SRTP的主要目标是什么？

