# 理解基于WiFi的人体呼吸感知的菲涅尔模型

郝旺$^{1,2,3}$，张大庆$^{1,2,3}$，马俊毅$^{2,3}$，王亚莎$^{2,3}$，王亚星$^{2,3}$，吴丹$^{2,3}$，顾涛$^{4}$，谢冰$^{2,3}$

$^{1}$高可信软件技术教育部重点实验室，中国北京
$^{2}$北京大学信息科学技术学院，中国北京
$^{3}$北京大学软件工程国家工程研究中心，中国北京
$^{4}$皇家墨尔本理工大学理学院，澳大利亚
{haowangsei, dqzsei, majunyi, wangyasha, wyxpk, wudan, xiebing}@pku.edu.cn; tao.gu@rmit.edu.au

## 摘要

近年来的研究已经证明了利用商用WiFi设备非侵入式地检测人体呼吸率的可行性。然而，是否无论受试者身处何处、面向何方，总能感知到人体呼吸？什么因素影响人体呼吸感知，其背后的理论是什么？在本文中，我们首先介绍了自由空间中的菲涅尔模型，然后验证了室内环境下WiFi无线电传播的菲涅尔模型。利用菲涅尔模型和由此导出的WiFi无线电传播特性，我们研究了人体呼吸对接收射频信号的影响，并发展了一套理论，将一个人的呼吸深度、位置和朝向与呼吸的可检测性联系起来。借助这一理论，不仅明确了何时以及为何可以使用WiFi设备检测到人体呼吸，而且有助于理解基于WiFi的传感系统的物理极限和基础。大量的评估验证了所发展的理论，案例研究展示了如何应用该理论指导呼吸监测系统的设计。

## ACM 分类关键词

H.5.m. 信息接口与表示（例如HCI）：其他

## 作者关键词

菲涅尔区；信道状态信息；WiFi。

## 引言

随着普适传感技术的快速发展和全球人口老龄化，能够监测并响应老年人日常活动的智能环境受到了广泛关注[2][14]。早期工作主要关注环境监测和日常活动识别，而过去几年中，人们对健康威胁监测（如跌倒检测[12][43][38]和生命体征监测[13][3]）的兴趣激增，以确保对老年人的及时干预和安全。呼吸率是一项重要的生命体征，可以指示疾病的进展和健康状况的下降。异常呼吸率，无论是过高（呼吸急促）、过低（呼吸缓慢）还是呼吸暂停，都是生理窘迫的敏感指标，需要立即临床干预。鉴于近$5\%$的总人口患有睡眠呼吸暂停综合征等呼吸系统疾病，并且在发达国家，约$30\%$的七十多岁老人据报道患有呼吸系统疾病[25]，经济有效的连续呼吸监测至关重要。

临床环境中连续呼吸率监测的两种常用方法是阻抗呼吸图和二氧化碳图。然而，它们昂贵且侵入性强，阻碍了这些系统在普通家庭的老年人中大规模部署。为了最小化侵入性呼吸测量方法带来的不适，一些研究尝试使用可穿戴[28]和压力传感器[27]进行长期呼吸监测。虽然这两种基于接触的传感方法对老年人来说更容易接受，但基于压力传感器的系统在受试者离开床时无法测量呼吸率，而可穿戴呼吸测量设备对于相当多的老年人存在接受度或可用性问题[10]。

与基于接触的呼吸传感方法相比，非接触式传感方法更具吸引力，因为它既不用电缆或床限制受试者，也不会因接触电极或绑带而引起不适或皮肤刺激[34]。因此，许多研究致力于非接触式呼吸测量。例如，Penne等人使用飞行时间相机并应用高级图像处理算法来估计家庭内的人体呼吸率[32]，不幸的是，这种基于摄像头的方法除了存在隐私问题和受光照条件影响外，还要求受试者近距离面对摄像头。Kondo等人部署了激光传感器来测量呼吸过程中的胸壁运动[18]，而Min等人应用超声波传感器实现了相同目标[25]。研究最广泛的方法是射频方法，范围从多普勒雷达[26]、UWB雷达[36]、FMCW雷达[3]到基于USRP的解决方案[33]。所有这些系统的基本思想是测量呼吸过程中人体胸部的位移。虽然这些解决方案相当精确，但其成本通常过高，使得这些解决方案在家庭环境中不切实际。

为了开发经济有效的家庭监测系统，近年来研究人员开始将注意力转向已安装在家庭中的商用WiFi设备，用于非接触式生命体征测量。在这些努力中，Abdelnasser等人使用WiFi接收信号强度信号提取呼吸率[1]；而Liu等人和Wu等人利用精细的信道状态信息[11]通过商用WiFi设备检测呼吸率[22][21][42][23]。尽管这些工作仍处于探索阶段，但它们为将应用扩展到消费者家庭医疗保健产品提供了一种潜在的低成本手段。虽然基于RSS的方法仅在受试者靠近视距时有效，但基于CSI的方法看起来更有吸引力，因为它们可以远距离捕获受试者的呼吸率，使其适用于长期呼吸率监测。

尽管基于WiFi CSI的呼吸传感解决方案看起来非常有前景，但我们注意到所有先前的工作，包括所有2.4GHz射频呼吸传感研究，都基于纯粹的经验实验，未能说明何时人体呼吸引起的微小运动是可检测的。此外，缺乏具体的理论来指导呼吸传感系统的设计。

在本文中，我们旨在回答以下问题：（1）在房间内使用一对WiFi设备，是否在所有地方都能检测到人体呼吸率？如果人们待在 不同地方且面向不同方向，呼吸检测是否存在差异？（2）如果固定两个WiFi设备，哪些位置和朝向是呼吸检测的最佳、良好和较差的位置？如果固定一个人或两个人的位置和朝向，我们应该将WiFi设备放在哪里才能最优地测量呼吸率？（3）指导系统设计背后的理论是什么？简而言之，我们不仅旨在为基于WiFi的呼吸传感系统设计提供实践指南，而且旨在发展一套理论来揭示基于WiFi射频活动传感的一般原理。

为了回答上述问题，我们首先介绍菲涅尔区概念和自由空间中的无线电传播特性，然后验证当一对收发器分开时，室内环境中WiFi无线电传播的菲涅尔模型。通过使用菲涅尔模型和室内环境中的无线电传播特性分析静态和运动物体如何影响接收射频信号，我们推导了描述给定射频频率下接收无线电信号幅度和相位的数学公式。通过进一步将人体建模为一个尺寸变化的半圆柱体，我们发展了一套理论，将一个人的呼吸深度、位置和朝向与呼吸率的可检测性联系起来。基于所发展的理论，我们不仅提供了感知地图，指示呼吸检测的最佳、良好和较差的位置/朝向（固定两个WiFi设备的位置），而且可以在两个人躺在床上期望被同时监测时，提供关于WiFi发射器/接收器放置的实用指南。

