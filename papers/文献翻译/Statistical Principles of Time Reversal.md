# 时间反演的统计原理 (Statistical Principles of Time Reversal)

统计效应表明，时间反演聚焦点并非一个点，而是具有贝塞尔功率分布。这一发现为在多径丰富且大多处于非视线条件下的室内环境提供了一种准确可靠的速度估计方法，并催生了诸如无线感知和跟踪等多种室内应用。目前尚无已知技术能在这种场景下表现出色。本质上，时间反演是一种有效工具，它将多径视为虚拟传感器，赋予我们成百上千的自由度来加以利用。

## 引言 (Introduction)

时间反演的物理原理早已为人所知，但在很长一段时间内，它主要局限于基础科学探索[1]或小众的国防应用，尤其是在水声通信领域[2]。由于对日常生活影响有限，公众对其知之甚少。直到21世纪初，随着商用射频 (radio frequency, RF) 带宽的增加，可解析的多径增多，利用时间反演技术面向广泛的消费级应用才成为可能[3]。

随着现代社会中射频无线电的普及，多径无处不在。与大多数现有技术常常试图消除多径不同，时间反演接纳并利用了多径。本质上，每条多径信号都沿着其传播路径携带有价值且独特的信息，使得每条多径信号都类似于一个虚拟传感器[4]。因此，可以想象，环绕在我们周围的大量多径可以作为随时可用的虚拟传感器，为我们提供前所未有的新自由度[5][4]。

物理学中的时间反演原理指出，当捕获足够数量的多径信号并将其时间反演后，它们会在波长尺度上汇聚成一个单一的焦点，汇聚来自每条多径的功率。具体来说，当接收机 (receiver, Rx) A 在一个多径环境中发射一个射频脉冲时，发射机 (transmitter, Tx) B 会接收到一个多径分布。如果 Tx B 将这个多径分布进行时间反演（即颠倒到达顺序，后到的先发，先到的后发），那么所有多径信号将在 Rx A 处同相相干地汇聚，能量精确地聚焦于一点[1]。正如我们将在后文看到的，这只是时间反演所能提供的开始。

## 从物理学到系统理论 (From physics to system theory)

从系统理论的角度来看，如图 1 所示，时间反演实现了从收发机 B 接收到的多径波形完美恢复到收发机 A 处的脉冲。存在成百上千条多径并不罕见。由于计算能力和精度的限制，在数字实现中无法实现如此完美的解卷积过程。值得注意的是，在无线通信中，通常只保留 5 到 10 条主要多径用于信道均衡[6]。

完美的解卷积意味着将信道均衡为无失真的脉冲响应。这表明，在双方进行时间反演操作时，周围的墙壁和障碍物仿佛消失了。如同双方在外太空通信，周围空无一物，它们之间没有任何阻隔。

可以推断，多径携带着宝贵的室内结构信息，并且可以被利用来克服障碍。考虑到每条多径在其传播路径中都包含独特信息，另一种观点是，聚焦点是来自所有多径的信息汇聚点。由于每一条多径都在其路径中包含特定信息，因此聚焦点也可以看作是除了功率之外，来自所有多径的信息汇聚地。

## 打破米级精度的诅咒 (Breaking the curse of meter accuracy)

令 $h(k;\bar{T} -\bar{R}_0)$ 表示从 $\bar{T}$ 到 $\bar{R}_0$ 的信道冲激响应 (channel impulse response, CIR) 的第 k 个抽头，其中 $\bar{T}$ 和 $\bar{R}_0$ 分别表示 Tx 和 Rx 的坐标。在时间反演传输中，位于 $\bar{R}_0$ 的 Rx 首先发射一个脉冲，位于 $\bar{T}$ 的 Tx 捕获从 $\bar{R}_0$ 到 $\bar{T}$ 的 CIR。然后，Tx 将捕获到的 CIR 的时间反演和共轭版本（即 $h^{*}(- k;\bar{R}_0\rightarrow \bar{T})$，其中 $*$ 表示复共轭）发射回去。根据信道互易性，即前向和后向信道相同，当发射时间反演波形 $h^{*}(- k;\bar{R}_0\rightarrow \bar{T})$ 时，在任意位置 $\bar{R}$ 接收到的信号可以写为：

$$s(k;\bar{R}) = \sum_{l = 0}^{L - 1}h(l;\bar{T} -\bar{R})h^{*}(l - k;\bar{R}_0\rightarrow \bar{T}) \quad (1)$$

