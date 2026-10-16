---
## Front matter
title: "Отчет по лабораторной работе № 7"
subtitle: "Расширенные настройки межсетевого экрана"
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

Получить навыки настройки межсетевого экрана в Linux в части переадресации портов и настройки Masquerading.

# Задание

1. Настройте межсетевой экран виртуальной машины server для доступа к серверу по протоколу SSH не через 22-й порт, а через порт 2022 (см. разделы 7.4.1 и 7.4.2).
2. Настройте Port Forwarding на виртуальной машине server (см. разделы 7.4.3).
3. Настройте маскарадинг на виртуальной машине server для организации доступа клиента к сети Интернет (см. раздел 7.4.3).
4. Напишите скрипт для Vagrant, фиксирующий действия по расширенной настройке межсетевого экрана. Соответствующим образом внести изменения в Vagrantfile (см. раздел 7.4.4).

# Выполнение лабораторной работы

## Создание пользовательской службы firewall

Загружаем операционную систему и переходим в рабочий каталог с проектом. Запускаем виртуальную машину server. На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя (рис. [-@fig:001]).

![Вход на виртуальную машину server под пользователем mavinogradova и переход в режим суперпользователя](/home/mavinogradova/skreens/LbA7/1.png){#fig:001 width=70%}

На основе существующего файла описания службы ssh создаём файл с собственным описанием. Для этого копируем системный файл `/usr/lib/firewalld/services/ssh.xml` в пользовательский каталог `/etc/firewalld/services/` под именем `ssh-custom.xml`, переходим в этот каталог и просматриваем содержимое созданного файла службы (рис. [-@fig:002]).

![Создание файла ssh-custom.xml и просмотр его исходного содержимого](/home/mavinogradova/skreens/LbA7/2.png){#fig:002 width=70%}

Построчно прокомментируем принцип синтаксиса файла описания службы:

Строка объявления XML-документа указывает версию языка разметки и используемую кодировку символов.

Корневой элемент `<service>` открывает описание службы FirewallD.

Элемент `<short>` содержит краткое имя службы, которое отображается в списках.

Элемент `<description>` содержит развёрнутое текстовое описание назначения службы.

Элемент `<port>` описывает порт и протокол, на котором работает служба. В исходном файле указан порт 22 и протокол tcp.

Закрывающий тег `</service>` завершает описание службы.

Открываем файл описания службы на редактирование. Заменяем порт 22 на новый порт 2022. В этом же файле корректируем описание службы для демонстрации, указываем, что это модифицированный файл службы (рис. [-@fig:003]).

![Редактирование файла ssh-custom.xml: изменённый порт 2022 и модифицированное описание](/home/mavinogradova/skreens/LbA7/3.png){#fig:003 width=70%}

Полный текст итогового файла `/etc/firewalld/services/ssh-custom.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>SSH-custom</short>
  <description>Modified SSH service file.</description>
  <port protocol="tcp" port="2022"/>
</service>
```

Просматриваем список доступных FirewallD служб. Обращаем внимание, что новая служба ещё не отображается в списке. Перегружаем правила межсетевого экрана с сохранением информации о состоянии и вновь выводим на экран список служб, а также список активных служб (рис. [-@fig:004]).

![Просмотр списка служб FirewallD до и после перезагрузки правил](/home/mavinogradova/skreens/LbA7/4.png){#fig:004 width=70%}

Добавляем новую службу в FirewallD и выводим на экран список активных служб. Служба `ssh-custom` появляется в списке активных служб. Далее добавляем службу на постоянной основе с параметром `--permanent`, перегружаем правила межсетевого экрана с сохранением информации о состоянии и повторно проверяем список активных служб (рис. [-@fig:005]).

![Добавление службы ssh-custom в FirewallD и проверка списка активных служб](/home/mavinogradova/skreens/LbA7/5.png){#fig:005 width=70%}

## Перенаправление портов

Организуем на сервере переадресацию с порта 2022 на порт 22 и проверяем, что правило добавлено в список правил переадресации (рис. [-@fig:006]).

![Организация переадресации с порта 2022 на порт 22 и проверка правила](/home/mavinogradova/skreens/LbA7/6.png){#fig:006 width=70%}

На клиенте пробуем получить доступ по SSH к серверу через порт 2022 (вместо user указываем свой логин). Подтверждаем fingerprint ключа хоста, вводим пароль. Подключение выполняется успешно, что подтверждает корректность настройки переадресации портов (рис. [-@fig:007]).

![Подключение к серверу по SSH через порт 2022 с клиента](/home/mavinogradova/skreens/LbA7/7.png){#fig:007 width=70%}

## NAT, Masquerading, Port Forwarding

На сервере смотрим, активирована ли в ядре системы возможность перенаправления IPv4-пакетов. В выводе видно, что параметр `net.ipv4.ip_forward` имеет значение 1 (рис. [-@fig:008]).

![Проверка активации перенаправления IPv4-пакетов в ядре](/home/mavinogradova/skreens/LbA7/8.png){#fig:008 width=70%}

Включаем перенаправление IPv4-пакетов на сервере: создаём файл `/etc/sysctl.d/90-forward.conf` с параметром `net.ipv4.ip_forward = 1` и применяем настройки (рис. [-@fig:009]).

![Включение перенаправления IPv4-пакетов на сервере](/home/mavinogradova/skreens/LbA7/9.png){#fig:009 width=70%}

Полный текст файла `/etc/sysctl.d/90-forward.conf`:

```
net.ipv4.ip_forward = 1
```

Включаем маскарадинг на сервере в зоне public на постоянной основе, перегружаем правила и проверяем статус маскарадинга — вывод `yes` (рис. [-@fig:010]).

![Включение маскарадинга на сервере и проверка статуса](/home/mavinogradova/skreens/LbA7/10.png){#fig:010 width=70%}

На клиенте проверяем доступность выхода в Интернет. Пакеты успешно доставляются, что подтверждает корректность настройки маскарадинга (рис. [-@fig:011]).

![Проверка доступности выхода в Интернет на клиенте](/home/mavinogradova/skreens/LbA7/11.png){#fig:011 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`, создаём в нём каталог `firewall`, в который помещаем в соответствующие подкаталоги конфигурационные файлы FirewallD. В подкаталог `etc/firewalld/services` копируем пользовательское описание службы `ssh-custom.xml`, в подкаталог `etc/sysctl.d` — файл `90-forward.conf`. Проверяем структуру каталогов и наличие файлов (рис. [-@fig:012]).

![Создание каталога firewall и копирование конфигурационных файлов в каталог провижининга](/home/mavinogradova/skreens/LbA7/12.png){#fig:012 width=70%}

В каталоге `/vagrant/provision/server` создаём исполняемый файл `firewall.sh` (рис. [-@fig:013]).

![Создание исполняемого файла firewall.sh](/home/mavinogradova/skreens/LbA7/13.png){#fig:013 width=70%}

Открываем файл на редактирование и прописываем в нём скрипт, который копирует конфигурационные файлы из каталога провижининга в систему, настраивает маскарадинг, добавляет пользовательскую службу `ssh-custom`, задаёт правило переадресации порта 2022 на порт 22 и восстанавливает SELinux-контексты. Этот скрипт, по сути, повторяет произведённые действия по расширенной настройке межсетевого экрана (рис. [-@fig:014]).

![Содержимое скрипта провижининга firewall.sh](/home/mavinogradova/skreens/LbA7/14.png){#fig:014 width=70%}

Полный текст файла `/vagrant/provision/server/firewall.sh`:

```bash
#!/bin/bash
echo "Provisioning script $0"
echo "Copy configuration files"
cp -R /vagrant/provision/server/firewall/etc/* /etc
echo "Configure masquerading"
firewall-cmd --add-service=ssh-custom --permanent
firewall-cmd --add-forward-port=port=2022:proto=tcp:toport=22 --permanent
firewall-cmd --zone=public --add-masquerade --permanent
firewall-cmd --reload
restorecon -vR /etc
```

Для отработки созданного скрипта во время загрузки виртуальных машин в конфигурационном файле `Vagrantfile` добавлена в конфигурации сервера запись, объявляющая новый провижининг типа shell с сохранением порядка выполнения и указанием пути к скрипту `provision/server/firewall.sh`. Данный блок добавлен в секцию `server` после блока `server http` и перед блоком `server mysql` (рис. [-@fig:015]).

![Фрагмент Vagrantfile с добавленным блоком провижининга server firewall](/home/mavinogradova/skreens/LbA7/15.png){#fig:015 width=70%}

Фрагмент `Vagrantfile` с добавленным блоком провижининга:

```ruby
server.vm.provision "server firewall",
                    type: "shell",
                    preserve_order: true,
                    path: "provision/server/firewall.sh"
```

# Выводы

В ходе лабораторной работы были получены навыки настройки межсетевого экрана в Linux в части переадресации портов и настройки Masquerading. Создана пользовательская служба FirewallD `ssh-custom` с портом 2022 на основе существующего описания службы ssh, служба успешно добавлена в FirewallD. Организована переадресация с порта 2022 на порт 22, проверено SSH-подключение к серверу через порт 2022 с клиента. Включено перенаправление IPv4-пакетов в ядре и маскарадинг в зоне public, проверен выход клиента в сеть Интернет. Написан скрипт провижининга `firewall.sh`, автоматизирующий расширенную настройку межсетевого экрана, и внесены соответствующие изменения в `Vagrantfile`.

# Ответы на контрольные вопросы

1. Где хранятся пользовательские файлы firewalld? Пользовательские файлы firewalld хранятся в каталоге `/etc/firewalld/`. В частности, пользовательские описания служб размещаются в каталоге `/etc/firewalld/services/`. Этот каталог имеет приоритет над системным `/usr/lib/firewalld/services/`, поэтому файл с тем же именем, размещённый в `/etc/firewalld/services/`, переопределяет системное описание службы.

2. Какую строку надо включить в пользовательский файл службы, чтобы указать порт TCP 2022? В пользовательский файл службы необходимо включить строку `<port protocol="tcp" port="2022"/>`.

3. Какая команда позволяет вам перечислить все службы, доступные в настоящее время на вашем сервере? Команда `firewall-cmd --get-services`. Она выводит список всех служб, известных FirewallD (как системных, так и пользовательских). Дополнительно список активных служб текущей зоны можно получить командой `firewall-cmd --list-services`.

4. В чем разница между трансляцией сетевых адресов (NAT) и маскарадингом (masquerading)? NAT — это механизм преобразования IP-адресов транзитных пакетов, который может быть статическим (SNAT, 1:1), динамическим (DNAT, 1:N) или с перегрузкой (NAT Overload, N:1). Маскарадинг (masquerading) — это частный случай NAT Overload (PAT), при котором адрес отправителя динамически подменяется адресом назначенного интерфейса вместе с номером порта. Маскарадинг обычно применяется для предоставления узлам локальной сети с внутренними адресами доступа во внешнюю сеть через один внешний адрес шлюза.

5. Какая команда разрешает входящий трафик на порт 4404 и перенаправляет его в службу ssh по IP-адресу 10.0.0.10? Команда `firewall-cmd --zone=public --add-forward-port=port=4404:proto=tcp:toport=22:toaddr=10.0.0.10 --permanent`.

6. Какая команда используется для включения маскарадинга IP-пакетов для всех пакетов, выходящих в зону public? Команда `firewall-cmd --zone=public --add-masquerade --permanent`.

# Список литературы

1. Методические указания к лабораторной работе № 7 «Расширенные настройки межсетевого экрана».
2. NAT: вопросы и ответы. — URL: https://www.cisco.com/cisco/web/support/RU/9/92/92029_nat-faq.html (дата обр. 13.09.2021).
3. Динамический брандмауэр с использованием FirewallD. — URL: https://fedoraproject.org/wiki/FirewallD/ru (дата обр. 13.09.2021).
