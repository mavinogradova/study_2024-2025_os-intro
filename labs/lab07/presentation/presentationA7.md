---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №7
subtitle: Расширенные настройки межсетевого экрана
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 17 октября 2026

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
toc-title: Содержание
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Информация

## Докладчик

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

  * Виноградова Мария Андреевна
  * студентка НПИбд-02-24
  * номер студ. билета 1132240691
  * Российский университет дружбы народов
  * [1132240691@rudn.ru](mailto:1132240691@rudn.ru)
  * <https://github.com/mavinogradova/study_2026-2027>

:::
::::::::::::::

# Цель работы

Получить навыки настройки межсетевого экрана в Linux в части переадресации портов и настройки Masquerading.

# Задание

1. Настройте межсетевой экран виртуальной машины server для доступа к серверу по протоколу SSH не через 22-й порт, а через порт 2022 (см. разделы 7.4.1 и 7.4.2).
2. Настройте Port Forwarding на виртуальной машине server (см. разделы 7.4.3).
3. Настройте маскарадинг на виртуальной машине server для организации доступа клиента к сети Интернет (см. раздел 7.4.3).
4. Напишите скрипт для Vagrant, фиксирующий действия по расширенной настройке межсетевого экрана. Соответствующим образом внести изменения в Vagrantfile (см. раздел 7.4.4).

# Выполнение лабораторной работы

## Создание службы firewall. Вход на server

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину server под пользователем mavinogradova и переход в режим суперпользователя](/home/mavinogradova/skreens/LbA7/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Создание службы firewall. Копирование ssh.xml

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание файла ssh-custom.xml и просмотр его исходного содержимого](/home/mavinogradova/skreens/LbA7/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Создание службы firewall. Редактирование ssh-custom.xml

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование файла ssh-custom.xml: изменённый порт 2022 и модифицированное описание](/home/mavinogradova/skreens/LbA7/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Создание службы firewall. Список служб до и после reload

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр списка служб FirewallD до и после перезагрузки правил](/home/mavinogradova/skreens/LbA7/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Создание службы firewall. Добавление ssh-custom

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление службы ssh-custom в FirewallD и проверка списка активных служб](/home/mavinogradova/skreens/LbA7/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Перенаправление портов. Организация проброса

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Организация переадресации с порта 2022 на порт 22 и проверка правила](/home/mavinogradova/skreens/LbA7/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Перенаправление портов. Проверка с клиента

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подключение к серверу по SSH через порт 2022 с клиента](/home/mavinogradova/skreens/LbA7/7.png){#fig:007 width=70%}

:::
::::::::::::::

## NAT. Проверка ip_forward

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка активации перенаправления IPv4-пакетов в ядре](/home/mavinogradova/skreens/LbA7/8.png){#fig:008 width=70%}

:::
::::::::::::::

## NAT. Включение ip_forward

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Включение перенаправления IPv4-пакетов на сервере](/home/mavinogradova/skreens/LbA7/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Masquerading. Включение

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Включение маскарадинга на сервере и проверка статуса](/home/mavinogradova/skreens/LbA7/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Masquerading. Проверка с клиента

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка доступности выхода в Интернет на клиенте](/home/mavinogradova/skreens/LbA7/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Окружение ВМ. Копирование файлов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание каталога firewall и копирование конфигурационных файлов в каталог провижининга](/home/mavinogradova/skreens/LbA7/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Окружение ВМ. Создание firewall.sh

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание исполняемого файла firewall.sh](/home/mavinogradova/skreens/LbA7/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Окружение ВМ. Содержимое firewall.sh

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое скрипта провижининга firewall.sh](/home/mavinogradova/skreens/LbA7/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Окружение ВМ. Правка Vagrantfile

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с добавленным блоком провижининга server firewall](/home/mavinogradova/skreens/LbA7/15.png){#fig:015 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были получены навыки настройки межсетевого экрана в Linux в части переадресации портов и настройки Masquerading. Создана пользовательская служба FirewallD `ssh-custom` с портом 2022 на основе существующего описания службы ssh, служба успешно добавлена в FirewallD. Организована переадресация с порта 2022 на порт 22, проверено SSH-подключение к серверу через порт 2022 с клиента. Включено перенаправление IPv4-пакетов в ядре и маскарадинг в зоне public, проверен выход клиента в сеть Интернет. Написан скрипт провижининга `firewall.sh`, автоматизирующий расширенную настройку межсетевого экрана, и внесены соответствующие изменения в `Vagrantfile`.
