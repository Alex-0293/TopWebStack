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
graph TB
    subgraph dev_env["💻 Среда разработки"]
        direction TB
        developer["👨‍💻 Разработчик"]
        vscode["🔧 VS Code"]
        local_git["📁 Git Repository"]
        
        developer --> vscode
        vscode --> local_git
    end
    
    subgraph cloud_env["☁️ Облачные сервисы"]
        direction TB
        github["🐙 GitHub<br/>Repository"]
        actions["⚡ GitHub Actions<br/>CI/CD"]
        
        github --> actions
    end
    
    subgraph prod_env["🖥️ Production"]
        direction TB
        komodo["🎛️ Komodo<br/>Dashboard"]
        almalinux["🐧 AlmaLinux<br/>Server"]
        podman["� Podman<br/>Containers"]
        
        komodo --> almalinux
        almalinux --> podman
    end
    
    local_git --> |"git push"| github
    github --> |"webhook"| komodo
    actions --> |"deploy"| komodo
    
    style dev_env fill:#F05032,stroke:#D73027,stroke-width:3px,color:#fff
    style cloud_env fill:#24292E,stroke:#181C1F,stroke-width:3px,color:#fff
    style prod_env fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style developer fill:#FF6B35,stroke:#E55100,stroke-width:2px,color:#fff
    style vscode fill:#007ACC,stroke:#005A9D,stroke-width:2px,color:#fff
    style local_git fill:#F05032,stroke:#D73027,stroke-width:2px,color:#fff
    style github fill:#181717,stroke:#0D1117,stroke-width:2px,color:#fff
    style actions fill:#2088FF,stroke:#1976D2,stroke-width:2px,color:#fff
    style komodo fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style almalinux fill:#0F4C81,stroke:#0D47A1,stroke-width:2px,color:#fff
    style podman fill:#892CA0,stroke:#6A1B9A,stroke-width:2px,color:#fff
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
graph LR
    subgraph workspace["📁 Рабочая область"]
        direction TB
        untracked["📄 Новые файлы<br/>(Untracked)"]
        modified["✏️ Измененные файлы<br/>(Modified)"]
    end
    
    subgraph staging_area["📋 Staging Area"]
        direction TB
        staged["🎯 Подготовленные<br/>(Staged)"]
    end
    
    subgraph git_repo["💾 Git Repository"]
        direction TB
        committed["✅ Зафиксированные<br/>(Committed)"]
    end
    
    untracked --> |"git add"| staged
    modified --> |"git add"| staged
    staged --> |"git commit"| committed
    committed --> |"edit files"| modified
    
    style workspace fill:#FFF3E0,stroke:#FF9800,stroke-width:3px,color:#333
    style staging_area fill:#E8F5E8,stroke:#4CAF50,stroke-width:3px,color:#333
    style git_repo fill:#E3F2FD,stroke:#2196F3,stroke-width:3px,color:#333
    style untracked fill:#FFCDD2,stroke:#F44336,stroke-width:2px,color:#333
    style modified fill:#FFF9C4,stroke:#FFC107,stroke-width:2px,color:#333
    style staged fill:#C8E6C9,stroke:#4CAF50,stroke-width:2px,color:#333
    style committed fill:#BBDEFB,stroke:#2196F3,stroke-width:2px,color:#333
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
graph LR
    subgraph main_branch["🎯 Main Branch (Production)"]
        initial["🎯 Initial Commit"]
        v1_0["🎉 Release v1.0"]
        v1_0_1["🔧 Hotfix v1.0.1"]
    end
    
    subgraph develop_branch["🔧 Develop Branch (Integration)"]
        setup["⚙️ Setup Project"]
        merge_auth["🔗 Merge Authentication"]
        merge_api["🔗 Merge REST API"]
        merge_hotfix["🔧 Merge Hotfix"]
    end
    
    subgraph feature_branches["🚀 Feature Branches"]
        auth_start["🔐 Start Auth Feature"]
        auth_tests["🧪 Add Auth Tests"]
        api_start["🚀 Start API Feature"]
        api_docs["📝 Add API Docs"]
    end
    
    subgraph hotfix_branch["🛡️ Hotfix Branch"]
        security_fix["🛡️ Security Patch"]
    end
    
    initial --> setup
    setup --> auth_start
    auth_start --> auth_tests
    auth_tests --> merge_auth
    
    merge_auth --> api_start
    api_start --> api_docs
    api_docs --> merge_api
    
    merge_api --> v1_0
    
    v1_0 --> security_fix
    security_fix --> v1_0_1
    security_fix --> merge_hotfix
    
    style main_branch fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style develop_branch fill:#2196F3,stroke:#1976D2,stroke-width:3px,color:#fff
    style feature_branches fill:#FF9800,stroke:#F57C00,stroke-width:3px,color:#fff
    style hotfix_branch fill:#F44336,stroke:#D32F2F,stroke-width:3px,color:#fff
    
    style initial fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style v1_0 fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style v1_0_1 fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style setup fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style merge_auth fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style merge_api fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
