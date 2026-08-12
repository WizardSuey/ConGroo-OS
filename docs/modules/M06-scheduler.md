# M06 — Потоки ядра и планировщик

**Статус:** `not_started`  
**Зависимости:** M05 (стек/аллокации), M03 (timer)

## Цель

Несколько задач ядра делят CPU; сначала cooperative, затем preemptive.

## Теория

- TCB/PCB минимальный набор полей
- Стеки потоков, выравнивание, red zone
- Context switch asm: callee-saved vs полный save
- Ready queue
- Cooperative `yield`
- Preemption из timer IRQ
- Состояния задач
- Запрет гонок на UP: политика IRQ во время schedule

## Углубление

- Sleep/wakeup
- Приоритеты (простые)
- Stack canary / overflow
- Fairness и starvation

## Практика

- Создать ≥2 runnable tasks
- `yield` работает
- Включить preemption
- Дамп задач в лог (`ps` прототип)

## Acceptance criteria

- [ ] Задачи чередуются без ручного перезапуска VM
- [ ] Timer-based preemption включена осознанно
- [ ] Есть способ увидеть список задач
- [ ] Заметка: что сохраняется при switch

## Контрольные вопросы

1. Почему нельзя просто `call` в другой стек без смены sp?
2. Что будет, если schedule() вызвать с полузаписанной структурой очереди?
3. Чем kernel thread отличается от будущего user process?

## Выход

PASS → M07.
