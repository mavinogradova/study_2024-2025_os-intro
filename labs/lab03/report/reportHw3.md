---
## Front matter
title: "Отчет по Домашней работе №3"
subtitle: "Анализ трафика в Wireshark"
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

Изучение посредством Wireshark кадров Ethernet, анализ PDU протоколов транспортного и прикладного уровней стека TCP/IP.

# Задание

1. Изучение возможностей команды ipconfig для ОС типа Windows (ifconfig для систем типа Linux).
2. Определение MAC-адреса устройства и его типа.
3. С помощью Wireshark захватить и проанализировать пакеты ARP и ICMP в части кадров канального уровня.
4. С помощью Wireshark захватить и проанализировать пакеты HTTP, DNS в части заголовков и информации протоколов TCP, UDP, QUIC.
5. С помощью Wireshark проанализировать handshake протокола TCP.

# Выполнение лабораторной работы

## MAC-адресация

С помощью команды `ipconfig` для ОС типа Windows выведем информацию о текущем сетевом соединении. Используем разные опции команды (рис. [-@fig:001]).

![Вывод команды ipconfig](/home/mavinogradova/skreens/hw3/1.png){#fig:001 width=70%}

Команда `ipconfig` показывает IP-адрес, маску подсети и основной шлюз для каждого адаптера, для которого выполнена привязка к TCP/IP. Для активного адаптера «Беспроводная сеть»: IPv4-адрес — `192.168.1.11`, маска подсети — `255.255.255.0`, основной шлюз — `192.168.1.1`.

Используем опцию `/all` для получения подробных сведений о конфигурации (рис. [-@fig:002]).

![Вывод команды ipconfig /all](/home/mavinogradova/skreens/hw3/2.png){#fig:002 width=70%}

Команда `ipconfig /all` показывает расширенную информацию: имя компьютера (MASHA), тип узла (Гибридный), описание каждого адаптера, физический адрес (MAC-адрес), состояние DHCP, DHCP-сервер, DNS-серверы, срок аренды адреса.

Используем опцию `/?` для получения справки по команде (рис. [-@fig:003]).

![Вывод команды ipconfig /?](/home/mavinogradova/skreens/hw3/3.png){#fig:003 width=70%}

Команда `ipconfig /?` выводит справочное сообщение со списком всех доступных параметров: `/all`, `/release`, `/release6`, `/renew`, `/renew6`, `/flushdns`, `/registerdns`, `/displaydns`, `/showclassid`, `/setclassid`, `/showclassid6`, `/setclassid6`.

Определим MAC-адреса сетевых интерфейсов на компьютере. Для этого используем вывод команды `ipconfig /all` (рис. [-@fig:004]).

![MAC-адреса сетевых интерфейсов](/home/mavinogradova/skreens/hw3/4.png){#fig:004 width=70%}

MAC-адреса имеют следующие адаптеры: Radmin VPN — `02-50-8D-29-B3-EC`, TAP-Windows Adapter V9 — `00-FF-0A-E8-41-53`, VirtualBox Host-Only — `0A-00-27-00-00-18`, Microsoft Wi-Fi Direct Virtual Adapter #3 — `C4-3D-1A-1C-01-5C`, Microsoft Wi-Fi Direct Virtual Adapter #4 — `C6-3D-1A-1C-01-5B`, основной беспроводной адаптер Intel(R) Wi-Fi 6 AX201 160MHz — `C4-3D-1A-1C-01-5B`, Teredo Tunneling — `00-00-00-00-00-00-00-E0`.

Опишем структуру MAC-адреса нашего устройства (рис. [-@fig:005]). Возьмём MAC-адрес основного беспроводного адаптера: `C4-3D-1A-1C-01-5B`. В шестнадцатеричной записи через двоеточие: `C4:3D:1A:1C:01:5B`.

![Разбор структуры MAC-адреса](/home/mavinogradova/skreens/hw3/5.png){#fig:005 width=70%}

## Анализ кадров канального уровня в Wireshark

Wireshark установлен на домашнем устройстве (версия 4.6.9). Установка была выполнена ранее при выполнении лабораторной работы № 2.

Запускаем Wireshark. Выбираем активный сетевой интерфейс «Беспроводная сеть» (рис. [-@fig:006]).

![Стартовое окно Wireshark. Выбор интерфейса для прослушивания](/home/mavinogradova/skreens/hw3/6.png){#fig:006 width=70%}

В списке доступных интерфейсов присутствуют: «Беспроводная сеть», «Подключение по локальной сети», Adapter for loopback traffic capture, виртуальные адаптеры (Подключение по локальной сети* 12, 11, 10, 4, 3), Ethernet 3, Radmin VPN, Event Tracing for Windows. Для захвата выбран интерфейс «Беспроводная сеть».

Запускаем захват трафика. Убеждаемся, что начался процесс захвата трафика (рис. [-@fig:007]).

![Запущенный захват трафика в Wireshark](/home/mavinogradova/skreens/hw3/7.png){#fig:007 width=70%}

Захват идёт (внизу надпись «Беспроводная сеть: <live capture in progress>», счётчик пакетов растёт). В списке присутствуют UDP-пакеты от `89.46.237.45` и ARP-пакеты. ARP-пакет № 4: `Who has 192.168.1.11? Tell 192.168.1.1`; ARP-ответ № 5: `192.168.1.11 is at c4:3d:1a:1c:01:5b`.

В консоли определим с помощью команды `ipconfig` IP-адрес устройства и шлюз по умолчанию (рис. [-@fig:008]).

![Определение IP-адреса и шлюза по умолчанию](/home/mavinogradova/skreens/hw3/8.png){#fig:008 width=70%}

IP-адрес устройства — `192.168.1.11`, основной шлюз — `192.168.1.1`.

В консоли с помощью команды `ping` пропигнуем шлюз по умолчанию (рис. [-@fig:009]).

![Ping шлюза по умолчанию](/home/mavinogradova/skreens/hw3/9.png){#fig:009 width=70%}

Все 4 эхо-запроса получили ответ от шлюза `192.168.1.1`. TTL=64, время отклика 1–3 мс. Потерь 0%.

В Wireshark остановим захват трафика. В строке фильтра пропишем фильтр `arp or icmp`. Убедимся, что в списке пакетов отобразятся только пакеты ARP или ICMP (рис. [-@fig:010]).

![Фильтр arp or icmp](/home/mavinogradova/skreens/hw3/10.png){#fig:010 width=70%}

Отображены ARP- и ICMP-пакеты. Видны ARP-запросы к `.11`, `.24`, `.13` и ICMP-эхо-запросы/ответы между `192.168.1.11` и `192.168.1.1`. Выделен ARP-запрос: `Ethernet II`, Src `AtwTechnolog_04:00:70`, Dst `Broadcast`.

Изучим эхо-запрос и эхо-ответ ICMP. Выберем первый указанный кадр ICMP — эхо-запрос (рис. [-@fig:011]).

![ICMP-пакеты в Wireshark](/home/mavinogradova/skreens/hw3/11.png){#fig:011 width=70%}

Видны 4 пары ICMP: Echo request (№ 13624, 13639, 13658, 13675) и Echo reply (№ 13625, 13640, 13659, 13676). Sequence Number: 8/2048, 9/2304, 10/2560, 11/2816. Длина кадров — 74 байта.

**Эхо-запрос ICMP (пакет № 13624).** В отчёте укажем длину кадра, к какому типу Ethernet относится кадр, определим MAC-адреса источника и шлюза, определим тип MAC-адресов (рис. [-@fig:012]).

![Разбор ICMP Echo request](/home/mavinogradova/skreens/hw3/12.png){#fig:012 width=70%}

**Эхо-ответ ICMP (пакет № 13625).** MAC-адреса поменялись местами (рис. [-@fig:013]).

![ICMP Echo reply в Wireshark](/home/mavinogradova/skreens/hw3/13.png){#fig:013 width=70%}

Ethernet II: Src `AtwTechnolog_04:00:70` (`14:c2:4d:04:00:70`), Dst `Intel_1c:01:5b` (`c4:3d:1a:1c:01:5b`). Wireshark подтверждает: LG bit = 0 (Globally unique), IG bit = 0 (Individual address) (рис. [-@fig:014]).

![Разбор ICMP Echo reply](/home/mavinogradova/skreens/hw3/14.png){#fig:014 width=70%}

**Сводная таблица ICMP:**

| Параметр | Эхо-запрос (№ 13624) | Эхо-ответ (№ 13625) |
|---|---|---|
| Длина кадра | 74 байта | 74 байта |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | c4:3d:1a:1c:01:5b (наш ПК) | 14:c2:4d:04:00:70 (шлюз) |
| MAC получателя | 14:c2:4d:04:00:70 (шлюз) | c4:3d:1a:1c:01:5b (наш ПК) |
| Тип MAC источника | индивидуальный, глобальный | индивидуальный, глобальный |
| Тип MAC получателя | индивидуальный, глобальный | индивидуальный, глобальный |


Изучим кадры данных протокола ARP. Изучим данные в полях заголовка Ethernet II. Рассмотрим ARP Request (рис. [-@fig:015]).

![ARP Request](/home/mavinogradova/skreens/hw3/15.png){#fig:015 width=70%}

В ARP-запросе Opcode = 1 (request), Sender MAC = `14:c2:4d:04:00:70`, Sender IP = `192.168.1.1`, Target MAC = `00:00:00:00:00:00` (неизвестен), Target IP = `192.168.1.11`.

Рассмотрим ARP Reply (рис. [-@fig:016]).

![ARP Reply](/home/mavinogradova/skreens/hw3/16.png){#fig:016 width=70%}

В ARP-ответе Opcode = 2 (reply), Sender MAC = `c4:3d:1a:1c:01:5b`, Sender IP = `192.168.1.11`, Target MAC = `14:c2:4d:04:00:70`, Target IP = `192.168.1.1`.

**Сводная таблица ARP:**

| Параметр | ARP Request (№ 411) | ARP Reply (№ 413) |
|---|---|---|
| Длина кадра | 60 байт | 42 байта |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | 14:c2:4d:04:00:70 (шлюз) | c4:3d:1a:1c:01:5b (наш ПК) |
| MAC получателя | ff:ff:ff:ff:ff:ff (broadcast) | 14:c2:4d:04:00:70 (шлюз) |
| Тип MAC источника | индивидуальный, глобальный | индивидуальный, глобальный |
| Тип MAC получателя | групповой, локально администрируемый | индивидуальный, глобальный |


Протокол ARP используется для определения MAC-адреса по известному IP-адресу. Когда роутер (`192.168.1.1`) хочет отправить пакет нашему ПК (`192.168.1.11`), но не знает его MAC, он рассылает широковещательный ARP-запрос на адрес `ff:ff:ff:ff:ff:ff`. Наш ПК отвечает ARP-ответом, в котором сообщает свой MAC `c4:3d:1a:1c:01:5b`.

Начнём новый процесс захвата трафика в Wireshark. В консоли пропигнуем по имени известный адрес: `ping rudn.ru`. Также выполним `ping -n 4 77.88.8.8` (рис. [-@fig:017]).

![Ping rudn.ru и ping 77.88.8.8](/home/mavinogradova/skreens/hw3/17.png){#fig:017 width=70%}

`ping rudn.ru` дал 100% потерь (сервер блокирует ICMP), а `ping -n 4 77.88.8.8` получил 4 ответа (0% потерь), TTL=52, время 115–122 мс.

В Wireshark остановим захват трафика. Изучим запросы и ответы протоколов ARP и ICMP. Определим MAC-адреса источника и получателя, определим тип MAC-адресов (рис. [-@fig:018]).

![Фильтр arp or icmp после ping 77.88.8.8](/home/mavinogradova/skreens/hw3/18.png){#fig:018 width=70%}

Видны ARP-запросы к `.11`, `.10`, `.24` и ICMP-пары: № 655/656, 668/669, 677/678, 692/693. Sequence Number: 23/5888, 24/6144, 25/6400, 26/6656.

**Разбор ICMP Echo request и Echo reply (рис. [-@fig:019]):**

![Разбор ICMP Echo request и Echo reply](/home/mavinogradova/skreens/hw3/19.png){#fig:019 width=70%}

| Параметр | Echo request (№ 655) | Echo reply (№ 656) |
|---|---|---|
| Длина кадра | 74 байта | 74 байта |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | c4:3d:1a:1c:01:5b (наш ПК) | 14:c2:4d:04:00:70 (шлюз) |
| MAC получателя | 14:c2:4d:04:00:70 (шлюз) | c4:3d:1a:1c:01:5b (наш ПК) |
| Тип MAC источника | индивидуальный, глобальный | индивидуальный, глобальный |
| Тип MAC получателя | индивидуальный, глобальный | индивидуальный, глобальный |

**Разбор ARP Request и ARP Reply (рис. [-@fig:020]):**

![Разбор ARP Request и ARP Reply](/home/mavinogradova/skreens/hw3/20.png){#fig:020 width=70%}

| Параметр | ARP Request (№ 411) | ARP Reply (№ 413) |
|---|---|---|
| Длина кадра | 60 байт | 42 байта |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | 14:c2:4d:04:00:70 (шлюз) | c4:3d:1a:1c:01:5b (наш ПК) |
| MAC получателя | ff:ff:ff:ff:ff:ff (broadcast) | 14:c2:4d:04:00:70 (шлюз) |
| Тип MAC источника | индивидуальный, глобальный | индивидуальный, глобальный |
| Тип MAC получателя | групповой, локально администрируемый | индивидуальный, глобальный |

## Анализ протоколов транспортного уровня в Wireshark

Запускаем Wireshark. Выбираем активный сетевой интерфейс «Беспроводная сеть». Убеждаемся, что начался процесс захвата трафика. В браузере переходим на сайт, работающий по протоколу HTTP: `http://neverssl.com/` (рис. [-@fig:021]).

![Сайт neverssl.com, работающий по HTTP](/home/mavinogradova/skreens/hw3/21.png){#fig:021 width=70%}

Открыт сайт `http://neverssl.com/` (без HTTPS). В адресной строке — `astoundingsplendiddsoothingsmile.neverssl.com/online/`.

В Wireshark в строке фильтра укажем `http` и проанализируем информацию по протоколу TCP в случае запросов и ответов (рис. [-@fig:022]).

![Фильтр http](/home/mavinogradova/skreens/hw3/22.png){#fig:022 width=70%}

Видны: `GET / HTTP/1.1` (№ 708), `HTTP/1.1 200 OK` (№ 720), `301 Moved Permanently` (№ 803), `GET /online/ HTTP/1.1` (№ 804), `GET /favicon.ico` (№ 816), `200 OK (PNG)` (№ 818). Также retransmission-пакеты с `408 Request Time-out`.

**Сводная таблица HTTP:**

| Параметр | HTTP-запрос (№ 708) | HTTP-ответ (№ 720) |
|---|---|---|
| Длина кадра | 528 байт | 867 байт |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | c4:3d:1a:1c:01:5b (наш ПК) | 14:c2:4d:04:00:70 (шлюз) |
| MAC получателя | 14:c2:4d:04:00:70 (шлюз) | c4:3d:1a:1c:01:5b (наш ПК) |
| IP источника | 192.168.1.11 | 34.223.124.45 |
| IP получателя | 34.223.124.45 | 192.168.1.11 |
| Порт источника | 52731 | 80 (HTTP) |
| Порт назначения | 80 (HTTP) | 52731 |
| Флаги TCP | PSH, ACK | PSH, ACK |
| Sequence Number | 1 (relative) | 1461 (relative) |
| Acknowledgment Number | 1 (relative) | 475 (relative) |
| Длина TCP-сегмента | 474 байта | 813 байт |
| Метод / Статус | GET / HTTP/1.1 | HTTP/1.1 200 OK |
| Заголовки | Host: neverssl.com, User-Agent: Mozilla/5.0 ... Edg/154, Accept-Encoding: gzip, deflate | Server: Apache/2.4.68, Content-Type: text/html; charset=UTF-8, Content-Encoding: gzip, Content-Length: 1900 |

HTTP работает поверх TCP, стандартный порт — 80. TCP обеспечивает надёжную доставку за счёт нумерации байтов и подтверждений. Клиент отправляет HTTP-запрос методом GET, сервер отвечает статусом 200 OK и передаёт запрошенный ресурс. Порт источника `52731` — динамический (клиентский), порт назначения `80` — стандартный HTTP. Флаги PSH+ACK означают передачу данных с подтверждением. Заголовок `Host` указывает имя сервера, `User-Agent` — браузер клиента, `Accept-Encoding` — поддерживаемые сжатия, `Content-Type` — тип передаваемого контента, `Content-Length` — размер тела ответа.

В Wireshark в строке фильтра укажем `dns` и проанализируем информацию по протоколу UDP в случае запросов и ответов (рис. [-@fig:023]).

![Фильтр dns](/home/mavinogradova/skreens/hw3/23.png){#fig:023 width=70%}

Видны DNS-запросы (`Standard query A c.msn.com`, № 185) и ответы (`Standard query response ... CNAME`, № 193). Transaction ID: 0x6486. Порт назначения — 53 (UDP).

**Сводная таблица DNS:**

| Параметр | DNS-запрос (№ 185) | DNS-ответ (№ 193) |
|---|---|---|
| Длина кадра | 69 байт | 213 байт |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | c4:3d:1a:1c:01:5b (наш ПК) | 14:c2:4d:04:00:70 (шлюз/DNS) |
| MAC получателя | 14:c2:4d:04:00:70 (шлюз/DNS) | c4:3d:1a:1c:01:5b (наш ПК) |
| IP источника | 192.168.1.11 | 192.168.1.1 |
| IP получателя | 192.168.1.1 | 192.168.1.11 |
| Транспорт | UDP | UDP |
| Порт источника | 57924 | 53 (DNS) |
| Порт назначения | 53 (DNS) | 57924 |
| Transaction ID | 0x6486 | 0x6486 |
| Содержимое | Standard query A c.msn.com | CNAME + A |

DNS работает поверх UDP, стандартный порт — 53. UDP не устанавливает соединение и не гарантирует доставку — это подходит для DNS, потому что запросы короткие, а при потере пакета клиент просто повторит запрос. DNS-запрос типа A запрашивает IPv4-адрес по имени домена. Ответ может содержать не только A-запись, но и CNAME-запись (псевдоним).

В Wireshark в строке фильтра укажем `quic` и проанализируем информацию по протоколу QUIC (рис. [-@fig:024]).

![Фильтр quic](/home/mavinogradova/skreens/hw3/24.png){#fig:024 width=70%}

Видны QUIC-пакеты: Initial-пакеты (№ 214 `DCID=50dd12acb8f6d5d4, PKN: 1`), № 215, № 236 (ответ `SCID=251a5c4e88004632, PKN: 1, ACK`), № 237–240 (Handshake, Protected Payload). Порт 443 (UDP).

**Сводная таблица QUIC:**

| Параметр | QUIC-запрос (№ 214) | QUIC-ответ (№ 236) |
|---|---|---|
| Длина кадра | 1292 байта | 82 байта |
| Тип Ethernet | Ethernet II | Ethernet II |
| MAC источника | c4:3d:1a:1c:01:5b (наш ПК) | 14:c2:4d:04:00:70 (шлюз) |
| MAC получателя | 14:c2:4d:04:00:70 (шлюз) | c4:3d:1a:1c:01:5b (наш ПК) |
| IP источника | 192.168.1.11 | 23.216.134.145 |
| IP получателя | 23.216.134.145 | 192.168.1.11 |
| Транспорт | UDP | UDP |
| Порт источника | 52015 | 443 |
| Порт назначения | 443 (QUIC) | 52015 |
| Тип пакета | Initial | Initial |
| Connection ID | DCID = 50dd12acb8f6d5d4 | SCID = 251a5c4e88004632 |
| Packet Number | 1 | 1 |
| Флаги | — | ACK |

QUIC — транспортный протокол поверх UDP, используется в HTTP/3, стандартный порт — 443. В отличие от TCP, где установление соединения выполняется через трёхступенчатый handshake, QUIC использует обмен Initial-пакетами с идентификаторами соединения (Connection ID). DCID и SCID позволяют соединению «переживать» смену сети.

Останавливаем захват трафика в Wireshark.

## Анализ handshake протокола TCP в Wireshark

Запускаем Wireshark. Выбираем активный сетевой интерфейс «Беспроводная сеть». Убеждаемся, что начался процесс захвата трафика. Для захвата пакетов TCP используем соединение по HTTP с сайтом `http://neverssl.com/`.

В Wireshark проанализируем handshake протокола TCP. Найдём три пакета handshake (рис. [-@fig:025]).

![Три пакета TCP handshake](/home/mavinogradova/skreens/hw3/25.png){#fig:025 width=70%}

**Сводная таблица TCP handshake:**

| Параметр | Пакет 1 (SYN) | Пакет 2 (SYN+ACK) | Пакет 3 (ACK) |
|---|---|---|---|
| № пакета | 24 | 25 | 26 |
| Длина кадра | 66 байт | 66 байт | 54 байта |
| Тип Ethernet | Ethernet II | Ethernet II | Ethernet II |
| MAC источника | c4:3d:1a:1c:01:5b | 14:c2:4d:04:00:70 | c4:3d:1a:1c:01:5b |
| MAC получателя | 14:c2:4d:04:00:70 | c4:3d:1a:1c:01:5b | 14:c2:4d:04:00:70 |
| IP источника | 192.168.1.11 | 57.144.223.32 | 192.168.1.11 |
| IP получателя | 57.144.223.32 | 192.168.1.11 | 57.144.223.32 |
| Порт источника | 14063 | 443 | 14063 |
| Порт назначения | 443 | 14063 | 443 |
| Флаги TCP | SYN | SYN, ACK | ACK |
| Sequence Number | ISSa = 2118954237 | ISSb = 1668721569 | 2118954238 |
| Acknowledgment | 0 | 2118954238 | 1668721570 |
| Опции | MSS, WS, SACK_PERM | MSS, WS, SACK_PERM | — |

Трёхступенчатый handshake TCP: SYN → SYN+ACK → ACK. Sequence Number клиента (ISSa = 2118954237) и сервера (ISSb = 1668721569) выбираются независимо. Номера подтверждений увеличиваются на 1: клиент подтверждает ISSa+1, сервер — ISSb+1. Опции TCP (MSS, Window Scale, SACK permitted) передаются только в SYN и SYN+ACK — в третьем пакете их нет, поэтому он короче (54 байта против 66). После установления соединения начинается передача данных — следующий пакет (№ 27) уже содержит TLS Client Hello.

В Wireshark в меню «Статистика» выберем «График Потока» (рис. [-@fig:026]).

![График потока TCP](/home/mavinogradova/skreens/hw3/26.png){#fig:026 width=70%}

На графике потока отображены три пакета handshake: `14063 → 443` SYN, `443 → 14063` SYN+ACK, `14063 → 443` ACK. Сразу после handshake начинается передача прикладных данных — `Client Hello (SNI=web.whatsapp.com)` — начало TLS-соединения. Стрелки показывают направление передачи: вправо — от клиента к серверу, влево — от сервера к клиенту. График наглядно демонстрирует, что handshake состоит из трёх шагов, после чего начинается передача данных.

Останавливаем захват трафика в Wireshark.

## Выводы

В ходе лабораторной работы были изучены возможности анализатора трафика Wireshark для исследования кадров Ethernet и PDU транспортного и прикладного уровней стека TCP/IP. Определены MAC-адреса сетевых интерфейсов, разобрана их структура. Проанализированы ARP-запросы и ARP-ответы, ICMP-эхо-запросы и эхо-ответы: установлено, что ARP-запросы рассылаются широковещательно, а ответы приходят напрямую от устройства. Изучены протоколы прикладного уровня: HTTP, DNS, QUIC. Разобран трёхступенчатый TCP handshake, определены начальные Sequence Number клиента и сервера и их изменения при подтверждении. Wireshark позволяет детально исследовать структуру сетевых пакетов на всех уровнях стека TCP/IP.

# Ответы на контрольные вопросы

Контрольные вопросы в методических указаниях к домашней работе №3 не приведены.

# Список литературы

1. Методические указания к лабораторной работе № 3 «Анализ трафика в Wireshark».
2. Официальная документация Wireshark. — URL:
