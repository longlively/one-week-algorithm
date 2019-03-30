# 任务二：逻辑回归算法梳理
## 本期学习目标：

1. 逻辑回归与线性回归的联系与区别
2. 逻辑回归的原理
3. 逻辑回归损失函数推导及优化
4. 正则化与模型评估指标
5. 逻辑回归的优缺点
6. 样本不均衡问题解决办法
7. sklearn参数 

<br/>

## 1. 逻辑回归与线性回归的联系与区别
主要区别如表格中所列：
对比|Linear|Logistic
--|--|--
自变量|连续离散|连续离散
**因变量(最大不同)**|连续|0-1
关系|线性|非线性
表达式|f(x)=wT*X|f(x)=1/(1+e^(wTX))
函数形式|拟合预测|概率分类
代价函数|MSE|MLE  
> 逻辑回归多了一个Sigmoid函数，使样本能映射到[0,1]之间的数值，用来做分类问题。
> 两种都可以可以归于同一个家族，即广义线性模型（generalized linear model）。这一家族中的模型形式基本上都差不多，不同的就是因变量不同，如果是连续的，就是多重线性回归，如果是二项分布，就是logistic回归。logistic回归的因变量可以是二分类的，也可以是多分类的，但是二分类的更为常用，也更加容易解释 。

<br/>

## 2. 逻辑回归的原理
面对一个回归或者分类问题，建立代价函数，然后通过优化方法迭代求解出最优的模型参数，然后测试验证我们这个求解的模型的好坏.
如果将线性回归的结果做一个在函数g(x)上的转换，可以变化为逻辑回归。这个函数g(x)在逻辑回归中我们一般取为sigmoid函数，形式如下：

```math
y(z)=1/1+e^{-z}
```
它有一个非常好的性质，即当z趋于正无穷时，g(z)趋于1，而当z趋于负无穷时，g(z)趋于0，这非常适合于我们的分类概率模型。另外，它还有一个很好的导数性质：

```math
g'(z)=g(z)(1-g(z))
```
这个通过函数对g(z)求导很容易得到，后面我们会用到这个式子。 
如果我们令g(z)中的z为：z=xθ，这样就得到了二元逻辑回归模型的一般形式：

```math
hθ(x)=1/(1+e^{-xθ})
```
其中x为样本输入，hθ(x)为模型输出，可以理解为某一分类的概率大小。而θ为分类模型的要求出的模型参数。对于模型输出hθ(x)，我们让它和我们的二元样本输出y（假设为0和1）有这样的对应关系，如果hθ(x)>0.5 ，即xθ>0, 则y为1。如果hθ(x)<0.5，即xθ<0, 则y为0。y=0.5是临界情况，此时xθ=0为， 从逻辑回归模型本身无法确定分类。

hθ(x)的值越小，而分类为0的的概率越高，反之，值越大的话分类为1的的概率越高。如果靠近临界点，则分类准确率会下降。 

此处我们也可以将模型写成矩阵模式：

```math
hθ(x)=1/(1+e^{-xθ})
```
其中hθ(X)为模型输出，为 mx1的维度。X为样本特征矩阵，为mxn的维度。θ为分类的模型系数，为nx1的向量。

<br/>