本文的主要贡献可总结如下：

1) 据我们所知，这是第一项将菲涅尔区模型引入室内环境，用于使用商用WiFi设备检测呼吸率的工作。这项工作为探索细微运动检测以及可能基于射频系统的传感极限提供了普遍的理论基础。

2) 通过分析运动物体如何影响接收射频信号，并将人体建模为一个尺寸变化的半圆柱体，我们发展了一套理论，通过在菲涅尔区的背景下检查接收信号强度，将一个人的呼吸深度、位置和朝向与呼吸的可检测性联系起来。

3) 我们通过大量实验评估了所发展的理论。特别是，我们利用该理论指导了两种典型场景下的呼吸率监测系统设计。

## 相关工作

使用无线技术进行非接触式生命体征传感的研究始于70年代末[19]。在过去几年中，已经探索了各种基于射频的方法，范围从多普勒雷达[7]、UWB雷达[8]、FMCW雷达[3]到基于USRP的解决方案[9]，用于感知人体呼吸率。虽然这些工作表明生命体征可以被准确检测，但缺点是它们依赖复杂且昂贵的硬件，使其在普通家庭中大规模部署不切实际。为了提高老年人安全性，对非侵入式且经济有效的健康监测系统的迫切需求，促使研究人员致力于利用家中现有WiFi设备的呼吸率传感解决方案。与本文密切相关的工作大致可分为三类：

**基于RSS的呼吸检测。** 这一研究方向最早由Patwari和Wilson探索，他们利用Zigbee收发器网络和先进的信号处理技术从RSS数据流中提取呼吸率[29][30]。O.J. Kaltiokallio等人开发了一种更具成本效益的解决方案[17]，其中仅部署一对Zigbee收发器从RSS流中检测呼吸率。基于类似的想法，Abdelnasser等人使用两个商用WiFi设备从RSS中提取呼吸率[1]。然而，发现RSS对于可靠追踪由呼吸引起的微小胸部运动不够敏感，因为呼气和吸气引起的RSS变化非常小，容易被环境噪声淹没[23]。因此，在实际实现中，要求受试者要么停留在WiFi设备的视距之间[1]，要么非常靠近Zigbee收发器，并假设一个人的呼吸是恒定且周期性的[29][29][17]，使得异常呼吸检测（例如呼吸暂停）成为不可能。

**基于CSI的呼吸检测。** 与RSS相比，WiFi设备中的信道状态信息更丰富，对检测呼吸更敏感。WiSleep [22]是第一项基于商用WiFi设备中的CSI检测人体呼吸率以用于睡眠监测的工作。该工作在[23]中得到扩展，考虑了睡姿和异常呼吸模式。Liu等人[21]进一步提出使用现成WiFi设备追踪睡眠期间的呼吸率和心率两种生命体征。Wu等人[42]将呼吸检测从睡眠姿势扩展到站立姿势，用于静止人体检测。然而，在所有系统和鼓舞人心的结果背后，我们注意到当前基于WiFi的呼吸检测解决方案主要基于经验实验，它们未能回答何时人体呼吸率是可检测的。此外，缺乏基础理论来指导系统设计，研究人员不得不依靠试错来使系统工作。

**菲涅尔区模型。** 菲涅尔区的概念源于19世纪初对光的干涉和衍射的研究[16]。首次提到在射频中使用菲涅尔区概念出现在1936年的一项美国专利中[41][4]。从那时起，菲涅尔区模型已在微波传播、无线电台站放置到天线设计等多个领域得到应用[15]。最近的工作[20]将菲涅尔-基尔霍夫刃边衍射模型应用于传感器网络中的定位。基于当受试者出现在一对对称网格中时人体引起的衍射效应的对称性，他们将一个人的位置与MICAz节点传感器网络中的接收信号强度联系起来，在室外环境中实现了米级的定位分辨率。

然而，在我们的工作中，我们进一步考虑了WiFi射频信号的反射和频率分集，以构建菲涅尔区模型，并揭示了由受试者微小运动引起的每个子载波中的信号变化模式。凭借这一新发现，我们能够在射频波长的粒度上捕获接收射频信号中的细微身体位移，将传感分辨率推向前所未有的厘米级。这为室内环境中的高精度人体传感（如呼吸检测）开辟了新的机遇。

## 理解WiFi菲涅尔区

在本节中，我们首先介绍自由空间中菲涅尔区模型的基础知识，然后定性分析静态和运动物体如何影响与菲涅尔区相关的接收射频信号。基于自由空间中的理想菲涅尔区模型，我们设计了实际实验来验证室内环境中WiFi无线电传播的菲涅尔区的存在。最后，我们定量描述了存在运动物体时的接收信号，并推导了在WiFi菲涅尔区背景下的射频传播特性。

## 菲涅尔区模型的基础

在无线电传播的背景下，菲涅尔区是指以一对收发器为焦点的同心椭圆。假设$P_{1}$和$P_{2}$是两个具有一定高度的收发器（如图1所示），对于给定的无线电波长$\lambda$，包含$n$个椭圆的菲涅尔区可以通过确保下式来构造：

$$|P_{1}Q_{n}| + |Q_{n}P_{2}| - |P_{1}P_{2}| = n\lambda /2$$

其中$Q_{n}$是第$n$个椭圆上的点。最内层的椭圆被定义为第1菲涅尔区，第一个椭圆和第二个椭圆之间的椭圆环被定义为第2菲涅尔区，而第$n$菲涅尔区对应的是第$(n - 1)$和第$n$个椭圆之间的椭圆环。由于相邻两个菲涅尔区之间的边界是一个椭圆，我们进一步将第$n$菲涅尔区的边界定义为第$n$和第$(n + 1)$菲涅尔区之间的椭圆：

$$b_{n} = \{Q_{n},P_{1},P_{2}||P_{1}Q_{n}| + |Q_{n}P_{2}| - |P_{1}P_{2}| = n\lambda /2\} \quad (1)$$

显然，随着$n$从1增加到$N$，菲涅尔区的宽度不断减小，趋近于$\lambda /2$。根据先前的研究，对射频传输重要的区域是前$8 - 12$个区，超过$70\%$的能量通过第一菲涅尔区传输[15]。

## 静态/运动物体如何影响接收射频信号

如图1所示，$P_{1},P_{2}$是自由空间中的一对收发器。当$P_{1}$向$P_{2}$发送射频信号时，接收信号的幅度和相移由$P_{1}P_{2}$（视距）的长度决定。