其中 $L$ 表示可解析的多径数量。当 $\bar{R} = \bar{R}_0$ 且 $k = 0$ 时，我们有 $s(0;\bar{R}) = \sum_{l = 0}^{L - 1}\left|h(l,\bar{T} -\bar{R}_0)\right|^2$，所有多径分量 (multipath components, MPCs) 相干叠加；即，信号能量在特定时刻重新聚焦于特定的空间位置。这种现象被称为时间反演聚焦效应。

![图 1: 时间反演原理。](placeholder)

为了确定其是否对应精确的聚焦点，可以定义两个位置 $\bar{R}_0$ 和 $\bar{R}$ 的 CIR 之间的时间反演共振强度 (time reversal resonating strength, TRRS) [7] 为当发射针对位置 $\bar{R}_0$ 的时间反演波形时，接收信号的归一化能量，如底部公式 (2) 所示，其中当 $\bar{T}$ 固定时，我们使用 $\mathbf{h}(\bar{R})$ 作为 $h(l;\bar{T}\rightarrow \bar{R}), l = 0,\dots,L - 1$ 的缩写。

$$\eta(\mathbf{h}(\bar{R}_0), \mathbf{h}(\bar{R})) = \frac{\left| \sum_{l=0}^{L-1} h(l;\bar{T}\rightarrow \bar{R}_0) h^*(l;\bar{T}\rightarrow \bar{R}) \right|^2}{\left( \sum_{l=0}^{L-1} |h(l;\bar{T}\rightarrow \bar{R}_0)|^2 \right) \left( \sum_{l=0}^{L-1} |h(l;\bar{T}\rightarrow \bar{R})|^2 \right)} \quad (2)$$

这使得可以利用 CIR 或其傅里叶变换（在 Wi-Fi 环境中通常称为信道状态信息 (channel state information, CSI)）来确定精确匹配，从而无需直接测量设备上的物理功率即可实现精确定位。在后续讨论中，将使用 CSI 来指代多径分布。

在室内环境中，当带宽足够大，或者等效地说，多径数量非常大时，每个位置都将拥有独特的**多径分布签名**[8]，可用于室内定位目的。即使在 NLOS 环境下，使用 5.4 GHz Wi-Fi 工业、科学、医学频段，仅用单个 Tx 和 Rx，时间反演聚焦也能达到 1-2 厘米的精度。这是因为时间反演聚焦利用了数百甚至数千个自由度（多径），从而即使在完全 NLOS 环境下也能显著提高定位精度。

众所周知，可以利用飞行时间、信号强度和三角测量方法在无线网络中定位设备。然而，这些定位技术通常需要多个信号源，并且其精度通常仅限于米级，这种米级精度的诅咒源于三角测量只有三个自由度，而且这些技术仅在 LOS 条件下有效[9]。

相比之下，时间反演聚焦利用数百甚至数千个自由度（多径），实现了更高的定位精度，同时在完全 NLOS 环境下也能工作——多径越多越好，这与之前的科学信念和方法相反！当使用更高频段时，它可以实现毫米级的精度[9]。

## 对无线感知的暗示 (A hint to wireless sensing)

如上所述，不同位置具有独特的多径分布签名。类似地，环境的变化，例如门关闭或打开，可以显著影响多径分布，并导致时间反演聚焦点移动到不同位置。如果可以使用 TRRS 精确定位这两个位置，那么就有可能检测到诸如门打开或关闭之类的事件。这种现象也使得通过使用 CSI 来确定环境变化或监测事件成为可能。此外，它还可用于区分基于射频的人体生物特征。

然而，这种指纹方法有一个缺点：它需要通过匹配 CSI 来训练以确定事件。如果环境发生变化，即使是缓慢变化，也可能需要频繁重新训练，这限制了这些方案的实际应用。

那么，是否可以避免训练呢？后续讨论的时间反演聚焦效应的统计原理给出了答案。

## 迈向室内速度估计 (Toward indoor speed estimation)

速度估计是一种具有广泛应用的关键工具。室外环境提供了充足的 LOS 条件，雷达、望远镜和卫星可以利用多普勒效应直接观测或探测。然而，目前还没有简单可靠/准确的方法用于 NLOS 条件下的室内速度估计。我们看到，当存在大量多径时，时间反演聚焦点的功率分布表现出平稳特性，遵循贝塞尔函数分布[7]。这种特性可用于室内速度估计。

![图 2: 极坐标下的 MPC [7]，接收天线位于原点，每个 MPC 由点 A 表示。每个 MPC 的坐标由其到达角 $\theta$ 和总传播距离 $r$ 决定，$G(\omega)$ 表示功率增益，其中 $\omega = (r,\theta)$。](placeholder)

