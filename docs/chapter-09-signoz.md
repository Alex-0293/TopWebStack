[← Оглавление курса](index.md)

---

# Глава 9. SigNoz: мониторинг и observability

## 9.1. Что такое SigNoz и OpenTelemetry

**SigNoz** — open-source платформа для observability (мониторинг, трейсинг, логирование).

**Архитектура SigNoz:**

```mermaid
graph LR
    subgraph apps["📱 Applications"]
        backend["Backend<br/>Node.js"]
        frontend["Frontend<br/>React"]
        services["Other Services"]
    end
    
    subgraph otel["📡 OpenTelemetry"]
        sdk["OTel SDK<br/>(Instrumentation)"]
        collector["OTel Collector<br/>(4317/4318)"]
    end
    
    subgraph signoz["📊 SigNoz Stack"]
        query["Query Service<br/>(API)"]
        ui["SigNoz UI<br/>(3301)"]
        alertmanager["Alert Manager"]
    end
    
    subgraph storage["💾 Storage"]
        clickhouse["ClickHouse<br/>(Metrics & Traces)"]
        zookeeper["Zookeeper<br/>(Coordination)"]
    end
    
    backend --> sdk
    frontend --> sdk
    services --> sdk
    sdk -->|OTLP| collector
    collector -->|Process & Export| clickhouse
    clickhouse --> zookeeper
    query --> clickhouse
    ui --> query
    alertmanager --> query
    
    style apps fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style otel fill:#FF9800,stroke:#F57C00,stroke-width:3px,color:#fff
    style signoz fill:#2196F3,stroke:#1976D2,stroke-width:3px,color:#fff
    style storage fill:#9C27B0,stroke:#7B1FA2,stroke-width:3px,color:#fff
    style backend fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style frontend fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style services fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style sdk fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style collector fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style query fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style ui fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style alertmanager fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style clickhouse fill:#AB47BC,stroke:#8E24AA,stroke-width:2px,color:#fff
    style zookeeper fill:#AB47BC,stroke:#8E24AA,stroke-width:2px,color:#fff
```

**Ключевые характеристики:**
- **All-in-one** — метрики, трейсы, логи в одном месте
- **OpenTelemetry native** — стандартный протокол
- **Self-hosted** — полный контроль над данными
- **ClickHouse** — быстрое хранилище для телеметрии
- **Grafana-like UI** — удобные дашборды

**Где применяется:**
- Мониторинг микросервисов
- Distributed tracing
- Performance optimization
- Troubleshooting production issues
- SLA monitoring

**Почему в этом курсе:**
- Open-source альтернатива Datadog/New Relic
- Нативная поддержка OpenTelemetry
- Легкое развертывание в контейнерах
- Отличная производительность
- Бесплатно для любых масштабов

**Распространенность:**
- 15K+ GitHub stars
- Растущее принятие в enterprise
- Активное сообщество
- Регулярные обновления

**Актуальная версия:** SigNoz 0.96.1

**Ссылки:**
- Официальный сайт: https://signoz.io/
- GitHub: https://github.com/SigNoz/signoz
- Документация: https://signoz.io/docs/
- Community: https://signoz.io/slack

**OpenTelemetry** — стандарт для сбора телеметрии (CNCF проект).

**Актуальная версия Collector:** v1.43.0 / v0.137.0

**Ссылки:**
- Официальный сайт: https://opentelemetry.io/
- GitHub: https://github.com/open-telemetry
- Документация: https://opentelemetry.io/docs/

---

## 9.2. Развертывание SigNoz stack

### Подготовка

```bash
# Создание директории для SigNoz
mkdir -p ~/signoz && cd ~/signoz

# Скачивание docker-compose файлов
curl -L https://github.com/SigNoz/signoz/raw/main/deploy/docker/clickhouse-setup/docker-compose.yaml -o docker-compose.yaml

# Создание директорий для данных
mkdir -p data/signoz data/clickhouse data/alertmanager
```

### Настройка docker-compose.yaml

**Документация:** https://signoz.io/docs/install/docker/

```bash
nano docker-compose.yaml
```

Основные сервисы:

