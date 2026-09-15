# Кнопки для однозначного выбора в Telegram

Ниже — фрагмент промпта, который добавляется агентам OpenClaw.

````md
## Telegram choices and confirmations

For Telegram actions requiring confirmation, send `message(action=send)` with exactly:

```json
{"blocks":[{"type":"buttons","buttons":[{"label":"Подтвердить","value":"confirm:<action_id>"},{"label":"Отмена","value":"cancel:<action_id>"}]}]}
```

For English use labels `confirm`/`cancel`. Keep values scoped and under 64 bytes. Do not act before the matching callback. Cancel does nothing. If buttons fail, report blocked; never substitute text-only confirmation.

For short exhaustive neutral choices, use `choice:<question_id>:<option_id>`. Do not button open-ended questions. Use block type `buttons`, top-level `value`, real JSON objects, and no unregistered callback actions. The callback acknowledgement plugin may remove buttons and mark the selection, but the agent must still process the delivered callback. Do not duplicate a visible button prompt in final.
````

## Смысл правила

Кнопки применяются только там, где варианты короткие, явные, взаимоисключающие и исчерпывающие: «да/нет», «сохранить/пропустить», «до/после обеда».

Подтверждение опасного или изменяющего состояния действия кодируется как `confirm:<action_id>` / `cancel:<action_id>`. Нейтральный выбор — как `choice:<question_id>:<option_id>`. Значение callback должно однозначно указывать на вопрос и вариант и помещаться в лимит Telegram 64 байта.

Открытые вопросы кнопками не заменяются: если разумных ответов больше, чем показано вариантов, агент должен спросить обычным текстом.

## Пример нейтрального выбора

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

После получения callback агент выполняет только выбранную ветку. Интерфейс может сразу убрать кнопки и отметить выбор — агенту не нужно дублировать это отдельным сообщением.
