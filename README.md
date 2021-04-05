# 2021-GAIIC-phase1-idea

干货~
现在分数0.92的模型配置是bert(916)+bert(910)+bert(907)，三个模型加权融合
传统深度学习一直搞不好，果断弃坑去调bert，发现还是效果挺不错的，看大家讨论似乎bert上不去分，给几个trick

0. 最主要的就是，让bert预训练的时候mlm欠拟合，来应对fine-tuning时候带来的过拟合，尽量让bert在Mlm阶段train少点
1. BERT-base(从头训练)基础分（无任何trick）是0.904，大概在这个比赛刚开始的时候就能在做到，后边一直卡在这个分的原因就是尝试传统模型失败
2. 动态MASK + NGRAM_MASK (2k)
3. Finetuning PGD & FGM (4k)
4. Lookahead + SWA (1k)
5. 做完上边的基本上就有个910左右了，换几个模型如NEZHA-LARGE, NEZHA-BASE加权融合应该就可以到915左右了
6. 不要开FP16，一定不要开

Other
1. 手写了一个验证集mlogloss大于0.97x就早停的callbacks，省得手动停
2. 模型融合收益非常高，且这题只适用于加权平均，我试了stacking效果不行，大家可以尝试一下把之前手里的垃圾模型加权看看收益
3. 对2的进一步猜测是，mlogloss对数值敏感，加权后降低方差，让数值更稳定
