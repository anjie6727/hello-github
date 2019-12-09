## ASHRAE - Great Energy Predictor III 能耗预测
这是一份对基于赵老师教授的 模式识别课程的 作业
作业的对象是 kaggle :[ASHRAE - Great Energy Predictor III](https://www.kaggle.com/c/ashrae-energy-prediction/overview)
核心算法为决策树，所用优化框架为LightGBM。模型没有就时间序列，特征等方向优化，所以只是得一个基础分。
所得成绩如下
    ![result](https://raw.githubusercontent.com/anjie6727/hello-github/master/score.png)
    ![result](https://raw.githubusercontent.com/anjie6727/hello-github/master/rank.png)
score:1.1 rank:1422
### 1 INTRDUCTIONscore.png
问题描述：开发精确的为建筑物计量能源使用情况的模型。能源类型有：冷水表，电表，热水表和蒸汽表。数据集来自三年时间内超过1,000座建筑物。通过开发的能源模型可以对建筑物的节能投资的效果进行评估，以此说服大型投资者和金融机构将更倾向于在这一领域进行投资，以提高建筑效率。
数据描述:  数据包含以下文件：\
train.csv\
	>building_id - 元数据的键值\
	meter - 计量表的类型 {0: 电表, 1: 冷水, 2: 蒸汽, 3: 热水}. 不是所有建筑物都具备所有类型的计量表.\
	timestamp - 测量值记录的时间\
	meter_reading - 模型的目标变量. 能耗单位 kWh. 这是带误差的真实测量值, 这会增加模型本身的误差. 注意:  0 电表读数单位为kBTU（千磅纯水上升一华氏度所需的能量）.\
building_meta.csv\
	site_id - 天气数据的键值.\
	building_id - 对应 train.csv 中的 building_id\
	primary_use - 基于EnergyStar标准定义的建筑主要活动类别\
	square_feet - 平方英尺\
	year_built - 建筑年限\
	floor_count - 建筑楼层\
weather_[train/test].csv 分为train和test两个文件\
	site_id - 距离建筑物最近气象站的编码\
	air_temperature - 空气温度，摄氏度\
	cloud_coverage - 云遮度\
	dew_temperature - 湿温，摄氏度\
	precip_depth_1_hr - 降水，毫米\
	sea_level_pressure - 海平面气压 毫米汞柱/百帕\
	wind_direction - 风向 (0-360)\
	wind_speed - 风速 米每秒\
test.csv\
test.csv 没有特征数据;它就是表征那些数据是属于测试集的.\
	row_id - 行号\
	building_id - 之前文件中给定的Building id\
	meter - 计量表类型\
	timestamp - 时间戳\
### 2 FEATURES

### 3 CLASSIFICTION

### 4 ESTIMATION
