# ISAC 知识体系 (ISAC Knowledge System)

## 1. 核心理论与基础 (Core Theory and Fundamentals)

### 信号处理基础 (Signal Processing Fundamentals)
- 正交频分复用 (Orthogonal Frequency Division Multiplexing, OFDM(5G主要在用))
- 正交时频空（Orthogonal Time Frequency Space） 调制技术
    
    * 6G 需要在更极端的物理环境下工作：

    * 高频段挑战： 在毫米波或太赫兹频段，微小的移动都会引起巨大的多普勒频移。

    * 卫星通信： 卫星相对于地面的高速运动是 OTFS 的完美应用场景。
    * 可靠性： 对于自动驾驶等需要极低误码率的场景，OTFS 的表现比传统方案更稳健。
- 多输入多输出 (Multiple-Input Multiple-Output, MIMO)

- 波束赋形 (Beamforming)
- 信道估计 (Channel Estimation)
- 信号检测与估计 (Signal Detection and Estimation)
- 虚拟天线增益（virtual sensor gain）
- 多目标轨迹追踪理论（Multi-Target Trajectory Tracking）
    * 时频图谱分析（Spectrogram Analysis）：将CSI时间序列转化为时频图，作为多目标特征的观测域
    * 迭代动态规划（Iterative Dynamic Programming, IDP）：解决多目标信号在时频图上的路径优化问题，用于从噪声中提取连续的频率轨迹

### 感知理论 (Sensing Theory)
- 雷达方程 (Radar Equation)
- 目标检测理论 (Target Detection Theory)
- 参数估计理论 (Parameter Estimation Theory)
- 分辨力理论 (Resolution Theory)
- 模糊函数 (Ambiguity Function)
- 动态散射体建模 (Dynamic Scatterer Modeling)
- 时间反演理论（Time Reversal Theory）
    * 时间反演感知理论 (Time Reversal Sensing Theory)
        * 时间反演共振强度衰减 (TRRS Decay)
        * 基于TRRS的距离映射
    * 时间反演聚焦理论 (Time Reversal Focusing Theory)

- 室内多径统计模型（Statistical Multipath Modeling for Indoors）
    * 将CSI分解为时变分量（由人体运动引起）和时不变分量（背景环境）
    * 不同子载波对同一运动的响应在幅度和相位上存在差异
    * 考虑室内所有反射和散射路径的叠加
- 分集感知理论 (Diversity Sensing Theory)
    * 频谱分集 (Spectrum Diversity)：利用不同子载波对不同位置反射的频率响应差异，分离空间中不同目标的信号
    * 时间分集 (Time Diversity)：利用某些信号在时间上的连续性和规律性，解决瞬时信号衰落或重叠问题

### 通信理论 (Communication Theory)
- 香农定理 (Shannon-Hartley Theorem)
- 编码理论 (Coding Theory)
- 调制解调技术 (Modulation and Demodulation Techniques)
- 多址接入技术 (Multiple Access Techniques)
- 信息论基础 (Information Theory Basics)
- 信道互易性（Channel Reciprocity）

    * 信道冲激响应（CIR）相同
    * 信道状态信息（CIS）相同

### 联合波形设计 (Integrated Waveform Design)
- 正交波形 (Orthogonal Waveforms)
- 非正交波形 (Non-orthogonal Waveforms)
- 多载波感知波形 (Multicarrier Sensing Waveforms)
- 调频连续波 (Frequency Modulated Continuous Wave, FMCW)
- 相位编码波形 (Phase-Coded Waveforms)

## 2. 系统架构与模式 (System Architecture and Modes)

### 一体化硬件架构 (Integrated Hardware Architecture)
- 共享天线阵列 (Shared Antenna Array)
- 共享射频前端 (Shared RF Front-end)
- 共享基带处理单元 (Shared Baseband Processing Unit)
- 全双工收发机 (Full-Duplex Transceiver)
- 可重构智能表面辅助 (Reconfigurable Intelligent Surface Aided, RIS-Aided)

