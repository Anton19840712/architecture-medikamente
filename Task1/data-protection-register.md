# Реестр данных для защиты и механизм тегирования

## 1. Реестр данных

| Данные | Категория | Где встречаются сейчас | Рекомендуемая защита |
|---|---|---|---|
| ФИО, дата рождения, телефон, e-mail | PII | Excel, договоры, журналы | Шифрование, маскирование в UI и логах |
| Адрес, место работы/учёбы | SENSITIVE_PII | формы, сканы договоров | Шифрование, маскирование, ограничение экспорта |
| Диагнозы, анализы, назначения, история лечения | PHI | JPG/PDF/Excel | Шифрование, псевдонимизация для аналитики, строгий ABAC |
| Договоры и согласия | CONSENT / LEGAL | файловый сервер | Шифрование, WORM-аудит, контроль сроков хранения |
| Чеки, суммы платежей, возвраты | FINANCE | Excel, 1С, ККМ | Шифрование, токенизация идентификаторов, аудит |
| Кадровые и зарплатные данные | HR_CONFIDENTIAL | 1С | Шифрование, RBAC, ограниченный экспорт |
| Логи доступа и действий | AUDIT | отсутствуют централизованно | Неподменяемое хранение, SIEM, ограниченный доступ |
| Выгрузки для BI/ML | ANALYTICS_RESTRICTED | ad-hoc Excel/Jupyter | Обезличивание, mask/tokenize, sandboxed access |

## 2. Правила выбора способа защиты

| Метод | Когда использовать |
|---|---|
| Шифрование | Первичное хранение, резервные копии, межсервисные соединения, архив |
| Обфускация | Демонстрационные данные, небоевые скриншоты и примеры |
| Обезличивание / псевдонимизация | BI, ML, тестовые среды, обмен с командами разработки |
| Маскирование | UI, логи, отчёты, служебные интерфейсы, поддержка |
| Токенизация | Идентификаторы оплат, клиентские ссылки во внешних интеграциях |

## 3. Модель тегирования данных

### Базовые теги конфиденциальности

- `PUBLIC`
- `INTERNAL`
- `PII`
- `SENSITIVE_PII`
- `PHI`
- `FINANCE`
- `CONSENT`
- `HR_CONFIDENTIAL`
- `AUDIT`
- `ANALYTICS_RESTRICTED`

### Дополнительные теги жизненного цикла

- `RAW`
- `CURATED`
- `GOLD`
- `RETENTION_1Y`
- `RETENTION_5Y`
- `DELETE_ON_REQUEST`
- `MASK_IN_UI`
- `NO_EXTERNAL_TRANSFER`

### Теги происхождения

- `SOURCE_PORTAL`
- `SOURCE_RECEPTION`
- `SOURCE_1C`
- `SOURCE_LAB`
- `SOURCE_MANUAL_UPLOAD`

## 4. Инструменты тегирования и управления метаданными

| Задача | Инструмент |
|---|---|
| Каталог метаданных и lineage | Apache Atlas |
| Политики доступа | Keycloak / AuthZ service + policy engine |
| DLP и контроль утечек | SearchInform / InfoWatch / Microsoft Purview DLP |
| Сканирование PII/PHI в хранилищах | Apache NiFi + data classifiers / OpenMetadata-compatible scanners |
| Секреты и ключи | HashiCorp Vault / KMS |
| SIEM / корреляция событий | ELK + Wazuh / Splunk / QRadar |

## 5. Меры по этапам потока

| Этап потока | Обязательные меры |
|---|---|
| Ввод данных | consent capture, validation, anti-bot, MFA для сотрудников |
| Передача | TLS 1.2+, mTLS для сервисов, schema validation |
| Хранение | encryption at rest, segmentation, backup encryption |
| Использование | RBAC/ABAC, masking, least privilege |
| Аналитика | de-identification, sandbox access, lineage |
| Удаление | retention jobs, legal hold rules, deletion workflow with audit |
