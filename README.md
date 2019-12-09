## ASHRAE - Great Energy Predictor III 能耗预测
这是一份基于赵老师教授的 模式识别 课程的 作业
作业的对象是 kaggle :[ASHRAE - Great Energy Predictor III](https://www.kaggle.com/c/ashrae-energy-prediction/overview)
核心算法为决策树，所用优化框架为LightGBM。模型没有就时间序列，特征等方向进行优化，所以只是得一个基础分。
所得成绩如下
    ![result](https://raw.githubusercontent.com/anjie6727/hello-github/master/score.png)
    ![result](https://raw.githubusercontent.com/anjie6727/hello-github/master/rank.png)
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
	&emsp;timestamp - 时间戳\
### 2 特征

### 3 回归

### 4 评价与展望

### 5 参考
[🔌⚡ASHRAE -Start Here: A GENTLE Introduction](https://www.kaggle.com/caesarlupum/ashrae-start-here-a-gentle-introduction)
[ASHRAE: Half and Half](https://www.kaggle.com/rohanrao/ashrae-half-and-half)

