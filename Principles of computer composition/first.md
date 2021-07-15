# 概述
## tips：
1. 低电平和高电平表示0、1，CPU，内存的小金属片就是接受和发送电平的引脚，板子上的线就是电路
2. ENIAC 第一台电子数字计算机，冯诺依曼，电子管为逻辑元件，程序员靠纸带写程序
3. 晶体管时代，出现fortran等语言，OS雏形初现
4. 小规模集成电路，出现分时操作系统
5. 大规模集成电路，微处理器等出现，PC萌芽
6. 两极分化：微型计算机：微型化、网络化、高性能、多用途；巨型机：超高速、并行处理、智能化
7. 设备：
## 计算机系统
计算机系统=硬件加软件，软件分为`系统软件`和`应用软件`，系统软件包括OS、DBMS、标准程序库、网络软件、语言处理程序、服务程序 
## 计算机硬件的基本组成
### 早期冯诺依曼机
提出存储程序：将指令以二进制代码的形式事先输入主存储器，按其在存储器中的首地址执行程序的第一条指令，之后按程序规定的顺序执行其他指令，直至结束  第一台是EDVAC  
![5](https://github.com/heeler-deer/Molder/blob/main/png/5.png)
现代计算机：
![6](https://github.com/heeler-deer/Molder/blob/main/png/6.png)
特点：
1. 由五大部件组成  ![7](https://github.com/heeler-deer/Molder/blob/main/png/7.png)
2. 指令和数据以同等地位存于存储器，可以按地址寻访
3. 指令和数据以二进制表示
4. 指令由操作码和地址码组成
5. 存储程序
6. 以`运算器`为中心  
而现代计算机以存储器为中心，CPU=运算器+控制器    
## 各种硬件
### 主存储器
![8](https://github.com/heeler-deer/Molder/blob/main/png/8.png)
MAR和MDR  
memory address register 存储`地址`寄存器   
memory data register 存储`数据`寄存器  
现代计算机MAR、MDR属于CPU
数据在存储体内按地址存储，包含：  
1. 存储单元：每个存储单元存放一串二进制代码
2. 存储字：存储单元中二进制代码的组合
3. 存储字长：存储单元中二进制代码的位数   
4. 存储元：存储二进制的电子元件   
### 运算器
![9](https://github.com/heeler-deer/Molder/blob/main/png/9.png)
ACC：`累加器`，存放操作数或者运算结果     
MQ：乘商寄存器，在`乘除`时存放操作数或者运算结果    
X：`通用`的操作数寄存器，存放操作数    
ALU：算术逻辑单元，`实现`算术、逻辑运算  
### 控制器
![10](https://github.com/heeler-deer/Molder/blob/main/png/10.png)
CU：控制单元，分析指令并`给出`控制信号   
IR：指令寄存器，`存放`当前的指令   
PC：程序计数器，存放`下一条`指令地址，自动加一  
![11](https://github.com/heeler-deer/Molder/blob/main/png/11.png) 
## 多级层次结构
看图：  ![12](https://github.com/heeler-deer/Molder/blob/main/png/12.png)
## 存储器以及CPU以及系统整体的性能指标
MAR反映最多支持多少个存储单元，MDR表示每个存储单元的大小，若MAR为32位，MDR为8位，则总容量为：   
2^32*8 bit=4GB      
CPU主频：CPU内数字脉冲信号的频率，为操作带来“节奏”       
CPI：执行一条指令所需的时钟周期的次数    
IPS：每秒执行的指令的条数      
FLOPS：每秒执行浮点运算的次数    
数据通路带宽：数据总线一次所能并行传送信息的位数   
吞吐量：系统在单位时间内处理请求的数量   
响应时间：指用户发送请求到系统对请求做出响应并获得结果所需的等待时间   
基准程序：跑分软件   
