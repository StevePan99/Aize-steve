# Meta(Facebook) 事件上报接口与官方资料索引

> **适用公司**：AZSL / 艾泽 / AITRI
> **文档定位**：汇总 Meta/Facebook 事件上报相关的接口形态、官方文档入口、阅读顺序与接入要点
> **适用场景**：网站转化上报、服务端转化回传、App 事件上报、CRM / 线下转化回传、事件排错与验收
> **相关文档**：`ad-data-analytics-technical-plan.md` 负责内部数据分析底座，本文档聚焦 Meta 官方接口与资料索引

---

## 一、背景与目标

Meta 的事件上报资料并不是一篇文档就能覆盖全部场景，而是按上报方式拆成多组文档：

- 网站前端事件主要看 `Meta Pixel`
- 服务端事件主要看 `Conversions API`
- App 场景主要看 `App Events`
- 线下 / CRM / 业务消息等场景有各自的扩展文档

如果不先把资料入口整理清楚，后续很容易出现几个问题：

- 不知道应该看 `Pixel` 还是 `Conversions API`
- 只看总览，不看参数和去重说明，导致落地后重复计数
- Web、App、Offline 三类资料混在一起，阅读效率很低
- 排错时不知道该去 `Verifying Setup`、`Deduplication` 还是 `Reference`

这份文档的目标是：

- 把 Meta 事件上报相关资料集中到一个地方
- 说明不同上报方式分别适合什么场景
- 给出最推荐的阅读顺序
- 补充一份内部接入时最常用的要点清单

---

## 二、推荐结论

对大多数广告转化追踪场景，建议这样理解：

1. 网站浏览器事件优先看 `Meta Pixel`
2. 网站后端转化回传优先看 `Conversions API`
3. 如果网站要做完整追踪，推荐 `Pixel + Conversions API` 同时接，并用 `event_id` 去重
4. App 场景优先看 `App Events` 和 `Conversions API for App Events`
5. CRM、线下成交、销售线索等后端数据回传，优先看 `Offline Events` 或 `Conversion Leads Integration`

一句话说：

`Pixel` 管浏览器端行为，`Conversions API` 管服务端回传，`App Events` 管移动端事件，`Offline Events` 管线下和 CRM 转化。

---

## 三、方案边界

### 3.1 本文档包含

- Meta 事件上报接口形态总览
- 官方文档入口整理
- 各类资料的适用场景说明
- 推荐阅读顺序
- 接入与验收时的常用检查点

### 3.2 本文档不包含

- 具体业务埋点方案设计
- 内部事件口径定义和数据库表设计
- Meta 广告账户开户、权限和资产绑定细节

---

## 四、接口形态总览

### 4.1 网站前端事件：`Meta Pixel`

适用场景：

- 页面浏览
- 按钮点击
- `ViewContent`
- `Lead`
- `Purchase`
- `CompleteRegistration`

接口形态：

- 浏览器侧 `JavaScript` 埋点
- 通过 `fbq()` 上报
- 常配合标准事件和自定义事件使用

特点：

- 接入快
- 能抓到页面级行为
- 容易受浏览器限制、拦截器和隐私策略影响

### 4.2 网站后端事件：`Conversions API`

适用场景：

- 注册成功
- 支付成功
- 下单成功
- 首充成功
- CRM 中确认成交

接口形态：

- `HTTPS`
- `POST`
- `JSON`
- 服务端直连 Meta

典型接口风格：

- `POST https://graph.facebook.com/{api_version}/{pixel_id}/events`

特点：

- 更适合真实业务结果回传
- 更稳定
- 更适合和订单、支付、CRM、风控系统打通

### 4.3 App 事件：`App Events`

适用场景：

- App 安装
- App 激活
- 注册
- 付费
- 订阅
- 应用内行为

接口形态：

- SDK 方式
- Codeless App Events
- 部分场景可结合服务端或 `Conversions API for App Events`

特点：

- 更适合移动应用归因和优化
- 常与 `AppsFlyer`、`Adjust` 等 MMP 配合

### 4.4 线下 / CRM 转化：`Offline Events`

适用场景：

- 线下成交
- 电话销售转化
- CRM 阶段推进
- 门店转化

接口形态：

