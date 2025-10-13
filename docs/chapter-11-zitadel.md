[← Оглавление курса](index.md)


# Глава 9. Zitadel IAM: централизованная аутентификация

## 9.1. Zitadel: современный IAM

**Zitadel** — open-source Identity and Access Management (IAM) платформа.

**Ключевые характеристики:**
- **Multi-tenancy** — поддержка множества организаций
- **OIDC/OAuth2/SAML** — стандартные протоколы
- **MFA** — многофакторная аутентификация
- **Self-hosted** — полный контроль над данными
- **API-first** — все через API

**Где применяется:**
- Централизованная аутентификация
- SSO (Single Sign-On)
- B2B/B2C приложения
- Микросервисная архитектура
- Multi-tenant SaaS

**Почему в этом курсе:**
- Современная альтернатива Keycloak/Auth0
- Отличная производительность
- Простая интеграция
- Богатая функциональность
- Open-source и бесплатно

**Распространенность:**
- 7K+ GitHub stars
- Растущее принятие в enterprise
- Активное развитие
- Хорошая документация

**Актуальная версия:** v4.3.0

**Ссылки:**
- Официальный сайт: https://zitadel.com/
- GitHub: https://github.com/zitadel/zitadel
- Документация: https://zitadel.com/docs
- Community: https://zitadel.com/chat


## 9.2. Развертывание Zitadel

### Подготовка

```bash
# Создание директории
mkdir -p ~/zitadel && cd ~/zitadel

# Создание сети
podman network create zitadel-network
```

### PostgreSQL для Zitadel

**Документация:** https://zitadel.com/docs/self-hosting/deploy/compose

```bash
nano docker-compose.zitadel.yaml
```

Содержимое:

```yaml
version: '3.8'

services:
  # PostgreSQL для Zitadel
  zitadel-db:
    image: postgres:18-alpine
    container_name: zitadel-db
    environment:
      POSTGRES_USER: zitadel
      POSTGRES_PASSWORD: ${ZITADEL_DB_PASSWORD:-zitadel_secret}
      POSTGRES_DB: zitadel
    volumes:
      - zitadel-db-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U zitadel"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - zitadel-network
    restart: unless-stopped

  # Zitadel
  zitadel:
    image: ghcr.io/zitadel/zitadel:v4.3.0
    container_name: zitadel
    command: 'start-from-init --masterkeyFromEnv --tlsMode disabled'
    environment:
      - ZITADEL_DATABASE_POSTGRES_HOST=zitadel-db
      - ZITADEL_DATABASE_POSTGRES_PORT=5432
      - ZITADEL_DATABASE_POSTGRES_DATABASE=zitadel
      - ZITADEL_DATABASE_POSTGRES_USER_USERNAME=zitadel
      - ZITADEL_DATABASE_POSTGRES_USER_PASSWORD=${ZITADEL_DB_PASSWORD:-zitadel_secret}
      - ZITADEL_DATABASE_POSTGRES_USER_SSL_MODE=disable
      - ZITADEL_DATABASE_POSTGRES_ADMIN_USERNAME=zitadel
      - ZITADEL_DATABASE_POSTGRES_ADMIN_PASSWORD=${ZITADEL_DB_PASSWORD:-zitadel_secret}
      - ZITADEL_DATABASE_POSTGRES_ADMIN_SSL_MODE=disable
      - ZITADEL_EXTERNALSECURE=false
      - ZITADEL_EXTERNALPORT=8080
      - ZITADEL_EXTERNALDOMAIN=localhost
      - ZITADEL_MASTERKEY=${ZITADEL_MASTERKEY:-MasterkeyNeedsToHave32Characters}
      - ZITADEL_FIRSTINSTANCE_ORG_NAME=EventBrain
      - ZITADEL_FIRSTINSTANCE_ORG_HUMAN_USERNAME=admin
      - ZITADEL_FIRSTINSTANCE_ORG_HUMAN_PASSWORD=${ZITADEL_ADMIN_PASSWORD:-Admin123!}
    ports:
      - "8080:8080"
    depends_on:
      zitadel-db:
        condition: service_healthy
    networks:
      - zitadel-network
    restart: unless-stopped

networks:
  zitadel-network:
    external: true

volumes:
  zitadel-db-data:
```

### Создание .env файла

**Документация:** https://docs.docker.com/compose/environment-variables/

```bash
nano .env
```

Содержимое:

```env
ZITADEL_DB_PASSWORD=strong_db_password_here
ZITADEL_MASTERKEY=MasterkeyNeedsToHave32Characters
ZITADEL_ADMIN_PASSWORD=Admin123!Strong
```

### Запуск Zitadel

