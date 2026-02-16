# WiResP: 一种基于频谱增强的鲁棒性WiFi呼吸监测系统 (WiResP: A Robust WiFi-Based Respiration Monitoring via Spectrum Enhancement)

Wei-Hsiang Wang, Student Member, IEEE, Beibei Wang, Fellow, IEEE, Xiaolu Zeng, and K. J. Ray Liu, Fellow, IEEE

**摘要** — 呼吸监测因其在评估睡眠阶段和质量方面的潜力而备受关注。传统的呼吸频率跟踪方法需要专用的可穿戴设备，这可能会干扰用户并带来不舒适的体验。为了解决这个问题，研究人员提出了基于WiFi的呼吸监测系统，该系统利用WiFi的普及性、成本效益和非接触特性，有效地将现有基础设施转化为无处不在的传感器。然而，现有系统通常难以处理嵌入在信号中微弱的呼吸信号的信噪比问题，导致在嘈杂环境中覆盖范围有限且部署不灵活。本文提出了WiResP (WiFi Respiration Perception， WiFi呼吸感知)，一种巧妙且实用的基于WiFi的呼吸跟踪系统，采用频谱增强方法来增强呼吸检测。通过将从信道状态信息获得的呼吸信号频谱视为图像，该系统能够利用图像处理技术显著增强呼吸信号轨迹，提高可检测性并增加呼吸频率估计的感知覆盖范围。此外，提出了一种基于图像的连续性检查器模块来验证信号轨迹的连续性以减少误报。我们在不同设置下进行了大量实验并评估了WiResP。实验表明，WiResP能够在睡眠期间可靠地捕获呼吸频率，达到92%的检测率和<5%的误报率，从而在灵活的设备放置下显著改善睡眠阶段识别。其优异的性能使WiResP成为现实世界中家庭呼吸跟踪系统的候选方案。

**关键词** — 信道状态信息，物联网，呼吸监测，频谱增强，传感器，WiFi感知。

## I. 引言

呼吸频率 (Respiratory Rate, RR) 是衡量个人健康状况的关键指标，例如，可以通过RR进行睡眠阶段[2]分析来确定睡眠的质量和深度[1]。随着传感、通信技术和低成本微型计算机的进步，基于物联网的呼吸监测得到了迅速发展。然而，大多数现有的RR监测系统[3]–[5]，如胸带、心电图设备和鼻罩，由于物理接触而具有侵入性且不舒适，导致准确性受损。因此，一些无接触（无设备）RR监测系统被提出，它们使用专用相机[6],[7]、超宽带脉冲无线电[8],[9]、连续波雷达[10],[11]、毫米波雷达[12],[13]等。尽管这些系统可能提供准确的RR估计，但所需的专用设备可能价格昂贵，使得商业部署不可行。

在过去的十年中，WiFi感知[14]–[19]因其非侵入性、保护隐私和成本效益高的特性而受到越来越多的关注，这是通过将现有基础设施重新用作无处不在的传感器来实现的。这些特性使其成为广泛应用的诱人选择，包括室内定位[20],[21]、运动检测和分类[22]–[24]，以及生命体征和睡眠监测[25]–[43]。其中，基于WiFi的RR估计已成为一个特别受欢迎的话题，有许多研究和提议致力于其发展。

这项工作得到了马里兰州罗克维尔的Origin Research的支持，第一作者作为实习生在此进行了这个研究项目，并得到了加利福尼亚州尔湾市的Key Bridge Foundation通过研究奖学金提供的部分支持。

W.-H. Wang 与 Origin Research (地址: 美国马里兰州罗克维尔，20852) 以及马里兰大学学院帕克分校电子与计算机工程系 (地址: 美国马里兰州学院帕克，20742) 有联系 (电子邮件: whwang19@umd.edu)。  
B. Wang、X. Zeng 和 K. J. R. Liu 与 Origin Research 有联系 (地址: 美国马里兰州罗克维尔，20852) (电子邮件: {beibei.wang, xiaolu.zeng, ray.liu}@originwirelessai.com)。

基于WiFi的RR估计主要可分为两类：基于接收信号强度指示 (Received Signal Strength Indicator, RSSI) 的方法和基于信道状态信息 (Channel State Information, CSI) 的方法。基于RSSI的RR估计依赖于呼吸过程中胸部运动引起的WiFi信号接收强度的变化。该方法通过跟踪信号强度随时间的变化来计算RR[35]–[38],[44]；另一方面，基于CSI的RR估计使用与WiFi信号相位和幅度相关的信息，因为CSI提供了比RSSI更精细和更丰富的信息，使其能够捕获更复杂的呼吸模式，并可应用于RR估计以外的各种应用，包括睡眠模式和阶段分析等[25]–[34],[39]–[43]。

尽管先前的研究[25]–[34],[39]–[43]已经证明了使用WiFi进行RR估计的潜力，但其中许多系统在现实场景中表现出有限的有效性，它们依赖于受控设置和特定的设备部署配置。当呼吸信号强度较弱，导致低信噪比 (Signal to Noise Ratio, SNR) 时，这些局限性变得尤为明显。例如，其中一些技术可能要求在受监测个体与WiFi收发器之间存在直接视线 (Line-of-Sight, LOS) 连接。此外，它们的操作范围通常受限，需要受试者靠近WiFi设备或链路，这是因为与呼吸相关的细微胸部运动可能不足以被鲁棒地检测到，除非设备处于最佳位置。如果当个体离设备太远或系统在非视线 (Non-Line-of-Sight, NLOS) 场景下运行时，呼吸信号极弱，这些方法甚至可能失败。此类限制阻碍了部署的灵活性，并限制了感知覆盖的范围。

在本文中，我们提出WiResP (WiFi Respiration Perception， WiFi呼吸感知)，一种基于WiFi的呼吸监测系统，旨在克服设备放置不灵活和感知范围有限的挑战。WiResP是一种新颖的、无需训练的、实用的、受图像启发的RR跟踪系统，即使在低SNR条件下也能可靠地捕获呼吸信号。WiResP基于自相关函数 (Auto Correlation Function, ACF) 谱估计RR，该谱已被证明能以与环境无关的方式反映呼吸频率[22]，并进一步将频谱视为图像，应用频谱增强技术来突出/锐化呼吸信号的轨迹。WiResP整合了瞬时和时域数据，以增强峰值识别和呼吸模式识别，显著提高了检测能力并扩大了RR估计的感知范围。此外，我们设计了一个利用ACF谱上呼吸模式（呼吸轨迹的连续性）的误报 (False Alarm, FA) 消除模块来减少误报。大量的实验评估证明了WiResP在感知覆盖范围和部署灵活性方面的卓越改进，通过WiFi感知为现实环境中的睡眠阶段识别提供了可靠和精确的呼吸监测。本文的主要贡献包括：

- 我们引入了WiResP，它为整个房屋提供宽广的感知覆盖范围，并允许灵活的设备放置。我们使用频谱增强技术来放大微弱的呼吸模式，并通过利用ACF谱上呼吸模式的连续性来综合考虑瞬时和时域信息。
- 我们通过在多种部署场景下进行的大量实验，通过检测率和睡眠阶段分析评估了WiResP的性能。WiResP以25%更高的检测率和低于5%的误报率超越了两种参考系统。
- 据我们所知，所提出的系统是首个通过将ACF谱建模为图像，利用图像处理技术来增强RR跟踪/估计的系统。

