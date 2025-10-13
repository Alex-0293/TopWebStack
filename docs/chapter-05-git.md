[← Оглавление курса](index.md)


# Глава 4. Git и GitHub: управление версиями и совместная работа

## 4.1. Git: система контроля версий

**Git** — распределенная система контроля версий, созданная Линусом Торвальдсом.

**Ключевые характеристики:**
- **Распределенность** — каждый разработчик имеет полную копию репозитория
- **Скорость** — большинство операций выполняются локально
- **Ветвление** — легковесные ветки для параллельной разработки
- **Целостность** — все изменения хешируются SHA-1
- **Staging Area** — промежуточная область для подготовки коммитов

**Где применяется:**
- Управление исходным кодом
- Совместная разработка
- CI/CD пайплайны
- Документация и конфигурации
- Infrastructure as Code

**Почему в этом курсе:**
- Стандарт индустрии
- Интеграция с GitHub/GitLab
- Необходим для деплоя
- Отслеживание изменений
- Командная работа

**Распространенность:**
- 95%+ проектов используют Git
- GitHub: 100M+ разработчиков
- GitLab, Bitbucket, Azure DevOps
- Поддержка всех IDE

**Актуальная версия:** Git 2.47+

**Ссылки:**
- Официальный сайт: https://git-scm.com/
- Документация: https://git-scm.com/doc
- Pro Git Book: https://git-scm.com/book/en/v2
- GitHub: https://github.com

### Git Workflow в нашем стеке

```mermaid
graph TB
    subgraph local["💻 Локальная разработка"]
        direction TB
        working["📁 Working Directory<br/>(рабочая директория)"]
        staging["📋 Staging Area<br/>(индекс)"]
        local_repo["💾 Local Repository<br/>(.git)"]
        
        working --> |"git add"| staging
        staging --> |"git commit"| local_repo
    end
    
    subgraph remote["☁️ Удаленный репозиторий"]
        direction TB
        github["🐙 GitHub/GitLab"]
        ci_cd["⚙️ CI/CD Pipeline"]
        deployment["🚀 Automatic Deploy"]
        
        github --> ci_cd
        ci_cd --> deployment
    end
    
    local_repo --> |"git push"| github
    github --> |"git pull/fetch"| local_repo
    
    style local fill:#F05032,stroke:#D73027,stroke-width:3px,color:#fff
    style remote fill:#4078c0,stroke:#2c5f99,stroke-width:3px,color:#fff
    style working fill:#FF6B35,stroke:#E55100,stroke-width:2px,color:#fff
    style staging fill:#FFA500,stroke:#FF8C00,stroke-width:2px,color:#fff
    style local_repo fill:#F05032,stroke:#D73027,stroke-width:2px,color:#fff
    style github fill:#24292e,stroke:#181717,stroke-width:2px,color:#fff
    style ci_cd fill:#2088FF,stroke:#1976D2,stroke-width:2px,color:#fff
    style deployment fill:#28a745,stroke:#22863a,stroke-width:2px,color:#fff
```


## 4.2. Установка и настройка Git

### Проверка установки

```bash
# Git уже установлен на AlmaLinux
git --version
# Вывод: git version 2.43.0 или выше
```

### Глобальная конфигурация

```bash
# Настройка имени и email
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Настройка редактора по умолчанию
git config --global core.editor "nano"

# Настройка ветки по умолчанию
git config --global init.defaultBranch main

# Цветной вывод
git config --global color.ui auto

# Автоматическое исправление опечаток
git config --global help.autocorrect 1

# Настройка pull стратегии
git config --global pull.rebase false

# Просмотр всех настроек
git config --list
```

### Полезные алиасы

```bash
# Короткие команды
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.unstage 'reset HEAD --'
git config --global alias.last 'log -1 HEAD'

# Красивый лог
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.lga "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --all"

# Показать изменения
git config --global alias.df diff
git config --global alias.dfs 'diff --staged'
```


## 4.3. Основные команды Git

