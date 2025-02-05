# 如何通过 PayPal 实现自动订阅功能

自动订阅是现代电商系统中常见的功能，而 PayPal 提供了完整的解决方案。本文将详细介绍如何使用 PayPal 实现自动订阅，并分享一些开发中的注意事项。

## 自动订阅的实现步骤

PayPal 官方文档中提到的自动订阅流程可以分为以下五个步骤：

1. **创建并激活订阅计划**：首先需要创建一个订阅计划，并将其激活以支持后续的订阅操作。
2. **用户创建订阅**：用户选择订阅后，将被重定向到 PayPal 网站进行授权。
3. **用户授权订阅**：用户同意订阅后，系统将跳转回网站并执行订阅。
4. **获取账单信息**：系统需要处理每次扣款的结果通知，或主动查询支付状态。
5. **处理订阅变更**：支持用户取消订阅等操作，并处理相关通知。

## 使用 PayPal SDK

为了简化开发，PayPal 提供了官方的 SDK。通过以下命令即可安装 PHP 版本的 SDK：

bash
composer require paypal/rest-api-sdk-php


官方还提供了完整的 [Samples](https://paypal.github.io/PayPal-PHP-SDK/sample/#billing)，开发者可以直接参考。此外，[PayPal Sandbox](https://www.sandbox.paypal.com/) 是调试自动订阅功能的好工具。

## 创建订阅计划并激活

在实现订阅功能时，有几个关键点需要注意：

- **订阅计划（Billing Plan）**：每个计划对应一个产品，不同价格需要创建不同的计划。同时，可以为不同用户创建协议时进行调整。
- **试用期支付**：如果设置了 `TRIAL` 类型的支付，必须同时存在 `REGULAR` 类型的支付。试用期的优惠逻辑需要业务代码自行实现。
- **协议生效时间**：由于用户订阅协议的生效时间必须为当前时间 24 小时之后，因此首次扣款通常需要手动设置。可以通过 `MerchantPreferences` 的 `setSetupFee` 方法设置首次扣款的费用。
- **Paypal SDK 错误处理**：如果在开发中遇到 `"NotifyUrl" value is NULL` 的错误，可以参考 [官方 issue](https://github.com/paypal/PayPal-PHP-SDK/pull/1152/files) 进行解决。

## 创建订阅

用户在创建订阅时，以下几点需要注意：

- **多个订阅协议**：用户可以为同一个订阅计划创建多个订阅协议。系统需要在用户同意后跳转回网站时，提取 `token` 以对应协议信息。
- **协议开始时间**：由于 `start_date` 最早为当前时间 24 小时后，因此该值实际上是第二次扣款的时间。首次扣款可以通过 `setSetupFee` 设置。
- **处理重复订阅**：同一个用户可以多次订阅同一计划，因此在创建新协议时需要手动取消之前的协议。

php
$link = $agreement->getApprovalLink();
parse_str(parse_url($link, PHP_URL_QUERY), $params);
$token = $params['token'];


## 订阅管理与扣款通知

- **扣款延迟**：PayPal 的实际扣款时间可能会比 `AgreementDetail.next` 显示的时间晚几个小时。建议设置提前一天扣款，以保证连续性。
- **Webhook 通知**：可以在 `My Apps -> REST API apps -> WEBHOOKS` 中设置 `webhook`。每次扣款成功后，PayPal 会发送 `PAYMENT.SALE.COMPLETED` 的事件通知。通过其中的 `billing_agreement_id` 字段，可以匹配对应的订阅协议。
- **交易查询**：可以通过 `Agreement::searchTransactions` 方法查询协议的所有交易。由于扣款可能会延迟，建议多次重试以确保数据准确性。

👉 [WildCard | 一分钟注册，轻松订阅海外线上服务](https://bbtdd.com/WildCard)