---
## Front matter
title: "Лабораторная работа № 7"
subtitle: "Учёт физических параметров сети"
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

Получить навыки работы с физической рабочей областью Packet Tracer,
а также учесть физические параметры сети.

# Задание

Требуется заменить соединение между коммутаторами двух территорий
msk-donskaya-sw-1 и msk-pavlovskaya-sw-1 на соединение, учитывающее физические параметры сети, а именно — расстояние между двумя
территориями.
При выполнении работы необходимо учитывать соглашение об именовании.

# Выполнение лабораторной работы

Откроем проект предыдущей лабораторной работы.

Перейдем в физическую рабочую область Packet Tracer. Присвоим название городу — Moscow (рис. [-@fig:001]).

![Физическая рабочая область Packet Tracer](image/1.png)

Щёлкнув на изображении города, увидим изображение здания.
Присвоим ему название Donskaya. Добавим здание для территории
Pavlovskaya (рис. [-@fig:002]).

![Изображение зданий в физической рабочей области Packet Tracer](image/2.png)

Щёлкнув на изображении здания Donskaya, переместим изображение, обозначающее серверное помещение, в него (рис. [-@fig:003]).

![Размещение в физической рабочей области Packet Tracer серверной с подключением оконечных устройств (сеть территории «Донская»)](image/3.png)
Щёлкнув на изображении серверной, увидим отображение серверных
стоек (рис. [-@fig:004]).

![Отображение серверных стоек в Packet Tracer](image/4.png)

Вернувшись в логическую рабочую область Packet Tracer, пропингуем с коммутатора msk-donskaya-sw-1 коммутатор msk-pavlovskaya-sw-1 (рис. [-@fig:006]).
Убедимся в работоспособности соединения. Соединение действительно работает.

![Проверка работоспособности соединения](image/6.png)

В меню Options , Preferences во вкладке Interface активируем разрешение
на учёт физических характеристик среды передачи (Enable Cable Length
Effects) (рис. [-@fig:008]).

![Активация разрешения на учёт физических характеристик среды передачи](image/13.png)

В физической рабочей области Packet Tracer разместим две территории на
расстоянии более 100 м друг от друга (рекомендуемое расстояние — около
1000 м или более).

![Размещение территорий на расстоянии более 100 м друг от друга](image/14.png)

Вернувшись в логическую рабочую область Packet Tracer, пропингуем с коммутатора msk-donskaya-sw-1 коммутатор msk-pavlovskaya-sw-1 .
Убедимся в неработоспособности соединения. Соединение теперь не работоспособно.

![Проверка неработоспособности соединения](image/15.png)

Удалим соединение между msk-donskaya-sw-1 и msk-pavlovskaya-sw-1.
Добавим в логическую рабочую область два повторителя (RepeaterPT). Присвоим им соответствующие названия msk-donskaya-mc-1
и msk-pavlovskaya-mc-1. Заменим имеющиеся модули на PT-REPEATERNM-1FFE и PT-REPEATER-NM-1CFE для подключения оптоволокна
и витой пары по технологии Fast Ethernet (рис. [-@fig:010]).

![Повторитель с портами PT-REPEATER-NM-1FFE и PT-REPEATER-NM-1CFE для подключения оптоволокна и витой пары по технологии Fast Ethernet](image/10.png)

Переместим msk-pavlovskaya-mc-1 на территорию Pavlovskaya (в физической рабочей области Packet Tracer) .

![Схема сети с учётом физических параметров сети в логической рабочей области Packet Tracer](image/16.png)


Также внесем соответствующие изменения в таблицу портов (табл. [-@tbl:fiz]).

:Таблица портов {#tbl:fiz}

| Устройство                       | Порт        | Примечание                         |
|----------------------------------|-------------|------------------------------------|
| msk-donskaya-cahanqirov-gw-1     | f0/1        | UpLink                             |
|                                  | f0/0        | msk-donskaya-cahanqirov-sw-1       |
| msk-donskaya-cahanqirov -sw-1    | f0/24       | msk-donskaya-cahanqirov-gw-1       |
|                                  | g0/1        | msk-donskaya-cahanqirov-sw-2       |
|                                  | g0/2        | msk-donskaya-cahanqirov-sw-4       |
|                                  | f0/1        | msk-donskaya-cahanqirov-mc-1       |
| msk-donskaya-cahanqirov -sw-2    | g0/1        | msk-donskaya-cahanqirov-sw-1       |
|                                  | f0/1        | Web-server                         |
|                                  | g0/2        | msk-donskaya-cahanqirov-sw-3       |
|                                  | f0/2        | File-server                        |
| msk-donskaya-cahanqirov -sw-3    | g0/1        | msk-donskaya-cahanqirov-sw-2       |
|                                  | f0/2        | Dns-server                         |
|                                  | f0/1        | Mail-server                        |
| msk-donskaya-cahanqirov -sw-4    | g0/1        | msk-donskaya-cahanqirov-sw-1       |
|                                  | f0/6–f0/10  | departments                        |
|                                  | f0/1–f0/5   | dk                                 |
|                                  | f0/11–f0/15 | adm                                |
|                                  | f0/16–f0/24 | other                              |
| msk-donskaya-cahanqirov-mc-1     | f0/0        | msk-donskaya-cahanqirov-sw-1       |
|                                  | f0/1        | msk-pavlovskaya-cahanqirov-mc-1    |
| msk-pavlovskaya-cahanqirov-mc-1  | f0/0        | msk-pavlovskaya-cahanqirov-sw-1    |
|                                  | f0/1        | msk-donskaya-cahanqirov-mc-1       |
| msk-pavlovskaya-cahanqirov-sw-1  | f0/24       | msk-pavlovskaya-cahanqirov-mc-1    |
|                                  | f0/1–f0/15  | dk                                 |
|                                  | f0/20       | other                              |

Убедимся в работоспособности соединения между msk-donskaya-sw-1
и msk-pavlovskaya-sw-1 (рис. [-@fig:017]).

![Проверка работоспособности соединения](image/17.png)

# Выводы

В результате выполнения лабораторной работы я получил навыки работы с физической рабочей областью Packet Tracer,
а также учитывала физические параметры сети.

