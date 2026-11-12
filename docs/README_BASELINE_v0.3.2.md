> **PROJECT SNAPSHOT 12.11.2026** — Core OS = experimental Termux **v0.3.2**, Android verified for T1A smoke scenarios. This repository kit updates documentation and showcase only; it is **not OS v0.3.3**. See [Docs Index](docs/DOCS_INDEX_RU.md), [Project vision](docs/PRODUCT_VISION_RU.md), [Codex setup](docs/REPOSITORY_SETUP_RU.md).

# MATRYOSHKA OS — компьютер, написанный с нуля (исследовательский прототип)

**Основа:** Termux v0.3.2, 12 ноября 2026. **Не полноценная настольная ОС.**

Откройте **[CODE_TOUR.html](CODE_TOUR.html)** в браузере для просмотра всех основных исходников с номерами строк. Полная карта исходников: **[SOURCE_MAP_RU.md](SOURCE_MAP_RU.md)**.

## Где находится настоящее «железо»

- `matryoshka.py`: `VirtualMachine.step()` выполняет 33 инструкции M32; `VirtualMachine._guard()` проверяет доступы к памяти; `BlockDisk` реализует дисковое устройство; `assemble()` — двухпроходный ассемблер M32.
- `firmware.asm`: гостевая загрузочная прошивка. Читает ядро из `images/system.img`.
- `kernel.asm`: гостевое ядро на ассемблере; планировщик, обработка системных вызовов, загрузка программ из системного диска.
- `programs/shell.asm`: гостевая оболочка команд `help`, `ps`, `run`, `save`, `load` и других.
- `hardware_m8/`: самостоятельный 8-битный процессор и модель логической ALU, пока **не** совместим с M32.
- `engine_lab/`: отдельный прототип графического движка хоста (C/C++, клиенты Python/Java, пример C#). **Это не код гостевой ОС.**

## Запуск на Termux

```sh
pkg install python -y
python matryoshka.py --selftest
python matryoshka.py --os
```

## Показать карту всех частей в терминале

```sh
python tools/show_components.py
python tools/show_components.py --json
```

## Смотреть в браузере Termux

В папке репозитория:

```sh
python -m http.server 8765 --bind 127.0.0.1
```

Затем откройте `http://127.0.0.1:8765/CODE_TOUR.html` в Chrome. Сеть не нужна: сервер на вашем собственном телефоне. Завершить — `Ctrl+C` в Termux.

## Движок хоста

В `engine_lab/README_RU.md` — сборка `bash build.sh` для C/C++ и запуск Python/Java-клиентов. На обычном Linux воспроизводимое сравнение кадров прошло; C# ещё не протестирован с .NET SDK. Интеграция этого движка в MATRYOSHKA OS — будущий этап.

## Архитектурное ограничение

Python по-прежнему эмулирует устройства, CPU, память и часть механизма переключений. Гостевые ядро, firmware и shell исполняются как машинный код M32. Нет современной файловой системы, GUI и изоляции уровня production. Для Proteus окончательная ISA не согласована. Никакой APK/EXE/.tea совместимости сейчас нет.

Идея проекта — довести один гостевой машинный код до запуска и в Python-модели CPU, и на логической схеме Proteus, затем портировать общие исходники ОС на x86/ARM (уже отдельные бинарные сборки).
