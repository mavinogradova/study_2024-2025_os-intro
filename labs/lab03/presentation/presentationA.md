---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №3
subtitle: Настройка DHCP-сервера
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 19 сентября 2025

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

Приобретение практических навыков по установке и конфигурированию DHCP-сервера.

# Задание

1. Установить на виртуальной машине server DHCP-сервер.
2. Настроить виртуальную машину server в качестве DHCP-сервера для виртуальной внутренней сети.
3. Получить параметры от DHCP-сервера в виртуальной внутренней сети путём запуска виртуальной машины client и применения соответствующих утилит диагностики.
4. Настроить обновление DNS-зоны при появлении в виртуальной внутренней сети новых узлов.
5. Проверить корректность работы DHCP-сервера и обновления DNS-зоны в виртуальной внутренней сети путём запуска виртуальной машины client и применения соответствующих утилит диагностики.
6. Написать скрипт для Vagrant, фиксирующий действия по установке и настройке DHCP-сервера во внутреннем окружении виртуальной машины server. Внести соответствующие изменения в Vagrantfile.

# Выполнение лабораторной работы

# Подключение к виртуальной машине server

## Запускаем виртуальную машину server и подключаемся к ней по SSH.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск виртуальной машины server](/home/mavinogradova/skreens/lab3/1.png){#fig:001 width=70%}

:::
::::::::::::::

# Установка DHCP-сервера

## Входим под пользователем `mavinogradova` и переходим в режим суперпользователя.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход под mavinogradova и получение root](/home/mavinogradova/skreens/lab3/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Устанавливаем DHCP-сервер Kea.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка kea](/home/mavinogradova/skreens/lab3/3.png){#fig:003 width=70%}

:::
::::::::::::::

# Конфигурирование DHCP-сервера

