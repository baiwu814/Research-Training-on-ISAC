# WiCPD: 智能汽车的无线儿童存在检测系统 (WiCPD: Wireless Child Presence Detection System for Smart Cars)

Xiaolu Zeng, Beibei Wang, Senior Member, IEEE, Chenshu Wu, Senior Member, IEEE, Sai Deepika Regani, and K. J. Ray Liu, Fellow, IEEE

**摘要** — 儿童存在检测 (child presence detection, CPD) 正逐渐成为汽车制造商的一项法规要求，以在儿童被单独留在无人看管的车辆中时挽救他们的生命。然而，大多数现有解决方案需要专用设备，并且准确性和覆盖范围有限。在本文中，我们构建了 WiCPD，这是首个使用商用 Wi-Fi 的车内 CPD 系统，能够覆盖整个汽车内部，无任何盲点。首先，我们引入了一个统计电磁模型，该模型解释了运动对车内所有多径的影响，随后提出了一个指示环境运动强度的运动统计量指标和一个提升信噪比 (signal-to-noise-ratio, SNR) 的方案，以提取微小的胸部运动。然后，我们设计了一个统一的 CPD 框架，包含三个目标检测器模块：用于检测运动/清醒状态儿童的运动目标检测器、用于检测静止/睡眠儿童的静止目标检测器，以及用于检测处于过渡状态（即睡眠中伴有零星运动，但被运动和静止目标检测器遗漏）的儿童的过渡目标检测器。我们使用商用 Wi-Fi 芯片组实现了一个实时的 WiCPD 系统，将其部署在 20 多辆不同的汽车上，并收集了多名年龄从 4 个月到 50 个月不等的儿童的数据。结果表明，当儿童清醒/运动时，WiCPD 能在 8 秒内达到 100% 的检测率；对于静态/睡眠儿童，能在 20 秒内达到 96.56% 的检测率。大量实验还表明，WiCPD 可在几分钟内轻松部署，无需校准，且 CPU 和内存消耗非常低，因此有望成为 CPD 应用的实用候选方案。

**关键词** — 儿童存在检测 (child presence detection, CPD)，车内感知，实时系统，智能汽车，无线感知。

## I. 引言

随着汽车的普及，儿童被单独留在车内而导致中暑和死亡的事件在全球范围内引起了越来越多的关注。根据《封闭车厢热应激》(Heat Stress From Enclosed Vehicles) [1], [2] 的数据，自 1998 年以来，美国已有 882 名儿童因车辆中暑 (pediatric vehicular heatstroke, PVH) 死亡（平均每年 38 人），还有更多儿童因器官或脑损伤而致残。由于报告的儿童中超过 97% 是 6 岁以下、几乎没有能力自行离开车辆的幼儿，儿童存在检测 (child presence detection, CPD) 正成为提高汽车安全性的必要功能，尤其是在炎热的天气里，封闭车内的温度可能在几分钟内就变得致命。为此，从 2022 年起，欧洲新车评估计划 (European new car assessment programme, NCAP) 将对 CPD 系统解决方案进行奖励，并且从 2023 年起，CPD 也被视为所有新制造乘用车的法规要求。配备 CPD 后，可以检测到被单独留在无人看管车辆中的儿童，并向照护者或紧急服务部门发出警报，以避免中暑死亡 [3]。

早期的 CPD 研究已基于不同技术/设备进行了探索，总结于表 I。早期的 CPD 系统利用婴儿座椅上配备的各种传感器（如光学/重量/热/压力传感器）的信息来实现 [4]–[8]。然而，基于传感器的方法有几个缺点，例如覆盖范围有限，难以检测到婴儿座椅外的儿童；误报率高（例如，重量传感器难以将儿童与相同重量的无生命物品区分开）；以及针对不同儿童和汽车需要调整特定参数。后来，引入了无线收发器 [9] 和电容/电传感器 [10] 来降低误报率，但检测区域仍然局限于婴儿座椅内/旁。基于热释电红外 (pyroelectric infrared, PIR) 传感器的方法 [11]–[13] 通过检测儿童的运动，似乎能够扩大覆盖范围。然而，PIR 传感器易受周围环境温度的影响，这降低了其在实际应用中的可靠性。基于视觉的 CPD 系统 [14]–[16] 通过使用图像/视频处理，在检测准确性方面更加可靠。然而，它们通常需要专用硬件/摄像头，从而增加了汽车的成本和能耗。同时，图像/视频的质量易受光照条件影响。尽管最新的基于机器学习的技术 [17] 取得了更好的检测准确性，但它们仍然严重依赖输入图像的质量，从而阻碍了其实用部署。

近年来，基于射频 (radio frequency, RF) 的车内人体感知因其在保护隐私和无需佩戴传感器方面的优势 [26], [27]，已基于生命体征检测 [18], [19]、位置估计 [20]–[25] 等得到了广泛探索。然而，为了检测与生命体征相对应的微小运动，例如 4–12 毫米的胸部运动和 0.2–0.5 毫米的心跳位移，并进而识别人的存在，大多数方法 [20]–[25] 依赖毫米波 (millimeter-Wave, mmWave) 信号，而目前大多数车辆内部并不普遍配备这种信号。

尽管已经有许多关于实现 CPD 的工作被提出，但据我们所知，现有技术中还没有一种能够满足理想情况下全球部署应具备的以下所有要求：

1.  **准确**：应足够灵敏，以达到接近 100% 的检测率，因为每一次漏检都可能是灾难性的。伴随高灵敏度而来的是可能升高的误报率，但如果误报率合理较低，家长/照护者关闭误报警报并确信孩子得到更好保护并不费力。
2.  **快速响应**：应能对儿童的存在（如果有）做出快速响应，主要原因有二。首先，封闭车内 22°C (73°F) 的环境温度每 5 分钟可升高 3.47°C (6.25°F) [1], [28]，热衰竭可能在 40°C 时开始，而超过 54°C 通常会导致中暑 [29]。此外，家长/照护者希望在他们走离车足够远之前尽早收到警报。
3.  **大覆盖范围**：应能覆盖整个汽车内部，无任何盲点，包括座位上和座位下，因为儿童挣扎时可能跌落到脚部空间区域。
4.  **无需校准**：应能在不同车型、不同年龄/性别/体重的儿童、各种天气/温度/环境下鲁棒地工作，无需校准。
5.  **低成本**：CPD 的安装应非常简便，理想情况下应能重复利用当前车内设施，无需额外的硬件更改。

表 I 现有车内 CPD 最先进技术比较

| 方法 | 覆盖范围 | 低成本 | 无需校准 | 准确 | 快速响应 |
|------|----------|--------|----------|------|----------|
| 传感器（重量/压力）[4]–[10] | 婴儿座椅 | × | × | × | 快速³ |
| PIR（运动/传感）[11]–[13] | 视线 (LoS)¹ | ✓ | ✓ | × | 中等 |
| 视觉（图像/视频）[14]–[16] | 视线 (LoS) | × | ✓ | ✓ | 中等 |
| 雷达（生命体征/运动）[20]–[25] | 视场 (FoV)² | × | ✓ | ✓ | 快速 |
| **WiCPD（运动/呼吸/分类器）⁴** | **整个车厢** | **✓** | **✓** | **✓** | **≤ 6/17秒** |

¹ 视线 (Line-of-Sight, LoS)
² 雷达阵列的视场 (Field-of-View, FoV)
³ 如果 CPD 系统能在 1 分钟内响应，则视为快速；长于 1 分钟则在本论文中称为中等，这是根据 [1] 中车内温度上升实验经验计算得出的。
⁴ 6 秒对应清醒/运动婴儿，17 秒对应睡眠/静止婴儿，检测率 ≥ 95%

随着物联网 (Internet of Things, IoT) 时代 Wi-Fi 的普及，全球范围内约有 12%–33%（不同地区有所差异）的在用车辆已配备 Wi-Fi，并且更多车辆（到 2025 年约 4 亿辆 [30]–[34]）计划配备 Wi-Fi 设备。因此，通过重复利用车内 Wi-Fi 设备，许多基于车内 Wi-Fi 感知的功能应运而生，例如驾驶员活动监测 [35]、情绪感知 [36] 等，以提高驾驶安全性。因此，我们提出以下问题：我们能否使用 Wi-Fi 感知来实现 CPD？尽管 Wi-Fi 感知技术已推动了许多实用的车内应用 [35]–[39]，但要使用商用 Wi-Fi 实现 CPD 并同时满足上述所有五个要求，仍面临若干挑战。

首先，虽然现有工作已表明利用无线信号变化 [40] 检测成人 [41] 的可行性，但儿童的体型比成人小得多，其运动/呼吸强度也弱得多。因此，儿童对无线信号的影响远比成人弱，使得检测他们更具挑战性。基于毫米波的方法 [21]–[25] 因其波长短、带宽大，已被证明能够捕捉活动运动（头/手臂/躯干运动）和呼吸运动（静止儿童的胸部运动）。然而，基于毫米波的方法的覆盖范围仅限于相应毫米波雷达的视场 (Field-of-View, FoV) 内。

其次，实现 CPD 的快速响应并非易事。尽管现有的运动检测器 [40], [41] 可在一般运动感知应用中使用，但它们大多依赖于信道分布的变化检测，例如方差、相位差等。由于它们没有建设性地利用来自所有动态散射体的反射信号，大多数需要较长的样本窗口，从而导致较大的检测延迟。例如，DeMan [41] 需要 500 个数据包（即，在 30 Hz 采样率下约 17 秒）才能实现对运动中的成人达到 98% 的检测率。由于儿童体型更小、运动强度更弱，检测儿童所需的检测延迟预计会更长。

