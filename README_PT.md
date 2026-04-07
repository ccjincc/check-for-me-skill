<div align="center">

# check-for-me.skill

> *Um olhar a mais antes de enviar costuma evitar culpa que voce nem precisava carregar.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

Voce escreveu uma explicacao longa e ainda nao se sente seguro antes de enviar?<br>
Tem receio de estar falando demais e entregando margem, responsabilidade e espaco de manobra ao mesmo tempo?<br>
Tem medo de que o tom esteja macio demais e ensine a outra parte a continuar apertando voce depois?<br>
Tem receio de que uma frase mal posicionada vire print, seja lida de forma errada ou reapareca como enquadramento de responsabilidade?<br>

<br>

**Antes de enviar, deixe o Skill verificar se essa mensagem realmente esta segura para sair.**

[Fontes Compativeis](#fontes-compativeis) · [Instalacao](#instalacao) · [Ferramentas Opcionais](#ferramentas-opcionais) · [Uso](#uso) · [Exemplos](#exemplos) · [Saida Padrao](#saida-padrao) · [Estrutura do Projeto](#estrutura-do-projeto) · [Notas](#notas) · [Instalacao Detalhada](INSTALL.md) · [**中文**](README.md) · [**English**](README_EN.md) · [**Español**](README_ES.md) · [**Deutsch**](README_DE.md) · [**日本語**](README_JA.md) · [**Русский**](README_RU.md)

</div>

---

## O Que E

`check-for-me.skill` e um verificador de risco antes do envio.

Ele olha para perguntas como:

- esta mensagem entrega margem demais
- esta frase assume responsabilidade demais
- os limites estao aparecendo fracos demais
- informacao nao confirmada esta sendo fixada cedo demais

A saida nao e um vago "o tom parece estranho". O objetivo e entregar:

- a frase mais perigosa
- por que ela e perigosa
- o que deveria ser apagado
- o que convem adiar
- uma reescrita mais segura

---

## Fontes Compativeis

| Fonte | Chats | Docs / PDF | Imagens / Capturas | Codigo / Comentarios | Notas |
|------|:-----:|:----------:|:------------------:|:--------------------:|------|
| Rascunho que vai ser enviado | Yes | No | No | No | Entrada mais comum |
| Contexto do chat | Yes | No | Yes | No | Ajuda a ver o padrao de pressao da outra parte |
| Docs / PDF / Markdown | No | Yes | No | Yes | Bom para relatorios, postmortems e explicacoes de handoff |
| Explicacoes tecnicas / comentarios de codigo | Yes | No | No | Yes | Bom para checar se voce expoe detalhe desnecessario |
| Objetivo informado pelo usuario | Yes | No | No | No | Por exemplo: "nao quero puxar a culpa" ou "nao quero soar mole demais" |

---

## Instalacao

Veja `INSTALL.md`.

Se voce so precisa do comportamento basico do Skill, o conteudo atual do repositorio ja basta.
Se tambem quiser suporte para Feishu, DingTalk, Slack, email e parsing de exportacoes, voce pode instalar as dependencias opcionais:

```bash
pip3 install -r requirements.txt
playwright install chromium
```

---

## Ferramentas Opcionais

Este repositorio tambem inclui um diretorio `tools/` para transformar material bruto em entrada mais limpa antes da revisao de risco.

- Coleta automatica de Feishu: `tools/feishu_auto_collector.py`
- Parsing de exportacoes do Feishu: `tools/feishu_parser.py`
- Captura de Feishu via navegador: `tools/feishu_browser.py`
- Acesso ao Feishu por MCP: `tools/feishu_mcp_client.py`
- Coleta automatica de DingTalk: `tools/dingtalk_auto_collector.py`
- Coleta automatica de Slack: `tools/slack_auto_collector.py`
- Parsing de email: `tools/email_parser.py`
- Parsing generico de exportacoes de chat: `tools/generic_chat_parser.py`
- Parsing de exportacoes do WeChat: `tools/wechat_parser.py`
- Parsing de exportacoes do QQ: `tools/qq_parser.py`

Para mais detalhe, veja `tools/README.md`. Fontes de terceiros e avisos de licenca estao em `docs/THIRD_PARTY.md`.

---

## Uso

Chame dentro de um runtime compativel com Skills:

```text
/check-for-me
```

O fluxo mais comum e:

- colar exatamente a mensagem que esta prestes a enviar
- depois adicionar um pouco de contexto e quem vai receber

---

## Exemplos

**Cenario 1: Evitar assumir culpa cedo demais**

```text
Usuario:
Estou prestes a responder: "Esse problema foi causado principalmente pelo meu erro de julgamento ontem..."

check-for-me.skill:
O maior problema nessa frase e que voce esta puxando a responsabilidade inteira antes de a conclusao estar estabilizada.
Reescrita mais segura:
Ja confirmamos que o problema esta ligado ao caminho de alteracao de ontem. Ainda estou fechando escopo e gatilho, e em seguida compartilho a conclusao completa com o plano de mitigacao.
```

**Cenario 2: Evitar linguagem complacente demais**

```text
Usuario:
Quero falar para meu colega: "Tranquilo, eu resolvo isso esta noite."

check-for-me.skill:
Essa frase envia dois sinais:
1. isso pode continuar sendo empurrado para voce no futuro
2. o custo do seu tempo nao precisa ser considerado
```

**Cenario 3: Apertar explicacoes tecnicas**

```text
Usuario:
Essa explicacao de incidente esta falando demais?

check-for-me.skill:
Sim. A versao atual mistura hipotese nao confirmada, detalhe interno de implementacao e enquadramento de responsabilidade no mesmo bloco.
Mais seguro seria separar em fatos confirmados, impacto atual, o que esta sendo tratado agora e quando sai a proxima atualizacao.
```

---

## Saida Padrao

- leitura geral
- as 1 a 3 frases mais perigosas
- o que pode ficar
- o que convem apagar ou reescrever
- uma versao reescrita mais segura

---

## Estrutura do Projeto

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

## Notas

- Funciona com um unico rascunho, mas com mais contexto o julgamento de risco fica bem melhor.
- O objetivo e reduzir exposicao desnecessaria, nao ajudar alguem a escapar de toda responsabilidade.
- O Skill nao ajuda a mentir, incriminar outra pessoa nem empurrar culpa com ma-fe.
