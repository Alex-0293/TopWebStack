[← Оглавление курса](index.md)


# Глава 1. Настройка рабочего места разработчика

В этой главе мы настроим минималистичное рабочее место на macOS 16 для разработки в контейнерах на удаленном AlmaLinux сервере. Философия подхода — локальная машина используется только как терминал доступа, вся разработка происходит на сервере.

**Что вы освоите:**
- Установка VSCode с минимальным набором расширений
- Настройка SSH-доступа к удаленному серверу
- Создание проекта на удаленном сервере
- Разработка в контейнерах через VSCode Remote
- Работа с Git на удаленном сервере

**Преимущества такого подхода:**
- Чистая локальная система без зависимостей
- Одинаковое окружение для всей команды
- Мощности сервера для сборки и тестирования
- Легкое переключение между проектами
- Работа с любого устройства


## 1.1. Установка VSCode на macOS

**Visual Studio Code** — единственный инструмент, который нам нужен локально.

**Установка через Homebrew:**

```bash
# Установка Homebrew (если еще не установлен)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Установка VSCode
brew install --cask visual-studio-code
```

**Или скачайте с официального сайта:**

```bash
open https://code.visualstudio.com/download
```

**Проверка установки:**

```bash
# Проверяем, что VSCode доступен из терминала
code --version
```

## 1.2. Установка необходимых расширений VSCode

Установим только те расширения, которые необходимы для удаленной разработки.

**Обязательные расширения:**

1. **Remote - SSH** (ms-vscode-remote.remote-ssh)
   - Подключение к удаленному серверу

2. **Remote - SSH: Editing Configuration Files** (ms-vscode-remote.remote-ssh-edit)
   - Редактирование SSH конфигурации

3. **Dev Containers** (ms-vscode-remote.remote-containers)
   - Разработка внутри контейнеров

**Установка через CLI:**

```bash
# Установка расширений одной командой
code --install-extension ms-vscode-remote.remote-ssh
code --install-extension ms-vscode-remote.remote-ssh-edit
code --install-extension ms-vscode-remote.remote-containers
```

**Установка через UI:**

1. Откройте VSCode
2. Нажмите `Cmd+Shift+X` (Extensions)
3. Найдите "Remote - SSH"
4. Нажмите "Install"
5. Повторите для остальных расширений

**Проверка установки:**

```bash
# Список установленных расширений
code --list-extensions | grep remote
```

## 1.3. Настройка SSH-доступа к AlmaLinux серверу

**SSH (Secure Shell)** — единственный протокол, который нам нужен для работы с удаленным сервером.

### 1.3.1. Генерация SSH-ключа

```bash
# Создание ED25519 ключа (современный и безопасный)
ssh-keygen -t ed25519 -C "dev@almalinux-server" -f ~/.ssh/almalinux_dev

# Вывод:
# Generating public/private ed25519 key pair.
# Enter passphrase (empty for no passphrase): [нажмите Enter]
# Enter same passphrase again: [нажмите Enter]
```

**Почему ED25519:**
- Более безопасный, чем RSA
- Меньший размер ключа
- Быстрее в работе
- Рекомендован для современных систем

### 1.3.2. Копирование ключа на сервер

```bash
# Замените user и server_ip на ваши данные
ssh-copy-id -i ~/.ssh/almalinux_dev.pub user@server_ip

# Пример:
# ssh-copy-id -i ~/.ssh/almalinux_dev.pub developer@192.168.1.100
```

**Если ssh-copy-id не работает:**

```bash
# Альтернативный способ
cat ~/.ssh/almalinux_dev.pub | ssh user@server_ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### 1.3.3. Настройка SSH config

Создадим конфигурацию для удобного подключения:

```bash
# Создаем или редактируем SSH config
nano ~/.ssh/config
```

Добавьте следующую конфигурацию:

```
# AlmaLinux Development Server
Host almalinux-dev
    HostName 192.168.1.100
    User developer
    IdentityFile ~/.ssh/almalinux_dev
    ServerAliveInterval 60
    ServerAliveCountMax 3
    ForwardAgent yes
    Compression yes