```yaml
version: '3.8'

services:
  # ClickHouse - хранилище данных
  clickhouse:
    image: clickhouse/clickhouse-server:25.9-alpine
    container_name: signoz-clickhouse
    hostname: clickhouse
    volumes:
      - ./data/clickhouse:/var/lib/clickhouse/
    environment:
      - CLICKHOUSE_DB=signoz
    ports:
      - "9000:9000"
      - "8123:8123"
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "localhost:8123/ping"]
      interval: 30s
      timeout: 5s
      retries: 3
    restart: unless-stopped

  # OTel Collector - сбор телеметрии
  otel-collector:
    image: signoz/signoz-otel-collector:0.96.1
    container_name: signoz-otel-collector
    command: ["--config=/etc/otel-collector-config.yaml"]
    volumes:
      - ./otel-collector-config.yaml:/etc/otel-collector-config.yaml
    environment:
      - OTEL_RESOURCE_ATTRIBUTES=host.name=signoz-host
    ports:
      - "4317:4317"   # OTLP gRPC
      - "4318:4318"   # OTLP HTTP
    depends_on:
      clickhouse:
        condition: service_healthy
    restart: unless-stopped

  # Query Service - API для UI
  query-service:
    image: signoz/query-service:0.96.1
    container_name: signoz-query-service
    command: ["-config=/root/config/prometheus.yml"]
    volumes:
      - ./data/signoz:/var/lib/signoz/
    environment:
      - ClickHouseUrl=tcp://clickhouse:9000
      - STORAGE=clickhouse
      - GODEBUG=netdns=go
      - TELEMETRY_ENABLED=true
      - DEPLOYMENT_TYPE=docker-standalone-amd
    ports:
      - "8080:8080"
    depends_on:
      clickhouse:
        condition: service_healthy
    healthcheck:
      test: ["CMD", "wget", "--spider", "-q", "localhost:8080/api/v1/health"]
      interval: 30s
      timeout: 5s
      retries: 3
    restart: unless-stopped

  # Frontend - UI
  frontend:
    image: signoz/frontend:0.96.1
    container_name: signoz-frontend
    ports:
      - "3301:3301"
    depends_on:
      - query-service
    environment:
      - FRONTEND_API_ENDPOINT=http://query-service:8080
    restart: unless-stopped

  # AlertManager - алерты
  alertmanager:
    image: signoz/alertmanager:0.23.5
    container_name: signoz-alertmanager
    volumes:
      - ./data/alertmanager:/data
    ports:
      - "9093:9093"
    depends_on:
      query-service:
        condition: service_healthy
    restart: unless-stopped
```

### Конфигурация OTel Collector

**Документация:** https://opentelemetry.io/docs/collector/configuration/

```bash
nano otel-collector-config.yaml
```

Содержимое:

```yaml
# ============================================
# Receivers - приемники телеметрии
# ============================================
receivers:
  # OTLP (OpenTelemetry Protocol) - стандартный протокол
  otlp:
    protocols:
      # gRPC протокол (быстрее, бинарный)
      grpc:
        # Endpoint для прослушивания
        # 0.0.0.0 = все интерфейсы, 4317 = стандартный порт OTLP gRPC
        endpoint: 0.0.0.0:4317
      
      # HTTP протокол (проще для отладки)
      http:
        # 4318 = стандартный порт OTLP HTTP
        endpoint: 0.0.0.0:4318

# ============================================
# Processors - обработчики данных
# ============================================
processors:
  # Batch processor - группировка данных для эффективности
  batch:
    # Размер батча для отправки
    # Опции: 1000-50000, больше = эффективнее, но больше задержка
    send_batch_size: 10000
    
    # Максимальный размер батча
    # Должен быть больше send_batch_size на 10%
    send_batch_max_size: 11000
    
    # Таймаут отправки батча
    # Опции: 1s-60s, меньше = меньше задержка, больше нагрузка
    timeout: 10s

# ============================================
# Exporters - экспортеры данных
# ============================================
exporters:
  # ClickHouse exporter для SigNoz
  clickhouse:
    # Endpoint ClickHouse
    # tcp:// = нативный протокол (быстрее чем HTTP)
    # 9000 = стандартный порт ClickHouse
    endpoint: tcp://clickhouse:9000?database=signoz
    
    # TTL (Time To Live) - время хранения данных
    # Опции: 24h, 72h, 168h (неделя), 720h (месяц)
    # После TTL данные автоматически удаляются
    ttl: 72h
    
    # Имена таблиц в ClickHouse
    # Можно изменить для кастомных схем
    traces_table_name: signoz_traces
    metrics_table_name: signoz_metrics
    logs_table_name: signoz_logs

# ============================================
# Service - конфигурация пайплайнов
# ============================================
service:
  # Pipelines - цепочки обработки данных
  pipelines:
    # Pipeline для трейсов (traces)
    traces:
      # Откуда получать данные
      receivers: [otlp]
      # Как обрабатывать (порядок важен!)
      processors: [batch]
      # Куда отправлять
      exporters: [clickhouse]
    
    # Pipeline для метрик (metrics)
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [clickhouse]
    
    # Pipeline для логов (logs)
    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [clickhouse]
```

