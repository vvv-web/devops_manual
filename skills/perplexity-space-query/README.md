# Perplexity Space Query — скилл для Cursor

Агент **формирует промпты** для Perplexity. Пользователь копирует промпт, вставляет в Perplexity, получает ответ и вставляет ответ обратно в чат агенту. Запросы из Cursor, есть связь с Linux. При проект-запросах — обязательный блок «Вставка для Linux».

---

## Описание

Скилл даёт агенту в Cursor инструкции, как:

1. Формировать промпты для Perplexity с контекстом и требованием офф. док.
2. Выдавать блок «📝 Скопируйте и вставьте в Perplexity».
3. При запросах с командами/конфигами — выдавать блок «🖥️ Вставка для Linux».
4. Обрабатывать ответ Perplexity (сравнение, сводка, шаги — по контексту).

---

## Когда использовать

- «промпт для Perplexity», «сформулируй запрос Perplexity», «что спросить в Perplexity»
- «/perplexity-query», «perplexity space»
- Пользователь просит подготовить вопрос для Perplexity

---

## Активация

### 1. Клонировать репозиторий

```bash
git clone https://github.com/vvv-web/devops_manual.git
cd devops_manual
```

(или `git pull`, если репо уже склонирован)

### 2. Скопировать скилл в папку Cursor

**Личный скилл** (все проекты):

```bash
mkdir -p ~/.cursor/skills
cp -r skills/perplexity-space-query ~/.cursor/skills/
```

**Проектный скилл** (только текущий репо):

```bash
mkdir -p .cursor/skills
cp -r skills/perplexity-space-query .cursor/skills/
```

### 3. Проверить структуру

```
~/.cursor/skills/perplexity-space-query/
├── SKILL.md
└── README.md
```

### 4. Включить скилл в Cursor

- Открой Cursor.
- В Agent chat введи `/` или выбери скилл из списка.
- Подключи `perplexity-space-query`.
- Скилл будет использоваться при запросах про промпты для Perplexity.

### 5. Требования

- Perplexity открыт у пользователя вручную.
- Никаких MCP browser не требуется.
- Cursor + Linux (WSL/ssh или локальный терминал).

---

## Workflow

1. Агент пишет промпт (с контекстом и требованием офф. док)
2. Агент выдаёт блок «📝 Скопируйте и вставьте в Perplexity»
3. Пользователь: Ctrl+C → Perplexity → Ctrl+V → Enter
4. Пользователь копирует ответ Perplexity и вставляет в чат
5. Агент обрабатывает ответ (сравнение, сводка, шаги — по контексту)

---

## Лицензия

Используй свободно в своих проектах.
