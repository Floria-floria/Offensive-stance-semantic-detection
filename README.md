## 项目背景：
- 文本评论针对某个具体的话题，结合上下文判断句子立场是否有冒犯含义
- 每句话由2人打标 0-2，已知的是总标签由两人得分加总  
**特点**：
- 立场分类比情感分类更隐晦，且涉及和话题有关，人工判断尚且存在主观认知导致准确率不高的情况
- 问题较为新颖，目前没有特别多成熟的方法


## 任务：提高多分类宏观F1, baseline 0.33(基于RoBerta，代码在colab上用GPU实现)

## tricks:
- try:
    - 分别尝试了三塔模型（F1：提升5%）和训练两个3分类器融合（F1：提升6%），以模拟两人打分的场景【奇数得分随机拆分效果更好】
    - 三塔模型：
      - 尝试在三塔模型加入两塔输出和句子信息的乘积attenion，以拉开两个并行3分类间的差距（F1效果无提升）
      - 加入早停策略，使用focal_loss改善样本分布不均衡(F1提升4%)
      - 尝试longformer（F1不升反降）
      - 最终宏观F1：42%
    - 模型融合模拟两人打分：
      - 使用两阶段loss: 2epoch用focal loss，2epoch用cross entropy loss（F1提升2%）
      - 最终宏观F1: 41%
- todo:
    - 爬取更多外网语料，丰富roberta预训练语料库，进行further-pretrain（爬虫难度限制和gpu内存限制，未能实现，作为一个可优化方向）
