---
sidebar_label: '迁移到 uSpeedo'
sidebar_position: 1
---

# 如何将您的邮件服务迁移到 uSpeedo

## 学习目标

了解如何将你现有的发信域名（也称为专用发信域名）从之前的邮件服务提供商迁移到 uSpeedo。

## 开始之前

新的 uSpeedo 账户，以及注册时间少于 30 天的域名，在配置发送域时需要规划对发送基础设施进行预热。

预热是一个建立发送信誉的过程，使你被识别为合法或“优质”的邮件发送者。如果没有正确进行预热，可能会损害你的发送信誉。

要确认你的账户是否需要预热，请参考[域名预热指南](https://uspeedo.com/docs/zh-Hans/products/email/campaigns/deliverability/getting-started/warm-up-domains)。

此外，请确保：

- 你拥有用于发送邮件的域名
- 并且可以访问和修改该域名的 DNS（域名系统）记录

## 在 uSpeedo 中配置你的发信信息

### 发信域名

当你准备在 uSpeedo 中启用发送域时，需要配置一个发信域名。

你需要更新 DNS 设置，添加通过 uSpeedo 账户生成的 CNAME 和 TXT 记录。这样可以让你使用自己的发信域名发送邮件，而不是使用 uSpeedo 的共享域名。

请确保你的品牌发送子域当前未被 DNS 使用。如果该子域已经存在于 DNS 中，可能会与现有记录发生冲突，并影响域名的其他配置。

连接发信域名还会启用 DKIM 和 SPF 认证。这些认证是邮件最佳实践，有助于防止和缓解投递问题，并提升送达率。

### 跟踪域名

如果你之前在邮件服务提供商中使用过自定义点击跟踪域，或者希望在 uSpeedo 中使用，可以在 DNS 中添加额外的 CNAME 记录。

专用点击跟踪可以让邮件中的跟踪链接显示为你的品牌域名，而不是 uSpeedo 的默认编码，从而增强用户对邮件的信任，因为链接更容易识别。

## 可达性因素

在将发信域名从其他邮件服务提供商迁移到 uSpeedo 时，需要重点关注邮件送达率，以确保邮件能够成功进入收件箱。

### 发件人信誉

当你将发送域迁移到 uSpeedo 时，该域名的发件人信誉也会一并迁移。

域名的发送信誉是邮箱服务提供商（MBP）判断邮件分类的重要因素。

如果你当前的发信域名存在送达问题，应遵循邮件送达最佳实践来改善信誉并调整发送策略。

更换 ESP 并不会自动解决送达问题。

### DMARC

如果你的发件邮箱域（即 From 地址的域名）设置了 DMARC 策略，当发信域名与发件域不一致时，可能会影响邮件进入收件箱。

DMARC 是一种用于保护域名、防止未经授权发送邮件（即邮件伪造）的协议。

使用发信域名时，请确保发送域与发件地址域一致。

例如：

如果你使用 `sales@example.com` 作为发件地址，且 `example.com` 启用了 DMARC，则你需要使用类似 `send.example.com` 的发信域名，通过 uSpeedo 发送邮件，以满足 DMARC 认证要求。

如果不一致，可能会影响发送效果。

这种不一致通常发生在：

使用 uSpeedo 默认共享发送域，同时发件地址域设置了 DMARC。

如果你使用共享域发送邮件，应移除发件域上的 DMARC，以避免问题。

## 删除之前邮件服务提供商生成的 DNS 记录

当你不再使用之前的邮件服务提供商发送邮件时，应删除其相关 DNS 记录。

这一步需要在 uSpeedo 之外完成，可能需要与你的 IT 团队协作。

请注意：

并非所有域名服务商都允许直接编辑所有 DNS 记录。如果无法修改，请联系 DNS 提供商获取帮助。

删除这些 DNS 记录后，该域名将不再通过原服务商发送邮件。

在删除之前，请确认你不再需要原有的发送基础设施。

### 操作步骤

1. 登录你的 DNS 提供商（常见包括）：
   - [GoDaddy](https://www.godaddy.com/en/help/manage-dns-records-680)
   - [Google Domains](https://knowledge.workspace.google.com/admin/domains/dns-basics?hl=en&visit_id=639118513286081731-1908671832&rd=1)
   - [HostGator](https://www.hostgator.com/help/article/manage-dns-records-with-hostgatorenom)
   - [Hover](https://support.hover.com/support/solutions/articles/201000064728)
   - [Namecheap](https://www.namecheap.com/support/knowledgebase/article.aspx/9214/31/cpanel-email-deliverability-tool-spf-dkim-and-dmarc-records/)
   - [Squarespace](https://support.squarespace.com/hc/en-us/articles/205812348-Accessing-your-Squarespace-managed-domain-s-DNS-settings)
   - [AWS](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html)
   - [Cloudflare](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-editing.html)
2. 删除 DNS 中由之前邮件服务提供商生成的所有 CNAME 和 TXT 记录。
3. 某些服务商还可能添加了 MX 记录（用于处理回复邮件），也需要一并检查。

如果存在其他类型的记录，请联系支持团队： support@mail-uspeedo.com。

## 收信路由

- 您可以联系客户经理，提供收信邮箱添加收信路由配置或者通过 SMTP 传递参数 Reply-To。

## 回执推送

- 我们提供了基于 Webhook 的回执推送，您可以在控制台配置 [Webhook 推送地址](https://console.uspeedo.com/email/setting?type=webhook)，我们支持 Token 的验证保障您接口的安全。
- 您可以选择需要推送的邮件状态类型按需开启。

## 发信统计

- 您的邮件发送后，可以在[控制台分析界面](https://console.uspeedo.com/email/analytics?type=record)查看，对于失败的邮件您可以查看具体失败原因，以此来提升您的发信效果。