为了应对这些挑战，我们考虑采用一种统计电磁 (electromagnetic, EM) 模型 [42]，该模型计算由所有多径分量组成的信道状态信息 (channel state information, CSI) 测量的自相关函数 (autocorrelation function, ACF)。然后，我们阐述了一个运动统计量指标来量化周围运动的强度，该指标可以确保每个动态散射体，无论其位置如何，都能建设性地贡献于整体运动统计量。然而，在大多数传统的基于变化的方法中，不同位置的动态散射体可能破坏性地贡献于 CSI 的变化。因此，我们的运动检测比大多数传统的基于变化的方法 [40] 更灵敏。

为了无论儿童处于何种状态（即运动/清醒、静止/睡眠或介于两者之间（睡眠中偶尔运动））都能准确检测儿童，我们设计了三个目标检测器：1) **运动目标检测器**：基于运动统计量检测运动/清醒状态的儿童；2) **静止目标检测器**：用于识别运动很少的静止儿童，例如在睡眠中。我们考虑采用最大比合并 (maximal ratio combining, MRC) [43] 方案，利用每个子载波上的运动强度作为信道增益，来提高 ACF 的信噪比 (signal-to-noise-ratio, SNR)。然后利用增强后的 ACF 提取微小的呼吸模式/胸部运动，并估计儿童的呼吸频率 [44]。接着，我们设计了一个模块来检查是否能检测到连续的呼吸频率，且该呼吸频率应在儿童的正常范围内，以用于静止儿童检测；3) **基于朴素贝叶斯分类器的过渡目标检测器**：用于处理当运动和静止目标检测器均未能检测到处于过渡状态的儿童的情况。例如，睡眠中的儿童可能有轻微运动，如轻微的摇头或手臂运动。这种运动过于微弱和短暂，无法被运动目标检测器检测到。同时，它破坏了呼吸频率估计的连续性，因此也被静止目标检测器遗漏。据我们所知，这是在 CPD 设计中首次考虑这种过渡状态，经实验验证（见第 IV-C 节），它有助于将检测精度提高约 5%。

由于 WiCPD 建设性地利用了来自所有动态散射体的反射信号，运动目标检测器可以在两个连续的 CSI 样本/测量值内检测到运动，对应于最短延迟为 2Δt，其中 Δt = 1 / F_s，F_s 为采样频率。另一方面，MRC 方案可以显著提高 ACF 的 SNR（见图 5），优于简单的子载波平均 [45]，从而在 20 秒内对静止儿童达到 96.56% 的检测率，而 DeMan 需要 3000 个数据包（在 30 Hz 采样率下约 100 秒）才能对成人达到 95% 的检测率。

我们已使用商用 NXP Wi-Fi 芯片组 [46] 构建了 WiCPD 的实时原型系统，并在 20 多辆不同的汽车上进行了大量实验，检测了不同年龄和性别的五名儿童，车辆停放在各种地点。我们还进行了长期测试，以评估实时 WiCPD 系统的 CPU 和内存消耗。结果表明，仅消耗 11% 的 Dual-core ARM Cortex-A7 CPU（最高 1 GHz）和 40 MB 随机存取存储器 (random access memory, RAM)，WiCPD 就能在检测车内儿童时，无论其状态如何，都能在 20 秒内实现大于 96.5% 的检测准确率，且响应时间短。

总而言之，WiCPD 的主要贡献如下：

1.  据我们所知，WiCPD 是首个基于商用 Wi-Fi 的 CPD 系统，具有高检测率、快速响应时间和广覆盖范围。只要能够获取 CSI [31], [32]，它就可以轻松集成到当前和未来的车载 Wi-Fi 系统中，安装成本极低。
2.  我们提出了一个统一的 CPD 框架，包含三个目标检测器模块，用于检测所有可能状态下的儿童。WiCPD 表现出 ≥96.4% 的检测准确率，≤3.96% 的误报率，检测运动儿童 ≤8 秒，检测静止/睡眠儿童 ≤20 秒。
3.  我们在不同天气/温度/环境下，使用婴儿玩偶和不同年龄/性别/体重的真实婴儿进行了大量实验。实验证明，WiCPD 能够以可接受的 CPU 和内存消耗实现准确、鲁棒且快速的检测，使其成为全球部署的有希望的候选方案。

本文的其余部分组织如下。第二部分介绍统计信号模型。第三部分介绍 WiCPD 的设计，接着在第四部分介绍实现和评估。最后，第五部分讨论局限性和未来工作，第六部分总结本文。

## II. 统计信号模型 (Statistical Signal Model)

### A. 商用 Wi-Fi 上的 CSI (CSI on Commercial Wi-Fi)

令 X(t,f) 和 Y(t,f) 分别表示在时间 t 频率为 f 的子载波上发送和接收的信号。那么，相应的 CSI 可以估计为 H(t,f) = Y(t,f) / X(t,f) [50]。由于多径效应，H(t,f) 可以表示如下：

$$H(t,f) = s(t,f) + n(t,f) = \sum_{l = 1}^{L}\alpha_{l}(t,f)e^{-j2\pi f\tau_{l}(t)} + n(t,f) \quad (1)$$

其中 s(t,f) 表示由所有传播路径组成的信道信息，n(t,f) 表示在时间 t 和频率 f 处功率密度为 σ_n²(f) 的加性高斯白噪声 (additive white Gaussian noise, AWGN)。L 是路径数，α_l 和 τ_l 分别表示第 l 条路径的复增益和传播时延。

### B. 统计 CSI 模型 (Statistical CSI Model)

在这一部分，我们基于电磁场 (EM fields) 的叠加特性 [42] 介绍公式 (1) 中 s(t,f) 的统计模型。需要注意的是，统计模型、运动统计量以及使用 MRC 增强 ACF 以更好地估计呼吸频率，最初是在我们之前针对室内感知应用的工作 [42], [44], [51] 中提出的。本文简要回顾它们以保持完整性，同时重点探索其在车内环境和 CPD 中的性能/可靠性。

该统计模型的直觉/原理是，每个散射体都可视为一个虚拟天线，它将接收到的电磁波向所有方向发射，如图 1 所示。这些电磁波在车内散射体上反射后，最终在接收天线处叠加 [52]。因此，s(t,f) 可以重写如下：

$$s(t,f) = \sum_{k\in \Omega_s(t)}s_k(t,f) + \sum_{m\in \Omega_d(t)}s_m(t,f) \quad (2)$$

其中 Ω_s(t) 和 Ω_d(t) 分别表示静态和动态散射体的集合。s_k(t,f) 和 s_m(t,f) 分别表示由第 k 个静态散射体和第 m 个动态散射体发射/反射的电磁波。

在实际中，在很短的时间内，可以合理地假设 Ω_s(t) 和 s_k(t,f)（k ∈ Ω_s(t)）几乎不变。因此，所有静态散射体的贡献可以近似为一个常数，即：

$$s(t,f)\approx E_s(f) + \sum_{m\in \Omega_d(t)}s_m(t,f) \quad (3)$$

其中 E_s(f) = ∑_{k∈Ω_s(t)} s_k(t,f)。

![图 1: 车内多径传播。](placeholder)

给定信道互易性，双向传播的电磁波将经历相同的变化 [52]。因此，如果接收机 (receiver, RX) 发射电磁波，在第 m 个散射体/虚拟天线处接收到的电磁波将与 s_m(t,f) 相同。因此，s_m(t,f) 可以表示如下 [53]：

$$s_m(t,f) = \int_0^{2\pi}\int_0^\pi F_m(\Theta ,f)\exp \left(-j\vec{k}\cdot \vec{v}_m t\right)\sin (\alpha)\mathrm{d}\alpha \mathrm{d}\beta \quad (4)$$

其中 \vec{v}_m 表示第 m 个散射体的运动速度。F(Θ) 表示来自方向 Θ = (α, β) 的复增益，α 和 β 分别表示仰角和方位角。\vec{k} = -k(\vec{x}\sin (\alpha)\cos (\beta) + \vec{y}\sin (\alpha)\sin (\beta) + \vec{z}\cos (\alpha))，k = (2πf)/c 是自由空间波数，c 为光速。

## III. WiCPD 设计 (WiCPD Design)

本节介绍 WiCPD 的设计，旨在利用商用 Wi-Fi 实现准确、快速响应的车内 CPD。一般来说，我们可以将儿童分为以下三种不同状态，并设计相应的检测器模块。

1.  **运动 (Motion)**：清醒的儿童有频繁/大量的运动，例如挥动手臂/腿/躯干，挣扎着离开婴儿座椅时的随机身体运动等。通常，这种运动会产生大量动态散射体，可以被运动目标检测器检测到。
2.  **静止 (Stationary)**：睡眠中的儿童运动可忽略不计。静止目标检测器旨在捕捉儿童连续的呼吸频率，从而识别其存在。
3.  **过渡 (Transition)**：儿童正在睡觉，但有轻微且间歇性的运动，例如睡眠中偶尔动一下头。由于这种运动仅对应儿童身体非常小部分的运动（即少数动态散射体），它不会导致 CSI 发生可检测的变化，因此无法被运动目标检测器捕获。然而，它会破坏与微小胸部运动相对应的细微呼吸运动，导致连续的呼吸频率估计失败。为了处理这种情况，我们设计了过渡目标检测器。

### A. 系统概述 (System Overview)