## Сохраняем резервную копию конфигурационного файла и открываем его на редактирование.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Резервная копия kea-dhcp4.conf](/home/mavinogradova/skreens/lab3/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Анализируем содержимое `/etc/kea/kea-dhcp4.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое kea-dhcp4.conf](/home/mavinogradova/skreens/lab3/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Задаём конфигурацию DHCP-сети и проверяем конфигурационный файл.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка конфигурационного файла](/home/mavinogradova/skreens/lab3/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Перезагружаем конфигурацию dhcpd и включаем автозапуск.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезагрузка конфигурации и включение автозапуска](/home/mavinogradova/skreens/lab3/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Добавляем запись для DHCP-сервера в DNS-зоны (прямую и обратную).

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Прямая DNS-зона mavinogradova.net](/home/mavinogradova/skreens/lab3/8.png){#fig:008 width=70%}

:::
::::::::::::::

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Обратная DNS-зона 192.168.1](/home/mavinogradova/skreens/lab3/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Перезапускаем named и проверяем ping dhcp.mavinogradova.net.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск named и проверка ping dhcp.mavinogradova.net](/home/mavinogradova/skreens/lab3/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Разрешаем работу с DHCP в межсетевом экране.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разрешение DHCP в firewall](/home/mavinogradova/skreens/lab3/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Восстанавливаем контекст безопасности в SELinux.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление контекста SELinux](/home/mavinogradova/skreens/lab3/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Запускаем мониторинг системных сообщений.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Мониторинг системных сообщений](/home/mavinogradova/skreens/lab3/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Запускаем DHCP-сервер и убеждаемся в успешном запуске.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск DHCP-сервера](/home/mavinogradova/skreens/lab3/14.png){#fig:014 width=70%}

:::
::::::::::::::

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск kea-dhcp4.service](/home/mavinogradova/skreens/lab3/15.png){#fig:015 width=70%}

:::
::::::::::::::

# Анализ работы DHCP-сервера

## Создаём файл `01-routing.sh` в каталоге provision/client.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание файла 01-routing.sh](/home/mavinogradova/skreens/lab3/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Прописываем в нём скрипт, изменяющий настройки NetworkManager.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое 01-routing.sh](/home/mavinogradova/skreens/lab3/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Подключаем скрипт в `Vagrantfile` в разделе конфигурации для клиента.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с client routing](/home/mavinogradova/skreens/lab3/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Запускаем клиента с провижинингом.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск клиента с провижинингом](/home/mavinogradova/skreens/lab3/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Наблюдаем логи DHCP-сервера о выдаче IP-адреса.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Логи DHCP-сервера](/home/mavinogradova/skreens/lab3/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Информация о работе DHCP-сервера в файле `/var/lib/kea/kea-leases4.csv`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое kea-leases4.csv](/home/mavinogradova/skreens/lab3/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Входим на client и вводим `ifconfig`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод ifconfig на клиенте](/home/mavinogradova/skreens/lab3/22.png){#fig:022 width=70%}

:::
::::::::::::::

# Настройка обновления DNS-зоны

## Создаём TSIG-ключ на сервере с Bind9.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Генерация TSIG-ключа](/home/mavinogradova/skreens/lab3/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Поправляем права доступа на ключ.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Права доступа на ключ](/home/mavinogradova/skreens/lab3/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Подключаем ключ в файле `/etc/named.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подключение ключа в named.conf](/home/mavinogradova/skreens/lab3/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Разрешаем обновление зон в `/etc/named/mavinogradova.net`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разрешение обновления зон](/home/mavinogradova/skreens/lab3/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Проверяем конфигурационный файл и перезапускаем DNS-сервер.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка named.conf и перезапуск named](/home/mavinogradova/skreens/lab3/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Формируем ключ для Kea — `/etc/kea/tsig-keys.json`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание tsig-keys.json](/home/mavinogradova/skreens/lab3/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Переносим ключ и переписываем его в формате JSON.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое tsig-keys.json](/home/mavinogradova/skreens/lab3/29.png){#fig:029 width=70%}

:::
::::::::::::::

## Сменим владельца и поправим права доступа.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Права на tsig-keys.json](/home/mavinogradova/skreens/lab3/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Настройка в файле `/etc/kea/kea-dhcp-ddns.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка kea-dhcp-ddns.conf](/home/mavinogradova/skreens/lab3/31.png){#fig:031 width=70%}

:::
::::::::::::::

## Проверяем синтаксис, запускаем службу ddns и проверяем статус.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка и запуск kea-dhcp-ddns](/home/mavinogradova/skreens/lab3/32.png){#fig:032 width=70%}

:::
::::::::::::::

## Включаем `enable-updates` в `/etc/kea/kea-dhcp4.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Правка kea-dhcp4.conf (enable-updates)](/home/mavinogradova/skreens/lab3/33.png){#fig:033 width=70%}

:::
::::::::::::::

## Проверяем и перезапускаем DHCP-сервер.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка и перезапуск kea-dhcp4](/home/mavinogradova/skreens/lab3/34.png){#fig:034 width=70%}

:::
::::::::::::::

## На машине client переполучаем адрес.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Переполучение адреса на клиенте](/home/mavinogradova/skreens/lab3/35.png){#fig:035 width=70%}

:::
::::::::::::::

## Появление файла `mavinogradova.net.jnl` в каталоге прямой зоны.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Появление mavinogradova.net.jnl](/home/mavinogradova/skreens/lab3/36.png){#fig:036 width=70%}

:::
::::::::::::::

# Анализ работы DHCP-сервера после настройки DDNS

## Убеждаемся в наличии DNS-записи о клиенте через dig.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка DNS-записи клиента через dig](/home/mavinogradova/skreens/lab3/37.png){#fig:037 width=70%}

:::
::::::::::::::

# Внесение изменений в настройки внутреннего окружения

## Копируем конфигурационные файлы DHCP в каталог provision.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационных файлов DHCP](/home/mavinogradova/skreens/lab3/38.png){#fig:038 width=70%}

:::
::::::::::::::

## Копируем конфигурационные файлы DNS-сервера.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационных файлов DNS](/home/mavinogradova/skreens/lab3/39.png){#fig:039 width=70%}

:::
::::::::::::::

## Создаём исполняемый файл `dhcp.sh`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание dhcp.sh](/home/mavinogradova/skreens/lab3/40.png){#fig:040 width=70%}

:::
::::::::::::::

## Прописываем в нём скрипт для Vagrant.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое dhcp.sh](/home/mavinogradova/skreens/lab3/41.png){#fig:041 width=70%}

:::
::::::::::::::

## Добавляем строку `server.vm.provision "server dhcp"` в `Vagrantfile`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с server dhcp](/home/mavinogradova/skreens/lab3/42.png){#fig:042 width=70%}

:::
::::::::::::::

## После этого виртуальные машины можно выключить.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Выключение виртуальных машин](/home/mavinogradova/skreens/lab3/43.png){#fig:043 width=70%}

:::
::::::::::::::

# Выводы

Приобретены практические навыки по установке и конфигурированию DHCP-сервера.

На виртуальной машине server:

- установлен DHCP-сервер Kea;
- настроена выдача адресов из пула `192.168.1.30–199` для виртуальной внутренней сети;
- выполнена привязка службы к интерфейсу `eth1`;
- виртуальная машина client получила IP-адрес `192.168.1.30` по DHCP, что подтверждено утилитами `ifconfig` и файлом аренд `/var/lib/kea/kea-leases4.csv`;
- настроено обновление DNS-зон при появлении новых узлов: сгенерирован TSIG-ключ `DHCP_UPDATE`, разрешено обновление зон в конфигурации `named`, сформирован файл `tsig-keys.json`, сконфигурирован сервис `kea-dhcp-ddns` и включён параметр `enable-updates` в `kea-dhcp4.conf`;
- корректность работы проверена утилитой `dig` — A-запись `client.mavinogradova.net` автоматически создана в прямой зоне;
- написан скрипт `dhcp.sh` для Vagrant, фиксирующий действия по установке и настройке DHCP-сервера, и внесены соответствующие изменения в `Vagrantfile`.