# Если используете нестандартный порт
#   Port 2222

# Для работы через jump host (bastion)
#   ProxyJump bastion-host
```

**Параметры конфигурации:**
- `ServerAliveInterval` — отправка keepalive каждые 60 секунд
- `ServerAliveCountMax` — максимум 3 попытки
- `ForwardAgent` — проброс SSH-агента для Git
- `Compression` — сжатие трафика

Сохраните: `Ctrl+O`, `Enter`, `Ctrl+X`

### 1.3.4. Проверка подключения

```bash
# Подключение по алиасу
ssh almalinux-dev

# Должны увидеть приглашение сервера:
# [developer@almalinux ~]$
```

**Если возникли проблемы:**

```bash
# Проверка с подробным выводом
ssh -v almalinux-dev

# Проверка прав на ключи
chmod 600 ~/.ssh/almalinux_dev
chmod 644 ~/.ssh/almalinux_dev.pub
chmod 700 ~/.ssh
```

## 1.4. Подключение VSCode к удаленному серверу

Теперь подключим VSCode к серверу для удаленной разработки.

### 1.4.1. Первое подключение

1. Откройте VSCode
2. Нажмите `F1` или `Cmd+Shift+P`
3. Введите: `Remote-SSH: Connect to Host`
4. Выберите `almalinux-dev` из списка
5. Выберите платформу: `Linux`
6. Дождитесь установки VS Code Server на удаленной машине

**Что происходит:**
- VSCode устанавливает свой сервер на AlmaLinux
- Создается туннель между локальной машиной и сервером
- Все расширения работают на удаленной машине
- Терминал открывается на сервере

### 1.4.2. Проверка подключения

После подключения в левом нижнем углу VSCode должна появиться зеленая кнопка с текстом `SSH: almalinux-dev`.

Откройте терминал в VSCode (`Ctrl+` ` или View → Terminal):

```bash
# Проверяем, что мы на сервере
hostname
# almalinux

uname -a
# Linux almalinux 5.14.0-xxx.el9.x86_64 ...

# Проверяем доступные ресурсы
free -h
df -h
```

## 1.5. Настройка окружения на удаленном сервере

Теперь настроим сервер для комфортной разработки.

### 1.5.1. Установка базовых инструментов

```bash
# Обновление системы
sudo dnf update -y

# Установка базовых инструментов
sudo dnf install -y \
  git \
  nano \
  htop \
  curl \
  wget \
  tar \
  unzip \
  ncdu \
  tree
```

**Описание инструментов:**
- `git` — система контроля версий
- `nano` — простой текстовый редактор
- `htop` — мониторинг процессов
- `curl/wget` — загрузка файлов
- `ncdu` — анализ использования диска
- `tree` — визуализация структуры директорий

### 1.5.2. Настройка bash

```bash
# Редактирование .bashrc
nano ~/.bashrc
```

Добавьте в конец файла:

```bash
# ============================================
# Custom Configuration for Container Development
# ============================================

# Цветной промпт с Git веткой
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\] \[\033[01;33m\]$(parse_git_branch)\[\033[00m\]\$ '

# Алиасы для навигации
alias ll='ls -lah --color=auto'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

# Алиасы для Podman
alias pd='podman'
alias pdc='podman-compose'
alias pdps='podman ps -a'
alias pdimg='podman images'
alias pdlogs='podman logs -f'
alias pdstop='podman stop $(podman ps -q)'
alias pdrm='podman rm $(podman ps -aq)'

# Алиасы для системы
alias ports='ss -tulpn'
alias update='sudo dnf update -y'
alias meminfo='free -h'
alias diskinfo='df -h'
alias cpuinfo='lscpu'

# История команд
export HISTSIZE=10000
export HISTFILESIZE=20000
export HISTCONTROL=ignoredups:erasedups
shopt -s histappend

# Автодополнение для Git
if [ -f /usr/share/bash-completion/completions/git ]; then
    . /usr/share/bash-completion/completions/git
fi
```

