<div align="center">

# check-for-me.skill

> *One extra look before sending can save you from carrying avoidable blame.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

Wrote a long explanation and still feel uneasy before sending?<br>
Worried the tone is too soft or reveals too much?<br>

**Review a draft before sending and point out what is risky, weak, or oversharing.**

[Supported Sources](#supported-data-sources)  [Install](#install)  [Optional Tools](#optional-tools)  [Usage](#usage)  [Examples](#examples)  [Detailed Install](INSTALL.md)  [**中文**](README.md)  [**Español**](README_ES.md)  [**Deutsch**](README_DE.md)  [**日本語**](README_JA.md)  [**Русский**](README_RU.md)  [**Português**](README_PT.md)

</div>

---

## What It Is

`check-for-me.skill` is a pre-send risk reviewer. It flags dangerous lines, explains the risk, and rewrites them into safer alternatives.

## Supported Data Sources

- Draft messages and follow-up context
- Screenshots and message history
- Reports, handover docs, Markdown, and PDF
- Technical explanations, PR comments, and code notes

## Install

See `INSTALL.md`.

```bash
pip3 install -r requirements.txt
playwright install chromium
```

## Usage

```text
/check-for-me
```