```bash
# Запуск
podman-compose -f docker-compose.zitadel.yaml up -d

# Проверка логов
podman-compose -f docker-compose.zitadel.yaml logs -f zitadel

# Ожидание инициализации (может занять 1-2 минуты)
# Открыть в браузере
# http://localhost:8080

# Логин:
# Username: admin
# Password: Admin123!Strong (из .env)
```


## 9.3. Настройка организации и проектов

### Создание проекта через UI

1. Откройте http://localhost:8080
2. Войдите как admin
3. Перейдите в "Projects"
4. Создайте новый проект:
   - Name: EventBrain API
   - Description: API authentication

### Создание приложения (Application)

1. В проекте "EventBrain API"
2. Нажмите "New Application"
3. Выберите тип: "Web"
4. Настройки:
   - Name: Fastify API
   - Authentication Method: PKCE
   - Redirect URIs: http://localhost:3001/auth/callback
   - Post Logout URIs: http://localhost:3001
   - Grant Types: Authorization Code, Refresh Token

5. Сохраните Client ID и Client Secret

### Создание через API

```bash
# Получение access token
ACCESS_TOKEN=$(curl -X POST http://localhost:8080/oauth/v2/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=YOUR_CLIENT_ID" \
  -d "client_secret=YOUR_CLIENT_SECRET" \
  -d "scope=openid" | jq -r '.access_token')

# Создание проекта
curl -X POST http://localhost:8080/management/v1/projects \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "EventBrain API",
    "projectRoleAssertion": true,
    "projectRoleCheck": true
  }'
```


## 9.4. OIDC/OAuth2 интеграция

### Установка зависимостей

```bash
cd ~/fastify-api

# Установка Lucia Auth и адаптеров
npm install lucia @lucia-auth/adapter-postgresql
npm install --save-dev @types/lucia
```

### Настройка Lucia Auth

**Документация:** https://lucia-auth.com/getting-started/

```bash
nano src/auth.ts
```

Содержимое:

```typescript
import { Lucia } from 'lucia';
import { PostgresJsAdapter } from '@lucia-auth/adapter-postgresql';
import postgres from 'postgres';

const sql = postgres(process.env.DATABASE_URL!);

const adapter = new PostgresJsAdapter(sql, {
  user: 'users',
  session: 'sessions'
});

export const lucia = new Lucia(adapter, {
  sessionCookie: {
    attributes: {
      secure: process.env.NODE_ENV === 'production'
    }
  },
  getUserAttributes: (attributes) => {
    return {
      email: attributes.email,
      name: attributes.name
    };
  }
});

declare module 'lucia' {
  interface Register {
    Lucia: typeof lucia;
    DatabaseUserAttributes: {
      email: string;
      name: string;
    };
  }
}
```

### Создание таблиц для сессий

```sql
-- init-scripts/02-auth.sql
CREATE TABLE IF NOT EXISTS sessions (
    id TEXT PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    expires_at TIMESTAMP WITH TIME ZONE NOT NULL
);

CREATE INDEX idx_sessions_user_id ON sessions(user_id);
```

### OIDC endpoints

```typescript
// src/routes/auth.ts
import { FastifyInstance } from 'fastify';
import { lucia } from '../auth.js';

export async function authRoutes(fastify: FastifyInstance) {
  // Login redirect
  fastify.get('/auth/login', async (request, reply) => {
    const authUrl = new URL('http://localhost:8080/oauth/v2/authorize');
    authUrl.searchParams.set('client_id', process.env.ZITADEL_CLIENT_ID!);
    authUrl.searchParams.set('redirect_uri', 'http://localhost:3001/auth/callback');
    authUrl.searchParams.set('response_type', 'code');
    authUrl.searchParams.set('scope', 'openid profile email');
    
    reply.redirect(authUrl.toString());
  });

  // Callback
  fastify.get('/auth/callback', async (request, reply) => {
    const { code } = request.query as { code: string };
    
    // Exchange code for tokens
    const tokenResponse = await fetch('http://localhost:8080/oauth/v2/token', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
      },
      body: new URLSearchParams({
        grant_type: 'authorization_code',
        code,
        redirect_uri: 'http://localhost:3001/auth/callback',
        client_id: process.env.ZITADEL_CLIENT_ID!,
        client_secret: process.env.ZITADEL_CLIENT_SECRET!,
      }),
    });
    
    const tokens = await tokenResponse.json();
    
    // Get user info
    const userResponse = await fetch('http://localhost:8080/oidc/v1/userinfo', {
      headers: {
        Authorization: `Bearer ${tokens.access_token}`,
      },
    });
    
    const userInfo = await userResponse.json();
    
    // Create or update user in database
    const user = await prisma.user.upsert({
      where: { email: userInfo.email },
      update: { name: userInfo.name },
      create: {
        email: userInfo.email,
        name: userInfo.name,
        passwordHash: '', // Not used for OIDC users
      },
    });
    
    // Create session
    const session = await lucia.createSession(user.id, {});
    const sessionCookie = lucia.createSessionCookie(session.id);
    
    reply.setCookie(sessionCookie.name, sessionCookie.value, sessionCookie.attributes);
    reply.redirect('/');
  });

  // Logout
  fastify.post('/auth/logout', async (request, reply) => {
    const sessionId = request.cookies.auth_session;
    
    if (sessionId) {
      await lucia.invalidateSession(sessionId);
    }
    
    reply.clearCookie('auth_session');
    reply.send({ success: true });
  });

  // Current user
  fastify.get('/auth/me', async (request, reply) => {
    const sessionId = request.cookies.auth_session;
    
    if (!sessionId) {
      reply.code(401);
      return { error: 'Unauthorized' };
    }
    
    const { session, user } = await lucia.validateSession(sessionId);
    
    if (!session) {
      reply.code(401);
      return { error: 'Invalid session' };
    }
    
    return { user };
  });
}
```