- 服务端批量或系统对接
- 偏后端数据回传

### 4.5 线索回传：`Conversion Leads Integration`

适用场景：

- Lead Ads
- CRM 线索状态回传
- 线索下载与销售漏斗回传

接口形态：

- CRM 集成
- 开发者集成
- 部分场景支持无代码或自动化接入

---

## 五、官方资料索引

### 5.1 `Conversions API` 核心资料

| 文档 | 作用 | 链接 |
|------|------|------|
| `Conversions API Overview` | 先看总览，理解 CAPI 是什么、解决什么问题 | https://developers.facebook.com/documentation/ads-commerce/conversions-api |
| `Get Started` | 看接入前准备和最小落地步骤 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/get-started |
| `Using the API` | 看 API 调用方式和实际调用逻辑 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/using-the-api |
| `Verifying Setup` | 看如何验证事件是否成功送达 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/verifying-setup |
| `Parameters` | 参数总入口 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters |
| `Main Body Parameters` | 请求体顶层字段 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/main-body |
| `Server Event Parameters` | 事件级字段，如 `event_name`、`event_time`、`action_source` | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/server-event |
| `Customer Information Parameters` | 用户标识字段，如邮箱、手机号、IP、UA | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/customer-information-parameters |
| `External ID` | `external_id` 的使用方式 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/external-id |
| `fbp and fbc Parameters` | 浏览器侧标识与广告点击参数 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/fbp-and-fbc |
| `Standard Parameters` | `custom_data` 等业务参数说明 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/custom-data |
| `Original Event Data` | 原始事件补充字段说明 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/parameters/original-event |
| `Handling Duplicate Pixel and Conversions API Events` | 讲 `event_id` 去重，`Pixel + CAPI` 必看 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/deduplicate-pixel-and-server-events |

### 5.2 `Meta Pixel` 核心资料

| 文档 | 作用 | 链接 |
|------|------|------|
| `Meta Pixel - Get Started` | Pixel 接入起点 | https://developers.facebook.com/docs/meta-pixel/get-started |
| `Conversion Tracking` | 标准事件、自定义事件、自定义转化的主文档 | https://developers.facebook.com/docs/meta-pixel/implementation/conversion-tracking |
| `Reference` | 标准事件和参数参考手册 | https://developers.facebook.com/docs/meta-pixel/reference |
| `Pixel for the Marketing API` | 面向广告投放使用 Pixel 的说明 | https://developers.facebook.com/docs/meta-pixel/implementation/marketing-api |

### 5.3 `App Events` 核心资料

| 文档 | 作用 | 链接 |
|------|------|------|
| `Meta App Events - Overview` | App 事件总览 | https://developers.facebook.com/docs/app-events/overview |
| `Getting Started` | App Events 接入起点 | https://developers.facebook.com/docs/app-events/getting-started |
| `Get Started with App Events (Android)` | Android 接入说明 | https://developers.facebook.com/docs/app-events/getting-started-app-events-android |
| `Get Started with App Events on iOS` | iOS 接入说明 | https://developers.facebook.com/docs/app-events/getting-started-app-events-ios |
| `Reference` | App 事件 API / 字段参考 | https://developers.facebook.com/docs/app-events/reference |
| `FAQ` | 事件数量、参数限制、测试方式等常见问题 | https://developers.facebook.com/docs/app-events/faq |
| `Conversions API for App Events` | App 场景的服务端事件回传说明 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/app-events |

### 5.4 `Offline / CRM / 业务消息` 扩展资料

| 文档 | 作用 | 链接 |
|------|------|------|
| `Conversions API for Offline Events` | 线下和离线转化回传 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/offline-events |
| `Conversions API for Business Messaging` | 业务消息相关转化场景 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/business-messaging |
| `Conversion Leads Integration` | Lead Ads 与 CRM 线索回传 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/conversion-leads-integration |
| `Payload Specification` | Lead / CRM 集成时的载荷说明 | https://developers.facebook.com/documentation/ads-commerce/conversions-api/conversion-leads-integration/payload-specification |

---

## 六、不同场景应该先看什么

### 6.1 网站转化追踪

推荐阅读顺序：