假设所有电磁 (electromagnetic, EM) 波在远场区域传播，每个 MPC 近似为一个平面电磁波。如图 2 所示，假设接收天线位于原点，每个 MPC 由点 $A$ 表示。每个 MPC 的坐标由其到达角 $\theta$ 和总传播距离 $r$ 决定，$G(\omega)$ 表示功率增益，其中 $\omega = (r,\theta)$。在富散射环境中，$\omega$ 可以假设在空间中均匀分布，MPC 总数很大，接收信号是入射电磁波电场的标量和。对于无源区域中具有恒定平均电场和磁场的任意点 $\tilde{R}$，当发射矩形脉冲时的 CIR 可以写为：

$$\begin{array}{l}{h(t;\tilde{T}\rightarrow \tilde{R}) =}\\ {\sum_{\omega \in \Omega}G(\omega)e^{j(2\pi f_0(t - \tau (\omega)) - \phi (\omega) - \tilde{k} (\omega)\cdot \tilde{R})}} \end{array} \quad (3)$$

其中 $\tau (\omega) = r / c$ 是 MPC $\omega$ 的传播时延；$f_{0}$ 是载波频率；$\Omega$ 是 MPC 的集合；$\phi (\omega)$ 是由反射引起的相位变化；$\tilde{k} (\omega)$ 是波矢量，幅度为 $k = f_0 / c$，$c$ 是光速。相应地，位置 $\tilde{R}$ 处采样 CIR 的第 $l$ 个抽头可以表示为底部公式 (4)，其中 $T$ 是信道测量间隔，$\Delta \tau (l,\omega) = lT - \tau (\omega)$，对于 $l = 0,1,\dots,L - 1$。

$$h(l;\tilde{T}\rightarrow \tilde{R}) = \sum_{\omega \in \Omega} G(\omega) e^{j(2\pi f_0(lT - \tau(\omega)) - \phi(\omega) - \tilde{k}(\omega)\cdot \tilde{R})} \text{sinc}(B \Delta \tau(l, \omega)) \quad (4)$$

当发射时间反演波形 $h^{*}(- l;\tilde{R}_0\rightarrow \tilde{T})$ 时，每个点 $\tilde{R}$ 处的接收信号可以从 (1) 近似为：

$$s(0;\tilde{R})\approx \sum_{l = 1}^{L}|G(m)|^{2}e^{-j\tilde{k} (m)\cdot (\tilde{R} -\tilde{R}_{0})}. \quad (5)$$

当带宽足够大时，不失一般性，假设多径从所有方向到达，使得 MPC 的功率分布在方向 $\theta$ 上是均匀的，即 $G(\omega)$ 仅取决于 $r$，并且对于任何 $\theta$ 近似相同[7]，则接收信号 $s(0;\tilde{R})$ 可以进一步近似为：

$$s(0;\tilde{R}) = \sum_{\omega \in \Omega}|G(\omega)|^{2}e^{-j\tilde{k} (\tilde{R} -\tilde{R}_{0})}$$ $$\qquad \approx \int_{0}^{2\pi}P(\theta)e^{-j\tilde{k} d\cos (\theta)}d\theta$$ $$\qquad = PJ_{0}(kd) \quad (6)$$

其中离散求和由连续积分近似，$\Omega$ 代表所有显著 MPC 的集合，$J_{0}(x)$ 是第一类零阶贝塞尔函数 $J_{0}(x) = 1 / 2\pi \int_{0}^{2\pi}\exp (- jx\cos (\theta))d\theta$，$d$ 是 $\tilde{R}_{0}$ 和 $\tilde{R}$ 之间的欧几里得距离。(6) 表明，时间反演聚焦点的功率分布具有贝塞尔函数分布。

由于实际功率测量不可行，我们可以借助 TRRS，它是一种互相关，允许我们通过 CSI 来测量这种效应。对于 $\tilde{R} = \tilde{R}_{0}$，它退化为 $d = 0$ 的情况，因此 $s(0;\tilde{R}_0)\approx P$，即聚焦点的峰值。因此，(2) 中定义的 TRRS 可以由下式给出[7]：

$$\eta (\mathbf{h}(\tilde{R}_0),\mathbf{h}(\tilde{R})) = J_0(kd). \quad (7)$$

本质上，这要求射频设备具有足够大的带宽来捕获大量的多径，而与周围的几何形状无关。值得注意的是，时间反演聚焦点不是一个点，而是具有一种与位置和环境无关的结构。这一特性使其成为室内速度估计的理想工具。

