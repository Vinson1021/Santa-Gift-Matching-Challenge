# Santa-Gift-Matching-Challenge

## 1 题目解读

### 1.1 赛题背景

这是2017年的圣诞前夜，圣诞老人变得易怒、刻薄。以前对老圣尼克来说很简单的事情，现在太令人费解了，它让他生病了!看，圣诞老人在内心深处总是知道每个孩子想要什么玩具——没有“如果”或“但是”。但是，人口的迅猛增长、双胞胎的增多以及玩具的创新，使得问题过于复杂，迫切需要优化。“别担心，圣诞老人先生，”一个叫麦克马格尔的小精灵说，“我有一个解决办法!”你听说过Kaggle吗?当她深入解释Kaggle的时候，圣诞老人的疑虑开始转变，他开始相信……机器学习。所以，今年圣诞老人的团队比以往任何时候都需要你，来解决这个痛苦的问题，拯救圣诞节的欢乐。

### 1.2 比赛详情

在这个比赛中，你面临的挑战是建立一个玩具匹配算法，通过将孩子和他们想要的玩具配对来最大化快乐。在数据集中，每个孩子偏好于1000个礼物中的10个，而圣诞老人对每个可用的礼物有1000个偏好的孩子。这个挑战额外的困难是：有0.4%的孩子是双胞胎，根据父母的要求，他们需要相同的礼物。

### 1.3 评价指标

目标是最大化
`Average Normalized Happiness (ANH) = (AverageNormalizedChildHappiness (ANCH) ) ^ 3 + (AverageNormalizedSantaHappiness (ANSH) ) ^ 3`

- NCH：归一化孩子幸福感，等于每个孩子的幸福感除以幸福感的最大可能值。NSH：归一化礼物幸福感，等于每个礼物的幸福感除以礼物幸福感的最大可能值。

<img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\fn_cs&space;\large&space;ANCH&space;=&space;\frac{1}{n_c}&space;\sum_{i=0}^{n_c-1}&space;\frac{ChildHappiness}{MaxChildHappiness}" title="\large ANCH = \frac{1}{n_c} \sum_{i=0}^{n_c-1} \frac{ChildHappiness}{MaxChildHappiness}" />

<img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\fn_cs&space;\large&space;ANSH&space;=&space;\frac{1}{n_g}&space;\sum_{i=0}^{n_g-1}&space;\frac{GiftHappiness}{MaxGiftHappiness}" title="\large ANSH = \frac{1}{n_g} \sum_{i=0}^{n_g-1} \frac{GiftHappiness}{MaxGiftHappiness}" />

- n_c是孩子的数量，n_g是礼物的数量，MaxChildHappiness = len(ChildWishList) * 2,MaxGiftHappiness = len(GiftGoodKidsList) * 2.
如果给孩子的礼物在孩子的愿望列表中，ChildHappiness = 2 * GiftOrder；否则ChildHappiness = -1。同样地，如果孩子在礼物的好孩子列表中GiftHappiness = 2 * ChildOrder否则GiftHappiness = -1.

- For example, if a child has a preference of gifts [5,2,3,1,4], and is given gift 3, then ChildHappiness = [len(WishList)-indexOf(gift_3)] * 2 = [5 - 2] * 2 = 6. If this child is given gift 4, then ChildHappiness = [5-4] * 2 = 2. 

- 如上述公式可知，ChildHappiness的计算过程如下：
1. 若礼物不在愿望列表，为-1；
2. 若礼物在愿望列表，同时与愿望列表的长度和礼物在愿望列表的顺序有关，由于孩子的愿望列表最多为10个，而且孩子的愿望列表长度不一样，所以每个孩子的幸福感的最大可能值由其愿望列表长度决定，为2 * len(WishList)，最小值在礼物为愿望列表最后一个时取到，为2。
- 计算Average Normalized Happiness的代码样例查看demo，来自https://www.kaggle.com/wendykan/average-normalized-happiness-demo

### 1.4 数据描述

#### 1.4.1 数据构成

- 1,000,000个孩子和他们的对1000个礼物的愿望列表
- 1000个礼物和他们倾向的1000个孩子列表

#### 1.4.2 注意点：

1. 前0.5%(ChildID 0-5000)的孩子是三胞胎。0、1、2是三胞胎,3,4,5,三胞胎....4998 4999 5000是三胞胎。三胞胎需要得到相同的礼物，即使他们可能有不同的喜好。（所以需要寻找他们愿望列表中相同的礼物）
2. 接下来的4% (ChildId 5001-45000)的孩子是双胞胎。5001和5002是双胞胎,5003和5004是双胞胎,....44999和45000是双胞胎。双胞胎需要相同的礼物，即使他们可能有不同的喜好。（所以需要寻找他们愿望列表中相同的礼物）
3. 对于每个GiftId，有1000个可用的礼物。礼物的数量完全相同(1000 * 1000 = 1,000,000)。每件礼物的数量不得超过1000件。即一共有1000种礼物，每一种有1000个，一共有1,000,000个礼物，与孩子的数量相同。
4. 按照1.3的评价指标Average Normalized Happiness计算分数。

#### 1.4.3 数据文件说明

1. child_wishlist_v2.csv：100kx101，每一行代表一个ChildID，后面是100个偏好的GiftID。
2. gift_goodkids_v2.csv：1000x1001，每一行代表一个giftID，后面是1000个偏好的ChildID
3. sample_submission_random_v2.csv：100kx2，随机生成的提交样例文件，第一列是ChildID，第二列是GiftID。

### 1.5 参考资料

1. 冠军：https://www.kaggle.com/c/santa-gift-matching/discussion/47376
2. 亚军：https://www.kaggle.com/c/santa-gift-matching/discussion/47386
3. 季军：https://www.kaggle.com/c/santa-gift-matching/discussion/47374
