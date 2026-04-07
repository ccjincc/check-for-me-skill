<div align="center">

# check-for-me.skill

> *Una revision extra antes de enviar puede evitarte cargar culpas que nunca debiste asumir.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Python 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://python.org)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-green)](https://agentskills.io)

<br>

Escribiste una explicacion larga y aun asi no te sientes seguro antes de enviarla?<br>
Te preocupa que estes diciendo demasiado y regalando margen, responsabilidad y espacio de maniobra al mismo tiempo?<br>
Te preocupa que el tono sea demasiado blando y le ensene a la otra parte que puede seguir presionandote despues?<br>
Te preocupa que una frase inestable acabe en una captura, se malinterprete o se reutilice como una declaracion de responsabilidad?<br>

<br>

**Antes de enviarlo, deja que el Skill revise si ese mensaje realmente es seguro para salir.**

[Fuentes Compatibles](#fuentes-compatibles) · [Instalacion](#instalacion) · [Herramientas Opcionales](#herramientas-opcionales) · [Uso](#uso) · [Ejemplos](#ejemplos) · [Salida por Defecto](#salida-por-defecto) · [Estructura del Proyecto](#estructura-del-proyecto) · [Notas](#notas) · [Instalacion Detallada](INSTALL.md) · [**中文**](README.md) · [**English**](README_EN.md) · [**Deutsch**](README_DE.md) · [**日本語**](README_JA.md) · [**Русский**](README_RU.md) · [**Português**](README_PT.md)

</div>

---

## Que Es

`check-for-me.skill` es un verificador de riesgo previo al envio.

Se centra en preguntas como:

- este mensaje entrega demasiada ventaja
- esta frase asume demasiada responsabilidad
- los limites se ven demasiado debiles
- se esta fijando informacion no confirmada demasiado pronto

Su salida no es un vago "el tono suena raro". Intenta darte:

- la frase mas peligrosa
- por que es peligrosa
- que deberia borrarse
- que conviene posponer
- una reescritura mas estable

---

## Fuentes Compatibles

| Fuente | Chats | Docs / PDF | Imagenes / Capturas | Codigo / Comentarios | Notas |
|------|:-----:|:----------:|:-------------------:|:--------------------:|------|
| Borrador a punto de enviarse | Yes | No | No | No | La entrada mas comun |
| Contexto del chat | Yes | No | Yes | No | Util para ver el patron de presion de la otra parte |
| Docs / PDF / Markdown | No | Yes | No | Yes | Bueno para reportes, postmortems y explicaciones de traspaso |
| Explicaciones tecnicas / comentarios de codigo | Yes | No | No | Yes | Util para revisar si expones detalles innecesarios |
| Objetivo aportado por el usuario | Yes | No | No | No | Por ejemplo: "no quiero cargar con la culpa" o "no quiero sonar demasiado blando" |

---

## Instalacion

Consulta `INSTALL.md`.

Si solo necesitas el comportamiento base del Skill, el contenido actual del repositorio ya es suficiente.
Si tambien quieres soporte para Feishu, DingTalk, Slack, email y parsing de exportaciones, puedes instalar las dependencias opcionales:

```bash
pip3 install -r requirements.txt
playwright install chromium
```

---

## Herramientas Opcionales

Este repositorio tambien incluye un directorio `tools/` para convertir material bruto en entradas mas limpias antes de ejecutar la revision de riesgo.

- Recoleccion automatica de Feishu: `tools/feishu_auto_collector.py`
- Parsing de exportaciones de Feishu: `tools/feishu_parser.py`
- Captura de Feishu via navegador: `tools/feishu_browser.py`
- Acceso a Feishu por MCP: `tools/feishu_mcp_client.py`
- Recoleccion automatica de DingTalk: `tools/dingtalk_auto_collector.py`
- Recoleccion automatica de Slack: `tools/slack_auto_collector.py`
- Parsing de email: `tools/email_parser.py`
- Parsing generico de exportaciones de chat: `tools/generic_chat_parser.py`
- Parsing de exportaciones de WeChat: `tools/wechat_parser.py`
- Parsing de exportaciones de QQ: `tools/qq_parser.py`

Para mas detalle, consulta `tools/README.md`. Para fuentes de terceros y licencias, consulta `docs/THIRD_PARTY.md`.

---

## Uso

Lanzalo dentro de cualquier runtime compatible con Skills:

```text
/check-for-me
```

El flujo mas comun es:

- pegar exactamente el mensaje que estas a punto de enviar
- luego anadir un poco de contexto y quien es el destinatario

---

## Ejemplos

**Escenario 1: Evitar asumir culpa demasiado pronto**

```text
Usuario:
Estoy a punto de responder: "Este problema fue causado sobre todo por mi mal juicio de ayer..."

check-for-me.skill:
El mayor problema de esa frase es que estas asumiendo toda la responsabilidad antes de que la conclusion este estable.
Reescritura mas segura:
Hemos confirmado que el problema esta conectado con la ruta de cambios de ayer. Todavia estoy cerrando alcance y disparador, y en breve comparto la conclusion completa y el plan de mitigacion.
```

**Escenario 2: Evitar un tono demasiado complaciente**

```text
Usuario:
Quiero decirle a mi companero: "No pasa nada, yo lo saco esta noche."

check-for-me.skill:
Esa frase envia dos senales:
1. esto puede volver a caerte encima en el futuro
2. tu costo de tiempo no necesita ser considerado
```

**Escenario 3: Apretar explicaciones tecnicas**

```text
Usuario:
Esta explicacion del incidente esta diciendo demasiado?

check-for-me.skill:
Si. La version actual mezcla hipotesis no confirmadas, detalle interno de implementacion y reparto de responsabilidad en el mismo bloque.
Una separacion mas segura seria: hechos confirmados, impacto actual, lo que se esta gestionando ahora y cuando llegara la siguiente actualizacion.
```

---

## Salida por Defecto

- juicio general
- las 1 a 3 frases mas peligrosas
- que conviene mantener
- que conviene borrar o reescribir
- una version reescrita mas segura

---

## Estructura del Proyecto

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

- Puede funcionar con un solo borrador, pero con mas contexto el juicio de riesgo mejora mucho.
- El objetivo es reducir exposicion innecesaria, no animar a la gente a evitar toda responsabilidad.
- No ayuda a mentir, incriminar a otra persona ni descargar culpa de forma maliciosa.
