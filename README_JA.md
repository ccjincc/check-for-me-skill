<div align="center">

# check-for-me.skill

> *送る前にもう一度見るだけで、背負わなくてよい責任を避けられることが多い。*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

長い説明を書いたのに、送る前にまだ不安がある？<br>
言いすぎて、立場や責任や余地をまとめて差し出してしまっていないか心配？<br>
語気が弱すぎて、相手に「今後も押せる」と学習させてしまわないか不安？<br>
一文の置き方を誤って、スクショ・誤読・責任口径化の材料にならないか怖い？<br>

<br>

**送る前に、その文章が本当に送ってよい状態かを Skill に見てもらう。**

[対応データ](#対応データ) · [インストール](#インストール) · [任意ツール](#任意ツール) · [使い方](#使い方) · [例](#例) · [標準出力](#標準出力) · [プロジェクト構成](#プロジェクト構成) · [注意](#注意) · [詳細インストール](INSTALL.md) · [**中文**](README.md) · [**English**](README_EN.md) · [**Español**](README_ES.md) · [**Deutsch**](README_DE.md) · [**Русский**](README_RU.md) · [**Português**](README_PT.md)

</div>

---

## これは何か

`check-for-me.skill` は送信前リスク確認 Skill です。

主に見るのは次のような点です。

- この文は余計な手札を渡していないか
- この一文で責任を抱え過ぎていないか
- 境界線が弱く見えていないか
- 未確定情報を早過ぎる形で固定していないか

出力は「なんとなくトーンが変」という曖昧な感想ではありません。目指すのは次のような整理です。

- 最も危ない文
- なぜ危ないのか
- どこを削るべきか
- 何を今は言わない方がよいか
- より安定した書き換え案

---

## 対応データ

| ソース | チャット | Docs / PDF | 画像 / スクリーンショット | コード / コメント | 備考 |
|------|:--------:|:----------:|:-------------------------:|:-----------------:|------|
| 送信予定の下書き | Yes | No | No | No | 最も一般的な入力 |
| チャットの文脈 | Yes | No | Yes | No | 相手の圧のかけ方の癖を見るのに有効 |
| Docs / PDF / Markdown | No | Yes | No | Yes | 報告書、障害説明、引き継ぎ文に向く |
| 技術説明 / コードコメント | Yes | No | No | Yes | 不要な技術詳細を出し過ぎていないか見やすい |
| ユーザーの目的説明 | Yes | No | No | No | 例: 「責任を引きたくない」「弱く見えたくない」 |

---

## インストール

`INSTALL.md` を参照してください。

基本的な Skill の動作だけなら、現在のリポジトリ内容で十分です。
Feishu、DingTalk、Slack、メール、各種エクスポート解析まで使いたい場合は、任意依存を追加できます。

```bash
pip3 install -r requirements.txt
playwright install chromium
```

---

## 任意ツール

このリポジトリには、元データをリスク確認しやすい形へ整える `tools/` ディレクトリも含まれています。

- Feishu 自動収集: `tools/feishu_auto_collector.py`
- Feishu エクスポート解析: `tools/feishu_parser.py`
- Feishu ブラウザ取得: `tools/feishu_browser.py`
- Feishu MCP 読み取り: `tools/feishu_mcp_client.py`
- DingTalk 自動収集: `tools/dingtalk_auto_collector.py`
- Slack 自動収集: `tools/slack_auto_collector.py`
- メール解析: `tools/email_parser.py`
- 汎用チャットエクスポート解析: `tools/generic_chat_parser.py`
- WeChat エクスポート解析: `tools/wechat_parser.py`
- QQ エクスポート解析: `tools/qq_parser.py`

詳しくは `tools/README.md`、第三者由来とライセンス表記は `docs/THIRD_PARTY.md` を参照してください。

---

## 使い方

Skill 対応ランタイムで次のように呼び出します。

```text
/check-for-me
```

よくある流れは次の通りです。

- まず送る予定の文面をそのまま貼る
- 次に相手と文脈を少し補う

---

## 例

**シナリオ 1: 早過ぎる全面責任を避ける**

```text
ユーザー:
「この問題は昨日の私の判断ミスが主な原因でした……」と返信しようとしています。

check-for-me.skill:
一番危ないのは、結論がまだ固まっていない段階で全面的な責任を引いていることです。
より安全な書き換え:
問題が昨日の変更経路と関係していることは確認できています。現在、影響範囲と発火条件を詰めており、結論と対処方針を整理してすぐ共有します。
```

**シナリオ 2: 過度に迎合する言い方を防ぐ**

```text
ユーザー:
同僚に「大丈夫です、今夜こちらでやっておきます」と送りたいです。

check-for-me.skill:
この文は二つのシグナルを出します:
1. 今後も同じ押し込み方が通る
2. あなたの時間コストは考慮不要である
```

**シナリオ 3: 技術説明を締める**

```text
ユーザー:
この障害説明、言い過ぎでしょうか。

check-for-me.skill:
はい。現状は未確定推測、内部実装詳細、責任の言い方が一つに混ざっています。
より安全なのは、確認済み事実、現在の影響、今やっている対応、次の更新時刻に分ける形です。
```

---

## 標準出力

- 全体判断
- 1 から 3 個の危ない文
- 残してよい部分
- 削るべき部分 / 書き換えるべき部分
- より安全な書き換え案

---

## プロジェクト構成

```text
check-for-me/
|- SKILL.md
|- README.md
|- README_EN.md
|- README_ES.md
|- README_DE.md
|- README_JA.md
|- README_RU.md
|- README_PT.md
|- INSTALL.md
|- LICENSE
|- .gitignore
|- requirements.txt
|- docs/
|  |- PRD.md
|  |- FAQ.md
|  `- THIRD_PARTY.md
|- prompts/
|  |- intake.md
|  |- risk_classifier.md
|  |- redactor.md
|  `- rewriter.md
|- tools/
|  |- README.md
|  |- feishu_auto_collector.py
|  |- feishu_parser.py
|  |- feishu_browser.py
|  |- feishu_mcp_client.py
|  |- dingtalk_auto_collector.py
|  |- slack_auto_collector.py
|  |- email_parser.py
|  |- generic_chat_parser.py
|  |- wechat_parser.py
|  `- qq_parser.py
`- examples/
   |- blame-risk-example.md
   |- oversharing-example.md
   `- weak-boundary-example.md
```

---

## 注意

- 下書き一つでも使えますが、文脈があるほどリスク判断はかなり強くなります。
- 目的は余計な露出を減らすことであって、本来の責任回避を助けることではありません。
- 嘘をつくこと、他人をはめること、悪意ある責任転嫁を助けることはしません。