### 网络部署模式 (Network Deployment Modes)
- 单站单功能模式 (Monostatic Single-function Mode)
- 单站多功能模式 (Monostatic Multi-function Mode)
- 多站协作感知模式 (Multistatic Cooperative Sensing Mode)
- 蜂窝网络感知模式 (Cellular Network Sensing Mode)
- 车联网协同模式 (V2X Cooperative Mode)
- 穿墙感知部署模式 (Through-the-Wall Sensing Deployment)
    * 发射机与接收机置于不同房间，利用WiFi信号的穿透性探测隔墙目标
    * 非视距（NLOS）感知架构

### 信号处理流程 (Signal Processing Flow)
- 感知优先处理链 (Sensing-First Processing Chain)
- 通信优先处理链 (Communication-First Processing Chain)
- 交替处理链 (Time-Division / Alternating Processing Chain)
- 并行处理链 (Parallel Processing Chain)
- 联合优化处理链 (Joint Optimization Processing Chain)
- 多状态感知处理链 (Multi-State Sensing Processing Chain)
    * 原始数据采集（CSI）与净化
    * 统计量提取状态（转化为ACF）
    * 信号增强（子载波选择 & 频谱图像均衡化）
    * 峰值检测或模型匹配
- 状态感知与切换机制 (State-Aware Processing Mechanism)
- 时间反演感知处理流程 (Time-Reversal-Based Sensing Processing Flow)
- 多目标呼吸追踪处理链 (Multi-Person Respiration Tracking Chain)
    * 自适应子载波组合 (Adaptive Subcarrier Combination)
    * 基于投影或加权的方法提升周期性成分，使得目标信号的能量最大化
    * 轨迹提取与拼接（Trace Extraction and Concatenating）

### 资源分配框架 (Resource Allocation Framework)
- 时域资源分配 (Time Domain Resource Allocation)
- 频域资源分配 (Frequency Domain Resource Allocation)
- 空域资源分配 (Spatial Domain Resource Allocation)
- 功率资源分配 (Power Domain Resource Allocation)
- 波形资源分配 (Waveform Resource Allocation)

## 3. 关键使能技术 (Key Enabling Technologies)

### 感知信号处理技术 (Sensing Signal Processing Technologies)
- 距离-多普勒处理 (Range-Doppler Processing)
- 角度估计 (Angle of Arrival Estimation / DOA)
- 高分辨率成像 (High-Resolution Imaging)
- 动目标检测 (Moving Target Detection / MTI)
- 微多普勒特征提取 (Micro-Doppler Feature Extraction)
- 基于CSI的微动感知 (Micro-Motion Sensing via CSI)
- 自相关函数频谱分析（Auto Correlation Function Spectrum Analysis，ACF）
    * 相比于FFT的优势：提供更高的频率分辨率，适用于捕捉瞬时频率变化
    * 具有噪声抑制特性
- 统计电磁建模 (Statistical Electromagnetic Modeling)
- 感知信号加权 (Sensing Weighting)
- 时间反演共振强度 (Time Reversal Resonating Strength, TRRS)
- 贝塞尔功率分布感知模型 (Bessel Power Distribution-Based Sensing)
- 自适应子载波组合方法（adaptive subcarrier combination method）
- 轨迹拼接算法（trace concatenating algorithm）
- 多目标信号分离与计数 (Multi-Target Separation and Counting)
    * 出自文献《Respiration Tracking for People Counting and Recognition》
    * 基于呼吸轨迹的计数 (Respiration-Based Counting)
        * 原理：不同人的呼吸频率和相位随时间变化具有独立性。通过统计独立的呼吸轨迹数量，实现对静止/睡眠人群的计数
        * 优势：无需大幅度体动，可用于监测静态人群（如多人睡眠、静坐）
    * 解决频率重叠问题：当两人呼吸频率相近时，利用历史轨迹趋势和动态规划预测，区分是信号合并还是独立轨迹

### 通信-感知干扰消除 (Comm-Sensing Interference Cancellation)
- 自干扰消除 (Self-Interference Cancellation)
- 互干扰管理 (Mutual Interference Management)
- 串行干扰消除 (Serial Interference Cancellation, SIC)
- 预编码干扰抑制 (Precoding Interference Suppression)
- 盲源分离 (Blind Source Separation)
- 非目标动态干扰抑制 (Non-Target Motion Suppression)
- 最大比合并 (Maximal Ratio Combining)
    * 加权合并多子载波的ACF，可显著提升NLOS环境下的感知信噪比

