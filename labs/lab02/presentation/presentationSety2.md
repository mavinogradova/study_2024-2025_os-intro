---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №2
subtitle: Простые сети в GNS3. Анализ трафика
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 24 сентября 2026

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

Построение простейших моделей сети на базе коммутатора и маршрутизаторов FRR и VyOS в GNS3, анализ трафика посредством Wireshark.

# Задание

1. Построить в GNS3 топологию сети, состоящей из коммутатора Ethernet и двух оконечных устройств (персональных компьютеров). Задать оконечным устройствам IP-адреса в сети 192.168.1.0/24. Проверить связь.
2. Запустить на соединении между PC-1 и коммутатором анализатор трафика Wireshark. Проанализировать ARP-трафик, а также эхо-запросы в ICMP-, UDP- и TCP-режимах.
3. Построить в GNS3 топологию сети, состоящей из маршрутизатора FRR, коммутатора Ethernet и оконечного устройства. Задать оконечному устройству IP-адрес в сети 192.168.1.0/24, присвоить интерфейсу маршрутизатора адрес 192.168.1.1/24. Проверить связь.
4. Построить в GNS3 топологию сети, состоящей из маршрутизатора VyOS, коммутатора Ethernet и оконечного устройства. Задать оконечному устройству IP-адрес в сети 192.168.1.0/24, присвоить интерфейсу маршрутизатора адрес 192.168.1.1/24. Проверить связь.

# Выполнение лабораторной работы

# Моделирование простейшей сети на базе коммутатора в GNS3

## Запускаем GNS3 VM и GNS3. Создаём новый проект `prg-gns-01`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание проекта prg-gns-01](/home/mavinogradova/skreens/SetyLb2/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Размещаем коммутатор Ethernet и два VPCS, соединяем их.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология простейшей сети в GNS3](/home/mavinogradova/skreens/SetyLb2/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Просматриваем синтаксис возможных для ввода команд VPCS.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр синтаксиса возможных для ввода команд VPCS в GNS3](/home/mavinogradova/skreens/SetyLb2/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Настраиваем IP-адресацию PC1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IP-адресации PC1](/home/mavinogradova/skreens/SetyLb2/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Настраиваем IP-адресацию PC2.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IP-адресации PC2](/home/mavinogradova/skreens/SetyLb2/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Проверяем связь между PC1 и PC2.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка связи между PC1 и PC2](/home/mavinogradova/skreens/SetyLb2/6.png){#fig:006 width=70%}

:::
::::::::::::::

# Анализ трафика в GNS3 посредством Wireshark

## Запускаем захват трафика на соединении PC1–коммутатор.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск захвата трафика на соединении PC1–коммутатор](/home/mavinogradova/skreens/SetyLb2/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Открывается окно Wireshark с захватом.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Окно Wireshark после запуска захвата](/home/mavinogradova/skreens/SetyLb2/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Стартуем все узлы проекта.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология с запущенными узлами](/home/mavinogradova/skreens/SetyLb2/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Анализируем информацию по протоколу ARP в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Информация по протоколу ARP в Wireshark](/home/mavinogradova/skreens/SetyLb2/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Смотрим опции команды ping на PC-2.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр опций команды ping](/home/mavinogradova/skreens/SetyLb2/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Делаем один эхо-запрос в ICMP-режиме к PC-1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Один эхо-запрос в ICMP-режиме](/home/mavinogradova/skreens/SetyLb2/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Анализируем ICMP-пакеты в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ ICMP-пакетов в Wireshark](/home/mavinogradova/skreens/SetyLb2/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Делаем один эхо-запрос в UDP-режиме к PC-1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Один эхо-запрос в UDP-режиме](/home/mavinogradova/skreens/SetyLb2/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Анализируем UDP-пакеты в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ UDP-пакетов в Wireshark](/home/mavinogradova/skreens/SetyLb2/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Делаем один эхо-запрос в TCP-режиме к PC-1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Один эхо-запрос в TCP-режиме](/home/mavinogradova/skreens/SetyLb2/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Анализируем TCP-пакеты в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ TCP-пакетов в Wireshark](/home/mavinogradova/skreens/SetyLb2/17.png){#fig:017 width=70%}

