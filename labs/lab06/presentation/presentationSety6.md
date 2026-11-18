---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №6
subtitle: Адресация IPv4 и IPv6. Настройка маршрутизации по протоколу RIP
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 17 ноября 2026

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

Изучение принципов маршрутизации по протоколу RIP в IPv4- и IPv6-сетях и принципов настройки сетевого оборудования.

# Задание

1. Настройте динамическую маршрутизацию по протоколу RIP в сетях IPv4 и IPv6 (см. раздел 6.3.1).
2. Выполните задание для самостоятельного выполнения (см. раздел 6.3.2).

# Выполнение лабораторной работы

## Создание проекта

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание нового проекта](/home/mavinogradova/skreens/lbA6/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Сборка топологии сети

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология сети в GNS3](/home/mavinogradova/skreens/lbA6/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Схемы L1 и L3

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Схема L1](/home/mavinogradova/skreens/lbA6/3.png){#fig:003 width=100%}

:::
::: {.column width="50%"}

![Схема L3](/home/mavinogradova/skreens/lbA6/4.png){#fig:004 width=100%}

:::
::::::::::::::

## Настройка IPv4 на PC1 и PC2

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка IPv4 на PC1 и PC2](/home/mavinogradova/skreens/lbA6/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Настройка IPv4 на gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка IPv4 на gw-01](/home/mavinogradova/skreens/lbA6/6.png){#fig:006 width=100%}

:::
::: {.column width="50%"}

![Настройка IPv4 на gw-02](/home/mavinogradova/skreens/lbA6/7.png){#fig:007 width=100%}

:::
::::::::::::::

## Настройка IPv4 на gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка IPv4 на gw-03](/home/mavinogradova/skreens/lbA6/8.png){#fig:008 width=100%}

:::
::: {.column width="50%"}

![Настройка IPv4 на gw-04](/home/mavinogradova/skreens/lbA6/9.png){#fig:009 width=100%}

:::
::::::::::::::

## Настройка IPv6 на PC1 и PC2

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка IPv6 на PC1](/home/mavinogradova/skreens/lbA6/10.png){#fig:010 width=100%}

:::
::: {.column width="50%"}

![Настройка IPv6 на PC2](/home/mavinogradova/skreens/lbA6/11.png){#fig:011 width=100%}

:::
::::::::::::::

## Настройка IPv6 на gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка IPv6 на gw-01](/home/mavinogradova/skreens/lbA6/12.png){#fig:012 width=100%}

:::
::: {.column width="50%"}

![Настройка IPv6 на gw-02](/home/mavinogradova/skreens/lbA6/13.png){#fig:013 width=100%}

:::
::::::::::::::

## Настройка IPv6 на gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка IPv6 на gw-03](/home/mavinogradova/skreens/lbA6/14.png){#fig:014 width=100%}

:::
::: {.column width="50%"}

![Настройка IPv6 на gw-04](/home/mavinogradova/skreens/lbA6/15.png){#fig:015 width=100%}

:::
::::::::::::::

## Настройка RIPv2 на gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка RIPv2 на gw-01](/home/mavinogradova/skreens/lbA6/16.png){#fig:016 width=100%}

:::
::: {.column width="50%"}

![Настройка RIPv2 на gw-02](/home/mavinogradova/skreens/lbA6/17.png){#fig:017 width=100%}

:::
::::::::::::::

## Настройка RIPv2 на gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка RIPv2 на gw-03](/home/mavinogradova/skreens/lbA6/18.png){#fig:018 width=100%}

:::
::: {.column width="50%"}

![Настройка RIPv2 на gw-04](/home/mavinogradova/skreens/lbA6/19.png){#fig:019 width=100%}

:::
::::::::::::::

## Проверка RIPv2 на gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка RIPv2 на gw-01](/home/mavinogradova/skreens/lbA6/20.png){#fig:020 width=100%}

:::
::: {.column width="50%"}

![Проверка RIPv2 на gw-02](/home/mavinogradova/skreens/lbA6/21.png){#fig:021 width=100%}

:::
::::::::::::::

## Проверка RIPv2 на gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка RIPv2 на gw-03](/home/mavinogradova/skreens/lbA6/22.png){#fig:022 width=100%}

:::
::: {.column width="50%"}

![Проверка RIPv2 на gw-04](/home/mavinogradova/skreens/lbA6/23.png){#fig:023 width=100%}

:::
::::::::::::::

## Проверка связи с PC1 (IPv4) и метрики RIP

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка связи с PC1 до PC2 по IPv4](/home/mavinogradova/skreens/lbA6/24.png){#fig:024 width=100%}

:::
::: {.column width="50%"}

![Метрики RIPv2 на gw-01](/home/mavinogradova/skreens/lbA6/25.png){#fig:025 width=100%}

:::
::::::::::::::

## Отключение eth0 на gw-02 и проверка изменений

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Отключение интерфейса eth0 на gw-02](/home/mavinogradova/skreens/lbA6/26.png){#fig:026 width=100%}

:::
::: {.column width="50%"}

![Изменение маршрутов RIPv2 на gw-01 после отказа](/home/mavinogradova/skreens/lbA6/27.png){#fig:027 width=100%}

:::
::::::::::::::

## Проверка связи после отказа и включение интерфейса

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка связи после отказа канала](/home/mavinogradova/skreens/lbA6/28.png){#fig:028 width=100%}

:::
::: {.column width="50%"}

![Включение интерфейса eth0 на gw-02](/home/mavinogradova/skreens/lbA6/29.png){#fig:029 width=100%}

:::
::::::::::::::

## Восстановление короткого пути (IPv4)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление короткого пути](/home/mavinogradova/skreens/lbA6/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Захват трафика RIPv2

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Захват RIPv2 на gw-01](/home/mavinogradova/skreens/lbA6/31.png){#fig:031 width=100%}

:::
::: {.column width="50%"}

![Захват RIPv2 на gw-03](/home/mavinogradova/skreens/lbA6/32.png){#fig:032 width=100%}

:::
::::::::::::::

## Настройка RIPng на gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка RIPng на gw-01](/home/mavinogradova/skreens/lbA6/33.png){#fig:033 width=100%}

:::
::: {.column width="50%"}

![Настройка RIPng на gw-02](/home/mavinogradova/skreens/lbA6/34.png){#fig:034 width=100%}

:::
::::::::::::::

## Настройка RIPng на gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка RIPng на gw-03](/home/mavinogradova/skreens/lbA6/35.png){#fig:035 width=100%}

:::
::: {.column width="50%"}

![Настройка RIPng на gw-04](/home/mavinogradova/skreens/lbA6/36.png){#fig:036 width=100%}

:::
::::::::::::::

## Проверка связи с PC1 (IPv6) и метрики RIPng

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка связи с PC1 до PC2 по IPv6](/home/mavinogradova/skreens/lbA6/37.png){#fig:037 width=100%}

:::
::: {.column width="50%"}

![Метрики RIPng на gw-01](/home/mavinogradova/skreens/lbA6/38.png){#fig:038 width=100%}

:::
::::::::::::::

## Отключение eth0 на gw-02 и проверка RIPng

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Отключение интерфейса eth0 на gw-02](/home/mavinogradova/skreens/lbA6/39.png){#fig:039 width=100%}

:::
::: {.column width="50%"}

![Изменение маршрутов RIPng на gw-01 после отказа](/home/mavinogradova/skreens/lbA6/40.png){#fig:040 width=100%}

:::
::::::::::::::

## Проверка связи после отказа и включение интерфейса (IPv6)

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка связи после отказа канала по IPv6](/home/mavinogradova/skreens/lbA6/41.png){#fig:041 width=100%}

:::
::: {.column width="50%"}

![Включение интерфейса eth0 на gw-02](/home/mavinogradova/skreens/lbA6/42.png){#fig:042 width=100%}

:::
::::::::::::::

## Восстановление короткого пути (IPv6)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление короткого пути по IPv6](/home/mavinogradova/skreens/lbA6/43.png){#fig:043 width=70%}

:::
::::::::::::::

## Захват трафика RIPng

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Захват RIPng на gw-01](/home/mavinogradova/skreens/lbA6/44.png){#fig:044 width=100%}

:::
::: {.column width="50%"}

![Захват RIPng на gw-03](/home/mavinogradova/skreens/lbA6/45.png){#fig:045 width=100%}

:::
::::::::::::::

## Самостоятельное задание: создание проекта

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание нового проекта для самостоятельного задания](/home/mavinogradova/skreens/lbA6/46.png){#fig:046 width=70%}

:::
::::::::::::::

## Самостоятельное задание: сборка топологии

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Топология сети для самостоятельного задания](/home/mavinogradova/skreens/lbA6/47.png){#fig:047 width=70%}

:::
::::::::::::::

## Самостоятельное задание: схемы L1 и L3

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Схема L1 для самостоятельного задания](/home/mavinogradova/skreens/lbA6/48.png){#fig:048 width=100%}

:::
::: {.column width="50%"}

![Схема L3 для самостоятельного задания](/home/mavinogradova/skreens/lbA6/49.png){#fig:049 width=100%}

:::
::::::::::::::

## Самостоятельное задание: настройка PC1 и PC2

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка PC1 и PC2](/home/mavinogradova/skreens/lbA6/50.png){#fig:050 width=70%}

:::
::::::::::::::

## Самостоятельное задание: настройка gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка gw-01](/home/mavinogradova/skreens/lbA6/51.png){#fig:051 width=100%}

:::
::: {.column width="50%"}

![Настройка gw-02](/home/mavinogradova/skreens/lbA6/52.png){#fig:052 width=100%}

:::
::::::::::::::

## Самостоятельное задание: настройка gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка gw-03](/home/mavinogradova/skreens/lbA6/53.png){#fig:053 width=100%}

:::
::: {.column width="50%"}

![Настройка gw-04](/home/mavinogradova/skreens/lbA6/54.png){#fig:054 width=100%}

:::
::::::::::::::

## Самостоятельное задание: настройка RIP на gw-01 и gw-02

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка RIP на gw-01](/home/mavinogradova/skreens/lbA6/55.png){#fig:055 width=100%}

:::
::: {.column width="50%"}

![Настройка RIP на gw-02](/home/mavinogradova/skreens/lbA6/56.png){#fig:056 width=100%}

:::
::::::::::::::

## Самостоятельное задание: настройка RIP на gw-03 и gw-04

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Настройка RIP на gw-03](/home/mavinogradova/skreens/lbA6/57.png){#fig:057 width=100%}

:::
::: {.column width="50%"}

![Настройка RIP на gw-04](/home/mavinogradova/skreens/lbA6/58.png){#fig:058 width=100%}

:::
::::::::::::::

## Самостоятельное задание: проверка связи и маршрутов

:::::::::::::: {.columns align=center}
::: {.column width="50%"}

![Проверка связи с PC1 до PC2](/home/mavinogradova/skreens/lbA6/59.png){#fig:059 width=100%}

:::
::: {.column width="50%"}

![Проверка таблиц маршрутизации на gw-01](/home/mavinogradova/skreens/lbA6/60.png){#fig:060 width=100%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были изучены принципы маршрутизации по протоколу RIP в IPv4- и IPv6-сетях и принципы настройки сетевого оборудования. Была настроена адресация IPv4 и IPv6 на оконечных устройствах и маршрутизаторах. Настроены протоколы динамической маршрутизации RIPv2 и RIPng. Проверены пути прохождения пакетов, проанализирована перестройка маршрутов при отказе каналов связи. Изучен механизм «отравления маршрута» и multicast-рассылка обновлений. Все поставленные задачи выполнены в полном объеме.
