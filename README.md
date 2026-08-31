<div align="center">

# 📊 Monitor · API 额度监控

快速便捷地查看各厂商模型余额，并聚合市面上一些免费厂商模型的最新消息。

**Kotlin · Jetpack Compose · Material 3**

</div>

---

## 📖 项目简介

**Monitor（API 额度监控）** 是一款面向 Android 的轻量工具应用，帮助开发者与用户**一站式管理多个 AI 厂商的 API 额度**，随时掌握各模型的余额与用量情况，并实时聚合市场上可用的**免费模型资源**，避免在多个控制台之间来回切换。

应用深度适配 **OnePlus 15**（6.78 英寸 / 2772×1272 / 450PPI），全面采用 **Monet 2026 配色**与 **iOS26 Liquid Glass 液态玻璃**设计语言，兼顾功能与美学。

---

## ✨ 功能特性

### 📊 额度查询
- 多厂商实例统一管理，首页以信息卡片展示**作者 / 内容预览 / 状态 / 余额**
- 支持点击进详情、滑动删除（可撤销）、长按拖拽排序（边缘自动滚动）
- 额度详情页提供**大幅余额数字卡片 + 配额/套餐明细**

### 🤖 免费模型聚合
- 国内 / 国际分区，免费模型卡片标注「注册即可用」与「直连可用」
- 聚合来源：精选目录、OpenRouter、SiliconFlow、商汤 SenseNova、Google Gemini、Cohere、Cerebras、Together AI、OVHcloud 等

### 🧩 桌面小组件
- 支持多尺寸（含 4×2 紧凑布局），统一深蓝渐变主题
- 多厂商数据一键同步刷新

### ⚙️ 系统能力
- **12 小时自动刷新** + 余额阈值告警通知
- 前台刷新器 + 后台 Worker 并行刷新，单实例异常不拖慢整体
- 安全密钥存储，保护 API Key
- **TLS 指纹 (WAF) 穿透**：自动处理部分 API 服务器对 OkHttp 的拦截，保障直连可用

---

## 🛠️ 技术栈

| 类别 | 技术 |
| ---- | ---- |
| 语言 | Kotlin |
| UI | Jetpack Compose、Material 3、Compose BOM 2024.12.01 |
| 网络 | OkHttp 4.12.0、Kotlinx Serialization |
| 后台 | WorkManager、前台服务刷新器 |
| 组件 | 桌面小组件 (AppWidgetProvider) |
| 最低版本 | Android 6.0 (API 23) |
| 目标版本 | Android 15 (API 35) |

---

## 📦 安装

从 [Releases](../../releases) 页面下载最新 APK，传到手机后点击安装即可。

> ⚠️ 需要开启「允许安装未知来源应用」。

**系统要求**：Android 6.0 及以上。

---

## 🧭 使用指南

1. **添加实例**：点击首页右上角「+」，选择厂商类型，填写显示名称、API Key、Base URL 等信息。
2. **查看额度**：点击任意实例卡片进入详情页，查看余额大卡与套餐明细。
3. **管理实例**：滑动卡片可删除，长按卡片可拖拽排序。
4. **免费模型**：切换到「免费 AI 模型」标签页，按国内 / 国际分区浏览可用免费模型。

---

## 🆕 版本更新

各版本变更详情见 [CHANGELOG](./CHANGELOG.md)。

#### v1.0.0 — 初次发布
- 完整的多厂商额度查询与余额告警
- 国内 / 国际免费模型聚合
- 桌面小组件与自动刷新
- Monet 2026 + iOS26 Liquid Glass 全面视觉升级

---

## 🧑‍💻 开发

```bash
# 克隆仓库
git clone https://github.com/qiuqiu-fist/Monitor.git

# 使用 Android Studio 打开 android-app 目录
# 等待 Gradle 同步完成后即可构建运行
```

---

## 📄 许可证

本项目仅供学习与技术交流使用，请遵守各 AI 厂商的服务条款。

---

<div align="center">
<sub>Made with ❤️ · API 额度监控</sub>
</div>