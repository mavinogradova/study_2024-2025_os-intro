---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №1
subtitle: Подготовка лабораторного стенда
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 11 сентября 2025

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
  * [1132240691@pfur.ru](mailto:1132240691@pfur.ru)
  * <https://github.com/mavinogradova/study_2025-2026_os2>

:::
::::::::::::::

# Цель работы

Приобретение практических навыков установки Rocky Linux на виртуальную машину с помощью инструмента Vagrant.

# Задание

1. Сформировать box-файл с дистрибутивом Rocky Linux для VirtualBox.
2. Запустить виртуальные машины сервера и клиента и убедиться в их работоспособности.
3. Внести изменения в настройки загрузки образов виртуальных машин server и client, добавив пользователя с правами администратора и изменив названия хостов.
4. Скопировать необходимые для работы с Vagrant файлы и box-файлы виртуальных машин на внешний носитель.

# Выполнение лабораторной работы

# Подготовка рабочего каталога

## Создаём каталог для проекта. В ОС Windows — `D:\work\mavinogradova\packer` и `D:\work\mavinogradova\vagrant`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Структура рабочего каталога](/home/mavinogradova/skreens/lbA1/1.png){#fig:001 width=70%}

:::
::::::::::::::

## В подкаталоге `packer` размещаем образ Rocky Linux (`Rocky-10.2-x86_64-minimal.iso`).

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое каталога packer](/home/mavinogradova/skreens/lbA1/2.png){#fig:002 width=70%}

:::
::::::::::::::

## В подкаталоге `vagrant` размещаем `Vagrantfile` и `Makefile`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое каталога vagrant](/home/mavinogradova/skreens/lbA1/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Создаём каталог `provision` с подкаталогами `default`, `server` и `client`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое каталога provision](/home/mavinogradova/skreens/lbA1/4.png){#fig:004 width=70%}

:::
::::::::::::::

# Настройка скриптов провижининга

## Скрипт `01-user.sh` — в переменной `username` указываем `mavinogradova`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Скрипт 01-user.sh](/home/mavinogradova/skreens/lbA1/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Скрипт `01-hostname.sh` — также указываем `mavinogradova`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Скрипт 01-hostname.sh](/home/mavinogradova/skreens/lbA1/6.png){#fig:006 width=70%}

:::
::::::::::::::

# Конфигурационный файл Vagrantfile

## Блок `## Common configuration` — инструкции по применению скриптов провижининга.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Vagrantfile — common configuration](/home/mavinogradova/skreens/lbA1/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Блок `## Server configuration` — имя box-файла, hostname, SSH, сеть.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Vagrantfile — server configuration](/home/mavinogradova/skreens/lbA1/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Блок `## Client configuration`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Vagrantfile — client configuration](/home/mavinogradova/skreens/lbA1/9.png){#fig:009 width=70%}

:::
::::::::::::::

# HCL-файл настроек виртуальной машины

## Файл `vagrant-rocky.pkr.hcl` — версия дистрибутива, хэш, имя ISO.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant-rocky.pkr.hcl — переменные](/home/mavinogradova/skreens/lbA1/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Проверяем контрольную сумму ISO-образа Rocky Linux 10.2.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![certutil — хэш ISO](/home/mavinogradova/skreens/lbA1/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Значение `iso_checksum` в hcl совпадает с полученным хэшем.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![hcl — iso_checksum](/home/mavinogradova/skreens/lbA1/12.png){#fig:012 width=70%}

:::
::::::::::::::

# Сборка box-файла с помощью Packer

## Инициализация Packer.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![packer init](/home/mavinogradova/skreens/lbA1/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Запуск сборки box-файла.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![packer build — начало](/home/mavinogradova/skreens/lbA1/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Автоматическое развёртывание дистрибутива (Anaconda).

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Anaconda — установка](/home/mavinogradova/skreens/lbA1/15.png){#fig:015 width=70%}

:::
::::::::::::::

# Регистрация box-файла в Vagrant

## Регистрация образа виртуальной машины.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant box add](/home/mavinogradova/skreens/lbA1/16.png){#fig:016 width=70%}

:::
::::::::::::::

# Запуск виртуальной машины Server

## Запуск сервера.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant up server](/home/mavinogradova/skreens/lbA1/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Виртуальная машина `vagrant_server_...` в VirtualBox — работает.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![VirtualBox — server](/home/mavinogradova/skreens/lbA1/18.png){#fig:018 width=70%}

:::
::::::::::::::

# Запуск виртуальной машины Client

## Запуск клиента.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant up client](/home/mavinogradova/skreens/lbA1/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Обе виртуальные машины — server и client — в состоянии «Работает».

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![VirtualBox — обе ВМ](/home/mavinogradova/skreens/lbA1/20.png){#fig:020 width=70%}

:::
::::::::::::::

# Проверка работоспособности стенда

## Проверяем состояние виртуальных машин.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant status](/home/mavinogradova/skreens/lbA1/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Подключаемся к серверу.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant ssh server](/home/mavinogradova/skreens/lbA1/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Переходим к пользователю `mavinogradova`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Приглашение на сервере](/home/mavinogradova/skreens/lbA1/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Подключаемся к клиенту.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant ssh client](/home/mavinogradova/skreens/lbA1/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Переходим к пользователю `mavinogradova` на клиенте.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Приглашение на клиенте](/home/mavinogradova/skreens/lbA1/25.png){#fig:025 width=70%}

:::
::::::::::::::

# Выключение виртуальных машин

## Выключаем обе виртуальные машины.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![ВМ выключены](/home/mavinogradova/skreens/lbA1/26.png){#fig:026 width=70%}

:::
::::::::::::::

# Внесение изменений в настройки окружения

## Фиксируем изменения для server.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant up server --provision](/home/mavinogradova/skreens/lbA1/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Фиксируем изменения для client.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![vagrant up client --provision](/home/mavinogradova/skreens/lbA1/28.png){#fig:028 width=70%}

:::
::::::::::::::

# Выводы

Приобретены практические навыки установки Rocky Linux на виртуальную машину с помощью Vagrant. Освоены создание и настройка `Vagrantfile`, работа с box-файлами, развёртывание стенда из двух машин (server и client), настройка окружения с помощью скриптов провижининга.

В результате стенд из двух виртуальных машин успешно развёрнут, пользователь `mavinogradova` создан, приглашения имеют вид:

- `[mavinogradova@server.mavinogradova.net ~]$`
- `[mavinogradova@client.mavinogradova.net ~]$`
