# Auswahlschaltflächen für kurze Fragen in Codex

## Das Problem und die vorhandene Möglichkeit

Bei einer einfachen Rückfrage wie „Ja oder Nein?“ oder „Anwenden oder Abbrechen?“ ist eine getippte Antwort unnötig umständlich. Codex kann über eine Anfrage zur Nutzereingabe anklickbare Antwortmöglichkeiten anzeigen. Wir haben das in der Desktop-App mit **Yes / No** und **Ja / Nein** getestet; die gewählte Antwort wurde an die Unterhaltung zurückgegeben.

Die Auswahl erscheint als eigenes Element der Benutzeroberfläche, nicht als Telegram-Tastatur unter einer gewöhnlichen Nachricht. Ob sie angezeigt werden kann, hängt vom Client und vom verfügbaren Werkzeug für Nutzereingaben ab. Siehe die offizielle [Codex App Server-Dokumentation zu `tool/requestUserInput`](https://learn.chatgpt.com/docs/app-server).

## Die eingefügte Anweisung und ihr Speicherort

Der folgende englische Text wurde unverändert in den Wert `developer_instructions` auf oberster Ebene der **benutzerweiten** Konfigurationsdatei `~/.codex/config.toml` (unter Windows `%USERPROFILE%\.codex\config.toml`) eingefügt.

```text
developer_instructions = """
....
When you need to ask the user a question that can be answered with two or three short, clear options, always use the available user-input tool to show clickable choices instead of requiring a typed reply. Use labels in the language of the conversation (for example, Yes/No, Apply/Don't apply, or Proceed/Cancel). If the interface has no choice tool, ask in plain text. Do not ask for a choice when you can safely proceed without one.
```

Dies ist eine benutzerweite Voreinstellung für Codex-Sitzungen in allen Projekten. Eine Projekt- oder Befehlszeilenkonfiguration mit höherer Priorität kann sie überschreiben. Gewöhnliche ChatGPT-Chats im Web werden dadurch nicht konfiguriert. Die offiziellen [Grundlagen der Codex-Konfiguration](https://learn.chatgpt.com/docs/config-file/config-basic) erklären Speicherorte und Priorität; die [Konfigurationsreferenz](https://learn.chatgpt.com/docs/config-file/config-reference) beschreibt `developer_instructions`.
