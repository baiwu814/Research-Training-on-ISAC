# WiSpeed：一种用于无设备室内速度估计的统计电磁方法

Feng Zhang\*†, Chen Chen\*†, Beibei Wang\*†, and K. J. Ray Liu\*†

\*美国马里兰大学帕克分校，马里兰州学院市 20742。 †Origin Wireless, Inc.，7500 Greenway Center Drive，马里兰州 20770。 \*电子邮件：{fzhang15, cc8834, bewang, kjrliu}@umd.edu

**摘要-** 由于严重的多径效应，尤其是在非视距场景下，源与观测者之间的直达路径被阻挡，目前尚无令人满意的无设备室内速度估计方法。本文提出 WiSpeed，一种通用的低复杂度室内速度估计系统，利用无线电信号（如商用 WiFi、LTE、5G 等），可在无设备和有设备两种情况下工作。通过利用电磁波的统计理论，我们建立了物理层信道状态信息的自相关函数与运动物体速度之间的联系，为 WiSpeed 奠定了理论基础。WiSpeed 与其他需要源与观测者之间强视距条件的方案不同，它利用室内典型的多径丰富环境来实现高精度的速度估计。此外，作为一种免校准系统，WiSpeed 免去了用户进行大规模训练和系统参数精细调整的工作。同时，WiSpeed 还能提取步幅长度并检测异常活动（如跌倒），跌倒是老年人面临的主要威胁，每年导致大量死亡。大量实验表明，WiSpeed 在无设备人体行走速度估计中的平均绝对百分比误差为 $4.85\%$，在有设备速度估计中为 $4.62\%$，跌倒检测的检测率达到 $95\%$ 且无虚警。

## I. 引言

如今人们越来越多的时间在室内度过，理解他们日常的室内活动将成为未来生活的必需品。由于人体速度是能够表征人类活动类型的关键物理参数之一，人体运动的速度估计是人类活动监测系统中的关键模块。与传统的基于可穿戴传感器的方法相比，无设备速度估计因其更好的用户体验而更具前景，可广泛应用于智能家居 [1]、医疗保健 [2]、健身追踪 [3] 和娱乐等领域。

然而，室内无设备速度估计极具挑战性，主要由于信号的严重多径传播以及监测设备与被监测物体之间的遮挡。传统的运动传感方法需要专用设备，从雷达、声纳、激光到摄像头。其中，基于视觉的方案 [4] 只能在视野范围内进行运动监测，且在光线昏暗条件下性能下降，同时还会带来隐私问题。同时，雷达或声纳 [5] 产生的速度估计会因运动方向不同而变化，这主要是因为速度估计是从多普勒频移导出的，而多普勒频移与物体的运动方向相关。此外，室内空间的多径传播进一步削弱了雷达和声纳的效果。

最近，WiGait [6] 和 WiDar [7] 被提出用于利用无线电信号测量室内步态速度和步幅长度。然而，WiGait 使用专用硬件发送调频载波探测信号，并且需要高达 $1.69\mathrm{GHz}$ 的带宽来分辨多径分量。另一方面，WiDar 只能在强视距条件和密集部署 WiFi 设备的情况下良好工作，因为其性能严重依赖于射线追踪/几何技术的精度。

本文提出 WiSpeed，一种鲁棒的通用人体运动速度估计器，适用于多径丰富的室内环境，能够在无设备或有设备条件下估计运动物体的速度。WiSpeed 实际上是一个基本原理，不需要特定硬件，仅需一对商用现成 WiFi 设备即可。首先，我们利用电磁波的统计理论，描述了运动对电磁波接收电场自相关函数的影响。然而，接收电场是一个矢量，不易直接测量。因此，我们进一步推导了接收电场功率的自相关函数与运动速度之间的关系，因为电场功率可以在商用 WiFi 设备上直接测量 [8]。通过分析自相关函数的不同分量，我们发现自相关函数微分的第一个局部峰值包含了关键的速度信息，并提出了一种新颖的峰值识别算法来提取速度。此外，步数和步幅长度可以作为速度估计的副产品来估计。同时，可以从速度估计的模式中检测跌倒。

为了评估 WiSpeed 的性能，我们在两个场景中进行了大量实验：人体行走监测和人体跌倒检测。对于人体行走监测，通过将估计的行走距离与真实距离进行比较来评估 WiSpeed 的准确性。实验结果表明，当人不携带设备时，WiSpeed 的平均绝对百分比误差为 $4.85\%$；当受试者携带设备时，平均绝对百分比误差为 $4.62\%$。此外，WiSpeed 可以从无设备设置下的速度估计模式中提取步幅长度并估计步数。对于人体跌倒检测，WiSpeed 能够将跌倒与其他正常活动（如坐下、站起、捡起物品和行走）区分开。平均检测率为 $95\%$，无虚警。据我们所知，WiSpeed 是第一个同时实现高估计精度、高检测率、低部署成本、大覆盖范围、低计算复杂度和隐私保护的无设备/有设备无线运动速度估计器。

由于 WiFi 基础设施在大多数室内空间已 readily available，WiSpeed 是一种可广泛部署的低成本解决方案。WiSpeed 将支持大量重要的室内应用，例如：

1.  **室内健身追踪：** 越来越多的人关注自己的身体状况，并希望了解每日的运动量。WiSpeed 可以通过速度估计的模式来估计步数，从而评估一个人的运动量。借助 WiSpeed，人们无需佩戴任何可穿戴传感器即可获取运动量并评估个人健康状况。

2.  **室内导航：** 尽管室外实时追踪已通过 GPS 成功解决，但室内追踪至今仍是一个开放问题。基于航位推算的方法是现有流行的室内导航技术之一，它基于速度和运动方向的测量，从参考点开始计算位置。然而，其精度主要受限于基于惯性测量单元的移动距离估计。由于 WiSpeed 也能测量移动 WiFi 设备的速度，通过结合 WiSpeed，可以显著提高基于航位推算的系统中距离估计模块的精度。

3.  **跌倒检测：** 对于独居老人而言，实时监测人体运动的速度至关重要，因为系统可以检测到对他们的生命构成主要威胁的跌倒。

4.  **家庭安防：** WiSpeed 可以在家庭安防系统中发挥重要作用，因为它能够通过不同的移动速度模式区分入侵者和主人的宠物，并立即通知主人和执法部门。

本文的其余部分组织如下。第二节总结了关于使用 WiFi 信号进行人体活动识别的相关工作。第三节介绍了空腔中电磁波的统计理论及其在无线运动传感中的扩展。第四节阐述了 WiSpeed 的基本原理，第五节展示了 WiSpeed 的详细设计。第六节展示了实验评估。第七节讨论了参数选择和 WiSpeed 的计算复杂度，第八节总结全文。

## II. 相关工作

现有的使用商用 WiFi 进行无设备运动传感技术的工作包括手势识别 [9], [10], [11], [12], [13]、人体活动识别 [14], [15], [16]、运动追踪 [17], [18]、被动定位 [7], [19]、生命信号估计 [20]、室内事件检测 [21] 等。这些方法基于一个现象：人体运动不可避免地会扭曲 WiFi 信号，并可以被 WiFi 接收器记录下来以供进一步分析。从原理上讲，这些工作可以分为两类：基于学习的和基于射线追踪的。下面详细阐述这两类方法。

*   **基于学习的方法：** 这些方案包括两个阶段：离线阶段和在线阶段。在离线阶段，从 WiFi 信号中提取与不同人体活动相关的特征并存储在数据库中；在线阶段，从实时 WiFi 信号中提取相同特征集，并与存储的特征进行比较，从而分类人体活动。特征可以从 CSI 或接收信号强度指示器（RSSI）中获取，RSSI 是一种 readily available 但粒度较粗的信息，包含 WiFi 信号的接收功率。例如，E-eyes [14] 利用 CSI 幅度的直方图来识别日常活动，如洗碗和刷牙。CARM [15] 利用 CSI 动态频谱分量的特征来区分人体活动。WiGest [9] 利用 RSSI 变化的特征进行手势识别。

基于学习方法的一个主要缺点在于，这些工作利用运动速度来识别不同活动，但它们只获取与速度相关的特征，而不是直接测量速度。例如，多普勒频移不仅取决于运动速度，还取决于物体的反射角。因此，这些特征容易受到外部因素的影响，如环境变化、人体受试者的异质性、设备位置的变化等，这可能违反它们在离线阶段和在线阶段特征可复现性的潜在假设。

*   **基于射线追踪的方法：** 根据所采用的技术，它们可以分为多径避免和多径衰减。多径避免方案仅追踪由人体反射的多径分量，并避免其他多径分量。它们使用高时间分辨率 [22] 或“虚拟”相控天线阵列 [18]，使得与运动相关的多径分量可以在时域或空间域中与不相关的多径分量区分开。这些方法的缺点是要求专用硬件，如 USRP、WARP [23] 等，以实现精细的时间和空间分辨率，这在 WiFi 设备上不可用。

在多径衰减方案中，通过将 WiFi 设备放置在监测对象的近邻，使得大多数多径分量受到对象的影响，从而衰减多径分量的影响 [7], [10], [17]。其缺点是需要非常强的视距工作条件，这限制了它们在实际中的部署。

WiSpeed 与现有文献中的最先进技术不同之处如下：

