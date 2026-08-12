# M01 — Toolchain, linker, Hello from ConGroo

**Статус:** `not_started`  
**Зависимости:** M00  
**Стек акцент:** linker + 20–40 строк asm + C `kmain`

## Цель

Получить воспроизводимый boot ядра в QEMU и строку `ConGroo boot ok` в serial.

## Теория (обязательно)

1. Freestanding среда: нет libc «как на Linux»
2. ELF-секции и символы
3. Linker script: `ENTRY`, раскладка секций, откуда берётся адрес кода
4. Multiboot2 header: поля, alignment, magic
5. Почему early print через UART удобнее VGA
6. Что нельзя делать до инициализации (нет heap, осторожнее со статикой)

## Углубление

- VMA vs LMA (если столкнёшься)
- Zeroing `.bss`
- Путь `qemu -kernel` vs ISO+GRUB: плюсы/минусы для обучения

## Практика

Создай `congrou/` примерно так (имена можно варьировать, смысл сохранить):

```text
congrou/
  Makefile
  linker.ld
  boot/
    boot.S
  kernel/
    kmain.c
    serial.c
    serial.h
```

Минимум:
- asm entry + переход в C
- `kmain` печатает в serial
- `make run` запускает QEMU со serial в терминал

## Acceptance criteria

- [ ] `make run` показывает `ConGroo boot ok`
- [ ] Есть linker script, ты объясняешь каждую ключевую строку
- [ ] Asm-файлы сопровождены комментариями «зачем», не только «что»
- [ ] Нет «ручного» запуска из десяти несвязанных команд как единственного пути

## Контрольные вопросы

1. Что делает `ENTRY(_start)`?
2. Почему `.bss` может содержать мусор без очистки?
3. Чем запись в UART отличается от `printf` на хосте?

## Bonus

- VGA text вывод параллельно serial
- Lab-баннер в духе [`PHILOSOPHY.md`](../PHILOSOPHY.md): `ConGroo Lab` / `runline boot` / `congruence: pending` (без внешних отсылок)

## Выход

PASS → M02.
