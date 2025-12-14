---
## Front matter
title: "Отчёт по лабораторной работе 7"
subtitle: "Адресация IPv4 и IPv6. Настройка DHCP"
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

Получение навыков настройки службы DHCP на сетевом оборудовании для распределения адресов IPv4 и IPv6.

# Ход выполнения

## Создание проекта и развертывание топологии в GNS3

В **GNS3** был создан новый проект. На рабочее поле добавлены устройства: маршрутизатор **ngaforov-gw-01**, коммутатор **ngaforov-sw-01** и хост **PC1-ngaforov**. Устройства соединены в соответствии с заданной топологией.

![Топология сети в GNS3](Screenshot_1.png){ #fig:001 width=50% }

Названия устройств приведены к требуемому формату:  
маршрутизатор — **ngaforov-gw-01**,  
коммутатор — **ngaforov-sw-01**,  
клиент — **PC1-ngaforov**.

На линке между коммутатором и маршрутизатором активирован захват пакетов.

## Установка и настройка VyOS

На маршрутизаторе выполнена установка системы VyOS через мастер install image. После завершения установки устройство перезагружено. Затем маршрутизатор переведён в режим конфигурирования, задано имя хоста, доменное имя, создан пользователь **ngaforov** и удалён стандартный пользователь vyos.

![Настройка имени устройства и домена](Screenshot_2.png){ #fig:002 width=80% }

Настройки сохранены через commit и save.

## Настройка интерфейса и DHCP-сервера

Интерфейсу eth0 маршрутизатора присвоен адрес **10.0.0.1/24**. После этого выполнена настройка DHCP-сервера: создана shared-network с именем **ngaforov**, указано доменное имя, DNS-сервер, адрес маршрутизатора, а также диапазон адресов для выдачи клиентам — от **10.0.0.2** до **10.0.0.253**.

![Настройка DHCP сервера](Screenshot_3.png){ #fig:003 width=80% }

Конфигурация сохранена.

## Получение IPv4-адреса по DHCP на клиенте PC1

На хосте **PC1-ngaforov** выполнен запрос параметров через DHCP. На экране отображён полный DORA-цикл: Discover, Offer, Request, Ack. Клиент получил адрес **10.0.0.2/24**, шлюз **10.0.0.1**, DNS **10.0.0.1**, домен **ngaforov.net**.

![Получение IP по DHCP на PC1](Screenshot_4.png){ #fig:004 width=80% }

## Проверка конфигурации и связности

Команда show ip вывела полную конфигурацию интерфейса, включая адрес, шлюз, аренду DHCP и MAC-адрес. Затем выполнена проверка доступности маршрутизатора командой ping, результат — два успешных ответа.

![Просмотр IP-конфигурации на PC1](Screenshot_5.png){ #fig:005 width=70% }

## Статистика DHCP-сервера

На маршрутизаторе просмотрены текущие показатели DHCP-сервера и активные аренды. Пул содержит 252 возможных адреса, выдан один адрес — **10.0.0.2** хосту **PC1-ngaforov**.

![Статистика DHCP-сервера](Screenshot_6.png){ #fig:006 width=80% }

## Журнал работы DHCP

Просмотр журнала работы DHCP показал корректную обработку клиентского запроса: DISCOVER → OFFER → REQUEST → ACK. Все записи соответствуют последовательности выдачи адреса.

![Журнал DHCP](Screenshot_7.png){ #fig:007 width=80% }

## Анализ DHCP-трафика в Wireshark

В Wireshark зафиксированы основные пакеты DHCP, включая Offer и Ack, а также ARP-запросы клиента, подтверждающие отсутствие конфликта IP-адресов. Пакеты отображают полный процесс назначения IP-адреса клиенту.

![DHCP-пакеты в Wireshark](Screenshot_8.png){ #fig:008 width=90% }

## Дополнение топологии и подготовка рабочей среды

В существующий проект GNS3 была добавлена расширенная топология, включающая дополнительные коммутаторы **ngaforov-sw-02**, **ngaforov-sw-03**, а также клиентский хост **PC2-ngaforov** на базе Kali Linux CLI, поскольку VPCS не поддерживает протокол DHCPv6. Все устройства были соединены в соответствии с требуемой схемой.

![Топология сети](Screenshot_9.png){ #fig:009 width=80% }

Все устройства были переименованы в соответствии с правилами:
- маршрутизатор: **ngaforov-gw-01**  
- коммутаторы: **ngaforov-sw-01**, **ngaforov-sw-02**, **ngaforov-sw-03**  
- клиенты: **PC1-ngaforov**, **PC2-ngaforov**, **PC3-ngaforov**

На соединениях между маршрутизатором и коммутаторами **sw-02** и **sw-03** был включён захват трафика.

## Настройка IPv6-адресации на маршрутизаторе

Маршрутизатор **ngaforov-gw-01** переведён в режим конфигурирования, после чего интерфейсам были назначены IPv6-адреса:

- Интерфейс **eth1** — сеть **2000::/64**, адрес **2000::1/64**
- Интерфейс **eth2** — сеть **2001::/64**, адрес **2001::1/64**

![Настройка IPv6 интерфейсов](Screenshot_10.png){ #fig:010 width=80% }

После проверки конфигурации с помощью `show interfaces` настройки были зафиксированы командами commit и save.

## Настройка Router Advertisements и DHCPv6 Stateless

Для автоматического предоставления хостам IPv6-адресов по SLAAC, а также передачи вспомогательных параметров через DHCPv6 Stateless, выполнена настройка RA на интерфейсе **eth1**:

- Указан префикс **2000::/64**
- Включён флаг **other-config-flag**, указывающий, что неадресная информация будет передаваться через DHCPv6

Далее настроен DHCPv6-сервер в режиме Stateless:

- создана shared-network **ngaforov-stateless**
- задан домен **ngaforov.net**
- указан DNS-сервер **2000::1**

![Настройка DHCPv6 Stateless](Screenshot_11.png){ #fig:011 width=80% }

Конфигурация сервиса отображена командой `run show configuration`:

![Конфигурация DHCPv6 Stateless](Screenshot_12.png){ #fig:012 width=80% }

## Проверка IPv6-параметров на узле PC2

На хосте **PC2-ngaforov** после получения Router Advertisements команда `ifconfig` показала, что интерфейс eth0 автоматически получил IPv6-адрес, сформированный по SLAAC.

Маршрутизация IPv6 также отображена корректно — присутствует маршрут по префиксу 2000::/64 и маршрут по умолчанию через fe80::/e1d:79ff:fee3:1.

![Начальные IPv6 параметры на PC2](Screenshot_13.png){ #fig:013 width=80% }

Проверка связи с маршрутизатором успешна

## Проверка настроек DNS

Файл `/etc/resolv.conf` содержит параметры, переданные по DHCPv6-Stateless:

- домен: **ngaforov.net**
- DNS: **2000::1**

![Первичное содержимое resolv.conf](Screenshot_13.png){ #fig:014 width=80% }

## Запрос параметров через DHCPv6

Для получения параметров DHCPv6 выполнена команда: dhclient -6 -S -v eth0


Опции:
- `-6` — использование DHCPv6  
- `-S` — запрос только информации, без выдачи адреса  
- `-v` — детализированный вывод  

Хост получил DHCPv6-параметры — доменное имя и DNS-сервер.

![Работа DHCPv6 клиента](Screenshot_14.png){ #fig:015 width=80% }

После этого параметры DNS повторно подтверждены:

![Обновлённый resolv.conf](Screenshot_14.png){ #fig:016 width=80% }

## Проверка статистики DHCPv6-сервера на маршрутизаторе

Команда `run show dhcpv6 server leases` вывела пустой список, что является нормальным для DHCPv6-Stateless — сервер не выдаёт адресов, а только параметры, поэтому таблица аренд остаётся пустой.

![Статистика DHCPv6](Screenshot_15.png){ #fig:017 width=80% }

## Анализ DHCPv6-трафика в Wireshark

В захваченном трафике присутствуют пакеты:
- **Router Advertisement** — объявление префикса 2000::/64  
- **Neighbor Solicitation / Neighbor Advertisement** — часть работы IPv6 ND  
- **DHCPv6 Information-request** — запрос с опциями:  
  - DNS Recursive Name Server  
  - Domain Search List  
  - Client FQDN  
  - Simple NTP Server  
- **DHCPv6 Reply** — ответ DHCPv6-сервера с требуемыми параметрами

![DHCPv6 трафик в Wireshark](Screenshot_16.png){ #fig:018 width=90% }

Пакеты соответствуют сценарию Stateless, при котором адрес формируется по SLAAC, а дополнительные настройки передаются через DHCPv6-сервер.

## Настройка DHCPv6 Stateful на маршрутизаторе

Для реализации режима DHCPv6 Stateful на интерфейсе **eth2** маршрутизатора **ngaforov-gw-01** был активирован флаг **managed-flag**, информирующий клиентов о необходимости получения IPv6-адресов по DHCPv6.

Далее была создана разделяемая сеть **ngaforov-stateful**, настроен DNS-сервер, доменное имя и указан диапазон адресов **2001::100 – 2001::199**, который будет использоваться для выдачи адресов клиентам.

![Настройка DHCPv6 Stateful](Screenshot_17.png){ #fig:019 width=80% }

Конфигурация сохранена командами commit и save.

## Проверка начальных параметров на узле PC3

Перед запросом адреса по DHCPv6 были просмотрены текущие параметры интерфейса, таблица маршрутизации и содержимое файла resolv.conf.

На интерфейсе присутствовал SLAAC-адрес из сети **2001::/64**, полученный через RA, но отсутствовал адрес, выдаваемый DHCPv6.

![Начальные параметры PC3](Screenshot_18.png){ #fig:020 width=80% }

## Получение IPv6-адреса по DHCPv6 Stateful

На клиенте **PC3-ngaforov** была выполнена команда: dhclient -6 -v eth0


Запрос прошёл полный процесс DORA-подобного обмена, адаптированного под DHCPv6:

- Forming Solicit
- Advertise message from сервер
- Request
- Reply с выдачей адреса из диапазона

![Процесс DHCPv6 на PC3](Screenshot_19.png){ #fig:021 width=80% }

Клиент получил адрес из диапазона DHCPv6 Stateful — **2001::198/128**.

## Повторная проверка состояния интерфейса и маршрутизации

После получения адреса интерфейс получил новый non-temporary адрес, выданный DHCPv6. Таблица маршрутизации также обновилась.

![Интерфейс и маршрутизация после получения адреса](Screenshot_20.png){ #fig:022 width=80% }

Проверка доступности маршрутизатора:

Оба пакета успешно доставлены.

## Просмотр таблицы аренд DHCPv6 на маршрутизаторе

Команда: run show dhcpv6 server leases


вывела два активных адреса:

- **2001::198**  
- **2001::199**

Каждая аренда содержит:
- состояние — active  
- срок аренды  
- IAID/DUID клиента  

![Аренды DHCPv6](Screenshot_21.png){ #fig:023 width=80% }

Это подтверждает корректную работу DHCPv6 Stateful.

## Анализ DHCPv6-трафика в Wireshark

В Wireshark были зафиксированы ключевые DHCPv6-пакеты:

- **Solicit** — клиент запрашивает сервер  
- **Advertise** — сервер предлагает конфигурацию  
- **Request** — клиент запрашивает конкретный адрес  
- **Reply** — сервер подтверждает назначение  

![Трафик DHCPv6 Stateful](Screenshot_22.png){ #fig:024 width=90% }

Внутри пакетов видны обязательные поля:

- Client Identifier (DUID)
- Server Identifier (DUID)
- Requested non-temporary address (IA_NA)
- Предпочтительное и максимальное время жизни адреса

# Вывод  

В ходе выполнения лабораторной работы была исследована работа протоколов автоматической настройки IPv6-адресов в среде **GNS3** на основе маршрутизатора **VyOS** и клиентских узлов на базе **Kali Linux**. В проекте были реализованы два механизма конфигурации — **Stateless DHCPv6** и **Stateful DHCPv6**, что позволило на практике изучить различия между ними и особенности применения каждого варианта.

В режиме **Stateless** клиент формировал IPv6-адрес самостоятельно с использованием **SLAAC**, а DHCPv6-сервер передавал только дополнительные параметры: доменное имя и адрес DNS-сервера. Анализ захваченного трафика подтвердил корректность работы RA-сообщений и обмена DHCPv6 Information-Request / Reply.

При настройке **Stateful DHCPv6** маршрутизатор полностью управлял назначением адресов клиентам. Клиентский узел получил IPv6-адрес из заранее определённого диапазона, что было подтверждено как выводом утилиты `dhclient`, так и отображением аренды в таблице DHCPv6-сервера. В трафике были зафиксированы сообщения Solicit, Advertise, Request и Reply, соответствующие стандартному циклу выделения IPv6-адреса по DHCPv6.

Полученные результаты подтверждают корректность функционирования механизмов SLAAC, RA и DHCPv6 в различных режимах, а также демонстрируют понимание процессов автоматизации сетевой конфигурации и взаимодействия сетевых протоколов в IPv6-среде.  
