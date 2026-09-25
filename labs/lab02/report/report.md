---
## Front matter
title: "Отчет по лабораторной работе № 2"
subtitle: "Настройка DNS-сервера"
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

Приобретение практических навыков по установке и конфигурированию DNS-сервера, усвоение принципов работы системы доменных имён.

# Задание

1. Установите на виртуальной машине server DNS-сервер bind и bind-utils.
2. Сконфигурируйте на виртуальной машине server кэширующий DNS-сервер.
3. Сконфигурируйте на виртуальной машине server первичный DNS-сервер.
4. При помощи утилит dig и host проанализируйте работу DNS-сервера.
5. Напишите скрипт для Vagrant, фиксирующий действия по установке и конфигурированию DNS-сервера во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile.

# Выполнение лабораторной работы

### Подключение к виртуальной машине server

Запускаем виртуальную машину server и подключаемся к ней по SSH:
Вводим пароль `vagrant`. Подключаемся к терминалу Linux (рис. [-@fig:001]).

![Подключение к server](/home/mavinogradova/skreens/lbA2/1.png){#fig:001 width=70%}

### Установка DNS-сервера

На виртуальной машине server входим под созданным ранее пользователем `mavinogradova` и переходим в режим суперпользователя:

Результат входа под пользователем и получения прав root (рис. [-@fig:002]).

![Вход под mavinogradova и получение root](/home/mavinogradova/skreens/lbA2/2.png ){#fig:002 width=70%}

Устанавливаем bind и bind-utils (рис. [-@fig:003]).

![Установка bind и bind-utils](/home/mavinogradova/skreens/lbA2/3.png ){#fig:003 width=70%}

В качестве упражнения при помощи утилиты dig делаем запрос к DNS-адресу `www.yandex.ru` (рис. [-@fig:004]).

![Запрос dig www.yandex.ru](/home/mavinogradova/skreens/lbA2/4.png ){#fig:004 width=70%}

### Конфигурирование кэширующего DNS-сервера

Анализируем содержимое файлов `/etc/resolv.conf` и `/etc/named.conf` (рис. [-@fig:005]).

![Содержимое resolv.conf и named.conf](/home/mavinogradova/skreens/lbA2/5.png){#fig:005 width=70%}

Анализируем содержимое файла `/var/named/named.ca` (рис. [-@fig:006]).

![Содержимое named.ca](/home/mavinogradova/skreens/lbA2/6.png){#fig:006 width=70%}

Анализируем содержимое файлов `/var/named/named.localhost` и `/var/named/named.loopback` (рис. [-@fig:007]).

![Содержимое named.localhost и named.loopback](/home/mavinogradova/skreens/lbA2/7.png){#fig:007 width=70%}

Запускаем DNS-сервер и включаем его в автозапуск при загрузке системы (рис. [-@fig:008]).

![Запуск и включение автозапуска named](/home/mavinogradova/skreens/lbA2/8.png){#fig:008 width=70%}

Повторно выполняем запрос `dig www.yandex.ru` (рис. [-@fig:009]).

![Повторный запрос dig www.yandex.ru](/home/mavinogradova/skreens/lbA2/9.png){#fig:009 width=70%}

Выполняем запрос напрямую к локальному DNS-серверу — `dig @127.0.0.1 www.yandex.ru` (рис. [-@fig:010]).

![Запрос dig @127.0.0.1 www.yandex.ru](/home/mavinogradova/skreens/lbA2/10.png){#fig:010 width=70%}

Делаем DNS-сервер сервером по умолчанию для хоста server и внутренней виртуальной сети. Для этого изменяем настройки сетевого соединения `eth0` в NetworkManager, указывая в качестве DNS-сервера адрес `127.0.0.1` (рис. [-@fig:011]).

![Редактирование соединения eth0](/home/mavinogradova/skreens/lbA2/11.png){#fig:011 width=70%}

Пытаемся выполнить аналогичные действия для соединения `System eth0` — в системе Rocky Linux 10 такое соединение отсутствует (рис. [-@fig:012]).

![Соединение System eth0 отсутствует](/home/mavinogradova/skreens/lbA2/12.png){#fig:012 width=70%}

Перезапускаем NetworkManager и проверяем наличие изменений в файле `/etc/resolv.conf` (рис. [-@fig:013]).

![Перезапуск NetworkManager и проверка resolv.conf](/home/mavinogradova/skreens/lbA2/13.png){#fig:013 width=70%}

Настраиваем направление DNS-запросов от всех узлов внутренней сети, включая запросы от узла server, через узел server. Для этого вносим изменения в файл `/etc/named.conf`: заменяем строку `listen-on port 53 { 127.0.0.1; };` на `listen-on port 53 { 127.0.0.1; any; };`, а строку `allow-query { localhost; };` — на `allow-query { localhost; 192.168.0.0/16; };` (рис. [-@fig:014]).

![Изменение параметров listen-on и allow-query в named.conf](/home/mavinogradova/skreens/lbA2/14.png){#fig:014 width=70%}

Вносим изменения в настройки межсетевого экрана узла server, разрешив работу с DNS (рис. [-@fig:015]).

![Разрешение DNS в firewall](/home/mavinogradova/skreens/lbA2/15.png){#fig:015 width=70%}

Убеждаемся, что DNS-запросы идут через узел server, который прослушивает порт 53, при помощи команды lsof (рис. [-@fig:016]).

![Проверка прослушивания порта 53 через lsof](/home/mavinogradova/skreens/lbA2/16.png){#fig:016 width=70%}

### Конфигурирование первичного DNS-сервера

Копируем шаблон описания DNS-зон `named.rfc1912.zones` из каталога `/etc` в каталог `/etc/named` и переименовываем его в `mavinogradova.net` (рис. [-@fig:017]).

![Копирование шаблона зон](/home/mavinogradova/skreens/lbA2/17.png){#fig:017 width=70%}

Подключаем файл описания зон `/etc/named/mavinogradova.net` в конфигурационном файле DNS `/etc/named.conf`, добавляя в его конец строку:

Результат подключения (рис. [-@fig:018]).

![Подключение файла зон в named.conf](/home/mavinogradova/skreens/lbA2/18.png){#fig:018 width=70%}

Открываем файл `/etc/named/mavinogradova.net` на редактирование и вместо зон `localhost.localdomain` и `1.0.0.127.in-addr.arpa` прописываем свою прямую и обратную зоны. Остальные записи удаляем (рис. [-@fig:019]).

![Файл /etc/named/mavinogradova.net после правки](/home/mavinogradova/skreens/lbA2/19.png){#fig:019 width=70%}

В каталоге `/var/named` создаём подкаталоги `master/fz` и `master/rz`, в которых будут располагаться файлы прямой и обратной зоны соответственно (рис. [-@fig:020]).

![Создание каталогов master/fz и master/rz](/home/mavinogradova/skreens/lbA2/20.png){#fig:020 width=70%}

Копируем шаблон прямой DNS-зоны `named.localhost` из каталога `/var/named` в каталог `/var/named/master/fz` и переименовываем его в `mavinogradova.net` (рис. [-@fig:021]).

![Копирование шаблона прямой зоны](/home/mavinogradova/skreens/lbA2/21.png){#fig:021 width=70%}

Изменяем файл `/var/named/master/fz/mavinogradova.net`, указывая необходимые DNS-записи для прямой зоны (рис. [-@fig:022]).

![Правка файла прямой зоны](/home/mavinogradova/skreens/lbA2/22.png){#fig:022 width=70%}

Копируем шаблон обратной DNS-зоны `named.loopback` из каталога `/var/named` в каталог `/var/named/master/rz` и переименовываем его в `192.168.1` (рис. [-@fig:023]).

![Копирование шаблона обратной зоны](/home/mavinogradova/skreens/lbA2/23.png){#fig:023 width=70%}

Изменяем файл `/var/named/master/rz/192.168.1`, указывая необходимые DNS-записи для обратной зоны (рис. [-@fig:024]).

![Правка файла обратной зоны](/home/mavinogradova/skreens/lbA2/24.png){#fig:024 width=70%}

Исправляем права доступа к файлам в каталогах `/etc/named` и `/var/named`, чтобы демон named мог с ними работать (рис. [-@fig:025]).

![Исправление прав доступа](/home/mavinogradova/skreens/lbA2/25.png){#fig:025 width=70%}

После изменения доступа к конфигурационным файлам named корректно восстанавливаем их метки в SELinux, проверяем состояние переключателей SELinux, относящихся к named, и при необходимости даём named разрешение на запись в файлы DNS-зоны (рис. [-@fig:026]).

![Восстановление меток SELinux и настройка переключателей](/home/mavinogradova/skreens/lbA2/26.png){#fig:026 width=70%}

В дополнительном терминале запускаем в режиме реального времени расширенный лог системных сообщений, а в первом терминале перезапускаем DNS-сервер (рис. [-@fig:027]).

```bash
# во втором терминале
journalctl -x -f -u named
```

```bash
# в первом терминале
systemctl restart named
systemctl status named
```

![Перезапуск named и логи во втором терминале](/home/mavinogradova/skreens/lbA2/27.png){#fig:027 width=70%}

### Анализ работы DNS-сервера

При помощи утилиты dig получаем описание DNS-зоны с сервера `ns.mavinogradova.net` (рис. [-@fig:028]).

![Получение описания DNS-зоны](/home/mavinogradova/skreens/lbA2/28.png){#fig:028 width=70%}

При помощи утилиты host анализируем корректность работы DNS-сервера (рис. [-@fig:029]).

![Анализ работы DNS-сервера через host](/home/mavinogradova/skreens/lbA2/29.png){#fig:029 width=70%}

### Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`, создаём в нём каталог `dns`, в который помещаем в соответствующие каталоги конфигурационные файлы DNS (рис. [-@fig:030]).

![Копирование конфигурационных файлов DNS в каталог provision](/home/mavinogradova/skreens/lbA2/30.png){#fig:030 width=70%}

В каталоге `/vagrant/provision/server` создаём исполняемый файл `dns.sh` и прописываем в нём скрипт, повторяющий произведённые действия по установке и настройке DNS-сервера (рис. [-@fig:031]).

![Содержимое скрипта dns.sh](/home/mavinogradova/skreens/lbA2/31.png){#fig:031 width=70%}

Для отработки созданного скрипта во время загрузки виртуальной машины server в конфигурационном файле `Vagrantfile` добавляем в разделе конфигурации для сервера строку:

Фрагмент `Vagrantfile` с добавленной строкой (рис. [-@fig:032]).

![Фрагмент Vagrantfile с добавленной строкой](/home/mavinogradova/skreens/lbA2/32.png){#fig:032 width=70%}

## Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и конфигурированию DNS-сервера, а также усвоены принципы работы системы доменных имён. На виртуальной машине server установлены пакеты `bind` и `bind-utils`, сконфигурирован кэширующий DNS-сервер, а также первичный DNS-сервер с прямой зоной `mavinogradova.net` и обратной зоной `1.168.192.in-addr.arpa`. Работа DNS-сервера проверена при помощи утилит `dig` и `host`. Написан скрипт `dns.sh` для Vagrant, фиксирующий действия по установке и конфигурированию DNS-сервера во внутреннем окружении виртуальной машины server, и внесены соответствующие изменения в `Vagrantfile`.

## Контрольные вопросы

**1. Что такое DNS?**

DNS (Domain Name System, система доменных имён) — распределённая система (распределённая база данных), ставящая в соответствие доменному имени хоста (компьютера или другого сетевого устройства) IP-адрес, и наоборот.

**2. Каково назначение кэширующего DNS-сервера?**

Кэширующий DNS-сервер получает рекурсивные запросы от клиентов и выполняет их с помощью нерекурсивных запросов к авторитативным серверам, кэшируя полученные ответы для ускорения последующих обращений.

**3. Чем отличается прямая DNS-зона от обратной?**

Прямая зона ставит в соответствие доменному имени IP-адрес (записи A, AAAA), а обратная — IP-адрес доменному имени (записи PTR). Обратная зона располагается в специальном домене `in-addr.arpa`.

**4. В каких каталогах и файлах располагаются настройки DNS-сервера? Кратко охарактеризуйте, за что они отвечают.**

- `/etc/named.conf` — главный конфигурационный файл DNS-сервера;
- `/etc/named/` — каталог с дополнительными файлами описания зон;
- `/var/named/` — каталог с файлами данных зон (прямых и обратных);
- `/etc/resolv.conf` — файл, задающий DNS-серверы, используемые локальной системой.

**5. Что указывается в файле resolv.conf?**

В файле `resolv.conf` указываются DNS-серверы (директива `nameserver`), домен поиска (`search`, `domain`) и другие параметры разрешения имён.

**6. Какие типы записи описания ресурсов есть в DNS и для чего они используются?**

- SOA — указывает на авторитативность для зоны;
- NS — перечисляет DNS-серверы зоны;
- A — задаёт отображение имени узла в IP-адрес;
- PTR — задаёт отображение IP-адреса в имя узла;
- CNAME — задаёт каноническое имя (для псевдонимов);
- MX — задаёт имена почтовым серверам.

**7. Для чего используется домен in-addr.arpa?**

Домен `in-addr.arpa` используется для обратного разрешения имён: в нём хранятся PTR-записи, ставящие в соответствие IP-адресу доменное имя.

**8. Для чего нужен демон named?**

Демон `named` — это реализация DNS-сервера в составе пакета BIND, которая обслуживает DNS-запросы: отвечает за разрешение имён, кэширование, обслуживание зон.

**9. В чём заключаются основные функции slave-сервера и master-сервера?**

Master-сервер (первичный) производит загрузку данных для зоны из файла на машине-сервере. Slave-сервер (вторичный) получает данные зоны от master-сервера и синхронизирует их с ним.

**10. Какие параметры отвечают за время обновления зоны?**

Параметры `refresh` (интервал обращения к master), `retry` (интервал повторной попытки), `expire` (интервал, после которого slave прекращает обслуживание зоны) и `minimum` (время негативного кэширования) в SOA-записи.

**11. Как обеспечить защиту зоны от скачивания и просмотра?**

При помощи директивы `allow-transfer { none; };` в описании зоны, а также ограничения `allow-query`.

**12. Какая запись RR применяется при создании почтовых серверов?**

Запись MX.

**13. Как протестировать работу сервера доменных имён?**

При помощи утилит `dig` и `host` — например, `dig @127.0.0.1 <имя>`, `host -l <зона>`, `host -a <зона>`, `host -t PTR <ip>`.

**14. Как запустить, перезапустить или остановить какую-либо службу в системе?**

```bash
systemctl start <service>
systemctl restart <service>
systemctl stop <service>
```

**15. Как посмотреть отладочную информацию при запуске какого-либо сервиса или службы?**

При помощи `journalctl -x -f -u <service>` в реальном времени или `journalctl -x -u <service>` за прошедший период.

**16. Где храниться отладочная информация по работе системы и служб? Как её посмотреть?**

В журнале systemd (journal). Просмотр — через `journalctl`.

**17. Как посмотреть, какие файлы использует в своей работе тот или иной процесс? Приведите несколько примеров.**

При помощи утилиты `lsof`, например: `lsof | grep named`, `lsof -p <PID>`, `lsof | grep UDP`.

**18. Приведите несколько примеров по изменению сетевого соединения при помощи командного интерфейса nmcli.**

```bash
nmcli connection show
nmcli connection edit eth0
nmcli connection modify eth0 ipv4.dns "127.0.0.1"
nmcli connection up eth0
nmcli connection down eth0
```

**19. Что такое SELinux?**

SELinux (Security-Enhanced Linux) — механизм мандатного контроля доступа в Linux, реализованный в ядре.

**20. Что такое контекст (метка) SELinux?**

Контекст (метка) SELinux — специальный атрибут безопасности, присваиваемый процессам и файлам, на основе которого система принимает решения о доступе.

**21. Как восстановить контекст SELinux после внесения изменений в конфигурационные файлы?**

```bash
restorecon -vR /etc
restorecon -vR /var/named
```

**22. Как создать разрешающие правила политики SELinux из файлов журналов, содержащих сообщения о запрете операций?**

При помощи утилиты `audit2allow`, например: `audit2allow -a -M mypol`, затем `semodule -i mypol.pp`.

**23. Что такое булевый переключатель в SELinux?**

Булевый переключатель SELinux — параметр политики, который можно включать и выключать во время работы системы, изменяя разрешения для определённых операций.

**24. Как посмотреть список переключателей SELinux и их состояние?**

```bash
getsebool -a
getsebool -a | grep named
```

**25. Как изменить значение переключателя SELinux?**

```bash
setsebool <name> 1
setsebool -P <name> 1
```

## Список литературы

1. Bart D. Common DNS Operational and Configuration Errors: RFC / RFC Editor. — 02/1996. — DOI: 10.17487/rfc1912.
2. Security-Enhanced Linux. Linux с улучшенной безопасностью: руководство пользователя / M. McAllister, S. Radvan, D. Walsh, D. Grift, E. Paris, J. Morris.
3. Systemd. — 2015. — URL: https://wiki.archlinux.org/index.php/Systemd
4. Костромин В. А. Утилита lsof — инструмент администратора.
5. Поттеринг Л. Systemd для администраторов: цикл статей. — 2010.
6. Сайт проекта NetworkManager. — URL: https://wiki.gnome.org/Projects/NetworkManager
7. Сайт проекта nmcli. — URL: https://developer.gnome.org/NetworkManager/stable/nmcli.html
