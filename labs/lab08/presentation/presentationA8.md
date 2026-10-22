---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №8
subtitle: Настройка SMTP-сервера
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 18 октября 2026

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

Приобретение практических навыков по установке и конфигурированию SMTP-сервера.

# Задание

1. Установите на виртуальной машине server SMTP-сервер postfix (см. раздел 8.4.1).
2. Сделайте первоначальную настройку postfix при помощи утилиты postconf, задав отправку писем не на локальный хост, а на сервер в домене (см. раздел 8.4.2).
3. Проверьте отправку почты с сервера и клиента (см. раздел 8.4.3).
4. Сконфигурируйте Postfix для работы в домене. Проверьте отправку почты с сервера и клиента (см. раздел 8.4.4).
5. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке Postfix во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 8.4.5).

# Выполнение лабораторной работы

## Установка Postfix. Вход на server

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину server под пользователем mavinogradova и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA8/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Установка Postfix. Установка пакетов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка пакетов postfix и s-nail на сервере](/home/mavinogradova/skreens/lbA8/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Установка Postfix. Настройка firewall и запуск

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка межсетевого экрана, восстановление контекста SELinux и запуск Postfix](/home/mavinogradova/skreens/lbA8/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Изменение параметров. Просмотр конфигурации

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр полного списка текущих параметров Postfix](/home/mavinogradova/skreens/lbA8/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Изменение параметров. myorigin и mydomain

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр и изменение параметров myorigin и mydomain, проверка конфигурации](/home/mavinogradova/skreens/lbA8/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Изменение параметров. postconf -n

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр параметров Postfix, отличных от значений по умолчанию](/home/mavinogradova/skreens/lbA8/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Изменение параметров. Задание домена и IPv4

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Жесткое задание домена, отключение IPv6 и перезагрузка конфигурации](/home/mavinogradova/skreens/lbA8/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Проверка работы. Отправка с сервера

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Отправка тестового письма с сервера](/home/mavinogradova/skreens/lbA8/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Проверка работы. Лог сервера

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Мониторинг лога maillog на сервере, подтверждение доставки письма](/home/mavinogradova/skreens/lbA8/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Проверка работы. Вход на client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину client и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA8/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Проверка работы. Установка на client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка пакетов postfix и s-nail на клиенте](/home/mavinogradova/skreens/lbA8/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Проверка работы. Настройка на client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv4 и запуск Postfix на клиенте](/home/mavinogradova/skreens/lbA8/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Проверка работы. Отправка с client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Отправка письма с клиента](/home/mavinogradova/skreens/lbA8/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Проверка работы. Ожидание в очереди

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка очереди на клиенте: письмо ожидает отправки](/home/mavinogradova/skreens/lbA8/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Проверка работы. Настройка сети на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка сетевых интерфейсов и доверенных сетей на сервере](/home/mavinogradova/skreens/lbA8/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Проверка работы. Повторная отправка

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Повторная отправка письма с клиента после настройки сети](/home/mavinogradova/skreens/lbA8/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Проверка работы. Успешная доставка

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Логи сервера и клиента, подтверждающие успешную доставку письма](/home/mavinogradova/skreens/lbA8/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Отправка на домен

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Отправка письма на доменный адрес с клиента](/home/mavinogradova/skreens/lbA8/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Ожидание в очереди

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Логи сервера и клиента при отправке письма на доменный адрес](/home/mavinogradova/skreens/lbA8/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Проверка очереди

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка очереди на сервере и клиенте](/home/mavinogradova/skreens/lbA8/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Прямая зона

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование файла прямой DNS-зоны с добавлением MX-записи](/home/mavinogradova/skreens/lbA8/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Обратная зона

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование файла обратной DNS-зоны](/home/mavinogradova/skreens/lbA8/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Конфигурация для домена. mydestination и DNS

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка mydestination, перезагрузка Postfix и DNS](/home/mavinogradova/skreens/lbA8/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Проталкивание очереди

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проталкивание очереди и проверка её состояния на клиенте](/home/mavinogradova/skreens/lbA8/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Конфигурация для домена. Успешная доставка

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Лог сервера, подтверждающий успешную доставку письма на доменный адрес](/home/mavinogradova/skreens/lbA8/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Окружение ВМ. Создание mail.sh на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание исполняемого файла mail.sh в каталоге провижининга сервера](/home/mavinogradova/skreens/lbA8/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Окружение ВМ. Содержимое mail.sh на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое скрипта provision/server/mail.sh](/home/mavinogradova/skreens/lbA8/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Окружение ВМ. Создание mail.sh на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание исполняемого файла mail.sh в каталоге провижининга клиента](/home/mavinogradova/skreens/lbA8/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Окружение ВМ. Содержимое mail.sh на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое скрипта provision/client/mail.sh](/home/mavinogradova/skreens/lbA8/29.png){#fig:029 width=70%}

:::
::::::::::::::

## Окружение ВМ. Правка Vagrantfile (client)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с добавленным блоком провижининга client mail](/home/mavinogradova/skreens/lbA8/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Окружение ВМ. Правка Vagrantfile (server)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с добавленным блоком провижининга server mail](/home/mavinogradova/skreens/lbA8/31.png){#fig:031 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и конфигурированию SMTP-сервера. Был установлен и настроен Postfix на сервере и клиенте, изучены основные параметры конфигурации (`mydomain`, `myorigin`, `mydestination`, `inet_interfaces`, `mynetworks`). Проверена работа почтовой службы: отправка писем с сервера и клиента, анализ логов доставки и очереди сообщений. Настроены DNS-зоны с MX-записями для корректной работы доменной почты. Автоматизированы действия по установке и настройке Postfix с помощью скриптов провижининга для Vagrant.
