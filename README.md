## Laboratory work XI

Данная лабораторная работа посвещена изучению процесса создания сеансов совместной разработки с использованием инструмента **ngrok**

```sh
$ open https://ngrok.com/
```

## Tasks

- [ ] 1. Ознакомиться со ссылками учебного материала
- [ ] 2. Выполнить инструкцию учебного материала
- [ ] 3. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial
Переходим в корневой каталог. Создаём две папки install и tmp. Задаём переменную HOME_PREFIX, после выводим её значение. Задаём переменную USERNAME
```sh
$ cd ~
$ mkdir install
$ mkdir tmp
$ export HOME_PREFIX=`pwd`/install
$ echo $HOME_PREFIX
$ export USERNAME=`whoami`
```
Переходим в папку tmp
```sh
$ cd tmp
```
Скачиваем архив, после распаковываем его и выводим то, что в нём было. Переходим в папку libevent-2.1.8-stable. Дальше указываем, где будет проходить установка. Устанавливаем. Возвращаемся в предыдущую папку. libevent (из википедии) библиотека программного обеспечения, обеспечивающая асинхронное уведомление о событиях. Нужна для работы с libevent API
```sh
$ wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz
$ tar -xvzf libevent-2.1.8-stable.tar.gz
$ cd libevent-2.1.8-stable
$ ./configure --prefix=${HOME_PREFIX}
$ make && make install
$ cd ..
```
Здесь действия аналогичны. ncurses (из википедии) библиотека, написанная на языках Си и Ада, предназначенная для управления вводом-выводом на терминал, в числе прочего, библиотека позволяет задавать экранные координаты (в знакоместах) и цвет выводимых символов.
```sh
$ wget http://invisible-island.net/datafiles/release/ncurses.tar.gz
$ tar -xvzf ncurses.tar.gz
$ cd ncurses-5.9
$ ./configure --prefix=${HOME_PREFIX}
$ make && make install
$ cd ..
```
Действия аналогичны предыдущим.Tmux (терминальный мультиплексор) позволяет работать с несколькими сессиями в 1 окне. Вместо нескольких окон терминала к серверу — вы можете использовать одно. 

```sh
$ wget https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz
$ tar -xvzf tmux-2.5.tar.gz
$ cd tmux-2.5
$ ./configure --prefix=${HOME_PREFIX} CFLAGS="-I${HOME_PREFIX}/include -I${HOME_PREFIX}/include/ncurses" LDFLAGS="-L${HOME_PREFIX}/lib"
$ make && make install
$ cd ..
```
Скачиваем архив с ngrok, распаковываем его. Перемещаем папку с ngrok в директорию bin
```sh
$ wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
$ unzip ngrok-stable-linux-amd64.zip
$ mv ngrok ${HOME_PREFIX}/bin
```
Задаём переменные LD_LIBRARY_PATH и PATH. Запускаем tmux
```sh
$ export LD_LIBRARY_PATH=${HOME_PREFIX}/lib
$ export PATH="${HOME_PREFIX}/bin:${PATH}"
$ tmux
```
Перезодим в корневой каталог и удаляем tmp и install
```sh
$ cd ~
$ rm -rf tmp install
```
Другой способ установки tmux и ngrok
```sh
$ brew install tmux ngrok # or use linuxbrew 🎉
```
Запускаем новую сессию
```sh
$ tmux new -s session_with_group
```
Открываем сайт, регистрируемся. Придаём переменной NGROK_TOKEN значение токена. Указываем токен в качестве аутентификационного. Указываем порт, чтобы получить доступ по SSH
```sh
# Alisa:
$ open https://ngrok.com/signup
$ export NGROK_TOKEN=<токен>
$ ngrok authtoken ${NGROK_TOKEN}
$ ngrok tcp 22
<порт_ngrok_сервера>
```
Подключемся к созданной групповой сессии. Используем вертикальное разделение консольного окна
```sh
# Bob:
$ ssh ${USERNAME}@0.tcp.ngrok.io -p<порт_ngrok_сервера>
<пароль_от_учетной_записи>
$ tmux a -t session_with_group
$ <C-B>"
```

## Links

- [Tmux](https://raw.githubusercontent.com/tmux/tmux/master/README)
- [Libevent](http://libevent.org)
- [Ncurses](http://invisible-island.net/ncurses/)

```
Copyright (c) 2015-2021 The ISC Authors
```
