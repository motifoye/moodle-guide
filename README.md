# Moodle guide

## Требования

Для сборки книги требуется [mdBook] >= v0.4.43. Для установки выполните команду:

[mdBook]: https://github.com/rust-lang-nursery/mdBook/

```bash
$ cargo install mdbook
```


## Сборка

Для того, чтобы скомпилировать книгу, перейдите в нужный каталог с помощью команды cd - first-edition для первого, либо second-edition для второго издания.
Далее введите следующую команду:

```bash
$ mdbook build
```

Результаты выполнения команды появятся в подкаталоге `book`. Для проверки откройте книгу в браузере.

_Firefox:_
```bash
$ firefox book/index.html                       # Linux
$ open -a "Firefox" book/index.html             # OS X
$ Start-Process "firefox.exe" .\book\index.html # Windows (PowerShell)
$ start firefox.exe .\book\index.html           # Windows (Cmd)
```

_Chrome:_
```bash
$ google-chrome book/index.html                 # Linux
$ open -a "Google Chrome" book/index.html       # OS X
$ Start-Process "chrome.exe" .\book\index.html  # Windows (PowerShell)
$ start chrome.exe .\book\index.html            # Windows (Cmd)
```

Для запуска тестов:

```bash
$ mdbook test
```
