---
title: SRE Materials
layout: default
nav_order: 2
has_children: true
---


# Road to SRE

Актуализировано: 2026-05-16.

Эта дорожная карта рассчитана на путь от уверенного junior/middle backend- или DevOps-инженера к SRE / Platform / Reliability engineer. В 2026 SRE уже не сводится к дежурствам и починке Kubernetes: надежность стала пересечением SLO, incident response, cloud-native платформ, observability, security, FinOps и аккуратного применения AI в операционной работе.

Главный принцип: учить не набор модных инструментов, а способность держать production понятным, измеримым, безопасным и восстанавливаемым.

## Связанные материалы

- [SRE Theory Check-list](theory-checklist/) — чек-листы и практические задания к разделам этой дорожной карты.
- [SRE, SLO и алертинг](slo-alerting/) — подборка открытых материалов по SLO, error budget, burn rate, alert fatigue и incident response.

{: .note }
Публичная версия очищена от Obsidian metadata и рабочего контекста. Если материал пришел из рабочей заметки, здесь оставлены только открытые ссылки и обобщенные практики.

## Как пользоваться

- Иди по разделам последовательно, но не жди "идеального знания" перед практикой.
- На каждый раздел делай маленький артефакт: конфиг, дашборд, runbook, postmortem, policy, демо-кластер.
- Проверяй себя по [SRE Theory check-list](theory-checklist/): там чек-листы для каждого раздела.
- Не коллекционируй сертификаты вместо опыта. Сертификат полезен как рамка, но портфолио сильнее.
- Не принимай вендорский маркетинг за индустриальный стандарт. Если утверждение звучит как "все уже перешли", ищи первоисточник.

## Карта по времени

| Этап | Фокус | Оценка времени |
| --- | --- | --- |
| 1 | Базовые концепции SRE | 1-2 недели |
| 2 | Linux, сети, низкоуровневая observability | 1-2 месяца |
| 3 | IaC, GitOps, автоматизация | 2-3 месяца |
| 4 | Контейнеры, Kubernetes, управление трафиком | 2-3 месяца |
| 5 | Observability через OpenTelemetry | 1-2 месяца |
| 6 | Облака / private cloud / hybrid | 1-2 месяца |
| 7 | Platform Engineering и IDP | 1-2 месяца |
| 8 | AI в SRE и надежность AI-систем | 1 месяц |
| 9 | Security, supply chain, Policy as Code | 1-2 месяца |
| 10 | FinOps, chaos, performance, data reliability | 2-3 месяца |
| 11 | Итоговый практический проект | 1-2 месяца |
| 12 | Подготовка к собеседованиям | 1 месяц |

---

## 1. Базовые концепции SRE

### Что изучать

- SLA, SLO, SLI, error budget, burn rate.
- Toil: как распознавать, измерять и постепенно убирать ручную операционку.
- Blameless postmortem, incident command, роли IC / scribe / comms / ops.
- Reliability vs velocity: error budget как язык переговоров с продуктом.
- Resilience engineering: сложные системы ломаются не из-за "одной причины", а из-за комбинации условий.
- Golden signals, RED, USE как рамки для выбора SLI.
- Алерты на основе SLO: сигнал по влиянию на пользователей, а не по "CPU 80%".

### Что почитать

- "Site Reliability Engineering" и "The Site Reliability Workbook" от Google.
- "Implementing Service Level Objectives" Alex Hidalgo.
- "Seeking SRE" David Blank-Edelman.
- "Learning from Incidents in Software".
- learningfromincidents.io и SRE Weekly.

### Практика

- Выбери один сервис и сформулируй 2-3 пользовательских SLO.
- Посчитай error budget для 99.9%, 99.95%, 99.99% за 28/30 дней.
- Напиши postmortem для учебного инцидента без поиска виноватого.
- Сделай runbook для "API начал отдавать 5xx".

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 2. Linux, сети и низкоуровневая observability

### Что изучать

- Linux: процессы, signals, systemd, journald, users/groups, permissions, namespaces, cgroups v2.
- Storage: ext4/xfs, inode, mount, LVM, RAID basics, I/O saturation, fsync.
- Сети: TCP/IP, DNS, HTTP/1.1, HTTP/2, HTTP/3/QUIC, TLS 1.3, NAT, L4/L7 balancing, anycast, CDN.
- Диагностика: latency, packet loss, retransmits, DNS timeout, connection pool exhaustion.
- eBPF: трассировка системных вызовов, сетевая observability, события безопасности. Инструменты: bpftrace, bcc, Cilium Hubble, Pixie, Parca.
- Базы: PostgreSQL basics, indexes, locks, replication, backups, PITR; Redis, Kafka, object storage.
- Distributed systems basics: quorum, consensus, idempotency, retries, backoff+jitter, circuit breaker, bulkhead.