*   WiSpeed 拥抱室内多径传播，并且能够在严重的非视距条件下生存和发展，而不是试图消除多径效应 [7], [10], [18], [22]。
*   WiSpeed 利用与运动速度相关的电磁波物理特征，直接估计运动速度，无需迂回。由于这些物理特征适用于不同的室内环境和人体受试者，WiSpeed 能够在环境和受试者变化时仍表现良好，并且无需任何训练或校准。
*   WiSpeed 在计算复杂度方面具有优势，因为它不需要像主成分分析、离散小波变换和短时傅里叶变换 [7], [11], [15] 等高成本操作。
*   WiSpeed 是一种低成本解决方案，因为它只部署一对商用 WiFi 设备，而 [6], [7], [12], [17], [22] 需要专用硬件或多对 WiFi 设备。

## III. 用于无线运动传感的电磁波统计理论

在本节中，我们首先将接收器处的接收电场分解为不同分量，然后在某些统计假设下分析每个分量的统计行为。

### A. 接收电场的分解

为了深入理解运动对电磁波的影响，我们考虑一个如图 1a 所示的多径丰富环境，这是室内空间的典型场景。假设散射体是扩散的，能够将入射电磁波向各个方向反射。环境中部署了一个发射器和一个接收器，两者都配备全向天线。发射器通过其天线发射连续电磁波，由接收器接收。在室内环境或混响室中，电磁波通常近似为平面波，可以用其电场完全表征。令 $\vec{E}_{Rx}(t,f)$ 表示接收器在时间 $t$ 接收到的电场，其中 $f$ 是发射电磁波的频率。为了分析接收电场的行为，我们基于电场的叠加原理将 $\vec{E}_{Rx}(t,f)$ 分解为不同散射体贡献的电场之和

$$\vec{E}_{Rx}(t,f) = \sum_{i\in \Omega_s(t)}\vec{E}_i(t,f) + \sum_{j\in \Omega_d(t)}\vec{E}_j(t,f) \quad (1)$$

其中 $\Omega_{s}(t)$ 和 $\Omega_{d}(t)$ 分别表示静态散射体和动态（移动）散射体的集合，$\vec{E}_i(t,f)$ 表示由第 $i$ 个散射体散射的接收电场部分。这种分解的直观理解是，每个散射体可以被视为一个“虚拟天线”，将接收到的电磁波向各个方向扩散，然后这些电磁波在从建筑物的墙壁、天花板、窗户等反射后在接收天线处叠加。当发射天线静止时，它可以被视为一个“特殊的”静态散射体，即 $Tx \in \Omega_s(t)$；当它移动时，它可以归类到动态散射体集合中，即 $Tx \in \Omega_d(t)$。$\vec{E}_{Tx}(t,f)$ 的功率远大于散射体散射的电场功率。

在一个足够短的时间内，可以合理地假设集合 $\Omega_s(t)$、$\Omega_d(t)$ 以及电场 $\vec{E}_i(t,f)$（$i \in \Omega_s(t)$）随时间变化缓慢。那么，我们有如下近似：

$$\vec{E}_{Rx}(t,f) \approx \vec{E}_s(f) + \sum_{j \in \Omega_d} \vec{E}_j(t,f), \quad (2)$$

$$\vec{E}_s(f) \approx \sum_{i \in \Omega_s(t)} \vec{E}_i(t,f).$$

### B. 接收电场的统计行为

根据信道互易性，双向传播的电磁波将经历相同的物理扰动（即反射、折射、衍射等）。因此，如果接收器发射电磁波，所有散射体将接收到与它们贡献给 $\vec{E}_{Rx}(t,f)$ 相同的电场，如图 1b 所示。因此，为了理解 $\vec{E}_{Rx}(t,f)$ 的性质，我们只需要分析其各个分量 $\vec{E}_i(t,f)$，它等于如果接收器发射时第 $i$ 个散射体接收到的电场。那么，$\vec{E}_i(t,f)$ 可以解释为所有方向角上的平面波的积分，如图 2 所示。对于每个入射方向角为 $\Theta = (\alpha , \beta)$ 的平面波，其中 $\alpha$ 和 $\beta$ 分别表示仰角和方位角，令 $\vec{k}$ 表示其波矢，$\vec{F}(\Theta)$ 表示其角谱，表征波的电场。波矢 $\vec{k}$ 由 $- k(\hat{x} \sin (\alpha) \cos (\beta) + \hat{y} \sin (\alpha) \sin (\beta) + \hat{z} \cos (\alpha))$ 给出，

---

**图 1：具有许多散射体的波传播示意图。**

---

**图 2：具有波矢 $\vec{k}$ 的电场平面波分量 $\vec{F}(\Theta)$。**

其中对应的自由空间波数为 $k = \frac{2\pi f}{c}$，$c$ 为光速。角谱 $\vec{F}(\Theta)$ 可以写成 $\vec{F}(\Theta) = F_{\alpha}(\Theta)\hat{\alpha} + F_{\beta}(\Theta)\hat{\beta}$，其中 $F_{\alpha}(\Theta)$、$F_{\beta}(\Theta)$ 是复数，$\hat{\alpha}$、$\hat{\beta}$ 是彼此正交且与 $\vec{k}$ 正交的单位向量。如果第 $i$ 个散射体的速度为 $v_{i}$，那么 $\vec{E}_{i}(t, f)$ 可以表示为

$$\vec{E}_{i}(t,f) = \int_{0}^{2\pi f}\vec{F}(\Theta)\exp (-j\vec{k}\cdot \vec{v}_{i}t)\sin (\alpha)\mathrm{d}\alpha \mathrm{d}\beta , \quad (3)$$

其中 $z$ 轴与散射体 $i$ 的运动方向对齐，如图 2 所示，并且时间依赖性 $\exp (- j2\pi ft)$ 被省略，因为它不影响后续推导的任何结果。角谱 $\vec{F}(\Theta)$ 可以是确定性的或随机的。方程 (3) 中的电场满足麦克斯韦方程组，因为每个平面波分量都满足麦克斯韦方程组 [24]。

建筑物内部的无线电传播通常很难分析，因为电磁波可能被墙壁、门、窗户、移动物体等吸收和散射。然而，建筑物和房间可以视为混响腔，因为它们表现出内部多径传播。因此，我们采用统计建模而非确定性建模，并将为混响腔开发的电磁场统计理论应用于分析 $\vec{E}_{i}(t, f)$ 的统计特性。我们假设 $\vec{E}_{i}(t, f)$ 是大量平面波的叠加，这些平面波具有均匀分布的到达方向、极化和相位，这可以很好地捕捉混响腔波函数的特性 [24]。因此，我们将 $\vec{F}(\Theta)$ 视为随机变量，并对 $\vec{F}(\Theta)$ 做出以下统计假设：

**假设 1.** 对于 $\forall \Theta$，$F_{\alpha}(\Theta)$ 和 $F_{\beta}(\Theta)$ 都是具有相同方差的循环对称高斯随机变量 [25]，并且它们统计独立。

**假设 2.** 对于每个动态散射体，来自不同方向的角谱分量是不相关的。

**假设 3.** 对于任意两个动态散射体 $i_{1}$、$i_{2} \in \Omega_{d}$，对于 $\forall t_{1}, t_{2}$，$\vec{E}_{i_{1}}(t_{1}, f)$ 和 $\vec{E}_{i_{2}}(t_{2}, f)$ 是不相关的。

假设 1 是因为角谱是许多具有随机相位的射线或反射的结果，因此可以假设在中心极限定理下，$\vec{F}(\Theta)$ 的每个正交分量趋向于高斯分布。假设 2 是因为对应于不同方向的角谱分量经历了非常不同的多重散射路径，因此可以假设它们彼此不相关。假设 3 是由于相隔至少半波长的两个位置的信道响应在统计上不相关 [26][27]，因此可以假设不同散射体贡献的电场是不相关的。

在这三个假设下，$\vec{E}_{i}(t, f)$，$\forall i \in \Omega_{d}$ 可以近似为时间上的平稳过程。定义电场 $\vec{E}(t, f)$ 的时间自相关函数为

$$\rho_{\vec{E}}(\tau ,f) = \frac{\langle\vec{E}(0,f),\vec{E}(\tau,f)\rangle}{\sqrt{\langle|\vec{E}(0,f)|^{2}\rangle\langle|\vec{E}(\tau,f)|^{2}\rangle}}, \quad (4)$$

其中 $\tau$ 是时间延迟，$\langle \rangle$ 表示对所有实现的系综平均，$\langle \vec{X}, \vec{Y} \rangle$ 表示 $\vec{X}$ 和 $\vec{Y}$ 的内积，即 $\langle \vec{X}, \vec{Y} \rangle \triangleq \langle \vec{X} \cdot \vec{Y}^{*} \rangle$，$^*$ 是复共轭算子，$\cdot$ 是点积，$|\vec{E}(t, f)|^{2}$ 表示电场幅值的平方。由于 $\vec{E}(t, f)$ 被假定为平稳过程，(4) 的分母退化为 $E^{2}(f)$，表示电场的功率，即 $E^{2}(f) = \langle |\vec{E}(t, f)|^{2} \rangle$，$\forall t$，并且自相关函数仅仅是自协方差函数的归一化对应物。

$$\begin{array}{rl} & {\langle \vec{E}_i(0,f)\cdot \vec{E}_i^* (\tau ,f)\rangle}\\ & {= \int_{4\pi}\int_{4\pi}\langle \vec{F} (\Theta_1)\cdot \vec{F} (\Theta_2)\rangle \exp (j\vec{k}_2\cdot \vec{v}_i\tau)\mathrm{d}\Theta_1\mathrm{d}\Theta_2}\\ & {= \frac{E_i^2(f)}{4\pi}\int_{4\pi}\exp (jk_i\tau \cos (\alpha_2))\mathrm{d}\Theta_2}\\ & {= E_i^2 (f)\frac{\sin (kv_i\tau)}{kv_i\tau},} \end{array} \quad (5)$$

