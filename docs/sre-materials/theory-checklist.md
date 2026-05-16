---
title: SRE Theory Check-list
layout: default
parent: SRE Materials
nav_order: 20
---

# SRE Theory Check-list

Связано с дорожной картой [Road to SRE](../). После каждого раздела дорожной карты возвращайся сюда и закрывай не "прочитано", а "могу объяснить и сделать".

Как пользоваться:

- Ставь галочку только если можешь объяснить тему на примере и сделать маленькое практическое задание.
- Если пункт кажется "я примерно понимаю" - то он ещё не закрыт.
- Для каждого раздела минимум один артефакт: конфиг, дашборд, runbook, postmortem, policy, демо.

---

## 1. Базовые концепции SRE

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить разницу SLA, SLO и SLI на примере реального сервиса. |
| <input type="checkbox" /> | Рассчитать error budget для 99.9%, 99.95%, 99.99% за 28/30 дней. |
| <input type="checkbox" /> | Объяснить burn rate и почему алерт по burn rate лучше простого threshold alert. |
| <input type="checkbox" /> | Назвать golden signals и объяснить, когда RED/USE удобнее. |
| <input type="checkbox" /> | Объяснить toil и привести 5 примеров toil из операционной практики. |
| <input type="checkbox" /> | Разделить mitigation, root cause и action items в incident response. |
| <input type="checkbox" /> | Объяснить blameless postmortem без превращения его в "никто не виноват, ничего не меняем". |
| <input type="checkbox" /> | Объяснить, как error budget помогает спору reliability vs velocity. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Сформулировать 2-3 SLO для API, checkout, search или feed. |
| <input type="checkbox" /> | Описать SLI: источник данных, good/bad events, окно измерения, exclusions. |
| <input type="checkbox" /> | Написать учебный postmortem: влияние на пользователей, timeline, contributing factors, action items. |
| <input type="checkbox" /> | Сделать runbook для "API отдаёт 5xx" с первыми 15 минутами действий. |
| <input type="checkbox" /> | Разобрать публичный инцидент и отделить факты от гипотез. |

### Вопросы самопроверки

- Чем SLA отличается от SLO?
- Почему 99.99% может быть плохой целью для внутреннего сервиса?
- Что делать, если error budget исчерпан, а продукт хочет релиз?
- Почему "root cause = человеческая ошибка" почти всегда плохой вывод?

### Критерий успеха

Ты можешь спроектировать SLO для сервиса, настроить логику алерта по burn rate и провести postmortem без поиска виноватого.

---

## 2. Linux, сети и низкоуровневая observability

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить process/thread, signal, file descriptor, exit code. |
| <input type="checkbox" /> | Объяснить cgroups v2 и namespaces на уровне "как это связано с контейнерами". |
| <input type="checkbox" /> | Читать systemd unit и понимать restart policy, dependencies, journal. |
| <input type="checkbox" /> | Объяснить inode, mount, disk usage vs inode exhaustion, fsync. |
| <input type="checkbox" /> | Объяснить TCP handshake, TLS handshake, keepalive, connection pooling. |
| <input type="checkbox" /> | Диагностировать DNS, packet loss, retransmits, high latency. |
| <input type="checkbox" /> | Объяснить HTTP/2 multiplexing и базовую идею HTTP/3/QUIC. |
| <input type="checkbox" /> | Понимать, где использовать `ss`, `tcpdump`, `dig`, `mtr`, `curl`, `strace`, `perf`. |
| <input type="checkbox" /> | Объяснить, что делает eBPF и почему это важно для observability и безопасности. |
| <input type="checkbox" /> | Объяснить базовые проблемы PostgreSQL: locks, slow queries, replication lag, backup/PITR. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Найти причину, почему systemd service не стартует. |
| <input type="checkbox" /> | Снять `tcpdump` для запроса и показать DNS/TCP/TLS/HTTP этапы. |
| <input type="checkbox" /> | Найти процесс, который держит порт или файл. |
| <input type="checkbox" /> | Написать `bpftrace` one-liner для системных вызовов или TCP connect. |
| <input type="checkbox" /> | Снять flamegraph или profiling snapshot для процесса, упирающегося в CPU. |
| <input type="checkbox" /> | Разобрать большой access log и найти top endpoints по p99/error rate. |

### Вопросы самопроверки

- Почему `df -h` показывает место, но приложение всё равно не пишет файл?
- Чем timeout отличается от connection refused?
- Почему p99 важнее average latency?
- Как понять, что проблема в DNS, а не в приложении?

