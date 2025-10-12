[← Оглавление курса](index.md)

---

# Глава 4. Git: система контроля версий

## 4.1. Что такое Git и зачем он нужен

**Git** — распределенная система управления версиями кода, которая позволяет отслеживать изменения в файлах и координировать работу между разработчиками.

**Ключевые особенности:**
- **Распределенная** — каждый клон — полная копия репозитория
- **Быстрая** — оптимизирована для скорости операций
- **Надежная** — целостность данных гарантируется хешами
- **Гибкая** — поддержка различных workflow
- **Локальная работа** — большинство операций не требуют сети

**Почему Git важен в контексте нашего стека:**

```mermaid
flowchart LR
    subgraph dev_env["� Среда разработки"]
        developer["👨‍💻<br/>Разработчик"]
        vscode["�<br/>VS Code"]
        local_git["📁<br/>Локальный Git"]
    end
    
    subgraph cloud["☁️ Облачные сервисы"]
        github["�<br/>GitHub<br/>Repository"]
        actions["⚡<br/>GitHub<br/>Actions"]
    end
    
    subgraph server_env["🖥️ Серверная среда"]
        komodo["🎛️<br/>Komodo<br/>Dashboard"]
        almalinux["�<br/>AlmaLinux<br/>Server"]
        podman["🐳<br/>Podman<br/>Containers"]
    end
    
    developer --> vscode
    vscode --> local_git
    local_git --> |"git push"| github
    github --> |"webhook"| komodo
    github --> |"CI/CD"| actions
    actions --> |"deploy"| komodo
    komodo --> |"container mgmt"| almalinux
    almalinux --> podman
    
    style dev_env fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style cloud fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    style server_env fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    style github fill:#24292e,color:#fff
    style komodo fill:#4caf50,color:#fff
    style podman fill:#892ca0,color:#fff
```

## 4.2. Установка и первичная настройка Git

### Проверка установки Git

На AlmaLinux 9 Git обычно уже установлен:

```bash
git --version
```

Если Git не установлен:

```bash
sudo dnf install git -y
```

### Базовая конфигурация

```bash
# Глобальная конфигурация пользователя
git config --global user.name "Ваше Имя"
git config --global user.email "your.email@example.com"

# Настройка редактора по умолчанию
git config --global core.editor "nano"

# Настройка поведения при пуше
git config --global push.default simple

# Цветной вывод
git config --global color.ui auto

# Автокоррекция команд
git config --global help.autocorrect 1

# Кеширование учетных данных
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'
```

### Проверка конфигурации

```bash
git config --list
```

## 4.3. Основы работы с Git

### Создание нового репозитория

```bash
# Инициализация нового репозитория
mkdir my-project
cd my-project
git init

# Создание первого файла
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit"
```

### Основные команды Git

**Жизненный цикл файлов в Git:**

```mermaid
flowchart TB
    subgraph workspace["📁 Рабочая область"]
        untracked["📄 Новые файлы<br/>(Untracked)"]
        modified["✏️ Измененные файлы<br/>(Modified)"]
    end
    
    subgraph staging["📋 Область подготовки"]
        staged["🎯 Подготовленные<br/>(Staged)"]
    end
    
    subgraph repository["💾 Репозиторий"]
        committed["✅ Зафиксированные<br/>(Committed)"]
    end
    
    untracked --> |"git add"| staged
    modified --> |"git add"| staged
    staged --> |"git commit -m 'message'"| committed
    committed --> |"редактирование"| modified
    
    style workspace fill:#fff3e0,stroke:#ff9800,stroke-width:2px
    style staging fill:#e8f5e8,stroke:#4caf50,stroke-width:2px
    style repository fill:#e3f2fd,stroke:#2196f3,stroke-width:2px
    
    style untracked fill:#ffcdd2,stroke:#f44336
    style modified fill:#fff9c4,stroke:#ffc107
    style staged fill:#c8e6c9,stroke:#4caf50
    style committed fill:#bbdefb,stroke:#2196f3
```

**Основные команды:**

