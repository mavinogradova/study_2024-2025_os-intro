---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №12
subtitle: Синхронизация времени
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 16 ноября 2026

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

Получение навыков по управлению системным временем и настройке синхронизации времени.

# Задание

1. Изучите команды по настройке параметров времени (см. раздел 12.4.1).
2. Настройте сервер в качестве сервера синхронизации времени для локальной сети (см. раздел 12.4.2).
3. Напишите скрипты для Vagrant, фиксирующие действия по установке и настройке NTP-сервера и клиента (см. раздел 12.4.3).

# Выполнение лабораторной работы

## Статус виртуальных машин

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка статуса виртуальных машин](/home/mavinogradova/skreens/lbA12/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Параметры времени на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод параметров времени на сервере](/home/mavinogradova/skreens/lbA12/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Параметры времени на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод параметров времени на клиенте](/home/mavinogradova/skreens/lbA12/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Смена часового пояса на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Смена часового пояса на клиенте](/home/mavinogradova/skreens/lbA12/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Список часовых поясов на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр списка доступных часовых поясов](/home/mavinogradova/skreens/lbA12/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Системное время на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр системного времени на клиенте](/home/mavinogradova/skreens/lbA12/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Системное время на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр системного времени на сервере](/home/mavinogradova/skreens/lbA12/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Аппаратное время на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Попытка просмотра аппаратного времени на сервере](/home/mavinogradova/skreens/lbA12/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Аппаратное время на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Попытка просмотра аппаратного времени на клиенте](/home/mavinogradova/skreens/lbA12/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Установка пакета chrony

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка пакета chrony на сервере](/home/mavinogradova/skreens/lbA12/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Источники времени на сервере (до настройки)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка источников времени на сервере](/home/mavinogradova/skreens/lbA12/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Источники времени на клиенте (до настройки)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка источников времени на клиенте](/home/mavinogradova/skreens/lbA12/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Конфигурация chrony.conf на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование конфигурационного файла chrony.conf на сервере](/home/mavinogradova/skreens/lbA12/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Настройка межсетевого экрана

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка межсетевого экрана на сервере](/home/mavinogradova/skreens/lbA12/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Конфигурация chrony.conf на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование конфигурационного файла chrony.conf на клиенте](/home/mavinogradova/skreens/lbA12/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Источники времени на сервере (после настройки)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка источников времени на сервере после настройки](/home/mavinogradova/skreens/lbA12/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Источники времени на клиенте (после настройки)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка источников времени на клиенте после настройки](/home/mavinogradova/skreens/lbA12/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Статистика синхронизации на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр подробной информации о синхронизации на сервере](/home/mavinogradova/skreens/lbA12/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Статистика синхронизации на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр подробной информации о синхронизации на клиенте](/home/mavinogradova/skreens/lbA12/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Подготовка провижининга сервера

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подготовка окружения на сервере](/home/mavinogradova/skreens/lbA12/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Скрипт ntp.sh для сервера

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта ntp.sh для сервера](/home/mavinogradova/skreens/lbA12/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Подготовка провижининга клиента

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подготовка окружения на клиенте](/home/mavinogradova/skreens/lbA12/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Скрипт ntp.sh для клиента

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта ntp.sh для клиента](/home/mavinogradova/skreens/lbA12/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Vagrantfile (сервер)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление скрипта для сервера в Vagrantfile](/home/mavinogradova/skreens/lbA12/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Vagrantfile (клиент)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление скрипта для клиента в Vagrantfile](/home/mavinogradova/skreens/lbA12/25.png){#fig:025 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были получены навыки по управлению системным временем и настройке синхронизации времени. Были изучены команды `timedatectl`, `date` и `hwclock` для управления системным и аппаратным временем. Сервер был настроен в качестве сервера синхронизации времени для локальной сети с использованием пакета `chrony`. Была выполнена настройка клиента для синхронизации с локальным сервером. Все действия по установке и настройке NTP-сервера и клиента автоматизированы с помощью скриптов Vagrant.
