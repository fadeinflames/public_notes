---
title: SRE, SLO и алертинг
layout: default
parent: SRE Materials
nav_order: 30
---
# Материалы по SRE, SLO и алертингу

Актуализировано: 2026-05-16.

Это прикладное продолжение [Road to SRE](../) и [SRE Theory Check-list](../theory-checklist/): здесь собраны открытые материалы, которые помогают пройти разделы про SLO, error budget, burn rate, alert fatigue и incident response.

{: .note }
Публичная версия содержит только открытые источники и общие формулировки. Внутренние рабочие детали и приватные ссылки не публикуются.

Подборка открытых материалов на русском и английском по темам:

- SLA / SLO / SLI
- Error budget и burn rate
- Multi-window burn-rate alerts
- Alert fatigue
- Page alert vs ticket alert
- Severity, owner, runbook, escalation
- Golden Signals, RED, USE

## Рекомендуемый порядок изучения

1. Разобраться с базой: `SLI`, `SLO`, `SLA`.
2. Понять `error budget` и зачем он нужен.
3. Разобрать `burn rate` как коэффициент расхода бюджета ошибок.
4. Перейти к `multi-window multi-burn-rate alerts`.
5. Изучить практики алертинга: `page` vs `ticket`, severity, owner, runbook, escalation.
6. Закрепить через Golden Signals, RED и USE.

## Короткая шпаргалка

`SLI` — что измеряем: доступность, latency, error rate, freshness и т.д.

`SLO` — целевой уровень для SLI за период. Например: 99.9% успешных запросов за 30 дней.

`SLA` — внешний договор с пользователем или клиентом, обычно с последствиями: компенсации, штрафы, обязательства поддержки.

`Error budget = 100% - SLO`.

Если SLO равен 99.9%, то error budget равен 0.1%.

`Burn rate` — во сколько раз быстрее сервис расходует error budget относительно допустимого темпа.

`Burn rate = 1` означает, что бюджет ошибок закончится ровно к концу SLO-окна.

`Burn rate > 1` означает, что бюджет ошибок расходуется быстрее допустимого.

`Page alert` — алерт, который должен немедленно прервать человека, например разбудить on-call инженера.

`Ticket alert` — алерт, который требует реакции, но может быть обработан в рабочем порядке.

## SLA / SLO / SLI

### Русскоязычные материалы

