# DS-RM.md — Delta / Updates after `README_PayDigital_DS_Architecture.md`

> Этот документ содержит только **обновления и изменения**, появившиеся **после** генерации предыдущего большого README `PayDigital_DS_Architecture`.
>
> Читать **вместе** с предыдущим README, а не вместо него.

---

# 1. Новые вводные, которые меняют архитектуру

## 1.1. Подтверждена брендовая иерархия

Теперь известно, что брендовая модель такая:

- **Umbrella / master brand:** `PayDigital`
- **Sub-brands / product brands:** `Travel` и `Market`
- **White-label client layer:** клиенты поверх платформы

### Что это значит
Это уже не просто “один общий бренд”, но и не две отдельные дизайн-системы.

Правильная интерпретация:
- нужна **одна экосистемная дизайн-система**
- с **общим ядром**
- с **master brand layer**
- с **product flavour / sub-brand layer**
- с **client white-label layer**

---

## 1.2. Подтверждено, что DS и core components должны быть едиными

Зафиксировано:
- Travel и Market **не должны превращаться в две отдельные дизайн-системы**
- core components должны быть едиными
- foundations должны быть едиными
- token architecture должна быть единой

### Что может различаться
Travel и Market могут различаться:
- акцентами
- marketing / hero flavour
- частью визуальных cue’ов
- некоторыми product-level accent roles

### Что не должно расходиться
Не должны расходиться:
- foundations
- semantic structure
- core components
- базовая UI-архитектура платформы

---

## 1.3. Подтверждено, что мобильное приложение будет на Flutter

Это новая критически важная вводная.

### Что это меняет
Архитектуру теперь нельзя делать purely web-centric.

Нужно закладывать:
- общую token architecture
- общую semantic model
- общую brand hierarchy
- но разные implementation layers:
  - Web / Next.js
  - Flutter

### Что не надо делать
Не нужно:
- пытаться сделать одну и ту же компонентную библиотеку для Web и Flutter
- строить токены вокруг CSS/HTML/React терминологии
- считать web-компоненты “истиной” для mobile implementation

---

# 2. Что в предыдущем README нужно считать уточнённым

## 2.1. Уточнение по модели “1 клиент = 1 общая тема на все продукты”

Предыдущая формулировка больше не является достаточной.

### Было раньше
- “1 клиент = 1 общая тема на все продукты”

### Теперь точнее
Нужно понимать так:
- есть **PayDigital umbrella brand**
- есть **Travel / Market как sub-brand flavours**
- есть **client white-label layer**
- клиентская тема живёт **внутри продуктового контекста**, а не в вакууме

### Следствие
Формулировку про “одну тему” теперь надо понимать как:
- общая экосистема
- общая DS
- единый каркас
- но с product flavour layer

---

## 2.2. Уточнение по brand architecture

Предыдущая модель была более плоской:
- system
- brand/client
- semantic

Теперь этого недостаточно.

### Новая корректная модель
- `system`
- `umbrella brand`
- `product sub-brand`
- `client white-label`
- `semantic`
- `platform implementation`

---

# 3. Новая рекомендуемая архитектурная модель

## 3.1. Новая целевая модель

Нужна:

## **One ecosystem design system**
с такими слоями:

1. **Platform System Layer**
2. **Umbrella Brand Layer — PayDigital**
3. **Sub-brand / Product Flavour Layer — Travel / Market**
4. **Client White-label Layer**
5. **Semantic UI Layer**
6. **Platform Implementation Layers — Web / Flutter**

---

## 3.2. Расшифровка слоёв

### Layer A — Platform System
Общее для всей платформы:
- foundations
- neutral scale
- status scale
- spacing
- radius
- type scale
- layout primitives
- interaction states
- accessibility basics

### Layer B — Umbrella Brand (PayDigital)
Общая брендовая логика экосистемы:
- master brand cues
- общая визуальная направленность
- master typographic direction
- umbrella consistency

### Layer C — Product Flavour (Travel / Market)
Sub-brand слой:
- product accent
- product emphasis
- hero / promo flavour
- product-specific visual cues
- flavour differences в marketing layer

