---
sidebar_label: '创建并发送邮件营销活动'
sidebar_position: 1
---

# 创建并发送邮件营销活动

## 概述

邮件营销活动允许您向一个或多个收件人同时发送结构化消息。在 uSpeedo 中，您可以创建活动、设计邮件内容、选择收件人，并立即发送或定时稍后发送。

本指南将引导您完成整个流程 —— 从账号准备到发送您的首个营销活动并追踪效果。

## 开始之前

创建活动前，请确保满足以下条件：

-   您已在 **[控制台](https://console.uspeedo.com/email)** 注册账号。
-   您的账号拥有可用 **余额或邮件发送额度**。
-   已遵循 <a href="https://uspeedo.com/docs/products/email/campaigns/deliverability/getting-started/comply-gmail-yahoo-microsoft">**Gmail 和 Yahoo 发件人要求**</a>。
-   您的收件人已准备就绪：
    -   可在 **[联系人](https://console.uspeedo.com/audience/contact)** 中导入，或
    -   在 [创建活动](#step-1-create-a-campaign) 时直接添加收件人
-   若计划大批量发送邮件，**强烈建议**认证您自己的发信域名（详见 <a href="https://uspeedo.com/docs/products/email/campaigns/deliverability/domains/domain-authentication">域名认证</a> 指南）。

## 步骤 1：创建活动 {#step-1-create-a-campaign}

1.  进入 **发送邮件 → 创建活动**。
    - 或者，您也可以进入 **模板**，从模板开始创建活动。

2.  输入 **活动名称**。
    - 该名称**仅对您可见**，方便后续查找活动。
    - **不会**出现在收件人收到的邮件中。

3.  点击 **创建** 继续。

<div className="doc-video">
  <video className="doc-video-player" controls alt="Create a Email Campaign" preload="metadata">
    <source src="https://cdn.uspeedo.com/docs_external/1.mp4" type="video/mp4" />
    您的浏览器不支持视频播放
  </video>
</div>

## 步骤 2：设置基础信息

在活动页面内，填写以下字段：

-   **发件人** – 发送邮件的**邮箱地址**，以及发送时显示的**发件人名称**
-   **收件人** – 选择联系人、下载 Excel 模板批量上传，或直接添加邮箱地址
-   **主题** – 收件人可见的邮件主题，以及在收件箱中显示在主题后的前置摘要文本
-   **发送时间** – 立即发送或定时稍后发送

填写完成或部分填写后，点击右侧的 **设计邮件**。

![Design Email in Email Editor - uSpeedo](https://cdn.uspeedo.com/docs_external/1-2.png)

## 步骤 3：设计您的邮件

您可以选择适合自己需求的编辑方式：

**选项 1：从零开始创建**

-   从空白画布开始。适合需要完全掌控设计的场景。

**选项 2：使用现成模板（新手推荐）**

-   浏览 uSpeedo 内置模板
-   选择喜欢的模板并点击 **使用模板**
-   模板将在拖拽编辑器中打开

**选项 3：使用 HTML 编辑器**

-   若您已有 HTML 邮件模板可使用此方式
-   或您熟悉代码编辑

设计完成后：

1.  点击 **保存并前往设置**
2.  核对您的活动设置信息
3.  确认无误后，点击 **发送**

您的活动将按时开始发送。

## 如何查看收件箱

当活动状态变为 **已完成** 时，您可以查看收件箱确认邮件是否送达。

---
> 💡 如果您在主收件箱中未看到邮件：
> 
> -   检查 **垃圾邮件** 文件夹
> -   检查 **推广** 标签页（Gmail 用户）
> -   若出现在垃圾邮件中，请将其移至收件箱——这有助于提升后续送达率
> 
> 邮件落点可能受以下因素影响：
> 
> -   您的发送 **IP 配置**
> -   您的 **域名认证**
> -   您的 **发件人信誉**
> 
> 更多指导请查看：<u>**如何提升邮件送达率**</u>

---

## 追踪活动效果

发送完成后，您可以在 <a href="https://console.uspeedo.com/email/analytics" target="_blank">**数据统计**</a> 中监控效果。

您可以查看：

-   送达状态
-   发送进度
-   其他发送相关指标

这有助于您了解邮件是否成功送达以及活动表现如何。

## 小贴士与最佳实践

-   大批量发送活动前先完成域名认证
-   务必先给自己发送测试邮件
-   使用清晰且相关的邮件主题
-   避免新域名过快大批量发送
-   若邮件频繁进入垃圾邮件，请先优化域名与发件人信誉再重新发送