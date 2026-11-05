---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №5
subtitle: Адресация IPv4 и IPv6. Настройка DHCP
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 3 ноября 2026

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

Получение навыков настройки службы DHCP на сетевом оборудовании для распределения адресов IPv4 и IPv6.

# Задание

1. Настроить на маршрутизаторе, имеющим адрес 10.0.0.1, DHCP-сервис по распределению IPv4-адресов из диапазона 10.0.0.2 - 10.0.0.253, настроить получение адреса по DHCP на узле (PC), а также исследовать пакеты DHCP.
2. На интерфейсе маршрутизатора eth1 настроить DHCPv6 без отслеживания состояния.
3. На интерфейсе маршрутизатора eth2 настроить DHCPv6 с учётом отслеживания состояния.

# Выполнение лабораторной работы

## Создание проекта

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание нового проекта в GNS3](/home/mavinogradova/skreens/SetyLb5/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Топология сети (IPv4)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология сети для первой части работы](/home/mavinogradova/skreens/SetyLb5/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Захват трафика (IPv4)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология с включенным захватом трафика](/home/mavinogradova/skreens/SetyLb5/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Первичная настройка VyOS

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Первичная настройка маршрутизатора VyOS](/home/mavinogradova/skreens/SetyLb5/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Вход под новым пользователем

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход в систему под новым пользователем](/home/mavinogradova/skreens/SetyLb5/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Удаление стандартного пользователя

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Удаление стандартного пользователя](/home/mavinogradova/skreens/SetyLb5/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Сохранение конфигурации

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Сохранение конфигурации](/home/mavinogradova/skreens/SetyLb5/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Настройка IPv4-адреса

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv4-адреса](/home/mavinogradova/skreens/SetyLb5/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Настройка DHCP-сервера

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка DHCP-сервера](/home/mavinogradova/skreens/SetyLb5/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Статистика до запроса

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка статистики DHCP до запроса](/home/mavinogradova/skreens/SetyLb5/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Настройка PC1

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка PC1](/home/mavinogradova/skreens/SetyLb5/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Проверка конфигурации PC1

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка конфигурации на PC1](/home/mavinogradova/skreens/SetyLb5/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Статистика после выдачи

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка статистики после выдачи адреса](/home/mavinogradova/skreens/SetyLb5/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Журнал DHCP-сервера

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Журнал работы DHCP-сервера](/home/mavinogradova/skreens/SetyLb5/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Анализ пакетов IPv4

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ пакетов DHCP в Wireshark](/home/mavinogradova/skreens/SetyLb5/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Скачанные образы

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Скачанные образы](/home/mavinogradova/skreens/SetyLb5/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Шаблоны QEMU

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка шаблонов QEMU](/home/mavinogradova/skreens/SetyLb5/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Топология IPv6

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология для IPv6](/home/mavinogradova/skreens/SetyLb5/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Захват трафика IPv6

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология с захватом трафика](/home/mavinogradova/skreens/SetyLb5/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Настройка IPv6-адресации

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv6-адресации](/home/mavinogradova/skreens/SetyLb5/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Stateless DHCPv6

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка stateless DHCPv6](/home/mavinogradova/skreens/SetyLb5/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Проверка настроек PC2

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка настроек на PC2 (Alpine)](/home/mavinogradova/skreens/SetyLb5/22.png){#fig:022 width=90%}

:::
::: {.column width="50%"}

![Проверка настроек на PC2 (Alma)](/home/mavinogradova/skreens/SetyLb5/23.png){#fig:023 width=90%}

:::
::::::::::::::

## Лизинги DHCPv6

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка лизингов DHCPv6](/home/mavinogradova/skreens/SetyLb5/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Анализ пакетов на eth1

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ пакетов на eth1](/home/mavinogradova/skreens/SetyLb5/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Stateful DHCPv6

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка stateful DHCPv6](/home/mavinogradova/skreens/SetyLb5/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Проверка лизингов stateful

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка лизингов stateful](/home/mavinogradova/skreens/SetyLb5/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Проверка настроек PC3

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка настроек на PC3 (Alpine)](/home/mavinogradova/skreens/SetyLb5/28.png){#fig:028 width=90%}

:::
::: {.column width="50%"}

![Проверка настроек на PC3 (Alma)](/home/mavinogradova/skreens/SetyLb5/29.png){#fig:029 width=90%}

:::
::::::::::::::

## Повторная проверка PC3

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Повторная проверка на PC3 (Alpine)](/home/mavinogradova/skreens/SetyLb5/30.png){#fig:030 width=90%}

:::
::: {.column width="50%"}

![Повторная проверка на PC3 (Alma)](/home/mavinogradova/skreens/SetyLb5/31.png){#fig:031 width=90%}

:::
::::::::::::::

## Финальная проверка лизингов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Финальная проверка лизингов](/home/mavinogradova/skreens/SetyLb5/32.png){#fig:032 width=70%}

:::
::::::::::::::

## Анализ пакетов на eth2

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ пакетов на eth2](/home/mavinogradova/skreens/SetyLb5/33.png){#fig:033 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были получены навыки настройки службы DHCP на сетевом оборудовании для распределения адресов IPv4 и IPv6. Был настроен DHCP-сервер на маршрутизаторе VyOS для выдачи IPv4-адресов, а также сконфигурированы режимы stateless и stateful для DHCPv6. Проверка работоспособности подтверждена успешным получением адресов клиентами, анализом лизингов на сервере и захваченным трафиком в Wireshark. Все задачи выполнены в полном объеме.