图 3 描述了在位置 1 和位置 2（相距 10 米）获得的实验结果。距离每个预定义焦点 $d$ 从 0 变化到 $2\lambda$。测得的 TRRS 分布函数与理论预测表现出良好的一致性。具体来说，测量曲线中波峰和波谷的位置与理论曲线中的位置相匹配。由于 TRRS 分布函数仅取决于两点之间的距离，在不滥用术语的情况下，我们可以简单地使用 $\eta (d) = J_0(kd)$ 来表示相距 $d$ 的两点之间的 TRRS 近似值。TRRS 分布函数 $\eta (d) = J_0(kd)$ 的形状仅由波数 $k$ 决定，与具体位置无关。因此，TRRS 分布函数可以作为测量空间距离的**内在标尺**。

例如，考虑一个 Rx 以恒定速度 $\nu$ 沿直线从位置 $\tilde{R}_{0}$ 开始移动，而 Tx 定期持续发射对应于 $\tilde{R}_{0}$ 的时间反演波形。在 Rx 处测得的 TRRS 是 $\eta (d)$ 的采样版本，并表现出类似贝塞尔函数的模式。我们可以通过测量 Rx 从 $\tilde{R}_{0}$ 到达第一个局部峰值所需的时间 $\bar{t}$ 来估计 Rx 的速度。速度可以估计为 $\hat{\nu} = (0.61\lambda) / \hat{t}$，其中 $d\approx 0.61\lambda$ 是理论上对应于类似贝塞尔函数模式第一个局部峰值的距离[7]。

许多现有技术，包括多普勒效应，在多径存在的情况下性能会下降，即使在 LOS 条件下也是如此。最终，现在可以在室内环境中准确可靠地估计速度和距离。与先前的科学方法相反，性能随着多径数量的增加而提高。

一个显而易见的应用是室内跟踪。物联网 (Internet of Things, IoT) 设备可以利用来自环境无线电的 CSI 来计算自身速度。通过利用智能手机和物联网设备中常见的惯性测量单元获取方向信息，结合地理位置图，可以实现对无限数量用户的跟踪[7]。

![图 3: 实验结果与理论结果中 TRRS 分布的比较 [7]，发射信号载频为 $5.8\mathrm{GHz}$，$\lambda = 5.17\mathrm{cm}$。](placeholder)

## 虚拟时间反演效应 (Virtual time reversal effect)

如上所述，一个设备（例如 Rx）移动时的速度估计是可行的。但是，在没有主动源产生物理时间反演效应的情况下，我们能做什么呢？图 4 表明，当一个物体在充满环境无线电波的空间中移动时，仿佛在物体上产生了多个以相同速度移动的动态散射体。每个动态散射体都可以看作是一个辐射功率低的虚拟无源源。因此，需要借助集体统计来揭示它们共同的集体行为。

![图 4: 室内富散射环境的多径模型，其中物体散射信号并产生多条路径 [10]。](placeholder)

根据电磁波的叠加原理，CSI $H(t,f)$ 可以分解为：

$$H(t,f) = \sum_{i\in \Omega_{s}(t)}H_{i}(t,f) + \sum_{j\in \Omega_{d}(t)}H_{j}(t,f) + \epsilon (t,f) \quad (8)$$

其中 $\Omega_{s}(t)$ 代表静态散射体的集合，$\Omega_{d}(t)$ 代表动态散射体的集合，$H_{i}(t,f)$ 表示第 $i$ 个散射体贡献的部分。噪声项 $\epsilon (t,f)$ 是加性高斯白噪声，方差为 $\sigma^{2}(f)$，且与 $H_{i}(t,f)$ 统计独立[11]。该分解基于以下事实：每个散射体就像一个“虚拟 Tx”，向各个方向散射电磁波，而接收天线处接收到的电磁波是室内物体反射后叠加的结果。因此，$H(t,f)$ 测量了所有入射电磁波电场的总和。在实际中，假设集合 $\Omega_{s}(t)$ 和 $\Omega_{d}(t)$ 在足够短的时间内变化缓慢，它们可以近似为时不变的集合。