### Критерий успеха

Ты можешь зайти на Linux-сервер, быстро собрать факты по CPU, memory, disk, network и processes, а не гадать вслепую.

---

## 3. IaC, GitOps и автоматизация

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить Terraform/OpenTofu state, locking, drift, plan/apply/destroy. |
| <input type="checkbox" /> | Объяснить разницу Terraform и OpenTofu в лицензии, управлении проектом и ситуациях, где это важно. |
| <input type="checkbox" /> | Спроектировать modules и environments без копипасты на 1000 строк. |
| <input type="checkbox" /> | Объяснить remote state backend и риски хранения secrets в state. |
| <input type="checkbox" /> | Понимать, где Pulumi лучше/хуже HCL-подхода. |
| <input type="checkbox" /> | Объяснить Crossplane как API платформы поверх Kubernetes. |
| <input type="checkbox" /> | Объяснить GitOps reconciliation loop. |
| <input type="checkbox" /> | Сравнить Argo CD и Flux на уровне базовых компромиссов. |
| <input type="checkbox" /> | Объяснить путь secret'ов: SOPS, External Secrets, Vault/OpenBao, KMS. |
| <input type="checkbox" /> | Понимать policy checks в CI/CD: Checkov/tfsec/OPA/Infracost. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Развернуть сеть, VM/cluster, bucket и managed DB через Terraform/OpenTofu. |
| <input type="checkbox" /> | Настроить remote state с locking. |
| <input type="checkbox" /> | Написать reusable module с version pinning. |
| <input type="checkbox" /> | Настроить Argo CD app-of-apps или ApplicationSet. |
| <input type="checkbox" /> | Сделать rollback через Git revert. |
| <input type="checkbox" /> | Зашифровать secret через SOPS или подключить External Secrets. |
| <input type="checkbox" /> | Добавить проверку безопасности и стоимости IaC в CI. |

### Вопросы самопроверки

- Почему нельзя хранить state "просто в репозитории"?
- Что делать, если ручное изменение в cloud вызвало drift?
- Почему GitOps не равен CI/CD?
- Как безопасно передавать secret в Kubernetes?

### Критерий успеха

Ты можешь воспроизводимо поднять инфраструктуру, объяснить состояние, откатить изменение и не потерять secrets в state/logs.

---

## 4. Контейнеры, Kubernetes и управление трафиком

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить image layers, multi-stage build, digest vs tag. |
| <input type="checkbox" /> | Объяснить requests/limits, QoS classes, OOMKilled. |
| <input type="checkbox" /> | Отличить readiness, liveness и startup probes. |
| <input type="checkbox" /> | Объяснить Deployment, StatefulSet, DaemonSet, Job, CronJob. |
| <input type="checkbox" /> | Объяснить Service, EndpointSlice, DNS inside cluster. |
| <input type="checkbox" /> | Понимать CNI, NetworkPolicy и kube-proxy/eBPF datapath на базовом уровне. |
| <input type="checkbox" /> | Объяснить Gateway API и почему для новых сценариев он предпочтительнее Ingress. |
| <input type="checkbox" /> | Объяснить Helm vs Kustomize. |
| <input type="checkbox" /> | Объяснить CRD/operator/reconciliation loop. |
| <input type="checkbox" /> | Объяснить Kyverno и OPA/Gatekeeper как admission policy. |
| <input type="checkbox" /> | Объяснить компромиссы service mesh: mTLS, retries, traffic shifting, стоимость и сложность. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Собрать container image с non-root user и multi-stage build. |
| <input type="checkbox" /> | Задеплоить приложение в kind/k3d с probes, resources, PDB. |
| <input type="checkbox" /> | Настроить маршрут Gateway API и TLS через cert-manager. |
| <input type="checkbox" /> | Написать Kyverno policy: запрет `:latest` и privileged pods. |
| <input type="checkbox" /> | Настроить HPA и объяснить, почему он масштабирует именно так. |
| <input type="checkbox" /> | Сделать rollout/rollback/canary. |
| <input type="checkbox" /> | Написать минимальный operator walkthrough или разобрать готовый operator. |

### Вопросы самопроверки

- Почему pod в CrashLoopBackOff?
- Почему readiness failed не всегда означает, что приложение умерло?
- Чем Service отличается от Ingress/Gateway?
- Когда service mesh вреднее, чем полезнее?

### Критерий успеха