本文的剩余部分安排如下。第二部分回顾了基于WiFi的RR估计的相关工作。第三部分提供了CSI模型的基本理论。第四部分概述了WiResP。第五部分详细讨论了WiResP。第六部分展示了系统评估和实验结果。我们在第七部分探讨了所提出方法的局限性并探索了一些未来发展的潜在方向。第八部分总结了全文。

## II. 相关工作

在本节中，我们回顾了关于无接触RR监测的相关工作。

### A. 无接触RR监测
早期的无接触RR估计方法包括采用专用相机[6],[7]、超宽带脉冲无线电[8],[9]、连续波雷达[10],[11]和毫米波雷达[12],[13]的方法。基于视觉的相机系统通过分析帧间差异来跟踪胸部和腹部的运动，从而估计RR。然而，它们引发了隐私问题，并且对光照条件敏感，需要特定的放置位置和方向。另一方面，基于雷达的方法向目标发射脉冲，并分析反射信号来估计由呼吸运动引起的周期性变化。尽管它们能提供准确的估计，甚至能够捕获心跳[12],[13]，但它们通常涉及专用设备，这限制了它们在家庭环境中的实用性。此外，雷达信号随距离快速衰减且具有方向性，需要精确的设备方向和对准感兴趣的区域。

### B. 基于WiFi的RR监测
WiFi的广泛部署激发了关于WiFi感知的重要研究，催生了许多基于WiFi的RR监测应用。文献[35]–[38]的作者使用RSSI，通过分析呼吸引起的信号强度变化来估计RR；然而，RSSI的低分辨率导致准确性较差。Wi-Sleep[31]利用CSI的幅度来估计呼吸，因为与RSSI相比，CSI提供了更细粒度的信息。Phasebeat[25]和TensorBeat[34]利用两天线之间的相位差来估计RR。后来，引入了菲涅尔模型[40]，从室内WiFi传播的角度解释了个体位置、方向和呼吸深度对呼吸可检测性的影响。TR-BREATH[33]提出利用时间反转共振强度 (Time-Reversal Resonating Strength, TRRS) 和root-MUSIC算法来估计RR。一些基于学习的技术[41]–[43]也被提出来用于从噪声无线电信号中准确估计呼吸模式。此外，多项工作研究了使用WiFi进行多用户RR和心跳估计[27]–[30],[33],[34],[45]。

这些系统中有许多依赖于特定的设备布置和受控环境，当呼吸信号较弱（低SNR）时，常常无法准确捕获RR。由于呼吸时胸部运动的细微性，它们可能需要在测试者和收发器之间存在LOS，并且操作范围有限，要求受试者靠近设备。基于学习的方法[41]–[43]在特定设置下通常具有高精度，但在新环境中可能需要进一步训练，限制了其适用性。因此，由于其严格的部署要求，这些系统的覆盖区域常常受到限制。为了提取微弱的呼吸信号并提高感知能力，SMARS[46]采用最大比合并 (Maximal Ratio Combining, MRC) 来合并多个子载波的信号，并提高呼吸信号的SNR，而FarSense[39]则利用两个天线之间CSI的比率。然而，实验结果表明，当人体距离WiFi设备较远时，这些系统的有效性仍然容易受到影响，从而限制了它们的覆盖范围。

在我们之前的工作[47]中，我们已经证明了频谱增强在提高房间级别场景（设备和测试者位于同一房间）检测率方面的有效性。然而，在房屋级别场景（设备放置在房间外）中，由于覆盖范围更广，我们遇到了对环境运动和噪声的敏感性增加的问题，导致应用频谱增强后误报显著增加。这些误报无法通过我们先前工作中的方差滤波器去除。为了应对这一挑战，本文通过引入一个更全面、更鲁棒的系统来扩展我们之前的工作。所提出的系统进一步结合了运动检测以及一个从图像处理角度考虑ACF谱连续性来减少误报的专用模块。我们进行了大量实验以证明其性能。

## III. CSI模型

给定一对发射机 (Transmitter, TX) 和接收机 (Receiver, RX)，在时间实例t、链路m上的特定频率f处的CSI可以建模[46]为

$$H^{(m)}(f,t) = \sum_{i = 1}^{L}a_{i}(t)\exp \left(-j2\pi f\frac{d_{i}(t)}{c}\right) + n(f,t), \quad (1)$$

其中d_i(t)是第i条路径的传播距离，a_i(t)是幅度，c表示光速，L表示多径的总数。此外，n(f,t)是加性白噪声。

![图1: 室内多径传播。](placeholder)

如图1所示，无线信号通过不同的多径传播，每条多径对环境变化的敏感度各不相同。敏感度取决于位置，意味着运动源的存在会导致某些多径经历时变的传播路径和增益，而其他路径保持静态，使得CSI可以表示为

$$H^{(m)}(f,t) = \sum_{i\in \Omega_S}a_i e^{\frac{-j2\pi f d_i}{c}} + \sum_{i\in \Omega_D(t)}a_i(t)e^{\frac{-j2\pi f d_i(t)}{c}} + n(f,t) = H_S^{(m)}(f) + H_D^{(m)}(f,t) + n(f,t), \quad (2)$$

其中Ω_D(t)表示受时变散射体影响的动态多径集合，而Ω_S表示静态路径的集合。在足够短的时间帧内，文献[46]表明静态路径保持相对一致，可以通过均值相减来消除，即

$$\hat{H}^{(m)}(f,t) = H_D^{(m)}(f,t) + n(f,t), \quad (3)$$

其中\hat{H}^{(m)}(t,f_k)表示链路m上在时刻t、频率f处均值为零的CSI。对于在每个时刻t的每条链路上有N_s个子载波的多载波WiFi系统，CSI可以表示为

$$\hat{H}^{(m)}(f_k,t) = H_D^{(m)}(f_k,t) + n(f_k,t), \quad k = 1,\dots,N_s. \quad (4)$$

当存在周期性环境波动（如呼吸运动）时，\hat{H}^{(m)}(f_k,t)表现出周期性变化，从而使得RR估计成为可能。

## IV. 系统概述

本节概述了图2中所示的WiResP。WiResP是一种基于WiFi的感知系统，利用CSI仅通过一对TX-RX对来检测和估计RR。主要组件包括CSI预处理、运动检测器和呼吸监测模块。

![图2: 系统概述](placeholder)

CSI预处理包括对CSI进行清理，然后为后续的运动检测和呼吸监测模块计算ACF。

运动检测器旨在检测周围环境中是否有任何显著的运动。如果没有检测到运动，则将提取的ACF传递给呼吸监测模块。这是因为RR的估计通常会因运动产生的干扰而受损。这种干扰会破坏对WiFi信号细微变化的准确测量，导致RR估计不准确或无法检测。

