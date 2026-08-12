# ConGroo — углублённый план обучения

Цель курса: ты **сам** собираешь ОС на C под x86_64 в QEMU — от boot до userspace-шелла — и при этом понимаешь, *почему* каждый слой устроен так.

Параллельные цели:
1. Понять устройство машины (CPU, память, прерывания, привилегии).
2. Получить **полезный минимум**: лаборатория + хост для своих программ + наблюдаемость системы.

Связанные документы:
- Протокол сдачи/проверки: [`WORKFLOW.md`](WORKFLOW.md)
- Трекер: [`PROGRESS.md`](PROGRESS.md)
- Карточки модулей: [`modules/`](modules/)

---

## Философия продукта (чтобы ОС была хоть капельку полезна)

**ConGroo** (*continue + grow*) — среда, которая растёт вместе с автором.

| Принцип | Смысл | Когда проявляется |
|---------|--------|-------------------|
| Grow in public | Состояние системы видимо | M02 логи, M09 `dmesg`/`mem`/`ps` |
| Small, then real | Сначала serial-лаборатория, потом «как у взрослых» | M01→M08 |
| One truth path | Один boot-path, один syscall ABI | M01, M07 |
| Useful early | Шелл и запуск программ до идеальной красоты | M08 |
| Fail loudly | Понятный panic лучше тихого зависания | M02, M04 |

**Полезные сценарии к концу ядра курса:**
- запускать свои user-программы (`hello`, утилиты);
- смотреть uptime, память, задачи;
- использовать ConGroo как стенд для экспериментов с прерываниями/памятью;
- иметь предсказуемый initrd + шелл без чужого userspace-дистрибутива.

---

## Как устроен курс

### Формат модуля
Каждый модуль = **теория → практика → критерии → контрольные вопросы → выход**.

### Глубина
Это не «скопируй osdev wiki и иди дальше». На каждом модуле есть:
- обязательное ядро темы;
- углубление (разбираем механизмы, edge cases, типичные баги);
- опциональный bonus (не блокирует прогресс).

### Темп
Ориентир — **качество понимания**, не календарь.  
Модуль закрывается только при вердикте PASS / PASS WITH NOTES ([`WORKFLOW.md`](WORKFLOW.md)).

### Стек (фиксируем)
- C11 (или GNU C), freestanding
- Минимум asm (NASM или GAS) — entry, ISR stubs, context switch
- x86_64 long mode
- QEMU + serial console
- Multiboot2 на старте; UEFI — опциональный трек в M10

### Чего сознательно нет в первой половине курса
- полноценной сети;
- SMP (несколько ядер) — только введение в M10;
- GUI (кроме опционального framebuffer трека);
- совместимости с Linux ABI.

---

## Карта модулей

```text
M00 Машина и модель
 └─ M01 Boot + toolchain + Hello
     └─ M02 Kernel infra
         └─ M03 Interrupts & devices
             └─ M04 Physical memory + paging
                 └─ M05 Heap
                     └─ M06 Scheduling
                         └─ M07 Userspace + syscalls
                             └─ M08 VFS + shell
                                 └─ M09 Observability / useful OS
                                     └─ M10 Deep track (выбор)
```

Зависимости жёсткие до M08. Прыгать нельзя без согласования.

---

## M00 — Машина, boot-chain, модель памяти

**Карточка:** [`modules/M00-machine-model.md`](modules/M00-machine-model.md)

### Зачем
Без ментальной модели код ядра превращается в шаманство.

### Теория (углублённо)
- Firmware → bootloader → kernel entry
- Real mode / protected mode / long mode (обзор; детали long mode — в M01/M04)
- Privilege rings, зачем изолировать код
- Физические адреса vs виртуальные (пока концепт)
- Стек: рост вниз, кадры, что будет при переполнении
- Interrupt/exception/fault/abort — различия
- Роль линкера и load address
- UB в freestanding C: почему «на хосте работало» не аргумент

