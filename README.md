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
