# timem-dsh-mcp

一键向 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 追加 **TiMEM 主平台** 的 MCP 桥接配置。

等价于其他平台（Cursor / Claude Code）的 `mcpServers` JSON：

```json
{
  "mcpServers": {
    "TiMEM-MCP": {
      "url": "https://api.timem.cloud/mcp",
      "headers": {
        "X-API-Key": "YOUR_API_KEY",
        "X-TiMEM-User-Id": "usr_xxxx"
      }
    }
  }
}
```

本工具把这个配置以 `@deepseek-ai/dsh-mcp-client` 插件实例的形式，写入 DSH profile 的 `cordis.patch.yml`。重启 DSH host 后，工具以 `mcp__timem__*` 命名注册（如 `mcp__timem__search_memories`）。

## 快速开始

```bash
# 自动查找 cordis.patch.yml + 交互式输入 API Key 与用户 ID（粘贴不回显，用户 ID 可留空）
npx --yes github:aimaoge/timem-dsh-mcp

# 指定配置文件 / 免交互（脚本、CI 用）
npx --yes github:aimaoge/timem-dsh-mcp --file /path/to/cordis.patch.yml --key sk-xxxx --user-id usr_xxx -y

# 只预览不落盘
npx --yes github:aimaoge/timem-dsh-mcp --dry-run --key sk-xxxx
```

> 本机 HTTPS 拉取若报 `UNABLE_TO_VERIFY_LEAF_SIGNATURE`（TLS 被中间人拦截），改用 SSH 通道：
> `npx --yes git+ssh://git@github.com/aimaoge/timem-dsh-mcp.git`

## 选项

| 选项 | 说明 |
|---|---|
| `--file <path>` | 指定 `cordis.patch.yml`（跳过自动查找） |
| `--key <key>` | 直接提供 API Key（跳过交互输入） |
| `--user-id <id>` | TiMEM 用户 ID（`X-TiMEM-User-Id` 头；省略时交互输入，可留空） |
| `--url <url>` | MCP 端点，默认 `https://api.timem.cloud/mcp` |
| `--server-name <name>` | 工具命名空间，默认 `timem` |
| `--dry-run` | 只打印将要写入的内容，不修改文件 |
| `-y, --yes` | 跳过确认 |
| `-h, --help` | 帮助 |

## 本地开发

```bash
node dsh-add-timem-mcp.mjs [选项]
```

- 自动查找范围：`$DSH_HOME` → `~/.dsh` → npm npx 缓存目录；多个结果时优先 web profile
- 幂等：已存在条目时原地更新 key，不产生重复
- 跨平台：Windows 7/10/11、Linux、macOS，纯 Node 内置模块零依赖
- 保持原文件换行符（CRLF/LF）与 UTF-8 编码

## License

MIT