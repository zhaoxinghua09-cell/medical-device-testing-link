# 安全审计（SECURITY）— 检械通 MedTestLink

## 资产构成

| 文件 | 类型 | 网络 | 凭据 | 说明 |
|---|---|---|---|---|
| agents/medical-device-testing-link.md | Agent 提示词 | 否 | 否 | 对话式对接逻辑，无代码执行 |
| skills/medical-device-testing/SKILL.md | 技能定义 | 否 | 否 | 工具调用手册 |
| avatars/medical-device-testing-link.png | 头像 | 否 | 否 | 生成图，≤500KB |
| plugin.json / manifest.json | 元数据 | 否 | 否 | 清单 |
| ortho-mcp 连接器（依赖） | MCP stdio 服务 | 否 | 否 | 本地闭环，零依赖 |

## 去敏复核（发布前）

对全包执行去敏清单 grep（大小写不敏感）：

覆盖「个人真名 / 私人邮箱域名 / 私有机器路径 / 私有云目录 / 内网 VPN / API Key 形态」等类别（详见 connector-openplatform-publish 去敏清单）

**结果：0 命中。**

## 运行时安全（依赖的 ortho-mcp 连接器）

- `network: false` — 不发起任何网络请求
- `credentials: false` — 不读取任何凭据
- `dataEgress: false` — 无数据外发
- `readOnly: true` — 只读，不修改宿主环境
- 仅用 Node 标准库，零 npm 依赖；数据来自随包 `ortho-data.json`（用户可替换）

## 结论

本专家包及其依赖连接器均为本地、只读、零凭据、零外发，不含任何 PII 或私有凭据，
符合开放平台发布安全要求。