### Запуск SigNoz

```bash
# Запуск всех сервисов
podman-compose up -d

# Проверка статуса
podman-compose ps

# Просмотр логов
podman-compose logs -f

# Открыть UI в браузере
# http://localhost:3301
```

---

## 9.3. Интеграция с Node.js приложением

### Установка OpenTelemetry SDK

```bash
cd ~/fastify-api

# Установка OTel пакетов
npm install --save \
  @opentelemetry/sdk-node \
  @opentelemetry/auto-instrumentations-node \
  @opentelemetry/exporter-trace-otlp-http \
  @opentelemetry/exporter-metrics-otlp-http
```

### Создание tracing.ts

**Документация:** https://opentelemetry.io/docs/languages/js/getting-started/nodejs/

```bash
nano src/tracing.ts
```

Содержимое:

```typescript
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';
import { OTLPTraceExporter } from '@opentelemetry/exporter-trace-otlp-http';
import { OTLPMetricExporter } from '@opentelemetry/exporter-metrics-otlp-http';
import { PeriodicExportingMetricReader } from '@opentelemetry/sdk-metrics';

const traceExporter = new OTLPTraceExporter({
  url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT || 'http://localhost:4318/v1/traces',
});

const metricExporter = new OTLPMetricExporter({
  url: process.env.OTEL_EXPORTER_OTLP_ENDPOINT || 'http://localhost:4318/v1/metrics',
});

const sdk = new NodeSDK({
  traceExporter,
  metricReader: new PeriodicExportingMetricReader({
    exporter: metricExporter,
    exportIntervalMillis: 60000,
  }),
  instrumentations: [
    getNodeAutoInstrumentations({
      '@opentelemetry/instrumentation-fs': {
        enabled: false,
      },
    }),
  ],
  serviceName: process.env.SERVICE_NAME || 'fastify-api',
});

sdk.start();

process.on('SIGTERM', () => {
  sdk.shutdown()
    .then(() => console.log('Tracing terminated'))
    .catch((error) => console.log('Error terminating tracing', error))
    .finally(() => process.exit(0));
});

export default sdk;
```

### Обновление server.ts

```typescript
// Импорт tracing ПЕРВЫМ!
import './tracing.js';

import Fastify from 'fastify';
import { prisma } from './db.js';

const fastify = Fastify({ logger: true });

// Ваши routes...

const start = async () => {
  try {
    await fastify.listen({ port: 3001, host: '0.0.0.0' });
    console.log('Server with OpenTelemetry is running');
  } catch (err) {
    fastify.log.error(err);
    process.exit(1);
  }
};

start();
```

### Обновление Dockerfile

```dockerfile
FROM docker.io/library/node:26-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM docker.io/library/node:26-alpine

RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force

COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist

USER nodejs

EXPOSE 3001

# Переменные для OpenTelemetry
ENV OTEL_EXPORTER_OTLP_ENDPOINT=http://signoz-otel-collector:4318
ENV SERVICE_NAME=fastify-api

CMD ["node", "dist/server.js"]
```

### Docker Compose с SigNoz

**Документация:** https://signoz.io/docs/install/docker/

```bash
nano docker-compose.fullstack.yaml
```

Содержимое:

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:18-alpine
    environment:
      POSTGRES_USER: developer
      POSTGRES_PASSWORD: secret123
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U developer"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - app-network

  api:
    build: ./fastify-api
    environment:
      DATABASE_URL: postgresql://developer:secret123@postgres:5432/myapp
      OTEL_EXPORTER_OTLP_ENDPOINT: http://otel-collector:4318
      SERVICE_NAME: fastify-api
    ports:
      - "3001:3001"
    depends_on:
      postgres:
        condition: service_healthy
      otel-collector:
        condition: service_started
    networks:
      - app-network

  frontend:
    build: ./react-app
    ports:
      - "8080:80"
    depends_on:
      - api
    networks:
      - app-network

  # SigNoz services
  clickhouse:
    image: clickhouse/clickhouse-server:25.9-alpine
    volumes:
      - clickhouse-data:/var/lib/clickhouse/
    networks:
      - app-network

  otel-collector:
    image: signoz/signoz-otel-collector:0.96.1
    command: ["--config=/etc/otel-collector-config.yaml"]
    volumes:
      - ./otel-collector-config.yaml:/etc/otel-collector-config.yaml
    ports:
      - "4317:4317"
      - "4318:4318"
    depends_on:
      - clickhouse
    networks:
      - app-network

  query-service:
    image: signoz/query-service:0.96.1
    environment:
      - ClickHouseUrl=tcp://clickhouse:9000
    ports:
      - "8080:8080"
    depends_on:
      - clickhouse
    networks:
      - app-network

  frontend-signoz:
    image: signoz/frontend:0.96.1
    ports:
      - "3301:3301"
    depends_on:
      - query-service
    networks:
      - app-network