其中我们定义 $\begin{array}{rlr}{\int_{4\pi}} & {\triangleq} & {\int_{0}^{2\pi}\int_{0}^{\pi}} \end{array}$ 和 $\begin{array}{rlr}{\mathrm{d}\Theta} & {\triangleq} & {\sin (\alpha)\mathrm{d}\alpha \mathrm{d}\beta} \end{array}$，$E_{i}^{2}(f)$ 是 $\vec{E}_{i}(t,f)$ 的功率。根据假设 3，$\vec{E}_{Rx}(t,f)$ 的自协方差函数可以写成

$$\begin{array}{rl} & {\left\langle (\vec{E}_{Rx}(0,f) - \vec{E}_s(f))\cdot (\vec{E}_{Rx}^* (\tau ,f) - \vec{E}_s^*(f))\right\rangle}\\ & {= \sum_{i\in \Omega_d}E_i^2 (f)\frac{\sin (kv_i\tau)}{kv_i\tau},} \end{array} \quad (6)$$

相应的自相关函数可以推导为

$$\rho_{\vec{E}_{Rx}}(\tau ,f) = \frac{1}{\sum_{j\in \Omega_d}E_j^2(f)\sum_{i\in \Omega_d}E_i^2(f)}\frac{\sin(kv_i\tau)}{kv_i\tau}. \quad (7)$$

从 (7) 可以看出，$\vec{E}_{Rx}$ 的自相关函数实际上是每个运动散射体的自相关函数按其辐射功率加权的组合，并且每个动态散射体的运动方向在自相关函数中不起作用。(7) 的重要性在于，动态散射体的速度信息实际上嵌入在接收电场的自相关函数中。

## IV. WISPEED 的理论基础

在第三节中，我们推导了接收器处接收电场的自相关函数，它取决于动态散射体的速度。如果所有或大部分动态散射体以相同速度 $v$ 运动，那么 (7) 的右边将退化为 $\rho_{\vec{E}_{Rx}}(\tau ,f) = \frac{\sin(kv\tau)}{kv\tau}$，从自相关函数估计共同速度就变得非常简单。然而，直接测量接收器处的电场并分析其自相关函数并不容易。相反，电场的功率可以视为等同于信道响应的功率，而信道响应可以用商用 WiFi 设备测量。在本节中，我们将讨论 WiSpeed 利用 CSI 功率响应的自相关函数进行速度估计的原理。

不失一般性，我们以基于 OFDM 的 WiFi 系统的信道响应为例。令 $X(t,f)$ 和 $Y(t,f)$ 分别为时间 $t$ 在频率为 $f$ 的子载波上发送和接收的信号。那么，对于频率为 $f$ 的子载波在时间 $t$ 测量的 CSI 的最小二乘估计器是 $H(t,f) = \frac{Y(t,f)}{X(t,f)}$ [28]。我们定义功率响应 $G(t,f)$ 为 CSI 幅度的平方，其形式为

$$G(t,f)\triangleq |H(t,f)|^{2} = \| \vec{E}_{Rx}(t,f)\|^{2} + \epsilon (t,f), \quad (8)$$

其中 $\| \vec{E}\| ^2$ 表示 $\vec{E}$ 的总功率，$\epsilon (t,f)$ 被假定为由于 CSI 测量不完美而产生的加性噪声。

可以假设噪声 $\epsilon (t,f)$ 服从正态分布。为了证明这一点，我们在静态室内环境中以信道采样率 $F_{s} = 30\mathrm{Hz}$ 收集了一小时的 CSI 数据。对于给定的子载波，归一化 $G(t,f)$ 的 Q-Q 图与标准正态分布的比较如图 3a 所示，这表明噪声的分布非常接近正态分布。为了验证噪声的白性，我们还研究了 $G(t,f)$ 的自相关函数，其定义为 [29] $\rho_{G}(\tau ,f) = \frac{\gamma_{G}(\tau,f)}{\gamma_{G}(0,f)}$，其中 $\gamma_{G}(\tau ,f)$ 表示自协方差函数，即 $\gamma_{G}(\tau ,f)\triangleq \mathrm{cov}(G(t,f),G(t - \tau ,f))$。在实践中，使用样本自协方差函数 $\hat{\gamma}_{G}(\tau ,f)$ 代替。如果 $\epsilon (t,f)$ 是白噪声，那么对于 $\forall \tau \neq 0$，样本自相关函数 $\hat{\rho}_{G}(\tau ,f)$ 可以近似为一个均值为零、标准差为 $\sigma_{\hat{\rho}_{G}(\tau ,f)} = \frac{1}{\sqrt{T}}$ 的正态随机变量。图 3b 显示了当使用第一个子载波上的 2000 个样本时 $G(t,f)$ 的样本自相关函数。从图中可以看出，样本自相关函数的所有抽头都在 $\pm 2\sigma_{\hat{\rho}_{G}(\tau ,f)}$ 的区间内，因此，可以假设 $\epsilon (t,f)$ 是加性高斯白噪声，即 $\epsilon (t,f)\sim \mathcal{N}(0,\sigma^{2}(f))$。

在第三节的先前分析中，我们假设发射器发射连续电磁波，但在实践中传输时间有限。例如，在 $5\mathrm{GHz}$ 频段、$40\mathrm{MHz}$ 带宽信道上运行的 IEEE 802.11n WiFi 系统中，一个标准 WiFi 符号为 $4\mu \mathrm{s}$，由一个 $3.2\mu \mathrm{s}$ 的有用符号持续时间和一个 $0.8\mu \mathrm{s}$ 的保护间隔组成。根据 [30]，对于大多数办公建筑，延迟扩展在 40 到 $70\mathrm{ns}$ 范围内，远小于标准 WiFi 符号的持续时间。因此，我们可以假设在 WiFi 系统中传输的是连续波。

基于上述假设和 (2)，(8) 可以近似为

$$\begin{array}{rl} & {G(t,f)\approx \| \vec{E}_s(f) + \sum_{i\in \Omega_d}\vec{E}_i(t,f)\| ^2 +\epsilon (t,f)}\\ & {= \left\| \sum_{u\in \{x,y,z\}}\left(E_{su}(f)\hat{u} +\sum_{i\in \Omega_d}E_{iu}(t,f)\hat{u}\right)\right\| ^2 +\epsilon (t,f)}\\ & {= \sum_{u\in \{x,y,z\}}\left|E_{su}(f) + \sum_{i\in \Omega_d}E_{iu}(t,f)\right|^2 +\epsilon (t,f)}\\ & {= \sum_{u\in \{x,y,z\}}\left(|E_{su}(f)|^2 +2\mathrm{Re}\left\{E_{su}^*(f)\sum_{i\in \Omega_d}E_{iu}(t,f)\right\} \right.}\\ & {\left. + \left|\sum_{i\in \Omega_d}E_{iu}(t,f)\right|^2\right) + \epsilon (t,f),} \end{array} \quad (9)$$

其中 $\hat{x}$、$\hat{y}$ 和 $\hat{z}$ 是如图 2 所示的彼此正交的单位向量，$\mathrm{Re}\{\cdot \}$ 表示取复数实部的运算，$E_{iu}$ 表示 $\vec{E}_i$ 在 $u$ 轴方向的分量，对于 $\forall u\in \{x,y,z\}$。那么，$G(t,f)$ 的自协方差函数可以推导为

$$\begin{array}{rl} & {\gamma_G(\tau ,f) = \mathrm{cov}(G(t,f),G(t - \tau ,f))}\\ & {\approx \sum_{u\in \{x,y,z\}}\left(2|E_{su}(f)|^2\sum_{i\in \Omega_d}\mathrm{cov}(E_{iu}(t,f),E_{iu}(t - \tau ,f)) + \sum_{i_1,i_2\in \Omega_d}\mathrm{cov}(E_{i_1u}(t,f),E_{i_1u}(t - \tau ,f))\right.}\\ & {\left. + \sum_{i_1,i_2\in \Omega_d}\mathrm{cov}(E_{i_2u}(t,f),E_{i_2u}(t - \tau ,f))\right) + \delta (\tau)\sigma^2 (f),} \end{array} \quad (10)$$

其中应用了假设 1-3 和 (3) 来简化表达式，详细推导见附录 VIII-A。

---

**图 3：典型 CSI 功率响应的 Q-Q 图和样本自相关函数。**

根据自协方差与自相关的关系，$\gamma_G(\tau ,f)$ 可以用每个散射体的自相关函数形式重写为

$$\begin{array}{rl} & {\gamma_G(\tau ,f)\approx \sum_{u\in \{x,y,z\}}\left(\sum_{i\in \Omega_d}\frac{2|E_{su}(f)|^2E_i^2(f)}{3}\rho_{E_{iu}}(\tau ,f)\right.}\\ & {\left. + \sum_{i_1,i_2\in \Omega_d}\frac{E_{i_1}^2(f)E_{i_2}^2(f)}{9}\rho_{E_{i_1u}}(\tau ,f)\rho_{E_{i_2u}}(\tau ,f)\right) + \delta (\tau)\sigma^2 (f),} \end{array} \quad (11)$$