```

**Git Flow последовательность:**

1. **🎯 Main Branch** — стабильные релизы
2. **🔧 Develop Branch** — активная разработка  
3. **🚀 Feature Branches** — новые функции
4. **🛡️ Hotfix Branch** — критические исправления

### Структура веток

```mermaid
graph TD
    subgraph permanent["🏛️ Постоянные ветки"]
        direction TB
        main["🎯 main<br/>(production-ready)"]
        develop["🔧 develop<br/>(интеграция)"]
    end
    
    subgraph temporary["⏱️ Временные ветки"]
        direction TB
        feature["🚀 feature/*<br/>(новые функции)"]
        release["📦 release/*<br/>(подготовка к релизу)"]
        hotfix["🛡️ hotfix/*<br/>(критические исправления)"]
    end
    
    subgraph examples["📝 Примеры веток"]
        direction TB
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
    
    style permanent fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style temporary fill:#FF9800,stroke:#F57C00,stroke-width:3px,color:#fff
    style examples fill:#9C27B0,stroke:#7B1FA2,stroke-width:3px,color:#fff
    style main fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style develop fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style feature fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style release fill:#FFA726,stroke:#F57C00,stroke-width:2px,color:#fff
    style hotfix fill:#EF5350,stroke:#E53935,stroke-width:2px,color:#fff
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

### Цель задания

Освоить основы Git workflow: настройка, работа с ветками и Fine-grained токены.

### Задание: Git Workflow на практике

**Шаг 1: Базовая настройка**

```bash
# Настройте Git
git config --global user.name "Ваше Имя"  
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# Создайте проект
mkdir git-practice && cd git-practice
git init
echo "# Git Practice" > README.md
git add README.md
git commit -m "feat: initial commit"
```

**Шаг 2: Простой Git Flow**

```bash
# Создайте feature-ветку
git checkout -b feature/auth
echo "// Authentication module" > auth.js
git add auth.js
git commit -m "feat(auth): add authentication"

# Вернитесь в main и слейте
git checkout main
git merge feature/auth
git branch -d feature/auth

# Создайте тег релиза
git tag -a v1.0 -m "Release v1.0"
```

**Шаг 3: GitHub и Fine-grained токены**

```bash
# Создайте токен в GitHub Settings → Developer settings → Fine-grained tokens
# Добавьте remote и запушьте
git remote add origin https://github.com/username/git-practice.git
git push -u origin main --tags
```

### Проверка знаний

**Вопросы для самопроверки:**

1. Чем отличается `git fetch` от `git pull`?
2. Как отменить последний коммит, сохранив изменения?
3. Какую команду использовать для создания новой ветки?
4. Как настроить Fine-grained токен в GitHub?

**Практические команды:**
```bash
# Отмена последнего коммита (сохраняя изменения)
git reset --soft HEAD~1

# Просмотр различий между ветками
git diff main..develop

# Временное сохранение изменений
git stash
git stash pop
```

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

### Задание 4: Удаленный репозиторий

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

### Проверка знаний

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

### Критерии оценки

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

### Подсказки для выполнения

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