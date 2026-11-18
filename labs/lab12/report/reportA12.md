---
## Front matter
title: "Отчет по лабораторной работе № 12"
subtitle: "Синхронизация времени"
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

Получение навыков по управлению системным временем и настройке синхронизации времени.

# Задание

1. Изучите команды по настройке параметров времени (см. раздел 12.4.1).
2. Настройте сервер в качестве сервера синхронизации времени для локальной сети (см. раздел 12.4.2).
3. Напишите скрипты для Vagrant, фиксирующие действия по установке и настройке NTP-сервера и клиента (см. раздел 12.4.3).

# Выполнение лабораторной работы

## Настройка параметров времени

Выполнение работы производится в соответствии с методическими указаниями [1]. Проверяем статус виртуальных машин: `vagrant status` (рис. [-@fig:001]).

![Проверка статуса виртуальных машин](/home/mavinogradova/skreens/lbA12/1.png){#fig:001 width=70%}

На сервере смотрим параметры настройки даты и времени: `timedatectl` (рис. [-@fig:002]). Определяем, что сервер находится во временной зоне UTC, сетевая синхронизация времени проводится (`System clock synchronized: yes`), служба NTP активна (`NTP service: active`).

![Вывод параметров времени на сервере](/home/mavinogradova/skreens/lbA12/2.png){#fig:002 width=70%}

На клиенте смотрим параметры настройки даты и времени: `timedatectl` (рис. [-@fig:003]). Клиент находится в той же временной зоне UTC, синхронизация включена.

![Вывод параметров времени на клиенте](/home/mavinogradova/skreens/lbA12/3.png){#fig:003 width=70%}

Поэкспериментируем с параметрами команды на клиенте, изменив часовой пояс на `Europe/Moscow` с помощью `timedatectl set-timezone Europe/Moscow` и проверив результат (рис. [-@fig:004]).

![Смена часового пояса на клиенте](/home/mavinogradova/skreens/lbA12/4.png){#fig:004 width=70%}

На сервере посмотрим список доступных часовых поясов: `timedatectl list-timezones` (рис. [-@fig:005]).

![Просмотр списка доступных часовых поясов](/home/mavinogradova/skreens/lbA12/5.png){#fig:005 width=70%}

На клиенте посмотрим текущее системное время и поэкспериментируем с параметрами команды: `date`, `date +"%Y-%m-%d %H:%M:%S %Z"` (рис. [-@fig:006]).

![Просмотр системного времени на клиенте](/home/mavinogradova/skreens/lbA12/6.png){#fig:006 width=70%}

Аналогично на сервере: `date`, `date +"%Y-%m-%d %H:%M:%S %Z"` (рис. [-@fig:007]).

![Просмотр системного времени на сервере](/home/mavinogradova/skreens/lbA12/7.png){#fig:007 width=70%}

На сервере смотрим аппаратное время: `hwclock --show` (рис. [-@fig:008]). В виртуальной среде прямой доступ к аппаратным часам ограничен, поэтому система выдает соответствующее уведомление.

![Попытка просмотра аппаратного времени на сервере](/home/mavinogradova/skreens/lbA12/8.png){#fig:008 width=70%}

На клиенте смотрим аппаратное время: `hwclock --show` (рис. [-@fig:009]). Выдается аналогичное уведомление.

![Попытка просмотра аппаратного времени на клиенте](/home/mavinogradova/skreens/lbA12/9.png){#fig:009 width=70%}

## Управление синхронизацией времени

На сервере устанавливаем необходимое программное обеспечение: `sudo dnf -y install chrony` (рис. [-@fig:010]).

![Установка пакета chrony на сервере](/home/mavinogradova/skreens/lbA12/10.png){#fig:010 width=70%}

Проверяем источники времени на сервере: `chronyc sources` (рис. [-@fig:011]). В столбце `S` отображается знак `?`, что означает отсутствие первоначальной синхронизации с этими часами.

![Проверка источников времени на сервере](/home/mavinogradova/skreens/lbA12/11.png){#fig:011 width=70%}

Проверяем источники времени на клиенте: `chronyc sources` (рис. [-@fig:012]). В столбце `S` виден знак `*`, что означает сервер, с которым в настоящий момент синхронизирован хост.

![Проверка источников времени на клиенте](/home/mavinogradova/skreens/lbA12/12.png){#fig:012 width=70%}

На сервере открываем на редактирование файл `/etc/chrony.conf` и добавляем строку: `allow 192.168.0.0/16` (рис. [-@fig:013]).

![Редактирование конфигурационного файла chrony.conf на сервере](/home/mavinogradova/skreens/lbA12/13.png){#fig:013 width=70%}

Настраиваем межсетевой экран на сервере: `firewall-cmd --add-service=ntp --permanent`, `firewall-cmd --reload` (рис. [-@fig:014]).

![Настройка межсетевого экрана на сервере](/home/mavinogradova/skreens/lbA12/14.png){#fig:014 width=70%}

На клиенте открываем файл `/etc/chrony.conf` и добавляем строку (вместо user указываем свой логин): `server server.mavinogradova.net iburst` (рис. [-@fig:015]).

![Редактирование конфигурационного файла chrony.conf на клиенте](/home/mavinogradova/skreens/lbA12/15.png){#fig:015 width=70%}

После перезапуска службы проверяем источники времени на сервере: `chronyc sources` (рис. [-@fig:016]).

![Проверка источников времени на сервере после настройки](/home/mavinogradova/skreens/lbA12/16.png){#fig:016 width=70%}

Проверяем источники времени на клиенте: `chronyc sources` (рис. [-@fig:017]). В отчёте поясняем: в строке `server.user.net` стоит символ `*`, что означает, что клиент успешно синхронизировался с локальным сервером.

![Проверка источников времени на клиенте после настройки](/home/mavinogradova/skreens/lbA12/17.png){#fig:017 width=70%}

Смотрим подробную информацию о синхронизации на сервере: `chronyc tracking` (рис. [-@fig:018]). Поясняем: `Reference ID` указывает на внешний сервер, `Stratum` равен 2, `System time` показывает микроскопическое отклонение [2].

![Просмотр подробной информации о синхронизации на сервере](/home/mavinogradova/skreens/lbA12/18.png){#fig:018 width=70%}

Смотрим подробную информацию о синхронизации на клиенте: `chronyc tracking` (рис. [-@fig:019]). Поясняем: `Reference ID` указывает на наш сервер, `Stratum` равен 3 [2].

![Просмотр подробной информации о синхронизации на клиенте](/home/mavinogradova/skreens/lbA12/19.png){#fig:019 width=70%}

## Внесение изменений в настройки внутреннего окружения виртуальных машин

На виртуальной машине server переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/server/`, создаем в нём каталог ntp, в который помещаем в соответствующие подкаталоги конфигурационные файлы (рис. [-@fig:020]).

![Подготовка окружения на сервере](/home/mavinogradova/skreens/lbA12/20.png){#fig:020 width=70%}

В каталоге `/vagrant/provision/server` создаем исполняемый файл `ntp.sh` и прописываем в нём скрипт (рис. [-@fig:021]).

![Редактирование скрипта ntp.sh для сервера](/home/mavinogradova/skreens/lbA12/21.png){#fig:021 width=70%}

Полный текст файла `/vagrant/provision/server/ntp.sh`:

~~~bash
#!/bin/bash
echo "Provisioning script $0"
echo "Install needed packages"
dnf -y install chrony
echo "Copy configuration files"
cp -R /vagrant/provision/server/ntp/etc/* /etc
restorecon -vR /etc
echo "Configure firewall"
firewall-cmd --add-service=ntp
firewall-cmd --add-service=ntp --permanent
echo "Restart chronyd service"
systemctl restart chronyd
~~~

На виртуальной машине client переходим в каталог для внесения изменений в настройки внутреннего окружения `/vagrant/provision/client/`, создаем в нём каталог ntp, в который помещаем в соответствующие подкаталоги конфигурационные файлы (рис. [-@fig:022]).

![Подготовка окружения на клиенте](/home/mavinogradova/skreens/lbA12/22.png){#fig:022 width=70%}

В каталоге `/vagrant/provision/client` создаем исполняемый файл `ntp.sh` и прописываем в нём скрипт (рис. [-@fig:023]).

![Редактирование скрипта ntp.sh для клиента](/home/mavinogradova/skreens/lbA12/23.png){#fig:023 width=70%}

Полный текст файла `/vagrant/provision/client/ntp.sh`:

~~~bash
#!/bin/bash
echo "Provisioning script \$0"
echo "Copy configuration files"
cp -R /vagrant/provision/client/ntp/etc/* /etc
restorecon -vR /etc
echo "Restart chronyd service"
systemctl restart chronyd
~~~

Для отработки созданных скриптов во время загрузки виртуальных машин server и client в конфигурационном файле Vagrantfile добавляем в соответствующих разделах конфигураций для сервера и клиента соответствующие строки (рис. [-@fig:024], рис. [-@fig:025]).

![Добавление скрипта для сервера в Vagrantfile](/home/mavinogradova/skreens/lbA12/24.png){#fig:024 width=70%}

![Добавление скрипта для клиента в Vagrantfile](/home/mavinogradova/skreens/lbA12/25.png){#fig:025 width=70%}

# Выводы

В ходе лабораторной работы были получены навыки по управлению системным временем и настройке синхронизации времени. Были изучены команды `timedatectl`, `date` и `hwclock` для управления системным и аппаратным временем. Сервер был настроен в качестве сервера синхронизации времени для локальной сети с использованием пакета `chrony`. Была выполнена настройка клиента для синхронизации с локальным сервером. Все действия по установке и настройке NTP-сервера и клиента автоматизированы с помощью скриптов Vagrant.

# Ответы на контрольные вопросы

1. **Почему важна точная синхронизация времени для служб баз данных?**
Точная синхронизация времени критически важна для баз данных, так как от неё зависит порядок транзакций, корректность временных меток в журналах транзакций, а также процессы репликации и восстановления после сбоев. Рассинхронизация может привести к конфликтам данных и нарушению целостности [2].

2. **Почему служба проверки подлинности Kerberos сильно зависит от правильной синхронизации времени?**
Kerberos использует временные метки в тикетах аутентификации для защиты от атак повторного воспроизведения (replay attacks). Если время на клиенте и сервере расходится более чем на допустимый интервал (обычно 5 минут), сервер отклонит запрос на аутентификацию [2].

3. **Какая служба используется по умолчанию для синхронизации времени на RHEL 7?**
На RHEL 7 по умолчанию используется служба `chronyd` (пакет `chrony`) [1].

4. **Какова страта по умолчанию для локальных часов?**
Страта локальных часов (когда нет внешней синхронизации) по умолчанию равна 10 [1].

5. **Какой порт брандмауэра должен быть открыт, если вы настраиваете свой сервер как одноранговый узел NTP?**
Должен быть открыт порт 123/UDP (служба `ntp`) [1].

6. **Какую строку вам нужно включить в конфигурационный файл chrono, если вы хотите быть сервером времени, даже если внешние серверы NTP недоступны?**
Необходимо добавить строку `local stratum 10` в конфигурационный файл `/etc/chrony.conf` [1].

7. **Какую страту имеет хост, если нет текущей синхронизации времени NTP?**
Если текущей синхронизации нет, хост имеет страту 16 (считается несинхронизированным) [1].

8. **Какую команду вы бы использовали на сервере с chrony, чтобы узнать, с какими серверами он синхронизируется?**
Команда `chronyc sources` выводит список серверов, с которыми проводится синхронизация [1].

9. **Как вы можете получить подробную статистику текущих настроек времени для процесса chrony вашего сервера?**
Для получения подробной статистики используется команда `chronyc tracking`. Дополнительно можно использовать `chronyc sourcestats` для просмотра статистики по источникам [2].

# Список литературы

1. Методические указания к лабораторной работе № 12 «Синхронизация времени». (дата обращения: 16.11.2026).
2. Документация Chrony (руководство пользователя) // Перевод официальной документации chrony. — URL: https://chrony.tuxfamily.org/doc/4.2/chronyc.html (дата обращения: 16.11.2026).
