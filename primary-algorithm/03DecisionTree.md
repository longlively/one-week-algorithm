# 任务三：决策树算法梳理

> DATE: 2019.4.1 ~ 2019.4.3

## 本期学习目标：

1. 信息论基础（熵 联合熵 条件熵 信息增益 基尼不纯度）
2. 决策树的不同分类算法（ID3算法、C4.5、CART分类树）的原理及应用场景
3. 回归树原理
4. 决策树防止过拟合手段
5. 模型评估
6. sklearn参数详解，Python绘制决策树

<br/>

## 1. 信息论基础

熵Entropy：表示随机变量的不确定性.物理学中表示对事物的混乱程度的度量，熵值趋高越有秩序，反之越低越混乱无序  
H(x) = E[I(xi)] = E[ log(2,1/p(xi)) ] = -∑p(xi)log(2,p(xi)) (i=1,2,..n)  
H(x)表示熵值，x是随机变量，p(xi)表示第i个x发生的概率值。  
变量不确定性越大，即可能性越多，越混乱无序，熵越大。  
  
联合熵：度量一个联合分布的随机系统的不确定度，比如有两个随机变量X,Y.  物理含义是观察一个多随机变量的随机系统的信息量。  
H(X,Y) = H(X)+H(Y∣X)    

条件熵：上式右半部分H(Y∣X)即条件熵，物理含义是：在得知某一确定信息的基础上，获取另外一个信息时所获得的信息量。  
随机变量X在给定条件下随机变量Y的条件熵，对定义描述为：X给定条件下Y的条件干率分布的熵对X的数学期望。  
条件熵是用来解释信息增益而引入的概念。  
对一个两个随机变量的随机系统，我们可以先观察一个随机变量获取信息量，观察完后，我们可以在拥有这个信息量的基础上观察第二个随机变量的信息量。  

信息增益：通俗理解就是熵的变化，差值算出来就是信息增益。信息增益在决策树算法中是用来选择特征的指标，信息增益越大，则这个特征的选择性越好。在概率中定义为：待分类的集合的熵和选定某个特征的条件熵之差。  

基尼不纯度Gini: 指将来自集合中的某种结果随机应用在集合中，某一数据项的预期误差率。 可以理解为一个随机事件变成它的对立事件的概率。  
Gini(p) =  1- 概率的平方（越确定的越纯的可能性越大的概率越接近1，1-1 此值 Gini系数 越接近0，越纯，也就是不纯度越低）  
也就是Gini系数越小越好。和熵的概念差不多。 

<br/>

## 2. 决策树的不同分类算法
ID3算法：即信息增益算法，通过比较熵值变化，变化越大的决策越优先。  
应用场景：决策树是通过一系列规则对数据进行分类的过程。ID3是最常用的一种指标，用于选择一个最适合的特征做切分树条件。
ID3算法只能处理离散型的描述性属性。

C4.5：即信息增益率，考虑了自身的信息增益，中和考虑。比如一个数据集中的ID，决策选节点时用ID3算得的信息增益值很高（每个值都分为单个叶子节点，变化多，熵高，与之前的熵之差大），应该是最优选，但它是没有意义的，所以除以它自身的熵值（自身是各个不同，无重复id，熵值高），信息增益率的引入就是这样一个中和的目的。  
应用场景：C4.5是针对解决ID3出现的，适用于，当一个特征的可取值较多时， 每个可取值的样本数不多，很少，信息增益非常高。这时需考虑中和自身熵，适合用C4.5信息增益率。
C4．5算法是ID3算法的后续算法，它能够处理连续型数据。

CART：用基尼系数（基尼不纯度）来当做衡量标准，来度量自变量的不确定性。  
CART算法是一种通过计算Diversity(整体)-diversity(左节点)-diversity(右节点)的值取最佳分割的算法。  
应用场景：CART算法用Gini系数选择变量的不纯性度量。如果目标变量是标称的，并且是具有两个以上的类别，则CART可能考虑将目标类别合并成两个超类别（双化）。

<br/>

## 3. 回归树原理 
分类数与回归树比较：  

上面的决策树目标变量（即输出结果）为分类型数值，叫分类决策树。  
目标变量为连续型数值时的决策树叫回归决策树。  
前者用于分类，如晴天/阴天/雨天、用户性别、邮件是否是垃圾邮件，后者用于预测实数值，如明天的温度、用户的年龄等。    
   
回归树原理：  

是通过把连续数值离散化（即把连续型属性的值分成不同的区间），是遍历数值，找到一个合适的（衡量标准同分类树，ID3,C4.5,CART都可以用？）阈值，做为切分树条件。
<br/>