其中右边是通过使用关系 $E_{iu}^2 (f) = \frac{E_i^2(f)}{2}$ $\forall u\in \{x,y,z\}$ $\forall i\in$ $\Omega_d$ [24] 得到的。$G(t,f)$ 对应的自相关函数 $\rho_{G}(\tau ,f)$ 通过 $\rho_{G}(\tau ,f) = \frac{\gamma_{G}(\tau,f)}{\gamma_{G}(0,f)}$ 获得，其中 $\gamma_{G}(\tau ,0)$ 可以通过将 $\rho_{E_{iu}}(0,f) = 1$ 代入 (11) 得到。当所有动态散射体的运动方向大致相同时，我们可以选择 $\mathcal{Z}$ 轴与共同运动方向对齐。那么，$\rho_{E_{iu}}(\tau ,f)$ $\forall u\in \{x,y,z\}$ 的闭合形式在假设 1-2 [24] 下推导出来，即对于 $\forall i\in \Omega_d$

$$\begin{array}{rl} & {\rho_{E_{iu}}(\tau ,f) = \rho_{E_{iu}}(\tau ,f)}\\ & {= \frac{3\left[\sin (k\nu_i\tau)\right]}{2\left[k\nu_i\tau\right]}\frac{1}{(k\nu_i\tau)^2}\left(\frac{\sin(k\nu_i\tau)}{k\nu_i\tau}\right) - \cos (k\nu_i\tau)\Bigg\}}\\ & {\rho_{E_{iu}}(\tau ,f) = \frac{3}{(k\nu_i\tau)^2}\left[\frac{\sin(k\nu_i\tau)}{k\nu_i\tau}\right]\cos (k\nu_i\tau)\Bigg\}} \end{array} \quad (13)$$

理论空间自相关函数如图 4a 所示，其中 $d\triangleq \nu_i\tau$。从图 4a 可以看出，随着距离 $d$ 的增加，所有自相关函数的幅度都随着振荡而衰减。

对于一个带宽为 $40\mathrm{MHz}$、载波频率为 $5.805\mathrm{GHz}$ 的 WiFi 系统，每个子载波的波数 $k$ 的差异可以忽略，例如 $k_{\mathrm{max}} = 122.00$ 和 $k_{\mathrm{min}} = 121.16$。那么，我们可以假设 $\rho (\tau ,f) \approx \rho (\tau)$，$\forall f$。因此，我们可以通过对所有子载波取平均来改善样本自相关函数，即 $\hat{\rho}_{G}(\tau) \triangleq \frac{1}{F} \sum_{f \in \mathcal{F}} \hat{\rho}_{G}(\tau , f)$，其中 $\mathcal{F}$ 表示所有可用子载波的集合，$F$ 是子载波总数。当所有动态散射体具有相同速度时，即 $v_{i} = v$ 对于 $\forall i \in \Omega_{d}$（这在监测单个受试者的运动时是成立的），通过定义替代量 $E_{su}^{2} \triangleq \frac{2}{F} \sum_{f \in \mathcal{F}} |E_{su}(f)|^{2}$，$E_{d}^{2} \triangleq \frac{1}{3F} \sum_{i \in \Omega_{d}} \sum_{f \in \mathcal{F}} E_{i}^{2}(f)$，$\hat{\rho}_{G}(\tau)$ 可以进一步近似为（对于 $\tau \neq 0$）

$$\hat{\rho}_{G}(\tau)\approx C\sum_{u\in \{x,y,z\}}\left(E_{d}^{2}\hat{\rho}_{E_{iu}}^{2}(\tau) + E_{su}^{2}\hat{\rho}_{E_{iu}}(\tau)\right), \quad (14)$$

其中 $C$ 是比例因子，并假设每个子载波的方差彼此接近。

从 (14) 我们观察到，$\rho_{G}(\tau)$ 是 $\rho_{E_{iu}}(\tau)$ 和 $\rho_{E_{iu}}^{2}(\tau)$（$\forall u \in \{x, y, z\}$）的加权组合。(14) 的左边可以从 CSI 估计得到，速度嵌入在右边的每一项中。如果我们能将右边的一项与其他项分离开，那么速度就可以被估计出来。

对所有理论空间自相关函数取微分，如图 4b 所示，其中我们用 $\Delta \rho (\tau)$ 表示 $\frac{\mathrm{d}\rho(\tau)}{\mathrm{d}\tau}$，我们发现尽管接收电磁波不同分量的自相关函数叠加在一起，但 $\Delta \rho_{E_{iu}}^{2}(\tau)$（$\forall u \in \{x, y\}$）的第一个局部峰值恰好也是 $\Delta \rho_{G}(\tau)$ 的第一个局部峰值。因此，分量 $\rho_{E_{iu}}^{2}(\tau)$ 可以从 $\rho_{G}(\tau)$ 中识别出来，从而可以通过定位 $\Delta \hat{\rho}_{G}(\tau)$ 的第一个局部峰值来获取速度信息，这是 WiSpeed 从含噪 CSI 测量中提取的最重要特征。

为了验证 (14)，我们用商用 WiFi 设备构建了 WiSpeed 的原型。原型的配置总结如下：两个 WiFi 设备都在 WLAN 信道 161 上运行，中心频率为 $f_{c} = 5.805 \mathrm{GHz}$，带宽为 $40 \mathrm{MHz}$；发射器配备了一个商用 WiFi 芯片和两根全向天线，而接收器配备了三根全向天线，并使用带有修改固件和驱动程序的 Intel Ultimate N WiFi Link 5300 [8]。发射器以 $1500 \mathrm{~Hz}$ 的信道采样率 $F_{s}$ 发送探测帧，并在接收器处获取 CSI。发射功率配置为 $20 \mathrm{~dBm}$。

---

**图 4：电磁波不同正交分量的理论空间自相关函数。**

---

**图 5：典型办公环境中的实验设置，包含不同的 Tx/Rx 位置和行走路线。**

本文的所有实验都在一个典型的室内办公环境中进行，如图 5 所示。在每次实验中，Tx 和 Rx 之间的视距路径至少被一堵墙阻挡，导致严重的非视距条件。更具体地说，我们研究了两种情况：

1.  **发射器运动，接收器静止：** 发射器固定在一辆小车上，接收器放置在图 5 所示的 Rx #1 位置。小车沿着图 5 中标示的路线 #1 从 $t = 3.7 \mathrm{~s}$ 到 $t = 14.3 \mathrm{~s}$ 以近似恒速被推向前。
2.  **发射器和接收器均静止，有人经过：** 发射器和接收器分别放置在 Tx #1 和 Rx #1 位置。一个人从 $t = 4.9 \mathrm{~s}$ 到 $t = 16.2 \mathrm{~s}$ 沿着路线 #1 以类似于情况 (1) 的速度行走。

由于理论近似仅在短持续时间假设下有效，我们将最大时间延迟 $\tau$ 设为 $0.2 \mathrm{~s}$。在两种情况下，我们每 $0.05 \mathrm{~s}$ 计算一次样本自相关函数 $\hat{\rho}_{G}(\tau)$。

图 6 展示了这两种情况下的样本自相关函数。特别地，图 6a 可视化了在固定时间 $t$ 下，对于不同子载波，时间延迟 $\tau \in [0, 0.2 \mathrm{~s}]$ 的样本自相关函数，对应于图 6e 的一个快照；图 6c 显示了平均自相关函数 $\hat{\rho}_{G}(\tau)$，与单个 $\hat{\rho}_{G}(\tau, f)$ 相比噪声小得多。在这种情况下，发射器可以视为一个具有主导辐射功率的移动散射体，相比其他散射体，这导致 (14) 中 $E_{d}^{2} \rho_{E_{iu}}^{2}(\tau)$（$u \in \{x, y, z\}$）分量占主导。此外，$\rho_{E_{iu}}^{2}(\tau)$ 衰减得比 $\rho_{E_{iu}}^{2}(\tau)$ 和 $\rho_{E_{iy}}^{2}(\tau)$ 快得多，并且 $\rho_{E_{ix}}^{2}(\tau) = \rho_{E_{iy}}^{2}(\tau)$。因此，可以在 $\hat{\rho}_{G}(\tau)$ 和 $\rho_{E_{ix}}^{2}(\tau)$（$\rho_{E_{iy}}^{2}(\tau)$）之间观察到相似的模式，它们有一个共同的主导分量 $\frac{\sin^2(kv\tau)}{(kv\tau)^2}$，其中 $v$ 是小车和人的速度。图 6c 所示的实验结果与理论分析吻合良好。

类似地，对于情况 (2)，图 6b 显示了不同子载波的样本自相关函数 $\hat{\rho}_{G}(\tau, f)$，图 6d 显示了平均样本自相关函数 $\hat{\rho}_{G}(\tau)$，这是图 6f 在固定时间 $t$、时间延迟 $\tau = [0, 0.2s]$ 的一个快照。显然，样本自相关函数中分量 $\rho_{E_{iu}}^{2}(\tau)$（$u \in \{x, y\}$）的模式远不如情况 (1)（图 6c 和图 6e）中明显。这可以通过以下事实解释：辐射功率 $E_{d}^{2}$ 远小于情况 (1)，因为动态散射体集合仅由人体在移动中的不同部位组成。因此，$\hat{\rho}_{G}(\tau)$ 的形状更接近于 $\rho_{E_{iu}}(\tau)$（$\forall u \in \{x, y, z\}$），具有主导分量 $\frac{\sin(kv\tau)}{kv\tau}$。此外，从图 6d 我们可以观察到 $\frac{\sin(kv\tau)}{kv\tau}$ 和 $\frac{\sin^2(kv\tau)}{(kv\tau)^2}$ 的叠加，并且 $\frac{\sin(kv\tau)}{kv\tau}$ 的权重大于 $\frac{\sin^2(kv\tau)}{(kv\tau)^2}$。我们还观察到嵌入的分量 $\frac{\sin^2(kv\tau)}{(kv\tau)^2}$ 与情况 (1) 相比具有相似的模式，因为两次实验中的运动速度彼此接近。

