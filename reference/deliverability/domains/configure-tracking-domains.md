---
sidebar_label: '如何配置追踪域名？'
sidebar_position: 4
---

# 如何配置追踪域名？

## 一、什么是追踪域名？
**追踪域名**是专门用于邮件中链接点击统计和打开统计的独立域名。
当收件人点击邮件中的链接或加载图片时，请求会先经过该域名，从而实现行为统计与数据分析。

示例：
原始链接：

```
https://yourwebsite.com/promo
```


使用追踪域名后：
```
https://track.yourdomain.com/click/abc123
```


---

## 二、为什么需要使用追踪域名？
使用邮件服务商提供的默认公共追踪域名可能会带来以下问题：
- ❌ 降低品牌可信度（链接域名不一致）
- ❌ 更容易被判定为垃圾邮件
- ❌ 多用户共享域名，信誉不可控

使用**自定义追踪域名**可以：
- ✅ 提升邮件送达率（域名对齐）
- ✅ 强化品牌一致性
- ✅ 提升用户点击信任度
- ✅ 建立独立域名信誉
- ✅ 实现更精准的数据追踪

---

## 三、追踪域名核心功能
### 1. 点击追踪
邮件内所有链接会被重写为追踪链接：

```
https://track.yourdomain.com/click/{tracking_id}
```


用户点击后：
1. 先访问追踪服务器
2. 记录点击行为（时间、IP、设备等）
3. 302 重定向到原始目标页面

### 2. 打开追踪
通过嵌入透明像素（追踪像素）实现：

```
https://track.yourdomain.com/open/{tracking_id}.png
```


邮件被打开时：
- 图片加载
- 记录打开行为

### 3. 域名对齐
追踪域名与发信域名保持一致（或同根域名）：

```
发信域名：mail.yourdomain.com
追踪域名：track.yourdomain.com
```


优势：
- 提升 SPF / DKIM / DMARC 对齐效果
- 提高收件箱送达率
- 降低被拦截概率

---

## 四、推荐域名结构
我们建议使用子域名作为追踪域名。

| 类型 | 示例 |
| :--- | :--- |
| 根域名 | yourdomain.com |
| 发信域名 | mail.yourdomain.com |
| 追踪域名 | track.yourdomain.com |

---

## 五、如何配置追踪域名
### 步骤 1：添加追踪域名
在邮件平台控制台中：
- 进入 **设置** → **追踪域名**
- 点击 **添加追踪域名**
- 输入您的子域名（例如 track.yourdomain.com）

### 步骤 2：获取 DNS 记录
系统会提供一条或多条 CNAME 记录，示例：

```
track.yourdomain.com CNAME tracking.provider.com
```


### 步骤 3：配置 DNS
登录您的 DNS 服务商（如 Cloudflare、GoDaddy）并添加记录：
- 类型：CNAME
- 主机记录：track
- 指向地址：平台提供的追踪地址

### 步骤 4：等待生效
- 常规：10–30 分钟
- 最长：24 小时

### 步骤 5：验证域名
返回平台点击 **验证**，确认解析成功。

---

## 六、追踪域名工作流程

<div className="doc-flowchart">

<p className="doc-flowchart-step">用户点击邮件链接</p>

<div className="doc-flowchart-arrow" aria-hidden="true">↓</div>

<p className="doc-flowchart-step">访问 <code>track.yourdomain.com</code></p>

<div className="doc-flowchart-arrow" aria-hidden="true">↓</div>

<p className="doc-flowchart-step">记录点击数据</p>

<div className="doc-flowchart-arrow" aria-hidden="true">↓</div>

<p className="doc-flowchart-step">302 重定向</p>

<div className="doc-flowchart-arrow" aria-hidden="true">↓</div>

<p className="doc-flowchart-step">跳转到目标页面</p>

</div>

---

## 七、最佳实践
### 1. 使用独立子域名
推荐：

```
track.yourdomain.com
```


避免使用根域名，防止影响主站 SEO 与安全策略。

### 2. 与发信域名使用同根域名

```
mail.yourdomain.com
track.yourdomain.com
```


有助于提升邮件信誉与送达效果。

### 3. 开启 HTTPS
确保追踪域名支持 SSL/TLS：
- 避免浏览器“不安全”提示
- 提升用户信任度
- 符合现代邮箱客户端要求

### 4. 不要频繁更换域名
频繁更换追踪域名会：
- 重置域名信誉
- 降低点击率
- 打断数据连续性

### 5. 不同环境使用不同域名

| 环境 | 示例 |
| :--- | :--- |
| 生产环境 | track.yourdomain.com |
| 测试环境 | track-test.yourdomain.com |

---

## 八、常见问题
### 1. 点击后跳转缓慢
原因：
- DNS 解析未完全生效
- 追踪服务器延迟

解决方案：
- 检查 DNS 解析状态
- 联系服务商支持

### 2. 链接被拦截或标记为风险
原因：
- 使用公共追踪域名
- 域名信誉较低

解决方案：
- 使用自定义追踪域名
- 避免发送垃圾内容

### 3. 打开率不准确
原因：
- 用户禁用图片加载
- 邮箱客户端缓存

说明：
- 打开率仅为参考指标

---

## 九、与其他域名的关系

| 类型 | 功能 |
| :--- | :--- |
| 发信域名 | 邮件发信身份（MAIL FROM） |
| 追踪域名 | 点击与打开行为统计 |
| 回复域名 | 接收用户回复邮件 |

建议三者统一使用同根域名，提升整体信誉。

