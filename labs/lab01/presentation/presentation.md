---
## Front matter
lang: ru-RU
title: Методы кодирования и модуляция сигналов
subtitle: Лабораторная работа №1
author:
  - Гафоров Нурмухаммад
institute:
  - Российский университет дружбы народов, Москва, Россия
date: 16 декабря 2025

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

# Цель и задачи работы

## Цель лабораторной работы
Изучение методов кодирования и модуляции сигналов с использованием Octave, исследование спектральных свойств и самосинхронизации.

## Задачи лабораторной работы
1. Построить графики синусоидальных и косинусоидальных функций.  
2. Исследовать приближение меандра с помощью ряда Фурье.  
3. Определить спектры отдельных сигналов и их суммы.  
4. Продемонстрировать амплитудную модуляцию.  
5. Построить кодовые последовательности и проверить самосинхронизацию.  

# Построение графиков

## График функции y1
![График y1](plot-sin.png){ #fig:001 width=70% }

## Графики функций y1 и y2
![Графики y1 и y2](plot-sin-cos.png){ #fig:002 width=70% }

# Приближение меандра

## Cos-разложение
![Меандр cos-ряд](meandr.png){ #fig:003 width=70% }

## Sin-разложение
![Меандр sin-ряд](meandr2.png){ #fig:004 width=70% }

# Спектральный анализ

## Исходные сигналы
![Сигналы](spectre1/signal/spectre.png){ #fig:005 width=70% }

## Исправленные спектры
![Исправленные спектры](spectre1/spectre/spectre_fix.png){ #fig:007 width=70% }

## Суммарный сигнал и его спектр
![Суммарный сигнал](spectre_sum/signal/spectre_sum.png){ #fig:008 width=70% }  
![Спектр суммы](spectre_sum/spectre/spectre_sum.png){ #fig:009 width=70% }

# Амплитудная модуляция

## АМ-сигнал и огибающая
![АМ-сигнал](modulation/signal/am.png){ #fig:010 width=70% }

## Спектр АМ-сигнала
![Спектр АМ](modulation/spectre/am.png){ #fig:011 width=70% }

# Линейные коды

## Примеры сигналов
![Unipolar](coding/signal/unipolar.png){ #fig:012 width=40% }
![AMI](coding/signal/ami.png){ #fig:013 width=40% }

![Manchester](coding/signal/manchester.png){ #fig:016 width=40% }
![Diff Manchester](coding/signal/diffmanc.png){ #fig:017 width=40% }

## Самосинхронизация
![Sync Unipolar](coding/sync/unipolar.png){ #fig:018 width=40% }
![Sync Manchester](coding/sync/manchester.png){ #fig:022 width=40% }

# Спектры кодов

## Примеры спектров
![Спектр AMI](coding/spectre/ami.png){ #fig:025 width=40% }
![Спектр Manchester](coding/spectre/manchester.png){ #fig:028 width=40% }

# Выводы

## Итог работы
- Построены графики и спектры сигналов.  
- Исследовано приближение меандра и эффект Гиббса.  
- Рассмотрены принципы амплитудной модуляции.  
- Изучены различные методы линейного кодирования.  
- Подтверждены свойства самосинхронизации и спектрального распределения.  