### Практика
- Схема загрузки ConGroo до `kmain`
- Глоссарий своими словами
- Проверка toolchain на машине

### Критерий выхода
Можешь объяснить путь до ядра и отличить interrupt от syscall без шпаргалки.

---

## M01 — Toolchain, linker, Hello from ConGroo

**Карточка:** [`modules/M01-hello-kernel.md`](modules/M01-hello-kernel.md)

### Зачем
Первый контакт с «голым» железом (эмулированным) и дисциплиной сборки.

### Теория
- Freestanding vs hosted
- Cross-compiler: зачем (и когда можно учиться осторожно на host-gcc)
- ELF: секции `.text/.rodata/.data/.bss`
- Linker script: `ENTRY`, `SECTIONS`, VMA/LMA (на пальцах)
- Multiboot2 header: контракт с загрузчиком
- Serial (UART 16550) vs VGA text: почему serial лучше для отладки
- Early boot: что ещё нельзя делать (нет кучи, нет прерываний)

### Практика
- Репозиторий `congrou/`
- `boot.S` / `boot.asm` + `kmain.c`
- `linker.ld`, `Makefile`
- Вывод `ConGroo boot ok` в serial QEMU
- `make run` одним правилом

### Углубление
- Почему порядок секций важен
- Что такое `bss` zeroing и кто это делает
- Разница `qemu -kernel` и ISO+GRUB

### Критерий выхода
Стабильный boot + воспроизводимый `make run` + объяснение linker script своими словами.

---

## M02 — Инфраструктура ядра

**Карточка:** [`modules/M02-kernel-infra.md`](modules/M02-kernel-infra.md)

### Зачем
Без логов и panic ты слепой. Это фундамент «Grow in public».

### Теория
- `printk` levels, early console
- `panic` / `abort`: halt, cli, сообщение, stack context (пока простой)
- Минимальная kernel libc: `memcpy`, `memset`, `memcmp`, `strlen`, `snprintf` (урезанный)
- Атрибуты: `noreturn`, `packed`, alignment
- Инварианты и `assert`
- Организация каталогов ядра

### Практика
- Единый лог-формат: `[INFO ]`, `[WARN ]`, `[ERROR]`, `[PANIC]`
- `panic("...")` с вечным halt
- Тест на переполнение простого форматтера (осознанно)

### Углубление
- Реентерабельность логов (пока отметить проблему — решить в M03/M06)
- Почему float в ядре обычно запрещают на старте

### Критерий выхода
Любой осознанный сбой даёт читаемый panic; логи помогают дебажить M03+.

---

## M03 — Прерывания, таймер, клавиатура

**Карточка:** [`modules/M03-interrupts.md`](modules/M03-interrupts.md)

### Зачем
ОС «жива», только когда реагирует на время и устройства.

### Теория
- IDT gate descriptors
- Exceptions vs IRQs
- PIC 8259 → APIC (сначала PIC, APIC как углубление)
- ISR stub на asm: сохранение регистров, EOI, `iretq`
- PIT (или HPET overview): тики, частота
- PS/2 keyboard: scancode set 1 (базово)
- Page fault как exception (обработчик-заглушка)
- Critical sections: `cli`/`sti`, когда это грубый инструмент

### Практика
- Работающий IDT
- Timer tick + uptime
- Echo клавиш в serial
- Exception handler с номером и rip (насколько доступно)

### Углубление
- Spurious IRQ
- Nested interrupts (пока запрещены политикой)
- Разница fault (можно перезапустить) vs abort

### Критерий выхода
Система тикает, отвечает на клавиши, фатальные exceptions не молчат.

---

## M04 — Физическая память и paging

**Карточка:** [`modules/M04-memory-paging.md`](modules/M04-memory-paging.md)

### Зачем
Без контроля памяти нет изоляции и нормального роста системы.

