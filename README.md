# MCP PostgreSQL для Cursor

Простой MCP сервер для подключения Cursor к PostgreSQL. Работает на чистом bash + psql.

## Быстрый старт

1. **Настройка:**
```bash
cp env.example .env
# Отредактируйте .env с вашими данными PostgreSQL
```

2. **Добавьте в Cursor:**
```json
{
  "mcpServers": {
    "postgres": {
      "command": "/Users/dog/Sites/mcp-pg/run_mcp.sh"
    }
  }
}
```
путь должен быть абсолютными

## Использование


Теперь можете работать с любой базой данных, просто указав её название в запросе.

**Примеры команд в Cursor:**
- "какая самая большая таблица в БД o3?"
- "какие БД есть на текущем соединении?"
- "покажи владельцев таблиц в db1"
- "выполни в базе insales: SELECT count(*) FROM users"
- "переключись на БД metabase и покажи таблицы"

Cursor автоматически поймёт из вашего запроса, с какой базой работать!

## Конфигурация

Каждое подключение настраивается отдельно с собственными правами доступа:

```env
# Локальное подключение (разработка)
DB_LOCAL_HOST=localhost
DB_LOCAL_PORT=5432
DB_LOCAL_NAME=postgres
DB_LOCAL_USER=postgres
DB_LOCAL_PASSWORD=password
DB_LOCAL_READONLY=false  # полный доступ для разработки

# Docker подключение (тестирование)
DB_DOCKER_HOST=0.0.0.0
DB_DOCKER_PORT=15432
DB_DOCKER_NAME=insales_dev
DB_DOCKER_USER=postgres
DB_DOCKER_PASSWORD=password
DB_DOCKER_READONLY=true  # только чтение для безопасности

# Продакшен подключение (боевой сервер)
DB_PROD_HOST=your-prod-server.com
DB_PROD_PORT=5432
DB_PROD_NAME=production_db
DB_PROD_USER=postgres
DB_PROD_PASSWORD=your-password
DB_PROD_READONLY=true  # только чтение для безопасности
```

## Возможности

- **Выполнение SQL запросов** (с настраиваемой безопасностью)
- **READONLY режим** для безопасного доступа к продакшену
- **Проверка подключений** к разным серверам
- **Переключение между базами данных** без перезапуска
- **Множественные подключения** (local, docker, prod)

## Устранение проблем

Если в Cursor статус "client closed":

1. **Проверьте работу вручную:**
```bash
echo '{"method":"tools/list","id":1}' | ./mcp_postgres
```

2. **Проверьте, что psql установлен:**
```bash
which psql
```

3. **Проверьте пути в настройках Cursor** - они должны быть абсолютными

4. **Смотрите лог** - View / Output - MCP output

5. **Перезапустите Cursor** после изменения конфигурации

6. **После изменения .env файла** обязательно перезапустите Cursor - иначе LLM будет видеть устаревшие режимы безопасности в описаниях MCP инструментов

Никаких зависимостей, только bash + psql!