### Что использовать в 2026

- `ss` вместо `netstat`, `nft` вместо новых `iptables`-сетапов.
- `tcpdump`, `dig`, `mtr`, `curl`, `openssl s_client`, `perf`, `strace`.
- Ubuntu 24.04 LTS / Debian 12 / AlmaLinux / Rocky Linux.
- Continuous profiling: Parca или Pyroscope.

### Практика

- Подними VM, сломай DNS, firewall, systemd unit, disk pressure и восстанови.
- Сними `tcpdump` для HTTP/TLS запроса и объясни, где DNS, TCP handshake, TLS handshake.
- Напиши `bpftrace`-скрипт для подсчета открытий файлов или TCP connect.
- Найди p95/p99 latency в access log на 1-10 GB.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 3. IaC, GitOps и автоматизация

### Что изучать

- Terraform и OpenTofu: HCL, state, remote backend, locking, modules, workspaces, provider versions.
- OpenTofu: открытый форк Terraform, принятый в CNCF Sandbox; хороший выбор для команд, которым важны открытая модель управления и независимость от одного вендора.
- Terraform: всё ещё широко используется, особенно там, где уже есть HCP Terraform / Terraform Enterprise.
- Pulumi: IaC на Go/Python/TypeScript, полезен там, где нужна логика на обычном языке.
- Crossplane: cloud resources как Kubernetes API; сильный инструмент для платформенных команд.
- Ansible: конфигурация серверов, bare metal, унаследованные системы, простые orchestration-задачи.
- GitOps: Argo CD и Flux. Для Kubernetes в 2026 это выбор по умолчанию, если нет веской причины делать иначе.
- Secrets: Vault / OpenBao, SOPS, External Secrets Operator, Sealed Secrets, cloud KMS/secret manager.
- Policy in CI/CD: Checkov, tfsec, OPA/Conftest, Infracost.

### Практика

- Разверни сеть, VM, managed DB и bucket через OpenTofu/Terraform.
- Настрой remote state с locking и разделением окружений.
- Подними Argo CD с app-of-apps или ApplicationSet.
- Сделай rollback через Git revert, а не ручной `kubectl edit`.
- Добавь SOPS или External Secrets Operator.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 4. Контейнеры, Kubernetes и управление трафиком

### Что изучать

- Docker / Podman / BuildKit / containerd: images, layers, multi-stage builds, rootless, SBOM.
- Kubernetes: Pod, Deployment, StatefulSet, DaemonSet, Job, Service, ConfigMap, Secret, HPA, PDB, requests/limits, probes, scheduling.
- Storage: PV/PVC, CSI, Stateful workloads, backup/restore.
- Networking: CNI, kube-proxy vs eBPF datapath, NetworkPolicy, DNS inside cluster.
- Gateway API: в 2026 это предпочтительный стандарт для новых сценариев входящего трафика и маршрутизации; Ingress оставь для старых схем.
- Gateway API v1.5 продвинул ряд возможностей в Standard channel: ListenerSet, TLSRoute, HTTPRoute CORS filter, client certificate validation, certificate selection for TLS origination и ReferenceGrant.
- Helm и Kustomize: когда шаблонизировать, когда патчить.
- Operators: CRD, reconciliation loop, Kubebuilder / Operator SDK.
- Policy as Code: Kyverno и OPA/Gatekeeper.
- Service Mesh: Istio Ambient, Linkerd, Cilium Service Mesh. Важно понимать компромиссы, а не ставить mesh "для галочки".
- Дистрибутивы: managed Kubernetes, vanilla, k3s/k3d/kind, Talos Linux для immutable Kubernetes nodes.

### Практика

- Подними локальный кластер через kind/k3d.
- Собери приложение с readiness/liveness/startup probes, resources, PDB.
- Настрой Gateway API, cert-manager, ExternalDNS.
- Напиши Kyverno policy: запрет `:latest`, обязательные requests/limits, запрет privileged.
- Напиши простой operator или хотя бы CRD + controller walkthrough.
- Протестируй rollout, rollback, canary/blue-green.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 5. Observability через OpenTelemetry

### Что изучать