### Теория
- Карта памяти от Multiboot2 mmap
- Frame = страница 4KiB (базовый вариант)
- Bitmap / freelist allocator кадров
- x86_64 page tables: 4 levels, PTE flags (`P`, `RW`, `US`, `NX` если доступно)
- Identity map vs higher-half kernel (выбрать путь и зафиксировать)
- TLB и `invlpg`
- Page fault anatomy: cr2, error code

### Практика
- Парсер mmap
- PMM: alloc/free frame
- Включение/переключение paging с известной картой
- Намеренный page fault → понятный panic

### Углубление
- Fragmentation физических кадров
- Почему нельзя «просто malloc» до paging/heap
- Guard pages (идея)

### Критерий выхода
PMM тестируется; paging включён; обращение по мусорному адресу объясняется.

---

## M05 — Heap и объектные аллокаторы

**Карточка:** [`modules/M05-heap.md`](modules/M05-heap.md)

### Зачем
Ядру нужны структуры данных переменного размера.

### Теория
- `kmalloc`/`kfree` поверх PMM
- Стратегии: bump → freelist → (bonus) slab/buckets
- Alignment, header блока, coalescing (базово)
- OOM policy в ядре
- Lifetime: кто free-ит

### Практика
- Рабочий heap
- Stress-тест: alloc/free паттерны
- Лог статистики heap (`used`, `free`, `peak`)

### Углубление
- Use-after-free симптомы
- Double-free detection (canary/magic)
- Почему userspace allocator ≠ kernel allocator

### Критерий выхода
Стабильный kmalloc для следующих модулей + базовая диагностика утечек в тестах.

---

## M06 — Потоки ядра и планировщик

**Карточка:** [`modules/M06-scheduler.md`](modules/M06-scheduler.md)

### Зачем
Многозадачность — сердце «живой» ОС.

### Теория
- Task/Thread control block
- Стеки задач, red zone (x86_64 SysV — учесть в asm)
- Context switch: что сохраняем
- Cooperative `yield` → preemptive (IRQ timer)
- Состояния: Running / Ready / Blocked / Dead
- Race вокруг очередей; пока унипроцессор + запрет вложенных IRQ упрощает жизнь
- Sleep/wakeup идея

### Практика
- 2–3 kernel threads с разными стеками
- Cooperative switch
- Preemptive switch по tick
- `ps`-подобный дамп задач (хотя бы в лог)

### Углубление
- Starvation
- Stack overflow детект (canary)
- Почему «поток ядра» ≠ «процесс пользователя»

### Критерий выхода
Вытеснение работает; одна плотная задача не делает serial полностью мёртвым навсегда (в разумном тесте).

---

## M07 — Userspace, syscalls, ELF

**Карточка:** [`modules/M07-userspace.md`](modules/M07-userspace.md)

### Зачем
Польза ОС = запуск *твоих* программ вне ядра.

### Теория
- Ring 3, сегменты/STAR/LSTAR (syscall путь) или `int 0x80`-стиль (учебный выбор — зафиксировать один)
- TSS / IST: зачем для стека при privilege change
- User page mappings: `US` bit, изоляция
- Syscall ABI ConGroo (своя таблица): минимум `write`, `exit`, `yield`, потом `open/read`
- ELF64 loader: заголовки, PT_LOAD, entry
- Альтернатива: свой flat binary формат → затем ELF
- Kill/fault userspace vs panic kernel

### Практика
- Переход в user mode
- Программа `hello` через syscall write
- Корректный `exit`
- Page fault в user не валит ядро молча

### Углубление
- W^X
- SMEP/SMAP (обзор + optional enable)
- Аргументы syscall и проверка указателей из user

### Критерий выхода
Userspace hello работает; syscall boundary осознан; fault user ≠ silent death kernel.

---

## M08 — VFS, initrd, шелл

