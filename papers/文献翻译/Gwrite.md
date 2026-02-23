# GWrite：利用 WiFi 实现穿墙手势书写识别

Sai Deepika Regani, Beibei Wang, _Senior Member, IEEE_, Yuqian Hu, and K. J. Ray Liu

**摘要**— 识别空中手势能够实现智能人机交互应用并便利人类生活。然而，现有的基于传感器/摄像头的手势识别方法要么不普适、侵犯隐私，要么不便携带。当前的免设备方法要求人员在传感器的视线范围内且靠近传感设备。本文表明，即使手势位置处于 WiFi 收发器的非视线/墙后，WiFi 信号也能识别手绘的空中手势。所提出的 GWrite 系统利用商用 WiFi 芯片组的信道状态信息时间序列数据。GWrite 采用一种独特的手势执行方式，从而能够设计出手部运动模型。利用该模型和时间反转技术，本文推导出 CSI 的相似性与手部相对移动距离之间的对应关系。这种关系催生了独特的特征，如线段数量、角度以及线段之间的交点，这些特征可以对由直线段组成的一组手势形状进行分类。GWrite 在 15 个手势的集合上达到了 92% 的准确率。与当前仅在有限手势集上运行的系统不同，所提出的方法可应用于更广泛的手势集。

**关键词**— 信道状态信息（CSI），手势模型，人机交互（HCI），时间反转共振强度（TRRS），WiFi 感知。

## I. 引言

随着设备和计算机正在改变我们周围的空间。人机交互（HCI）构成了我们日常生活的重要部分，需要尽可能简化它。随着时间的推移，世界各地的研究人员提出了创新的解决方案和方法来简化 HCI。我们已经从手动操作发展到按钮、触摸传感器、运动传感器，再到更花哨的指纹和面部识别认证系统。我们从使用键盘输入发展到使用空中手势和语音命令与计算机对话。这些努力突显了我们渴望简化与计算机的交互，便利人类生活。

空中手势识别能轻松融入未来的智能环境。研究人员尝试了不同的信号来构建实用的手势识别系统。基于视觉的方法通过跟踪手/手指的运动来重建手部轨迹。像 Microsoft Kinect 和 LeapMotion 这样的商业应用在游戏领域变得非常流行。基于视觉的方法专门针对视线（LOS）应用，需要环境光，并可能引发隐私问题。基于传感器的技术为手势识别提供了可靠的解决方案，并且已经开发了许多此类系统 [1], [2]。然而，为了获得更好的用户体验，被动式或免设备的方法更受青睐。一些工作使用 RF 信号来跟踪手部运动并执行手写/手势识别 [1]，这通常需要专用硬件并产生额外成本。另一方面，基于被动 WiFi 的方法拥有最合适的特性，例如轻松穿透墙壁从而放宽 LOS 限制、无处不在的基础设施、保护隐私以及更好的用户体验，适用于许多令人兴奋的应用，包括实用的手势识别 [3], [4], [5], [6], [7]。在这项工作中，我们提出了 GWrite，一个使用商用 WiFi 设备的免设备手势书写识别系统，它可以在穿墙场景下工作。构建这样的系统面临着多重挑战。

首先，鉴于商用 WiFi 系统的带宽较低（20-80 MHz），很难分离出感兴趣信号，即对应于移动手部的信号。例如，对于 40 MHz 带宽，所有路径长度相差 7.5m 的路径都叠加在一个信道冲激响应（CIR）抽头上。因此，过去的工作直接在训练和测试阶段之间，为一组预定义的手势和位置匹配信道状态信息（CSI）时间序列。这种方法有几个缺点，后来文献中用“位置/用户依赖性”和“模式不一致性”等术语指出了这些问题。由于难以在时间上分离信号，也无法使用诸如多普勒速度或飞行时间（ToF）等特征来确定目标移动的速度/距离。此外，由于非视线（NLOS）设置和缺乏天线阵列，空间信息也无法提取。通过 GWrite，我们提出了一种规避上述限制的方法。我们不在在线和离线阶段直接匹配 CSI，而是建立手部移动的相对距离与 CSI 时间序列内相似性值下降之间的对应关系。虽然缺少绝对方向/朝向信息，但我们使用描述手势轨迹相对形状的特征来执行手势分类。

===== Page 2 =====

1) 我们提出了 GWrite，一个手势书写识别系统，它可以在穿墙环境下工作，并识别由直线段组成的手势形状。我们已经开发了这样一个系统的完整流程，包括手势分割、转角分类和交点检测。

<center>图 1. 典型的穿墙实验设置。 </center>

其次，商用 WiFi 要么工作在 2.4 GHz 频段，要么工作在 5 GHz 频段，在这些频段电磁波（EM）可以轻松穿透墙壁并覆盖典型的室内空间。而且，它们不易随距离衰减。虽然这些特性使其成为穿墙应用的理想选择，但从手部反射的信号完全被淹没，并且在室内环境强烈的多径中引起极小的扰动。这种行为迫使大多数先前的工作将其覆盖范围限制在 LOS 或约 1m 的距離內，以使感兴趣信号主导来自静态环境的其他无关信号。我们通过移除静态环境/散射体的贡献，并使用最大比合并（MRC）来增强手部运动产生的动态信号并抑制噪声来解决这个问题。一个典型的穿墙实验设置如图 1 所示。

最后，在手势过程中，手的不同部分以不同的速度运动，并且对信道扰动的贡献相当。因此，为手势建立物理模型变得具有挑战性。过去，研究人员要么模拟侧重于躯干速度的全身运动 [8]，要么使用更可控的手指手势 [9]。不同地，在这项工作中，我们通过指定一种独特的手势执行方法，并推导出手部运动程度与 CSI 之间相似性衰减相关的数学表达式，提出了一种手势模型。

GWrite 系统的核心是 WiFi 信道时间变化的统计模型。该模型使我们能够简化两个 CIR 之间采用的相似性度量的表达式。所提出的独特手势进一步帮助我们推导出手部移动的相对距离与相似性衰减之间的对应关系。利用这一特性，我们为包含直线段的手势形状提取了可区分的特征。我们设计了算法来自动识别直线段、确定两段之间的角度以及识别段之间的交点。通过累加每个特征的概率分数来计算每个手势形状的似然度。手势形状被识别为具有最高似然分数的那个。这项工作的贡献可以总结如下：

1) 我们提出了 GWrite，一个手势书写识别系统，它可以在穿墙环境下工作，并识别由直线段组成的手势形状。我们已经开发了这样一个系统的完整流程，包括手势分割、转角分类和交点检测。

2) 我们提出了一个独特的手势过程中手部运动模型。利用该模型，我们推导了物理环境中的扰动与穿墙环境下 CSI 时间序列内相似性之间的关系。我们相信这样的理解将为开发实用的手势识别系统开辟多样化的机会，并扩展当今仅局限于 LOS 设置的现有基于 WiFi 的系统。
3) 我们使用商用现货（COTS）WiFi 设备构建了所提出系统的原型，并使用一组 15 个大写英文字母评估了性能。实现的分类准确率为 92%，随着更多带宽变得可用，这一准确率可以进一步提高。

本文的其余部分组织如下。第二部分总结了先前关于手势识别的工作。第三部分介绍了所提出的手势模型和时间反转共振强度（TRRS）衰减的概念。第四部分描述了提取手势轨迹特征和分类手势的算法。第五部分展示了 GWrite 的性能评估。第六部分讨论了微基准测试和各种因素的影响，最后，第八部分总结了本文。

## II. 相关工作

关于被动手势识别的研究已经走过了很长一段路。人们观察到，环境中的任何物理运动/动作都会引起无线信道及其相关多径分布的扰动。研究人员开始分析这些变化，并将其与相应活动联系起来，以设计有用的应用。这方面的初步工作使用接收信号强度指示器（RSSI）来实现不同的应用，包括手势识别、目标跟踪和呼吸估计 [10], [11], [12], [13]。然而，RSSI 是一个单一的标量值，只能提供关于导致信道变化的活动的大致信息，并且对环境变化也很敏感。随着用于商用 WiFi 设备的 CSI 工具的发布 [14]，信道测量的细粒度信息变得可用，为实现新应用和改进此前依赖 RSSI 的系统开辟了许多机会。

当物理环境相对不变时，给定的运动/活动会产生可重复的 CSI 时间序列。因此，许多工作在训练阶段维护 CSI 模式的数据库，并在测试阶段使用相似性度量（如动态时间规整（DTW）或神经网络）与 CSI 进行比较 [15]。虽然这种方法理论上应该有效，但实际上要为可能的各种位置、用户和环境构建一个 CSI 原型训练数据库是不可能的。此外，应用仅限于一小部分具有可用 CSI 模式的预定义手势。一些工作尝试通过对 CSI 进行后处理，使用不同的滤波和特征选择技术来克服这个问题，例如

