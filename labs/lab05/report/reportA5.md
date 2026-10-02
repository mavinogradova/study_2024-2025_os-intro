---
## Front matter
title: "Отчет по лабораторной работе № 5"
subtitle: "Расширенная настройка HTTP-сервера Apache"
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

Приобретение практических навыков по расширенному конфигурированию HTTP-сервера Apache в части безопасности и возможности использования PHP.

# Задание

1. Сгенерируйте криптографический ключ и самоподписанный сертификат безопасности для возможности перехода веб-сервера от работы через протокол HTTP к работе через протокол HTTPS (см. раздел 5.4.1).
2. Настройте веб-сервер для работы с PHP (см. раздел 5.4.2).
3. Напишите (или скорректируйте) скрипт для Vagrant, фиксирующий действия по расширенной настройке HTTP-сервера во внутреннем окружении виртуальной машины server (см. раздел 5.4.3).

# Выполнение лабораторной работы

## Конфигурирование HTTP-сервера для работы через протокол HTTPS

Загружаем операционную систему и переходим в рабочий каталог с проектом. Запускаем виртуальную машину server. На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя. В каталоге `/etc/ssl` создаём каталог `private`, создаём символическую ссылку `/etc/ssl/private` на `/etc/pki/tls/private` и переходим в каталог `/etc/pki/tls/private` (рис. [-@fig:001]).