### Layer D — Client White-label
Слой конкретного клиента:
- client logo
- client colors
- client fonts
- client banners / illustrations / icons
- client-specific allowed overrides

### Layer E — Semantic UI Layer
Главный смысловой слой:
- bg
- text
- border
- action
- field
- link
- status
- focus / overlay

### Layer F — Platform Implementations
Раздельные реализации:
- Web / Next.js
- Flutter

---

# 4. Обновлённая модель токенов

## 4.1. Новая рекомендуемая иерархия токенов

Теперь рекомендуется такая иерархия:

1. `sys.*`
2. `brand.master.*`
3. `brand.product.*`
4. `brand.client.*`
5. `sem.*`

Опционально позже:
6. `cmp.web.*`
7. `cmp.flutter.*`

---

## 4.2. Что означает каждый слой

### `sys.*`
Платформенные primitives:
- platform-agnostic
- не зависят от бренда
- общие для Web и Flutter

### `brand.master.*`
Umbrella brand PayDigital:
- общая брендовая основа экосистемы

### `brand.product.*`
Sub-brand flavour:
- Travel / Market
- не отдельные DS
- не отдельные foundations
- только продуктовая flavour-надстройка

### `brand.client.*`
White-label клиент:
- клиентские цвета
- клиентские шрифты
- клиентские allowed overrides

### `sem.*`
Финальные UI-роли:
- именно на них должны ссылаться компоненты

---

## 4.3. Новый приоритет маппинга

Базовая рекомендуемая логика:

`sys → brand.master → brand.product → brand.client → sem`

### Operational смысл
- `sys` даёт платформенную базу
- `brand.master` даёт umbrella brand cohesion
- `brand.product` даёт Travel/Market flavour
- `brand.client` даёт white-label override
- `sem` превращает всё это в роли интерфейса

---

# 5. Обновлённая модель collections в Figma

## 5.1. Новый список основных collections

### 1. `SYS / Primitives`
- без modes
- platform-agnostic base

### 2. `BRAND / Master`
- umbrella brand PayDigital
- modes: `light`, `dark`

### 3. `BRAND / Product`
- product flavour layer
- modes: `travel`, `market`

### 4. `BRAND / Client`
- не живёт в core DS
- живёт локально в client validation file
- modes: `light`, `dark`

### 5. `SEM / Tokens`
- modes: `light`, `dark`
- главный рабочий semantic слой

---

## 5.2. Что допустимо как modes

### Допустимо:
- `light / dark`
- `travel / market` — как product axis

### Недопустимо:
- клиенты как modes
- клиентские темы в ядре
- web / flutter как global modes в core token system
- product modes в `SYS / Primitives`

---

# 6. Обновлённая ownership / source of truth модель

| Collection | Modes | Purpose | Source of truth | Editable by |
|---|---|---|---|---|
| `SYS / Primitives` | none | Платформенные primitives для всей экосистемы и обеих платформ | Core DS / Foundations | DS owner only |
| `BRAND / Master` | `light`, `dark` | Umbrella brand PayDigital | Core DS / Foundations | DS owner |
| `BRAND / Product` | `travel`, `market` | Product flavour layer for Travel / Market | Core DS / Foundations | DS owner |
| `BRAND / Client` | `light`, `dark` | White-label client layer | Client Validation File → later config/code source | Design during validation, later controlled config workflow |
| `SEM / Tokens` | `light`, `dark` | Semantic UI layer used by components | Core DS / Foundations | DS owner |
| `CMP / Web` *(later, optional)* | none | Web aliases if really needed | Web library | DS + frontend |
| `CMP / Flutter` *(later, optional)* | none | Flutter aliases if really needed | Flutter design/dev layer | DS + mobile/flutter |

---

# 7. Что меняется в client validation model

## 7.1. Раньше
Validation file мыслился как:
- client brand mapping
- validation screens
- light/dark

## 7.2. Теперь
Validation file должен учитывать ещё и **product context**.

