---
name: cursor-cloud-agent-pr-playbook
description: Готовый playbook для автономной работы через Cursor Cloud Agent без BugBot. Используй при настройке GitHub App, PR review через `@cursor`, GitHub Actions, обязательных mock-тестах, permissions, secrets и Linux-командах для копирования.
---

# Cursor Cloud Agent PR Playbook

Используй этот скилл, когда пользователь хочет:

- запускать автономный workflow на своем GitHub;
- использовать `Cursor Cloud Agent`, а не `BugBot`;
- ревьюить и чинить PR через комментарии `@cursor`;
- сделать обязательные `mock-tests` в CI;
- получить готовые блоки для Linux: команды, YAML, комментарии для PR.

## Базовый принцип

Целевой workflow:

1. Пользователь создает ветку и PR.
2. GitHub Actions запускает `mock-tests`.
3. Пользователь пишет комментарий `@cursor ...` в PR.
4. Cloud Agent читает PR, смотрит CI, вносит исправления и пушит коммиты.
5. При новых падениях CI агенту дают повторную команду или заранее просят следить за CI.

Важно:

- Это **не BugBot**.
- Используется **Cursor GitHub integration + Cloud Agent**.
- Основной CI должен быть быстрым и детерминированным: `mock/unit tests only`.

## Что проверять сначала

Перед рекомендациями проверь и явно уточни:

1. GitHub или GitHub Enterprise?
2. Есть ли платный/trial план Cursor с usage-based pricing?
3. Нужен только GitHub Actions или еще self-hosted runners?
4. Какой стек: Node, Python, Go, mixed?
5. Как в проекте отделяются `mock/unit` и `integration/e2e` тесты?

Если ответов нет, предложи безопасный дефолт:

- GitHub.com
- GitHub Actions
- отдельный job `mock-tests`
- интеграционные тесты не запускаются в PR по умолчанию

## Как отвечать

По умолчанию давай ответ в таком порядке:

1. Коротко: можно ли сделать через Cloud Agent без BugBot.
2. Что нужно настроить:
   - GitHub App / интеграция
   - permissions
   - workflow mock-tests
   - prompt/comment для `@cursor`
3. Блоки для копирования:
   - `🖥️ Вставка для Linux`
   - `📝 Комментарий в PR`
   - `📄 GitHub Actions workflow`
4. Риски и ограничения.

## Обязательные правила

- Всегда предпочитай **Cloud Agent** вместо BugBot, если пользователь хочет избежать отдельной оплаты за BugBot.
- Всегда рекомендуй **mock-tests** как обязательный быстрый PR-gate.
- Не предлагай real DB/API calls в основном PR pipeline, если пользователь явно не просил.
- Для project-зависимых задач обязательно давай готовые вставки для Linux.
- Если речь о секрете, напоминай: хранить в GitHub Secrets / Cursor Secrets, не в репозитории.

## Минимальный playbook

### 1. GitHub интеграция

Объясняй так:

- Подключить GitHub account / GitHub App для Cursor.
- Проверить доступ к нужному репозиторию.
- Убедиться, что Cloud Agent может читать и пушить ветки.

### 2. Mock-tests в CI

Рекомендуй отдельный job с явным названием:

- `mock-tests`
- `unit-tests`

И явным правилом:

- только mock/unit;
- без внешней сети;
- без реальных зависимостей;
- быстрое завершение.

### 3. Комментарий в PR

Используй такой шаблон по умолчанию:

```
@cursor Review this PR, check the failing mock tests and fix the code. Push commits to the PR branch. Do not add integration or e2e coverage to this CI path. Keep the PR pipeline limited to mock/unit tests only.
```

Если пользователь хочет цикл, предлагай усиленный вариант:

```
@cursor Review this PR, fix issues you find, push commits, monitor CI, and fix failing mock/unit checks until the PR is green. Do not rely on real external services. Use mocks/stubs/fakes where needed.
```

## Шаблоны, которые нужно выдавать

### 🖥️ Вставка для Linux

Когда пользователь просит настройку, всегда по возможности формируй блок:

```
🖥️ Вставка для Linux
---
[команды для терминала]
---
Скопируй блок выше и вставь в терминал Linux.
```

### 📄 GitHub Actions workflow

Базовый шаблон:

```yaml
name: Mock Tests

on:
  pull_request:
    branches: [main]

jobs:
  mock-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup runtime
        run: echo "replace with project setup"
      - name: Run mock tests
        run: echo "replace with unit/mock test command"
```

Адаптируй под стек:

- Node: `npm ci` + `npm test -- --runInBand`
- Python: `pip install -r requirements.txt` + `pytest tests/unit -q`
- Go: `go test ./...`

Только если пользователь подтвердил, что эти команды реально соответствуют проекту.

### 📝 Комментарий в PR

```
📝 Комментарий в PR
---
@cursor Review this PR, fix failing mock/unit tests, push commits, and keep the CI path limited to mocks only. Do not use real external services.
---
Скопируй комментарий и вставь его в PR.
```

## Permissions и безопасность

Всегда отдельно проговаривай:

- нужен доступ на чтение и запись в репозиторий;
- нужны права на PR/checks/statuses, если агент должен видеть CI;
- секреты держать в GitHub Secrets / Cursor Secrets;
- least privilege: не просить больше прав, чем нужно;
- не хранить токены в YAML, shell history или repo.

## Ограничения, которые нужно честно говорить

- Cloud Agent не равен BugBot по продуктовой модели.
- Полностью бесконечный loop "сам себя ревьюит вечно" не гарантирован.
- CI autofix зависит от сценария и настроек workspace/team.
- Если тесты не отделены на `mock/unit` и `integration`, сначала нужно навести порядок в тестовой стратегии.

## Когда этот скилл особенно уместен

- "хочу без BugBot"
- "хочу автономно гонять PR"
- "как настроить @cursor для PR"
- "сделай CI только на mock tests"
- "нужен workflow для GitHub Actions + Cloud Agent"

## Когда не использовать

- Вопросы только про Perplexity
- Общие вопросы по Git без Cloud Agent
- Сложные multi-platform сценарии, где нужен отдельный playbook под GitLab

## Формат финального ответа

Если пользователь просит практическую настройку, используй такой каркас:

1. `📍 Что делаем`
2. `📄 Что нужно настроить`
3. `🖥️ Вставка для Linux`
4. `📄 GitHub Actions workflow`
5. `📝 Комментарий в PR`
6. `✅ Что считать готовым`