呼吸监测模块包含两个主要部分：(i) 呼吸增强模块和 (ii) 呼吸估计模块。在呼吸增强模块中，首先采用子载波选择和合并技术来增强呼吸模式。其次，对ACF谱执行频谱增强，该ACF谱是通过连接给定时间窗口内的ACF快照生成的。

之后，WiResP执行连续性检查以消除误报并确保估计的RR准确可靠，包括对增强后的ACF谱进行二值化以简化分析，以及一个呼吸模式检查器来验证呼吸模式。如果呼吸模式通过连续性检查，则将ACF谱发送给峰值检测模块以估计RR。

## V. 系统设计

本节详细介绍WiResP的系统设计。假设RX有N_r根天线，TX有N_t根天线，总共有N_t × N_r条链路。每条链路包含N_s个子载波。探测速率设置为F_s Hz，呼吸提取使用窗口大小T_w。

### A. CSI预处理

CSI测量容易受到噪声、硬件缺陷等的破坏。此外，CSI值可能因接收机的自动增益控制机制而波动。因此，预处理对于提取有价值的信息至关重要。我们对所有子载波的CSI样本进行归一化，并对每个归一化的子载波在时间窗口T_w上应用中值滤波以去除异常值。接下来，我们仅提取CSI测量的幅度用于进一步处理，因为WiFi信号的相位由于载波频率偏移和测量噪声而通常噪声太大而无法使用。因此，总共有N_t N_r N_s个幅度流用于提取呼吸模式。之后，为每个流计算ACF，用于后续的运动检测和RR估计。通过计算CSI的ACF，我们可以识别信号与自身相关的滞后时间。ACF出现显著峰值处的滞后时间对应于信号的周期；因此，可以估计RR。选择使用ACF是因为它能在短窗口内产生即时估计[46]，并且已被证明能以与环境无关的方式反映呼吸频率[22]。具体来说，将s_i^{(m)}表示为链路(n_t,n_r)上第i^th个子载波的CSI向量，由幅度得到的ACF ρ_i^{(n_t,n_r)}(τ)计算如下

$$\rho_{i}^{(n_{t},n_{r})}(\tau) = \frac{\mathrm{COV}\left[|\boldsymbol{s}_{i}^{(n_{t},n_{r})}(t)|, |\boldsymbol{s}_{i}^{(n_{t},n_{r})}(t + \tau)|\right]}{\mathrm{COV}\left[|\boldsymbol{s}_{i}^{(n_{t},n_{r})}(t)|, |\boldsymbol{s}_{i}^{(n_{t},n_{r})}(t)|\right]}, \quad (5)$$

其中τ = 0,…,T_w F_s表示时间滞后，COV表示协方差运算。在下文中，ρ_i^{(m)}(τ)用于表示从幅度流获得的ACF，其中m = 1,…,N_t N_r N_s。

![图3: 子载波合并示意图。第一个峰值位于\hat{τ} = 4.267，估计的RR为60/4.267 = 14.06 BPM。](placeholder)

### B. 运动检测器
由于呼吸信号很容易被环境中的大幅度运动破坏，导致呼吸频率检测不准确，因此在存在显著运动时不应进行呼吸提取。因此，在RR估计之前，我们可以采用我们早期工作中提出的基于ACF的运动检测[22]，以降低系统复杂性和功耗。如文献[22]所示，从CSI功率获得的ACF的第一个抽头，称为运动统计量ξ，指示了周围运动的整体强度，在实践中可以通过计算τ = 1 / F_s处的值来近似，如下所示

$$\xi = \mathbf{E}\left[\rho_{i}^{(m)}(\tau)\right]\Big|_{\tau = \frac{1}{F_{s}}}, \quad (6)$$

其中\mathbf{E}是期望算子，m = 1…N_t N_r N_s。然后，仅当ξ小于预定阈值η时才启用呼吸跟踪，该阈值η是通过评估典型日常人体运动（不包括静态活动）凭经验选择的[22]。

### C. 呼吸信号增强
本节介绍一个具有两阶段配置的增强模块，用于提高呼吸信号的SNR。

#### 1) 阶段1 - 子载波选择和合并
由于多径对单个子载波的影响波动不定，子载波的选择和合并可能具有挑战性。仅仅计算所有子载波的平均值可能无法有效提高合并后信号的SNR，特别是当测试者远离链路时。在这种情况下，某些子载波可能比其他子载波受到更严重的破坏，如文献[46][48]所示。为了克服这个问题，我们提出一个解决方案，即根据最高的呼吸噪声比 (Breathing-to-Noise Ratio, BNR)[49]，从总共N_s N_t N_r个子载波中选择K个子载波。BNR是感兴趣范围（即正常人体呼吸）内能量与范围外能量的比例。合并后的ACF可以通过ρ_combined = Σ_{i∈V} ρ_i × BNR_i获得，其中V指的是包含展现出最高BNR的前K个候选者的集合。接下来，我们应用局部估计散点图平滑 (LOESS) 来细化合并后的ACF信号[46]。图3展示了一个合并和平滑后的ACF实例，显示了BNR的显著改善。

#### 2) 阶段2 - ACF谱增强
通过连接连续时间窗口的ACF来获得ACF谱。当在此时间跨度内存在有效呼吸时，连接起来的ACF形成可辨别的呼吸轨迹，具有互连的波峰和波谷，代表呼吸信号的周期性。图4(a)显示了从20分钟呼吸数据中提取的ACF谱，在某些时间间隔内展示了清晰的呼吸模式。然而，在其他时间间隔内，由于各种原因（如受试者改变睡姿或在特定睡眠阶段出现浅呼吸），呼吸信号强度不足以产生可检测的模式，导致ACF谱模糊且RR无法被检测到。

为了改善峰值识别和呼吸模式识别，我们引入了一种将ACF谱视为图像的方法。我们应用直方图均衡化方法来提高呼吸信号的SNR。该技术均匀分布强度并增加对比度，从而实现更好的呼吸模式识别。令T_e为增强窗口大小（以秒为单位），X = [x_1, …, x_{T_w F_s × T_e F_s}]表示ACF谱的数组。变换后的变量y_i的计算值为

$$\hat{F}_X(t) = \frac{\sum_{i=1}^{F_s T_w \cdot F_s T_e} \mathbb{1}(x_i \leq t)}{F_s T_w \cdot F_s T_e},$$
$$y_i = \hat{F}_X(x_i), \quad (7)$$

其中\hat{F}_X(·)表示X的经验累积分布函数 (Cumulative Distribution Function, CDF)，\mathbb{1}(·)表示指示函数。图4(b)显示，应用直方图均衡化后，微弱和弱的呼吸信号轨迹变得更加清晰可辨。

![图4: (a) 原始谱。 (b) 增强后的谱。](placeholder)

### D. 呼吸轨迹连续性检查
尽管上述技术可以显著提高RR检测能力，但它仍然容易受到由环境微小运动(ξ < η)或被子载波合并和直方图均衡器放大的噪声引起的误报影响，这些情况可能产生类似呼吸的ACF，从而导致错误检测。因此，仅在检测到实际RR并持续一定时间后才估计RR非常重要。