不失一般性，我们考虑一个二维散射模型，所有散射体都在同一水平面内。由于信道互易性，双向传播的电磁波经历相同的物理扰动，如反射、折射和衍射。因此，如果 Rx 发射电磁波，在第 $i$ 个散射体或“虚拟 Tx”处“测量”到的 CSI 将与 $H_{i}(t,f)$ 相同。由于 $H_{i}(t,f)$ 实际测量的是入射电磁波的电场 $\tilde{E}_{i}(t,f) = \int_{0}^{2\pi}\tilde{F}_{i}(\theta ,f)\exp (- j\tilde{k}\cdot \tilde{v}_{i}t)d\theta$，其中第 $i$ 个散射体的速度为 $v_{i}$，则 $H_{i}(t,f)$ 可表示为：

$$\begin{array}{l}{H_{i}(t,f) =}\\ {\int_{0}^{2\pi}F_{i}(\theta ,f)\exp (-jkv_{i}\cos (\theta)t)d\theta} \end{array} \quad (9)$$

其中 $F_{i}(\theta ,f)$ 表示来自方向 $\theta$ 的 MPC 对于第 $i$ 个散射体的复信道增益，$k$ 是波数。

注意 $H_{i}(t,f)$ 的均值为零，表示为 $\mathbb{E}[H_{i}(t,f)] = 0$，其中 $\mathbb{E}[\cdot ]$ 表示期望算子。两个具有时间延迟 $\tau$ 的 CSI 值的协方差可表示为[11]：

$$\begin{array}{rl} & {\mathrm{Cov}\big[H_i(t,f),H_i(t + \tau ,f)\big]}\\ & {\quad = \mathbb{E}\big[H_i(t,f)H_i^{*}(t + \tau ,f)\big]}\\ & {\quad \approx 2\pi \sigma_{H_i}^{2}(f)J_0(kv_i\tau)} \end{array} \quad (10)$$

其中 $J_{0}(\cdot)$ 是第一类零阶贝塞尔函数，$\sigma_{H_i}^{2}(f)$ 是 $F_{i}(\theta ,f)$ 的方差，并且假设 $F_{i}(\theta ,f)$ 是对于 $\forall \theta$ 具有相同方差的循环对称高斯随机变量。$H_{i}(t,f)$ 在时间延迟 $\tau$ 下的自相关函数 (autocorrelation function, ACF)，记为 $\rho_{H_{i}}(\tau ,f)$，推导为：

$$\begin{array}{rl} & {\rho_{H_i}(\tau ,f) = \frac{\mathrm{Cov}[H_i(t,f),H_i(t + \tau,f)]}{\mathrm{Cov}[H_i(t,f),H_i(t,f)]}}\\ & {\qquad = J_0(kv_i\tau).} \end{array} \quad (11)$$

考虑只有一个物体在移动，并且该物体引起的所有散射体的速度近似相同，即对于 $\forall i\in \Omega_{d}$，有 $v_{i} = v$。那么，CSI $H(t,f)$ 在时间延迟 $\tau$ 下的 ACF，记为 $\rho_{H}(\tau ,f)$，可得为：

$$\rho_{H}(\tau ,f) = \alpha (f)J_{0}(kv\tau) \quad (12)$$

其中 $\alpha (f)$ 是每个子载波 $f$ 的增益。当考虑所有频率时，$\alpha (f)$ 因子将被聚合而消失。方程 (12) 等价于 (7)，唯一的区别在于 (7) 是从功率分布的角度出发，且 $d = v\tau$。

## 反思 (Reflection)

ACF 也遵循贝塞尔函数并不奇怪，因为它也是二阶统计量，与 TRRS 一样，并共享相同的数学表达式。本质上，一个移动的物体可以被视为一组虚拟源，它们的组合强度可以通过统计电磁学揭示出来，这是虚拟时间反演聚焦效应的结果[9][11]。

现在，有源和无源源被统一到一个时间反演框架中，我们可以断言，无论是主动源的 TRRS 还是被动源的 ACF，其二阶统计量都表现出贝塞尔函数分布，这是由于聚焦点处的时间反演贝塞尔功率分布。我们可以简单地写为：

$$\rho (\tau) = J_0(kv\tau) \quad (13)$$

其中 $\rho (\cdot)$ 代表时间反演效应的二阶统计量，$\tau$ 是时间延迟，$k = 2\pi f / c$ 是波数，$v$ 是速度。在多径条件下，即使在室内 NLOS 环境下，也可以准确可靠地估计速度。

