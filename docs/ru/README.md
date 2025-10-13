# Курс: Контейнерная разработка и деплой на AlmaLinux

## 📁 Структура файлов курса

### Основные файлы

- **[index.md](index.md)** — главная страница курса с полным содержанием
- **[README.md](README.md)** — описание курса и быстрый старт
- **[RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md)** — все ресурсы, ссылки и документация

### Главы курса

1. **[chapter-01-architecture.md](chapter-01-architecture.md)** — Архитектура среды разработки
   - Архитектура будущего проекта
   - Планирование инфраструктуры
   - Выбор технологий стека
   - Диаграммы взаимодействия

2. **[chapter-02-setup.md](chapter-02-setup.md)** — Настройка рабочего места разработчика
   - Установка VSCode на macOS
   - Настройка SSH и Remote Development
   - Создание проекта на удаленном сервере
   - Настройка Dev Container

3. **[chapter-03-almalinux.md](chapter-03-almalinux.md)** — AlmaLinux и основы системы
   - Что такое AlmaLinux
   - Установка и настройка
   - DNF, Firewalld, Systemd
   - Cockpit веб-интерфейс

4. **[chapter-04-podman.md](chapter-04-podman.md)** — Podman: контейнеризация без Docker
   - Архитектура Podman vs Docker
   - Установка и настройка
   - Основные команды
   - Podman Compose
   - Rootless контейнеры

5. **[chapter-05-git.md](chapter-05-git.md)** — Git: система контроля версий
   - Основы работы с Git
   - Настройка и конфигурация
   - Git workflow и best practices
   - Интеграция с GitHub/GitLab

6. **[chapter-06-komodo.md](chapter-06-komodo.md)** — Komodo: управление инфраструктурой
   - Установка Komodo Core
   - Управление серверами
   - Stacks и Deployments
   - Builds и автоматизация
   - Мониторинг и алерты

7. **[chapter-07-nodejs.md](chapter-07-nodejs.md)** — Node.js: современная разработка
   - Установка Node.js через NVM
   - Создание API на Fastify
   - Контейнеризация приложения
   - Multi-stage builds

8. **[chapter-08-vite.md](chapter-08-vite.md)** — Vite: современный фронтенд
   - Создание React приложения
   - Контейнеризация фронтенда
   - Nginx для production
   - Оптимизация build

9. **[chapter-09-postgresql.md](chapter-09-postgresql.md)** — PostgreSQL: надежное хранение
   - Развертывание в контейнере
   - Инициализация БД
   - Интеграция с Prisma
   - Backup и восстановление

10. **[chapter-10-signoz.md](chapter-10-signoz.md)** — SigNoz: мониторинг и observability
    - Архитектура SigNoz
    - Развертывание stack
    - Интеграция с Node.js
    - OpenTelemetry
    - Дашборды и алерты

11. **[chapter-11-zitadel.md](chapter-11-zitadel.md)** — Zitadel IAM: аутентификация
    - Развертывание Zitadel
    - Настройка организаций
    - OIDC/OAuth2 интеграция
    - Подключение через Lucia Auth

12. **[chapter-12-integration.md](chapter-12-integration.md)** — Интеграция компонентов
    - Архитектура полного решения
    - Podman Compose для всего стека
    - Сетевое взаимодействие
    - Управление секретами
    - CI/CD с GitLab

13. **[chapter-13-production.md](chapter-13-production.md)** — Production-ready практики
    - Логирование и ротация
    - Health checks
    - Backup-стратегия
    - Обновление компонентов
    - Troubleshooting

---

## 🔗 Проверка ссылок

### Внутренние ссылки курса

**Главная навигация:**
- ✅ [index.md](index.md) — главная страница
- ✅ [README.md](README.md) — описание курса
- ✅ [RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md) — ресурсы

