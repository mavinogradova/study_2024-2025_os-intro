---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №2
subtitle: Настройка DNS-сервера
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 12 сентября 2025

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

Приобретение практических навыков по установке и конфигурированию DNS-сервера, усвоение принципов работы системы доменных имён.

# Задание

1. Установить на виртуальной машине server DNS-сервер bind и bind-utils.
2. Сконфигурировать на виртуальной машине server кэширующий DNS-сервер.
3. Сконфигурировать на виртуальной машине server первичный DNS-сервер.
4. При помощи утилит dig и host проанализировать работу DNS-сервера.
5. Написать скрипт для Vagrant, фиксирующий действия по установке и конфигурированию DNS-сервера во внутреннем окружении виртуальной машины server. Внести соответствующие изменения в Vagrantfile.

# Выполнение лабораторной работы

# Подключение к виртуальной машине server

## Запускаем виртуальную машину server и подключаемся к ней по SSH.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подключение к server](/home/mavinogradova/skreens/lbA2/1.png){#fig:001 width=70%}

:::
::::::::::::::

# Установка DNS-сервера

## Входим под пользователем `mavinogradova` и переходим в режим суперпользователя.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход под mavinogradova и получение root](/home/mavinogradova/skreens/lbA2/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Устанавливаем bind и bind-utils.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка bind и bind-utils](/home/mavinogradova/skreens/lbA2/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Выполняем запрос `dig www.yandex.ru`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запрос dig www.yandex.ru](/home/mavinogradova/skreens/lbA2/4.png){#fig:004 width=70%}

:::
::::::::::::::

# Конфигурирование кэширующего DNS-сервера

## Анализируем содержимое `/etc/resolv.conf` и `/etc/named.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое resolv.conf и named.conf](/home/mavinogradova/skreens/lbA2/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Анализируем содержимое `/var/named/named.ca`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое named.ca](/home/mavinogradova/skreens/lbA2/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Анализируем содержимое `named.localhost` и `named.loopback`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое named.localhost и named.loopback](/home/mavinogradova/skreens/lbA2/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Запускаем DNS-сервер и включаем его в автозапуск.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск и включение автозапуска named](/home/mavinogradova/skreens/lbA2/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Повторно выполняем запрос `dig www.yandex.ru`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Повторный запрос dig www.yandex.ru](/home/mavinogradova/skreens/lbA2/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Запрос к локальному DNS-серверу — `dig @127.0.0.1 www.yandex.ru`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запрос dig 127.0.0.1 www.yandex.ru](/home/mavinogradova/skreens/lbA2/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Делаем DNS-сервер сервером по умолчанию: настраиваем соединение `eth0`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование соединения eth0](/home/mavinogradova/skreens/lbA2/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Соединение `System eth0` в Rocky Linux 10 отсутствует.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Соединение System eth0 отсутствует](/home/mavinogradova/skreens/lbA2/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Перезапускаем NetworkManager и проверяем `/etc/resolv.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск NetworkManager и проверка resolv.conf](/home/mavinogradova/skreens/lbA2/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Вносим изменения в `/etc/named.conf`: listen-on и allow-query.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Изменение параметров listen-on и allow-query в named.conf](/home/mavinogradova/skreens/lbA2/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Разрешаем работу с DNS в межсетевом экране.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разрешение DNS в firewall](/home/mavinogradova/skreens/lbA2/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Проверяем прослушивание порта 53 при помощи lsof.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка прослушивания порта 53 через lsof](/home/mavinogradova/skreens/lbA2/16.png){#fig:016 width=70%}

:::
::::::::::::::

# Конфигурирование первичного DNS-сервера

## Копируем шаблон зон `named.rfc1912.zones` и переименовываем в `mavinogradova.net`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование шаблона зон](/home/mavinogradova/skreens/lbA2/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Подключаем файл зон в `/etc/named.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подключение файла зон в named.conf](/home/mavinogradova/skreens/lbA2/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Прописываем прямую и обратную зоны, остальное удаляем.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Файл /etc/named/mavinogradova.net после правки](/home/mavinogradova/skreens/lbA2/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Создаём каталоги `master/fz` и `master/rz`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание каталогов master/fz и master/rz](/home/mavinogradova/skreens/lbA2/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Копируем шаблон прямой зоны `named.localhost`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование шаблона прямой зоны](/home/mavinogradova/skreens/lbA2/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Правка файла прямой зоны.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Правка файла прямой зоны](/home/mavinogradova/skreens/lbA2/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Копируем шаблон обратной зоны `named.loopback`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование шаблона обратной зоны](/home/mavinogradova/skreens/lbA2/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Правка файла обратной зоны.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Правка файла обратной зоны](/home/mavinogradova/skreens/lbA2/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Исправляем права доступа к файлам в `/etc/named` и `/var/named`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Исправление прав доступа](/home/mavinogradova/skreens/lbA2/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Восстанавливаем метки SELinux и настраиваем переключатели.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление меток SELinux и настройка переключателей](/home/mavinogradova/skreens/lbA2/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Перезапускаем named и смотрим логи во втором терминале.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск named и логи во втором терминале](/home/mavinogradova/skreens/lbA2/27.png){#fig:027 width=70%}

:::
::::::::::::::

# Анализ работы DNS-сервера

## Получаем описание DNS-зоны при помощи dig.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Получение описания DNS-зоны](/home/mavinogradova/skreens/lbA2/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Анализируем работу DNS-сервера при помощи host.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ работы DNS-сервера через host](/home/mavinogradova/skreens/lbA2/29.png){#fig:029 width=70%}

:::
::::::::::::::

# Внесение изменений в настройки внутреннего окружения

## Копируем конфигурационные файлы DNS в каталог `provision`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационных файлов DNS в каталог provision](/home/mavinogradova/skreens/lbA2/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Создаём скрипт `dns.sh` для Vagrant.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое скрипта dns.sh](/home/mavinogradova/skreens/lbA2/31.png){#fig:031 width=70%}

:::
::::::::::::::

## Добавляем строку `server.vm.provision "server dns"` в `Vagrantfile`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с добавленной строкой](/home/mavinogradova/skreens/lbA2/32.png){#fig:032 width=70%}

:::
::::::::::::::

# Выводы

Приобретены практические навыки по установке и конфигурированию DNS-сервера, усвоены принципы работы системы доменных имён.

На виртуальной машине server:

- установлены пакеты `bind` и `bind-utils`;
- сконфигурирован кэширующий DNS-сервер;
- сконфигурирован первичный DNS-сервер с прямой зоной `mavinogradova.net` и обратной зоной `1.168.192.in-addr.arpa`;
- работа DNS-сервера проверена утилитами `dig` и `host`;
- написан скрипт `dns.sh` для Vagrant, фиксирующий действия по установке и конфигурированию DNS-сервера, и внесены соответствующие изменения в `Vagrantfile`.
