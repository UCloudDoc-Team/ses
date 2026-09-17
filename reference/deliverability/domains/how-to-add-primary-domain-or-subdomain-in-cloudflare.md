---
sidebar_label: '如何在 Cloudflare 添加主域名或子域名？'
sidebar_position: 7
---

# 如何在 Cloudflare 添加主域名或子域名？

## I. 前提条件

开始前请确认：

- 你拥有一个域名（例如 example.com）
- 已注册并登录 Cloudflare 账号
- 域名已添加到 Cloudflare，并且 NS 记录已指向 Cloudflare

---

## II. 进入 DNS 管理界面

1. 登录 Cloudflare
2. 选择你的域名站点
3. 点击顶部菜单 **DNS**
4. 进入 **Records（记录）** 页面

---

## III. 添加 DNS 记录

根据 uSpeedo 控制台提供的参数，逐条添加以下记录：

---

#### 1. 添加 SPF 记录（TXT）

用于声明哪些服务器允许代表你的域名发信。

- Type：TXT
- Name：@（或你的域名）
- Content：v=spf1 include:xxx ~all
- TTL：Auto

---

#### 2. 添加 DKIM 记录（CNAME）

用于邮件签名验证并提升送达率。

- Type：CNAME
- Name：xxx._domainkey
- Target：xxx.uspeedo.net
- TTL：Auto

⚠️ 注意：

- 必须设置为 **DNS Only（灰云）**
- **不要**开启 Cloudflare 代理（橙云）

---

#### 3. 添加 DMARC 记录（TXT）

用于执行认证策略。

- Type：TXT
- Name：_dmarc
- Content：v=DMARC1; p=none; rua=mailto:xxx@yourdomain.com
- TTL：Auto

---

#### 4. 添加 MX 记录（如需接收邮件）

如果你也需要接收邮件：

- Type：MX
- Name：@
- Value：你的邮件服务器地址
- Priority：10（或按服务商要求）

---

## IV. 重要配置说明

❗ 1. 关闭代理（关键）

以下记录：

- MX
- TXT（SPF / DMARC）
- CNAME（DKIM）

👉 都必须设置为：**DNS Only（灰云）**

否则可能出现：

- DNS 解析异常
- 邮件认证失败
- 邮件进入垃圾箱

---

❗ 2. DNS 生效时间

- 通常：5 分钟 ~ 2 小时
- 最长：24 ~ 48 小时

---

❗ 3. 自动扫描记录不完整

Cloudflare 可能会自动扫描已有记录，但：

- 可能漏掉 MX 等邮件相关记录
- 需要手动补齐

---

## V. 验证域名配置

完成 DNS 配置后：

1. 回到 uSpeedo 控制台
2. 点击 **Verify Domain**
3. 等待验证通过

---

## VI. 常见问题（FAQ）

❓ 为什么验证失败？

常见原因：

- DKIM 没有设置为 DNS Only
- SPF 值不正确
- DNS 尚未完全生效
- 子域名填写错误

---

❓ 可以使用子域名吗？

可以，且推荐：`mail.yourdomain.com`

优势：

- 不影响主域名信誉
- 更容易隔离发信流量

---

❓ Cloudflare 支持 CNAME 吗？

支持，但：

- 根域名（example.com）**不支持** CNAME
- 子域名可以使用 CNAME

---

## VII. 最佳实践（强烈推荐）

#### 1. 使用子域名发信

```
推荐：
- mail.xxx.com
- send.xxx.com

不推荐：
- xxx.com（直接用根域名发信）
```

#### 2. 按用途分离

```
事务邮件：
- notify.xxx.com

营销邮件：
- promo.xxx.com
```

#### 3. 三项认证都要齐全

必须配置：

- SPF
- DKIM
- DMARC

否则：邮件很容易进入垃圾箱
