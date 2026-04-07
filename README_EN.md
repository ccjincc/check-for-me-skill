<div align="center">

# check-for-me.skill

> *One extra look before sending can save you from carrying blame you never needed to take on.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

Wrote a long explanation and still do not feel safe sending it?<br>
Worried you are saying too much and giving away your leverage, responsibility, and room to maneuver all at once?<br>
Worried your tone is too soft and teaches people they can keep pressing you later?<br>
Worried one unstable sentence will be screenshotted, misread, or reused as a responsibility statement against you?<br>

<br>

**Before you send it, let the Skill check whether this message is actually safe to send.**

[Supported Sources](#supported-data-sources) · [Install](#install) · [Optional Tools](#optional-tools) · [Usage](#usage) · [Examples](#examples) · [Default Output](#default-output) · [Project Structure](#project-structure) · [Notes](#notes) · [Detailed Install](INSTALL.md) · [**中文**](README.md) · [**Español**](README_ES.md) · [**Deutsch**](README_DE.md) · [**日本語**](README_JA.md) · [**Русский**](README_RU.md) · [**Português**](README_PT.md)

</div>

---

## What It Is

`check-for-me.skill` is a pre-send risk checker.

It focuses on questions like:

- does this message expose too much leverage
- does it take on too much responsibility
- does it show boundaries that are too weak
- does it lock in unconfirmed information too early

Its output is not a generic "the tone feels off." It tries to give you:

- the most dangerous sentence
- why it is dangerous
- what should be deleted
- what should be delayed
- a steadier replacement version

---

## Supported Data Sources

| Source | Chat Logs | Docs / PDF | Images / Screenshots | Code / Comments | Notes |
|------|:---------:|:----------:|:--------------------:|:---------------:|------|
| Draft text to be sent | ✅ | — | — | — | Most common input |
| Chat context | ✅ | — | ✅ | — | Useful for judging the other person's usual pressure pattern |
| Docs / PDF / Markdown | — | ✅ | — | ✅ | Good for status reports, incident notes, and handoff explanations |
| Technical explanations / code comments | ✅ | — | — | ✅ | Useful for checking whether you expose unnecessary detail |
| User-added goal | ✅ | — | — | — | For example: "I do not want to take the blame" or "I do not want to sound too soft" |

---

## Install

See `INSTALL.md`.

If you only need the core Skill behavior, the current repository contents are already enough.
If you also want Feishu, DingTalk, Slack, email, and export parsing support, you can install the optional dependencies:

```bash
pip3 install -r requirements.txt
playwright install chromium
```

---

## Optional Tools

This repository also includes a `tools/` directory for turning raw material into cleaner input before running risk review.

- Feishu auto collection: `tools/feishu_auto_collector.py`
- Feishu export parsing: `tools/feishu_parser.py`
- Feishu browser capture: `tools/feishu_browser.py`
- Feishu MCP access: `tools/feishu_mcp_client.py`
- DingTalk auto collection: `tools/dingtalk_auto_collector.py`
- Slack auto collection: `tools/slack_auto_collector.py`
- Email parsing: `tools/email_parser.py`
- Generic chat export parsing: `tools/generic_chat_parser.py`
- WeChat export parsing: `tools/wechat_parser.py`
- QQ export parsing: `tools/qq_parser.py`

For more detail, see `tools/README.md`. For third-party sources and license notices, see `docs/THIRD_PARTY.md`.

---

## Usage

Call it inside any Skill-compatible runtime:

```text
/check-for-me
```

Most common input style:

- paste the exact message you are about to send
- then add a bit of context and who the recipient is

---

## Examples

**Scenario 1: Avoid Taking the Blame Too Early**

```text
User                ❯ I am about to reply with: "This problem was mainly caused by my wrong judgment yesterday..."

check-for-me.skill  ❯ The biggest problem in that sentence is that you are taking full responsibility before the conclusion is even stable.
                    Safer rewrite:
                    We have confirmed that the issue is connected to yesterday's change path. I am still completing the scope and trigger analysis, and I will share the full conclusion and mitigation plan shortly.
```

**Scenario 2: Prevent Over-Pleasing**

```text
User                ❯ I want to tell my coworker: "No worries, I will just take care of it tonight."

check-for-me.skill  ❯ That sentence sends two signals:
                    1. this can keep being pushed onto you in the future
                    2. your time cost does not need to be considered
```

**Scenario 3: Tightening Technical Explanations**

```text
User                ❯ Is this incident explanation saying too much?

check-for-me.skill  ❯ Yes. The current version mixes unconfirmed guesses, internal implementation detail, and responsibility framing in one place.
                    A safer split would be: confirmed facts, current impact, what is being handled now, and when the next update will be sent.
```

---

## Default Output

- overall judgment
- the 1 to 3 most dangerous sentences
- what should be kept
- what should be deleted or rewritten
- a safer rewritten version

---

## Project Structure

```text
check-for-me/
├── SKILL.md
├── README.md
├── README_EN.md
├── README_ES.md
├── README_DE.md
├── README_JA.md
├── README_RU.md
├── README_PT.md
├── INSTALL.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── docs/
│   ├── PRD.md
│   ├── FAQ.md
│   └── THIRD_PARTY.md
├── prompts/
│   ├── intake.md
│   ├── risk_classifier.md
│   ├── redactor.md
│   └── rewriter.md
├── tools/
│   ├── README.md
│   ├── feishu_auto_collector.py
│   ├── feishu_parser.py
│   ├── feishu_browser.py
│   ├── feishu_mcp_client.py
│   ├── dingtalk_auto_collector.py
│   ├── slack_auto_collector.py
│   ├── email_parser.py
│   ├── generic_chat_parser.py
│   ├── wechat_parser.py
│   └── qq_parser.py
└── examples/
    ├── blame-risk-example.md
    ├── oversharing-example.md
    └── weak-boundary-example.md
```

---

## Notes

- It works from a single draft, but adding context makes the risk judgment much stronger.
- The point is to reduce unnecessary exposure, not to encourage people to avoid all responsibility.
- It does not help you lie, frame someone else, or maliciously dump blame.