**Карточка:** [`modules/M08-vfs-shell.md`](modules/M08-vfs-shell.md)

### Зачем
Появляется ежедневная польза: файлы, команды, запуск программ.

### Теория
- VFS как тонкий слой: `open/read/write/close` над backend
- Initrd (cpio/tar/свой формат) в памяти
- Inode/dentry упрощённо
- Шелл: парсинг строк, builtin vs exec
- Политика: что в ядре, что в user

### Практика
- Initrd с `hello` и текстовыми файлами
- Builtins: `help`, `ls`, `cat`, `run`, `ps`, `mem`, `uptime`
- `run hello` загружает ELF/бинарник

### Углубление
- Пути и cwd (можно упростить до flat namespace)
- Ошибки: not found / invalid elf

### Критерий выхода
Интерактивный цикл в serial: посмотреть файлы, прочитать, запустить программу.

---

## M09 — Наблюдаемость и «полезная» ConGroo

**Карточка:** [`modules/M09-observability.md`](modules/M09-observability.md)

### Зачем
Доводим философию до продукта: ОС как лаборатория.

### Теория
- Telemetry: что измерять (ticks, faults, allocs, context switches)
- Стабильный ABI команд/syscalls для диагностики
- Документирование syscall table и формата initrd
- Минимальный «учебный SDK»: как собрать user-программу

### Практика
- `dmesg` буфер
- Счётчики в `mem`/`ps`
- `docs/USERGUIDE.md` — как пользоваться ConGroo
- Демо-сценарий: 3 команды показывают здоровье системы

### Критерий выхода
Новый человек (или ты через месяц) может запустить QEMU и провести базовый эксперимент по гайду.

---

## M10 — Трек углубления (выбор одного+)

**Карточка:** [`modules/M10-deep-tracks.md`](modules/M10-deep-tracks.md)

Выбери трек(и) после M09:

| Трек | Суть |
|------|------|
| A. Virtio-blk | Блок-устройство вместо чистого initrd |
| B. Framebuffer | Минимальный текстовый/графический UI |
| C. UDP echo | Сеть в учебном объёме |
| D. Hardening | W^X, SMEP/SMAP, строже user ptr checks |
| E. aarch64 | Порт ментальной модели на другую арх |
| F. SMP intro | Второй CPU: IPI, per-cpu (очень сложно) |

---

## Сквозные навыки (на всём курсе)

1. **Чтение Intel SDM / osdev точечно** — не целиком, а по текущей главе.
2. **Отладка**: serial log, QEMU monitor, `info registers`, `x/` memory.
3. **Гигиена C**: типы фиксированной ширины, `volatile` для MMIO, packed structs.
4. **Документирование решений**: ADR-заметки в `docs/notes/` (коротко: решили X, потому что Y).
5. **Регрессии**: простой `make test` / smoke script, когда появится что тестировать.

---

## Рекомендуемые источники (не вместо думания)

- OSDev Wiki — справка, не единственный учитель
- Intel SDM Vol. 3 — privilege, paging, interrupts
- «Operating Systems: Three Easy Pieces» — память/процессы концептуально
- Linux/BSD исходники — только как *чтение*, не для копипаста модулей

При использовании источника — ссылка в сдаче.

---

## Definition of Done всего ядра курса (после M09)

- [ ] `make run` поднимает ConGroo в QEMU
- [ ] Есть serial-шелл и initrd
- [ ] Можно запустить user-программу
- [ ] Видны память/задачи/uptime/dmesg
- [ ] Ты объясняешь boot → paging → syscall без конспекта
- [ ] Известные ограничения честно записаны в USERGUIDE

---

## Следующий шаг

1. Прочитать [`WORKFLOW.md`](WORKFLOW.md).
2. Открыть [`modules/M00-machine-model.md`](modules/M00-machine-model.md).
3. Сдать домашку M00 по [`templates/SUBMISSION.md`](templates/SUBMISSION.md).
