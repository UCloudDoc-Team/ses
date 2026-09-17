---
sidebar_label: '如何在 GoDaddy 添加主域名或子域名？'
sidebar_position: 6
---

# 如何在 GoDaddy 添加主域名或子域名？

## 概览

本指南将带你在 GoDaddy 添加主域名或子域名，并配置 uSpeedo 所需的 DNS 记录，包括：

- SPF
- DKIM
- MX
- DMARC

**前提条件**：你已拥有 GoDaddy 账号。

如遇到配置问题，建议联系 GoDaddy 官方支持。

---

## I. 选择主域名还是子域名

开始之前需要决定：

👉 使用主域名还是子域名？

#### 示例

- **主域名**：
  - example.com
  - google.com
- **子域名**：
  - mail.example.com
  - sc.example.com

#### 建议

👉 建议使用 **子域名** 作为发信域名（更利于信誉隔离）。

确定后：

1. 在 uSpeedo 控制台添加域名
2. 系统将自动生成对应 DNS 记录

---

## II. 在 GoDaddy 添加域名

你可以通过以下方式添加域名：

#### 方式 1：注册新域名

1. 登录 GoDaddy

![img](https://cdn.uspeedo.com/docs_external/4-1.PNG)

2. 输入你要购买的域名

![img](https://cdn.uspeedo.com/docs_external/4-2.PNG)

3. 完成购买流程

---

#### 方式 2：使用已有域名（推荐）

如果你的域名注册在其他平台：

1. 登录 GoDaddy

![img](https://cdn.uspeedo.com/docs_external/4-3.png)

2. 添加 DNS Hosting

![img](https://cdn.uspeedo.com/docs_external/4-4.png)

3. 输入域名并点击 Add

![img](https://cdn.uspeedo.com/docs_external/4-5.png)

4. 可以先跳过修改 NS
5. 之后必须回到原注册商处修改 NS，否则解析不会生效

⚠️ 注意：  
👉 若不更新 NS，uSpeedo 的 DNS 验证将无法生效。

---

## III. 进入 DNS 配置界面

操作路径：

1. 登录 GoDaddy
2. 进入 **Domain Management**
3. 选择你的域名
4. 点击 **Manage DNS**
5. 点击 **Add Record**

---

## IV. SPF 配置

#### 作用

防止发件人伪造并提升投递能力。

#### 主域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机 | @ |
| 记录值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

📌 注意：  
若已存在 SPF 记录：  
将以下内容插入 `v=spf1` 与 `~all` 之间：  
`include:sendcloud.org`

#### 子域名 SPF

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机 | 子域名前缀（例如 sc） |
| 记录值 | v=spf1 include:sendcloud.org ~all |
| TTL | 600 |

---

## V. DKIM 配置

#### 作用

验证邮件签名，防止伪造。

#### 主域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机 | sendcloud._domainkey |
| 记录值 | k=rsa; p=your_public_key |
| TTL | 600 |

📌 注意：  
selector 可能会是：

- default._domainkey
- sc._domainkey

👉 **必须使用控制台提供的值。**

#### 子域名 DKIM

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机 | sendcloud._domainkey.subdomain_prefix |
| 记录值 | k=rsa; p=your_public_key |
| TTL | 600 |

---

## VI. MX 记录配置

#### 作用

指定负责接收邮件的邮件服务器。

#### 主域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| 主机 | @ |
| 记录值 | mx.sendcloud.org |
| 优先级 | 10 |
| TTL | 600 |

📌 注意：  
👉 建议仅保留 uSpeedo 的 MX 记录；其他 MX 可能导致接收异常。

#### 子域名 MX

| 字段 | 值 |
| :--- | :--- |
| 类型 | MX |
| 主机 | 子域名前缀 |
| 记录值 | mx.sendcloud.org |
| 优先级 | 10 |
| TTL | 600 |

---

## VII. DMARC 配置

#### 作用

防止邮件欺诈并提供策略控制。

#### 主域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机 | _dmarc |
| 记录值 | v=DMARC1; p=none; rua=mailto:xxx; ruf=mailto:xxx; fo=1 |
| TTL | 600 |

#### 参数说明

- `v=DMARC1`：协议版本
- `p=none`：监控模式
- `p=quarantine`：隔离（进垃圾箱）
- `p=reject`：拒收
- `rua`：汇总报告地址
- `ruf`：取证报告地址
- `fo=1`：失败触发机制

#### 子域名 DMARC

| 字段 | 值 |
| :--- | :--- |
| 类型 | TXT |
| 主机 | _dmarc.subdomain_prefix |
| 记录值 | v=DMARC1; p=none; rua=mailto:xxx |
| TTL | 600 |

---

## VIII. NS（域名服务器）说明

如果你是从其他 DNS 服务迁移到 GoDaddy：

你 **必须** 在域名注册商处更新 NS 记录。

否则：

❌ DNS 配置不会生效。

---

## IX. 注意事项

- DNS 示例仅供参考，请以控制台实际值为准
- 生效时间：
  - 通常：几分钟
  - 最长：24–48 小时（GoDaddy）
- 推荐 TTL：
  - 初始：300–600
  - 稳定后可提高

---

## X. 常见问题

#### 1. 验证失败？

检查：

- NS 是否正确指向 GoDaddy
- 记录值是否与控制台完全一致
- 是否存在冲突记录

#### 2. 为什么建议使用子域名？

👉 优势：

- 隔离发信信誉
- 不影响主域名（官网/品牌）
- 更适合营销邮件

