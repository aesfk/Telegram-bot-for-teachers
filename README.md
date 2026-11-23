# Telegram Practice Bot for Teachers and Students

A Python Telegram bot for topic-based practice. Teachers can enter problems, and students can solve them in chat, get immediate feedback, and submit their working for review. The bot uses pyTelegramBotAPI and MySQL.

## Practice workflow

- Use `/start` or `/help` to select the student or teacher workflow.
- Students choose a topic and use `/run <topic>` to start practice. The bot checks text answers and updates a priority score for each problem type within the session. Send `stop` to return to topic selection.
- Authorised teachers use `/document` to add a problem and `/solutions` to request student submissions. The prototype can store an optional image with a task.

## Data model

| Table | Purpose |
| --- | --- |
| `tasks` | Topics, problem types, prompts, answer strings, and optional attachments |
| `pupils` | Student records and fields for practice scores by topic and problem type |
| `solves` | Written solutions submitted for teacher review |

## Local development

The prototype is contained in [main.py](main.py). For a local demo, create a fresh Telegram test bot and a disposable MySQL database named `train_brain`. Update the script to read configuration from the environment, for example `TELEGRAM_BOT_TOKEN`, `TEACHER_IDS`, `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD`, and `MYSQL_DATABASE`. Export your local settings before starting the bot. The script attempts to create its tables at startup.

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install pyTelegramBotAPI mysql-connector-python
python main.py
```

Start with synthetic student accounts and text-only practice tasks. Score changes are held in memory during practice. The current `/solutions` handler drops the submissions table and attempts to recreate it after review, so use a disposable database for that workflow.
