## [1.0.0] - 2026-02-20

### 新增

- 创建 `privacy-gap-analysis` 技能，支持 GDPR/ePrivacy/DSA 与 CCPA/CPRA 两大法律框架的网站隐私合规差距分析
- SKILL.md：完整六步操作流程（输入→确认→子域探测→爬取→分析→报告生成）
- `references/gdpr-framework.md`：四模块（用户交互界面、隐私政策、用户协议、Cookie）GDPR 差距检查清单，含高/中/低风险分级和法规依据原文
- `references/ccpa-framework.md`：四模块 CCPA/CPRA 差距检查清单，含 CPRA 2023 年新增权利（更正权、限制敏感信息使用权）和 GPC 合规要求
- `references/report-template.md`：单文件 HTML 报告模板，使用 Bootstrap 5 + Chart.js，含雷达图、可折叠模块、差距卡片、法条展开交互和 Wise DPO 产品 CTA
- `references/crawl-targets.md`：多语言隐私政策/用户协议/Cookie 政策 URL 路径列表、已知第三方追踪器检测列表、爬取失败分类标准
- DECISIONS.md：6 项关键设计决策记录
- TASKS.md：v1.0.0 完成项目与 v1.1.0/v2.0.0 路线图

### 技术规格

- 报告格式：自包含单文件 HTML，无需服务端
- 分析框架：GDPR（EU 2016/679）+ ePrivacy 指令（2002/58/EC）+ DSA（EU 2022/2065）+ CCPA/CPRA（Cal. Civ. Code § 1798.100 et seq.）+ CPPA 2023 最终规则
- 风险评分：初始 100 分，高风险 -20、中风险 -10、低风险 -5，最低 0 分
- 扫描范围：仅公开页面（v1.0.0 限制）
