<h1>Шпаргалка по использованию linux.</h1>

<h3 name="content">Содержание</h3>
<hr>

1. [Перемещение](#movement)
2. [Переменные окружения](#env)
3. [Команды head & tail](#ht)
4. [Команды touch, mv, cp, rm](#tmcr)
5. [Символьные ссылки](#ln)
6. [Установка пакетов](#install)

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

> **Note** \
> <i>Для команд <code><b>mv</b></code> <code><b>cp</b></code> <code><b>rm</b></code>
> желательно создать alias с ключем <code>-i</code>, чтобы случайно не перезаписать существующий
> файл.</i>

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

<h3 name="install">Установка пакетов</h3>
<hr>

> **Note** \
> <i>В большинстве случаев для установки новых пакетов требуются права суперпользователя.
> Далее все команды начинаются с sudo, подразумевая что пользователь состоит в группе sudo.
> Чтобы запустить пакет, обычно нужно ввести команду, которая соответствует названию пакета.</i>


- установить пакет для проверки размера файлов

  ```bash 
  sudo apt-get install ncdu
  ```
  
- запустить команду (первый запуск команды может занять продолжительное время):
  ```bash
  ncdu
  ```

- удалить зависимости, которые не используются ни одним приложением:

  ```bash
  sudo apt-get autoremove
  ```

Так происходит, потому что sudo apt-get purge не удаляет зависимости.

Посмотрим все доступные пакеты, которые у нас есть на данный момент.
apt-cache pkgnames

Пакетный менеджер apt предлагает нам возможность посмотреть информацию о пакете перед его загрузкой.

apt-cache search vsftpd

Мы можем получить более подробную информацию о пакете, включая версию, размер, зависимости, описание и многое другое. В
том числе о тех пакетах, которые не установлены

apt-cache show netcat

Apt также позволяет вам увидеть зависимости конкретного пакета.
apt-cache showpkg postgresql
Можем посмотреть общую статистику apt.
apt-cache stats

Команда upgrade в apt делает очень много важных вещей. Я советую всегда запускать сразу после установки свежей системы.
Она безопасно обновит все пакеты, у которых по какой-то причине были установлены старые версии. Запустим

sudo apt-get update
sudo apt-get upgrade

Если вы хотите просто обновить пакет для этого можно использовать команду install с ключом --only-upgrade. Запускаем

sudo apt-get install vim --only-upgrade

Удаление пакета
sudo apt-get remove

Если вы хотите удалить также и настройки пакета, для этого используем команду purge.
sudo apt-get purge

ChangeLog - это список изменений, которые были внесены в конкретную версию того или иного приложения.

sudo apt-get changelog postgresql

просмотр репозиториев:

sudo apt-cache policy
или
vim /etc/apt/sources.list

Сделать файл запускаемым chmod +x file.bin

# Добавление пользователей

(Я предпочитаю adduser)
sudo useradd newuser (Флаг -m создаст домашнюю директорию)
Проверяем наличие нового пользователя
cat /etc/passwd
Добавим пароль новому пользователю  
sudo passwd newuser
Удаление пользователей
sudo userdel newuser (Флаг -r удалит пользователя вместе с его домашеней директорией)

# Группы пользователей

Позволяет создать инфраструктуру

Создадим группу юристы

sudo groupadd lawyers

Проверим, что группа создана  
getent group

Добавим Кирилла в группу юристов  
sudo usermod -aG lawyers kirill

Проверяем наличие Кирилла в группе юристов

getent group lawyers

Добавим Илью в группу юристов

sudo usermod -a -G lawyers ilya

удалить Илью из группы lawyers.

sudo gpasswd --delete ilya lawyers

# Доступы

r - read
w - write
x - execute (исполнять)

Собственник Группа Все остальные
-rwx rwx rwx

Поменяем собственника файла test-2.sh
sudo chown maria:principals test-2.sh

Дадим возможность пользователю pavel работать с sudo. Для этого нужно добавить его в группе sudo.
Выполняем

sudo usermod -aG sudo pavel

Проверим наличие нового пользователя в группе sudo

getent group sudo

Теперь нам нужно предоставить ему доступы самой команды sudo. Вводим команду

sudo visudo

Открылся файл /etc/sudoers.d в текстовом редакторе nano. Единственное отличие от обыкновенного открытия в редакторе,
visudo проведёт валидацию файла перед его сохранением. Старайтесь всегда пользоваться именно visudo.
Добавляем такую строку в этот файл

pavel ALL=(ALL) NOPASSWD:ALL

Добавление alias

alias mv="mv -i"

find -name "test*" -type f

Это выражение означает, что мы ищем файлы, у которых слово test в названии. Обращаем ваше внимание, файл another_test не
появляется, что логично.
Воспользуемся флагом -iname, который не реагирует на регистр.

find -iname "test*"

Получаем список всех директорий.

find -type d

Получаем список всех обыкновенных файлов.

find -type f

Комбинируем флаги.

find -name "test*" -type f

find -size +100k

Мы получили список всех файлов, которые по размеру больше, чем 100 килобайт.
Найдём все пустые файлы

find -empty

Попробуем найти файлы, которые были обновлены за последнюю минуту.

find -mmin 1

Как вы можете заметить есть 3 файла, в каждом написано по одной строчке.
Заменим во всех строчках слово username на ваш text.

find . -type f -exec sed -i "s/username/text/g" {} \;

jobs -l - просмотр фоновых процессов
fg 1 - переключение на фоновый процесс

sudo apt-get install ncdu -y
- узнать размер файлов в указанной директории:

  ```bash
  ncdu
  ```