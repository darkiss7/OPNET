# 3 定义和惯例
## 3.1 概述

对于FC-AE-1553，适用以下定义，约定，缩写和首字母缩略词。以下定义适用于本标准。在参考标准中使用的词语应使用该定义。在这里或在参考标准中没有定义的词应具有标准的技术英语含义。有关特殊定义的单词或短语的印刷惯例见3.2。

其他标准的词汇表或主体的一些定义在这里包括，以便于参考。

## 3.2 定义

### 3.2.1 简介

以下段落中的定义适用于本标准。在参考标准中使用的词语应使用该定义。在这里或在参考标准中没有定义的词应具有标准的技术英语含义。

### 3.2.2 命令序列

FC-AE-1553光纤通道序列始终是任何FC-AE-1553交换机的第一个序列，并且始终由网络控制器（NC）发布。命令序列始终将R_CTL字段中的信息类别位设置为0b0110。

注2：某些配置文件可能要求将所有命令序列作为单个帧传输。

### 3.2.3 数据序列

由NT或NC发送的FC-AE-1553光纤通道序列。数据序列始终将R_CTL字段中的信息类别位设置为0100'b。

### 3.2.4 传输

传输由一个或多个相关的不同步序列组成的信息的基本机制，这些序列可能以相同或相反的方向流动。






### 4.4.4 MIL-STD-1553高层协议映射到FC-AE-1553
#### 4.4.4.1 MIL-STD-1553命令字映射到FC-AE-1553
##### 4.4.4.1.1 介绍

每个FC-AE-1553交换机都由网络控制器发送命令序列来启动。FC-AE-1553命令头包含在这个序列中，以及NT-NT传输的NT传输响应的状态序列中。对于这些序列，FC-AE-1553命令序列头由6个字的光纤通道头和由命令序列的6到11个字组成的6个字的FC-AE-1553命令头扩展组成，通常是有效载荷的前六个字。命令序列标题如表7所示。所有FC-AE-1553数据序列都应使用标题字5中的参数作为相对偏移字段。

对于其中网络控制器（或NT-NT传输）的数据交换，网络控制器或传输NT可以在命令序列的有效载荷中从字＃12开始发送多达2048个数据字节。如果这样的交换机有超过2048个数据字节，则NC将在命令序列之后传送一个或多个数据序列。或者，对于NC到NT的传输，如果网络控制器在其命令序列中没有传输任何数据字节，则应在命令序列之后的一个或多个数据序列中传输数据字节。

对于NC-NT的传输，除非在命令序列中设置了Supress状态位，否则NT将以状态进行响应。如果在命令序列中设置了超时状态位，则NT不应答。


对于NT-NC的传输，NT应以状态序列进行响应。此序列可能包含8个字节FC-AE-1553状态报头扩展（参考表9）后的最多2048个数据字节。如果NT有超过2048个数据字节要发送，则应在状态序列之后的一个或多个数据序列中发送剩余的数据字节。

数据序列头应遵循表7中的字0至5的格式。

##### 4.4.4.1.2 R_CTL

R_CTL字段分为两个子字段，路由位和信息类别位。

##### 4.4.4.1.3 路由位

> 高四位？反序？

对于FC-AE-1553，为R_CTL路由字段定义的值。

| 值 | 含义 |
| --- | --- |
| 0b0000 | FC-4设备数据 |
| 0b0010 | 扩展链接服务 |
| 0b1000 | 基本链接服务 |
| 0b1100 | 链路控制 |

##### 4.4.4.1.4 信息类别位

> 表4和表5待整理

R_CTL字段的信息类别位应根据表4和表5中定义的类别进行设置。对于FC-AE-1553，所有信息单元均为所有种类的序列定义。

##### 4.4.4.1.5 目的标识符 (D_ID)

D_ID字段包含NT地址，多播别名地址或已知地址。每个NT和NC都应识别以本地N_Port地址或广播地址0xFFFFFF或所有别名（隐式或显式预定义的多播别名组）为D_ID的序列。

##### 4.4.4.1.6 广播和多播传输

