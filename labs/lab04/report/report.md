---
## Front matter
title: "Отчёт по лабораторной работе 4"
subtitle: "Подготовка экспериментального стенда GNS3"
author: "Гафоров Нурмухаммад"

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
lot: true # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a
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
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
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
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Введение

## Цель работы  

Установка и настройка GNS3 и сопутствующего программного обеспечения.

# Ход выполнения  

После запуска виртуальной машины **GNS3 VM** было открыто приложение **GNS3**. При первом запуске открылся мастер настройки, где выбран вариант работы *Run appliances in a virtual machine*, позволяющий запускать сетевые устройства на виртуальной машине.  

Далее был выполнен переход к настройке подключения удалённого контроллера **GNS3**. В окне мастера указаны параметры подключения:  
- **Protocol:** HTTP  
- **Host:** 192.168.133.131  
- **Port:** 80/TCP  
- **Username:** admin  
- **Password:** *****  

После заполнения всех полей нажата кнопка **Next**, что обеспечило успешное подключение к контроллеру.  

![Настройка подключения к удалённому контроллеру GNS3](Screenshot_1.png){ #fig:001 width=80% }  

Затем через меню добавления шаблонов (`New template`) из списка доступных устройств выбран маршрутизатор **FRR (FRRouting Project)**, использующий эмулятор **Qemu**.  

![Выбор образа FRR в списке шаблонов GNS3](Screenshot_2.png){ #fig:002 width=80% }  

На этапе выбора версии установочного образа выбрана **FRR version 8.2.2**, состояние которой отображено как *Ready to install*. Это значит, что необходимый файл `frr-8.2.2.qcow2` найден локально.  

![Выбор версии и подтверждение установки FRR](Screenshot_3.png){ #fig:003 width=80% }  

После завершения установки отобразилось окно с инструкциями по использованию образа. В частности, указано, что шаблон будет доступен в категории **Routers**, а для входа в систему используются учетные данные:  
`Username: root`  
`Password: root`  

![Информация об установленном шаблоне FRR](Screenshot_4.png){ #fig:004 width=80% }  

В параметрах шаблона во вкладке **General settings** установлены значения по умолчанию:  
- **RAM:** 256 MB  
- **vCPUs:** 1  
- **Qemu platform:** x86_64  
- **Console type:** telnet  
- **On close:** Send the shutdown signal (ACPI)  

![Основные настройки шаблона FRR](Screenshot_5.png){ #fig:005 width=80% }  

Во вкладке **HDD** указано использование дискового образа `frr-8.2.2.qcow2` с интерфейсом `IDE`. Также активирована опция *Automatically create a config disk on HDD*, что позволяет автоматически создавать конфигурационный диск для сохранения параметров устройства.  

![Настройки виртуального диска FRR](Screenshot_6.png){ #fig:006 width=80% }  

После завершения установки и запуска устройства в окне **PuTTY** появился приветственный баннер **FRRouting (version 8.2.2)**, подтверждающий успешную загрузку маршрутизатора.  

![Запуск маршрутизатора FRR в GNS3](Screenshot_7.png){ #fig:007 width=80% }  

Затем аналогичным образом был добавлен образ маршрутизатора **VyOS**. На этапе выбора файлов для установки выбрана версия **VyOS 1.3.3-qemu**, статус которой также отображён как *Ready to install*.  

![Выбор версии VyOS для установки](Screenshot_8.png){ #fig:008 width=80% }  

На финальном этапе установки указано, что шаблон будет доступен в категории **Routers**, а для входа в систему используются стандартные учетные данные:  
`Username: vyos`  
`Password: vyos`  

![Информация об установленном шаблоне VyOS](Screenshot_9.png){ #fig:009 width=80% }  

После запуска устройства в окне **PuTTY** появилась консоль **VyOS**, подтверждающая корректную установку и инициализацию маршрутизатора.  

![Запуск маршрутизатора VyOS в GNS3](Screenshot_10.png){ #fig:010 width=80% }  

# Вывод  

В ходе выполнения работы были успешно добавлены и настроены образы маршрутизаторов **FRR** и **VyOS** в среде **GNS3**. Для каждого устройства выполнена установка, настройка параметров шаблона и проверка запуска в консоли. Оба маршрутизатора корректно загрузились, что подтверждает готовность виртуальной лабораторной среды к дальнейшему моделированию сетевых топологий.  
