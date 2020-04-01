# Task2 数据分析：EDA-数据探索性分析
## EDA目标
  - 熟悉数据集，对数据集进行验证来确定所获得数据集可以用于接下来的机器学习或者深度学习使用。
  - 了解变量间的相互关系以及变量与预测值之间的存在关系。
  - 进行数据处理以及特征工程，使数据集的结构和特征集让接下来的预测问题更加可靠。

## 过程笔记
  - 绘制Null直方图的作用
    - 检查训练集里面有哪些列是含有Null值，并统计了Null值的数量
    - 如果数据Null值相对较大，可以考虑去除此列
    - 如果Null值较为合理，可以使用lgb树模型优化
  - 绘制缺省图的作用
    - 发现缺省列
    - 确定缺省程度
    - 把缺省程度异常的列最nan处理
  - 处理异常列
    - 数据严重倾斜，对预测作用不大，降低维度
  - 预测值的分布
    - 查看预测值分布频数
    - price不符合正态分布，在回归之前，要做变换(log)
  - 特征分析
    - 类别特征：unique分布、箱型图、小提琴图、柱状图、频数可视化
    - 数字特征：偏度、峰值、unique分布
    - 相关性分析（难点）


## 函数总结
  - `Train_data.head().append(Train_data.tail())`查看数据集
  - `Train_data.info()` 查看数据类型
  - `Train_data.describe()` 查看统计量
  - `Train_data.isnull().sum()` 查看null值情况
  - `msno.matrix(Train_data.sample(250))` 查看缺省情况
  - `msno.bar(Test_data.sample(1000))` 查看缺省情况(直方图)
  - `pandas_profiling.ProfileReport(Train_data)` 生成报告
  
  
  

# Task3 数据分析：特征工程

## 特征工程目标
  - 最大限度地从原始数据中提取特征以供算法和模型使用
  - 核心：特征处理

## 数据预处理存在问题及解决办法
  - 特征规格不同 -> 无量纲化（标准化和区间放缩）
  - 信息冗余 -> 二值化
  - 定性特征不能直接使用 -> 使用**哑编码**的方式将定性特征转换为定量特征
  - 存在缺失值 -> 补充缺失值
  - 信息利用率低（暂时不太懂）

### 标准化
  - 依照特征矩阵的列处理数据，其通过求z-score的方法，将样本的特征值转换到同一量纲

### 归一化
  - 依照特征矩阵的行处理数据，目的在于样本向量在点乘运算或其他核函数计算相似性时，拥有统一的标准（单位化）

### 二值化
  - 核心：设定阈值

### 独热编码
  - **[原理理解](https://blog.csdn.net/a2099948768/article/details/82384616)**

### 数据变换
  - 基于多项式的、基于指数函数的、基于对数函数

## 特征选择
  - 特征是否发散 -> 方差法（方差为0，表明该特征对数据无影响）
  - 特征与目标是否相关 -> 相关程度决定影响程度

### 过滤法
  - 原理：按照发散性或者相关性对各个特征进行评分，设定阈值或者待选择阈值的个数，选择特征。
  - 种类：方差选择法、相关系数法、卡方检验、互信息法

### 包装法
  - 根据目标函数（通常是预测效果评分），每次选择若干特征，或者排除若干特征。
  - 种类：递归特征消除法

### 集成法
  - 先使用某些机器学习的算法和模型进行训练，得到各个特征的权值系数，根据系数从大到小选择特征
  - 基于惩罚项的特征选择法、基于树模型的特征选择法

## 降维
  - 主成分分析法（PCA）：为了让映射后的样本具有最大的发散性 -> 无监督
  - 线性判别分析（LDA）：为了让映射后的样本有最好的分类性能 -> 有监督
  
# Task4 数据分析：模型选择与调参

## 监督学习
  - 概念：必须明确目标变量的值（类别），以便算法能够发现特征与目标变量之间的关系
  - 分类：将实例数据划分到合适的类别中（离散值）
    - 二分类：垃圾邮件分类
    - 多分类：手写数字识别
    - 评价指标：正确率、召回率、F值
  - 回归：主要用于预测数值型数据（连续值）
    - 二手车价格预测
    - 评价指标：误差(error)
  - 主要算法
    - k-近邻
    - 朴素贝叶斯
    - 支持向量机
    - 决策树
    - 线性回归
    - 局部加权线性回归

## 非监督学习
  - 概念：在未加标签的数据中找到隐藏的结构
  - 聚类：由类似的对象组成多个类
  - 密度估计：根据样本分布的紧密程度，估计与分组的相似性
  - 主要算法
    - K-Means
    - DBSCN
    - 最大期望算法
  - 评价指标：簇内距离（越小越好）、簇间距离（越大越好）

## 评价指标
  - 正确率：提取出的正确信息条数/提取出的总条数
  - 召回率：提取出的正确信息条数/样本总条数
  - F值：正确率与召回率的调和平均值（正确率*召回率*2/(召回率+正确率)


## 算法选择(场景分析）
  - <img src="/Users/qinjw/Library/Application Support/typora-user-images/image-20200327133923648.png" alt="image-20200327133923648" style="zoom:50%;" />

## 过拟合与欠拟合

  - 过拟合：在训练集上表现较好，在未知数据上预测表现差，泛化能力弱
    - 原因：模型复杂度过高（网络深）；特征多；训练数据少
    - 结果：出现**高方差**问题（预测的稳定性差，波动大）
    - 解决方法：扩增数据集、减少特征数量、增加正则化程度（L1，LR)、减少迭代次数（early stopping）、多模型投票（互补）
  - 欠拟合：在训练数据与未知数据上都表现过差，不能够很好的拟合数据

    - 原因：模型复杂度低；拟合函数能力不够
    - 结果：出现**高偏差**问题（偏离正确结果较远）
    - 解决办法：增加迭代次数、更换算法、增加模型的复杂程度
  - 判断方法：从**训练集**中随机选一部分作为**验证集**，采用K折叠交叉验证，训练的同时输出训练集与验证集loss
  	- 若两者误差率都较大 => 欠拟合
  	- 若验证集达到最低 => 拟合适中
  	- 若训练集误差率较低，而验证集误差率达到最低点时升高 => 过拟合
  	- <img src="/Users/qinjw/Library/Application Support/typora-user-images/image-20200327184627546.png" alt="image-20200327184627546" style="zoom:50%;" />




