# M04 — Физическая память и paging

**Статус:** `not_started`  
**Зависимости:** M03 (M02 минимум; M03 желателен для fault логов)

## Цель

Взять контроль над физической памятью и включить страничную трансляцию.

## Теория

- Multiboot2 memory map: свободные/занятые регионы
- Физический frame allocator (bitmap или freelist)
- 4-level page tables на x86_64
- Флаги PTE, NX bit (если включаешь)
- Identity map vs higher-half: выбрать и зафиксировать ADR в `docs/notes/`
- TLB, `invlpg`, CR3
- Page fault error codes

## Углубление

- Почему нельзя свободно резать память bootloader'а
- Fragmentation frames
- Guard page идея для стеков (подготовка к M06)

## Практика

- Парсер mmap + лог регионов
- `pmm_alloc` / `pmm_free`
- Построение таблиц и переключение
- Тест: аллокация N кадров, проверка, fault на bad address

## Acceptance criteria

- [ ] PMM не выдаёт кадры из reserved регионов
- [ ] После включения paging ядро продолжает работать
- [ ] Bad pointer → понятный fault/panic путь
- [ ] ADR: выбранная модель отображения ядра записана

## Контрольные вопросы

1. Что хранит запись page table: адрес чего?
2. Зачем нужен bit `US`?
3. Что случится при записи в read-only page?

## Выход

PASS → M05.
