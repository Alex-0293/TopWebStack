# 🚀 TOP WEB STACK
## Современный стек веб технологий. Контейнерная разработка и деплой на AlmaLinux 9

## Предисловие

Этот курс предназначен для разработчиков, которые хотят освоить современный стек контейнерной разработки на базе enterprise-ready технологий. Вы научитесь разворачивать, настраивать и интегрировать компоненты production-ready инфраструктуры.

**Целевая аудитория:**
- Backend-разработчики с опытом Node.js
- DevOps-инженеры, переходящие на Podman
- Системные администраторы, изучающие контейнеризацию
- Fullstack-разработчики, работающие с микросервисами

**Что вы получите:**
- Практические навыки работы с Podman 5.4
- Опыт развертывания Node.js 26 LTS приложений
- Настройку PostgreSQL 18 в контейнерах
- Интеграцию мониторинга через SigNoz
- Централизованную аутентификацию с Zitadel IAM
- Готовую инфраструктуру для production

**Требования:**
- macOS 16+ с установленным VSCode
- Базовые знания Linux и командной строки
- Понимание HTTP/REST API
- Опыт работы с Git

**Длительность:** 40-50 часов практики

---

## Содержание

### [Глава 1. Инфраструктура и планирование](chapter-01-infrastructure.md)
- 1.1. Установка VSCode на macOS
- 1.2. Установка необходимых расширений VSCode
- 1.3. Настройка SSH-доступа к AlmaLinux серверу
- 1.4. Подключение VSCode к удаленному серверу
- 1.5. Настройка окружения на удаленном сервере
- 1.6. Создание структуры проекта на сервере
- 1.7. Настройка Dev Container для разработки
- 1.8. Настройка VSCode для продуктивной работы
- 1.9. Первый коммит

### [Глава 2. Настройка рабочего места разработчика](chapter-02-setup.md)
- 2.1. Что такое AlmaLinux
- 2.2. Установка и первичная настройка
- 2.3. Управление пакетами через DNF
- 2.4. Firewalld и сетевая безопасность
- 2.5. Systemd: управление сервисами
- 2.6. Cockpit: веб-интерфейс для управления сервером

### [Глава 3. AlmaLinux и основы системы](chapter-03-almalinux.md)
- 3.1. Что такое Podman и чем он отличается от Docker
- 3.2. Установка Podman 5.4
- 3.3. Основные команды Podman
- 3.4. Работа с образами и контейнерами
- 3.5. Podman Compose: оркестрация контейнеров
- 3.6. Rootless-контейнеры и безопасность

### [Глава 4. Podman: контейнеризация без Docker](chapter-04-podman.md)
- 4.1. Что такое Git и зачем он нужен
- 4.2. Установка и первичная настройка Git
- 4.3. Основы работы с Git
- 4.4. Работа с удаленными репозиториями
  - SSH-ключи для GitHub/GitLab
  - Fine-grained Personal Access Tokens
  - Remote-репозитории и синхронизация
- 4.5. Git Workflow для нашего проекта
- 4.6. .gitignore для нашего стека
- 4.7. Git hooks и автоматизация
- 4.8. Интеграция Git с Komodo
- 4.9. Практические сценарии и troubleshooting
- 4.10. Практическое задание
- 4.11. Проверка знаний и заключение

### [Глава 5. Git: система контроля версий](chapter-05-git.md)
- 5.1. Что такое Komodo
- 5.2. Установка Komodo
- 5.3. Добавление серверов в Komodo
- 5.4. Работа со Stacks (Docker Compose)
- 5.5. Deployments: Git-based деплой
- 5.6. Builds: сборка Docker образов
- 5.7. Actions и Procedures: автоматизация
- 5.8. Alerts и мониторинг

### [Глава 6. Komodo: управление инфраструктурой и деплоем](chapter-06-komodo.md)
- 6.1. Node.js 26 LTS: что нового
- 6.2. Установка Node.js через NVM
- 6.3. Создание простого API на Fastify
- 6.4. Контейнеризация Node.js приложения
- 6.5. Multi-stage builds для оптимизации

### [Глава 7. Node.js: современная разработка](chapter-07-nodejs.md)
- 7.1. Vite: зачем нужен и как работает
- 7.2. Создание React-приложения с Vite
- 7.3. Контейнеризация фронтенда
- 7.4. Nginx для production

### [Глава 8. Vite: современный фронтенд](chapter-08-vite.md)
- 8.1. PostgreSQL 18: новые возможности
- 8.2. Развертывание PostgreSQL в контейнере
- 8.3. Инициализация базы данных
- 8.4. Подключение из Node.js через Prisma
- 8.5. Backup и восстановление

### [Глава 9. PostgreSQL: надежное хранение данных](chapter-09-postgresql.md)
- 9.1. Что такое SigNoz и OpenTelemetry
- 9.2. Развертывание SigNoz stack
- 9.3. Интеграция с Node.js приложением
- 9.4. Настройка дашбордов и алертов
- 9.5. Анализ трейсов и метрик

### [Глава 10. SigNoz: мониторинг и observability](chapter-10-signoz.md)
- 10.1. Zitadel: современный IAM
- 10.2. Развертывание Zitadel
- 10.3. Настройка организации и проектов
- 10.4. OIDC/OAuth2 интеграция
- 10.5. Подключение к Node.js через Lucia Auth

