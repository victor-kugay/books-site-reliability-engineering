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
- [Eliminating Toil](#eliminating-toil)
- [Monitoring Distributed Systems](#monitoring-distributed-systems)

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

> SLI - это показатель (индикатор) уровня качества обслуживания, четко определенное числовое значение конкретной характеристики предоставляемого обслуживания. Для большинства сервисов ключевым SLI является время отклика, или латентность запросов, - время, которое требуется для того, чтобы вернуть ответ на запрос. В качестве SLI могут также использоваться уровень (или частота) ошибок, который часто выражается как процент от общего количества запросов, и пропускная способность системы, чаще всего измеряемая в запросах в секунду. В идеале SLI непосредственно отражает интересующий нас уровень качества обслуживания, но иногда нам доступны только приближенные данные, поскольку желаемые точно измеренные показатели трудно получить или интерпретировать. Например, наиболее релевантным для пользователя показателем часто бывает время отклика на стороне клиента, но измерить задержку можно только на сервере. Еще один SLI, имеющий значение для отдела SRE, - это доступность: доля времени, когда сервис можно использовать. Обычно она определяется как доля успешных запросов, иногда ее называют выработкой (yield). Аналогично важная для систем хранения данных характеристика долговечность (durability) - это вероятность того, что данные будут сохранены в течение длительного промежутка времени.

> SLO - это целевой уровень качества обслуживания: целевое значение или диапазон значений, измеряемый с помощью SLI. Естественная структура SLO выглядит как SLI <= целевое значение или нижняя граница <= SLI <= верхняя граница. Определение и объявление SLO задает уровень ожиданий, относящихся к работе сервиса. Это может уменьшить количество необоснованных жалоб к владельцам сервиса по поводу, например, его медленной работы. Не имея явного SLO, пользователи зачастую руководствуются собственными представлениями о желаемой производительности, которые могут не совпадать с соответствующими представлениями разработчиков сервиса.

> SLA - это соглашения об уровне обслуживания, явный или неявный контракт с вашими пользователями, включающий в себя последствия, которые влечет за собой соответствие (или несоответствие) требованиям SLO. Последствия лучшего всего видны, когда затрагиваются финансы - скидки или штрафы, но они могут иметь и другие формы. Простой способ понять разницу между SLO и SLA заключается в том, чтобы задать себе вопрос: "Что случится, если требования SLO не соблюдаются?" Если явных последствий не существует, то скорее всего перед вами SLO. Если нарушается SLA, то за этим может последовать судебный иск о нарушении юридического договора.

> Если вам важна форма кривых производительности, можете указать в SLO несколько целевых показателей: 
> - 90% вызовов будут завершены менее, чем за 1 мс
> - 99% вызовов будут завершены менее, чем за 10 мс
> - 99,9% вызовов будут завершены менее, чем за 100 мс

## Eliminating Toil

> Если во время выполнения стандартны операций в работу системы должен вмешаться человек-оператор, у вас есть ошибка - Из определения "нормальных изменений" при эволюции систем, Карл Грейссер, Google

## Monitoring Distributed Systems
> В области мониторинга не существует единого словаря терминов - даже в компании Google используемая терминология может различаться. Самые распространенные определения перечислены ниже:
> - Мониторинг (наблюдение). Сбор, обработка, агрегирование и отображение в реальном времени количественных показателей системы, например общее число и тип запросов, количество ошибок и их типы, время обработки запросов и время функционирования серверов.
> - Наблюдение методом белого ящика. Наблюдение с использованием показателей, доступных внутри системы, включая журналы, интерфейсы вроде Java Virtual Machine Profiling Interface или обработчики HTTP, которые предоставляют внутреннюю статистику.
> - Наблюдение методом черного ящика. Наблюдение и проверка (тестирование) поведения, видимого извне, с точки зрения пользователя.
> - Информационная панель (панель управления). Приложение (как правило, веб-приложение), которое предоставляет сводную информацию об основных показателях сервиса.
> - Оповещения ("алерт"). Сообщение, которые должен обратить внимание человек и которые направляются в конкретную систему, например в очередь запросов ("тикетов"), в электронную почту или на специальное устройство - пейджер. Соответственно, оповещения делятся на "тикеты" (оповещения по электронной почте) и вызовы (экстренные оповещения).
> - Основная причина. Дефект в ПО или ошибка, допускаемая человека, после исправления которых можно быть уверенными, что аналогичное событие не произойдет. Некоторые события могут иметь несколько основных причин: например, событие было вызвано недостаточной автоматизацией процесса, программным сбоем из-за некорректных входных данных и недостаточно качественным тестированием сценария, использованного для создания конфигурации. Каждый из этих факторов может быть основной причиной и каждый из них должен быть исправлен.
> - Узел или машина. Эти взаимозаменяемые термины используются для обозначения единственного экземпляра запущенного ядра на физическом сервере, виртуальной машине или в контейнере. На одной машине могут работать несколько сервисов, за которыми нужно следить.
> - Установка версии (push). Любые изменения, производимые в ПО работающего сервиса или его конфигурации. 

> Четыре золотых сигнала: 
> - Время отклика. Время, которое требуется для выполнения запроса. Очень важно различать задержки успешных и неуспешных запросов. Например, код ошибки HTTP 500, вызванной потерей соединения с базой данных или каким-то критическим сбоем на стороне бэкенда, может быть возвращен очень быстро. Однако, поскольку ошибка HTTP 500 указывает на то, что запрос не был выполнен, учитывать такие результаты при подсчете общей задержки нельзя. С другой стороны, медленная (возвращенная с большой задержкой) ошибка даже хуже быстрой! Поэтому важно не просто фильтровать ошибки, но и анализировать задержку выполнения соответствующих запросов. 
> - Величина трафика. Величина нагрузки, которая приходится на вашу систему и измеряется в высокоуровневых единицах, зависящих от конкретной системы. Для веб-сервисов трафик измеряется как количество запросов HTTP в секунду, обычно дополняемое информацией о характере запросов (например, статический или динамический контент). Для системы потокового аудио это может быть скорость передачи данных по сети или количество параллельных сессий. Для системы хранения, построенных по схеме "ключ-значение", таким показателем может служить количество выполненных транзакций и возвращенных значений в секунду.
> - Уровень ошибок. Количество (или частота) неуспешного выполнения запросов: явно (например, если возвращен код HTTP 500), неявно (если возвращен код HTTP 200, но результат получен неправильный) или несоответствующих требованиям (например, если вы решили, что ответ должен приходить не более чем, через секунду, то после ожидания ответа в течение этого времени любой запрос будет считаться неуспешным). 
> - Степень загруженности. Показатель того, насколько полно загружен ваш сервис. Эти измерения показывают те компоненты системы и те ресурсы, в которых вы ограничены больше всего (например, в системах, ограниченных в памяти , это память). Обратите внимание, что многие системы теряют в производительности еще до того, как будут загружены на 100%, поэтому следить за уровнем загрузки как за целевым показателем также очень важно.

> При проектировании системы мониторинга с нуля очень заманчивой может показаться идея использовать средние значения: среднюю задержку, средний процент загрузки процессора ваших узлов или среднюю заполненность ваших баз данных. Рискованность такой оценки в двух последних случаях очевидна: процессор и база данных могут использоваться очень неравномерно. То же верно и для задержек. Если вы запускаете веб-сервис, имеющий среднюю задержку отклика 100мс при 1000 запросах в секунду, от обработка 1% запросов может занять и 5 секунд. Если для формирования страницы нужно выполнить несколько подобных запросов, 99-й процентиль задержки отклика одного из серверов легко может стать средней задержкой для клиента. Самый простой способ различить медленное среднее время обработки и очень медленные "хвостовые" запросы = вместо самих значений задержки использовать количество запросов, величина задержки для которых попадает в заданные интервалы, удобные для построения гистограммы: сколько запросов потребовали для обработки от 0 до 10 мс, от 10 дом 30 мс, от 30 до 100 и т.д. Построение гистограммы с примерно логарифмически расставленными границами интервалов (в нашем случае с основанием, приблизительно равным 3) - зачастую самый простой способ наглядно представить распределенные характеристики ваших запросов. 

> При создании правил для мониторинга и оповещения, чтобы избежать как реальных ошибок, оставшихся незамеченными, так и огромного количества экстренных оповещений, вам нужно задать себе следующие вопросы:
> - Позволяет ли правило выявить не обнаруживаемые иными средствами состояние, которое требует быстрой реакции и последствия которого уже заметны (или обязательно будут заметны) пользователю?
> - Смогу ли я проигнорировать это оповещения, зная, что оно не критическое? Когда и почему я смогу проигнорировать оповещение и как можно избежать этого варианта развития событий?
> - Говорит ли это оповещение однозначно о том, что на пользователей уже оказывается какое-то негативное влияние? Можно ли распознать ситуации, при которых пользователи не страдают (например, при отводе трафика или развертывании тестов) и которые должны быть отфильтрованы?
> - Могу ли я что-то предпринять в ответ на это оповещение? Нужно ли отреагировать немедленно, или же это может подождать до утра? Можно ли безопасно автоматизировать это действие? Будет ли это действие постоянным решением или временным?
> - Приходит ли это оповещение другим инженерам? Это означало бы, что как минимум одно из них бесполезно.

> Следующие вопросы отражают нашу позицию по экстренным оповещениям, передаваемым на пейджеры:
> - Каждый раз, когда сигналит пейджер, я должен отреагировать немедленно, но осмысленно. Без переутомления я могу так реагировать всего несколько раз за день.
> - Каждому оповещению должны соответствовать определенные ответные действия.
> - Каждый ответ на экстренное оповещение должен быть продуманным. Если оповещение заслуживает лишь автоматического ответа, оно не должно быть экстренным.
> - Экстренное оповещение и вмешательство человека ...

