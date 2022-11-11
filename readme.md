![Last Commit](https://img.shields.io/github/last-commit/ooopsididitagain/linux?style=for-the-badge&logo=github)

<h3 name="content">Содержание</h3>
<hr>

1.  [Перемещение по системе](#movement)
2.  [Переменные окружения](#env)
3.  [Команды head & tail](#ht)
4.  [Команды touch, mv, cp, rm](#tmcr)
5.  [Символьные ссылки](#ln)
6.  [Пакетный менеджер apt](#apt)
7.  [Пользователи и группы](#usgr)
8.  [Доступы](#pr)
9.  [Alias](#al)
10. [Find](#fi)
11. [Прочее](#an)

<h3 name="movement">Перемещение</h3>
<hr>

- перейти в домашнюю директорию:

  ```bash
  cd ~
  ```
- перейти в предыдущую директорию:

  ```bash
  cd -
  ```

<h3 name="env">Переменные окружения</h3>
<hr>

- показать все переменные окружения:

  ```bash
  printenv 
  ```
- посмотреть значение переменной:

  ```bash
  echo $VAR
  ```
- посмотреть значение переменной <code><b>$HOME</b></code>:

    - <small><code><b>$HOME</b></code>: <i>абсолютный адрес домашней директории.</i></small>

  ```bash
  echo $HOME
  ```

<h3 name="ht">Команды head & tail</h3>
<hr>

- показать первые 10 строк файла:

  ```bash
  head /var/log/syslog
  ```

- показать первые 1000 строк файла:

  ```bash
  head -n 1000 /var/log/syslog
  ```

- показать последние 10 строк файла:

  ```bash
  tail /var/log/syslog
  ```

- ключ -f позволяет показывать новые строки по мере их появления:

  ```bash
  tail -f /var/log/syslog
  ```

- следить за обновлением файла, только показать перед этим последние 100 строк:

  ```bash
  tail -100f /var/log/syslog
  ```

<h3 name="tmcr">Команды touch mv, cp, rm</h3>
<hr>

> **Warning** \
> <i>Для команд <code><b>mv</b></code> <code><b>cp</b></code> <code><b>rm</b></code>
> желательно создать alias с ключом <code>-i</code>, чтобы случайно не перезаписать существующий
> файл или не удалить без дополнительного оповещения.</i>

- перенос нескольких файлов одной командой:

  ```bash
  mv -t dest/ test-1.txt test-2.txt test-3.txt
  ```

- создать файл можно командой <code><b>touch</b></code> или <code><b>></b></code>
    - <small>Хотя эта команда предназначена не для создания файла</small>

  ```bash
  > newfile.txt
  touch newfile.txt
  ```

<h3 name="ln">Ссылки мягкие и жесткие</h3>
<hr>

> **Note** \
> soft link & hard link \
> <i>Мягкие ссылки, в отличии от жестких, не следят за состоянием файла. \
> Жесткие ссылки работают с индексным дескриптором, храня мета данные файла. \
> Файлы в файловой системе являются жесткими ссылками. \
> При создании мягкой ссылки необходимо указывать полный путь до файла.</i>

- проверка типа файла с помощью <code>ls -l</code>, l в начале укажет на тип "ссылка".

```bash
lrwxrwxrwx   1 root root      20 мар 22  2020 libsidplay2.so.1 -> libsidplay2.so.1.0.1
-rw-r--r--   1 root root  278808 мар 22  2020 libsidplay2.so.1.0.1
```

- создание мягкой ссылки:

  ```bash
  ln -s test1.txt soft-link.txt
  ```
- создание жесткой ссылки:

  ```bash
  ln test1.txt hard-link.txt
  ```

<h3 name="apt">Пакетный менеджер apt</h3>
<hr>

> **Note** \
> <i>В большинстве случаев для установки новых пакетов требуются права суперпользователя.
> Далее все команды начинаются с sudo, подразумевая что пользователь состоит в группе sudo.
> Что-бы запустить пакет, обычно нужно ввести команду, которая соответствует названию пакета.</i>

- пакетный менеджер <code><b>apt</b></code> предлагает нам возможность посмотреть информацию о пакете перед его
  загрузкой:

  ```bash
  sudo apt-cache search ncdu
  ```

- установить пакет для проверки размера файлов:

  ```bash
  sudo apt-get install ncdu
  ```

- запустить команду (первый запуск команды может занять продолжительное время):
  ```bash
  ncdu
  ```


- удалить пакет:

  ```bash
  sudo apt-get remove
  ```

- удалить пакет и его настройки:

  ```bash
  sudo apt-get purge ncdu
  ```

- удалить зависимости, которые не используются ни одним приложением:

  > **Note** \
  > <i>Да, зависимости остаются после удаления пакета в системе.
  > Так происходит, потому что <code><b>sudo apt-get purge</b></code> не удаляет зависимости.</i>

  ```bash
  sudo apt-get autoremove
  ```

- посмотреть все доступные пакеты, которые у нас есть на данный момент:

  ```bash
  sudo apt-cache pkgnames
  ```

- получить более подробную информацию о пакете, включая <u>версию</u>, <u>размер</u>, <u>зависимости</u>,
  <u>описание</u> и <u>многое другое</u>, в том числе о тех пакетах, которые не установлены:

  ```bash
  sudo apt-cache show ncdu
  ```

- посмотреть зависимости конкретного пакета:

  ```bash
  sudo apt-cache showpkg postgresql
  ```

- посмотреть общую статистику <code><b>apt</b></code>:

  ```bash
  sudo apt-cache stats
  ```

- команда <code><b>upgrade</b></code> делает очень много важных вещей. Рекомендуется всегда запускать эту команду
  сразу после установки свежей системы. Она безопасно обновит все пакеты, у которых по какой-то причине были
  установлены старые версии:

  ```bash
  sudo apt-get update
  sudo apt-get upgrade
  ```

- просто обновить пакет:

  ```bash
  sudo apt-get install vim --only-upgrade
  ```

- список изменений, которые были внесены в конкретную версию того или иного пакета:

  ```bash
  sudo apt-get changelog postgresql
  ```
- просмотр репозиториев:

  ```bash
  sudo apt-cache policy
  ```
  или

  ```bash
  vim /etc/apt/sources.list
  ```

<h3 name="usgr">Пользователи и группы</h3>
<hr>

> **Note** \
> <i>Я предпочитаю команду <code><b>adduser</b></code>, это удобно, но есть и команда <code><b>useradd</b></code>,
> которая значительно отличается, например, не создается домашняя директория.
> Флаг <code><b>useradd -m</b></code> создаст домашнюю директорию.</i>

- создать пользователя, проверить наличие нового пользователя и добавить пароль:

  ```bash
  sudo useradd newuser
  cat /etc/passwd
  sudo passwd newuser
  ```

- удалить пользователя:
  <i>Флаг <code><b>-r</b></code> удалит пользователя вместе с его домашней директорией.</i>

  ```bash
  sudo userdel newuser   
  ```
- посмотреть список существующих групп

  ```bash
  getent group
  ```

- посмотреть список существующих групп

  ```bash
  getent group
  ```


- создать новую группу

  ```bash
  sudo groupadd newgroup
  ```

- добавить пользователя в группу

  ```bash
  sudo usermod -aG newgroup newuser
  ```

- удалить пользователя из группы

  ```bash
  sudo gpasswd --delete newuser newgroup
  ```

<h3 name="pr">Доступы</h3>
<hr>

> **Note** \
> r - read \
> w - write \
> x - execute (исполнять) \
> todo: добавить цифровое обозначение

- распределение прав:

  ```bash
  Собственник Группа Все остальные
  -rwx rwx rwx
  ```

- сделать файл запускаемым

  ```bash
  chmod +x file.bin
  ```

- поменяем собственника файла:

  ```bash
  sudo chown newuser:newgroup test-2.sh
  ```


- добавить пользователя в группу sudo:

  ```bash
  sudo usermod -aG sudo newuser
  ```

- Предоставить доступы самой команды sudo:

  ```bash
  sudo visudo
  ```

> **Note** \
> <i>Открылся файл <b>/etc/sudoers.d</b> в текстовом редакторе nano. Единственное отличие от обыкновенного открытия в
> редакторе, visudo проведёт валидацию файла перед его сохранением. Всегда пользуемся visudo. Добавляем следующую
> строку в файл:</i>

  ```bash
  newuser ALL=(ALL) NOPASSWD:ALL
  ```

<h3 name="al">Alias</h3>
<hr>

> **Note** \
> <i>alias необходимо добавить в файл .bashrc. Пример файла есть в репозитории.</i>

- создать новый alias:

  ```bash
  alias mv="mv -i"
  ```

<h3 name="fi">Find</h3>
<hr>

- найти файл, у которого есть слово <b>test</b> в названии:

  ```bash
  find -name "test*" -type f
  ```

- независимый от регистра поиск:

  ```bash
  find -iname "test*"
  ```

- получить список всех директорий:

  ```bash
  find -type d
  ```

- получить список всех обыкновенных файлов:

  ```bash
  find -type f
  ```

- комбинируем флаги:

  ```bash
  find -name "test*" -type f
  ```

- получить файлы, которые по размеру больше чем 100 килобайт:

  ```bash
  find -size +100k
  ```

- найти все пустые файлы:

  ```bash
  find -empty
  ```

- найти файлы, которые были обновлены за последнюю минуту:

  ```bash
  find -mmin 1
  ```

<h3 name="an">Прочее</h3>
<hr>

- узнать модель монитора:

  ```bash
  xrandr | grep " connected" | cut -f1 -d " "
  ```

- изменить яркость:

  ```bash
  xrandr --output [monitor-name] --brightness [brightness-level]
  xrandr --output LVDS-1 --brightness 0.75
  ```

- просмотр фоновых процессов:

  ```bash
  jobs -l
  ```

- переключение на фоновый процесс:

  ```bash
  fg 1
  ```


