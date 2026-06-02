# KidLock

Родительский контроль для Windows — блокировка компьютера по расписанию.

## Что делает

- Блокирует сессию по расписанию (разрешённые часы по дням недели)
- Ограничивает суммарное время работы в день (дневной лимит)
- Показывает предупреждения за 120 / 60 / 30 секунд до блокировки
- Принудительный выход из сессии (`shutdown /l /f`)
- Работает как Windows-сервис (автозапуск, защита от остановки)
- Учитывает использованное время (сбрасывается в полночь)

## Быстрый старт

### 1. Установи Python 3.10+
Скачай с [python.org](https://python.org) — обязательно отметь **«Add Python to PATH»**

### 2. Установи зависимости
```cmd
pip install pywin32
python Scripts\pywin32_postinstall.py -install
```

### 3. Настрой расписание
Открой `agent/config.json` и задай своё расписание.

### 4. Запусти как сервис (от администратора)
```cmd
cd agent
python agent.py install
python agent.py start
```

Или просто для теста:
```cmd
python agent/agent.py
```

## Структура

```
kidlock/
├── agent/
│   ├── agent.py        ← главный процесс
│   ├── scheduler.py    ← логика расписания и лимитов
│   ├── locker.py       ← блокировка Windows
│   ├── notifier.py     ← предупреждения
│   └── config.json     ← расписание (редактируй здесь)
├── docs/               ← документация (.docx)
└── requirements.txt
```

## Пример config.json

```json
{
  "warning_seconds": 120,
  "check_interval_seconds": 30,
  "schedule": {
    "allowed_hours": {
      "monday":    { "start": "14:00", "end": "20:00" },
      "tuesday":   { "start": "14:00", "end": "20:00" },
      "wednesday": { "start": "14:00", "end": "20:00" },
      "thursday":  { "start": "14:00", "end": "20:00" },
      "friday":    { "start": "14:00", "end": "22:00" },
      "saturday":  { "start": "10:00", "end": "22:00" },
      "sunday":    { "start": "10:00", "end": "21:00" }
    },
    "daily_limit_minutes": {
      "monday":    90,
      "tuesday":   90,
      "wednesday": 90,
      "thursday":  90,
      "friday":    180,
      "saturday":  240,
      "sunday":    180
    }
  }
}
```

> Изменения в `config.json` применяются сразу — перезапуск не нужен.

## Управление сервисом

Все команды — от администратора в папке `agent/`:

| Команда | Действие |
|---|---|
| `python agent.py install` | Установить сервис |
| `python agent.py start`   | Запустить |
| `python agent.py stop`    | Остановить |
| `python agent.py restart` | Перезапустить |
| `python agent.py remove`  | Удалить |

## Требования

- Windows 10 / 11
- Python 3.10+
- pywin32 >= 306
- Запуск от администратора