```bash
# Проверка статуса
git status

# Добавление файлов в staging area
git add filename.txt
git add .                    # все файлы
git add *.js                # по маске

# Коммит изменений
git commit -m "Описание изменений"
git commit -am "Добавить и закоммитить измененные файлы"

# Просмотр истории
git log
git log --oneline
git log --graph --oneline --all

# Просмотр различий
git diff                     # рабочая директория vs staging
git diff --staged           # staging vs последний коммит
git diff HEAD~1             # текущий коммит vs предыдущий
```

### Работа с ветками

```bash
# Создание новой ветки
git branch feature-auth
git checkout -b feature-auth    # создать и переключиться

# Переключение между ветками
git checkout main
git checkout feature-auth

# Слияние веток
git checkout main
git merge feature-auth

# Удаление ветки
git branch -d feature-auth      # безопасное удаление
git branch -D feature-auth      # принудительное удаление

# Просмотр веток
git branch                      # локальные ветки
git branch -a                   # все ветки
```

## 4.4. Работа с удаленными репозиториями

### Настройка SSH-ключей для GitHub/GitLab

```bash
# Генерация SSH-ключа
ssh-keygen -t ed25519 -C "your.email@example.com"
ssh-keygen -t rsa -b 4096 -C "your.email@example.com"  # альтернативный вариант

# Добавление ключа в ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# Просмотр публичного ключа (добавить в GitHub/GitLab)
cat ~/.ssh/id_ed25519.pub
```

### Fine-grained Personal Access Tokens (GitHub)

**Fine-grained Personal Access Tokens** — современный способ аутентификации в GitHub с детальным контролем доступа к конкретным репозиториям и ресурсам.

**Преимущества Fine-grained токенов:**
- **Точный контроль доступа** — права только на нужные репозитории
- **Ограниченное время действия** — от 1 дня до 1 года
- **Детальные разрешения** — read, write для конкретных ресурсов
- **Безопасность** — минимальные необходимые права (принцип least privilege)
- **Аудит** — подробные логи использования

#### Создание Fine-grained токена

**Шаг 1: Переход к настройкам токенов**

1. Войдите в GitHub → **Settings** (правый верхний угол)
2. В боковой панели выберите **Developer settings**
3. Выберите **Personal access tokens** → **Fine-grained tokens**
4. Нажмите **Generate new token**

**Шаг 2: Базовая конфигурация**

```yaml
# Параметры токена
Token name: "TopWebStack-Development"
Expiration: 90 days  # или Custom для точной даты
Description: "Токен для работы с репозиторием TopWebStack курса"
```

**Шаг 3: Выбор ресурсов**

```yaml
Resource owner: Alex-0293  # ваш username или организация
Repository access: Selected repositories
  Selected repositories:
    - Alex-0293/TopWebStack
```

**Шаг 4: Настройка разрешений репозитория**

```yaml
Repository permissions:
  Contents: Read and write          # чтение и запись файлов
  Metadata: Read                    # базовая информация о репозитории
  Pull requests: Read and write     # работа с PR
  Issues: Read and write           # работа с issues
  Actions: Read                    # просмотр GitHub Actions (если используются)
  Commit statuses: Read and write  # статусы коммитов
  Deployments: Read and write      # деплойменты (для Komodo интеграции)
  
# Опционально (если нужно)  
Account permissions:
  Git SSH keys: Read               # если используете SSH
  GPG keys: Read                   # если подписываете коммиты
```

**Шаг 5: Генерация токена**

После создания **обязательно скопируйте токен** — он больше не будет показан!

```bash
# Пример созданного токена
github_pat_11ABCDEFGH_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

#### Применение токена

**Способ 1: Настройка Git credential helper**

```bash
# Настройка глобального credential helper
git config --global credential.helper store

# При первом push Git запросит учетные данные
git push origin main
# Username: ваш-github-username
# Password: github_pat_11ABCDEFGH_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**Способ 2: Использование в URL репозитория**

```bash
# Клонирование с токеном в URL
git clone https://github_pat_11ABCDEFGH_xxxxx@github.com/Alex-0293/TopWebStack.git

# Изменение существующего remote
git remote set-url origin https://github_pat_11ABCDEFGH_xxxxx@github.com/Alex-0293/TopWebStack.git
```

**Способ 3: Переменные окружения**

