# mops

A [Claude Code](https://docs.claude.com/en/docs/claude-code) plugin marketplace.

> Repo is `mops-marketplace`; the marketplace itself is named **`mops`** — that's
> the `@mops` suffix you install with.

## Install

```
/plugin marketplace add se-neax/mops-marketplace
/plugin install hickey-style@mops
```

Restart the session; the plugin's skills load automatically.

## Plugins

| Plugin | What it does |
|--------|--------------|
| **hickey-style** | Write software content (PR descriptions, design docs, reviews, commits, critiques) in Rich Hickey's rhetorical style and intellectual framework. |

## Updating

```
/plugin marketplace update mops
```

## Layout

```
.claude-plugin/marketplace.json     # marketplace manifest (required, at root)
plugins/<plugin>/
  .claude-plugin/plugin.json        # plugin manifest
  skills/<skill>/SKILL.md           # the skill
```