### Middleware для защиты routes

```typescript
// src/middleware/auth.ts
import { FastifyRequest, FastifyReply } from 'fastify';
import { lucia } from '../auth.js';

export async function requireAuth(request: FastifyRequest, reply: FastifyReply) {
  const sessionId = request.cookies.auth_session;
  
  if (!sessionId) {
    reply.code(401);
    throw new Error('Unauthorized');
  }
  
  const { session, user } = await lucia.validateSession(sessionId);
  
  if (!session) {
    reply.code(401);
    throw new Error('Invalid session');
  }
  
  request.user = user;
}

declare module 'fastify' {
  interface FastifyRequest {
    user?: {
      id: string;
      email: string;
      name: string;
    };
  }
}
```

### Использование middleware

```typescript
// src/server.ts
import { authRoutes } from './routes/auth.js';
import { requireAuth } from './middleware/auth.js';

// Регистрация auth routes
fastify.register(authRoutes);

// Защищенный endpoint
fastify.get('/protected', { preHandler: requireAuth }, async (request, reply) => {
  return {
    message: 'This is protected',
    user: request.user
  };
});
```


## 9.5. Подключение к React через Lucia Auth

### Установка зависимостей

```bash
cd ~/react-app

npm install @tanstack/react-query axios
```

### Создание auth context

```typescript
// src/contexts/AuthContext.tsx
import { createContext, useContext, useState, useEffect } from 'react';
import axios from 'axios';

interface User {
  id: string;
  email: string;
  name: string;
}

interface AuthContextType {
  user: User | null;
  loading: boolean;
  login: () => void;
  logout: () => Promise<void>;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    checkAuth();
  }, []);

  const checkAuth = async () => {
    try {
      const response = await axios.get('http://localhost:3001/auth/me', {
        withCredentials: true
      });
      setUser(response.data.user);
    } catch (error) {
      setUser(null);
    } finally {
      setLoading(false);
    }
  };

  const login = () => {
    window.location.href = 'http://localhost:3001/auth/login';
  };

  const logout = async () => {
    await axios.post('http://localhost:3001/auth/logout', {}, {
      withCredentials: true
    });
    setUser(null);
  };

  return (
    <AuthContext.Provider value={{ user, loading, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
}
```

### Использование в компонентах

```typescript
// src/App.tsx
import { AuthProvider, useAuth } from './contexts/AuthContext';

function LoginButton() {
  const { user, loading, login, logout } = useAuth();

  if (loading) {
    return <div>Loading...</div>;
  }

  if (user) {
    return (
      <div>
        <p>Welcome, {user.name}!</p>
        <button onClick={logout}>Logout</button>
      </div>
    );
  }

  return <button onClick={login}>Login with Zitadel</button>;
}

function App() {
  return (
    <AuthProvider>
      <div className="App">
        <h1>EventBrain</h1>
        <LoginButton />
      </div>
    </AuthProvider>
  );
}

export default App;
```


**Практическое задание:**

1. Разверните Zitadel с PostgreSQL
2. Создайте организацию и проект
3. Настройте OIDC приложение
4. Интегрируйте Lucia Auth в Node.js API
5. Создайте защищенные endpoints
6. Подключите React приложение
7. Протестируйте полный flow аутентификации

**Проверка знаний:**

```bash
# Работает ли Zitadel?
curl http://localhost:8080/debug/healthz

# Доступен ли OIDC endpoint?
curl http://localhost:8080/.well-known/openid-configuration

# Работает ли auth в API?
curl http://localhost:3001/auth/me

# Можно ли получить токен?
# Проверьте через Zitadel UI
```


