---
## Front matter
title: "Отчет по лабораторной работе № 6"
subtitle: "Установка и настройка системы управления базами данных MariaDB"
author: "Виноградова Мария Андреевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Приобретение практических навыков по установке и конфигурированию системы управления базами данных на примере программного обеспечения MariaDB.

# Задание

1. Установите необходимые для работы MariaDB пакеты (см. раздел 6.4.1).
2. Настройте в качестве кодировки символов по умолчанию utf8 в базах данных.
3. В базе данных MariaDB создайте тестовую базу addressbook, содержащую таблицу city с полями name и city, т.е., например, для некоторого сотрудника указан город, в котором он работает (см. раздел 6.4.1).
4. Создайте резервную копию базы данных addressbook и восстановите из неё данные (см. раздел 6.4.1).
5. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке базы данных MariaDB во внутреннем окружении виртуальной машины server. Соответствующим образом следует внести изменения в Vagrantfile (см. раздел 6.4.5).

# Выполнение лабораторной работы

## Установка MariaDB

Загружаем операционную систему и переходим в рабочий каталог с проектом. Запускаем виртуальную машину server. На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя. Устанавливаем необходимые для работы с базами данных пакеты командой `dnf -y install mariadb mariadb-server` (рис. [-@fig:001]).

![Вход на виртуальную машину server под пользователем mavinogradova, переход в режим суперпользователя и установка пакетов mariadb и mariadb-server](/home/mavinogradova/skreens/lbA6/1.png){#fig:001 width=70%}

Просматриваем конфигурационные файлы mariadb в каталоге `/etc/my.cnf.d` и в файле `/etc/my.cnf`.

Главный конфигурационный файл `/etc/my.cnf` содержит секцию `[client-server]`, которая читается и клиентом, и сервером, и директиву `!includedir /etc/my.cnf.d`, подключающую все `.cnf`-файлы из каталога `/etc/my.cnf.d`.

Построчно:

Группа строк, начинающихся с `#`, — комментарии, они игнорируются парсером и поясняют назначение следующей секции.

`[client-server]` — секция, читаемая и клиентом, и сервером, для опций, влияющих на обе стороны.

`!includedir /etc/my.cnf.d` — директива, подключающая все конфигурационные файлы из указанного каталога.

В каталоге `/etc/my.cnf.d` находятся файлы `auth_gssapi.cnf`, `client.cnf`, `enable_encryption.preset`, `mariadb-server.cnf`, `mysql-clients.cnf`, `provider_bzip2.cnf`, `provider_lz4.cnf`, `provider_lzo.cnf`, `provider_snappy.cnf`, `spider.cnf`.

Содержание подключённых `.cnf`-файлов:

`[mariadb]` — секция опций только для MariaDB. Строка `#plugin-load-add=auth_gssapi.so` закомментирована, значит плагин аутентификации GSSAPI не загружается. Последующие строки — комментарии, поясняющие назначение секций клиента.

`[client]` — секция общих опций для всех клиентов.

Группа строк, начинающихся с `#`, — комментарии, поясняющие назначение следующей секции.

`[client-mariadb]` — секция опций только для клиентов MariaDB.

`[server]` — секция общих опций для standalone-демона и embedded-серверов.

`[mysqld]` — секция опций только для демона `mysqld`.

`datadir=/var/lib/mysql` — каталог, где хранятся базы данных.

`socket=/var/lib/mysql/mysql.sock` — путь к UNIX-сокету.

`log-error=/var/log/mariadb/mariadb.log` — путь к лог-файлу ошибок.

`pid-file=/run/mariadb/mariadb.pid` — путь к PID-файлу.

`[galera]` — секция для синхронной репликации Galera. Все директивы внутри закомментированы, значит Galera не используется. Строка `#bind-address=0.0.0.0` — закомментированная директива, если её раскомментировать, сервер будет слушать все интерфейсы.

`[embedded]` — секция для встраиваемого сервера.

`[mariadb]` — секция опций только для MariaDB.

`[mariadb-10.11]` — секция опций только для MariaDB версии 10.11.

Секции `[mysql]`, `[mysql_upgrade]`, `[mysqladmin]`, `[mysqlbinlog]`, `[mysqlcheck]`, `[mysqldump]`, `[mysqlimport]`, `[mysqlshow]`, `[mysqlslap]` — настройки для отдельных клиентских утилит.

Секции `[server]` с директивами `plugin_load_add=provider_bzip2` и `provider_bzip2=force_plus_permanent` (и аналогично для lz4, lzo, snappy) подключают плагины сжатия InnoDB и запрещают их выгрузку.

Секция `[mariadb]` с закомментированной строкой `#plugin-load-add = ha_spider` означает, что движок Spider отключён; далее идёт комментарий со ссылкой на документацию (рис. [-@fig:002]).

![Просмотр конфигурационных файлов MariaDB](/home/mavinogradova/skreens/lbA6/2.png){#fig:002 width=70%}

Запускаем и включаем программное обеспечение mariadb командами `systemctl start mariadb` и `systemctl enable mariadb`. При включении создаются символические ссылки на службу `mariadb.service`. Статус службы — `active (running)`, автозапуск включён (`enabled`) (рис. [-@fig:003]).

![Запуск и включение службы mariadb, проверка её статуса](/home/mavinogradova/skreens/lbA6/3.png){#fig:003 width=70%}

Убеждаемся, что mariadb прослушивает порт.Для проверки использована альтернативная команда по номеру порта `ss -tulpen | grep 3306`. В выводе видно, что процесс `mariadbd` слушает TCP-порт 3306 на всех интерфейсах — по IPv4 (`0.0.0.0:3306`) и по IPv6 (`[::]:3306`) (рис. [-@fig:004]).

![Проверка прослушиваемого порта](/home/mavinogradova/skreens/lbA6/4.png){#fig:004 width=70%}

Запускаем скрипт конфигурации безопасности mariadb командой `mysql_secure_installation`. С помощью запустившегося диалога и путём выбора `[Y/n]` устанавливаем пароль для пользователя root базы данных (не путать с пользователем root операционной системы), отключаем удалённый корневой доступ, удаляем тестовую базу данных и любых анонимных пользователей. Все шаги сопровождаются сообщением `Success!` (рис. [-@fig:005]).

![Запуск mysql_secure_installation](/home/mavinogradova/skreens/lbA6/5.png){#fig:005 width=70%}

Для входа в базу данных с правами администратора базы данных вводим команду `mysql -u root -p` и пароль. Отображается приветствие MariaDB и приглашение `MariaDB [(none)]>` (рис. [-@fig:006]).

![Вход в базу данных с правами администратора](/home/mavinogradova/skreens/lbA6/6.png){#fig:006 width=70%}

Просматриваем список команд MySQL, введя `\h`. Отображается список команд клиента (рис. [-@fig:007]).

![Список команд клиента MariaDB](/home/mavinogradova/skreens/lbA6/7.png){#fig:007 width=70%}

Из приглашения интерактивной оболочки MariaDB для отображения доступных в настоящее время баз данных вводим MySQL-запрос `SHOW DATABASES;`. В системе доступны базы данных `information_schema` (служебная база с метаданными — имена таблиц, столбцов, права), `mysql` (системная база с учётными записями и привилегиями), `performance_schema` (база для мониторинга производительности) и `sys` (представления для удобной диагностики). Базы данных `test` нет, поскольку она была удалена при выполнении `mysql_secure_installation`. Для выхода из интерфейса интерактивной оболочки MariaDB вводим `exit;` (рис. [-@fig:008]).

![Вывод списка доступных баз данных MariaDB](/home/mavinogradova/skreens/lbA6/8.png){#fig:008 width=70%}

## Конфигурация кодировки символов

Входим в базу данных с правами администратора и для отображения статуса MariaDB вводим из приглашения интерактивной оболочки MariaDB команду `status`.

Построчно поясним вывод:

`mysql Ver 15.1 Distrib 10.11.18-MariaDB, for Linux (x86_64) using EditLine wrapper` — версия клиента mysql и дистрибутив.

`Connection id: 12` — идентификатор текущего соединения.

`Current database:` — текущая база данных, пусто, мы не сделали USE.

`Current user: root@localhost` — пользователь и хост подключения.

`SSL: Not in use` — шифрование не используется.

`Current pager: stdout` — вывод направлен в стандартный поток.

`Using outfile: ''` — файл для SELECT INTO OUTFILE не задан.

`Using delimiter: ;` — разделитель SQL-выражений точка с запятой.

`Server: MariaDB` — СУБД.

`Server version: 10.11.18-MariaDB` — версия сервера.

`Protocol version: 10` — версия протокола клиент-сервер.

`Connection: Localhost via UNIX socket` — подключение через UNIX-сокет.

`Server characterset: latin1` — кодировка сервера, сейчас latin1.

`Db characterset: latin1` — кодировка базы данных по умолчанию.

`Client characterset: utf8mb3` — кодировка клиента.

`Conn. characterset: utf8mb3` — кодировка соединения.

`UNIX socket: /var/lib/mysql/mysql.sock` — путь к сокету.

`Uptime: 11 min 6 sec` — время работы сервера.

`Threads: 1` — активные потоки.

`Questions: 23` — количество запросов.

`Slow queries: 0` — медленных запросов нет.

`Opens: 20` — открытий таблиц.

`Open tables: 13` — открытых таблиц.

`Queries per second avg: 0.034` — средняя скорость запросов (рис. [-@fig:009]).

![Вывод статуса MariaDB до изменения кодировки](/home/mavinogradova/skreens/lbA6/9.png){#fig:009 width=70%}

В каталоге `/etc/my.cnf.d` создаём файл `utf8.cnf` командами `cd /etc/my.cnf.d`, `touch utf8.cnf` и `nano utf8.cnf` (рис. [-@fig:010]).

![Создание файла конфигурации utf8.cnf](/home/mavinogradova/skreens/lbA6/10.png){#fig:010 width=70%}

В файле `utf8.cnf` указываем конфигурацию, задающую кодировку utf8 для клиента и для сервера:

`[client]` — секция для клиента.

`default-character-set = utf8` — кодировка клиента utf8.

`[mysqld]` — секция для сервера.

`character-set-server = utf8` — кодировка сервера utf8 (рис. [-@fig:011]).

![Содержимое файла utf8.cnf](/home/mavinogradova/skreens/lbA6/11.png){#fig:011 width=70%}

Перезапускаем MariaDB командой `systemctl restart mariadb`. Статус службы — `active (running)`, автозапуск включён (`enabled`) (рис. [-@fig:012]).

![Перезапуск службы mariadb](/home/mavinogradova/skreens/lbA6/12.png){#fig:012 width=70%}

Входим в базу данных с правами администратора и снова смотрим статус MariaDB. По сравнению с предыдущим выводом изменились параметры кодировки: `Server characterset` и `Db characterset` теперь имеют значение `utf8mb3` вместо `latin1`, `Client characterset` и `Conn. characterset` — также `utf8mb3`. Это означает, что настройка кодировки `utf8` в файле `utf8.cnf` применилась (рис. [-@fig:013]).

![Вывод статуса MariaDB после изменения кодировки](/home/mavinogradova/skreens/lbA6/13.png){#fig:013 width=70%}

## Создание базы данных

Входим в базу данных с правами администратора и создаём базу данных с именем `addressbook` командой `CREATE DATABASE addressbook CHARACTER SET utf8 COLLATE utf8_general_ci;`. Сервер отвечает `Query OK, 1 row affected`. Переходим к базе данных командой `USE addressbook;`. Сервер отвечает `Database changed` (рис. [-@fig:014]).

![Создание базы данных addressbook](/home/mavinogradova/skreens/lbA6/14.png){#fig:014 width=70%}

Отображаем имеющиеся в базе данных `addressbook` таблицы командой `SHOW TABLES;`. Вывод пустой (`Empty set`), таблиц пока нет. Создаём таблицу `city` с полями `name` и `city` командой `CREATE TABLE city(name VARCHAR(40), city VARCHAR(40));`.

Заполняем несколько строк таблицы данными по аналогии в соответствии с синтаксисом MySQL: `INSERT INTO city(name,city) VALUES ('Иванов','Москва');`, `INSERT INTO city(name,city) VALUES ('Петров','Сочи');`, `INSERT INTO city(name,city) VALUES ('Сидоров','Дубна');`.

Делаем MySQL-запрос `SELECT * FROM city;`. В результате выводятся три строки — по одной на каждого сотрудника, где указан город, в котором он работает: Иванов/Москва, Петров/Сочи, Сидоров/Дубна. Запрос `SELECT * FROM city;` выбирает все столбцы из таблицы `city` (рис. [-@fig:015]).

![Создание таблицы city и заполнение её данными](/home/mavinogradova/skreens/lbA6/15.png){#fig:015 width=70%}

Создаём пользователя для работы с базой данных `addressbook`, вместо user до знака @ используем свой логин, и задаём для него пароль командой `CREATE USER mavinogradova@'%' IDENTIFIED BY 'password';`.

Предоставляем права доступа созданному пользователю на действия с базой данных `addressbook` (просмотр, добавление, обновление, удаление данных) командой `GRANT SELECT, INSERT, UPDATE, DELETE ON addressbook.* TO mavinogradova@'%';`.

Обновляем привилегии (права доступа) базы данных командой `FLUSH PRIVILEGES;`. Смотрим общую информацию о таблице `city` базы данных `addressbook` командой `DESCRIBE city;`. Выходим из окружения MariaDB командой `quit` (рис. [-@fig:016]).

![Создание пользователя и выдача прав](/home/mavinogradova/skreens/lbA6/16.png){#fig:016 width=70%}

Просматриваем список баз данных командой `mysqlshow -u root -p`. Просматриваем список таблиц базы данных `addressbook` командой `mysqlshow -u root -p addressbook`. Аналогично проверяем доступ под созданным пользователем командой `mysqlshow -u mavinogradova -p addressbook` (рис. [-@fig:017]).

![Просмотр списка баз данных и таблиц](/home/mavinogradova/skreens/lbA6/17.png){#fig:017 width=70%}

## Резервные копии

На виртуальной машине server создаём каталог для резервных копий командой `mkdir -p /var/backup`. Создаём резервную копию базы данных `addressbook` командой `mysqldump -u root -p addressbook > /var/backup/addressbook.sql`.

Просматриваем первые строки дампа:

`MariaDB dump 10.19 Distrib 10.11.18-MariaDB, for Linux (x86_64)` — заголовок дампа.

`Host: localhost` — хост, с которого сделан дамп.

`Database: addressbook` — имя базы данных.

`SET NAMES utf8` — подтверждает работу настройки кодировки utf8.

`SET TIME_ZONE='+00:00'` — часовой пояс UTC на время восстановления.

`SET UNIQUE_CHECKS=0` — отключение проверок уникальности для ускорения загрузки.

`SET FOREIGN_KEY_CHECKS=0` — отключение проверок внешних ключей.

`SET SQL_MODE='NO_AUTO_VALUE_ON_ZERO'` — корректная работа с AUTO_INCREMENT.

Далее начинается блок структуры таблицы `city` (рис. [-@fig:018]).

![Создание каталога /var/backup и обычной резервной копии](/home/mavinogradova/skreens/lbA6/18.png){#fig:018 width=70%}

Создаём сжатую резервную копию базы данных `addressbook` командой `mysqldump -u root -p addressbook | gzip > /var/backup/addressbook.sql.gz`.

Создаём сжатую резервную копию с указанием даты создания копии.
В результате создан файл `addressbook.20261005.142156.sql.gz`. Проверяем содержимое каталога. В каталоге `/var/backup/` теперь три резервные копии: `addressbook.sql` (2007 байт), `addressbook.sql.gz` (797 байт) и `addressbook.20261005.142156.sql.gz` (797 байт) (рис. [-@fig:019]).

![Создание сжатых резервных копий](/home/mavinogradova/skreens/lbA6/19.png){#fig:019 width=70%}

Восстанавливаем базу данных `addressbook` из несжатой резервной копии. Для проверки корректности восстановления сначала удаляем таблицу `city` командой `mysql -u root -p123456 -e "DROP TABLE addressbook.city;"`.

Затем проверяем, что таблиц в базе нет, командой `mysql -u root -p123456 -e "SHOW TABLES FROM addressbook;"`. Вывод пустой.

Выполняем восстановление командой `mysql -u root -p addressbook < /var/backup/addressbook.sql`.

После восстановления проверяем содержимое таблицы командой `mysql -u root -p123456 -e "SELECT * FROM addressbook.city;"`. Снова выводятся три строки, что подтверждает корректность восстановления из несжатой резервной копии (рис. [-@fig:020]).

![Восстановление базы данных из несжатой резервной копии](/home/mavinogradova/skreens/lbA6/20.png){#fig:020 width=70%}

Восстанавливаем базу данных `addressbook` из сжатой резервной копии. Снова удаляем таблицу командой `mysql -u root -p123456 -e "DROP TABLE addressbook.city;"`. Затем выполняем восстановление из сжатой копии командой `zcat /var/backup/addressbook.sql.gz | mysql -u root -p addressbook`.

После восстановления проверяем содержимое таблицы командой `mysql -u root -p123456 -e "SELECT * FROM addressbook.city;"`. Снова выводятся три строки, что подтверждает корректность восстановления из сжатой резервной копии (рис. [-@fig:021]).

![Восстановление базы данных из сжатой резервной копии](/home/mavinogradova/skreens/lbA6/21.png){#fig:021 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`, создаём в нём каталог `mysql`, в который помещаем в соответствующие подкаталоги конфигурационные файлы MariaDB и резервную копию базы данных `addressbook`.

Выполняем команды: `cd /vagrant/provision/server`, `mkdir -p /vagrant/provision/server/mysql/etc/my.cnf.d`, `mkdir -p /vagrant/provision/server/mysql/var/backup`, `cp -R /etc/my.cnf.d/utf8.cnf /vagrant/provision/server/mysql/etc/my.cnf.d/`, `cp -R /var/backup/* /vagrant/provision/server/mysql/var/backup/`.

Проверяем результат командой `find /vagrant/provision/server/mysql -type f`. В каталоге провижининга находятся файл `utf8.cnf` и три резервные копии (рис. [-@fig:022]).

![Копирование конфигурационного файла и резервных копий в каталог провижининга](/home/mavinogradova/skreens/lbA6/22.png){#fig:022 width=70%}

В каталоге `/vagrant/provision/server` создаём исполняемый файл `mysql.sh` командами `cd /vagrant/provision/server`, `touch mysql.sh`, `chmod +x mysql.sh` и `nano mysql.sh` (рис. [-@fig:023]).

![Создание исполняемого файла mysql.sh](/home/mavinogradova/skreens/lbA6/23.png){#fig:023 width=70%}

Прописываем в файле `mysql.sh` следующий скрипт:

`#!/bin/bash` — шебанг, указывающий интерпретатор.

`echo "Provisioning script $0"` — вывод сообщения о запуске скрипта.

`systemctl restart named` — перезапуск службы named.

`echo "Install needed packages"` — вывод сообщения.

`dnf -y install mariadb mariadb-server` — установка пакетов mariadb и mariadb-server.

`echo "Copy configuration files"` — вывод сообщения.

`cp -R /vagrant/provision/server/mysql/etc/* /etc` — копирование конфигурационных файлов в `/etc`.

`mkdir -p /var/backup` — создание каталога для резервных копий.

`cp -R /vagrant/provision/server/mysql/var/backup/* /var/backup` — копирование резервных копий.

`echo "Start mysql service"` — вывод сообщения.

`systemctl enable mariadb` — включение автозапуска.

`systemctl start mariadb` — запуск службы.

`if [[ ! -d /var/lib/mysql/mysql ]]` — условный блок, выполняется только при первом запуске.

`echo "Securing mariadb"` — вывод сообщения.

`mysql_secure_installation <<EOF` с ответами `y`, `123456`, `123456`, `y`, `y`, `y` — первичная настройка безопасности.

`echo "Create database"` — вывод сообщения.

`mysql -u root -p123456 <<EOF` с командой `CREATE DATABASE addressbook CHARACTER SET utf8 COLLATE utf8_general_ci;` — создание базы данных.

`mysql -u root -p123456 addressbook < /var/backup/addressbook.sql` — восстановление базы данных из резервной копии.

`fi` — конец условного блока.

Этот скрипт, по сути, повторяет произведённые действия по установке и настройке сервера баз данных (рис. [-@fig:024]).

![Содержимое скрипта провижининга mysql.sh](/home/mavinogradova/skreens/lbA6/24.png){#fig:024 width=70%}

Для отработки созданного скрипта во время загрузки виртуальных машин в конфигурационном файле `Vagrantfile` добавлена в конфигурации сервера следующая запись:

`server.vm.provision "server mysql"` — объявление нового провижининга.

`type: "shell"` — тип провижининга — shell.

`preserve_order: true` — сохранение порядка выполнения.

`path: "provision/server/mysql.sh"` — путь к скрипту.

Данный блок добавлен в секцию `server` после блока `server http` (рис. [-@fig:025]).

![Фрагмент Vagrantfile с добавленным блоком провижининга](/home/mavinogradova/skreens/lbA6/25.png){#fig:025 width=70%}

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и конфигурированию СУБД MariaDB. Установлены пакеты `mariadb` и `mariadb-server`, служба включена в автозапуск и запущена. Настроена кодировка символов по умолчанию utf8 через файл `utf8.cnf`. Создана тестовая база данных `addressbook` с таблицей `city` и тремя записями, создан пользователь с правами `SELECT`, `INSERT`, `UPDATE`, `DELETE` на эту базу. Освоены резервное копирование (несжатое, сжатое, с датой в имени) и восстановление базы данных. Написан скрипт провижининга `mysql.sh`, автоматизирующий установку и настройку MariaDB, и внесены соответствующие изменения в `Vagrantfile`.

# Ответы на контрольные вопросы

1. Какая команда отвечает за настройки безопасности в MariaDB? Команда `mysql_secure_installation`. Она запускает интерактивный скрипт, который позволяет установить пароль для пользователя root базы данных, отключить удалённый корневой доступ, удалить анонимных пользователей и тестовую базу данных, а также перезагрузить таблицы привилегий.

2. Как настроить MariaDB для доступа через сеть? В конфигурационном файле `mariadb-server.cnf` в каталоге `/etc/my.cnf.d` задать директиву `bind-address=0.0.0.0`, чтобы сервер слушал не только localhost. Также открыть порт 3306 в межсетевом экране и создать пользователя с хостом `'%'` (например, `CREATE USER 'user'@'%' IDENTIFIED BY 'password';`), а затем выдать ему соответствующие права через `GRANT`.

3. Какая команда позволяет получить обзор доступных баз данных после входа в среду оболочки MariaDB? Команда `SHOW DATABASES;`.

4. Какая команда позволяет узнать, какие таблицы доступны в базе данных? Команда `SHOW TABLES;` (после выполнения `USE имя_базы;`) или `SHOW TABLES FROM имя_базы;` без предварительного переключения базы.

5. Какая команда позволяет узнать, какие поля доступны в таблице? Команда `DESCRIBE имя_таблицы;` или `SHOW COLUMNS FROM имя_таблицы;`.

6. Какая команда позволяет узнать, какие записи доступны в таблице? Команда `SELECT * FROM имя_таблицы;`.

7. Как удалить запись из таблицы? Командой `DELETE FROM имя_таблицы WHERE условие;`. Если не указать `WHERE`, удалятся все записи таблицы.

8. Где расположены файлы конфигурации MariaDB? Что можно настроить с их помощью? Главный конфигурационный файл — `/etc/my.cnf`, также подключаются файлы из каталога `/etc/my.cnf.d/*.cnf`. С их помощью настраиваются кодировка символов, порт, `bind-address`, каталог данных, путь к сокету, лог-файлы, PID-файл и другие параметры.

9. Где располагаются файлы с базами данных MariaDB? В каталоге, заданном директивой `datadir`. По умолчанию это `/var/lib/mysql`.

10. Как сделать резервную копию базы данных и затем её восстановить? Резервная копия создаётся командой `mysqldump -u root -p addressbook > /var/backup/addressbook.sql`, сжатая — `mysqldump -u root -p addressbook | gzip > /var/backup/addressbook.sql.gz`. Восстановление из несжатой копии — `mysql -u root -p addressbook < /var/backup/addressbook.sql`, из сжатой — `zcat /var/backup/addressbook.sql.gz | mysql -u root -p addressbook`.

# Список литературы

1. Методические указания к лабораторной работе № 6 «Установка и настройка системы управления базами данных MariaDB».
2. MariaDB Foundation. — URL: https://mariadb.org (дата обр. 13.09.2021).
3. Документация по MariaDB. — URL: https://mariadb.com/kb/ru/5306/.
4. Основы языка SQL. — URL: http://citforum.ru/programming/321less/les44.shtml (дата обр. 13.09.2021).