Ты можешь не только "задеплоить YAML", но и объяснить сетевой путь запроса, rollout, ресурсы, policy и восстановление после сбоя.

---

## 5. Observability через OpenTelemetry

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить OpenTelemetry API, SDK, Collector, OTLP, semantic conventions. |
| <input type="checkbox" /> | Объяснить context propagation, trace_id/span_id, baggage. |
| <input type="checkbox" /> | Объяснить metrics/logs/traces/profiles и почему это не должны быть изолированные силосы. |
| <input type="checkbox" /> | Спроектировать Collector pipeline: receivers, processors, exporters. |
| <input type="checkbox" /> | Объяснить head-based vs tail-based sampling. |
| <input type="checkbox" /> | Понимать label cardinality и почему она ломает бюджеты и Prometheus. |
| <input type="checkbox" /> | Объяснить Prometheus, VictoriaMetrics, Mimir, Loki, Tempo, Jaeger на уровне назначения. |
| <input type="checkbox" /> | Объяснить SLO tooling: Sloth, Pyrra, OpenSLO. |
| <input type="checkbox" /> | Настроить алерт с владельцем, severity, runbook и правилами silence. |
| <input type="checkbox" /> | Объяснить экономику телеметрии: retention, sampling, redaction, cost attribution. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Инструментировать приложение через OTel SDK. |
| <input type="checkbox" /> | Отправить telemetry в OTel Collector и дальше в backend. |
| <input type="checkbox" /> | Связать логи и трейсы через trace_id/span_id. |
| <input type="checkbox" /> | Сделать dashboard по golden signals. |
| <input type="checkbox" /> | Настроить burn-rate alerts для SLO. |
| <input type="checkbox" /> | Найти и исправить high-cardinality label. |
| <input type="checkbox" /> | Настроить sampling policy: сохранять ошибки и медленные traces. |

### Вопросы самопроверки

- Почему "у нас есть Grafana" не равно "у нас есть observability"?
- Чем telemetry pipeline отличается от backend?
- Почему не надо собирать все traces навсегда?
- Как понять, что алерт actionable?

### Критерий успеха

Ты можешь спроектировать наблюдаемость от SLO до telemetry pipeline и получить полезный сигнал во время инцидента.

---

## 6. Облака, private cloud и hybrid

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить compute, managed DB, object storage, load balancer, IAM. |
| <input type="checkbox" /> | Понимать VPC/VNet, subnets, route tables, NAT, private endpoints. |
| <input type="checkbox" /> | Объяснить IAM least privilege, сервисные аккаунты и workload identity. |
| <input type="checkbox" /> | Объяснить компромиссы managed Kubernetes. |
| <input type="checkbox" /> | Понимать managed PostgreSQL/Redis/Kafka: limits, backups, maintenance windows. |
| <input type="checkbox" /> | Объяснить RTO/RPO, backup, restore test, PITR. |
| <input type="checkbox" /> | Сравнить public cloud, private cloud, hybrid и on-prem по рискам. |
| <input type="checkbox" /> | Понимать российский контур: Yandex Cloud, VK Cloud, Selectel, MWS, Cloud.ru. |
| <input type="checkbox" /> | Понимать international contour: AWS, GCP, Azure. |
| <input type="checkbox" /> | Объяснить основные драйверы стоимости: egress, managed DB, storage, observability. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Развернуть HA-приложение в managed Kubernetes. |
| <input type="checkbox" /> | Подключить managed PostgreSQL и object storage. |
| <input type="checkbox" /> | Настроить IAM для CI/CD без долгоживущих admin keys. |
| <input type="checkbox" /> | Сделать backup и restore test. |
| <input type="checkbox" /> | Описать DR-план с RTO/RPO. |
| <input type="checkbox" /> | Сделать отчет о стоимости по сервису/namespace. |

### Вопросы самопроверки

- Почему multi-region не всегда нужен?
- Что сломается при потере одной AZ?
- Где лежат бэкапы и кто проверял restore?
- Почему egress может стоить дороже compute?

### Критерий успеха

Ты можешь объяснить архитектуру cloud-сервиса, его failure modes, recovery plan и основные статьи расходов.

---