===== Page 3 =====

REGANI等人：GWrite：利用WiFi实现穿墙手势书写识别
5981
图 2.
基于WiFi的手势识别方法的演变。每个阶段的改进（推动当前穿墙鲁棒手势识别的发展）以粗体突出显示。
如主成分分析（PCA）、子载波选择、小波变换和低通
滤波 [16], [17], [18], [19], [20]。然而，这些尝试
未能成功获得表征运动模式的与位置无关的特征。
后来使用了诸如手势方向 [21] 和重复次数 [22] 等特征，这些特征是
与位置无关的。这种方法也适用于 NLOS 和
穿墙设置，从而放宽了对设备
放置位置的限制。然而，使用这些特征，可以区分的
手势数量是有限的。考虑到如今我们周围有大量智能
设备，交互需要更广泛的手势集，并为用户
增加更多灵活性。实现更全面的可识别手势集的直观方法
是设计能够描述手部运动原始轨迹的特征，或者
换句话说，能够重建该轨迹的特征。
为了重建手势轨迹，需要知道手部随时间变化的
相对位置。为此，人们使用了诸如飞行时间（ToF）（用于确定手部
到接收器的距离/范围）、到达角（AOA）[23], [24]（用于确定
角度位置）和多普勒频移（用于确定手部的
径向速度）[25] 等信息。然而，
所有这些方法都需要修改硬件或严格的 LOS
设置，且操作范围小。WiDraw 依靠来自多个接入点（AP）的信号强度来确定手部在不同时刻的位置并构建轨迹 [24]。在
FingerDraw [9] 中，通过使用两个正交放置的接收器实现了厘米级精度的
手指跟踪。这些系统仅在较小范围内有效（<1 m）。随着手部与
收发器之间距离的增加，分离手部反射信号
变得更加困难。

通过 GWrite，我们提出了一种穿墙手势书写
识别系统，该系统可以提取手势轨迹的信息，
从而促进大量可识别手势集的形成。此外，时间反转（TR）技术结合独特的手势模型，使我们能够
提取重建手势形状的特征。图 2 展示了基于 WiFi 的手势识别方法如何随时间演变的总结。

## III. GWRITE 手势模型

在本节中，我们开发了一个统计模型来解释
手部手势引起的无线信道变化。一个无线信道实例可以由
其 CSI 或其等效的 CIR 来表征。一个手势是手部的连
续运动，因此可以由一个 CIR/CSI 时间序列表示。为了在手部
运动和无线信道变化之间建立对应关系，量化
时间序列中任意两个 CSI 之间的关系/相似性是有用的。为此，我们采用了 TRRS 指标 [26]。
假设发射器（Tx）和接收器（Rx）之间的无线信道由 CIR $h(l)$ 表示，其中
$l = 0, 1, \ldots, L-1$，$L$ 是总抽头数。同时，
假设 CIR 已归一化，即 $\sum_{l=0}^{L-1} |h(l)|^2 = 1$。如果
Tx 发送一个冲激 $\delta(l)$，Rx 接收到的信号为 $y(l) = h(l)$。注意，遵循信道互易性，从 Tx 到 Rx 和从 Rx 到 Tx 的无线信道可视为等效。现在，如果 Rx 发送 $h^*(-l)$，即信道的时间反转和共轭版本，那么 Tx 处接收到的信号变为 $y'(l) = h^*(-l) \star h(l)$，其中 $\star$ 表示卷积。$y'(l)$ 的第零个抽头变为
$$y'(0) = \sum_{l=0}^{L-1} h^*(l)h(l) = \sum_{l=0}^{L-1} |h(l)|^2 = 1$$
(1)
也就是说，通过从 Rx 发送初始 CIR 的时间反转和共轭版本并记录
授权许可使用仅限于：浙江大学。下载于 2024年5月27日 04:15:49 UTC 从 IEEE Xplore 获取。限制适用。

===== Page 4 =====

Tx 处的 $y'(0)$，我们可以知道稍后时刻的信道是否与初始信道不同。如果 $y'(0) \neq 1$，则可以得出结论：信道已改变。或者，我们可以计算

$$\eta (\mathbf{h}_1,\mathbf{h}_2) = \left| \frac{\sum_{l = 0}^{L - 1}h_1(l)h_2(l)^*}{\sqrt{\sum_{l = 0}^{L - 1}|h_1(l)|^2\sqrt{\sum_{l = 0}^{L - 1}|h_2(l)|^2}}\right| \quad (2)$$

它给出了两个 CIR $\mathbf{h}_1$ 和 $\mathbf{h}_2$ 之间的 TRRS。$\eta$ 的值介于 0 和 1 之间，其中 0 表示一对不相关的 CIR。评估信道中物理扰动的程度可能比仅仅检测变化能揭示更多关于手部运动的信息。让我们进一步分析手势情况下的方程 (2)，以了解 TRRS 下降与手部运动程度之间的关系。

令 $\mathbf{h}_0$ 表示手部在初始位置时的 CIR。由于室内环境中的多次反射，CIR 的每个抽头由多个多径分量（MPC）组成，可以明确地写为

$$h_0[l] = \sum_{m\in M}\zeta_{l,m}e^{-j2\pi f_c\tau_{0,l}(m)} \quad (3)$$

其中 $M$ 是多径集合，$\zeta$ 是复数路径增益，$f_{c}$ 是载波频率，$\tau$ 是路径延迟。令 $\mathbf{h}_x$ 表示手部移动一小段距离 $x$ 后的 CIR。那么

$$h_0[l]h_x[l]^* = \sum_{m\in M}\zeta_{l,m}e^{-j2\pi f_c\tau_{0,l}(m)}\sum_{m\in M}\zeta_{l,m}^* e^{j2\pi f_c\tau_{x,l}(m)} \quad (4)$$

假设在短时间内集合 $M$ 被认为不变。假设带宽足够大，使得显著的 MPC 被捕获在不同的 CIR 抽头上 [26]，我们可以近似 $\eta (\mathbf{h}_0, \mathbf{h}_x)$ 的分子为

$$\left|\sum_{l = 0}^{L - 1}h_0[l]h_x[l]^*\right|\approx \left|\sum_{l = 0}^{L - 1}|\zeta_l|^2 e^{j2\pi f_c(\tau_{x,l} - \tau_{0,l})}\right|. \quad (5)$$

方程 (5) 并未表明移动距离 $(x)$ 与 TRRS 值的下降/衰减之间存在直接关系。为了使这种对应关系更加明显，我们利用以下观察结果简化方程。

<center>图 3. 当反射器移动一小段距离 $x$ 时，多径长度差异的示意图。</center>

**路径长度差**：当手部移动一小段距离 $x$ 时，路径长度的变化可以使用反射角 $\nu_{l}$ 以及表面法线与移动方向之间的角度 $\psi_{l}$ 来计算，如图 3 所示。$F_{1}$ 和 $F_{2}$ 是 Tx 和 Rx 的位置，可以视为椭圆的焦点。根据椭圆的几何定义，沿表面的小位移不会影响路径的总长度。因此，总路径长度的变化仅取决于沿反射表面/椭圆法线方向的位移。当手部移动距离 $x$ 时，路径 $l$ 的延迟差可以写为

$$\tau_{x,l} - \tau_{0,l} = \frac{2x\cos\psi_l\cos\nu_l}{c}. \quad (6)$$

**均匀散射**：在具有大量多径的丰富室内环境中，我们可以假设 MPC 在空间中均匀分布。这允许我们将 MPC 在空间中任意点的入射角建模为在 $(0, 2\pi]$ 上均匀分布，将 (5) 中的求和转换为对角度进行积分，如下所示

$$\sum_{l = 0}^{L - 1}h_0[l]h_x[l]^*\approx K\int_{\nu_l = 0}^{2\pi}\int_{\psi_l = 0}^{2\pi}e^{(j2\pi f_c\frac{2x\cos\psi_l\cos\nu_l}{c})}d\nu_l d\psi_l \quad (7)$$

其中 $K$ 是归一化常数。

**多径比例**：在密集的多径中，只有一部分 MPC 受到手部运动的影响。如果我们将静态环境反射贡献的多径比例记为 $T_{S}$，则 (7) 的右边可以写为

$$\begin{array}{l}{K\int_{\nu_l = 0}^{2\pi}\int_{\psi_l = 0}^{2\pi}T_S + (1 - T_S)e^{j2\pi f_c\frac{2x\cos\psi_l\cos\nu_l}{c}}d\nu_l d\psi_l}\\ {= \mathrm{KT}_S(2\pi)^2 +K(1 - T_S)(2\pi)^2 J_0^2\left(\frac{2\pi f_cx}{c}\right)} \end{array} \quad (9)$$

其中 $J_{0}(.)$ 是第一类零阶贝塞尔函数 [27]。

**伸展的手势**：在手势过程中，手的不同部分以不同的速度和方向运动。因此，对手部任意运动进行建模是很复杂的。在这项工作中，我们提出了一种独特的手势执行方式，如图 4 所示，手臂伸展并以肩部 $O$ 为支点旋转。我们将在第六节 E 部分展示在放宽此约束时 GWrite 的性能。这种受约束运动的含义有三点。

1) 手臂所有部分的空间方向可以根据手腕相对于肩部的位置来确定。
2) 移动散射体（手的不同部分）的轨迹是唯一的，不会相互重叠/交叉。在一个手势中，手臂某部分重复某个空间位置的唯一方式是整个手臂重复其位置。

