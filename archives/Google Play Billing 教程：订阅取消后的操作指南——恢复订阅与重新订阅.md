# Google Play Billing 教程：订阅取消后的操作指南——恢复订阅与重新订阅

> **作者**：高寒蕊，Google 开发技术推广工程师  
> **系列简介**：本系列文章专为中文开发者设计，旨在解答 Google Play Billing 中常见的疑问。如果您有任何问题，欢迎在评论区提出，我们将在后续文章中为您解答。

订阅模式在应用和游戏的盈利中扮演着重要角色。对于已经实现订阅或计划接入 Play 订阅服务的开发者来说，用户在取消订阅后的操作尤其值得关注。本文将深入探讨**恢复订阅**和**重新订阅**的区别及处理注意事项。

## 恢复订阅 vs 重新订阅：本质区别

许多中文开发者对 Play 订阅中的 **Restore（恢复订阅）** 和 **Resubscribe（重新订阅）** 功能感到困惑。事实上，这两者在本质上有显著区别：

1. **恢复订阅 (Restore)**  
   - **特点**：恢复未过期的已取消订阅。  
   - **条件**：  
     - 用户已取消订阅，但订阅未过期。  
     - 操作仅限于 Play 订阅中心。  
   - **结果**：恢复后，`purchaseToken` 保持不变。

2. **重新订阅 (Resubscribe)**  
   - **特点**：本质上是一次新的订阅购买。  
   - **条件**：  
     - 可从应用内或 Play 订阅中心发起（适用于过期不满一年的订阅）。  
     - 过期超过一年后，只能从应用内重新购买。  
   - **结果**：每次重新订阅都会生成一个新的 `purchaseToken`。

### 关键区别对比表

| 操作类型     | 恢复订阅 (Restore) | 重新订阅 (Resubscribe) |
|--------------|-------------------|------------------------|
| 订阅状态     | 未过期             | 过期或未过期           |
| 操作发起位置 | Play 订阅中心      | 应用内或 Play 订阅中心 |
| `purchaseToken` | 保持不变         | 生成新的 `purchaseToken` |

## 特别注意事项

### 1. 新订阅替换旧订阅的场景
当用户取消的订阅尚未到期，但再次购买时，新订阅将替换旧订阅，并在同一到期日期续订。开发者需注意：  
- **旧订阅的 `purchaseToken`** 需及时标记为失效，避免同一用户对同一 SKU 持有多个有效 `purchaseToken`。  
- **`linkedPurchaseToken` 字段**：在新订阅中会记录与之关联的旧订阅的 `purchaseToken`。

### 2. 订阅有效期验证
开发者不应依赖 `linkedPurchaseToken` 是否存在来判断订阅是否有效。应通过 **Google Developer API** 的 `expiryTimeMillis` 字段获取订阅的准确到期时间。

### 3. 重新订阅的交易确认
对于重新订阅的交易，开发者需及时确认购买。尤其是使用 **Play Billing Library 2.0** 及以上版本的开发者，任何未在三天内确认的交易将被 Play 取消。

## 相关资源

- **Google Developer API 订阅接口**  
  [查看文档](https://developers.google.google.cn/android-publisher/api-ref/rest/v3/purchases.subions/get)  
- **销售订阅官方指南**  
  [查看文档](https://developer.android.google.cn/google/play/billing/subs)  
- **实时开发者通知参考指南**  
  [查看文档](https://developer.android.google.cn/google/play/billing/rtdn-reference)

## 结语

希望通过本文，您能够清晰区分恢复订阅与重新订阅的操作及处理方式。如果您对订阅功能仍有疑问，欢迎在评论区提出您的问题。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)