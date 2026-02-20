# 任务跟踪

## 当前版本：v1.0.0

### 已完成

- [x] 设计产品定位与核心功能需求（2026-02-20）
- [x] 创建 SKILL.md：操作流程、合规分析规则、报告生成规范（2026-02-20）
- [x] 创建 references/gdpr-framework.md：GDPR/ePrivacy/DSA 四模块差距检查清单（2026-02-20）
- [x] 创建 references/ccpa-framework.md：CCPA/CPRA 四模块差距检查清单（2026-02-20）
- [x] 创建 references/report-template.md：HTML 报告模板与变量说明（2026-02-20）
- [x] 创建 references/crawl-targets.md：爬取路径、多语言模式、追踪器检测列表（2026-02-20）
- [x] 创建 DECISIONS.md、TASKS.md、CHANGELOG.md、LICENSE.txt（2026-02-20）
- [x] 更新 README.md 添加新技能条目（2026-02-20）

---

## v1.1.0 路线图（待开发）

### 功能扩展

- [ ] 新增中国大陆市场支持（PIPL 个人信息保护法 + 数据安全法 + 网络安全法）
- [ ] 新增巴西市场支持（LGPD — Lei Geral de Proteção de Dados）
- [ ] 新增新加坡市场支持（PDPA — Personal Data Protection Act）
- [ ] 新增日本市场支持（APPI — Act on the Protection of Personal Information）

### 分析增强

- [ ] 增加对常见 CMP（OneTrust、Cookiebot、Usercentrics）配置的专项检测和评估
- [ ] 增加第三方追踪器与实际 Cookie 政策披露内容的交叉比对自动化
- [ ] 增加针对 DSA 适用规模（大型平台 / 超大型在线平台 VLOP）的自动判断逻辑
- [ ] 增加对网站语言版本与目标市场匹配度的评估（如面向德国市场但无德语隐私政策）

### 报告优化

- [ ] 支持生成 PDF 版本报告（通过 Playwright 打印或 wkhtmltopdf）
- [ ] 增加报告摘要页（Executive Summary），适合向管理层呈现
- [ ] 在雷达图中增加行业基准线（如同类网站平均分）用于对比
- [ ] 增加"本次整改优先级建议"模块，按投入产出比排列整改事项

### v2.0.0 规划（长期）

- [ ] 支持账号登录后的扫描（需用户提供凭证）
- [ ] 支持 Cookie 横幅的 JavaScript 渲染环境检测（需集成 Playwright 脚本）
- [ ] 支持对 DSAR 完整流程链路的端到端测试（提交→确认→响应）
- [ ] 跨语言内容一致性检测（各语言版本隐私政策的差异比对）