### 人工智能赋能技术 (AI-Empowered Technologies)
- 深度学习目标识别 (Deep Learning Target Recognition)
- 神经网络信道预测 (Neural Network Channel Prediction)
- 强化学习资源调度 (Reinforcement Learning Resource Scheduling)
- 联邦学习隐私保护 (Federated Learning Privacy Protection)
- 生成式数据增强 (Generative Data Augmentation)
- 非深度学习感知方法 (Non-Deep-Learning-Based Sensing Methods)
- 面向边缘设备的低复杂度感知智能(Low-Complexity Edge Intelligence for ISAC)
- 隐马尔可夫模型状态机（Hidden Markov Model）
- 支持向量机（Support Vector Machine，SVM）
    * 一种经典的机器学习算法，常用于分类任务
- 基于生物特征的身份识别 (Biometric-Based Identity Recognition)
    * 呼吸指纹 (Respiration Fingerprint)
    * 身份匹配机制 (Identity Matching Mechanism)

### 高精度定位与同步 (High-Precision Positioning and Synchronization)
- 到达时间差定位 (Time Difference of Arrival, TDOA)
- 到达角定位 (Angle of Arrival, AOA)
- 载波相位定位 (Carrier Phase Positioning)
- 网络时间同步 (Network Time Synchronization)
- 联合定位与建图 (Simultaneous Localization and Mapping, SLAM)
- 多径统计定位（Multipath-Statistics-Based Localization）
- 时间反演高精度NLOS定位 (Time-Reversal-Based NLOS Localization)

## 4. 应用场景与用例 (Application Scenarios and Use Cases)

### 智能交通系统 (Intelligent Transportation Systems, ITS)
- 车辆防撞雷达 (Vehicle Collision Avoidance Radar)
- 交通流量监控 (Traffic Flow Monitoring)
- 自动驾驶环境感知 (Autonomous Driving Environmental Perception)
- 车辆编队通信 (Vehicle Platooning Communication)
- 路边单元感知服务 (Roadside Unit Sensing Services)
- 儿童遗留检测 (Child Presence Detection)

### 智慧城市与物联网 (Smart Cities and IoT)
- 室内人员检测与定位 (Indoor Human Detection and Localization)
- 无人机监控与通信 (UAV Monitoring and Communication)
- 智能家居态势感知 (Smart Home Situation Awareness)
- 基础设施健康监测 (Infrastructure Health Monitoring)
- 环境监测传感器网络 (Environmental Sensing Sensor Networks)
- 封闭空间人员存在与状态感知 (Enclosed-Space Presence and State Sensing)
- 室内设备速度感知 (Device-Free Indoor Speed Sensing)
- 室内连续跟踪 (Indoor Continuous Tracking)
- 非接触式人机交互 (Contactless HCI)
    * 穿墙手势书写识别 (Through-the-Wall Gesture Writing Recognition)
    * 位置无关的手势控制 (Location-Independent Gesture Control)

### 工业互联网 (Industrial Internet / IIoT)
- 工厂机器人导航 (Factory Robot Navigation)
- 物料库存监控 (Inventory Monitoring)
- 设备状态无线监测 (Equipment Status Wireless Monitoring)
- 高精度室内定位 (High-Precision Indoor Positioning)
- 安全生产监控 (Work Safety Monitoring)

### 下一代无线通信 (Next-Generation Wireless Communications / 6G)
- 6G通感一体化空口 (6G ISAC Air Interface)
- 毫米波通感基站 (mmWave Sensing Base Station)
- 太赫兹成像通信 (Terahertz Imaging and Communication)
- 智能反射面辅助感知 (RIS-Aided Sensing)
- 无蜂窝大规模MIMO感知 (Cell-Free Massive MIMO Sensing)