图 2 描述了 WiCPD 系统的概览。左侧是车内环境的图示，绿色虚线表示车内的多径信号传播。RX 测量来自 TX 发送的数据包的 CSI。CSI 测量值首先通过 Hampel 滤波器 [54] 去除由实际失真（如锁相环 (phase-locked loops, PLLs) 的抖动 [55]）引起的异常值。之后，CSI 由运动目标检测器处理，以检测车内是否有儿童在运动。如果决策为“是”，WiCPD 报告“车内有儿童”。否则，触发静止目标检测器来检测静止儿童的存在。如果运动目标检测器和静止目标检测器均未检测到儿童存在，则过渡目标检测器将进一步确认儿童是否处于过渡状态，即睡眠中伴有轻微/间歇性运动。只有当上述三个检测器均未检测到车内有儿童时，WiCPD 才输出“车内无儿童”。

![图 2: WiCPD 系统概述。](placeholder)

为了检测车内运动中的儿童，我们利用 Wi-Fi CSI 测量的统计 ACF 和一个运动统计量指标，该指标建设性地利用来自所有动态散射体的反射信号来反映周围运动目标的瞬时环境运动强度。

为了检测静止儿童，我们首先利用每个子载波上的运动统计量来挑选对运动敏感的子载波，因为儿童的呼吸运动对应于细微的胸部运动，容易被噪声淹没。然后，利用这些被选子载波上的运动统计量作为权重，以 MRC 方式进一步提高呼吸运动的 SNR [44], [56]。之后，估计呼吸频率，只有当正常呼吸频率被估计出并持续一定时长时，才检测到“车内有儿童”，因为瞬时呼吸频率可能被噪声破坏。

为了检测过渡状态的儿童，我们利用以下三个观察结果：1) 可以估计出间歇性的呼吸频率，即使不是连续的；2) 有儿童存在时的运动统计量与真正空车时的运动统计量不同，即使仅使用运动目标检测无法区分；3) 受 [59] 启发，CSI 的协方差矩阵 R_H 的前 k 个最大特征值可以代表当前目标的多径分布（更具体地说，到达角 (Angle of Arrival, AoA)）。因此，我们利用 R_H 的前 k 个最大特征值构建一个新特征，以提取由过渡状态儿童的间歇轻微运动引起的 CSI 在 AoA 上的变化。最终，运动统计量、呼吸频率估计值和 R_H 的前 k 个最大特征值构成一个特征向量，然后输入到朴素贝叶斯分类器 [60] 中，以检测是否存在处于过渡状态的儿童。

接下来，我们将详细介绍运动目标检测器、静止目标检测器和过渡目标检测器。

### B. 运动目标检测器 (Motion Target Detector)

为了检测运动中的儿童，我们借鉴之前工作 [42] 中的建模，该工作建立了 CSI 的 ACF 与周围动态散射体/物体运动之间的联系，定义了一个运动统计量来量化周围运动的强度，并提出了一个基于运动统计量的运动目标检测器。然而，我们是在车内环境中对其进行评估。

**ACF 计算 (ACF Calculation):** 回顾公式 (1)，测量到的 CSI 的 ACF 由下式给出：

$$\rho_{H}(\tau ,f) = \frac{\mathrm{cov}[H(t,f),H(t + \tau ,f)]}{\sqrt{\sigma_{H(t,f)}^{2}\sigma_{H(t + \tau,f)}^{2}}} \quad (5)$$

其中 cov[A,B] 表示 A 和 B 之间的协方差。σ_H(t,f)² 和 σ_H(t+τ,f)² 分别表示 H(t,f) 和 H(t+τ,f) 的方差。将公式 (1) 和 (2) 代入 (5)，我们得到：

$$\rho_{H}(\tau ,f) = \frac{E_{d}^{2}(f)}{E_{d}^{2}(f) + \sigma_{n}^{2}(f)}\rho_{s}(\tau ,f) + \frac{\sigma_{n}^{2}(f)}{E_{d}^{2} + \sigma_{n}^{2}(f)}\delta (\tau) \quad (6)$$

其中 δ(·) 是狄拉克δ函数。E_d²(f) 是 s(t,f) 的方差，它衡量所有动态散射体反射的功率。ρ_s(τ, f) 可以写成：

$$\rho_{s}(\tau ,f)\approx \frac{1}{E_{d}^{2}(f)}\sum_{m\in \Omega_{d}}\sigma_{f_{m}}^{2}(f)\rho_{E_{m}}(\tau ,f) \quad (7)$$

其中 ρ_{E_m}(τ, f) = ∑_{u∈{x,y,z}} ρ_{E_m,u}(τ, f) 表示接收到的电磁场在 {x, y, z} 方向上的自相关，σ_{f_m}²(f) 表示第 m 个动态散射体的反射功率。可以证明 ρ_{E_m}(τ, f) 在 τ = 0 处是连续函数 [52]。因此，在公式 (7) 中，如果存在动态散射体，即 σ_{f_m}²(f) > 0，那么当 τ → 0 时，ρ_s(τ, f) → 1。将公式 (7) 代入 (6)，我们有：

$$\rho_{H}(\tau ,f) = \frac{E_{d}^{2}(f)}{E_{d}^{2}(f) + \sigma_{n}^{2}(f)} > 0, \quad \text{当 } \tau \rightarrow 0. \quad (8)$$

否则，如果没有动态散射体，我们有 σ_{f_m}²(f) = 0 且 E_d²(f) = 0，因此：

$$\rho_{H}(\tau ,f) = \frac{E_{d}^{2}(f)}{E_{d}^{2}(f) + \sigma_{n}^{2}(f)} = 0, \quad \text{当 } \tau \rightarrow 0. \quad (9)$$

因此，lim_{τ→0} ρ_H(τ, f) 可以作为周围动态散射体/运动目标的指示器。更重要的是，所有动态散射体的散射功率在公式 (7) 中以建设性的方式相加，从而使其对目标运动更加敏感。作为初步验证，我们在一辆典型汽车中，使用一对具有 2 个 TX 天线和 2 个 RX 天线、运行在 5.8 GHz 且带宽为 40 MHz 的 Wi-Fi 设备，收集了约 3 分钟有和没有运动/清醒儿童存在时的 CSI。图 3 显示，运动统计量在空车环境和有运动环境中的表现不同。此外，它与空车和有运动情况之间的重叠比 CSI 方差少，从而表明运动统计量具有更好的灵敏度。

**运动统计量 (Motion Statistics):** 在实践中，lim_{τ→0} ρ_H(τ, f) 可以用 lim_{τ→0} ρ_H(τ, f) ≈ ρ_H(1/F_s, f) 来近似，并且我们对所有子载波上的 lim_{τ→0} ρ_H(τ, f) 进行平均，以获得更可靠的运动指标，此后称之为运动统计量，即：

$$\hat{\rho}_H(\tau) = \frac{1}{N_f}\sum_{i = 1}^{N_f}\rho_H\left(\frac{1}{F_s},f_i\right) \quad (10)$$

其中 F_s 是采样率，N_f 表示子载波数，f_i 表示第 i 个子载波的频率。为避免因瞬时失真/噪声导致运动统计量出现异常值，在实际实验/应用中，我们使用 2 秒的滑动窗口来计算平均运动统计量。

一个问题：运动统计量是否容易受到车外运动/动态目标（例如过往车辆和行人）的影响？我们注意到，运动统计量对车外运动具有鲁棒性。这是因为封闭的车辆可以看作一个金属腔体，它将大部分无线电信号限制在车内，同时屏蔽了外部的无线/射频干扰。我们在第 IV-C 节中展示了实验验证。

**运动目标检测器 (Motion Target Detector):** 给定运动统计量，运动目标检测器的原理非常直接，即当运动统计量大于阈值 η₀ 时，检测到车内有儿童，η₀ 推导如下。

当车内无运动时，根据公式 (1) 和 (3)，CSI H(t, f_i) 仅由静态散射信号 E_s(f) 和白噪声 n(t, f) 组成，即 H(t, f_i) = E_s(f) + n(t, f)，其中 E_s(f) 在空车中可视为常数。因此，给定足够大的样本数 N_T，ρ_H(1/F_s, f_i) 可近似为均值为 1/N_T、方差为 1/N_T 的高斯变量，即 ρ_H(1/F_s, f_i) ∼ N(1/N_T, 1/N_T)。因此，空车中运动统计量 \hat{ρ}_H(τ) 的分布可以近似为：

$$\hat{\rho}_H(\tau)\sim \mathcal{N}\left(\frac{1}{N_T},\frac{1}{N_T N_f}\right). \quad (11)$$

为验证公式 (11) 中的推导，我们使用一对商用 Wi-Fi 设备（载波频率 f_c = 5.805 GHz，带宽 40 MHz）在空车中收集了 1 小时的 CSI 数据。图 4 显示了根据实际 CSI 测量值计算出的 \hat{ρ}_H(τ) 的分位数-分位数 (Quantile-Quantile, Q-Q) 图，以及均值为 1/N_T、方差为 1/(N_T N_f) 的高斯分布，这验证了我们的推导。

![图 3: 空车和有运动/清醒儿童存在的车内，运动统计量与 CSI 方差的比较。](placeholder)

![图 4: 空车中 \hat{ρ}_H(τ) 的统计分布。](placeholder)

根据公式 (11)，我们可以推导出具有预定误报率 p_F 的运动检测阈值 η₀，即：

$$P(\hat{\rho}_H(\tau) > \eta_0) = p_F \Rightarrow \eta_0 = Q^{-1}(p_F) * \frac{1}{\sqrt{N_T N_f}} + \frac{1}{N_T} \quad (13)$$

其中 Q⁻¹(·) 是 Q 函数的反函数，Q(x) = (1/√(2π)) ∫_x^∞ exp(-u²/2) du。

### C. 静止目标检测器 (Stationary Target Detector)

