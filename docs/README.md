# 🚀 TOP WEB STACK
## Современный стек веб технологий. Контейнерная разработка и деплой на AlmaLinux 9

**🇷🇺 Русская версия** | **[🇬🇧 English version](https://alex-0293.github.io/TopWebStack/#/en/)** | **[🇪🇸 Versión en Español](https://alex-0293.github.io/TopWebStack/#/es/)**

---

## 📖 О курсе

Этот курс предоставляет практическое обучение современной веб-разработке с использованием enterprise-ready технологий и контейнеризации.

### 🎯 Что изучается:

- **🐧 AlmaLinux 9** — Enterprise-ready дистрибутив Linux
- **📦 Podman 5.4** — Контейнеризация без Docker
- **🚀 Node.js 26 LTS + Fastify** — Быстрый backend-фреймворк
- **⚡ Vite 6 + React** — Молниеносная сборка фронтенда
- **🗄️ PostgreSQL 18 + Prisma** — Современная ORM и SQL-база
- **📊 SigNoz + OpenTelemetry** — Мониторинг и трассировка
- **🔐 Zitadel IAM** — Управление пользователями и аутентификацией
- **🧩 Komodo** — Управление инфраструктурой и деплоем

### � Структура курса:

Комплексный курс включает 13 глав, охватывающих всё от настройки инфраструктуры до production деплоя.

---

## 📁 Структура проекта

### Мультиязычность

Курс доступен на трех языках по стандарту Docsify:

```
docs/
├── index.html           # Единая точка входа
├── _coverpage.md        # Русский coverpage
├── _sidebar.md          # Русское меню
├── index.md             # Русская главная
├── README.md            # Этот файл
├── chapter-*.md         # Русские главы (по умолчанию)
│
├── en/                  # 🇬🇧 English version
│   ├── _coverpage.md
│   ├── _sidebar.md
│   ├── index.md
│   └── README.md
│
└── es/                  # 🇪🇸 Versión en Español  
    ├── _coverpage.md
    ├── _sidebar.md
    ├── index.md
    └── README.md
```

### Основные файлы

- **[index.md](index.md)** — главная страница курса с полным содержанием
- **[README.md](README.md)** — описание курса и быстрый старт (этот файл)
- **[RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md)** — все ресурсы, ссылки и документация
- **[_sidebar.md](_sidebar.md)** — боковое меню навигации
- **[_coverpage.md](_coverpage.md)** — обложка курса

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

## 🌍 Мультиязычность

Курс поддерживает три языка через стандартный механизм Docsify:

### Структура URL:

```
/                          → Русская версия (по умолчанию)
/en/                       → English version
/es/                       → Versión en Español

/chapter-01-architecture   → Русская глава 1
/en/chapter-01-architecture → English Chapter 1 (fallback на русский если не переведено)
/es/chapter-01-architecture → Capítulo español 1 (fallback на русский если не переведено)
```

### Fallback система:

Если страница не найдена в выбранном языке, автоматически загружается русская версия (язык по умолчанию).

**Конфигурация:**
```javascript
fallbackLanguages: ['en', 'es']
```

---


## 📊 Статистика курса

**Всего файлов основной документации:** 18
- Главы курса: 13
- Основные файлы: 3 (index.md, README.md, RESOURCES-AND-LINKS.md)
- Служебные файлы: 2 (index.html, _sidebar.md, _coverpage.md)

**Мультиязычность:**
- Языков: 3 (русский, английский, испанский)
- Структура по стандарту Docsify
- Fallback на русский язык

**Общий объем:**
- Примерно 40-50 часов практики
- 13 глав с теорией и практикой
- Множество примеров кода
- Практические задания в каждой главе
- Mermaid диаграммы для визуализации

---

## 🚀 Быстрый старт

### Для изучающих курс:

1. **Выберите язык:**
   - [🇷🇺 Русский](/) (по умолчанию)
   - [🇬🇧 English](/en/)
   - [🇪🇸 Español](/es/)

2. **Начните изучение:**
   - Откройте [index.md](index.md) для полного содержания
   - Или начните сразу с [Главы 1](chapter-01-architecture.md)

3. **Следуйте курсу:**
   - Изучайте главы последовательно
   - Выполняйте практические задания
   - Используйте [RESOURCES-AND-LINKS.md](RESOURCES-AND-LINKS.md) как справочник

4. **Переключение языков:**
   - Используйте переключатель 🌐 в правом верхнем углу
   - Язык сохраняется автоматически
   - При отсутствии перевода показывается русская версия

### Для контрибьюторов:

**Структура файлов для добавления перевода:**

```bash
# Для добавления нового языка (например, немецкого):
docs/
└── de/                    # Создать папку с кодом языка
    ├── _coverpage.md      # Перевести обложку
    ├── _sidebar.md        # Перевести меню
    ├── index.md           # Перевести главную
    └── README.md          # Перевести описание
```

**Затем обновить [`docs/index.html`](docs/index.html ):**
1. Добавить в `fallbackLanguages: ['en', 'es', 'de']`
2. Добавить в `nameLink`
3. Добавить опцию в Language Dropdown
4. Обновить логику `detectAndSetLanguage()`

---

## 📝 Правила оформления

### Каждая глава должна содержать:

1. **Навигационная ссылка в начале:**
   - [→ Глава Y: Название](chapter-YY.md)
   ```

### Соглашения по ссылкам:

- **Внутренние ссылки:** относительные пути (`chapter-01-architecture.md`)
- **Ссылки на оглавление:** `index.md`
- **Ссылки на ресурсы:** `RESOURCES-AND-LINKS.md`
- **Внешние ссылки:** полные URL с https://
- **Кросс-языковые ссылки:** `../chapter-XX.md` (fallback на русский)

---

## 🛠️ Технологии документации

**Генератор сайта:**
- [Docsify](https://docsify.js.org/) — генератор документации без сборки

**Стилизация:**
- Docsify Themeable
- Кастомные CSS стили
- Темная/светлая тема

**Функциональность:**
- Мультиязычность (fallbackLanguages)
- Поиск по документации
- Копирование кода
- Zoom изображений
- Mermaid диаграммы
- Pagination
- Виджет обратной связи

**Хостинг:**
- GitHub Pages
- Автоматический деплой из ветки dev

---

## 📞 Обратная связь

### Способы связи:

1. **Issues на GitHub:**
   - [🐛 Сообщить об ошибке](https://github.com/Alex-0293/TopWebStack/issues/new?labels=bug)
   - [💡 Предложить улучшение](https://github.com/Alex-0293/TopWebStack/issues/new?labels=enhancement)
   - [👍 Оставить отзыв](https://github.com/Alex-0293/TopWebStack/issues/new?labels=feedback)

2. **Discussions:**
   - [💬 Обсуждения](https://github.com/Alex-0293/TopWebStack/discussions)

3. **Pull Requests:**
   - Форкните репозиторий
   - Внесите изменения
   - Создайте PR

### Помощь проекту:

- ⭐ Поставьте звезду на [GitHub](https://github.com/Alex-0293/TopWebStack)
- 🌍 Помогите с переводом на другие языки
- 📝 Улучшите документацию
- 🐛 Сообщайте об ошибках
- 💬 Делитесь опытом в Discussions

---

**Версия курса:** 1.1  
**Дата обновления:** Октябрь 2025  
**Статус:** ✅ Активная разработка  
**Мультиязычность:** 🇷🇺 🇬🇧 🇪🇸

**[→ Начать обучение](index.md)** | **[→ Список ресурсов](RESOURCES-AND-LINKS.md)**


## 📝 Правила оформления глав

### Каждая глава должна содержать:

1. Навигационную ссылку в начале на оглавление
2. Заголовок главы с номером
3. Разделы с нумерацией
4. Mermaid диаграммы (где применимо)
5. Практические задания в конце
6. Проверку знаний
7. Навигацию в конце (предыдущая/следующая глава)

### Соглашения по ссылкам:

- **Внутренние ссылки:** относительные пути
- **Ссылки на оглавление:** `index.md`
- **Ссылки на ресурсы:** `RESOURCES-AND-LINKS.md`
- **Внешние ссылки:** полные URL с https://

---

**Версия:** 1.2  
**Дата обновления:** Октябрь 2025  
**Статус:** ✅ Поддержка 3 языков (🇷🇺/🇬🇧/🇪��)
