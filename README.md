# ConGroo

Учебная операционная система на **C** (x86_64, QEMU).  
Код ядра и userspace пишет ученик; в репозитории живут план, протокол сдачи и lab notes.

**ConGroo** — лаборатория расхождений: каждый boot это *runline*, каждый сбой — измерение, каждая удачная сессия — шаг к **конгруэнтности** (когда замысел, код и CPU совпадают).

**Слоган:** `ConGroo — congruence with the machine.`

## Документы

| Файл | Зачем |
|------|--------|
| [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md) | Философия и вайб лаборатории |
| [docs/LEARNING_PLAN.md](docs/LEARNING_PLAN.md) | Углублённый план обучения (M00–M10) |
| [docs/WORKFLOW.md](docs/WORKFLOW.md) | Как сдавать работу в Cursor и как учитель проверяет |
| [docs/PROGRESS.md](docs/PROGRESS.md) | Трекер прогресса |
| [docs/templates/SUBMISSION.md](docs/templates/SUBMISSION.md) | Шаблон отчёта эксперимента / сдачи |

## Стек (целевой)

- Язык: C (+ минимум NASM/GAS)
- Архитектура: x86_64
- Эмуляция: QEMU
- Загрузка: Multiboot2 → позже UEFI (опционально)
- Сборка: Make + linker script

## Принципы лаборатории

1. **Congruence** — цель: согласие замысла, кода и поведения машины.
2. **Divergence is data** — panic и fault обязаны быть видимы и измеримы.
3. **Retain the signal** — уроки и метрики переживают reboot (`dmesg`, notes, счётчики).
4. **Lab before legend** — сначала serial и факты, потом театральный тон.
5. **Close the loop** — сессия заканчивается явным статусом, не «тихим висом».

Подробности — в [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md).

## С чего начать

1. Прочитай [философию](docs/PHILOSOPHY.md), [план](docs/LEARNING_PLAN.md) и [workflow](docs/WORKFLOW.md).
2. Отметь старт в [PROGRESS.md](docs/PROGRESS.md).
3. Открой Модуль 0 и сдай первый отчёт по шаблону.