### Жизненный цикл файлов в Git

```mermaid
graph LR
    subgraph workspace["📁 Рабочая директория"]
        direction TB
        untracked["📄 Untracked<br/>(новые файлы)"]
        modified["✏️ Modified<br/>(изменены)"]
    end
    
    subgraph staging_area["📋 Staging Area"]
        direction TB
        staged["🎯 Staged<br/>(подготовлены)"]
    end
    
    subgraph repository["💾 Git Repository"]
        direction TB
        committed["✅ Committed<br/>(зафиксированы)"]
    end
    
    untracked --> |"git add"| staged
    modified --> |"git add"| staged
    staged --> |"git commit"| committed
    committed --> |"edit"| modified
    staged --> |"git restore --staged"| modified
    
    style workspace fill:#FFF3E0,stroke:#FF9800,stroke-width:3px,color:#333
    style staging_area fill:#E8F5E8,stroke:#4CAF50,stroke-width:3px,color:#333
    style repository fill:#E3F2FD,stroke:#2196F3,stroke-width:3px,color:#333
    style untracked fill:#FFCDD2,stroke:#F44336,stroke-width:2px,color:#333
    style modified fill:#FFF9C4,stroke:#FFC107,stroke-width:2px,color:#333
    style staged fill:#C8E6C9,stroke:#4CAF50,stroke-width:2px,color:#333
    style committed fill:#BBDEFB,stroke:#2196F3,stroke-width:2px,color:#333
```

### Создание репозитория

```bash
# Инициализация нового репозитория
mkdir my-project && cd my-project
git init

# Клонирование существующего репозитория
git clone https://github.com/user/repo.git
git clone git@github.com:user/repo.git  # SSH

# Клонирование конкретной ветки
git clone -b develop https://github.com/user/repo.git
```

### Базовый workflow

```bash
# Проверка статуса
git status

# Добавление файлов в staging
git add file.txt              # Один файл
git add *.js                  # По маске
git add .                     # Все файлы
git add -p                    # Интерактивное добавление

# Создание коммита
git commit -m "Add new feature"
git commit -am "Update files"  # Add + commit для tracked файлов

# Просмотр истории
git log
git log --oneline
git log --graph --all
git lg  # Если настроен alias

# Просмотр изменений
git diff                      # Unstaged изменения
git diff --staged             # Staged изменения
git diff HEAD                 # Все изменения
git diff branch1..branch2     # Между ветками
```

### Работа с ветками

```bash
# Создание ветки
git branch feature-auth
git checkout -b feature-auth  # Создать и переключиться

# Переключение между ветками
git checkout main
git switch main               # Новая команда (Git 2.23+)

# Список веток
git branch                    # Локальные
git branch -r                 # Удаленные
git branch -a                 # Все

# Удаление ветки
git branch -d feature-auth    # Безопасное удаление
git branch -D feature-auth    # Принудительное удаление

# Переименование ветки
git branch -m old-name new-name
```

### Git Flow модель ветвления

```mermaid
graph TB
    subgraph main_line["🎯 Main Branch"]
        direction LR
        initial["📦 Initial"] --> v1_0["🎉 v1.0"] --> v1_0_1["🔧 v1.0.1"]
    end
    
    subgraph develop_line["🔧 Develop Branch"]
        direction LR
        dev_start["🚀 Start"] --> dev_auth["✅ Auth"] --> dev_api["✅ API"] --> dev_fix["🔧 Fix"]
    end
    
    subgraph feature_work["💡 Feature Branches"]
        direction TB
        feat_auth["🔐 feature/auth"]
        feat_api["🚀 feature/api"]
    end
    
    subgraph hotfix_work["🚨 Hotfix Branch"]
        direction TB
        hotfix["🛡️ hotfix/security"]
    end
    
    initial --> dev_start
    feat_auth --> dev_auth
    dev_auth --> feat_api
    feat_api --> dev_api
    dev_api --> v1_0
    v1_0 --> hotfix
    hotfix --> v1_0_1
    hotfix --> dev_fix
    
    style main_line fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style develop_line fill:#2196F3,stroke:#1976D2,stroke-width:3px,color:#fff
    style feature_work fill:#FF9800,stroke:#F57C00,stroke-width:3px,color:#fff
    style hotfix_work fill:#F44336,stroke:#D32F2F,stroke-width:3px,color:#fff
    style initial fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style v1_0 fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff
    style v1_0_1 fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff
    style dev_start fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style dev_auth fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style dev_api fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style dev_fix fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style feat_auth fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style feat_api fill:#FFB74D,stroke:#FB8C00,stroke-width:2px,color:#fff
    style hotfix fill:#EF5350,stroke:#E53935,stroke-width:2px,color:#fff
```

