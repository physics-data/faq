# 迟交处理

经过课程组讨论，对于迟交作业的评分按照如下规则进行：

助教将会选取 deadline 之前的最后一次 commit 的版本进行黑盒和白盒的评分。如果你未能够在截止时间前提交作业，或者想更换评测用的版本，请给教学团队邮箱 <physics-data+assignments@tuna.tsinghua.edu.cn> 发邮件重新标记最终版本（原则上每个作业只允许更换一次），邮件模板如下（请使用 Markdown 格式，即在两行文本间空一行）：

```markdown
姓名：续本达

学号：2018888888

Git 用户名：orv

作业名称：aplusb

标记为最终版本的 commit id：fe19a62be71616fde0846209ef3b0615f694a8ad

提交时间：2022/7/12 17:17:20

提交链接：https://git.tsinghua.edu.cn/physics-data/2022/aplusb/aplusb-orv/-/commit/fe19a62be71616fde0846209ef3b0615f694a8ad
```

当助教处理请求后，会在网络学堂更新你的分数，并回复你的对应邮件。

除非特别说明，迟交作业的得分 $S'$ 按照如下公式计算：

$$ S' = S \times \min(0.9, 0.95^D) $$

其中 $S$ 是作业原始得分， $D$ 是向上取整的迟交天数（即 deadline 后即记为迟交一天）。

例如，作业的 deadline 是 7/13，则 7/20 补交的作业总分将被折合为原始作业评分的 $69.8\%$。