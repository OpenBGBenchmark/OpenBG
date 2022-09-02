# OpenBG：大规模开放数字商业知识图谱

<p align="left">
    <b> 简体中文 | <a href="https://github.com/OpenBGBenchmark/OpenBG/blob/main/README_EN.md">English</a> </b>
</p>

[OpenBG](https://kg.alibaba.com/)是开放的数字商业知识图谱，是一个使用统一Schema组织、涵盖产品和消费需求的百万级多模态数据集。OpenBG由浙江大学、阿里巴巴藏经阁团队联合提供，开放的目标是利用开放的商业知识发现社会经济的价值，促进数字商务数字经济等领域的交叉学科研究，服务数字经济健康发展的国家战略需求。

[OpenBG Benchmark](https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271)是一个以OpenBG为基础构建的大规模开放数字商业知识图谱评测基准，包含多个子数据集和子任务。欢迎小伙伴打榜[https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271](https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271)。


## 数据集构建流程
<center><img src="./img/OpenBG.png" width="80%"></center>

## 数据集

数据集统计数据如下：
|    Dataset    |    # Ent   | # Rel |   # Train   |  # Dev  | # Test  |
| ------------- | ---------- | ----- | ----------- | ------- | ------- |
|   OpenBG-IMG  | 27,910     |  136  | 230,087     | 5,000   | 14,675  |
|   OpenBG500   | 249,743    |  500  | 1,242,550   | 5,000   |  5,000  |
|   OpenBG500-L | 2,782,223  |  500  | 47,410,032  | 10,000  | 10,000  |
|  OpenBG(Full) | 88,881,723 | 2,681 | 260,304,683 |    -    |    -    |


### OpenBG-IMG
OpenBG-IMG是电子商务领域的多模态知识图谱，来自于<a href="https://tianchi.aliyun.com/competition/entrance/531957/introduction">CCKS2022面向数字商务的知识处理与应用评测任务三：多模态商品知识图谱链接预测</a>。OpenBG-IMG数据集相比原CCKS竞赛数据集，添加了新的dev划分，并且添加了实体和关系的文本信息，测试集来自CCKS竞赛初赛测试集。

多模态链接预测指的是给定一个三元组的头实体、头实体图片和关系，预测对应的尾实体，如下图所示：

<center><img src="./img/OpenBG-IMG.png" width="80%"></center>

其中80%的头实体会给出图片信息。
输入：（头实体[部分有图片]<\t>关系）
输出：（尾实体）

#### 数据集目录结构

```
OpenBG-IMG
├───── OpenBG-IMG_images			# 图片集
	├── ent_xxxxxx					# 实体对应图片
	...
├── OpenBG-IMG_train.tsv 			# 训练数据
├── OpenBG-IMG_dev.tsv 				# 验证数据
├── OpenBG-IMG_test.tsv 			# 需要预测的数据，选手需为每条记录预测10个尾实体
├── OpenBG-IMG_entity2text.tsv 		# 实体对应文本
├── OpenBG-IMG_relation2text.tsv 	# 关系对应文本
└── OpenBG-IMG_example_pred.tsv 	# 提交结果示例
```

### OpenBG500

OpenBG500包含500个关系，从OpenBG中筛选采样得到。

#### 数据集目录结构
```shell
OpenBG500
├── OpenBG500_train.tsv 			# 训练数据
├── OpenBG500_dev.tsv 				# 验证数据
├── OpenBG500_test.tsv 				# 需要预测的数据，选手需为每条记录预测10个尾实体
├── OpenBG500_entity2text.tsv 		# 实体对应文本
├── OpenBG500_relation2text.tsv 	# 关系对应文本
└── OpenBG500_example_pred.tsv 		# 提交结果示例
```

### OpenBG500-L

OpenBG500-L包含500个关系，从OpenBG中筛选采样得到，规模比OpenBG大。

#### 数据集目录结构
```shell
OpenBG500-L
├── OpenBG500-L_train.tsv 				# 训练数据
├── OpenBG500-L_dev.tsv 				# 验证数据
├── OpenBG500-L_test.tsv 				# 需要预测的数据，选手需为每条记录预测10个尾实体
├── OpenBG500-L_entity2text.tsv 		# 实体对应文本
├── OpenBG500-L_relation2text.tsv 		# 关系对应文本
└── OpenBG500-L_example_pred.tsv 		# 提交结果示例
```

## 使用数据

### 数据集格式

* 三元组数据，tsv格式

```shell
# {数据集}_train.tsv/{数据集}_dev.tsv
头实体<\t>关系<\t>尾实体<\n>
```

* 实体/关系对应文本数据，tsv格式

```shell
# {数据集}_entity2text.tsv/{数据集}_relation2text.tsv
实体（关系）<\t>实体（关系）对应文本<\n>
```

* 评测相关数据格式

```shell
# {数据集}_test.tsv，选手需要为每行记录补充10格预测的尾实体，提交格式参照{数据集}_example_pred.tsv
头实体<\t>关系<\n>

# {数据集}_example_pred.tsv
头实体<\t>关系<\t>尾实体1<\t>尾实体2<\t>...<\t>尾实体10<\n>
```

### 查看数据集数据

```
$ head -n 3 {数据集}_train.tsv
<http://ali.openkg.cn/alischema#Product/pid_95366c60c08b45d8931d2f459712ff12>	<http://ali.openkg.cn/alischema#Property/External_Material>	苦荞麦
<http://ali.openkg.cn/alischema#Product/pid_6c3646395ec10143cdc8de645aeb93a9>	<http://ali.openkg.cn/alischema#Property/Flavor>	原味硬糕850克【10包40块糕】
<http://ali.openkg.cn/alischema#Product/pid_a31f0d7ce23fbb80f250ec5b2dd2bc82>	<http://ali.openkg.cn/alischema#Property/inMarket>	<http://ali.openkg.cn/alischema#Market_segment/tag_0b256e3b23f5b7a9d006f87d28ce0ead>
```

### 使用python转换并读取数据集

1. 读取原始数据：
```python
with open('{数据集}_train.tsv', 'r') as fp:
    data = fp.readlines()
    train = [line.strip('\n').split('\t') for line in data]
    _ = [print(line) for line in train[:2]]
    # ['ent_135492', 'rel_0352', 'ent_015651']
    # ['ent_020765', 'rel_0448', 'ent_214183']
```

2. 获取实体、关系对应文本字典：`ent2text`和`rel2text`
```python
with open('{数据集}_entity2text.tsv', 'r') as fp:
    data = fp.readlines()
    lines = [line.strip('\n').split('\t') for line in data]
    _ = [print(line) for line in lines[:2]]
    # ['ent_101705', '短袖T恤']
    # ['ent_116070', '套装']

ent2text = {line[0]: line[1] for line in lines}

with open('{数据集}_relation2text.tsv', 'r') as fp:
    data = fp.readlines()
    lines = [line.strip().split('\t') for line in data]
    _ = [print(line) for line in lines[:2]]
    # ['rel_0418', '细分市场']
    # ['rel_0290', '关联场景']

rel2text = {line[0]: line[1] for line in lines}
```

3. 数据转换成文本：
```python
train = [[ent2text[line[0]],rel2text[line[1]],ent2text[line[2]]] for line in train]
_ = [print(line) for line in train[:2]]
# ['苦荞茶', '外部材质', '苦荞麦']
# ['精品三姐妹硬糕', '口味', '原味硬糕850克【10包40块糕】']
```

## 获取数据

- 数据集下载链接：

| Dataset       |   Google Drive   | 百度网盘 | 
| ------------- | ---------------- | ------ |
| OpenBG-IMG    | [下载链接](https://drive.google.com/file/d/1jg4YcFgOfgjUJCnxBjw9w-6ID8VS_L-X/view?usp=sharing)    |   [提取码ke65](https://pan.baidu.com/s/1rq1AGTSKLcfuIEnuq6gnJQ) |
| OpenBG500     |    [下载链接](https://drive.google.com/file/d/1pD_icqV-lLbCXN2rfBaq-Y5i_XcKVCzM/view?usp=sharing)    |   [提取码78fw](https://pan.baidu.com/s/1NsRWct-u63QmxVgyjJeXsg) |
| OpenBG500-L   | [下载链接](https://drive.google.com/file/d/1DZZRqc8Yl9mfO66cOS8IKCuim_Bw2oOM/view?usp=sharing) | [提取码767v](https://pan.baidu.com/s/1SbTs7HFfHIrYSlK_hoLxTg) |


- OpenBG(Full)数据集可以在[kg.alibaba.com](https://kg.alibaba.com/)申请获取

## 阿里天池评测

- OpenBG-IMG数据集来自阿里天池[CCKS2022面向数字商务的知识处理与应用评测任务三：多模态商品知识图谱链接预测](https://tianchi.aliyun.com/competition/entrance/531957/information)，baseline代码请关注https://github.com/OpenBGBenchmark/OpenBG-IMG 。
- 阿里天池数据集[OpenBG Benchmark：大规模开放数字商业知识图谱评测基准](https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271)将长期开放数据集的评测。

### 如何提交

阿里天池数据集[OpenBG Benchmark：大规模开放数字商业知识图谱评测基准](https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271)的结果需要在天池的界面提交。

<center><img src="./img/submit.png" width="80%"></center>

1. 每个任务的预测文件，**提交文件需要严格命名为 ${数据集}_test.tsv**，每个任务对应的提交文件名为：
* OpenBG500: OpenBG500_test.tsv
* OpenBG500-L: OpenBG500-L_test.tsv
* OpenBG-IMG: OpenBG-IMG_test.tsv

2. 每个任务生成一个预测文件，输出内容见各任务的要求，也可参照各任务目录下的${数据集名称}_example_pred.tsv文件，所有的预测文件压缩为zip格式在数据集的“提交结果”页面进行提交，压缩命令是:

```
zip somename.zip  {数据集}_test.tsv
```

**注：请用zip压缩命令来进行压缩，保证解压后的结果是tsv文件，不可再包含中间目录层。**

3. 选手可以只提交部分任务的结果，如只提交“OpenBG500”任务：zip somename.zip OpenBG500_test.tsv，未预测任务的分数默认为0。