## 7. Platform Engineering и IDP

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить Platform as a Product. |
| <input type="checkbox" /> | Объяснить golden paths/paved roads и почему эти "безопасные пути" должны быть удобными. |
| <input type="checkbox" /> | Понимать Team Topologies и роль platform team. |
| <input type="checkbox" /> | Объяснить Internal Developer Platform vs Internal Developer Portal. |
| <input type="checkbox" /> | Объяснить Backstage: catalog, TechDocs, Scaffolder, plugins. |
| <input type="checkbox" /> | Понимать scorecards: владение, документация, SLO, алерты, security, cost. |
| <input type="checkbox" /> | Объяснить DORA metrics и почему они не должны быть KPI-дубинкой. |
| <input type="checkbox" /> | Объяснить SPACE/DevEx подход к измерению фрикции. |
| <input type="checkbox" /> | Понимать platform API lifecycle: versioning, deprecation, support, exceptions. |
| <input type="checkbox" /> | Объяснить мысль DORA 2025: AI усиливает систему работы, а не заменяет фундамент. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Развернуть Backstage и добавить 3 сервиса в catalog. |
| <input type="checkbox" /> | Сделать шаблон нового сервиса с CI, сборкой image, Helm, OTel, заготовкой SLO и runbook. |
| <input type="checkbox" /> | Добавить scorecards и метаданные владения. |
| <input type="checkbox" /> | Описать support model платформы: кто владелец, SLA/SLO платформы, как просить исключение. |
| <input type="checkbox" /> | Провести мини-интервью с разработчиком и найти одну боль в платформе. |
| <input type="checkbox" /> | Улучшить golden path на основе найденной боли. |

### Вопросы самопроверки

- Почему платформа должна конкурировать за добровольное использование?
- Чем портал отличается от платформы?
- Что лучше: запретить всё policy или дать безопасный шаблон?
- Как понять, что платформа помогает, а не добавляет бюрократию?

### Критерий успеха

Ты можешь построить маленькую IDP-демо и объяснить, как она снижает toil, ускоряет delivery и повышает надежность.

---

## 8. AI в SRE и надежность AI-систем

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Назвать реальные полезные применения AI в SRE: суммаризация, корреляция, черновик runbook, черновик postmortem. |
| <input type="checkbox" /> | Объяснить, почему AI не должен получать write-доступ в production без ограничителей. |
| <input type="checkbox" /> | Объяснить MCP/tool-calling на уровне "LLM вызывает ограниченные инструменты". |
| <input type="checkbox" /> | Спроектировать incident assistant в режиме чтения. |
| <input type="checkbox" /> | Объяснить RAG по runbooks/postmortems и его ограничения. |
| <input type="checkbox" /> | Объяснить риски: hallucination, data leakage, prompt injection, runaway automation. |
| <input type="checkbox" /> | Объяснить LLM observability: model, prompt version, token usage, tool calls, latency, стоимость. |
| <input type="checkbox" /> | Сформулировать SLO для AI/LLM-сервиса: availability, latency, стоимость, proxy-метрика качества. |
| <input type="checkbox" /> | Объяснить redaction перед отправкой telemetry в LLM. |
| <input type="checkbox" /> | Разделить human-in-the-loop, human-on-the-loop и полностью автоматические действия. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Собрать RAG по своим runbooks. |
| <input type="checkbox" /> | Сделать read-only tool/MCP для чтения Kubernetes events или metrics. |
| <input type="checkbox" /> | Сгенерировать черновик incident summary и вручную проверить факты. |
| <input type="checkbox" /> | Добавить redaction secrets/PII в telemetry pipeline. |
| <input type="checkbox" /> | Для LLM-функции собрать metrics: latency, errors, token usage, стоимость. |
| <input type="checkbox" /> | Описать allowlist действий, которые AI может предложить, но не выполнить сам. |

### Вопросы самопроверки

- Где AI реально снижает toil, а где просто добавляет красивый UI?
- Почему "AI нашёл root cause" нельзя принимать без evidence?
- Какие данные нельзя отправлять внешнему LLM?
- Какие действия в production можно автоматизировать первыми?

### Критерий успеха

Ты можешь безопасно встроить AI как помощника расследования, не отдавая ему бесконтрольный production.

---