尽管现有工作已证明使用无线信号估计成人呼吸频率的可行性 [41], [61], [62]，但估计儿童的呼吸频率更具挑战性，因为儿童体型比成人小得多，其运动/呼吸强度也弱得多。因此，由于胸部运动轻微，儿童的呼吸信号 SNR 非常低。为解决此问题，我们首先选择具有前 N_s（默认为 10）个最大运动统计量的子载波，旨在提取对细微胸部/呼吸运动最敏感的子载波。然后，利用 MRC [43] 方案来最大化用于呼吸频率估计的 ACF 的 SNR [44]。我们简要回顾主要过程如下，详情可参考文献 [44]。

![图 5: 呼吸信号的增强 ACF 及相应的呼吸频率估计。(a) 平均 ACF。(b) MRC 增强 ACF。(c) 呼吸频率估计。](placeholder)

考虑 MRC 技术，增强后的 ACF 可以表示如下：

$$\hat{\rho}_H^b (\tau) = \sum_{i = 1}^{N_s}\omega (f_i)\rho_H(\tau ,f_i) \quad (14)$$

其中 ω(f_i) 是子载波 f_i 上呼吸强度的信道增益。回顾公式 (6)，子载波 f_i 在 ACF 方面的信道增益为 E_d²(f) / (E_d²(f) + σ_n²(f))，这可以通过 lim_{τ→0} ρ_H(τ, f_i) 来估计（详见公式 (7)-(9)）。因此，WiCPD 将 lim_{τ→0} ρ_H(τ, f_i) 作为最优 ω(f_i)，从而呼吸信号的 ACF 可以通过下式增强：

$$\hat{\rho}_H^b (\tau) = \sum_{i = 1}^{N_f}\left[\lim_{\tau \to 0}\rho_H\left(\tau = \frac{1}{F_s},f_i\right)\right]\rho_H(\tau ,f_i). \quad (15)$$

图 5 显示了一个示例，当婴儿玩偶在车内睡觉（真实呼吸频率为 27.5 BPM）时，CSI 测量值的 ACF。如图 5(a) 和 (b) 所示，与直接对所有子载波进行平均的现有方法 [45] 相比，MRC 方法在 ACF 方面实现了约 10 dB 的改善。因此，从 MRC 增强的 ACF 估计出的呼吸频率更加准确和连续，如图 5(c) 所示。注意，我们无法直接最大化呼吸信号/运动，因为无法直接从 CSI 测量中提取呼吸信号的信道增益。但是，通过将每个子载波的运动统计量作为最优权重应用于 ACF 的 MRC，规避了这个问题。

一旦我们得到增强的 ACF \hat{ρ}_H^b(τ)，儿童的呼吸频率可以通过 f_B = 60 / \hat{τ} (BPM) 估计，其中 \hat{τ} 对应于 \hat{ρ}_H^b(τ) 中第一个峰值的时间滞后。之后，如果检测到正常儿童的呼吸频率（即在 [6, 35] BPM 范围内）并持续一定时长，静止目标检测器报告“车内有儿童”。

### D. 过渡目标检测器 (Transition Target Detector)

如果之前的运动目标检测器和静止目标检测器均未检测到“车内有儿童”，则触发过渡目标检测器。

处于过渡状态的儿童可以引起一定水平的运动统计量和间歇性的呼吸频率估计，这与真正的空车表现出不同的模式，即使它们无法仅通过运动目标检测器或静止目标检测器检测到。为了检测过渡状态目标的存在，我们考虑这样一个事实：儿童的轻微/间歇性运动可以改变部分多径信号的到达角 (AoAs)。即使这种变化不能直接用于估计目标的 AoA，但它固有地嵌入在测量的 CSI 中，并且可以被用作一个新特征。我们利用 CSI 的协方差矩阵 R_H [59] 的前 k 个最大特征值来提取间歇运动信息，并构建一个包含运动统计量、呼吸频率和 R_H 的前 k 个最大特征值的特征向量。所有这些特征在朴素贝叶斯分类器中融合在一起。我们省略朴素贝叶斯分类器的细节，读者可参考 [59]。

## IV. 评估 (Evaluation)

为了全面评估 WiCPD，我们使用商用 Wi-Fi 芯片组构建了一个实时系统，并在各种车型中进行了大量实验，使用真实婴儿和婴儿玩偶来验证 CPD 检测性能。

### A. 方法 (Methodology)

**实现 (Implementation):** 如图 6(a) 所示，我们使用商用 NXP Wi-Fi 芯片组构建了硬件系统，该芯片组支持 2.4 和 5 GHz 双频。为了获取 CSI，我们修改了 Wi-Fi 芯片组的驱动程序。主处理板是 PICO-IMX7 系统模块，包含一个最高 1 GHz 的双核 ARM Cortex-A7 CPU。Wi-Fi 芯片组包含两个带有 U.FL/IPX 连接器接口的天线，可以方便地连接到外部 PCB 天线或车载天线。RX 接收从 TX 发送的数据包，并捕获 CSI，除非另有说明，否则包含 58 个子载波，采样率为 30 Hz。TX 和 RX 均由两个全向 PCB 天线组成。系统运行在 5.805 GHz 信道（默认美国信道 161），带宽为 40 MHz。

我们首先使用 MATLAB 开发算法和实时系统，用于性能分析和验证。然后使用 C++ 实现，并移植到运行 Linux 系统的 PICO-IMX7 板上。为了显示 CPD 估计结果，我们使用 Python 开发了一个演示套件，如图 6(b) 所示，它显示实时运动统计量、呼吸频率估计和 CPD 检测结果。在实时实验期间，RX 持续捕获 CSI 并运行 WiCPD 算法来计算相应的 CPD 决策，然后每隔 1 秒通过 Wi-Fi 发送到演示套件。

![图 6: (a) 硬件平台。(b) WiCPD 演示套件 GUI。](placeholder)

**数据收集 (Data Collection):** 数据收集主要包括三部分：1) 在 20 多辆不同汽车的不同场景下收集空车情况数据；2) 使用逼真的 Ashley 婴儿玩偶进行实验；3) 使用真实婴儿进行实验。由于汽车大部分时间都是空的，我们通过在 20 多辆不同汽车上收集数据，对误报进行了广泛评估。对于婴儿玩偶测试，我们将 WiCPD 部署在四辆不同的家用车中⁵⁻⁶。对于每辆车，TX 首先位于仪表板中央，然后位于手套箱中，如图 7 所示。RX 安装在四个不同位置，包括三个在后排座椅顶部附近的车顶内衬上和后排车门杯架中的一个。如图 7 所示，我们总共有八种 TX-RX 配置和两类测试对象，包括：1) 婴儿玩偶：总共使用婴儿玩偶作为对象测试了车内的 11 个不同位置。具体来说，婴儿玩偶坐在标准汽车座椅上 [见图 7(a)] 的位置 #3、#4、#9、#10 和 #11，而将其放在脚部空间区域（无汽车座椅）的位置 #1、#2、#5、#6、#7 和 #8，以模拟儿童从汽车座椅上跌落的情况。在每个位置，当婴儿玩偶坐在汽车座椅上时，分别测试面向前和面向后的方向。此外，当婴儿玩偶在脚部空间区域时，测试了三种不同的姿势，包括面朝上躺在地板上、面朝下躺在地板上和坐在地板上。2) 真实婴儿：我们还评估了 WiCPD 在检测五名不同年龄和性别的真实婴儿方面的性能，如表 II 所示。总体而言，我们的实验范围从四个月的婴儿到五岁的幼儿，身高和体重各不相同。在测试期间，婴儿坐在汽车座椅上并系好安全带，位于位置 #3、#4、#9、#10 和 #11。由于脚部空间区域空间有限，无法放置汽车座椅，我们未对该区域进行测试。TX 放置在位置 #1，而 RX 的两个天线分别安装在位置 #1 和 #3，以获得最佳覆盖。