## 3. 逻辑回归损失函数推导及优化
参考文档：[逻辑回归原理总结](https://www.cnblogs.com/pinard/p/6029432.html)

### 1. 损失函数
线性回归是连续的，所以可以使用模型误差的的平方和来定义损失函数。但是逻辑回归不是连续的，自然线性回归损失函数定义的经验就用不上了。但是可以用最大似然法来推导出我们的损失函数。 
按照二元逻辑回归的定义，假设我们的样本输出是0或者1两类。则有：

```math
P(y=1|x,0)=hθ(x)    
P(y=0|x,0)=1-hθ(x)
```
把这两个式子写成一个式子，就是：
```math
P(y|x,0)=hθ(x)y(1-hθ(x))1-y
```
其中y的取值只能是0或者1。得到了y的概率分布函数表达式，我们就可以用似然函数最大化来求解我们需要的模型系数θ。

为了方便求解，这里我们用对数似然函数最大化，对数似然函数取反即为我们的损失函数J(θ)。其中： 
似然函数的代数表达式为：  
<br/>
$$
L(\theta) = \prod\limits_{i=1}^{m}(h_{\theta}(x^{(i)}))^{y^{(i)}}(1-h_{\theta}(x^{(i)}))^{1-y^{(i)}}
$$  
<br/>
其中m为样本的个数。 
对似然函数对数化取反的表达式，即损失函数表达式为：  

$$
J(\theta) = -lnL(\theta) = -\sum\limits_{i=1}^{m}(y^{(i)}log(h_{\theta}(x^{(i)}))+ (1-y^{(i)})log(1-h_{\theta}(x^{(i)})))
$$
损失函数用矩阵法表达更加简洁：
$$
J(\theta) = -Y^T\bullet logh_{\theta}(X) - (E-Y)^T\bullet log(E-h_{\theta}(X))
$$
其中E为全1向量，·为内积。

### 2.  损失函数的优化
对于二元逻辑回归的损失函数极小化，有比较多的方法，最常见的有梯度下降法，坐标轴下降法，牛顿法等。这里推导出梯度下降法中θ每次迭代的公式。这里给出矩阵法推导二元逻辑回归梯度的过程。     
对于
$$
J(\theta) = -Y^T\bullet logh_{\theta}(X) - (E-Y)^T\bullet log(E-h_{\theta}(X))
$$
我们用$J(\theta)$对$\theta$向量求导可得：     
$$
\frac{\partial}{\partial\theta}J(\theta) = X^T[\frac{1}{h_{\theta}(X)}\odot h_{\theta}(X)\odot (E-h_{\theta}(X))\odot (-Y)] + X^T[\frac{1}{E-h_{\theta}(X)}\odot h_{\theta}(X)\odot (E-h_{\theta}(X))\odot (E-Y)]
$$
这一步我们用到了向量求导的链式法则，和下面三个基础求导公式的矩阵形式：   
$$
\frac{\partial}{\partial x}logx = 1/x    

\frac{\partial}{\partial z}g(z) = g(z)(1-g(z))

\frac{\partial x\theta}{\partial \theta} = x
$$
对于刚才的求导公式我们进行化简可得：
$$
\frac{\partial}{\partial\theta}J(\theta) = X^T(h_{\theta}(X) - Y )
$$

从而在梯度下降法中每一步向量θ的迭代公式如下：
$$
\theta = \theta - \alpha X^T(h_{\theta}(X) - Y )
$$
其中，$\alpha$为梯度下降法的步长。

一般不用操心优化方法，大部分机器学习库都内置了各种逻辑回归的优化方法，不过了解至少一种优化方法还是有必要的。

<br/>

## 4. 正则化与模型评估指标
### 1. 正则化
逻辑回归也会面临过拟合问题，需要考虑正则化。常见的有L1正则化和L2正则化。

逻辑回归的L1正则化的损失函数表达式如下，相比普通的逻辑回归损失函数，增加了L1的范数做作为惩罚，超参数α作为惩罚系数，调节惩罚项的大小。

二元逻辑回归的L1正则化损失函数表达式如下：
$$
J(\theta) = -Y^T\bullet logh_{\theta}(X) - (E-Y)^T\bullet log(E-h_{\theta}(X)) + ||\theta||_1　　　　
$$
其中$||\theta||_1$为$\theta$的L1范数。

逻辑回归的L1正则化损失函数的优化方法常用的有坐标轴下降法和最小角回归法。
二元逻辑回归的L2正则化损失函数表达式如下：
$$
J(\theta) = -Y^T\bullet logh_{\theta}(X) - (E-Y)^T\bullet log(E-h_{\theta}(X)) + \frac{1}{2}\alpha||\theta||_2^2
$$
其中$||\theta||_2$为$\theta​$的L2范数。
逻辑回归的L2正则化损失函数的优化方法和普通的逻辑回归类似。
### 2. 模型评估指标
- 平均均方误差MSE 
- 拟合优度Goodness of fit

<br/>

## 5. 逻辑回归的优缺点
###  1. 优点
- 预测结果是界于0和1之间的概率；
- 可以适用于连续性和类别性自变量；
- 容易使用和解释。

###  2.  缺点
- 对模型中自变量多重共线性较为敏感，例如两个高度相关自变量同时放入模型，可能导致较弱的一个自变量回归符号不符合预期，符号被扭转。需要利用因子分析或者变量聚类分析等手段来选择代表性的自变量，以减少候选变量之间的相关性；
- 预测结果呈“S”型，因此从log(odds)向概率转化的过程是非线性的，在两端随着log(odds)值的变化，概率变化很小，边际值太小，slope太小，而中间概率的变化很大，很敏感。 导致很多区间的变量变化对目标概率的影响没有区分度，无法确定阀值。

<br/>

## 6. 样本不均衡问题解决办法
参考文档：[分类样本不均衡解决办法](https://blog.csdn.net/heyongluoyao8/article/details/49408131)
- 扩大数据集
- 尝试其它评价指标 F1/ReCall/Kappa/ROC
- 重采样
- 其他分类模型
- 增加惩罚项

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