## V. WISPEED 的关键组件

基于第四节推导的理论结果，我们提出 WiSpeed，它集成了三个模块：运动速度估计器、加速度估计器和步态周期估计器。运动速度估计器是 WiSpeed 的核心模块，而另外两个模块从运动速度估计器中提取有用特征，用于检测跌倒和估计行走者的步态周期。

### A. 运动速度估计器

WiSpeed 通过从 CSI 测量中计算样本自相关函数 $\Delta \hat{\rho}_{G}(\tau)$，定位 $\Delta \hat{\rho}_{G}(\tau)$ 的第一个局部峰值，并将峰值位置映射到速度估计，来估计受试者的运动速度。由于通常样本自相关函数 $\Delta \hat{\rho}_{G}(\tau)$ 是有噪声的，如图 6e 和图 6f 所示，我们基于局部回归 [31] 的思想开发了一种新颖的鲁棒局部峰值识别算法，以可靠地检测 $\Delta \hat{\rho}_{G}(\tau)$ 第一个局部峰值的位置。

---

**图 6：两种场景下的自相关函数。** (a) 移动 Tx 时不同子载波测量的自相关函数；(b) 行走的人时不同子载波测量的自相关函数；(c) 移动 Tx 的自相关函数快照；(d) 行走的人的自相关函数快照；(e) 移动 Tx 的自相关函数矩阵；(f) 行走的人的自相关函数矩阵。

为方便起见，将用于局部峰值检测的离散信号记为 $y[n]$，我们的目标是识别 $y[n]$ 中的局部峰值。首先，我们对 $y[n]$ 应用一个长度为 $2L+1$ 的滑动窗口，其中 $L$ 选择与所需局部峰值的宽度相当。然后，对于每个中心位于 $n$ 的窗口，我们通过对窗口内的数据分别执行线性回归和二次回归，来验证窗口内是否存在任何潜在的局部峰值。令 $SSE$ 表示二次回归的误差平方和，$SSEr$ 表示线性回归的误差平方和。如果给定窗口内没有局部峰值，那么比率 $\alpha[n] \triangleq \frac{(SSEr - SSE)/(3-2)}{SSE/(2L+1-3)}$ 可以解释为窗口内存在峰值的似然度量，并且在某些假设下，具有中心 $F$ 分布，自由度为 1 和 $2(L-1)$ [32]。仅当 $\alpha[n]$ 大于预设阈值 $\eta$（由期望的发现虚假峰值的概率决定）且 $\alpha[n]$ 也大于其邻域 $\alpha[n-L],...,\alpha[n+L]$ 时，我们才选择以点 $n$ 为中心的窗口作为潜在窗口。当 $L$ 足够小且窗口内仅存在一个局部峰值时，可以从拟合的二次曲线直接获得局部峰值的位置。

下面我们用一个数值例子来验证所提出的局部峰值识别算法的有效性。令 $y(t) = \cos (2\pi f_1t + 0.2\pi) + \cos (2\pi f_2t + 0.3\pi) + n(t)$，其中设 $f_1 = 1\mathrm{Hz}$，$f_2 = 2.5\mathrm{Hz}$，$n(t) \sim \mathcal{N}(0, \sigma^2)$ 是均值为零、方差为 $\sigma^2$ 的加性高斯白噪声。信号 $y(t)$ 从时间 $t = 0$ s 到 $t = 1$ s 以 $100\mathrm{Hz}$ 的速率采样。当不存在噪声时，两个局部峰值的真实位置为 $t_1 \approx 0.331$ s 和 $t_2 \approx 0.760$ s，我们提出的局部峰值识别算法的估计值为 $\hat{t}_1 \approx 0.327$ s 和 $\hat{t}_2 \approx 0.763$ s，如图 7a 所示。当存在噪声且 $\sigma$ 设为 0.2 时，估计值为 $\hat{t}_1 \approx 0.336$ s 和 $\hat{t}_2 \approx 0.762$ s，如图 7b 所示。从结果可以看出，即使信号被噪声污染，估计的局部峰值位置也非常接近实际峰值的位置，这表明了所提出的局部峰值识别算法的有效性。

然后，运动物体的速度可以估计为 $\hat{v} = \frac{0.54\lambda}{\hat{\tau}}$，其中 $0.54\lambda$ 是 $\Delta \rho_{E_{i x}}^{2}(d)$ 的第一个局部峰值与原点的距离，$\hat{\tau}$ 是 $\Delta \hat{\rho}_{G}(\tau)$ 的第一个局部峰值的位置。然后对速度估计应用中值滤波器以去除异常值。

### B. 加速度估计器

加速度可以通过第五-A节得到的 $\hat{v}$ 计算。一种直观的加速度估计方法是取两个相邻速度估计的差值，然后除以它们的测量时间差。然而，这种方案并不鲁棒，因为它可能放大估计噪声。相反，我们利用这样一个事实：只要在短时间内有足够的速度估计，加速度值可以近似为分段线性函数。$\ell_1$ 趋势滤波器产生的趋势估计在分段线性意义上是平滑的 [33]，非常适合我们的目的。因此，我们采用 $\ell_1$ 趋势滤波器来提取嵌入在速度估计中的分段线性趋势，然后通过对平滑后的速度估计取微分来估计加速度。

数学上，令 $\hat{v} [n]$ 表示 $\hat{v} (n\Delta T)$，其中 $\Delta T$ 是两个估计之间的间隔，$\tilde{v} [n]$ 表示平滑后的估计。那么，$\tilde{v} [n]$ 通过求解以下无约束优化问题获得：

$$\min_{\tilde{v}[n],\forall n}\sum_{n = 1}^{N}(\tilde{v}[n] - \hat{v}[n])^{2} + \lambda \sum_{n = 2}^{N - 1}\tilde{v}[n - 1] - 2\tilde{v}[n] + \tilde{v}[n + 1]\Bigg|, \quad (15)$$

其中 $\lambda \geq 0$ 是正则化参数，用于控制 $\tilde{v} [n]$ 的平滑度与残差 $|\tilde{v} [n] - \hat{v} [n]|$ 的大小之间的权衡。然后，我们得到加速度估计 $\hat{a} [n] = \frac{(\tilde{v}[n] - \tilde{v}[n - 1])}{\Delta T}$。如 [33] 所示，$\hat{\ell}_1$ 滤波器的复杂度随数据长度线性增长，可以在大多数平台上实时计算。

### C. 步态周期估计器

当估计的速度在某个范围内时，例如从 $1m / s$ 到 $2m / s$，且加速度估计较小时，WiSpeed 开始估计相应的步态周期。事实上，单步行走的过程可以分解为三个阶段：将一条腿抬离地面，用抬起的腿接触地面并将身体向前推，以及在下一步之前短暂静止。重复此过程直至到达目的地。

就速度而言，一个行走周期由一个加速阶段和一个减速阶段组成。WiSpeed 利用速度变化的周期性模式进行步态周期估计。更具体地说，WiSpeed 定位速度估计中对应于速度最大的时刻的局部峰值。为了实现峰值定位，我们使用 [34] 中提出的基于持久性的方案来形成多对局部最大值和局部最小值，并将局部最大值的位置视为峰值位置。每两个相邻峰值之间的时间间隔被计算为一个步态周期。同时，每两个相邻峰值之间的移动距离被计算为步幅长度的估计值。

---

**图 7：峰值识别算法的示意图。**

## VI. 实验结果

在本节中，我们首先介绍实验的室内环境和系统设置。然后，在两个应用中评估 WiSpeed 的性能：人体行走监测和人体跌倒检测。

### A. 环境

我们在一个典型的办公环境中进行了大量实验，其平面图如图 5 所示。室内空间被办公桌、计算机、架子、椅子和家用电器占据。实验中使用的是与第四节介绍的相同的 WiFi 设备。

### B. 实验设置

进行了两组实验。在第一组实验中，我们研究了 WiSpeed 在估计人体行走速度方面的性能。对于无设备场景，结果表明除了行走速度外，还可以估计步数和步幅长度。使用估计的行走距离与真实距离进行比较作为准确度度量，因为测量行走距离比直接测量速度更容易、更准确。测试了不同的路线和设备位置，实验设置的详细信息总结在表 I 和表 II 中。在第二组实验中，我们研究了 WiSpeed 作为人体活动监测方案的性能。要求两名参与者执行不同的活动，包括站立、坐下、从地面捡起物品、行走和跌倒。

**表 I：无设备人体行走监测的实验设置**

| 配置设置 | Tx 位置 | Rx 位置 | 路线索引 |
| :------- | :------ | :------ | :------- |
| 设置 #1  | Tx #1   | Rx #1   | 路线 #1/#2 |
| 设置 #2  | Tx #1   | Rx #2   | 路线 #1/#2 |
| 设置 #3  | Tx #2   | Rx #1   | 路线 #1/#2 |
| 设置 #4  | Tx #3   | Rx #2   | 路线 #3/#4 |
| 设置 #5  | Tx #4   | Rx #2   | 路线 #3/#4 |
| 设置 #6  | Tx #3   | Rx #3   | 路线 #3/#4 |

