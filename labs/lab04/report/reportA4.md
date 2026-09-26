---
## Front matter
title: "Отчет по лабораторной работе № 4"
subtitle: "Базовая настройка HTTP-сервера Apache"
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

Приобретение практических навыков по установке и базовому конфигурированию HTTP-сервера Apache.

# Задание

1. Установите необходимые для работы HTTP-сервера пакеты (см. раздел 4.4.1).
2. Запустите HTTP-сервер с базовой конфигурацией и проанализируйте его работу (см. разделы 4.4.2 и 4.4.3).
3. Настройте виртуальный хостинг (см. раздел 4.4.4).
4. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке HTTP-сервера во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 4.4.5).

# Выполнение лабораторной работы

## Установка HTTP-сервера

Загружаем операционную систему и переходим в рабочий каталог с проектом. Запускаем виртуальную машину server. На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя (рис. [-@fig:001]).

![Вход на виртуальную машину server под пользователем mavinogradova и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA4/1.png){#fig:001 width=70%}

Просматриваем список доступных групп пакетов с помощью команды `LANG=C yum grouplist` (рис. [-@fig:002]). 

![Просмотр доступных групп пакетов командой LANG=C yum grouplist](/home/mavinogradova/skreens/lbA4/2.png){#fig:002 width=70%}

В списке доступных групп присутствуют Server, Minimal Install, Workstation.

Устанавливаем из репозитория стандартный веб-сервер (HTTP-сервер и утилиты httpd, криптоутилиты и пр.) командой `dnf -y groupinstall "Basic Web Server"` (рис. [-@fig:003]). 

![Установка группы пакетов Basic Web Server](/home/mavinogradova/skreens/lbA4/3.png){#fig:003 width=70%}

Вместе с httpd устанавливаются httpd-manual, mod_fcgid, mod_ssl, а также зависимости apr, apr-util, httpd-core, httpd-filesystem, httpd-tools, rocky-logos-httpd.


## Базовое конфигурирование HTTP-сервера

Просматриваем и комментируем содержание конфигурационных файлов в каталогах `/etc/httpd/conf` и `/etc/httpd/conf.d` (рис. [-@fig:004]). 


![Содержимое каталогов /etc/httpd/conf и /etc/httpd/conf.d](/home/mavinogradova/skreens/lbA4/4.png){#fig:004 width=70%}

В каталоге `/etc/httpd/conf` расположен основной файл конфигурации веб-сервера `httpd.conf`, содержащий директивы, управляющие работой сервера, а также файл `magic`. В каталоге `/etc/httpd/conf.d` расположены дополнительные конфигурационные файлы, подключаемые к основному: `autoindex.conf`, `fcgid.conf`, `manual.conf`, `README`, `ssl.conf`, `userdir.conf`, `welcome.conf`.

Вносим изменения в настройки межсетевого экрана узла server, разрешив работу с http. Проверяем текущий список разрешённых служб, список всех известных firewalld служб, добавляем http в runtime и permanent (рис. [-@fig:005]).

![Настройка межсетевого экрана: добавление службы http](/home/mavinogradova/skreens/lbA4/5.png){#fig:005 width=70%}

В дополнительном терминале запускаем в режиме реального времени расширенный лог системных сообщений, чтобы проверить корректность работы системы (рис. [-@fig:006]).

![Запуск расширенного лога системных сообщений journalctl -x -f](/home/mavinogradova/skreens/lbA4/6.png){#fig:006 width=70%}

В первом терминале активируем и запускаем HTTP-сервер командами `systemctl enable httpd` и `systemctl start httpd` (рис. [-@fig:007]). 

![Активация и запуск HTTP-сервера](/home/mavinogradova/skreens/lbA4/7.png){#fig:007 width=70%}

При выполнении `systemctl enable httpd` создаётся символическая ссылка `/etc/systemd/system/multi-user.target.wants/httpd.service` на `/usr/lib/systemd/system/httpd.service`.

Просмотрев расширенный лог системных сообщений, убеждаемся, что веб-сервер успешно запустился (рис. [-@fig:008]). 

![Проверка статуса HTTP-сервера](/home/mavinogradova/skreens/lbA4/8.png){#fig:008 width=70%}

Статус службы — `active (running)`, в логе видно сообщение `Server configured, listening on: port 443, port 80` и `Started httpd.service - The Apache HTTP Server`.

## Анализ работы HTTP-сервера

Запускаем виртуальную машину client (рис. [-@fig:009]). 

![Запуск виртуальной машины client](/home/mavinogradova/skreens/lbA4/9.png){#fig:009 width=70%}

При запуске видно, что Adapter 1 — nat, Adapter 2 — intnet, SSH-адрес — 127.0.0.1:2200, машина успешно загружена.

На виртуальной машине server просматриваем лог ошибок работы веб-сервера командой `tail -f /var/log/httpd/error_log` (рис. [-@fig:010]).

![Просмотр лога ошибок работы веб-сервера](/home/mavinogradova/skreens/lbA4/10.png){#fig:010 width=70%}

 В логе видны стартовые сообщения Apache: suEXEC mechanism enabled, AH00558 (не удаётся определить FQDN сервера), SELinux policy enabled, Apache/2.4.63 configured — resuming normal operations.

На виртуальной машине client запускаем браузер и в адресной строке вводим 192.168.1.1 (рис. [-@fig:011]). 

![Обращение к веб-серверу с виртуальной машины client](/home/mavinogradova/skreens/lbA4/11.png){#fig:011 width=70%}

В ответ отдаётся стандартная тестовая страница HTTP Server Test Page, что подтверждает доступность веб-сервера по IP-адресу.

На виртуальной машине server запускаем мониторинг доступа к веб-серверу командой `tail -f /var/log/httpd/access_log` (рис. [-@fig:012]). 

![Мониторинг доступа к веб-серверу](/home/mavinogradova/skreens/lbA4/12.png){#fig:012 width=70%}

В логе появляется запись `192.168.1.30 - - [26/Sep/2026:14:56:29 +0000] "GET / HTTP/1.1" 403 7620 "-" "curl/8.12.1"`. Код 403 (Forbidden) появляется, потому что в каталоге DocumentRoot отсутствует файл index.html, а листинг каталогов запрещён директивой Options -Indexes.

## Настройка виртуального хостинга для HTTP-сервера

Требуется настроить виртуальный хостинг по двум DNS-адресам: server.mavinogradova.net и www.mavinogradova.net.

Останавливаем работу DNS-сервера для внесения изменений в файлы описания DNS-зон (рис. [-@fig:013]). 

![Остановка DNS-сервера](/home/mavinogradova/skreens/lbA4/13.png){#fig:013 width=70%}

В логе видно, что named корректно завершил работу: no longer listening on адресах 10.0.2.15#53, ::1#53, 192.168.1.1#53, shutting down: flushing changes, exiting.

Добавляем запись для HTTP-сервера в конце файла прямой DNS-зоны `/var/named/master/fz/mavinogradova.net` (рис. [-@fig:014]). 

![Добавление записи www в файл прямой DNS-зоны](/home/mavinogradova/skreens/lbA4/14.png){#fig:014 width=70%}

В файл добавлена строка `www A 192.168.1.1`. В файле присутствуют записи SOA, NS, A для dhcp, ns, server, client (с DHCID) и другие.

Добавляем запись в конце файла обратной DNS-зоны `/var/named/master/rz/192.168.1` (рис. [-@fig:015]). 

![Добавление PTR-записи в файл обратной DNS-зоны](/home/mavinogradova/skreens/lbA4/15.png){#fig:015 width=70%}

В файл добавлена PTR-запись для IP 1 — `server.mavinogradova.net.`. В файле присутствуют SOA, NS, PTR-записи для IP 1 (server, ns, dhcp) и для IP 30 (client, с DHCID).

Перезапускаем DNS-сервер командой `systemctl start named`. В каталоге `/etc/httpd/conf.d` создаём файлы `server.mavinogradova.net.conf` и `www.mavinogradova.net.conf` (рис. [-@fig:016]).

![Запуск DNS-сервера и создание конфигурационных файлов виртуальных хостов](/home/mavinogradova/skreens/lbA4/16.png){#fig:016 width=70%}

Открываем на редактирование файл `server.mavinogradova.net.conf` и вносим необходимое содержание (рис. [-@fig:017]). 

![Содержимое файла server.mavinogradova.net.conf](/home/mavinogradova/skreens/lbA4/17.png){#fig:017 width=70%}

В файле задаются директивы VirtualHost, ServerName (server.mavinogradova.net), DocumentRoot (/var/www server.mavinogradova.net), ErrorLog и CustomLog.

Открываем на редактирование файл `www.mavinogradova.net.conf` и вносим необходимое содержание (рис. [-@fig:018]). 

![Содержимое файла www.mavinogradova.net.conf](/home/mavinogradova/skreens/lbA4/18.png){#fig:018 width=70%}

В файле задаются директивы VirtualHost, ServerAdmin, DocumentRoot (/var/www/www.mavinogradova.net), ServerName (www.mavinogradova.net), ErrorLog и CustomLog.

Переходим в каталог `/var/www/html`, в котором должны находиться файлы с содержимым (контентом) веб-серверов, и создаём тестовые страницы для виртуальных веб-серверов server.mavinogradova.net и www.mavinogradova.net. Для виртуального веб-сервера server.mavinogradova.net создаём каталог и файл index.html (рис. [-@fig:019]).

![Создание каталога и файла index.html для виртуального веб-сервера server.mavinogradova.net](/home/mavinogradova/skreens/lbA4/19.png){#fig:019 width=70%}

Открываем на редактирование файл index.html и вносим следующее содержание для виртуального веб-сервера server.mavinogradova.net (рис. [-@fig:020]): «Welcome to the server.mavinogradova.net server».

![Содержимое index.html виртуального веб-сервера server.mavinogradova.net](/home/mavinogradova/skreens/lbA4/20.png){#fig:020 width=70%}

Создаём каталог и файл index.html для виртуального веб-сервера www.mavinogradova.net (рис. [-@fig:021]).

![Создание каталога и файла index.html для виртуального веб-сервера www.mavinogradova.net](/home/mavinogradova/skreens/lbA4/21.png){#fig:021 width=70%}

Открываем на редактирование файл index.html и вносим следующее содержание для виртуального веб-сервера www.mavinogradova.net (рис. [-@fig:022]): «Welcome to the www.mavinogradova.net server».

![Содержимое index.html виртуального веб-сервера www.mavinogradova.net](/home/mavinogradova/skreens/lbA4/22.png){#fig:022 width=70%}

Скорректируем права доступа в каталог с веб-контентом командой `chown -R apache:apache /var/www` (рис. [-@fig:023]).

![Корректировка прав доступа в каталог с веб-контентом](/home/mavinogradova/skreens/lbA4/23.png){#fig:023 width=70%}

Восстанавливаем контекст безопасности в SELinux командами `restorecon -vR /etc`, `restorecon -vR /var/named`, `restorecon -vR /var/www` (рис. [-@fig:024]). 

![Восстановление контекста безопасности в SELinux](/home/mavinogradova/skreens/lbA4/24.png){#fig:024 width=70%}

При выполнении первой команды выполнена релэйбл eth1.nmconnection с user_tmp_t на NetworkManager_etc_rw_t.

Перезапускаем HTTP-сервер командой `systemctl restart httpd` (рис. [-@fig:025]).

![Перезапуск HTTP-сервера](/home/mavinogradova/skreens/lbA4/25.png){#fig:025 width=70%}

На виртуальной машине client убеждаемся в корректном доступе к веб-серверу по адресам server.mavinogradova.net и www.mavinogradova.net (рис. [-@fig:026]). 

![Проверка корректного доступа к виртуальным веб-серверам с машины client](/home/mavinogradova/skreens/lbA4/26.png){#fig:026 width=70%}

При обращении к первому адресу получен ответ «Welcome to the server.mavinogradova.net server», ко второму — «Welcome to the www.mavinogradova.net server». Это подтверждает корректную работу виртуального хостинга.

## Внесение изменений в настройки внутреннего окружения виртуальной машины

Переходим в каталог `/vagrant/provision/server`. Создаём каталоги `/vagrant/provision/server/http/etc/httpd/conf.d` и `/vagrant/provision/server/http/var/www/html`. Копируем конфигурационные файлы и контент: `cp -R /etc/httpd/conf.d/* /vagrant/provision/server/http/etc/httpd/conf.d/` и `cp -R /var/www/html/* /vagrant/provision/server/http/var/www/html`. Переходим в каталог `/vagrant/provision/server/dns/` и копируем зоны DNS командой `cp -R /var/named/* /vagrant/provision/server/dns/var/named/` (рис. [-@fig:027]). 

![Копирование конфигурационных файлов и зон в каталог provision](/home/mavinogradova/skreens/lbA4/27.png){#fig:027 width=70%}

При копировании выданы предупреждения overwrite (файлы уже существовали) и ошибки cannot create symbolic link ... Protocol error (symlinks через shared folder не поддерживаются).

В каталоге `/vagrant/provision/server` создаём исполняемый файл `http.sh` командами `touch http.sh` и `chmod +x http.sh` (рис. [-@fig:028]).

![Создание исполняемого файла http.sh](/home/mavinogradova/skreens/lbA4/28.png){#fig:028 width=70%}

Открыв файл http.sh на редактирование, прописываем в нём скрипт провижининга (рис. [-@fig:029]). 

![Содержимое скрипта провижининга http.sh](/home/mavinogradova/skreens/lbA4/29.png){#fig:029 width=70%}

Скрипт выполняет: вывод сообщения о запуске, установку группы пакетов «Basic Web Server», копирование конфигурационных файлов из /vagrant/provision/server/http/etc/httpd/* в /etc/httpd и из /vagrant/provision/server/http/var/www/* в /var/www, смену владельца каталога /var/www на apache:apache, восстановление SELinux-контекстов, добавление http в firewall (runtime и permanent), включение и запуск httpd. Этот скрипт, по сути, повторяет произведённые действия по установке и настройке HTTP-сервера.

Для отработки созданного скрипта во время загрузки виртуальных машин в конфигурационном файле Vagrantfile добавляем в конфигурации сервера следующую запись (рис. [-@fig:030]): `server.vm.provision "server http", type: "shell", preserve_order: true, path: "provision/server/http.sh"`. Блок добавлен после провижининга `server dhcp`.

![Добавление записи о провижининге в Vagrantfile](/home/mavinogradova/skreens/lbA4/30.png){#fig:030 width=70%}

## Полные тексты конфигурационных файлов

### Файл /etc/httpd/conf.d/server.mavinogradova.net.conf

```
<VirtualHost *:80>
    ServerName server.mavinogradova.net
    DocumentRoot /var/www/server.mavinogradova.net
    ErrorLog /var/log/httpd/server.mavinogradova.net-error.log
    CustomLog /var/log/httpd/server.mavinogradova.net-access.log combined
</VirtualHost>
```

### Файл /etc/httpd/conf.d/www.mavinogradova.net.conf

```
<VirtualHost *:80>
    ServerAdmin webmaster@mavinogradova.net
    DocumentRoot /var/www/www.mavinogradova.net
    ServerName www.mavinogradova.net
    ErrorLog /var/log/httpd/www.mavinogradova.net-error.log
    CustomLog /var/log/httpd/www.mavinogradova.net-access.log combined
</VirtualHost>
```

### Файл /var/named/master/fz/mavinogradova.net

```
$ORIGIN .
$TTL 86400      ; 1 day
mavinogradova.net       IN SOA  mavinogradova.net. server.mavinogradova.net. (
                                2026091917 ; serial
                                86400      ; refresh (1 day)
                                3600       ; retry (1 hour)
                                604800     ; expire (1 week)
                                10800      ; minimum (3 hours)
                                )
                        NS      mavinogradova.net.
                        A       192.168.1.1
$ORIGIN mavinogradova.net.
$TTL 1200       ; 20 minutes
client                  A       192.168.1.30
                        DHCID   ( AAEBOW4bnNLlYsViJMoCgSGUngZ/9eejxOcobuJ+mcGz
                                ylE= ) ; 1 1 32
$TTL 86400      ; 1 day
dhcp                    A       192.168.1.1
ns                      A       192.168.1.1
server                  A       192.168.1.1
www                     A       192.168.1.1
```
### Файл /var/named/master/rz/192.168.1

```
$ORIGIN .
$TTL 86400      ; 1 day
1.168.192.in-addr.arpa  IN SOA  1.168.192.in-addr.arpa. server.mavinogradova.net. (
                                2026091916 ; serial
                                86400      ; refresh (1 day)
                                3600       ; retry (1 hour)
                                604800     ; expire (1 week)
                                10800      ; minimum (3 hours)
                                )
                        NS      1.168.192.in-addr.arpa.
                        A       192.168.1.1
$ORIGIN 1.168.192.in-addr.arpa.
1                       PTR     server.mavinogradova.net.
                        PTR     ns.mavinogradova.net.
                        PTR     dhcp.mavinogradova.net.
$TTL 1200       ; 20 minutes
30                      PTR     client.mavinogradova.net.
                        DHCID   ( AAEBOW4bnNLlYsViJMoCgSGUngZ/9eejxOcobuJ+mcGz
                                ylE= ) ; 1 1 32
1                       PTR     server.mavinogradova.net.
```

### Файл /vagrant/provision/server/http.sh

```
#!/bin/bash
echo "Provisioning script $0"
echo "Install needed packages"
dnf -y groupinstall "Basic Web Server"
echo "Copy configuration files"
cp -R /vagrant/provision/server/http/etc/httpd/* /etc/httpd
cp -R /vagrant/provision/server/http/var/www/* /var/www
chown -R apache:apache /var/www
restorecon -vR /etc
restorecon -vR /var/www
echo "Configure firewall"
firewall-cmd --add-service=http
firewall-cmd --add-service=http --permanent
echo "Start http service"
systemctl enable httpd
systemctl start httpd
```

### Фрагмент Vagrantfile

```
server.vm.provision "server http",
    type: "shell",
    preserve_order: true,
    path: "provision/server/http.sh"
```

## Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и базовому конфигурированию HTTP-сервера Apache. Были установлены необходимые для работы HTTP-сервера пакеты (группа «Basic Web Server»), включающие httpd, httpd-manual, mod_fcgid, mod_ssl и зависимости. HTTP-сервер запущен с базовой конфигурацией, проанализирована его работа с помощью логов `/var/log/httpd/error_log` и `/var/log/httpd/access_log`: в логе ошибок зафиксированы стартовые сообщения Apache, в логе доступа — запросы клиента. Настроен виртуальный хостинг по двум DNS-адресам — server.mavinogradova.net и www.mavinogradova.net: внесены изменения в прямую и обратную зоны DNS, созданы конфигурационные файлы виртуальных хостов в /etc/httpd/conf.d, созданы каталоги с тестовым контентом, восстановлены SELinux-контексты, перезапущен httpd, и проверен доступ к веб-серверу с клиента — оба адреса возвращают разный контент. Написан скрипт для Vagrant `http.sh`, фиксирующий действия по установке и настройке HTTP-сервера во внутреннем окружении виртуальной машины server, и внесены соответствующие изменения в Vagrantfile.

# Ответы на контрольные вопросы

1. **Через какой порт по умолчанию работает Apache?** По умолчанию Apache работает через порт 80 (протокол HTTP). Для защищённого соединения HTTPS используется порт 443.

2. **Под каким пользователем запускается Apache и к какой группе относится этот пользователь?** Apache запускается под пользователем apache, который относится к группе apache.

3. **Где располагаются лог-файлы веб-сервера? Что можно по ним отслеживать?** Лог-файлы веб-сервера располагаются в каталоге /var/log/httpd/: access_log — лог доступа, error_log — лог ошибок. По ним можно отслеживать запросы клиентов (IP-адрес, дата и время, метод, запрошенный ресурс, код ответа, размер ответа, User-Agent), а также ошибки работы сервера.

4. **Где по умолчанию содержится контент веб-серверов?** По умолчанию контент веб-серверов содержится в каталоге /var/www/html. Именно он указывается в директиве DocumentRoot основного конфигурационного файла httpd.conf.

5. **Каким образом реализуется виртуальный хостинг? Что он даёт?** Виртуальный хостинг реализуется посредством директивы `<VirtualHost>` в конфигурационных файлах, располагаемых в каталоге /etc/httpd/conf.d/. Для каждого виртуального хоста задаются директивы ServerName, DocumentRoot, ErrorLog, CustomLog. Виртуальный хостинг позволяет на одном сервере (с одним IP-адресом) обслуживать несколько сайтов (доменов) с разным содержимым, что экономит ресурсы и упрощает администрирование.