假设一个静态物体出现在第一菲涅尔区的边界上，即图1中的$Q_{1}$点，则引入了一条来自物体的额外信号路径，$P_{2}$处的接收信号是反射信号和经由视距的信号的线性组合。因为源信号相同，而反射信号的路径长度$(P_{1}Q_{1}P_{2})$比视距长度长$\lambda /2$，两个信号之间的相位差为$\pi$，加上反射引入的$\pi$相移[15]，两个信号具有相同的相位但幅度不同，导致叠加后的接收信号更强。

但如果我们将物体放在第二菲涅尔区的边界上，由于反射信号的路径长度比视距信号长$\lambda$，两个信号之间的相位差为$2\pi$。考虑到额外的$\pi$相移，接收信号将具有相消相位，导致两个信号相互抵消。类似地，当物体位于奇数菲涅尔区边界时，反射信号将增强$P_{2}$处的接收信号，因为它与视距信号同相；当物体位于偶数菲涅尔区边界时，反射信号和视距信号将相互抵消，因为它们具有相消相位，因此$P_{2}$将观察到比空间中没有任何物体时更弱的信号。

现在假设一个物体从第1菲涅尔区移动到第$n$菲涅尔区，而通过视距传播的信号保持

---

**图1. 菲涅尔区的几何形状。**

不变，并且从物体反射的信号的路径长度持续变化，那么在$P_{2}$处观察到的信号将是交替增强和减弱。图2（a）显示了一个真实的实验，其中一个物体在收发器对之间沿着垂直平分线连续穿过多个菲涅尔区，得到的接收信号呈现出连续的类似正弦波的波形，波峰和波谷对应于穿过菲涅尔区边界。实际上，根据菲涅尔-基尔霍夫衍射理论[15]，由于衍射，接收信号中的波峰和波谷并不对称于视距信号的强度。但在本文中，我们忽略这种不对称性，将波峰和波谷视为对称的，以简化讨论。

## 运动物体的反射信号如何影响接收信号

为了进一步揭示在菲涅尔区中存在运动物体时接收信号的潜在原理，我们利用图3（a）所示的模型，其中发射信号通过多条路径到达接收器。我们将所有这些路径分为静态路径和动态路径，那么接收信号$H(f,t)$可以表示为一个相位向量，满足以下方程[35][39]：

$$H(f,t) = H_{s}(f) + H_{d}(f,t) = H_{s}(f) + a(f,t)e^{-j2\pi d(t) / \lambda}$$

其中静态向量$H_{s}(f)$是来自静态路径的信号之和，而动态向量$H_{d}(f,t)$是由运动物体反射的信号引入的，如图3（b）所示。反射信号可以进一步用一个向量表示，其中$a(f,t)$是动态路径幅度和初始相位偏移的复数值表示，$e^{- j2\pi d(t) / \lambda}$是动态路径长度$d(t)$上的相移。显然，当反射信号的长度变化$\lambda$时，其相位偏移$2\pi$（旋转一圈）。因此，接收信号$H(f,t)$在复平面上具有时变幅度：

$$|H(f,\theta)|^2 = |H_s(f)|^2 +|H_d(f)|^2 +2|H_s(f)||H_d(f)|cos\theta$$

其中$\theta$是静态向量$|H_{s}(f)|$和动态向量$|H_{d}(f)|$之间的相位差。特别地，当物体移动一小段距离时，例如图2（a）中的几个波长，可以安全地假设动态向量的幅度保持不变，即$|H_{d}(f)|$是常数。这解释了为什么当物体穿过几个菲涅尔区时，接收信号的幅度看起来像正弦波。具体来说，当$\theta = 2\pi ,4\pi ,\ldots$时出现波峰，当$\theta = 3\pi ,5\pi ,\ldots$时出现波谷，对应于菲涅尔区的边界。

## 室内空间中的WiFi射频传播特性

通过以上讨论，我们总结室内空间中的WiFi射频传播特性如下：

1) WiFi菲涅尔区呈现为以一对收发器为焦点的同心椭圆形状，可以数学计算得出。
2) 运动物体通常产生具有变化幅度和相位的反射信号。在小尺度运动下，反射信号大致具有固定幅度，而变化的相位影响接收信号。在大尺度运动下，反射信号在输入到接收信号时既经历相位变化也经历幅度变化。
3) 当物体穿过一系列菲涅尔区时，接收信号呈现连续的类似正弦波的波形，波峰和波谷由穿过边界产生。
4) 如果运动物体反射的信号改变路径长度$\lambda$，其相位将经历$2\pi$的变化，产生一个完整的正弦波周期；如果反射信号改变的路径长度小于$\lambda$，则产生的信号是正弦波周期的一个片段。

## 在WiFi菲涅尔区中感知人体呼吸

在本节中，我们首先将人体建模为一个尺寸变化的半圆柱体，模拟呼吸过程中的胸部运动，然后将胸部位移转换为相位变化，以表征人体呼吸如何影响最终的接收信号。通过将此相位变化与一个人的位置/朝向联系起来，我们发展了关于呼吸可检测性相对于菲涅尔模型的理论。然后我们展示何时以及如何利用频率分集帮助呼吸检测。最后，我们研究多用户呼吸感知场景。

## 对人体呼吸建模

人体呼吸是一个由肺部周期性充气和放气组成的过程。从肋骨骨架描述的运动和形变如图4（a）所示[37]。呼吸过程中的胸部位移在前后向上为$4.2\sim 5.4\mathrm{mm}$，在左右向上为$0.6\sim 1.1\mathrm{mm}$[24]；而在深吸气屏息期间，前后向上的位移可增加到$12.6\mathrm{mm}$[31]。因此，我们将人建模为一个尺寸变化的半圆柱体，如图4（b）所示，其中外圆柱面和内圆柱面分别对应呼气和吸气时的胸部位置。

## 将胸部位移转换为相位变化

为了感知一个人的呼吸率，我们将人体视为反射面（见图5（a）），并研究呼吸引起的胸部位移如何作为运动物体影响接收射频信号。为此，我们首先将胸部位移转换为反射路径长度的变化，然后将路径长度变化转换为相位变化。令$\Delta d$为胸部位移的距离，那么由胸部运动引起的路径长度变化约为（不超过）$2\Delta d$[39]。如果反射信号的路径长度连续变化一个波长$\lambda$，则产生的反射信号将表现出$2\pi$的相位变化（旋转）。因此，如图5（b）所示，由呼吸引起的相位旋转$\theta$可通过以下公式计算：

$$\theta \approx 2\pi *2\Delta d / \lambda \quad (3)$$

