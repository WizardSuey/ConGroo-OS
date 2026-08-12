# M03 — Прерывания, таймер, клавиатура

**Статус:** `not_started`  
**Зависимости:** M02

## Цель

Научить CPU сообщать ядру о событиях; получить uptime и ввод.

## Теория

- IDT, gate descriptors, selector/offset/IST (обзор IST)
- Exceptions (делим на «пока panic» и «потом handle»)
- IRQ линии, PIC remap
- Asm ISR stubs + общий C handler
- EOI
- PIT programming (или выбранный таймер) и tick accounting
- PS/2 keyboard status/data ports, scancodes
- `cli`/`sti` и критические секции

## Углубление

- Error code у некоторых exceptions
- Spurious interrupts
- Политика: вложенные IRQ пока запрещены
- Page fault handler-заглушка (`cr2`)

## Практика

- Заполнение и загрузка IDT (`lidt`)
- IRQ0 tick + счётчик
- IRQ1 keyboard echo в serial
- Дамп exception vector + rip при делении на ноль / int3 (выбрать безопасный тест)

## Acceptance criteria

- [ ] Тики видны (uptime растёт)
- [ ] Клавиши отражаются в serial
- [ ] Фатальная exception даёт лог, не чёрный экран без текста
- [ ] Есть комментарии/заметки: зачем remap PIC

## Контрольные вопросы

1. Что будет, если забыть EOI?
2. Чем exception отличается от device IRQ?
3. Почему ISR prologue делают на asm?

## Bonus

- Простейший scancode → ASCII map
- Команда/ключ, печатающая uptime по запросу

## Выход

PASS → M04.
