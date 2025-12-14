---
## Front matter
lang: ru-RU
title: Настройка DHCP для IPv4 и IPv6 в GNS3
subtitle: Лабораторная работа №7
author:
  - Гафоров Нурмухаммад
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 28 ноября 2025

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цель работы  

Получить практические навыки настройки механизмов **DHCP**, **DHCPv6 (Stateless/Stateful)** и автоматической конфигурации IPv6 (**SLAAC**, **RA**) в среде **GNS3** с использованием маршрутизатора **VyOS** и клиентских узлов.

# Ход выполнения  

## Первичная топология и настройка IPv4 DHCP  

В GNS3 создана базовая сеть, включающая маршрутизатор, коммутатор и хост **PC1-ngaforov**.

![Топология IPv4](Screenshot_1.png){ width=70% }

## Настройка DHCP для IPv4  

- сеть: **10.0.0.0/24**
- диапазон: **10.0.0.2 — 10.0.0.253**
- шлюз: **10.0.0.1**
- DNS: **10.0.0.1**

![DHCP IPv4](Screenshot_3.png){ width=70% }

## Получение IPv4-адреса клиентом  

Узел **PC1-ngaforov** получил адрес **10.0.0.2/24** через полный цикл DORA:  
**Discover → Offer → Request → Ack**.

![DORA в Wireshark](Screenshot_8.png){ width=70% }

## Расширение топологии для IPv6  

Добавлены коммутаторы **ngaforov-sw-02**, **ngaforov-sw-03** и клиент **PC2-ngaforov** (Kali Linux), поддерживающий DHCPv6.

![Расширенная топология](Screenshot_9.png){ width=70% }

## Настройка IPv6-адресов на маршрутизаторе  

Назначены адреса:

- eth1 → **2000::1/64**
- eth2 → **2001::1/64**

![Интерфейсы IPv6](Screenshot_10.png){ width=70% }

## Настройка SLAAC + DHCPv6 Stateless  

- префикс: **2000::/64**
- флаг **other-config-flag**

DHCPv6 Stateless выдаёт только параметры: DNS, домен.

![Stateless конфигурация](Screenshot_11.png){ width=70% }

## Результат на клиенте PC2  

PC2 получил IPv6-адрес по SLAAC и параметры DNS через DHCPv6 Stateless.

![PC2 SLAAC](Screenshot_13.png){ width=70% }

## DHCPv6 Stateless в Wireshark  

Фиксация пакетов:

- Router Advertisement  
- DHCPv6 Information-Request / Reply  
- Neighbor Discovery  

![DHCPv6 Stateless Capture](Screenshot_16.png){ width=80% }

## Настройка DHCPv6 Stateful  

Создан DHCPv6-пул:

- диапазон: **2001::100 — 2001::199**
- DNS: **2001::1**
- domain-search: **ngaforov.net**

![Stateful конфиг](Screenshot_17.png){ width=70% }

## Получение IPv6-адреса PC3  

Команда `dhclient -6 -v eth0` получила адрес из диапазона DHCPv6 Stateful — **2001::198/128**.

![PC3 DHCPv6 Stateful](Screenshot_19.png){ width=70% }

## Проверка маршрутизации и связности  

Появился non-temporary IPv6-адрес, маршруты обновлены.  
Ping до **2001::1** успешен.

![PC3 маршруты](Screenshot_20.png){ width=70% }

## Таблица DHCPv6 Stateful  

Маршрутизатор показывает активные аренды:

- **2001::198**
- **2001::199**

![Таблица аренд](Screenshot_21.png){ width=70% }

## DHCPv6 Stateful в Wireshark  

Зафиксирован полный обмен:

- **Solicit**
- **Advertise**
- **Request**
- **Reply**

![Stateful пакеты](Screenshot_22.png){ width=80% }

# Выводы  

В ходе работы:  

- Выполнена настройка DHCP для IPv4 и IPv6.  
- Исследованы два режима DHCPv6: **Stateless** и **Stateful**.  
- Подтверждена корректная работа **SLAAC**, **RA**, **DHCPv6**.  
- В Wireshark детально проанализированы циклы обмена:
  - **DORA** (IPv4)
  - **Information-Request / Reply** (Stateless)
  - **Solicit → Advertise → Request → Reply** (Stateful)

Работа позволила закрепить навыки построения IPv4/IPv6-сетей, настройки VyOS и анализа трафика.
