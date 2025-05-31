# MCP PostgreSQL для Cursor

Простой MCP сервер для подключения Cursor к PostgreSQL. Работает на чистом bash + psql.

## Быстрый старт

1. **Настройка:**
```bash
cp env.example .env
# Отредактируйте .env с вашими данными PostgreSQL
```

2. **Добавьте в Cursor:**
Скопируйте содержимое `mcp_postgres.json` в настройки MCP серверов Cursor.

## Использование

После настройки можете писать в Cursor:

- "Выполни на local: SELECT * FROM users LIMIT 5"
- "Проверь соединение с local базой"
- "Покажи таблицы: SELECT tablename FROM pg_tables WHERE schemaname='public'"

## Конфигурация

В `.env` файле настройте подключения:

```env
# Для каждой базы добавьте блок вида:
DB_ИМЯ_HOST=адрес
DB_ИМЯ_PORT=порт  
DB_ИМЯ_NAME=база
DB_ИМЯ_USER=пользователь
DB_ИМЯ_PASSWORD=пароль
```

Поддерживается любое количество подключений.

## Команды

- `execute_sql` - выполнить SQL запрос (только SELECT)
- `check_connection` - проверить подключение

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

3. **Проверьте пути в mcp_postgres.json** - они должны быть абсолютными

4. **Перезапустите Cursor** после изменения конфигурации

Никаких зависимостей, только bash + psql!
