# Short answer buttons in Codex

## The problem and the available feature

When Codex asks a simple question such as “Yes or no?” or “Apply or cancel?”, typing a reply is unnecessary work. Codex can show clickable choices through a user-input request. We tested this in the desktop app with **Yes / No** and **Ja / Nein**; the selected answer was returned to the conversation.

The choices appear as a separate interface prompt, not as a Telegram-style keyboard attached to an ordinary message. Whether they can be shown depends on the client and the available user-input tool. See the official [Codex App Server documentation for `tool/requestUserInput`](https://learn.chatgpt.com/docs/app-server).

## The instruction added to the configuration

I added the following exact English text inside the top-level `developer_instructions` value in the **user-level** configuration file, `~/.codex/config.toml` (`%USERPROFILE%\.codex\config.toml` on Windows).

```text
developer_instructions = """
....
When you need to ask the user a question that can be answered with two or three short, clear options, always use the available user-input tool to show clickable choices instead of requiring a typed reply. Use labels in the language of the conversation (for example, Yes/No, Apply/Don't apply, or Proceed/Cancel). If the interface has no choice tool, ask in plain text. Do not ask for a choice when you can safely proceed without one.
"""
```

This is a user-level default for Codex sessions across projects. A higher-priority project or command-line configuration can override user-level settings. The instruction does not configure ordinary ChatGPT web chats. See the official [Codex config basics](https://learn.chatgpt.com/docs/config-file/config-basic) for configuration locations and precedence, and the [configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) for `developer_instructions`.
