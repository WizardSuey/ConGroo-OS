# Basins — известные колодцы неизбежности

Живой каталог. Новые записи добавляет оператор, когда runline снова умирает «тем же концом».

Формат:

```text
## basin.<id>
Симптом:
Почему притягивает:
Third path / выход:
Статус: open | escaped | accepted-limit
```

---

## basin.silent_hang
Симптом: нет логов, QEMU жив, система мертва для оператора.  
Почему притягивает: забыли serial/panic path; тупик без witness.  
Third path: всегда иметь final print + halt/panic loop.  
Статус: open (ожидаем M01–M02)

## basin.amnesia
Симптом: сбой был, но после reboot нечего вспомнить.  
Почему притягивает: удобно «начать чисто».  
Third path: retention dump + counters + notes.  
Статус: open (M02/M09)

## basin.panic_everything
Симптом: любая ошибка валит ядро.  
Почему притягивает: проще, чем различать вину.  
Third path: изоляция user-fault, отказ alloc, осмысленные errno.  
Статус: open (M07+)