如果重新审视 (6)，会发现当存在许多从各个方向均匀到达的多径时，该方程成立，这正是多径丰富的室内场景。这意味着，即使没有进行时间反演操作，Rx 处的接收功率分布也应该遵循贝塞尔分布。Clarke 很久以前就观察到[12]，多径丰富环境中电场的空间自协方差函数是贝塞尔函数。但他未能得出结论，认为这种现象是由于贝塞尔函数的功率分布所致。一个原因是没有人像在时间反演聚焦的特定时刻那样去观察它。在通信中，信号不断到来，每个贝塞尔功率分布被收集和聚合，导致只能看到功率包络；即，每个单独贝塞尔函数的影响消失了。时间反演允许我们观察所有多径汇聚在一起清晰展现贝塞尔效应的特定瞬间。但在每一瞬间，仍然有所有的多径到来并混合在一起，尽管不像时间反演那样完全同相，但功率分布仍然是贝塞尔的。这有助于解释为什么 Clarke 观察到的多径条件下的空间相关是贝塞尔函数。

![图 5: 不同活动和受试者（受试者1和受试者2）的速度和加速度：(a) 跌倒，(b) 站起，(c) 坐下，(d) 捡东西 [11]。](placeholder)

## 实现可靠的无线感知 (Enabling reliable wireless sensing)

关系式 $\rho (\tau) = J_0(k\nu \tau)$ 可以使用 CSI 进行评估，而 CSI 在无处不在的 Wi-Fi 和 5G/6G 系统的环境无线电波中很容易获得[13]。跌倒是老年护理中的一个重要问题，因为它可能带来严重后果。跌倒的特征在于其独特的运动模式，身体持续加速然后突然猛烈减速，这使其与其他典型的人体运动区分开来（见图 5）。只要能准确测量速度，就可以利用这一独特特征进行准确的跌倒检测[11]。

尽管人类是双足行走，但每个人的行走方式都独具特色。可以从速度信息中推断出步态模式[10]（见图 6），包括步幅长度和步行节奏。人体运动也可以从宠物或机器（例如 iRobot、风扇等）的运动中推断出来。运动统计量可用于灵敏地检测轻微的人体运动，用于存在检测或入侵检测，以及用于自动控制灯泡、电视和空调以实现能源管理。呼吸也可以被轻松监测以进行睡眠监测，所有这些都无需穿戴任何设备。

毫米波无线电可用于拾取心跳以推断心率变异性，用于声音增强或分离，以及其他应用，如手势、手写和击键识别，所有这些都仅使用商用 Wi-Fi，无需穿戴设备。

![图 6: 步态周期，定义为行走过程中同一只脚后跟两次连续着地事件之间的持续时间。在正常人类行走过程中，受试者的速度先增加后减少，导致每一步都有一个速度峰值 [10]。载频为 5.8 GHz，带宽 40 MHz，采样率 1500 Hz。](placeholder)

## 与大规模多输入多输出的关系 (Relation to massive multiple input, multiple-output)

我们已经看到一种强大/通用的基于时间反演的统计工具，通过将（室内）大量多径视为虚拟传感器来进行速度估计。将利用成百上千个虚拟传感器与大规模多输入多输出 (massive multiple-input, multiple-output, MIMO) 中常用的大量真实天线进行比较是否可行？为了刻画聚焦效应的功率分布并研究其与目标运动的关系，类似于前述，我们可以定义大规模 MIMO 中接收信号的 ACF。

假设一个 Rx 位于移动物体/目标上，接收来自具有 $M$ 个天线的基站发射的信号，其中 $\mathbf{r}_i$ 表示时间 $t$ 时 Rx 的位置，$A_{e}$ 表示天线孔径。接收信号包括 LOS 和 NLOS 分量，可以表示如下[14]：

$$\begin{array}{l}{y(t) = y_{L}(t) + y_{N}(t) + n(t)}\\ {y_{L}(t) = \sqrt{K_{L}}\sum_{m = 1}^{M}\frac{\exp(j(k\mid \mathbf{x}_{m}-\mathbf{r}_{t}\mid + \phi_{m}))}{4\pi\mid \mathbf{x}_{m}-\mathbf{r}_{t}\mid}}\\ {y_{N}(t) = \sqrt{K_{N}}\sum_{n = 1}^{N}\exp [j(\omega_{d}t\cos \alpha_{n} + \phi_{n})]} \end{array} \quad (14)$$

