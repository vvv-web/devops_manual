# Cursor Cloud Agent PR Playbook — скилл для Cursor

Готовый playbook для автономной работы через **Cursor Cloud Agent** на GitHub без платного BugBot. Позволяет настраивать PR review и fix через комментарии `@cursor`, GitHub Actions и обязательные mock-тесты.

---

## Описание

Скилл даёт агенту в Cursor инструкции, как:

1. Помогать с настройкой GitHub App и Cursor Cloud Agent.
2. Собирать workflow для PR с обязательными mock/unit-тестами в CI.
3. Подготавливать шаблоны комментариев для PR (`@cursor ...`).
4. Выдавать блоки «Вставка для Linux» для команд и конфигов.
5. Учитывать permissions, secrets и безопасность.
6. Соблюдать правило: **без BugBot**, только Cloud Agent.

---

## Когда использовать

- «Хочу автономно гонять PR без BugBot»
- «Настроить @cursor для PR»
- «CI только на mock tests»
- «Workflow GitHub Actions + Cloud Agent»
- «Получить готовые шаблоны комментариев и YAML»

---

## Активация на корпоративном ПК

### 1. Клонировать репозиторий

```bash
git clone https://github.com/<ваш-org>/devops_manual.git
cd devops_manual
```

(или `git pull`, если репо уже склонирован)

### 2. Скопировать скилл в папку Cursor

**Личный скилл** (все проекты):

```bash
mkdir -p ~/.cursor/skills
cp -r skills/cursor-cloud-agent-pr-playbook ~/.cursor/skills/
```

**Проектный скилл** (только текущий репо):

```bash
mkdir -p .cursor/skills
cp -r skills/cursor-cloud-agent-pr-playbook .cursor/skills/
```

### 3. Проверить структуру

```
~/.cursor/skills/cursor-cloud-agent-pr-playbook/
├── SKILL.md
└── README.md
```

### 4. Включить скилл в Cursor

- Открой Cursor.
- В Agent chat введи `/` или выбери скилл из списка.
- Подключи `cursor-cloud-agent-pr-playbook`.
- Скилл будет использоваться при запросах про Cloud Agent, PR, mock-tests и т.п.

### 5. Убедиться в требованиях

- Cursor с trial или paid plan (usage-based pricing для Cloud Agent).
- Подключённый GitHub-аккаунт в Cursor.
- Cursor GitHub App установлен и настроен для репозиториев.
- Права на чтение/запись для Cloud Agent.

---

## Краткий workflow

1. Создать ветку и PR на GitHub.
2. Добавить workflow `.github/workflows/mock-tests.yml` (шаблон в SKILL.md).
3. В PR написать комментарий:

   ```
   @cursor Review this PR, fix failing mock/unit tests, push commits, and keep the CI path limited to mocks only. Do not use real external services.
   ```

4. Cloud Agent проанализирует PR, внесёт правки, запушит коммиты.
5. При необходимости повторить комментарий или уточнить инструкции.

---

## Ссылки

- [Cursor Cloud Agent](https://cursor.com/docs/cloud-agent)
- [Cursor GitHub integration](https://cursor.com/docs/integrations/github)
- [Cloud Agent Capabilities (CI fixing)](https://cursor.com/docs/cloud-agent/capabilities)

---

## Лицензия

Используй свободно в своих проектах.