===== Page 5 =====

3) 当手按图 4 移动时，手可以建模为多个散射体的组合，其位移从 0 线性增加到 $x$，其中 $x$ 是手腕的位移。如果手腕位移为 $x$，那么手臂上距肩部距离为 $d$ 的点位移为 $(dx / D)$，其中 $D$ 是手臂的总长度。(9) 中的动态多径部分可以进一步细分为沿手臂长度的 $N_D$ 个散射体的贡献。这导致 (9) 的第二项中出现求和，其右边现在变为

$$\mathrm{KTS}(2\pi)^2 +\frac{K(1 - T_S)(2\pi)^2}{N_D}\sum_{i = 1}^{N_D}J_o^2\left(\frac{2\pi f_cx_i}{cN_D}\right) \quad (10)$$

其中 $N_D$ 是构成移动手部的动态散射体总数。

(2) 的分母变为

$$\left[\sum_{l = 0}^{L - 1}|h_1[l]|^2 \left[\sum_{l = 0}^{L - 1}|h_2[l]|^2 = K\int_{v_l = 0}^{2\pi} \int_{v_l = 0}^{2\pi} 1 d v_l d\psi_l\right]. \quad (11)$$

使用 (10) 和 (11)，$\mathbf{h}_0$ 和 $\mathbf{h}_x$ 之间的 TRRS 可以写为，

$$\eta (\mathbf{h}_0,\mathbf{h}_x) = T_S + \frac{(1 - T_S)}{N_D}\sum_{i = 1}^{N_D}J_o^2\left(\frac{2\pi f_cx_i}{cN_D}\right). \quad (12)$$

方程 (12) 指定了手臂伸展时 TRRS 衰减与手腕移动距离之间的关系。对于 $N_D = 50$，绘制右边曲线如图 5 所示，显示出单调递减。我们利用这种单调递减特性在下一节中开发一种基于似然度的方法用于手势分类。

## IV. GWRITE 算法

本节设计一个流程，为每个手势形状提取独特特征并将其用于手势分类。我们在本文中仅考虑由直线段组成的手势。要描述任何由直线段组成的形状，了解线段的数量、两个相邻线段之间的角度以及线段之间交点（如果有）的位置就足够了。GWrite 模块的概述如图 6 所示。由于我们没有手部运动绝对/相对方向的信息，我们只关注轨迹的相对形状。因此，字母 "N" 和 "Z" 的轨迹特征将是相同的：三个线段、相邻线段之间为锐角、且无交点。同样，"M" 和 "W" 的轨迹特征也将相同。图 8 显示了来自大写英文字母的具有独特特征的示例轨迹。本文提出的 GWrite 原型将在这组 15 种手势形状上演示手势分类。

<center>图 6. GWrite 算法概述。 </center>

### A. 预处理

GWrite 原型构建在配备 $2\times 2$ MIMO 的 WiFi 芯片组上，总共创建了四个不同的空间链路。在预处理步骤中，我们计算所有 CSI 对之间的 $\eta$，并为每个链路 $z$ 生成 TRRS 矩阵 $(\eta_z)$。然后我们使用 MRC 合并来自所有链路的矩阵，得到的矩阵将用作特征提取模块的输入。

1) **TRRS 计算**：对于每个链路 $z$，$\eta_z$ 的第 $(i,j)$ 个条目是时间序列中第 $i$ 个和第 $j$ 个 CSI 之间的 TRRS。对于两个 CSI $H_{i}$ 和 $H_{j}$，$\eta_z(i,j)$ 在频域中定义为 [28]

$$\eta_z(i,j) = \frac{|\sum_{k\in v}H_i[k]H_j[k]^*|}{\sqrt{\sum_k|H_i[k]|^2\sum_k|H_j[k]|^2}} \quad (13)$$

其中 $v$ 是子载波集合，$(.)^*$ 是复共轭运算。当 $z = 1,2,3,4$ 时，$\eta_z$ 的一个实例如图 7(a) 所示。

2) **归一化**：注意 (9) 中的 $T_S$ 表示静态散射体对 TRRS 值的贡献。不同的链路基于其多径传播可能有不同水平的 $T_S$。对于每个链路，$T_S$ 被确定为对应于一个手势的所有 CSI 对中 TRRS 的最低值。随着 $T_S$ 减小，对于相同程度的手部运动，我们观察到 TRRS 有更大的变化。通过减去静态环境的贡献，我们均衡了不同链路上的信号水平。归一化后的矩阵将

===== Page 6 =====

5982

记为 $\hat{\eta}^z$ [图 7(b)]，定义为

$$\hat{\eta}_z = \frac{\eta_z - T_S}{1 - T_S}. \quad (14)$$

3) **最大比合并（MRC）**：可以观察到，由于 MIMO 系统提供的空间分集，不同的链路呈现不同重要性的信息。我们使用 MRC 技术，根据其信号和噪声水平对来自不同链路的信息进行加权合并。当手部在转弯处的速度为零时，CSI 之间的 $\eta$ 应等于 1。由于环境因素（环境运动引起的噪声）和人为因素（其他身体部位/手部的微小运动），实际情况可能并非如此。对于每个链路 $z$，让手部位于转弯位置时 CSI 之间的最大 $\eta$ 值记为 $\eta_z^{\mathrm{max}}$。在这些零速度时刻，平均 $\eta$ 值与 1 的偏移量表示噪声水平。MRC 权重与噪声水平的平方成反比。合并后的 $\eta^{\mathrm{MRC}}$ 由下式给出

$$\eta^{\mathrm{MRC}} = \frac{\sum_{z = 1}^{N_{\mathrm{TX}}\times N_{\mathrm{RX}}}\frac{\hat{\eta}(z)}{(1 - \eta_z^{\mathrm{max}})^2}}{\sum_{z = 1}^{N_{\mathrm{TX}}\times N_{\mathrm{RX}}}\frac{(1 - \eta_z^{\mathrm{max}})^2}{(1 - \eta_z^{\mathrm{max}})^2}} \quad (15)$$

其中 $N_{\mathrm{TX}}$ 和 $N_{\mathrm{RX}}$ 分别表示 Tx 和 Rx 天线的数量。$\eta^{\mathrm{MRC}}$ 在手势过程中显示出更显著的值变化，并降低了噪声水平，如图 7(c) 所示。在接下来的特征提取模块中，将分析 $\eta^{\mathrm{MRC}}$ 以提取手势形状的区分特征。在下面的讨论中，两个空间点之间的 TRRS 应理解为手部位于这两个空间位置时 CSI 之间的 TRRS。

<center>图 7. 四个 TRRS 链路 $z = 1,2,3,4$ 的 MRC 图解。 (a) 原始 TRRS 矩阵。 (b) 归一化静态偏移后的 TRRS 矩阵。 (c) MRC 后的 TRRS 矩阵。 </center>

### B. 特征提取

该模块提取手势轨迹的特征，这些特征将在后续用于手势分类。

1) **手势分割**：手势形状中的每条直线称为一个线段。例如，图 8 显示了不同的手势形状，它们根据线段数量 $(N_{s})$ 进行分组。手势分割旨在确定给定手势形状中的线段数量。

<center>图 8. 具有不同线段数量的大写英文字符手势轨迹。圆点表示每个形状的起始点。 </center>

在手势过程中，当手部改变运动方向时，即转弯时，速度最小。假设用户在转弯时以降低的速度或零速度执行手势，则转弯次数可以从速度非常低或为零的时刻数量推断出来。我们利用 [29] 中提出的运动统计量（MS）来实现这一目标。

MS 表示收发器感知到的无线信道中运动/扰动的程度。因此，靠近收发器的小型动态物体和距离收发器较远的大型动态物体可能引起相似程度的扰动，并产生相同的 MS。类似地，快速运动的小物体和运动较慢的大物体可能具有相同的 MS。MS 是根据 CSI 的相关性计算得出的，详见 [29]。其理论值介于 $-1$ 和 1 之间，其中 1 表示最高的运动/扰动水平。