其中$\lambda$约为$5.7\mathrm{cm}$（对于$5\mathrm{GHz}$）和$11\mathrm{cm}$（对于$2.4\mathrm{GHz}$）。由于正常呼吸和深吸气屏息等深呼吸在前后向上的平均胸部运动距离$\Delta d$分别约为$5\mathrm{mm}$和$12\mathrm{mm}$[24][31]，因此产生的相位变化$\theta$在$60^{\circ}$到$150^{\circ}$之间。

因为反射信号经历的相位变化为$60^{\circ}\sim 150^{\circ}$，对应于正弦波周期的一个片段。现在我们要问以下问题：不同的片段如何影响接收信号？在正弦波周期中，片段的最佳和最差位置在哪里？

## 人体位置 vs. 最终接收信号

根据方程（2），我们知道当$H_{s}$和$H_{d}$恒定时，接收信号$H(f,t)$的幅度与$cos\theta$项有关。需要注意的是，当运动物体穿过几个波长（对应于几个周期和几个菲涅尔区）时，$H(f,t)$看起来像一个正弦波。但对于一个相位变化在$60^{\circ}$到$150^{\circ}$之间的正常呼吸，接收信号$H(f,t)$肯定不再是正弦波，那么它看起来像什么？由于一个呼吸周期包括吸气、暂停、呼气和另一个暂停，并且人体呼吸通常是周期性的，那么根据方程（2），接收信号$H(f,t)$也由四部分组成：吸气产生的波形、吸气后暂停引起的直线、呼气产生的波形、以及呼气后暂停引起的另一条直线。

正如我们已经知道的，由吸气或呼气引起的相位变化$\theta$在$60^{\circ}$到$150^{\circ}$之间，并且它位于余弦波周期中（见图6（a）），那么$\theta$的角度及其位置都会影响$H(f,t)$信号波形的形状。假设$\theta = \pi /3$对应于一次正常呼吸，如果角度覆盖的范围是从$- \pi /6$到$\pi /6$，如图6（b）所示，那么我们会看到吸气期间由$cos\theta$项产生的一个小弧线和呼气期间产生的另一个小弧线。在这种最差情况下，不仅由于当$\theta = 0$或$\pi$时$cos\theta$变化很小导致波形幅度小，而且一个周期包含多于一个波峰/波谷。但如果角度覆盖的范围是从$\pi /3$到$2\pi /3$，如图6（b）所示，那么我们会看到由吸气和呼气周期产生的一个大的类似梯形的波形。假设$\theta = 2\pi /3$对应于一次深呼吸，让角度覆盖的范围是从$\pi /6$到$5\pi /6$，那么我们会有一个甚至更大的由吸气和呼气周期产生的类似梯形的波形。显然，为了使呼吸率易于从接收信号中正确提取，期望$\theta$角不仅覆盖大范围，而且完全位于余弦波单调变化的片段中（大约在$\pi /2$或$3\pi /2$附近）。特别地，对于固定的$\theta$，余弦波片段的最佳选择是以$\pi /2$或$3\pi /2$为中心，对应于每个菲涅尔区的中间。

---

**图4. (a) 呼吸过程中的生理行为，(b) 半圆柱体呼吸模型。**

---

**图5. 菲涅尔区中的人体位置。**

---

**图6. 不同人体位置下最终接收信号的幅度和波形。**

总之，接收信号受呼吸深度和人体位置两者的影响。在每个菲涅尔区内，呼吸感知的最差人体位置在边界附近，而最佳位置出现在中间，如图5（b）（c）所示。比较不同的菲涅尔区，如果受试者出现在菲涅尔区内相同的相对位置，位于内区由于反射路径较短而产生更强的反射信号，使得接收信号比在外区更容易检测。

## 身体朝向 vs. 最终接收信号

我们已经知道呼吸过程中的胸部位移在前后向和左右向上是不同的，背部在呼吸过程中的位移几乎为零。因此，随着朝向的改变，用于信号反射的身体位移也会改变。由于接收信号的变化取决于由身体位移引起的反射信号的变化，因此除了呼吸深度和身体位置外，它还受身体朝向的影响。

如图7所示，我们假设受试者已经位于最佳位置（情况1），例如，在第二菲涅尔区的中间。当受试者旋转$180^{\circ}$时，有效身体位移将从最大变为零，对应于朝向从面对视距变为背对视距。

如图7的情况3所示，我们将左右向上的身体位移分解为两个分量：有效位移沿着法线方向，它导致反射路径长度变化；另一个沿着切线方向，它不引起反射路径变化。因此理论上，每个朝向都有一个相关的有效身体位移，范围从0到$12mm$。对于图7中的情况1，由于受试者与切线平行且面对视距，有效身体位移最大，将产生最大的相位变化；对于情况2，受试者的胸部与法线平行，由于左右向上的胸部运动仅约$1mm$（从$0.6 - 1.1mm$）[24]，有效身体位移将很小。进一步对于情况3和情况4，有效位移可能更小。在所有情况下，产生的反射信号能否被检测到不仅取决于位置（影响信号幅度），还取决于环境噪声。

## 菲涅尔区 vs. 频率分集

到目前为止，我们只研究了单一频率$f$载波的菲涅尔区模型，但实际上在WiFi Intel 5300设备中，我们有30个子载波的CSI测量值，每个子载波都有自己的波长和频率。现在让我们选择两个子载波$f = 5.24GHz$，$f' = f + \Delta F$，其中802.11n中相邻子载波之间的频率间隔$\Delta F$对于30个子载波最多为625KHz，并将它们的菲涅尔区放在一起。有趣的是，我们注意到对于内菲涅尔区，这两个子载波几乎具有重叠的边界；但随着区数的增加，它们对应区边界之间的差异不断增加，直到$f$的第$i$菲涅尔区边界赶上子载波$f^{\prime}$的第$(i + 1)$菲涅尔区边界。从上述观察中，我们理解对于内菲涅尔区，如果一个子载波无法测量呼吸，其他子载波也无法测量。但从某个菲涅尔区向外，当一个人的位置接近一个子载波的坏位置（靠近菲涅尔区边界）时，也许我们可以利用频率分集找到另一个对于相同位置显示出最佳或良好位置（靠近菲涅尔区中间）的子载波。

带着这个想法，假设我们选择第一个子载波来感知一个人的呼吸，对应的相位角为$\pi /3$，然后我们问受试者位于子载波1的最差位置（从$- \pi /6$到$\pi /6$），从哪个菲涅尔区$m$开始，我们总能找到一个与子载波1相移为$\pi / 6$的子载波？