- OpenTelemetry: API, SDK, Collector, OTLP, semantic conventions, context propagation.
- Сигналы: metrics, logs, traces; profiles как развивающееся направление. Не застревать в старой "three pillars" модели как в отдельных силосах.
- Collector pipeline: receivers, processors, exporters; batching, sampling, filtering, redaction.
- Metrics: Prometheus, VictoriaMetrics, Mimir.
- Logs: Loki, VictoriaLogs, ClickHouse, OpenSearch/Elastic там, где уже есть.
- Traces: Tempo, Jaeger, Honeycomb/Datadog/New Relic в commercial-средах.
- SLO tooling: Sloth, Pyrra, OpenSLO, Nobl9.
- Alerting: Alertmanager + on-call слой вроде PagerDuty / Grafana OnCall / incident.io / Rootly.
- Экономика телеметрии: cardinality, retention, sampling, cost attribution.

### Важная поправка на 2026

OpenTelemetry - хороший выбор по умолчанию для новой инструментализации. На май 2026 формулировку про статус надо обновить: CNCF указывает, что OpenTelemetry перешел из Incubating в Graduated 11 мая 2026. Более практичная формулировка: это зрелый вендор-нейтральный стандарт для инструментирования, сбора и транспортировки телеметрии, но зрелость конкретных SDK, semantic conventions и сигналов все равно нужно проверять по компонентам.

### Практика

- Инструментируй приложение через OTel SDK и отправь traces/metrics/logs в Collector.
- Разверни LGTM stack или VictoriaMetrics + VictoriaLogs + Grafana.
- Настрой trace_id correlation между логами и трейсами.
- Сделай burn-rate alerts по SLO.
- Ограничь cardinality: найди плохие labels и перепиши схему.
- Добавь tail-based sampling для ошибок и медленных traces.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 6. Облака, private cloud и hybrid

### Что изучать

- Compute, networking, IAM, object storage, managed PostgreSQL/Redis/Kafka, managed Kubernetes.
- AWS/GCP/Azure для международного рынка и общей модели облаков.
- Yandex Cloud, VK Cloud, Selectel, MWS, Cloud.ru для российского контура.
- S3-compatible storage, IAM/сервисные аккаунты, KMS/Lockbox/Secrets Manager.
- Private cloud/on-prem: OpenStack, Proxmox, bare-metal Kubernetes, storage/network trade-offs.
- HA/DR: multi-AZ, backups, PITR, cross-region replication, RTO/RPO.
- Cloud networking: VPC/VNet, route tables, NAT gateways, load balancers, private endpoints.
- Cost model: egress, managed DB, storage tiers, observability bills.

### Практика

- Разверни HA-приложение в managed Kubernetes с managed PostgreSQL и object storage.
- Опиши recovery plan: какие бэкапы, где лежат, как часто проверяются restore-тестом.
- Настрой least-privilege IAM для CI/CD и workloads.
- Сделай отчет о стоимости по ресурсу/namespace/service.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 7. Platform Engineering и Internal Developer Platforms

### Что изучать

- Platform as a Product: платформа как продукт для внутренних разработчиков.
- Golden paths / paved roads: стандартизированные и поддерживаемые "безопасные пути", а не запреты ради запретов.
- Team Topologies: stream-aligned, platform, enabling, complicated-subsystem teams.
- Internal Developer Platform / Portal: service catalog, templates, docs, scorecards, ownership.
- Backstage: Software Catalog, TechDocs, Scaffolder, plugins, scorecards.
- Port, Cortex, OpsLevel, Humanitec, Kratix как альтернативы/дополнения.
- DORA metrics и SPACE/DevEx: измерять не только скорость, но и стабильность, фрикцию, ownership.
- Platform governance: API платформы, versioning, deprecation, support model, paved road exceptions.

### Актуальность 2026

DORA/Google 2025 формулирует важную мысль: AI не чинит систему работы сам по себе, а усиливает то, что уже есть. Поэтому качественная внутренняя платформа, быстрые циклы обратной связи, тесты и ясное владение становятся фундаментом, а не приятным дополнением.

### Практика

- Разверни Backstage и заведи catalog-info для 3 сервисов.
- Сделай шаблон нового сервиса: репозиторий, CI, Dockerfile, Helm chart, базовые настройки OTel, заготовка SLO, заготовка runbook.
- Добавь scorecards: владелец, документация, алерты, SLO, дашборды, политика бэкапов.
- Проведи интервью с "внутренним пользователем" платформы и исправь один реальный pain point.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 8. AI в SRE и надежность AI-систем

### Что изучать

