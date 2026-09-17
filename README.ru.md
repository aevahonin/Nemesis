# Nemesis

Локальный музыкальный плеер. Нативный интерфейс на [iced](https://iced.rs/), без встроенного браузера.

English: [README.md](README.md)

Релизы: [github.com/aevahonin/Nemesis/releases](https://github.com/aevahonin/Nemesis/releases)

<p align="center">
  <img src="media/grid.gif" alt="Nemesis" />
</p>

<p align="center">
  <img src="media/1.png" width="49%" alt="" />
  <img src="media/2.png" width="49%" alt="" />
  <img src="media/3.png" width="49%" alt="" />
  <img src="media/4.png" width="49%" alt="" />
  <img src="media/5.png" width="49%" alt="" />
  <img src="media/6.png" width="49%" alt="" />
</p>


---

## Что умеет

Библиотека на диске (mp3, flac, wav, ogg/Vorbis, opus, m4a/aac, alac), вкладки: очередь / альбомы / исполнители / треки / папки / плейлисты, shuffle, repeat, кроссфейд, перемотка, избранное, CUE-листы, обложки. Полноэкранная визуализация: спектр с «шапочками» пиков, обложка альбома, кнопка на весь экран, автоскрытие курсора. Безрамочное окно, тёмная и светлая темы, ру/ен.

**Нормализация:** теги ReplayGain с целевой громкостью (−24…−12 LUFS, по умолчанию −18) и переключателем защиты от клиппинга. **Нативный формат:** приложение выводит родные частоту и разрядность файла и само не ресемплирует. PipeWire или общий WASAPI могут ресемплировать ниже по цепочке. Эксклюзивный режим, DSD и PEQ в этом релизе отсутствуют.

**Плейлисты:** создание, добавление треков, импорт `m3u8` / `m3u` / `pls` (старые m3u в CP1251 тоже читаются). **Поиск:** Enter играет выдачу начиная с первого результата.

Данные: Linux `~/.nemesis/`, Windows `%USERPROFILE%\.nemesis\` (библиотека, кэш обложек, окно, настройки).

---

## Linux и Windows

Windows x86_64 smoke: окно, скан, play/next/stop, SMTC, тема.

| | Linux (x86_64) | Windows 10/11 (x86_64) |
|---|---|---|
| Скан, очередь, shuffle, repeat, кроссфейд | да | да |
| Нативный PCM-формат | да | да |
| Громкость в нативном формате | `pactl` (sink-input) | громкость WASAPI-сессии |
| Нормализация ReplayGain, защита от клиппинга | да | да |
| Визуализация (полноэкранный спектр + обложка) | да | да |
| Плейлисты, импорт m3u/m3u8/pls | да | да |
| Медиа-клавиши без фокуса | MPRIS (`nemesis` на D-Bus) | SMTC |
| Тема / акцент системы | portal, GNOME, KDE | реестр |
| Безрамочное, прозрачное окно | да | да |
| Установка | `.deb` / `.rpm` / `.pkg.tar.zst` / tar.gz | **portable zip**, без установщика (установщик скоро) |

---

## Установка

На обычный ПК берите артефакт **x86_64**. Имена файлов — на странице релиза (тег совпадает с Cargo: `v0.1.3`).

### Debian / Ubuntu (12+ / 22.04+)

```bash
sudo dpkg -i nemesis_*_amd64.deb
sudo apt-get install -f   # если не хватает зависимостей
```

Нужны PipeWire или PulseAudio и драйвер GPU (Vulkan/OpenGL).

### Fedora (39+)

```bash
sudo dnf install nemesis-*.x86_64.rpm
```

### Arch / Manjaro / EndeavourOS

```bash
sudo pacman -U nemesis-*-x86_64.pkg.tar.zst
```

В AUR пакета пока нет.

### Linux без пакетa дистрибутива

Распакуйте `nemesis-*-linux-x86_64.tar.gz` в корень `/` (внутри `usr/bin/nemesis` и `.desktop`-файл). Или запустите бинарник из `usr/bin/` на системе с совместимой glibc.

### Windows

Распакуйте `nemesis-*-windows-x86_64-portable.zip` и запустите `nemesis.exe`. Дополнительных DLL не нужно. В меню «Пуск» сам не добавляется.

---

## Матрица поддержки

| Сборка | Статус |
|---|---|
| Linux x86_64, glibc ≥ 2.36 (Debian 12, Ubuntu 22.04, Fedora 39+, актуальный Arch) | основная |
| Windows 10/11 x86_64 | основная, portable |
| Linux aarch64, Windows ARM64 | **экспериментальная** |

Экспериментальные сборки **нигде не запускались**: пакет может не установиться, окно или звук могут не завестись. Для повседневного использования — x86_64.

Не тестировалось (отчёты приветствуются, если попробуете):

- DE / композитор кроме GNOME/KDE
- HiDPI, несколько мониторов, Windows snap
- аппаратные клавиши гарнитуры, SMTC/MPRIS из интерфейса ОС
- большие библиотеки, CUE, нестандартные теги
- нативный формат 24 бита / 96 кГц на конкретном устройстве
- Windows ARM / Linux ARM

---

## Стек

[Rust](https://rust-lang.org), [iced](https://github.com/iced-rs/iced) 0.14, [wgpu](https://github.com/gfx-rs/wgpu), [rodio](https://github.com/rustaudio/rodio), [cpal](https://github.com/RustAudio/cpal), [Symphonia](https://github.com/pdeljanov/symphonia) (flac / alac / mp4 / mp3 / ogg / wav), [libopus](https://github.com/xiph/opus) через audiopus, [Lofty](https://github.com/Serial-ATA/lofty-rs), [rusqlite](https://github.com/rusqlite/rusqlite) (SQLite вшит), [rfd](https://github.com/PolyMeilex/rfd) (диалоги файлов/папок), [souvlaki](https://github.com/sinono3/souvlaki) (медиа-клавиши). Звук: [ALSA](https://github.com/alsa-project/alsa-lib) → [PipeWire](https://github.com/PipeWire/pipewire)/[Pulse](https://github.com/pulseaudio/pulseaudio) на Linux, [WASAPI](https://learn.microsoft.com/en-us/windows/win32/coreaudio/wasapi) на Windows.

---

## Обратная связь

[Issues](https://github.com/aevahonin/Nemesis/issues) этого репозитория.

Пожалуйста, укажите:

1. ОС и версию (например Arch, Ubuntu 24.04, Windows 11 23H2), окружение (GNOME / KDE / другое).
2. Архитектуру: x86_64 или ARM.
3. Точное имя файла релиза (`nemesis_…deb`, zip, …).
4. Версию из «О приложении».
5. Что сделали, что ожидали, что произошло. Для багов интерфейса — скриншот.
6. Включён ли нативный формат, формат проблемного файла (flac 16/44.1 и т.п.).

У Windows-релиза нет консоли — лога cmd не будет. На Linux вставьте вывод терминала, если запускали из терминала.

Не прикладывайте всю библиотеку или `library.db`. Достаточно одного воспроизводящего файла (если можете поделиться) и шагов.
