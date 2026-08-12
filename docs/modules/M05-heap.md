# M05 — Heap и kmalloc

**Статус:** `not_started`  
**Зависимости:** M04

## Цель

Дать ядру безопасный (насколько возможно) аллокатор объектов.

## Теория

- Связь heap ↔ PMM
- Bump allocator vs freelist
- Заголовок блока, alignment
- Coalescing / splitting (базовый уровень)
- OOM поведение
- Magic/canary против простейших corruption

## Углубление

- Slab/bucket идея (bonus)
- Почему fragmentation логическая ≠ физическая
- Debug: poison pattern `0xDEADBEEF` / `0xA5`

## Практика

- `kmalloc` / `kfree`
- Статистика: used/free/peak
- Stress в early boot tests
- Интеграция: хотя бы одна структура ядра живёт в heap

## Acceptance criteria

- [ ] Alloc/free паттерны из теста проходят
- [ ] Double-free/detectable corruption хотя бы частично ловятся или документированы как ограничения
- [ ] Есть вывод статистики heap
- [ ] Без heap-регрессий boot+timer+keyboard остаются живы

## Контрольные вопросы

1. Кто возвращает страницы в PMM при `kfree` (всегда ли)?
2. Чем опасен use-after-free в ядре по сравнению с user app?
3. Почему alignment важен для структур и SIMD/флагов (даже если SIMD нет)?

## Выход

PASS → M06.