- AI-assisted incident response: суммаризация, корреляция алертов, исследование логов/трейсов, черновики runbook и postmortem.
- RAG по runbooks/wiki/postmortems: поиск похожих инцидентов и процедур.
- MCP и tool-calling: как безопасно подключать LLM к Grafana, Kubernetes, PagerDuty, GitHub, cloud APIs.
- Ограничители: режим чтения по умолчанию, approvals, allowlist действий, audit log, rollback, ограничения blast radius.
- Надежность AI/LLM-сервисов: latency, availability, error rate, стоимость запроса, token usage, quality/evaluation metrics, fallback.
- LLM observability: OTel GenAI semantic conventions, traces по tool calls, prompt/version tracking, redaction.
- Риски: hallucinations, data leakage, prompt injection, runaway automation, feedback loops.

### Здоровая позиция на 2026

AI полезен как ускоритель расследования и генератор черновиков, но автоматические действия в production без человека допустимы только для узких, обратимых и заранее протестированных процедур. Сначала телеметрия, runbooks, владение сервисами и права доступа; потом AI-агенты.

### Практика

- Сделай RAG по своим runbooks и postmortems.
- Напиши MCP/tool-wrapper, который умеет только читать события, метрики и логи.
- Сгенерируй черновик postmortem по timeline, но вручную проверь факты.
- Добавь redaction для secrets/PII в telemetry pipeline перед отправкой в LLM.
- Для LLM-сервиса определи SLO по latency, error rate, стоимости и proxy-метрике качества.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 9. Security, supply chain и Policy as Code

### Что изучать

- Безопасность supply chain: SLSA, Sigstore/cosign, Rekor, in-toto, provenance, attestations.
- SBOM: SPDX, CycloneDX; генерация через Syft, проверка через Grype/Trivy.
- Безопасность образов: distroless, minimal base images, rootless, read-only FS, seccomp/AppArmor.
- Безопасность Kubernetes: RBAC, OIDC, NetworkPolicy, Pod Security Standards, admission control.
- Runtime security: Falco, Tetragon, eBPF events.
- Secrets: Vault/OpenBao, cloud secret managers, External Secrets Operator, SOPS.
- Compliance: CIS Benchmarks, kube-bench, audit logs.
- Zero Trust: workload identity, mTLS, SPIFFE/SPIRE.

### Практика

- Собери image, сгенерируй SBOM, подпиши cosign, проверь подпись при admission.
- Запрети privileged pods через Kyverno/OPA.
- Настрой least-privilege RBAC для сервисного аккаунта.
- Просканируй cluster benchmark через kube-bench.
- Настрой Falco/Tetragon и поймай тестовое suspicious event.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 10. FinOps, chaos, performance и data reliability

### FinOps

- FinOps Foundation framework: inform, optimize, operate.
- Unit economics: cost per request, customer, namespace, job, feature.
- OpenCost/Kubecost, Infracost, cloud billing export.
- Rightsizing: requests/limits, VPA, HPA, Cluster Autoscaler, Karpenter в AWS/EKS-экосистеме.
- Spot/preemptible, reserved/committed use, storage lifecycle.
- Observability cost: sampling, retention, cardinality, tiering.

### Chaos и resilience

- Game days, failure injection, blast radius, steady state hypothesis.
- LitmusChaos, Chaos Mesh, Gremlin/Steadybit.
- Проверка fallback, retry storm, dependency timeout, queue backpressure.

### Performance

- k6, Gatling, Locust.
- Profiling: perf, flamegraph, Parca/Pyroscope.
- Load model: RPS, concurrency, p95/p99, saturation, queueing.

### Data reliability

- Airflow/Dagster, Kafka/Flink, data freshness, completeness, duplication, schema drift.
- Backfills, idempotency, exactly-once как цель/иллюзия в разных системах.

### Практика

- Сделай дашборд стоимости по namespace/service.
- Проведи game day: выключи dependency, проверь алерты/runbook/recovery.
- Напиши k6-тест и найди saturation point.
- Добавь data freshness SLO для pipeline.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 11. Итоговый практический проект

Собери маленькую платформу, похожую на production. Не обязательно дорогую: локальный kind/k3d и одно облачное окружение уже достаточно.

### Минимальный состав

- Git repo с OpenTofu/Terraform.
- Kubernetes cluster: kind/k3d или managed.
- Argo CD/Flux для GitOps.
- Gateway API + cert-manager.
- External Secrets / SOPS.
- Observability: OTel Collector + metrics/logs/traces backend + Grafana.
- Приложение с OTel instrumentation.
- SLO definitions + burn-rate alerts.
- Runbooks и postmortem template.
- Security: SBOM, image scan, signature/policy.
- Оценка стоимости: хотя бы на уровне namespace/service.
- Chaos experiment.

