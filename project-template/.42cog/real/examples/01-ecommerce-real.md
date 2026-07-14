---
name: ecommerce-real
description: Real-world constraints for e-commerce platform including security, payment, inventory and legal compliance
---

# 电商系统 - 现实约束 (Real)

<real>
- 用户支付信息必须通过第三方支付平台处理，系统不得存储银行卡号
- 库存扣减必须使用数据库行锁或分布式锁，防止超卖
- 订单支付后48小时内必须发货（预售商品除外）
- 商品图片必须使用CDN加速，首页加载时间必须小于2秒
</real>