FC-AE-1553支持多种选项来实现广播和组播。对于多播或广播传输，如果FC-AE-1553命令头的状态抑制位(Suppress Status bit)是0，那么所有接收的NT应该用一个单独的序列进行状态响应。但是，如果状态抑制是1，那么所有接收的NT都不会进行状态响应。

广播和多播的情况如下：

1. 所有NT和NC都识别广播地址0xFFFFFF。

2. 仲裁环中的NC和NT均须识别广播复制的仲裁环物理地址0xFF。

3. 为了支持桥接到FC-AE-1553网络的MIL-STD-1553总线中的RT广播功能，NC须能识别命令序列中子地址字段的9-5位的广播地址0x1F。

4. NT须能对多播地址进行响应。

5. NT须支持可选复制。

##### 4.4.4.1.7 CS_CTL

CS_CTL字段表示帧的优先级。该优先级是一个7位的数值，在命令帧头的字1的30-24位。优先级的使用是可选的，在字2中第17位，对优先级的使用进行设置，1表示使用优先级，0表示不使用优先级。使用优先级的情况下，帧的路由顺序须按优先级进行，高优先级优先。优先级有127-0共128个级别，并逐级降低，127级最高，0级最低。如果只实现两个优先级，则1级为高，0级为低。在优先级未使能的情况下，CS_CTL字段为0。

##### 4.4.4.1.8 源标识符 (S_ID)

S_ID字段是源Nx_Port的地址标识。每个NT或NC都有各自不同的 S_ID。

##### 4.4.4.1.9 类型字段 (TYPE)

TYPE字段用于区分不同的上层协议，对于FC-AE-1553的序列，该字段为0x48。

##### 4.4.4.1.10 参数字段 (Parameter)

对于FC-AE-1553的序列，该字段指明相对偏移量。在命令序列和状态序列中，该字段的值为0x00000000。

注7：对于FC-AE-1553，其他帧头部字段的操作在标准附录B中进行了描述。

##### 4.4.4.1.11 NT突发请求 (NT Burst Size Request)

NT Burst Size Request在进行NC-NT模式的数据传输时，在NC命令序列中对该位进行设置，或者在NT-NT模式的数据传输时，在传输NT的命令序列中对该位进行设置。当NT Burst Size Request设置为1时，则Delayed NT Burst Size Request必须为0。若NC或NT发送的命令序列中 NT Burst Size Request 设置为 1，则在其命令序列中不能包含任何的数据，并且不能在发送命令序列后立即发送数据序列。若NT Burst Size Request设置为1，NC 即等待来自接收 NT 的状态响应。若在规定时间内收到此状态响应，且该状态响应的“忙”比特为 0，Burst SizeAcknowledge 比特为 1，则表示状态响应的字 7 为 NC 能接收的一个数据序列的最大负载长度。

If NT Burst Size Request is set, the NC shall wait for at least NC_CD_BURST_TOV until reception of a Status Sequence from the receiving NT before making a determination of a “No Response” condition. If the Status Sequence’s Busy bit has a value of ‘0’b and Burst Size Acknowledge has a value of ‘1’b, this indicates that word 7 of the Status Sequence represents the maximum number of payload bytes that the NT can receive within a single Data Sequence. If this value is less than the value of the Command Sequence Byte Count field, then the NC or transmitting NT will need to segment its overall payload into multiple Data Sequences. In this case, some time following the reception of the first Data Sequence from the NC, the NT shall respond with a second Status Sequence with Burst Size Acknowledge = ‘1’b. This is followed by the NC’s (or transmitting NT’s) transmission of a second Data Sequence. This process is repeated until the NC or transmitting NT has sent the total number of data bytes specified in its Command Sequence.

If at any time the receiving NT cannot receive any additional data bytes, it shall respond with a value of ‘1’b for the Busy bit and a value of ‘0’b for Burst Size Acknowledge, thereby terminating the Exchange.

Following valid reception of the last Data Sequence, if the Command Sequence Suppress Status bit was ‘0’b, the receiving NT shall respond with its final Status Sequence. In this Status Sequence, Burst Size Acknowledge shall have a value of ‘0’b.


