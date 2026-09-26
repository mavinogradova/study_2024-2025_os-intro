---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №4
subtitle: Базовая настройка HTTP-сервера Apache
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 26 сентября 2026

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

Приобретение практических навыков по установке и базовому конфигурированию HTTP-сервера Apache.

# Задание

1. Установите необходимые для работы HTTP-сервера пакеты (см. раздел 4.4.1).
2. Запустите HTTP-сервер с базовой конфигурацией и проанализируйте его работу (см. разделы 4.4.2 и 4.4.3).
3. Настройте виртуальный хостинг (см. раздел 4.4.4).
4. Напишите скрипт для Vagrant, фиксирующий действия по установке и настройке HTTP-сервера во внутреннем окружении виртуальной машины server. Соответствующим образом внесите изменения в Vagrantfile (см. раздел 4.4.5).

# Выполнение лабораторной работы

# Установка HTTP-сервера

## Загружаем операционную систему и переходим в рабочий каталог с проектом. Запускаем виртуальную машину server. На виртуальной машине server входим под своим пользователем и открываем терминал. Переходим в режим суперпользователя.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Вход на виртуальную машину server под пользователем mavinogradova и переход в режим суперпользователя](/home/mavinogradova/skreens/lbA4/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Просматриваем список доступных групп пакетов с помощью команды `LANG=C yum grouplist`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр доступных групп пакетов командой LANG=C yum grouplist](/home/mavinogradova/skreens/lbA4/2.png){#fig:002 width=70%}

:::
::::::::::::::

## В списке доступных групп присутствуют Server, Minimal Install, Workstation. Устанавливаем из репозитория стандартный веб-сервер командой `dnf -y groupinstall "Basic Web Server"`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка группы пакетов Basic Web Server](/home/mavinogradova/skreens/lbA4/3.png){#fig:003 width=70%}

:::
::::::::::::::

# Базовое конфигурирование HTTP-сервера

## Просматриваем и комментируем содержание конфигурационных файлов в каталогах `/etc/httpd/conf` и `/etc/httpd/conf.d`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое каталогов /etc/httpd/conf и /etc/httpd/conf.d](/home/mavinogradova/skreens/lbA4/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Вносим изменения в настройки межсетевого экрана узла server, разрешив работу с http.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Настройка межсетевого экрана: добавление службы http](/home/mavinogradova/skreens/lbA4/5.png){#fig:005 width=70%}

:::
::::::::::::::

## В дополнительном терминале запускаем в режиме реального времени расширенный лог системных сообщений.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск расширенного лога системных сообщений journalctl -x -f](/home/mavinogradova/skreens/lbA4/6.png){#fig:006 width=70%}

:::
::::::::::::::

## В первом терминале активируем и запускаем HTTP-сервер командами `systemctl enable httpd` и `systemctl start httpd`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Активация и запуск HTTP-сервера](/home/mavinogradova/skreens/lbA4/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Просмотрев расширенный лог системных сообщений, убеждаемся, что веб-сервер успешно запустился.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка статуса HTTP-сервера](/home/mavinogradova/skreens/lbA4/8.png){#fig:008 width=70%}

:::
::::::::::::::

# Анализ работы HTTP-сервера

## Запускаем виртуальную машину client.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск виртуальной машины client](/home/mavinogradova/skreens/lbA4/9.png){#fig:009 width=70%}

:::
::::::::::::::

## На виртуальной машине server просматриваем лог ошибок работы веб-сервера командой `tail -f /var/log/httpd/error_log`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Просмотр лога ошибок работы веб-сервера](/home/mavinogradova/skreens/lbA4/10.png){#fig:010 width=70%}

:::
::::::::::::::

## На виртуальной машине client запускаем браузер и в адресной строке вводим 192.168.1.1.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Обращение к веб-серверу с виртуальной машины client](/home/mavinogradova/skreens/lbA4/11.png){#fig:011 width=70%}

:::
::::::::::::::

## На виртуальной машине server запускаем мониторинг доступа к веб-серверу командой `tail -f /var/log/httpd/access_log`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Мониторинг доступа к веб-серверу](/home/mavinogradova/skreens/lbA4/12.png){#fig:012 width=70%}

:::
::::::::::::::

# Настройка виртуального хостинга для HTTP-сервера

## Останавливаем работу DNS-сервера для внесения изменений в файлы описания DNS-зон.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Остановка DNS-сервера](/home/mavinogradova/skreens/lbA4/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Добавляем запись для HTTP-сервера в конце файла прямой DNS-зоны `/var/named/master/fz/mavinogradova.net`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление записи www в файл прямой DNS-зоны](/home/mavinogradova/skreens/lbA4/14.png){#fig:014 width=70%}

:::
::::::::::::::

## Добавляем запись в конце файла обратной DNS-зоны `/var/named/master/rz/192.168.1`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление PTR-записи в файл обратной DNS-зоны](/home/mavinogradova/skreens/lbA4/15.png){#fig:015 width=70%}

:::
::::::::::::::

