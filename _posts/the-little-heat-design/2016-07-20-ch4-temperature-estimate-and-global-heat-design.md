---
layout:     post
title:      散热设计4-动态温升估算与全局热设计
category:   the-little-heat-design
---

关于热功率损耗的模型，请大家根据自己的情况参考英飞凌的官方热设计文档《Infineon.IPOSIM7.5》，我这里结合这个文档给出自己的结论与分析。

### 1.动态温升估算

根据英飞凌文档中给出的估算公式是平均功率，而多数IGBT的应用场合是方波损耗功率或者正弦半波损耗功率，不同功率损耗波形引起的温升变化如图1所示。

![](/images/the-little-heat-design/Cover_Heat_S0_E6.png)

##### **图1.不同频率的温升变化**

方波的温升可以直接估算作图，然后求出方波周期与不同时间常数比下，热阻降低达到的比例因数，得到图2所示，由可以看出方波热功率周期很小时热阻趋近于0.5倍比例因子，较大时趋近于1倍因子。**方波的最大功率是平均功率的二倍。**

![](/images/the-little-heat-design/Cover_Heat_S4_E1.png)

##### **图2.矩形热冲击的估算因数(T0为热功率周期，ti为热网络时间常数)**

正弦半波温升估算没有理论公式，直接在Matlab仿真画出对应的热阻降低的比例因数曲线如图3所示，方波周期较小时趋于1/π倍。**正弦半波的最大功率是平均功率的**π**倍。**

![](/images/the-little-heat-design/Cover_Heat_S4_E2.png)

##### **图3.正弦半波热冲击的估算因数(T0为热功率周期，ti为热网络时间常数)**

得到热阻比例因子之后，用对应的热阻乘以比例因子，就是最终的热冲击对应的有效热阻。

### **2.全局温升热设计**

1. 初步选定功率芯片型号，根据工况对功率器件损耗功率估计，同时估计功率器件核心对基板最高温升。
2. 确定散热器热阻对风流量的关系，实测或者从供应商那里获取。
3. 风扇挡板宽度对机网侧流量比的对比，以及不同挡条宽度与不同风扇配比的工作点选定。见第3章。
4. 根据变频器工作的海拔与环境温度，最终确定工作环境的空气密度，同时将此海拔下的风扇体积流量转化为0海拔下测试温度下的等效体积流量。
5. 将体积流量转化为散热器热阻（包含硅脂），与损耗功率相乘即为散热器对空气温升，同时加上功率器件核心对基板的温升，即最后求得的功率器件核心对环境温度的温升。
6. 如果最高超界，首先跳到3步，优化热平衡设计。如果不行的话，只能重新选择功率芯片。

分析中需要考虑的因素如下：

* 变频机型——功率大小
* 工作条件——功率因数，工作电压， 决定工作电流大小
* 变频频率——工作频率范围
* 环境条件——海拔，40度环境温度，决定空气密度与核心温度
* IGBT条件——IGBT型号，开通电阻和关断电阻，核心温度，工作电压与电流，这些决定损耗功率与IGBT内部温升。
* 散热条件—— 风扇与风道曲线，挡条宽度，散热器，决定IGBT外部温升