在手势过程中，人距离收发器有固定的距离。因此，MS 可以捕捉手部运动的相对快慢。考虑图 9(a) 所示的 "D" 形手势以及图 9(b) 中对应的 MS。观察到 MS 在

===== Page 7 =====

转弯位置（如图中用圆圈标记）较低。手势中直线段的数量可以通过 MS 中的局部最小值/谷值数量减 1 来确定。

<center>图 9. 手势分割示意图。 (a) "D" 形手势轨迹。 (b) 对应的 MS 以及不同的手势线段。 </center>

<center>图 10. (a) 手势中转弯的几何形状。 (b) 显示 TRRS 衰减特征的 $\mathbf{S}^{\Delta}$ 图。 </center>

2) **转角分类**：手部运动方向的突然改变称为转弯事件。例如，图 10(a) 显示了一个手势轨迹，从原点 ("O") 开始，沿直线直到在点 "T" 处发生转弯。点 T 之后的轨迹是一条直线，结束于点 "F"。两条直线段之间的角度称为转角 $\theta$。我们将 $\theta$ 分为三类：1) $\theta \approx 0^{\circ}$；2) $\theta$ 为锐角，即 $0^{\circ} < \theta < 90^{\circ}$；以及 3) $\theta \geq 90^{\circ}$。选择这些角度类别的理由有两点。首先，它使用户能够有效地执行转弯，而无需过多关注手部绘制的精确角度。其次，如下文所述，这些角度组具有独特的区分特征，可以基于 TRRS 相似性进行分类。

考虑图 10(a) 中轨迹第二段上的任意点 "S"。点 "Ps" 是第一段上离 S 最近的点。如果我们计算点 S 处的 CSI 与沿轨迹向回走的所有 CSI 之间的 TRRS 相似性，我们会观察到在点 T 处的 CSI 处有一个谷值，在点 Ps 处的 CSI 处有一个峰值，如图 10(b) 所示。我们将此图称为第二段上任意点 S 的 $\mathbf{S}^{\Delta}$。相对距离与 TRRS 之间的直接对应关系是第三节中推导的单调衰减模式的结果。利用 S、Ps 和 T 之间成对 TRRS 的相对关系，我们定义了两个特征 $r$ 和 $f$，并使用它们为不同的 $\theta$ 分配概率分数。

**参数 $f$**：TRRS 相似性的单调递减在超过一定距离后会趋于饱和。当 S 沿第二段向 F 移动时 [图 10(a)]，S 与对应 Ps 之间的距离增加。结果，$\mathbf{S}^{\Delta}$ 图中的峰值显著性持续下降，并在某点之后变得可以忽略不计。图 11(a) 显示了两个线段之间的 TRRS 矩阵，它们之间的夹角为锐角 $\theta$。第二段上两点 $\mathbf{S}_0$ 和 $\mathbf{S}_1$ 的 $\mathbf{S}^{\Delta}$ 图如图 11(b) 所示。与 $\mathbf{S}_0$ 对应的峰值是显著的，而与 $\mathbf{S}_1$ 对应的峰值则不明显。类似地，对于固定的 $\mathbf{S}$，随着 $\theta$ 增加，$\mathbf{S}^{\Delta}$ 图中的峰值变得不那么显著，并且对于较大的 $\theta$ 可能会消失。这一观察促使我们设计用于角度分类的特征 $f$。它被定义为在第二段上能观察到 $\mathbf{S}^{\Delta}$ 图中峰值的部分所占的比例。如果 $S_{f}$ 是第二段上离 T 最远的、在 $\mathbf{S}^{\Delta}$ 图上能观察到显著峰值的点，那么 $f$ 由 (距离 $\mathbf{TS}_f$ )/ (距离 TF) 定义。

<center>图 11. 峰值显著性随距转弯点距离变化的说明。 (a) 两个线段之间的 TRRS，夹角为锐角 $\theta$ 之间。 (b) 第二段上两点 $\mathbf{S}_0$ 和 $\mathbf{S}_1$ 的 $\mathbf{S}^{\Delta}$ 曲线。 </center>

鉴于每个线段都遵循直线路径，线段上任意两个实例/点之间移动的距离可以确定为该线段总长度的一部分。回想一下，在第四节 B1 部分，MS 被用来确定手势在每个时刻的相对速度。假设 CSI 在时间上等间隔，我们可以通过累加任意两个时间实例之间的 MS，将 MS 用作相对移动距离的近似指标。这种近似成立，因为在单次手势执行中，相似的运动程度将导致相似的 MS。线段 AB 上两点 $X$ 和 $Y$ 之间的相对距离，作为 AB 长度的一部分，通过下式获得

$$\frac{\sum_{i\in(X,Y)}\mathrm{MS}(i)}{\sum_{i\in(A,B)}\mathrm{MS}(i)} \quad (16)$$

那么，$f$ 可以定义为

$$f = \frac{\sum_{i\in(\mathbf{T},\mathbf{S}_f)}\mathrm{MS}(i)}{\sum_{i\in(\mathbf{T},\mathbf{F})}\mathrm{MS}(i)}. \quad (17)$$

在这项工作中，$f \leq 0.2$ 被归为 $\theta \geq 90^{\circ}$ 类，而对于 $\theta \approx 0^{\circ}$，$f$ 应等于 1。

**参数 $r$**：对于第二段上的每个点 S，可以从 $\mathbf{S}^{\Delta}$ 曲线获得对应的点 T 和 Ps。对于给定的转角 $\theta$，参数 $r$ 定义为曲线 $T_{\mathrm{max}} - \mathrm{TRRS}(\mathbf{S}, \mathbf{P}_{\mathrm{S}})$ 和 $T_{\mathrm{max}} - \mathrm{TRRS}(\mathbf{S}, \mathbf{T})$ 下面积之比，其中 $T_{\mathrm{max}}$ 是两个线段上点之间的最高 TRRS。理想情况下 $T_{\mathrm{max}} \approx 1$，因为

===== Page 8 =====

位于不同线段上靠近点 $\mathbf{T}$ 的点之间的 TRRS 接近于 1。如果 $\theta = 45^{\circ}$，$\mathrm{SP} = \mathrm{PT}$，并且假设环境均匀，则 $\mathrm{TRRS}(\mathbf{S},\mathbf{P}_{\mathbf{S}})\approx \mathrm{TRRS}(\mathbf{P}_{\mathbf{S}},\mathbf{T})$，导致 $r\approx 1$。对于 $\theta = 0^{\circ}$，$\mathrm{TRRS}(\mathbf{S},\mathbf{P}_{\mathbf{S}})\approx T_{\mathrm{max}}$，导致 $r\approx 0$。

参数 $f$ 和 $r$ 的理想值见表 I。然而，由于以下原因，观测值与其理想值存在差异。

1) TRRS 的单调递减在大距离上不明显。因此，对于较大的锐角，可能无法在整个第二段长度上检测到峰值，并且 $f< 1$。
2) 对于 $\theta \approx 0^{\circ}$，$\mathrm{TRRS}(\mathbf{S},\mathbf{P}_{\mathbf{S}})$ 并不总是等于 1。这是因为人体和手是非刚性物体，即使当人意图描绘相同的路径，CSI 也可能不完全匹配。因此，对于 $\theta \approx 0^{\circ}$，$r > 0$。
3) 由于在空中绘制手势时没有参考，用户可能难以做出精确的 $45^{\circ}$ 转弯。因此，对于锐角转弯，$r$ 的值范围很广。

<center>图 12. $\theta \approx 0^{\circ}$ 和 $\theta \approx 45^{\circ}$ 时参数 $r$ 的分布。 </center>

我们使用数据驱动的方法来获得参数 $r$ 的分布，并为角度分类设计概率分数。为此，我们收集了对应于三个角度类别各 400 次转弯的 CSI 数据，并计算了 $r$。$r$ 的经验分布如图 12 所示。$r$ 对于 $\theta = 0^{\circ}$ 和 $\theta = 45^{\circ}$ 的理论值分别是 0 和 1。然而，我们不限制用户执行严格的 $45^{\circ}$ 转弯，而是允许任何锐角，导致后一类的 $r$ 分布更广。分类阈值确定为达到最高分类准确率的值（0.35）。$\theta \approx 0^{\circ}$ 的概率得分曲线被定义为以 $r = 0$ 为中心的半正态密度。曲线的定义使得它们的和为 1，并在阈值（0.35）处相交，概率得分为 0.5，如图 12 所示。概率得分的相对值将使我们能够为分类计算每个手势形状的概率得分。

<center>图 13. (a) 交点检测示意图。 (b) 线段 1 和线段 3 的 CSI 之间的 TRRS。 </center>