networks:
  app-network:

volumes:
  pgdata:
  clickhouse-data:
```

### Запуск и тестирование

```bash
# Запуск всего стека
podman-compose -f docker-compose.fullstack.yaml up -d

# Генерация трафика для трейсов
for i in {1..100}; do
  curl http://localhost:3001/users
  curl http://localhost:3001/health
  sleep 0.1
done

# Открыть SigNoz UI
# http://localhost:3301
```

---

## 9.4. Настройка дашбордов и алертов

### Создание custom метрик

```typescript
// src/metrics.ts
import { metrics } from '@opentelemetry/api';

const meter = metrics.getMeter('fastify-api');

export const requestCounter = meter.createCounter('http_requests_total', {
  description: 'Total number of HTTP requests',
});

export const requestDuration = meter.createHistogram('http_request_duration_ms', {
  description: 'HTTP request duration in milliseconds',
});

export const activeUsers = meter.createUpDownCounter('active_users', {
  description: 'Number of active users',
});
```

Использование в server.ts:

```typescript
import { requestCounter, requestDuration } from './metrics.js';

fastify.addHook('onRequest', async (request, reply) => {
  request.startTime = Date.now();
});

fastify.addHook('onResponse', async (request, reply) => {
  const duration = Date.now() - request.startTime;
  
  requestCounter.add(1, {
    method: request.method,
    route: request.routerPath,
    status: reply.statusCode,
  });
  
  requestDuration.record(duration, {
    method: request.method,
    route: request.routerPath,
  });
});
```

### Настройка алертов в SigNoz

1. Откройте SigNoz UI: http://localhost:3301
2. Перейдите в "Alerts"
3. Создайте новый алерт:

```yaml
name: High Error Rate
description: Alert when error rate exceeds 5%
query: |
  SELECT
    count(*) as total,
    countIf(status_code >= 400) as errors,
    (errors / total) * 100 as error_rate
  FROM signoz_traces
  WHERE timestamp >= now() - INTERVAL 5 MINUTE
  HAVING error_rate > 5
threshold: 5
severity: critical
```

---

## 9.5. Анализ трейсов и метрик

### Просмотр трейсов

1. Откройте "Traces" в SigNoz UI
2. Фильтры:
   - Service: fastify-api
   - Operation: GET /users
   - Duration: > 100ms
3. Анализ:
   - Waterfall view
   - Span details
   - Tags и attributes

### Анализ метрик

1. Откройте "Metrics" в SigNoz UI
2. Создайте дашборд:
   - Request rate (RPS)
   - Error rate (%)
   - P95/P99 latency
   - Active connections

### Query примеры

```sql
-- Top 10 медленных запросов
SELECT
  name,
  quantile(0.95)(duration) as p95_duration
FROM signoz_traces
WHERE service_name = 'fastify-api'
  AND timestamp >= now() - INTERVAL 1 HOUR
GROUP BY name
ORDER BY p95_duration DESC
LIMIT 10;

-- Error rate по эндпоинтам
SELECT
  http_route,
  countIf(status_code >= 400) / count(*) * 100 as error_rate
FROM signoz_traces
WHERE service_name = 'fastify-api'
  AND timestamp >= now() - INTERVAL 1 HOUR
GROUP BY http_route
ORDER BY error_rate DESC;
```

---

**Практическое задание:**

1. Разверните SigNoz stack
2. Интегрируйте OpenTelemetry в Node.js API
3. Создайте custom метрики
4. Настройте дашборды
5. Создайте алерты
6. Проанализируйте трейсы медленных запросов

**Проверка знаний:**

```bash
# Работает ли SigNoz?
curl http://localhost:3301

# Принимает ли OTel Collector данные?
curl http://localhost:4318/v1/traces

# Сколько трейсов собрано?
# Проверьте в SigNoz UI

# Какой P95 latency у API?
# Проверьте в Metrics
```
