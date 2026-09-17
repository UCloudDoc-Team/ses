---
sidebar_label: '如何在 DigitalOcean 添加主域名或子域名？'
sidebar_position: 9
---

# 如何在 DigitalOcean 添加主域名或子域名？

## 概览

本指南将带你完成：

- 在 DigitalOcean 添加主域名或子域名
- 配置 uSpeedo 所需的 DNS 记录

**前提条件**：你已拥有 DigitalOcean 账号。

如遇到配置问题，建议联系 DigitalOcean 官方支持团队协助处理。

---

## 选择主域名还是子域名

开始前请决定：

👉 使用主域名还是子域名。

### 概念说明

- **主域名示例**：
  - uspeedo.com
  - google.com
- **子域名示例**：
  - mail.uspeedo.com
  - mail.google.com

📌 特点：

- 子域名是在主域名前增加一个或多个前缀
- **强烈建议**优先使用子域名作为发信域名

---

## 确定后

1. 在 uSpeedo 控制台添加域名/子域名
2. 系统会自动生成所需 DNS 记录

---

## 添加域名

在 DigitalOcean 中添加域名通常有三种方式：

- **方式 1**：在 DigitalOcean 注册新域名
- **方式 2**：从其他注册商转入域名
- **方式 3**：使用已有域名，并将 DNS 托管切换到 DigitalOcean

👉 本文重点覆盖 **方式 1** 与 **方式 3**。

---

## 方式 1：注册新域名

步骤：

1. 登录 DigitalOcean

![img](https://cdn.uspeedo.com/docs_external/6-1.png)

2. 进入 **Domain Management**

![img](https://cdn.uspeedo.com/docs_external/6-2.png)

3. 点击 **Add Domain**

![img](https://cdn.uspeedo.com/docs_external/6-3.png)

4. 输入你的域名
5. 按提示完成注册

---

## 方式 3：使用已有域名

如果你的域名在其他平台：

1. 登录 DigitalOcean
2. 将域名添加到 DigitalOcean 的 DNS 管理
3. 系统会提供对应 NS（Name Server）记录
4. 回到域名注册商处，将 NS 更新为 DigitalOcean 提供的值

---

## 配置域名记录

操作路径：

1. 登录 DigitalOcean
2. 进入 **Domain Management**
3. 找到域名并点击 **Manage**
4. 点击 **Add Record** 添加 DNS 记录

---

## 配置 SPF

SPF 用于防止冒用发信并降低进垃圾箱风险。

### 主域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | @ |
| 记录值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

📌 注意：  
如果已存在 SPF：  
👉 将 `include:sendcloud.org` 插入到 `v=spf1` 与 `~all` 之间。

### 子域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | 子域名前缀（例如 sc） |
| 记录值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

---

## 配置 DKIM

DKIM 用于验证邮件来源并防止伪造邮件。

### 主域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | sendcloud._domainkey（或系统分配值） |
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
| Host | sendcloud._domainkey.subdomain_prefix（例如 sendcloud._domainkey.sc） |
| 记录值 | k=rsa; p=public key |
| TTL | 600 |

---

## 配置 MX

MX 用于指定负责接收邮件的邮件服务器。

### 主域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| Host | @ |
| Value | mx.sendcloud.org |
| Priority | 10 |
| TTL | 600 |

📌 注意：  
❗ 仅保留 uSpeedo 的 MX 记录。  
❗ 避免混用多个邮件服务商，否则可能出现投递问题。

### 子域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| Host | 子域名前缀 |
| Value | mx.sendcloud.org |
| Priority | 10 |
| TTL | 600 |

---

## 配置 DMARC

DMARC 是建立在 SPF 与 DKIM 之上的邮件认证策略。

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
- `fo=1`：失败上报策略

### 子域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| Host | _dmarc.subdomain_prefix（例如 _dmarc.sc） |
| 记录值 | v=DMARC1; p=none; rua=mailto:dmarc-reports@yourdomain.com |
| TTL | 600 |