Применить изменения:

```bash
source ~/.bashrc
```

### 1.5.3. Настройка Git

```bash
# Конфигурация Git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
git config --global core.editor "nano"

# Полезные алиасы Git
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm commit
git config --global alias.lg "log --oneline --graph --all --decorate"
git config --global alias.last "log -1 HEAD"

# Цветной вывод
git config --global color.ui auto

# Проверка конфигурации
git config --list
```

## 1.6. Создание структуры проекта на сервере

Создадим проект для курса на удаленном сервере.

### 1.6.1. Создание директории проекта

```bash
# Создаем рабочую директорию
mkdir -p ~/projects/container-dev-course
cd ~/projects/container-dev-course

# Инициализация Git
git init
```

### 1.6.2. Создание структуры проекта

```bash
# Создаем базовую структуру
mkdir -p {backend,frontend,database,deployment,docs}

# Создаем .gitignore
cat > .gitignore << 'EOF'
# Node.js
node_modules/
npm-debug.log
yarn-error.log
.env
.env.local
.env.*.local

# Logs
*.log
logs/

# Build
dist/
build/
.next/
out/

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Database
*.sql
*.sqlite
*.db

# Containers
.podman/
EOF
```

### 1.6.3. Создание README

```bash
cat > README.md << 'EOF'
# Container Development Course
```

Проект для изучения контейнерной разработки на AlmaLinux 9 и Podman 5.

## Архитектура

```mermaid
graph TB
    subgraph local["💻 macOS Developer"]
        vscode["VSCode + SSH Only"]
    end
    
    subgraph server["🖥️ AlmaLinux 9 Server"]
        subgraph podman["📦 Podman Containers"]
            backend["Backend<br/>Node.js 26"]
            frontend["Frontend<br/>Vite 6"]
            database["Database<br/>PostgreSQL 18"]
            redis["Cache<br/>Redis 8"]
        end
    end
    
    vscode -->|SSH + Remote Development| podman
    backend --> database
    backend --> redis
    frontend --> backend
    
    style local fill:#2196F3,stroke:#1976D2,stroke-width:3px,color:#fff
    style server fill:#FF9800,stroke:#F57C00,stroke-width:3px,color:#fff
    style podman fill:#9C27B0,stroke:#7B1FA2,stroke-width:2px,color:#fff
    style vscode fill:#0277BD,stroke:#01579B,stroke-width:2px,color:#fff
    style backend fill:#4CAF50,stroke:#388E3C,stroke-width:2px,color:#fff
    style frontend fill:#00BCD4,stroke:#0097A7,stroke-width:2px,color:#fff
    style database fill:#F44336,stroke:#D32F2F,stroke-width:2px,color:#fff
    style redis fill:#FF5722,stroke:#E64A19,stroke-width:2px,color:#fff
```

## Структура проекта

- `backend/` - Node.js API на Fastify + Prisma
- `frontend/` - React приложение на Vite
- `database/` - PostgreSQL схемы и миграции
- `deployment/` - Podman Compose конфигурации
- `docs/` - Документация проекта

## Требования

**Локальная машина (macOS):**
- VSCode с расширениями Remote-SSH и Dev Containers

**Удаленный сервер (AlmaLinux 9):**
- Podman 5.4+
- Node.js 26 LTS (в контейнере)
- PostgreSQL 18 (в контейнере)

## Быстрый старт

### 1. Подключение к серверу

```bash
# С локальной машины
ssh almalinux-dev
```

### 2. Запуск окружения разработки

```bash
# На сервере
cd ~/projects/container-dev-course
podman-compose up -d
```

### 3. Разработка в VSCode

1. Откройте VSCode
2. `F1` → `Remote-SSH: Connect to Host` → `almalinux-dev`
3. `File` → `Open Folder` → `~/projects/container-dev-course`
4. `F1` → `Dev Containers: Reopen in Container`

## Полезные команды

```bash
# Статус контейнеров
podman ps

# Логи
podman-compose logs -f

# Остановка
podman-compose down

# Пересборка
podman-compose up -d --build
```