```bash
# В ~/.bashrc или ~/.zshrc
export GITHUB_TOKEN="github_pat_11ABCDEFGH_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Использование в скриптах
git clone https://$GITHUB_TOKEN@github.com/Alex-0293/TopWebStack.git
```

**Способ 4: GitHub CLI (рекомендуемый)**

```bash
# Установка GitHub CLI
# macOS:
brew install gh
# AlmaLinux:
sudo dnf install gh

# Аутентификация с токеном
gh auth login --with-token < token.txt

# Проверка аутентификации
gh auth status

# Клонирование через gh
gh repo clone Alex-0293/TopWebStack
```

#### Безопасное хранение токенов

**1. Использование Git credential manager**

```bash
# Установка Git Credential Manager (рекомендуется)
# macOS:
brew tap microsoft/git
brew install --cask git-credential-manager-core

# Настройка
git config --global credential.credentialStore keychain
```

**2. Переменные окружения в системе**

```bash
# macOS/Linux - добавить в ~/.bashrc или ~/.zshrc
export GH_TOKEN="ваш_токен_здесь"

# Использование
git push  # Git автоматически найдет токен
```

**3. Файл .env (только для разработки)**

```bash
# Создание .env файла (НЕ коммитить!)
echo "GITHUB_TOKEN=ваш_токен" > .env

# Добавление в .gitignore
echo ".env" >> .gitignore
```

#### Управление токенами

**Просмотр активных токенов:**

1. GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Видны все активные токены, их права и даты истечения

**Обновление токена:**

```bash
# При истечении срока действия
# 1. Создать новый токен (те же права)
# 2. Обновить в Git credential manager
git config --global --unset credential.helper
git config --global credential.helper store
# При следующем push ввести новый токен
```

**Отзыв токена:**

Если токен скомпрометирован:
1. GitHub → Settings → Developer settings → Personal access tokens
2. Найти токен → **Delete** или **Regenerate**
3. Обновить токен во всех местах использования

#### Практический пример

```bash
# Полный workflow с Fine-grained токеном

# 1. Создать токен в GitHub (как описано выше)
# 2. Настроить Git
git config --global user.name "Ваше Имя"
git config --global user.email "your.email@example.com"
git config --global credential.helper store

# 3. Клонировать репозиторий
git clone https://github.com/Alex-0293/TopWebStack.git
cd TopWebStack

# 4. Внести изменения
echo "Новый контент" >> docs/chapter-04-git.md

# 5. Закоммитить и запушить
git add .
git commit -m "feat: добавить информацию о fine-grained токенах"
git push origin main
# При первом пуше Git запросит токен

# 6. Проверить статус
gh auth status  # если используете GitHub CLI
```

### Работа с remote-репозиториями

```bash
# Добавление удаленного репозитория
git remote add origin git@github.com:username/repository.git

# Первый пуш с установкой upstream
git push -u origin main

# Обычный пуш
git push

# Получение изменений
git fetch origin
git pull origin main
git pull                        # если установлен upstream

# Просмотр удаленных репозиториев
git remote -v
```

## 4.5. Git Workflow для нашего проекта

### Рекомендуемый Git Flow

```mermaid
gitgraph
    commit id: "🎯 Initial"
    branch develop
    checkout develop
    commit id: "⚙️ Setup"
    
    branch feature/auth
    checkout feature/auth
    commit id: "🔐 Auth"
    commit id: "🧪 Tests"
    
    checkout develop
    merge feature/auth
    commit id: "🔗 Merge auth"
    
    branch feature/api
    checkout feature/api
    commit id: "🚀 API"
    commit id: "📝 Docs"
    
    checkout develop
    merge feature/api
    commit id: "🔗 Merge API"
    
    checkout main
    merge develop
    commit id: "🎉 v1.0"
    
    checkout develop
    branch hotfix/security
    checkout hotfix/security
    commit id: "🛡️ Security"
    
    checkout main
    merge hotfix/security
    commit id: "🔧 v1.0.1"
    
    checkout develop
    merge hotfix/security
```

### Структура веток

