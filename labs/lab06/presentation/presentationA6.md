---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №6
subtitle: Установка и настройка системы управления базами данных MariaDB
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
  * [1132240691@rudn.ru](mailto:1132240691@rudn.ru)
  * <https://github.com/mavinogradova/study_2026-2027>

:::
::::::::::::::

# Цель работы

Приобретение практических навыков по установке и конфигурированию системы управления базами данных на примере программного обеспечения MariaDB.

# Задание

1. Установите необходимые для работы MariaDB пакеты (см. раздел 6.4.1).
2. Настройте в качестве кодировки символов по умолчанию utf8 в базах данных.
3. В базе данных MariaDB создайте тестовую базу addressbook, содержащую таблицу city с полями name и city, т.е., например, для некоторого сотрудника указан город, в котором он работает (см. раздел 6.4.1).
4. Создайте резервную копию базы данных addressbook и восстановите из неё данные (см. раздел 6.4.1).
5. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке базы данных MariaDB во внутреннем окружении виртуальной машины server. Соответствующим образом следует внести изменения в Vagrantfile (см. раздел 6.4.5).

# Выполнение лабораторной работы

## Установка MariaDB. Установка пакетов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину server под пользователем mavinogradova, переход в режим суперпользователя и установка пакетов mariadb и mariadb-server](/home/mavinogradova/skreens/lbA6/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Установка MariaDB. Просмотр конфигурационных файлов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр конфигурационных файлов MariaDB](/home/mavinogradova/skreens/lbA6/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Установка MariaDB. Запуск и включение службы

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск и включение службы mariadb, проверка её статуса](/home/mavinogradova/skreens/lbA6/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Установка MariaDB. Проверка прослушиваемого порта

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка прослушиваемого порта](/home/mavinogradova/skreens/lbA6/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Установка MariaDB. Настройка безопасности

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск mysql_secure_installation](/home/mavinogradova/skreens/lbA6/5.png){#fig:005 width=70%}

:::
::::::::::::::

## Установка MariaDB. Вход в базу данных

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход в базу данных с правами администратора](/home/mavinogradova/skreens/lbA6/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Установка MariaDB. Список команд клиента

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Список команд клиента MariaDB](/home/mavinogradova/skreens/lbA6/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Установка MariaDB. Список доступных баз данных

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод списка доступных баз данных MariaDB](/home/mavinogradova/skreens/lbA6/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Конфигурация кодировки. Статус до изменения

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод статуса MariaDB до изменения кодировки](/home/mavinogradova/skreens/lbA6/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Конфигурация кодировки. Создание файла utf8.cnf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание файла конфигурации utf8.cnf](/home/mavinogradova/skreens/lbA6/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Конфигурация кодировки. Содержимое utf8.cnf

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое файла utf8.cnf](/home/mavinogradova/skreens/lbA6/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Конфигурация кодировки. Перезапуск службы

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск службы mariadb](/home/mavinogradova/skreens/lbA6/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Конфигурация кодировки. Статус после изменения

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вывод статуса MariaDB после изменения кодировки](/home/mavinogradova/skreens/lbA6/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Создание базы данных. Создание addressbook

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание базы данных addressbook](/home/mavinogradova/skreens/lbA6/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Создание базы данных. Таблица city

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание таблицы city и заполнение её данными](/home/mavinogradova/skreens/lbA6/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Создание базы данных. Пользователь и права

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание пользователя и выдача прав](/home/mavinogradova/skreens/lbA6/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Создание базы данных. Проверка через mysqlshow

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр списка баз данных и таблиц](/home/mavinogradova/skreens/lbA6/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Резервные копии. Обычная копия

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание каталога /var/backup и обычной резервной копии](/home/mavinogradova/skreens/lbA6/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Резервные копии. Сжатые копии

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание сжатых резервных копий](/home/mavinogradova/skreens/lbA6/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Резервные копии. Восстановление из .sql

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление базы данных из несжатой резервной копии](/home/mavinogradova/skreens/lbA6/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Резервные копии. Восстановление из .gz

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление базы данных из сжатой резервной копии](/home/mavinogradova/skreens/lbA6/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Внесение изменений в окружение. Копирование файлов

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационного файла и резервных копий в каталог провижининга](/home/mavinogradova/skreens/lbA6/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Внесение изменений в окружение. Создание mysql.sh

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание исполняемого файла mysql.sh](/home/mavinogradova/skreens/lbA6/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Внесение изменений в окружение. Содержимое mysql.sh

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое скрипта провижининга mysql.sh](/home/mavinogradova/skreens/lbA6/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Внесение изменений в окружение. Правка Vagrantfile

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Фрагмент Vagrantfile с добавленным блоком провижининга](/home/mavinogradova/skreens/lbA6/25.png){#fig:025 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и конфигурированию СУБД MariaDB. Установлены пакеты `mariadb` и `mariadb-server`, служба включена в автозапуск и запущена. Настроена кодировка символов по умолчанию utf8 через файл `utf8.cnf`. Создана тестовая база данных `addressbook` с таблицей `city` и тремя записями, создан пользователь с правами `SELECT`, `INSERT`, `UPDATE`, `DELETE` на эту базу. Освоены резервное копирование (несжатое, сжатое, с датой в имени) и восстановление базы данных. Написан скрипт провижининга `mysql.sh`, автоматизирующий установку и настройку MariaDB, и внесены соответствующие изменения в `Vagrantfile`.
