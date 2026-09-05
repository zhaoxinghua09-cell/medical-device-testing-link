---
name: medical-device-testing
description: 医械检测对接 MCP 工具操作手册（ortho-mcp，8 工具）。当需要为医疗器械（尤其骨科植入物）匹配检测机构、推导检验项目、核算报价交期、设计A+B分包、校验UDI、查询法规节点时使用。通过 MCP 服务 medxpert-ortho-mcp 调用。
---

# 医械检测对接工具手册

本技能配套 MCP 服务 **`medxpert-ortho-mcp`**（stdio，零依赖，本地闭环）。
服务端见 ortho-mcp 连接器（同套发布），机构/标准/报价数据外置于 ortho-data.json，可替换为真实数据。

## ⚠️ 数据口径（每次输出必声明）

机构库 20 家、标准库 43 条、器械 18 类均为**演示数据**。输出任何候选榜/报价前必须注明
「基于演示数据，正式委托前须向机构核验最新资质与报价」。

## 8 个工具速查

| 工具 | 何时调 | 必填参数 |
|---|---|---|
| `match_labs` | 求机构候选榜 | `deviceId` 或 `standardIds`（二选一，可叠加） |
| `calc_quote` | 指定机构出报价 | `labId` + `standardIds` |
| `find_subcontract` | 一家接不全时求 A+B 分包 | `standardIds`（可带 `deviceId`） |
| `list_devices` | 用户说不清品类时展示目录 / 查器械 | 无（可选 `keyword`） |
| `list_standards` | 查标准详情（送样量/基准价/现行状态） | 无（可选 `keyword`/`cat`） |
| `get_lab` | 查机构详情（资质四态/健康分/覆盖范围） | `labId` |
| `check_udi` | 校验/解析 UDI | `gtin` 或 `ai` |
| `list_deadlines` | 法规节点与倒计时 | 无 |

## 关键参数口径

- **deviceId**：D01-D18。常用：D01 股骨柄 / D02 髋臼杯 / D04 膝关节 / D09 金属接骨板 / D17 3D打印多孔植入物。全目录用 `list_devices` 查。
- **attrs**（附加属性，影响检验项目推导）：`sterile` 无菌（+7项）/ `implant` 植入 / `blood` 血路接触 / `metal` 金属 / `coating` 涂层 / `mri` 核磁兼容。
- **examId**（检验类型与 CMA 要求）：E1 注册检验 / E2 委托检验 / E3 监督抽检（均须 CMA）· E4 出口检验（CNAS 互认更重要）· E5 自检 / E6 研发摸底（无强制 CMA）。
- **rush**：加急；**region**：地域偏好（省/市名，如「广东」）；**top**：返回前 N 家（默认 5）。

## 典型调用序列

**场景 A：用户给产品求机构**
1. 品类不明 → `list_devices` 让用户选；
2. `match_labs {deviceId:"D01", attrs:{sterile:true}, examId:"E1", top:5}` 出榜；
3. 锁定候选 → `calc_quote {labId:"L03", standardIds:[...], rush:true}` 比价、`get_lab` 查详情。

**场景 B：覆盖不全求分包**
1. `match_labs` 结果中有 `coverage.miss` 非空且无 `subcontract` 字段 →
2. `find_subcontract {standardIds:[...], examId:"E2"}` 取主检+分包组合；
3. 关注返回的 `uncovered`（未覆盖项）与 `degradedToDeclare`（CMA→CNAS 降级）。

**场景 C：UDI / 法规**
- `check_udi {gtin:"4006381333931", ai:"(01)06901234567892(10)ABC"}` → gtin.valid + ai 解析；
- `list_deadlines {}` → GMP 2025（107号公告 2026-11-01）等，`level` 为 red 的必须主动提醒。

## 错误处理

- 返回 `isError` 时读 text 里的中文错误信息，通常是 ID 拼错或缺参数——先用 `list_devices`/`list_standards` 查证合法 ID 再重试，**禁止编造 ID**。
- 服务未挂载（会话重启后首次使用）时，提示用户确认已安装 ortho-mcp 连接器并重启 WorkBuddy 会话。

## 维护

数据更新：按 ortho-mcp 连接器 SAMPLE_DATA.md 修改 ortho-data.json 后，跑连接器目录的 `node test_core.js` 与 `python test_server.py` 双层回归。