### 1.6.4. Открытие проекта в VSCode

В VSCode (уже подключенном к серверу):

1. `File` → `Open Folder`
2. Выберите `/home/developer/projects/container-dev-course`
3. Нажмите `OK`

Проект открыт! Теперь вы работаете с файлами на удаленном сервере.

## 1.7. Настройка Dev Container для разработки

Создадим конфигурацию для разработки внутри контейнера.

### 1.7.1. Создание .devcontainer

```bash
# Создаем директорию для конфигурации
mkdir -p .devcontainer
```

### 1.7.2. Создание devcontainer.json

```bash
cat > .devcontainer/devcontainer.json << 'EOF'
{
  "name": "Container Dev Course",
  "dockerComposeFile": "docker-compose.yml",
  "service": "dev",
  "workspaceFolder": "/workspace",
  
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "ms-azuretools.vscode-docker",
        "prisma.prisma",
        "rangav.vscode-thunder-client",
        "eamodio.gitlens"
      ],
      "settings": {
        "terminal.integrated.defaultProfile.linux": "bash",
        "editor.formatOnSave": true,
        "editor.defaultFormatter": "esbenp.prettier-vscode",
        "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true
        },
        "files.autoSave": "onFocusChange"
      }
    }
  },
  
  "forwardPorts": [3000, 5173, 5432, 6379],
  "portsAttributes": {
    "3000": {
      "label": "Backend API",
      "onAutoForward": "notify"
    },
    "5173": {
      "label": "Frontend Dev Server",
      "onAutoForward": "openBrowser"
    },
    "5432": {
      "label": "PostgreSQL"
    },
    "6379": {
      "label": "Redis"
    }
  },
  
  "postCreateCommand": "echo 'Dev Container Ready!'",
  "remoteUser": "node"
}
EOF
```

### 1.7.3. Создание docker-compose.yml

```bash
cat > .devcontainer/docker-compose.yml << 'EOF'
version: '3.8'

services:
  dev:
    image: node:26-alpine
    volumes:
      - ..:/workspace:cached
    command: sleep infinity
    working_dir: /workspace
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://devuser:devpass@postgres:5432/devdb
      - REDIS_URL=redis://redis:6379
    networks:
      - dev-network
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:18-alpine
    restart: unless-stopped
    environment:
      POSTGRES_USER: devuser
      POSTGRES_PASSWORD: devpass
      POSTGRES_DB: devdb
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - dev-network
    ports:
      - "5432:5432"

  redis:
    image: redis:8-alpine
    restart: unless-stopped
    command: redis-server --appendonly yes
    volumes:
      - redis-data:/data
    networks:
      - dev-network
    ports:
      - "6379:6379"

networks:
  dev-network:
    driver: bridge

volumes:
  postgres-data:
  redis-data:
EOF
```

### 1.7.4. Открытие проекта в Dev Container

1. В VSCode нажмите `F1`
2. Введите: `Dev Containers: Reopen in Container`
3. Дождитесь сборки и запуска контейнеров
4. VSCode перезапустится внутри контейнера

**Проверка окружения:**

```bash
# Терминал теперь работает внутри контейнера!

# Проверяем версию Node.js
node --version
# v26.x.x

# Проверяем доступность PostgreSQL
nc -zv postgres 5432
# postgres [172.x.x.x] 5432 (postgresql) open

# Проверяем доступность Redis
nc -zv redis 6379
# redis [172.x.x.x] 6379 (redis) open

# Проверяем рабочую директорию
pwd
# /workspace
```

## 1.8. Настройка VSCode для продуктивной работы

### 1.8.1. Создание tasks.json

