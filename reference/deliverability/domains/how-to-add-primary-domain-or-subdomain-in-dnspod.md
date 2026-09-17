---
sidebar_label: '如何在 DNSPod 中添加主域名或子域名？'
sidebar_position: 5
---

# 如何在 DNSPod 中添加主域名或子域名？

## 概述
本文将指导您在 DNSPod 中添加根域名或子域名，并配置 uSpeedo 所需的 DNS 记录，包括：
- SPF
- DKIM
- MX
- DMARC

**前提条件**：您已拥有 DNSPod 账号。

若在配置过程中遇到问题，请联系 DNSPod 官方支持获取协助。

---

## 一、选择根域名或子域名
开始前，请确定使用**根域名**还是**子域名**。

### 示例
- **根域名**：
  - example.com
  - google.com
- **子域名**：
  - mail.example.com
  - sc.example.com

### 推荐方案
👉 大多数情况下，**建议使用子域名作为发信域名**。

确认后：
1. 在 uSpeedo 控制台添加域名
2. 系统将生成对应的 DNS 记录

---

## 二、在 DNSPod 中添加域名
### 方式一：注册新域名
1. 登录 DNSPod
2. 进入**域名管理**
3. 点击**添加域名**
4. 输入域名并完成注册

### 方式二：使用已有域名（推荐）
若您的域名在其他注册商处注册：
1. 登录 DNSPod
2. 添加该域名
3. 获取 DNSPod 提供的 NS 记录
4. 在原注册商处更新 NS 服务器

---

## 三、配置 DNS 记录
路径：
1. 登录 DNSPod
2. 域名管理 → 选择您的域名
3. 点击**管理**
4. 点击**添加记录**

---

## 四、SPF 配置
### 作用
防止发件人伪造，提升送达率。

### 根域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机记录 | @ |
| 值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

📌 注意：
若已存在 SPF 记录：
👉 在 `v=spf1` 与 `~all` 之间插入：
`include:sendcloud.org`

### 子域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机记录 | 子域名前缀（如 sc） |
| 值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

---

## 五、DKIM 配置
### 作用
验证邮件签名，防止伪造。

### 根域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机记录 | sendcloud._domainkey |
| 值 | k=rsa; p=your_public_key |
| TTL | 600 |

📌 注意：
选择器可能不同，例如：
- default._domainkey
- sc._domainkey
👉 **必须使用控制台提供的值**。

### 子域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机记录 | sendcloud._domainkey.sc |
| 值 | k=rsa; p=your_public_key |
| TTL | 600 |

---

## 六、MX 记录配置
### 作用
指定负责接收邮件的邮件服务器。

### 根域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| 主机记录 | @ |
| 值 | mx.sendcloud.org |
| 优先级 | 10 |
| TTL | 600 |

📌 注意：
👉 **请勿同时保留其他 MX 记录**（会产生冲突）。

### 子域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| 主机记录 | 子域名前缀 |
| 值 | mx.sendcloud.org |
| 优先级 | 10 |
| TTL | 600 |

---

## 七、DMARC 配置
### 作用
防范欺诈邮件，实施策略控制。

### 根域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机记录 | _dmarc |
| 值 | v=DMARC1; p=none; rua=mailto:xxx; ruf=mailto:xxx; fo=1 |
| TTL | 600 |

### 参数说明
- `v=DMARC1`：协议版本
- `p=none`：监控模式
- `p=quarantine`：将邮件隔离
- `p=reject`：直接拒绝邮件
- `rua`：聚合报告
- `ruf`：取证报告
- `fo=1`：认证失败时触发

### 子域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机记录 | _dmarc.sc |
| 值 | v=DMARC1; p=none; rua=mailto:xxx |
| TTL | 600 |

---

## 八、NS（域名服务器）说明
若您正在迁移 DNS 服务：
👉 **必须在域名注册商处更新 NS 记录**。

否则：
❌ uSpeedo 将无法完成域名认证。

---

## 九、注意事项
- DNS 记录必须与控制台提供的值一致（示例仅供参考）
- 生效时间：数分钟至数小时
- 推荐 TTL：300–600
- 记录生效后可提高 TTL 值

---

## 十、常见问题
### 1. 认证失败？
检查：
- 是否已切换 NS 记录
- 记录是否完全匹配
- 是否存在冲突记录

### 2. 为什么推荐使用子域名？
👉 隔离发送信誉
避免影响根域名（官网/品牌）