![Вход на виртуальную машину server под пользователем mavinogradova, переход в режим суперпользователя, создание каталога /etc/pki/tls/private и символической ссылки /etc/ssl/private](/home/mavinogradova/skreens/lbA5/1.png){#fig:001 width=70%}

Сгенерируем ключ и сертификат.

В этой строке параметр `-req -x509` означает, что используется запрос подписи сертификата x509 (CSR); параметр `-nodes` указывает OpenSSL, что нужно пропустить шифрование сертификата SSL с использованием парольной фразы, т.е. позволить Apache читать файл без какого-либо вмешательства пользователя; параметр `-newkey rsa:2048` указывает, что одновременно создаются новый ключ и новый сертификат, причём используется 2048-битный ключ RSA; параметр `-keyout` указывает, где хранить сгенерированный файл закрытого ключа при создании; параметр `-out` указывает, где разместить созданный сертификат SSL.

Далее заполняем поля сертификата:
- в строке кода страны указываем `RU`;
- в строке названия страны указываем `Russia`;
- в строке названия города указываем `Moscow`;
- в строке названия организации указываем `mavinogradova`;
- в строке названия подразделения указываем `mavinogradova`;
- в строке названия хоста указываем `www.mavinogradova.net`;
- в строке e-mail адреса указываем `mavinogradova@mavinogradova.net`.

Сгенерированные ключ и сертификат появляются в соответствующем каталоге `/etc/ssl/private`. Перемещаем сертификат в каталог `/etc/pki/tls/certs`. (рис.[-@fig:002]).

![Генерация ключа и сертификата командой openssl req, заполнение полей сертификата и перемещение сертификата в каталог /etc/pki/tls/certs](/home/mavinogradova/skreens/lbA5/2.png){#fig:002 width=70%}

Для перехода веб-сервера www.mavinogradova.net на функционирование через протокол HTTPS требуется изменить его конфигурационный файл. Переходим в каталог `/etc/httpd/conf.d` и открываем на редактирование файл `/etc/httpd/conf.d/www.mavinogradova.net.conf`, заменяя его содержимое на следующее (рис. [-@fig:003]):

```
<VirtualHost *:80>
ServerAdmin webmaster@mavinogradova.net
DocumentRoot /var/www/html/www.mavinogradova.net
ServerName www.mavinogradova.net
ServerAlias www.mavinogradova.net
ErrorLog logs/www.mavinogradova.net-error_log
CustomLog logs/www.mavinogradova.net-access_log common
RewriteEngine on
RewriteRule ^(.*)$ https://%{HTTP_HOST}$1 [R=301,L]
</VirtualHost>
<IfModule mod_ssl.c>
<VirtualHost *:443>
SSLEngine on
ServerAdmin webmaster@mavinogradova.net
DocumentRoot /var/www/html/www.mavinogradova.net
ServerName www.mavinogradova.net
ServerAlias www.mavinogradova.net
ErrorLog logs/www.mavinogradova.net-error_log
CustomLog logs/www.mavinogradova.net-access_log common
SSLCertificateFile /etc/ssl/certs/www.mavinogradova.net.crt
SSLCertificateKeyFile /etc/ssl/private/www.mavinogradova.net.key
</VirtualHost>
</IfModule>
```

В конфигурационном файле заданы два виртуальных хоста. Первый — на порту 80 — обслуживает HTTP-запросы: в нём включён механизм перезаписи URL (`RewriteEngine on`), а правило `RewriteRule` перенаправляет все запросы на HTTPS с кодом 301 (постоянное перенаправление), что обеспечивает автоматическое переключение на защищённое соединение. Второй виртуальный хост — на порту 443 — обслуживает HTTPS-запросы: директива `SSLEngine on` включает шифрование SSL/TLS, а директивы `SSLCertificateFile` и `SSLCertificateKeyFile` указывают пути к файлу сертификата и файлу закрытого ключа соответственно. Оба виртуальных хоста используют один и тот же корневой каталог `/var/www/html/www.mavinogradova.net`, то есть контент сайта одинаково доступен и по HTTP, и по HTTPS. Второй виртуальный хост обёрнут в блок `<IfModule mod_ssl.c>`, поэтому он подключается только если загружен модуль SSL.

![Содержимое файла /etc/httpd/conf.d/www.mavinogradova.net.conf](/home/mavinogradova/skreens/lbA5/3.png){#fig:003 width=70%}

Вносим изменения в настройки межсетевого экрана на сервере, разрешив работу с https. Просматриваем список активных служб.

В списке присутствуют cockpit, dhcp, dhcpv6-client, dns, http, ssh; службы https пока нет. Просматриваем список всех доступных служб.

Служба https в списке присутствует. Добавляем службу https в runtime-конфигурацию и в permanent-конфигурацию:
oбе команды возвращают success. Применяем изменения - также success. Перезапускаем веб-сервер. (рис. [-@fig:004]).

![Настройка межсетевого экрана: добавление службы https и перезапуск веб-сервера](/home/mavinogradova/skreens/lbA5/4.png){#fig:004 width=70%}

Для проверки корректности работы HTTPS и просмотра содержания сертификата подключаемся к виртуальной машине client. При попытке подключения командой `vagrant ssh client` Vagrant выводит предупреждение о том, что машина настроена на аутентификацию по паролю, и Vagrant не может автоматически передать пароль. Подключение к client выполняется по SSH напрямую (рис. [-@fig:005]).

![Подключение к виртуальной машине client по SSH](/home/mavinogradova/skreens/lbA5/5.png){#fig:005 width=70%}

## Анализ работы HTTP-сервера через HTTPS

На виртуальной машине client проверяем корректность работы HTTPS и просматриваем содержание сертификата. 

В ответ на команду сервер возвращает `HTTP/1.1 301 Moved Permanently` с заголовком `Location: https://www.mavinogradova.net/` и заголовком `Server: Apache/2.4.63 (Rocky Linux) OpenSSL/3.5.5 mod_fcgid/2.3.9`, что подтверждает автоматическое переключение на работу по протоколу HTTPS.

Ответом на команду сервер отдаёт страницу «Welcome to the www.mavinogradova.net server». Для просмотра содержания сертификата выполняем команду и она выводит поля subject и issuer сертификата (`C=RU, ST=Russia, L=Moscow, O=mavinogradova, OU=mavinogradova, CN=www.mavinogradova.net, emailAddress=mavinogradova@mavinogradova.net`), а также даты начала и окончания действия сертификата notBefore (Sep 30 20:54:05 2026 GMT) и notAfter (Oct 30 20:54:05 2026 GMT). Поскольку поля subject и issuer совпадают, сертификат является самоподписанным, то есть выдан самим веб-сервером. Результат представлен на рис. [-@fig:006].

![Проверка корректности работы HTTPS и просмотр содержания сертификата](/home/mavinogradova/skreens/lbA5/6.png){#fig:006 width=70%}

## Конфигурирование HTTP-сервера для работы с PHP

Устанавливаем пакеты для работы с PHP:
Вместе с пакетом php версии 8.3.33 устанавливаются зависимости: capstone, nginx-filesystem, php-common, а также слабые зависимости php-cli, php-fpm, php-mbstring, php-opcache. Все пакеты устанавливаются из репозиториев Rocky Linux 10 (BaseOS, AppStream, CRB, Extras) (рис. [-@fig:007]).

![Установка пакета PHP командой dnf -y install php](/home/mavinogradova/skreens/lbA5/7.png){#fig:007 width=70%}

В каталоге `/var/www/html/www.mavinogradova.net` заменяем файл `index.html` на `index.php`. Переходим в каталог: /var/www/html/www.mavinogradova.net. Там удаляем старый файл и открываем на редактирование новый.(рис. [-@fig:008]).

![Замена файла index.html на index.php в каталоге сайта](/home/mavinogradova/skreens/lbA5/8.png){#fig:008 width=70%}

Вносим в файл `index.php` следующее содержание (рис. [-@fig:009]):

```
<?php
phpinfo();
?>
```

Функция `phpinfo()` выводит страницу с полной информацией о версии PHP, его настройках, установленных модулях и параметрах конфигурации. Это позволяет убедиться, что PHP корректно работает на веб-сервере.

![Содержимое файла index.php](/home/mavinogradova/skreens/lbA5/9.png){#fig:009 width=70%}

Скорректируем права доступа в каталог с веб-контентом: это необходимо, чтобы Apache мог читать файлы сайта. Восстановим контекст безопасности в SELinux: команда `restorecon` возвращает стандартные метки безопасности для файлов и каталогов, изменённые при копировании или редактировании; при выполнении первой команды был выполнен relabel для файла `/etc/NetworkManager/system-connections/eth1.nmconnection`. 
После этого перезапустим HTTP-сервер и проверим его статус: Статус — `active (running)`, в блоке `Drop-In` видно подключение `php-fpm.conf`, что означает, что Apache обрабатывает PHP-файлы через PHP-FPM (FastCGI Process Manager) (рис. [-@fig:010]).

![Корректировка прав доступа, восстановление SELinux-контекстов и перезапуск HTTP-сервера](/home/mavinogradova/skreens/lbA5/10.png){#fig:010 width=70%}

На виртуальной машине client проверяем работу PHP.

Сервер отдаёт HTML-страницу `phpinfo()` с информацией об используемой на веб-сервере версии PHP. В частности, в теге `<title>` видно `PHP 8.3.33 - phpinfo()`, что подтверждает корректную работу PHP (рис. [-@fig:011]).

![Проверка работы PHP: страница phpinfo() с информацией о версии PHP](/home/mavinogradova/skreens/lbA5/11.png){#fig:011 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальной машины

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/http` и в соответствующие каталоги копируем конфигурационные файлы.

В результате выполнения команд конфигурационный файл виртуального хоста `www.mavinogradova.net.conf`, содержимое каталога `/var/www/html` (включая `index.php`), а также файлы сертификата `www.mavinogradova.net.crt` и закрытого ключа `www.mavinogradova.net.key` скопированы в соответствующие каталоги провижининга (рис. [-@fig:012]).

![Копирование конфигурационных файлов, сертификата и ключа в каталог провижининга](/home/mavinogradova/skreens/lbA5/12.png){#fig:012 width=70%}

В имеющийся скрипт `/vagrant/provision/server/http.sh` внесены изменения: добавлена установка PHP и настройка межсетевого экрана, разрешающая работу с https. Скрипт теперь содержит следующие команды (рис. [-@fig:013]):


```
#!/bin/bash
echo "Provisioning script $0"
echo "Install needed packages"
dnf -y groupinstall "Basic Web Server"
dnf -y install php
echo "Copy configuration files"
cp -R /vagrant/provision/server/http/etc/httpd/* /etc/httpd
cp -R /vagrant/provision/server/http/var/www/* /var/www
chown -R apache:apache /var/www
restorecon -vR /etc
restorecon -vR /var/www
echo "Configure firewall"
firewall-cmd --add-service=http
firewall-cmd --add-service=http --permanent
firewall-cmd --add-service=https
firewall-cmd --add-service=https --permanent
firewall-cmd --reload
echo "Start http service"
systemctl enable httpd
systemctl start httpd
```


Скрипт `http.sh` выполняет следующие действия: выводит сообщение о запуске, устанавливает группу пакетов «Basic Web Server» и пакет php, копирует конфигурационные файлы из каталога провижининга в `/etc/httpd` и `/var/www`, меняет владельца каталога `/var/www` на `apache:apache`, восстанавливает SELinux-контексты, добавляет службы http и https в firewall (runtime и permanent), применяет изменения firewall, включает и запускает службу httpd.

![Содержимое скрипта провижининга http.sh с добавленной установкой PHP и настройкой https](/home/mavinogradova/skreens/lbA5/13.png){#fig:013 width=70%}


## Выводы

В ходе лабораторной работы были приобретены практические навыки по расширенному конфигурированию HTTP-сервера Apache в части безопасности и возможности использования PHP. Был сгенерирован криптографический ключ и самоподписанный сертификат безопасности `www.mavinogradova.net.crt` с использованием команды `openssl req -x509 -nodes -newkey rsa:2048`; заполнены поля сертификата: код страны `RU`, название страны `Russia`, город `Moscow`, организация `mavinogradova`, подразделение `mavinogradova`, имя хоста `www.mavinogradova.net`, e-mail `mavinogradova@mavinogradova.net`. Это обеспечило возможность перехода веб-сервера от работы через протокол HTTP к работе через протокол HTTPS. В конфигурационном файле `/etc/httpd/conf.d/www.mavinogradova.net.conf` были созданы два виртуальных хоста: один на порту 80 с автоматическим редиректом на HTTPS (правило `RewriteRule` с кодом 301), другой — на порту 443 с включённым SSL (`SSLEngine on`) и указанием путей к сертификату и закрытому ключу. Настроен межсетевой экран: служба https добавлена в runtime и permanent конфигурации firewalld, применены изменения командой `firewall-cmd --reload`. Проверка на виртуальной машине client подтвердила корректность настройки: `curl -I http://www.mavinogradova.net` возвращает 301 с перенаправлением на https, `curl -k https://www.mavinogradova.net` отдаёт содержимое страницы, а `openssl s_client` подтверждает корректное содержание сертификата (поля subject и issuer, даты notBefore и notAfter). Веб-сервер настроен для работы с PHP: установлен пакет php версии 8.3.33, файл `index.html` заменён на `index.php` с вызовом `phpinfo()`, скорректированы права доступа (`chown -R apache:apache /var/www`) и восстановлены SELinux-контексты. Проверка через `curl -k https://www.mavinogradova.net` подтвердила вывод страницы с информацией о версии PHP. В скрипт Vagrant `http.sh` внесены изменения: добавлена установка PHP командой `dnf -y install php` и настройка межсетевого экрана для работы с https (`firewall-cmd --add-service=https` с `--permanent` и `--reload`).

# Ответы на контрольные вопросы

1. **В чём отличие HTTP от HTTPS?** HTTP (HyperText Transfer Protocol) — протокол передачи гипертекста, работающий по порту 80 без шифрования. HTTPS (HyperText Transfer Protocol Secure) — расширение протокола HTTP для поддержки шифрования в целях повышения безопасности, работающее по порту 443. Улучшение безопасности при использовании HTTPS вместо HTTP достигается за счёт использования криптографических протоколов при организации HTTP-соединения и передачи по нему данных. Для шифрования может применяться протокол SSL (Secure Sockets Layer) или протокол TLS (Transport Layer Security). Оба протокола используют асимметричное шифрование для аутентификации, симметричное шифрование для конфиденциальности и коды аутентичности сообщений для сохранения целостности сообщений.

2. **Каким образом обеспечивается безопасность контента веб-сервера при работе через HTTPS?** Безопасность контента веб-сервера при работе через HTTPS обеспечивается за счёт использования криптографических протоколов SSL/TLS при организации HTTP-соединения и передачи по нему данных. При этом применяется асимметричное шифрование для аутентификации (пара ключей — открытый и закрытый), симметричное шифрование для конфиденциальности (один и тот же криптографический ключ для шифрования и дешифрования данных) и коды аутентичности сообщений для сохранения целостности сообщений. Открытый ключ известен, передаётся по открытому каналу и используется для аутентификации пользователей и для шифрования передаваемых данных. Закрытый ключ хранится втайне на стороне получателя шифрованного сообщения; при помощи закрытого ключа сообщение дешифруется и подтверждается подлинность отправителя. Основной характеристикой криптостойкости ключа является его длина: для симметричных алгоритмов рекомендуемая минимальная длина ключа — 128 бит, для асимметричных — 1024 бит.

3. **Что такое сертификационный центр? Приведите пример.** Сертификат открытого ключа — документ (электронный или бумажный), содержащий как сам открытый ключ, так и информацию о его владельце и области применения. Сертификат подписывается выдавшим его сертификационным центром, который подтверждает принадлежность открытого ключа владельцу. Сертификационный центр (Certification authority, CA) представляет собой компонент глобальной службы каталогов, отвечающий за управление криптографическими ключами пользователей. Его открытый ключ широко известен общественности и не вызывает сомнений в подлинности. Примером сертификационного центра может служить Let's Encrypt — бесплатный автоматизированный центр сертификации, выпускающий сертификаты для веб-сайтов; также можно привести в пример коммерческие удостоверяющие центры, такие как GlobalSign, DigiCert, Comodo.

# Список литературы

1. Методические указания к лабораторной работе № 5 «Расширенная настройка HTTP-сервера Apache».
2. Официальная документация Apache HTTP Server. — URL: https://httpd.apache.org/docs/
3. Официальная документация OpenSSL. — URL: https://www.openssl.org/docs/
4. Официальная документация PHP. — URL: https://www.php.net/docs.php
5. Официальная документация Rocky Linux. — URL: https://docs.rockylinux.org/
6. Официальная документация Vagrant. — URL: https://developer.hashicorp.com/vagrant/docs
