<div align="center">

# check-for-me.skill

> *Ein zusaetzlicher Blick vor dem Senden kann dir Schuld ersparen, die du nie haettest tragen muessen.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

Du hast eine lange Erklaerung geschrieben und fuehlst dich vor dem Senden trotzdem nicht sicher?<br>
Du hast Angst, zu viel zu sagen und damit Hebel, Verantwortung und Spielraum gleichzeitig aus der Hand zu geben?<br>
Du befuerchtest, dass dein Ton zu weich ist und der anderen Seite zeigt, dass sie dich spaeter weiter druecken kann?<br>
Du hast Sorge, dass ein instabiler Satz gescreenshottet, falsch gelesen oder spaeter als Verantwortungsaussage gegen dich verwendet wird?<br>

<br>

**Bevor du es abschickst, lass das Skill pruefen, ob diese Nachricht wirklich sicher rausgehen sollte.**

[Unterstuetzte Datenquellen](#unterstuetzte-datenquellen) · [Installation](#installation) · [Optionale Tools](#optionale-tools) · [Verwendung](#verwendung) · [Beispiele](#beispiele) · [Standardausgabe](#standardausgabe) · [Projektstruktur](#projektstruktur) · [Hinweise](#hinweise) · [Detaillierte Installation](INSTALL.md) · [**中文**](README.md) · [**English**](README_EN.md) · [**Español**](README_ES.md) · [**日本語**](README_JA.md) · [**Русский**](README_RU.md) · [**Português**](README_PT.md)

</div>

---

## Was Es Ist

`check-for-me.skill` ist ein Risiko-Checker vor dem Senden.

Es schaut auf Fragen wie:

- gibt diese Nachricht zu viel Hebel aus der Hand
- uebernimmt dieser Satz zu viel Verantwortung
- wirken die Grenzen zu weich
- wird unbestaetigte Information zu frueh festgeschrieben

Die Ausgabe ist kein vages "der Ton klingt komisch". Sie versucht dir zu geben:

- den gefaehrlichsten Satz
- warum er gefaehrlich ist
- was gestrichen werden sollte
- was besser spaeter gesagt wird
- eine stabilere Neuformulierung

---

## Unterstuetzte Datenquellen

| Quelle | Chats | Docs / PDF | Bilder / Screenshots | Code / Kommentare | Hinweise |
|------|:-----:|:----------:|:--------------------:|:-----------------:|------|
| Entwurf, der gleich gesendet werden soll | Yes | No | No | No | Hauefigster Eingang |
| Chat-Kontext | Yes | No | Yes | No | Nuetzlich, um das uebliche Druckmuster der anderen Seite zu sehen |
| Docs / PDF / Markdown | No | Yes | No | Yes | Gut fuer Reports, Incident-Notizen und Handover-Erklaerungen |
| Technische Erklaerungen / Code-Kommentare | Yes | No | No | Yes | Gut, um zu pruefen, ob unnoetige Details offengelegt werden |
| Ziel des Nutzers | Yes | No | No | No | Zum Beispiel: "ich will die Schuld nicht ziehen" oder "ich will nicht zu weich klingen" |

---

## Installation

Siehe `INSTALL.md`.

Wenn du nur das Kernverhalten des Skills brauchst, reicht der aktuelle Repository-Inhalt bereits aus.
Wenn du zusaetzlich Feishu-, DingTalk-, Slack-, E-Mail- und Export-Parsing-Unterstuetzung willst, kannst du die optionalen Abhaengigkeiten installieren:

```bash
pip3 install -r requirements.txt
playwright install chromium
```

---

## Optionale Tools

Dieses Repository enthaelt ausserdem ein Verzeichnis `tools/`, das Rohmaterial in sauberere Eingaben fuer die Risikopruefung verwandelt.

- Feishu Auto-Sammlung: `tools/feishu_auto_collector.py`
- Feishu Export-Parsing: `tools/feishu_parser.py`
- Feishu Browser-Erfassung: `tools/feishu_browser.py`
- Feishu Zugriff ueber MCP: `tools/feishu_mcp_client.py`
- DingTalk Auto-Sammlung: `tools/dingtalk_auto_collector.py`
- Slack Auto-Sammlung: `tools/slack_auto_collector.py`
- E-Mail-Parsing: `tools/email_parser.py`
- Generisches Chat-Export-Parsing: `tools/generic_chat_parser.py`
- WeChat Export-Parsing: `tools/wechat_parser.py`
- QQ Export-Parsing: `tools/qq_parser.py`

Fuer mehr Details siehe `tools/README.md`. Hinweise zu Quellen und Lizenzen stehen in `docs/THIRD_PARTY.md`.

---

## Verwendung

Rufe es in einer Skill-kompatiblen Laufzeit auf:

```text
/check-for-me
```

Typischer Ablauf:

- fuege die exakte Nachricht ein, die du senden willst
- fuege dann etwas Kontext hinzu und wer der Empfaenger ist

---

## Beispiele

**Szenario 1: Nicht zu frueh die Schuld uebernehmen**

```text
Nutzer:
Ich bin kurz davor zu schreiben: "Dieses Problem wurde vor allem durch meine falsche Einschaetzung von gestern verursacht..."

check-for-me.skill:
Das groesste Problem an diesem Satz ist, dass du die volle Verantwortung uebernimmst, bevor die Schlussfolgerung stabil ist.
Sicherere Umformulierung:
Wir haben bestaetigt, dass das Problem mit dem Aenderungspfad von gestern zusammenhaengt. Ich schliesse gerade noch Umfang und Ausloeser sauber ab und teile danach die vollstaendige Einordnung samt Gegenmassnahmen.
```

**Szenario 2: Zu gefaellige Sprache vermeiden**

```text
Nutzer:
Ich will meinem Kollegen schreiben: "Kein Problem, ich mache das heute Nacht einfach."

check-for-me.skill:
Dieser Satz sendet zwei Signale:
1. so etwas kann in Zukunft wieder bei dir abgeladen werden
2. deine Zeitkosten muessen nicht beruecksichtigt werden
```

**Szenario 3: Technische Erklaerungen straffer machen**

```text
Nutzer:
Sagt diese Incident-Erklaerung zu viel?

check-for-me.skill:
Ja. Die aktuelle Version mischt unbestaetigte Vermutungen, internes Implementierungsdetail und Verantwortungsrahmung in einem Block.
Sicherer waere eine Trennung in bestaetigte Fakten, aktuelle Auswirkung, was jetzt bearbeitet wird und wann das naechste Update kommt.
```

---

## Standardausgabe

- Gesamturteil
- die 1 bis 3 gefaehrlichsten Saetze
- was bleiben kann
- was gestrichen oder umgeschrieben werden sollte
- eine sicherere Neuformulierung

---

## Projektstruktur

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

## Hinweise

- Es funktioniert schon mit einem einzelnen Entwurf, aber mit mehr Kontext wird das Risikourteil deutlich besser.
- Ziel ist es, unnoetige Offenlegung zu reduzieren, nicht Menschen dazu zu bringen, jede Verantwortung zu vermeiden.
- Es hilft nicht beim Luegen, beim Anschwaerzen anderer oder beim boeswilligen Abschieben von Schuld.
