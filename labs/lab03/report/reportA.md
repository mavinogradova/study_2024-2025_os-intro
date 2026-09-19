---
## Front matter
title: "Отчет по лабораторной работе № 3"
subtitle: "Настройка DHCP-сервера"
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
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Приобретение практических навыков по установке и конфигурированию DHCP-сервера.

# Задание

1. Установите на виртуальной машине server DHCP-сервер.
2. Настройте виртуальную машину server в качестве DHCP-сервера для виртуальной внутренней сети.
3. Получите параметры от DHCP-сервера в виртуальной внутренней сети путём запуска виртуальной машины client и применения соответствующих утилит диагностики.
4. Настройте обновление DNS-зоны при появлении в виртуальной внутренней сети новых узлов.
5. Проверьте корректность работы DHCP-сервера и обновления DNS-зоны в виртуальной внутренней сети путём запуска виртуальной машины client и применения соответствующих утилит диагностики.
6. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке DHCP-сервера во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile.

# Выполнение лабораторной работы

### Подключение к виртуальной машине server

Запускаем виртуальную машину server и подключаемся к ней по SSH (рис. [-@fig:001]).

![Запуск виртуальной машины server](/home/mavinogradova/skreens/lab3/1.png){#fig:001 width=70%}

На виртуальной машине server входим под созданным ранее пользователем `mavinogradova` и переходим в режим суперпользователя (рис. [-@fig:002]).

![Вход под mavinogradova и получение root](/home/mavinogradova/skreens/lab3/2.png){#fig:002 width=70%}

### Установка DHCP-сервера

Устанавливаем DHCP-сервер Kea (рис. [-@fig:003]).

![Установка kea](/home/mavinogradova/skreens/lab3/3.png){#fig:003 width=70%}

### Конфигурирование DHCP-сервера

Сохраняем на всякий случай конфигурационный файл и открываем его на редактирование (рис. [-@fig:004]).

![Резервная копия kea-dhcp4.conf](/home/mavinogradova/skreens/lab3/4.png){#fig:004 width=70%}

Анализируем содержимое файла `/etc/kea/kea-dhcp4.conf` (рис. [-@fig:005]).

![Содержимое kea-dhcp4.conf](/home/mavinogradova/skreens/lab3/5.png){#fig:005 width=70%}

В файле `/etc/kea/kea-dhcp4.conf` заменяем шаблон для `domain-name` и `domain-search` на `mavinogradova.net`, блок `domain-name-servers` — на адрес `192.168.1.1`, а также задаём собственную конфигурацию DHCP-сети: адрес подсети `192.168.1.0/24`, диапазон адресов для распределения клиентам `192.168.1.30 – 192.168.1.199`, адрес маршрутизатора `192.168.1.1`. Остальные примеры задания конфигураций подсетей удаляем. Настраиваем привязку dhcpd к интерфейсу `eth1` виртуальной машины server.

Проверяем правильность конфигурационного файла (рис. [-@fig:006]).

![Проверка конфигурационного файла](/home/mavinogradova/skreens/lab3/6.png){#fig:006 width=70%}

Перезагружаем конфигурацию dhcpd и разрешаем загрузку DHCP-сервера при запуске виртуальной машины server (рис. [-@fig:007]).

![Перезагрузка конфигурации и включение автозапуска](/home/mavinogradova/skreens/lab3/7.png){#fig:007 width=70%}

Добавляем запись для DHCP-сервера в конце файла прямой DNS-зоны `/var/named/master/fz/mavinogradova.net`: dhcp    A    192.168.1.1 и в конце файла обратной зоны `/var/named/master/rz/192.168.1`: 1    PTR    dhcp.mavinogradova.net.

При этом в обоих файлах изменяем серийный номер файла зоны, указывая текущую дату в нотации ГГГГММДДВВ.

Прямая зона после правки (рис. [-@fig:008]).

![Прямая DNS-зона mavinogradova.net](/home/mavinogradova/skreens/lab3/8.png){#fig:008 width=70%}

Обратная зона после правки (рис. [-@fig:009]).

![Обратная DNS-зона 192.168.1](/home/mavinogradova/skreens/lab3/9.png){#fig:009 width=70%}

Перезапускаем named и проверяем, что можно обратиться к DHCP-серверу по имени (рис. [-@fig:010]).


![Перезапуск named и проверка ping dhcp.mavinogradova.net](/home/mavinogradova/skreens/lab3/10.png){#fig:010 width=70%}

Вносим изменения в настройки межсетевого экрана узла server, разрешив работу с DHCP (рис. [-@fig:011]).

![Разрешение DHCP в firewall](/home/mavinogradova/skreens/lab3/11.png){#fig:011 width=70%}

Восстанавливаем контекст безопасности в SELinux (рис. [-@fig:012]).

![Восстановление контекста SELinux](/home/mavinogradova/skreens/lab3/12.png){#fig:012 width=70%}

В дополнительном терминале запускаем мониторинг происходящих в системе процессов в реальном времени (рис. [-@fig:013]).

![Мониторинг системных сообщений](/home/mavinogradova/skreens/lab3/13.png){#fig:013 width=70%}

В основном рабочем терминале запускаем DHCP-сервер (рис. [-@fig:014]).

![Запуск DHCP-сервера](/home/mavinogradova/skreens/lab3/14.png){#fig:014 width=70%}

Убеждаемся, что запуск DHCP-сервера прошёл успешно (рис. [-@fig:015]).

![Запуск kea-dhcp4.service](/home/mavinogradova/skreens/lab3/15.png){#fig:015 width=70%}

### Анализ работы DHCP-сервера

Перед запуском виртуальной машины client в каталоге с проектом в подкаталоге `vagrant/provision/client` создаём файл `01-routing.sh` (рис. [-@fig:016]).

![Создание файла 01-routing.sh](/home/mavinogradova/skreens/lab3/16.png){#fig:016 width=70%}

Открыв его на редактирование, прописываем в нём скрипт, изменяющий настройки NetworkManager так, чтобы весь трафик на виртуальной машине client шёл по умолчанию через интерфейс `eth1` (рис. [-@fig:017]).

![Содержимое 01-routing.sh](/home/mavinogradova/skreens/lab3/17.png){#fig:017 width=70%}

В `Vagrantfile` подключаем этот скрипт в разделе конфигурации для клиента (рис. [-@fig:018]).

![Фрагмент Vagrantfile с client routing](/home/mavinogradova/skreens/lab3/18.png){#fig:018 width=70%}

Зафиксировав внесённые изменения для внутренних настроек виртуальной машины client, запускаем её (рис. [-@fig:019]).

![Запуск клиента с провижинингом](/home/mavinogradova/skreens/lab3/19.png){#fig:019 width=70%}

После загрузки виртуальной машины client на виртуальной машине server в терминале с мониторингом происходящих в системе процессов можно наблюдать записи о подключении к виртуальной внутренней сети узла client и выдачи ему IP-адреса из соответствующего диапазона адресов (рис. [-@fig:020]).

![Логи DHCP-сервера](/home/mavinogradova/skreens/lab3/20.png){#fig:020 width=70%}

Также информацию о работе DHCP-сервера можно наблюдать в файле `/var/lib/kea/kea-leases4.csv` (рис. [-@fig:021]).

![Содержимое kea-leases4.csv](/home/mavinogradova/skreens/lab3/21.png){#fig:021 width=70%}

В таблице [-@tab:leases] приведён построчный комментарий информации из файла `/var/lib/kea/kea-leases4.csv`.

: Построчный комментарий kea-leases4.csv {#tab:leases}

| Поле | Значение | Комментарий |
|------|----------|-------------|
| `address` | `192.168.1.30` | IP-адрес, выданный клиенту из пула `192.168.1.30–199` |
| `hwaddr` | `08:00:27:23:bf:22` | MAC-адрес клиента, совпадает с `ifconfig eth1` на клиенте |
| `client_id` | `01:08:00:27:23:bf:22` | DHCP client-id, сформированный из MAC-адреса (префикс `01`) |
| `valid_lifetime` | `3600` | Срок аренды — 3600 секунд (1 час), совпадает с `valid-lifetime` в конфиге |
| `expire` | `1789837551` | Unix-timestamp истечения аренды |
| `subnet_id` | `1` | ID подсети, соответствует `"id": 1` в конфиге Kea |
| `fqdn_fwd` | `0` → `1` | Флаг прямого DNS-обновления |
| `fqdn_rev` | `0` → `1` | Флаг обратного DNS-обновления |
| `hostname` | `client.mavinogradova.net` | FQDN клиента, переданный им в DHCP-запросе |
| `state` | `0` | Состояние аренды: 0 = active |
| `user_context` | *(пусто)* | Пользовательский контекст не задан |
| `pool_id` | `0` | ID пула |

Войдя в систему виртуальной машины client под пользователем `mavinogradova` и открыв терминал, вводим `ifconfig` (рис. [-@fig:022]).

![Вывод ifconfig на клиенте](/home/mavinogradova/skreens/lab3/22.png){#fig:022 width=70%}

В таблице [-@tab:ifconfig] приведён построчный комментарий информации об имеющихся интерфейсах.

: Построчный комментарий вывода ifconfig {#tab:ifconfig}

| Интерфейс/строка | Что означает |
|------------------|--------------|
| `eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>` | Внешний NAT-интерфейс, поднят, работает |
| `inet 10.0.2.15 netmask 255.255.255.0 broadcast 10.0.2.255` | IPv4-адрес NAT, выданный VirtualBox автоматически |
| `inet6 fd17:625c:f037:2:a00:27ff:fe64:5285` | Глобальный IPv6-адрес (ULA) |
| `inet6 fe80::a00:27ff:fe64:5285` | IPv6 link-local |
| `ether 08:00:27:64:52:85` | MAC-адрес интерфейса eth0 |
| `RX packets 2399 bytes 228308` | Принято 2399 пакетов, 222.9 КБ |
| `RX errors 0 dropped 0 overruns 0 frame 0` | Нет ошибок приёма |
| `TX packets 1646 bytes 222476` | Передано 1646 пакетов, 217.2 КБ |
| `TX errors 0 dropped 0 overruns 0 carrier 0 collisions 0` | Нет ошибок передачи |
| `eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>` | Внутренний интерфейс, поднят |
| `inet 192.168.1.30 netmask 255.255.255.0 broadcast 192.168.1.255` | IP-адрес, выданный DHCP-сервером Kea |
| `ether 08:00:27:23:bf:22` | MAC-адрес клиента, совпадает с kea-leases4.csv |
| `RX packets 57 bytes 8081` | Принято 57 пакетов |
| `TX packets 405 bytes 36158` | Передано 405 пакетов |
| `lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536` | Локальный loopback-интерфейс |
| `inet 127.0.0.1 netmask 255.0.0.0` | Стандартный loopback IPv4 |
| `inet6 ::1 prefixlen 128` | IPv6 loopback |

На машине server ещё раз смотрим список выданных адресов (рис. [-@fig:023]).

![Повторный просмотр kea-leases4.csv](/home/mavinogradova/skreens/lab3/21.png){#fig:023 width=70%}

### Настройка обновления DNS-зоны

Создаём ключ на сервере с Bind9 (на виртуальной машине server) (рис. [-@fig:024]).

![Генерация TSIG-ключа](/home/mavinogradova/skreens/lab3/23.png){#fig:024 width=70%}

Поправляем права доступа (рис. [-@fig:025]).

![Права доступа на ключ](/home/mavinogradova/skreens/lab3/24.png){#fig:025 width=70%}

Подключаем ключ в файле `/etc/named.conf` (рис. [-@fig:026]).

![Подключение ключа в named.conf](/home/mavinogradova/skreens/lab3/25.png){#fig:026 width=70%}

На виртуальной машине server под пользователем с правами суперпользователя редактируем файл `/etc/named/mavinogradova.net`, разрешив обновление зоны (рис. [-@fig:027]).

![Разрешение обновления зон](/home/mavinogradova/skreens/lab3/26.png){#fig:027 width=70%}

Сделаем проверку конфигурационного файла и перезапустим DNS-сервер (рис. [-@fig:028]).

![Проверка named.conf и перезапуск named](/home/mavinogradova/skreens/lab3/27.png){#fig:028 width=70%}

Формируем ключ для Kea. Файл ключа назовём `/etc/kea/tsig-keys.json` (рис. [-@fig:029]).

![Создание tsig-keys.json](/home/mavinogradova/skreens/lab3/28.png){#fig:029 width=70%}

Переносим ключ с сервера Kea DHCP и переписываем его в формате JSON (рис. [-@fig:030]).

![Содержимое tsig-keys.json](/home/mavinogradova/skreens/lab3/29.png){#fig:030 width=70%}

Сменим владельца и поправим права доступа (рис. [-@fig:031]).

![Права на tsig-keys.json](/home/mavinogradova/skreens/lab3/30.png){#fig:031 width=70%}

Настройка происходит в файле `/etc/kea/kea-dhcp-ddns.conf` (рис. [-@fig:032]).

![Настройка kea-dhcp-ddns.conf](/home/mavinogradova/skreens/lab3/31.png){#fig:032 width=70%}

Проверяем файл на наличие возможных синтаксических ошибок, запускаем службу ddns и проверяем статус работы службы (рис. [-@fig:033]).

![Проверка и запуск kea-dhcp-ddns](/home/mavinogradova/skreens/lab3/32.png){#fig:033 width=70%}

Вносим изменения в конфигурационный файл `/etc/kea/kea-dhcp4.conf`, добавив в него разрешение на динамическое обновление DNS-записей с локального узла прямой и обратной зон (рис. [-@fig:034]).

![Правка kea-dhcp4.conf (enable-updates)](/home/mavinogradova/skreens/lab3/33.png){#fig:034 width=70%}

Проверяем файл на наличие возможных синтаксических ошибок, перезапускаем DHCP-сервер и проверяем статус (рис. [-@fig:035]).

![Проверка и перезапуск kea-dhcp4](/home/mavinogradova/skreens/lab3/34.png){#fig:035 width=70%}

На машине client переполучаем адрес (рис. [-@fig:036]).

![Переполучение адреса на клиенте](/home/mavinogradova/skreens/lab3/35.png){#fig:036 width=70%}

В каталоге прямой DNS-зоны `/var/named/master/fz` должен появиться файл `mavinogradova.net.jnl`, в котором в бинарном файле автоматически вносятся изменения записей зоны (рис. [-@fig:037]).

![Появление mavinogradova.net.jnl](/home/mavinogradova/skreens/lab3/36.png){#fig:037 width=70%}

### Анализ работы DHCP-сервера после настройки обновления DNS-зоны

На виртуальной машине client под пользователем `mavinogradova` открываем терминал и с помощью утилиты dig убеждаемся в наличии DNS-записи о клиенте в прямой DNS-зоне (рис. [-@fig:038]).

![Проверка DNS-записи клиента через dig](/home/mavinogradova/skreens/lab3/37.png){#fig:038 width=70%}

В таблице [-@tab:dig] приведён построчный комментарий выведенной информации.

: Построчный комментарий вывода dig {#tab:dig}

| Строка | Значение |
|--------|----------|
| `; <<>> DiG 9.18.33 <<>> @192.168.1.1 client.mavinogradova.net` | Версия dig, запрос к DNS-серверу 192.168.1.1 для имени client.mavinogradova.net |
| `;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 52334` | Операция QUERY, статус NOERROR (успех), id запроса |
| `;; flags: qr aa rd ra` | qr — ответ, aa — авторитетный, rd — рекурсия запрошена, ra — рекурсия доступна |
| `;; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1` | Один вопрос, один ответ |
| `;client.mavinogradova.net.      IN      A` | Запрос A-записи для client.mavinogradova.net |
| `client.mavinogradova.net. 1200  IN  A  192.168.1.30` | A-запись: TTL 1200 секунд, IP 192.168.1.30 |
| `;; Query time: 6 msec` | Время ответа — 6 мс |
| `;; SERVER: 192.168.1.1#53(192.168.1.1) (UDP)` | DNS-сервер 192.168.1.1, порт 53, UDP |
| `;; MSG SIZE  rcvd: 97` | Размер полученного ответа — 97 байт |

### Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`, создаём в нём каталог `dhcp`, в который помещаем в соответствующие подкаталоги конфигурационные файлы DHCP (рис. [-@fig:039]).

![Копирование конфигурационных файлов DHCP](/home/mavinogradova/skreens/lab3/38.png){#fig:039 width=70%}

Заменяем конфигурационные файлы DNS-сервера (рис. [-@fig:040]).

![Копирование конфигурационных файлов DNS](/home/mavinogradova/skreens/lab3/39.png){#fig:040 width=70%}

В каталоге `/vagrant/provision/server` создаём исполняемый файл `dhcp.sh` (рис. [-@fig:041]).

![Создание dhcp.sh](/home/mavinogradova/skreens/lab3/40.png){#fig:041 width=70%}

Открыв его на редактирование, прописываем в нём скрипт, повторяющий произведённые действия по установке и настройке DHCP-сервера (рис. [-@fig:042]).

![Содержимое dhcp.sh](/home/mavinogradova/skreens/lab3/41.png){#fig:042 width=70%}

Для отработки созданного скрипта во время загрузки виртуальной машины server в конфигурационном файле `Vagrantfile` необходимо добавить в разделе конфигурации для сервера:
Фрагмент `Vagrantfile` с добавленной строкой (рис. [-@fig:043]).

![Фрагмент Vagrantfile с server dhcp](/home/mavinogradova/skreens/lab3/42.png){#fig:043 width=70%}

После этого виртуальные машины client и server можно выключить (рис. [-@fig:044]).

![Выключение виртуальных машин](/home/mavinogradova/skreens/lab3/43.png){#fig:044 width=70%}

## Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и конфигурированию DHCP-сервера. На виртуальной машине server установлен DHCP-сервер Kea, настроена выдача адресов из пула `192.168.1.30–199` для виртуальной внутренней сети, а также выполнена привязка службы к интерфейсу `eth1`. Виртуальная машина client получила IP-адрес `192.168.1.30` по DHCP, что подтверждено утилитами `ifconfig` и файлом аренд `/var/lib/kea/kea-leases4.csv`. Настроено обновление DNS-зон при появлении новых узлов: сгенерирован TSIG-ключ `DHCP_UPDATE`, разрешено обновление зон в конфигурации `named`, сформирован файл `tsig-keys.json`, сконфигурирован сервис `kea-dhcp-ddns` и включён параметр `enable-updates` в `kea-dhcp4.conf`. Корректность работы проверена утилитой `dig` — A-запись `client.mavinogradova.net` автоматически создана в прямой зоне. Написан скрипт `dhcp.sh` для Vagrant, фиксирующий действия по установке и настройке DHCP-сервера, и внесены соответствующие изменения в `Vagrantfile`.

## Контрольные вопросы

**1. В каких файлах хранятся настройки сетевых подключений?**

В RHEL-подобных системах — в `/etc/sysconfig/network-scripts/ifcfg-<интерфейс>` или (для NetworkManager) в `/etc/NetworkManager/system-connections/<соединение>.nmconnection`. Просмотр и изменение выполняется утилитой `nmcli`.

**2. За что отвечает протокол DHCP?**

DHCP (Dynamic Host Configuration Protocol) отвечает за автоматическую выдачу клиентам IP-адресов и других параметров, необходимых для работы в сети TCP/IP: маски подсети, адреса шлюза по умолчанию, адресов DNS-серверов, имени домена и т. д.

**3. Опишите принцип работы протокола DHCP. Какими сообщениями обмениваются клиент и сервер?**

Работа DHCP описывается схемой DORA:

- **Discover** — клиент рассылает широковещательный запрос в поисках DHCP-сервера;
- **Offer** — сервер предлагает клиенту свободный IP-адрес;
- **Request** — клиент запрашивает предложенный адрес;
- **Ack** — сервер подтверждает выделение адреса и фиксирует аренду.

**4. В каких файлах обычно находятся настройки DHCP-сервера? За что отвечает каждый из файлов?**

Для Kea DHCP:

- `/etc/kea/kea-dhcp4.conf` — основная конфигурация DHCPv4-сервера (интерфейсы, пулы, опции, DDNS);
- `/etc/kea/kea-dhcp-ddns.conf` — конфигурация сервиса DHCP-DDNS (D2);
- `/etc/kea/tsig-keys.json` — TSIG-ключи для аутентификации DDNS-обновлений;
- `/var/lib/kea/kea-leases4.csv` — база выданных аренд.

**5. Что такое DDNS? Для чего применяется?**

DDNS (Dynamic DNS) — механизм автоматического обновления DNS-записей при изменении IP-адреса узла, выданного DHCP-сервером. Применяется для того, чтобы при выдаче нового адреса клиенту соответствующая A- и PTR-запись автоматически появлялась в DNS-зонах без ручного вмешательства администратора.

**6. Какую информацию можно получить, используя утилиту ifconfig? Приведите примеры с использованием разных опций.**

Утилита `ifconfig` показывает состояние сетевых интерфейсов: имя, флаги, MTU, IPv4- и IPv6-адреса, маску, broadcast, MAC-адрес, статистику RX/TX. Примеры:

- `ifconfig` — вывести

**7. Какую информацию можно получить, используя утилиту ping? Приведите примеры с использованием разных опций.**

Утилита `ping` применяется для проверки соединений в сетях на основе TCP/IP: она отправляет ICMP Echo-Request указанному узлу и фиксирует поступление ICMP Echo-Reply.

`ping` позволяет определить доступность узла, время задержки (RTT), маршрут, потери пакетов и стабильность соединения.

Примеры использования:

```bash
ping 192.168.1.1                # бесконечная проверка связи с узлом
ping -c 4 ya.ru                 # отправить 4 пакета и завершить
ping -i 0.5 -c 10 192.168.1.1   # интервал между пакетами 0.5 с, всего 10 пакетов
ping -s 1000 -c 4 192.168.1.1   # размер данных в пакете 1000 байт
ping -W 2 -c 4 192.168.1.1      # таймаут ожидания ответа 2 секунды
ping -q -c 4 192.168.1.1        # тихий режим: только итоговая статистика
```

Пример вывода `ping -c 4 dhcp.mavinogradova.net`:

```
PING dhcp.mavinogradova.net (192.168.1.1) 56(84) bytes of data.
64 bytes from ns.mavinogradova.net (192.168.1.1): icmp_seq=1 ttl=64 time=3.99 ms
...
--- dhcp.mavinogradova.net ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3047ms
rtt min/avg/max/mdev = 0.099/1.189/3.991/1.623 ms
```

Значения: `icmp_seq` — номер пакета, `ttl` — Time To Live, `time` — RTT; `4 packets transmitted, 4 received, 0% packet loss` — потери отсутствуют; `rtt min/avg/max/mdev` — минимальное, среднее, максимальное время отклика и его отклонение.
