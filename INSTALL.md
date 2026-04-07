# 安装说明

## Claude Code

Claude Code 从 git 仓库根目录的 `.claude/skills/` 查找 skill。

```bash
mkdir -p .claude/skills
 git clone https://github.com/ccjincc/check-for-me-skill.git .claude/skills/check-for-me
```

## OpenClaw

```bash
 git clone https://github.com/ccjincc/check-for-me-skill.git ~/.openclaw/workspace/skills/check-for-me
```

## Trae

将仓库根目录的全部内容复制到项目根目录的 `.trae/skills/check-for-me/` 下即可。

## 可选工具依赖

如果你想启用 `tools/` 目录下的自动采集、导出解析和浏览器抓取脚本，可额外安装：

```bash
pip3 install -r requirements.txt
playwright install chromium
```

这些依赖不是基础 Skill 运行必需项；只想分析截图、PDF、Markdown、代码评论时可以不装。

更多脚本说明见 `tools/README.md`。

## 使用

```text
/check-for-me
```
