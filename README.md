<div align="center">

# 📊 Monitor · API 额度监控

聚合查看你在各家 AI 平台与 New API 中转站的账户余额、模型定价与调用记录。

**Kotlin · Jetpack Compose · Material 3**

</div>

---

## 📖 项目简介

**Monitor（API 额度监控）** 是一款**纯本地运行**的 Android 工具应用，帮助开发者一站式管理多个 AI 厂商的 API 额度，不必在多个控制台之间来回切换。

- **只读请求**：全部接口均为只读 `GET`，不消耗任何模型额度
- **凭据不出本机**：API Key 用 Android Keystore（AES-256-GCM）加密存储，不经过任何第三方服务器
- **无需账号体系**：不注册、不上传、无埋点
- **接不上的站点能自己接**：内置 7 类平台，另有「自定义查询」支持自填接口与解析规则

界面采用 **Monet 2026 配色**与 **iOS26 Liquid Glass 液态玻璃**设计语言，深度适配 OnePlus 15（6.78" / 2772×1272 / 450PPI）。

---

## ✨ 功能特性

### 📊 多厂商额度聚合
首页以卡片展示各账户的**余额 / 状态 / 品牌图标**，异常（透支、查询失败）自动高亮。

| 厂商 | 余额 | 模型定价 | 调用记录 | 币种 | 实现方式 |
|---|:---:|:---:|:---:|:---:|---|
| **DeepSeek 官方** | ✓ | — | — | CNY | 官方接口 `GET /user/balance`（Bearer `sk-`） |
| **New API / One API 中转站** | ✓ | ✓ | ✓ | USD | `<baseUrl>/api/user/self`（账户余额）→ 回退 billing；`/api/pricing` 读倍率；`/api/log/token` 读调用记录 |
| **智谱 BigModel** | ✓ | — | — | CNY | 内置 WebView 登录后注入脚本抓取页面（官方无公开余额接口） |
| **Kimi / Moonshot** | ✓ | ✓ | — | CNY | 开放平台余额接口 + `/v1/models` |
| **OpenRouter** | ✓ | ✓ | — | USD | Credits 余额 + 公开模型列表（含每百万 token 价格） |
| **阶跃星辰 StepFun** | ✓ | ✓ | — | CNY | `GET /v1/accounts`（含现金 / 赠券明细）+ `/v1/models` |
| **自定义查询** | ✓ | 尽力探测 | — | 自选 | 自填接口地址 + 三选一解析规则 |

### 🧩 自定义查询（接任意站点）

内置平台名单之外的站点，用「自定义查询」自己接线。三种解析器按站点能力选：

| 模式 | 行为 | 适用场景 |
|---|---|---|
| **OpenAI / One API 兼容** | 复用 New API 双通道（`/api/user/self` → billing） | 标准中转站 |
| **JSON 字段路径** | 取 JSON 字段，**支持数组下标**（`data.balance`、`balance_infos.0.total_balance`） | 绝大多数自建服务 |
| **正则表达式提取** | 在响应文本里跑正则，有捕获组取第 1 组 | 返回网页 / 非 JSON 的站点 |

- **站点地址自动推导**：只填余额接口地址即可，`https://api.x.com/v1/balance` → 自动识别站点根为 `https://api.x.com`
- **赠送额度**：可另填「赠送额度字段路径」，自定义站也能显示「赠」额度并参与临期提醒
- **编辑页按能力动态出字段**：选到哪个模式，就只出现该模式需要的输入框

### 💱 按账户计价币种

不同平台计价单位不同，阈值口径也应当不同：

- 内置平台币种为常量表（DeepSeek / Kimi / 智谱 / StepFun = **CNY**，New API / OpenRouter = **USD**）
- **New API 与自定义查询**可手动覆盖币种（站点既可能收人民币也可能收美元）
- 低余额阈值留空时按币种取默认：**人民币站 ¥10 / 美元站 $1**
- 通知文案按币种补符号

### 🔔 通知分级与灵动岛
按「是否真的属于进行中事项」把通知分成两级，而不是一股脑全塞进灵动岛：

| 级别 | 场景 | 做法 |
|---|---|---|
| **Live Updates** | 余额跌破阈值 | `ProgressStyle` 进度条 + ongoing，接入 **Android 16/17 灵动岛**（锁屏 / 状态栏胶囊） |
| **普通通知** | 套餐临期、模型上新 | 标准高优先级通知，不请求提升 |

- 进度语义：跌破阈值 = 100%（刚开始危险），彻底耗尽 = 0%（进度条走完）
- 余额回升到阈值 1.2 倍以上（充过值了）自动撤销常驻告警

### ⏱️ 定时刷新
- **15 分钟**自动刷新（前台定时器 + 后台 WorkManager 共用同一周期常量）
- 「上次同步时间」持久化落盘，**跨进程重启、跨覆盖升级**都继续走定时，不会每次冷启动都打一遍接口
- 手动「刷新」按钮可绕过周期立即执行

### 🧩 桌面小组件
- 常驻显示各账户余额快照，由后台任务周期性喷新数据
- 点击直达 App

### 🎨 设计与安全
- 设计系统单一来源（`ui/theme/Theme.kt` + `ui/components/Glass.kt`）
- 各厂商使用**官方品牌图标**（Simple Icons，CC0）区分身份；**靠图标形状 + 名称区分，不靠色相**
- 密钥 AES-256-GCM 加密，密文格式 `enc1:<iv>:<ct>`，旧版明文自动迁移
- 卡片原生支持**滑动删除**、**长按拖拽排序**

