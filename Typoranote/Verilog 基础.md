# Verilog HDL

[TOC]



## 1.历史

 硬件描述语言 Hardware Description Language，代码脚本的方式，比图形化的效率高；

VHDL：欧洲、中国高校等；Verilog：美国；

Verilog语法比较简单，使用随意，VHDL严谨，语法要求高；

Verilog HDL最初在1983年被一家公司开发；

1992年选为标准；



## 2.综合

Verilog是硬件描述语言，描述，使用综合器对Verilog代码进行解释后变成电路；

QUARTUS, ISE, VIVADO等都是综合器；

可综合设计：可以变成电路的；

综合就是把编写的rtl代码转换成对应的电路；

综合要做的事情：编译rtl代码，从库里选择用到的门器件，把这些期间按照逻辑搭建成门电路；

设计的时候要确保所写的代码是可以综合的；



## 3.仿真

一般情况下可以认为没经过仿真的代码都是有bug的；

通过输入激励给设计文件看输出，看是否正确，模拟最真实的情况；

Verilog定义的语法大部分都是用来仿真的；

while、initial、force、fork、#n仅用于仿真，严禁在设计中使用；

所有的信号类型定义都用reg和wire；

除法和求余运算的电路面积较大，不建议使用；



## 4.模块结构

模块套模块，自上而下展开；

以module开始endmodule结束；

端口（input，output，inout）、参数（parameter）；

input [ 信号位宽-1 : 0 ];

对一个系统的顶层模块采用结构化设计，即顶层分别调用了各个功能模块；

模块例化：一个模块能在另一个中被使用；



## 5.信号类型  wire  reg

连线类型wire，综合到电路就是一根导线；

input	wire	[7:0]	a; 	//该wire型信号的位宽为8位；

寄存器类型reg，用于逻辑设计；

output	reg	[7:0]	b；

没有定义信号类型默认为wire，若没有定义范围，默认为1位；

把wire定义的变量用在有逻辑性的语句中就会出现综合错误：

**wire**对应于连续赋值，如**assign**，**reg**对应于过程赋值，如**always，initial**；

**输入信号**一般来说你是不知道上一级是寄存器输出还是组合逻辑输出，那么对于本级来说就是一根导线，也就是**wire型**;

一般的，整个设计的外部输出（即最顶层模块的输出），要求是**寄存器**输出，较稳定、扇出能力也较好；

wire默认形态是高阻态z，reg默认是不定态x；



## 6.组合逻辑

assign 是**连续赋值**语句，将一个变量的值不间断地赋给另一个变量；

assign a = b (?) c；

always 是条件循环语句，是一直、总是的意思；

always @（敏感事件）begin

​	程序语句

end

always	@(a or b or d)begin

​	if(sel == 0)

​		c = a + b;

​	else

​		c = a + d;

​	end

当a、b、c任一发生变换时就执行一次，sel变化不执行，因为不是敏感事件；

可以用 * 来代替程序语句中的所有信号：always   @(*)begin

逻辑组合，信号变化结果立即变化：always   @(posedge clk)begin

always @(A or B)begin

时序逻辑：信号边沿触发，即信号上升沿或下降沿才变化的always，此时信号clk是时钟；



## 7.数字进制

<位宽> ‘ <基数><数值>

如  4‘d11 = 4’b1011

位宽就是二进制位数；

基数表示 进制；

若基数是b，数值可以是0，1，x，X，z，Z；

一切的根本是二进制，在FPGA中，小数、有符号数都可以对应二进制；



## 8.不定态	x

x和z，如1’bx ，1‘bz，没有实际的电平对应这两者；

用来表示设计者的意图，告诉仿真器和综合器如何解释这段代码

X态Z态称为不定态，常用于无关项，无论0和1；

在设计中尽量解释清楚，在电路层面是没有不定态的；

如果在仿真中出现不定态，设计者需要分析是否需要关心它的电平是多少；；

常用来节省代码量；



## 9.高阻态	z

表示行为，表示不驱动这个信号（不给0也不给1）；

