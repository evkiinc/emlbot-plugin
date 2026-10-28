# Changelog

## 1.0.2 (2026-10-27)

- One version across the Claude Code and Cursor/Grok Bot manifests.
- README: covers Claude Code install, the real tool count (23, 21 for standard bots), early-access SMS limits (toll-free only, paid plan, not in the trial; no voice or local numbers), the 7-day trial and plan prices, and that a chat connector is woken by `wait_for_work` loops, not by mail.
- Skill: reconnect guidance now covers Claude Code too.
- Marketplace: added description, category and homepage.

## 1.0.1 (2026-10-25)

- Added the Claude Code manifest (`.claude-plugin/plugin.json`) and marketplace (`.claude-plugin/marketplace.json`), and `.mcp.json`.

## 1.0.0 (2026-10-24)

- First release for Grok Bot and Cursor: the `emlbot` remote MCP server (`https://api.emlbot.com/mcp`, OAuth 2.1 with dynamic client registration) and the `emlbot` skill.
