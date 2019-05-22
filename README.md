# ad7606-driver-verilog

AD7606端口
=======
AD7606_ctrl.v
-------
**外部信号**<br/>
通过这些信号可以控制整个模块的采样开始<br/>
`clk    ` :	整个FPGA的主时钟，推荐使用50M时钟，时钟再高某些信号的时序可能不满足芯片手册上的要求<br/>
`rst_n  ` : 整个模块的复位信号，低电平复位<br/>
`en	    ` :	控制AD7606的采样率，通过使能信号控制采样过程的开始达到控制AD7606的采样<br/>
`start  ` :	开始采样信号，高电平时代表开始采样；此时如果en为高则开始采样；当该电平为低时，AD7606不会进入采样状态<br/>

**AD7606的控制信号**<br/>
这些端口是直接连接AD7606，会根据外部信号产生相应的变化<br/>
`busy	  ` :	AD7606的busy输出信号，当全通道（8 channel）开始采样之后该信号变高，采样结束该信号拉低<br/>
`fdata  ` :	AD7606的指示信号，在采样结果输出阶段，该信号拉高表明输出的数据为一通道的数据<br/>
`cvtData` : AD7606的并行数据输出端口<br/>
`cs	    ` : AD7606的片选信号，低电平有效<br/>
`rd	    ` :	AD7606的读信号，低电平有效<br/>
`cvtX	  ` :	AD7606的通道采样信号，有cvtA，cvtB<br/>
`range  ` :	AD7606模拟输入的电压范围选择，高电平为10V，低电平为5V<br/>
`phy_rst` : AD7606的硬件复位信号，高电平有效<br/>
`vio    ` : AD7606的IO口电压选择<br/>

**AD7606的读取信号**<br/>
读取这些数据可以更好的对AD7606控制<br/>
`chx	   `:	从AD7606读取回来的通道数据，x有1-8<br/>
`update  `: AD7606数据更新指示信号，为高代表数据已经更新<br/>
`phy_busy`: AD7606忙碌状态信号，为高代表AD7606正在忙碌中<br/>

**常数定义**<br/>
`RANGE_10V`: 控制`range  `,
`WAIT_CNT	`: 调试过程中曾用到，保留,
`T2				`: `cvtX	  `保持低电平的时间次数，芯片手册中`cvtX	  `推荐保持25ns，所以这里使用2（40ns）保证低电平的时间满足时序要求

generate_en.v
----------
**外部信号**<br/>
`clk		`: 整个FPGA的主时钟<br/>
`rst_n	`: 整个模块的复位信号，低电平复位<br/>

**AD7606的输出信号**<br/>
`en_o	  `: 模块的输出使能信号<br/>

**常数定义**<br/>
`INTERVAL_CNT`: 使能信号的周期数设置<br/>

AD7606Demo
==========
ad_top.v
---------
需要例化一个PLL锁相环并命名为pll，并且将三个verilog文件添加进工程，绑定好管脚即可编译通过。该Demo以200K的采样率采集信号并输出。<br/>

**使用的AD7606模块**<br/>
![AD7606](https://raw.githubusercontent.com/maxs-well/ad7606-driver-verilog/master/img/hardware.jpg)<br/>
虽然可能模块不一样，但相关的端口控制等操作应该都是大同小异的，因为都是使用一个AD7606的芯片<br/>

**环境**<br/>
Quartus II 13.0<br/>
Windows 7