### [Глава 11. Zitadel IAM: централизованная аутентификация](chapter-11-zitadel.md)
- 11.1. Zitadel: современный IAM
- 11.2. Развертывание Zitadel
- 11.3. Настройка организации и проектов
- 11.4. OIDC/OAuth2 интеграция
- 11.5. Подключение к Node.js через Lucia Auth

### [Глава 12. Интеграция всех компонентов](chapter-12-integration.md)
- 12.1. Архитектура финального решения
- 12.2. Podman Compose для всего стека
- 12.3. Сетевое взаимодействие контейнеров
- 12.4. Управление секретами
- 12.5. CI/CD с GitLab

### [Глава 13. Production-ready практики](chapter-13-production.md)
- 13.1. Логирование и ротация логов
- 13.2. Health checks и мониторинг
- 13.3. Backup-стратегия
- 13.4. Обновление компонентов
- 13.5. Troubleshooting типичных проблем


## Заключение

Поздравляем с завершением курса! Теперь у вас есть все необходимые знания для разработки и развертывания современных контейнерных приложений на базе AlmaLinux 9 и Podman 5.

**Что дальше:**

1. **Практикуйтесь** — создайте собственный проект с нуля
2. **Изучайте** — углубляйтесь в интересующие технологии
3. **Делитесь** — помогайте другим разработчикам
4. **Развивайтесь** — следите за обновлениями технологий

**Рекомендуемые следующие шаги:**

- Изучите Kubernetes для оркестрации на больших масштабах
- Освойте Terraform для Infrastructure as Code
- Углубитесь в security best practices
- Изучите advanced observability patterns
- Настройте полноценный CI/CD pipeline

**Полезные ссылки для продолжения обучения:**

- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [CNCF Landscape](https://landscape.cncf.io/)
- [DevOps Roadmap](https://roadmap.sh/devops)
- [SRE Books](https://sre.google/books/)

---

## 📝 Обратная связь

Ваше мнение очень важно для улучшения курса! Поделитесь своими впечатлениями, предложениями или сообщите об ошибках.

<div class="feedback-widget">
  <h4>💬 Помогите улучшить курс TopWebStack</h4>
  <p>Поделитесь своим опытом прохождения курса, предложите улучшения или сообщите о найденных ошибках.</p>
  <div class="feedback-buttons">
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=feedback%2C+positive&template=&title=%F0%9F%91%8D+%D0%9F%D0%BE%D0%BB%D0%B5%D0%B7%D0%BD%D1%8B%D0%B9+%D0%BA%D1%83%D1%80%D1%81&body=**%D0%A7%D1%82%D0%BE+%D0%BF%D0%BE%D0%BD%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%BE%D1%81%D1%8C%3A**%0A%0A**%D0%9F%D1%80%D0%B5%D0%B4%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D1%8F+%D0%BF%D0%BE+%D1%83%D0%BB%D1%83%D1%87%D1%88%D0%B5%D0%BD%D0%B8%D1%8E%3A**%0A%0A" 
       class="feedback-btn positive" target="_blank" rel="noopener">
       👍 Полезный курс
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=bug%2C+needs-review&template=&title=%F0%9F%90%9B+%D0%9E%D1%88%D0%B8%D0%B1%D0%BA%D0%B0+%D0%B2+%D0%BA%D1%83%D1%80%D1%81%D0%B5&body=**%D0%93%D0%BB%D0%B0%D0%B2%D0%B0%2F%D1%80%D0%B0%D0%B7%D0%B4%D0%B5%D0%BB%3A**%0A%0A**%D0%9E%D0%BF%D0%B8%D1%81%D0%B0%D0%BD%D0%B8%D0%B5+%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B8%3A**%0A%0A**%D0%9E%D0%B6%D0%B8%D0%B4%D0%B0%D0%B5%D0%BC%D0%BE%D0%B5+%D0%BF%D0%BE%D0%B2%D0%B5%D0%B4%D0%B5%D0%BD%D0%B8%D0%B5%3A**%0A%0A" 
       class="feedback-btn negative" target="_blank" rel="noopener">
       🐛 Нашел ошибку
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/issues/new?assignees=&labels=enhancement%2C+suggestion&template=&title=%F0%9F%92%A1+%D0%9F%D1%80%D0%B5%D0%B4%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5&body=**%D0%9F%D1%80%D0%B5%D0%B4%D0%BB%D0%BE%D0%B6%D0%B5%D0%BD%D0%B8%D0%B5%3A**%0A%0A**%D0%9E%D0%B1%D0%BE%D1%81%D0%BD%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5%3A**%0A%0A**%D0%94%D0%BE%D0%BF%D0%BE%D0%BB%D0%BD%D0%B8%D1%82%D0%B5%D0%BB%D1%8C%D0%BD%D1%8B%D0%B5+%D0%B4%D0%B5%D1%82%D0%B0%D0%BB%D0%B8%3A**%0A%0A" 
       class="feedback-btn suggestion" target="_blank" rel="noopener">
       💡 Есть идея
    </a>
    <a href="https://github.com/Alex-0293/TopWebStack/discussions" 
       class="feedback-btn github" target="_blank" rel="noopener">
       💬 Обсуждения
    </a>
  </div>
  <p style="margin-top: 1rem; font-size: 0.85rem; color: #666;">
    ⭐ Поставьте звезду на <a href="https://github.com/Alex-0293/TopWebStack" target="_blank" rel="noopener">GitHub</a>, если курс был полезен!
  </p>
</div>

---

**Успехов в разработке!** 🚀

---

**Версия курса:** 1.1  
**Дата публикации:** Октябрь 2025  
**Обратная связь:** Создайте issue в репозитории курса