为了解决这个问题，我们考虑两个极端子载波$f_{1} = 5.24GHz$和$f_{2} = f_{1} + 29*\Delta F$，其波长分别为$\lambda_{1}$和$\lambda_{2}$。假设受试者位于$f1$的第$m$菲涅尔区的边界上。令$c$为光速，$\phi_{ref}$为反射引起的附加相移，$L$为视距长度，则反射信号的路径长度$P_{L}$为$P_{L} = m / 2\lambda_{1} + L$。视距与反射信号之间的相位差$\Psi (f)$为$\Psi (f) = 2\pi f(P_{L} - L) / c + \phi_{ref}$，则$f_{1},f_{2}$之间的相移由$\Delta \Psi (f_{1},f_{2}) = 2\pi f_{2}(P_{L} - L) / c - 2\pi f_{1}(P_{L} - L) / c$给出，将$f_{1},f_{2},P_{L}$代入方程：

$$\Delta \Psi (m) = m\pi \Delta F / f_{1}\approx 0.0035m\pi \quad (4)$$

因此，可以解出满足$\Delta \Psi (m) > = \pi /6$的$m$。得到的$m$为48，这意味着如果受试者位于第48菲涅尔区之外，至少有一个子载波适合使用。对于视距设置为1m的情况，第48菲涅尔区沿视距垂直平分线距离为108 cm。当然，除了频率分集，我们还需要考虑当受试者远离收发器时反射路径中的功率损耗。

---

**图7. 菲涅尔区中的身体朝向$\alpha$。**

---

**图8. 两人呼吸检测示意图：实绿线代表菲涅尔区中间，红线代表边界；两条虚线代表将设备从$R_{x}$移动到$R_{x}^{\prime}$后的两条线。**

## 多用户位置 vs. 呼吸可检测性

基于为单个受试者发展的理论，我们现在考虑感知两个受试者的呼吸率，假设两个受试者如图8所示躺在床上。由于两个受试者被视为同一静态环境中的两个运动物体，总的接收信号方差可以近似为由每个人胸部位移引起的方差的线性组合[40]。假设呼吸是来自两个受试者的唯一身体运动，那么每个受试者的呼吸深度、身体位置和朝向对接收信号的影响是独立的，并且与之前相同。因此，为了同时检测两个受试者的呼吸率，我们需要确保每个受试者的位置都在某个内菲涅尔区的中间附近。如图8所示，我们展示了通过固定他们的位置同时改变一个WiFi设备的位置，很容易将两个受试者的位置暴露在某个菲涅尔区的中间（最佳位置）。在这种设置下，每个人的呼吸率可以在频域中使用功率谱密度提取。

## 评估

为了验证我们发展的理论，我们首先使用现成的WiFi设备实现了一个实时人体呼吸检测系统。然后我们进行全面评估，并在本节报告结果。

## 系统实现与实验环境

我们的原型系统包括两个主要模块：信号预处理和呼吸率估计。我们遵循其他系统中的许多设计选择[6, 21]，以在真实世界环境中理解和验证我们的理论。

我们收集WiFi CSI，每个CSI流包含来自30个子载波的读数。CSI流首先由信号预处理模块处理以减少噪声。特别地，我们在每个子载波上应用带有滑动窗口的Hampel滤波器[6]来移除异常值，这些异常值通常与邻近的CSI测量值有显著差异。此外，由于人体呼吸率通常具有低频范围，我们应用移动平均滤波器来移除不太可能由呼吸引起的高频噪声。处理后的CSI流随后由呼吸率估计模块分析以估计呼吸率。具体地，我们利用基于阈值的方法，选择在时间窗口内具有较大CSI幅度方差的子载波，如[21]中所述。对于单用户呼吸检测，为了捕捉由吸气和呼气过程中胸部运动引起的周期性变化，我们使用如[21]中的虚假峰值移除算法来移除彼此过于接近的峰值。对于多用户呼吸检测，由于CSI信号是每个受试者方差的线性组合，因此我们使用功率谱密度将每个子载波上的CSI测量时间序列转换为其在频域中的功率分布。一个强的周期性信号会在PSD中对应于其周期的频率处产生一个峰值，例如，当检测两个受试者的呼吸率时，CSI测量应呈现两个峰值，分别对应于每个受试者的频率。

我们部署了一个原型系统，包括一个WiFi设备（即迷你PC）和一个商用无线接入点（即TP-Link WDR5300，带一根天线，运行在5.24GHz频道上），组成一个802.11n WiFi网络。迷你PC配备了一个Intel WiFi Link 5300卡用于测量CSI[11]和一根外置天线。数据包传输率设置为20包/秒。我们选择并放置天线的方式与我们之前验证菲涅尔区时相同。

## 实验方法

我们在三个月内招募了九名参与者。要求每位参与者坐在椅子上或躺在床上，正常自然地呼吸。我们构建了一个基于Web的用户界面，用于显示呼吸率以及实时录制的视频，如图9所示。当受试者进行实验时，另外两名受试者独立观看视频流以手动记录真实值。

使用原型系统，我们研究了以下两种情况：（1）在室内环境中放置一对WiFi收发器，我们能否在任何位置有效地检测单用户呼吸？用户位置或身体朝向是否重要，它们如何影响性能？（2）给定两个受试者，具有固定的用户位置和身体朝向，我们应该将WiFi设备放在哪里以实现最佳性能？

## 案例1：单用户与一对WiFi Tx/Rx

我们首先要求一名受试者坐在办公室的椅子上。我们固定WiFi Tx和Rx之间的视距距离（例如，相隔1m，高度70cm），并通过改变受试者的位置和身体朝向评估性能。

## A. 固定视距距离

视距距离$(R_{1} + R_{2})$设置为1m，波长$(\lambda)$设置为$5.7\mathrm{cm}$（即中心频率$5.24\mathrm{GHz}$），我们首先通过数学上根据方程（1）计算菲涅尔区来获得案例1的理论基础，例如，前三个菲涅尔区的边界沿垂直平分线距离视距分别为$12.05cm$、$17.16cm$和$21.16cm$。然后我们进行一系列实验，通过改变用户位置、身体朝向以及CSI子载波来评估性能。

