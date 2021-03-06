# 金融风控训练营Task03特征工程学习笔记
本学习笔记为阿里云天池龙珠计划Docker训练营的学习内容，学习链接为：
https://tianchi.aliyun.com/specials/activity/promotion/aicampfr?spm=5176.12901015.0.i12901015.419b525cZREgQM

## 一、学习知识点概要
* 数据特征值预处理
* 异常值处理的梁两种方法
* 数据分箱
* 特征交互、编码、选择

## 二、学习内容
### 数据特征值预处理
* 数据处理--查找对象特征和数值特征
```
numerical_fea = list(data_train.select_dtypes(exclude=['object']).columns)
category_fea = list(filter(lambda x: x not in numerical_fea,list(data_train.columns)))
label = 'isDefault'
numerical_fea.remove(label)
```
* 缺失值填充--清除异常值，创造更优的比赛数据
 * 对象型类别特征需要进行预处理，其中['issueDate']为时间格式特征。
```
#查看缺失值情况
data_train.isnull().sum()
```
* 时间格式处理--构造时间特征
```
#转化成时间格式
for data in [data_train, data_test_a]:
    data['issueDate'] = pd.to_datetime(data['issueDate'],format='%Y-%m-%d')
    startdate = datetime.datetime.strptime('2007-06-01', '%Y-%m-%d')
    #构造时间特征
    data['issueDateDT'] = data['issueDate'].apply(lambda x: x-startdate).dt.days
    
```
* 类别特征处理
 * Label encoding：使用字典的形式，将不同的类别标签和整数关联，生成一个数组索引。

### 异常值处理
* 方法一--均方差
  * 在统计学中，如果一个数据分布近似正态，那么大约 68% 的数据值会在均值的一个标准差范围内，大约 95% 会在两个标准差范围内，大约 99.7% 会在三个标准差范围内。
    * 数据点超过标准差的 3 倍，那么这些点很有可能是异常值或离群点。
  * 得到特征的异常值后可以进一步分析变量异常值和目标变量的关系
* 方法二--箱型图

### 数据分箱
* 分箱优点
  * 处理缺失值：当数据源可能存在缺失值，此时可以把null单独作为一个分箱。
  * 处理异常值：当数据中存在离群点时，可以把其通过分箱离散化处理，从而提高变量的鲁棒性（抗干扰能力）。例如，age若出现200这种异常值，可分入“age > 60”这个分箱里，排除影响。
* 分箱目的
  * 将连续变量离散化
  * 将多状态的离散变量合并成少状态
### 特征交互：自动学习多个相关特征之间的交互关系。
（原文链接：https://www.xianjichina.com/news/details_81731.html 来源：贤集网）
### 特征编码的两种常见方式：
实际上机器学习模型需要的数据是数字型的，因为只有数字类型才能进行计算。因此，对于各种特殊的特征值，我们都需要对其进行相应的编码，也是量化的过程。
* Label encoding
* One-hot encoding
### 特征选择：
（精简掉无用的特征，以降低最终模型的复杂性，它的最终目的是得到一个简约模型，在不降低预测准确率或对预测准确率影响不大的情况下提高计算速度）
* Filter：基于特征间的关系进行筛选
  * 方差选择法：先计算各个特征的方差，根据设定的阈值，选择方差大于阈值的特征
  * 相关系数法：帮助理解特征和响应变量之间关系的方法，衡量变量之间的线性相关性。
* Wrapper：
  * 递归特征消除法：使用一个基模型来进行多轮训练，每轮训练后，消除若干权值系数的特征，再基于新的特征集进行下一轮训练(类递归)。
* Embedded：
  * 基于**惩罚项**的特征选择法 
  * 基于**树模型**的特征选择

## 三、学习问题与解答
* 类别特征的自映射是什么？？
* 树模型：分为单棵决策树和集成树；
  * 决策树的生成是递归地构建二叉决策树的过程（个人理解是if-else指令集合，通过对特征空间的类型的划分来进行分类任务和回归），**对回归树用平方误差最小化准则**，对分类树用基尼指数最小化准则，进行特征选择，生成二叉树；**分枝时穷举每一个feature的每个阈值找最好的分割点**
## 四、学习思考与总结
特征工程在实际的数据的处理过程中十分重要，也复杂且费时，除了缺失值异常值等比较常见的特征值外还有太多的特征值，
处理起来也要用到很多以前完全没接触过的方法，以及数据的分箱处理，这些对于我来说都是比较难的知识点，但是这些没接触过
的知识点都有可能能提高我的数据处理能力，进而对我以后的比赛、项目等等产生作用，虽然难但我会继续迎着上。