## 9. Security, supply chain и Policy as Code

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить SLSA, provenance, attestation. |
| <input type="checkbox" /> | Объяснить Sigstore/cosign/Rekor на уровне подписи image. |
| <input type="checkbox" /> | Объяснить SBOM: SPDX и CycloneDX. |
| <input type="checkbox" /> | Понимать Trivy, Grype, Syft. |
| <input type="checkbox" /> | Объяснить Kubernetes RBAC и least privilege. |
| <input type="checkbox" /> | Объяснить Pod Security Standards. |
| <input type="checkbox" /> | Объяснить admission control через Kyverno/OPA. |
| <input type="checkbox" /> | Понимать secrets management: Vault/OpenBao, cloud secret manager, SOPS. |
| <input type="checkbox" /> | Объяснить runtime security: Falco, Tetragon, eBPF events. |
| <input type="checkbox" /> | Понимать Zero Trust: workload identity, mTLS, SPIFFE/SPIRE. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Сгенерировать SBOM для image. |
| <input type="checkbox" /> | Просканировать image через Trivy/Grype. |
| <input type="checkbox" /> | Подписать image через cosign. |
| <input type="checkbox" /> | Проверить подпись на admission через policy. |
| <input type="checkbox" /> | Написать least-privilege Role/RoleBinding. |
| <input type="checkbox" /> | Запретить privileged/root containers через Kyverno/OPA. |
| <input type="checkbox" /> | Запустить kube-bench и разобрать findings. |
| <input type="checkbox" /> | Настроить Falco/Tetragon test rule. |

### Вопросы самопроверки

- Почему image scan в CI не заменяет runtime security?
- Что опасного в Kubernetes Secret по умолчанию?
- Как понять, что service account слишком привилегирован?
- Что делать с критической CVE в base image?

### Критерий успеха

Ты можешь встроить security checks в delivery path и объяснить, какие риски закрыты, а какие остались.

---

## 10. FinOps, chaos, performance и data reliability

### Теория

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | Объяснить FinOps phases: inform, optimize, operate. |
| <input type="checkbox" /> | Объяснить unit economics: cost per request/customer/namespace/job. |
| <input type="checkbox" /> | Понимать OpenCost/Kubecost, Infracost, billing export. |
| <input type="checkbox" /> | Объяснить rightsizing: requests/limits, HPA, VPA, Cluster Autoscaler, Karpenter. |
| <input type="checkbox" /> | Объяснить spot/preemptible risks и interruption handling. |
| <input type="checkbox" /> | Объяснить telemetry cost: sampling, retention, cardinality. |
| <input type="checkbox" /> | Объяснить chaos engineering: steady state, hypothesis, blast radius. |
| <input type="checkbox" /> | Понимать LitmusChaos, Chaos Mesh, game days. |
| <input type="checkbox" /> | Объяснить load testing: RPS, concurrency, p95/p99, saturation. |
| <input type="checkbox" /> | Объяснить data reliability: freshness, completeness, duplication, schema drift. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Сделать дашборд стоимости по namespace/service. |
| <input type="checkbox" /> | Найти завышенные requests/limits и предложить rightsizing. |
| <input type="checkbox" /> | Добавить Infracost/OpenCost check. |
| <input type="checkbox" /> | Провести game day с потерей зависимости. |
| <input type="checkbox" /> | Сделать k6/Locust/Gatling нагрузочный тест. |
| <input type="checkbox" /> | Найти точку насыщения и описать bottleneck. |
| <input type="checkbox" /> | Добавить data freshness SLO для pipeline. |
| <input type="checkbox" /> | Описать backfill/runbook для data incident. |

### Вопросы самопроверки

- Почему экономия compute может ухудшить надежность?
- Как ограничить счет за observability без потери видимости во время инцидента?
- Почему chaos experiment без гипотезы бесполезен?
- Чем data freshness incident отличается от API outage?

### Критерий успеха

Ты можешь говорить о надежности вместе со стоимостью, нагрузкой, проверкой устойчивости и качеством данных.

---

## 11. Итоговый практический проект

### Обязательные компоненты

| Проверка | Что должно быть |
| --- | --- |
| <input type="checkbox" /> | Git-репозиторий с понятной структурой. |
| <input type="checkbox" /> | OpenTofu/Terraform для инфраструктуры. |
| <input type="checkbox" /> | Kubernetes cluster: kind/k3d или managed. |
| <input type="checkbox" /> | Argo CD/Flux для GitOps. |
| <input type="checkbox" /> | Gateway API + TLS. |
| <input type="checkbox" /> | External Secrets или SOPS. |
| <input type="checkbox" /> | OTel Collector. |
| <input type="checkbox" /> | Backend для metrics/logs/traces + Grafana. |
| <input type="checkbox" /> | Приложение с OTel instrumentation. |
| <input type="checkbox" /> | SLO definitions + burn-rate alerts. |
| <input type="checkbox" /> | Runbooks и шаблон postmortem. |
| <input type="checkbox" /> | SBOM, image scan, image signing или admission policy. |
| <input type="checkbox" /> | Оценка стоимости по namespace/service. |
| <input type="checkbox" /> | Chaos experiment. |
| <input type="checkbox" /> | AI/RAG helper только с доступом на чтение. |

