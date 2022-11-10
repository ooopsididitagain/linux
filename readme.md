<h1>Шпаргалка по linux.</h1>

<h4>Содержание</h4>

1. [Перемещение](#Перемещение)
2. [Переменные](#Переменные%20окружения)

<h3>Перемещение</h3>

   - <code><b>cd ~</b></code>: перейти в домашнюю директорию.
   - <code><b>cd -</b></code>: перейти в предыдущую директорию.

<h3>Переменные окружения</h3>

- <code><b>echo $VAR</b></code>: посмотреть значение переменной.
- <code><b>echo $HOME</b></code>: посмотреть значение переменной $HOME.
    - <code><b>$HOME</b></code>: абсолютный адрес домашней директории.

<h3>head & tail</h3>

- <code><b>head ~/somefile.log</b></code>: показать первые 10 строк файла.

- <code><b>head -n 1000 ~/somefile.log</b></code>: показать первые 1000 строк файла.

- <code><b>tail -f ~/somefile.log</b></code>: ключ -f позволяет показывать новые строки по мере их появления.

<code>tail -100f ~/somefile.log</code> - следить за обновлением файла, только показать перед этим последние 100 строк.

<code>> newfile.txt == touch newfile.txt</code>

mv -t dest/ test-1.txt test-2.txt test-3.txt - перенос нескольких файлов одной командой.

mv -i - использовать команду необходимо с -i, чтобы случайно не перезаписать существующий файл.

cp -i - использовать команду необходимо с -i, чтобы случайно не перезаписать существующий файл.

rm -i - использовать команду необходимо с -i, чтобы получать запрос на удаление каждого файла.

ncdu - узнать размер файлов в указаной директории.

#

Ссылки мягкие и жесткие
Мягкие не следят за состоянием файла.
Создание мягкой ссылки(указывать полный путь до файла!):
ln -s test1.txt soft-link.txt
Проверка типа файла (l в начале укажет на тип "ссылка")
ls -l

Жесткие ссылки работают с индексным дескриптером, храня мета данные файла. Создание жесткой ссылки:
ln test1.txt hard-link.txt
Проверка типа файла
ls -li

(Файлы в файловой системе являются жесткими ссылками)

### Установка пакетов

sudo apt-get install ncdu - для проверки размера файлов
sudo apt-get install htop - диспетчер задач

Удаление зависимостей, которые не используются ни одним приложением:
sudo apt-get autoremove
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

## test

<h3>test2</h3>