其中 $y_{L}(t)$ 和 $y_{N}(t)$ 分别表示具有功率系数 $K_{L}$ 和 $K_{N}$ 的 LOS 和 NLOS 分量；$N$ 是 NLOS 信号分量的总数；$k$ 表示波数；$\omega_{d}$ 是最大多普勒频率；$\mathbf{x}_{m}$ 和 $\mathbf{r}_t$ 分别是第 $m$ 个天线和 Rx 在时间 $t$ 的坐标；$|\mathbf{x}_{m} - \mathbf{r}_{t}|$ 表示 $\mathbf{x}_{m}$ 和 $\mathbf{r}_t$ 之间的距离；$n(t)$ 是加性高斯噪声；$\phi_{m}$ 是第 $m$ 个 LOS 信号的相位畸变；$\alpha_{n}$ 和 $\phi_{n}$ 是第 $n$ 个 NLOS 信号分量的到达方向和相位畸变。通常，可以假设 $\phi_{m}$、$\alpha_{n}$ 和 $\phi_{n}$ 在 $[- \pi ,\pi)$ 上独立同分布。

基于 (14)，接收信号的 ACF 可以表示为：

$$\eta_{y}(\mathbf{r}_{0},\mathbf{r}_{s}) = \mathbb{E}\left[y(t_{0})y^{*}(t_{s})\right]$$ $$\approx \eta_{y_{L}} + \eta_{y_{N}} + \eta_{n} \quad (15)$$

其中 $\eta_{y_{N}} = K_{N}N J_{0}(kd)$ 类似于“迈向室内速度估计”部分中的推导，$\mathbb{E}$ 表示期望算子，$J_{0}(\cdot)$ 是零阶贝塞尔函数，$d$ 表示 $\mathbf{r}_{0}$ 和 $\mathbf{r}_{s}$ 之间的距离。LOS 信号的 ACF 可由下式给出[14]：

$$\eta_{y_{L}} = \frac{A_{e}\exp(jk\epsilon)}{z}\mathrm{sinc} \left(\frac{k\xi A_{e}}{2L}\right) \quad (16)$$

其中 $\epsilon$ 和 $\xi$ 分别代表 $\mathbf{r}_{0}$ 和 $\mathbf{r}_{s}$ 之间的径向距离和横向距离，$z$ 代表发射天线阵列中的阵元间距，$\mathrm{sinc}(t) = \sin (t) / t$ 是辛格函数，这是在远场条件下得到的。那么，接收信号的 ACF 可以表示为：

$$\eta_{y}(\mathbf{r}_{0},\mathbf{r}_{s}) = \frac{A_{e}\exp(jk\epsilon)}{z}\mathrm{sinc} \left(\frac{k\xi A_{e}}{2L}\right) + K_{N}N J_{0}(kd) + \sigma^{2}\mathbf{I} \quad (17)$$

其中 $\sigma^{2}$ 是噪声 $n(t)$ 的功率谱密度。

文献[14]已证明，$K_{N}N J_{0}(kd)$ 的衰减速度远快于 $\mathrm{sinc}(k\xi A_{e} / 2L)$。因此，LOS 分量主导效应，$J_{0}(kd)$ 项可以忽略。当没有 LOS 分量时，它就退化为前几节讨论的相同的时间反演效应。在任何一种情况下，都可以类似地进行速度估计。

## 结论 (Conclusions)

大多数现有技术依赖于 LOS 观测，并试图最小化多径传播的影响。相比之下，时间反演接纳并依赖于多径——越多越好，这与大多数现有范式相反。它提供了准确可靠的速度估计，并揭示了非常适合室内应用的新思维和解决方案，特别是在由于多径效应而长期被认为困难和具有挑战性的 NLOS 条件下。

Peter Siegel 在[9]中指出，时间反演的统计效应可以用简单而优美的方程 $\rho (\tau) = J_{0}(k\tau)$ 来描述，这代表了“对近两个世纪以来寻找新物理学的探索的独特解决方案，这种新物理学可以等同于，并且在某些情况下超越，由 Christian Doppler 于 1842 年首次描述的移动物体与波源之间普遍存在的频率与速度关系。”时间反演在多径环境中表现出色，而另一方面，多普勒效应则不需要多径。因此，两者相辅相成，提供了两种不同的方法，为未来应用解锁无限可能。

## 作者 (Authors)

**K. J. Ray Liu** (ray.liu@originwirelessai.com) 是美国马里兰州罗克维尔 Origin Wireless 的创始人、董事长兼首席技术官。他曾是马里兰大学学院帕克分校的杰出大学教授，研究贡献涵盖信号处理和通信的广泛领域。他曾任 2022 年 IEEE 主席兼首席执行官，并荣获 2021 年 IEEE Fourier 信号处理奖。他是 IEEE 会士。

