# 爬取目标路径与多语言 URL 模式

本文档列出爬取各类型页面时需按优先级尝试的 URL 路径，以及常见语言版本的路径前缀/后缀模式。

---

## 1. 隐私政策页面（Privacy Policy）

按优先级顺序尝试以下路径，找到有效内容即停止：

```
/privacy-policy
/privacy
/legal/privacy
/legal/privacy-policy
/about/privacy
/info/privacy
/site/privacy
/help/privacy
/privacy-notice
/data-privacy
/data-protection
/privacy-statement
/datenschutz                  # 德语
/datenschutzerklarung         # 德语变体
/datenschutzerklaerung        # 德语变体
/politique-de-confidentialite  # 法语
/politique-confidentialite     # 法语
/informativa-privacy           # 意大利语
/privacybeleid                 # 荷兰语
/politica-de-privacidad        # 西班牙语
/politica-privacidad           # 西班牙语
/politica-de-privacidade       # 葡萄牙语
/prywatnosc                    # 波兰语
/privacy.html
/privacy-policy.html
```

**多语言子路径模式：**

若主路径找到隐私政策，还需尝试以下语言子路径（爬取所有可用语言版本）：

```
/de/privacy-policy             # 德语版
/fr/privacy-policy             # 法语版
/es/privacy-policy             # 西班牙语版
/it/privacy-policy             # 意大利语版
/nl/privacy-policy             # 荷兰语版
/pl/privacy-policy             # 波兰语版
/pt/privacy-policy             # 葡萄牙语版
/zh/privacy-policy             # 简体中文版
/zh-tw/privacy-policy         # 繁体中文版
/ja/privacy-policy             # 日语版
/ko/privacy-policy             # 韩语版
```

---

## 2. 用户协议页面（Terms of Service）

```
/terms
/terms-of-service
/terms-and-conditions
/tos
/legal/terms
/legal/terms-of-service
/about/terms
/info/terms
/user-agreement
/service-agreement
/eula
/nutzungsbedingungen           # 德语
/conditions-utilisation        # 法语
/condizioni-di-servizio        # 意大利语
/terminos-de-servicio          # 西班牙语
/termos-de-servico             # 葡萄牙语
/terms.html
/tos.html
```

---

## 3. Cookie 政策页面（Cookie Policy）

```
/cookie-policy
/cookies
/cookie-notice
/cookie-statement
/legal/cookies
/legal/cookie-policy
/info/cookies
/cookie-declaration
/cookie-preferences
/cookiebeleid                  # 荷兰语
/politique-cookies             # 法语
/cookie-richtlinie             # 德语
/politica-cookie               # 意大利语/西班牙语
/cookies.html
/cookie-policy.html
```

---

## 4. 法律声明 / 综合法律页面

```
/legal
/legal-notice
/legal-information
/impressum                     # 德语法律声明（通常含联系信息）
/mentions-legales              # 法语法律声明
/aviso-legal                   # 西班牙语法律声明
/legal.html
/disclaimer
/imprint
```

---

## 5. 首页相关检测（Homepage）

首页（`/`）需检测的元素：

| 检测目标 | 检测方式 | 备注 |
|---------|---------|------|
| Cookie 同意横幅 | 关键词搜索：`cookie`、`consent`、`accept`、`gdpr`、`ccpa` | 搜索 HTML 结构中的横幅容器 |
| Cookie 横幅按钮 | 关键词：`Accept All`、`Reject All`、`Manage`、`Preferences`、`Do Not Sell` | 检测按钮文案与视觉权重 |
| 底部导航链接 | 关键词：`privacy`、`cookie`、`terms`、`legal`、`datenschutz`、`impressum` | 汇总页脚中的法律链接 |
| CCPA 退出链接 | 关键词：`do not sell`、`do not share`、`your privacy choices`、`opt-out` | 检测是否存在及位置 |
| DSAR 入口 | 关键词：`data request`、`privacy request`、`subject access`、`your rights` | 检测用户权利行使入口 |
| 第三方脚本 | 脚本 src 检测已知追踪域名（见下方列表） | 推断 Cookie 类型 |

---

## 6. 已知第三方追踪器检测列表

检测首页及其他页面 HTML 中 `<script src>` 标签是否包含以下域名，用于"Cookie 与追踪技术"模块的分析：

| 追踪器 | 检测域名 | 类型 |
|-------|---------|------|
| Google Analytics 4 | `googletagmanager.com`、`google-analytics.com` | 分析 |
| Google Ads | `googleadservices.com`、`googlesyndication.com` | 营销 |
| Meta Pixel | `connect.facebook.net`、`facebook.com/tr` | 营销 |
| LinkedIn Insight Tag | `snap.licdn.com`、`linkedin.com` | 营销 |
| TikTok Pixel | `analytics.tiktok.com` | 营销 |
| Twitter/X Pixel | `static.ads-twitter.com`、`t.co` | 营销 |
| HubSpot | `js.hs-scripts.com`、`hubspot.com` | 营销/分析 |
| Hotjar | `static.hotjar.com` | 行为分析 |
| Mixpanel | `cdn.mxpnl.com` | 分析 |
| Segment | `cdn.segment.com` | 数据管道 |
| Intercom | `widget.intercom.io` | 功能/营销 |
| Drift | `js.driftt.com` | 营销 |
| Zendesk | `static.zdassets.com` | 功能 |
| Cookiebot | `consent.cookiebot.com` | CMP（说明使用了 CMP） |
| OneTrust | `cdn.cookielaw.org` | CMP |
| Usercentrics | `app.usercentrics.eu` | CMP |
| TrustArc | `consent.trustarc.com` | CMP |

**注：** 检测到 CMP（Cookiebot、OneTrust 等）不自动视为合规，仍需评估 CMP 配置是否正确（如是否在同意前加载非必要脚本）。

---

## 7. CCPA 专项检测路径

```
/do-not-sell
/do-not-sell-my-personal-information
/ccpa
/california-privacy-rights
/privacy-rights
/opt-out
/your-privacy-choices
/privacy-choices
/limit-sensitive-personal-information
/dsar
/data-subject-request
/privacy-request
```

---

## 8. 爬取失败分类标准

| 失败类型 | 判定标准 | 在报告中的标注 |
|---------|---------|--------------|
| 页面不存在 | HTTP 404 | ✗ 404 — 页面不存在 |
| 访问被拒 | HTTP 403 / 401 | ✗ 403 — 需要权限 |
| 服务器错误 | HTTP 5xx | ✗ 5xx — 服务器错误 |
| 需要登录 | 重定向至登录页 | ⚠ 需要登录 — 超出扫描范围 |
| 反爬保护 | 返回 Cloudflare/bot 检测页 | ⚠ 反爬保护 — 无法抓取 |
| 内容为空 | HTTP 200 但正文少于 100 字 | ⚠ 内容为空 |
| 动态渲染 | HTTP 200 但 HTML 为 SPA 占位符 | ⚠ 动态渲染 — 可能需 JS 执行 |
| 超时 | 30 秒内无响应 | ✗ 超时 |