![图 7: 实验场景。测试了婴儿的 11 个不同位置、TX 的 2 个不同位置和 RX 的 4 个不同位置。注意：位置 #1、#2、#5、#6 和 #7 的婴儿/儿童代表脚部空间区域，而 #8 代表掀背车的后备箱区域。位置 #1 的 TX 固定在仪表板中央，而位置 #2 的 TX 放在手套箱内。位置 #1、#2 和 #3 的 RX 位于座椅顶部附近的车顶内衬上，而位置 #4 表示后车门上的杯架。(a) 实验设置示例。(b) 典型汽车中的测试场景。](placeholder)

表 II WiCPD 实验的婴儿对象

| # | 年龄 (月) | 性别 | 身高 (英寸) | 体重 (磅) |
|---|-----------|------|-------------|-----------|
| 1 | 4         | 男   | 25          | 16.07     |
| 2 | 6         | 女   | -           | -         |
| 3 | 20        | 男   | 32.9        | 24        |
| 4 | 36        | 男   | -           | -         |
| 5 | 58        | 女   | 44.4        | 43        |

实验在 13 个月内的不同日子进行，地点包括贸易中心旁的室外停车场、购物中心的地下停车场、路边停车位和家庭使用的私人车库。随着车主日常使用汽车，车内环境会发生自然变化。注意，在此期间，车内环境允许自由变化，没有任何限制。WiCPD 是一个无需校准的系统，可在不同的真实环境中工作，没有任何不切实际的假设/约束。

### B. 整体准确率 (Overall Accuracy)

为了评估 WiCPD 的整体性能，我们展示了三个关键指标：响应时间、检测率和误报率¹⁰，因为它们是及时检测/救援单独留在车内的儿童的关键要求。注意，我们没有独立评估检测过渡状态儿童的表现，因为过渡状态通常与睡眠/静止状态合并。此后，“睡眠”指静止的睡眠儿童，“清醒”指儿童清醒且更可能产生运动。

图 8 显示，对于有正常活动（如挣扎着离开汽车）的运动中清醒儿童，WiCPD 在不到 8 秒的响应时间内达到 100% 的检测率。此外，对于睡眠儿童，WiCPD 在 20 秒内显示出 96.56% 的检测率。如图 8(a) 所示，WiCPD 检测睡眠儿童需要稍长的响应时间。这是因为静止目标检测器至少需要一个以上的呼吸周期时间来估计呼吸频率，从而导致更长的延迟。然而，图 8 显示，总体而言，WiCPD 在响应时间小于 20 秒时仍能达到超过 97.87% 的检测率，这对于 CPD 应用来说是一个相当安全的时间。图 8(c) 显示了 WiCPD 的接收器工作特性曲线 (Receiver's operating characteristic curve, ROC)，表明对于清醒和睡眠儿童，在检测率 ≥96.4% 的情况下，误报率分别为 1.04% 和 3.96%。

![图 8: WiCPD 系统的整体性能。(a) 检测率。(b) ROC 曲线。](placeholder)

### C. 与现有工作的比较 (Comparison With Existing Works)

我们还实现了一个基准方法（在图 8 中称为“Overall-CSI variance”），其中我们将运动目标检测器模块替换为基于 CSI 方差的运动检测器 [40]，并排除了过渡目标检测器。从图 8 可以看出，WiCPD 在所有响应时间、检测率和误报率方面均优于基准方法，主要因为：

1.  **鲁棒的运动目标检测器 (Robust Motion Target Detector):** 一方面，WiCPD 利用基于考虑所有多径分量的统计 EM 模型的运动统计量。因此，它比基于 CSI 方差的方法对周围运动具有更好的灵敏度，如第 III-B 节和图 3 所示。另一方面，由于封闭的汽车可以视为一个封闭的金属盒，能很好地将车内和车外的无线/射频信号隔离开，因此 WiCPD 对车外运动也具有非常鲁棒的性能。为了验证这一点，我们在八种不同场景下收集了空车数据，即 a) 空车停在室外停车场，周围无运动目标；b) 周围有汽车驶过，例如经过相邻停车位；c) 行人绕空车行走；d) 车库中的空车；e) 刮风天气下的空车；f) 路边停车的空车，偶尔有汽车经过；g) 室外停车场；h) 雨天。从图 9(a) 可以看出，对于车外运动，超过 99.29% 的运动统计量小于 0.1，而 94.4% 的车内运动表现出大于 0.1 的运动统计量。然而，如图 9(b) 所示，CSI 方差在车内和车外运动之间显示出更大的重叠。因此，WiCPD 不仅擅长捕捉周围运动，而且对车外运动具有鲁棒性，有望成为实际应用中的鲁棒解决方案；
2.  **强大的静止目标检测器 (Powerful Stationary Target Detector):** WiCPD 利用运动统计量作为信道增益，首先选择对呼吸运动敏感的子载波，然后以 MRC 方式合并它们。因此，WiCPD 可以大大提高嵌入在 ACF 中的呼吸运动的 SNR（见图 5），并实现更准确、更快速的呼吸频率估计，以捕捉车内的静止儿童；
3.  **新颖的过渡目标检测器 (Novel Transition Target Detector):** 为了处理实验观察到的、运动目标检测器和静止目标检测器均遗漏的情况¹¹，我们提出了一个基于朴素贝叶斯分类器的过渡目标检测器，据我们所知，现有工作中从未考虑过这一点。为了独立测试过渡检测器的贡献，我们仅使用真实婴儿数据，排除了婴儿玩偶的数据。这主要是因为婴儿玩偶通常是静态的或运动的，缺乏仅存在于真实儿童中的这种过渡状态¹²。表 III 展示了有和没有过渡目标检测器时 CPD 的性能。可以看出，通过加入过渡目标检测器，WiCPD 的检测率提高了约 5.5%，而误报率仅适度增加了 0.65%。

![图 9: 运动统计量与 CSI 方差的比较：在空车或有运动/清醒儿童存在的车内。(a) 使用运动统计量检测的 CDF。(b) 使用 CSI 方差检测的 CDF。](placeholder)

表 III 过渡目标检测器评估

| CPD 无过渡检测器      | CPD 有过渡检测器，即 WiCPD |
|----------------------|---------------------------|
| 真阳性率¹ (TP) | 假阳性率² (FP) | 真阳性率 (TP) | 假阳性率 (FP) |
| 93.1%                | 1.14%           | 98.6%           | 1.79%          |

¹ 真阳性率 (True Positive Rate, TP)，即检测率。
² 假阳性率 (False Positive Rate, FP)，即误报率。

### D. 采样率的影响 (Impact of Sample Rate)

如第 III-B 节所示，更高的采样率 F_s 可以为 WiCPD 提供更好的运动统计量和呼吸估计，从而提高整体性能。然而，在实际中，更高的采样率会增加硬件系统的开销，并可能对周围的 Wi-Fi 网络造成干扰。因此，我们评估了不同采样率下的性能，这可以指导实际设置。图 10(a) 显示，对于清醒儿童的检测，WiCPD 需要 13、8 和 8 秒的响应时间才能达到 100% 的检测准确率，分别对应 10、20 和 30 Hz 的采样率。此外，在检测静止儿童时，对于 10、20 和 30 Hz 的情况，WiCPD 在 20 秒响应时间内均能达到 ≥96.18% 的检测率。注意，在实际中，CPD 系统应首先考虑检测率，因为每次漏检都可能是灾难性的。因此，我们推荐 F_s = 30 Hz，这对大多数 Wi-Fi 系统来说是可以承受的。

![图 10: WiCPD 性能与采样率 F_s 的关系。(a) 检测率。(b) ROC 曲线。](placeholder)

### E. 有效带宽的影响 (Impact of Effective Bandwidth)

我们研究了由天线和带宽共同决定的有效带宽的影响。具体来说，有效带宽 N_e 定义为 N_e = N_s B，其中 N_s = N_TX · N_RX 表示 TX 和 RX 之间的空间链路数，N_TX 和 N_RX 分别表示 TX 和 RX 的数量。B 是 Wi-Fi 系统的带宽，在 WiCPD 系统中，5 GHz 信道为 40 MHz，2.4 GHz 信道为 20 MHz。图 11(a) 显示，当检测清醒儿童且响应时间设置为 8 秒时，随着有效带宽从 40 MHz 增加到 160 MHz，WiCPD 的检测率从 85.46% 提高到 92.86%，再到 100%。对于睡眠儿童的存在，当 N_e 从 40 MHz 增加到 160 MHz 时，在同样 8 秒响应时间内，检测率从 75.62% 增加到 86.68%。如图 11 所示，响应灵敏度和误报率也随着有效带宽的增加而改善。总体而言，图 11 表明，通过使用典型的 2×2 Wi-Fi 系统和 40 MHz 带宽，WiCPD 可以取得显著的性能。

![图 11: WiCPD 性能与带宽 N_e 的关系。(a) 检测率。(b) ROC 曲线。](placeholder)

### F. 环境无关性 (Environment Independence)

要成为一个全球性的 CPD 系统，评估其在不同环境下的鲁棒性非常重要，例如在不同的汽车中、不同年龄和性别的儿童中。因此，本节从以下几个方面评估 WiCPD 的检测率：1) 不同车型；2) 不同儿童；3) 儿童不同姿势；4) 在不同中心频率和频段上工作的不同商用天线。

**对不同车型的无关性 (Independence on Different Cars):** 图 12 显示了 WiCPD 在四种不同车型中的检测率，包括：丰田汉兰达 (Toyota Highlander)（车 #1）、丰田凯美瑞 SE 轿车 (Toyota Camry SE Sedan)（车 #2）、本田 HR-V 掀背车 (Honda HR-V Hatchback)（车 #3）和本田思域轿车 (Honda Civic Sedan)（车 #4）。可以看出，WiCPD 在不同车型中的检测率均大于 96.3%，差异小于 2.47%。性能的轻微差异主要是由于不同汽车的材料、结构和尺寸不同。不同的尺寸会导致无线信号传播路径长度/损耗不同，从而引起从动态散射体反射的接收能量 E_d²(f) [见公式 (6)] 的差异。另一方面，不同材料/结构的汽车对反射车内信号和屏蔽车外信号也有不同的影响。

![图 12: 不同车型的验证（有关车型的详细信息，请参见第 IV-F 节）。](placeholder)

**对不同儿童的无关性 (Independence on Different Children):** 由于被单独留在车内的儿童年龄可以从新生儿到六岁 [2]，我们现在评估 WiCPD 在检测不同年龄和性别儿童方面的性能。如表 II 所示，我们征得家长同意，招募了五名年龄从四个月到四岁零十个月不等的儿童。图 13 表明，WiCPD 能够成功检测不同的儿童，检测率至少为 97.25%。不同儿童之间的检测率略有偏差，最大为 1.01%，这显示了 WiCPD 无论儿童年龄和性别如何，都能检测出儿童存在的强大能力。

![图 13: 不同儿童的验证（从小到大，表 II）。](placeholder)

**对不同姿势的无关性 (Independence on Different Postures):** 我们研究了姿势的影响，因为儿童在实际中可能表现出不同的姿势。与现有使用成人模拟儿童以便于控制姿势的工作不同，我们改用婴儿玩偶。原因是成人的体型比婴儿大得多，胸部运动也比婴儿强。因此，从婴儿和成人反射的接收能量 E_d²(f) [见公式 (6)] 完全不同。在图 14 中，“面朝上”、“面朝下”和“坐在地板上”的姿势对应婴儿玩偶收集的数据，而“汽车座椅”来自根据法律规定坐在汽车座椅上的真实婴儿。如图 14 所示，WiCPD 对所有姿势的检测率均大于 96.97%，从而促进了 WiCPD 在实际应用中的部署。

