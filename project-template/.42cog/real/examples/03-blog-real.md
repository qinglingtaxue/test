---
name: blog-real
description: Real-world constraints for blog platform including content moderation, copyright protection, anti-spam and data privacy
---

# 博客系统 - 现实约束 (Real)

<real>
- 用户密码必须使用bcrypt或argon2加密存储，用户邮箱和手机号必须脱敏显示
- 文章和评论必须经过敏感词过滤，新用户评论需人工审核，违规内容24小时内删除
- 原创文章需标注"原创"，转载文章需标注出处，收到侵权投诉后48小时内处理
- API必须限流（单IP 100次/分钟），同一IP每分钟最多发布3条评论，检测并阻止刷量行为
</real>
