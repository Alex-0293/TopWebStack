[← Оглавление курса](index.md)

---

# Глава 7. Vite: современный фронтенд

## 7.1. Vite: зачем нужен и как работает

**Vite** — современный build tool для фронтенд разработки, созданный автором Vue.js.

**Ключевые характеристики:**
- **Мгновенный запуск** — нативные ES modules
- **HMR** — Hot Module Replacement за миллисекунды
- **Оптимизация** — автоматический code splitting
- **TypeScript** — встроенная поддержка
- **Framework agnostic** — работает с React, Vue, Svelte

**Где применяется:**
- Single Page Applications (SPA)
- Progressive Web Apps (PWA)
- Static Site Generation (SSG)
- Component libraries
- Micro-frontends

**Почему в этом курсе:**
- Самый быстрый dev server
- Простая настройка
- Отличная производительность
- Современный стандарт
- Легкая контейнеризация

**Распространенность:**
- #1 выбор для новых проектов
- 10M+ загрузок в неделю
- Используется в Vue 3, Nuxt 3, SvelteKit
- Растущее принятие в React сообществе

**Vite 6 — новые возможности:**
- Улучшенная производительность
- Лучшая поддержка Environment API
- Оптимизированный build
- Улучшенный dev server
- Новые плагины

**Актуальная версия:** Vite 6.0.0

**Ссылки:**
- Официальный сайт: https://vitejs.dev/
- GitHub: https://github.com/vitejs/vite
- Документация: https://vitejs.dev/guide/
- Awesome Vite: https://github.com/vitejs/awesome-vite

---

## 7.2. Создание React-приложения с Vite

### Создание проекта

```bash
# Создание React + TypeScript проекта
npm create vite@latest react-app -- --template react-ts

# Переход в директорию
cd react-app

# Установка зависимостей
npm install

# Запуск dev server
npm run dev
```

### Структура проекта

```
react-app/
├── public/              # Статические файлы
├── src/
│   ├── assets/         # Изображения, стили
│   ├── components/     # React компоненты
│   ├── App.tsx         # Главный компонент
│   ├── main.tsx        # Entry point
│   └── vite-env.d.ts   # TypeScript definitions
├── index.html          # HTML template
├── package.json
├── tsconfig.json       # TypeScript config
├── tsconfig.node.json  # TypeScript config для Vite
└── vite.config.ts      # Vite configuration
```

### Настройка Vite

**Документация:** https://vitejs.dev/config/

```bash
nano vite.config.ts
```

Содержимое:

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
  server: {
    host: '0.0.0.0',
    port: 5173,
    strictPort: true,
    watch: {
      usePolling: true
    }
  },
  preview: {
    host: '0.0.0.0',
    port: 5173,
    strictPort: true
  },
  build: {
    outDir: 'dist',
    sourcemap: false,
    minify: 'esbuild',
    chunkSizeWarningLimit: 1000
  }
});
```

### Создание простого компонента

**Документация:** https://react.dev/learn

```bash
mkdir src/components
nano src/components/UserCard.tsx
```

Содержимое:

```typescript
import { useState } from 'react';

interface User {
  id: string;
  name: string;
  email: string;
}

