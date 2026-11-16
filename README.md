# 检械通 MedTestLink — 医械检测对接专家

> **Status: this pack is an expert/agent card, not a server — and the connector it depends on exists.**
> `medxpert-ortho-mcp` is published on PyPI and in the official MCP Registry, and a real `uvx` handshake
> returns its 8 tools. Install it:
>
> ```bash
> uvx medxpert-ortho-mcp
> ```
>
> **The version number is deliberately not written here.** It changed twice in one day; a number frozen in
> a card goes stale without anyone noticing. Ask the registry instead
> (`io.github.zhaoxinghua09-cell/medxpert-ortho-mcp`), or just run the install line above.
>
> The tools run on illustrative sample data and mark every result `sample_data: true`. Point
> `ORTHO_DATA_DIR` at your own datasets for real work, and run
> `python -m medxpert_ortho.server --validate-data` first — it fails loudly on a wrong path, a broken JSON
> file or a structurally bad dataset rather than quietly answering from the samples.
>
> This repository still ships no `server.py`: it is a card that a host loads, and the capability lives in
> the connector.
>
> **状态：本包是专家/Agent 卡片，不是服务；它所依赖的连接器已经存在。**`medxpert-ortho-mcp` 已发布在 PyPI 与官方 MCP Registry，并由真实 `uvx` 握手验证——返回 8 个工具。安装：
>
> ```bash
> uvx medxpert-ortho-mcp
> ```
>
> **版本号有意不写在这里。** 它在一天之内变了两次；冻在卡片里的数字会悄悄过期。要版本请查 Registry（`io.github.zhaoxinghua09-cell/medxpert-ortho-mcp`），或者直接跑上面的安装命令。
>
> 工具跑在示例数据上，每个返回值都标 `sample_data: true`。正式使用请把 `ORTHO_DATA_DIR` 指向你自己的数据集，并先跑 `python -m medxpert_ortho.server --validate-data`——路径错、JSON 坏、结构不合规都会报错退出，而不是拿示例悄悄作答。
>
> 本仓仍然没有 `server.py`：它是宿主加载的卡片，能力在连接器里。

## 许可说明 · License Notice

- **权利状态**：本仓库以 **MIT 许可** 许可发布，可依该许可证条款自由使用、修改与再分发。
- **引用建议**：引用时请标注仓库名与原文链接 `https://github.com/zhaoxinghua09-cell/medical-device-testing-link`
  与权利人「赵兴华 / Steven Zhao·China」。
- **品牌状态限定**：MedXpert、SynomosAI、LGD 等为相关项目标识，
  **均未申请实体注册、未申请商标注册**；出现仅作来源标识，
  不构成对法人实体或商标权的任何主张。
- **完整条款**：见仓库根目录 [LICENSE](LICENSE)。
- **联系**：zhaoxinghua06@126.com ｜ ORCID 0009-0001-0512-1237

---


医械（尤其骨科植入物）检测全链路对接：从器械推导检验项目，匹配比价检测机构，设计 A+B 分包方案，核算报价交期，校验 UDI 并提醒法规节点。

## 类型

Agent 型（单个 AI 专家）

## 功能

- **检验项目推导**：18 类骨科器械 × 附加属性（无菌/植入/血路/金属/涂层/MRI）→ 完整检验清单
- **机构匹配比价**：六类检验（注册/委托/抽检/出口/自检/研发）的 CMA 资质要求筛选，资质四态判定，覆盖率高亮
- **报价交期核算**：分项费用、8% 管理费、加急系数、关键路径交期，支持横向比价
- **A+B 分包方案**：主检优选 + 分包补位，未覆盖项与降级提示
- **UDI 校验解析**：GS1 校验位 + AI 串（括号/FNC1 格式）
- **法规节点提醒**：GMP 2025（107号公告 2026-11-01）、EU MDR/IVDR 过渡期倒计时

能力底座：MCP 服务 `medxpert-ortho-mcp`（8 工具，零依赖，本地闭环），
源码见 ortho-mcp 连接器（同套发布），机构/标准/报价数据已外置为可替换的示例数据。

## 使用示例

- 「帮我的髋关节假体（无菌要求）找检测机构，出个报价和交期」
- 「13 个检验项目一家机构接不全，帮我设计 A+B 分包方案」
- 「帮我校验解析这个 UDI：(01)06901234567892(10)ABC，另外 GMP 2025 还有多少天施行？」

## ⚠️ 数据口径

机构库/标准库为演示数据，输出均附声明；正式委托前须向机构核验最新资质与报价。

## 头像

`avatars/medical-device-testing-link.png`（ImageGen 生成，可手动替换，PNG 512×512 ≤500KB）

## 安装

将专家包目录放到专家目录下：

```
你的专家插件目录（如 ~/.workbuddy/.../plugins/<市场名>/plugins/ 下）
```

然后运行注册命令使其可见：

```bash
python3 scripts/register_expert.py <expert-dir>
```

## 打包分享

```bash
zip -r medical-device-testing-link.zip medical-device-testing-link/
```

---
2026-09-03 by 潘布达（Buda Pan）