- [SLA, SLO, SLI простыми словами и с примерами, Хабр](https://habr.com/ru/articles/956318/)  
  Хороший вход в тему. Объясняет разницу между SLA, SLO и SLI, есть примеры и фрагменты Prometheus/Sloth.

- [FAQ про SLO и SLI, Хабр](https://habr.com/ru/articles/718796/)  
  Коротко и практично про SLI, SLO и расчет бюджета ошибок.

- [SLI / SLO / SLA и бюджет ошибок, System Design Space](https://system-design.space/chapter/sli-slo-sla-error-budgets)  
  Структурированный конспект: как связаны надежность, риск, SLO и error budget.

- [Соглашение об уровне обслуживания SLA, Yandex Cloud](https://yandex.cloud/ru/docs/glossary/sla)  
  Пример официального описания SLA на русском языке.

- [SLA, SLO и SLI: что такое и как использовать в ИТ-мониторинге, Naumen](https://www.naumen.ru/products/bsm/articles/sla_slo_sli/)  
  Материал с точки зрения ИТ-мониторинга и управления сервисами.

### Первоисточники

- [Google SRE: Service Level Objectives](https://sre.google/sre-book/service-level-objectives/)  
  Главный первоисточник по SLI, SLO, SLA и error budgets.

## Error Budget и Burn Rate

### Русскоязычные материалы

- [Error Budget, SLO и мониторинг: советы для начинающих SRE-инженеров, Хабр](https://habr.com/ru/companies/slurm/articles/715762/)  
  Практичный материал о том, как внедрять SLO и error budget в команде.

- [SLI/SLO. Что такое Error Budget Burn Rate на самом деле, Хабр](https://habr.com/ru/articles/1028008/)  
  Очень полезная статья именно про burn rate. Объясняет, почему burn rate — это коэффициент расхода бюджета ошибок, а не абсолютная скорость в ошибках в час.

- [Проверяем реалистичность SLO и анализируем риски, Хабр](https://habr.com/ru/companies/slurm/articles/699414/)  
  Материал про реалистичность SLO, риски и связь надежности с архитектурой.

### Англоязычные открытые материалы

- [Atlassian: What is an error budget?](https://www.atlassian.com/incident-management/kpis/error-budget)  
  Простое объяснение error budget и его роли в управлении надежностью.

- [Google SRE Workbook: Example Error Budget Policy](https://sre.google/workbook/error-budget-policy/)  
  Пример политики error budget: что делать при нарушениях SLO, когда проводить postmortem, как эскалировать спорные случаи.

## Multi-Window Burn-Rate Alerts

### Главный первоисточник

- [Google SRE Workbook: Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/)  
  Самый важный материал по теме. Объясняет:
  - alerting on SLOs;
  - burn-rate alerts;
  - multiple burn-rate alerts;
  - multi-window multi-burn-rate alerts;
  - page vs ticket;
  - рекомендуемые окна и пороги.

### Практические материалы и документация

- [Grafana SLO: Configure burn-rate notifications](https://grafana.com/docs/plugins/grafana-slo-app/latest/set-up/configure-burn-rate-notifications/)  
  Как burn-rate notifications устроены в Grafana SLO: fast-burn, slow-burn, labels, routing.

- [Grafana SLO: Introduction](https://grafana.com/docs/plugins/grafana-slo-app/latest/introduction/)  
  Объясняет SLO, error budget и burn rate в контексте Grafana.

- [Splunk Observability Cloud: Burn rate alerts](https://help.splunk.com/en/splunk-observability-cloud/create-alerts-detectors-and-service-level-objectives/create-service-level-objectives-slos/burn-rate-alerts)  
  Хорошая документация по multi-window, multi-burn-rate подходу в Splunk.

### Open-source инструменты

- [Sloth](https://sloth.dev/)  
  Open-source генератор SLO для Prometheus. Генерирует recording rules, SLO metadata и multi-window multi-burn alert rules.

- [GitHub: slok/sloth](https://github.com/slok/sloth)  
  Репозиторий Sloth. Полезен для примеров YAML-спек и Prometheus rules.

- [Pyrra](https://pyrra.dev/)  
  Open-source SLO monitoring для Prometheus. Генерирует recording rules, burn-rate alerts и dashboards.

- [Google slo-generator](https://github.com/google/slo-generator)  
  Инструмент от Google для расчета SLI, SLO, error budgets и burn rates из YAML/JSON-конфигураций.

## Alert Fatigue

### Русскоязычные материалы

- [Как жить, когда у тебя N тысяч алертов в секунду, Хабр/VK](https://habr.com/ru/companies/vk/articles/916302/)  
  Хорошая статья про шум, усталость от алертов и обработку больших потоков событий.

- [Zabbix: Укрощение шторма алертов. От гистерезиса до Telegram и авто-ремедиации, Хабр](https://habr.com/ru/articles/947884/)  
  Практический материал про false positives, гистерезис, дедупликацию и авто-ремедиацию.

- [Как принципы HumanOps применяются в Server Density, Хабр](https://habr.com/ru/companies/slurm/articles/331678/)  
  Есть важный раздел про то, что alert fatigue — это не только техническая, но и человеческая проблема.

- [Когнитивная нагрузка операторов в системах мониторинга, Хабр](https://habr.com/ru/articles/953054/)  
  Про перегрузку операторов, однотипные сигналы, шумный UI, дедупликацию и корреляцию.

### Англоязычные открытые материалы

- [Atlassian: Understanding and fighting alert fatigue](https://www.atlassian.com/incident-management/on-call/alert-fatigue)  
  Хороший обзор причин alert fatigue и способов борьбы с ним.

- [Datadog: Alert fatigue: What it is and how to prevent it](https://www.datadoghq.com/blog/best-practices-to-prevent-alert-fatigue/)  
  Практики предотвращения шумных и бесполезных алертов.

- [PagerDuty: Understanding Alert Fatigue & How to Prevent it](https://www.pagerduty.com/resources/digital-operations/learn/alert-fatigue/)  
  Материал от PagerDuty про alert fatigue, on-call и операционные последствия.

## Page Alert vs Ticket Alert

### Главная идея

`Page alert` нужен, когда промедление быстро ухудшит пользовательский impact или сожжет error budget.

`Ticket alert` нужен, когда проблема значимая, но у команды есть время разобраться без немедленного прерывания человека.

Пример из подхода Google SRE:

| Расход error budget | Окно | Burn rate | Тип реакции |
|---:|---:|---:|---|
| 2% | 1 час | 14.4x | Page |
| 5% | 6 часов | 6x | Page |
| 10% | 3 дня | 1x | Ticket |

### Материалы

- [Google SRE Workbook: Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/)  
  Ключевой материал: объясняет, когда нужен page, а когда ticket.

- [Grafana SLO: Configure burn-rate notifications](https://grafana.com/docs/plugins/grafana-slo-app/latest/set-up/configure-burn-rate-notifications/)  
  Практическая настройка fast-burn и slow-burn алертов.

- [Grafana SLO: Create SLOs](https://grafana.com/docs/plugins/grafana-slo-app/latest/create/)  
  Описывает добавление SLO alert rules и routing.

## Severity, Owner, Runbook, Escalation

### Что должно быть у хорошего алерта

У каждого важного алерта должны быть:

- `severity` — насколько срочно реагировать;
- `owner` или `team` — кто владеет сервисом и алертом;
- `service` — какой сервис затронут;
- `runbook_url` — ссылка на инструкцию;
- `dashboard_url` — ссылка на дашборд;
- `summary` — короткое описание проблемы;
- `description` — что произошло и почему это важно;
- `escalation` — кого звать дальше, если первичная реакция не помогает.

### Русскоязычные материалы

- [SRE: управление инцидентами, Хабр](https://habr.com/ru/companies/otus/articles/722892/)  
  Обзор incident management на русском: роли, процесс, управление инцидентами.

- [Повышаем производительность и безопасность мониторингом логов и метрик, Хабр](https://habr.com/ru/companies/ruvds/articles/715638/)  
  Есть полезная мысль про назначение ответственного за критически важные ресурсы.

### Первоисточники и документация

- [Google SRE Workbook: Incident Response](https://sre.google/workbook/incident-response/)  
  Роли, координация, incident commander, communications lead, operations lead, эскалация.

- [Google SRE: Incident Management Guide](https://sre.google/resources/practices-and-processes/incident-management-guide/)  
  Открытый гайд Google по управлению инцидентами.

- [Google SRE Book: Managing Incidents](https://sre.google/sre-book/managing-incidents/)  
  Глава из SRE Book про управляемые и неуправляемые инциденты.

- [Prometheus: Alerting Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)  
  Документация по alerting rules, labels и annotations.

- [Prometheus: Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/)  
  Группировка, дедупликация, routing, silences, inhibition.

- [Grafana: Labels and annotations](https://grafana.com/docs/grafana/latest/alerting/fundamentals/alert-rules/annotation-label/)  
  Как использовать labels для routing и annotations для контекста, включая `runbook_url`.

- [Yandex Cloud: Creating an escalation policy](https://yandex.cloud/en/docs/monitoring/operations/alert/create-escalation)  
  Пример документации по escalation policy: шаги уведомлений, задержки, получатели.

## Golden Signals, RED, USE

### Golden Signals

Четыре золотых сигнала Google SRE:

- `Latency` — сколько времени занимает обработка запроса;
- `Traffic` — сколько нагрузки приходит на систему;
- `Errors` — сколько запросов завершается ошибкой;
- `Saturation` — насколько система близка к пределу ресурсов.

### RED

RED обычно применяют к request-driven сервисам:

- `Rate` — количество запросов в секунду;
- `Errors` — количество или доля ошибочных запросов;
- `Duration` — длительность обработки запросов.

### USE

USE обычно применяют к ресурсам инфраструктуры:

- `Utilization` — насколько ресурс занят;
- `Saturation` — есть ли очередь или давление на ресурс;
- `Errors` — ошибки ресурса.

### Русскоязычные материалы

- [Для чего нужны золотые сигналы мониторинга и SRE, Хабр/Флант](https://habr.com/ru/companies/flant/articles/462503/)  
  Вводная статья про Golden Signals.

- [Как мониторить золотые сигналы SRE, Хабр/Слёрм](https://habr.com/ru/companies/slurm/articles/688082/)  
  Практический материал про сбор и мониторинг золотых сигналов.

- [PostgreSQL, RED, Golden Signals: руководство к действию, Хабр](https://habr.com/ru/articles/520460/)  
  Пример применения RED и Golden Signals к PostgreSQL.

- [USE, RED, PgBouncer, его настройки и мониторинг, Хабр](https://habr.com/ru/companies/okmeter/articles/420429/)  
  Практика USE/RED на PgBouncer.

- [База по метрикам в Prometheus, Хабр](https://habr.com/ru/companies/sportmaster_lab/articles/872204/)  
  Есть разделы про RED, USE и Golden Signals.

### Первоисточники и англоязычные материалы

- [Google SRE Book: Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)  
  Главный первоисточник по Golden Signals.

- [Grafana: The RED Method: How to Instrument Your Services](https://grafana.com/blog/the-red-method-how-to-instrument-your-services/)  
  Материал про RED Method и связь с USE.

- [InfoQ: Monitoring SRE's Golden Signals](https://www.infoq.com/articles/monitoring-SRE-golden-signals/)  
  Сравнение Golden Signals, RED и USE.

- [Sysdig: The four Golden Signals of Monitoring](https://www.sysdig.com/blog/golden-signals-kubernetes)  
  Обзор Golden Signals, RED и USE в контексте Kubernetes.

## Мини-глоссарий

| Термин | Значение |
|---|---|
| `SLI` | Service Level Indicator. Метрика качества сервиса. |
| `SLO` | Service Level Objective. Целевое значение SLI за период. |
| `SLA` | Service Level Agreement. Внешний договор об уровне сервиса. |
| `Error budget` | Допустимый объем ненадежности: `100% - SLO`. |
| `Burn rate` | Кратность расхода error budget относительно допустимого темпа. |
| `Fast burn` | Быстрое сгорание бюджета ошибок, обычно требует page. |
| `Slow burn` | Медленное, но устойчивое сгорание бюджета, часто подходит для ticket. |
| `Page alert` | Срочный алерт, который прерывает on-call инженера. |
| `Ticket alert` | Несрочный алерт, который превращается в задачу. |
| `Alert fatigue` | Усталость от большого количества шумных или бесполезных алертов. |
| `Runbook` | Инструкция по диагностике и mitigation конкретной проблемы. |
| `Escalation` | Процесс привлечения следующего уровня ответственности. |
| `Golden Signals` | Latency, Traffic, Errors, Saturation. |
| `RED` | Rate, Errors, Duration. |
| `USE` | Utilization, Saturation, Errors. |

## Что читать в первую очередь

Если времени мало, достаточно такого набора:

1. [SLA, SLO, SLI простыми словами и с примерами, Хабр](https://habr.com/ru/articles/956318/)
2. [Error Budget, SLO и мониторинг, Хабр](https://habr.com/ru/companies/slurm/articles/715762/)
3. [SLI/SLO. Что такое Error Budget Burn Rate на самом деле, Хабр](https://habr.com/ru/articles/1028008/)
4. [Google SRE Workbook: Alerting on SLOs](https://sre.google/workbook/alerting-on-slos/)
5. [Как жить, когда у тебя N тысяч алертов в секунду, Хабр/VK](https://habr.com/ru/companies/vk/articles/916302/)
6. [Google SRE Book: Monitoring Distributed Systems](https://sre.google/sre-book/monitoring-distributed-systems/)