**表 II：有设备速度监测的实验设置**

| 配置设置 | Tx 位置 | Rx 位置 | 路线索引 |
| :------- | :------ | :------ | :------- |
| 设置 #7  | 移动    | Rx #1   | 路线 #1/#2 |
| 设置 #8  | 移动    | Rx #4   | 路线 #1/#2 |
| 设置 #9  | 移动    | Rx #1   | 路线 #3/#4 |
| 设置 #10 | 移动    | Rx #4   | 路线 #3/#4 |

### C. 人体行走监测

图 8 可视化了设置 #1、路线 #1 下的一个实验结果，即 Tx 和 Rx 均静止，一名实验者沿着指定路线行走。图 8a-c 显示了在图 8d 中标记的不同时间点的估计自相关函数的三个快照。从图 8 我们可以得出结论，尽管自相关函数差异很大，但只要在相似的行走速度下计算自相关函数，$\Delta \hat{\rho}_{G}(\tau)$ 的第一个局部峰值的位置是高度一致的。

图 8d 显示了实验中行走速度估计的结果，由于加速和减速，我们可以看到一个非常清晰的行走模式。相应的步幅长度估计如图 8e 所示。估计的行走距离为 $8.46\mathrm{m}$，与 $8\mathrm{m}$ 的真实距离相比，误差在 $5.75\%$ 以内。另一方面，平均步幅长度为 $0.7\mathrm{m}$，非常接近参与者的平均行走步幅长度。

图 9 显示了在设置 #7、路线 #1 下的两个典型速度估计结果，其中发射器固定在小车上，一名实验者沿着指定路线推车。小车在这两次实验中以不同的速度移动，图 9a 和图 9b 分别显示了相应的速度估计。从估计的速度模式可以看出，没有像图 8d 中无设备行走速度估计那样的周期性模式。这是因为当发射器移动时，人体反射的电磁波能量被发射天线辐射的能量所主导，WiSpeed 只能估计移动天线的速度。对于小车以较高速度移动的情况，估计的移动距离为 $8.26\mathrm{m}$，另一个为 $8.16\mathrm{m}$，而真实距离为 $8\mathrm{m}$。

---

**图 8：设置 #1 和路线 #1 下人体行走监测的实验结果。**

---

**图 9：移动 Tx 的速度估计。**

图 10 总结了 200 次人体行走速度估计实验的准确性。更具体地说，图 10a 显示了设置 #1 - #6 的误差分布，图 10b 显示了路线 #1 - #4 对应的误差分布；图 10c 显示了设置 #7 - #10 的误差分布，图 10d 显示了路线 #1 - #4 对应的误差分布。底部和顶部的误差条分别代表估计值的 $5\%$ 分位数和 $95\%$ 分位数，点中间是估计值的样本均值。路线 #1- #4 的真实距离如图 5 所示。从结果中我们发现：(i) WiSpeed 在不同的 Tx/Rx 位置、路线、受试者和行走速度下表现一致，表明 WiSpeed 在各种场景下的鲁棒性，以及 (ii) 在无设备设置下，WiSpeed 倾向于高估移动距离。这是因为我们使用路线距离作为基准，而忽略了受试者在重力方向上的位移。由于 WiSpeed 测量的是覆盖区域内受试者的绝对移动距离，重力方向上的运动会给距离估计带来偏差。

总之，WiSpeed 在无设备人体行走速度估计中的平均绝对百分比误差为 $4.85\%$，在有设备速度估计中为 $4.62\%$，即使仅使用一对 WiFi 设备且在严重的非视距条件下，也优于现有方法。

### D. 人体跌倒检测

在本小节中，我们展示 WiSpeed 能够将跌倒与其他日常正常活动区分开。我们总共收集了五组数据：

---

**图 10：不同条件下距离估计的误差分布。**

(i) 跌倒在地上，(ii) 从椅子上站起来，(iii) 坐到椅子上，(iv) 弯腰从地面捡起物品，(v) 在房间内行走。每次实验持续 8 秒。我们从两名受试者那里收集了 20 个跌倒活动的数据集，以及来自同一两名受试者的其他四种活动各 10 个数据集。实验在 5 号房间进行，WiFi Tx 和 Rx 放置在图 5 所示的 Tx #1 和 Rx #2 位置。图 11 显示了不同活动和受试者的速度和加速度估计结果的快照。

认识到现实世界中跌倒的持续时间可能短至 $0.5\mathrm{s}$，并且人体会经历突然加速然后减速 [35]，我们提出了两个用于跌倒检测的度量：(i) $0.5\mathrm{s}$ 内的最大加速度变化，记为 $\Delta a$，以及 (ii) 最大加速度变化期间的最大速度，记为 $v_{\mathrm{max}}$。图 12 显示了来自两名受试者的所有活动的 $(\Delta a,v_{\mathrm{max}})$ 分布。显然，通过设置两个阈值：$\Delta a\geq 1.6m / s^2$ 和 $v_{\mathrm{max}}\geq 1.2m / s$，WiSpeed 可以将跌倒与其他四种活动区分开，除了一个离群点，检测率达到 $95\%$ 且零虚警，而 [14] 需要机器学习技术。这是因为 WiSpeed 提取了活动分类中最重要的物理特征，即速度和加速度变化，而 [14] 是间接推断这两个物理量的。

## VII. 讨论

在本节中，我们讨论了不同应用的系统参数选择及其对 WiSpeed 计算复杂度的影响。

### A. 追踪快速运动物体

为了追踪快速变速的物体，我们采用以下减少样本数的方程来计算样本自协方差函数：

$$\begin{array}{l}\hat{\gamma}_G(\tau ,f) = \\ \displaystyle \frac{1}{M}\sum_{t = T - M + 1}^{T}(G(t - \tau ,f) - \bar{G} (f))(G(t,f) - \bar{G} (f)) \end{array} \quad (16)$$

其中 $T$ 是窗口长度，$M$ 是用于平均的样本数，$\bar{G} (f)$ 是样本均值。(16) 表明，要估计速度为 $v$ 的运动物体，WiSpeed 需要一个持续时间 $T_0 = \frac{0.54\lambda}{v} +\frac{M}{F_s}$ 秒的时间窗口。本质上，WiSpeed 捕获的是一段时间内的平均运动速度，而不是瞬时运动速度。例如，当 $v = 1.3m / s$，$F_s = 1500\mathrm{Hz}$，$f_c = 5.805\mathrm{GHz}$，$M = 100$ 时，$T_0$ 约为 $0.12\mathrm{s}$。如果在 $T_0$ 持续时间内速度发生显著变化，WiSpeed 的性能将会下降。为了追踪快速变化的运动物体的速度，需要较小的 $T_0$，这可以通过增加信道采样率 $F_s$ 或增加载波频率以减小波长 $\lambda$ 来实现。

### B. 计算复杂度

主要的计算复杂度来自于总体自相关函数 $\hat{\rho}_G(\tau)$ 的估计，总共需要 $F M T_0 F_s$ 次乘法，其中 $F$ 是可用子载波的数量。对于速度变化缓慢的运动，如行走和站立，较低的信道采样率就足够了，这可以降低复杂度。例如，在我们的行走速度估计和跌倒检测实验中，$F_s = 1500\mathrm{Hz}$，$f_c = 5.805\mathrm{GHz}$，$F = 180$，$M = 100$，WiSpeed 产生一次输出所需的总乘法次数约为 300 万次。这使得在配备 Intel Core i7-7500U 处理器和 16GB 内存的台式机上，计算时间为 $80.4\mathrm{ms}$，足够满足实时应用的要求。

---

**图 11：不同活动和受试者的速度和加速度。**

---

**图 12：所有活动的两个度量的分布。**

## VIII. 结论

在这项工作中，我们提出了 WiSpeed，一种利用商用 WiFi 的通用室内人体运动速度估计系统，可以在无设备或有设备条件下估计运动物体的速度。WiSpeed 建立在电磁波统计理论的基础上，该理论量化了室内环境中人体运动对电磁波的影响。我们在典型的室内环境中进行了大量实验，结果表明 WiSpeed 在无设备人体行走速度监测中的平均绝对百分比误差为 $4.85\%$，在有设备速度估计中为 $4.62\%$。同时，它在人体跌倒检测中的平均检测率达到 $95\%$，且无虚警。由于其覆盖范围大、鲁棒性强、成本低和计算复杂度低，WiSpeed 是室内被动人体活动监测系统中非常有前景的候选方案。

## 附录

### A. (10) 的推导

首先，我们可以将 $G(t,f)$ 重写为

$$G(t,f) = \sum_{u\in \{x,y,z\}}G_{u}(t,f) + \epsilon (t,f), \quad (17)$$

其中 $G_{u}(t,f) \triangleq |E_{su}(f)|^{2} + 2\mathrm{Re}\left\{E_{su}^{*}(f)\sum_{i\in \Omega_{d}}E_{iu}(t,f)\right\} + |\sum_{i\in \Omega_{d}}E_{iu}(t,f)|^{2}$。那么，$G(t,f)$ 的协方差可以写成

$$\begin{array}{rl} & {\gamma_{G}(\tau ,f) = \mathrm{cov}\big(G(t,f),G(t - \tau ,f)\big)}\\ & {= \sum_{u\in \{x,y,z\}}\mathrm{cov}\big(G_{u}(t,f),G_{u}(t - \tau ,f)\big) + \mathrm{cov}\big(\epsilon (t,f)\epsilon (t - \tau ,f)\big)}\\ & {= \sum_{u\in \{x,y,z\}}\mathrm{cov}\big(G_{u}(t,f),G_{u}(t - \tau ,f)\big) + \delta (\tau)\sigma^{2}(f),} \end{array} \quad (18)$$