![图 14: 儿童不同姿势的验证（在汽车座椅内或外）。](placeholder)

**对不同天线的无关性 (Independence on Different Antennas):** 图 15 评估了 WiCPD 在 2.4 GHz 和 5 GHz 频段工作的不同商用天线上的检测性能，以检验其与配备不同天线的不同汽车集成的潜力。如图所示，通过使用中心频率为 5 GHz 的天线 #1 和 #2，WiCPD 的检测率分别达到 98.13% 和 98.78%。总体而言，WiCPD 在 2.4G 和 5G 系统的两种不同天线上显示出可比较的性能（即检测率差异在 0.5% 以内）。因此，图 15 表明，如果集成得当，WiCPD 有望部署在配备不同天线的各种汽车上。此外，随着未来天线和带宽的增加（尽管目前尚不可用），WiCPD 有望通过获取更多有效带宽来提高性能。

![图 15: 不同物理天线和载波频率的验证。](placeholder)

### G. 系统开销 (System Overhead)

针对实际应用，从不同方面评估系统开销是有意义的。为此，我们分析了 WiCPD 的理论计算复杂度和实时实验中的资源消耗。为了测量实时资源消耗，我们在四辆不同的汽车中进行了长期实验，每辆车进行两次实验，每次试验持续 30 分钟。使用婴儿玩偶模拟被困儿童，并使用玩具车（遥控）偶尔引起运动。这里我们选择婴儿玩偶主要有两个原因：1) 将儿童长时间留在封闭车内会对其健康造成严重损害，因此是被禁止的；2) 我们的目标是评估系统延迟和消耗，这与测试对象无关。在测试期间，我们通过保存 Linux 系统中 top¹⁴ 命令提供的日志来跟踪资源消耗。值得注意的是，由于 WiCPD 具有无需校准的优雅特性，当部署在新车中时，可以在几分钟内完成设置。

**计算复杂度 (Computational Complexity):** 回顾 N_s 表示 TX 和 RX 之间的空间链路数，N_f 表示子载波数，表 IV 总结了 WiCPD 中三个检测器的计算复杂度。T_M 和 T_B 分别表示估计运动统计量和呼吸频率的时间窗口长度。T_P 表示覆盖 ACF 第一个局部峰值以估计呼吸频率的时间滞后长度，T_T 是计算协方差矩阵以估计特征值的时间窗口长度。为了更清晰，表 IV 包含了一个计算复杂度的典型示例，其中 T_M = T_T = 2 s，T_B = 12 s，T_P = 8 s，N_s = 4，N_f = 58，f_s = 30，K = 10。注意，计算复杂度是按复数乘法操作 (complex multiplication operations, CMs) 的次数计算的。从表 IV 可以看出，静止目标检测器和过渡目标检测器比运动目标检测器消耗更多的计算资源。然而，WiCPD 以级联方式检测运动目标、静止目标和过渡目标，从而在实际中最小化资源消耗。

表 IV 计算复杂度

| 方法 | 复杂度 | 示例 (CMs) |
|------|--------|-------------|
| **WiCPD** | | |
| 运动目标检测器 | O{(2N_s N_f - 1)(T_M f_s - 1)} | 2.561×10⁴ |
| 静止目标检测器 | O{(2T_B - T_P + 4N_s N_f)(T_B f_s)/2} | 3.398×10⁶ |
| 过渡目标检测器 | O{(T_T N_s N_f + (N_s N_f)³ + 2(2+K))} | 1.250×10⁷ |

**资源消耗 (Resource Consumption):** 图 16 显示了 10 分钟实时结果的快照，图 17 显示了 WiCPD 在运行于最高 1 GHz 的双核 ARM Cortex-A7 CPU Linux 板载系统上的 CPU 和内存资源消耗。如图所示，WiCPD 对运动和静止情况之间的交替响应及时，并实现了鲁棒的 CPD 检测结果，仅消耗约 11% 的 CPU 和 40 MB RAM。注意，在实际中，CPU 和 RAM 的消耗可以进一步降低，因为只传输必要的二进制 CPD 检测决策，而不涉及如图 6 所示的 GUI，从而节省更多资源。

![图 16: 10 分钟实时测试快照。](placeholder)

![图 17: WiCPD 在 30 分钟实时实验中的资源消耗。](placeholder)

### H. 对比研究 (Comparative Study)

尽管我们在第 IV-B 节中已将 WiCPD 与基准算法进行了比较，但我们希望与更多现有的 CPD 系统 [4]–[16], [20]–[25], [63] 进行比较，如表 I 所总结。然而，其中很少有系统在如此广泛的条件下进行过测试，因为大多数都是专利 [4]–[7], [11], [63]、在线演示 [21]–[25]，侧重于方法论和概念验证。因此，我们简要地将现有工作分类为 SensorCPD [4]–[10]、PIR Motion-CPD [11]–[13], [63]、VisionCPD [14]–[16]、Radar-CPD [20]–[25]，并从技术角度进行比较。

*   **Sensor-CPD:** Sensor-CPD 方法通常在婴儿座椅上配备物理传感器，例如用于检测重量的重量传感器 [4], [5]、用于检测电气连续性的 RFID 传感器 [7]，或用于检测照护者与婴儿座椅之间距离的传感器 [8]。最近的工作还使用电容传感器检测儿童身体与附近电容传感器之间的电容 [9], [10]，以及多种传感器的融合 [6]。虽然它们大多数在检测婴儿座椅上的儿童时效果良好，但它们的误报率较高，因为重量/压力传感器只要重量相同就无法区分儿童和无生命物品。此外，它们的感知区域仅限于婴儿座椅附近，无法处理儿童跌入脚部空间的情况。然而，WiCPD 由于其统计 CSI 模型，可以覆盖整个车内空间。
*   **PIR Motion-CPD:** PIR Motion-CPD 使用红外传感器感知儿童引起的运动 [11]–[13]，从而拓宽了传统 Sensor-CPD 的感知覆盖范围。尽管它很普遍，但它易受周围温度影响，因为它实际上是在 LoS 区域内检测温暖人体的温度。从这个意义上说，WiCPD 优于 PIR Motion-CPD，因为它与环境无关，并且覆盖了 LoS 和 NLoS 区域。
*   **Vision-CPD:** Vision-CPD 方法首先使用专用摄像头捕捉车内图像 [14]–[16]，然后利用图像处理执行 CPD。它们非常准确，尤其是结合了流行的机器学习驱动的图像处理技术 [17] 时。然而，它们需要专用摄像头和良好的光照条件才能获得高质量图像，从而阻碍了其实用应用。WiCPD 通过利用车载 Wi-Fi 系统，无论光照条件如何，极大地简化了任务。
*   **Radar-CPD:** 基于雷达的方法检测微动，例如生命运动（即呼吸和心率运动），作为 CPD 的指示器。最近的工作也展示了使用工作在 24、60、69 和 79 GHz 的毫米波雷达 [21]–[25] 的潜在 CPD 候选方案。它们通过利用毫米波信号由于中心频率高而带来的高方向性、角度和距离分辨率，当目标位于雷达 FoV 内时，显示出惊人的高精度。然而，事物都有两面性。高频率也导致信号传播衰减快，高方向性使得毫米波系统对儿童的位置/姿势更加敏感 [20]。虽然毫米波非常令人鼓舞，但它尚未与当前车载 Wi-Fi 系统（大多数工作在 2.4 和 5 GHz）集成，而无需额外的硬件成本。相反，WiCPD 在与现有车载 Wi-Fi 系统结合方面显示出更好的潜力，几乎为零的额外硬件成本。

## V. 讨论与未来工作 (DISCUSSIONS AND FUTURE WORKS)

### A. 多儿童存在检测 (Multiple Children Presence Detection)

目前，WiCPD 主要展示了检测单个儿童存在的性能。然而，它很容易扩展到多儿童场景，原因如下。首先，通过利用运动统计量，WiCPD 可以建设性地检测所有动态散射体的运动。从这个意义上说，WiCPD 更擅长捕捉多个儿童的运动，从而报告 CPD 检测，因为多个儿童比单个儿童更有可能引起运动。当车内有多个处于静止状态的儿童时，CSI 的 ACF 将由具有最强呼吸运动/位移的儿童的呼吸信号主导。因此，WiCPD 的静止目标检测器仍然可以估计并检查估计呼吸频率的连续性/持续时间，以确定 CPD。类似地，被困在车内的多个处于过渡状态的儿童将改变车内的多径分布，这也可以被过渡检测器检测到。

### B. 未来工作 (Future Work)

虽然 WiCPD 专注于 CPD，但考虑所有多径分量的统计建模为基于 Wi-Fi 感知的车内应用带来了新的机遇。首先，一个直接的扩展是基于估计的呼吸频率进行车内生命体征监测。驾驶员的生命体征可以进一步用于分析驾驶员的行为，例如疲劳和注意力不集中驾驶，我们相信这可以促进流行的 ADAS 研究。然而，监测驾驶员行为更具挑战性，因为驾驶过程中存在振动，而 WiCPD 针对的是停放的车辆。不过，最近的工作 [64], [65] 已经显示了通过去趋势等技术检测/补偿/抑制非期望运动的可能性。其次，如果我们能够在驾驶过程中估计驾驶员和乘客的呼吸频率，它可以作为先验信息，进一步缩短 WiCPD 的响应时间。第三，占用检测（即车内乘客计数）可能通过 Wi-Fi 感知实现，因为接收到的 CSI 可以视为来自不同个体的呼吸信号的叠加（如果有的话）。因此，最近提出的分解技术，如变分模态分解 (variational mode decomposition, VMD) [66] 和多变量 VMD (multivariate VMD, MVMD) [67]，可以应用于估计信号模式，从而指示车内的人数。

