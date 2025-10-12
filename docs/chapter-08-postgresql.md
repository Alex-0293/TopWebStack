[← Оглавление курса](index.md)

---

# Глава 8. PostgreSQL: надежное хранение данных

## 8.1. PostgreSQL 18: новые возможности

**PostgreSQL** — мощная open-source объектно-реляционная СУБД с 35+ летней историей.

**Ключевые характеристики:**
- **ACID** — полная транзакционность
- **Расширяемость** — custom types, functions, operators
- **JSON/JSONB** — NoSQL возможности
- **Full-text search** — встроенный поиск
- **Репликация** — streaming, logical replication

**Где применяется:**
- Enterprise приложения
- Финансовые системы
- Геопространственные данные (PostGIS)
- Time-series данные (TimescaleDB)
- Аналитика и BI

**Почему в этом курсе:**
- Надежность и стабильность
- Богатая функциональность
- Отличная производительность
- Активное сообщество
- Долгосрочная поддержка

**Распространенность:**
- #1 СУБД по версии Stack Overflow
- Используется в 40%+ production систем
- Поддержка всех cloud providers

**PostgreSQL 18 — новые возможности:**
- Улучшенная производительность запросов
- Оптимизированная репликация
- Новые JSON функции
- Улучшенный vacuum
- Лучшая поддержка параллелизма

**Актуальная версия:** PostgreSQL 18.0  
**Поддержка до:** Ноябрь 2029

**Ссылки:**
- Официальный сайт: https://www.postgresql.org/
- GitHub: https://github.com/postgres/postgres
- Документация: https://www.postgresql.org/docs/18/
- Wiki: https://wiki.postgresql.org/

---

## 8.2. Развертывание PostgreSQL в контейнере

### Простой запуск

```bash
# Запуск PostgreSQL 18
podman run -d \
  --name postgres \
  -e POSTGRES_PASSWORD=secret123 \
  -e POSTGRES_USER=developer \
  -e POSTGRES_DB=myapp \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:18-alpine

# Проверка логов
podman logs postgres

# Подключение к БД
podman exec -it postgres psql -U developer -d myapp
```

### Production-ready конфигурация

**Документация:** https://www.postgresql.org/docs/18/runtime-config.html

```bash
# Создание директории для конфигурации
mkdir -p ~/postgres-config

# Создание custom postgresql.conf
nano ~/postgres-config/postgresql.conf
```

Содержимое:

```conf
# Connection Settings
# listen_addresses: IP адреса для прослушивания (* = все интерфейсы)
# Опции: 'localhost', '*', '192.168.1.1', или список через запятую
listen_addresses = '*'

# max_connections: Максимальное количество одновременных подключений
# Рекомендуется: 100-200 для небольших систем, 300-500 для средних
max_connections = 100

# superuser_reserved_connections: Резерв подключений для суперпользователя
# Рекомендуется: 3-5 для административных задач
superuser_reserved_connections = 3

# Memory Settings
# shared_buffers: Память для кэширования данных (25% от RAM)
# Рекомендуется: 256MB-8GB в зависимости от доступной памяти
shared_buffers = 256MB

# effective_cache_size: Оценка памяти для кэша ОС (50-75% от RAM)
# Используется планировщиком запросов для оптимизации
effective_cache_size = 1GB

# maintenance_work_mem: Память для операций обслуживания (VACUUM, CREATE INDEX)
# Рекомендуется: 64MB-1GB
maintenance_work_mem = 64MB

# work_mem: Память для сортировки и хэш-таблиц на одну операцию
# Рекомендуется: 4MB-64MB (осторожно: умножается на количество операций)
work_mem = 4MB

# WAL (Write-Ahead Logging) Settings
# wal_buffers: Буфер для WAL записей (-1 = auto, обычно 1/32 от shared_buffers)
# Рекомендуется: 16MB для большинства случаев
wal_buffers = 16MB

# min_wal_size: Минимальный размер WAL файлов
# Опции: 80MB-1GB, влияет на частоту checkpoints
min_wal_size = 1GB

# max_wal_size: Максимальный размер WAL перед checkpoint
# Опции: 1GB-4GB, больше = реже checkpoints, но дольше восстановление
max_wal_size = 4GB

# Query Tuning
# random_page_cost: Стоимость чтения случайной страницы (SSD: 1.1, HDD: 4.0)
# Опции: 1.0-4.0, меньше для SSD
random_page_cost = 1.1

# effective_io_concurrency: Количество одновременных I/O операций (SSD: 200, HDD: 2)
# Опции: 1-1000, зависит от типа хранилища
effective_io_concurrency = 200

# Logging
# logging_collector: Включить сборщик логов в фоновом процессе
# Опции: on/off
logging_collector = on

# log_directory: Директория для логов (относительно data directory)
log_directory = 'log'

# log_filename: Шаблон имени файла лога
# Поддерживает: %Y (год), %m (месяц), %d (день), %H (час), %M (минута), %S (секунда)
log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'

# log_rotation_age: Ротация по времени (0 = отключено)
# Опции: 1min, 1h, 1d, 1w
log_rotation_age = 1d

# log_rotation_size: Ротация по размеру (0 = отключено)
# Опции: 10MB, 100MB, 1GB
log_rotation_size = 100MB

# log_line_prefix: Префикс для каждой строки лога
# %t=timestamp, %p=PID, %l=log line, %u=user, %d=database, %a=app, %h=host
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '

# log_timezone: Часовой пояс для логов
log_timezone = 'UTC'

# Locale Settings
# datestyle: Формат даты
# Опции: 'iso, mdy', 'iso, dmy', 'sql, mdy', 'postgres, mdy'
datestyle = 'iso, mdy'

# timezone: Часовой пояс для timestamp
# Опции: 'UTC', 'Europe/Moscow', 'America/New_York', etc.
timezone = 'UTC'

# lc_messages: Язык сообщений
lc_messages = 'en_US.utf8'

# lc_monetary: Формат денежных значений
lc_monetary = 'en_US.utf8'

# lc_numeric: Формат чисел
lc_numeric = 'en_US.utf8'

# lc_time: Формат времени
lc_time = 'en_US.utf8'

# default_text_search_config: Конфигурация полнотекстового поиска
# Опции: 'pg_catalog.english', 'pg_catalog.russian', 'pg_catalog.simple'
default_text_search_config = 'pg_catalog.english'
```

### Docker Compose для PostgreSQL

**Документация:** https://hub.docker.com/_/postgres

```bash
nano docker-compose.postgres.yml
```

Содержимое:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:18-alpine
    container_name: postgres
    environment:
      POSTGRES_USER: developer
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD:-secret123}
      POSTGRES_DB: myapp
      PGDATA: /var/lib/postgresql/data/pgdata
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./postgres-config/postgresql.conf:/etc/postgresql/postgresql.conf:ro
      - ./init-scripts:/docker-entrypoint-initdb.d:ro
    command: postgres -c config_file=/etc/postgresql/postgresql.conf
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U developer -d myapp"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped
    networks:
      - db-network

networks:
  db-network:
    driver: bridge

volumes:
  pgdata:
```

### Запуск

```bash
# Создание .env файла
echo "POSTGRES_PASSWORD=secret123" > .env

# Запуск
podman-compose -f docker-compose.postgres.yml up -d

# Проверка
podman-compose -f docker-compose.postgres.yml ps

# Подключение
podman exec -it postgres psql -U developer -d myapp
```

---

## 8.3. Инициализация базы данных

### Создание init скрипта

**Документация:** https://www.postgresql.org/docs/18/sql.html

```bash
mkdir -p init-scripts
nano init-scripts/01-init.sql
```

Содержимое:

```sql
-- Создание расширений
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";
CREATE EXTENSION IF NOT EXISTS "pg_trgm";

-- Создание таблицы users
CREATE TABLE IF NOT EXISTS users (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Создание индексов
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_created_at ON users(created_at DESC);

-- Создание таблицы posts
CREATE TABLE IF NOT EXISTS posts (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(500) NOT NULL,
    content TEXT,
    published BOOLEAN DEFAULT false,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);

-- Создание индексов для posts
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_published ON posts(published) WHERE published = true;
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);

-- Создание функции для обновления updated_at
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ language 'plpgsql';

