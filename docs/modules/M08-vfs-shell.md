# M08 — VFS, initrd, шелл

**Статус:** `not_started`  
**Зависимости:** M07

## Цель

Сделать ConGroo интерактивно полезной: файлы в памяти + команды + запуск программ.

## Теория

- VFS ops table
- Initrd формат (выбрать: cpio/tar/свой) + ADR
- Flat namespace допустим на v1
- Shell loop: prompt → parse → builtin/exec
- Разделение: shell в kernel временно vs shell как user — выбрать и обосновать

## Углубление

- Streaming read vs целиком в буфер
- Коды ошибок для shell
- История команд (bonus)

## Практика

Builtins минимум:
- `help`
- `ls`
- `cat`
- `run`
- `ps`
- `mem`
- `uptime`

Initrd содержит текстовый файл и `hello`.

## Acceptance criteria

- [ ] Интерактивный prompt в serial
- [ ] `cat` читает файл из initrd
- [ ] `run hello` запускает user-программу
- [ ] Ошибка «нет файла» понятна человеку

## Контрольные вопросы

1. Что такое VFS и зачем, если файловая система одна?
2. Где проходит граница kernel/user для shell в твоём выборе?
3. Как `run` связан с ELF loader из M07?

## Выход

PASS → M09.
