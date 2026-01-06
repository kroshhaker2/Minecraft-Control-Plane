# Minecraft Control Plane

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://mit-license.org/)
[![Rust](https://img.shields.io/badge/Rust-1.92+-orange?logo=rust)](https://blog.rust-lang.org/2025/12/11/Rust-1.92.0/)
[![Node.js](https://img.shields.io/badge/Node.js-24-green?logo=node.js)](https://nodejs.org/en/blog/release/v24.12.0)
[![Build Status](https://img.shields.io/github/actions/workflow/status/kroshhaker2/mcp-agent/rust.yml?branch=main)](https://github.com/kroshhaker2/mcp-agent.git)
[![Master Status](https://img.shields.io/github/actions/workflow/status/kroshhaker2/mcp-master/nodejs.yml?branch=main)](https://github.com/kroshhaker2/mcp-master.git)
[![Web Status](https://img.shields.io/github/actions/workflow/status/kroshhaker2/mcp-web/nodejs.yml?branch=main)](https://github.com/kroshhaker2/mcp-web.git)

**Distributed Master–Agents система для управления серверами Minecraft**

Проект предназначен для централизованного управления множеством серверов Minecraft через Master–Agent архитектуру. Система позволяет запускать, останавливать и перезагружать серверы, получать логи в реальном времени, передавать файлы и выполнять команды RCON.

---

## Архитектура

```text
Браузер
   │  (HTTP + WebSocket)
   ▼
Master (Express + TypeScript)
   │  (gRPC)
   ▼
Agent (Rust)
   ├── Minecraft server #1
   ├── Minecraft server #2
   └── ...
````

**Компоненты:**

* **Master (Control Plane)**

    * Отвечает за авторизацию и UI
    * Оркестрация серверов через gRPC агенты
    * REST API + WebSocket для браузера

* **Agent**

    * Rust-программа, развёртываемая на хосте с Minecraft-серверами
    * Управляет процессами, логами, файлами и RCON
    * Обменивается данными с Master через gRPC

---

## Особенности

* Запуск, остановка и перезагрузка серверов Minecraft
* Потоковое получение логов в реальном времени
* Загрузка своего ядра или из встроенного магазина
* Передача файлов (backups, конфиги, world)
* Информация о состоянии процессов
* Поддержка RCON-команд
* Разделение ответственности: Master не управляет процессами напрямую
* Безопасность: агент работает с минимальными правами, обмен через аутентифицированный gRPC

---

## Технологии

* **Master:** Node.js + Express + TypeScript
* **Agent:** Rust + Tokio + Tonic (gRPC)
* **Протокол:** gRPC + Protobuf

---

## Установка и запуск

### Agent

1. Клонировать репозиторий агента:

```bash
git clone https://github.com/kroshhaker2/mcp-agent.git
cd mc-agent
```

2. Настроить `config.toml` (адрес Master, токен, директории серверов)
3. Собрать и запустить:

```bash
cargo build --release
./target/release/mc-agent
```

### Master

1. Клонировать репозиторий Master:

```bash
git clone https://github.com/kroshhaker2/mcp-master.git
cd mc-master
```

2. Установить зависимости:

```bash
npm install
```

3. Настроить `.env` (JWT секрет, база данных и т.д.)
4. Запустить сервер:

```bash
npm run dev
```

### Web

1. Клонировать репозиторий Web

```bash
git clone https://github.com/kroshhaker2/mcp-web.git
cd mc-web
```

2. Установить зависимости:

```bash
npm install
```

3. Настроить `.env`
4. Запустить сервер:

```bash
npm run dev
```

---

## Контакт и вклад

Если вы хотите внести вклад, создать issue или предложить улучшение, пожалуйста, откройте PR или issue в соответствующем репозитории агента или протокола.

---

## План развития

* Поддержка нескольких агентов на одном Master
* Поддержка Docker-контейнеров
* Web UI с отображением статусов серверов и логов
* Возможность управления пользователями и ролями
* Версионирование протокола gRPC
