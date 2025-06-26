```
 _____ _____      _____      
|  ___|  ___|   _|  ___|     
| |_  | |_ | | | | |_        
|  _| |  _|| |_| |  _|       
|_|   |_|   \__,_|_|         
  ____ _   _ ___ ____  _____ 
 / ___| | | |_ _|  _ \| ____|
| |  _| | | || || | | |  _|  
| |_| | |_| || || |_| | |___ 
 \____|\___/|___|____/|_____|
```

[![FFUF Banner](https://github.com/ffuf/ffuf/blob/master/_img/ffuf_mascot_600.png)](https://github.com/ffuf/ffuf)

# 🛡️ FFUF Guide — Полный гайд по фаззингу директорий, файлов и параметров

📌 Автор: [Artemy-dev](https://github.com/Artemy-dev) | 📬 Telegram: [@Artemy_Develop](https://t.me/Artemy_Develop)

---

## ⚡ Что такое FFUF?

**FFUF (Fuzz Faster U Fool)** — CLI-инструмент для *фаззинга*, то есть автоматического перебора директорий, файлов, параметров, заголовков и даже тела запроса на веб-серверах.

> 🔍 Пример: поиск скрытых путей `/admin`, `/backup.zip`, `/test.php` с помощью словаря слов.

---

## 🛠️ Установка FFUF

### ✅ Через `go install`

> Необходим установленный язык **Go**.

```bash
go install github.com/ffuf/ffuf/v2@latest
```

### 📂 Пути установки

- **Linux/macOS**: `~/go/bin/ffuf`
- **Windows**: `%USERPROFILE%\go\bin\ffuf.exe`

---

## ⚙️ Добавление FFUF в PATH

### 💻 Linux/macOS

1. Открой файл конфигурации:
```bash
nano ~/.zshrc   # или ~/.bashrc
```

2. Добавь строку:
```bash
export PATH="$HOME/go/bin:$PATH"
```

3. Сохрани:
- `Ctrl + X` → `Y` → `Enter`

4. Примени изменения:
```bash
source ~/.zshrc
```

5. Проверь:
```bash
ffuf -h
```

### 🪟 Windows

1. Открой "Переменные среды" → "Path" → "Изменить"
2. Добавь:
```txt
C:\Users\USERNAME\go\bin
```
3. Перезапусти PowerShell и введи `ffuf -h`

---

## 📚 Параметры FFUF

### 🔧 Основные:
```
-u        Целевой URL (вместо FUZZ подставляются слова)
-w        Путь к словарю
-t        Потоки (по умолчанию 40)
-X        Метод запроса (GET, POST и т.д.)
-H        Заголовки (можно много)
-d        Тело запроса
-p        Прокси
--rate    Ограничение по RPS
--delay   Задержка между запросами
--timeout Таймаут ответа
--random-agent  Случайный User-Agent
```

### 🎯 Фильтрация и матчеры:
```
-mc       Показать ответы с указанным кодом (200,302)
-fc       Скрыть ответы с кодом
-ml/fl    По длине
-mr/fr    По тексту (Regex)
```

### 🔁 Рекурсия и генерация:
```
--recursion
--recursion-depth
--matcher-subdir
--input-num      Использовать числа
--input-cmd      Использовать команду (например, seq 1 100)
```

### 📤 Вывод:
```
-o     Файл для сохранения
-of    Формат: json, html, csv, md
-v     Verbose-режим
```

---

## 📂 Словари (Wordlists)

FFUF работает с обычными `.txt`-файлами, в которых:

```
admin
login
robots.txt
config.php
```

🔗 Основной источник: [SecLists](https://github.com/danielmiessler/SecLists)
```bash
git clone https://github.com/danielmiessler/SecLists.git
```

### 🧩 Полезные словари:

| Название | Назначение |
|----------|------------|
| `common.txt` | Базовый аудит директорий |
| `directory-list-2.3-medium.txt` | Глубокий аудит |
| `raft-large-directories.txt` | Максимальное покрытие |
| `burp-parameter-names.txt` | Имена параметров |
| `sql-injection.txt` | SQL-инъекции |

---

## 🎯 Как выбрать словарь под задачу?

| Сценарий | Словарь |
|----------|---------|
| Быстрый обзор сайта | `common.txt` |
| Глубокий аудит | `directory-list-2.3-medium.txt` |
| Поиск параметров | `burp-parameter-names.txt` |
| Тест инъекций | `sql-injection.txt`, `xss.txt` |
| Брутфорс форм | `passwords.txt`, `usernames.txt` |

---

## 🚀 Примеры команд

### 1. 🔎 Базовый фуззинг директорий
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -t 50
```

### 2. ✅ Показать только 200 и 302
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -mc 200,302
```

### 3. ❌ Скрыть 404 ошибки
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -fc 404
```

### 4. 🔄 Использование прокси (Burp)
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -p http://127.0.0.1:8080
```

### 5. 📑 Фуззинг параметров в URL
```bash
ffuf -u "https://example.com/page.php?FUZZ=test" -w ./params.txt
```

### 6. 🧪 Фуззинг значений параметров
```bash
ffuf -u "https://example.com/page.php?id=FUZZ" -w ./payloads.txt
```

### 7. 💉 SQL-инъекции
```bash
ffuf -u "https://example.com/page.php?id=FUZZ" -w ./sql-injection.txt -mc 200
```

### 8. 🔐 POST-атака на форму логина
```bash
ffuf -u https://example.com/login -X POST \
  -d "username=admin&password=FUZZ" \
  -w ./passwords.txt
```

### 9. 🔁 Рекурсивный фуззинг
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt --recursion --recursion-depth 2 -mc 200
```

### 10. 📦 Сохранение результата
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -o result.json -of json
```

### 11. 🎟️ Заголовки авторизации
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt \
  -H "Authorization: Bearer TOKEN" -H "X-Forwarded-For: 127.0.0.1"
```

### 12. 🧠 Мультифуззинг с несколькими словарями
```bash
ffuf -u https://example.com/FUZZ/BURP \
  -w ./common.txt -w ./params.txt:BURP
```

### 13. 🎯 FUZZ в заголовках, теле и параметрах

**Заголовок:**
```bash
ffuf -u https://example.com -H "X-Api-Version: FUZZ" -w ./versions.txt
```

**Имя заголовка:**
```bash
ffuf -u https://example.com -H "FUZZ: test" -w ./header-names.txt
```

**Тело запроса:**
```bash
ffuf -u https://example.com/api -X POST \
  -d '{"user":"admin","input":"FUZZ"}' \
  -H "Content-Type: application/json" -w ./payloads.txt
```

**Имя параметра:**
```bash
ffuf -u "https://example.com/page.php?FUZZ=value" -w ./param-names.txt
```

**Значение параметра:**
```bash
ffuf -u "https://example.com/page.php?id=FUZZ" -w ./numbers.txt
```

---

## 🧩 SEO Keywords

`ffuf`, `фаззинг`, `пентест`, `directory brute-force`, `bug bounty`, `web security`, `sql injection`, `content discovery`, `security tools`, `burp`, `authorization headers`

---

_🔥 Гайд создан с ❤️ специально для начинающих пентестеров. Делитесь, ставьте ⭐️ на [GitHub](https://github.com/Artemy-dev/ffuf-guide) и добавляйте свои примеры!_