3) **交点检测**：识别和定位两个非相邻线段之间的交点可以是区分手势形状的有用特征。例如，在 "P" 和 "D" 形状中，第三段分别在第一段的中点和起点处与第一段相交。如果手势轨迹中发生交叉，这意味着手臂在那些时刻的空间朝向/位置几乎相同，导致两个 CSI 之间的 TRRS 值相对较高（理想情况下等于 1）。然而，交点的出现和位置并不是手势分类的唯一决定因素。定义一个概率得分可能比二元决策更能为整体分类提供信息。存在两个主要挑战。

1) 很明显，如果发生交点，它将是两个线段的所有 CSI 之间成对 TRRS 相似性中的最大值所对应的位置/时刻。然而，我们不知道任意两个时刻之间移动的绝对距离，这使得估计交点位置变得不简单。
2) 尽管交点处的 TRRS 相似性理想情况下等于 1，但环境和人为因素会使其值降至 1 以下。这些因素的影响不可预测且难以建模，使得概率得分的分配更加复杂。

请注意，我们只需要交点在所考虑的两个线段上的相对位置。和之前一样，我们利用累加的 MS 作为两点之间距离的度量。考虑图 13 所示的 P 形手势。我们用 $\phi$ 表示线段 $i$ 上的交点距离该线段起点占其总长度的比例。线段 1 和线段 3 之间的交点为 $(\phi_1, \phi_3) = (0.5, 1)$，并用较暗的圆圈标记。由于用户在绘制轨迹时没有空间参考，因此很难在期望的位置交叉线段。因此，我们允许 0.25 的余量，如图 13 中扩展的圆形区域所示。如果交点落在两个线段上期望位置的 0.25 余量内，我们为交点估计一个概率得分。

我们采用类似于第四节 B2 部分的方法进行概率得分分配。我们设计了一个启发式

===== Page 9 =====

指标 $T_{N} = (T_{\mathrm{max}} - T_{\mathrm{min}} / 1 - T_{\mathrm{min}})$，其中 $T_{\mathrm{max}}$ 和 $T_{\mathrm{min}}$ 是分别位于可能相交的两个线段上的 $i$ 和 $j$ 的 $\mathrm{TRRS}(i,j)$ 的最大值和最小值。图 14 显示了对于有交点和无交点的各 600 对线段，$T_{N}$ 值的直方图。交点的 $T_{N}$ 值理想情况下等于 1。从直方图来看，分类阈值 0.71 对于区分两段之间是否存在交点达到了最高的分类准确率。如前所述，保留信息而非二元分类将更有用，并将用于手势分类的最后一步。"交点"类的概率曲线被定义为以 1 为中心的半正态密度，使得概率得分值在阈值（0.71）处为 0.5，如图 14 所示。"无交点"类的概率曲线取为 1 减去交点的概率。在极端情况下，如果 $T_{N}$ 非常低，即如果两段相距很远，那么存在交点的概率等于 0。

<center>图 14. 有交点和无交点情况下 $T_{N}$ 值的直方图及相应的概率得分分配。 </center>

### C. 手势分类

可以使用在特征提取模块中计算的特征概率得分来对手势进行分类。首先，根据线段数量决定手势组。然后，组内每个字符 $(\Omega)$ 的概率得分计算如下：

$$\begin{array}{l}{P(\Omega) = \frac{\alpha}{N_s - 1}\sum_{i = 1}^{N_s - 1}P(\theta_i^{\Omega})}\\ {+\frac{(1 - \alpha)}{N_s - 1C_2}\sum_{i = 1}^{N_s - 1}\sum_{j = i + 2}^{N_s}P(\{\phi_i,\phi_j\}^{\Omega})} \end{array} \quad (18)$$

其中 $N_{s}$ 是手势轨迹中的总段数，$N_{s} - 1C_{2}$ 是组合符号，表示可能相交的非连续线段对的总数。注意 $j$ 是从 $i + 2$ 开始索引的，因为相邻线段除了在转弯点外不可能相交。轨迹形状 $\Omega$ 的特征实际值由 $\theta_{i}^{\Omega}$ 和 $\{\phi_{i},\phi_{j}\}^{\Omega}$ 给出。例如，$\{\theta_{i}^{P},\theta_{i}^{P},\{\phi_{1},\phi_{3}\}^{P}\} = \{45^{\circ},45^{\circ},\{0.5,1\} \}$。参数 $\alpha$ 是一个超参数，可以调整以权衡角度分类和交点检测模块的贡献。表 II 显示了一个 P 形手势计算的概率得分示例，其中 $\alpha = 0.5$ 且 $P(\{\phi_{1},\phi_{3}\}) = 0.8$。$\Omega = \mathbf{P}$ 的概率得分（0.82）高于其他字符，因此手势形状可以被分类为 P 形。

**表 II 三个线段字符的概率得分**

<table>
<tr><th>Ω</th><th>概率得分</th></tr>
<tr><td>D</td><td>0.42</td></tr>
<tr><td>P</td><td>0.82</td></tr>
<tr><td>T</td><td>0.17</td></tr>
<tr><td>Y</td><td>0.32</td></tr>
<tr><td>Z</td><td>0.42</td></tr>
</table>

<center>图 15. (a) 角度分类 和 (b) 交点检测的混淆矩阵。 </center>

## V. 性能评估

为了评估性能，我们在商用现货（COTS）WiFi 芯片组上构建了 GWrite 原型，带宽为 $80\mathrm{MHz}$，采用 $2\times 2$ MIMO，采样率为 $350\mathrm{Hz}$。典型的穿墙设置如图 1 所示。发射器（Tx）和接收器（Rx）放置在不同的房间。手势在另一个房间执行，每个收发器与手势位置之间至少隔着一堵墙。

### A. 特征提取性能

1) **手势分割准确率**：我们收集了具有 3、4 和 5 个线段的每种手势各 100 个样本进行评估。线段数量被正确识别的概率为 $100\%$。由于手势分割模块可以准确估计手势轨迹中的线段数量，在手势分类准确率评估中，我们评估的是具有相同线段数量的手势的分类效果。

2) **转角分类准确率**：第四节 B2 部分讨论的角度分类模块使用大约 400 次 $\theta \approx 0^{\circ}$ 和 $\theta \approx 45^{\circ}$ 的手势实现进行评估。分类的混淆矩阵如图 15(a) 所示。平均分类准确率为 $90.4\%$。

3) **交点检测准确率**：第四节 B3 部分讨论的交点检测模块使用大约 600 对存在交点和不存在交点的线段进行评估。分类的混淆矩阵

===== Page 10 =====

如图 15(b) 所示，总体分类准确率为 $81.1\%$。

### B. 手势分类准确率

为了评估手势分类算法的整体性能，我们将具有相同线段数量的大写英文字符分组，并在每个组内进行手势分类。这是因为手势分割非常准确，我们发现基于线段数量的分类错误为零。我们将具有独特手势形状的字符按图 8 所示分为三组：

1) 3 个线段：D, P, T, Y, Z 形状；
2) 4 个线段：A, F, J, M, O 形状；
3) 5 个线段：B, E, H, K, Q 形状。

每个组的混淆矩阵如图 16 所示。3、4 和 5 线段组的平均分类准确率分别为 $90.8\%$、$88.4\%$ 和 $96.8\%$。5 线段手势的分类准确率更高是因为具有更多区分特征，这降低了错误分类的概率。所有 15 个手势的平均分类准确率为 $92\%$。对于需要较少手势数量的应用，可以使用 5 线段手势，其分类准确率约为 $97\%$。

<center>图 16. 具有不同线段数量的手势分类混淆矩阵。 (a) 三个线段。 (b) 四个线段。 (c) 五个线段。 </center>

数据是在五个月的非工作时间收集的。大约有 25 人在办公空间工作，在很长一段时间内导致室内环境发生许多变化。此外，Tx、Rx 和手势位置既不是固定的，也没有标记。相反，设备被随意放置在桌子上，大致位于地板图上标记的位置附近，这与大多数现有工作即使在 Tx/Rx 位置稍有变化时也无法工作的情况不同。因此，我们证明了 GWrite 对室内环境变化具有鲁棒性。在 VI-A 节中，我们还讨论了不同类别的特征分布在完全不同的实验设置中也遵循相同的趋势。

## VI. 讨论

### A. 放置与覆盖范围

GWrite 的收发器放置非常灵活，因为分类特征是轨迹形状的特征，而不是放置几何形状的特征。虽然确切位置无关紧要，但存在一个特定的操作区域，GWrite 可以在该区域内发挥最佳性能。如果收发器彼此放置得太远，则可能无法感知到手部运动引起的信道扰动。另一方面，如果设备放置得非常近，则均匀散射假设可能不成立，性能可能会下降。我们在图 17 中展示了一些适合 GWrite 的实验设置。

<center>图 17. 设备放置示例。 (a) 位置 1。 (b) 位置 2。 </center>