## VI. 结论 (CONCLUSION)

本文介绍了 WiCPD 的设计、实现和评估，这是一种新颖的 CPD，使用从商用 Wi-Fi 芯片组提取的 CSI 测量值。我们利用统计电磁理论建模来解释目标对所有多径分量的影响。然后，设计了三个检测器模块，使 WiCPD 无论儿童处于何种状态（运动、静止或介于两者之间的过渡状态）都能进行检测。我们使用商用 Wi-Fi 芯片组实现了该系统，并将其部署在不同的汽车中，以检测不同年龄和性别的儿童。大量结果表明，WiCPD 可以在 20 秒内实现 ≥96.5% 的准确率。实时测试表明，WiCPD 以可承受的计算开销运行，使其成为全球 CPD 应用的有希望的候选方案。

## 参考文献 (REFERENCES)

[1] M. Catherine, N. Jan, and Q. James, "Heat stress from enclosed vehicles: Moderate ambient temperatures cause significant temperature rise in enclosed vehicles," *Pediatrics*, vol. 116, no. 1, pp. e109-e112, 2005.
[2] "Heatstroke deaths of children in vehicles." [Online]. Available: https://www.noheatstroke.org/original/ (Accessed: Mar. 6, 2021).
[3] "Euro NCAP 2025 roadmap." [Online]. Available: https://cdn.euroncap.com/media/30700/euroncap-roadmap-2025-v4.pdf (Accessed: Mar. 6, 2021).
[4] A. R. Marc, "Warning system for detecting presence of a child in an infant seat," U.S. Patent 5 949 340, Sep. 1999.
[5] J. C. Charles, "System to detect the presence of an unattended child in a vehicle," U.S. Patent 7 170 401, Jan. 2007.
[6] L. Davis, "Child carseat alert system," U.S. Patent 7 250 869, Jul. 2007.
[7] J. K. Lee, D. R. Spach, P. Srinivasan, and D. P. Umstead, "Wireless system to detect presence of child in a baby car seat," U.S. Patent 7 321 306, Jan. 2008.
[8] K. N. Khamil, S. Rahman, and M. Gambilok, "Babycare alert system for prevention of child left in a parked vehicle," *ARPN J. Eng. Appl. Sci.*, vol. 10, no. 22, pp. 17313-17319, 2015.
[9] J. Albesa and M. Gasulla, "Occupancy and belt detection in removable vehicle seats via inductive power transmission," *IEEE Trans. Veh. Technol.*, vol. 64, no. 8, pp. 3392-3401, Aug. 2015.
[10] A. Ranjan and B. George, "A child-left-behind warning system based on capacitive sensing principle," in *Proc. IEEE Int. Instrum. Meas. Technol. Conf. (I2MTC)*, 2013, pp. 702-706.
[11] F. R. M. Rashidi and I. H. Muhamad, "Vehicle's interior movement detection and notification system," *Recent Adv. Autom. Control Model. Simulat.*, vol. 2013, pp. 139-144, Apr. 2013. [Online]. Available: https://www.safetylit.org/citations/index.php?fuseaction=citations.viewdetails&citationIds[]=citjournalarticle_493561_26
[12] P. Zappi, E. Farella, and L. Benini, "Tracking motion direction and distance with pyroelectric IR sensors," *IEEE Sensors J.*, vol. 10, no. 9, pp. 1486-1494, Sep. 2010.
[13] N. Hashim, H. Basri, A. Jaafar, M. Aziz, and A. S. A. Ja, "Child in car alarm system using various sensors," *ARPN J. Eng. Appl. Sci.*, vol. 9, no. 9, pp. 1653-1658, 2014.
[14] M. I. Hafiz and F. R. M. Rashidi, "In-car suffocating prevention using image motion detection," in *Proc. SCOPORO*, 2013, p. 12.
[15] H. Cai, D. Lee, H. Joonkoo, Y. Fang, S. Li, and H. Liu, "Embedded vision based automotive interior intrusion detection system," in *Proc. IEEE Int. Conf. Syst. Man Cybern. (SMC)*, 2017, pp. 2909-2914.
[16] C. Fan, Y. Wang, and C. Huang, "Heterogeneous information fusion and visualization for a large-scale intelligent video surveillance system," *IEEE Trans. Syst., Man, Cybern., Syst.*, vol. 47, no. 4, pp. 593-604, Apr. 2017.
[17] J. Redmon and A. Farhadi, "Yolov3: An incremental improvement," 2018, arXiv:1804.02767.
[18] F. Wang, X. Zeng, C. Wu, B. Wang, and K. J. R. Liu, "mmHRV: Contactless heart rate variability monitoring using millimeter-wave radio," *IEEE Internet Things J.*, vol. 8, no. 22, pp. 16623-16636, Nov. 2021.
[19] F. Wang, X. Zeng, C. Wu, B. Wang, and K. J. R. Liu, "Driver vital signs monitoring using millimeter wave radio," *IEEE Internet Things J.*, vol. 9, no. 13, pp. 11283-11298, Jul. 2022.
[20] A. Caddemi and E. Cardillo, "Automotive anti-abandon systems: A millimeter-wave radar sensor for the detection of child presence," in *Proc. 14th Int. Conf. Adv. Technol. Syst. Services Telecommun. (TELSIKS)*, 2019, pp. 94-97.
[21] Vayyar Inc. "Testing with all ages of infants in various child seats." [Online]. Available: https://vayyar.com/auto/ (Accessed: Mar. 6, 2021).
[22] Novelic Inc. "NOVELIC radar automotive in-cabin monitoring." [Online]. Available: https://www.novelic.com/radar-solutions/ (Accessed: Mar. 6, 2021).
[23] Infineon Inc. "Presence detection." [Online]. Available: https://www.infineon.com/cms/en/applications/solutions/sensor-solutions/presence-detection/ (Accessed: Mar. 6, 2021).
[24] Innosent Inc. "Child presence detection with RADAR—Anonymous vehicle interior monitoring." [Online]. Available: https://www.innosent.de/en/automotive/incabin-radar-monitoring/ (Accessed: Mar. 6, 2021).
[25] IEE-Sensing, Inc. "Sensing solutions for child presence detection." [Online]. Available: https://www.iee-sensing.com/en/automotive/safety-and-comfort/vitasese (Accessed: Mar. 6, 2021).
[26] B. Wang, Q. Xu, C. Chen, F. Zhang, and K. J. R. Liu, "The promise of radio Analytics: A future paradigm of wireless positioning, tracking, and sensing," *IEEE Signal Process. Mag.*, vol. 35, no. 3, pp. 59-80, May 2018.
[27] K. R. Liu and B. Wang, *Wireless AI: Wireless Sensing, Positioning, IoT, and Communications*. Cambridge, U.K.: Cambridge Univ. Press, 2019.
[28] B. Schleicher, I. Nasr, A. Trasser, and H. Schumacher, "IR-UWB radar demonstrator for ultra-fine movement detection and vital-sign monitoring," *IEEE Trans. Microw. Theory Techn.*, vol. 61, no. 5, pp. 2076-2085, May 2013.
[29] "Hot and cold: Extreme temperature safety." [Online]. Available: https://www.healthline.com/health/extreme-temperature-safety#extreme-heat-temperatures (Accessed: Aug. 12, 2021).
[30] "Connected cars worldwide—Statistics & facts." [Online]. Available: https://www.statista.com/topics/1918/connected-cars/#topicHeader_wrapper (Accessed: Jun. 19, 2022).
[31] "Cheap cars with WiFi capability." [Online]. Available: https://www.autobytel.com/car-buying-guides/features/cheap-cars-with-wifi-capability-130477/ (Accessed: Mar. 6, 2021).
[32] Car and Driver Research. "Cars with Wi-Fi: Everything you need to know." [Online]. Available: https://www.caranddriver.com/research/a32814112/cars-with-wifi/ (Accessed: Mar. 6, 2021).
[33] "How many connected cars are sold worldwide?" [Online]. Available: https://smartcar.com/blog/connected-cars-worldwide/ (Accessed: Jun. 19, 2022).
[34] "Projected share of new Internet-connected light-duty vehicles sales worldwide and in the United States in 2023." [Online]. Available: https://www.statista.com/statistics/275849/number-of-vehicles-connected-to-the-internet/ (Accessed: Jun. 19, 2022).
[35] X. Xie, K. G. Shin, H. Yousefi, and S. He, "Wireless CSI-based head tracking in the driver seat," in *Proc. 14th Int. Conf. Emerg. Netw. Exp. Technol.*, 2018, pp. 112-125.
[36] M. Raja and S. Sigg, "RFexpress! Exploiting the wireless network edge for RF-based emotion sensing," in *Proc. 22nd IEEE Int. Conf. Emerg. Technol. Factory Autom. (ETFA)*, 2017, pp. 1-8.
[37] X. Zeng, F. Wang, B. Wang, C. Wu, K. J. R. Liu, and O. C. Au, "In-vehicle sensing for smart cars," *IEEE Open J. Veh. Technol.*, vol. 3, pp. 221-242, 2022.
[38] X. Zeng, B. Wang, W. Chenshu, S. D. Regani, and K. J. R. Liu, "Intelligent Wi-Fi based child presence detection system," in *Proc. ICASSP IEEE Int. Conf. Acoust. Speech Signal Process. (ICASSP)*, 2022, pp. 11-15.
[39] Q. Xu, B. Wang, F. Zhang, D. S. Regani, F. Wang, and K. J. R. Liu, "Wireless AI in smart car: How smart a car can be?" *IEEE Access*, vol. 8, pp. 55091-55112, 2020.
[40] Y. Gu, J. Zhan, Y. Ji, J. Li, F. Ren, and S. Gao, "MoSense: An RF-based motion detection system via off-the-shelf WiFi devices," *IEEE Internet Things J.*, vol. 4, no. 6, pp. 2326-2341, Dec. 2017.
[41] C. Wu, Z. Yang, Z. Zhou, X. Liu, Y. Liu, and J. Cao, "Non-invasive detection of moving and stationary human with WiFi," *IEEE J. Sel. Areas Commun.*, vol. 33, no. 11, pp. 2329-2342, Nov. 2015.
[42] F. Zhang, C. Wu, B. Wang, H. Q. Lai, Y. Han, and K. J. R. Liu, "WiDetect: Robust motion detection with a statistical electromagnetic model," *Proc. ACM Interact. Mobile Wearable Ubiquitous Technol.*, vol. 3, no. 3, pp. 1-24, 2019.
[43] E. A. Lee and D. G. Messerschmitt, *Digital Communication*. New York, NY, USA: Springer, 2012.
[44] F. Zhang et al., "SMARS: Sleep monitoring via ambient radio signals," *IEEE Trans. Mobile Comput.*, vol. 20, no. 1, pp. 217-231, Jan. 2021.
[45] J. Liu, Y. Wang, Y. Chen, J. Yang, X. Chen, and J. Cheng, "Tracking vital signs during sleep leveraging off-the-shelf WiFi," in *Proc. 16th ACM Int. Symp. Mobile Ad Hoc Netw. Comput.*, 2015, pp. 267-276.
[46] "NXP S32 automotive platform." [Online]. Available: https://www.nxp.com/products/processors-and-microcontrollers/s32-automotive-platform:S32 (Accessed: Jun. 19, 2022).
[47] "Atheros CSI tool." [Online]. Available: https://wands.sg/research/wifi/AtherosCSI/ (Accessed: Jun. 19, 2022).
[48] "Linux 802.11n CSI tool." [Online]. Available: https://dhalperi.github.io/linux-80211n-csi/ (Accessed: Jun. 19, 2022).
[49] F. Restuccia, "IEEE 802.11 BF: Toward ubiquitous Wi-Fi sensing," 2021. [Online]. Available: https://dhalperi.github.io/linux-80211n-csi/ (Accessed: Jun. 19, 2022).
[50] T. Chiueh, P. T. I. Lai, and T. Chiueh, *Baseband Receiver Design for Wireless MIMO-OFDM Communications*. Hoboken, NJ, USA: Wiley, 2012.
[51] F. Zhang, C. Chen, B. Wang, and K. J. R. Liu, "WiSpeed: A statistical electromagnetic approach for device-free indoor speed estimation," *IEEE Internet Things J.*, vol. 5, no. 3, pp. 2163-2177, Jun. 2018.
[52] D. A. Hill, *Electromagnetic Fields in Cavities: Deterministic and Statistical Theories*, vol. 35. Hoboken, NJ, USA: Wiley, 2009.
[53] D. A. Hill, "Plane wave integral representation for fields in reverberation chambers," *IEEE Trans. Electromagn. Compat.*, vol. 40, no. 3, pp. 209-217, Aug. 1998.
[54] R. K. Pearson, Y. Neuvo, J. Astola, and M. Gabbouj, "Generalized hampel filters," *EURASIP J. Adv. Signal Process.*, vol. 2016, no. 1, pp. 1-18, 2016. [Online]. Available: https://doi.org/10.1186/s13634-016-0383-6
[55] Y. Xie, Z. Li, and M. Li, "Precise power delay profiling with commodity Wi-Fi," *IEEE Trans. Mobile Comput.*, vol. 18, no. 6, pp. 1342-1355, Jan. 2019.
[56] L. Kahn, "Correspondence," *Proc. IRE*, vol. 42, no. 11, pp. 1698-1704, 1954.
[57] G. Shafiq and K. C. Veluvolu, "Surface chest motion decomposition for cardiovascular monitoring," *Sci. Rep.*, vol. 4, no. 1, pp. 1-9, 2014.
[58] L. A. Wallis, M. Healy, M. Undy, and I. Maconochie, "Age related reference ranges for respiration rate and heart rate from 4 to 16 years," *Arch. Disease Childhood*, vol. 90, no. 11, pp. 1117-1121, 2005.
[59] S. Fang, R. Alterovitz, and S. Nirjon, "Non-line-of-sight around the corner human presence detection using commodity WiFi devices," in *Proc. 1st ACM Int. Workshop Device Free Human Sens.*, 2019, pp. 22-26.
[60] S. Di Domenico, M. De Sanctis, E. Cianca, and M. Ruggieri, "WiFi-based through-the-wall presence detection of stationary and moving humans analyzing the doppler spectrum," *IEEE Aerosp. Electron. Syst. Mag.*, vol. 33, nos. 5-6, pp. 14-19, May/Jun. 2018.
[61] X. Liu, J. Cao, S. Tang, and J. Wen, "Wi-sleep: Contactless sleep monitoring via WiFi signals," in *Proc. IEEE Real-Time Syst. Symp.*, 2014, pp. 346-355.
[62] H. Wang et al., "Human respiration detection with commodity wifi devices: Do user location and body orientation matter?" in *Proc. ACM Int. Joint Conf. Pervasive Ubiquitous Comput.*, 2016, pp. 25-36.
[63] C. P. B. N. V. Naik, "Infant alert system for prevention of accidental death of infants in vehicles," Int. J. Eng. Res. Technol., vol. 4, no. 1, pp. 1-4, 2015.
[64] Y. Chen, W. Dong, Y. Gao, X. Liu, and T. Gu, "Rapid: A multimodal and device-free approach using noise estimation for robust person identification," *Proc. ACM Interact. Mobile Wearable Ubiquitous Technol.*, vol. 1, no. 3, p. 41, 2017.
[65] K. Qian, C. Wu, F. Xiao, Y. Zheng, Y. Zhang, and Z. Yang, "Acousticcardiogram: Monitoring heartbeats using acoustic signals on smart devices," in *Proc. IEEE INFOCOM*, 2018, pp. 1574-1582.
[66] K. Dragomiretskiy and D. Zosso, "Variational mode decomposition," *IEEE Trans. Signal Process.*, vol. 62, no. 3, pp. 531-544, Feb. 2014.
[67] N. ur Rehman and H. Aftab, "Multivariate variational mode decomposition," *IEEE Trans. Signal Process.*, vol. 67, no. 23, pp. 6039-6052, Dec. 2019.

