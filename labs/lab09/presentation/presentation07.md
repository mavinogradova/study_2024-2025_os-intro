---
## Front matter
lang: ru-RU
title: Отчет по лабораторной работе №7
subtitle: Элементы криптографии. Однократное гаммирование
author:
  - Виноградова М.А
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 15 мая 2026

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
  * студентка НПИбд-01-24
  * номер студ. билета 1132240691
  * Российский университет дружбы народов
  * [1132240691@pfur.ru](mailto:k1132240691@pfur.ru)
  * <https://github.com/mavinogradova/study_2024-2025_infosec-intro>
  
::: 
::::::::::::::

## Цель работы

Освоить на практике применение режима однократного гаммирования (шифрование XOR) — изучить процесс шифрования и расшифрования данных, а также научиться подбирать ключ шифрования при известном открытом тексте и шифротексте.
 

## Задание

Необходимо разработать приложение, которое позволяет:

1. **Зашифровать** открытый текст с использованием известного ключа.
2. **Подобрать ключ**, с помощью которого шифротекст может быть преобразован в заданный фрагмент текста — фразу **«С Новым Годом, друзья!»**.

## 

Выполнение работы

## Была проверена версия Python.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Проверка версии Python](/home/mavinogradova/skreens/lb7/1.png){#fig:001 width=70%}

:::
::::::::::::::

## Создана рабочая директория и выполнен переход в неё

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Создание рабочей директории](/home/mavinogradova/skreens/lb7/2.png){#fig:002 width=70%}
:::
::::::::::::::

## Был создан скрипт `gamma.py`

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое gamma.py](/home/mavinogradova/skreens/lb7/3.png){#fig:003 width=70%}

:::
::::::::::::::

## Скрипту были выданы права на выполнение.
:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Права на выполнение](/home/mavinogradova/skreens/lb7/4.png){#fig:004 width=70%}

:::
::::::::::::::

## Был получен HEX-код сообщения «С Новым Годом, друзья!» в кодировке CP1251.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![HEX сообщения](/home/mavinogradova/skreens/lb7/5.png){#fig:005 width=70%}

:::
::::::::::::::

## В переменную `CIPHER` был сохранён шифротекст из примера лабораторной работы

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Запись шифротекста в переменную CIPHER](/home/mavinogradova/skreens/lb7/6.png){#fig:006 width=70%}

:::
::::::::::::::

## Была проверена длина сообщения в байтах (22 байта).
:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Длина сообщения](/home/mavinogradova/skreens/lb7/7.png){#fig:007 width=70%}


:::
::::::::::::::

## Был сгенерирован случайный ключ

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Генерация ключа](/home/mavinogradova/skreens/lb7/8.png){#fig:008 width=70%}

:::
::::::::::::::

## И после чего выполнено шифрование сообщения.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Шифрование](/home/mavinogradova/skreens/lb7/9.png){#fig:009 width=70%}

:::
::::::::::::::

## По известному шифротексту и известному открытому тексту был подобран ключ.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Подбор ключа](/home/mavinogradova/skreens/lb7/10.png){#fig:010 width=70%}

:::
::::::::::::::

## С помощью подобранного ключа шифротекст был расшифрован.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Расшифрование](/home/mavinogradova/skreens/lb7/11.png){#fig:011 width=70%}

:::
::::::::::::::

## Был создан bash-скрипт, автоматизирующий все шаги.

:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Содержимое lab_solution.sh](/home/mavinogradova/skreens/lb7/12.png){#fig:012 width=70%}

:::
::::::::::::::

## Скрипт был запущен, получен итоговый результат
:::::::::::::: {.columns align=center}
::: {.column width="70%"}

![Результат выполнения](/home/mavinogradova/skreens/lb7/13.png){#fig:013 width=70%}

:::
::::::::::::::

## Вывод

В ходе выполнения лабораторной работы было разработано приложение для шифрования и дешифрования данных в режиме однократного гаммирования. На практике освоены:
- шифрование сообщения по известному ключу;
- подбор ключа по известному шифротексту и открытому тексту;
- проверка расшифрования подобранным ключом.

Задача полностью выполнена: для сообщения **«С Новым Годом, друзья!»** был успешно подобран ключ.


