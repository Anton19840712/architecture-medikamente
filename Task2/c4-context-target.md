# C4 Context - целевое состояние MVP

## Ключевая идея

Целевая архитектура разделяет клиентский, операционный, клинический, финансовый и аналитический контуры. Защита конфиденциальных данных выносится в сквозные платформенные блоки, а не решается локально в каждом сервисе.

## Новые блоки, обеспечивающие Privacy by Design

1. `API Gateway + WAF` - единая точка входа, ограничение контрактов, rate limiting, авторизация.
2. `Identity & Access Management` - OIDC, MFA, RBAC/ABAC.
3. `Consent & Privacy Service` - хранение согласий, обработка запросов на удаление, политики минимизации.
4. `Metadata / Tag Catalog` - классификация, lineage, теги конфиденциальности.
5. `Audit, SIEM, DLP, UEBA` - аудит доступа, корреляция событий, алертинг.
6. `KMS / Vault` - управление ключами, секретами, сертификатами.
7. `Analytical Privacy Layer` - зона обезличивания и подготовки данных для BI/ML/AI.

## Диаграмма контекста

```mermaid
flowchart TB
    patient["Пациент"] --> portal["Клиентский портал / мобильное приложение"]
    reception["Ресепшен"] --> staff["Портал сотрудника"]
    doctor["Медицинский специалист"] --> staff
    cashier["Кассир / бухгалтер"] --> staff
    lab["Лаборатория"] <--> gateway["API Gateway + WAF"]

    portal --> gateway
    staff --> gateway

    gateway --> crm["CRM / Scheduling Service"]
    gateway --> emr["EMR / Medical Record Service"]
    gateway --> pay["Payment Gateway / Billing"]
    gateway --> notify["Notification Service"]

    crm --> privacy["Consent & Privacy Service"]
    emr --> privacy
    pay --> privacy

    crm --> iam["IAM / OIDC / RBAC-ABAC"]
    emr --> iam
    pay --> iam

    crm --> opdb["Operational Data Store"]
    emr --> clinical["Clinical Data Store"]
    pay --> fin["Finance Data Store"]

    opdb --> atlas["Metadata Catalog / Tagging / Lineage"]
    clinical --> atlas
    fin --> atlas

    opdb --> etl["Secure Data Pipeline"]
    clinical --> etl
    fin --> etl
    etl --> privacyLayer["Analytical Privacy Layer"]
    privacyLayer --> dwh["DWH / Data Lakehouse"]
    dwh --> bi["BI / ML / AI"]

    crm --> audit["Audit Log / SIEM / DLP / UEBA"]
    emr --> audit
    pay --> audit
    gateway --> audit

    kms["KMS / Vault"] -.ключи и секреты.-> gateway
    kms -.-> crm
    kms -.-> emr
    kms -.-> pay
    kms -.-> etl
```

## Почему это соответствует заданию

- принципы `Privacy by Design` встроены в архитектуру, а не добавляются как постфактум;
- аналитический слой отделён от операционного и получает данные через контур обезличивания;
- новые интеграции проходят через единые контракты, теги и политики доступа;
- медицинские и финансовые данные разведены по доменам и моделям доступа.
