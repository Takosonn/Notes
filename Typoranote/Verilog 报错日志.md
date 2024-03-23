# Quartus报错



## quartus II编译报错：Error: Current license file does not support the XXX device

破解问题，未正确破解，重新破解；

最新更新，13.0破解后如果代码出现模块定义错误，很容易让破解失效，真垃圾



## Error (12007): Top-level design entity "xxx" is undefined

1、顶层模块的**modulename**名没有和**quartus_prj中的工程名**同名

2、命名与quartus库文件里某个名字重复

解决方法：改名



## Error：Verilog  near text "negedge";  expecting an operand

negedge使用方法错误，检查是否混用；



## Error  : object "led" on left-hand side of assignment must have a variable data type

说明变量是wire型出现在了逻辑设计中，应当改成reg型



## Error : Verilog HDL syntax error at counter_tb.v(20) near text "#";  expecting ";"

检查端口书写位置，检查符号格式；



## Error ：object “clk_out” on left-hand side of assignment must have a net type

检查assign后面变量的类型必须是wire；

**等号左右都是wire**；



## Error : Verilog HDL syntax error at coke_FSM.v(67) near text "b";  expecting ")"

是不是把**2'b01**写成了2b'01















# ModelSim报错



## ModelSim联合仿真时出现# Error loading design

首先保证安装一定没问题，其他实例可以顺利编译。

检查测试文件的实例化是否与设计文件中的模块是否匹配，是否给了实例化一个名字

检查模块名，参数名，参数端口（参数顺序，仿真文件中实例化参数端口设置等）

重新加载一遍，在修改完仿真文件后，重新通过settings将其加载至testbench中，有些情况可解决

或者重新建project，然后compile



## ModelSim错误：syntax error, unexpected “IDENTIFIER“, expecting “.*“ or ‘.‘

先进行compile检查语法错误，比如实例化中关联信号前是否漏了一点 **.**

检查所有的 : ; , .是否是英文符号，是否正确使用；



## ModelSim错误：Instantiation of ‘***‘ failed. The design unit was not found

在setting simulation中**同时**添加tb代码和原模块代码；
