# ConGroo

Учебная операционная система на **C** (x86_64, QEMU).  
Код ядра и userspace пишет ученик; в репозитории живут план обучения, протокол сдачи и заметки.

**Философия:** *continue + grow* — система растёт вместе с тобой. Каждый слой сначала простой и видимый, потом углубляется. ConGroo полезна как лаборатория железа и как минимальный хост для своих программ.

**Слоган:** `ConGroo — grow with the machine.`

## Документы

| Файл | Зачем |
|------|--------|
| [docs/LEARNING_PLAN.md](docs/LEARNING_PLAN.md) | Углублённый план обучения (модули, теория, практика, критерии) |
| [docs/WORKFLOW.md](docs/WORKFLOW.md) | Как сдавать работу в Cursor и как учитель проверяет |
| [docs/PROGRESS.md](docs/PROGRESS.md) | Трекер прогресса |
| [docs/templates/SUBMISSION.md](docs/templates/SUBMISSION.md) | Шаблон сдачи домашки / модуля |

## Стек (целевой)

- Язык: C (+ минимум NASM/GAS)
- Архитектура: x86_64
- Эмуляция: QEMU
- Загрузка: Multiboot2 → позже UEFI (опционально)
- Сборка: Make + linker script

## Принципы ConGroo

1. **Grow in public** — состояние системы наблюдаемо (`dmesg`, `mem`, `ps`).
2. **Small, then real** — сначала serial и текст, потом устройства и userspace.
3. **One truth path** — один понятный boot-path и syscall ABI.
4. **Useful early** — шелл и запуск программ появляются до «идеальной» архитектуры.

## С чего начать

1. Прочитай [план](docs/LEARNING_PLAN.md) и [workflow](docs/WORKFLOW.md).
2. Отметь старт в [PROGRESS.md](docs/PROGRESS.md).
3. Открой Модуль 0 и сдай домашку по шаблону сдачи.