**（1）受试者位置是否重要？** 在这个实验中，一对WiFi Tx/Rx（视距距离$= 1\mathrm{m}$）放置在办公室，要求一名受试者坐在不同位置的椅子上，以$0^{\circ}$身体朝向正常呼吸。我们首先在同一个菲涅尔区内改变受试者的位置。受试者首先沿着一个菲涅尔区的中间椭圆移动（一个菲涅尔区在物理上可以看作是内椭圆和外椭圆之间的区域）。图10（a）显示了第二菲涅尔区内不同位置的结果。我们观察到，沿着同一菲涅尔区中间椭圆的不同位置，CSI信号相当一致且足够清晰，可用于检测。然后要求受试者穿过不同的菲涅尔区移动。图10（b）显示了受试者沿垂直平分线穿过第2和第3菲涅尔区时的结果。该图显示，虽然CSI信号在菲涅尔区的中间椭圆处存在强烈，但在向边界移动时开始衰减。我们在每个边界处都观察不到清晰的周期性模式，因此很可能导致检测失败。虽然我们在每个菲涅尔区中都观察到类似现象，

---

**图9. 基于Web的用户界面。**

---

**图10. 用户位置和身体朝向的评估：(a) 沿椭圆线移动，固定身体朝向；(b) 沿垂直平分线移动，固定身体朝向；(c) 在固定用户位置旋转身体朝向。**

受试者越接近视距，获得的周期性模式越清晰。在这个实验设置中，结果表明当受试者沿垂直平分线距离视距超过$2m$时（即大约第110菲涅尔区），我们无法检测到呼吸。

**（2）受试者身体朝向是否重要？** 我们现在研究受试者的身体朝向，并评估系统在菲涅尔区不同位置对不同身体朝向的表现如何。以图例说明，一名受试者坐在第二菲涅尔区的中间，并将他的身体朝向从$0^{\circ}$改变到$135^{\circ}$。结果如图10（c）所示。从图中我们观察到，最清晰的周期性模式出现在$0^{\circ}$，并且随着身体朝向的增加开始衰减。在$90^{\circ}$之后，由于胸部反射面被人体阻挡，信号迅速变得平坦。使用与上述类似的实验设置，我们现在通过增加与视距的距离，移动到更远的菲涅尔区（即第2、第3等）。虽然我们观察到类似的趋势，但可检测到的身体朝向范围随着距离增加而不断减小，如图11所示。

**（3）CSI子载波的影响** 最后，我们评估CSI子载波的影响。我们首先评估CSI子载波在可检测区域（即理想情况）中是否响应不同。要求一名受试者坐在靠近视距的第二菲涅尔区中间，我们将每个子载波的幅度绘制在图12（a）中。该图显示由人体呼吸引起的周期性模式是清晰的，并且所有子载波都给出类似的响应。然后我们在菲涅尔区边界（即最差情况）进行类似研究。现在将受试者移动到第二菲涅尔区的边界。图12（b）显示，对于所有子载波都观察不到清晰的周期性模式。在相同的实验设置下，要求受试者深呼吸，如图12（b）所示，我们观察到所有子载波的每个呼吸周期中都出现两个双峰，这与我们的理论分析一致。最后，我们评估另一种最差情况，即受试者坐在离视距更远的地方。一名受试者沿垂直平分线远离视距1m移动。如图12（d）所示，我们现在能够观察到子载波之间的差异。我们可以看到前五个子载波的响应明显比其他所有子载波更清晰。回想方程（1）和（4），如果$L = 100cm$，边界约为$100cm$，如果第一个子载波无法响应，至少有一个子载波（间隔29个）可以使用。这证明了我们之前的分析，即当受试者远离视距时，我们可以利用不同的CSI子载波来提供更好的检测。

**（4）讨论** 我们现在总结迄今为止获得的结果，并重新审视我们开始时提出的问题。我们在图13（a）中绘制了用户位置热图，该图显示了CSI信号如何随不同用户位置变化。从这个图中可以得出几个观察结果。首先，空间中存在清晰的菲涅尔区。受试者越接近Tx/Rx，我们获得的CSI信号越强。当受试者超过某个极限（即检测边界）时，没有清晰的CSI信号存在以有效检测人体呼吸。其次，在接近检测边界的区域，我们称之为频率分集启用区，除非选择特定的CSI子载波，否则无法保证成功检测。第三，对于每个菲涅尔区，我们在中间观察到强CSI信号，但在向边界移动时信号开始衰减，直到在每个边界处没有清晰信号出现，导致检测失败。图13（b）显示了CSI信号如何随不同身体朝向变化。请注意，该图是在受试者位于可检测区域（例如，每个菲涅尔区的中间）时绘制的。我们观察到，虽然我们能够在靠近Tx/Rx的菲涅尔区中检测受试者从0-90度的身体朝向，但在靠近检测边界的菲涅尔区中，可检测的身体朝向不断减小，直到0度。请注意，虽然我们在第一菲涅尔区中取得了良好的结果，但受试者遮挡视距情况下的理论模型涉及更复杂的因素，如阴影、扩散等，这有待进一步研究。

总之，我们得出结论，用户位置和身体朝向确实影响CSI信号的质量，从而影响检测人体呼吸时的系统性能。通过这个案例研究，我们验证了WiFi菲涅尔区的存在，并在现实世界环境中证明了其特性，揭示了对于设计任何基于CSI的人体呼吸检测系统都很重要的见解和新原理。

### B. 改变视距距离

在这个实验中，我们分别将视距距离改变为$0.5\mathrm{m}$、$2\mathrm{m}$、$3\mathrm{m}$和$6\mathrm{m}$，并重复我们固定视距距离时所做的相同实验。为了确保前12个菲涅尔区不受地面等反射体的影响，天线高度根据上述视距距离设置分别设置为$50cm$、$70cm$、$70cm$和$100cm$。我们在同一办公室进行前两种情况（即$\mathrm{LOS} = 0.5\mathrm{m}$、$2\mathrm{m}$）的实验，但移动到大厅进行其他两种情况（即$\mathrm{LOS} = 3\mathrm{m}$、$6\mathrm{m}$）的实验。我们观察到与先前实验类似的结果。然而，当视距距离大于$3\mathrm{m}$时，CSI信号变得易受环境噪声影响，即使使用最佳用户位置和身体朝向，也无法保证有效检测。

## 案例2：双用户与一对WiFi Tx/Rx

最后，我们简要介绍多用户案例，以双用户为例。一对WiFi Tx/Rx相距$2\mathrm{m}$部署在一个房间内。从九名参与者中，我们选择了六对具有不同呼吸率的参与者，并要求他们并排躺在床上，位于Tx和Rx之间，如图9所示。我们首先固定Tx，然后我们意图通过改变Rx的位置来感知一个人的呼吸率（不感知另一个人的）以及两个受试者的呼吸率。图14显示了三种不同场景的结果。图14中的上图按时间域绘制，而下图按频率域绘制。图14（a,d）说明了第一个场景，我们成功检测到两个受试者的呼吸率。在第二个场景中（如图14（b,e）所示），我们有目的地检测一个受试者的呼吸率但错过了另一个，反之亦然，在第三个场景中（图14（c,f））。

