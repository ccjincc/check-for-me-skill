# Tools

这些脚本用于把原始材料整理成更适合 Skill 分析的输入，不是基础运行必需项。

## 可用工具

- `feishu_auto_collector.py`: 自动采集飞书群聊、私聊、文档、Wiki、多维表格
- `feishu_parser.py`: 解析飞书导出的 JSON 或 TXT 消息记录
- `feishu_browser.py`: 复用本机浏览器登录态抓取飞书文档、Wiki、表格、群聊
- `feishu_mcp_client.py`: 通过 Feishu MCP 读取飞书文档和消息
- `dingtalk_auto_collector.py`: 采集钉钉用户相关文档、多维表格和消息材料
- `slack_auto_collector.py`: 拉取 Slack 频道、群 DM、私聊中的目标消息
- `email_parser.py`: 解析 `.eml`、`.mbox`、纯文本邮件记录

## 安装依赖

```bash
pip3 install -r requirements.txt
playwright install chromium
```

## 使用建议

- 如果你的运行时支持 `Bash`，可以直接在 Skill 内调用这些脚本。
- 如果运行时不支持 `Bash`，先在本地跑脚本，再把输出结果贴给 Skill 也可以。
- 自动采集类脚本通常需要你自己配置平台凭证或本机登录态。