-- Создание триггеров
CREATE TRIGGER update_users_updated_at BEFORE UPDATE ON users
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_posts_updated_at BEFORE UPDATE ON posts
    FOR EACH ROW EXECUTE FUNCTION update_updated_at_column();

-- Вставка тестовых данных
INSERT INTO users (email, name, password_hash) VALUES
    ('john@example.com', 'John Doe', '$2a$10$...'),
    ('jane@example.com', 'Jane Smith', '$2a$10$...')
ON CONFLICT (email) DO NOTHING;
```

### Перезапуск с инициализацией

```bash
# Остановка и удаление
podman-compose -f docker-compose.postgres.yml down -v

# Запуск с инициализацией
podman-compose -f docker-compose.postgres.yml up -d

# Проверка таблиц
podman exec -it postgres psql -U developer -d myapp -c "\dt"
```

---

## 8.4. Подключение из Node.js через Prisma

**Prisma** — современный ORM для Node.js и TypeScript.

**Архитектура работы с Prisma:**

```mermaid
graph TB
    subgraph app["🚀 Node.js Application"]
        code["Application Code<br/>(TypeScript)"]
        client["Prisma Client<br/>(Auto-generated)"]
    end
    
    subgraph prisma["⚙️ Prisma Tools"]
        schema["Schema Definition<br/>(schema.prisma)"]
        migrate["Prisma Migrate<br/>(Migrations)"]
        studio["Prisma Studio<br/>(GUI)"]
    end
    
    subgraph db["💾 PostgreSQL"]
        tables["Database Tables"]
        indexes["Indexes"]
        relations["Relations"]
    end
    
    code -->|Query| client
    client -->|SQL| tables
    schema -->|Generate| client
    schema -->|Create| migrate
    migrate -->|Apply| tables
    studio -->|Manage| tables
    tables --> indexes
    tables --> relations
    
    style app fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style prisma fill:#2196F3,stroke:#1976D2,stroke-width:3px,color:#fff
    style db fill:#FF9800,stroke:#F57C00,stroke-width:3px,color:#fff
    style code fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style client fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style schema fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style migrate fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style studio fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style tables fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style indexes fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style relations fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
```

**Актуальная версия:** Prisma 6.2.0

**Ссылки:**
- Официальный сайт: https://www.prisma.io/
- GitHub: https://github.com/prisma/prisma
- Документация: https://www.prisma.io/docs

### Установка Prisma

```bash
cd ~/fastify-api

# Установка Prisma CLI
npm install --save-dev prisma

# Установка Prisma Client
npm install @prisma/client

# Инициализация Prisma
npx prisma init
```

### Настройка schema.prisma

**Документация:** https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference

```bash
nano prisma/schema.prisma
```

Содержимое:

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id           String   @id @default(uuid()) @db.Uuid
  email        String   @unique @db.VarChar(255)
  name         String   @db.VarChar(255)
  passwordHash String   @map("password_hash") @db.VarChar(255)
  createdAt    DateTime @default(now()) @map("created_at") @db.Timestamptz
  updatedAt    DateTime @updatedAt @map("updated_at") @db.Timestamptz
  
  posts Post[]

  @@index([email])
  @@index([createdAt(sort: Desc)])
  @@map("users")
}

model Post {
  id        String   @id @default(uuid()) @db.Uuid
  userId    String   @map("user_id") @db.Uuid
  title     String   @db.VarChar(500)
  content   String?  @db.Text
  published Boolean  @default(false)
  createdAt DateTime @default(now()) @map("created_at") @db.Timestamptz
  updatedAt DateTime @updatedAt @map("updated_at") @db.Timestamptz
  
  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@index([userId])
  @@index([published])
  @@index([createdAt(sort: Desc)])
  @@map("posts")
}
```

### Настройка .env

**Документация:** https://www.prisma.io/docs/orm/more/development-environment/environment-variables

```bash
nano .env
```

Содержимое:

```env
DATABASE_URL="postgresql://developer:secret123@localhost:5432/myapp?schema=public"
```

### Генерация Prisma Client

```bash
# Генерация клиента
npx prisma generate

# Применение миграций (если нужно)
npx prisma db push

# Просмотр данных через Prisma Studio
npx prisma studio
```

