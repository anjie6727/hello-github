## ASHRAE - Great Energy Predictor III 能耗预测
这是一份基于赵老师教授的 模式识别 课程的 作业
作业的对象是 kaggle :[ASHRAE - Great Energy Predictor III](https://www.kaggle.com/c/ashrae-energy-prediction/overview)
**核心算法为决策树，所用优化框架为LightGBM。模型没有就时间序列，特征等方向进行优化，所以只是先得一个基础分。**
所得成绩如下
&emsp;[这是我的kaggle:ANAN6727](https://www.kaggle.com/anan6727903376998/competitions)
    ![score](https://raw.githubusercontent.com/anjie6727/hello-github/master/score.png)
    ![rank](https://raw.githubusercontent.com/anjie6727/hello-github/master/rank.png)
score:1.1 rank:1422
### 1 介绍
问题描述：开发精确的为建筑物计量能源使用情况的模型。能源类型有：冷水表，电表，热水表和蒸汽表。数据集来自三年时间内超过1,000座建筑物。通过开发的能源模型可以对建筑物的节能投资的效果进行评估，以此说服大型投资者和金融机构将更倾向于在这一领域进行投资，以提高建筑效率。
数据描述:  数据包含以下文件：\
train.csv\
	&emsp;building_id - 元数据的键值\
	&emsp;meter - 计量表的类型 {0: 电表, 1: 冷水, 2: 蒸汽, 3: 热水}. 不是所有建筑物都具备所有类型的计量表.\
	&emsp;timestamp - 测量值记录的时间\
	&emsp;meter_reading - 模型的目标变量. 能耗单位 kWh. 这是带误差的真实测量值, 这会增加模型本身的误差. 注意:  0 电表读数单位为kBTU（千磅纯水上升一华氏度所需的能量）.\
building_meta.csv\
	&emsp;site_id - 天气数据的键值.\
	&emsp;building_id - 对应 train.csv 中的 building_id\
	&emsp;primary_use - 基于EnergyStar标准定义的建筑主要活动类别\
	&emsp;square_feet - 平方英尺\
	&emsp;year_built - 建筑年限\
	&emsp;floor_count - 建筑楼层\
weather_[train/test].csv 分为train和test两个文件\
	&emsp;site_id - 距离建筑物最近气象站的编码\
	&emsp;air_temperature - 空气温度，摄氏度\
	&emsp;cloud_coverage - 云遮度\
	&emsp;dew_temperature - 湿温，摄氏度\
	&emsp;precip_depth_1_hr - 降水，毫米\
	&emsp;sea_level_pressure - 海平面气压 毫米汞柱/百帕\
	&emsp;wind_direction - 风向 (0-360)\
	&emsp;wind_speed - 风速 米每秒\
test.csv\
test.csv 没有特征数据;它就是表征那些数据是属于测试集的.\
	&emsp;row_id - 行号\
	&emsp;building_id - 之前文件中给定的Building id\
	&emsp;meter - 计量表类型\
	&emsp;timestamp - 时间戳
### 2 特征
由问题描述可以得到这是一个Regression问题，故选择回归决策树来解决问题。\
针对大量的数据，对特征进行筛选。已选择出对问题有效，同时又方便解决的特征。主要对时间特征进行筛选，处理，提取出几个对问题可能有效的时间特征，如，工作时间等。\
增加的特征有 :\
&emsp;hour：时间戳中的小时\
&emsp;weekday：时间戳是否在周末\
&emsp;is holiday: 时间戳是否为假日\
不予考虑的特征:\
&emsp;timestamp: 完整时间戳数据\
&emsp;sea_level_pressure:天气大气压数据\
&emsp;wind_direction：风向数据\
&emsp;wind_speed：风力数据
### 3 回归
&emsp;LightGBM框架是基于树的梯度增强框架。它被设计为分布式且高效的，具有以下优点：
#### 优化速度和内存使用率
&emsp;LightGBM使用基于直方图的算法，该算法将连续特征（属性）值存储到离散的桶中。这样可以加快训练速度并减少内存使用量。基于直方图的算法的优点包括：\
&emsp;&emsp;计算成本：降低了计算每次节点分割的增益的成本\
&emsp;&emsp;时间复杂度：基于预排序的算法具有时间复杂性 O(数据)。计算直方图本身具有时间复杂性为O(数据)，但这仅涉及快速求和操作。一旦构建好了特征的直方图，基于直方图的算法就具有时间复杂度O(桶)，并且桶的规模远小于数据的规模。\
&emsp;&emsp;直方图可以直接作差：使用直方图减法可以进一步提高速度。要在二叉树中获得叶子的直方图，使用其父级和邻居的直方图减法即可。因此，它只需要为一个叶子（数据规模比它的邻居小）构建直方图。然后，可以通过直方图减法以较小的成本获得邻居的直方图。\
&emsp;&emsp;减少内存使用：用离散的桶替换连续的值。如果桶的规模较小，则可以使用较小的数据类型（例如uint8_t）来存储训练数据。而且，无需存储其他信息即可对特征值进行预排序。
#### 精度优化
##### 逐叶（最佳优先）生长
&emsp;大多数未经优化的决策树学习算法都是按深度来生长树，LightGBM的叶子是树木生长的最好优先。它将选择具有最大损失的叶子来生长。该算法得到的叶子规模与一般算法相比，逐叶算法可以趋于实现更低的损耗。当数据规模较小时，逐叶算法可能会导致过拟合，因此LightGBM包含max_depth参数用于限制树的深度。但是，即使max_depth指定了树，树仍然会向特定的方向生长。
##### 离散分类特征的最佳分割
&emsp;通常用one-hot编码来表示分类特征，即离散的特征，特征与特征之间是没有顺序意义的。但是这种方法不是最理想的。特别是对于多种的分类特征，基于one-hot特征构建的树倾向于不平衡，并且需要非常深地生长才能获得良好的精度。而决策树等算法，通过将类别的类别划分为2个子集来对类别进行拆分。这种回归树算法能有效地找到最佳分区。
#### 稀疏优化
&emsp;只需要构造稀疏特征的直方图
```
params = {
    "objective": "regression",
    "boosting": "gbdt",
    "num_leaves": 40,
    "learning_rate": 0.05,
    "feature_fraction": 0.85,
    "reg_lambda": 2,
    "metric": "rmse"
}
model = lgb.train(params, train_set, num_boost_round=1000, valid_sets=list, verbose_eval=200, early_stopping_rounds=200)
```
### 4 评价与展望

### 5 参考
[🔌⚡ASHRAE -Start Here: A GENTLE Introduction](https://www.kaggle.com/caesarlupum/ashrae-start-here-a-gentle-introduction)\
[ASHRAE: Half and Half](https://www.kaggle.com/rohanrao/ashrae-half-and-half)\
[LightGBM](https://lightgbm.readthedocs.io/en/latest/Features.html)

