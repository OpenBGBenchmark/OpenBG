# OpenBG：大规模数字商业知识图谱

[OpenBG](https://kg.alibaba.com/)是开放的数字商业知识图谱，是一个使用统一Schema组织、涵盖产品和消费需求的百万级多模态数据集。OpenBG由浙江大学ZJUKG实验室、阿里巴巴藏经阁团队联合提供，开放的目标是利用开放的商业知识发现社会经济的价值，促进数字商务数字经济等领域的交叉学科研究，服务数字经济健康发展的国家战略需求。

## 数据集构建流程
<center><img src="./img/OpenBG.png" width="80%"></center>

## 数据集

数据集统计数据如下：
|    Dataset    |    # Ent   | # Rel |   # Train   |  # Dev  | # Test  |
| ------------- | ---------- | ----- | ----------- | ------- | ------- |
|   OpenBG-IMG  | 27,910     |  136  | 230,087     | 5,000   | 14,675  |
|   OpenBG500   | 249,743    |  500  | 1,242,550   | 5,000   |  5,000  |
|   OpenBG500-L | 2,782,223  |  500  | 47,410,032  | 696,839 | 700,321 |
|  OpenBG(Full) | 88,881,723 | 2,681 | 260,304,683 |    -    |    -    |

### OpenBG500

OpenBG500包含500个关系，从OpenBG中筛选采样得到。

#### 数据集目录结构
```shell
OpenBG500
├── train.tsv			        # 训练集数据
├── valid.tsv			        # 验证集数据
├── test.tsv			        # 测试集数据
├── entity2text.tsv			    # 实体对应的中文描述
└── relation2text.tsv			# 关系对应的中文描述
```

#### 查看数据集数据

```shell
$ head -n 3 train.tsv
<http://ali.openkg.cn/alischema#Product/pid_95366c60c08b45d8931d2f459712ff12>	<http://ali.openkg.cn/alischema#Property/External_Material>	苦荞麦
<http://ali.openkg.cn/alischema#Product/pid_6c3646395ec10143cdc8de645aeb93a9>	<http://ali.openkg.cn/alischema#Property/Flavor>	原味硬糕850克【10包40块糕】
<http://ali.openkg.cn/alischema#Product/pid_a31f0d7ce23fbb80f250ec5b2dd2bc82>	<http://ali.openkg.cn/alischema#Property/inMarket>	<http://ali.openkg.cn/alischema#Market_segment/tag_0b256e3b23f5b7a9d006f87d28ce0ead>
```

#### 使用python转换并读取数据集

获取实体、关系对应文本字典：`ent2text`和`rel2text`
```python
with open('entity2text.tsv', 'r') as fp:
    data = fp.readlines()
    lines = [line.strip().split('\t') for line in data]
    _ = [print(line) for line in lines[:2]]
    # ['<http://ali.openkg.cn/alischema#Product/pid_f4e703be773bc04e6b425f59b15a078c>', '短袖T恤']
    # ['<http://ali.openkg.cn/alischema#Product/pid_e529f00852668f253caf79a4155b0678>', '套装']

    ent2text = {line[0]: line[1] for line in lines}

with open('relation2text.tsv', 'r') as fp:
    data = fp.readlines()
    lines = [line.strip().split('\t') for line in data]
    _ = [print(line) for line in lines[:2]]
    # ['<http://ali.openkg.cn/alischema#Property/inMarket>', '细分市场']
    # ['<http://ali.openkg.cn/alischema#Property/relatedScene>', '关联场景']

    rel2text = {line[0]: line[1] for line in lines}
```

数据转换成文本：
```python
with open('train.tsv', 'r') as fp:
    data = fp.readlines()
    lines = [line.strip().split('\t') for line in data]
    lines = [[ent2text[line[0]],rel2text[line[1]],ent2text[line[2]]] for line in lines]
    _ = [print(line) for line in lines[:2]]
    # ['苦荞茶', '外部材质', '苦荞麦']
    # ['精品三姐妹硬糕', '口味', '原味硬糕850克【10包40块糕】']
```

### OpenBG500-L

OpenBG500-L包含500个关系，从OpenBG中筛选采样得到，规模比OpenBG大。

#### 数据集目录结构
```shell
OpenBG500-L
├── train.tsv			        # 训练集数据
├── valid.tsv			        # 验证集数据
└── test.tsv			        # 测试集数据
```

#### 查看数据集数据

```shell
$ head -n 3 train.tsv 
<http://ali.openkg.cn/alischema#Product/pid_ca08a196fd0388a37d2b671826b4456f>	<http://ali.openkg.cn/alischema#Property/Imported>	否
<http://ali.openkg.cn/alischema#Product/pid_39eee17628071aa99e2d45be140d5cb7>	<http://ali.openkg.cn/alischema#Property/Applicable_Season>	夏季
<http://ali.openkg.cn/alischema#Product/pid_549c825765a62f6b8c6b53836d61963c>	<http://ali.openkg.cn/alischema#Property/placeOfOrigin>	<http://ali.openkg.cn/alischema#Place_Of_Origin/中国大陆>
```

#### 使用python读取数据集

```python
with open('train.tsv', 'r') as fp:
    for idx, line in enumerate(fp):
        line = line.strip().split('\t')
        print(line)
        if idx == 2:
            break
    # ['<http://ali.openkg.cn/alischema#Product/pid_ca08a196fd0388a37d2b671826b4456f>', '<http://ali.openkg.cn/alischema#Property/Imported>', '否']
    # ['<http://ali.openkg.cn/alischema#Product/pid_39eee17628071aa99e2d45be140d5cb7>', '<http://ali.openkg.cn/alischema#Property/Applicable_Season>', '夏季']
    # ['<http://ali.openkg.cn/alischema#Product/pid_549c825765a62f6b8c6b53836d61963c>', '<http://ali.openkg.cn/alischema#Property/placeOfOrigin>', '<http://ali.openkg.cn/alischema#Place_Of_Origin/中国大陆>']
```

## 获取数据

- 数据集下载链接：

| Dataset       |   Google Drive   | 百度网盘 | 
| ------------- | ---------------- | ------ |
| OpenBG500     |    [下载链接](https://drive.google.com/file/d/1EC3UzuC9o1jQNiteICUInPfTvHz7aKSp/view?usp=sharing)    |   [提取码wi7s](https://pan.baidu.com/s/1Arm4Jbw5HgY2-jHT2THu8g) |
| OpenBG500-L   | [下载链接](https://drive.google.com/file/d/1A9K_AOEMyzQUlAc7cRBlGXr8CBjUWOtV/view?usp=sharing) | [提取码bgq2](https://pan.baidu.com/s/1THLmoYVM_zB5cLMVzPPmtg) |

- OpenBG-IMG数据集可通过参加阿里天池[CCKS2022面向数字商务的知识处理与应用评测任务三：多模态商品知识图谱链接预测](https://tianchi.aliyun.com/competition/entrance/531957/information)比赛获取，baseline代码请关注https://github.com/OpenBGBenchmark/OpenBG-IMG 。
- OpenBG(Full)数据集可以在[kg.alibaba.com](https://kg.alibaba.com/)申请获取

## 阿里天池评测

- OpenBG-IMG相关评测链接：https://tianchi.aliyun.com/competition/entrance/531957/information
- OpenBG500相关评测链接：https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271