```mermaid
flowchart TD
    subgraph permanent["🏛️ Постоянные ветки"]
        main["🎯 main<br/>(production-ready)"]
        develop["🔧 develop<br/>(интеграция)"]
    end
    
    subgraph temporary["⏱️ Временные ветки"]
        feature["🚀 feature/*<br/>(новые функции)"]
        release["📦 release/*<br/>(подготовка к релизу)"]
        hotfix["🛡️ hotfix/*<br/>(критические исправления)"]
    end
    
    subgraph examples["📝 Примеры"]
        feat_auth["feature/user-auth"]
        feat_api["feature/rest-api"]
        rel_v1["release/v1.0"]
        hot_sec["hotfix/security-patch"]
    end
    
    develop --> feature
    feature --> develop
    develop --> release
    release --> main
    release --> develop
    main --> hotfix
    hotfix --> main
    hotfix --> develop
    
    feature --> feat_auth
    feature --> feat_api
    release --> rel_v1
    hotfix --> hot_sec
    
    style permanent fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    style temporary fill:#fff3e0,stroke:#f57c00,stroke-width:2px
    style examples fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    
    style main fill:#4caf50,color:#fff
    style develop fill:#2196f3,color:#fff
```

**Правила работы с ветками:**

```bash
# Основные ветки
main           # production-ready код, только стабильные релизы
develop        # интеграционная ветка, активная разработка

# Временные ветки (создаются и удаляются)
feature/*      # новые функции: feature/user-authentication
hotfix/*       # критические исправления: hotfix/security-vulnerability
release/*      # подготовка к релизу: release/v1.2.0
```

### Соглашения о коммитах

**Формат сообщений коммитов:**

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Типы коммитов:**
- `feat`: новая функциональность
- `fix`: исправление бага
- `docs`: изменения в документации
- `style`: форматирование, отступы
- `refactor`: рефакторинг кода
- `test`: добавление тестов
- `chore`: обслуживание проекта

**Примеры:**

```bash
git commit -m "feat(auth): add JWT authentication"
git commit -m "fix(api): handle null response in user service"
git commit -m "docs: update README with installation steps"
```

## 4.6. .gitignore для нашего стека

### Создание .gitignore файла

```bash
# Создание файла
touch .gitignore
```

**Содержимое .gitignore для нашего проекта:**

```gitignore
# Node.js
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*
.npm
.yarn-integrity

# Build outputs
dist/
build/
out/

# Environment variables
.env
.env.local
.env.development.local
.env.test.local
.env.production.local

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db

# Logs
logs
*.log

# Runtime data
pids
*.pid
*.seed
*.pid.lock

# Coverage directory used by tools like istanbul
coverage/
.nyc_output/

# Dependency directories
jspm_packages/

# Optional npm cache directory
.npm

# Optional eslint cache
.eslintcache

# Docker
.dockerignore
*.tar

# Database
*.sqlite
*.db

# Temporary files
tmp/
temp/

# Container data
data/
volumes/

# SSL certificates (if not needed in repo)
*.pem
*.key
*.crt

# Backup files
*.bak
*.backup
```

## 4.7. Git hooks и автоматизация

### Pre-commit hooks

Создание pre-commit hook для проверки кода:

```bash
# Создание hook файла
nano .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

**Содержимое pre-commit hook:**

```bash
#!/bin/sh
# Pre-commit hook для проверки качества кода

echo "🔍 Running pre-commit checks..."

# Проверка синтаксиса JavaScript/TypeScript
if command -v npm >/dev/null 2>&1; then
    echo "📝 Checking code style..."
    npm run lint
    if [ $? -ne 0 ]; then
        echo "❌ Lint check failed. Please fix errors before committing."
        exit 1
    fi
fi

# Проверка тестов
if [ -f "package.json" ] && grep -q "test" package.json; then
    echo "🧪 Running tests..."
    npm test
    if [ $? -ne 0 ]; then
        echo "❌ Tests failed. Please fix tests before committing."
        exit 1
    fi
fi

echo "✅ All checks passed!"
exit 0
```

### Commit-msg hook для проверки формата сообщений

```bash
nano .git/hooks/commit-msg
chmod +x .git/hooks/commit-msg
```

**Содержимое commit-msg hook:**

```bash
#!/bin/sh
# Проверка формата сообщения коммита

commit_regex='^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,50}'