### Демонстрация

| Проверка | Сценарий |
| --- | --- |
| <input type="checkbox" /> | Сломать сервис контролируемо. |
| <input type="checkbox" /> | Получить алерт с владельцем, runbook и severity. |
| <input type="checkbox" /> | Определить влияние на пользователей и масштаб проблемы. |
| <input type="checkbox" /> | Найти связь metrics/logs/traces. |
| <input type="checkbox" /> | Выполнить mitigation/rollback. |
| <input type="checkbox" /> | Написать postmortem. |
| <input type="checkbox" /> | Закрыть хотя бы один action item. |

### Вопросы самопроверки

- Что в проекте похоже на production, а что является учебным упрощением?
- Как восстановить систему с нуля?
- Где single points of failure?
- Какое следующее улучшение надежности самое ценное?

### Критерий успеха

Ты можешь показать полный reliability loop: deploy, observe, alert, mitigate, learn, improve.

---

## 12. Подготовка к собеседованиям

### Темы

| Проверка | Что нужно уметь |
| --- | --- |
| <input type="checkbox" /> | System design с SLO, scaling, queues, caching, failover, degradation. |
| <input type="checkbox" /> | Incident response: влияние на пользователей, масштаб, mitigation, коммуникации, RCA после восстановления. |
| <input type="checkbox" /> | Linux debugging: CPU, memory, disk, network, process. |
| <input type="checkbox" /> | Kubernetes debugging: OOMKilled, DNS, scheduling, probes, storage. |
| <input type="checkbox" /> | Observability design: SLI, labels, sampling, alerts, dashboards. |
| <input type="checkbox" /> | IaC/GitOps: state, drift, promotion, rollback, secrets. |
| <input type="checkbox" /> | Security: RBAC, supply chain, secrets leakage, image signing. |
| <input type="checkbox" /> | FinOps: расследование скачка стоимости и безопасная оптимизация. |
| <input type="checkbox" /> | AI-aware SRE: сценарии применения, риски, ограничения. |
| <input type="checkbox" /> | Coding: Python/Go scripts for logs, APIs, CLIs, automation. |

### Практика

| Проверка | Задание |
| --- | --- |
| <input type="checkbox" /> | Проговорить инцидент "API 500" за 15 минут. |
| <input type="checkbox" /> | Спроектировать SLO для checkout/payment/search/feed. |
| <input type="checkbox" /> | Разобрать публичный postmortem. |
| <input type="checkbox" /> | Написать скрипт анализа access log: p99, error rate, top endpoints. |
| <input type="checkbox" /> | Объяснить Kubernetes rollout/rollback на доске. |
| <input type="checkbox" /> | Спроектировать observability pipeline для микросервисов. |
| <input type="checkbox" /> | Ответить на вопрос "счет за облако вырос в 2 раза, что делаешь?" |

### Вопросы самопроверки

- Как ты начинаешь расследование production incident?
- Какие первые 5 команд ты выполнишь на Linux-сервере?
- Какой SLO ты выберешь для payment API?
- Как ты используешь AI в on-call безопасно?

### Критерий успеха

Ты отвечаешь структурно: assumptions, влияние, hypothesis, evidence, mitigation, компромиссы, follow-up.

---

## Финальная самооценка

| Уровень | Признаки |
| --- | --- |
| Готов к junior SRE | Понимаешь SLO, Linux basics, Kubernetes basics, можешь дежурить с runbooks. |
| Готов к middle SRE | Сам проектируешь observability, IaC/GitOps flow, incident process и security baseline. |
| Готов к strong middle / senior SRE | Держишь надежность как систему: продуктовые SLO, platform golden paths, стоимость, security, AI-ограничители, learning from incidents. |

## Минимальный портфолио-набор

- [ ] Репозиторий итогового проекта.
- [ ] 2-3 SLO definitions.
- [ ] 1 postmortem.
- [ ] 2 runbooks.
- [ ] 1 dashboard.
- [ ] 1 alert rule с burn rate.
- [ ] 1 IaC module.
- [ ] 1 GitOps setup.
- [ ] 1 security policy.
- [ ] 1 отчет о стоимости.
- [ ] 1 chaos experiment report.
- [ ] 1 AI/RAG helper с ограничениями и доступом только на чтение.
