# Site Reliability Engineering: How Google Runs Production Systems

<div>
  <a>
    <img src="https://img.shields.io/badge/-Site_Reliability_Engineering-black?&logoColor=1DA1F2&style=for-the-badge&logoWidth=30" />
  </a>
</div>

## ✍ Description

This repository includes the most valuable quotes (in my mind) from "Site Reliability Engineering: How Google Runs Production Systems" book by Betsy Beyer, Chris Jones, Jennifer Petoff, Niall Murphy.

All rights are reserved by Betsy Beyer, Chris Jones, Jennifer Petoff, Niall Murphy and publisher "Piter".

Main language is Russian 🇷🇺

## 💻 Table of contents

Quotes from next sections were presented:

<!-- no toc -->
- [Introduction](#introduction)
- [The Production Environment at Google from the Viewpoint of an SRE](#the-production-environment-at-google-from-the-viewpoint-of-an-sre)
- [Embracing Risk](#embracing-risk)
- [Service Level Objectives](#service-level-objectives)

## Introduction

> Существуют три категории данных от системы мониторинга:
>
> - Срочные оповещения (alerts, "алерты") - указывают, что нужно немедленно реагировать на что-то, что либо уже произошло, либо вот-вот произойдет
> - Запросы на действия (tickets, "тикеты") - указывают, что человеку нужно вмешаться, но не обязательно немедленно. Система не может обработать ситуацию автоматически, но предпринимать какие-то действия допустимо не сразу, а в течение нескольких дней без каких-либо негативных последствий
> - Журналирование (logging) - нет необходимости кому-либо просматривать эту информацию, она записывается для диагностических целей или для последующего анализа. По умолчанию журнал читать не требуется, пока что-либо иное не заставит сделать это

## The Production Environment at Google from the Viewpoint of an SRE

> Необходимо разворачивать как минимум N + 2 задач, потому что:
>
> - Во время обновления одна задача будет временно недоступна, поэтому активными будут оставаться N + 1 задач
> - Во время обновления может произойти сбой в аппаратной части, из-за чего останется всего N задач - ровно столько, сколько нужно для обслуживания пиковой нагрузки

## Embracing Risk

> При определении целевого уровня доступности для каждого сервиса мы задаем следующие вопросы:
>
> - Если на нужно построить и эксплуатировать подобную систему с доступностью на одну девятку выше заданной, насколько в таком случае вырастет наша прибыль?
> - Покрывает ли дополнительная прибыль затраты на достижение такого уровня надежности?
>   Для того, чтобы сделать это компромиссное равенство более конкретным, рассмотрим следующий анализ рентабельности для типового сервиса, где все запросы одинаково важны
> - Предлагаемое улучшение целевого уровня доступности: 99,9% -> 99,99%
> - Предлагаемое увеличение уровня доступности: 0,09%
> - Прибыль получаемая от сервиса: $1M
> - Прибыль, которую можно будет дополнительно получить от улучшения доступности: $1M X 0.0009 = $900
>   В этом случае, если стоимость улучшения доступности на одну девятку меньше $900, такое улучшение будет стоить своих денег. Если же эта стоимость больше $900, она будет превышать потенциальное увеличение прибыли.

## Service Level Objectives