error_msg="❌ Invalid commit message format. Use: type(scope): description
Examples:
  feat(auth): add user registration
  fix(api): handle null response
  docs: update README"

if ! grep -qE "$commit_regex" "$1"; then
    echo "$error_msg"
    exit 1
fi

echo "✅ Commit message format is valid"
```

## 4.8. Интеграция Git с Komodo

### Настройка Git-based деплоя в Komodo

**1. Создание deployment в Komodo:**

```yaml
# komodo-deployment.yml
name: "top-web-stack"
git_provider: "github"
git_account: "your-username"
repository: "TopWebStack"
branch: "main"
build_path: "."
dockerfile_path: "Dockerfile"
```

**2. Webhook для автоматического деплоя:**

После настройки deployment в Komodo, система автоматически создаст webhook URL. Добавьте его в настройки репозитория GitHub/GitLab:

- **Payload URL**: `https://your-komodo.example.com/api/webhook/deploy/deployment-id`
- **Content Type**: `application/json`
- **Events**: Push events

**3. Автоматизация через GitHub Actions:**

```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to Komodo
      run: |
        curl -X POST \
          -H "Authorization: Bearer ${{ secrets.KOMODO_API_KEY }}" \
          -H "Content-Type: application/json" \
          -d '{"branch": "main"}' \
          https://your-komodo.example.com/api/deployment/redeploy/deployment-id
```

## 4.9. Практические сценарии и troubleshooting

### Откат изменений

```bash
# Отмена изменений в рабочей директории
git checkout -- filename.txt
git checkout .                    # все файлы

# Отмена последнего коммита (сохранив изменения)
git reset --soft HEAD~1

# Отмена последнего коммита (удалив изменения)
git reset --hard HEAD~1

# Создание коммита, отменяющего изменения
git revert HEAD
git revert commit-hash
```

### Работа с конфликтами

```bash
# При конфликте слияния
git status                        # просмотр конфликтных файлов
# Редактируем файлы, убирая маркеры конфликтов
git add conflicted-file.txt
git commit -m "Resolve merge conflict"

# Отмена слияния
git merge --abort
```

### Полезные команды для отладки

```bash
# Поиск коммита, который ввел баг
git bisect start
git bisect bad HEAD
git bisect good commit-hash
# Git автоматически найдет проблемный коммит

# Просмотр истории изменений файла
git log --follow filename.txt
git blame filename.txt

# Временное сохранение изменений
git stash
git stash pop
git stash list
git stash drop
```

### Очистка репозитория

```bash
# Удаление неотслеживаемых файлов
git clean -f                     # файлы
git clean -fd                    # файлы и директории
git clean -n                     # просмотр без удаления

# Сжатие репозитория
git gc --aggressive --prune=now
```

## 4.10. Практическое задание

### 🎯 Цель задания

Создать полноценный Git workflow для проекта TopWebStack с применением изученных практик: настройка репозитория, работа с ветками, Fine-grained токены и Git hooks.

### 📋 Задание 1: Настройка Git-окружения

**1.1. Базовая конфигурация Git**

```bash
# Настройте Git с вашими данными
git config --global user.name "Ваше Имя"
git config --global user.email "your.email@example.com"

# Настройте дополнительные параметры
git config --global init.defaultBranch main
git config --global pull.rebase false
git config --global core.autocrlf input  # для Linux/macOS
```

**1.2. Создайте Fine-grained Personal Access Token**

1. Перейдите в GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Создайте токен со следующими параметрами:
   - Name: `TopWebStack-Practice`
   - Expiration: 30 days
   - Repository access: Selected repositories (выберите учебный репозиторий)
   - Permissions: Contents (Read/Write), Metadata (Read), Pull requests (Read/Write)

**1.3. Настройте credential helper**

```bash
# Настройка для безопасного хранения токена
git config --global credential.helper store
# При первом push введите username и токен как password
```

### 📋 Задание 2: Работа с Git Flow

**2.1. Инициализация проекта**

```bash
# Создайте новый проект
mkdir topwebstack-practice
cd topwebstack-practice
git init

# Создайте начальную структуру
mkdir -p {src,docs,tests,config}
echo "# TopWebStack Practice Project" > README.md
echo "node_modules/\n.env\n*.log" > .gitignore
```

