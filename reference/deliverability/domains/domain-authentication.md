---
sidebar_label: '域名认证'
sidebar_position: 2
---

# 域名认证

## 一、准备工作
- 域名：用于发送邮件的域名
- 域名注册商账号：用于进行 DNS 解析
- uSpeedo 账号：用于获取 DNS 记录值

## 二、配置步骤
1. 进入 uSpeedo 控制台后，点击**前往邮件控制台**进入邮件专属控制台。

![img](https://cdn.uspeedo.com/docs_external/Domain%20Authentication-1.png)

2. 进入**设置** → **域名与发件人** → **添加新域名**。

在此页面添加您的发信域名。
![img](https://cdn.uspeedo.com/docs_external/Domain%20Authentication-2.png)

3. 在弹窗中选择您的域名注册商。若列表中没有，可选择**其他**——此选择仅为引导参考，选错不会影响配置。
![img](https://cdn.uspeedo.com/docs_external/Domain%20Authentication-7.png)

4. 点击**下一步**并填写配置项：
   1. **发信域名**：建议使用子域名，避免主域名信誉受损。例如，若您的主域名是 `example.com`，建议使用 `mail.example.com`。
   2. **发送类型**：促销类邮件选择**营销**，通知、订单、注册、告警等事务类邮件选择**事务邮件**。
   3. **追踪域名**：大多数情况下选择**自动生成**，我们将为您自动签发 SSL 证书。您也可以手动申请并添加。
![img](https://cdn.uspeedo.com/docs_external/Domain%20Authentication-4.png)

5. 此步骤提示您打开域名注册商的 DNS 配置页面。若已打开，直接点击**下一步**即可。
![img](https://cdn.uspeedo.com/docs_external/Domain%20Authentication-5.png)

6. 此时您将看到配置详情：
   1. **基础配置**：发送邮件必需配置 SPF、DKIM、DMARC 和 MX 记录。
   2. **追踪域名**：解析完成后，即可追踪用户邮件打开、阅读等数据。
![img](https://cdn.uspeedo.com/docs_external/Domain%20Authentication-6.png)

## 三、查看结果
通常情况下，您可在 **3–10 分钟** 内看到 DNS 解析结果。特殊情况下可能需要 **1 小时至最长 24 小时**。
![img](https://cdn.uspeedo.com/docs_external/3-7.png)