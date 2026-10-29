---
## Front matter
title: "Отчет по лабораторной работе № 9"
subtitle: "Настройка POP3/IMAP сервера"
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

Приобретение практических навыков по установке и простейшему конфигурированию POP3/IMAP-сервера.

# Задание

1. Установите на виртуальной машине server Dovecot и Telnet для дальнейшей проверки корректности работы почтового сервера (см. раздел 9.4.1).
2. Настройте Dovecot (см. раздел 9.4.2).
3. Установите на виртуальной машине client программу для чтения почты Evolution и настройте её для манипуляций с почтой вашего пользователя. Проверьте корректность работы почтового сервера как с виртуальной машины server, так и с виртуальной машины client (см. раздел 9.4.3).
4. Измените скрипт для Vagrant, фиксирующий действия по установке и настройке Postfix и Dovecot во внутреннем окружении виртуальной машины server, создайте скрипт для Vagrant, фиксирующий действия по установке Evolution во внутреннем окружении виртуальной машины client. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 9.4.4).

# Выполнение лабораторной работы

## Установка Dovecot

На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя. Устанавливаем необходимые для работы пакеты: dovecot и telnet (рис. [-@fig:001]).

![Вход на виртуальную машину server, переход в режим суперпользователя и установка пакетов dovecot и telnet](/home/mavinogradova/skreens/lbA9/1.png){#fig:001 width=70%}

## Настройка Dovecot

В конфигурационном файле `/etc/dovecot/dovecot.conf` прописываем список почтовых протоколов, по которым разрешено работать Dovecot: `protocols = imap pop3` (рис. [-@fig:002]).

![Настройка списка протоколов в файле dovecot.conf](/home/mavinogradova/skreens/lbA9/2.png){#fig:002 width=70%}

В конфигурационном файле `/etc/dovecot/conf.d/10-auth.conf` проверяем, что указан метод аутентификации plain: `auth_mechanisms = plain` (рис. [-@fig:003]).

![Проверка метода аутентификации в файле 10-auth.conf](/home/mavinogradova/skreens/lbA9/3.png){#fig:003 width=70%}

В конфигурационном файле `/etc/dovecot/conf.d/auth-system.conf.ext` проверяем, что для поиска пользователей и их паролей используются PAM и файл passwd: `passdb { driver = pam }` (рис. [-@fig:004]) и `userdb { driver = passwd }` (рис. [-@fig:005]).

![Проверка настройки passdb в файле auth-system.conf.ext](/home/mavinogradova/skreens/lbA9/4.png){#fig:004 width=70%}

![Проверка настройки userdb в файле auth-system.conf.ext](/home/mavinogradova/skreens/lbA9/5.png){#fig:005 width=70%}

В конфигурационном файле `/etc/dovecot/conf.d/10-mail.conf` настраиваем местоположение почтовых ящиков пользователей: `mail_location = maildir:~/Maildir` (рис. [-@fig:006]).

![Настройка местоположения почтовых ящиков в файле 10-mail.conf](/home/mavinogradova/skreens/lbA9/6.png){#fig:006 width=70%}

В Postfix задаем каталог для доставки почты: `postconf -e 'home_mailbox = Maildir/'`. Сконфигурируем межсетевой экран, разрешив работу службам протоколов POP3 и IMAP (рис. [-@fig:007]).

![Настройка Postfix и межсетевого экрана для POP3/IMAP](/home/mavinogradova/skreens/lbA9/7.png){#fig:007 width=70%}

Восстанавливаем контекст безопасности в SELinux, перезапускаем Postfix и запускаем Dovecot (рис. [-@fig:008]).

![Восстановление контекста SELinux, перезапуск Postfix и запуск Dovecot](/home/mavinogradova/skreens/lbA9/8.png){#fig:008 width=70%}

## Проверка работы Dovecot

На дополнительном терминале виртуальной машины server запускаем мониторинг работы почтовой службы: `tail -f /var/log/maillog` (рис. [-@fig:009]).

![Запуск мониторинга логов почтовой службы](/home/mavinogradova/skreens/lbA9/9.png){#fig:009 width=70%}

На терминале сервера для просмотра имеющихся писем используем `MAIL=~/Maildir mail` (рис. [-@fig:010]). Писем пока нет, что ожидаемо.

![Просмотр имеющихся писем утилитой mail](/home/mavinogradova/skreens/lbA9/10.png){#fig:010 width=70%}

Для просмотра mailbox пользователя на сервере на терминале с правами суперпользователя используем команду `doveadm mailbox list -u mavinogradova` (рис. [-@fig:011]).

![Просмотр списка почтовых ящиков пользователя через doveadm](/home/mavinogradova/skreens/lbA9/11.png){#fig:011 width=70%}

На виртуальной машине client входим под своим пользователем, открываем терминал и переходим в режим суперпользователя (рис. [-@fig:012]).

![Вход на виртуальную машину client и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA9/12.png){#fig:012 width=70%}

Устанавливаем почтовый клиент: `dnf -y install evolution` (рис. [-@fig:013]).

![Установка почтового клиента Evolution](/home/mavinogradova/skreens/lbA9/13.png){#fig:013 width=70%}

Запускаем почтовый клиент Evolution из графического меню приложений (рис. [-@fig:014]).

![Графическое окружение виртуальной машины client с установленным Evolution](/home/mavinogradova/skreens/lbA9/14.png){#fig:014 width=70%}

В окне настройки учётной записи почты указываем имя, адрес почты в виде `mavinogradova@mavinogradova.net`, вводим пароль пользователя (рис. [-@fig:015]).

![Настройка учетной записи в Evolution (вкладка Identity)](/home/mavinogradova/skreens/lbA9/15.png){#fig:015 width=70%}

В качестве IMAP-сервера для входящих сообщений прописываем `mail.mavinogradova.net`, в качестве пользователя указываем `mavinogradova`. Проверяем номер порта: для IMAP – порт 143. Проверяем настройки SSL и метода аутентификации: для IMAP — STARTTLS, аутентификация по обычному паролю (рис. [-@fig:016]).

![Настройка получения почты (IMAP) в Evolution](/home/mavinogradova/skreens/lbA9/16.png){#fig:016 width=70%}

Для SMTP-сервера для исходящих сообщений прописываем `mail.mavinogradova.net`, порт 25. Для SMTP — без шифрования, аутентификация — «Без аутентификации» (рис. [-@fig:017]).

![Настройка отправки почты (SMTP) в Evolution](/home/mavinogradova/skreens/lbA9/17.png){#fig:017 width=70%}

После завершения настройки в главном окне Evolution появляется учетная запись (рис. [-@fig:018]).

![Главное окно Evolution после настройки учетной записи](/home/mavinogradova/skreens/lbA9/18.png){#fig:018 width=70%}

Из почтового клиента отправляем себе несколько тестовых писем (рис. [-@fig:019], рис. [-@fig:020], рис. [-@fig:021], рис. [-@fig:022]).

![Создание первого тестового письма](/home/mavinogradova/skreens/lbA9/19.png){#fig:019 width=70%}

![Создание и отправка третьего тестового письма](/home/mavinogradova/skreens/lbA9/20.png){#fig:020 width=70%}

![Создание четвертого тестового письма](/home/mavinogradova/skreens/lbA9/21.png){#fig:021 width=70%}

![Создание финального тестового письма](/home/mavinogradova/skreens/lbA9/22.png){#fig:022 width=70%}

При возникновении сообщения о небезопасном соединении выставляем галочку о понимании риска и подтверждаем исключение безопасности (рис. [-@fig:023]). Вводим пароль пользователя (рис. [-@fig:024]).

![Предупреждение о небезопасном SSL/TLS сертификате](/home/mavinogradova/skreens/lbA9/23.png){#fig:023 width=70%}

![Ввод пароля для учетной записи](/home/mavinogradova/skreens/lbA9/24.png){#fig:024 width=70%}

Убеждаемся, что письма доставлены. В папке «Отправленные» (Sent) находятся 5 писем (рис. [-@fig:025]), а в папке «Входящие» (Inbox) — успешно полученные письма (рис. [-@fig:026]).

![Папка Sent с отправленными письмами](/home/mavinogradova/skreens/lbA9/25.png){#fig:025 width=70%}

![Папка Inbox с полученными письмами](/home/mavinogradova/skreens/lbA9/26.png){#fig:026 width=70%}

Параллельно смотрим, какие сообщения выдаются при мониторинге почтовой службы на сервере, а также при использовании `doveadm` (рис. [-@fig:027]). В логах видно успешное принятие писем Postfix, доставку в Maildir и IMAP-авторизацию клиента с TLS (рис. [-@fig:028]).

![Проверка статуса почтового ящика через doveadm](/home/mavinogradova/skreens/lbA9/27.png){#fig:027 width=70%}

![Мониторинг логов maillog во время отправки и получения писем](/home/mavinogradova/skreens/lbA9/28.png){#fig:028 width=70%}

Проверяем работу почтовой службы, используя на сервере протокол Telnet. Подключаемся к почтовому серверу по протоколу POP3 (через порт 110), вводим логин и пароль, с помощью команды `list` получаем список писем, командой `retr 1` получаем первое письмо, командой `dele 2` удаляем второе письмо, командой `quit` завершаем сеанс (рис. [-@fig:029]).

![Проверка работы POP3 через Telnet](/home/mavinogradova/skreens/lbA9/29.png){#fig:029 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`. В соответствующие подкаталоги помещаем конфигурационные файлы Dovecot (рис. [-@fig:030]).

![Копирование конфигурационных файлов Dovecot в каталог провижининга](/home/mavinogradova/skreens/lbA9/30.png){#fig:030 width=70%}

Вносим изменения в файл `/vagrant/provision/server/mail.sh`, добавив в него строки по установке Dovecot и Telnet, по настройке межсетевого экрана, а также копирование конфигурационных файлов и запуск службы Dovecot (рис. [-@fig:031]).

![Редактирование скрипта provision/server/mail.sh](/home/mavinogradova/skreens/lbA9/31.png){#fig:031 width=70%}

Полный текст файла `/vagrant/provision/server/mail.sh`:

```bash
#!/bin/bash
echo "Provisioning script $0"

echo "Install needed packages"
dnf -y install postfix
dnf -y install s-nail
dnf -y install dovecot
dnf -y install telnet

echo "Copy configuration files"
#cp -R /vagrant/provision/server/mail/etc/* /etc

# Копирование конфигурационных файлов Dovecot 
cp /vagrant/provision/server/mail/etc/dovecot/dovecot.conf /etc/dovecot/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/10-auth.conf /etc/dovecot/conf.d/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/auth-system.conf.ext /etc/dovecot/conf.d/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/10-mail.conf /etc/dovecot/conf.d/

echo "Configure firewall"
firewall-cmd --add-service=smtp --permanent
# Добавление портов для POP3/IMAP 
firewall-cmd --add-service=pop3 --permanent
firewall-cmd --add-service=pop3s --permanent
firewall-cmd --add-service=imap --permanent
firewall-cmd --add-service=imaps --permanent
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
# Настройка доставки писем в Maildir 
postconf -e 'home_mailbox = Maildir/'
postfix set-permissions
restorecon -vR /etc

# Запуск Dovecot
systemctl enable dovecot
systemctl start dovecot

systemctl stop postfix
```

На виртуальной машине client в каталоге `/vagrant/provision/client` корректируем файл `mail.sh`, прописав в нём установку Evolution и добавление записи в `/etc/hosts` для корректного разрешения имени почтового сервера (рис. [-@fig:032]).

![Редактирование скрипта provision/client/mail.sh](/home/mavinogradova/skreens/lbA9/32.png){#fig:032 width=70%}

Полный текст файла `/vagrant/provision/client/mail.sh`:

```bash
#!/bin/bash
echo "Provisioning script $0"

echo "Install needed packages"
dnf -y install postfix
dnf -y install s-nail
# Установка почтового клиента Evolution 
dnf -y install evolution

echo "Configure postfix"
postconf -e 'inet_protocols = ipv4'

# Добавление записи в hosts, чтобы клиент видел почтовый сервер
echo "192.168.1.1  mail.mavinogradova.net" >> /etc/hosts

echo "Start postfix service"
systemctl enable postfix
```

Для отработки созданных скриптов во время загрузки виртуальных машин в конфигурационном файле Vagrantfile необходимо добавить в разделах конфигурации для клиента и сервера записи, объявляющие новый провижининг. Фрагмент `Vagrantfile` для клиента:

```ruby
client.vm.provision "client mail",
                    type: "shell",
                    preserve_order: true,
                    path: "provision/client/mail.sh"
```

Фрагмент `Vagrantfile` для сервера:

```ruby
server.vm.provision "server mail",
                    type: "shell",
                    preserve_order: true,
                    path: "provision/server/mail.sh"
```

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и простейшему конфигурированию POP3/IMAP-сервера. Был установлен и настроен Dovecot на сервере, настроены протоколы IMAP и POP3, методы аутентификации и расположение почтовых ящиков (формат Maildir). Проверена работа почтового сервера с помощью утилиты Evolution на клиентской машине, а также с использованием протокола Telnet. Автоматизированы действия по установке и настройке Postfix и Dovecot с помощью скриптов провижининга для Vagrant, а также установка клиента Evolution.

# Ответы на контрольные вопросы

1. **За что отвечает протокол SMTP?** 
Протокол SMTP (Simple Mail Transfer Protocol) отвечает за отправку и передачу электронной почты между серверами, а также за отправку писем от клиента на сервер. Он работает по умолчанию на порту 25.

2. **За что отвечает протокол IMAP?** 
Протокол IMAP (Internet Message Access Protocol) отвечает за доступ к почтовому ящику, который хранится на сервере. Он позволяет клиенту просматривать папки, управлять сообщениями (читать, удалять, перемещать) и синхронизировать их состояние с сервером. Работает по портам 143 (с STARTTLS) и 993 (IMAPS).

3. **За что отвечает протокол POP3?** 
Протокол POP3 (Post Office Protocol 3) отвечает за загрузку писем с сервера на клиентский компьютер. Обычно после загрузки письма удаляются с сервера (хотя это можно настроить). Работает по портам 110 (с STARTTLS) и 995 (POP3S).

4. **В чём назначение Dovecot?** 
Dovecot — это агент доставки почты (MDA) и сервер POP3/IMAP с возможностью обеспечения безопасности и надёжности за счёт использования протокола TLS. Он поддерживает основные форматы почтовых ящиков (mbox и Maildir) и отвечает за предоставление клиентам доступа к их почтовым ящикам.

5. **В каких файлах обычно находятся настройки работы Dovecot? За что отвечает каждый из файлов?** 
Настройки располагаются в файле `/etc/dovecot/dovecot.conf` (основной файл, задающий список протоколов) и в файлах каталога `/etc/dovecot/conf.d`:
   - `10-auth.conf` — отвечает за методы аутентификации (например, `auth_mechanisms = plain`).
   - `auth-system.conf.ext` — отвечает за базы данных пользователей и паролей (passdb, userdb, например, PAM и passwd).
   - `10-mail.conf` — отвечает за местоположение почтовых ящиков пользователей (например, `mail_location = maildir:~/Maildir`).
   - Файл сертификатов безопасности располагается в каталоге `/etc/pki/dovecot`.

6. **В чём назначение Postfix?** 
Postfix — это агент передачи почты (MTA). Его назначение — принимать почту от других серверов или локальных пользователей и доставлять её в нужные почтовые ящики (локальные или удаленные) по протоколу SMTP.

7. **Какие методы аутентификации пользователей можно использовать в Dovecot и в чём их отличие?** 
Dovecot поддерживает такие методы, как `plain`, `login`, `cram-md5`, `digest-md5`, `ntlm`, `gssapi`, `apop`.
   - `plain` и `login` передают пароль в открытом виде (поэтому требуют использования шифрования TLS/SSL).
   - `cram-md5` и `digest-md5` используют хеширование и передают не сам пароль, а хеш, что безопаснее в незащищенных сетях.
   - `gssapi` используется для Kerberos-аутентификации.

8. **Приведите пример заголовка письма с пояснениями его полей.**
```text
Return-Path: <mavinogradova@mavinogradova.net>  # Обратный адрес для ответа об ошибках
Delivered-To: mavinogradova@mavinogradova.net   # Адрес получателя, куда доставлено письмо
Received: from client.mavinogradova.net ...     # Цепочка серверов, через которые прошло письмо
Message-ID: <d5b12...@mavinogradova.net>        # Уникальный идентификатор сообщения
Subject: Test 4                                 # Тема письма
From: mavinogradova <mavinogradova@mavinogradova.net>        # Отправитель
To: mavinogradova@mavinogradova.net             # Получатель
Date: Sat, 24 Oct 2026 13:55:58 +0000           # Дата и время отправки
```

9. **Приведите примеры использования команд для работы с почтовыми протоколами через терминал (например через telnet).**
Подключение по POP3:
```text
telnet mail.mavinogradova.net 110
user mavinogradova
pass ваш_пароль
list        # вывести список писем
retr 1      # получить текст первого письма
dele 2      # пометить второе письмо на удаление
quit        # выйти и применить удаление
```
Подключение по IMAP (пример):
```text
telnet mail.mavinogradova.net 143
a1 LOGIN mavinogradova ваш_пароль
a2 LIST "" "*"
a3 SELECT INBOX
a4 FETCH 1 BODY[]
a5 LOGOUT
```

10. **Приведите примеры с пояснениями по работе с doveadm.**
   - `doveadm mailbox list -u mavinogradova` — выводит список почтовых папок (mailbox) указанного пользователя.
   - `doveadm mailbox status -u mavinogradova all INBOX` — показывает статус папки INBOX (количество сообщений, размер и т.д.).
   - `doveadm user mavinogradova` — выводит информацию о пользователе из базы данных Dovecot.
   - `doveadm pw -s SHA512-CRYPT` — генерирует хеш пароля с использованием алгоритма SHA512.

# Список литературы

1. Методические указания к лабораторной работе № 9 «Настройка POP3/IMAP сервера».
2. Dovecot Documentation. — URL: https://dovecot.org/documentation (visited on 09/13/2021).
3. Postfix Documentation. — URL: http://www.postfix.org/documentation.html (visited on 09/13/2021).