### 智能健康与行为感知 (Smart Health and Behavior Sensing)
- 非接触式生命体征监测 (Contactless Vital Sign Monitoring)
    
    * 呼吸频率与呼吸节律感知 (Respiration Rate and Rhythm Sensing)
        * 基于ACF的周期检测：利用ACF峰值位置估计呼吸周期，实现的实时估计
        * 特征提取：峰值显著度 (Prominence)、峰值宽度 (Width)、峰值幅度 (Amplitude)

    * 睡眠状态与长期健康监测 (Sleep Monitoring and Long-Term Health Assessment)
        * 睡眠分期识别 (Sleep Stage Recognition)：区分清醒 (Wake)、快速眼动 (REM)、非快速眼动 (NREM)
        * 利用大幅度身体运动统计量区分清醒与睡眠

    * 非视距与跨房间环境下的感知 (Robust NLOS and Through-the-Wall Sensing)

- 室内行为与主体识别 (Indoor Behavior and Subject Identification)

    * 人体与非人体运动区分 (Human vs. Non-Human Motion Discrimination)

    * 宠物、机器人及家用电器运动识别 (Pet, Robot, and Appliance Motion Recognition)

    * 面向安防与入侵检测的误报抑制 (False Alarm Reduction for Security Sensing)

    * 代表性系统：WiResP（呼吸感知）、WI-MOID（人/非人运动识别）
- 跌倒检测 (Fall Detection)
- 步态与行走模式识别 (Gait and Walking Pattern Recognition)
- 人员计数与流量监控 (People Counting and Flow Monitoring)
    * 静态人群计数 (Static Crowd Counting)：
### 安全与监控 (Security and Surveillance)
- 无感身份认证 (Transparent Identity Authentication)
    * 基于呼吸的门禁/权限控制

## 5. 性能指标与评估 (Performance Indicators and Evaluation)

### 感知性能指标 (Sensing Performance Metrics)
- 探测概率与虚警概率 (Detection Probability and False Alarm Probability)
- 距离分辨率与精度 (Range Resolution and Accuracy)
- 角度分辨率与精度 (Angle Resolution and Accuracy)
- 速度分辨率与精度 (Velocity Resolution and Accuracy)
- 成像分辨率 (Imaging Resolution)
- 微动检测灵敏度 (Micro-Motion Detection Sensitivity)
- 复杂多径与环境变化鲁棒性 (Robustness to Multipath and Environmental Dynamics)
- 非目标运动抑制能力 (Non-Target Motion Suppression Capability)
- 长期监测稳定性 (Stability for Long-Term Monitoring)
- 时间反演聚焦分辨率 (Time Reversal Focusing Resolution)
- NLOS环境定位精度 (Accuracy in NLOS Environments)
- 有效检测距离 (Effective Detection Distance)
- 多目标计数准确率 (Multi-Target Counting Accuracy)
- 身份识别准确率 (Identity Recognition Accuracy)
- 轨迹跟踪鲁棒性 (Tracking Robustness)
- 跨域准确率 (Cross-Domain Accuracy)
- 用户多样性适应 (User Diversity Adaptation)

### 通信性能指标 (Communication Performance Metrics)
- 吞吐量 (Throughput)
- 误码率 (Bit Error Rate, BER)
- 时延 (Latency)
- 频谱效率 (Spectral Efficiency)
- 能量效率 (Energy Efficiency)
- 接收信号强度指示（Received Signal Strength Indicator，RSSI）

### 联合性能指标 (Joint Performance Metrics)
- 感知-通信速率区域 (Sensing-Communication Rate Region)
- 感知-通信误差权衡曲线 (Sensing-Communication Estimation Error Trade-off Curve)
- 资源效率 (Resource Efficiency)
- 服务质量联合保障度 (Joint QoS Guarantee)
- 系统容量 (System Capacity)

### 评估方法与平台 (Evaluation Methods and Platforms)
- 数学仿真平台 (Digital Simulation Platform)
- 硬件在环测试 (Hardware-in-the-Loop Test, HIL)
- 外场试验验证 (Field Trial Verification)
    * 穿墙实验 (Through-the-Wall Experiments)
    * 背景干扰测试 (Background Motion Interference Test)
- 标准化测试流程 (Standardized Test Procedures)
- 性能基准数据集 (Performance Benchmark Datasets)
- 多模态对比验证 (Cross-Modality Validation)
    * 与商用接触式传感器对比 (Comparison with Commercial Bed Sensors/Wearables)
    * 开放数据集验证 (Validation on Open Datasets)
- 复杂环境测试（Complex Environment Testing）
    * 车载环境测试 (In-Car Environment)
    * 实验室/家庭环境对比