## Перезапускаем DNS-сервер командой `systemctl start named`. В каталоге `/etc/httpd/conf.d` создаём файлы `server.mavinogradova.net.conf` и `www.mavinogradova.net.conf`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запуск DNS-сервера и создание конфигурационных файлов виртуальных хостов](/home/mavinogradova/skreens/lbA4/16.png){#fig:016 width=70%}

:::
::::::::::::::

## Открываем на редактирование файл `server.mavinogradova.net.conf` и вносим необходимое содержание.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое файла server.mavinogradova.net.conf](/home/mavinogradova/skreens/lbA4/17.png){#fig:017 width=70%}

:::
::::::::::::::

## Открываем на редактирование файл `www.mavinogradova.net.conf` и вносим необходимое содержание.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое файла www.mavinogradova.net.conf](/home/mavinogradova/skreens/lbA4/18.png){#fig:018 width=70%}

:::
::::::::::::::

## Переходим в каталог `/var/www/html` и создаём тестовые страницы для виртуальных веб-серверов server.mavinogradova.net и www.mavinogradova.net. Для виртуального веб-сервера server.mavinogradova.net создаём каталог и файл index.html.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание каталога и файла index.html для виртуального веб-сервера server.mavinogradova.net](/home/mavinogradova/skreens/lbA4/19.png){#fig:019 width=70%}

:::
::::::::::::::

## Открываем на редактирование файл index.html и вносим содержание для виртуального веб-сервера server.mavinogradova.net.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое index.html виртуального веб-сервера server.mavinogradova.net](/home/mavinogradova/skreens/lbA4/20.png){#fig:020 width=70%}

:::
::::::::::::::

## Создаём каталог и файл index.html для виртуального веб-сервера www.mavinogradova.net.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание каталога и файла index.html для виртуального веб-сервера www.mavinogradova.net](/home/mavinogradova/skreens/lbA4/21.png){#fig:021 width=70%}

:::
::::::::::::::

## Открываем на редактирование файл index.html и вносим содержание для виртуального веб-сервера www.mavinogradova.net.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое index.html виртуального веб-сервера www.mavinogradova.net](/home/mavinogradova/skreens/lbA4/22.png){#fig:022 width=70%}

:::
::::::::::::::

## Скорректируем права доступа в каталог с веб-контентом командой `chown -R apache:apache /var/www`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Корректировка прав доступа в каталог с веб-контентом](/home/mavinogradova/skreens/lbA4/23.png){#fig:023 width=70%}

:::
::::::::::::::

## Восстанавливаем контекст безопасности в SELinux командами `restorecon -vR /etc`, `restorecon -vR /var/named`, `restorecon -vR /var/www`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Восстановление контекста безопасности в SELinux](/home/mavinogradova/skreens/lbA4/24.png){#fig:024 width=70%}

:::
::::::::::::::

## Перезапускаем HTTP-сервер командой `systemctl restart httpd`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Перезапуск HTTP-сервера](/home/mavinogradova/skreens/lbA4/25.png){#fig:025 width=70%}

:::
::::::::::::::

## На виртуальной машине client убеждаемся в корректном доступе к веб-серверу по адресам server.mavinogradova.net и www.mavinogradova.net.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка корректного доступа к виртуальным веб-серверам с машины client](/home/mavinogradova/skreens/lbA4/26.png){#fig:026 width=70%}

:::
::::::::::::::

# Внесение изменений в настройки внутреннего окружения виртуальной машины

## Переходим в каталог `/vagrant/provision/server`. Создаём каталоги и копируем конфигурационные файлы и контент.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование конфигурационных файлов и зон в каталог provision](/home/mavinogradova/skreens/lbA4/27.png){#fig:027 width=70%}

:::
::::::::::::::

## В каталоге `/vagrant/provision/server` создаём исполняемый файл `http.sh` командами `touch http.sh` и `chmod +x http.sh`.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание исполняемого файла http.sh](/home/mavinogradova/skreens/lbA4/28.png){#fig:028 width=70%}

:::
::::::::::::::

## Открыв файл http.sh на редактирование, прописываем в нём скрипт провижининга.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое скрипта провижининга http.sh](/home/mavinogradova/skreens/lbA4/29.png){#fig:029 width=70%}

:::
::::::::::::::

## Для отработки созданного скрипта во время загрузки виртуальных машин в конфигурационном файле Vagrantfile добавляем запись.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление записи о провижининге в Vagrantfile](/home/mavinogradova/skreens/lbA4/30.png){#fig:030 width=70%}

:::
::::::::::::::

# Выводы

В ходе лабораторной работы были приобретены практические навыки по установке и базовому конфигурированию HTTP-сервера Apache. Были установлены необходимые для работы HTTP-сервера пакеты (группа «Basic Web Server»), включающие httpd, httpd-manual, mod_fcgid, mod_ssl и зависимости. HTTP-сервер запущен с базовой конфигурацией, проанализирована его работа с помощью логов `/var/log/httpd/error_log` и `/var/log/httpd/access_log`. Настроен виртуальный хостинг по двум DNS-адресам — server.mavinogradova.net и www.mavinogradova.net. Написан скрипт для Vagrant `http.sh`, фиксирующий действия по установке и настройке HTTP-сервера во внутреннем окружении виртуальной машины server, и внесены соответствующие изменения в Vagrantfile.
