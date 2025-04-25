---
## Front matter
title: "Лабораторная работа № 11"
subtitle: "Настройка NAT.Планирование"
author: "Джахангиров Илгар Залид оглы"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
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
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
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
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Провести подготовительные мероприятия по подключению локальной сети
организации к Интернету.

# Задание

1. Построить схему подсоединения локальной сети к Интернету;
2. Построить модельные сети провайдера и сети Интернет;
3. Построить схемы сетей L1, L2, L3;
4. При выполнении работы необходимо учитывать соглашение об именовании.

# Выполнение лабораторной работы

Модельные предположения:

- В сети провайдера располагаются 2 медиаконвертера provider-mc-1 и provider-mc-2 для связи с подсетью «Донская» и сетью модельного Интернета, маршрутизатор provider-gw-1 и коммутатор provider-sw-1. Оборудование соединяется между собой по Fast Ethernet согласно схеме.
- В модельной сети Интернет располагаются 4 сервера www.yandex.ru, www.rudn.ru, stud.rudn.university и esystem.pfur.ru, коммутатор internet-sw-1 и медиаконвертер internet-mc-1 для связи с сетью провайдера. Серверы подключены к коммутатору посредством Fast Ethernet, коммутатор подсоединён к медиаконвертеру также по Fast Ethernet.
- Имена и адреса серверам Интернета и маршрутизатору провайдера задаются согласно табл. [-@tbl:ip]. При этом учитывается, что под сеть адресов модельного Интернета выделяется адрес 192.0.2.0/24, а под сеть провайдера
- 198.51.100.1 (как рекомендовано в [4] для использования в примерах и документации при описании тестовых сетей).