1. `Meta Pixel - Get Started`
2. `Conversion Tracking`
3. `Conversions API Overview`
4. `Get Started`
5. `Parameters`
6. `Handling Duplicate Pixel and Conversions API Events`
7. `Verifying Setup`

适用目标：

- 网站注册
- 留资
- 支付
- 表单转化

### 6.2 网站服务端回传

推荐阅读顺序：

1. `Conversions API Overview`
2. `Using the API`
3. `Main Body Parameters`
4. `Server Event Parameters`
5. `Customer Information Parameters`
6. `fbp and fbc Parameters`
7. `Verifying Setup`

适用目标：

- 支付结果回传
- CRM 成交回传
- 自建后端服务对接

### 6.3 App 归因和应用内事件

推荐阅读顺序：

1. `Meta App Events - Overview`
2. `Getting Started`
3. `Android` 或 `iOS` 接入文档
4. `Reference`
5. `FAQ`
6. `Conversions API for App Events`

适用目标：

- App 安装
- 激活
- 注册
- 订阅
- 应用内购买

### 6.4 线下 / CRM / Lead 场景

推荐阅读顺序：

1. `Conversions API for Offline Events`
2. `Conversion Leads Integration`
3. `Payload Specification`
4. `Verifying Setup`

适用目标：

- 线索推进
- 销售成交
- 门店 / 电话 / CRM 回传

---

## 七、开发接入时最常看的字段和概念

### 7.1 Web / CAPI 场景

最常用字段：

- `event_name`
- `event_time`
- `action_source`
- `event_source_url`
- `event_id`
- `user_data`
- `custom_data`
- `fbp`
- `fbc`
- `external_id`

重点概念：

- `event_id`
  - 用于 `Pixel + CAPI` 去重
- `user_data`
  - 用于用户匹配质量
- `fbp / fbc`
  - 用于浏览器和广告点击标识
- `custom_data`
  - 用于业务值、金额、币种、商品等信息

### 7.2 Pixel 场景

最常见标准事件：

- `PageView`
- `ViewContent`
- `Lead`
- `CompleteRegistration`
- `InitiateCheckout`
- `AddToCart`
- `Purchase`

### 7.3 App 场景

重点关注：

- 自动事件记录
- 手动事件记录
- App Ads Helper / Test Events
- SDK 版本要求
- 事件命名和参数限制

### 7.4 `Conversions API` 请求格式示例

最常见的服务端上报方式是：

- 请求方法：`POST`
- 协议：`HTTPS`
- 地址格式：`https://graph.facebook.com/{api_version}/{pixel_id}/events`
- 内容类型：`application/json`
- 鉴权方式：`access_token`

一个简化但接近真实生产的请求示例如下：

```json
{
  "data": [
    {
      "event_name": "Purchase",
      "event_time": 1716000000,
      "action_source": "website",
      "event_source_url": "https://example.com/checkout/success",
      "event_id": "order_20260504_10001",
      "user_data": {
        "em": ["5ff860bf1190596c7188ab851db691f0f3169c45393619211d3b4730418b7d7f"],
        "ph": ["8a59780bb8cd2ba022bfa5ba2ea3b6e07af17a7d8b30c1f9b3390e36f69019e4"],
        "client_ip_address": "203.0.113.10",
        "client_user_agent": "Mozilla/5.0",
        "fbp": "fb.1.1716000000.1234567890",
        "fbc": "fb.1.1716000000.AbCdEfGhIjKlMnOpQrStUvWxYz",
        "external_id": ["user_987654"]
      },
      "custom_data": {
        "currency": "USD",
        "value": 19.99,
        "order_id": "10001",
        "content_type": "product",
        "content_ids": ["sku_1001"],
        "contents": [
          {
            "id": "sku_1001",
            "quantity": 1,
            "item_price": 19.99
          }
        ]
      }
    }
  ],
  "test_event_code": "TEST12345"
}
```

如果用 `curl` 表达，通常长这样：

```bash
curl -X POST "https://graph.facebook.com/v23.0/<PIXEL_ID>/events?access_token=<ACCESS_TOKEN>" \
  -H "Content-Type: application/json" \
  -d @payload.json
```

### 7.5 核心字段说明与常见取值

#### 7.5.1 顶层字段

