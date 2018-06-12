# 2018-user-risk
2018年联通大数据 用户风险预测 线上 RANK9 这个应该是我参加比赛以来，取得的最好成绩。 2018年6月11日
感谢队友

赛题分析：
本次赛题要求从用户通话/短信/上网记录中识别‘风险’用户，赛题提供用户于过去45天内的行为日志，因此，我们队伍分别通过对用户的短信日志，通话日志和上网日志三个维度描述用户特征，通过特征描述用户，构建用户画像。

## 特征分析：
### 1.	通话日志
### 字段：通话号码（脱敏）
### 通话号码的长度/区段，通话类型（地区/主叫被叫）
### 通话起始时间和结束时间
#### 1.	通话时长特征（用户通话时间的统计特征，最大值，最小值，均值等）
#### 2.	通话间隔特征（用户下一次，下两次通话的时间差特征）
#### 3.	尝试对用户通话号码进行LDA/SVD降维度处理（最终结果未采用）
##### 解释：
##### 用户1 号码1 号码2 号码3 号码4
##### 用户2 号码1 号码2 号码4 号码5
##### 用户3 号码1 号码2 号码6
##### 可以采取文本处理的只是，把号码序列当作文本，用NLP的技术处理，得到向量。

#### 4.	构造了类似于协同过滤矩阵的特征
#### 解释：采用pandas的透视功能，构造 号码长度 和 号码区段的特征矩阵，具体可参考：pd.pivot_table函数的解释。
#### 5.	用户-通话号码的出度与入度特征
#### 解释：将用户和通话号码看作两个节点，分别可以计算与用户相关通话号码的数量关系，同理，计算号码关联的用户的数量关系。 计算号码与用户相关的数量特征可以进一步的得到用户也号码相关特征的特征。 例如 号码1 23 号码2 43 都与用户1有关，这样用户1就可以得到 （23+43）/2 的一个均值特征
#### 6.	进一步交互特征（未完全实现）
##### 例子：对通话号码的频次和被叫时常统计，之后在根据用户对通话号码的关系，进一步统计用户都通话号码特征的特征的统计
#### 7.	时间衰减特征
##### 考虑到用户行为的时许行，使用46-当前天数的倒数作为全职，统计用户行为的时候，进行加权处理。也可以直接对权值求和，反馈用户的活跃天数（经过测试，这个问题效果不好）

### 2.	短信日志
#### 短信字段类型基本与通话部分类似，其特征构造基本一致。
### 3.	上网日志
#### 其实这个部分提供的特征比较少，我们也只是采用了通话日志特征中的2，4构造了简单的特征。

##### 最终单模型的特征约 400 维度

总结一些不足，没有完全挖掘用户在短信和通话两张表的交互特征，没有实现 图 相关特征的提取，同时最近发现，可以采取一些无监督方式去扩展又有的特征，这个部分需要细致学习。最近看到了一些关于只是图谱相关的东西，发现对于上网记录可以采纳相关的内容。感谢联通和jdata提供如此有意思的题目，也同时通过比赛，发现了自身的不足和很多需要完善的地方。
