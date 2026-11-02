---
## Front matter
title: "Отчет по лабораторной работе № 10"
subtitle: "Настройка аутентификации SMTP-сервера"
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

Приобретение практических навыков по конфигурированию SMTP-сервера в части настройки аутентификации.

# Задание

1. Настройте Dovecot для работы с LMTP (см. раздел 10.4.1).
2. Настройте аутентификацию посредством SASL на SMTP-сервере (см. раздел 10.4.2).
3. Настройте работу SMTP-сервера поверх TLS (см. раздел 10.4.3).
4. Скорректируйте скрипт для Vagrant, фиксирующий действия расширенной настройки SMTP-сервера во внутреннем окружении виртуальной машины server (см. раздел 10.4.4).

# Выполнение лабораторной работы

## Настройка LMTP в Dovecot

На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя (рис. [-@fig:001]).

![Вход на виртуальную машину server и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA10/1.png){#fig:001 width=70%}

В дополнительном терминале запускаем мониторинг работы почтовой службы: `tail -f /var/log/maillog` (рис. [-@fig:002]).

![Запуск мониторинга логов почтовой службы](/home/mavinogradova/skreens/lbA10/2.png){#fig:002 width=70%}

В файле `/etc/dovecot/dovecot.conf` добавляем в список протоколов, с которыми может работать Dovecot, протокол LMTP: `protocols = imap pop3 lmtp` (рис. [-@fig:003]).

![Настройка списка протоколов в файле dovecot.conf](/home/mavinogradova/skreens/lbA10/3.png){#fig:003 width=70%}

В файле `/etc/dovecot/conf.d/10-master.conf` настраиваем сервис lmtp для связи с Postfix, определяя расположение файла с описанием прослушиваемого unix-сокета, а также задавая права доступа к нему и принадлежность к группе и пользователю postfix (рис. [-@fig:004]).

![Настройка сервиса lmtp в файле 10-master.conf](/home/mavinogradova/skreens/lbA10/4.png){#fig:004 width=70%}

В файле `/etc/dovecot/conf.d/10-auth.conf` задаем формат имени пользователя для аутентификации в форме логина пользователя без указания домена: `auth_username_format = %Ln` (рис. [-@fig:005]).

![Настройка формата имени пользователя в файле 10-auth.conf](/home/mavinogradova/skreens/lbA10/5.png){#fig:005 width=70%}

Переопределяем в Postfix с помощью `postconf` передачу сообщений не напрямую, а через заданный unix-сокет: `mailbox_transport = lmtp:unix:private/dovecot-lmtp` (рис. [-@fig:006]).

![Переопределение транспорта доставки в Postfix](/home/mavinogradova/skreens/lbA10/6.png){#fig:006 width=70%}

Перезапускаем Postfix и Dovecot для применения настроек (рис. [-@fig:007]).

![Перезапуск служб Postfix и Dovecot](/home/mavinogradova/skreens/lbA10/7.png){#fig:007 width=70%}

Из-под учётной записи своего пользователя отправляем письмо с клиента на сервер (рис. [-@fig:008], рис. [-@fig:009]).

![Вход на виртуальную машину client](/home/mavinogradova/skreens/lbA10/8.png){#fig:008 width=70%}

![Отправка тестового письма с клиента](/home/mavinogradova/skreens/lbA10/9.png){#fig:009 width=70%}

Пояснения по содержанию логов при мониторинге почтовой службы: В логах видно, что Postfix принял письмо от клиента, затем передал его в транспорт `lmtp`. Dovecot успешно подключился, сохранил письмо в почтовый ящик (`saved mail to INBOX`), и Postfix подтвердил успешную доставку (`status=sent`) (рис. [-@fig:010]).

![Мониторинг логов maillog при доставке письма](/home/mavinogradova/skreens/lbA10/10.png){#fig:010 width=70%}

На сервере просматриваем почтовый ящик пользователя с помощью команды `MAIL=~/Maildir/ mail`. Убеждаемся, что отправленные с клиента письма успешно доставлены (рис. [-@fig:011]).

![Просмотр почтового ящика пользователя](/home/mavinogradova/skreens/lbA10/11.png){#fig:011 width=70%}

## Настройка SMTP-аутентификации

В файле `/etc/dovecot/conf.d/10-master.conf` определяем службу аутентификации пользователей. Пояснение записи: `service auth` запускает службу аутентификации; `unix_listener /var/spool/postfix/private/auth` создает сокет для Postfix, через который будут идти запросы на проверку паролей (с правами `postfix` и режимом `0660`); `unix_listener auth-userdb` создает сокет для внутренних запросов базы пользователей, доступный только пользователю `dovecot` с правами `0600` (рис. [-@fig:012]).

![Настройка службы аутентификации в 10-master.conf](/home/mavinogradova/skreens/lbA10/12.png){#fig:012 width=70%}

Для Postfix задаем тип аутентификации SASL для smtpd и путь к соответствующему unix-сокету. Также настраиваем Postfix для приема почты из Интернета только для обслуживаемых нашим сервером пользователей (опция `smtpd_recipient_restrictions`): `reject_unknown_recipient_domain` отклоняет письма для несуществующих доменов, `permit_mynetworks` разрешает свою сеть, `reject_non_fqdn_recipient` требует полный адрес, `reject_unauth_destination` запрещает релей, `reject_unverified_recipient` отклоняет неподтвержденных получателей, а `permit` разрешает остальным пройти. Ограничиваем сеть `mynetworks = 127.0.0.0/8` (рис. [-@fig:013]).

![Настройка SASL и ограничений в Postfix](/home/mavinogradova/skreens/lbA10/13.png){#fig:013 width=70%}

Для проверки работы аутентификации временно запускаем SMTP-сервер (порт 25) с возможностью аутентификации, заменив строку в `/etc/postfix/master.cf` и добавив параметры `-o smtpd_sasl_auth_enable=yes` и `-o smtpd_recipient_restrictions=...` (рис. [-@fig:014]).

![Редактирование файла master.cf для включения SASL](/home/mavinogradova/skreens/lbA10/14.png){#fig:014 width=70%}

Перезапускаем Postfix и Dovecot (рис. [-@fig:015]).

![Перезапуск служб после настройки аутентификации](/home/mavinogradova/skreens/lbA10/15.png){#fig:015 width=70%}

На клиенте устанавливаем telnet (рис. [-@fig:016]).

![Установка telnet на клиенте](/home/mavinogradova/skreens/lbA10/16.png){#fig:016 width=70%}

Генерируем строку для аутентификации, используя логин и пароль пользователя в формате base64 (рис. [-@fig:017]).

![Получение строки для аутентификации в base64](/home/mavinogradova/skreens/lbA10/17.png){#fig:017 width=70%}

Подключаемся на клиенте к SMTP-серверу посредством telnet, проверяем соединение командой `EHLO test`, проверяем авторизацию командой `AUTH PLAIN` и завершаем сессию (рис. [-@fig:018]).

![Проверка SMTP-аутентификации через telnet](/home/mavinogradova/skreens/lbA10/18.png){#fig:018 width=70%}

## Настройка SMTP over TLS

Настраиваем на сервере TLS, воспользовавшись временным сертификатом Dovecot. Предварительно копируем необходимые файлы сертификата и ключа из каталога `/etc/pki/dovecot` в каталог `/etc/pki/tls/` в соответствующие подкаталоги. Сконфигурируем Postfix, указав пути к сертификату и ключу, а также к каталогу для хранения TLS-сессий и уровень безопасности (рис. [-@fig:019]).

![Настройка сертификатов и TLS в Postfix](/home/mavinogradova/skreens/lbA10/19.png){#fig:019 width=70%}

Для того чтобы запустить SMTP-сервер на 587-м порту, в файле `/etc/postfix/master.cf` заменяем строки `smtp inet ...` на стандартную и добавляем блок `submission inet ...` с обязательным шифрованием (`smtpd_tls_security_level=encrypt`), SASL и ограничениями (рис. [-@fig:020]).

![Настройка порта 587 в master.cf](/home/mavinogradova/skreens/lbA10/20.png){#fig:020 width=70%}

Настраиваем межсетевой экран, разрешив работать службе smtp-submission, и перезапускаем Postfix (рис. [-@fig:021]).

![Настройка межсетевого экрана для smtp-submission](/home/mavinogradova/skreens/lbA10/21.png){#fig:021 width=70%}

На клиенте подключаемся к SMTP-серверу через 587-й порт посредством `openssl s_client -starttls smtp` (рис. [-@fig:022]).

![Подключение к порту 587 через openssl](/home/mavinogradova/skreens/lbA10/22.png){#fig:022 width=70%}

Внутри сессии openssl тестируем подключение по telnet (`EHLO test`), проверяем аутентификацию (`AUTH PLAIN ...`) и завершаем сессию. Сервер успешно подтверждает аутентификацию (рис. [-@fig:023]).

![Проверка аутентификации через openssl](/home/mavinogradova/skreens/lbA10/23.png){#fig:023 width=70%}

Проверяем корректность отправки почтовых сообщений с клиента посредством почтового клиента Evolution. Изначально видим список входящих писем (рис. [-@fig:024]).

![Список писем в Evolution до настройки](/home/mavinogradova/skreens/lbA10/24.png){#fig:024 width=70%}

Корректируем настройки учетной записи: для SMTP-сервера указываем порт 587, STARTTLS и обычный пароль (с включенной галочкой «Server requires authentication») (рис. [-@fig:025]).

![Настройка учетной записи в Evolution](/home/mavinogradova/skreens/lbA10/25.png){#fig:025 width=70%}

После отправки тестового письма убеждаемся, что оно успешно ушло и получено (появилось во Входящих) (рис. [-@fig:026]).

![Подтверждение успешной отправки и получения в Evolution](/home/mavinogradova/skreens/lbA10/26.png){#fig:026 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`. В соответствующие подкаталоги помещаем конфигурационные файлы Dovecot и Postfix (рис. [-@fig:027]).

![Копирование конфигурационных файлов в каталог провижининга](/home/mavinogradova/skreens/lbA10/27.png){#fig:027 width=70%}

Вносим соответствующие изменения по расширенной конфигурации SMTP-сервера в файл `/vagrant/provision/server/mail.sh` (рис. [-@fig:028]).

![Редактирование скрипта provision/server/mail.sh](/home/mavinogradova/skreens/lbA10/28.png){#fig:028 width=70%}

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

cp /vagrant/provision/server/mail/etc/dovecot/dovecot.conf /etc/dovecot/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/10-master.conf /etc/dovecot/conf.d/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/10-auth.conf /etc/dovecot/conf.d/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/auth-system.conf.ext /etc/dovecot/conf.d/
cp /vagrant/provision/server/mail/etc/dovecot/conf.d/10-mail.conf /etc/dovecot/conf.d/
cp /vagrant/provision/server/mail/etc/postfix/master.cf /etc/postfix/

chown -R root:root /etc/postfix
restorecon -vR /etc

echo "Configure firewall"
firewall-cmd --add-service=smtp --permanent
firewall-cmd --add-service=pop3 --permanent
firewall-cmd --add-service=pop3s --permanent
firewall-cmd --add-service=imap --permanent
firewall-cmd --add-service=imaps --permanent
firewall-cmd --add-service=smtp-submission --permanent
firewall-cmd --reload

echo "Start services"
systemctl enable postfix
systemctl enable dovecot
systemctl start postfix
systemctl start dovecot

echo "Configure postfix"
postconf -e 'mydomain = mavinogradova.net'
postconf -e 'myorigin = $mydomain'
postconf -e 'inet_protocols = ipv4'
postconf -e 'inet_interfaces = all'
postconf -e 'mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain'
postconf -e 'home_mailbox = Maildir/'

echo "Configure postfix for auth"
postconf -e 'smtpd_sasl_type = dovecot'
postconf -e 'smtpd_sasl_path = private/auth'
postconf -e 'smtpd_recipient_restrictions = reject_unknown_recipient_domain, permit_mynetworks, reject_non_fqdn_recipient, reject_unauth_destination, reject_unverified_recipient, permit'
postconf -e 'mynetworks = 127.0.0.0/8'

echo "Configure postfix for SMTP over TLS"
cp /etc/pki/dovecot/certs/dovecot.pem /etc/pki/tls/certs
cp /etc/pki/dovecot/private/dovecot.pem /etc/pki/tls/private
postconf -e 'smtpd_tls_cert_file=/etc/pki/tls/certs/dovecot.pem'
postconf -e 'smtpd_tls_key_file=/etc/pki/tls/private/dovecot.pem'
postconf -e 'smtpd_tls_session_cache_database = btree:/var/lib/postfix/smtpd_scache'
postconf -e 'smtpd_tls_security_level = may'
postconf -e 'smtp_tls_security_level = may'

postfix set-permissions
restorecon -vR /etc

echo "Restart Postfix"
systemctl stop postfix
systemctl start postfix
systemctl restart dovecot
```

Вносим изменения в файл `/vagrant/provision/client/mail.sh`, добавив установку telnet (рис. [-@fig:029]).

![Редактирование скрипта provision/client/mail.sh](/home/mavinogradova/skreens/lbA10/29.png){#fig:029 width=70%}

Полный текст файла `/vagrant/provision/client/mail.sh`:

```bash
#!/bin/bash
echo "Provisioning script $0"

echo "Install needed packages"
dnf -y install postfix
dnf -y install s-nail
dnf -y install evolution
dnf -y install telnet

echo "Configure postfix"
postconf -e 'inet_protocols = ipv4'

echo "192.168.1.1  mail.mavinogradova.net" >> /etc/hosts

echo "Start postfix service"
systemctl enable postfix
```

# Выводы

В ходе лабораторной работы были приобретены практические навыки по конфигурированию SMTP-сервера в части настройки аутентификации. Был настроен Dovecot для работы с LMTP, что позволило организовать фильтрацию и доставку почты на стороне сервера. Настроена аутентификация SASL для SMTP-сервера (Postfix), а также проверена ее работоспособность. Сконфигурирован SMTP-сервер для безопасной работы поверх TLS (порт 587) и проверена корректность отправки почты через почтовый клиент. Все действия расширенной настройки SMTP-сервера автоматизированы с помощью скриптов Vagrant.

# Список литературы

1. Методические указания к лабораторной работе № 10 «Настройка аутентификации SMTP-сервера». (дата обращения: 02.11.2026).
2. Настройка почтового сервера Postfix + Dovecot + Multidomain + SSL + IPv6 + OpenVPN + Multi-interfaces + SpamAssassin-learn + Bind // Хабр. — URL: https://habr.com/ru/articles/413689/ (дата обращения: 02.11.2026).
