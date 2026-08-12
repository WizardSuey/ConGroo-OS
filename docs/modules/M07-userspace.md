# M07 — Userspace, syscalls, ELF

**Статус:** `not_started`  
**Зависимости:** M06, M04

## Цель

Запускать программу вне кольца 0 через стабильный syscall ABI ConGroo.

## Теория

- User mode transition
- Syscall path: выбери **один** механизм и зафиксируй ADR
  - предпочтительно: `syscall`/`sysret` (x86_64)
  - учебный запасной: software interrupt
- Таблица syscalls, номера, соглашения аргументов
- Проверка user pointers
- ELF64: заголовки, PT_LOAD, entry, нулевой bss
- Изоляция: fault в user

## Углубление

- W^X
- SMEP/SMAP overview
- Аргументы и errno-стиль возвратов (свой простой код ошибок)

## Практика

- Минимальный user `hello`
- `sys_write` в serial
- `sys_exit`
- Загрузчик ELF (или промежуточный flat binary + план миграции на ELF)

## Acceptance criteria

- [ ] User программа печатает через syscall
- [ ] `exit` не возвращает управление «в никуда» молча
- [ ] Документирован ABI (номера и регистры) в `docs/notes/` или USERGUIDE draft
- [ ] User fault обрабатывается отдельно от kernel panic (хотя бы политикой сообщений)

## Контрольные вопросы

1. Почему нельзя доверять указателю из userspace без проверки?
2. Что должен сделать loader с `.bss`?
3. Чем `sysret` опасен при неправильных флагах/сегментах (обзорно)?

## Выход

PASS → M08.