---

**图11. 不同菲涅尔区中身体朝向的评估。**

---

**图12. CSI子载波的评估。**

---

**图13. (a) 用户位置热图，(b) 用户朝向热图（彩色观看效果更佳）。**

---

**图14. 两人呼吸检测：红线表示真实值；(a-c) 时域；(d-f) 频域。**

## 局限性

该系统具有以下局限性：（1）对于单用户呼吸感知，当附近有一些用户移动时，系统可能无法检测到一个人的呼吸率。然而，当这些用户在远离受试者和视距的地方进行正常活动时，受试者的呼吸率仍可能被检测到。（2）对于双用户情况，假设参与者具有不同的呼吸模式，以便在频域中区分他们的呼吸率。然而，除非我们假设不同的人具有不同且相对稳定的呼吸模式，否则很难知道谁的呼吸对应谁。

## 结论与未来工作

从最近关于基于WiFi CSI的人体呼吸检测和其他细粒度人体活动识别的工作出发，在我们的研究中，我们试图提出并回答以下问题：无论一个人待在房间里的什么地方、面向何方，是否总能用一对WiFi设备感知到人体呼吸？什么因素影响人体呼吸感知，指导呼吸感知系统设计背后的理论是什么？基于WiFi射频的非接触式传感可能的物理极限是什么？受上述问题驱动，在本文中，我们引入了一种新颖的WiFi菲涅尔模型，并发展了相关理论，为室内环境中WiFi无线电传播奠定了理论基础。我们将理论应用于非接触式人体呼吸检测应用，并进行了全面的理论和实验研究，以探讨用户位置、身体朝向和频率分集如何影响呼吸感知性能。所获得的结果不仅证明了我们发展的理论，而且为构建经济有效的基于WiFi CSI的人体呼吸感知系统提供了基本原理和实践指南。

值得注意的是，虽然我们在本文中仅将理论应用于检测人体呼吸，但实际上该理论可以普遍应用于任何基于射频的传感和检测系统。此外，这项工作也有助于理解基于射频的运动检测的物理极限，具有众多潜在应用，如手势识别、虚拟键盘等。对于未来的工作，一方面，我们计划利用更高的发射功率、频率分集和多天线，进一步改进基于WiFi CSI的呼吸监测系统；另一方面，我们打算进一步发展理论，以理解诸如衍射、反射和阴影如何相互干扰等问题，并将所发展的理论应用于新的应用。

## 致谢

本研究受国家重点研发计划资助（项目号：2016YFB1001200），国家自然科学基金资助（项目号：61572048），上海市国资委能力提升计划资助（项目号：2014-C-1-02），北京大学重点学科建设资助。我们感谢匿名审稿人提出的宝贵建议。

## 参考文献

