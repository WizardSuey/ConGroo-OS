# ConGroo

Операционная система на **C** (x86_64, QEMU), которую оператор собирает сам.  
В репозитории также лежат учебный план и протокол работы в Cursor — отдельно от души системы.

**ConGroo** (*congruence*) — ОС, которая отказывается от тихой неизбежности сбоев: помнит witness прошлых смертей, ищет третий путь вместо жестоких бинарных развилок и закрывает сессию только когда замысел и машина совпали.

**Слоган:** `ConGroo — congruence with the machine.`

## Документы ОС

| Файл | Зачем |
|------|--------|
| [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md) | Конституция: законы, смысл, голос |
| [docs/os/RUNTIME.md](docs/os/RUNTIME.md) | Как законы проявляются в boot/panic/шелле |
| [docs/os/BASINS.md](docs/os/BASINS.md) | Каталог «колодцев», в которые систему тянет |

## Документы обучения (отдельно)

| Файл | Зачем |
|------|--------|
| [docs/LEARNING_PLAN.md](docs/LEARNING_PLAN.md) | План модулей M00–M10 |
| [docs/WORKFLOW.md](docs/WORKFLOW.md) | Сдача и проверка в Cursor |
| [docs/PROGRESS.md](docs/PROGRESS.md) | Трекер прогресса |

## Законы (кратко)

1. **Basins** — у системы есть притягивающие плохие исходы; свобода = спроектировать выход.
2. **Third Path** — не принимать ложную вагонетку kill-all vs mute-fail.
3. **Retention** — опыт реален, пока удержан свидетелем (`dmesg`, counters, lastpanic).
4. **Mask & Charge** — lab-тон допустим; ответственность за ядро абсолютна.
5. **Reweigh** — прошлое не стирают ради чистой статистики; его переосмысляют.

Полный текст — в [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md).

## Стек (целевой)

- C (+ минимум asm), x86_64, QEMU, Multiboot2, Make + linker script

## С чего начать

1. Прочитай конституцию ОС: [PHILOSOPHY.md](docs/PHILOSOPHY.md) и [RUNTIME.md](docs/os/RUNTIME.md).
2. Для учёбы — [LEARNING_PLAN.md](docs/LEARNING_PLAN.md) / Модуль M00.