### Интеграция с Fastify

```bash
nano src/db.ts
```

Содержимое:

```typescript
import { PrismaClient } from '@prisma/client';

export const prisma = new PrismaClient({
  log: ['query', 'info', 'warn', 'error'],
});

// Graceful shutdown
process.on('beforeExit', async () => {
  await prisma.$disconnect();
});
```

### Обновление server.ts

```typescript
import Fastify from 'fastify';
import { prisma } from './db.js';

const fastify = Fastify({ logger: true });

// GET all users
fastify.get('/users', async (request, reply) => {
  const users = await prisma.user.findMany({
    select: {
      id: true,
      email: true,
      name: true,
      createdAt: true,
    },
  });
  return users;
});

// GET user by ID
fastify.get('/users/:id', async (request, reply) => {
  const { id } = request.params as { id: string };
  const user = await prisma.user.findUnique({
    where: { id },
    include: {
      posts: true,
    },
  });
  
  if (!user) {
    reply.code(404);
    return { error: 'User not found' };
  }
  
  return user;
});

// POST create user
fastify.post('/users', async (request, reply) => {
  const { email, name, password } = request.body as {
    email: string;
    name: string;
    password: string;
  };
  
  const user = await prisma.user.create({
    data: {
      email,
      name,
      passwordHash: password, // В production используйте bcrypt!
    },
  });
  
  reply.code(201);
  return user;
});

const start = async () => {
  try {
    await fastify.listen({ port: 3001, host: '0.0.0.0' });
  } catch (err) {
    fastify.log.error(err);
    process.exit(1);
  }
};

start();
```

---

## 8.5. Backup и восстановление

### Создание backup

```bash
# Backup всей БД
podman exec postgres pg_dump -U developer myapp > backup.sql

# Backup с сжатием
podman exec postgres pg_dump -U developer myapp | gzip > backup.sql.gz

# Backup конкретной таблицы
podman exec postgres pg_dump -U developer -t users myapp > users_backup.sql

# Custom format (рекомендуется)
podman exec postgres pg_dump -U developer -Fc myapp > backup.dump
```

### Восстановление

```bash
# Восстановление из SQL
podman exec -i postgres psql -U developer myapp < backup.sql

# Восстановление из gzip
gunzip -c backup.sql.gz | podman exec -i postgres psql -U developer myapp

# Восстановление из custom format
podman exec -i postgres pg_restore -U developer -d myapp backup.dump
```

### Автоматический backup

```bash
nano backup-postgres.sh
```

Содержимое:

```bash
#!/bin/bash

BACKUP_DIR="/home/developer/backups/postgres"
DATE=$(date +%Y%m%d_%H%M%S)
CONTAINER="postgres"
DB_USER="developer"
DB_NAME="myapp"

mkdir -p $BACKUP_DIR

# Создание backup
podman exec $CONTAINER pg_dump -U $DB_USER -Fc $DB_NAME > \
  $BACKUP_DIR/backup_${DATE}.dump

# Удаление старых backup'ов (старше 7 дней)
find $BACKUP_DIR -name "backup_*.dump" -mtime +7 -delete

echo "Backup completed: backup_${DATE}.dump"
```

Настройка cron:

```bash
chmod +x backup-postgres.sh

# Добавление в crontab (каждый день в 2:00)
crontab -e
# Добавьте: 0 2 * * * /home/developer/backup-postgres.sh
```

---

**Практическое задание:**

1. Разверните PostgreSQL 18 в контейнере
2. Создайте init скрипты с таблицами
3. Установите и настройте Prisma
4. Создайте API endpoints с Prisma
5. Настройте автоматический backup
6. Протестируйте восстановление из backup

**Проверка знаний:**

```bash
# Какая версия PostgreSQL?
podman exec postgres psql -U developer -c "SELECT version();"

# Сколько таблиц в БД?
podman exec postgres psql -U developer -d myapp -c "\dt"

# Размер БД?
podman exec postgres psql -U developer -d myapp -c \
  "SELECT pg_size_pretty(pg_database_size('myapp'));"

# Работает ли Prisma?
npx prisma studio
```

---