<center>图 18. 两种不同设备放置下 $T_N$ 的直方图。 (a) 位置 1。 (b) 位置 2。 </center>

(12) 中的项 $T_S$ 表示静态环境对 TRRS 的贡献程度，并且依赖于收发器、手势和环境的相对位置。在预处理阶段合并来自不同链路的 TRRS 时，我们对 $T_S$ 进行了补偿。因此，用于角度分类和交点检测的特征在操作区域内与收发器之间的位置/距离无关。图 18 显示了两个位置下 $T_N$ 的直方图。

===== Page 11 =====

可以观察到，在这两种情况下，分类阈值是相似的，我们据此设计了概率得分曲线。位置无关性是 GWrite 提供的关键优势之一。

<center>图 19. 分类准确率随 $\alpha$ 的变化。</center>

<center>图 20. GWrite 可以分类的更多形状。 </center>

### B. 特征权重 $(\alpha)$

(18) 中的参数 $\alpha$ 可以调整，以赋予角度分类和交点检测模块不同的权重。最优值取决于要分类的手势集。例如，在仅使用由锐角形状组成的手势集的极端情况下，$\alpha$ 可以设置为 0，此时分类完全基于交点检测模块。3 线段手势在不同 $\alpha$ 下的平均分类准确率如图 19 所示。

### C. 广义手势

我们评估了 GWrite 在一组 15 个大写英文字符上的性能。然而，这个集合可以扩展到包括更一般的直线形状，例如图 20 中的形状。GWrite 可以分类由直线段组成、且在转角和交点方面有所不同的手势。这类区分特征越多，分类准确率就越高。

### D. 用户多样性

依赖于手势的相对形状，我们预计 GWrite 不会有明显的用户多样性。为了证实这一断言，我们与六位不同的用户进行了实验，他们的信息见表 III。要求每位用户重复 15 种手势各 30 次，并使用收集的 CSI 时间序列进行手势分类。图 21 显示了用户 2 的参数 $r$ 和 $T_{N}$ 的相似分布。应用由第一位用户确定的阈值，我们在 15 个手势集上达到了 $89.3\%$ 的分类准确率。角度和交点分类准确率分别为 $85.6\%$ 和 $80.4\%$。我们相信，除了位置无关性之外，TRRS 计算中的归一化步骤也有助于实现用户无关性。如图 22 所示，所有用户的平均分类准确率介于 $83\%$ 和 $92\%$ 之间。分类准确率差异的主要部分可归因于不同用户绘制手势的方式，特别是交点和转角。根据这些数据，我们没有观察到明显的用户依赖性。然而，需要进行更广泛的大规模研究，以充分了解在更多用户以及同一用户随着时间推移（当他/她熟悉/训练绘制手势形状后）的表现。

**表 III 用户信息**

<table>
<tr><th>用户</th><th>身高(cm)</th><th>体重(kg)</th><th>年龄</th><th>性别</th><th>臂长(cm)</th></tr>
<tr><td>1</td><td>164</td><td>52</td><td>29</td><td>F</td><td>63</td></tr>
<tr><td>2</td><td>165</td><td>50</td><td>26</td><td>F</td><td>53</td></tr>
<tr><td>3</td><td>172</td><td>72</td><td>27</td><td>M</td><td>56</td></tr>
<tr><td>4</td><td>169</td><td>56.7</td><td>25</td><td>F</td><td>55</td></tr>
<tr><td>5</td><td>158</td><td>50</td><td>28</td><td>F</td><td>52</td></tr>
<tr><td>6</td><td>173</td><td>90</td><td>29</td><td>M</td><td>60</td></tr>
</table>

<center>图 21. (a) $r$ 和 (b) $T_{N}$ 对于新用户的分布。 </center>

<center>图 22. 不同用户的分类准确率。 </center>

### E. 放松手势

在第三节中，(12) 中 $\mathbf{h}_0$ 和 $\mathbf{h}_x$ 之间的 TRRS 关系是在假设执行手势时手部完全伸展的条
件下推导出来的，如图 23(a) 所示。然而，这可能与人们在空中书写的自然方式（即放松手臂）有些不同，如
图 23(b) 所示。为了评估在这种放松场景下的性能，我们评估了 GWrite 在相同 15 个手势集上且不对手臂施加任何约束的表现。参数 $r$ 和 $T_{N}$ 的分布如图 24 所示。角度和交点分类准确率分别为

===== Page 12 =====

$86.52\%$ 和 $79.09\%$，而整体手势分类准确率为 $74.16\%$。尽管准确率低于伸展手部方法，但对于包含 15 个手势的可观集合来说，这一准确率已经很高且颇具前景。

<center>图 23. (a) 伸展手臂 和 (b) 放松手臂的手势书写。</center>
<center>图 24. 放松手臂手势的 r 和 TN 分布。</center>

### F. 计算复杂度

手势识别系统在实时实现时具有实用性，能够提供流畅的用户体验。GWrite 中最复杂的步骤是 TRRS 矩阵计算、对 $\mathbf{S}^{\Delta}$ 图的平滑操作以及峰值检测。这些操作的复杂度为 $O(n^2)$，其中 $n$ 是 CSI 时间序列的长度。例如，如果手势持续时间为 3 秒，CSI 采样率为 350 Hz，那么 TRRS 矩阵需要计算 $1050 \times 1050$ 次 TRRS 指标。在 Windows 计算机上从 CSI 时间序列确定手势形状所需时间少于 5 秒（Intel Core i7-7700K @4.2 GHz，RAM 32 GB，64 位）。通过改进算法优化和利用 GPU，我们相信 GWrite 可以实时实现。

### G. 背景运动干扰

只要干扰用户的运动相对远离执行手势的用户，所提出的 GWrite 系统对干扰用户运动的存在具有鲁棒性。我们进行了实验，让一位用户执行手势，另一位用户在背景中行走，以了解干扰运动随与执行手势用户距离变化的程度。图 25 显示了实验设置，其中 $d$ 是手势位置与行走者之间的距离。行走的人造成的干扰程度可以通过运动统计水平来可视化，如图 26 所示，其中显示了与用户的不同距离。距离小于 6 m 的运动足以干扰

<center>图 25. 背景运动干扰测试的实验设置。</center>
<center>图 26. 干扰人员远离用户时引起的运动统计水平。</center>

手势分割，因为在运动统计曲线中找不到清晰的谷值（第四节 B1 部分）。此外，对于正确分割出线段数量的样本，由于背景运动导致匹配点和转角特征失真，可以观察到性能下降。例如，在一个涉及第二位用户在背景中行走的实验中，当 $d$ 分别为 6 m 和 5 m 时，我们获得的准确率分别为 86% 和 67%。总的来说，即使干扰人员坐在/工作在办公桌前，或者在远离 Tx-Rx 链路和手势位置的地方有剧烈运动，GWrite 也能在没有太大性能下降的情况下工作。学习技术可能有助于通过根据背景运动水平选择自适应参数来改进手势分割。我们将这项研究留待未来工作。

### H. 与最新技术的比较

CSI 已在先前工作中用于实现手势识别。在表 IV 中，我们比较了不同的基于 CSI 的手势识别工作在所用特征/方法、

===== Page 13 =====

**表 IV 最先进的基于 WiFi 的免设备 CSI 手势/活动识别系统比较**

