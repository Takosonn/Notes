## [FPGA速度等级](https://www.cnblogs.com/zhongguo135/p/9149139.html)

最初接触speed grade这个概念时，很是为Altera的-6、-7、-8速度等级逆向排序的方法困惑过一段时间。不很严密地说，“序号越低，速度等级越高”这是[Altera FPGA的排序方法](https://www.cnblogs.com/zhongguo135/p/9149139.html#speed)，“序号越高，速度等级也越高”这是[Xilinx FPGA的排序方法](http://forums.xilinx.com/xlnx/board/message?board.id=CPLD&thread.id=157)。 

前些天在博客里看到[huge](http://blog.ednchina.com/huge/)朋友的一篇[FPGA speed grade](http://blog.ednchina.com/huge/146320/Message.aspx)，激发了我进一步探索上述问题的动力。通过在网络上搜索，逐步得到了以下一些认识，但是我仍然有两点困惑：1. 是什么因素导致了同一批芯片的性能差异；2. 如果因素已知，为什么不人为控制这些因素，提高高速芯片的产率，达到既增加芯片厂商的利润又降低高速芯片价格的目的呢。 

1. 芯片的速度等级决定于芯片内部的门延时和线延时，[这两个因素又决定于晶体管的长度L和容值C]，这两个数值的差异最终决定于芯片的生产工艺。怎样的工艺导致了这一差异，我还没找到答案。 

2. 在芯片生产过程中，有一个阶段叫做[speed binning](http://www.ocforums.com/archive/index.php/t-388807.html)。就是采用一定的方法、按照一组标准对生产出来的芯片进行筛选和分类，进而划分不同的速度等级。[“测试和封装”应该就包含这一过程](http://en.wikipedia.org/wiki/Product_binning)。 

  关于speed binning的技术有很多专利： [riple](http://blog.ednchina.com/riple/) 

  [Integrated circuit with adaptive speed binning](http://www.freshpatents.com/Integrated-circuit-with-adaptive-speed-binning-dt20061026ptan20060238230.php)

  [Semiconductor device with speed binning test circuit and test method thereof](http://www.freepatentsonline.com/7260754.html)

  [Binning for semi-custom ASICs](http://www.freepatentsonline.com/7241635.html)

  [Method and apparatus for determining wafer quality profiles](http://www.freepatentsonline.com/6868353.html)

  [Method of sorting dice by speed during die bond assembly and packaging to customer order](http://www.freepatentsonline.com/6984533.html)

  [Method for prioritizing production lots based on grade estimates and output requirements](http://www.freepatentsonline.com/6699727.html)

3. 速度等级的标定[不仅仅取决于芯片本身的品质](http://www.ocforums.com/archive/index.php/t-388807.html)，还与[芯片的市场定位有很大关系](http://www.hardforum.com/showthread.php?t=1296166)，返修概率和成本也是因素之一。 

4. 芯片的等级可以在测试后加以具体调整和改善，[在存储器芯片的生产中这一技术应用很广泛](http://www.soccentral.com/results.asp?CatID=488&EntryID=19868)。 

5. 芯片生产的过程是充满各种变数的，生产过程可以得到控制，但是控制不可能精确到一个分子、一个原子，产品质量只能是一个统计目标。同一个wafer上的芯片会有差异，即使是同一芯片的不同部分也是有差异的。速度等级是一个统计数字，反映了一批芯片的某些共同特性，不代表个别芯片的质量。而且由于某些芯片的测试是抽样进行的，也不排除个别芯片的个别性能会低于标定的速度等级。不过，据说FPGA的测试是极严格的，很可能我们拿到手的芯片个个都经过了详尽的测试。这也是FPGA芯片价格高于普通芯片的原因。 

6. 同一等级的芯片中的绝大多数，其性能应该高于该速度等级的划分标准。这也是为什么在FPGA设计中，有少许时序分析违规的设计下载到芯片中仍然能够正常运行的原因（时序分析采用的模型参数是芯片的统计参数，是最保守也是最安全的）。不过，由于同一等级的芯片仍然存在性能差异，存在时序违规但是单次测试成功的FPGA设计不能确保在量产时不在个别芯片上出现问题（出了问题就要返修或现场调查，成本一下子就上去了）。所以，还是要把时序收敛了才能放心量产，这就是工程标准对产品质量的保证。 

7. 概率和统计学源于工程实践，对工程实践又起到了巨大的指导作用。工程实践中的标准都是前人经验教训的积累，是人类社会的宝贵精神财富。 

8. 现实世界是模拟的，不是数字的。在考察现实问题时，我们这些数字工程师和软件工程师应该抛弃“一是一、〇是〇”的观念，用连续的眼光看待这个连续变化的真实世界。 

9. 芯片生产过程中的不确定性导致了芯片的性能差异，这一差异影响了芯片的价格，价格和性能的折中又影响了我们这些FPGA设计工程师在器件选型、设计方法上的决策，我们生产的产品的性价比决定了产品的销售，产品的销量又决定了芯片的采购量，采购量又影响了芯片的采购价格...。原子、分子级别上的差异，就这样一级一级地传递和放大。人类社会就是这样环环相扣，互相制约的。

![img](https://img-service.csdnimg.cn/img_convert/e7cb87a9f4ce03194b0a753692e25ae8.png)