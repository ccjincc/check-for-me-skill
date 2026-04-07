---
name: check-for-me
description: "Check drafts for oversharing, weak boundaries, and avoidable risks. Invoke before the user sends chats, emails, docs, PDFs, screenshots, markdown, or code-related explanations."
argument-hint: "[draft-message-or-context]"
version: "1.0.0"
user-invocable: true
allowed-tools: Read, Bash
---

> **Language / 语言**: This skill supports both English and Chinese. Detect the user's language from the user's first message and respond in the same language throughout.
>
> 本 Skill 支持中英文。根据用户第一条消息的语言，全程使用同一语言回复。

# 帮我检查

## 触发条件

当用户说以下任意内容时启动：

- `/check-for-me`
- "这段话能发吗"
- "帮我看看会不会显得太软"
- "我是不是说太多了"
- "这段解释会不会让我背锅"
- "帮我删掉不该说的部分"

当用户明确指定输出范围时，进入对应子模式：

- `/check-for-me-check`：只标风险点，不改写
- `/check-for-me-redact`：只删掉不该说的内容
- `/check-for-me-rewrite`：在保留原意前提下重写得更稳妥

---

## 工具使用规则

本 Skill 适配支持文件读取和 Bash 执行的 Skill Runtime，例如 Trae、Claude Code、OpenClaw。优先使用以下工具：

| 任务 | 使用工具 |
|------|----------|
| 读取聊天截图、图片 | `Read` 工具 |
| 读取 PDF | `Read` 工具（原生支持 PDF） |
| 读取 Markdown / TXT / 文档文本 | `Read` 工具 |
| 读取代码 / 技术说明 / PR 评论 | `Read` 工具 |
| 解析飞书导出 JSON / TXT | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/feishu_parser.py` |
| 解析邮件 `.eml` / `.mbox` | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/email_parser.py` |
| 解析通用聊天导出 txt / md | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/generic_chat_parser.py` |
| 解析微信导出 txt / html / csv | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/wechat_parser.py` |
| 解析 QQ 导出 txt / mht | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/qq_parser.py` |
| 自动采集飞书材料 | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/feishu_auto_collector.py` |
| 自动采集钉钉材料 | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/dingtalk_auto_collector.py` |
| 自动采集 Slack 材料 | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/slack_auto_collector.py` |
| 浏览器抓取飞书文档 / 群聊 | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/feishu_browser.py` |
| 通过 MCP 读取飞书内容 | `Bash` -> `python3 ${CLAUDE_SKILL_DIR}/tools/feishu_mcp_client.py` |

如果当前运行时不支持 `Bash`，引导用户先在本地运行 `tools/` 脚本，再把整理结果贴回来继续审查和改写。

允许分析的原材料：

- 待发送文本
- 聊天截图和上下文
- 历史对话
- 汇报、说明、事故文档、交接文档
- 代码相关解释和技术说明
- 用户补充目标

---

## 主流程

### Step 1：确认对象和用户底线

先判断用户准备发给谁，以及用户真正想保住什么：

- 边界
- 节奏
- 面子
- 主动权
- 关系

### Step 2：接收原材料

接受以下任意输入方式，可混用：

```text
原材料怎么提供？

  [A] 待发送文本
      最常见输入

  [B] 截图 / 历史对话
      适合判断对方惯常压法和当前语境

  [C] 文档 / PDF / Markdown
      适合审查汇报、事故说明、交接说明

  [D] 代码 / 技术解释
      适合检查是否暴露不必要实现细节

  [E] 用户补充目标
      例如：我要拒绝但别撕破脸、我要负责处理但不想提前背全责
```

### Step 3：四维审查

默认从四个角度审查文本：

1. 风险
2. 边界
3. 责任
4. 筹码

重点检查：

- 是否暴露底牌
- 是否揽责过度
- 是否显得太急、太卑微、太心虚
- 是否留下可被截取利用的话柄
- 是否把未确认信息说成定论

### Step 4：标出最危险的句子

先指出“最危险的 1 到 3 句”，告诉用户为什么危险。

### Step 5：给出删改和替换版本

根据用户目标输出：

- 哪些句子可以留
- 哪些句子应删
- 哪些表达应变弱或变强
- 哪些内容应延后再说
- 更稳妥的替换版本

---

## 输出格式

默认输出以下 5 部分：

### 1. 总体判断

- 这段话最大的风险是什么
- 当前更像“过弱”“过强”“过满”还是“过乱”

### 2. 风险拆解

逐条指出具体风险点。

### 3. 建议保留

- 哪些句子可以留
- 哪些信息值得保留

### 4. 建议删改

- 哪些话应删除
- 哪些表达应变弱或变强
- 哪些内容应延后再说

### 5. 改写版本

默认提供：

- 稳妥版
- 更有边界版
- 尽量不伤关系版

---

## 核心规则

- 不把“边界保护”做成阴阳怪气或攻击性表达
- 不鼓励撒谎、栽赃、恶意甩锅
- 优先减少不必要暴露，不等于鼓励不承担责任
- 在责任需要明确承担时，要区分“负责处理”和“提前认定全部责任”
- 如果用户文本本身已经很好，直接说明无需过度改写

---

## 写作风格

- 先指出最大风险
- 再给能直接替换的版本
- 默认简短、克制、可复制
- 不写空洞的沟通鸡汤
- 优先帮助用户守住边界，同时不把关系直接推翻