**Типы веток:**
- **main** — production-ready код
- **develop** — интеграционная ветка
- **feature/** — разработка новых функций
- **hotfix/** — срочные исправления в production

### Слияние и rebase

```bash
# Слияние ветки
git checkout main
git merge feature-auth

# Rebase (перебазирование)
git checkout feature-auth
git rebase main

# Интерактивный rebase
git rebase -i HEAD~3          # Последние 3 коммита

# Разрешение конфликтов
git status                    # Посмотреть конфликты
# Отредактировать файлы
git add resolved-file.txt
git commit                    # Или git rebase --continue
```


## 4.4. Работа с удаленными репозиториями

### Настройка remote

```bash
# Добавление remote
git remote add origin https://github.com/user/repo.git

# Просмотр remotes
git remote -v

# Изменение URL
git remote set-url origin git@github.com:user/repo.git

# Удаление remote
git remote remove origin
```

### Push и Pull

```bash
# Отправка изменений
git push origin main
git push -u origin main       # Установить upstream
git push --all                # Все ветки
git push --tags               # Все теги

# Получение изменений
git pull origin main
git pull --rebase origin main # С rebase

# Fetch без merge
git fetch origin
git fetch --all
```

### Работа с тегами

```bash
# Создание тега
git tag v1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"

# Список тегов
git tag
git tag -l "v1.*"

# Отправка тегов
git push origin v1.0.0
git push origin --tags

# Удаление тега
git tag -d v1.0.0
git push origin :refs/tags/v1.0.0
```


## 4.5. GitHub и аутентификация

### SSH ключи для GitHub

```bash
# Генерация SSH ключа
ssh-keygen -t ed25519 -C "your.email@example.com" -f ~/.ssh/github_ed25519

# Добавление ключа в ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/github_ed25519

# Копирование публичного ключа
cat ~/.ssh/github_ed25519.pub
# Скопируйте вывод и добавьте в GitHub:
# Settings → SSH and GPG keys → New SSH key
```

**Настройка SSH config:**

```bash
nano ~/.ssh/config
```

Добавьте:

```
# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_ed25519
    AddKeysToAgent yes
```

**Проверка подключения:**

```bash
ssh -T git@github.com
# Вывод: Hi username! You've successfully authenticated...
```

### GitHub Fork & Pull Request Workflow

```mermaid
graph TB
    subgraph upstream["🏢 Upstream Repository"]
        direction TB
        orig_main["🎯 main branch"]
    end
    
    subgraph fork["👤 Your Fork"]
        direction TB
        fork_main["🎯 fork/main"]
        feature["💡 feature/new-api"]
        
        fork_main --> feature
    end
    
    subgraph local["💻 Local Clone"]
        direction TB
        local_main["🎯 local/main"]
        local_feature["💡 local/feature"]
        
        local_main --> local_feature
    end
    
    orig_main --> |"fork"| fork_main
    fork_main --> |"git clone"| local_main
    local_feature --> |"git push origin"| feature
    feature --> |"Pull Request"| orig_main
    orig_main --> |"git pull upstream"| local_main
    
    style upstream fill:#24292e,stroke:#181717,stroke-width:3px,color:#fff
    style fork fill:#6f42c1,stroke:#5a32a3,stroke-width:3px,color:#fff
    style local fill:#F05032,stroke:#D73027,stroke-width:3px,color:#fff
    style orig_main fill:#2ea44f,stroke:#22863a,stroke-width:2px,color:#fff
    style fork_main fill:#8957e5,stroke:#6f42c1,stroke-width:2px,color:#fff
    style feature fill:#ffa657,stroke:#fb8500,stroke-width:2px,color:#fff
    style local_main fill:#ff6b6b,stroke:#ee5a52,stroke-width:2px,color:#fff
    style local_feature fill:#ffaa5a,stroke:#ff8800,stroke-width:2px,color:#fff
```

**Этапы работы:**
1. **Fork** — создание копии репозитория
2. **Clone** — клонирование fork на локальную машину
3. **Branch** — создание feature-ветки
4. **Commit** — фиксация изменений
5. **Push** — отправка в fork
6. **Pull Request** — предложение изменений в upstream


## 4.6. Fine-grained Personal Access Tokens

**Fine-grained Personal Access Tokens (PAT)** — новый тип токенов GitHub с детальным контролем доступа.

### Преимущества Fine-grained PAT

- **Гранулярные разрешения** — доступ только к нужным репозиториям
- **Ограниченный срок действия** — автоматическое истечение
- **Аудит** — отслеживание использования токена
- **Безопасность** — минимальные необходимые права
- **Организации** — поддержка organization-level токенов

### Создание Fine-grained PAT

**Шаг 1: Переход к настройкам**

1. Откройте GitHub → Settings
2. Developer settings → Personal access tokens
3. Fine-grained tokens → Generate new token

**Шаг 2: Настройка токена**

```
Token name: container-dev-course-token
Description: Token for container development course project
Expiration: 90 days (рекомендуется)
Resource owner: Your username or organization
```

**Шаг 3: Repository access**

Выберите один из вариантов:
- **All repositories** — доступ ко всем репозиториям (не рекомендуется)
- **Only select repositories** — выберите конкретные репозитории
- **Public Repositories (read-only)** — только чтение публичных

**Шаг 4: Permissions**

**Repository permissions:**
```
Contents: Read and write
Metadata: Read-only (автоматически)
Pull requests: Read and write
Issues: Read and write
Workflows: Read and write (для GitHub Actions)
```

**Account permissions (опционально):**
```
Email addresses: Read-only
GPG Keys: Read-only
```

**Шаг 5: Генерация**

1. Нажмите "Generate token"
2. **ВАЖНО:** Скопируйте токен немедленно (он больше не будет показан)
3. Сохраните токен в безопасном месте

### Использование Fine-grained PAT

**Для HTTPS клонирования:**

```bash
# Клонирование с токеном
git clone https://github.com/username/repo.git

# При запросе пароля введите токен вместо пароля
Username: your-username
Password: ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

**Сохранение токена в credential helper:**

```bash
# Настройка credential helper
git config --global credential.helper store

# Или использовать cache (временное хранение)
git config --global credential.helper 'cache --timeout=3600'

# Для macOS (Keychain)
git config --global credential.helper osxkeychain

# Первый push с токеном
git push origin main
# Введите username и токен - они сохранятся
```

**Использование токена в URL (не рекомендуется для production):**

```bash
# Токен в URL (только для тестирования!)
git clone https://ghp_xxxxxxxxxxxx@github.com/username/repo.git

# Изменение существующего remote
git remote set-url origin https://ghp_xxxxxxxxxxxx@github.com/username/repo.git
```

### Безопасное хранение токенов

**Использование переменных окружения:**

```bash
# Добавьте в ~/.bashrc
export GITHUB_TOKEN="ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Использование в скриптах
git clone https://${GITHUB_TOKEN}@github.com/username/repo.git
```

**Использование .netrc файла:**

```bash
# Создайте ~/.netrc
nano ~/.netrc
```

Содержимое:

```
machine github.com
login your-username
password ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

```bash
# Установите правильные права
chmod 600 ~/.netrc
```

### Управление токенами

**Просмотр активных токенов:**

1. GitHub → Settings → Developer settings
2. Personal access tokens → Fine-grained tokens
3. Просмотр списка активных токенов

**Отзыв токена:**

1. Найдите токен в списке
2. Нажмите "Revoke" или "Delete"
3. Подтвердите действие

**Обновление токена:**

1. Создайте новый токен с теми же правами
2. Обновите токен в credential helper
3. Отзовите старый токен

### Best Practices для токенов

```bash
# ✅ Хорошие практики:
# 1. Минимальные необходимые права
# 2. Короткий срок действия (30-90 дней)
# 3. Отдельные токены для разных проектов
# 4. Регулярная ротация токенов
# 5. Хранение в безопасном месте

# ❌ Плохие практики:
# - Токены в коде или публичных репозиториях
# - Токены с полным доступом ко всем репозиториям
# - Токены без срока действия
# - Использование одного токена везде
# - Токены в URL (видны в истории команд)
```


## 4.7. GitHub Actions и CI/CD

### CI/CD Pipeline с GitHub Actions

```mermaid
graph LR
    subgraph developer["👨‍💻 Developer"]
        direction TB
        code["💻 Code"]
        commit["📝 Commit"]
        push["📤 Push"]
        
        code --> commit --> push
    end
    
    subgraph github["🐙 GitHub"]
        direction TB
        repo["📦 Repository"]
        actions["⚙️ GitHub Actions"]
        
        repo --> actions
    end
    
    subgraph pipeline["🔄 CI/CD Pipeline"]
        direction TB
        build["🔨 Build"]
        test["🧪 Test"]
        deploy["🚀 Deploy"]
        
        build --> test --> deploy
    end
    
    subgraph production["🖥️ Production"]
        direction TB
        komodo["🎛️ Komodo"]
        containers["📦 Containers"]
        
        komodo --> containers
    end
    
    push --> repo
    actions --> build
    deploy --> komodo
    
    style developer fill:#F05032,stroke:#D73027,stroke-width:3px,color:#fff
    style github fill:#24292e,stroke:#181717,stroke-width:3px,color:#fff
    style pipeline fill:#2088FF,stroke:#1976D2,stroke-width:3px,color:#fff
    style production fill:#4CAF50,stroke:#388E3C,stroke-width:3px,color:#fff
    style code fill:#FF6B35,stroke:#E55100,stroke-width:2px,color:#fff
    style commit fill:#FFA500,stroke:#FF8C00,stroke-width:2px,color:#fff
    style push fill:#F05032,stroke:#D73027,stroke-width:2px,color:#fff
    style repo fill:#24292e,stroke:#181717,stroke-width:2px,color:#fff
    style actions fill:#2088FF,stroke:#1976D2,stroke-width:2px,color:#fff
    style build fill:#42A5F5,stroke:#1E88E5,stroke-width:2px,color:#fff
    style test fill:#66BB6A,stroke:#43A047,stroke-width:2px,color:#fff
    style deploy fill:#FFA726,stroke:#F57C00,stroke-width:2px,color:#fff
    style komodo fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff
    style containers fill:#81C784,stroke:#66BB6A,stroke-width:2px,color:#fff
```

### Базовый workflow

```bash
# Создание директории для workflows
mkdir -p .github/workflows
nano .github/workflows/ci.yml
```

Содержимое:

```yaml
name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '26'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Build
      run: npm run build
```

### Использование токенов в Actions

```yaml
# Использование GITHUB_TOKEN (автоматически доступен)
- name: Push to another repo
  env:
    GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  run: |
    git push https://${GITHUB_TOKEN}@github.com/user/repo.git

# Использование custom токена
- name: Deploy
  env:
    DEPLOY_TOKEN: ${{ secrets.DEPLOY_TOKEN }}
  run: |
    ./deploy.sh
```


## 4.8. .gitignore и .gitattributes

### Создание .gitignore

```bash
nano .gitignore
```

Содержимое для Node.js проекта:

```gitignore
# Dependencies
node_modules/
npm-debug.log*
yarn-debug.log*
yarn-error.log*

# Environment variables
.env
.env.local
.env.*.local

# Build output
dist/
build/
.next/
out/

# Logs
*.log
logs/

# IDE
.vscode/
.idea/
*.swp
*.swo
.DS_Store

# Testing
coverage/
.nyc_output/

# Database
*.sql
*.sqlite
*.db

# Containers
.podman/
```

### .gitattributes для line endings

```bash
nano .gitattributes
```

Содержимое:

```gitattributes
# Auto detect text files and normalize line endings to LF
* text=auto eol=lf

# Explicitly declare text files
*.js text
*.ts text
*.json text
*.md text
*.yml text
*.yaml text

# Declare files that will always have CRLF line endings on checkout
*.bat text eol=crlf

# Denote all files that are truly binary and should not be modified
*.png binary
*.jpg binary
*.gif binary
*.ico binary
*.woff binary
*.woff2 binary
```


## 4.9. Git Hooks

### Pre-commit hook

```bash
# Создание pre-commit hook
nano .git/hooks/pre-commit
```

Содержимое:

```bash
#!/bin/bash

# Запуск линтера перед коммитом
npm run lint

# Если линтер вернул ошибку, отменить коммит
if [ $? -ne 0 ]; then
    echo "❌ Lint failed. Please fix errors before committing."
    exit 1
fi

echo "✅ Lint passed"
exit 0
```

```bash
# Сделать исполняемым
chmod +x .git/hooks/pre-commit
```

### Husky для управления hooks

```bash
# Установка Husky
npm install --save-dev husky
npx husky init

# Создание pre-commit hook
echo "npm run lint" > .husky/pre-commit

# Создание commit-msg hook
echo "npx commitlint --edit \$1" > .husky/commit-msg
```


## Практическое задание

**Задание 1: Настройка Git**
1. Настройте глобальную конфигурацию Git
2. Создайте полезные алиасы
3. Настройте SSH ключ для GitHub
4. Проверьте подключение

**Задание 2: Работа с репозиторием**
1. Создайте новый репозиторий на GitHub
2. Клонируйте его локально
3. Создайте несколько коммитов
4. Отправьте изменения на GitHub

**Задание 3: Fine-grained PAT**
1. Создайте Fine-grained Personal Access Token
2. Настройте минимальные необходимые права
3. Используйте токен для клонирования приватного репозитория
4. Сохраните токен в credential helper

**Задание 4: Ветвление**
1. Создайте feature ветку
2. Сделайте несколько коммитов
3. Создайте Pull Request на GitHub
4. Выполните merge в main

**Задание 5: GitHub Actions**
1. Создайте простой CI workflow
2. Настройте автоматический запуск тестов
3. Добавьте badge статуса в README
4. Проверьте работу workflow

## Проверка знаний

```bash
# 1. Проверка конфигурации Git
git config --list | grep user

# 2. Проверка SSH подключения
ssh -T git@github.com

# 3. Проверка remote
git remote -v

# 4. Просмотр истории
git log --oneline -10

# 5. Проверка статуса
git status
```

## Troubleshooting

**Проблема: Permission denied (publickey)**
```bash
# Проверка SSH ключей
ssh-add -l

# Добавление ключа
ssh-add ~/.ssh/github_ed25519

# Проверка подключения
ssh -vT git@github.com
```

**Проблема: Authentication failed (HTTPS)**
```bash
# Очистка сохраненных credentials
git credential-cache exit
git credential-store erase

# Повторная попытка с новым токеном
git push origin main
```

**Проблема: Merge conflicts**
```bash
# Просмотр конфликтов
git status

# Отмена merge
git merge --abort

# Или разрешить конфликты вручную
# Отредактировать файлы, затем:
git add resolved-file.txt
git commit
```