:::
::::::::::::::

# Моделирование простейшей сети на базе маршрутизатора FRR в GNS3

## Создаём новый проект `pr01-mar`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание проекта pr01-mar](/home/mavinogradova/skreens/SetyLb2/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Размещаем VPCS, коммутатор Ethernet и маршрутизатор FRR.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология простейшей сети с маршрутизатором FRR в GNS3](/home/mavinogradova/skreens/SetyLb2/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Настраиваем IP-адресацию PC1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IP-адресации PC1](/home/mavinogradova/skreens/SetyLb2/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Настраиваем IP-адресацию маршрутизатора FRR.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка маршрутизатора FRR](/home/mavinogradova/skreens/SetyLb2/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Проверяем конфигурацию маршрутизатора.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка конфигурации FRR](/home/mavinogradova/skreens/SetyLb2/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Проверяем подключение к FRR.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка подключения к FRR](/home/mavinogradova/skreens/SetyLb2/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Анализируем ICMP-пакеты между PC1 и FRR в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ ICMP-пакетов между PC1 и FRR в Wireshark](/home/mavinogradova/skreens/SetyLb2/24.png){#fig:024 width=70%}

:::
::::::::::::::

# Моделирование простейшей сети на базе маршрутизатора VyOS в GNS3

## Создаём новый проект `pr01-VyOS`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание проекта pr01-VyOS](/home/mavinogradova/skreens/SetyLb2/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Размещаем VPCS, коммутатор Ethernet и маршрутизатор VyOS.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология простейшей сети с маршрутизатором VyOS в GNS3](/home/mavinogradova/skreens/SetyLb2/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Настраиваем IP-адресацию PC1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IP-адресации PC1](/home/mavinogradova/skreens/SetyLb2/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Входим в VyOS, переходим в режим конфигурирования, меняем host-name.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход в VyOS и переход в режим конфигурирования](/home/mavinogradova/skreens/SetyLb2/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Удаляем DHCP, задаём IP, выполняем compare / commit / save, смотрим интерфейсы.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Полная настройка маршрутизатора VyOS](/home/mavinogradova/skreens/SetyLb2/29.png){#fig:029 width=70%}

:::
::::::::::::::

## Проверяем подключение к VyOS.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка подключения к VyOS](/home/mavinogradova/skreens/SetyLb2/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Анализируем ICMP-пакеты между PC1 и VyOS в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Анализ ICMP-пакетов между PC1 и VyOS в Wireshark](/home/mavinogradova/skreens/SetyLb2/31.png){#fig:031 width=70%}

:::
::::::::::::::

## Наблюдаем фоновый DHCP Discover в Wireshark.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фоновый DHCP Discover в Wireshark](/home/mavinogradova/skreens/SetyLb2/32.png){#fig:032 width=70%}

:::
::::::::::::::

## Общий результат успешной связи PC1 и VyOS.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Успешная связь между PC1 и VyOS](/home/mavinogradova/skreens/SetyLb2/33.png){#fig:033 width=70%}

:::
::::::::::::::

# Выводы

Приобретены практические навыки построения простейших моделей сети в GNS3 и анализа трафика посредством Wireshark.

- Построена топология из коммутатора Ethernet и двух VPCS, заданы IP-адреса в сети 192.168.1.0/24, проверена связь.
- С помощью Wireshark проанализирован ARP-трафик, а также эхо-запросы в ICMP-, UDP- и TCP-режимах.
- Построены топологии с маршрутизаторами FRR и VyOS, настроены IP-адреса на интерфейсах (192.168.1.1/24), проверена связь между оконечным устройством и маршрутизаторами.
- В Wireshark зафиксированы пакеты ARP, ICMP Echo Request/Reply, UDP и TCP, подтверждающие корректную работу сети на канальном, сетевом и транспортном уровнях.