| 字段 | 类型 | 是否常用 | 说明 |
|------|------|------|------|
| `data` | Array | 是 | 事件数组，可一次发送一个或多个事件 |
| `test_event_code` | String | 常用于联调 | 测试事件代码，用于 Events Manager 验证 |
| `partner_agent` | String | 可选 | 标识接入方或中间层实现 |
| `namespace_id` | String | 少见 | 特定集成场景下使用 |

#### 7.5.2 事件主体字段

| 字段 | 类型 | 是否推荐 | 说明 | 常见取值 |
|------|------|------|------|------|
| `event_name` | String | 必填 | 事件名称 | `Purchase`、`Lead`、`CompleteRegistration`、`AddToCart` |
| `event_time` | Integer | 必填 | 事件发生时间，Unix 秒级时间戳 | `1716000000` |
| `action_source` | String | 必填 | 事件来源类型 | `website`、`app`、`phone_call`、`chat`、`email`、`physical_store`、`system_generated`、`other` |
| `event_source_url` | String | Web 场景推荐 | 事件对应页面 URL | `https://example.com/signup/success` |
| `event_id` | String | 强烈推荐 | 去重标识，`Pixel + CAPI` 联用时很关键 | `order_10001`、`reg_user_888` |
| `opt_out` | Boolean/Integer | 可选 | 是否选择不用于广告用途 | `0`、`1` |

#### 7.5.3 `user_data` 常见字段

| 字段 | 类型 | 是否推荐 | 说明 | 备注 |
|------|------|------|------|------|
| `em` | Array[String] | 强烈推荐 | 邮箱哈希值 | 通常先标准化再做 `SHA-256` |
| `ph` | Array[String] | 强烈推荐 | 手机号哈希值 | 通常先标准化再做 `SHA-256` |
| `client_ip_address` | String | Web 推荐 | 客户端 IP | 不需要哈希 |
| `client_user_agent` | String | Web 推荐 | 浏览器 UA | 不需要哈希 |
| `fbp` | String | 强烈推荐 | 浏览器端 Pixel 标识 | 便于提升匹配质量 |
| `fbc` | String | 强烈推荐 | Facebook Click ID 标识 | 来自广告点击链路 |
| `external_id` | Array[String] | 推荐 | 内部用户 ID | 建议稳定且可追踪 |
| `fn` | Array[String] | 可选 | 名字哈希值 | 常用于增强匹配 |
| `ln` | Array[String] | 可选 | 姓氏哈希值 | 常用于增强匹配 |
| `ct` | Array[String] | 可选 | 城市哈希值 | 常用于增强匹配 |
| `country` | Array[String] | 可选 | 国家哈希值或标准值 | 以官方参数说明为准 |

补充说明：

- `em`、`ph`、`fn`、`ln` 等个人标识字段，通常需要按 Meta 规范预处理后再做 `SHA-256`
- `client_ip_address`、`client_user_agent`、`fbp`、`fbc` 一般不做哈希
- `user_data` 字段越完整，通常匹配质量越高，但前提是合规采集和合规使用

#### 7.5.4 `custom_data` 常见字段

| 字段 | 类型 | 是否推荐 | 说明 | 常见取值 |
|------|------|------|------|------|
| `currency` | String | 金额类事件必填/强烈推荐 | 币种，通常为 3 位 ISO 码 | `USD`、`IDR`、`INR` |
| `value` | Number | 金额类事件必填/强烈推荐 | 转化金额 | `19.99` |
| `order_id` | String | 推荐 | 订单号 | `10001` |
| `content_type` | String | 推荐 | 内容类型 | `product`、`product_group` |
| `content_ids` | Array[String] | 推荐 | 商品或内容 ID 数组 | `["sku_1001"]` |
| `contents` | Array[Object] | 电商推荐 | 明细对象数组 | 包含 `id`、`quantity`、`item_price` |
| `num_items` | Integer | 可选 | 商品数量 | `1`、`2` |
| `search_string` | String | 搜索事件常用 | 搜索关键词 | `top up` |
| `status` | String | 线索 / CRM 常用 | 业务状态 | `qualified`、`converted` |

#### 7.5.5 常见标准事件示例

