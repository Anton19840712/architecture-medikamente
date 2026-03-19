# Task6 — Классификация данных

## Содержимое директории

- `c2-data-classification-engine.md` — текстовое описание: компоненты движка, слои хранилища, метрики эффективности, масштабируемость.
- `c2-data-classification-engine.drawio` — **C4 Container Diagram (Level 2)** движка классификации данных в нотации C4 с использованием shape-библиотек draw.io.

## О диаграмме C4 Container (C2)

Диаграмма выполнена в нотации C4 Level 2 (Container diagram) и открывается в [app.diagrams.net](https://app.diagrams.net):

- `[Container]` (синий) — внутренние контейнеры движка классификации.
- `[External Software System]` (серый) — источники данных (CRM, EMR, 1С, файлы) и получатели (DWH, SIEM, Catalog).
- Граница системы — `Data Classification Engine [System Boundary]`.
- Зелёные стрелки — данные прошли проверку (allow / masked).
- Красные стрелки — данные отправлены в карантин.

## Слои хранилища (предложенные в движке)

1. **Raw Restricted Zone** — сырые данные, краткий TTL, обязательное тегирование.
2. **Privacy Processing Zone** — классификация, mask/tokenize/pseudonymize, quality gates.
3. **Curated Secure Zone** — обработанные наборы для BI с контролируемым доступом.
4. **Analytics Sandbox** — изолированная среда для ML/AI с аудитом запросов.
