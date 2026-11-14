# Отчет по лабораторной работе 16. Аудит и hardening

## 1. Резюме
**Область аудита:** приложение `com.notes.app` v1.0 (APK из main@a1b2c3).
**Методика:** OWASP MASVS v2. Инструменты: lint, gitleaks, adb, mitmproxy.
**Итог:** 2 critical, 3 high, 2 medium.

## 2. Методика (воспроизводимые команды)
```bash
# 1. Статический анализ (Lint)
./gradlew lint

# 2. SCA (Анализ зависимостей)
./gradlew dependencyCheckAnalyze

# 3. Секрет-скан
gitleaks detect --source . --verbose

# 4. Динамический анализ (Экспорт активности)
adb shell am start -n com.notes.app/.ui.DeleteNoteActivity --es note_id "123"

# 5. Дамп бэкапа
adb backup -f backup.ab com.notes.app
