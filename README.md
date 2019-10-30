# time-series
时序数据分析
任务

基于交通卡口历史数据、路网信息、出租车GPS信息等条件设计算法模型，预测指定卡口在未来某段时间内的交通流量。

数据

*注 : 报名参赛或加入队伍后，可获取数据下载权限。
此数据有两类内容，分别为交通卡口的过车数据、出租车GPS记录。需要预测的是交通卡口的过车流量，可借助出租车GPS记录来预测交通流向。

数据的时间跨度为2019年8月1日~2019年8月23日，被划分为训练集和测试集： 2019年8月19日（含）之前的数据为训练集，2019年8月20日（含）之后的数据为测试集。

注：由于摄像设备原因，交通卡口过车数据可能出现某时间段的整体缺失，此为正常现象，请选手自行解决

（待预测数据中若出现此类情况，该时间段的数据不计入最终评分）



1.1 交通卡口的过车数据

1.1.1 训练集
a) 交通卡口的编号及名字信息，文件名为crossroadName.csv。字段内容为：
crossroadID,crossroadName
100205,人民路与重庆南路交叉口
100353,人民路与温州路交叉口
分别表示交通卡口的编号和对应名字。
注：此记录是从“字段对应关系说明.xlsx”中提取而来，由于道路命名的原因，在提取过程中会丢失掉一些信息。提取示例如下：
若路段描述为“人民路-抚顺路-人民路-皋新路”，那么上游交叉口就对应人民路与抚顺路交叉口，下游交叉口为人民路与皋新路交叉口。

b) 路网信息，即交通卡口之间的连接信息，文件名为roadnet.csv。字段内容为：
crossroadID1,crossroadID2
100205,100353
100031,100030
分别表示两个交通卡口的编号，表示这两个交通卡口是相邻的，由一段道路连接，所以这一条记录也可以看作是对一段道路的描述。

c) 交通卡口过车数据，文件名为train_trafficFlow_x.csv，其中x取值为[1,19]，共19个文件。字段内容为：

![](http://dc-anhui.obs.cn-east-2.myhwclouds.com/pkbigdata/master.other.img/f4b7ec31-8ddc-4ba0-b2ab-57e8e45c55d9.png)

direction,laneID,timestamp,crossroadID,vehicleID

5,2,2019-08-01 13:28:36,100120,鲁U-3d1d4c9c9bc6a990

5,2,2019-08-01 16:56:27,100120,桂B-0ef8d356d7a038cb

注：训练集各卡口不同时间段的流量数据需要选手根据以上数据自行统计。

1.1.2 测试集

a) 交通卡口过车数据，文件名为test_trafficFlow_x.csv，其中x取值为[20,23]，共4个文件。字段内容与训练集中的train_trafficFlow_x.csv相同，不同点在于该测试集中的数据，仅保留了部分时段的记录。保留规则如下：
注：不可用未来时间的数据预测过去，如，不能用今天9:00:00的数据预测今天7:00:00的。

![](http://dc-anhui.obs.cn-east-2.myhwclouds.com/pkbigdata/master.other.img/abc59b48-3943-451d-9308-6ec46e051289.png)

b) 待预测信息，请参考提交结果的样本文件，submit_example.csv，共有35个交通卡口待预测，需预测4天的数据，共需提交5040行数据（不含表头）。
注：待预测的“交通卡口”，在训练集中没有记录，需要基于这些交通卡口的邻近交通卡口的交通信息，建立模型完成预测。

提交结果的样本文件，具体请参考示例文件，字段描述为：date,crossroadID,timeBegin,value

其中，date为日期，取值为20,21,22,23；timeBegin为hh:mm，如7:30，表示从7:30:00~7:34:59这个时间段；即value表示的是每5分钟的交通流量总量。  

1.2 出租车的GPS记录
1.2.1 训练集
出租车GPS记录，文件名为trainTaxiGPS_x.csv，其中x取值为[1,19]，共19个文件。字段内容为：
![](http://dc-anhui.obs.cn-east-2.myhwclouds.com/pkbigdata/master.other.img/41594cce-a5a5-432a-8ad9-9b69a83523b5.png)

taxiID,lng,lat,GPSspeed,direction,timestamp

鲁Uf26f3202675eaa169874b5a56376f966,120.404,36.1897,0.0,124,2019-08-01 07:35:36

鲁Uf26f3202675eaa169874b5a56376f966,120.407,36.1766,0.0,85,2019-08-01 07:46:44


1.2.2 测试集
出租车GPS记录，文件名为testTaxiGPS_x.csv，其中x取值为[20,23]，共4个文件。字段内容同训练集，不同点在于该测试集中的数据，仅保留了部分时段的记录。
保留规则与“交通卡口过车数据-1.1.2测试集-a)”中的描述是相同的。
