# 报告 HTML 模板规范

本文档规定 `privacy-gap-report-[domain]-[YYYYMMDD].html` 的结构、样式和交互逻辑。

---

## 技术栈

| 组件 | 来源 | 用途 |
|------|------|------|
| Bootstrap 5.3 | CDN | 布局、组件、响应式 |
| Chart.js 4.x | CDN | 雷达图绘制 |
| 原生 JS | 内联 | 折叠面板、法条切换 |

所有资源通过 CDN 引入，报告文件为单一 HTML，无需额外文件即可打开。

---

## HTML 完整结构

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>数据隐私差距分析报告 — [DOMAIN]</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.4/dist/chart.umd.min.js"></script>
  <style>
    /* === 全局样式 === */
    body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif; background: #f8f9fa; color: #212529; }
    .report-header { background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%); color: white; padding: 3rem 2rem; }
    .report-header h1 { font-size: 1.8rem; font-weight: 700; margin-bottom: 0.5rem; }
    .report-header .meta { opacity: 0.85; font-size: 0.9rem; }
    .wisedpo-brand { color: #4cc9f0; font-weight: 600; }

    /* === 风险等级徽章 === */
    .badge-high { background-color: #dc3545; color: white; }
    .badge-medium { background-color: #fd7e14; color: white; }
    .badge-low { background-color: #ffc107; color: #212529; }
    .badge-risk { font-size: 0.7rem; font-weight: 700; padding: 0.3em 0.7em; border-radius: 4px; text-transform: uppercase; letter-spacing: 0.05em; }

    /* === 模块卡片 === */
    .module-card { background: white; border-radius: 12px; box-shadow: 0 2px 12px rgba(0,0,0,0.08); margin-bottom: 1.5rem; overflow: hidden; }
    .module-header { padding: 1.25rem 1.5rem; cursor: pointer; display: flex; align-items: center; justify-content: space-between; border-bottom: 1px solid #f0f0f0; transition: background 0.2s; }
    .module-header:hover { background: #f8f9fa; }
    .module-header h2 { font-size: 1.1rem; font-weight: 600; margin: 0; display: flex; align-items: center; gap: 0.75rem; }
    .module-icon { width: 36px; height: 36px; border-radius: 8px; display: flex; align-items: center; justify-content: center; font-size: 1.1rem; }
    .icon-ui { background: #e0f2fe; }
    .icon-privacy { background: #f0fdf4; }
    .icon-terms { background: #fff7ed; }
    .icon-cookie { background: #fdf4ff; }
    .module-score { font-size: 1.5rem; font-weight: 800; }
    .score-high { color: #dc3545; }
    .score-medium { color: #fd7e14; }
    .score-low-risk { color: #198754; }
    .module-body { padding: 0; }
    .module-body.collapsed { display: none; }

    /* === Wise DPO CTA 按钮 === */
    .wisedpo-cta { display: block; margin: 1rem 1.5rem; padding: 0.85rem 1.25rem; border-radius: 10px; background: linear-gradient(90deg, #0f3460, #16213e); color: white; text-decoration: none; font-weight: 600; font-size: 0.9rem; display: flex; align-items: center; justify-content: space-between; transition: opacity 0.2s; }
    .wisedpo-cta:hover { opacity: 0.88; color: white; }
    .wisedpo-cta .cta-arrow { font-size: 1.1rem; }
    .wisedpo-cta .cta-label { opacity: 0.75; font-size: 0.75rem; font-weight: 400; }

    /* === 差距卡片 === */
    .gap-card { border-left: 4px solid; margin: 0 1.5rem 1rem; border-radius: 0 8px 8px 0; padding: 1rem 1.25rem; background: #fafafa; }
    .gap-card.high { border-color: #dc3545; }
    .gap-card.medium { border-color: #fd7e14; }
    .gap-card.low { border-color: #ffc107; }
    .gap-title { font-weight: 600; font-size: 0.95rem; margin-bottom: 0.4rem; display: flex; align-items: center; gap: 0.5rem; }
    .gap-desc { font-size: 0.88rem; color: #444; margin-bottom: 0.75rem; }
    .gap-evidence { font-size: 0.82rem; background: #f0f4f8; border-radius: 6px; padding: 0.6rem 0.85rem; margin-bottom: 0.75rem; }
    .gap-evidence strong { color: #1a1a2e; }
    .gap-evidence code { font-size: 0.8rem; word-break: break-all; color: #0f3460; }
    .gap-remediation { font-size: 0.85rem; color: #2d6a4f; background: #d8f3dc; border-radius: 6px; padding: 0.6rem 0.85rem; margin-bottom: 0.5rem; }
    .gap-legal-btn { font-size: 0.75rem; color: #6c757d; border: 1px solid #dee2e6; background: none; border-radius: 4px; padding: 0.2em 0.6em; cursor: pointer; }
    .gap-legal-btn:hover { background: #f0f0f0; }
    .gap-legal-text { font-size: 0.8rem; color: #555; background: #fff3cd; border-radius: 6px; padding: 0.6rem 0.85rem; margin-top: 0.4rem; display: none; }
    .gap-legal-text.visible { display: block; }

    /* === 雷达图容器 === */
    .radar-container { max-width: 380px; margin: 0 auto; }

    /* === 扫描摘要 === */
    .scan-table td, .scan-table th { font-size: 0.85rem; }
    .status-ok { color: #198754; font-weight: 600; }
    .status-fail { color: #dc3545; font-weight: 600; }
    .status-warn { color: #fd7e14; font-weight: 600; }

    /* === 折叠图标 === */
    .chevron { transition: transform 0.25s; display: inline-block; }
    .chevron.open { transform: rotate(180deg); }

    /* === 法规 Tab === */
    .jurisdiction-tabs .nav-link { font-size: 0.85rem; color: #495057; }
    .jurisdiction-tabs .nav-link.active { color: #0f3460; font-weight: 600; border-color: #0f3460 #0f3460 white; }

    /* === 页脚 === */
    .report-footer { background: #1a1a2e; color: rgba(255,255,255,0.6); padding: 2rem; text-align: center; font-size: 0.8rem; }
    .report-footer a { color: #4cc9f0; }
  </style>
</head>
<body>

<!-- ===================== 1. 报告头部 ===================== -->
<div class="report-header">
  <div class="container">
    <p class="wisedpo-brand mb-2">Wise DPO · 数据隐私差距分析报告</p>
    <h1>{{DOMAIN}} 隐私合规差距分析</h1>
    <div class="meta mt-3 row g-3">
      <div class="col-auto">📅 生成时间：{{GENERATED_AT}}</div>
      <div class="col-auto">🌐 被测域名：<strong>{{DOMAIN}}</strong></div>
      <div class="col-auto">⚖️ 适用法规：{{JURISDICTIONS}}</div>
    </div>
    <div class="mt-3 p-3 rounded" style="background:rgba(255,255,255,0.1); font-size:0.8rem; max-width:700px;">
      <strong>声明：</strong>本报告仅基于对网站公开页面的自动化扫描，不构成法律意见。报告中"需人工核验"的项目须由具备资质的法律专业人士进一步评估。扫描范围详见下文。
    </div>
  </div>
</div>

<!-- ===================== 2. 扫描覆盖摘要 ===================== -->
<div class="container my-4">
  <div class="card rounded-3">
    <div class="card-header fw-semibold">扫描覆盖摘要</div>
    <div class="card-body p-0">
      <table class="table scan-table mb-0">
        <thead class="table-light">
          <tr>
            <th>页面 URL</th>
            <th>语言版本</th>
            <th>状态</th>
            <th>说明</th>
          </tr>
        </thead>
        <tbody>
          <!-- 示例行（Claude 填充实际数据） -->
          <tr>
            <td><code>https://example.com/</code></td>
            <td>英语</td>
            <td class="status-ok">✓ 成功</td>
            <td>检测到 Cookie 横幅</td>
          </tr>
          <tr>
            <td><code>https://example.com/privacy-policy</code></td>
            <td>英语、德语</td>
            <td class="status-ok">✓ 成功</td>
            <td>发现多语言版本</td>
          </tr>
          <tr>
            <td><code>https://example.com/cookie-policy</code></td>
            <td>—</td>
            <td class="status-fail">✗ 失败</td>
            <td>404 页面不存在</td>
          </tr>
          <tr>
            <td><code>https://example.com/account/privacy</code></td>
            <td>—</td>
            <td class="status-warn">⚠ 跳过</td>
            <td>需要登录，超出当前版本扫描范围</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="card-footer text-muted" style="font-size:0.8rem;">
      ⚠️ 当前版本（v1.0.0）仅扫描公开页面。需登录的账户页、购买流程、App 子系统、动态内容、DSAR 完整链路等超出范围，建议人工核验。
    </div>
  </div>
</div>

<!-- ===================== 3. 风险总览雷达图 ===================== -->
<div class="container my-4">
  <h2 class="fs-5 fw-bold mb-3">风险总览</h2>
  <div class="row g-3">

    <!-- GDPR 雷达图（若用户选择了 GDPR） -->
    <div class="col-md-6">
      <div class="card rounded-3 p-3 text-center">
        <h3 class="fs-6 fw-semibold mb-3">欧洲经济区（GDPR/ePrivacy/DSA）</h3>
        <div class="radar-container">
          <canvas id="gdprRadar"></canvas>
        </div>
        <!-- 分数说明 -->
        <div class="mt-3 d-flex justify-content-around text-center">
          <div><div class="fw-bold fs-5" style="color:#dc3545">{{SCORE_UI_GDPR}}</div><div style="font-size:0.75rem;color:#666">用户界面</div></div>
          <div><div class="fw-bold fs-5" style="color:#fd7e14">{{SCORE_PP_GDPR}}</div><div style="font-size:0.75rem;color:#666">隐私政策</div></div>
          <div><div class="fw-bold fs-5" style="color:#198754">{{SCORE_TOS_GDPR}}</div><div style="font-size:0.75rem;color:#666">用户协议</div></div>
          <div><div class="fw-bold fs-5" style="color:#0f3460">{{SCORE_COOKIE_GDPR}}</div><div style="font-size:0.75rem;color:#666">Cookie</div></div>
        </div>
      </div>
    </div>

    <!-- CCPA 雷达图（若用户选择了 CCPA） -->
    <div class="col-md-6">
      <div class="card rounded-3 p-3 text-center">
        <h3 class="fs-6 fw-semibold mb-3">美国加州（CCPA/CPRA）</h3>
        <div class="radar-container">
          <canvas id="ccpaRadar"></canvas>
        </div>
        <div class="mt-3 d-flex justify-content-around text-center">
          <div><div class="fw-bold fs-5" style="color:#dc3545">{{SCORE_UI_CCPA}}</div><div style="font-size:0.75rem;color:#666">用户界面</div></div>
          <div><div class="fw-bold fs-5" style="color:#fd7e14">{{SCORE_PP_CCPA}}</div><div style="font-size:0.75rem;color:#666">隐私政策</div></div>
          <div><div class="fw-bold fs-5" style="color:#198754">{{SCORE_TOS_CCPA}}</div><div style="font-size:0.75rem;color:#666">用户协议</div></div>
          <div><div class="fw-bold fs-5" style="color:#0f3460">{{SCORE_COOKIE_CCPA}}</div><div style="font-size:0.75rem;color:#666">Cookie</div></div>
        </div>
      </div>
    </div>

  </div>
</div>

<!-- ===================== 4. 差距详情 — 四个模块 ===================== -->
<div class="container my-4">
  <h2 class="fs-5 fw-bold mb-3">差距详情</h2>

  <!-- ====== 模块一：用户交互界面 ====== -->
  <div class="module-card">
    <div class="module-header" onclick="toggleModule('ui')">
      <h2>
        <span class="module-icon icon-ui">🖥️</span>
        模块一：用户交互界面
      </h2>
      <div class="d-flex align-items-center gap-3">
        <span class="module-score score-high" id="score-ui">{{SCORE_UI}}/100</span>
        <span class="chevron" id="chevron-ui">▼</span>
      </div>
    </div>
    <div class="module-body" id="body-ui">
      <!-- Wise DPO CTA -->
      <a href="#wisedpo-ui-design" class="wisedpo-cta">
        <div>
          <div>优化您的隐私合规交互设计</div>
          <div class="cta-label">Wise DPO · 用户交互页面设计服务</div>
        </div>
        <span class="cta-arrow">→</span>
      </a>

      <!-- 法规 Tab -->
      <div class="px-3 pb-2 jurisdiction-tabs">
        <ul class="nav nav-tabs" id="tabUI" role="tablist">
          <li class="nav-item"><button class="nav-link active" data-bs-toggle="tab" data-bs-target="#ui-gdpr">🇪🇺 GDPR</button></li>
          <li class="nav-item"><button class="nav-link" data-bs-toggle="tab" data-bs-target="#ui-ccpa">🇺🇸 CCPA/CPRA</button></li>
        </ul>
        <div class="tab-content pt-3">

          <!-- GDPR 差距列表（按高→中→低排序，Claude 填充） -->
          <div class="tab-pane active" id="ui-gdpr">

            <!-- 差距卡片示例（高风险） -->
            <div class="gap-card high">
              <div class="gap-title">
                <span class="badge-risk badge-high">高风险</span>
                拒绝按钮不同等显眼（Dark Pattern）
              </div>
              <div class="gap-desc">
                Cookie 横幅中"全部接受"按钮以醒目蓝色大按钮呈现，而"管理偏好"仅以灰色文字链接显示，拒绝路径比接受路径多 2 步操作，构成 EDPB 认定的黑暗模式。
              </div>
              <div class="gap-evidence">
                <strong>证据：</strong><br>
                URL：<code>https://example.com/</code><br>
                抓取文本：<code>"Accept All" [蓝色大按钮] | "Manage Preferences" [灰色小链接]</code>
              </div>
              <div class="gap-remediation">
                <strong>整改建议：</strong>将"拒绝所有"按钮调整为与"接受所有"按钮在视觉权重、颜色、尺寸上完全对等，确保用户拒绝路径与接受路径步骤数相同。参考 EDPB 03/2022 号意见对 Cookie 横幅设计的具体要求。
              </div>
              <button class="gap-legal-btn" onclick="toggleLegal(this)">⚖️ 查看法规依据</button>
              <div class="gap-legal-text">
                <strong>EDPB 03/2022 号意见（关于黑暗模式）：</strong><br>
                "接受 Cookie 的选项不得相较拒绝的选项更为突出……拒绝非必要 Cookie 的步骤数不得多于接受的步骤数。"<br><br>
                <strong>GDPR 第 7 条第 3 款：</strong><br>
                "撤回同意应与给予同意同样便利（shall be as easy to withdraw consent as to give it）。"
              </div>
            </div>

            <!-- 中风险示例 -->
            <div class="gap-card medium">
              <div class="gap-title">
                <span class="badge-risk badge-medium">中风险</span>
                Cookie 偏好中心缺失精细化控制
              </div>
              <div class="gap-desc">Cookie 横幅仅提供"全部接受"和"全部拒绝"选项，未提供按类别（功能性/分析性/营销性）精细化设置的界面，限制了用户的选择粒度。</div>
              <div class="gap-evidence">
                <strong>证据：</strong><br>
                URL：<code>https://example.com/</code><br>
                横幅 HTML 中仅发现两个按钮元素，无类别勾选框。
              </div>
              <div class="gap-remediation">
                <strong>整改建议：</strong>在 Cookie 横幅中增加"管理偏好"入口，展示按类别（必要/功能/分析/营销）分类的 Cookie 开关，并为每类提供简要说明。建议引入成熟的 CMP（同意管理平台）实现。
              </div>
              <button class="gap-legal-btn" onclick="toggleLegal(this)">⚖️ 查看法规依据</button>
              <div class="gap-legal-text">
                <strong>EDPB 05/2020 号指南（关于同意）：</strong><br>
                "同意必须是具体的……笼统的'同意所有 Cookie'不满足 GDPR 对具体同意的要求。"<br><br>
                <strong>法国 CNIL 建议（2020）：</strong><br>
                "用户应能够按类别接受或拒绝 Cookie，且不得对接受或拒绝的难度加以区分。"
              </div>
            </div>

            <!-- 低风险示例 -->
            <div class="gap-card low">
              <div class="gap-title">
                <span class="badge-risk badge-low">低风险</span>
                Cookie 横幅缺少"了解更多"链接
              </div>
              <div class="gap-desc">Cookie 横幅文案中未提供直接跳转到完整 Cookie 政策页面的链接，用户需自行在网站中查找 Cookie 相关信息。</div>
              <div class="gap-evidence">
                <strong>证据：</strong><br>
                URL：<code>https://example.com/</code><br>
                横幅文本未检测到指向 Cookie 政策的超链接。
              </div>
              <div class="gap-remediation">
                <strong>整改建议：</strong>在 Cookie 横幅文案中添加"了解更多"或"查看 Cookie 政策"链接，直接指向完整的 Cookie 政策页面。
              </div>
              <button class="gap-legal-btn" onclick="toggleLegal(this)">⚖️ 查看法规依据</button>
              <div class="gap-legal-text">
                <strong>EDPB 05/2020 号指南第 69 段：</strong><br>
                "横幅或弹窗应提供可进一步了解 Cookie 使用方式的链接，确保用户能够在同意前获得充分信息。"
              </div>
            </div>

          </div><!-- /gdpr tab -->

          <div class="tab-pane" id="ui-ccpa">
            <!-- CCPA 差距卡片（Claude 按实际分析结果填充） -->
            <p class="text-muted px-2 py-3" style="font-size:0.85rem;">（Claude 根据扫描结果填充 CCPA/CPRA 差距列表）</p>
          </div>

        </div><!-- /tab-content -->
      </div><!-- /jurisdiction-tabs -->
    </div><!-- /module-body -->
  </div><!-- /module-card ui -->

  <!-- ====== 模块二：隐私政策 ====== -->
  <div class="module-card">
    <div class="module-header" onclick="toggleModule('pp')">
      <h2>
        <span class="module-icon icon-privacy">📋</span>
        模块二：隐私政策
      </h2>
      <div class="d-flex align-items-center gap-3">
        <span class="module-score score-medium" id="score-pp">{{SCORE_PP}}/100</span>
        <span class="chevron" id="chevron-pp">▼</span>
      </div>
    </div>
    <div class="module-body collapsed" id="body-pp">
      <a href="#wisedpo-privacy-policy" class="wisedpo-cta">
        <div>
          <div>一键生成合规隐私政策</div>
          <div class="cta-label">Wise DPO · 隐私政策生成器</div>
        </div>
        <span class="cta-arrow">→</span>
      </a>
      <div class="px-3 pb-2 jurisdiction-tabs">
        <ul class="nav nav-tabs" id="tabPP" role="tablist">
          <li class="nav-item"><button class="nav-link active" data-bs-toggle="tab" data-bs-target="#pp-gdpr">🇪🇺 GDPR</button></li>
          <li class="nav-item"><button class="nav-link" data-bs-toggle="tab" data-bs-target="#pp-ccpa">🇺🇸 CCPA/CPRA</button></li>
        </ul>
        <div class="tab-content pt-3">
          <div class="tab-pane active" id="pp-gdpr">
            <!-- Claude 填充 GDPR 隐私政策差距 -->
          </div>
          <div class="tab-pane" id="pp-ccpa">
            <!-- Claude 填充 CCPA 隐私政策差距 -->
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== 模块三：用户协议 ====== -->
  <div class="module-card">
    <div class="module-header" onclick="toggleModule('tos')">
      <h2>
        <span class="module-icon icon-terms">📄</span>
        模块三：用户协议
      </h2>
      <div class="d-flex align-items-center gap-3">
        <span class="module-score score-low-risk" id="score-tos">{{SCORE_TOS}}/100</span>
        <span class="chevron" id="chevron-tos">▼</span>
      </div>
    </div>
    <div class="module-body collapsed" id="body-tos">
      <a href="#wisedpo-terms" class="wisedpo-cta">
        <div>
          <div>一键生成合规用户协议</div>
          <div class="cta-label">Wise DPO · 用户协议生成器</div>
        </div>
        <span class="cta-arrow">→</span>
      </a>
      <div class="px-3 pb-2 jurisdiction-tabs">
        <ul class="nav nav-tabs" id="tabTOS" role="tablist">
          <li class="nav-item"><button class="nav-link active" data-bs-toggle="tab" data-bs-target="#tos-gdpr">🇪🇺 GDPR/DSA</button></li>
          <li class="nav-item"><button class="nav-link" data-bs-toggle="tab" data-bs-target="#tos-ccpa">🇺🇸 CCPA/CPRA</button></li>
        </ul>
        <div class="tab-content pt-3">
          <div class="tab-pane active" id="tos-gdpr">
            <!-- Claude 填充 -->
          </div>
          <div class="tab-pane" id="tos-ccpa">
            <!-- Claude 填充 -->
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ====== 模块四：Cookie 与追踪技术 ====== -->
  <div class="module-card">
    <div class="module-header" onclick="toggleModule('cookie')">
      <h2>
        <span class="module-icon icon-cookie">🍪</span>
        模块四：Cookie 与追踪技术
      </h2>
      <div class="d-flex align-items-center gap-3">
        <span class="module-score score-high" id="score-cookie">{{SCORE_COOKIE}}/100</span>
        <span class="chevron" id="chevron-cookie">▼</span>
      </div>
    </div>
    <div class="module-body collapsed" id="body-cookie">
      <a href="#wisedpo-cookie-platform" class="wisedpo-cta">
        <div>
          <div>部署专业 Cookie 合规管理平台</div>
          <div class="cta-label">Wise DPO · Cookie 管理平台（CMP）</div>
        </div>
        <span class="cta-arrow">→</span>
      </a>
      <div class="px-3 pb-2 jurisdiction-tabs">
        <ul class="nav nav-tabs" id="tabCookie" role="tablist">
          <li class="nav-item"><button class="nav-link active" data-bs-toggle="tab" data-bs-target="#cookie-gdpr">🇪🇺 ePrivacy/GDPR</button></li>
          <li class="nav-item"><button class="nav-link" data-bs-toggle="tab" data-bs-target="#cookie-ccpa">🇺🇸 CCPA/CPRA</button></li>
        </ul>
        <div class="tab-content pt-3">
          <div class="tab-pane active" id="cookie-gdpr">
            <!-- Claude 填充 -->
          </div>
          <div class="tab-pane" id="cookie-ccpa">
            <!-- Claude 填充 -->
          </div>
        </div>
      </div>
    </div>
  </div>

</div><!-- /container 差距详情 -->

<!-- ===================== 5. 页脚 ===================== -->
<div class="report-footer">
  <p>本报告由 <strong>Wise DPO</strong> 数据隐私差距分析工具自动生成 · {{GENERATED_AT}}</p>
  <p>本报告仅供参考，不构成法律意见。如需专业法律咨询，请联系具备资质的数据隐私律师。</p>
  <p><a href="https://wisedpo.com">Wise DPO 平台</a> · 隐私政策生成器 · 用户协议生成器 · Cookie 管理平台 · 用户交互设计</p>
</div>

<!-- ===================== JavaScript ===================== -->
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
<script>
  // --- 模块折叠 ---
  function toggleModule(id) {
    const body = document.getElementById('body-' + id);
    const chevron = document.getElementById('chevron-' + id);
    body.classList.toggle('collapsed');
    chevron.classList.toggle('open');
  }

  // --- 法条显示/隐藏 ---
  function toggleLegal(btn) {
    const legalText = btn.nextElementSibling;
    legalText.classList.toggle('visible');
    btn.textContent = legalText.classList.contains('visible') ? '⚖️ 收起法规依据' : '⚖️ 查看法规依据';
  }

  // --- 雷达图绘制 ---
  // （Claude 根据实际计算的分数填充 data 数组）
  const radarOptions = {
    scales: {
      r: {
        min: 0, max: 100,
        ticks: { stepSize: 20, font: { size: 10 } },
        pointLabels: { font: { size: 11 } },
        grid: { color: 'rgba(0,0,0,0.08)' }
      }
    },
    plugins: { legend: { display: false } },
    elements: { line: { borderWidth: 2 } }
  };

  // GDPR 雷达图（如用户未选择 GDPR，则注释掉此段）
  new Chart(document.getElementById('gdprRadar'), {
    type: 'radar',
    data: {
      labels: ['用户交互界面', '隐私政策', '用户协议', 'Cookie & 追踪'],
      datasets: [{
        data: [{{SCORE_UI_GDPR}}, {{SCORE_PP_GDPR}}, {{SCORE_TOS_GDPR}}, {{SCORE_COOKIE_GDPR}}],
        backgroundColor: 'rgba(15,52,96,0.15)',
        borderColor: '#0f3460',
        pointBackgroundColor: '#0f3460',
        pointRadius: 4
      }]
    },
    options: radarOptions
  });

  // CCPA 雷达图（如用户未选择 CCPA，则注释掉此段）
  new Chart(document.getElementById('ccpaRadar'), {
    type: 'radar',
    data: {
      labels: ['用户交互界面', '隐私政策', '用户协议', 'Cookie & 追踪'],
      datasets: [{
        data: [{{SCORE_UI_CCPA}}, {{SCORE_PP_CCPA}}, {{SCORE_TOS_CCPA}}, {{SCORE_COOKIE_CCPA}}],
        backgroundColor: 'rgba(220,53,69,0.12)',
        borderColor: '#dc3545',
        pointBackgroundColor: '#dc3545',
        pointRadius: 4
      }]
    },
    options: radarOptions
  });
</script>

</body>
</html>
```

---

## 模板变量说明

| 变量 | 说明 | 示例值 |
|------|------|-------|
| `{{DOMAIN}}` | 被测域名 | `example.com` |
| `{{GENERATED_AT}}` | 报告生成时间（ISO 格式） | `2026-02-20 14:30 UTC` |
| `{{JURISDICTIONS}}` | 适用法规列表 | `GDPR/ePrivacy/DSA；CCPA/CPRA` |
| `{{SCORE_UI_GDPR}}` | 用户交互界面 GDPR 得分（0-100） | `40` |
| `{{SCORE_PP_GDPR}}` | 隐私政策 GDPR 得分（0-100） | `60` |
| `{{SCORE_TOS_GDPR}}` | 用户协议 GDPR 得分（0-100） | `80` |
| `{{SCORE_COOKIE_GDPR}}` | Cookie GDPR 得分（0-100） | `20` |
| `{{SCORE_UI_CCPA}}` | 用户交互界面 CCPA 得分（0-100） | `50` |
| `{{SCORE_PP_CCPA}}` | 隐私政策 CCPA 得分（0-100） | `55` |
| `{{SCORE_TOS_CCPA}}` | 用户协议 CCPA 得分（0-100） | `75` |
| `{{SCORE_COOKIE_CCPA}}` | Cookie CCPA 得分（0-100） | `30` |
| `{{SCORE_UI}}` / `{{SCORE_PP}}` / `{{SCORE_TOS}}` / `{{SCORE_COOKIE}}` | 多法规下各模块综合最低分（取最差值） | `40` |

---

## 差距卡片 HTML 片段（可复用）

Claude 在填充差距内容时，按以下模板重复生成：

```html
<div class="gap-card [high|medium|low]">
  <div class="gap-title">
    <span class="badge-risk badge-[high|medium|low]">[高风险|中风险|低风险]</span>
    [差距名称]
  </div>
  <div class="gap-desc">[差距具体描述：现状是什么，为什么不合规]</div>
  <div class="gap-evidence">
    <strong>证据：</strong><br>
    URL：<code>[页面URL]</code><br>
    [抓取到的原文片段 或 HTML 元素描述]
  </div>
  <div class="gap-remediation">
    <strong>整改建议：</strong>[具体可操作的整改步骤]
  </div>
  <button class="gap-legal-btn" onclick="toggleLegal(this)">⚖️ 查看法规依据</button>
  <div class="gap-legal-text">
    <strong>[法规名称 + 条款编号]：</strong><br>
    [法条原文或权威官方翻译]
  </div>
</div>
```

---

## 若仅选择单一法规

若用户仅选择 GDPR 或仅选择 CCPA，则：
1. 删除未选法规对应的雷达图 `<canvas>` 及 JS 代码
2. 删除模块 Tab 中未选法规的 Tab 项，保留唯一 Tab 时直接展示内容（不需 Tab 切换）
3. 综合得分标题相应调整为单一法规名称