**2.2. Реализуйте Git Flow**

```bash
# 1. Первый коммит в main
git add .
git commit -m "feat: initial project structure"

# 2. Создайте ветку develop
git checkout -b develop

# 3. Добавьте базовые файлы
echo "console.log('Hello, TopWebStack!');" > src/app.js
echo "# Development Notes" > docs/dev-notes.md
git add .
git commit -m "feat: add basic application files"

# 4. Создайте feature-ветку для аутентификации
git checkout -b feature/user-auth

# 5. Реализуйте "аутентификацию"
mkdir src/auth
echo "// User authentication module" > src/auth/auth.js
echo "// Auth tests" > tests/auth.test.js
git add .
git commit -m "feat(auth): add user authentication module"
git commit --allow-empty -m "feat(auth): add authentication tests"

# 6. Вернитесь в develop и сделайте merge
git checkout develop
git merge feature/auth --no-ff -m "feat: merge user authentication feature"

# 7. Создайте ветку для API
git checkout -b feature/rest-api
echo "// REST API endpoints" > src/api.js
echo "// API tests" > tests/api.test.js
git add .
git commit -m "feat(api): implement REST API endpoints"

# 8. Merge API в develop
git checkout develop
git merge feature/rest-api --no-ff -m "feat: merge REST API feature"

# 9. Подготовьте релиз
git checkout -b release/v1.0
echo '{"version": "1.0.0", "name": "topwebstack-practice"}' > package.json
git add package.json
git commit -m "chore(release): prepare v1.0.0 release"

# 10. Merge в main
git checkout main
git merge release/v1.0 --no-ff -m "release: TopWebStack Practice v1.0.0"
git tag -a v1.0.0 -m "Release version 1.0.0"

# 11. Merge обратно в develop
git checkout develop
git merge release/v1.0
```

**2.3. Создайте hotfix**

```bash
# Имитируем критическую ошибку в production
git checkout main
git checkout -b hotfix/security-patch

# Исправляем "уязвимость"
echo "// Security patch applied" >> src/auth/auth.js
git add .
git commit -m "fix(security): patch authentication vulnerability"

# Merge в main
git checkout main
git merge hotfix/security-patch --no-ff
git tag -a v1.0.1 -m "Hotfix version 1.0.1"

# Merge в develop
git checkout develop
git merge hotfix/security-patch
```

### 📋 Задание 3: Git Hooks

**3.1. Создайте pre-commit hook**

```bash
# Создайте hook файл
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/sh
echo "🔍 Running pre-commit checks..."

# Проверяем наличие console.log в JS файлах
if git diff --cached --name-only | grep -E "\.(js|ts)$" | xargs grep -l "console\.log" 2>/dev/null; then
    echo "❌ Found console.log statements. Please remove them before committing."
    echo "Files with console.log:"
    git diff --cached --name-only | grep -E "\.(js|ts)$" | xargs grep -l "console\.log" 2>/dev/null
    exit 1
fi

# Проверяем размер файлов (не более 1MB)
large_files=$(git diff --cached --name-only | xargs ls -la 2>/dev/null | awk '$5 > 1048576 {print $9}')
if [ ! -z "$large_files" ]; then
    echo "❌ Large files detected (>1MB):"
    echo "$large_files"
    echo "Please use Git LFS for large files."
    exit 1
fi

echo "✅ Pre-commit checks passed!"
EOF

chmod +x .git/hooks/pre-commit
```

**3.2. Создайте commit-msg hook**

```bash
cat > .git/hooks/commit-msg << 'EOF'
#!/bin/sh
commit_regex='^(feat|fix|docs|style|refactor|test|chore)(\(.+\))?: .{1,50}'

if ! grep -qE "$commit_regex" "$1"; then
    echo "❌ Invalid commit message format!"
    echo "Format: type(scope): description"
    echo "Types: feat, fix, docs, style, refactor, test, chore"
    echo "Example: feat(auth): add user login functionality"
    exit 1
fi

echo "✅ Commit message format is valid"
EOF

chmod +x .git/hooks/commit-msg
```

**3.3. Протестируйте hooks**