我们提出了一种低复杂度的、基于模式的呼吸轨迹连续性检查方法，该方法采用Otsu阈值法[50]对第V-C2节中获得的ACF谱进行二值化。频谱二值化简化了增强呼吸模式中波峰和波谷的识别，使其更容易与参考信号进行比较，从而改善对增强后ACF谱的分析并降低计算复杂度。

生成的二值化ACF谱随后被发送到呼吸模式检查器，该检查器对持续时间为T_e的二值化谱进行平均，以获得整体趋势。然后将整体趋势与参考的二值化一维呼吸模式进行比较。步骤如下：

- **步骤1:** 将增强后的ACF谱Y = [y_1, …, y_i, …, y_{T_w F_s × T_e F_s}]（公式(7)）用阈值Θ分成两类C_0和C_1，Θ由下式确定，
  $$\Theta = \arg \max_{0< \theta < 1} \left\{ \hat{F}_Y(\theta)[1 - \hat{F}_Y(\theta)](\mu_{C_0} - \mu_{C_1})^2 \right\}, \quad (8)$$
  其中μ_{C_0}和μ_{C_1}分别表示C_0和C_1的均值；\hat{F}_Y(·)是增强谱Y的经验CDF。那么二值化谱Y = {Y_i}可以表示为
  $$\mathcal{Y}_i = \left\{ \begin{array}{ll}1, & y_i \in C_0 = \{a_i|a_i \geq \Theta \} \\ 0, & y_i \in C_1 = \{a_i|a_i < \Theta \} \end{array} \right.. \quad (9)$$
  图5(a)显示了呼吸ACF谱的示例，图5(c)显示了图5(a)的二值化谱。
- **步骤2:** 将二值化ACF谱在持续时间T_e上求平均，以评估呼吸轨迹的连续性。具体来说，将\tilde{\mathbf{u}}_t表示为时间t处的二值化ACF快照。平均二值化ACF谱为\frac{1}{T_e}\sum_{t = 1}^{T_e}\tilde{\mathbf{u}}_t。图5(d)（顶部）显示了平均二值化ACF谱。
- **步骤3:** 再次对步骤2中获得的平均二值ACF进行二值化，以减少短期波动的影响，得到一维平均二值ACF，\tilde{\mathbf{u}}_B，如图5(d)所示。具体来说，该一维信号通过下式计算
  $$\begin{array}{c}{\tilde{\mathbf{u}}_B = \mathcal{B}\left(\frac{1}{T_e}\sum_{t = 1}^{T_e}\tilde{\mathbf{u}}_t\right),}\\ {\mathcal{B}(v) = \delta_{v\geq \frac{1}{2}},} \end{array} \quad (10)$$
  其中\mathcal{B}(·)是逐元素算子，对输入的每个元素进行四舍五入，δ是克罗内克δ函数。

![图5: (a) 随时间变化的呼吸ACF谱。 (b) (a)的整体平均。 (c) (a)的二值化ACF谱。 (d) 从(c)获得的趋势及其二值化波形。](placeholder)

然后，利用此趋势\tilde{\mathbf{u}}_B基于正常呼吸模式的连续性来确定在T_e时间帧内是否存在有效呼吸。如果在整个T_e期间检测到一致的呼吸模式，则视为有效呼吸事件，可以据此估计RR。例如，呼吸周期数（即图5(d)中描绘的矩形脉冲）以及T_w内的平均脉冲宽度可以作为正常人体呼吸的鲁棒指标。需要注意的是，不应直接对原始ACF谱（即图5(b)）求平均，因为ACF的幅度随时间波动，使得与参考信号的比较变得困难。借助二值化波形，与参考信号的比较得以简化。

### E. 呼吸频率估计
在检测到时间帧T_e内存在有效呼吸后，使用增强谱通过第一个峰值与0之间的时间差来估计RR。例如，图3展示了一个τ的实例。我们使用增强谱而不是二值化谱来估计RR，因为与具有清晰波峰/波谷的原始增强谱相比，二值化谱存在信息损失，特别是在需要精确RR估计的应用中，例如睡眠阶段分析。

## VI. 实验结果
本节介绍使用商用WiFi芯片组对WiResP的评估。在家庭环境中进行了大量实验。

### A. 实验设置
我们在真实住宅环境中使用一套商用WiFi设备评估了WiResP，该设备采用支持802.11ac和2.4G及5G双频功能的高通WiFi模块。设备设置为工作在5.805GHz，F_s = 30Hz，RX/TX分别有两根/一根天线。带宽为40MHz，分为128个子载波，其中24个子载波保留作为保护带。我们收集了35晚的睡眠数据（约280小时），并使用SleepBreathe[51]（一种家用睡眠监测面罩）来收集准确的RR作为地面真值。

为了展示系统增强的覆盖范围和灵活的设备放置能力（无论设备/受试者是否处于固定位置），我们进行了全面的实验，考虑了不同的房间尺寸、各种设备方向/位置、以及相对于测试者的收发器链路距离，测试者在自然睡眠条件下，包括不同的睡眠姿势以及人是否被被子覆盖。此外，我们考虑了LOS和NLOS情况，将TX放置在室内和室外，并采用了不同的设备高度。我们在两个不同的房间放置了两套设备，并设置了多个RX以提高数据收集效率，在图6中用绿色圆圈和棕色矩形表示。对于每组，我们考虑了三种不同情况来评估性能。我们用TX_i^k表示第i组在第k种情况下的TX，用RX_{i,j}^k表示第i组在第k种情况下的第j^th个接收机。

![图6: 所有实验案例的图示。](placeholder)

- **组1 (绿色圆圈):** 接收机的放置位置取决于测试者房间内电源插座的可用性，高度由电源插座、书桌或床头柜的高度决定，这与许多其他需要特定设备布置的研究形成对比。
  - **案例1 (RX_{1,j}^1):** LOS条件，TX放置在床边的床头柜上。RX_{1,6}^1的放置使其链路在很近的距离内穿过身体，为呼吸检测提供了理想的位置，作为基线。
  - **案例2 (RX_{1,3}^2):** NLOS场景，评估系统时将TX放置在放置了组1 RX的房间外。
  - **案例3 (RX_{1,3}^3):** 这是LOS条件，我们交换了案例1中RX#7和TX的位置。我们评估了不同链路方向对系统性能的影响。
- **组2 (棕色矩形):** 我们研究了RX的高度和不同位置。我们将RX#1、RX#3、RX#5和RX#7放置在典型的桌面高度，而将RX#2、RX#4、RX#6和RX#8安装在天花板上。
  - **案例1 (RX_{2,1}^1 … RX_{2,8}^1):** NLOS场景，TX靠近部署RX的房间。RX#1和TX之间的距离约为2.5米。
  - **案例2 (RX_{2,1}^2 … RX_{2,8}^2):** NLOS场景，TX放置在餐桌上。RX#1和TX之间的距离约为5米。
  - **案例3 (RX_{2,1}^3 … RX_{2,8}^3):** NLOS场景，TX放置在台面上。RX#1和TX之间的距离约为8米。

注意，(RX_{2,2}^k,RX_{2,3}^k)、(RX_{2,4}^k,RX_{2,5}^k)和(RX_{2,7}^k,RX_{2,8}^k)位于相同位置，但高度不同。表I总结了所有设备的高度，图6展示了所有案例。

**表I: 组1和组2的TX和RX的高度。**
| 设备 | TX1 | TX2 | TX3 |
|------|-----|-----|-----|
| 组1 (i=1) | 0.47m | 0.77m | 0.75m |
| 组2 (i=2) | 0.77m | 0.95m | 0.95m |

| 设备 | RX1 | RX2 | RX3 | RX4 | RX5 | RX6 | RX7 | RX8 |
|------|-----|-----|-----|-----|-----|-----|-----|-----|
| 组1 (i=1) | 0.47m | 0.47m | 0.74m | 1.58m | 1.14m | 0.81m | 0.75m | - |
| 组2 (i=2) | 0.47m | 2.10m | 0.70m | 2.10m | 0.75m | 2.18m | 0.80m | 2.10m |

### B. 检测率比较
#### 1) 总体比较
图7展示了在三个系统中，相对于未经增强的原始呼吸信号的BNR，整体的检测率。当BNR约为3时，WiResP达到90%的检测率，而FarSense和SMARS分别在BNR约为8和10时达到相同的检测率。WiResP达到90%检测率所需的BNR相比其他参考系统降低了约2.5倍。即使在较低的BNR下，WiResP也显著改善了呼吸信号轨迹。值得注意的是，WiResP的检测率可以持续保持一定性能，直到BNR接近零，克服了现有方案的主要缺点。这是因为在极低BNR下，呼吸模式的质量太差，无法被可靠地增强。WiResP的优越性能归功于其有效的频谱增强技术，该技术显著提高了呼吸信号的SNR。

![图7: 检测率与BNR的关系。](placeholder)

#### 2) 按位置的详细比较
图8显示了组1三种情况下三个系统检测率的详细比较。由于其两阶段SNR增强技术，WiResP在所有位置上都比SMARS和FarSense高出超过20%。可以看出，案例1中的RX#6 (RX_{1,6}^1)表现出优越的性能，因为TX和RX之间的链路穿过胸部区域，能够鲁棒地捕获用于RR检测的胸部运动。对于房间级别的设置，即案例1和案例3，检测率普遍较好，因为存在LOS，系统可以有效地检测胸部运动。然而，随着RX、TX和测试者之间的距离增加（即案例2），所有三个系统的有效性都下降。RX_{1,1}^1、RX_{1,2}^1和RX_{1,5}^1的检测率不如其他RX位置高，因为RX和TX之间的链路长度不足以持续覆盖测试者。这些局限性部分可归因于文献[40]中阐述的菲涅尔区。菲涅尔区由以TX和RX为焦点的同心椭球体组成。这些区域内多径的相互作用导致RX处信号幅度的变化：来自奇数区的反射增强幅度，使CSI波形上的响应更明显，而偶数区则倾向于削弱幅度。尽管如此，WiResP在这些挑战性场景中仍然优于其他两种方法，这得益于其关键的增强功能，展示了其卓越的性能。

![图8: 组1在三种情况下的检测率。](placeholder)

图9评估了组2在NLOS条件下的检测率。很明显，得益于增强模块，WiResP显著优于其他两个系统，性能提升超过25%。值得注意的是，对于k = 1～3，RX_{2,3}^k、RX_{2,4}^k和RX_{2,5}^k位置的检测率普遍低于其他位置，这是因为收发器与测试者之间的距离增加，导致信号强度下降。如前所述[40]，这种性能下降可以部分地通过菲涅尔区的概念来解释。

![图9: 组2在三种情况下的检测率。](placeholder)

### C. 睡眠监测分析
我们通过我们先前验证过的基于RR的睡眠阶段分析方法[46]来评估三个系统在可靠检测RR方面的有效性。其原理是，在快速眼动 (Rapid Eye Movement, REM) 睡眠阶段，RR通常具有较高的变异性和不规则性，而在非快速眼动 (Non-Rapid Eye Movement, NREM) 睡眠阶段，RR则较低且更稳定。在清醒阶段，身体运动比NREM和REM阶段更频繁，但比持续活动期间（如非睡眠运动）少。因此，测试者的睡眠阶段可以分类为清醒、NREM和REM。我们定义活动阶段为测试者存在显著持续运动的时期，在此期间无法估计RR；而非活动阶段代表未检测到RR且无运动的时期。这种评估提供了对系统在呼吸监测方面稳定性的深入见解，而不仅仅是检测率分析。因此，我们通过计算睡眠监测中漏检的RR SM_missed来进行此睡眠分析，定义为

$$SM_{missed} = \frac{Dur_{\{Inactive\}}}{Dur_{\{Sleep\}} - Dur_{\{Active\}}},$$
$$SM_{pass} = 1 - SM_{missed}, \quad (11)$$

其中Dur_Sleep是每个睡眠数据集的总时长，Dur_{Active}和Dur_{Inactive}分别是活动阶段和非活动阶段的时长。SM_pass表示睡眠监测中成功RR检测的比例。

表II展示了组1和组2中每个位置的SM_pass平均值。清楚的是，WiResP为睡眠阶段分析提供了更稳定和连续的RR检测和估计。一个有趣的点是，即使在某些场景下，某些系统显示出令人满意的RR检测率，但它们的SM_pass仍然相对较差。这种差异可以归因于在睡眠阶段分析期间RR的检测不一致和间歇性，导致更频繁的非活动阶段分类。例如，虽然WiResP在RX_{2,1}^1和RX_{2,8}^1上保持大致相同的检测率，但在RX_{2,1}^1上表现出较低的SM_pass。此外，在某些情况下，一些系统的SM_pass高于其相应的检测率。这可以通过以下事实解释：尽管用于睡眠阶段分析的预定窗口内并非每秒都检测到RR，但检测到的次数足以进行可靠的睡眠阶段分析。例如，WiResP在RX_{2,6}^3上的SM_pass = 86.27%，而其在图9中相应的检测率为77.61%。总体而言，WiResP在房屋级别设置下可以达到与房间级别场景相当的性能。这表明WiResP能够提供广泛的感知覆盖范围，同时仍然保持稳定和准确的RR检测以用于睡眠阶段分析。

**表II: 组1和组2每个位置的平均SM_pass。**（表中数据为示例，需替换为真实数据）
| 方法 | RX1 | RX2 | RX3 | RX4 | RX5 | RX6 | RX7 | RX8 |
|------|-----|-----|-----|-----|-----|-----|-----|-----|
| 组1 WiResP | 92.1 | 90.5 | 89.3 | 91.0 | 88.7 | 95.2 | 91.8 | - |
| 组1 SMARS | 75.3 | 73.8 | 71.2 | 74.5 | 70.1 | 85.6 | 76.4 | - |
| 组1 FarSense | 80.5 | 78.9 | 76.4 | 79.8 | 75.3 | 90.1 | 82.0 | - |
| 组2 WiResP | 86.3 | 88.1 | 82.5 | 83.7 | 81.2 | 89.4 | 84.6 | 87.9 |
| 组2 SMARS | 61.5 | 64.2 | 58.7 | 60.1 | 57.3 | 70.5 | 62.8 | 65.1 |
| 组2 FarSense | 70.2 | 72.8 | 66.4 | 68.9 | 65.5 | 78.3 | 71.0 | 73.6 |

为了进一步评估基于三种算法计算的RR的睡眠监测性能，图10和图11显示了组1（绿色RX_{1,6}^1、黄色RX_{1,2}^1、粉色RX_{1,3}^1）和组2（绿色RX_{2,1}^1、黄色RX_{2,8}^1、粉色RX_{2,6}^1）中几个选定位置的睡眠阶段比较，这些位置代表了不同呼吸检测能力的场景。WiResP的RX_{1,6}^1和WiResP的RX_{2,1}^1分别根据其鲁棒的RR检测率被选为组1和组2的参考睡眠阶段。在组1中，SMARS和FarSense在RX_{1,2}^1和RX_{1,3}^1处表现不佳，导致对NREM和REM阶段的分类不准确。在组2中，所有三种方法在RX_{2,8}^1处的性能都与参考相当，而在RX_{2,6}^1处，只有WiResP保持了与参考几乎相同的睡眠阶段分析性能。这证实了WiResP能够以更高的检测率实现可靠的睡眠阶段分析，即使在具有挑战性的部署中也是如此。

![图10: 组1选定位置的睡眠阶段比较。](placeholder)

![图11: 组2选定位置的睡眠阶段比较。](placeholder)

### D. 不同窗口大小T_e的效果
图12显示了WiResP在组2中针对三个案例在不同T_e下的检测率。随着T_e的增加，WiResP的检测率在三个案例中逐渐提高，因为更长的窗口提供了更多的时间信息来增强呼吸信号。此外，案例1和案例2的检测率在T_e=40s后趋于饱和，而案例3的检测率在T_e=50s后仍略有提高，这可能是由于在案例3中测试者与设备之间的距离更大，导致信号更弱，需要更长的时间窗口来累积足够的呼吸周期以进行可靠的检测。

![图12: 不同窗口大小T_e对检测率的影响。](placeholder)

### E. 频谱增强的改进
图13展示了组2中不同RX位置通过频谱增强实现的详细改进。改进程度因位置而异。RX#6相比其他位置显示出轻微的改进，因为它安装在天花板中心位置，提供了对测试者更均匀的覆盖。从RX#3到RX#5的改进是显著的。值得注意的是，子载波选择和合并方法实际上捕获了呼吸模式，但SNR仍然太低，无法检测到该模式，特别是当测试者距离链路或设备相当远时。WiResP执行第二阶段增强，即频谱增强，以显著增强模式和SNR，从而提高检测率。

![图13: 组2中每个位置的改进。](placeholder)

### F. 通过轨迹连续性检查减少误报
图14展示了组2中8个RX位置在所有案例下的总体误报率图表，比较了三种方法，并与未进行轨迹连续性检查的WiResP的误报率进行了对比。如果没有连续性检查模块，WiResP的误报率约为10%，因为增强模块同时也增强了噪声，导致更多误报。然而，当加入轨迹连续性检查模块后，误报率降至低于3%。很明显，FarSense由于除法运算放大了一些噪声，产生了一些误报。相反，SMARS的误报最小，因为其可检测性相比其他方法较弱。

![图14: 组2中三个案例的误报率。](placeholder)

### G. 呼吸频率估计的精度
图15和图16分别描绘了在BNR≈3和BNR≈10时三种算法的RR估计精度。使用鼻罩[51]作为瞬时RR参考。当BNR较低时（图15），SMARS和FarSense都难以检测到呼吸信号，导致准确性差。相比之下，WiResP在BNR较低时能持续检测并准确估计呼吸频率，中位误差约为0.92 BPM，90th百分位误差为2.82 BPM。当BNR较高时（图16），三种方法都能够检测和估计RR。WiResP的中位误差和90th百分位误差仅为0.78 BPM和2.49 BPM。WiResP在90th百分位误差（约提高0.1/1.2 BPM）和中位误差（约提高0.1/0.28 BPM）方面优于其他两个系统。

![图15: RR估计的精度 (BNR ≈ 3)。](placeholder)

![图16: RR估计的精度 (BNR ≈ 10)。](placeholder)

## VII. 讨论与未来工作
本节讨论所提出系统的某些局限性以及未来研究的潜在方向。

### A. 复杂度降低
对于实际实现来说，降低复杂度总是必要且重要的。尽管所提出的系统能够扩大感知范围并允许设备灵活部署，但良好的性能自然是以增加复杂度为代价的。我们讨论了系统中各模块的开销，并给出了一些降低复杂度的潜在解决方案。

- **候选/子载波选择:** 在WiResP中，每个子载波都经过傅里叶变换以计算BNR进行选择，给定FFT大小N，一次呼吸估计的计算复杂度为O(N_t N_s N_r N log N)。随着更多天线和更大带宽，开销会增加。为了降低复杂度，如果受试者的位置、方向、姿势和环境保持稳定，则可能不需要对每次估计都进行子载波选择。选择可以基于诸如显著运动或环境变化等标准。另一种方法是选择单个最灵敏的TX和RX天线组合用于呼吸估计。
- **直方图均衡器:** 目前，即使原始频谱提供了清晰的呼吸模式，也会应用直方图均衡化，这可能导致不必要的复杂度。可以考虑一个决策算法，基于呼吸轨迹的连续性和评估原始频谱质量的窗口大小，来确定是否需要直方图均衡化。

### B. 多目标呼吸估计
虽然我们提出的系统专注于单个人的呼吸检测和估计，但它可以扩展到感知房间内的多个目标。然而，从向各个方向反射的无线电信号中分离出多个呼吸信号可能是一项艰巨的任务。此外，如果多个目标的呼吸频率重叠，任务将变得更加具有挑战性。一种可行的方法是利用信号分解技术；例如，经验模态分解 (Empirical Mode Decomposition, EMD)[52] 和独立成分分析 (Independent Component Analysis, ICA)[49]，这些技术已在多个领域中使用。我们将此作为未来工作的一个方向。

### C. ACF谱连续性识别
所提出的用于检查呼吸轨迹连续性的模块通过预定阈值（例如窗口大小T_w、脉冲数/宽度以及参考的一维呼吸模式）显著减少了误报。然而，根据特定的应用要求和环境，可能需要进一步的微调过程。未来的研究可以采用基于学习的分类器，利用来自不同案例的大量ACF谱图像，包括空环境和静止环境，以及具有不同呼吸模式的睡眠测试者环境，在各种睡眠姿势下，以减少漏报和误报。

## VIII. 结论
本文提出了WiResP，一种创新的呼吸监测系统，采用商用WiFi芯片组，适用于实际应用。通过利用频谱增强，WiResP增强了ACF谱内的对比度，提高了感知和估计呼吸频率的能力。WiResP在检测率和准确性方面超越了现有最先进的系统，无需额外的硬件或部署要求即可实现家庭内的呼吸频率监测。我们的方法代表了一种新颖的图像处理视角来应对呼吸跟踪的挑战，并有望扩展到其他基于WiFi的感知领域，将检测率提高超过25%，同时误报率低于5%。我们相信WiResP极大地简化了设备的设置，拓宽了感知覆盖范围，弥合了实验室研究与设备实际部署之间的差距。未来的工作包括在多样化的家庭环境中测试WiResP，通过智能选择天线或子载波子集来降低复杂度，以及结合基于学习的方法以进一步减少误报。

## 参考文献
[1] M. Baumert et al., "Mean nocturnal respiratory rate predicts cardiovascular and all-cause mortality in community-dwelling older men and women," *European Respiratory Journal*, vol. 54, no. 1, 2019.  
[2] J. W. Kantelhardt, T. Penzel, S. Rostig, H. F. Becker, S. Havlin, and A. Bunde, "Breathing during rem and non-rem sleep: correlated versus uncorrelated behaviour," *Physica A: Statistical Mechanics and its Applications*, vol. 319, pp. 447-457, 2003.  
[3] C. Massaroni, A. Nicolò, D. Presti, M. Sacchetti, S. Silvestri, and E. Schena, "Contact-based methods for measuring respiratory rate," *Sensors*, vol. 19, p. 908, 02 2019.  
[4] D. F. Al-khalidi, R. Saatchi, D. Burke, H. Elphick, and S. Tan, "Respiration rate monitoring methods: A review," *Pediatric Pulmonology*, vol. 46, pp. 523 - 529, 06 2011.  
[5] Y. Su, G. Chen, C. Chen, Q. Gong, G. Xie, M. Yao, H. Tai, Y. Jiang, and J. Chen, "Self-powered respiration monitoring enabled by a triboelectric nanogenerator," *Advanced Materials*, vol. 33, no. 35, p. 2101262, 2021.  
[6] Y.-C. Lee, A. Syakura, M. A. Khalil, C.-H. Wu, Y.-F. Ding and C.-W. Wang, "A real-time camera-based adaptive breathing monitoring system," *Medical Biological Engineering Computing*, no. 08, pp. 1285-1298, 2021.  
[7] I. Lorato, S. Stuijk, M. Meftah, D. Kommers, P. Andriessen, C. van Pul, and G. de Haan, "Towards continuous camera-based respiration monitoring in infants," *Sensors*, vol. 21, no. 7, 2021.  
[8] W. L. et al., "Feasibility of non-contact cardiorespiratory monitoring using impulse-radio ultra-wideband radar in the neonatal intensive care unit," *PloS one*, 12 2020.  
[9] A.-K. Moadi, M. Joshi, O. Kilic, and A. E. Fathy, "Low cost ir-uwb radar for multisubject non-contact vital sign detection," in *2021 IEEE International Symposium on Antennas and Propagation and USNC-URSI Radio Science Meeting (APS/URSI)*, 2021, pp. 135-136.  
[10] P. Kontou, S. Ben Smida, S. Nektarios Daskalakis, S. Nikolaou, M. Dragone, and D. E. Anagnostou, "Heartbeat and respiration detection using a low complexity cw radar system," in *2020 50th European Microwave Conference (EuMC)*, 2021, pp. 929-932.  
[11] Y. E. Acar, I. Saritas, and E. Yaldiz, "An experimental study: Detecting the respiration rates of multiple stationary human targets by stepped frequency continuous wave radar," *Measurement*, vol. 167, p. 108268, 2021.  
[12] F. Wang, X. Zeng, C. Wu, B. Wang, and K. J. R. Liu, "Driver vital signs monitoring using millimeter wave radio," *IEEE Internet of Things Journal*, vol. 9, no. 13, pp. 11283-11298, 2022.  
[13] F. Wang, F. Zhang, C. Wu, B. Wang, and K. J. R. Liu, "Vimo: Multiperson vital sign monitoring using commodity millimeter-wave radio," *IEEE Internet of Things Journal*, vol. 8, no. 3, pp. 1294-1307, 2021.  
[14] K. J. R. Liu and B. Wang, *Wireless AI: Wireless Sensing, Positioning, IoT, and Communications*. Cambridge University Press, 2019.  
[15] B. Wang, Q. Xu, C. Chen, F. Zhang, and K. R. Liu, "The promise of radio analytics: A future paradigm of wireless positioning, tracking, and sensing," *IEEE Signal Processing Magazine*, vol. 35, no. 3, pp. 59-80, 2018.  
[16] F. Zhang, C. Chen, B. Wang, and K. J. R. Liu, "Wiseped: A statistical electromagnetic approach for device-free indoor speed estimation," *IEEE Internet of Things Journal*, vol. 5, no. 3, pp. 2163-2177, 2018.  
[17] C. Wu, B. Wang, O. C. Au, and K. R. Liu, "Wi-fi can do more: Toward ubiquitous wireless sensing," *IEEE Communications Standards Magazine*, vol. 6, no. 2, pp. 42-49, 2022.  
[18] K. J. R. Liu and B. Wang, "Statistical principles of time reversal," *IEEE Signal Processing Magazine*, vol. 41, no. 1, pp. 31-37, 2024.  
[19] F. Zhang, C. Chen, B. Wang, H.-Q. Lai, Y. Han, and K. J. R. Liu, "Wiball: A time-reversal focusing ball method for decimeter-accuracy indoor tracking," *IEEE Internet of Things Journal*, vol. 5, no. 5, pp. 4031-4041, 2018.  
[20] C. Chen, Y. Chen, Y. Han, H.-Q. Lai, F. Zhang, and K. J. R. Liu, "Achieving centimeter-accuracy indoor localization on wifi platforms: A multi-antenna approach," *IEEE Internet of Things Journal*, vol. 4, no. 1, pp. 122-134, 2017.  
[21] M. Abbas, M. Elhamshary, H. Rizk, M. Torki, and M. Youssef, "Wideep: Wifi-based accurate and robust indoor localization system using deep learning," in *2019 IEEE International Conference on Pervasive Computing and Communications (PerCom)*, 2019, pp. 1-10.  
[22] F. Zhang, C. Wu, B. Wang, H.-Q. Lai, Y. Han, and K. J. R. Liu, "Widelect: Robust motion detection with a statistical electromagnetic model," *Proc. ACM Interact. Mob. Wearable Ubiquitous Technol.*, vol. 3, no. 3, sep 2019.  
[23] G. Zhu, C. Wu, X. Zeng, B. Wang, and K. J. R. Liu, "Who moved my cheese? human and non-human motion recognition with wifi," in *2022 IEEE 19th International Conference on Mobile Ad Hoc and Smart Systems (MASS)*, 2022, pp. 476-484.  
[24] G. Zhu, Y. Hu, B. Wang, C. Wu, X. Zeng, and K. J. R. Liu, "Wimoid: Human and nonhuman motion discrimination using wifi with edge computing," *IEEE Internet of Things Journal*, vol. 11, no. 8, pp. 13900-13912, 2024.  
[25] X. Wang, C. Yang, and S. Mao, "Phasebeat: Exploiting csi phase data for vital sign monitoring with commodity wifi devices," in *2017 IEEE 37th International Conference on Distributed Computing Systems (ICDCS)*, 2017, pp. 1230-1239.  
[26] C. Dou and H. Huan, "Full respiration rate monitoring exploiting doppler information with commodity wi-fi devices," *Sensors*, vol. 21, no. 10, 2021.  
[27] Y. Gu, X. Zhang, Z. Liu, and F. Ren, "Wifi-based real-time breathing and heart rate monitoring during sleep," in *2019 IEEE Global Communications Conference (GLOBECOM)*, 2019, pp. 1-6.  
[28] K. Chouroung and P. Mangkalakeere, "Wireless heart rate monitoring system using mqtt," *Procedia Computer Science*, vol. 86, pp. 160-163, 2016, 2016 International Electrical Engineering Congress, iEECON2016, 2-4 March 2016, Chiang Mai, Thailand.  
[29] P. Wang, B. Guo, T. Xin, Z. Wang, and Z. Yu, "Tinysense: Multi-user respiration detection using wi-fi csi signals," in *2017 IEEE 19th International Conference on e-Health Networking, Applications and Services (Healthcom)*, 2017, pp. 1-6.  
[30] J. Liu et al, "Tracking vital signs during sleep leveraging off-the-shelf wifi," in *Proceedings of the 16th ACM International Symposium on Mobile Ad Hoc Networking and Computing*, ser. MobiHoc '15. NY, USA: Association for Computing Machinery, 2015, p. 267-276.  
[31] X. Liu, J. Cao, S. Tang, and J. Wen, "Wi-sleep: Contactless sleep monitoring via wifi signals," in *2014 IEEE Real-Time Systems Symposium*, 2014, pp. 346-355.  
[32] H. Abdelnasser, M. Youssef, and K. A. Harras, "Wigest: A ubiquitous wifi-based gesture recognition system," in *2015 IEEE Conference on Computer Communications (INFOCOM)*, 2015, pp. 1472-1480.  
[33] C. Chen, Y. Han, Y. Chen, H.-Q. Lai, F. Zhang, B. Wang, and K. J. R. Liu, "Tr-breath: Time-reversal breathing rate estimation and detection," *IEEE Transactions on Biomedical Engineering*, vol. 65, no. 3, pp. 489-501, 2018.  
[34] X. Wang, C. Yang, and S. Mao, "Tensorbeat: Tensor decomposition for monitoring multiperson breathing beats with commodity wifi," *ACM Trans. Intell. Syst. Technol.*, vol. 9, no. 1, sep 2017.  
[35] H. Abdelnasser, K. A. Harras, and M. Youssef, "Ubiheartbeat: A ubiquitous non-invasive wifi-based breathing estimator," *Proceedings of the 16th ACM International Symposium on Mobile Ad Hoc Networking and Computing*, 2015.  
[36] N. Patwari, J. Wilson, S. Ananthanarayanan, S. K. Kasera, and D. R. Westenskow, "Monitoring breathing via signal strength in wireless networks," *IEEE Transactions on Mobile Computing*, vol. 13, no. 08, pp. 1774-1786, aug 2014.  
[37] H. Yigitler, O. Kaltiokallio, R. Hostettler, A. S. Abrar, R. Jantti, N. Patwari, and S. Sarkka, "Rss models for respiration rate monitoring," *IEEE Transactions on Mobile Computing*, vol. 19, no. 3, pp. 680-696, 2020.  
[38] X. Liu, J. Cao, S. Tang, J. Wen, and P. Guo, "Contactless respiration monitoring via off-the-shelf wifi devices," *IEEE Transactions on Mobile Computing*, vol. 15, no. 10, pp. 2466-2479, 2016.  
[39] Y. Zeng, D. Wu, J. Xiong, E. Yi, R. Gao, and D. Zhang, "Farsense: Pushing the range limit of wifi-based respiration sensing with csi ratio of two antennas," *Proc. ACM Interact. Mob. Wearable Ubiquitous Technol.*, vol. 3, no. 3, sep 2019.  
[40] H. Wang, D. Zhang, J. Ma, Y. Wang, Y. Wang, D. Wu, T. Gu, and B. Xie, "Human respiration detection with commodity wifi devices: Do user location and body orientation matter?" in *Proceedings of the 2016 ACM International Joint Conference on Pervasive and Ubiquitous Computing*, ser. UbiComp '16. New York, NY, USA: Association for Computing Machinery, 2016, p. 25-36.  
[41] J. Hu, J. Yang, J.-B. Ong, D. Wang, and L. Xie, "Resfi: Wifi-enabled device-free respiration detection based on deep learning," in *2022 IEEE 17th International Conference on Control Automation (ICCA)*, 2022, pp. 510-515.  
[42] U. M. Khan, Z. Kabir, S. A. Hassan, and S. H. Ahmed, "A deep learning framework using passive wifi sensing for respiration monitoring," in *GLOBECOM 2017 - 2017 IEEE Global Communications Conference*, 2017, pp. 1-6.  
[43] S. Mosleh, J. B. Coder, C. G. Scully, K. Forsyth, and M. O. A. Kalaa, "Monitoring respiratory motion with wi-fi csi: Characterizing performance and the breathsamet algorithm," *IEEE Access*, vol. 10, pp. 131932-131951, 2022.  
[44] S. S. Jayaweera, B. Wang, X. Zeng, W.-H. Wang, and K. J. Ray Liu, "Wifi-based robust child presence detection for smart cars," in *ICASSP 2023 - 2023 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)*, 2023, pp. 1-5.  
[45] F. Wang, F. Zhang, C. Wu, B. Wang, and K. J. R. Liu, "Respiration tracking for people counting and recognition," *IEEE Internet of Things Journal*, vol. 7, no. 6, pp. 5233-5245, 2020.  
[46] F. Zhang, C. Wu, B. Wang, M. Wu, D. Bugos, H. Zhang, and K. J. R. Liu, "Smars: Sleep monitoring via ambient radio signals," *IEEE Transactions on Mobile Computing*, vol. 20, no. 1, pp. 217-231, 2019.  
[47] W.-H. Wang, X. Zeng, B. Wang, Y. Cao, and K. J. Ray Liu, "Improved wifi-based respiration tracking via contrast enhancement," in *ICASSP 2023 - 2023 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)*, 2023, pp. 1-5.  
[48] X. Zeng, B. Wang, C. Wu, S. D. Regani, and K. J. R. Liu, "Wicpd: Wireless child presence detection system for smart cars," *IEEE Internet of Things Journal*, pp. 1-1, 2022.  
[49] S. Yue, H. He, H. Wang, H. Rahul, and D. Katabi, "Extracting multiperson respiration from entangled rf signals," *Proc. ACM Interact. Mob. Wearable Ubiquitous Technol.*, vol. 2, no. 2, jul 2018.  
[50] N. Otsu, "A threshold selection method from gray-level histograms," *IEEE Transactions on Systems, Man, and Cybernetics*, vol. 9, no. 1, pp. 62-66, 1979.  
[51] S. Circle. Sleepbreath. [Online]. Available: https://snorecircle.com/blogs/news/sleepbreath-pre-launch  
[52] A. Garde, W. Karlen, P. Dehkordi, J. Ansermino, and G. Dumont, "Empirical mode decomposition for respiratory and heart rate estimation from the photoplethysmogram," in *Computing in Cardiology 2013*, pp. 799-802.