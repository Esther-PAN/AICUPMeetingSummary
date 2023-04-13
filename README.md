# AICUPMeetingSummary
AICUP会议纪要


# 20221208会议纪要

纪要：讨论任务需求，协商团队分工，探讨初步提案。

（注：斜体为命名约定，加粗为值得注意信息。）
## 需求

- 背景：需对元件特别是*针脚*进行异常检测。现有方法检测元件针脚后计算得到针脚*检测位置*，并以此建立坐标系，根据坐标系确定*理想位置*。计算检测位置和理想位置的位置度作为异常评定标准。
- 任务：设计实现系统，根据元件图像计算元件针脚位置度，检测异常元件。
- 公司团队负责硬件部分，学校团队负责软件部分，最后统一对接。
- 学生团队需要交付软件系统满足以下需求：
  - 实现检测算法逻辑
  - 拥有友好的GUI交互功能
  - 默认位置度![](http://latex.codecogs.com/svg.latex?\phi=0.2)（暂定设计为可修改超参数）
- 困难：针脚倾斜带来的图像形状变化和光照条件变化会是检测位置出现误差，并影响后续检测步骤的进行。

## 提案

1. 实现主流方法并进行优化，重点调研实验光照不变性特征在该类方法上的应用，同时研究针脚形变对检测位置的影响（是否可忽略）。
2. 探究基于理想位置蒙版特征的特征匹配方法。（备选）
3. 设计网络接口及图形接口封装上述方法的实现，使公司和学生团队可通过接口调整优化系统整体性能。

---

# AICUP20230210会议记录

内容：

- 讨论下目前的学习和调研进展，安排接下来两周的工作
- 要更新工作进度，讨论工作上遇到的困难与支持
- 自由讨论


## 学习进度汇报

### 内容

#### 江俊

给定一张图片，可对针脚进行基础识别

利用传统方法，先二值化，提取点，确定包围框，再进行筛选

结果：

图片暂缺

**建华老师建议**：多次测量平均或者利用集合模型



需要明确标注图片各个数据的含义：每个区域有三个数据，第一行为x方向的偏差，第二行是y方向的偏差，第三行**是位置度**（欧氏距离2![2](http://latex.codecogs.com/svg.latex?\sqrt{})。)

**问题：如何计算位置度？**
偏差是指与工程图纸(理想距离,坐标系建立：利用平均角度，原点为中心点)的差距。紫色的数据是指明最佳模板。（可以不显示）



#### 唐铭锋

与江俊合作完成基本想法

#### 潘登烨

前端基础知识

#### 周悦

整理各方资料

#### 林宇靖

- 大致了解工作任务：本任务主要是一个**目标检测（or 关键点检测）+位置度判定**问题。目标检测工作要考虑**多角度，多尺度，和多光照条件**问题。角度和尺度可通过硬件固定；光照条件不好控制，需要通过算法解决。在目标或关键点检测准确的情况下，位置度计算只是一个简单的逻辑处理问题，即计算理想值和实际值的偏差。本任务的另一个显著特点是，数据量小，只有几张正常图片。

##### 问题分析

目标检测步骤：预处理-特征提取-候选框提议-候选框评分-目标筛选

关键点检测步骤：预处理-特征提取-关键点预测-**关键点精细定位**-关键点评分-关键点筛选

主流目标检测问题考虑的问题比我们实际的应用场景复杂，除了考虑多尺度，多角度问题外，通常还会利用检测出的结果的相对位置信息对结果进行进一步**矫正**，这不是我们想要的结果（容易影响异常点的定位）。

根据我们任务检测目标单一的特点，我们可以设计简单目标检测算法，**对触脚进行检测，再从检测出来的触脚上精确定位触点位置**。使用主要用色彩变换进行数据增强，结合小幅度角度和尺度数据增强处理小样本问题。

### 总结

### 工作安排

主要负责软件。利用已有的硬件，结合开发的软件，实现项目要求，对于运行速度，检测精度(与实际测量仪的差距)有要求。江俊和唐铭峰合作算法，主要继续完善数据增强、目标检测、实现关键点检测和位置度计算，算法尽量简洁，可自行标注或寻求帮助。潘登烨,负责实现图片上传和检测结果输出的前端页面。林宇靖负责后端和协调。周悦开始收集参考文献等资料，准备汇报文档等相关文书工作。

## 工作进度与所需支持

已完成基本调研学习，本周开始正式开发。

需要申请小型云服务器部署项目供工厂试用，方便反馈调整。

## 自由讨论

目前进展：给定一张图片，可对针脚进行基础识别。具体实现方法利用传统方法，先二值化，提取点，确定包围框，再进行筛选。后期会利用多次测量平均或者利用集合模型缩小误差。进下来将细化标注数据(具体地，每个区域有三个数据，第一行为x方向的偏差，第二行是y方向的偏差，第三行是位置度（欧氏距离$2 \sqrt{} $))，进行初步前端设计。



几点建议:1. 可以提供相机硬件等配置，学生可以主动随时去验证，也方便项目的进展。
2. 最终成果描述清楚，规划好，工厂要的是什么，有一个详细的描述: 比如，输入是什么，以什么方式输入，输出结果是什么，以什么方式输出，说出输出的结果有没有一个判断标准。
3. 建议根据最终成果需求，可以规划一下时间计划。

---

# 20230313会议纪要

## 汇报进度

- [ ] 触点检测
  - [x] 主要代码 *
  - [ ] 性能优化
  - [ ] 其他零件
- [ ] 位置度计算
- [ ] 界面交互
  - [x] 上传与展示
  - [ ] 其他功能
- [ ] 软件对接
  - [ ] API服务器
  - [ ] 前端调用
- [ ] 软硬件对接

总结：

- 本项目触点检测代码具有较大创新性，需要研究尝试，基本已经完成；
- 原界面交互复杂，功能繁多，一比一还原工程量大且并不一定方便使用。

## 算法讨论

模板匹配法步骤：

1. 图像预处理

去掉不必要区域，提高效率，选定模板：面板和角点模板

需要改进的问题：1. 位置信息需要根据每张图片进行变化

1. 检测工件(目标大易于检测)
2. 检测触电
3. 非极大值抑制

​		取得分最高的

问题

-  是否可以取前五个的平均，减少误差？
-  是否存在同一个位置被重复检测的问题？ 现触点模式比较简单，目前不存在重复性问题。
- ***\*算法时间？\****连接相机取像以及检测的时间需要控制在***\*2s以内\****
- 结合blob算法，以目前的区域做中心点，筛选亮斑，选定Pin点，再算偏差值
- 建立坐标系：单模板、多模板、利用边等方法
- 基准点的确立
- 确立是产品还是摄像导致亮斑显示问题？
- 区别正常和异常的图像，用不同方法解决

5.取前108个触电

优化方向：

- 预处理使用直方图均衡化消除光照影响
- 多模板匹配求最佳匹配（求均值可能不是最优解）
  - 需要注意模板一致性，触点中心需处于模板图片的同一像素相对位置中
- 代码效率提升
- 其他工程功能优化
  - 封装
  - 自动输出最差匹配的可能匹配，人工挑选，方便添加为新模板

## 交互讨论

## 讨论结果

交互可简化。
在模板匹配基础上加上blob算法定位触点。（性能高，但对异常光照，和NG图片，鲁棒性偏低）
多模板匹配，可通过添加模板提升鲁棒性。

主要进度：交互界面、触点检测算法

待实现：性能优化、软件对接、软硬件对接

需要配合、支持的部分：其他零件的图片，相机和产品、有难度的图像

交互界面进度：已实现上传和展示，其他功能待开发



---




# 20230331会议纪要


1. 认为工件图纸数据主要用于像素数比例计算，为减小误差，推荐用尺度最大的触点矩阵的长宽作为基准，确定各个结构像素数。@唐铭锋 @坐看云起 

   进度：实现了读入工件针脚设计位置的功能，现在可以在yaml文件中输入基准针脚的位置，一共有多少行多少列，间隔为多少厘米，从而得到所有的针脚（不同的工件可以直接修改yaml文件）。同时可以通过调节x和y方向的ppc将计划位置转化到图上。同时实现了针脚坐标和位置度的计算。

   

2. 考虑到现有硬件和需求，我们可能不需要使用太高级的前端技术栈，需要同潘同学简单了解下pyqt，花一小时部署学习，达到能跑demo的水平就行。@潘登烨 



---



# proposal

### Introduction 

Building a reliable tip inspection method that can capture point of the contact tip under different circumstances is critical yet underexolored. Tip inspection is a huge workload that must be down by machine as there are hundreds of EON contact on Whisper product, both Header and Rec, which must be 100% inspection before ship to customer. Previous work such as Keyence, PPT, VISCO has some drawbacks for EON tip inspection, when contact brightness various they can not capture the correct position of contact tip. Existing solution is using CTI system which can inspect the contact after loading multiple brightness models, however, due to cost, after-sales service and the uncertainty of international trade, we need to find a better and more workable solutions.

### Requirement

The task is to design a system that can detect abnormal components, the main idea includes three steps. First using existing methods to detect pins and calculate its positions from the given images. Then create a coordinate system with this, and calculating the ideal position of the pins. Finally calculate distance between the two positions and use it as a criterion for judging anomalies.

The task is divided into two parts: hardware and software, which are the responsibility of company team and school team respectively, and we will interface with each other at the end.

Requirements for software system delivered by student team are as follows:

- Implement the detection algorithm logic
- Friendly GUI interaction
- Default position degree $\phi=0.2$ (tentatively designed for modifiable  hyperparameters)

### Methods 

Our initial proposal is as follows:

- The most critical issue at present is that contact tilt will change the shape of  image and lighting conditions, resulting in errors in the detection position. So we will focus on the application of experimental light invariance characteristics to mainstream methods, and investigates the effect of pin deformation on detection position.
- The alternative method is exploring feature matching methods based on ideal position masking features.
- Design a web and graphical interface to encapsulates above methods, which allows both company and student team to optimize system overall performance through interface tuning.




