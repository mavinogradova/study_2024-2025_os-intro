---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №9
subtitle: Настройка POP3/IMAP сервера
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 24 октября 2026

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

Приобретение практических навыков по установке и простейшему конфигурированию POP3/IMAP-сервера.

# Задание

1. Установите на виртуальной машине server Dovecot и Telnet для дальнейшей проверки корректности работы почтового сервера (см. раздел 9.4.1).
2. Настройте Dovecot (см. раздел 9.4.2).
3. Установите на виртуальной машине client программу для чтения почты Evolution и настройте её для манипуляций с почтой вашего пользователя. Проверьте корректность работы почтового сервера как с виртуальной машины server, так и с виртуальной машины client (см. раздел 9.4.3).
4. Измените скрипт для Vagrant, фиксирующий действия по установке и настройке Postfix и Dovecot во внутреннем окружении виртуальной машины server, создайте скрипт для Vagrant, фиксирующий действия по установке Evolution во внутреннем окружении виртуальной машины client. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 9.4.4).

# Выполнение лабораторной работы

## Установка Dovecot. Вход на server и установка пакетов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину server, переход в режим суперпользователя и установка пакетов dovecot и telnet](/home/mavinogradova/skreens/lbA9/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Настройка Dovecot. dovecot.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка списка протоколов в файле dovecot.conf](/home/mavinogradova/skreens/lbA9/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Настройка Dovecot. 10-auth.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка метода аутентификации в файле 10-auth.conf](/home/mavinogradova/skreens/lbA9/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Настройка Dovecot. auth-system.conf.ext (passdb)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка настройки passdb в файле auth-system.conf.ext](/home/mavinogradova/skreens/lbA9/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Настройка Dovecot. auth-system.conf.ext (userdb)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка настройки userdb в файле auth-system.conf.ext](/home/mavinogradova/skreens/lbA9/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Настройка Dovecot. 10-mail.conf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка местоположения почтовых ящиков в файле 10-mail.conf](/home/mavinogradova/skreens/lbA9/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Настройка Dovecot. Postfix и firewall

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка Postfix и межсетевого экрана для POP3/IMAP](/home/mavinogradova/skreens/lbA9/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Настройка Dovecot. SELinux и перезапуск служб

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление контекста SELinux, перезапуск Postfix и запуск Dovecot](/home/mavinogradova/skreens/lbA9/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Проверка работы. Мониторинг логов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск мониторинга логов почтовой службы](/home/mavinogradova/skreens/lbA9/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Проверка работы. Просмотр писем (mail)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр имеющихся писем утилитой mail](/home/mavinogradova/skreens/lbA9/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Проверка работы. Просмотр mailbox (doveadm)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр списка почтовых ящиков пользователя через doveadm](/home/mavinogradova/skreens/lbA9/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Проверка работы. Вход на client

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину client и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA9/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Проверка работы. Установка Evolution

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка почтового клиента Evolution](/home/mavinogradova/skreens/lbA9/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Проверка работы. Запуск Evolution

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Графическое окружение виртуальной машины client с установленным Evolution](/home/mavinogradova/skreens/lbA9/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Настройка Evolution. Identity

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка учетной записи в Evolution (вкладка Identity)](/home/mavinogradova/skreens/lbA9/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Настройка Evolution. IMAP

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка получения почты (IMAP) в Evolution](/home/mavinogradova/skreens/lbA9/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Настройка Evolution. SMTP

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка отправки почты (SMTP) в Evolution](/home/mavinogradova/skreens/lbA9/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Настройка Evolution. Главное окно

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Главное окно Evolution после настройки учетной записи](/home/mavinogradova/skreens/lbA9/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Отправка писем. Test 1

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание первого тестового письма](/home/mavinogradova/skreens/lbA9/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Отправка писем. Test 3

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание и отправка третьего тестового письма](/home/mavinogradova/skreens/lbA9/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Отправка писем. Test 4

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание четвертого тестового письма](/home/mavinogradova/skreens/lbA9/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Отправка писем. Finish Test

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание финального тестового письма](/home/mavinogradova/skreens/lbA9/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Отправка писем. Сертификат

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Предупреждение о небезопасном SSL/TLS сертификате](/home/mavinogradova/skreens/lbA9/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Отправка писем. Пароль

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Ввод пароля для учетной записи](/home/mavinogradova/skreens/lbA9/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Результат. Папка Sent

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Папка Sent с отправленными письмами](/home/mavinogradova/skreens/lbA9/25.png){#fig:025 width=70%}

:::
::::::::::::::

## Результат. Папка Inbox

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Папка Inbox с полученными письмами](/home/mavinogradova/skreens/lbA9/26.png){#fig:026 width=70%}

:::
::::::::::::::

## Результат. Проверка статуса doveadm

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка статуса почтового ящика через doveadm](/home/mavinogradova/skreens/lbA9/27.png){#fig:027 width=70%}

:::
::::::::::::::

## Результат. Мониторинг логов maillog

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Мониторинг логов maillog во время отправки и получения писем](/home/mavinogradova/skreens/lbA9/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Проверка через Telnet (POP3)

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка работы POP3 через Telnet](/home/mavinogradova/skreens/lbA9/29.png){#fig:029 width=70%}

:::
::::::::::::::

## Окружение ВМ. Копирование конфигов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационных файлов Dovecot в каталог провижининга](/home/mavinogradova/skreens/lbA9/30.png){#fig:030 width=70%}

:::
::::::::::::::

## Окружение ВМ. mail.sh на сервере

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта provision/server/mail.sh](/home/mavinogradova/skreens/lbA9/31.png){#fig:031 width=70%}

:::
::::::::::::::

## Окружение ВМ. mail.sh на клиенте

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Редактирование скрипта provision/client/mail.sh](/home/mavinogradova/skreens/lbA9/32.png){#fig:032 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и простейшему конфигурированию POP3/IMAP-сервера. Был установлен и настроен Dovecot на сервере, настроены протоколы IMAP и POP3, методы аутентификации и расположение почтовых ящиков (формат Maildir). Проверена работа почтового сервера с помощью утилиты Evolution на клиентской машине, а также с использованием протокола Telnet. Автоматизированы действия по установке и настройке Postfix и Dovecot с помощью скриптов провижининга для Vagrant, а также установка клиента Evolution.

# Список литературы

1. Методические указания к лабораторной работе № 9 «Настройка POP3/IMAP сервера».
2. Dovecot Documentation. — URL: https://dovecot.org/documentation (visited on 09/13/2021).
3. Postfix Documentation. — URL: http://www.postfix.org/documentation.html (visited on 09/13/2021).
