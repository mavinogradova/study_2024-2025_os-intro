---
## Front matter
title: "Отчет по лабораторной работе № 8"
subtitle: "Настройка SMTP-сервера"
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

Приобретение практических навыков по установке и конфигурированию SMTP-сервера.

# Задание

1. Установите на виртуальной машине server SMTP-сервер postfix (см. раздел 8.4.1).
2. Сделайте первоначальную настройку postfix при помощи утилиты postconf, задав отправку писем не на локальный хост, а на сервер в домене (см. раздел 8.4.2).
3. Проверьте отправку почты с сервера и клиента (см. раздел 8.4.3).
4. Сконфигурируйте Postfix для работы в домене. Проверьте отправку почты с сервера и клиента (см. раздел 8.4.4).
5. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке Postfix во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 8.4.5).

# Выполнение лабораторной работы

## Установка Postfix

На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя (рис. [-@fig:001]).

![Вход на виртуальную машину server под пользователем mavinogradova и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA8/1.png){#fig:001 width=70%}

Устанавливаем необходимые для работы пакеты: postfix и s-nail (рис. [-@fig:002]).

![Установка пакетов postfix и s-nail на сервере](/home/mavinogradova/skreens/lbA8/2.png){#fig:002 width=70%}

Сконфигурируем межсетевой экран, разрешив работать службе протокола SMTP, восстановим контекст безопасности в SELinux и запустим Postfix (рис. [-@fig:003]).

![Настройка межсетевого экрана, восстановление контекста SELinux и запуск Postfix](/home/mavinogradova/skreens/lbA8/3.png){#fig:003 width=70%}

## Изменение параметров Postfix с помощью postconf

Для просмотра списка текущих настроек Postfix вводим команду `postconf` (рис. [-@fig:004]).

![Просмотр полного списка текущих параметров Postfix](/home/mavinogradova/skreens/lbA8/4.png){#fig:004 width=70%}

Смотрим текущее значение параметра `myorigin`. Затем смотрим текущее значение параметра `mydomain`. Заменяем значение параметра `myorigin` на значение параметра `mydomain`, повторяем команду и убеждаемся, что замена параметра была произведена. Проверяем корректность содержания конфигурационного файла main.cf и перезагружаем конфигурационные файлы Postfix (рис. [-@fig:005]).

![Просмотр и изменение параметров myorigin и mydomain, проверка конфигурации](/home/mavinogradova/skreens/lbA8/5.png){#fig:005 width=70%}

Просматриваем все параметры со значением, отличным от значения по умолчанию (рис. [-@fig:006]).

![Просмотр параметров Postfix, отличных от значений по умолчанию](/home/mavinogradova/skreens/lbA8/6.png){#fig:006 width=70%}

Задаем жестко значение домена. Отключаем IPv6 в списке разрешенных в работе Postfix протоколов и оставляем только IPv4. Перезагружаем конфигурацию Postfix (рис. [-@fig:007]).

![Жесткое задание домена, отключение IPv6 и перезагрузка конфигурации](/home/mavinogradova/skreens/lbA8/7.png){#fig:007 width=70%}

Полный текст измененных параметров в конфигурационном файле `/etc/postfix/main.cf`:

```text
myhostname = server.mavinogradova.net
mydomain = mavinogradova.net
myorigin = $mydomain
inet_interfaces = all
inet_protocols = ipv4
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
mynetworks = 127.0.0.0/8, 192.168.0.0/16
```

## Проверка работы Postfix

На сервере под учетной записью пользователя отправляем себе письмо, используя утилиту mail (рис. [-@fig:008]).

![Отправка тестового письма с сервера](/home/mavinogradova/skreens/lbA8/8.png){#fig:008 width=70%}

На втором терминале запускаем мониторинг работы почтовой службы и смотрим, что произошло с сообщением. В логе видно, что сообщение успешно доставлено в локальный почтовый ящик (рис. [-@fig:009]).

![Мониторинг лога maillog на сервере, подтверждение доставки письма](/home/mavinogradova/skreens/lbA8/9.png){#fig:009 width=70%}

На виртуальной машине client входим под своим пользователем, открываем терминал и переходим в режим суперпользователя (рис. [-@fig:010]).

![Вход на виртуальную машину client и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA8/10.png){#fig:010 width=70%}

На клиенте устанавливаем необходимые для работы пакеты postfix и s-nail (рис. [-@fig:011]).

![Установка пакетов postfix и s-nail на клиенте](/home/mavinogradova/skreens/lbA8/11.png){#fig:011 width=70%}

Отключаем IPv6 в списке разрешенных в работе Postfix протоколов и оставляем только IPv4. Запускаем Postfix на клиенте (рис. [-@fig:012]).

![Настройка IPv4 и запуск Postfix на клиенте](/home/mavinogradova/skreens/lbA8/12.png){#fig:012 width=70%}

На клиенте под учетной записью пользователя аналогичным образом отправляем себе второе письмо, используя утилиту mail (рис. [-@fig:013]).

![Отправка письма с клиента](/home/mavinogradova/skreens/lbA8/13.png){#fig:013 width=70%}

Сравниваем результат мониторинга почтовой службы на сервере при отправке сообщения с сервера и с клиента. В отчете отражаем, что сообщение ожидало в очереди, так как сервер прослушивал соединения только с локального узла (рис. [-@fig:014]).

![Проверка очереди на клиенте: письмо ожидает отправки](/home/mavinogradova/skreens/lbA8/14.png){#fig:014 width=70%}

На сервере в конфигурации Postfix разрешаем прослушивать соединения не только с локального узла, но и с других интерфейсов сети. Добавляем адрес внутренней сети, разрешив пересылку сообщений между узлами сети. Перезагружаем конфигурацию Postfix и перезапускаем Postfix (рис. [-@fig:015]).

![Настройка сетевых интерфейсов и доверенных сетей на сервере](/home/mavinogradova/skreens/lbA8/15.png){#fig:015 width=70%}

Повторяем отправку сообщения с клиента (рис. [-@fig:016]).

![Повторная отправка письма с клиента после настройки сети](/home/mavinogradova/skreens/lbA8/16.png){#fig:016 width=70%}

Сравниваем логи сервера и клиента. Письмо успешно доставлено на сервер (рис. [-@fig:017]).

![Логи сервера и клиента, подтверждающие успешную доставку письма](/home/mavinogradova/skreens/lbA8/17.png){#fig:017 width=70%}

## Конфигурация Postfix для домена

С клиента отправляем письмо на свой доменный адрес (рис. [-@fig:018]).

![Отправка письма на доменный адрес с клиента](/home/mavinogradova/skreens/lbA8/18.png){#fig:018 width=70%}

Запускаем мониторинг работы почтовой службы и смотрим, что произошло с сообщением. В отчете отражаем, что сообщение ожидало в очереди на отправление (рис. [-@fig:019]).

![Логи сервера и клиента при отправке письма на доменный адрес](/home/mavinogradova/skreens/lbA8/19.png){#fig:019 width=70%}

Дополнительно смотрим, какие сообщения ожидают в очереди на отправление (рис. [-@fig:020]).

![Проверка очереди на сервере и клиенте](/home/mavinogradova/skreens/lbA8/20.png){#fig:020 width=70%}

Для настройки возможности отправки сообщений не на конкретный узел сети, а на доменный адрес прописываем MX-запись с указанием имени почтового сервера mail.mavinogradova.net в файле прямой DNS-зоны (рис. [-@fig:021]).

![Редактирование файла прямой DNS-зоны с добавлением MX-записи](/home/mavinogradova/skreens/lbA8/21.png){#fig:021 width=70%}

Полный текст файла прямой DNS-зоны `/var/named/mavinogradova.net`:

```text
$TTL 1D
@ IN SOA @ server.mavinogradova.net. (
 2020110500 ; serial
 1D ; refresh
 1H ; retry
 1W ; expire
 3H ) ; minimum
 NS @
 A 192.168.1.1
 MX 10 mail.mavinogradova.net.
$ORIGIN mavinogradova.net.
server A 192.168.1.1
ns A 192.168.1.1
dhcp A 192.168.1.1
www A 192.168.1.1
mail A 192.168.1.1
```

И в файле обратной DNS-зоны (рис. [-@fig:022]).

![Редактирование файла обратной DNS-зоны](/home/mavinogradova/skreens/lbA8/22.png){#fig:022 width=70%}

Полный текст файла обратной DNS-зоны `/var/named/192.168.1.in-addr.arpa`:

```text
$TTL 1D
@ IN SOA @ server.mavinogradova.net. (
 2020110500 ; serial
 1D ; refresh
 1H ; retry
 1W ; expire
 3H ) ; minimum
 NS @
 A 192.168.1.1
 PTR server.mavinogradova.net.
 MX 10 mail.mavinogradova.net.
$ORIGIN 1.168.192.in-addr.arpa.
1 PTR server.mavinogradova.net.
1 PTR ns.mavinogradova.net.
1 PTR dhcp.mavinogradova.net.
1 PTR www.mavinogradova.net.
1 PTR mail.mavinogradova.net.
```

В конфигурации Postfix добавляем домен в список элементов сети, для которых данный сервер является конечной точкой доставки почты. Перезагружаем конфигурацию Postfix, восстанавливаем контекст безопасности в SELinux и перезапускаем DNS (рис. [-@fig:023]).

![Настройка mydestination, перезагрузка Postfix и DNS](/home/mavinogradova/skreens/lbA8/23.png){#fig:023 width=70%}

Пробуем отправить сообщения, находящиеся в очереди на отправление (рис. [-@fig:024]).

![Проталкивание очереди и проверка её состояния на клиенте](/home/mavinogradova/skreens/lbA8/24.png){#fig:024 width=70%}

Проверяем лог сервера. Письмо успешно доставлено на доменный адрес (рис. [-@fig:025]).

![Лог сервера, подтверждающий успешную доставку письма на доменный адрес](/home/mavinogradova/skreens/lbA8/25.png){#fig:025 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения и создаем исполняемый файл mail.sh (рис. [-@fig:026]).

![Создание исполняемого файла mail.sh в каталоге провижининга сервера](/home/mavinogradova/skreens/lbA8/26.png){#fig:026 width=70%}

Открыв его на редактирование, прописываем в нем скрипт для установки и настройки Postfix (рис. [-@fig:027]).

![Содержимое скрипта provision/server/mail.sh](/home/mavinogradova/skreens/lbA8/27.png){#fig:027 width=70%}

Полный текст файла `/vagrant/provision/server/mail.sh`:

```bash
#!/bin/bash
echo "Provisioning script $0"

echo "Install needed packages"
dnf -y install postfix
dnf -y install s-nail

echo "Copy configuration files"
#cp -R /vagrant/provision/server/mail/etc/* /etc

echo "Configure firewall"
firewall-cmd --add-service=smtp --permanent
firewall-cmd --reload
restorecon -vR /etc

echo "Start postfix service"
systemctl enable postfix
systemctl start postfix

echo "Configure postfix"
postconf -e 'mydomain = mavinogradova.net'
postconf -e 'myorigin = $mydomain'
postconf -e 'inet_protocols = ipv4'
postconf -e 'inet_interfaces = all'
postconf -e 'mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain'
postconf -e 'mynetworks = 127.0.0.0/8, 192.168.0.0/16'
postfix set-permissions
restorecon -vR /etc

systemctl stop postfix
systemctl start postfix
```

На виртуальной машине client переходим в каталог для внесения изменений в настройки внутреннего окружения и создаем исполняемый файл mail.sh (рис. [-@fig:028]).

![Создание исполняемого файла mail.sh в каталоге провижининга клиента](/home/mavinogradova/skreens/lbA8/28.png){#fig:028 width=70%}

Открыв его на редактирование, прописываем в нем скрипт для настройки клиента (рис. [-@fig:029]).

![Содержимое скрипта provision/client/mail.sh](/home/mavinogradova/skreens/lbA8/29.png){#fig:029 width=70%}

Полный текст файла `/vagrant/provision/client/mail.sh`:

```bash
#!/bin/bash
echo "Provisioning script $0"

echo "Install needed packages"
dnf -y install postfix
dnf -y install s-nail

echo "Configure postfix"
postconf -e 'inet_protocols = ipv4'

echo "Start postfix service"
systemctl enable postfix
systemctl start postfix
```

Для отработки созданного скрипта во время загрузки виртуальной машины client в конфигурационном файле Vagrantfile необходимо добавить в разделе конфигурации для клиента запись, объявляющую новый провижининг (рис. [-@fig:030]).

![Фрагмент Vagrantfile с добавленным блоком провижининга client mail](/home/mavinogradova/skreens/lbA8/30.png){#fig:030 width=70%}

Фрагмент `Vagrantfile` для клиента:

```ruby
client.vm.provision "client mail",
                    type: "shell",
                    preserve_order: true,
                    path: "provision/client/mail.sh"
```

Аналогично для отработки созданного скрипта во время загрузки виртуальной машины server в конфигурационном файле Vagrantfile необходимо добавить в разделе конфигурации для сервера (рис. [-@fig:031]).

![Фрагмент Vagrantfile с добавленным блоком провижининга server mail](/home/mavinogradova/skreens/lbA8/31.png){#fig:031 width=70%}

Фрагмент `Vagrantfile` для сервера:

```ruby
server.vm.provision "server mail",
                    type: "shell",
                    preserve_order: true,
                    path: "provision/server/mail.sh"
```

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и конфигурированию SMTP-сервера. Был установлен и настроен Postfix на сервере и клиенте, изучены основные параметры конфигурации (`mydomain`, `myorigin`, `mydestination`, `inet_interfaces`, `mynetworks`). Проверена работа почтовой службы: отправка писем с сервера и клиента, анализ логов доставки и очереди сообщений. Настроены DNS-зоны с MX-записями для корректной работы доменной почты. Автоматизированы действия по установке и настройке Postfix с помощью скриптов провижининга для Vagrant.

# Ответы на контрольные вопросы

1. В каком каталоге и в каком файле следует смотреть конфигурацию Postfix? Конфигурация Postfix хранится в каталоге `/etc/postfix`, основной файл конфигурации — `main.cf`.

2. Каким образом можно проверить корректность синтаксиса в конфигурационном файле Postfix? Для проверки синтаксиса используется команда `postfix check`.

3. В каких параметрах конфигурации Postfix требуется внести изменения в значениях для настройки возможности отправки писем не на локальный хост, а на доменные адреса? Основными параметрами являются `mydomain` (имя домена), `myorigin` (домен, с которого отправляется локальная почта), `mydestination` (домены, для которых сервер является конечной точкой доставки), `inet_interfaces` (сетевые интерфейсы для приема почты) и `mynetworks` (доверенные сети). Также требуется наличие MX-записи в DNS.

4. Приведите примеры работы с утилитой mail по отправке письма, просмотру имеющихся писем, удалению письма. Отправка: `echo . | mail -s "Тема" user@example.com`. Просмотр: команда `mail` (переход к следующему письму — `n`, заголовок письма — `h n`). Удаление: `d` (удалить письмо), `dq` (удалить и выйти).

5. Приведите примеры работы с утилитой postqueue. Как посмотреть очередь сообщений? Как определить число сообщений в очереди? Как отправить все сообщения, находящиеся в очереди? Как удалить письмо из очереди? Просмотр очереди: `postqueue -p`. Число сообщений определяется в последней строке вывода (`-- N Kbytes in M Requests`). Отправка всех сообщений: `postqueue -f`. Удаление письма из очереди: `postsuper -d <Queue_ID>`.

# Список литературы

1. Методические указания к лабораторной работе № 8 «Настройка SMTP-сервера».
2. Postfix Documentation. — URL: http://www.postfix.org/documentation.html (visited on 09/13/2021).