**Beibei Wang** (beibei.wang@originwirelessai.com) 于 2009 年获得马里兰大学学院帕克分校电气工程博士学位。她是美国马里兰州罗克维尔 Origin Wireless 的研究副总裁兼知识产权总监。她已提交超过 110 项专利申请，其中 50 多项已获授权。

## 参考文献 (References)

[1] M. Fink, "Time reversal of ultrasonic fields. I. Basic principles," *IEEE Trans. Ultrason., Ferroelectr., Freq. Control*, vol. 39, no. 5, pp. 555-566, Sep. 1992, doi: 10.1109/58.156174.
[2] W. Kuperman, W. S. Hodgkiss, H. C. Song, T. Akal, C. Ferla, and D. R. Jackson, "Phase conjugation in the ocean: Experimental demonstration of an acoustic time-reversal mirror," *J. Acoustical Soc. America*, vol. 103, no. 1, pp. 25-40, 1998, doi: 10.1121/1.423233.
[3] B. Wang, Y. Wu, F. Han, Y.-H. Yang, and K. J. R. Liu, "Green wireless communications: A time-reversal paradigm," *IEEE J. Sel. Areas Commun.*, vol. 29, no. 8, pp. 1698-1710, Sep. 2011, doi: 10.1109/JSAC.2011.110918.
[4] K. J. R. Liu and B. Wang, *Wireless AI: Wireless Sensing, Positioning, IoT, and Communications*. Cambridge, U.K.: Cambridge Univ. Press, 2019.
[5] B. Wang, Q. Xu, C. Chen, F. Zhang, and K. J. R. Liu, "The promise of radio analytics: A future paradigm of wireless positioning, tracking, and sensing," *IEEE Signal Process. Mag.*, vol. 35, no. 3, pp. 59-80, May 2018, doi: 10.1109/MSP.2018.2806300.
[6] K. Cheun, "Performance of direct-sequence spread-spectrum rake receivers with random spreading sequences," *IEEE Trans. Commun.*, vol. 45, no. 9, pp. 1130-1143, Sep. 1997, doi: 10.1109/26.623079.
[7] F. Zhang, C. Chen, B. Wang, H.-Q. Lai, Y. Han, and K. J. R. Liu, "WiBall: A time-reversal focusing ball method for decimeter-accuracy indoor tracking," *IEEE Internet Things J.*, vol. 5, no. 5, pp. 4031-4041, Oct. 2018, doi: 10.1109/JIOT.2018.2854825.
[8] Z.-H. Wu, Y. Han, Y. Chen, and K. J. R. Liu, "A time-reversal paradigm for indoor positioning system," *IEEE Trans. Veh. Technol.*, vol. 64, no. 4, pp. 1331-1339, Apr. 2015, doi: 10.1109/TVT.2015.2397437.
[9] P. H. Siegel, "IEEE President K. J. Ray Liu, 'follow multiple paths,' changing the world with microwave time reversal focusing," *IEEE J. Microwaves*, vol. 2, no. 3, pp. 360-373, Jul. 2022, doi: 10.1109/JMW.2022.3181498.
[10] C. Wu, F. Zhang, Y. Hu, and K. J. R. Liu, "GaitWay: Monitoring and recognizing gait speed through the walls," *IEEE Trans. Mobile Comput.*, vol. 20, no. 6, pp. 2186-2199, Jun. 2021, doi: 10.1109/TMC.2020.2975158.
[11] F. Zhang, C. Chen, B. Wang, and K. J. R. Liu, "WiSpeed: A statistical electromagnetic approach for device-free indoor speed estimation," *IEEE Internet Things J.*, vol. 5, no. 3, pp. 2163-2177, Jun. 2018, doi: 10.1109/JIOT.2018.2826227.
[12] R. H. Clarke, "A statistical theory of mobile-radio reception," *Bell Syst. Tech. J.*, vol. 47, no. 6, pp. 957-1000, Jul./Aug. 1968, doi: 10.1002/j.1538-7305.1968.tb00069.x.
[13] C. Wu, B. Wang, O. C. Au, and K. J. R. Liu, "Wi-Fi can do more: Toward ubiquitous wireless sensing," *IEEE Commun. Standards Mag.*, vol. 6, no. 2, pp. 42-49, Jun. 2022, doi: 10.1109/MCOMSTD.0001.2100111.
[14] X. Zeng, F. Zhang, B. Wang, and K. J. R. Liu, "Massive MIMO for high-accuracy target localization and tracking," *IEEE Internet Things J.*, vol. 8, no. 12, pp. 10,131-10,145, Jun. 2021, doi: 10.1109/JIOT.2021.3050720.