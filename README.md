[![FFUF Banner](https://github.com/ffuf/ffuf/blob/master/_img/ffuf_mascot_600.png)](https://github.com/ffuf/ffuf)

# 🛡️ FFUF Guide — Полный гайд по фаззингу директорий, файлов и параметров

📌 Автор: [Artemy-dev](https://github.com/Artemy-dev) | 📬 Telegram: [@Artemy_Develop](https://t.me/Artemy_Develop)

---

## 🗂️ Оглавление

### 🔹 Основы

* [⚡ Что такое FFUF?](#-что-такое-ffuf)
* [🛠️ Установка FFUF](#-установка-ffuf)
* [⚙️ Добавление FFUF в PATH](#-добавление-ffuf-в-path)

### 🔹 Работа с FFUF

* [📚 Параметры FFUF](#-параметры-ffuf)
* [📂 Словари (Wordlists)](#-словари-wordlists)
* [🎯 Как выбрать словарь под задачу?](#-как-выбрать-словарь-под-задачу)

### 🔹 Практика: Примеры команд

1. [🔎 Базовый фуззинг директорий](#1-базовый-фуззинг-директорий)
2. [✅ Показать только 200 и 302](#2-показать-только-200-и-302)
3. [❌ Скрыть 404 ошибки](#3-скрыть-404-ошибки)
4. [🔄 Использование прокси (Burp)](#4-использование-прокси-burp)
5. [📑 Фуззинг параметров в URL](#5-фуззинг-параметров-в-url)
6. [🧪 Фуззинг значений параметров](#6-фуззинг-значений-параметров)
7. [💉 SQL-инъекции](#7-sql-инъекции)
8. [🔐 POST-атака на форму логина](#8-post-атака-на-форму-логина)
9. [🔁 Рекурсивный фуззинг](#9-рекурсивный-фуззинг)
10. [📦 Сохранение результата](#10-сохранение-результата)
11. [🎟️ Заголовки авторизации](#11-заголовки-авторизации)
12. [🧠 Мультифуззинг с несколькими словарями](#12-мультифуззинг-с-несколькими-словарями)
13. [🎯 FUZZ в заголовках, теле и параметрах](#13-fuzz-в-заголовках-теле-и-параметрах)

### 🔹 Безопасность и нагрузка

* [🧨 Возможность DoS при фаззинге](#-возможность-dos-при-фаззинге)
* [🔍 Анонимность и скрытность с помощью Tor](#-анонимность-и-скрытность-с-помощью-tor)

### 🔹 Прочее

* [🧩 SEO Keywords](#-seo-keywords)

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
| [common.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common.txt) | Базовый аудит директорий |
| [directory-list-2.3-medium.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/directory-list-2.3-medium.txt) | Глубокий аудит |
| [raft-large-directories.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/raft-large-directories.txt) | Максимальное покрытие |
| [burp-parameter-names.txt](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/burp-parameter-names.txt) | Имена параметров |
| [sql-injection.txt](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/sql-injection.txt) | SQL-инъекции |

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

### 1. Базовый фуззинг директорий
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -t 50
```

```
ffuf -u https://example.com/FUZZ -w ./common.txt -t 50

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __ /\ \__/  __   
       \ \ ,__\\ \ ,__\/\ \/\ \\\ \ ,__\/\_\  
        \ \ \_/ \ \ \_/\ \ \_\ \\ \ \_\/_/_  
         \ \_\   \ \_\  \ \____/ \ \_\/\_\_\ 
          \/_/    \/_/   \/___/   \/_/\/_/_/  v2.1

________________________________________________

 :: Method           : GET
 :: URL              : https://example.com/FUZZ
 :: Wordlist         : ./common.txt
 :: Threads          : 50
 :: Follow redirects : false
 :: Timeout          : 10
________________________________________________

admin                 [Status: 200, Size: 192, Words: 20]
login                 [Status: 200, Size: 205, Words: 24]
config.php            [Status: 200, Size: 132, Words: 11]
backup.zip            [Status: 403, Size: 88, Words: 10]
robots.txt            [Status: 200, Size: 67, Words: 4]

:: Progress: 1000 / 1000 (100.00%)
```

### 2. Показать только 200 и 302
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -mc 200,302
```

### 3. Скрыть 404 ошибки
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -fc 404
```

### 4. Использование прокси (Burp)
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -p http://127.0.0.1:8080
```

### 5. Фуззинг параметров в URL
```bash
ffuf -u "https://example.com/page.php?FUZZ=test" -w ./params.txt
```

```
ffuf -u "https://example.com/page.php?FUZZ=test" -w ./params.txt

FUZZING PARAM NAMES ➜ page.php?FUZZ=test

username              [Status: 200, Words: 13]
search                [Status: 200, Words: 17]
id                    [Status: 200, Words: 19]
query                 [Status: 200, Words: 21]

:: Found 4 valid parameters
```

### 6. Фуззинг значений параметров
```bash
ffuf -u "https://example.com/page.php?id=FUZZ" -w ./payloads.txt
```

### 7. SQL-инъекции
```bash
ffuf -u "https://example.com/page.php?id=FUZZ" -w ./sql-injection.txt -mc 200
```

```
ffuf -u "https://example.com/page.php?id=FUZZ" -w ./sql-injection.txt -mc 200

TRYING COMMON SQLI PAYLOADS...

1' OR '1'='1           [200] ➜ Login bypass suspected
admin'--              [200] ➜ SQL comment injection
1 AND 1=1             [200] ➜ Valid logic
' OR 1=1--            [200] ➜ Possible vulnerability

:: Total hits: 4 / 50
```

### 8. POST-атака на форму логина
```bash
ffuf -u https://example.com/login -X POST \
  -d "username=admin&password=FUZZ" \
  -w ./passwords.txt
```

```
ffuf -u https://example.com/login -X POST \
  -d "username=admin&password=FUZZ" \
  -w ./passwords.txt

🧪 TESTING PASSWORDS FOR USER 'admin'

admin                 [Status: 401]
123456                [Status: 401]
password              [Status: 401]
letmein               [Status: 401]
admin123              [Status: 200] ✅
qwerty                [Status: 401]

→ FOUND VALID PASSWORD: admin123
```

### 9. Рекурсивный фуззинг
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt --recursion --recursion-depth 2 -mc 200
```

### 10. Сохранение результата
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -o result.json -of json
```

### 11. Заголовки авторизации
```bash
ffuf -u https://example.com/FUZZ -w ./common.txt \
  -H "Authorization: Bearer TOKEN" -H "X-Forwarded-For: 127.0.0.1"
```

```
ffuf -u https://example.com/FUZZ -w ./common.txt \
  -H "Authorization: Bearer TOKEN"

Testing auth-protected paths...

/dashboard            [Status: 200]
/admin                [Status: 403]
/me                   [Status: 200]
/settings             [Status: 200]

:: 3 accessible endpoints found with token
```

### 12. Мультифуззинг с несколькими словарями
```bash
ffuf -u https://example.com/FUZZ/BURP \
  -w ./common.txt -w ./params.txt:BURP
```

### 13. FUZZ в заголовках, теле и параметрах

**Заголовок:**
```bash
ffuf -u https://example.com -H "X-Api-Version: FUZZ" -w ./versions.txt
```

```
ffuf -u https://example.com/api -X POST \
  -d '{"user":"admin","input":"FUZZ"}' \
  -H "Content-Type: application/json" -w ./payloads.txt

[+] Injecting payloads into JSON

FUZZ ➜ ' OR 1=1--              [200]
FUZZ ➜ <script>alert(1)</script>  [403]
FUZZ ➜ ../../../../etc/passwd [403]
FUZZ ➜ {"$ne": null}           [200]

:: 2 responses with code 200 — possible vector
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

## 🧨 Возможность DoS при фаззинге

> 🛑 **Важно!** DoS-атаки — это незаконные действия, если проводятся без разрешения владельца ресурса. Используй ffuf только в рамках легального пентестинга.<br>
⚠️ Не используй ffuf для DoS. Это **инструмент для тестирования безопасности**, поиска уязвимостей и багов. Использование его в целях перегрузки серверов **может повлечь юридическую ответственность**.

---

### ⚙️ Как не “убить” сервер случайно

| Параметр    | Что делает                                                              |
| ----------- | ----------------------------------------------------------------------- |
| `-t`        | Кол-во одновременных потоков. Не ставь слишком много (оптимум: 10–50)   |
| `--delay`   | Задержка между запросами. Например, `--delay 0.1` = 100 мс              |
| `--rate`    | Ограничение запросов в секунду. Например, `--rate 50` — максимум 50 RPS |
| `--timeout` | Таймаут ожидания ответа. Лучше не занижать слишком сильно               |

---

### ✅ Пример “бережного” фуззинга

```bash
ffuf -u https://example.com/FUZZ -w ./common.txt \
  -t 20 --delay 0.2 --rate 50 --timeout 10
```

🔹 Потоков: 20<br>
🔹 Задержка: 200 мс между запросами<br>
🔹 Ограничение: 50 запросов/сек<br>
🔹 Таймаут: 10 секунд

---

### 🔐 Как понять, что ты перегружаешь цель?

* Появляются коды **502**, **503**, **504**
* Сайт начинает “лагать”
* Возникают **Rate limit exceeded**
* Сервер перестаёт отвечать на любые запросы
* В консоли ffuf видно большое количество таймаутов или "Connection reset"

---

## 🔍 Анонимность и скрытность с помощью Tor

Для скрытия своего реального IP-адреса и анонимного фаззинга можно использовать Tor. Это простой способ направлять трафик через сеть анонимных прокси.

---

### 1. Установка Tor Browser

* Скачай и установи [Tor Browser](https://www.torproject.org/).
* Запусти браузер, чтобы сеть Tor стартовала локально.

---

### 2. Определение порта SOCKS5 прокси Tor

* В адресной строке Tor Browser введи:

  ```
  about:config
  ```
* Нажми **"I accept the risk"** (если появится предупреждение).
* В поле поиска введи:

  ```
  network.proxy.socks_port
  ```
* Посмотри значение порта — обычно это `9050` или `9150`.

---

### 3. Проверка работы прокси Tor через curl

В терминале выполни команду с найденным портом:

```bash
curl --socks5 127.0.0.1:9150 https://ifconfig.me
```

**Что должно быть:**

* В ответ ты увидишь IP-адрес Tor-сети, а не свой настоящий IP.
* Если возникла ошибка подключения — проверь, что Tor Browser запущен.

---

### 4. Использование ffuf через Tor SOCKS5 прокси

Запускай ffuf с указанием прокси:

```bash
ffuf -u https://example.com/FUZZ -w ./common.txt -p socks5://127.0.0.1:9150
```

* `-p socks5://127.0.0.1:9150` — указывает использовать Tor-сеть для всех запросов.
* Это позволит скрыть твой реальный IP при фаззинге.

---

Так ты будешь работать анонимно и безопасно, направляя трафик через сеть Tor.

---

## 🧩 SEO Keywords

`ffuf`, `фаззинг`, `пентест`, `directory brute-force`, `bug bounty`, `web security`, `sql injection`, `content discovery`, `security tools`, `burp`, `authorization headers`

---

_🔥 Гайд создан с ❤️ специально для начинающих пентестеров. Делитесь, ставьте ⭐️ на [GitHub](https://github.com/Artemy-dev/ffuf-guide) и добавляйте свои примеры!_


