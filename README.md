## Сдача лабораторной 16
* **Отчёт аудита:** [audit_report.md](./audit_report.md)
* **Diff-файл исправлений:** [hardening.diff](./hardening.diff)
* **Ветки:** `vulnerable` (до исправлений) и `hardened` (после).


# Отчет по лабораторным работам 15 и 16

## Лабораторная 15. Безопасная передача данных

### 1. Ссылка на репозиторий
Проект собирается без правок. Ветка: `main`. Секретов в репозитории нет.

### 2. Canonical-строка подписи
В приложении используется HMAC-SHA256 для подписи запросов. 
Формат строки: `method + "\n" + path + "\n" + SHA256(body) + "\n" + timestamp + "\n" + nonce`

### 3. План ротации пинов
В NSC и в коде используется 2 пина:
* Primary: `sha256/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=`
* Backup: `sha256/BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB=`
Время жизни (expiry): 2026-06-01.

### 4. Ответы на вопросы
**Почему user-CA не работает в release?**
В Android 7.0+ приложения по умолчанию доверяют только системным сертификатам. Пользовательские CA (например, mitmproxy) игнорируются для защиты от MITM. Для отладки используется `debug-overrides`.

**Что при сдвиге часов?**
Сервер отклонит запрос, так как timestamp выйдет за окно ±30 секунд. Лечится синхронизацией NTP.

### 5. Три доказательства MITM-эксперимента (логи)
**A. Debug (без пиннинга):** трафик читается.
```text
[DEBUG] Request: POST /api/scan
[DEBUG] Body: {"visitorId": "V-001"}