## 4. 决策树防止过拟合手段 
决策树过拟合的风险很大。
可以通过剪枝（预剪枝和后剪枝）控制树的深度和广度，即，选取特征时，控制决策树的参数，即达到一定阈值停止分裂。参数包括：树的深度（几层），叶子节点数，叶节点所含样本数，信息增益等。 

<br/>

## 5. 模型评估 
sklearn中可以用：  
回归结果.score(X_test,y_test)来算分值评估。 

评估模型可以用：  
自助法（bootstrap）
准确度的区间估计  

也可用其它线性回归逻辑回归的评估方法：  

#MSE评估指标 均方误差  
skl_MSE = metrics.mean_squared_error(y_test,y_pred)  
#RMSE评估指标 均方根误差  
skl_RMSE = np.sqrt(skl_MSE)  
#算MAE平均绝对值误差 #单词absolute，绝对的  
skl_MAE = metrics.mean_absolute_error(y_test,y_pred)  
#算R^2  
skl_R2 = metrics.r2_score(y_test,y_pred)  

公式如下：  

#手算R方，MSE  
hand_MSE=np.sum(np.power((y_test.values.reshape(-1,1) - y_pred),2))/len(y_test.values)  
#均方误差公式：y真实值与预测值之差（误差）平方（方）和再求平均（均）  
#手算RMSE  
hand_RMSE = np.sqrt(hand_MSE)  
R2=1-hand_MSE/np.var(y_test.values)#代入【R方公式】,R方越大表示右半部损失越小，表示模型越好  
#算MAE平均绝对值误差  
hand_MAE=np.sum(np.abs((y_test.values.reshape(-1,1) - y_pred)))/len(y_test.values)

<br/>

## 6. sklearn参数详解，Python绘制决策树
参数：
注意max_depth树深度和
min_samples_split内部节点再划分所需最小样本数：
限制了子树继续划分的条件，如果某节点的样本数少于min_samples_split，则不会再分。 默认是2.如果样本量不大，不需要管这个值。
其它默认就好，不常改。

绘制见附件task3.ipynb

<br/>

## 7. sklearn参数 

```python
class sklearn.linear_model.LogisticRegression(penalty=’l2’, dual=False, 
                                              tol=0.0001, C=1.0, 
                                              fit_intercept=True,intercept_scaling=1, 
                                              class_weight=None, random_state=None,
                                              solver='warn', max_iter=100, 
                                              multi_class='warn', verbose=0, 
                                              warm_start=False, n_jobs=None)
```

penalty：参数类型：str，可选：‘l1’ or ‘l2’, 默认: ‘l2’。该参数用于确定惩罚项的范数。

dual：参数类型：bool,默认：False。双重或原始公式。使用liblinear优化器，双重公式仅实现l2惩罚。

tol：参数类型：float，默认：e-4。停止优化的错误率。

C：参数类型：float，默认；1。正则化强度的导数，值越小强度越大。

fit_intercept：参数类型：bool，默认：True。确定是否在目标函数中加入偏置。

intercept_scaling：参数类型：float，默认：1。仅在使用“liblinear”且self.fit_intercept设置为True时有用。

class_weight：参数类型：dict，默认：None。根据字典为每一类给予权重，默认都是1。

random_state：参数类型：int，默认：None。在打乱数据时，选用的随机种子。

solver：参数类型：str，可选：{'newton-cg', 'lbfgs', 'liblinear', 'sag', 'saga'}, 默认：liblinear。选用的优化器。

max_iter：参数类型：int，默认：100。迭代次数。

multi_class：参数类型：str，可选：{'ovr', 'multinomial', 'auto'}，默认：ovr。如果选择的选项是'ovr'，那么二进制问题适合每个标签。对于“多项式”，最小化的损失是整个概率分布中的多项式损失拟合，即使数据是二进制的。当solver ='liblinear'时，'multinomial'不可用。如果数据是二进制的，或者如果solver ='liblinear'，'auto'选择'ovr'，否则选择'multinomial'。

verbose：参数类型：int，默认：0。对于liblinear和lbfgs求解器，将详细设置为任何正数以表示详细程度。

warm_start：参数类型：bool，默认：False。是否使用之前的优化器继续优化。

n_jobs：参数类型：bool，默认：None。是否多线程。

<br/>

## 参考资料

1. 周志华. 机器学习: Machine learning[M]. 北京: 清华大学出版社, 2016.
2. [cs229吴恩达机器学习课程](http://open.163.com/special/opencourse/machinelearning.html)
3. 李航. 统计学习方法[M]. 北京: 清华大学出版社, 2012.
4. https://scikit-learn.org/stable/_downloads/scikit-learn-docs.pdf
5. [详细公式推导](http://t.cn/EJ4F9Q0)

<br/>

