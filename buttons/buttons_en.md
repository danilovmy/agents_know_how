# Buttons for unambiguous choices in Telegram

Below is a prompt fragment added to OpenClaw agents.

````md
## Telegram choices and confirmations

For Telegram actions requiring confirmation, send `message(action=send)` with exactly:

```json
{"blocks":[{"type":"buttons","buttons":[{"label":"Подтвердить","value":"confirm:<action_id>"},{"label":"Отмена","value":"cancel:<action_id>"}]}]}
```

For English use labels `confirm`/`cancel`. Keep values scoped and under 64 bytes. Do not act before the matching callback. Cancel does nothing. If buttons fail, report blocked; never substitute text-only confirmation.

For short exhaustive neutral choices, use `choice:<question_id>:<option_id>`. Do not button open-ended questions. Use block type `buttons`, top-level `value`, real JSON objects, and no unregistered callback actions. The callback acknowledgement plugin may remove buttons and mark the selection, but the agent must still process the delivered callback. Do not duplicate a visible button prompt in final.
````

## Purpose of the rule

Buttons are used only when the options are short, explicit, mutually exclusive, and exhaustive: “yes/no,” “save/skip,” or “before/after lunch.”

Confirmation of a dangerous or state-changing action is encoded as `confirm:<action_id>` / `cancel:<action_id>`. A neutral choice is encoded as `choice:<question_id>:<option_id>`. The callback value must identify the question and option unambiguously and fit within Telegram’s 64-byte limit.

Buttons do not replace open-ended questions: if there are more reasonable answers than the displayed options, the agent must ask in plain text.

## Neutral choice example

```json
{
  "presentation": {
    "blocks": [
      {
        "type": "buttons",
        "buttons": [
          {"label": "До обеда", "value": "choice:meeting_time:before_lunch"},
          {"label": "После обеда", "value": "choice:meeting_time:after_lunch"}
        ]
      }
    ]
  }
}
```

After receiving the callback, the agent executes only the selected branch. The interface may immediately remove the buttons and mark the selection—the agent does not need to duplicate this in a separate message.
