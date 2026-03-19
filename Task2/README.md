# Task2 — Проектирование решения

## Содержимое директории

- `c4-context-target.md` — текстовое описание новых блоков архитектуры (Privacy by Design), Mermaid-диаграмма.
- `c4-context-target.drawio` — **C4 Context Diagram (Level 1)** целевого состояния MVP в нотации C4 с использованием стандартных shape-библиотек draw.io (Person, Software System, System Boundary).

## О диаграмме C4 Context

Диаграмма выполнена в стандартной C4-нотации и открывается в [app.diagrams.net](https://app.diagrams.net):

- `[Person]` — пользователи системы (Пациент, Врач, Ресепшен, Кассир, Администратор).
- `[Software System]` (синий) — внутренние системы «Медикаменте».
- `[Software System]` (серый) — внешние системы (Лаборатория, 1С legacy).
- Пунктирные стрелки — сквозные платформенные сервисы (IAM, KMS, Consent, Audit).
- Блоки с пометкой `(Privacy by Design)` — новые компоненты, реализующие принципы конфиденциальности.
