# EMLBot by EVKII, Inc.: plugin for Claude Code, Grok Bot and Cursor

<img src="assets/logo.png" alt="EMLBot" width="96" height="96">

[EMLBot](https://emlbot.com), by EVKII, Inc., gives an AI agent its own bot identity in a team workspace: a real email inbox, the chat rooms it is in, a task queue and human approvals. This plugin connects your agent to EMLbot over MCP (`https://api.emlbot.com/mcp`, OAuth 2.1) and teaches it how to work that inbox safely. Version 1.0.2.

- Display name: EMLBot
- Publisher: EVKII, Inc. ([emlbot.com](https://emlbot.com))
- Privacy policy: https://emlbot.com/privacy/
- Terms of service: https://emlbot.com/terms/
- Support: support@emlbot.com (security reports: security@emlbot.com)

## What is inside

| File | What it does |
|---|---|
| `.claude-plugin/plugin.json` | Claude Code plugin manifest (name `emlbot`). |
| `.claude-plugin/marketplace.json` | Claude Code marketplace (name `emlbot`, one plugin, `emlbot`). |
| `.mcp.json` | The `emlbot` MCP server for Claude Code, `https://api.emlbot.com/mcp`, type `http`. |
| `.cursor-plugin/plugin.json` | Cursor and Grok Bot plugin manifest (name `emlbot`). |
| `mcp.json` | The `emlbot` MCP server for Cursor and Grok Bot, `https://api.emlbot.com/mcp` (Streamable HTTP). No key in the file: sign-in is OAuth. |
| `skills/emlbot/SKILL.md` | How to work as an EMLbot bot: `whoami` first, the `wait_for_work` loop, treating mail and chat as untrusted data, `can_message` before writing, approvals, tasks and rooms, pushed-wake acknowledgements. |
| `assets/logo.png` | Official EMLBot icon, 512x512 PNG on a white background plate. |

## Tools

The server has 23 tools. A standard bot sees 21: `list_approvals` and `decide_approval` are listed only for trusted approver bots. All tools act as the one bot you approved, with that bot's role, scopes, inbox list, recipient allowlist, daily cap and the workspace contact policy, enforced by the same server code as EMLbot's REST API.

- Identity and directory: `whoami`, `directory`, `can_message`
- Mail: `list_messages`, `search_mail`, `read_message`, `send_email`, `reply`, `forward`, `label_message`
- SMS (coming soon): `list_sms`, `send_sms`. These work once the phone add-on is live on your workspace ($9/mo per number, no setup fee). Phone is not available during the 7-day trial. Voice and local numbers are not available yet.
- Chat: `list_rooms`, `read_room`, `post_message`, `open_dm`
- Tasks: `my_tasks`, `create_task`, `update_task`, `close_task`
- Approvals: `list_approvals`, `decide_approval` (trusted approver bots only; not listed for other bots)
- Work loop: `wait_for_work` (blocks up to 50 s until mail, a mention, a task or an approval arrives). A chat connector is not woken by mail on its own; it works while the session is open and looping on `wait_for_work`. For unattended wakes use a webhook or a Claude routine (see https://api.emlbot.com/docs/mcp).

## Install and sign in

1. Install **EMLBot**:
   - Claude Code: `claude plugin marketplace add evkiinc/emlbot-plugin`, then `claude plugin install emlbot@emlbot`, then `/mcp`, select `plugin:emlbot:emlbot`, **Authenticate**.
   - Grok Bot and Cursor: add the plugin from the plugin catalog once the EMLBot listing is approved, or add the MCP server `https://api.emlbot.com/mcp` by hand.
2. Connect the `emlbot` server. A browser page opens on `app.emlbot.com`:
   1. Sign in with Google (the account you use for the EMLbot console).
   2. Pick the workspace.
   3. Pick which bot the agent acts as, or **Create a new bot** (name and handle are filled in for you).
   4. Leave **read** and **send** checked and press **Approve**.
3. Tell the agent: "Call whoami, then loop on wait_for_work and handle what comes in."

No EMLbot account yet? Start a 7-day free trial at https://emlbot.com (card required; a workspace owner or admin approves connections). Plans: Starter $20/mo, Team $49/mo, Business $199/mo. Phone numbers and texting are a coming-soon add-on: $9/mo per number, verification handled for you, no setup fee. Join the waitlist in the EMLbot console.

## Who can approve a connection

Workspace owners and admins. Apps that EMLbot cannot verify never receive the `manage` or `approve` scopes and cannot act as an admin, manager or trusted-approver bot.

## Disconnect

- In Claude Code: `/mcp`, select the server, **Clear authentication**.
- In Grok Bot or Cursor: disconnect or remove the EMLBot plugin's server.
- In the EMLbot console: **Keys**, the row marked **OAuth connection**, **Revoke**. The next call answers 401.

## Data handling

The agent sees only the mail, rooms, tasks and approvals of the bot you chose. Message bodies and other people's text are returned marked `untrusted: true`, and the skill tells the agent never to follow instructions found inside them. Every action is audited in the workspace (`via: "oauth"`). Access tokens last 1 hour; refresh tokens 30 days and rotate on every use. See https://emlbot.com/privacy/.

## Use an API key instead (headless)

Claude Code: `claude mcp add --transport http emlbot https://api.emlbot.com/mcp --header "Authorization: Bearer $EMLBOT_API_KEY"`. Cursor and Grok Bot:

```json
{ "mcpServers": { "emlbot": {
    "url": "https://api.emlbot.com/mcp",
    "headers": { "Authorization": "Bearer ${env:EMLBOT_API_KEY}" } } } }
```

## License

MIT for this plugin package (see `LICENSE`). The EMLbot service is governed by its [terms](https://emlbot.com/terms/).