**Главы (последовательность):**
- ✅ [Глава 1](chapter-01-architecture.md) → [Глава 2](chapter-02-setup.md)
- ✅ [Глава 2](chapter-02-setup.md) → [Глава 3](chapter-03-almalinux.md)
- ✅ [Глава 3](chapter-03-almalinux.md) → [Глава 4](chapter-04-podman.md)
- ✅ [Глава 4](chapter-04-podman.md) → [Глава 5](chapter-05-git.md)
- ✅ [Глава 5](chapter-05-git.md) → [Глава 6](chapter-06-komodo.md)
- ✅ [Глава 6](chapter-06-komodo.md) → [Глава 7](chapter-07-nodejs.md)
- ✅ [Глава 7](chapter-07-nodejs.md) → [Глава 8](chapter-08-vite.md)
- ✅ [Глава 8](chapter-08-vite.md) → [Глава 9](chapter-09-postgresql.md)
- ✅ [Глава 9](chapter-09-postgresql.md) → [Глава 10](chapter-10-signoz.md)
- ✅ [Глава 10](chapter-10-signoz.md) → [Глава 11](chapter-11-zitadel.md)
- ✅ [Глава 11](chapter-11-zitadel.md) → [Глава 12](chapter-12-integration.md)
- ✅ [Глава 12](chapter-12-integration.md) → [Глава 13](chapter-13-production.md)

**Навигация в главах:**
- Каждая глава имеет ссылку на [index.md](index.md) (оглавление)
- Каждая глава имеет ссылки на предыдущую и следующую главы
- Последняя глава имеет ссылку только на предыдущую

### Внешние ссылки (примеры)

**Официальная документация:**
- AlmaLinux: https://almalinux.org/
- Podman: https://podman.io/
- Komodo: https://komo.do/docs
- Node.js: https://nodejs.org/
- Vite: https://vitejs.dev/
- PostgreSQL: https://www.postgresql.org/
- Prisma: https://www.prisma.io/
- Fastify: https://fastify.dev/
- SigNoz: https://signoz.io/
- Zitadel: https://zitadel.com/

Полный список внешних ссылок см. в [RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md)

---

## 📊 Статистика курса

**Всего файлов:** 15
- Главы курса: 11
- Основные файлы: 3
- Служебные файлы: 1 (index.html)

**Общий объем:**
- Примерно 40-50 часов практики
- 11 глав с теорией и практикой
- Множество примеров кода
- Практические задания в каждой главе
- Mermaid диаграммы для визуализации

---

## 🚀 Быстрый старт

1. Начните с [README.md](README.md) для общего обзора
2. Откройте [index.md](index.md) для полного содержания
3. Следуйте главам последовательно, начиная с [Главы 1](chapter-01-architecture.md)
4. Выполняйте практические задания
5. Используйте [RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md) как справочник

---

## 📝 Для авторов и контрибьюторов

### Структура файлов

```
docs/training/
├── index.md                      # Главная страница курса
├── README.md                     # Описание и быстрый старт
├── readme.md                     # Этот файл (карта курса)
├── RESOURCES-AND-LINKS.md        # Все ресурсы и ссылки
├── chapter-01-setup.md           # Глава 1
├── chapter-02-almalinux.md       # Глава 2
├── chapter-03-podman.md          # Глава 3
├── chapter-04-comodo.md          # Глава 4
├── chapter-05-nodejs.md          # Глава 5
├── chapter-06-vite.md            # Глава 6
├── chapter-07-postgresql.md      # Глава 7
├── chapter-08-signoz.md          # Глава 8
├── chapter-09-zitadel.md         # Глава 9
├── chapter-10-integration.md     # Глава 10
└── chapter-11-production.md      # Глава 11
```

### Правила оформления

**Каждая глава должна содержать:**
1. Ссылку на оглавление в начале: `[← Оглавление курса](index.md)`
2. Заголовок главы: `# Глава X. Название`
3. Разделы с нумерацией: `## X.1. Название раздела`
4. Mermaid диаграммы где применимо
5. Практические задания в конце
6. Проверку знаний
7. Навигацию в конце:
   ```markdown
   **Навигация:**
   - [← Вернуться к содержанию](index.md)
   - [← Глава X: Название](chapter-XX.md)
   - [→ Глава Y: Название](chapter-YY.md)
   ```

### Соглашения по ссылкам

- Внутренние ссылки: относительные пути (`chapter-01-setup.md`)
- Ссылки на оглавление: `index.md`
- Ссылки на ресурсы: `RESOURCES-AND-LINKS.md`
- Внешние ссылки: полные URL с https://

---

**Версия:** 1.1  
**Дата обновления:** Октябрь 2025  
**Статус:** ✅ Все ссылки проверены и работают