: Распределение ip-адресов модельного Интернета {#tbl:ip}

| IP-адреса     | Примечание            |
|---------------|-----------------------|
| 192.0.2.1     | provider-gw-1         |
| 192.0.2.11    | www.yandex.ru         |
| 192.0.2.12    | stud.rudn.university  |
| 192.0.2.13    | esystem.pfur.ru       |
| 192.0.2.14    | www.rudn.ru           |

Network Address Translation (NAT) — механизм преобразования IP-адресов
транзитных пакетов.
В частности, механизм NAT используется для обеспечения доступа устройств
локальных сетей с внутренними IP-адресами к сети Интернет (рис. [-@fig:001]).

![Схема сети с NAT](image/1.png)

Внесем изменения в схему L1 сети, добавив в неё сеть провайдера и сеть модельного Интернета с указанием названий оборудования и портов подключения (рис. [-@fig:002]).

![Схема L1 сети с выходом в Интернет](image/2.png)

Внесем изменения в схемы L2(рис. [-@fig:003]) и L3 (рис. [-@fig:004]) сети, указав адреса и VLAN сети провайдера и модельной сети Интернета.

![Схема L2 сети с выходом в Интернет](image/3.png)

![Схема L3 сети с выходом в Интернет](image/4.png)

Скорректируем также таблицы распределения IP-адресов (табл. [-@tbl:ipplan]) и портов (табл. [-@tbl:port]).

: Таблица портов {#tbl:port}

| Устройство           | Порт          | Примечание           | Access VLAN | Trunk VLAN               |
|----------------------|---------------|----------------------|-------------|--------------------------|
| msk-donskaya-gw-1    | f0/1          | provider-mc-1        |             |                          |
|                      | f0/0          | msk-donskaya-sw-1    |             | 2, 3, 101, 102, 103, 104 |
| msk-donskaya-sw-1    | f0/24         | msk-donskaya-gw-1    |             | 2, 3, 101, 102, 103, 104 |
|                      | f0/20 — f0/23 | msk-donskaya-sw-4    |             | 2, 3                     |
|                      | g0/1          | msk-donskaya-sw-2    |             |                          |
|                      | g0/2          | msk-donskaya-sw-3    |             | 2, 101, 102, 103, 104    |
|                      | f0/1          | msk-donskaya-mc-1    |             | 2, 101, 104              |
| msk-donskaya-sw-2    | g0/1          | msk-donskaya-sw-1    |             | 2, 3                     |
|                      | g0/2          | msk-donskaya-sw-3    |             | 2, 3                     |
|                      | f0/1          | Web-server           | 3           |                          |
|                      | f0/2          | File-server          | 3           |                          |
| msk-donskaya-sw-3    | g0/1          | msk-donskaya-sw-2    |             | 2, 3                     |
|                      | g0/2          | msk-donskaya-sw-1    |             |                          |
|                      | f0/1          | Mail-server          | 3           |                          |
|                      | f0/2          | Dns-server           | 3           |                          |
| msk-donskaya-sw-4    | f0/20 — f0/23 | msk-donskaya-sw-1    |             | 2, 101, 102, 103, 104    |
|                      | f0/1–f0/5     | dk                   | 101         |                          |
|                      | f0/6–f0/10    | departments          | 102         |                          |
|                      | f0/11–f0/15   | adm                  | 103         |                          |
|                      | f0/16–f0/24   | other                | 104         |                          |
|                      | f0/24         | admin                | 104         |                          |
| msk-donskaya-mc-1    | f0/0          | msk-donskaya-sw-1    |             |                          |
|                      | f0/1          | msk-donskaya-mc-1    |             |                          |
| msk-donskaya-mc-2    | f0/0          | msk-donskaya-gw-1    |             |                          |
|                      | f0/1          | provider-mc-1        |             |                          |
| msk-pavlovskaya-mc-1 | f0/0          | msk-pavlovskaya-sw-1 |             |                          |
|                      | f0/1          | msk-donskaya-mc-1    |             |                          |
| msk-pavlovskaya-sw-1 | f0/24         | msk-pavlovskaya-mc-1 |             | 2, 101, 104              |
|                      | f0/1–f0/15    | dk                   | 101         |                          |
|                      | f0/20         | other                | 104         |                          |
|                      | f0/24         | admin-pavlovskaya    | 104         |                          |
| provider-gw-1        | f0/0          | provider-sw-1        |             |                          |
|                      | f0/1          | provider-mc-2        |             |                          |
| provider-sw-1        | f0/1          | provider-mc-1        |             |                          |
|                      | f0/2          | provider-gw-1        |             |                          |
| provider-mc-1        | f0/0          | provider-sw-1        |             |                          |
|                      | f0/1          | msk-donskaya-mc-2    |             |                          |
| provider-mc-2        | f0/0          | provider-gw-1        |             |                          |
|                      | f0/1          | internet-mc-1        |             |                          |
| internet-sw-1        | f0/1          | internet-mc-1        |             |                          |
|                      | f0/2          | esystem.pfur.ru      |             |                          |
|                      | f0/3          | www.rudn.ru          |             |                          |
|                      | f0/4          | stud.rudn.university |             |                          |
|                      | f0/5          | www.yandex.ru        |             |                          |
| internet-mc-1        | f0/0          | internet-sw-1        |             |                          |
|                      | f0/1          | provider-mc-2        |             |                          |


: Таблица IP {#tbl:ipplan}

| IP-адреса               | Примечание              | VLAN |
|-------------------------|-------------------------|------|
| 10.128.0.0/16           | Вся сеть                |      |
| 10.128.0.0/24           | Серверная ферма         | 3    |
| 10.128.0.1              | Шлюз                    |      |
| 10.128.0.2              | Web                     |      |
| 10.128.0.3              | File                    |      |
| 10.128.0.4              | Mail                    |      |
| 10.128.0.5              | Dns                     |      |
| 10.128.0.6-10.128.0.254 | Зарезервировано         |      |
| 10.128.1.0/24           | Управление              | 2    |
| 10.128.1.1              | Шлюз                    |      |
| 10.128.1.2              | msk-donskaya-sw-1       |      |
| 10.128.1.3              | msk-donskaya-sw-2       |      |
| 10.128.1.4              | msk-donskaya-sw-3       |      |
| 10.128.1.5              | msk-donskaya-sw-4       |      |
| 10.128.1.6              | msk-pavlovskaya-sw-1    |      |
| 10.128.1.7-10.128.1.254 | Зарезервировано         |      |
| 10.128.2.0/24           | Cеть Point-to-Point     |      |
| 10.128.2.1              | Шлюз                    |      |
| 10.128.2.2-10.128.2.254 | Зарезервировано         |      |
| 10.128.3.0/24           | Дисплейные классы (ДК)  | 101  |
| 10.128.3.1              | Шлюз                    |      |
| 10.128.3.2-10.128.3.254 | Пул для пользователей   |      |
| 10.128.4.0/24           | Кафедры (К)             | 102  |
| 10.128.4.1              | Шлюз                    |      |
| 10.128.4.2-10.128.4.254 | Пул для пользователей   |      |
| 10.128.5.0/24           | Администрация (А)       | 103  |
| 10.128.5.1              | Шлюз                    |      |
| 10.128.5.2-10.128.5.254 | Пул для пользователей   |      |
| 10.128.6.0/24           | Другие пользователи (Д) | 104  |
| 10.128.6.1              | Шлюз                    |      |
| 10.128.6.2-10.128.6.254 | Пул для пользователей   |      |
| 192.0.2.1               | provider-gw-1           |      |
| 192.0.2.11              | www.yandex.ru           | 4    |
| 192.0.2.12              | stud.rudn.university    | 4    |
| 192.0.2.13              | esystem.pfur.ru         | 4    |
| 192.0.2.14              | www.rudn.ru             | 4    |

На схеме предыдущего вашего проекта разместим необходимое оборудование для сети провайдера и сети модельного Интернета: 4 медиаконвертера (Repeater-PT), 2 коммутатора типа Cisco 2960-24TT,
маршрутизатор типа Cisco 2811, 4 сервера. Присвоим названия размещённым в сети провайдера и в сети модельного
Интернета объектам согласно модельным предположениям и схеме L1.

![Размещение новых устройств](image/5.png)

В физической рабочей области добавим здание провайдера и здание,
имитирующее расположение серверов модельного Интернета (рис. [-@fig:006]).
Присвоим им соответствующие названия. Перенесем из сети «Донская» оборудование провайдера (рис. [-@fig:008]) и модельной сети Интернета (рис. [-@fig:009]) в соответствующие здания.

![Схема сети в физической рабочей области Packet Tracer](image/6.png)

На медиаконвертерах заменим имеющиеся модули на PT-REPEATERNM-1FFE и PT-REPEATER-NM-1CFE для подключения витой пары по
технологии Fast Ethernet и оптоволокна соответственно (рис. [-@fig:007]).

![Медиаконвертер с модулями PT-REPEATER-NM-1FFE и PT-REPEATER-NM-1CFE](image/7.png)

![ Оборудование в здании сети провайдера](image/8.png)

![Оборудование в здании сети модельного Интернета](image/9.png)

Проведем соединение объектов согласно скорректированной схеме L1 (рис. [-@fig:010]).

![Схема сети с выходом в Интернет](image/10.png)

![Схема сети с выходом в Интернет](image/11.png)

Пропишем IP-адреса серверам согласно табл. [-@tbl:ip]. В качестве примера показываю задание адреса шлюза и ip-адреса для сервера www.yandex.ru (рис. [-@fig:010],[-@fig:011]).

![Задание адреса шлюза](image/12.png)

![Задание ip-адреса](image/13.png)

Пропишем сведения о серверах на DNS-сервере сети "Донская" (рис. [-@fig:013]).

![Добавление DNS-записей](image/14.png)

# Выводы

В процессе выполнения данной лабораторной работы я провел подготовительные мероприятия по подключению локальной сети организации к Интернету.