这是由于假设 2-3 和噪声项的假设。因此，在下文中，我们只需关注项 $\gamma_{G_{u}}(\tau ,f) \triangleq \mathrm{cov}\big(G_{u}(t,f),G_{u}(t - \tau ,f)\big)$，即对于 $\forall u \in \{x,y,z\}$，我们有方程 (19)。我们从

$$\begin{array}{r l} & {\gamma_{G_{u}}(\tau ,f) = \Big\langle G_{u}(t,f) - \langle G_{u}(t,f)\rangle ,G_{u}(t - \tau ,f) - \langle G_{u}(t - \tau ,f)\rangle \Big\rangle}\\ & {= \underbrace{\Big\langle 2\mathrm{Re}\{E_{s u}^{*}(f)\sum_{i\in \Omega_{d}}E_{i u}(t,f)\} + \Big(\big|\sum_{i\in \Omega_{d}}E_{i u}(t,f)\big|^{2} - \langle \big|\sum_{i\in \Omega_{d}}E_{i u}(t,f)\rangle^{2}\big)\Big\rangle}_{A_{1}},}\\ & {\underbrace{2\mathrm{Re}\{E_{s u}^{*}(f)\sum_{i\in \Omega_{d}}E_{i u}(t - \tau ,f)\} + \Big(\big|\sum_{i\in \Omega_{d}}E_{i u}(t - \tau ,\tau)\big|^{2} - \langle \big|\sum_{i\in \Omega_{d}}E_{i u}(t - \tau ,f)\rangle^{2}\big)\Big\rangle}_{A_{3}}.} \end{array}$$ (19)

项 $\left\langle A_{1},A_{3}\right\rangle$ 开始。为方便起见，定义 $E_{iu}(t,f)\triangleq a_{i}(t) + jb_{i}(t)$ 和 $E_{su}(f)\triangleq u + jv$ 对于 $\forall i\in \Omega_{d}$ $\forall u\in \{x,y,z\}$，且 $a_{i},b_{i},u,v$ 均为实数。那么，我们有

$$\begin{array}{r l} & {\Big\langle A_{1},A_{3}\Big\rangle}\\ & {= 4\Big\langle \underset {i\in \Omega_{d}}{\sum}a_{i}(t) + v\sum_{i\in \Omega_{d}}b_{i}(t),u\sum_{i\in \Omega_{d}}a_{i}(t - \tau) + v\sum_{i\in \Omega_{d}}b_{i}(t - \tau)\Big\rangle}\\ & {= 4u^{2}\underset {i\in \Omega_{d}}{\sum}\Big\langle a_{i}(t),a_{i}(t - \tau)\Big\rangle +4v^{2}\underset {i\in \Omega_{d}}{\sum}\Big\langle b_{i}(t),b_{i}(t - \tau)\Big\rangle}\\ & {= 4(u^{2} + v^{2})\underset {i\in \Omega_{d}}{\sum}\Big\langle a_{i}(t),a_{i}(t - \tau)\Big\rangle ,} \end{array} \quad (20)$$

其中我们应用了电场实部和虚部具有相同统计行为的假设。同时，我们有

$$\begin{array}{r l} & {\mathrm{cov}(E_{iu}(t,f),E_{iu}(t - \tau ,f))}\\ & {= \Big\langle E_{iu}(t,f),E_{iu}(t - \tau ,f)\Big\rangle}\\ & {= \Big\langle a_{i}(t),a_{i}(t - \tau)\Big\rangle +\Big\langle b_{i}(t),b_{i}(t - \tau)\Big\rangle}\\ & {= 2\Big\langle a_{i}(t),a_{i}(t - \tau)\Big\rangle .} \end{array} \quad (21)$$

因此，我们有

$$\Big\langle A_{1},A_{3}\Big\rangle$$ 
$$= 2|E_{su}(f)|^{2}\underset {i\in \Omega_{d}}{\sum}\mathrm{cov}\big(E_{iu}(t,f),E_{iu}(t - \tau ,f)\big). \quad (22)$$

接下来，我们推导项 $\Big\langle A_{1},A_{4}\Big\rangle$ 如 (23) 所示。根据 (3) 中电场的积分表示，我们有

$$\begin{array}{r l} & {|E_{iu}(t,f)|^{2}}\\ & {\iint_{4\pi}F_{iu}(\Theta_{1})F_{iu}^{*}(\Theta_{2})\mathrm{exp}(-j(\vec{k} (\Theta_{1}) - \vec{k} (\Theta_{2}))\vec{v}_{i}t)\mathrm{d}\Theta_{1}\mathrm{d}\Theta_{2},} \end{array} \quad (24)$$

因此，$E_{iu}(t,f)$ 和 $|E_{iu}(t - \tau ,f)|^{2}$ 之间的协方差可以表示为

