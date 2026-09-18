# Кнопки для коротких ответов в Codex

## Проблема и доступная возможность

Когда Codex задаёт простой вопрос вроде «Да или нет?» или «Применить или отменить?», вводить ответ вручную неудобно. Codex может показывать варианты ответа, на которые можно нажать, через запрос пользовательского ввода. Мы проверили это в настольном приложении с вариантами **Yes / No** и **Ja / Nein**: выбранный ответ возвращается в диалог.

Варианты появляются как отдельный элемент интерфейса, а не как кнопки под обычным сообщением, как в Telegram. Их отображение зависит от клиента и доступности инструмента для запроса ответа. См. официальную [документацию Codex App Server о `tool/requestUserInput`](https://learn.chatgpt.com/docs/app-server).

## Инструкция и место её добавления

Следующая английская инструкция добавлена в значение ключа `developer_instructions` на верхнем уровне **пользовательского** файла конфигурации `~/.codex/config.toml` (в Windows — `%USERPROFILE%\.codex\config.toml`).

```text
developer_instructions = """
....
When you need to ask the user a question that can be answered with two or three short, clear options, always use the available user-input tool to show clickable choices instead of requiring a typed reply. Use labels in the language of the conversation (for example, Yes/No, Apply/Don't apply, or Proceed/Cancel). If the interface has no choice tool, ask in plain text. Do not ask for a choice when you can safely proceed without one.
"""
```

Это пользовательская настройка по умолчанию для задач Codex во всех проектах. Настройки проекта или параметры командной строки с более высоким приоритетом могут её переопределить. На обычные чаты ChatGPT в браузере эта инструкция не распространяется. О расположении файлов и приоритете настроек см. [основы конфигурации Codex](https://learn.chatgpt.com/docs/config-file/config-basic), а о ключе `developer_instructions` — [справочник по конфигурации](https://learn.chatgpt.com/docs/config-file/config-reference).