1. Heba Abdelnasser, Khaled A Harras, and Moustafa Youssef. 2015. Ubibreathe: A ubiquitous non-invasive wifi-based breathing estimator. arXiv preprint arXiv:1505.02388 (2015).
2. Gregory D Abowd, Aaron F Bobick, Irfan A Essa, Elizabeth D Mynatt, and Wendy A Rogers. 2002. The aware home: A living laboratory for technologies for successful aging. In Proceedings of the AAAI-02 Workshop Automation as Caregiver. 1-7.
3. Fadel Adib, Hongzi Mao, Zachary Kabelac, Dina Katabi, and Robert C Miller. 2015. Smart homes that monitor breathing and heart rate. In Proceedings of the 33rd Annual ACM Conference on Human Factors in Computing Systems. ACM, 837-846.
4. Gabriel Clavier Andre and Henri Darbord Rene. 1936. Directional radio transmission system. (June 9 1936). US Patent 2,043,347.
5. Robert E Collin. 1985. Antennas and radiowave propagation. McGraw-Hill.
6. Laurie Davies and Ursula Gather. 1993. The identification of multiple outliers. J. Amer. Statist. Assoc. 88, 423 (1993), 782-792.
7. H Erhan Dincer and William O'Neill. 2006. Deleterious effects of sleep-disordered breathing on the heart and vascular system. Respiration 73, 1 (2006), 124-130.
8. NHLBI: Health Information for the Public. U.S. Department of Health and Human Services. 2010. Sleep apnea: What is sleep apnea? (2010).
9. GigaOm. 2016. Could a breath-monitoring headset improve your health? https://gigaom.com/2013/09/20/could-a-breath-monitoring-headset-improve-your-health/. (2016). [Online; accessed 8-mar-2016].
10. Hulya Gokalp and Malcolm Clarke. 2013. Monitoring activities of daily living of the elderly and the potential for its use in telecare and telehealth: a review. TELEMEDICINE and e-HEALTH 19, 12 (2013), 910-923.
11. Daniel Halperin, Wenjun Hu, Anmol Sheth, and David Wetherall. 2011. Tool release: Gathering 802.11 n traces with channel state information. ACM SIGCOMM Computer Communication Review 41, 1 (2011), 53-53.
12. Chunmei Han, Kaishun Wu, Yuxi Wang, and Lionel M Ni. 2014. WiFall: Device-free fall detection by wireless networks. In INFOCOM, 2014 Proceedings IEEE. IEEE, 271-279.
13. Healthcare. 2012. F. M. Market for embedded health monitoring-gadgets to hit 170M devices by 2017. http://www.fiercemobilehealthcare.com/story/market-embedded-health-monitoring-gadgets-hit-170m/-devices-2017/2012-08-03. (2012).
14. Sumi Helal, William Mann, Jeffrey King, Youssef Kaddoura, Erwin Jansen, and others. 2005. The gator tech smart house: A programmable pervasive space. Computer 38, 3 (2005), 50-60.
15. Hristo D Hristov. 2000. Fresnal Zones in Wireless Links, Zone Plate Lenses and Antennas. Artech House, Inc.
16. Francis A Jenkins and Harvey E White. 1957. Fundamentals of optics. Tata McGraw-Hill Education.
17. Ossi Kaltiokallio, Huseyin Yigitler, Riku Jantti, and Neal Patwari. 2014. Non-invasive respiration rate monitoring using a single COTS TX-RX pair. In Information Processing in Sensor Networks, IPSN-14 Proceedings of the 13th International Symposium on. IEEE, 59-69.
18. T Kondo, T Uhlig, P Pemberton, and PD Sly. 1997. Laser monitoring of chest wall displacement. European Respiratory Journal 10, 8 (1997), 1865-1869.
19. James C Lin, Joseph Kiernicki, Martin Kiernicki, and Paul B Wollschlaeger. 1979. Microwave aperiodography. Microwave Theory and Techniques, IEEE Transactions on 27, 6 (1979), 618-620.
20. Chen Liu, Dingyi Fang, Zhe Yang, Hongbo Jiang, Xiaojiang Chen, Wei Wang, Tianzhang Xing, and Lin Cai. 2016b. RSS Distribution-Based Passive Localization and Its Application in Sensor Networks. IEEE Transactions on Wireless Communications 15, 4 (2016), 2883-2895.
21. Jian Liu, Yan Wang, Yingying Chen, Jie Yang, Xu Chen, and Jerry Cheng. 2015. Tracking Vital Signs During Sleep Leveraging Off-the-shelf WiFi. In Proceedings of the 16th ACM International Symposium on Mobile Ad Hoc Networking and Computing. ACM, 267-276.
22. Xuefeng Liu, Jiannong Cao, Shaojie Tang, and Jiaqi Wen. 2014. Wi-Sleep: Contactless sleep monitoring via WiFi signals. In Real-Time Systems Symposium (RTSS), 2014 IEEE. IEEE, 346-355.
23. Xuefeng Liu, Jiannong Cao, Shaojie Tang, Jiaqi Wen, and Peng Guo. 2016a. Contactless Respiration Monitoring via WiFi Signals. Mobile Computing, IEEE Transactions on (2016).
24. C Lovanichkiattikul, M Dhanachai, C Sitathanee, S Khachonkham, and P Khaothong. 2016. Impact of chest wall motion caused by respiration in adjuvant radiotherapy for postoperative breast cancer patients. SpringerPlus 5, 1 (2016), 1-8.
25. Se Dong Min, Jin Kwon Kim, Hang Sik Shin, Yong Hyeon Yun, Chung Keun Lee, and Myoungho Lee. 2010. Noncontact respiration rate measurement system using an ultrasonic proximity sensor. Sensors Journal, IEEE 10, 11 (2010), 1732-1739.
26. M Nowogrodzki, DD Mawhinney, and HF Milgazio. 1984. Non-invasive microwave instruments for the measurement of respiration and heart rates. NAECON 1984 (1984), 958-960.
27. Shoko Nukaya, Toshihiro Shino, Yosuke Kurihara, Kajiro Watanabe, and Hiroshi Tanaka. 2012. Noninvasive bed sensing of human biosignals via piezoceramic devices sandwiched between the floor and bed. Sensors Journal, IEEE 12, 3 (2012), 431-438.
28. Rita Paradiso. 2003. Wearable health care system for vital signs monitoring. In Information Technology Applications in Biomedicine, 2003. 4th International IEEE EMBS Special Topic Conference on. IEEE, 283-286.
29. Neal Patwari, Lara Brewer, Quinn Tate, Ossi Kaltiokallio, and Maurizio Bocca. 2014a. Breathfinding: A wireless network that monitors and locates breathing in a home. Selected Topics in Signal Processing, IEEE Journal of 8, 1 (2014), 30-42.
30. Neal Patwari, James Wilson, Sundaram Ananthanarayanan, Sneha Kumar Kasera, and Dwayne R Westenskow. 2014b. Monitoring breathing via signal strength in wireless networks. Mobile Computing, IEEE Transactions on 13, 8 (2014), 1774-1786.
31. Anders N Pedersen, Stine Korreman, Hakan Nystrom, and Lena Specht. 2004. Breathing adapted radiotherapy of breast cancer: reduction of cardiac and pulmonary doses using voluntary inspiration breath-hold. Radiotherapy and oncology 72, 1 (2004), 53-60.
32. Jochen Penne, Christian Schaller, Joachim Hornegger, and Torsten Kuwert. 2008. Robust real-time 3D respiratory motion detection using time-of-flight cameras. International Journal of Computer Assisted Radiology and Surgery 3, 5 (2008), 427-431.
33. Ruth Ravichandran, Elliot Saba, Ke-Yu Chen, Mayank Goel, Sidhant Gupta, and Shwetak N Patel. 2015. WiBreathe: Estimating respiration rate using wireless signals in natural settings in the home. In Pervasive Computing and Communications (PerCom), 2015 IEEE International Conference on. IEEE, 131-139.
34. Stefano Savazzi, Stephan Sigg, Monica Nicoli, Vittorio Rampa, Sanaz Kianoush, and Umberto Spagnolini. 2016. Device-Free Radio Vision for Assisted Living: Leveraging wireless channel quality information for human sensing. IEEE Signal Processing Magazine 33, 2 (2016), 45-58.
35. David Tse and Pramod Viswanath. 2005. Fundamentals of wireless communication. Cambridge university press.
36. Svetha Venkatesh, Christopher R Anderson, Natalia V Rivera, and R Michael Buehrer. 2005. Implementation and analysis of respiration-rate estimation using impulse-based UWB. In Military Communications Conference, 2005. MILCOM 2005. IEEE. IEEE, 3314-3320.
37. Pierre-Frederic Villard, Piers Boshier, Fernando Bello, and Derek Gould. 2011. Virtual reality simulation of liver biopsy with a respiratory component. InTech.
38. Hao Wang, Daqing Zhang, Yasha Wang, Junyi Ma, Yuxiang Wang, and Shengjie Li. 2016. RT-Fall: A Real-time and Contactless Fall Detection System with Commodity WiFi Devices. Mobile Computing, IEEE Transactions on (2016).
39. Wei Wang, Alex X Liu, Muhammad Shahzad, Kang Ling, and Sanglu Lu. 2015. Understanding and modeling of wifi signal based human activity recognition. In Proceedings of the 21st Annual International Conference on Mobile Computing and Networking. ACM, 65-76.
40. Joey Wilson and Neal Patwari. 2011. See-through walls: Motion tracking using variance-based radio tomography networks. Mobile Computing, IEEE Transactions on 10, 5 (2011), 612-621.
41. James C Wiltse. 1999. History and evolution of Fresnel zone plate antennas for microwaves and millimeter waves. In Antennas and Propagation Society International Symposium, 1999. IEEE, Vol. 2. IEEE, 722-725.
42. Chenshu Wu, Zheng Yang, Zimu Zhou, Xuefeng Liu, Yunhao Liu, and Jiannong Cao. 2015. Non-Invasive Detection of Moving and Stationary Human With WiFi. Selected Areas in Communications, IEEE Journal on 33, 11 (2015), 2329-2342.
43. Daqing Zhang, Hao Wang, Yasha Wang, and Junyi Ma. 2015. Anti-fall: A Non-intrusive and Real-Time Fall Detector Leveraging CSI from Commodity WiFi Devices. In Inclusive Smart Cities and e-Health. Springer, 181-193.