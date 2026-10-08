---
## Front matter
lang: ru-RU
title: Отчет по Домашней работе №3
subtitle: Анализ трафика в Wireshark
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 8 октября 2026

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
  * <https://github.com/mavinogradova/study_2026-2027>

:::
::::::::::::::

# Цель работы

Изучение посредством Wireshark кадров Ethernet, анализ PDU протоколов транспортного и прикладного уровней стека TCP/IP.

# Задание

1. Изучение возможностей команды ipconfig для ОС типа Windows (ifconfig для систем типа Linux).
2. Определение MAC-адреса устройства и его типа.
3. С помощью Wireshark захватить и проанализировать пакеты ARP и ICMP в части кадров канального уровня.
4. С помощью Wireshark захватить и проанализировать пакеты HTTP, DNS в части заголовков и информации протоколов TCP, UDP, QUIC.
5. С помощью Wireshark проанализировать handshake протокола TCP.

# Выполнение домашней работы

## MAC-адресация. Вывод команды ipconfig

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод команды ipconfig](/home/mavinogradova/skreens/hw3/1.png){#fig:001 width=70%}

:::
::::::::::::::

## MAC-адресация. Вывод команды ipconfig /all

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод команды ipconfig /all](/home/mavinogradova/skreens/hw3/2.png){#fig:002 width=70%}

:::
::::::::::::::

## MAC-адресация. Вывод команды ipconfig /?

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод команды ipconfig /?](/home/mavinogradova/skreens/hw3/3.png){#fig:003 width=70%}

:::
::::::::::::::

## MAC-адресация. MAC-адреса сетевых интерфейсов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![MAC-адреса сетевых интерфейсов](/home/mavinogradova/skreens/hw3/4.png){#fig:004 width=70%}

:::
::::::::::::::

## MAC-адресация. Разбор структуры MAC-адреса

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разбор структуры MAC-адреса](/home/mavinogradova/skreens/hw3/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Стартовое окно Wireshark

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Стартовое окно Wireshark. Выбор интерфейса для прослушивания](/home/mavinogradova/skreens/hw3/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Запущенный захват трафика

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запущенный захват трафика в Wireshark](/home/mavinogradova/skreens/hw3/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Определение IP-адреса и шлюза

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Определение IP-адреса и шлюза по умолчанию](/home/mavinogradova/skreens/hw3/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Ping шлюза по умолчанию

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Ping шлюза по умолчанию](/home/mavinogradova/skreens/hw3/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Фильтр arp or icmp

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фильтр arp or icmp](/home/mavinogradova/skreens/hw3/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. ICMP-пакеты

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![ICMP-пакеты в Wireshark](/home/mavinogradova/skreens/hw3/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Разбор ICMP Echo request

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разбор ICMP Echo request](/home/mavinogradova/skreens/hw3/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. ICMP Echo reply

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![ICMP Echo reply в Wireshark](/home/mavinogradova/skreens/hw3/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Разбор ICMP Echo reply

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разбор ICMP Echo reply](/home/mavinogradova/skreens/hw3/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. ARP Request

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![ARP Request](/home/mavinogradova/skreens/hw3/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. ARP Reply

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![ARP Reply](/home/mavinogradova/skreens/hw3/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Ping rudn.ru и ping 77.88.8.8

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Ping rudn.ru и ping 77.88.8.8](/home/mavinogradova/skreens/hw3/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Фильтр arp or icmp после ping 77.88.8.8

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фильтр arp or icmp после ping 77.88.8.8](/home/mavinogradova/skreens/hw3/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Разбор ICMP Echo request и Echo reply

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разбор ICMP Echo request и Echo reply](/home/mavinogradova/skreens/hw3/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Анализ кадров канального уровня. Разбор ARP Request и ARP Reply

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Разбор ARP Request и ARP Reply](/home/mavinogradova/skreens/hw3/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Анализ протоколов транспортного уровня. Сайт neverssl.com

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Сайт neverssl.com, работающий по HTTP](/home/mavinogradova/skreens/hw3/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Анализ протоколов транспортного уровня. Фильтр http

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фильтр http](/home/mavinogradova/skreens/hw3/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Анализ протоколов транспортного уровня. Фильтр dns

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фильтр dns](/home/mavinogradova/skreens/hw3/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Анализ протоколов транспортного уровня. Фильтр quic

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фильтр quic](/home/mavinogradova/skreens/hw3/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Анализ handshake TCP. Три пакета TCP handshake

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Три пакета TCP handshake](/home/mavinogradova/skreens/hw3/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Анализ handshake TCP. График потока TCP

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![График потока TCP](/home/mavinogradova/skreens/hw3/26.png){#fig:026 width=70%}

:::
::::::::::::::

# Выводы

В ходе домашней работы были изучены возможности анализатора трафика Wireshark для исследования кадров Ethernet и PDU транспортного и прикладного уровней стека TCP/IP. Определены MAC-адреса сетевых интерфейсов, разобрана их структура. Проанализированы ARP-запросы и ARP-ответы, ICMP-эхо-запросы и эхо-ответы. Изучены протоколы прикладного уровня: HTTP, DNS, QUIC. Разобран трёхступенчатый TCP handshake. Wireshark позволяет детально исследовать структуру сетевых пакетов на всех уровнях стека TCP/IP.
