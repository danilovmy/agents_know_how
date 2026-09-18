# Schaltflächen für eindeutige Entscheidungen in Telegram

Nachfolgend steht ein Prompt-Fragment, das OpenClaw-Agenten hinzugefügt wird.

````md
## Telegram choices and confirmations

For Telegram actions requiring confirmation, send `message(action=send)` with exactly:

```json
{"blocks":[{"type":"buttons","buttons":[{"label":"Bestätigen","value":"confirm:<action_id>"},{"label":"Abbrechen","value":"cancel:<action_id>"}]}]}
```

For English use labels `confirm`/`cancel`. Keep values scoped and under 64 bytes. Do not act before the matching callback. Cancel does nothing. If buttons fail, report blocked; never substitute text-only confirmation.

For short exhaustive neutral choices, use `choice:<question_id>:<option_id>`. Do not button open-ended questions. Use block type `buttons`, top-level `value`, real JSON objects, and no unregistered callback actions. The callback acknowledgement plugin may remove buttons and mark the selection, but the agent must still process the delivered callback. Do not duplicate a visible button prompt in final.
````

## Zweck der Regel

Schaltflächen werden nur verwendet, wenn die Optionen kurz, eindeutig, gegenseitig ausschließend und vollständig sind: „Ja/Nein“, „Speichern/Überspringen“ oder „vor/nach dem Mittagessen“.

Die Bestätigung einer gefährlichen oder zustandsändernden Aktion wird als `confirm:<action_id>` / `cancel:<action_id>` codiert. Eine neutrale Auswahl wird als `choice:<question_id>:<option_id>` codiert. Der Callback-Wert muss die Frage und die Option eindeutig bezeichnen und in das Telegram-Limit von 64 Byte passen.

Offene Fragen werden nicht durch Schaltflächen ersetzt: Wenn es mehr sinnvolle Antworten als angezeigte Optionen gibt, muss der Agent die Frage als normalen Text stellen.

## Beispiel für eine neutrale Auswahl

```json
{
  "presentation": {
    "blocks": [
      {
        "type": "buttons",
        "buttons": [
          {"label": "Vor dem Mittagessen", "value": "choice:meeting_time:before_lunch"},
          {"label": "Nach dem Mittagessen", "value": "choice:meeting_time:after_lunch"}
        ]
      }
    ]
  }
}
```

Nach dem Empfang des Callbacks führt der Agent nur den ausgewählten Zweig aus. Die Oberfläche kann die Schaltflächen sofort entfernen und die Auswahl markieren; der Agent muss dies nicht in einer separaten Nachricht wiederholen.