<table>
<tr><th>参考文献</th><th>方法</th><th>设备放置</th><th>手势数量和类型</th><th>声称准确率</th><th>无需训练</th></tr>
<tr><td>WiG [34]</td><td>SVM</td><td>Tx-1Rx: 1-3m, 设备-用户: &lt; 1m</td><td>4种: 右, 左, 推, 拉</td><td>92% LOS, 88% NLOS</td><td>×</td></tr>
<tr><td>WiGeR [20]</td><td>小波变换, DTW</td><td>Tx-1Rx: 3.5m, 设备-用户: 2m</td><td>7种: 左, 右, 轻弹, 抓, 上, 下, 点</td><td>97% LOS, 84% NLOS</td><td>×</td></tr>
<tr><td>CARM [35]</td><td>DFS 剖面, HMM 模型</td><td>Tx-1Rx: 3.5 m, 设备-用户: 1.7m</td><td>8种: 跑, 走, 拳击, 刷牙, 推, 跌倒等</td><td>96% 域内, 85% 跨域</td><td>×</td></tr>
<tr><td>WiMU [32]</td><td>虚拟样本生成, STFT</td><td>Tx-1Rx: 1.5m, 设备-用户: 3m</td><td>6种: 开门/关门, 坐/站, 手臂画圆, 踢等</td><td>95% LOS, 89.5% NLOS</td><td>×</td></tr>
<tr><td>WiHF [36]</td><td>DFS 谱图, 用于手势和用户ID的 DNN </td><td>Tx-3Rx: 2m, 设备-用户: 1m</td><td>10种: 数字 0-9</td><td>94% 域内, 90% 跨域</td><td>×</td></tr>
<tr><td>WiDar 3.0 [37]</td><td>BVP 剖面, DFS 谱图, CNN, GRU</td><td>Tx-3Rx: 2m, 设备-用户: 1m</td><td>10种: 数字 0-9, 推, 扫, 拍手, 滑, 圆, 锯齿形</td><td>92% 域内, 83% 跨域</td><td>×</td></tr>
<tr><td>Cross Sense [38]</td><td>ANN, 混合专家模型, ACF, 谱图</td><td>Tx-1Rx: 1m, 设备-用户: 0.5 m</td><td>40种: 轻弹, 冲拳, 保龄球, 滑动, 坐, 刷牙, 烹饪等</td><td>95% 针对15种手势</td><td>×</td></tr>
<tr><td>WiGest [30]</td><td>小波变换, 手势家族</td><td>最多 Tx-3Rx: 8m, 设备-用户: 1.2m</td><td>7种: 远/近, 慢/快, 暂停的组合</td><td>LOS 和 NLOS; 88% - 单 AP, 96% - 3 APs</td><td>✓</td></tr>
<tr><td>EI [31]</td><td>线段间的滞后相关, 对抗网络</td><td>Tx-2Rx: 2m, 用户-设备: 0.5m</td><td>6种: 擦黑板, 走, 移动, 旋转椅子等</td><td>75% 跨域</td><td>✓</td></tr>
<tr><td>GWrite</td><td>TRRS, 线段间的角度和交点</td><td>Tx-1Rx: 13m, 设备-用户: 6m</td><td>15种: 由直线组成的大写英文字母</td><td>穿墙, 域内和跨域: 92%</td><td>✓</td></tr>
</table>

注：数值四舍五入为最接近的整数。

实验设置、手势集和性能。例如，第三列中的条目 iTx- jRx 表示该工作需要 i 个 Tx 和 j 个 Rx。此外，“设备-用户”距离表示系统工作时用户与其中一个收发器之间的距离。第五列比较了域内和跨域准确率，这衡量了相应特征所实现的位置无关性程度。最后一列表示系统是否需要为新环境/设置/手势或用户方向进行重新训练。从表中可以看出，只有 WiGest [30]、EI [31] 和 GWrite 在不同程度上实现了位置无关性，而其他工作至少需要为新位置提供一些训练样本。以下是从表中得出的另外几点观察结果。

1) 实现位置无关的手势识别具有挑战性，之前的尝试报告称跨域准确率相比域内准确率大幅下降。例如，WiMU [32] 报告称，即使 Tx 和 Rx 位置相同，仅因用户方向变化，准确率就从 $96\%$ 下降到 $85\%$。GWrite 是唯一即使在跨域情况下也报告准确率高于 $90\%$ 的工作，这要归功于其与位置无关的特征。

2) 在表中报告的三项实现位置无关的工作中，GWrite 具有最广的覆盖范围、最高的设备-用户距离、最大且可进一步扩展的手势集，以及良好的分类准确率。

3) 基于深度学习的去除环境依赖性方法只能容忍一定程度的环境变化，例如车内驾驶员认证 [33] 和其他声称环境无关性的工作 [31]。执行活动/手势的目标的相对设备位置相似，且设备到用户的距离短，从而产生强烈的手势信号。相比之下，GWrite 展示了更高级别的位置无关性，即使设备到用户的距离更大，并且能够容忍如第五节 B 部分所述的长期室内环境变化。

## VII. 未来工作

正如前一节所讨论的，实现鲁棒的穿墙位置无关手势识别具有挑战性，我们已经证明 GWrite 在实现这一目标方面更进一步，同时放宽了设备放置要求。随着时间的推移积累更多数据时，我们希望在未来的工作中探索以下几点。

1) 在这项工作中，我们保持特征提取和手势分类简单直观，以使系统更加鲁棒。下一步是尝试使用机器学习分类器或 CNN 进行模式识别，以当前性能为基准。
2) 需要进行更多实验来充分了解 GWrite 在不同设置、不同用户以及随时间推移的性能/局限性。

## VIII. 结论

我们提出了 GWrite，一个使用商用 WiFi 设备的免设备手势书写识别系统。所提出的系统可以在穿墙场景下工作，并适用于广泛的手势集。通过使用手势模型并利用典型室内环境中的密集多径，我们证明了 CSI 之间的 TRRS 随手部移动距离单调递减。这一观察结果使我们能够提取描述由直线段组成的手势形状中的几何关系的特征。我们在一组 15 个大写英文字符上评估了 GWrite，达到了 $92\%$ 的识别准确率。随着 WiFi 6/7 带来更多可用带宽，真实世界的手势书写将更紧密地符合 GWrite 的理论模型，GWrite 可以实现更高的识别准确率，在未来智能室内环境和 HCI 应用中发挥不可或缺的作用。

===== Page 14 =====

**参考文献**

