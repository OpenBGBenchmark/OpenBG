# OpenBG：Large Scale Open Business Knowledge Graph

[OpenBG](https://kg.alibaba.com/) is an open business knowledge graph that utilizes a unified Schema covering multi-modal datasets in large scale, which contains millions of products and consumer demand provided by ZJUKG Lab of Zhejiang University and the group of Alibaba Knowledge Engine.

## Building process of OpenBG benchmarks
<center><img src="./img/OpenBG.png" width="80%"></center>

## Datasets

Summary statistics of OpenBG datasets:
|    Dataset    |    # Ent   | # Rel |   # Train   |  # Dev  | # Test  |
| ------------- | ---------- | ----- | ----------- | ------- | ------- |
|   OpenBG-IMG  | 27,910     |  136  | 230,087     | 5,000   | 14,675  |
|   OpenBG500   | 249,743    |  500  | 1,242,550   | 5,000   |  5,000  |
|   OpenBG500-L | 2,782,223  |  500  | 47,410,032  | 696,839 | 700,321 |
|  OpenBG(Full) | 88,881,723 | 2,681 | 260,304,683 |    -    |    -    |

### OpenBG500

OpenBG500 contains 500 relations, which is filtered and sampled from OpenBG(Full).

#### Directory Tree
```shell
OpenBG500
├── train.tsv					# Training set
├── valid.tsv					# Validation set
├── test.tsv					# Test set
├── entity2text.tsv				# Description of entities in Chinese
└── relation2text.tsv			# Description of relations in Chinese
```

#### Check the data

```
$ head -n 3 train.tsv
<http://ali.openkg.cn/alischema#Product/pid_95366c60c08b45d8931d2f459712ff12>	<http://ali.openkg.cn/alischema#Property/External_Material>	苦荞麦
<http://ali.openkg.cn/alischema#Product/pid_6c3646395ec10143cdc8de645aeb93a9>	<http://ali.openkg.cn/alischema#Property/Flavor>	原味硬糕850克【10包40块糕】
<http://ali.openkg.cn/alischema#Product/pid_a31f0d7ce23fbb80f250ec5b2dd2bc82>	<http://ali.openkg.cn/alischema#Property/inMarket>	<http://ali.openkg.cn/alischema#Market_segment/tag_0b256e3b23f5b7a9d006f87d28ce0ead>
```

#### Use Python to transfer and read the data

Get the map of Entity(Relatioin)-Description: `ent2text` and `rel2text`:
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

Transfer the data to description: 
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

OpenBG500-L contains 500 relations, larger than OpenBG500, which is also filtered and sampled from OpenBG(Full).

#### Directory Tree
```shell
OpenBG500-L
├── train.tsv					# Training set
├── valid.tsv					# Validation set
└── test.tsv					# Test set
```

#### Check the data

```
$ head -n 3 train.tsv 
<http://ali.openkg.cn/alischema#Product/pid_ca08a196fd0388a37d2b671826b4456f>	<http://ali.openkg.cn/alischema#Property/Imported>	否
<http://ali.openkg.cn/alischema#Product/pid_39eee17628071aa99e2d45be140d5cb7>	<http://ali.openkg.cn/alischema#Property/Applicable_Season>	夏季
<http://ali.openkg.cn/alischema#Product/pid_549c825765a62f6b8c6b53836d61963c>	<http://ali.openkg.cn/alischema#Property/placeOfOrigin>	<http://ali.openkg.cn/alischema#Place_Of_Origin/中国大陆>
```

#### Use Python to read the data

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

## How to download

- Download link：

| Dataset       |   Google Drive   | Baidu Netdisk | 
| ------------- | ---------------- | ------ |
| OpenBG500     |    [Download](https://drive.google.com/file/d/1EC3UzuC9o1jQNiteICUInPfTvHz7aKSp/view?usp=sharing)    |   [Code: wi7s](https://pan.baidu.com/s/1Arm4Jbw5HgY2-jHT2THu8g) |
| OpenBG500-L   | [Download](https://drive.google.com/file/d/1A9K_AOEMyzQUlAc7cRBlGXr8CBjUWOtV/view?usp=sharing) | [Code: bgq2](https://pan.baidu.com/s/1THLmoYVM_zB5cLMVzPPmtg) |

- OpenBG-IMG is available at [CCKS2022 Knowledge Processing and Application Evaluation for Digital Commerce Task 3: Multimodal Commodity Knowledge Graph Link Prediction](https://tianchi.aliyun.com/competition/entrance/531957/information), and the source code of baselines is https://github.com/OpenBGBenchmark/OpenBG-IMG .
- OpenBG(Full) is available at [kg.alibaba.com](https://kg.alibaba.com/).

## Evaluation online

- OpenBG-IMG: https://tianchi.aliyun.com/competition/entrance/531957/information
- OpenBG500: https://tianchi.aliyun.com/dataset/dataDetail?dataId=122271