| 事件名 | 含义 | 常见最小字段 |
|------|------|------|
| `PageView` | 页面访问 | `event_name`、`event_time` |
| `ViewContent` | 查看内容页/商品页 | `event_name`、`event_time`、`content_ids` 或内容参数 |
| `Lead` | 留资/线索提交 | `event_name`、`event_time`，可补 `value`、`currency` |
| `CompleteRegistration` | 完成注册 | `event_name`、`event_time` |
| `AddToCart` | 加购 | `event_name`、`event_time`、`content_ids`、`value`、`currency` |
| `InitiateCheckout` | 发起结算 | `event_name`、`event_time`、`value`、`currency` |
| `Purchase` | 完成购买 | `event_name`、`event_time`、`currency`、`value` |

### 7.6 `Pixel + CAPI` 去重示例

如果浏览器端和服务端都上报同一次购买事件，推荐两边使用同一个 `event_id`。

浏览器侧 Pixel 示例：

```html
<script>
  fbq("track", "Purchase",
    {
      currency: "USD",
      value: 19.99
    },
    {
      eventID: "order_20260504_10001"
    }
  );
</script>
```

服务端 CAPI 示例：

```json
{
  "data": [
    {
      "event_name": "Purchase",
      "event_time": 1716000000,
      "action_source": "website",
      "event_id": "order_20260504_10001",
      "user_data": {
        "em": ["5ff860bf1190596c7188ab851db691f0f3169c45393619211d3b4730418b7d7f"]
      },
      "custom_data": {
        "currency": "USD",
        "value": 19.99
      }
    }
  ]
}
```

这组配置的关键点是：

- 两边的 `event_name` 一致
- 两边的 `event_id / eventID` 一致
- 浏览器事件负责补充前端行为
- 服务端事件负责保证关键转化不丢

### 7.7 一个更贴近业务的注册事件示例

如果是“注册成功”场景，可以考虑上报成：

```json
{
  "data": [
    {
      "event_name": "CompleteRegistration",
      "event_time": 1716000100,
      "action_source": "website",
      "event_source_url": "https://example.com/register/success",
      "event_id": "register_user_987654",
      "user_data": {
        "external_id": ["user_987654"],
        "em": ["5ff860bf1190596c7188ab851db691f0f3169c45393619211d3b4730418b7d7f"],
        "client_ip_address": "203.0.113.10",
        "client_user_agent": "Mozilla/5.0",
        "fbp": "fb.1.1716000000.1234567890"
      },
      "custom_data": {
        "registration_method": "phone",
        "country": "ID",
        "game_code": "game_a"
      }
    }
  ]
}
```

这个例子里：

- `event_name` 表示业务动作是“完成注册”
- `event_id` 用来做去重和排查
- `external_id` 用来和内部用户系统做映射
- `registration_method`、`country`、`game_code` 属于业务扩展字段

---

## 八、最推荐的内部阅读和落地方式

对 AZSL 当前阶段，更推荐下面这套做法：

1. 网站场景优先采用 `Pixel + Conversions API`
2. 关键转化结果以服务端事件为准
3. 接口开发前，先统一标准事件名和内部事件口径
4. 实施阶段同步规划 `event_id` 去重和上报日志
5. 联调阶段优先使用 `Verifying Setup`、`Test Events`、`Reference`

推荐内部最小交付清单：

- 标准事件清单
- 请求字段映射表
- `event_id` 生成规则
- 哈希规则说明
- 测试事件流程
- 回执 / 错误日志记录方式

---

## 九、常见误区

- 只接 `Pixel`，不接服务端回传
- 同时接 `Pixel + CAPI`，但没做 `event_id` 去重
- 只看总览，不看参数文档
- 把 Web、App、Offline 三种资料混在一起
- 没有做验证和测试就直接上线

---

## 十、最终建议

- 如果你的目标是“网站转化追踪”，先看 `Pixel + Conversions API`
- 如果你的目标是“后端真实结果回传”，重点看 `Conversions API`
- 如果你的目标是“App 事件”，重点看 `App Events`
- 如果你的目标是“CRM / 线索 / 线下成交回传”，重点看 `Offline Events` 和 `Conversion Leads Integration`
- 后续内部最好再补一份“Meta 事件字段映射表”和“标准事件命名规范”