[1] J. Wang, D. Vasisht, and D. Katabi, "RF-IDraw: Virtual touch screen in the air using RF signals," ACM SIGCOMM Comput. Commun. Rev., vol. 44, no. 4, pp. 235-246, 2014.
[2] S. Agrawal, I. Constandache, S. Gaonkar, R. R. Choudhury, K. Caves, and F. DeRuyter, "Using mobile phones to write in air," in Proc. 9th Int. Conf. Mobile Syst., Appl. Services, 2011, pp. 15-28.
[3] B. Wang, Q. Xu, C. Chen, F. Zhang, and K. J. R. Liu, "The promise of radio analytics: A future paradigm of wireless positioning, tracking, and sensing," IEEE Signal Process. Mag., vol. 35, no. 3, pp. 59-80, May 2018.
[4] H. Liu, H. Darabi, P. Banerjee, and J. Liu, "Survey of wireless indoor positioning techniques and systems," IEEE Trans. Syst., Man, Cybern. C, Appl. Rev., vol. 37, no. 6, pp. 1067-1080, Nov. 2007.
[5] S. D. Regani, Y. Hu, B. Wang, and K. R. Liu, "WiFi-based robust indoor localization for daily activity monitoring," in Proc. 1st ACM Workshop Mobile Wireless Sens. Smart Healthcare, 2022, pp. 1-6.
[6] X. Zeng, B. Wang, C. Wu, S. D. Regani, and K. R. Liu, "WiCPD: Wireless child presence detection system for smart cars," IEEE Internet Things J., early access, Jul. 29, 2022, doi: 10.1109/JIOT.2022.3194873.
[7] Y. Hu, M. Z. Ozturk, F. Zhang, B. Wang, and K. R. Liu, "Robust device-free proximity detection using WiFi," in Proc. IEEE Int. Conf. Acoust., Speech Signal Process. (ICASSP), 2021, pp. 7918-7922.
[8] F. Zhang, C. Chen, B. Wang, and K. J. R. Liu, "WiSpeed: A statistical electromagnetic approach for device-free indoor speed estimation," IEEE Internet Things J., vol. 5, no. 3, pp. 2163-2177, Jun. 2018.
[9] D. Wu et al., "FingerDraw: Sub-wavelength level finger motion tracking with WiFi signals," Proc. ACM Interactive, Mobile, Wearable Ubiquitous Technol., vol. 4, no. 1, pp. 1-27, 2020.
[10] M. Youssef, M. Mah, and A. Agrawala, "Challenges: Device-free passive localization for wireless environments," in Proc. 13th Annu. ACM Int. Conf. Mobile Comput. Netw., 2007, pp. 222-229.
[11] H. Abdelnasser, M. Youssef, and K. A. Harras, "WiGest: A ubiquitous WiFi-based gesture recognition system," in Proc. IEEE Conf. Comput. Commun. (INFOCOM), 2015, pp. 1472-1480.
[12] S. Nannuru, Y. Li, Y. Zeng, M. Coates, and B. Yang, "Radio-frequency tomography for passive indoor multitarget tracking," IEEE Trans. Mobile Comput., vol. 12, no. 12, pp. 2322-2333, Dec. 2013.
[13] H. Abdelnasser, K. A. Harras, and M. Youssef, "Ubibreathe: A ubiquitous non-invasive WiFi-based breathing estimator," in Proc. 16th ACM Int. Symp. Mobile Ad Hoc Netw. Comput., 2015, pp. 277-286.
[14] D. Halperin, W. Hu, A. Sheth, and D. Wetherall, "Tool release: Gathering 802.11 n traces with channel state information," ACM SIGCOMM Comput. Commun. Rev., vol. 41, no. 1, p. 53, 2011.
[15] Y. Ma, G. Zhou, S. Wang, H. Zhao, and W. Jung, "SignFi: Sign language recognition using WiFi," Proc. ACM Interactive, Mobile, Wearable Ubiquitous Technol., vol. 2, no. 1, pp. 1-21, 2018.
[16] S. Tan and J. Yang, "WiFinger: Leveraging commodity WiFi for fine-grained finger gesture recognition," in Proc. 17th ACM Int. Symp. Mobile Ad Hoc Netw. Comput., 2016, pp. 201-210.
[17] H. Li, W. Yang, J. Wang, Y. Xu, and L. Huang, "WiFinger: Talk to your smart devices with finger-grained gesture," in Proc. ACM Int. Joint Conf. Pervasive Ubiquitous Comput., 2016, pp. 250-261.
[18] W. He, K. Wu, Y. Zou, and Z. Ming, "WiG: WiFi-based gesture recognition system," in Proc. 24th Int. Conf. Comput. Commun. Netw. (ICCCN), 2015, pp. 1-7.
[19] K. Ali, A. X. Liu, W. Wang, and M. Shahzad, "Keystroke recognition using WiFi signals," in Proc. 21st Annu. Int. Conf. Mobile Comput. Netw., 2015, pp. 90-102.
[20] M. A. A. Al-qaness and F. Li, "Wiger: WiFi-based gesture recognition system," ISPRS Int. J. Geo-Inf., vol. 5, no. 6, p. 92, 2016.
[21] H. Abdelnasser, K. Harras, and M. Youssef, "A ubiquitous WiFi-based fine-grained gesture recognition system," IEEE Trans. Mobile Comput., vol. 18, no. 11, pp. 2474-2487, Nov. 2019.
[22] S. D. Regani, B. Wang, M. Wu, and K. J. R. Liu, "Time reversal based robust gesture recognition using WiFi," in Proc. IEEE Int. Conf. Acoust., Speech Signal Process. (ICASSP), 2020, pp. 8309-8313.
[23] J. Xiong and K. Jamieson, "ArrayTrack: A fine-grained indoor location system," in Proc. 10th USENIX Symp. Netw. Syst. Des. Implement. (NSDI), 2013, pp. 71-84.
[24] L. Sun, S. Sen, D. Koutsonikolas, and K.-H. Kim, "WiDraw: Enabling hands-free drawing in the air on commodity WiFi devices," in Proc. 21st Annu. Int. Conf. Mobile Comput. Netw., 2015, pp. 77-89.
[25] Q. Pu, S. Gupta, S. Gollakota, and S. Patel, "Whole-home gesture recognition using wireless signals," in Proc. 19th Annu. Int. Conf. Mobile Comput. Netw., 2013, pp. 27-38.
[26] B. Wang, Y. Wu, F. Han, Y.-H. Yang, and K. J. R. Liu, "Green wireless communications: A time-reversal paradigm," IEEE J. Sel. Areas Commun., vol. 29, no. 8, pp. 1698-1710, Sep. 2011.
[27] M. Abramowitz and I. A. Stegun, Handbook of Mathematical Functions with Formulas, Graphs, and Mathematical Tables. Washington, DC, USA: National Bureau of Standards, 1972.
[28] C. Chen, Y. Chen, Y. Han, H.-Q. Lai, and K. J. R. Liu, "Achieving centimeter-accuracy indoor localization on WiFi platforms: A frequency hopping approach," IEEE Internet Things J., vol. 4, no. 1, pp. 111-121, Feb. 2017.
[29] F. Zhang, C. Chen, B. Wang, and K. J. R. Liu, "WiSpeed: A statistical electromagnetic approach for device-free indoor speed estimation," IEEE Internet Things J., vol. 5, no. 3, pp. 2163-2177, Jun. 2018.
[30] H. Abdelnasser, M. Youssef, and K. A. Harras, "WiGest: A ubiquitous WiFi-based gesture recognition system," in Proc. IEEE Conf. Comput. Commun. (INFOCOM), 2015, pp. 1472-1480.
[31] W. Jiang et al., "Towards environment independent device free human activity recognition via acoustic communication," Proc. ACM Interactive, Mobile, Wearable Ubiquitous Technol., vol. 5, no. 1, pp. 1-21, 2021.
[32] R. H. Venkatnarayan, G. Page, and M. Shahzad, "Multi-user gesture recognition using WiFi," in Proc. 16th Annu. Int. Conf. Mobile Syst., Appl., Services, 2018, pp. 401-413.
[33] S. D. Regani, Q. Xu, B. Wang, M. Wu, and K. J. R. Liu, "Driver authentication for smart car using WiFi: A statistical approach," in Proc. IEEE 21st Int. Workshop Signal Process. Adv. Wireless Commun. (SPAWC), 2020, pp. 1-5.
[34] W. He, K. Wu, Y. Zou, and Z. Ming, "WiG: WiFi-based gesture recognition system," in Proc. 24th Int. Conf. Comput. Commun. Netw. (ICCCN), 2015, pp. 1-7.
[35] W. Wang, A. X. Liu, M. Shahzad, K. Ling, and S. Lu, "Understanding and modeling of WiFi signal based human activity recognition," in Proc. 21st Annu. Int. Conf. Mobile Comput. Netw., 2015, pp. 65-76.
[36] F. Wang, J. Feng, Y. Zhao, X. Zhang, S. Zhang, and J. Han, "Joint activity recognition and indoor localization with WiFi fingerprints," IEEE Access, vol. 7, pp. 80058-80068, 2019.
[37] Y. Zheng et al., "Zero-effort cross-domain gesture recognition with Wi-Fi," in Proc. 17th Annu. Int. Conf. Mobile Syst., Appl., Services, 2019, pp. 313-325.
[38] J. Zhang, B. Wei, W. Hu, and S. S. Kanhere, "WiFi-ID: Human identification using WiFi signal," in Proc. Int. Conf. Distrib. Comput. Sens. Syst. (DCOSS), 2016, pp. 75-82.

===== Page 15 =====

**Beibei Wang** (Senior Member, IEEE) received the B.S. degree (Highest Hons.) in electrical engineering from the University of Science and Technology of China, Hefei, China, in July 2004, and the M.S. and Ph.D. degrees in electrical engineering from the University of Maryland at College Park, College Park, MD, USA, in 2008 and 2009, respectively.
She was a Postdoctoral Research Associate with the University of Maryland at College Park from 2009 to 2010. She was with Qualcomm Research and Development, San Diego, CA, USA, from 2010 to 2012, working on system design and 3GPP RAN2 aspects of HSPA heterogeneous networks. From 2012 to 2014, she was with Qualcomm Research Center, Bridgewater, NJ, USA, working on system design and 3GPP RAN1 aspects of LTE-Direct. She has been with Origin Wireless Inc., Greenbelt, MD, USA, since 2015, where she is the Vice President of Research and Director of Intellectual Properties. She has over 100 technical papers and over 60 patent applications with 34 granted. She has coauthored the _Wireless AI: Wireless Sensing, Positioning, IoT, and Communications_ (Cambridge University Press, 2019) and _Cognitive Radio Networking and Security: A Game Theoretic View_ (Cambridge University Press, 2011). Her research interests include Internet of Things, mobile computing, wireless sensing and positioning, and communications and networking.

**Yuqian Hu** received the B.S. degree from the Department of Electronic Engineering and Information Science, University of Science and Technology of China, Hefei, China, in 2017, and the Ph.D. degree from the Department of Electrical and Computer Engineering, University of Maryland at College Park, College Park, MD, USA, in December 2021.
She is currently the Principal Scientist of Origin Wireless Inc., Greenbelt, MD, USA. Her research interests include Internet of Things and wireless sensing and signal processing.

**K. J. Ray Liu** is the Founder and the President of Origin AI that pioneers AI for wireless sensing and indoor tracking. He was a Distinguished University Professor, a Distinguished Scholar-Teacher, and a Christine Kim Eminent Professor of Information Technology with the University of Maryland at College Park, College Park, MD, USA, from where he retired after over three decades of career in education. He has trained over 70 doctoral/postdoctoral students, with the most recent research contributions encompassing broad aspects of signal processing and communications.
Dr. Liu is the recipient of two IEEE Technical Field Awards: the 2021 IEEE Fourier for Signal Processing and the 2016 IEEE Leon K. Kirchmayer Graduate Teaching Award. He also received the IEEE Signal Processing Society 2014 Norbert Wiener Society Award and the 2009 Claude Shannon-Harry Nyquist Technical Achievement Award. Recognized as a Web of Science Highly Cited Researcher, he is the American Association for the Advancement of Science, and the U.S. National Academy of Inventors. The invention of wireless AI won three prestigious CES Innovation Awards, including the CES Best of Innovation in 2021. He is the 2022 IEEE President and CEO. He was the 2019 IEEE Vice President for Technical Activities. He has also served as the 2012-2013 President of the IEEE Signal Processing Society, where he once served as the Editor-in-Chief for the _IEEE Signal Processing Magazine_.
