---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №10
subtitle: Настройка аутентификации SMTP-сервера
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 2 ноября 2026

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

Приобретение практических навыков по конфигурированию SMTP-сервера в части настройки аутентификации.

# Задание

1. Настройте Dovecot для работы с LMTP (см. раздел 10.4.1).
2. Настройте аутентификацию посредством SASL на SMTP-сервере (см. раздел 10.4.2).
3. Настройте работу SMTP-сервера поверх TLS (см. раздел 10.4.3).
4. Скорректируйте скрипт для Vagrant, фиксирующий действия расширенной настройки SMTP-сервера во внутреннем окружении виртуальной машины server (см. раздел 10.4.4).

# Выполнение лабораторной работы

## Настройка LMTP. Вход на server

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину server и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA10/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Настройка LMTP. Мониторинг логов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск мониторинга логов почтовой службы](/home/mavinogradova/skreens/lbA10/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Настройка LMTP. dovecot.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка списка протоколов в файле dovecot.conf](/home/mavinogradova/skreens/lbA10/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Настройка LMTP. 10-master.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка сервиса lmtp в файле 10-master.conf](/home/mavinogradova/skreens/lbA10/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Настройка LMTP. 10-auth.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка формата имени пользователя в файле 10-auth.conf](/home/mavinogradova/skreens/lbA10/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Настройка LMTP. postconf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Переопределение транспорта доставки в Postfix](/home/mavinogradova/skreens/lbA10/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Настройка LMTP. Перезапуск служб

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск служб Postfix и Dovecot](/home/mavinogradova/skreens/lbA10/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Проверка LMTP. Вход на client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину client](/home/mavinogradova/skreens/lbA10/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Проверка LMTP. Отправка письма

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Отправка тестового письма с клиента](/home/mavinogradova/skreens/lbA10/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Проверка LMTP. Логи maillog

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Мониторинг логов maillog при доставке письма](/home/mavinogradova/skreens/lbA10/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Проверка LMTP. Просмотр ящика

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр почтового ящика пользователя](/home/mavinogradova/skreens/lbA10/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Настройка SASL. 10-master.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка службы аутентификации в 10-master.conf](/home/mavinogradova/skreens/lbA10/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Настройка SASL. Ограничения Postfix

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка SASL и ограничений в Postfix](/home/mavinogradova/skreens/lbA10/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Настройка SASL. master.cf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование файла master.cf для включения SASL](/home/mavinogradova/skreens/lbA10/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Настройка SASL. Перезапуск служб

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск служб после настройки аутентификации](/home/mavinogradova/skreens/lbA10/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Проверка SASL. Установка telnet на client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка telnet на клиенте](/home/mavinogradova/skreens/lbA10/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Проверка SASL. base64

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Получение строки для аутентификации в base64](/home/mavinogradova/skreens/lbA10/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Проверка SASL. telnet

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка SMTP-аутентификации через telnet](/home/mavinogradova/skreens/lbA10/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Настройка TLS. Сертификаты

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка сертификатов и TLS в Postfix](/home/mavinogradova/skreens/lbA10/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Настройка TLS. master.cf (порт 587)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка порта 587 в master.cf](/home/mavinogradova/skreens/lbA10/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Настройка TLS. firewall

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка межсетевого экрана для smtp-submission](/home/mavinogradova/skreens/lbA10/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Проверка TLS. openssl

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подключение к порту 587 через openssl](/home/mavinogradova/skreens/lbA10/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Проверка TLS. Аутентификация

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка аутентификации через openssl](/home/mavinogradova/skreens/lbA10/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Проверка TLS. Evolution до настройки

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Список писем в Evolution до настройки](/home/mavinogradova/skreens/lbA10/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Проверка TLS. Настройка Evolution

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка учетной записи в Evolution](/home/mavinogradova/skreens/lbA10/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Проверка TLS. Успешная отправка

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подтверждение успешной отправки и получения в Evolution](/home/mavinogradova/skreens/lbA10/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Окружение ВМ. Копирование конфигов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационных файлов в каталог провижининга](/home/mavinogradova/skreens/lbA10/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Окружение ВМ. mail.sh на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта provision/server/mail.sh](/home/mavinogradova/skreens/lbA10/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Окружение ВМ. mail.sh на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта provision/client/mail.sh](/home/mavinogradova/skreens/lbA10/29.png){#fig:029 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были приобретены практические навыки по конфигурированию SMTP-сервера в части настройки аутентификации. Был настроен Dovecot для работы с LMTP, что позволило организовать фильтрацию и доставку почты на стороне сервера. Настроена аутентификация SASL для SMTP-сервера (Postfix), а также проверена ее работоспособность. Сконфигурирован SMTP-сервер для безопасной работы поверх TLS (порт 587) и проверена корректность отправки почты через почтовый клиент. Все действия расширенной настройки SMTP-сервера автоматизированы с помощью скриптов Vagrant.
