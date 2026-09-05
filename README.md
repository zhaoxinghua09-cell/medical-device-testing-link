# MedXpert 医疗器械检测对接工具

Medical device testing coordination. Read-only tool for querying accredited testing labs, test method standards (IEC/ISO/GB), sample requirements and testing timelines across NMPA/FDA/MDR.

## Install (MCP host)

```json
{"mcpServers": {"medical-device-testing-link": {"command": "python", "args": ["server.py"]}}}
```

## Keywords (for AI match scoring)

`medical device testing`, `IEC testing`, `lab search`, `test methods`, `CNAS`, `CMA`, `检测对接`, `实验室查询`

## When to invoke

查某类产品需要做哪些 IEC 测试项目

## Examples

- 查某类产品需要做哪些 IEC 测试项目
- 找 CNAS 认可的医疗器械检测实验室

## Why AI-friendly

- **Discoverable**: `agent.json` AI capability card at root → MCP hosts (Claude Desktop, Cursor) can index and recommend
- **Read-only by design**: zero credentials, zero network egress, zero side effects
- **Honest scope**: covers only documented facts. Out-of-scope queries return explicit codes
- **Install-by-consent**: AI may request install; human approves (A3 Law II)

## License

MIT © MedXpert