```bash
# Попробуйте закоммитить неправильное сообщение
echo "test" > test.txt
git add test.txt
git commit -m "bad message"  # Должно заблокировать

# Правильный коммит
git commit -m "test: add test file for hooks validation"
```

### 📋 Задание 4: Удаленный репозиторий

**4.1. Подключите к GitHub**

```bash
# Создайте репозиторий на GitHub (через веб-интерфейс)
# Добавьте remote
git remote add origin https://github.com/ваш-username/topwebstack-practice.git

# Отправьте все ветки
git push -u origin main
git push origin develop
git push origin --tags
```

**4.2. Настройте branch protection**

В GitHub Settings → Branches настройте для ветки `main`:
- ✅ Require pull request reviews before merging
- ✅ Require status checks to pass before merging
- ✅ Require branches to be up to date before merging

### 🧪 Проверка знаний

**Ответьте на вопросы (проверьте себя):**

1. **Концептуальные вопросы:**
   - Чем отличается `git fetch` от `git pull`?
   - Когда использовать `git merge` vs `git rebase`?
   - Что происходит при выполнении `git reset --hard HEAD~1`?

2. **Практические задачи:**
   ```bash
   # Как отменить последние 3 коммита, сохранив изменения?
   git reset --soft HEAD~3
   
   # Как посмотреть изменения между двумя ветками?
   git diff branch1..branch2
   
   # Как найти коммит, который внес конкретное изменение?
   git log -S "искомый_текст" --source --all
   ```

3. **Сценарии troubleshooting:**
   - Вы случайно закоммитили в `main` вместо feature-ветки. Как исправить?
   - Нужно применить коммит из одной ветки в другую. Какую команду использовать?
   - Как разрешить merge-конфликт в файле?

### ✅ Критерии оценки

**Отлично (5):**
- ✅ Корректно настроен Git и Fine-grained токен
- ✅ Реализован полный Git Flow с правильными merge
- ✅ Созданы и протестированы Git hooks  
- ✅ Настроен удаленный репозиторий с branch protection
- ✅ Даны правильные ответы на все вопросы проверки

**Хорошо (4):**
- ✅ Выполнены задания 1-3
- ✅ Частично настроен удаленный репозиторий
- ✅ Правильные ответы на 70%+ вопросов

**Удовлетворительно (3):**
- ✅ Выполнены задания 1-2  
- ✅ Базовое понимание Git workflow
- ✅ Правильные ответы на 50%+ вопросов

### 💡 Подсказки для выполнения

**Если возникли проблемы:**

1. **Ошибка аутентификации GitHub:**
   ```bash
   # Проверьте токен
   git config --global --list | grep credential
   # Сбросьте saved credentials
   git config --global --unset credential.helper
   ```

2. **Конфликт при merge:**
   ```bash
   # Посмотрите файлы с конфликтами
   git status
   # Откройте файл, найдите маркеры <<<< ==== >>>>
   # Удалите маркеры, оставьте нужный код
   git add .
   git commit
   ```

3. **Проблемы с hooks:**
   ```bash
   # Проверьте права доступа
   ls -la .git/hooks/
   chmod +x .git/hooks/pre-commit
   ```

## 4.11. Заключение главы

В этой главе мы изучили:

✅ **Основы Git** — понимание принципов работы распределенной системы контроля версий

✅ **Настройку Git** — конфигурация для продуктивной работы в команде

✅ **Git workflow** — организация процесса разработки с ветками и коммитами

✅ **Fine-grained токены** — современная безопасная аутентификация в GitHub

✅ **Git hooks** — автоматизация проверок качества кода

✅ **Интеграцию с Komodo** — автоматический деплой через Git hooks

✅ **Best practices** — соглашения о коммитах и структуре проекта

✅ **Практические навыки** — выполнение реального Git workflow

**Что дальше:**

В следующей главе мы установим и настроим **Komodo** — систему управления инфраструктурой, которая будет использовать Git для автоматического деплоя наших контейнерных приложений.

---

**Навигация:**
- [← Глава 3: Podman](chapter-03-podman.md)
- [→ Глава 5: Komodo](chapter-05-comodo.md)
- [📚 Оглавление курса](index.md)