### AI-блок

- RAG по runbooks.
- Read-only tool/MCP для получения статуса кластера и последних событий.
- Автогенерация черновика incident summary с обязательной ручной проверкой.

### Результат

В конце должен быть не отчет "кластер поднялся", а демонстрация полного цикла: сломал сервис, получил сигнал, понял влияние на пользователей, смягчил проблему, восстановил сервис, написал postmortem, закрыл action items.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## 12. Подготовка к собеседованиям

### Что тренировать

- System design для надежности: SLO, scaling, caching, queues, consistency, degradation, failover.
- Incident scenarios: структурированный triage, влияние на пользователей, blast radius, mitigation, коммуникации, RCA после восстановления.
- Linux/Kubernetes deep dive: OOMKilled, DNS, networking, scheduling, storage, probes.
- Observability design: какие SLI, какие labels, где sampling, как не утонуть в cardinality.
- IaC/GitOps: state, drift, rollback, secrets, promotion environments.
- Security: RBAC, supply chain, image signing, secrets leakage.
- FinOps: почему счет вырос, как найти владельца, как оптимизировать без поломки reliability.
- AI-aware SRE: где AI помогает, где опасен, какие ограничения нужны.
- Coding: Python/Go для логов, API, CLI, automation; алгоритмы базово, больше упор на практические data-processing задачи.

### Формат ответов

- Всегда начинай с влияния на пользователей и масштаба проблемы.
- Разделяй mitigation и root cause.
- Проговаривай trade-offs.
- Уточняй assumptions.
- Показывай, как проверишь гипотезу.
- Не обещай "поставлю Kubernetes и всё будет HA".

### Практика

- 10 раз проговори инцидент "API 500" по таймеру на 15 минут.
- Спроектируй SLO для checkout/payment/search/feed.
- Разбери публичный postmortem GitHub/AWS/GitLab.
- Напиши скрипт: распарсить большой access log, найти top endpoints по p99 и error rate.

Чек-лист: [SRE Theory check-list](theory-checklist/)

---

## Что в 2026 считать legacy / аккуратно использовать

| Было как выбор по умолчанию | В 2026 лучше |
| --- | --- |
| Ingress для всех новых сценариев | Gateway API, если controller поддерживает нужные features |
| Jaeger/Zipkin как отдельная стратегия | OpenTelemetry instrumentation + backend по задаче: Tempo/Jaeger/Honeycomb/etc. |
| ELK как безусловный выбор по умолчанию | Loki/VictoriaLogs/ClickHouse/OpenSearch/Elastic по требованиям и бюджету |
| Terraform как единственный ответ | Terraform, OpenTofu, Pulumi, Crossplane по контексту |
| Push-deploy из CI в Kubernetes | GitOps через Argo CD/Flux |
| iptables/netstat в новых инструкциях | nft/ss/eBPF-инструменты |
| "CPU 80%" как page alert | SLO/burn-rate alerting по влиянию на пользователей |
| Security после релиза | Supply chain, policy, secrets, RBAC в delivery path |
| AI как автономный on-call | AI как copilot/read-only investigator; автономия только с ограничителями |
| "Собрать всю телеметрию навсегда" | Sampling, retention tiers, cardinality budgets, владение стоимостью |

---

## Источники для актуализации

- Google SRE Books: https://sre.google/books/
- Google SRE Workbook: https://sre.google/workbook/
- OpenTelemetry docs/spec: https://opentelemetry.io/docs/ и https://opentelemetry.io/docs/specs/otel/
- CNCF OpenTelemetry project page: https://www.cncf.io/projects/opentelemetry/ — статус Graduated с 2026-05-11
- Kubernetes Gateway API v1.0 GA: https://kubernetes.io/blog/2023/10/31/gateway-api-ga/
- Kubernetes Gateway API v1.5: https://kubernetes.io/blog/2026/04/21/gateway-api-v1-5/
- Gateway API v1.1 service mesh support: https://kubernetes.io/blog/2024/05/09/gateway-api-v1-1/
- OpenTofu CNCF page: https://www.cncf.io/projects/opentofu/
- Linux Foundation OpenTofu GA announcement: https://www.linuxfoundation.org/press/opentofu-announces-general-availability
- DORA 2025 / Google Cloud: https://cloud.google.com/blog/products/ai-machine-learning/announcing-the-2025-dora-report
- OpenSLO specification: https://github.com/OpenSLO/OpenSLO
