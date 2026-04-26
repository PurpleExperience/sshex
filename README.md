<div align="center">
	<h1>SSHeX — Массовое управление SSH серверами</h1>
	<h1>Bulk SSH Server Management</h1>
</div>

<div align="center">

📬 Автор / Author: [@alex_dev404](https://t.me/alex_dev404) | Telegram: [@SSHeX_license_bot](https://t.me/SSHeX_license_bot) | Лицензия / License: Commercial

<br>

**[🇷🇺 Русский](#-русский) · [🇬🇧 English](#-english)**

</div>

---

# 🇷🇺 Русский

> Один `.exe` файл. Без установки. Windows 7 / 10 / 11.  
> Выполняйте команды, загружайте файлы и проверяйте SSH на сотнях серверов одновременно.

## Содержание

- [📋 Требования](#-требования)
- [⚖️ Сравнение с аналогами](#️-сравнение-с-аналогами)
- [🔄 Типичный рабочий процесс](#-типичный-рабочий-процесс)
- [🚀 Быстрый старт](#-быстрый-старт)
- [📡 Проверка SSH](#-проверка-ssh)
- [⚡ Отправка команд](#-отправка-команд)
- [📁 SFTP — загрузка файлов](#-sftp--загрузка-файлов)
- [🖥️ CLI режим](#️-cli-режим)
- [⏰ Планировщик задач Windows](#-планировщик-задач-windows)
- [📊 Excel отчёты](#-excel-отчёты)
- [🔍 Сценарии использования](#-сценарии-использования)
- [❓ Часто задаваемые вопросы](#-часто-задаваемые-вопросы)

---

## 📋 Требования

**Локальный компьютер:**
- Windows 7 / 10 / 11
- ОЗУ: от 4 ГБ (для параллельной загрузки файлов >10 ГБ — от 16 ГБ)
- SSD (NVMe/SATA) при работе с большими файлами
- Быстрая сеть (1 Гбит/с рекомендуется для массовых операций)

**Удалённые серверы:**
- Открытый порт 22 (SSH)
- Свободное место на диске (при загрузке файлов)

---

## ⚖️ Сравнение с аналогами

| Задача | SSHeX | Ansible | MobaXterm / PuTTY |
|---|---|---|---|
| Выполнить команду на 50 серверах | ✅ Сразу | ⚙️ Нужен playbook | ❌ Только 1 сервер |
| Загрузить файл на 100 серверов | ✅ Встроено | ✅ | ❌ |
| Работа без установки на Windows | ✅ Один .exe | ❌ | ✅ |
| Интерактивный терминал | ❌ | ❌ | ✅ |
| Excel-отчёт по результатам | ✅ | ❌ | ❌ |
| Планировщик задач Windows | ✅ CLI режим | ⚙️ Cron | ❌ |

**SSHeX закрывает нишу:** _"Мне нужно выполнить команду на 50 серверах прямо сейчас, и я не хочу настраивать Ansible."_

---

## 🔄 Типичный рабочий процесс

```
ШАГ 1 — SSHeX: Ping          → Кто из 100 серверов вообще отвечает? (10–15 сек)
ШАГ 2 — SSHeX: Проверить SSH → На каких работает SSH и верны ли пароли?
ШАГ 3 — SSHeX: Команды       → Выполнить задачу на всех доступных серверах
ШАГ 4 — MobaXterm / PuTTY    → Разобраться с проблемными вручную
ШАГ 5 — SSHeX: Проверка снова → Убедиться что всё исправлено
ШАГ 6 — Планировщик Windows  → Автоматически каждую ночь, отчёт утром
```

**Пример:**
```
Пн 08:00 — планировщик проверил 200 серверов
Пн 08:05 — Excel готов: 185 OK, 10 нет связи, 5 отказ
Пн 08:10 — SSHeX отправил команды на 185 серверов
Пн 08:30 — через MobaXterm разобрались с 15 проблемными
Вт 08:00 — уже 198 OK
```

---

## 🚀 Быстрый старт

### 1. Первый запуск

При первом запуске создаётся папка `profiles\` рядом с `.exe`:

```
SSHeX.exe
state.json
templates.json
profiles\
  web_servers\
    credentials.txt   ← список серверов
    commands.txt      ← команды для выполнения
    targets.txt       ← серверы прошедшие проверку SSH
    reports\          ← отчёты
    debug_output\     ← подробный вывод по каждому серверу
```

### 2. Профили

Профиль — группа серверов по назначению (например: `web_servers`, `db_servers`).

```
Создать:    Нажать "+ Создать профиль" в сайдбаре
Открыть:    Двойной клик по профилю
Контекстное меню (правый клик):
  - Переименовать
  - Дублировать
  - Открыть папку
  - Удалить
```

### 3. credentials.txt — список серверов

Три формата аутентификации:

```
# Пароль
10.35.36.28:admin:password123

# SSH ключ
10.35.36.29:root:/home/user/.ssh/id_rsa

# SSH ключ + пароль ключа
10.35.36.31:root:/home/user/.ssh/id_rsa|mypassphrase
```

Поддерживаемые форматы ключей: RSA, Ed25519, ECDSA, DSS (OpenSSH/PEM).  
В одном профиле можно смешивать пароли и ключи.

### 4. commands.txt — команды

Каждая строка — отдельная команда. `#` — комментарий.

```bash
# Linux
df -h
uptime
free -m

# Windows
systeminfo
tasklist
ipconfig /all
```

**Загрузка файла через SFTP:**
```
UPLOAD:C:\scripts\deploy.sh|/opt/scripts/
```

**Загрузить и сразу выполнить:**
```
UPLOAD:C:\scripts\setup.sh|/opt/scripts/
chmod +x /opt/scripts/setup.sh
/opt/scripts/setup.sh
```

---

## 📡 Проверка SSH

Что делает:
- Подключается к каждому серверу из `credentials.txt`
- Записывает успешные IP в `targets.txt`
- Создаёт отчёт в `reports\`

Результаты:
```
УСПЕХ          — подключение успешно
ОТКАЗ          — неверный логин или пароль
СМЕНА ПАРОЛЯ   — требуется смена пароля на сервере
НЕТ СВЯЗИ      — сервер недоступен или порт 22 закрыт
ОШИБКА         — другая ошибка
```

> ⚠️ Всегда выполняйте проверку SSH перед отправкой команд. `targets.txt` очищается при каждой новой проверке.

---

## ⚡ Отправка команд

- Берёт список серверов из `targets.txt`
- Выполняет команды из `commands.txt` на каждом
- Сохраняет вывод в `debug_output\`
- Создаёт сводный отчёт в `reports\`

**Режим "Без Ctrl+C"** — для смены пароля и интерактивных команд:

```
Смена пароля (passwd)                          → Без Ctrl+C ✅
Серверы с меню в .bashrc / .bash_profile       → Без Ctrl+C ✅
Массовая отправка команд на чистые серверы     → Обычный ✅
```

> ⚠️ При одновременной работе нескольких профилей порядок выполнения между ними не гарантируется. Объединяйте зависимые команды в один профиль.

---

## 📁 SFTP — загрузка файлов

```
UPLOAD:C:\scripts\deploy.sh|/opt/scripts/
UPLOAD:C:\install\agent.msi|C:\temp\
```

Прогресс в логе (обновляется каждые 5%):
```
UPLOAD 10.0.0.1 [████░░░░░░░░░░░░░░░░]  20%  2.4/12.0 МБ  18.3 МБ/с
UPLOAD 10.0.0.1 [████████████████████] 100%  12.0/12.0 МБ  21.7 МБ/с
```

При нажатии "Стоп" — недокачанные файлы **автоматически удаляются** со всех серверов.

### Слоты UPLOAD

Ограничивает количество одновременных SFTP передач:

```
Интернет-канал (медленный)   → 1–2 слота
Локальная сеть 100 МБит      → 2–3 слота (по умолчанию)
Локальная сеть 1 ГБит        → 3–5 слотов
Локальная сеть 10 ГБит       → 5–10 слотов
```

---

## 🖥️ CLI режим

Создайте `SSHeX_run.bat` рядом с `.exe`:

```bat
@echo off
chcp 1251 > nul
SSHeX.exe %* > "%TEMP%\SSHeX_out.txt" 2>nul
type "%TEMP%\SSHeX_out.txt"
del "%TEMP%\SSHeX_out.txt"
```

**Команды:**
```bat
SSHeX_run.bat --profile web_servers --check
SSHeX_run.bat --profile web_servers --check --workers 20
SSHeX_run.bat --profile db_servers --send
SSHeX_run.bat --profile db_servers --send --workers 10 --upload-slots 5
SSHeX_run.bat --profile web_servers --ping
SSHeX_run.bat --version
```

**Параметры:**
```
--profile NAME    имя профиля (обязательно)
--check           проверка SSH
--send            отправка команд
--ping            ping проверка
--workers N       количество потоков (по умолчанию 10)
--upload-slots N  слотов одновременной загрузки (по умолчанию 3)
```

---

## ⏰ Планировщик задач Windows

```bat
:: Проверка SSH каждую ночь в 02:00
schtasks /create /tn "SSHeX_check_web" ^
  /tr "C:\tools\SSHeX_run.bat --profile web_servers --check --workers 20" ^
  /sc daily /st 02:00 /ru SYSTEM /f

:: Отправка команд каждый понедельник в 06:00
schtasks /create /tn "SSHeX_send_db" ^
  /tr "C:\tools\SSHeX_run.bat --profile db_servers --send" ^
  /sc weekly /d MON /st 06:00 /ru SYSTEM /f
```

Отчёты сохраняются в: `profiles\ИМЯ_ПРОФИЛЯ\reports\`

---

## 📊 Excel отчёты

Включить: поставить галочку "Excel отчёт" в тулбаре.

Содержимое:
- Имя профиля и дата
- Таблица: IP, статус, причина, время
- Цветовое выделение (зелёный / красный / серый)
- Итоговая статистика

Файлы:
```
check_ДД_ММ_ГГГГ_ЧЧ_ММ_СС.xlsx    — после проверки SSH
commands_ДД_ММ_ГГГГ_ЧЧ_ММ_СС.xlsx — после отправки команд
```

---

## 🔍 Сценарии использования

<details>
<summary><b>Сценарий 1: Массовая проверка доступности</b></summary>

```
1. Создать профиль "all_servers"
2. Заполнить credentials.txt
3. Нажать "Ping" — кто отвечает
4. Нажать "Проверить SSH"
5. Открыть Excel отчёт
```
</details>

<details>
<summary><b>Сценарий 2: Деплой скрипта на 50 серверов</b></summary>

```
commands.txt:
  UPLOAD:C:\deploy\install.sh|/tmp/
  chmod +x /tmp/install.sh
  /tmp/install.sh
  rm /tmp/install.sh
```
</details>

<details>
<summary><b>Сценарий 3: Сбор логов со всех серверов</b></summary>

```
commands.txt:
  tail -100 /var/log/syslog
  journalctl -n 50 --no-pager

→ Открыть папку debug_output\ — логи каждого сервера
```
</details>

<details>
<summary><b>Сценарий 4: Массовая смена пароля</b></summary>

```
commands.txt:
  echo "user:newpassword" | chpasswd

→ Включить "Без Ctrl+C" перед запуском
```
</details>

<details>
<summary><b>Сценарий 5: Перезагрузка после обновления</b></summary>

```
# linux_servers → commands.txt
reboot

# win_servers → commands.txt
shutdown /r /t 0

→ Открыть обе вкладки и запустить одновременно
```
</details>

---

## ❓ Часто задаваемые вопросы

**Проверка занимает 10 секунд на каждый сервер?**  
Это таймаут SSH (10 сек). Используйте Ping перед проверкой чтобы исключить недоступные. Увеличьте количество потоков для ускорения.

**Команды не выполняются на Windows серверах?**  
Программа определяет Windows по SSH баннеру. Убедитесь что OpenSSH установлен:
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

**UPLOAD не работает — файл не найден?**  
Разделитель `|`, а не `:`.  
✅ `UPLOAD:C:\scripts\file.sh|/tmp/`  
❌ `UPLOAD:C:\scripts\file.sh:/tmp/`

**targets.txt пуст после проверки?**  
Ни один сервер не прошёл. Проверьте `credentials.txt` и откройте `Debug ▼` для диагностики.

**Как найти IP в логе?**  
`Ctrl+F` — встроенный поиск с навигацией ▲▼.

---

<br>
<br>

---

# 🇬🇧 English

> Single `.exe` file. No installation. Windows 7 / 10 / 11.  
> Run commands, upload files, and check SSH on hundreds of servers simultaneously.

## Table of Contents

- [📋 Requirements](#-requirements)
- [⚖️ Comparison with alternatives](#️-comparison-with-alternatives)
- [🔄 Typical workflow](#-typical-workflow)
- [🚀 Quick start](#-quick-start)
- [📡 SSH check](#-ssh-check)
- [⚡ Sending commands](#-sending-commands)
- [📁 SFTP — file upload](#-sftp--file-upload)
- [🖥️ CLI mode](#️-cli-mode)
- [⏰ Windows Task Scheduler](#-windows-task-scheduler)
- [📊 Excel reports](#-excel-reports)
- [🔍 Use cases](#-use-cases)
- [❓ FAQ](#-faq)

---

## 📋 Requirements

**Local machine:**
- Windows 7 / 10 / 11
- RAM: 4 GB minimum (16 GB for parallel uploads >10 GB)
- SSD (NVMe/SATA) for large file operations
- Fast network (1 Gbit/s recommended for bulk operations)

**Remote servers:**
- Port 22 open (SSH)
- Sufficient free disk space (when uploading files)

---

## ⚖️ Comparison with alternatives

| Task | SSHeX | Ansible | MobaXterm / PuTTY |
|---|---|---|---|
| Run command on 50 servers | ✅ Instantly | ⚙️ Needs a playbook | ❌ 1 server only |
| Upload file to 100 servers | ✅ Built-in | ✅ | ❌ |
| No installation on Windows | ✅ Single .exe | ❌ | ✅ |
| Interactive terminal | ❌ | ❌ | ✅ |
| Excel report of results | ✅ | ❌ | ❌ |
| Windows Task Scheduler | ✅ CLI mode | ⚙️ Cron | ❌ |

**SSHeX fills the gap:** _"I need to run a command on 50 servers right now without setting up Ansible."_

---

## 🔄 Typical workflow

```
STEP 1 — SSHeX: Ping          → Which of 100 servers respond?        (10–15 sec)
STEP 2 — SSHeX: Check SSH     → Where SSH works, are credentials OK?
STEP 3 — SSHeX: Commands      → Execute task on all available servers
STEP 4 — MobaXterm / PuTTY   → Handle problematic servers manually
STEP 5 — SSHeX: Check again   → Confirm everything is fixed
STEP 6 — Task Scheduler       → Automatically every night, report in the morning
```

**Example:**
```
Mon 08:00 — scheduler checked 200 servers
Mon 08:05 — Excel ready: 185 OK, 10 unreachable, 5 rejected
Mon 08:10 — SSHeX sent commands to 185 servers
Mon 08:30 — handled 15 problematic servers via MobaXterm
Tue 08:00 — already 198 OK
```

---

## 🚀 Quick start

### 1. First launch

On first launch, a `profiles\` folder is created next to the `.exe`:

```
SSHeX.exe
state.json
templates.json
profiles\
  web_servers\
    credentials.txt   ← server list
    commands.txt      ← commands to execute
    targets.txt       ← servers that passed SSH check
    reports\          ← reports
    debug_output\     ← detailed per-server output
```

### 2. Profiles

A profile is a group of servers by purpose (e.g. `web_servers`, `db_servers`).

```
Create:    Click "+ Create profile" in the sidebar
Open:      Double-click on a profile
Context menu (right-click):
  - Rename
  - Duplicate
  - Open folder
  - Delete
```

### 3. credentials.txt — server list

Three authentication formats:

```
# Password
10.35.36.28:admin:password123

# SSH key
10.35.36.29:root:/home/user/.ssh/id_rsa

# SSH key + passphrase
10.35.36.31:root:/home/user/.ssh/id_rsa|mypassphrase
```

Supported key formats: RSA, Ed25519, ECDSA, DSS (OpenSSH/PEM).  
Passwords and keys can be mixed within a single profile.

### 4. commands.txt — commands

Each line is a separate command. `#` starts a comment.

```bash
# Linux
df -h
uptime
free -m

# Windows
systeminfo
tasklist
ipconfig /all
```

**Upload file via SFTP:**
```
UPLOAD:C:\scripts\deploy.sh|/opt/scripts/
```

**Upload and immediately execute:**
```
UPLOAD:C:\scripts\setup.sh|/opt/scripts/
chmod +x /opt/scripts/setup.sh
/opt/scripts/setup.sh
```

---

## 📡 SSH check

What it does:
- Connects to each server in `credentials.txt`
- Writes successful IPs to `targets.txt`
- Creates a report in `reports\`

Results:
```
SUCCESS          — connection successful
REJECTED         — wrong login or password
CHANGE PASSWORD  — password change required on server
NO CONNECTION    — server unreachable or port 22 closed
ERROR            — other error
```

> ⚠️ Always run an SSH check before sending commands. `targets.txt` is cleared on each new check.

---

## ⚡ Sending commands

- Reads server list from `targets.txt`
- Executes commands from `commands.txt` on each server
- Saves output to `debug_output\`
- Creates a summary report in `reports\`

**"No Ctrl+C" mode** — for password changes and interactive commands:

```
Password change (passwd)                        → No Ctrl+C ✅
Servers with menus in .bashrc / .bash_profile   → No Ctrl+C ✅
Bulk commands on clean servers                  → Normal ✅
```

> ⚠️ When running multiple profiles simultaneously, execution order between profiles is not guaranteed. Combine dependent commands into a single profile.

---

## 📁 SFTP — file upload

```
UPLOAD:C:\scripts\deploy.sh|/opt/scripts/
UPLOAD:C:\install\agent.msi|C:\temp\
```

Progress in the log (updated every 5%):
```
UPLOAD 10.0.0.1 [████░░░░░░░░░░░░░░░░]  20%  2.4/12.0 MB  18.3 MB/s
UPLOAD 10.0.0.1 [████████████████████] 100%  12.0/12.0 MB  21.7 MB/s
```

On "Stop" — incomplete files are **automatically deleted** from all servers.

### UPLOAD slots

Limits the number of simultaneous SFTP transfers:

```
Internet connection (slow)   → 1–2 slots
LAN 100 Mbit                 → 2–3 slots (default)
LAN 1 Gbit                   → 3–5 slots
LAN 10 Gbit                  → 5–10 slots
```

---

## 🖥️ CLI mode

Create `SSHeX_run.bat` next to the `.exe`:

```bat
@echo off
chcp 1251 > nul
SSHeX.exe %* > "%TEMP%\SSHeX_out.txt" 2>nul
type "%TEMP%\SSHeX_out.txt"
del "%TEMP%\SSHeX_out.txt"
```

**Commands:**
```bat
SSHeX_run.bat --profile web_servers --check
SSHeX_run.bat --profile web_servers --check --workers 20
SSHeX_run.bat --profile db_servers --send
SSHeX_run.bat --profile db_servers --send --workers 10 --upload-slots 5
SSHeX_run.bat --profile web_servers --ping
SSHeX_run.bat --version
```

**Parameters:**
```
--profile NAME    profile name (required)
--check           SSH check
--send            send commands
--ping            ping check
--workers N       thread count (default 10)
--upload-slots N  simultaneous upload slots (default 3)
```

---

## ⏰ Windows Task Scheduler

```bat
:: SSH check every night at 02:00
schtasks /create /tn "SSHeX_check_web" ^
  /tr "C:\tools\SSHeX_run.bat --profile web_servers --check --workers 20" ^
  /sc daily /st 02:00 /ru SYSTEM /f

:: Send commands every Monday at 06:00
schtasks /create /tn "SSHeX_send_db" ^
  /tr "C:\tools\SSHeX_run.bat --profile db_servers --send" ^
  /sc weekly /d MON /st 06:00 /ru SYSTEM /f
```

Reports are saved to: `profiles\PROFILE_NAME\reports\`

---

## 📊 Excel reports

Enable: check "Excel report" in the toolbar.

Contents:
- Profile name and date
- Table: IP, status, reason, time
- Color coding (green / red / gray)
- Summary statistics

Files:
```
check_DD_MM_YYYY_HH_MM_SS.xlsx    — after SSH check
commands_DD_MM_YYYY_HH_MM_SS.xlsx — after sending commands
```

---

## 🔍 Use cases

<details>
<summary><b>Case 1: Bulk availability check</b></summary>

```
1. Create profile "all_servers"
2. Fill credentials.txt
3. Click "Ping" — see who responds
4. Click "Check SSH"
5. Open Excel report
```
</details>

<details>
<summary><b>Case 2: Deploy script to 50 servers</b></summary>

```
commands.txt:
  UPLOAD:C:\deploy\install.sh|/tmp/
  chmod +x /tmp/install.sh
  /tmp/install.sh
  rm /tmp/install.sh
```
</details>

<details>
<summary><b>Case 3: Collect logs from all servers</b></summary>

```
commands.txt:
  tail -100 /var/log/syslog
  journalctl -n 50 --no-pager

→ Open debug_output\ — logs per server
```
</details>

<details>
<summary><b>Case 4: Bulk password change</b></summary>

```
commands.txt:
  echo "user:newpassword" | chpasswd

→ Enable "No Ctrl+C" before running
```
</details>

<details>
<summary><b>Case 5: Reboot after update</b></summary>

```
# linux_servers → commands.txt
reboot

# win_servers → commands.txt
shutdown /r /t 0

→ Open both tabs and run simultaneously
```
</details>

---

## ❓ FAQ

**Check takes 10 seconds per server?**  
That's the SSH timeout (10 sec). Use Ping before checking to exclude unreachable servers. Increase thread count to speed things up.

**Commands don't run on Windows servers?**  
SSHeX detects Windows via the SSH banner. Make sure OpenSSH is installed:
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

**UPLOAD not working — file not found?**  
The separator is `|`, not `:`.  
✅ `UPLOAD:C:\scripts\file.sh|/tmp/`  
❌ `UPLOAD:C:\scripts\file.sh:/tmp/`

**targets.txt is empty after check?**  
No server passed. Check `credentials.txt` and open `Debug ▼` for diagnostics.

**How to find an IP in the log?**  
`Ctrl+F` — built-in search with ▲▼ navigation.
