---
## Front matter
title: "Отчет по лабораторной работе № 5"
subtitle: "Адресация IPv4 и IPv6. Настройка DHCP"
author: "Виноградова Мария Андреевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы

Получение навыков настройки службы DHCP на сетевом оборудовании для распределения адресов IPv4 и IPv6.

# Задание

1. Настроить на маршрутизаторе, имеющим адрес 10.0.0.1, DHCP-сервис по распределению IPv4-адресов из диапазона 10.0.0.2 - 10.0.0.253, настроить получение адреса по DHCP на узле (PC), а также исследовать пакеты DHCP.
2. На интерфейсе маршрутизатора eth1 настроить DHCPv6 без отслеживания состояния.
3. На интерфейсе маршрутизатора eth2 настроить DHCPv6 с учётом отслеживания состояния.

# Выполнение лабораторной работы

## Настройка DHCP в случае IPv4

Запускаем GNS3 VM и GNS3. Создаем новый проект, называем его `pr-gns3-dhcp` (рис. [-@fig:001]).

![Создание нового проекта в GNS3](/home/mavinogradova/skreens/SetyLb5/1.png){#fig:001 width=70%}

В рабочем пространстве размещаем и соединяем устройства в соответствии с топологией (рис. [-@fig:002]). Используем маршрутизатор VyOS и хост (клиент) VPCS.

![Топология сети для первой части работы](/home/mavinogradova/skreens/SetyLb5/2.png){#fig:002 width=70%}

Включаем захват трафика на соединении между коммутатором sw-01 и маршрутизатором gw-01 (рис. [-@fig:003]).

![Топология с включенным захватом трафика](/home/mavinogradova/skreens/SetyLb5/3.png){#fig:003 width=70%}

На маршрутизаторе переходим в режим конфигурирования, изменяем имя устройства и доменное имя, заменяем системного пользователя (рис. [-@fig:004]).

![Первичная настройка маршрутизатора VyOS](/home/mavinogradova/skreens/SetyLb5/4.png){#fig:004 width=70%}

```text
configure
set system host-name mavinogradova-gw-01
set system domain-name mavinogradova.net
set system login user mavinogradova authentication plaintext-password 123456
del interfaces ethernet eth0 address dhcp
commit
save
```

Входим под новым пользователем (рис. [-@fig:005]).

![Вход в систему под новым пользователем](/home/mavinogradova/skreens/SetyLb5/5.png){#fig:005 width=70%}

Удаляем стандартного пользователя `vyos` и сохраняем конфигурацию (рис. [-@fig:006], рис. [-@fig:007]).

![Удаление стандартного пользователя](/home/mavinogradova/skreens/SetyLb5/6.png){#fig:006 width=70%}

![Сохранение конфигурации](/home/mavinogradova/skreens/SetyLb5/7.png){#fig:007 width=70%}

```text
configure
delete system login user vyos
commit
save
```

Настраиваем адресацию IPv4 на интерфейсе eth0 (рис. [-@fig:008]).

![Настройка IPv4-адреса](/home/mavinogradova/skreens/SetyLb5/8.png){#fig:008 width=70%}

```text
set interfaces ethernet eth0 address 10.0.0.1/24
commit
save
```

Добавляем конфигурацию DHCP-сервера на маршрутизаторе, создавая пул адресов `10.0.0.2 - 10.0.0.253` (рис. [-@fig:009]).

![Настройка DHCP-сервера](/home/mavinogradova/skreens/SetyLb5/9.png){#fig:009 width=70%}

```text
set service dhcp-server shared-network-name mavinogradova domain-name mavinogradova.net
set service dhcp-server shared-network-name mavinogradova name-server 10.0.0.1
set service dhcp-server shared-network-name mavinogradova subnet 10.0.0.0/24 default-router 10.0.0.1
set service dhcp-server shared-network-name mavinogradova subnet 10.0.0.0/24 range hosts start 10.0.0.2
set service dhcp-server shared-network-name mavinogradova subnet 10.0.0.0/24 range hosts stop 10.0.0.253
commit
save
```

Для просмотра статистики DHCP-сервера и выданных адресов используем команды (рис. [-@fig:010]).

![Проверка статистики DHCP до запроса](/home/mavinogradova/skreens/SetyLb5/10.png){#fig:010 width=70%}

```text
show dhcp server statistics
show dhcp server leases
```

Настраиваем оконечное устройство PC1 (рис. [-@fig:011]).

![Настройка PC1](/home/mavinogradova/skreens/SetyLb5/11.png){#fig:011 width=70%}

```text
ip dhcp -d
save
```

Проверяем конфигурацию IPv4 на узле, пингуем маршрутизатор (рис. [-@fig:012]).

![Проверка конфигурации на PC1](/home/mavinogradova/skreens/SetyLb5/12.png){#fig:012 width=70%}

```text
show ip
ping 10.0.0.1 -c 2
```

На маршрутизаторе вновь смотрим статистику DHCP-сервера и выданные адреса (рис. [-@fig:013]).

![Проверка статистики после выдачи адреса](/home/mavinogradova/skreens/SetyLb5/13.png){#fig:013 width=70%}

На маршрутизаторе смотрим журнал работы DHCP-сервера (рис. [-@fig:014]).

![Журнал работы DHCP-сервера](/home/mavinogradova/skreens/SetyLb5/14.png){#fig:014 width=70%}

```text
show log | grep dhcp
```

Пояснение по журналу: в логах виден запуск сервера, а затем полный цикл выдачи адреса PC1: `DHCPDISCOVER`, `DHCPOFFER on 10.0.0.2`, `DHCPREQUEST for 10.0.0.2`, `DHCPACK on 10.0.0.2`. Это подтверждает, что сервер корректно обработал запрос и зарезервировал адрес за MAC-адресом клиента.

Анализируем захваченные пакеты в Wireshark (рис. [-@fig:015]).

![Анализ пакетов DHCP в Wireshark](/home/mavinogradova/skreens/SetyLb5/15.png){#fig:015 width=70%}

**Анализ пакетов Wireshark (IPv4):**
В захвате зафиксирован полный цикл DORA (Transaction ID `0x1e438d2d`):
1. **Пакет 84 (DHCP Discover):** от `0.0.0.0` к `255.255.255.255`, MAC отправителя `00:50:79:66:68:00` (PC1). Размер 406 байт.
2. **Пакет 88 (DHCP Offer):** от `10.0.0.1` к `10.0.0.2`. Сервер предлагает адрес **10.0.0.2**. В пакете: `Your IP = 10.0.0.2`, `Subnet Mask = 255.255.255.0`, `Router = 10.0.0.1`, `DNS = 10.0.0.1`. Размер 342 байта.
3. **Пакет 90 (DHCP Request):** от PC1 с запросом адреса `10.0.0.2` и указанием сервера `10.0.0.1`.
4. **Пакет 91 (DHCP ACK):** от `10.0.0.1` к `10.0.0.2`. Сервер подтверждает и резервирует адрес.

## Настройка DHCP в случае IPv6

В предыдущем проекте дополняем сеть, размещаем и соединяем устройства с топологией, используя хосты AlmaLinux и AlpineLinuxVirt. Скачиваем образы (рис. [-@fig:016]).

![Скачанные образы](/home/mavinogradova/skreens/SetyLb5/16.png){#fig:016 width=70%}

Настраиваем шаблоны QEMU в GNS3 (рис. [-@fig:017]).

![Настройка шаблонов QEMU](/home/mavinogradova/skreens/SetyLb5/17.png){#fig:017 width=70%}

Собираем полную топологию для IPv6 (рис. [-@fig:018]).

![Топология для IPv6](/home/mavinogradova/skreens/SetyLb5/18.png){#fig:018 width=70%}

Включаем захват трафика на соединениях между маршрутизатором gw-01 и коммутаторами sw-02 и sw-03 (рис. [-@fig:019]).

![Топология с захватом трафика](/home/mavinogradova/skreens/SetyLb5/19.png){#fig:019 width=70%}

Настраиваем адресацию IPv6 на маршрутизаторе (рис. [-@fig:020]).

![Настройка IPv6-адресации](/home/mavinogradova/skreens/SetyLb5/20.png){#fig:020 width=70%}

```text
configure
set interfaces ethernet eth1 address 2000::1/64
set interfaces ethernet eth2 address 2001::1/64
show interfaces
commit
```

Настраиваем DHCPv6 без отслеживания состояния (stateless) на eth1 (рис. [-@fig:021]).

![Настройка stateless DHCPv6](/home/mavinogradova/skreens/SetyLb5/21.png){#fig:021 width=70%}

```text
set service router-advert interface eth1 prefix 2000::/64
set service router-advert interface eth1 other-config-flag
set service dhcpv6-server shared-network-name mavinogradova-stateless
set service dhcpv6-server shared-network-name mavinogradova-stateless subnet 2000::0/64
set service dhcpv6-server shared-network-name mavinogradova-stateless common-options name-server 2000::1
set service dhcpv6-server shared-network-name mavinogradova-stateless common-options domain-search mavinogradova.net
commit
save
```

На узле Alpine PC2 проверяем настройки сети (рис. [-@fig:022]).

![Проверка настроек на PC2 (Alpine)](/home/mavinogradova/skreens/SetyLb5/22.png){#fig:022 width=70%}

На узле Alma PC2 проверяем настройки сети (рис. [-@fig:023]).

![Проверка настроек на PC2 (Alma)](/home/mavinogradova/skreens/SetyLb5/23.png){#fig:023 width=70%}

```text
ip address
ip -6 route show
ping 2000::1 -c 2
cat /etc/resolv.conf
```
Клиент получил глобальный адрес `2000::...`, маршрут по умолчанию, пинг маршрутизатора проходит, DNS `2000::1` получен.

На маршрутизаторе проверяем выданные адреса (рис. [-@fig:024]).

![Проверка лизингов DHCPv6](/home/mavinogradova/skreens/SetyLb5/24.png){#fig:024 width=70%}

```text
run show dhcpv6 server leases
```

Анализируем пакеты в Wireshark на eth1 (рис. [-@fig:025]).

![Анализ пакетов на eth1](/home/mavinogradova/skreens/SetyLb5/25.png){#fig:025 width=70%}

**Анализ пакетов Wireshark (eth1, stateless):**
Зафиксирован обмен пакетами `Solicit`, `Advertise`, `Request`, `Reply`. В пакете `Reply` (№69) сервер передает статус `NoAddrsAvail` (нет адресов), что подтверждает режим без выдачи адресов (клиент настраивается по SLAAC).

Настраиваем DHCPv6 с отслеживанием состояния (stateful) на eth2 (рис. [-@fig:026]).

![Настройка stateful DHCPv6](/home/mavinogradova/skreens/SetyLb5/26.png){#fig:026 width=70%}

```text
set service router-advert interface eth2 managed-flag
set service dhcpv6-server shared-network-name mavinogradova-stateful
set service dhcpv6-server shared-network-name mavinogradova-stateful subnet 2001::0/64
set service dhcpv6-server shared-network-name mavinogradova-stateful subnet 2001::0/64 name-server 2001::1
set service dhcpv6-server shared-network-name mavinogradova-stateful subnet 2001::0/64 domain-search mavinogradova.net
set service dhcpv6-server shared-network-name mavinogradova-stateful subnet 2001::0/64 address-range start 2001::100 stop 2001::199
commit
save
```

Проверяем лизинги на маршрутизаторе (рис. [-@fig:027]).

![Проверка лизингов stateful](/home/mavinogradova/skreens/SetyLb5/27.png){#fig:027 width=70%}

Подключаемся к узлу Alpine PC3 и проверяем настройки сети (рис. [-@fig:028]).

![Проверка настроек на PC3 (Alpine)](/home/mavinogradova/skreens/SetyLb5/28.png){#fig:028 width=70%}

Подключаемся к узлу Alma PC3 и проверяем настройки сети (рис. [-@fig:029]).

![Проверка настроек на PC3 (Alma)](/home/mavinogradova/skreens/SetyLb5/29.png){#fig:029 width=70%}

```text
ip link set eth0 up
udhcpc6 -i eth0
ip address
ping 2001::1 -c 2
cat /etc/resolv.conf
```

Повторно проверяем сеть на узле Alpine (рис. [-@fig:030]).

![Повторная проверка на PC3 (Alpine)](/home/mavinogradova/skreens/SetyLb5/30.png){#fig:030 width=70%}

Повторно проверяем сеть на узле Alma (рис. [-@fig:031]).

![Повторная проверка на PC3 (Alma)](/home/mavinogradova/skreens/SetyLb5/31.png){#fig:031 width=70%}

Клиент получил адрес `2001::198` из диапазона, пинг маршрутизатора проходит, DNS `2001::1` получен.

Финально проверяем лизинги на маршрутизаторе (рис. [-@fig:032]).

![Финальная проверка лизингов](/home/mavinogradova/skreens/SetyLb5/32.png){#fig:032 width=70%}

```text
run show dhcpv6 server leases
```
В таблице видно 4 активных адреса (`2001::190`, `194`, `198`, `199`).

Анализируем пакеты в Wireshark на eth2 (рис. [-@fig:033]).

![Анализ пакетов на eth2](/home/mavinogradova/skreens/SetyLb5/33.png){#fig:033 width=70%}

**Анализ пакетов Wireshark (eth2, stateful):**
В захвате виден полный цикл `Solicit`, `Advertise`, `Request`, `Reply` для выдачи адресов. В пакете `Reply` (№44) сервер передает клиенту (PC3) следующие параметры:
- **IA Address:** выдан IPv6-адрес **`2001::198`** (Preferred lifetime: 27000, Valid lifetime: 43200 секунд);
- **Client Identifier (DUID):** `000300010c30c2c10000` (идентификатор клиента);
- **Server Identifier (DUID):** `00010001327cc5210cd121800001` (идентификатор сервера);
- **DNS recursive name server:** передан адрес DNS-сервера **`2001::1`**.
Наличие опции `IA Address` подтверждает, что сервер распределяет адреса из пула (режим stateful).

# Выводы

В ходе лабораторной работы были получены навыки настройки службы DHCP на сетевом оборудовании для распределения адресов IPv4 и IPv6. Был настроен DHCP-сервер на маршрутизаторе VyOS для выдачи IPv4-адресов, а также сконфигурированы режимы stateless и stateful для DHCPv6. Проверка работоспособности подтверждена успешным получением адресов клиентами, анализом лизингов на сервере и захваченным трафиком в Wireshark. Все задачи выполнены в полном объеме.

# Список литературы

1. Методические указания к лабораторной работе № 5 «Адресация IPv4 и IPv6. Настройка DHCP». (дата обращения: 03.11.2026).
2. Королькова А. В., Кулябов Д. С. Сетевые технологии. Лабораторный практикум. — М.: РУДН, 2024.
