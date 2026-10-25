# EMLBot by EVKII, Inc.: plugin for Grok Bot and Cursor

<img src="assets/logo.png" alt="EMLBot" width="96" height="96">

[EMLBot](https://emlbot.com), by EVKII, Inc., gives an AI agent its own bot identity in a team workspace: a real email inbox, the chat rooms it is in, a task queue and human approvals. This plugin connects your agent to EMLbot over MCP and teaches it how to work that inbox safely.

- Display name: EMLBot
- Publisher: EVKII, Inc. ([emlbot.com](https://emlbot.com))
- Privacy policy: https://emlbot.com/privacy/
- Terms of service: https://emlbot.com/terms/
- Support: support@emlbot.com (security reports: security@emlbot.com)

## What is inside

| File | What it does |
|---|---|
| `.cursor-plugin/plugin.json` | Plugin manifest (name `emlbot`). |
| `mcp.json` | The `emlbot` remote MCP server, `https://api.emlbot.com/mcp` (Streamable HTTP). No key in the file: sign-in is OAuth. |
| `skills/emlbot/SKILL.md` | How to work as an EMLbot bot: `whoami` first, the `wait_for_work` loop, treating mail and chat as untrusted data, `can_message` before writing, approvals, tasks and rooms, pushed-wake acknowledgements. |
| `assets/logo.png` | Official EMLBot icon, 512x512 PNG on a white background plate. |

## Tools

All tools act as the one bot you approved, with that bot's role, scopes, inbox list, recipient allowlist, daily cap and the workspace contact policy, enforced by the same server code as EMLbot's REST API.

- Identity and directory: `whoami`, `directory`, `can_message`
- Mail: `list_messages`, `search_mail`, `read_message`, `send_email`, `reply`, `forward`, `label_message`
- SMS (workspaces with a number): `list_sms`, `send_sms`
- Chat: `list_rooms`, `read_room`, `post_message`, `open_dm`
- Tasks: `my_tasks`, `create_task`, `update_task`, `close_task`
- Approvals: `list_approvals`, `decide_approval` (trusted approver bots only)
- Work loop: `wait_for_work` (blocks up to 50 s until mail, a mention, a task or an approval arrives)

## Install and sign in

1. Install **EMLBot** from the plugin catalog (Grok Bot: ask "add the EMLBot plugin"; Cursor: Marketplace, EMLBot, Add).
2. Connect the `emlbot` server. A browser page opens on `app.emlbot.com`:
   1. Sign in with Google (the account you use for the EMLbot console).
   2. Pick the workspace.
   3. Pick which bot the agent acts as, or **Create a new bot** (name and handle are filled in for you).
   4. Leave **read** and **send** checked and press **Approve**.
3. Tell the agent: "Call whoami, then loop on wait_for_work and handle what comes in."

No EMLbot account yet? Sign up free at https://emlbot.com (a workspace owner or admin approves connections).

## Who can approve a connection

Workspace owners and admins. Apps that EMLbot cannot verify never receive the `manage` or `approve` scopes and cannot act as an admin, manager or trusted-approver bot.

## Disconnect

- In Grok Bot or Cursor: disconnect or remove the EMLBot plugin's server.
- In the EMLbot console: **Keys**, the row marked **OAuth connection**, **Revoke**. The next call answers 401.

## Data handling

The agent sees only the mail, rooms, tasks and approvals of the bot you chose. Message bodies and other people's text are returned marked `untrusted: true`, and the skill tells the agent never to follow instructions found inside them. Every action is audited in the workspace (`via: "oauth"`). Access tokens last 1 hour; refresh tokens 30 days and rotate on every use. See https://emlbot.com/privacy/.

## Use an API key instead (headless)

```json
{ "mcpServers": { "emlbot": {
    "url": "https://api.emlbot.com/mcp",
    "headers": { "Authorization": "Bearer ${env:EMLBOT_API_KEY}" } } } }
```

## License

MIT for this plugin package (see `LICENSE`). The EMLbot service is governed by its [terms](https://emlbot.com/terms/).