---

## 🛠️ 技术栈

| 类别 | 技术 |
| ---- | ---- |
| 语言 | Kotlin 2.0.21 |
| UI | Jetpack Compose、Material 3、Compose BOM 2024.10.01 |
| 网络 | OkHttp 4.12.0 |
| 后台 | WorkManager 2.9.1 |
| 存储 | SharedPreferences + AES-256-GCM（Android Keystore） |
| 构建 | AGP 8.9.1 / Gradle 8.14.4 / compileSdk 36 |
| 最低版本 | Android 8.0（API 26） |
| 目标版本 | Android 15（API 35） |

> 灵动岛（Live Updates）能力需要 **Android 16（API 36）+** 才生效；更低版本会自动降级为普通常驻通知。

---

## 📦 安装

从 [Releases](../../releases) 页面下载最新 APK，传到手机后点击安装。

> ⚠️ 需要开启「允许安装未知来源应用」。

**系统要求**：Android 8.0（API 26）及以上。

---

## 🧭 使用指南

1. **添加账户**：首页「+」，选择平台类型，填显示名称与凭据。
   - DeepSeek / Kimi / StepFun / OpenRouter：填 API Key 即可
   - New API 中转站：填 `Base URL` + `API Key`；建议再填「系统访问令牌」（站点 → 个人设置 → 生成系统访问令牌），这样即使站长关闭了 OpenAI 兼容 billing 路由也能读余额与日志
   - 智谱：点卡片进入 WebView，用账号登录一次，进「财务总览」后点「抓取」
   - 自定义查询：填名称 + API Key + **余额接口地址**，再选解析规则（见下节）
2. **查看详情**：点卡片进详情页，看余额大卡、套餐额度、模型定价与最近调用
3. **管理账户**：滑动卡片删除、长按拖拽排序
4. **设置阈值与币种**：编辑账户时填低余额告警阈值（留空按币种默认），New API / 自定义查询还可切换计价币种

### 接入一个自定义站点

以某站点 `GET https://api.example.com/v1/balance` 返回 `{"data":{"balance":59.0}}` 为例：

1. 平台类型选「自定义查询」
2. 填 API Key
3. 余额接口地址填完整 URL：`https://api.example.com/v1/balance`
4. 解析规则选「JSON 字段路径」，余额字段路径填 `data.balance`
   - 若返回是数组（如 `balance_infos`），用下标：`balance_infos.0.total_balance`
5. 若该站还有赠送额度字段（如 `data.voucher`），填进「赠送额度字段路径」
6. 若返回的不是 JSON 而是网页，改选「正则表达式提取」，填如 `¥([0-9.]+)`（有捕获组取第 1 组）

---

## 🔍 接口说明

- `GET /api/pricing` — 模型定价（多数站点匿名可访问）
- `GET /api/log/token`（`TokenAuthReadOnly`）— 用 `sk-` 查本 Key 调用记录
- `GET /api/log/self`（`UserAuth`）— 用系统访问令牌查本人全部日志
- `GET /api/user/self`（`UserAuth`）— `quota` / `used_quota`（**500000 quota = $1**，One API 系惯例）
  - ⚠️ `quota` 本身就是**剩余额度**，不要再减 `used_quota`
  - ⚠️ 新版 New API 的 `/v1/dashboard/billing/subscription` 在部分版本已下线，App 已做双通道自动回退
  - ⚠️ 令牌"无限额度"时 billing 返回哨兵值 `hard_limit_usd = 100000000`，App 识别为不限额而非余额
- `GET https://api.stepfun.com/v1/accounts` — StepFun 余额（含 `total_cash_balance` / `total_voucher_balance`）

---

## 🆕 版本更新

各版本详细变更见 [CHANGELOG](./CHANGELOG.md)。

#### v1.2.0 — 新增阶跃星辰 StepFun，支持自定义查询
- 新增 **阶跃星辰 StepFun** 平台（含现金 / 赠券明细）
- 新增 **自定义查询**：自填接口地址，三选一解析规则（兼容模式 / JSON 字段路径 / 正则）
- 新增 **按账户计价币种**，阈值默认值随币种走（¥10 / $1）
- 修复 `baseUrl` 被截断成 `https:` 的畸形 URL 问题
- 修复 StepFun / 自定义站的赠送额度显示不出来

#### v1.1.0 — 余额告警接入灵动岛
- 通知分级：余额告警走 Android 16/17 **Live Updates 灵动岛**（带进度条）
- 进入 App 不再立即刷新，改为**纯 15 分钟定时**（跨重启、跨升级保留）
- 修复异常 / 偏低余额**反复通知**的 bug
- 各厂商卡片换用**官方品牌图标**
- 密钥存储升级为 AES-256-GCM + Android Keystore

#### v1.0.0 — 初次发布
- 多厂商额度查询与余额告警
- 桌面小组件与后台自动刷新
- Monet 2026 + iOS26 Liquid Glass 视觉体系

---

## 📄 许可证

本项目仅供学习与技术交流使用，请遵守各 AI 厂商的服务条款。

---

<div align="center">
<sub>Made with ❤️ · API 额度监控</sub>
</div>