export function UserCard() {
  const [users, setUsers] = useState<User[]>([]);
  const [loading, setLoading] = useState(false);

  const fetchUsers = async () => {
    setLoading(true);
    try {
      const response = await fetch('http://localhost:3001/users');
      const data = await response.json();
      setUsers(Array.isArray(data) ? data : [data]);
    } catch (error) {
      console.error('Error fetching users:', error);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div style={{ padding: '20px' }}>
      <h2>User Management</h2>
      <button onClick={fetchUsers} disabled={loading}>
        {loading ? 'Loading...' : 'Fetch Users'}
      </button>
      
      <div style={{ marginTop: '20px' }}>
        {users.map(user => (
          <div key={user.id} style={{ 
            border: '1px solid #ccc', 
            padding: '10px', 
            margin: '10px 0',
            borderRadius: '4px'
          }}>
            <h3>{user.name}</h3>
            <p>Email: {user.email}</p>
            <p>ID: {user.id}</p>
          </div>
        ))}
      </div>
    </div>
  );
}
```

### Обновление App.tsx

```bash
nano src/App.tsx
```

Содержимое:

```typescript
import { UserCard } from './components/UserCard';
import './App.css';

function App() {
  return (
    <div className="App">
      <h1>Vite + React + TypeScript</h1>
      <UserCard />
    </div>
  );
}

export default App;
```

### Запуск и тестирование

```bash
# Запуск dev server
npm run dev

# Открыть в браузере
# http://localhost:5173

# Build для production
npm run build

# Preview production build
npm run preview
```

---

## 7.3. Контейнеризация фронтенда

### Dockerfile для development

**Документация:** https://docs.docker.com/engine/reference/builder/

```bash
nano Dockerfile.dev
```

Содержимое:

```dockerfile
FROM docker.io/library/node:26-alpine

WORKDIR /app

# Копирование package файлов
COPY package*.json ./

# Установка зависимостей
RUN npm install

# Копирование исходного кода
COPY . .

# Открытие порта
EXPOSE 5173

# Запуск dev server
CMD ["npm", "run", "dev"]
```

### Запуск dev контейнера

```bash
# Сборка dev образа
podman build -t react-app:dev -f Dockerfile.dev .

# Запуск с volume для hot reload
podman run -d \
  --name react-dev \
  -p 5173:5173 \
  -v $(pwd)/src:/app/src:z \
  react-app:dev

# Просмотр логов
podman logs -f react-dev
```

---

## 7.4. Nginx для production

### Multi-stage Dockerfile для production

**Документация:** https://docs.docker.com/build/building/multi-stage/

```bash
nano Dockerfile
```

Содержимое:

```dockerfile
# Stage 1: Build
FROM docker.io/library/node:26-alpine AS builder

WORKDIR /app

# Копирование package файлов
COPY package*.json ./

# Установка зависимостей
RUN npm ci

# Копирование исходного кода
COPY . .

# Build приложения
RUN npm run build

# Stage 2: Production с Nginx
FROM docker.io/library/nginx:alpine

# Копирование собранного приложения
COPY --from=builder /app/dist /usr/share/nginx/html

# Копирование nginx конфигурации
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Открытие порта
EXPOSE 80

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost/health || exit 1

# Запуск nginx
CMD ["nginx", "-g", "daemon off;"]
```

### Создание nginx.conf

**Документация:** https://nginx.org/en/docs/

```bash
nano nginx.conf
```

Содержимое:

```nginx
server {
    # Порт для прослушивания
    # Опции: 80 (HTTP), 443 (HTTPS), 8080, etc.
    listen 80;
    
    # Имя сервера (домен)
    # Опции: localhost, example.com, _ (любой)
    server_name localhost;
    
    # Корневая директория для статических файлов
    root /usr/share/nginx/html;
    
    # Индексный файл по умолчанию
    # Опции: index.html, index.htm, index.php
    index index.html;

    # ============================================
    # Gzip compression для уменьшения размера
    # ============================================
    # Включить gzip сжатие
    gzip on;
    
    # Добавлять Vary: Accept-Encoding header
    gzip_vary on;
    
    # Минимальный размер файла для сжатия (байты)
    # Файлы меньше этого размера не сжимаются
    gzip_min_length 1024;
    
    # MIME типы для сжатия
    # Изображения не сжимаются (уже сжаты)
    gzip_types text/plain text/css text/xml text/javascript 
               application/x-javascript application/xml+rss 
               application/javascript application/json;

    # ============================================
    # Security headers для защиты
    # ============================================
    # X-Frame-Options: защита от clickjacking
    # Опции: DENY (запретить), SAMEORIGIN (только свой домен), ALLOW-FROM uri
    add_header X-Frame-Options "SAMEORIGIN" always;
    
    # X-Content-Type-Options: запретить MIME-sniffing
    # Опции: nosniff (единственное значение)
    add_header X-Content-Type-Options "nosniff" always;
    
    # X-XSS-Protection: защита от XSS (устарело, но для старых браузеров)
    # Опции: 0 (отключить), 1 (включить), 1; mode=block (блокировать)
    add_header X-XSS-Protection "1; mode=block" always;

    # ============================================
    # Cache static assets - кэширование статики
    # ============================================
    # Регулярное выражение для статических файлов
    # ~* = case-insensitive regex
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        # Время кэширования (1y = 1 год)
        # Опции: 1d (день), 1w (неделя), 1M (месяц), 1y (год)
        expires 1y;
        
        # Cache-Control header
        # public = может кэшироваться прокси
        # immutable = файл никогда не изменится (для файлов с хэшем в имени)
        add_header Cache-Control "public, immutable";
    }

    # ============================================
    # SPA routing - маршрутизация для React
    # ============================================
    location / {
        # try_files: попытаться найти файл, иначе вернуть index.html
        # $uri = запрошенный путь
        # $uri/ = запрошенный путь как директория
        # /index.html = fallback для SPA роутинга
        try_files $uri $uri/ /index.html;
    }

    # ============================================
    # Health check endpoint для мониторинга
    # ============================================
    location /health {
        # Не писать в access log (уменьшает нагрузку)
        access_log off;
        
        # Вернуть HTTP 200 с текстом
        return 200 "healthy\n";
        
        # Content-Type для ответа
        add_header Content-Type text/plain;
    }

    # ============================================
    # API proxy - проксирование на backend
    # ============================================
    location /api/ {
        # Проксировать на backend API
        # Убирает /api/ из пути: /api/users -> /users
        proxy_pass http://fastify-api:3001/;
        
        # HTTP версия для проксирования
        # 1.1 нужна для WebSocket и keep-alive
        proxy_http_version 1.1;
        
        # Headers для WebSocket
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        
        # Передать оригинальный Host header
        proxy_set_header Host $host;
        
        # Не кэшировать WebSocket соединения
        proxy_cache_bypass $http_upgrade;
        
        # Передать реальный IP клиента
        proxy_set_header X-Real-IP $remote_addr;
        
        # Цепочка прокси (для нескольких прокси)
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # Протокол (http/https)
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Сборка и запуск production образа

```bash
# Сборка production образа
podman build -t react-app:prod .

# Проверка размера
podman images | grep react-app
# react-app  prod  ...  ~25MB (очень компактный!)

# Запуск production контейнера
podman run -d \
  --name react-prod \
  -p 8080:80 \
  react-app:prod

# Тестирование
curl http://localhost:8080/health
# Вывод: healthy

# Открыть в браузере
# http://localhost:8080
```

---

## 7.5. Docker Compose для fullstack приложения

### Создание docker-compose.yml

```bash
nano docker-compose.yml
```

Содержимое:

```yaml
version: '3.8'

services:
  # Backend API
  api:
    build:
      context: ../fastify-api
      dockerfile: Dockerfile.optimized
    image: fastify-api:latest
    container_name: fastify-api
    ports:
      - "3001:3001"
    environment:
      - NODE_ENV=production
      - PORT=3001
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "wget", "--quiet", "--tries=1", "--spider", "http://localhost:3001/health"]
      interval: 30s
      timeout: 3s
      retries: 3
    restart: unless-stopped

  # Frontend
  frontend:
    build:
      context: .
      dockerfile: Dockerfile
    image: react-app:latest
    container_name: react-app
    ports:
      - "8080:80"
    depends_on:
      api:
        condition: service_healthy
    networks:
      - app-network
    healthcheck:
      test: ["CMD", "wget", "--quiet", "--tries=1", "--spider", "http://localhost/health"]
      interval: 30s
      timeout: 3s
      retries: 3
    restart: unless-stopped

networks:
  app-network:
    driver: bridge
```

### Запуск fullstack приложения

```bash
# Сборка и запуск всех сервисов
podman-compose up -d --build

# Просмотр логов
podman-compose logs -f

# Проверка статуса
podman-compose ps

# Тестирование API
curl http://localhost:3001/health

# Тестирование Frontend
curl http://localhost:8080/health

# Открыть в браузере
# http://localhost:8080

# Остановка
podman-compose down
```

---

## 7.6. Оптимизация production build

### Настройка vite.config.ts для production

```typescript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { visualizer } from 'rollup-plugin-visualizer';

export default defineConfig({
  plugins: [
    react(),
    visualizer({
      filename: './dist/stats.html',
      open: false,
      gzipSize: true,
      brotliSize: true
    })
  ],
  build: {
    outDir: 'dist',
    sourcemap: false,
    minify: 'esbuild',
    target: 'es2020',
    cssCodeSplit: true,
    chunkSizeWarningLimit: 1000,
    rollupOptions: {
      output: {
        manualChunks: {
          'react-vendor': ['react', 'react-dom'],
        }
      }
    }
  }
});
```

### Установка плагина для анализа

```bash
npm install --save-dev rollup-plugin-visualizer

# Build с анализом
npm run build

# Просмотр статистики
# Откройте dist/stats.html в браузере
```

### Измерение производительности

```bash
# Размер build
du -sh dist/
# Вывод: ~500KB

# Количество файлов
find dist -type f | wc -l

# Gzip размер
gzip -c dist/assets/*.js | wc -c

# Lighthouse audit (требует Chrome)
npx lighthouse http://localhost:8080 --view
```

---

**Практическое задание:**

1. Создайте React приложение с Vite
2. Добавьте несколько компонентов
3. Настройте TypeScript
4. Создайте Dockerfile с multi-stage build
5. Настройте Nginx для production
6. Создайте docker-compose.yml с API и Frontend
7. Запустите fullstack приложение
8. Проверьте размер образов и производительность

**Проверка знаний:**

```bash
# Какой размер production образа?
podman images | grep react-app

# Работают ли health checks?
podman-compose ps

# Какой размер dist директории?
du -sh dist/

# Сколько памяти использует Nginx?
podman stats react-prod --no-stream
```

---

**Навигация:**
- [← Вернуться к содержанию](index.md)
- [← Глава 6: Node.js: современная разработка](chapter-06-nodejs.md)
- [→ Глава 8: PostgreSQL: надежное хранение данных](chapter-08-postgresql.md)