assign data	 = (wr_en == 1)?wr_data:1'bz;

assign rd_data = data;

三态门，使能 wr_en、写数据wr_data、读数据rd_data以及输出data；

当使能有效时wr_data = data，使能无效时不驱动；



## 10.算数运算符  +  -

always@(*)begin

​		c = a+b;    //加法器

end

always@(*)begin

​		c = a-b;    //减法器

end

乘法	*，就是多个加法运算；

除法	/，求余	%，包含加法、乘法、减法、移位等多个运算，要尽量避免；

如 a/2 就是a右移一位，a/4就是右移两位，数尽量为2^n^；



## 11.信号位宽

最终取决于等号左边信号的位宽，也就是被赋值的一方的位宽；

比方说a的位宽为3，则010001运算结果保留低三位001；

运算的最终结果，低位保存，高位丢弃；



## 12.补码

当出现进位的时候结果要扩展一个位宽才是正确的；

补码转换：增加符号位，正数保持不变，负数符号位保持不变，数值取反+1；

在FPGA中所有的数据都是以补码的方式保存的；

在计算前，要根据常识，预计结果的最大最小值；

要将加数、减数的位宽扩展为一致；



## 13.逻辑运算符 &&  || 

逻辑与：C = A && B，逻辑或C = A || B;，非！；

1位逻辑与：A 和 B 都为1时C为1；

多位逻辑与：A和B都不为0时C为1；

1位逻辑或：A B其中一个非0时C为1；

多位逻辑或：A和B其中一个非0时C为1

逻辑运算可看作一个整体，最终的结果只有0或1；

多位逻辑操作数可看作整体，每一位都是0才是0值；

逻辑运算符低于算术运算符（<、>），！的优先级大于另外两个；

在工作中要多用括号来区分优先级；



## 14.按位运算符 ~ & | ^ ^~

~（一元非）：非门；

& 与；| 或；^ 异或；^~ 异或非；

表示对信号进行每位的操作，比如

reg[3:0]	A,C;

assign	C = |A;

上述代码等价于 C = A[3] | A[2] | A[1] | A[0];

if A = 4'b0101, C的结果就是1；

 按位或就是按位取反；

又比如

reg[3:0]	A,B,C;

assgin	C = A | B;

上述代码等价于 C[0] = A[0] + B[0],  C[1] = A[1] + B[1],  C[2] = A[2] + B[2];

if A = 4'b0110, B = 4'b1010, then C = 4'b1110;

 

## 15.关系运算符  <, >, >=

<, >, >=, ==, <=, !=;

结果为真或者假，即1或0；

在逻辑相对与逻辑不等中只要其中一个为x，结果为x；

比如

a = 4'b11x0 ；b = 4'b11x0;

那么，a == b 这以运算的结果不确定，就为x；



## 16.移位运算符   <<   >>

<< 左移， >> 右移；

移位操作实际上只是选择哪根线相连；

左/右移属于逻辑移位，根据位宽储存结果，需要用0补多出来的空位；

左/右移操作时不消耗逻辑资源的；

 比如

wire	[5:0]	a;

assign a = 4'b0111 >> 2;

代码运行的结果a = 6'b000111;

可以利用左移运算来实现2的n次方的乘法

如 a*4 等价于 a<<2;



## 17.条件运算符（   ？ ： ）

比如case(S)，if-else；

条件运算符（   ？ ： ），带有三个操作数即三目运算符

格式一般为：

条件？满足的表达：不满足的表达；

condition_express?	true_express	:	false_express;

作用类似于多路选择器。同时也可以用if else替代；

条件运算符可嵌套使用，每个真/假表达式本身都可以是一个条件表达式；



## 18.if 语句

if(sum<75)	begin

​	if(min>65)

​		a = b;

​	else

​		a = c

end

begin和end相当于c语言中大括号的意义;



## 19.case 语句

case(case_express)

​	case_item_express	:	procedural_statement

[default	:	procedural_statement]

endcase

case语句的default必须写，防止产生锁存器；

