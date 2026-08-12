# Прогресс ConGroo

Ученик: *(укажи имя/ник)*  
Старт: 2026-08-12  
Стек: C, x86_64, QEMU, Multiboot2  
Уровень на входе: asm 1 / C 3.5 / linker 2

## Легенда статусов

- `not_started` — ещё не начинали
- `in_progress` — в работе
- `review` — сдано, ждёт проверки
- `rework` — на доработке
- `done` — закрыто (PASS / PASS WITH NOTES)

## Модули

| ID | Модуль | Статус | Последняя сдача | Заметки |
|----|--------|--------|-----------------|---------|
| M00 | Машина, boot-chain, модель памяти | `not_started` | — | |
| M01 | Toolchain, linker, Hello kernel | `not_started` | — | |
| M02 | Инфраструктура ядра (printk/panic/lib) | `not_started` | — | |
| M03 | Прерывания, таймер, клавиатура | `not_started` | — | |
| M04 | Физическая память и paging | `not_started` | — | |
| M05 | Heap и аллокаторы | `not_started` | — | |
| M06 | Потоки ядра и планировщик | `not_started` | — | |
| M07 | Userspace, syscalls, ELF | `not_started` | — | |
| M08 | VFS, initrd, шелл | `not_started` | — | |
| M09 | Наблюдаемость и «полезность» ConGroo | `not_started` | — | |
| M10 | Трек углубления (выбор) | `not_started` | — | |

## Текущий фокус

- Модуль: **M00**
- Подзадача: *ещё не выбрана*
- Блокер: —

## Журнал

| Дата | Событие |
|------|---------|
| 2026-08-12 | План и workflow зафиксированы в репозитории |
| 2026-08-12 | Философия сменена на Lab / runline / congruence (`docs/PHILOSOPHY.md`) |
