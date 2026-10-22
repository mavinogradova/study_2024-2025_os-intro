---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №4
subtitle: Адресация IPv4 и IPv6. Двойной стек
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 22 октября 2026

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

Изучение принципов распределения и настройки адресного пространства на устройствах сети.

# Задание

1. Настройка двойного стека адресации IPv4 и IPv6 в локальной сети.
2. Выполнение задания для самостоятельного выполнения: характеристика подсетей, предложение варианта таблицы адресации, настройка IP-адресации на маршрутизаторе VyOS и оконечных устройствах, проверка подключения.

# Выполнение лабораторной работы

## Создание проекта и сборка топологии

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание нового проекта в GNS3 с именем lab-4-mavinogradova](/home/mavinogradova/skreens/SetyLb4/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Настройка сетевых адаптеров VyOS

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление трех сетевых адаптеров в настройках маршрутизатора VyOS](/home/mavinogradova/skreens/SetyLb4/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Собранная топология сети

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Собранная топология сети в GNS3 с переименованными устройствами и соединенными линками](/home/mavinogradova/skreens/SetyLb4/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Захват трафика

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запущенный захват трафика в Wireshark на линке между коммутатором и сервером](/home/mavinogradova/skreens/SetyLb4/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Настройка IPv4 на узлах PC1, PC2, Server

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка и проверка IPv4-адресации на узлах PC1, PC2 и Server](/home/mavinogradova/skreens/SetyLb4/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Настройка IPv4 на маршрутизаторе FRR

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv4-адресов на маршрутизаторе FRR, смена имени хоста и сохранение конфигурации](/home/mavinogradova/skreens/SetyLb4/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Проверка конфигурации FRR

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка конфигурации маршрутизатора FRR](/home/mavinogradova/skreens/SetyLb4/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Проверка подключения по IPv4

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка подключения по IPv4 между PC1, PC2 и сервером](/home/mavinogradova/skreens/SetyLb4/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Настройка IPv6 на узлах PC3, PC4, Server

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv6-адресов на узлах PC3, PC4 и Server](/home/mavinogradova/skreens/SetyLb4/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Проверка конфигурации IPv6 на PC3 и PC4

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка конфигурации IPv4 и IPv6 на PC3 и PC4](/home/mavinogradova/skreens/SetyLb4/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Первичная настройка VyOS

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Первичная настройка маршрутизатора VyOS: смена имени хоста, удаление DHCP с eth0 и сохранение](/home/mavinogradova/skreens/SetyLb4/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Перезагрузка VyOS

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Ввод команды reboot на маршрутизаторе VyOS](/home/mavinogradova/skreens/SetyLb4/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Настройка IPv6 на интерфейсах VyOS

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv6-адресов на трех интерфейсах VyOS и проверка](/home/mavinogradova/skreens/SetyLb4/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Проверка IPv6-подключения

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка IPv6-подключения между PC3, PC4 и сервером](/home/mavinogradova/skreens/SetyLb4/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Проверка изоляции сетей

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка изоляции сетей](/home/mavinogradova/skreens/SetyLb4/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Анализ ARP-трафика

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ перехваченного трафика в Wireshark по фильтру arp](/home/mavinogradova/skreens/SetyLb4/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Анализ ICMP-трафика

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ перехваченного трафика в Wireshark по фильтру icmp](/home/mavinogradova/skreens/SetyLb4/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Анализ ICMPv6-трафика

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ перехваченного трафика в Wireshark по фильтру icmpv6](/home/mavinogradova/skreens/SetyLb4/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Топология для самостоятельного задания

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Собранная топология для второй части работы](/home/mavinogradova/skreens/SetyLb4/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Настройка ПК во второй части

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv4 и IPv6 адресов на компьютерах PC1 и PC2](/home/mavinogradova/skreens/SetyLb4/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Настройка VyOS во второй части

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv4 и IPv6 адресов на интерфейсах маршрутизатора VyOS и устранение конфликта с DHCP](/home/mavinogradova/skreens/SetyLb4/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Проверка связи во второй части

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка подключения между PC1 и PC2 в обеих подсетях](/home/mavinogradova/skreens/SetyLb4/22.png){#fig:022 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были изучены принципы распределения и настройки адресного пространства на устройствах сети. В первой части работы была собрана топология с двумя локальными подсетями, настроен двойной стек (Dual Stack) на сервере, проверена маршрутизация между IPv4 и IPv6 подсетями, а также их изоляция. Во второй части работы были охарактеризованы две подсети, составлена таблица адресации, настроена IP-адресация на маршрутизаторе VyOS и оконечных устройствах, а также проверено подключение между устройствами.