在运行速度比较关键的项目中，case的效果更好；



## 20.选择语句 +:  -:

vect[a +: b]或vector[a	-:	b];

a为起始位置，加减号代表着升序后者降序，b表示升降的宽度；

比如:vect[7	+:	3];

起始位置为7，比7大的方向的三个数，等价于vect[7	:	9]；

例如

data[15 - 8*cnt	-:	8]；

当cnt == 0时，data[15	:	8],当cnt == 1时，data[7	:	0];



## 21.拼接运算符{	}

即{	}，例如

reg[3	:	0]	A;

reg[2	:	0]	B;

always@(*)begin

​	B = {A[2],A[3],A[0]}；

end

结果为：A[0] == B[0]，A[3] == B[1]，A[2] == B[2]；

拼接是不消耗资源的；

默认顺序拼接为54321，要反向拼接：data = {seg[0],seg[1],seg[2],seg[3],seg[4],seg[5],seg[6],seg[7],sel};



## 22.时序逻辑

有两种，同步复位和异步复位；

同步复位不是立即有效，而是在时钟上升沿才有效；

always@(posedge clk)	begin

end

异步复位的时序逻辑立即有效，与时钟无关；

always@(posedge clk or negedge ret_n)	begin

end

在时钟是不稳定的时候使用异步复位；



## 23.D 触发器

FPGA只用D触发器；

可将其视为一个芯片，有四个管脚；

时钟clk、复位rst、信号d、输出q；

当rst为0，q输出低；当rst为1，观察clk，当clk上升沿，将此刻的d赋给q；

always	@(posedge clk or negedge rst_n)	begin

​	if(rst_n == 1'b0)begin

​		q <= 0;

​	end

​	else begin

​		q <= d;

​	end

end

先有时钟上升沿，再将d赋值给q，对于硬件来说有先后；

大部分数字系统都是同步电路，用同一个时钟；



## 24.时钟

高电平占比是占空比，设计师更关心时钟频率；

频率1Hz就是1秒一次，1kHz就是一毫秒一次，1mHZ就是一微秒一次；

一般的FPGA时钟频率一般不超过150M；

时钟是FPGA中最重要的信号，其他所有信号在时钟上升沿统一变化；

时钟就是最高命令，要判断电路能否在周期内完成任务；

不要把信号置于时序敏感列表中；



## 25.阻塞/非阻塞赋值

阻塞赋值 = ，非阻塞赋值 <= ；

阻塞赋值：**顺序**执行，在一个多行赋值语句，先执行当前行再下一行；

对应的电路往往与触发沿没有关系，只与输入电平的变化有关系；

非阻塞赋值：多行**同时**赋值；

对应的电路往往与触发沿有关系，只有在**触发沿**的时刻才进行非阻塞赋值；

非阻塞操作只能用于**寄存器**类型变量进行赋值，只能用于**initial**和**always**块中；

组合逻辑都用阻塞赋值，时序逻辑都用非阻塞赋值；

硬件描述语言应该用最简的方法描述；



## 26.模块

1、模块在语言形式上是以关键词**module**开始，以关键词**endmodule**结束的一段程序。

2、模块的实际意义是代表**硬件电路上的逻辑实体**。

3、每个模块都实现特定的功能。

4、模块的描述方式有**行为建模**和**结构建模**之分。

5、模块之间是**并行**运行的。

6、模块是分层的，高层模块通过调用、连接低层模块的**实例**来实现复杂的功能。

7、各模块连接完成整个系统需要一个**顶层模块**（top-module）。

 调用模块实例的一般形式为：

<模块名><参数列表><实例名>（<端口列表>）;

module top(A,B,CIN,S,COUT);

...

adder ADDER(A,B,CIN,S,COUT);//这里采用位置关联

...

endmodule

如果采用名称关联

adder ADDER(

.a(A),

.b(B),

.cin(CIN),

.cout(COUT)

);

例化中一定会有一个例化名，比如上面的ADDER,就代表着对adder模块的调用；

这个例化名可以自己定，没有什么特别要求；