**作者简介：**

**Xiaolu Zeng** (Member, IEEE) received the B.S. degree from Harbin Institute of Technology, Harbin, China, in 2014, and the Ph.D. degree from the School of Electronic Engineering, Xidian University, Xi'an, China, in 2020. He is currently an Assistant Professor with the School of Information and Electronics, Beijing Institute of Technology, Beijing, China. His research interests include intelligent wireless sensing, Internet of Things, and advanced driver assistance systems.

**Beibei Wang** (Fellow, IEEE) received the B.S. (Hons.) degree in electrical engineering from the University of Science and Technology of China, Hefei, China, in 2004, and the Ph.D. degree in electrical engineering from The University of Maryland, College Park, MD, USA, in 2009. She is currently the Vice President of Research with Origin Wireless Inc., Rockville, MD, USA. Her current research interests include Internet of Things, mobile computing, wireless sensing and positioning, and communications and networking.

**Chenshu Wu** (Senior Member, IEEE) received the B.E. and Ph.D. degrees from Tsinghua University, Beijing, China, in 2010 and 2015, respectively. He is currently an Assistant Professor with the Department of Computer Science, The University of Hong Kong, Hong Kong. His research interests include wireless AIoT systems at the intersection of wireless sensing, ubiquitous computing, digital health, and Internet of Things.

**Sai Deepika Regani** received the B.E. degree in electronics and communication engineering from the Birla Institute of Technology and Science, Pilani, India, in 2015, and the M.S. and Ph.D. degrees in electrical engineering from The University of Maryland, College Park, MD, USA, in 2021 and 2023, respectively. She is currently a Senior Scientist with Origin Research, Rockville, MD, USA. Her research interests include statistical signal processing, wireless sensing, and machine learning.

**K. J. Ray Liu** (Fellow, IEEE) is the Founder and the President of Origin AI that pioneers AI for wireless sensing and indoor tracking. He was a Distinguished University Professor, a Distinguished Scholar-Teacher, and a Christine Kim Eminent Professor of Information Technology with the University of Maryland at College Park, College Park, MD, USA, from where he retired after over three decades of career in education. His research contributions encompass broad aspects of signal processing and communications. He has trained over 70 doctoral/postdoctoral students, of which ten are now IEEE Fellows. Dr. Liu is the recipient of two IEEE Technical Field Awards: the 2021 IEEE Fourier for Signal Processing and the 2016 IEEE Leon K. Kirchmayer Graduate Teaching Award. He also received the IEEE Signal Processing Society 2014 Norbert Wiener Society Award and 2009 Claude Shannon-Harry Nyquist Technical Achievement Award. Recognized as a Web of Science Highly Cited Researcher. The invention of wireless AI won three prestigious CES Innovation Awards, including the CES Best of Innovation in 2021. He is a Fellow of the American Association for the Advancement of Science (AAAS) and the U.S. National Academy of Inventors. He is the 2022 IEEE President and the CEO. He was the 2019 IEEE Vice President for Technical Activities. He has also served as the 2012-2013 President of IEEE Signal Processing Society, where he once served as the Editor-in-Chief of IEEE Signal Processing Magazine.