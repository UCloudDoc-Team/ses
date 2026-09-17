---
sidebar_label: '如何在 Namecheap 添加主域名或子域名？'
sidebar_position: 8
---

# 如何在 Namecheap 添加主域名或子域名？

## 概览

本指南演示如何在 Namecheap 添加主域名或子域名，并配置 uSpeedo 所需的 DNS 记录。

**前提条件**：你已拥有 Namecheap 账号。

尽管本文尽量详细，但 DNS 配置仍可能遇到问题。若出现问题，建议直接联系 Namecheap 官方支持（通常定位更快）。

---

## 选择主域名还是子域名

开始前请先决定使用：

- **主域名（Primary Domain）**
- 或 **子域名（Subdomain）**

### 概念说明

- **主域名示例**：
  - uspeedo.com
  - google.com
- **子域名示例**：
  - mail.uspeedo.com
  - mail.google.com

👉 规则：子域名是在主域名前增加一个或多个前缀。

📌 **建议**：优先使用子域名。

---

### 确定后

1. 在 uSpeedo 控制台添加域名/子域名
2. 系统会自动生成所需 DNS 记录

---

## 添加域名

Namecheap 支持三种方式：

### 方式 1：注册新域名

直接在 Namecheap 购买新域名。

步骤：

1. 登录 Namecheap

![img](https://cdn.uspeedo.com/docs_external/5-1.png)

2. 输入你要购买的域名

![img](https://cdn.uspeedo.com/docs_external/5-2.png)

3. 完成支付与注册

![img](https://cdn.uspeedo.com/docs_external/5-3.png)

### 方式 2：转移域名

将域名从其他注册商转移到 Namecheap。

### 方式 3：使用已有域名（推荐）

保留原注册商，但将 DNS 托管切换到 Namecheap。

如果你的域名在其他平台：

1. 登录 Namecheap

![img](https://cdn.uspeedo.com/docs_external/5-4.png)

2. 进入 **Domains** → **FreeDNS**

![img](https://cdn.uspeedo.com/docs_external/5-5.png)

3. 输入域名并点击 **Get DNS**

![img](https://cdn.uspeedo.com/docs_external/5-6.png)

4. 加入购物车并点击 **Set up DNS**

![img](https://cdn.uspeedo.com/docs_external/5-7.png)

5. 获取 Namecheap 提供的 NS 记录
6. 回到原注册商处将 NS 记录修改为 Namecheap 给出的值

👉 本文主要覆盖 **方式 1** 与 **方式 3**。

---

## 配置域名记录

操作路径：

1. 登录 Namecheap

![img](https://cdn.uspeedo.com/docs_external/5-8.png)

2. 进入 **Domain Management**

![img](https://cdn.uspeedo.com/docs_external/5-9.png)

3. 找到域名并点击 **Manage**

![img](https://cdn.uspeedo.com/docs_external/5-10.png)

4. 点击 **Add Record** 添加 DNS 记录

![img](https://cdn.uspeedo.com/docs_external/5-11.png)

---

## SPF 配置

**作用**：防止冒用发信，降低进入垃圾箱风险。

### 主域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | @ |
| 记录值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

📌 注意：  
若已存在 SPF：  
👉 将 `include:sendcloud.org` 插入到 `v=spf1` 与 `~all` 之间。

### 子域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | 子域名前缀（例如 sc） |
| 记录值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

---

## DKIM 配置

**作用**：验证邮件来源，防止伪造。

### 主域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | sendcloud._domainkey |
| 记录值 | k=rsa; p=public key（控制台提供） |
| TTL | 600 |

📌 注意：  
Host 可能是：

- default._domainkey
- sc._domainkey

👉 以 uSpeedo 控制台提供值为准。

### 子域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | sendcloud._domainkey.sc |
| 记录值 | k=rsa; p=public key |
| TTL | 600 |

---

## MX 配置

**作用**：指定接收该域名邮件的邮件服务器。

### 主域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| Host | @ |
| Value | mx.sendcloud.org |
| Priority | 10 |
| TTL | 600 |

📌 注意：  
❗ 不要同时配置多个邮件服务商的 MX，否则投递会不稳定。

### 子域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| Host | 子域名前缀 |
| Value | mx.sendcloud.org |
| Priority | 10 |
| TTL | 600 |

---

## DMARC 配置

**作用**：认证策略 + 报告机制（基于 SPF/DKIM）。

### 主域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | _dmarc |
| 记录值 | v=DMARC1; p=none; rua=mailto:dmarc-reports@yourdomain.com; ruf=mailto:dmarc-forensics@yourdomain.com; fo=1 |
| TTL | 600 |

### 参数说明

- `v=DMARC1`：协议版本
- `p=none`：监控模式
- `p=quarantine`：隔离（进垃圾箱）
- `p=reject`：拒收
- `rua`：汇总报告地址
- `ruf`：取证报告地址
- `fo=1`：失败上报

### 子域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | _dmarc.sc |
| 记录值 | v=DMARC1; p=none; rua=mailto:dmarc-reports@yourdomain.com |
| TTL | 600 |

---

## 更新 NS 记录（关键）

如果你从其他 DNS 服务迁移：

👉 **必须更新 NS（Name Server）记录。**

否则：

❌ uSpeedo 将无法验证 DNS 配置。

