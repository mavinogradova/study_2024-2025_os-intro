---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №5
subtitle: Расширенная настройка HTTP-сервера Apache
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 1 октября 2026

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

Приобретение практических навыков по расширенному конфигурированию HTTP-сервера Apache в части безопасности и возможности использования PHP.

# Задание

1. Сгенерируйте криптографический ключ и самоподписанный сертификат безопасности для перехода веб-сервера с HTTP на HTTPS.
2. Настройте веб-сервер для работы с PHP.
3. Напишите скрипт для Vagrant, фиксирующий действия по расширенной настройке HTTP-сервера.

# Выполнение лабораторной работы

# Конфигурирование HTTP-сервера для работы через HTTPS

## Создаём каталог для ключей и символическую ссылку.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание каталога /etc/pki/tls/private и симлинка](/home/mavinogradova/skreens/lbA5/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Генерируем ключ и сертификат, заполняем поля сертификата.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Генерация ключа и сертификата](/home/mavinogradova/skreens/lbA5/2.png){#fig:002 width=70%}

:::
::::::::::::::

## Редактируем конфигурационный файл виртуального хоста.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Конфиг www.mavinogradova.net.conf](/home/mavinogradova/skreens/lbA5/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Настраиваем межсетевой экран и перезапускаем веб-сервер.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Добавление https в firewall](/home/mavinogradova/skreens/lbA5/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Подключаемся к client для проверки HTTPS.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подключение к client](/home/mavinogradova/skreens/lbA5/5.png){#fig:005 width=70%}

:::
::::::::::::::

# Анализ работы HTTPS

## Проверяем HTTPS и смотрим сертификат.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка HTTPS и сертификата](/home/mavinogradova/skreens/lbA5/6.png){#fig:006 width=70%}

:::
::::::::::::::

# Конфигурирование HTTP-сервера для работы с PHP

## Устанавливаем пакет PHP.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Установка PHP](/home/mavinogradova/skreens/lbA5/7.png){#fig:007 width=70%}

:::
::::::::::::::

## Заменяем index.html на index.php.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Замена index.html на index.php](/home/mavinogradova/skreens/lbA5/8.png){#fig:008 width=70%}

:::
::::::::::::::

## Содержимое index.php.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Файл index.php](/home/mavinogradova/skreens/lbA5/9.png){#fig:009 width=70%}

:::
::::::::::::::

## Права доступа, SELinux, перезапуск httpd.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![chown, restorecon, restart httpd](/home/mavinogradova/skreens/lbA5/10.png){#fig:010 width=70%}

:::
::::::::::::::

## Проверяем работу PHP на client.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Страница phpinfo()](/home/mavinogradova/skreens/lbA5/11.png){#fig:011 width=70%}

:::
::::::::::::::

# Внесение изменений в настройки внутреннего окружения

## Копируем конфигурационные файлы в провижининг.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Копирование в провижининг](/home/mavinogradova/skreens/lbA5/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Редактируем скрипт http.sh: добавляем PHP и HTTPS.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Скрипт http.sh](/home/mavinogradova/skreens/lbA5/13.png){#fig:013 width=70%}

:::
::::::::::::::

# Выводы

Приобретены практические навыки по расширенному конфигурированию HTTP-сервера Apache в части безопасности и использования PHP. Сгенерирован ключ и самоподписанный сертификат, настроен HTTPS с автоматическим редиректом с HTTP, настроен межсетевой экран, установлен PHP, заменён `index.html` на `index.php`, скорректирован скрипт `http.sh` для Vagrant.
