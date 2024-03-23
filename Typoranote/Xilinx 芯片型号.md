# 芯片命名规则

Xilinx 公司的芯片有一整套命名规则，可以通过相关文档查阅。

Xilinx 提供了很多关于 Device 的用户手册，很多会在文档的开始部分对命名规则及其含义进行讲解，如

UG-112：Device [Package](https://so.csdn.net/so/search?q=Package&spm=1001.2101.3001.7020) User Guide

UG-116：Device Reliability Report

外，针对某指定芯片，可查找所属系列的 DataSheet

例如，针对 Xilinx Artix-7 系列的 XC7A100T-2FGG484l 芯片，可以查看

概括性讲解 7-系列 [FPGA](https://so.csdn.net/so/search?q=FPGA&spm=1001.2101.3001.7020) 选型的说明文件：All Programmable 7 Series Product Selection Guide

![img](https://img-service.csdnimg.cn/img_convert/e7cb87a9f4ce03194b0a753692e25ae8.png)

可知，该芯片属 Artix-7 系列，速度等级为 2 级，封装形式为 FG(Wire-bond)，封装 IO 引脚共 484 个，温度等级为工业级

也可以查看所属子系列的数据手册：

DS-197：XA Artix-7 FPGAs Data Sheet: Overview

![img](https://img-service.csdnimg.cn/img_convert/dca2a6be60e1d65ddd24d1823d92f0cf.png)

对大部分芯片，还可以查看专门针对该芯片的数据手册，并可直接对照 FPGA 实物的上表面查看所代表的含义：

EN-230：Artix-7 XC7A100T and XC7A200T FPGA CES and CES9910 Errata

![img](https://img-service.csdnimg.cn/img_convert/f108b9173352b2c6120ad407bad171b7.png)

查看 Xilinx 官方文档时，推荐使用专用软件 Xilinx DocNav。在这个软件里可以方便的下载所需的文档，并可方便地进行文档的版本更新和新旧版本的管理。必要时还能开启代理，还可以进行文档共享

![img](https://img-service.csdnimg.cn/img_convert/e383712b986b82e07ba31c2aeedce765.png)

Xilinx 将所提供的文档进行了详细的分类，共分为以下几类。

![img](https://img-service.csdnimg.cn/img_convert/f1a1af07c1c48471114584a8943ba88d.png)

各文档编号的开头两个字母，即其所属分类的单词首字母组合，例如上面提到的 DS-197，即属于 Data Sheets。

![img](https://img-service.csdnimg.cn/img_convert/48199704f8e61ee5cbb7e6bf23570486.png)