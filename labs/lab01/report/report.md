---
## Front matter
title: "Отчет по лабораторной работе № 1"
subtitle: "Подготовка лабораторного стенда"
author: "Виноградова Мария Андреевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
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
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
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
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Целью данной работы является приобретение практических навыков установки Rocky Linux на виртуальную машину с помощью инструмента Vagrant

# Задание

1. Сформируйте box-файл с дистрибутивом Rocky Linux для VirtualBox.
2. Запустите виртуальные машины сервера и клиента и убедитесь в их работоспособности.
3. Внесите изменения в настройки загрузки образов виртуальных машин server и client, добавив пользователя с правами администратора и изменив названия хостов.
4. Скопируйте необходимые для работы с Vagrant файлы и box-файлы виртуальных машин на внешний носитель.

# Выполнение лабораторной работы

### Подготовка рабочего каталога

Перед началом работы с Vagrant создаём каталог для проекта. В ОС Windows, например, `D:\work\mavinogradova\packer` и `D:\work\mavinogradova\vagrant`, где `mavinogradova` — идентифицирующее имя пользователя.

Структура рабочего каталога (рис. [-@fig:001]).

![Структура рабочего каталога](/home/mavinogradova/skreens/lbA1/1.png){#fig:001 width=70%}

В созданном рабочем каталоге в подкаталоге `packer` размещаем образ варианта операционной системы Rocky Linux (`Rocky-10.2-x86_64-minimal.iso`). Содержимое подкаталога `packer`(рис. [-@fig:002]).

![Содержимое каталога packer](/home/mavinogradova/skreens/lbA1/2.png){#fig:002 width=70%}

В подкаталоге `vagrant` размещаем файл `Vagrantfile` — файл с конфигурацией запуска виртуальных машин, а также файл `Makefile` — набор инструкций для программы make по работе с Vagrant. Содержимое подкаталога `vagrant`  (рис. [-@fig:003]).

![Содержимое каталога vagrant](/home/mavinogradova/skreens/lbA1/3.png){#fig:003 width=70%}

В подкаталоге `vagrant` создаём каталог `provision` с подкаталогами `default`, `server` и `client`, в которых будут размещаться скрипты, изменяющие настройки внутреннего окружения базового (общего) образа виртуальной машины, сервера или клиента соответственно. Содержимое подкаталога `provision`( рис. [-@fig:004]).

![Содержимое каталога provision](/home/mavinogradova/skreens/lbA1/4.png){#fig:004 width=70%}


### Настройка скриптов провижининга

В каталоге `default` размещаем скрипт `01-user.sh` по изменению названия виртуальной машины. В этом скрипте в качестве значения переменной `username` вместо `user` указываем имя пользователя, совпадающее с логином, — `mavinogradova` (рис. [-@fig:005]).

![Скрипт 01-user.sh](/home/mavinogradova/skreens/lbA1/5.png){#fig:005 width=70%}

В каталоге `default` размещаем скрипт `01-hostname.sh` по изменению названия виртуальной машины. В этом скрипте в качестве значения переменной `username` вместо `user` указываем имя пользователя — `mavinogradova` (рис. [-@fig:006]).

![Скрипт 01-hostname.sh](/home/mavinogradova/skreens/lbA1/6.png){#fig:006 width=70%}

В каталоге `server` размещаем скрипт `02-forward.sh`, обеспечивающий корректную маршрутизацию ip-адресов между сервером и клиентом.

В каталоге `client` размещаем скрипт `01-routing.sh`, обеспечивающий корректную работу сетевых интерфейсов клиента.

### Конфигурационный файл Vagrantfile

В файле `Vagrantfile` блок общей конфигурации `## Common configuration` содержит инструкции по применению скриптов провижининга к обеим виртуальным машинам (рис. [-@fig:007]).

![Vagrantfile — common configuration](/home/mavinogradova/skreens/lbA1/7.png){#fig:007 width=70%}

Блок конфигурации сервера `## Server configuration` задаёт имя box-файла, hostname, параметры SSH, тип сетевого соединения и параметры провайдера VirtualBox (рис. [-@fig:008]).

![Vagrantfile — server configuration](/home/mavinogradova/skreens/lbA1/8.png){#fig:008 width=70%}

Блок конфигурации клиента `## Client configuration` (рис. [-@fig:009]).

![Vagrantfile — client configuration](/home/mavinogradova/skreens/lbA1/9.png){#fig:009 width=70%}


### HCL-файл настроек виртуальной машины

В подкаталоге `packer` размещаем файл `vagrant-rocky.pkr.hcl` — специальный файл с описанием метаданных по установке дистрибутива на виртуальную машину. В разделе переменных этот файл содержит указание на версию дистрибутива, его хэш-функцию, имя и пароль пользователя по умолчанию (рис. [-@fig:010]).

![vagrant-rocky.pkr.hcl — переменные](/home/mavinogradova/skreens/lbA1/10.png){#fig:010 width=70%}

Проверяем контрольную сумму ISO-образа Rocky Linux 10.2:(рис.[-@fig:011]).

```cmd
certutil -hashfile D:\work\mavinogradova\packer\Rocky-10.2-x86_64-minimal.iso SHA256
```

![certutil — хэш ISO](/home/mavinogradova/skreens/lbA1/11.png){#fig:011 width=70%}

Значение переменной `iso_checksum` в файле `vagrant-rocky.pkr.hcl` совпадает с полученным хэшем (рис. [-@fig:012]).

![hcl — iso_checksum](/home/mavinogradova/skreens/lbA1/12.png){#fig:012 width=70%}


### Сборка box-файла с помощью Packer

Используя FAR, переходим в созданный рабочий каталог с проектом. В этом же каталоге должен быть размещён файл `packer.exe`. В командной строке вводим:

```cmd
packer.exe init vagrant-rocky.pkr.hcl
```

Результат инициализации (рис. [-@fig:013]).

![packer init](/home/mavinogradova/skreens/lbA1/13.png){#fig:013 width=70%}

Для формирования box-файла с дистрибутивом Rocky Linux для VirtualBox в терминале набираем:

```cmd
packer.exe build vagrant-rocky.pkr.hcl
```

Начнётся процесс скачивания, распаковки и установки драйверов VirtualBox и дистрибутива ОС на виртуальную машину (рис. [-@fig:014]).

![packer build — начало](/home/mavinogradova/skreens/lbA1/14.png){#fig:014 width=70%}

Во время автоматического развёртывания дистрибутива на экране виртуальной машины отображается информация об установке (Anaconda, автоматический режим, все пункты отмечены) — (рис. [-@fig:015]).

![Anaconda — установка](/home/mavinogradova/skreens/lbA1/15.png){#fig:015 width=70%}


### Регистрация box-файла в Vagrant

Для регистрации образа виртуальной машины в Vagrant в командной строке вводим:

```cmd
vagrant box add rockylinux10 vagrant-virtualbox-rockylinux10-x86_64.box
```

Результат регистрации box-файла (рис. [-@fig:016]).

![vagrant box add](/home/mavinogradova/skreens/lbA1/16.png){#fig:016 width=70%}


### Запуск виртуальной машины Server

Для запуска виртуальной машины Server вводим в консоли:

```cmd
vagrant up server
```

и убеждаемся, что запуск прошёл успешно (`Machine booted and ready!`)(рис. [-@fig:017]).

![vagrant up server](/home/mavinogradova/skreens/lbA1/17.png){#fig:017 width=70%} 

Виртуальная машина `vagrant_server_...` в менеджере VirtualBox — работает (рис. [-@fig:018]).

![VirtualBox — server](/home/mavinogradova/skreens/lbA1/18.png){#fig:018 width=70%}

### Запуск виртуальной машины Client

Для запуска виртуальной машины Client вводим в консоли:

```cmd
vagrant up client
```

После автоматического развёртывания дистрибутива на экране отображается информация о ходе установки и настройки клиента (рис. [-@fig:019]).

![vagrant up client](/home/mavinogradova/skreens/lbA1/19.png){#fig:019 width=70%}

В менеджере VirtualBox обе виртуальные машины — server и client — находятся в состоянии «Работает» (рис. [-@fig:020]).

![VirtualBox — обе ВМ](/home/mavinogradova/skreens/lbA1/20.png){#fig:020 width=70%}

### Проверка работоспособности стенда

Проверяем состояние виртуальных машин:(рис. [-@fig:021]).

```cmd
vagrant status
```

![vagrant status](/home/mavinogradova/skreens/lbA1/21.png){#fig:021 width=70%}

Подключаемся к серверу из консоли:(рис. [-@fig:022]).

```cmd
vagrant ssh server
```

![vagrant ssh server](/home/mavinogradova/skreens/lbA1/22.png){#fig:022 width=70%}

Вводим пароль `vagrant`. Переходим к пользователю `mavinogradova`:

```bash
su - mavinogradova
```

Проверяем приглашение (рис. [-@fig:023]).

![Приглашение на сервере](/home/mavinogradova/skreens/lbA1/23.png){#fig:023 width=70%}

Отлогиниваемся. Выполняем то же самое для клиента:

```cmd
vagrant ssh client
```

Результат подключения приведён на (рис. [-@fig:024]).

![vagrant ssh client](/home/mavinogradova/skreens/lbA1/24.png){#fig:024 width=70%}

Переходим к пользователю `mavinogradova` (рис. [-@fig:025]).

![Приглашение на клиенте](/home/mavinogradova/skreens/lbA1/25.png){#fig:025 width=70%}

### Выключение виртуальных машин

Выключаем обе виртуальные машины:

```cmd
vagrant halt server
vagrant halt client
```
Проверяем состояние — обе ВМ выключены (`poweroff`) — (рис. [-@fig:026]).

![ВМ выключены](/home/mavinogradova/skreens/lbA1/26.png){#fig:026 width=70%}

### Внесение изменений в настройки внутреннего окружения виртуальной машины

Для отработки созданных скриптов во время загрузки виртуальных машин убеждаемся, что в конфигурационном файле `Vagrantfile` до строк с конфигурацией сервера имеется запись:

```
config.vm.provision "common user", type: "shell", preserve_order: true, path: "provision/default/01-user.sh"
config.vm.provision "common hostname", type: "shell", preserve_order: true, run: "always", path: "provision/default/01-hostname.sh"
```

Фиксируем внесённые изменения для внутренних настроек виртуальных машин:


vagrant up server --provision (рис. [-@fig:027]).

![server](/home/mavinogradova/skreens/lbA1/27.png){#fig:027 width=70%}

и vagrant up client --provision (рис. [-@fig:028]).

![client](/home/mavinogradova/skreens/lbA1/28.png){#fig:028 width=70%}


### Копирование на внешний носитель

После выключения виртуальных машин копируем необходимые для работы с Vagrant файлы и box-файлы виртуальных машин на внешний носитель или в другой каталог ОС. Используя эти файлы, можно развернуть виртуальные машины на другом компьютере.

## Выводы

В ходе лабораторной работы были приобретены практические навыки установки Rocky Linux на виртуальную машину с помощью инструмента Vagrant. Освоены создание и настройка конфигурационного файла `Vagrantfile`, работа с box-файлами, развёртывание стенда из двух виртуальных машин (server и client), а также настройка внутреннего окружения виртуальных машин с помощью скриптов провижининга. В результате работы стенд из двух виртуальных машин успешно развёрнут, обе ВМ работоспособны, пользователь `mavinogradova` создан, приглашения в терминале имеют вид `[mavinogradova@server.mavinogradova.net ~]$` и `[mavinogradova@client.mavinogradova.net ~]$`.

## Контрольные вопросы

**1. Для чего предназначен Vagrant?**

Vagrant — представляет собой инструмент для создания и управления средами виртуальных машин в одном рабочем процессе. Этот инструмент позволяет автоматизировать процесс установки на виртуальную машину как основного дистрибутива операционной системы, так и настройки необходимого в дальнейшем программного обеспечения.

**2. Что такое box-файл? В чём назначение Vagrantfile?**

Box-файл (или Vagrant Box) — сохранённый образ виртуальной машины с развёрнутой в ней операционной системой; используется как основа для клонирования виртуальных машин с теми или иными настройками.

Vagrantfile — конфигурационный файл, написанный на языке Ruby, в котором указаны настройки запуска виртуальной машины.

**3. Приведите описание и примеры вызова основных команд Vagrant.**

- `vagrant help` — вызов справки по командам Vagrant;
- `vagrant box list` — список подключённых к Vagrant box-файлов;
- `vagrant box add` — подключение box-файла к Vagrant;
- `vagrant destroy` — отключение box-файла от Vagrant и удаление его из виртуального окружения;
- `vagrant init` — создание «шаблонного» конфигурационного файла Vagrantfile;
- `vagrant up` — запуск виртуальной машины;
- `vagrant reload` — перезагрузка виртуальной машины;
- `vagrant halt` — остановка и выключение виртуальной машины;
- `vagrant provision` — настройка внутреннего окружения имеющейся виртуальной машины;
- `vagrant ssh` — подключение к виртуальной машине через ssh.

**4. Дайте построчные пояснения содержания файлов vagrant-rocky.pkr.hcl, ks.cfg, Vagrantfile, Makefile.**

**`vagrant-rocky.pkr.hcl`:**
- `packer { required_plugins { ... } }` — подключаемые плагины (vagrant, virtualbox, qemu);
- `variable "artifact_description"` — описание артефакта («Rocky 10.2»);
- `variable "artifact_version"` — версия артефакта («10.2»);
- `variable "disk_size"` — размер диска (61440 МБ);
- `variable "iso_checksum"` — SHA256-хэш ISO-образа;
- `variable "iso_url"` — имя ISO-файла;
- `source "virtualbox-iso" "rockylinux"` — описание сборщика для VirtualBox;
- `build { ... }` — этапы сборки и упаковки в box-файл.

**`ks.cfg`:**
- `bootloader`, `zerombr`, `clearpart`, `reboot`, `text` — настройки загрузчика и режима установки;
- `keyboard`, `lang`, `timezone` — языковые и временные настройки;
- `network --bootproto=dhcp` — настройки сети;
- `rootpw`, `user --name=vagrant` — учётные записи;
- `services --enabled="NetworkManager,sshd,chronyd"` — список включаемых служб;
- `autopart --type=lvm` — автоматическая разметка диска;
- `%post` — пост-установочные команды;
- `%packages` — список устанавливаемых пакетов.

**`Vagrantfile`:**
- `config.vagrant.plugins` — подключаемые плагины;
- `config.vm.provider :virtualbox do |virtualbox|` — параметры провайдера VirtualBox;
- `config.vm.provision` — скрипты провижининга;
- `config.vm.define "server"` / `config.vm.define "client"` — описание двух виртуальных машин;
- `server.vm.network :private_network, ip: "192.168.1.1"` — статический IP сервера;
- `client.vm.network :private_network, type: "dhcp"` — DHCP для клиента.

**`Makefile`:**
- `DEFAULT_GOAL := help` — цель по умолчанию;
- цели `init`, `virtualbox`, `qemu` — сборка box-файла;
- `help:` — вывод списка доступных целей;
- `check-virtualbox`, `check-libvirt` — проверка установки;
- `plugins` — установка плагинов Vagrant;
- `addbox` — добавление box-файла в Vagrant;
- `server-up`, `client-up` — запуск ВМ;
- `server-ssh`, `client-ssh` — подключение по SSH;
- `server-halt`, `client-halt` — выключение ВМ;
- `server-destroy`, `client-destroy`, `destroy` — удаление ВМ;
- `clean` — полная очистка проекта.

## Список литературы

1. GNU Bash Manual. — 2019. — URL: https://www.gnu.org/software/bash/manual/
2. GNU Make Manual. — 2016. — URL: http://www.gnu.org/software/make/manual/
3. Powers S. Vagrant Simplified / Пер.: А. Панин // Библиотека сайта rus-linux.net. — 2015.
4. Vagrant Documentation. — URL: https://www.vagrantup.com/docs
5. Купер М. Искусство программирования на языке сценариев командной оболочки. — 2004.
- Массивы большой емкости, где реконструкция данных после сбоя занимает много времени