```bash
mkdir -p .vscode
cat > .vscode/tasks.json << 'EOF'
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Start All Services",
      "type": "shell",
      "command": "podman-compose -f .devcontainer/docker-compose.yml up -d",
      "problemMatcher": [],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "Stop All Services",
      "type": "shell",
      "command": "podman-compose -f .devcontainer/docker-compose.yml down",
      "problemMatcher": []
    },
    {
      "label": "View Logs",
      "type": "shell",
      "command": "podman-compose -f .devcontainer/docker-compose.yml logs -f",
      "problemMatcher": []
    },
    {
      "label": "Rebuild Containers",
      "type": "shell",
      "command": "podman-compose -f .devcontainer/docker-compose.yml up -d --build",
      "problemMatcher": []
    }
  ]
}
EOF
```

### 1.8.2. Создание settings.json

```bash
cat > .vscode/settings.json << 'EOF'
{
  "files.exclude": {
    "**/.git": true,
    "**/node_modules": true,
    "**/dist": true,
    "**/.DS_Store": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/.git": true
  },
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSave": "onFocusChange",
  "terminal.integrated.defaultProfile.linux": "bash"
}
EOF
```

### 1.8.3. Полезные горячие клавиши

**Работа с Remote:**
- `Cmd+Shift+P` → `Remote-SSH: Connect to Host` — подключение к серверу
- `Cmd+Shift+P` → `Dev Containers: Reopen in Container` — открыть в контейнере
- `Cmd+Shift+P` → `Dev Containers: Reopen Locally` — вернуться на сервер

**Работа с терминалом:**
- `` Ctrl+` `` — открыть/закрыть терминал
- `Cmd+Shift+5` — разделить терминал
- `Cmd+\` — разделить редактор

**Работа с задачами:**
- `Cmd+Shift+P` → `Tasks: Run Task` — запуск задачи
- `Cmd+Shift+B` — запуск задачи по умолчанию

## 1.9. Первый коммит

Зафиксируем начальную структуру проекта:

```bash
# Добавляем все файлы
git add .

# Создаем первый коммит
git commit -m "Initial project structure

- Added project directories
- Configured dev container
- Added VSCode settings
- Created README with architecture"

# Проверяем статус
git status
# On branch main
# nothing to commit, working tree clean

# Смотрим историю
git lg
```


## Практическое задание

**Задание 1: Настройка локальной машины**
1. Установите VSCode на macOS
2. Установите расширения Remote-SSH и Dev Containers
3. Проверьте, что расширения установлены

**Задание 2: Настройка SSH**
1. Сгенерируйте SSH-ключ
2. Скопируйте ключ на сервер
3. Настройте SSH config
4. Проверьте подключение

**Задание 3: Настройка сервера**
1. Подключитесь к серверу через SSH
2. Установите базовые инструменты
3. Настройте bash с алиасами
4. Настройте Git

**Задание 4: Создание проекта**
1. Создайте структуру проекта
2. Настройте .gitignore и README
3. Откройте проект в VSCode через Remote-SSH
4. Создайте конфигурацию Dev Container

**Задание 5: Работа в контейнере**
1. Откройте проект в Dev Container
2. Проверьте доступность Node.js, PostgreSQL, Redis
3. Создайте первый коммит
4. Проверьте, что все работает

## Проверка знаний

```bash
# 1. Проверка SSH подключения
ssh almalinux-dev
hostname
# Должно вывести: almalinux

# 2. Проверка структуры проекта
cd ~/projects/container-dev-course
tree -L 2
# Должна быть видна структура директорий

# 3. Проверка Git
git status
git log --oneline

# 4. Проверка окружения в контейнере (после открытия в Dev Container)
node --version
nc -zv postgres 5432
nc -zv redis 6379
```

## Troubleshooting

**Проблема: SSH не подключается**
```bash
# Проверка с подробным выводом
ssh -v almalinux-dev

# Проверка прав на ключи
chmod 600 ~/.ssh/almalinux_dev
chmod 700 ~/.ssh
```

**Проблема: VSCode не может установить сервер**
```bash
# На сервере удалите старую установку
rm -rf ~/.vscode-server

# Попробуйте подключиться снова
```

**Проблема: Dev Container не запускается**
```bash
# Проверьте статус Podman
systemctl --user status podman.socket

# Проверьте логи
podman-compose logs
```


