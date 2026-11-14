---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №11
subtitle: Настройка удалённого доступа к серверу с помощью SSH
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 10 ноября 2026

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

Приобретение практических навыков по настройке удалённого доступа к серверу с помощью SSH.

# Задание

1. Настройте запрет удалённого доступа на сервер по SSH для пользователя root (см. раздел 11.4.1).
2. Настройте разрешение удалённого доступа к серверу по SSH только для пользователей группы vagrant и вашего пользователя (см. раздел 11.4.2).
3. Настройте удалённый доступ к серверу по SSH через порт 2022 (см. раздел 11.4.3).
4. Настройте удалённый доступ к серверу по SSH по ключу (см. раздел 11.4.4).
5. Организуйте SSH-туннель с клиента на сервер, перенаправив локальное соединение с TCP-порта 80 на порт 8080 (см. раздел 11.4.5).
6. Используя удалённое SSH-соединение, выполните с клиента несколько команд на сервере (см. раздел 11.4.6).
7. Используя удалённое SSH-соединение, запустите с клиента графическое приложение на сервере (см. раздел 11.4.7).
8. Напишите скрипт для Vagrant, фиксирующий действия по настройке SSH-сервера во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 11.4.8).

# Выполнение лабораторной работы

## Запрет доступа для root. Задание пароля

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Задание пароля для пользователя root](/home/mavinogradova/skreens/lbA11/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Запрет доступа для root. Мониторинг

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Мониторинг системных событий на сервере](/home/mavinogradova/skreens/lbA11/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Запрет доступа для root. Вход до изменений

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Успешный вход под root до изменения конфигурации](/home/mavinogradova/skreens/lbA11/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Запрет доступа для root. Настройка конфига

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка запрета входа для root](/home/mavinogradova/skreens/lbA11/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Запрет доступа для root. Перезапуск

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск службы sshd](/home/mavinogradova/skreens/lbA11/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Запрет доступа для root. Отказ

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Отказ в доступе для root после изменения конфигурации](/home/mavinogradova/skreens/lbA11/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Ограничение пользователей. Вход до ограничений

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Успешный вход пользователя до применения ограничений](/home/mavinogradova/skreens/lbA11/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Ограничение пользователей. AllowUsers vagrant

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление директивы AllowUsers](/home/mavinogradova/skreens/lbA11/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Ограничение пользователей. Перезапуск 1

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск sshd после добавления AllowUsers](/home/mavinogradova/skreens/lbA11/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Ограничение пользователей. Отказ

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Отказ в доступе после добавления AllowUsers](/home/mavinogradova/skreens/lbA11/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Ограничение пользователей. AllowUsers + mavinogradova

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление пользователя в AllowUsers](/home/mavinogradova/skreens/lbA11/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Ограничение пользователей. Перезапуск 2

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск sshd после изменения AllowUsers](/home/mavinogradova/skreens/lbA11/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Ограничение пользователей. Успех

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Успешный вход после добавления в AllowUsers](/home/mavinogradova/skreens/lbA11/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Порты. Добавление порта 2022

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление порта 2022](/home/mavinogradova/skreens/lbA11/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Порты. Ошибка SELinux

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Ошибка запуска на порту 2022](/home/mavinogradova/skreens/lbA11/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Порты. Успешный запуск

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Успешный запуск sshd на двух портах](/home/mavinogradova/skreens/lbA11/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Порты. Проверка входа

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка доступа через порты 22 и 2022](/home/mavinogradova/skreens/lbA11/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Ключи. Включение PubkeyAuthentication

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Включение аутентификации по ключу](/home/mavinogradova/skreens/lbA11/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Ключи. Перезапуск

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск sshd](/home/mavinogradova/skreens/lbA11/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Ключи. Генерация

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Генерация SSH-ключа](/home/mavinogradova/skreens/lbA11/2.png){#fig:020 width=70%}

:::
::::::::::::::

## Ключи. Копирование на сервер

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование ключа на сервер](/home/mavinogradova/skreens/lbA11/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Ключи. Вход без пароля

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход без пароля по ключу](/home/mavinogradova/skreens/lbA11/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Туннель. Службы до

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Список TCP-служб до создания туннеля](/home/mavinogradova/skreens/lbA11/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Туннель. Создание и службы после

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка TCP-служб после создания туннеля](/home/mavinogradova/skreens/lbA11/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Туннель. Проверка работы

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка работы туннеля](/home/mavinogradova/skreens/lbA11/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Консольные приложения

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Выполнение консольных команд через SSH](/home/mavinogradova/skreens/lbA11/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Графические приложения. Настройка

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разрешение X11Forwarding](/home/mavinogradova/skreens/lbA11/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Графические приложения. Перезапуск

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск sshd](/home/mavinogradova/skreens/lbA11/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Графические приложения. Запуск

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Попытка запуска графического приложения](/home/mavinogradova/skreens/lbA11/29.png){#fig:029 width=70%}

:::
::::::::::::::

## Окружение ВМ. Копирование конфигов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационного файла в каталог провижининга](/home/mavinogradova/skreens/lbA11/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Окружение ВМ. Скрипт ssh.sh

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта ssh.sh](/home/mavinogradova/skreens/lbA11/31.png){#fig:031 width=70%}

:::
::::::::::::::

## Окружение ВМ. Vagrantfile

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление скрипта в Vagrantfile](/home/mavinogradova/skreens/lbA11/32.png){#fig:032 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были приобретены практические навыки по настройке удалённого доступа к серверу с помощью SSH. Был настроен запрет удалённого доступа для пользователя root и ограничен список разрешенных пользователей. Организован доступ по нестандартному порту 2022 с настройкой SELinux и межсетевого экрана. Настроена аутентификация по ключам, что позволило входить на сервер без ввода пароля. Освоено создание SSH-туннелей для перенаправления портов, а также запуск команд и графических приложений на удаленном сервере. Все действия по настройке SSH-сервера автоматизированы с помощью скриптов Vagrant.