### Новая логика
Клиентская тема валидируется:
- не просто “в вакууме”
- а в контексте **Travel** или **Market**
- либо в обоих, если клиент использует оба подбренда

---

## 7.3. Что добавить в validation file

Новые обязательные блоки:

### `Brand Hierarchy`
- umbrella brand reference
- selected product flavour
- client brand data

### `Theme Mapping`
- master-derived values
- product flavour values
- client overrides

### `Screens`
- в явном продуктово-контекстном режиме:
  - Travel screens
  - Market screens

---

# 8. Обновление по Flutter

## 8.1. Что теперь обязательно учитывать
Система должна быть **platform-agnostic на уровне токенов**, но **platform-specific на уровне компонентов**.

### Общие для Web и Flutter:
- `sys.*`
- `brand.master.*`
- `brand.product.*`
- `brand.client.*`
- `sem.*`

### Раздельные:
- web components
- flutter widgets
- web templates
- app templates / flows

---

## 8.2. Новый обязательный принцип naming

Tokens и semantic roles не должны быть web-centric.

### Нельзя:
- `web.button.primary`
- `css.link.default`
- `html.input.border`

### Нужно:
- `sem.action.primary.bg`
- `sem.field.border`
- `sem.text.link`
- `sem.bg.surface.elevated`

---

## 8.3. Новый архитектурный принцип платформ

### Shared design language
- общие tokens
- общие brand layers
- общие semantic roles

### Separate implementation libraries
- Web implementation
- Flutter implementation

---

# 9. Новые ограничения и анти-паттерны

## 9.1. Нельзя превращать Travel и Market в отдельные дизайн-системы
Они должны быть **sub-brand flavours**, а не 2 независимые системы.

## 9.2. Нельзя смешивать product flavour и client brand
Это разные вещи:
- product = внутренний платформенный слой
- client = внешний white-label слой

## 9.3. Нельзя делать web-centric token architecture
Flutter ломает такую предпосылку.

## 9.4. Нельзя пытаться сделать одну компонентную библиотеку для Web и Flutter
Должна быть:
- одна design language
- две implementation libraries

---

# 10. Какие секции предыдущего README нужно считать обновлёнными

Ниже — карта, что именно в предыдущем большом README нужно считать изменённым:

1. Brand architecture / модель бренда
2. Token architecture
3. Figma collections
4. Validation model
5. Code / implementation model
6. Open questions
7. Governance / ownership
8. Platform architecture

---

# 11. Новые / уточнённые open questions

## 11.1. По бренду
1. Какие элементы umbrella brand PayDigital реально должны быть видимыми в Travel и Market?
2. Что именно отличает Travel и Market визуально:
   - только accent layer?
   - hero/promo layer?
   - иллюстрации?
   - tone?
3. Какие части product flavour являются обязательными и не должны убиваться client override?

## 11.2. По Flutter
4. Нужен ли общий token export pipeline и для Web, и для Flutter?
5. В каком формате Flutter-команда хочет получать токены?
6. Будет ли Flutter-команда строить свою widget library на общей semantic model?

## 11.3. По implementation
7. Где живёт итоговый source of truth по client theme?
8. Как именно маппится client theme на product flavour + umbrella brand?
9. Можно ли подключать нового клиента без нового релиза?

---

# 12. Итог обновлений — короткая версия

## Было раньше
- white-label platform DS

## Теперь точнее
- **ecosystem design system**
- с:
  - **platform system layer**
  - **umbrella brand layer**
  - **sub-brand flavour layer**
  - **client white-label layer**
  - **shared semantic layer**
  - **separate Web and Flutter implementations**

---

# 13. Новый итоговый тезис

После всех обновлений правильный target state звучит так:

## `One ecosystem design system for PayDigital`
with:
- shared foundations
- shared semantic model
- umbrella brand consistency
- sub-brand flavours (Travel / Market)
- client white-label overlays
- separate implementation libraries for Web and Flutter

Это и есть новая корректная архитектурная рамка поверх предыдущего README.