$$\begin{array}{r l} & {\mathrm{cov}(E_{iu}(t,f),|E_{iu}(t - \tau ,f)|^{2})}\\ & {= \Big\langle E_{iu}(t,f) - \langle E_{iu}(t,f)\rangle ,|E_{iu}(t - \tau ,f)|^{2} - \langle |E_{iu}(t - \tau ,f)|^{2}\rangle \Big\rangle}\\ & {= \Big\langle E_{iu}(t,f),|E_{iu}(t - \tau ,f)|^{2}\Big\rangle}\\ & {= \iint \int_{4\pi}^{4\pi}\Big\langle F_{iu}(\Theta_{1}),F_{iu}(\Theta_{2})F_{iu}^{*}(\Theta_{2})\Big\rangle \mathrm{exp}(-j\vec{k} (\Theta_{1})\cdot \vec{v}_{i}t)}\\ & {\qquad \mathrm{exp}(-j(\vec{k} (\Theta_{2}) - \vec{k} (\Theta_{2}))\cdot \vec{v}_{i}(t - \tau))\mathrm{d}\Theta_{1}\mathrm{d}\Theta_{2}\mathrm{d}\Theta_{2}}\\ & {= \int_{4\pi}\Big\langle F_{iu}(\Theta_{1}),|F_{iu}(\Theta_{1})|^{2}\Big\rangle \mathrm{exp}(-j\vec{k} (\Theta_{1})\cdot \vec{v}_{i}t)\mathrm{d}\Theta_{1}}\\ & {= \int_{4\pi}\Big(\Big\langle \mathrm{Re}\Big\{F_{iu}(\Theta_{1})\Big\} ,\mathrm{Re}\Big\{F_{iu}(\Theta_{1})\Big\}^{2}\Big\rangle +}\\ & {j\Big\langle \mathrm{Im}\Big\{F_{iu}(\Theta_{1})\Big\} ,\mathrm{Im}\Big\{F_{iu}(\Theta_{1})\Big\}^{2}\Big\rangle \Big\rangle \mathrm{exp}(-j\vec{k} (\Theta_{1})\cdot \vec{v}_{i}t)\mathrm{d}\Theta_{1}}\\ & {= 0,} \end{array} \quad (25)$$

因为对于任何零均值高斯随机变量，$\langle X^{3}\rangle \equiv 0$。同时，我们有

$$\begin{array}{r l} & {\Big\langle E_{iu}(t,f),|E_{iu}(t - \tau ,f)|^{2}\Big\rangle}\\ & {= \Big\langle a_{i}(t),a_{i}^{2}(t - \tau)\Big\rangle +j\Big\langle b_{i}(t),b_{i}^{2}(t - \tau)\Big\rangle ,} \end{array} \quad (26)$$

因此，有 $\Big\langle a_{i}(t),a_{i}^{2}(t - \tau)\Big\rangle = 0$。将此结果代入 (23)，我们可以得到

$$\Big\langle A_{1},A_{4}\Big\rangle = 0. \quad (27)$$

类似地，我们也可以推导出 $\Big\langle A_{2},A_{3}\Big\rangle = 0$。最后，我们推导项 $\Big\langle A_{2},A_{4}\Big\rangle$ 如 (28) 所示。由于对于任意两个零均值高斯随机变量 $X$ 和 $Y$，期望可以通过以下关系计算 [36]：

$$\Big\langle X^{2}Y^{2}\Big\rangle = \Big\langle X^{2}\Big\rangle \Big\langle Y^{2}\Big\rangle +2\Big\langle XY\Big\rangle^{2}, \quad (29)$$

那么，对于 $\forall i\in \Omega_d$，有

$$\begin{array}{rl} \mathrm{cov}\big(a_i^2(t), a_i^2(t-\tau)\big) &= \big\langle a_i^2(t) - \langle a_i^2(t)\rangle, a_i^2(t-\tau) - \langle a_i^2(t-\tau)\rangle \big\rangle \\ &= \big\langle a_i^2(t), a_i^2(t-\tau) \big\rangle - \big\langle a_i^2(t)\big\rangle \big\langle a_i^2(t-\tau) \big\rangle \\ &= 2\big\langle a_i(t), a_i(t-\tau) \big\rangle^2 \\ &= \frac{1}{2} \mathrm{cov}\big(E_{iu}(t,f), E_{iu}(t-\tau, f)\big)^2. \end{array} \quad (30)$$

对于 $i_1, i_2 \in \Omega_d$ 且 $i_1 \neq i_2$，有

$$\begin{array}{rl} \mathrm{cov}\big(a_{i_1}(t)a_{i_2}(t), a_{i_1}(t-\tau)a_{i_2}(t-\tau)\big) &= \big\langle a_{i_1}(t)a_{i_2}(t), a_{i_1}(t-\tau)a_{i_2}(t-\tau) \big\rangle \\ &= \big\langle a_{i_1}(t)a_{i_1}(t-\tau), a_{i_2}(t)a_{i_2}(t-\tau) \big\rangle \\ &= \big\langle a_{i_1}(t), a_{i_1}(t-\tau) \big\rangle \big\langle a_{i_2}(t), a_{i_2}(t-\tau) \big\rangle \\ &= \frac{1}{4} \mathrm{cov}\big(E_{i_1 u}(t,f), E_{i_1 u}(t-\tau, f)\big) \mathrm{cov}\big(E_{i_2 u}(t,f), E_{i_2 u}(t-\tau, f)\big). \end{array} \quad (31)$$

因此，$\big\langle A_2, A_4 \big\rangle$ 可以推导为

$$\big\langle A_2, A_4 \big\rangle = \sum_{\substack{i_1,i_2\in \Omega_d \\ i_1 \ge i_2}} \mathrm{cov}\big(E_{i_1 u}(t,f), E_{i_1 u}(t-\tau, f)\big) \mathrm{cov}\big(E_{i_2 u}(t,f), E_{i_2 u}(t-\tau, f)\big). \quad (32)$$

最后，我们可以得到 (10) 所示的结果。

## 参考文献

[1] M. Khan, B. N. Silva, and K. Han, “Internet of things based energy aware smart home control system,” IEEE Access, vol. 4, pp. 7556–7566, 2016.
[2] S. Pinto, J. Cabral, and T. Gomes, “We-care: An IoT-based health care system for elderly people,” in IEEE International Conference on Industrial Technology (ICIT), pp. 1378–1383, March 2017.
[3] S. E. Schaefer, C. C. Ching, H. Breen, and J. B. German, “Wearing, thinking, and moving: testing the feasibility of fitness tracking with urban youth,” American Journal of Health Education, vol. 47, no. 1, pp. 8–16, 2016.
[4] L. Wang, G. Zhao, L. Cheng, and M. Pietikäinen, Machine learning for vision-based motion analysis: Theory and techniques. Springer, 2010.
[5] S. Z. Gurbuz, C. Clemente, A. Balleri, and J. J. Soraghan, “Micro-Doppler-based in-home aided and unaided walking recognition with multiple radar and sonar systems,” IET Radar, Sonar Navigation, vol. 11, no. 1, pp. 107–115, 2017.
[6] C.-Y. Hsu, Y. Liu, Z. Kabelac, R. Hristov, D. Katabi, and C. Liu, “Extracting gait velocity and stride length from surrounding radio signals,” in Proc. of CHI Conference on Human Factors in Computing Systems, pp. 2116–2126, ACM, 2017.
[7] K. Qian, C. Wu, Z. Yang, Y. Liu, and K. Jamieson, “Widar: Decimeter-level passive tracking via velocity monitoring with commodity wi-fi,” in Proc. of the 18th ACM International Symposium on Mobile Ad Hoc Networking and Computing, p. 6, ACM, 2017.
[8] D. Halperin, W. Hu, A. Sheth, and D. Wetherall, “Tool release: Gathering 802.11n traces with channel state information,” SIGCOMM Comput. Commun. Rev., vol. 41, pp. 53–53, Jan. 2011.
[9] H. Abdelnasser, M. Youssef, and K. A. Harras, “WiGest: A ubiquitous WiFi-based gesture recognition system,” in Proc. of IEEE INFOCOM, pp. 1472–1480, April 2015.
[10] K. Qian, C. Wu, Z. Zhou, Y. Zheng, Z. Yang, and Y. Liu, “Inferring motion direction using commodity wi-fi for interactive exergames,” in Proc. of CHI Conference on Human Factors in Computing Systems, pp. 1961–1972, ACM, 2017.
[11] K. Ali, A. X. Liu, W. Wang, and M. Shahzad, “Keystroke recognition using WiFi signals,” in Proc. of the 21st Annual International Conference on Mobile Computing & Networking, pp. 90–102, ACM, 2015.
[12] Q. Pu, S. Gupta, S. Gollakota, and S. Patel, “Whole-home gesture recognition using wireless signals,” in Proc. of the 19th Annual International Conference on Mobile Computing & Networking, pp. 27–38, ACM, 2013.
[13] G. Wang, Y. Zou, Z. Zhou, K. Wu, and L. M. Ni, “We can hear you with Wi-Fi,” IEEE Transactions on Mobile Computing, vol. 15, pp. 2907–2920, Nov 2016.
[14] Y. Wang, J. Liu, Y. Chen, M. Gruteser, J. Yang, and H. Liu, “E-eyes: Device-free location-oriented activity identification using fine-grained wifi signatures,” in Proc. of the 20th Annual International Conference on Mobile Computing & Networking, pp. 617–628, ACM, 2014.
[15] W. Wang, A. X. Liu, M. Shahzad, K. Ling, and S. Lu, “Understanding and modeling of WiFi signal based human activity recognition,” in Proc. of the 21st Annual International Conference on Mobile Computing & Networking, pp. 65–76, ACM, 2015.
[16] Y. Wang, K. Wu, and L. M. Ni, “WiFall: Device-free fall detection by wireless networks,” IEEE Transactions on Mobile Computing, vol. 16, pp. 581–594, Feb 2017.
[17] L. Sun, S. Sen, D. Koutsoukolas, and K.-H. Kim, “WiDraw: Enabling hands-free drawing in the air on commodity WiFi devices,” in Proc. of the 21st Annual International Conference on Mobile Computing & Networking, pp. 77–89, ACM, 2015.
[18] F. Adib and D. Katabi, “See through walls with WiFi!,” SIGCOMM Comput. Commun. Rev., vol. 43, pp. 75–86, Aug. 2013.
[19] M. Seifeldin, A. Saeed, A. E. Kosba, A. El-Keyi, and M. Youssef, “Nuzzer: A large-scale device-free passive localization system for wireless environments,” IEEE Transactions on Mobile Computing, vol. 12, pp. 1321–1334, July 2013.
[20] C. Chen, Y. Han, Y. Chen, H. Q. Lai, F. Zhang, B. Wang, and K. J. R. Liu, “TR-BREATH: Time-reversal breathing rate estimation and detection,” IEEE Transactions on Biomedical Engineering, vol. PP, no. 99, pp. 1–14, 2017.
[21] Q. Xu, Y. Chen, B. Wang, and K. J. R. Liu, “TRIEDS: Wireless events detection through the wall,” IEEE Internet of Things Journal, vol. 4, pp. 723–735, June 2017.
[22] F. Adib, Z. Kabelac, D. Katabi, and R. C. Miller, “3D tracking via body radio reflections,” in 11th USENIX Symposium on Networked Systems Design and Implementation, pp. 317–329, USENIX Association, 2014.
[23] P. Murphy, A. Sabharwal, and B. Aazhang, “Design of warp: A flexible wireless open-access research platform,” in Proc. EUSIPCO, pp. 53–54, 2006.
[24] D. A. Hill, Electromagnetic fields in cavities: deterministic and statistical theories, vol. 35. John Wiley & Sons, 2009.
[25] D. Tse and P. Viswanath, Fundamentals of wireless communication. Cambridge university press, 2005.
[26] C. Chen, Y. Chen, Y. Han, H. Q. Lai, F. Zhang, and K. J. R. Liu, “Achieving centimeter-accuracy indoor localization on WiFi platforms: A multi-antenna approach,” IEEE Internet of Things Journal, vol. 4, pp. 122–134, Feb 2017.
[27] F. Zhang, C. Chen, B. Wang, H. Q. Lai, and K. J. R. Liu, “A time-reversal spatial hardening effect for indoor speed estimation,” in Proc. of IEEE ICASSP, pp. 5955–5959, March 2017.
[28] T.-D. Chiueh, P.-Y. Tsai, and I.-W. Lai, Baseband receiver design for wireless MIMO-OFDM communications. John Wiley & Sons, 2012.
[29] R. H. Shumway and D. S. Stoffer, “Time series analysis and its applications with r examples,” Time series analysis and its applications with R examples, 2006.
[30] R. Van Nee, “Delay spread requirements for wireless networks in the 2.4 GHz and 5 GHz bands,” IEEE, vol. 802, pp. 802–22, 1997.
[31] W. S. Cleveland, “Robust locally weighted regression and smoothing scatterplots,” Journal of the American statistical association, vol. 74, no. 368, pp. 829–836, 1979.
[32] H. Scheffe, The analysis of variance, vol. 72. John Wiley & Sons, 1999.
[33] S.-J. Kim, K. Koh, S. Boyd, and D. Gorinevsky, “11 trend filtering,” SIAM review, vol. 51, no. 2, pp. 339–360, 2009.
[34] Y. Kozlov and T. Weinkauf, “PersistenceID: Extracting and filtering minima and maxima of 1d functions,” http://people.mpi-inf.mpg.de/weinkauf/notes/persistence1d.html, pp. 11–01, 2015.
[35] F. Bagala, C. Becker, A. Cappello, L. Chiari, K. Aminian, J. M. Hausdorff, W. Zijlstra, and J. Klenk, “Evaluation of accelerometer-based fall detection algorithms on real-world falls,” PloS one, vol. 7, no. 5, p. e37062, 2012.
[36] A. Papoulis and U. Pillai, Probability, random variables, and stochastic processes. McGraw-Hill, 2002.