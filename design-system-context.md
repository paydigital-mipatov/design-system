# Design System — PayDigital

_Последнее обновление: апрель 2026_

Полный технический референс дизайн-системы. Для быстрого старта — см. README.md.

---

## 1. Продукт и бренд

PayDigital — зонтичный бренд с двумя продуктами:

- **Market** — витрина цифровых товаров (подписки, аккаунты, цифровые товары). Next.js, PWA. Клиенты с кастомными темами, сейчас 20+, целевое 50+.
- **Travel** — eSIM + пополняемая карта для оплаты за рубежом. Next.js web + Flutter mobile.

Модель бренда: **1 клиент = 1 общая тема на все продукты**. Travel может позже визуально отстроиться, но пока — единая бренд-рамка.

Компоненты единые, пока не решено иначе.

## 2. Стек

- Фронт: Next.js
- CMS: Strapi (уже используется)
- Мобайл: Flutter (только Travel)
- Дизайн: Figma + Tokens Studio Pro
- Токен пайплайн: Tokens Studio → Git → JSON → Style Dictionary → CSS Variables (web) + Dart ThemeExtension (Flutter)
- Тема в рантайме: CSS Variables в `<head>` при загрузке страницы

## 3. Тип решения

Строим **одну платформенную дизайн-систему** с:
- общим system foundation
- brand token layer (клиентская кастомизация)
- semantic token layer (мост brand ↔ компоненты)
- общими core-компонентами
- preset-вариативностью marketing-блоков
- light/dark modes
- отдельным validation file на каждого клиента

Это **white-label платформа с контролируемой кастомизацией**, а не page builder и не конструктор интерфейсов.

## 4. Архитектурные принципы

### 4.1. Разделение слоёв обязательно
Система различает: system → brand → semantic → components → page templates → client validation.

### 4.2. Semantic layer обязателен
Компоненты сидят **только** на `sem.*` токенах. Привязка к `brand.*` или raw hex запрещена.

### 4.3. Core modes = only light/dark
В ядре Figma только `light` и `dark`. Клиенты — не modes. Клиент — это операционная сущность (tenant), а не архитектурное состояние системы. Каждый новый токен пришлось бы руками заполнять для всех client modes — это не масштабируется.

### 4.4. Клиенты валидируются отдельно
Каждый клиент живёт в отдельном validation file, а не в core modes.

### 4.5. Marketing variability = presets, not freeform
Hero и marketing sections — preset variants и slots, не произвольный конструктор.

### 4.6. No detach
Компоненты и экраны не detach. Валидация и клиентские файлы работают на instances, section instances, templates, variables/mappings.

### 4.7. Figma ≠ единственный source of truth
Figma — место проектирования и проверки. Система переносима в код через JSON token schema.

## 5. Кастомизация: что можно и что нельзя

### Разрешено кастомизировать
- Логотип
- Primary / secondary / accent colors
- Шрифтовые семьи (base, heading)
- Font weight mapping
- Dark theme (on/off + ограниченные overrides)
- Иконки, иллюстрации, баннеры
- Контент
- Выбор preset marketing / hero block
- Включение / выключение разрешённых блоков

### Нельзя кастомизировать
- Core layout architecture
- Структуру core-компонентов
- Произвольную сборку страниц (page builder)
- Ad hoc клиентские overrides в ядро DS
- Клиентов как modes в core system
- Привязку компонентов напрямую к raw brand hex
- Detach экранов как стандартный workflow
- Уникальные наборы компонентов под конкретного клиента

### Scope v1
Кастомизируемо: color, typography family, typography weight, dark theme.
Системное (клиент НЕ трогает в v1): spacing, radius.

## 6. Архитектура токенов — 4 коллекции (Figma Variable Collections)

### SYS / Primitives (префикс `sys.*`)
Системная палитра, сырые значения. Клиент не трогает.

| Группа | Примеры |
|--------|---------|
| color / neutral | `sys.color.neutral.0` ... `sys.color.neutral.900` |
| color / status | `sys.color.status.success.500`, `.warning.500`, `.danger.500` |
| space | `sys.space.4`, `sys.space.8` ... `sys.space.64` |
| radius | `sys.radius.4` ... `sys.radius.16` |
| font | `sys.font.family.inter`, `sys.font.weight.medium` |
| type / size | `sys.type.size.h1.mobile`, `sys.type.size.h1.desktop` |
| layout | `sys.layout.padding.mobile`, `sys.layout.padding.desktop` |

### BRAND / Mapping (префикс `brand.*`)
Брендовые значения клиента. **2 Modes: light / dark.**

| Группа | Токены |
|--------|--------|
| color / core | `brand.color.primary`, `brand.color.secondary`, `brand.color.accent` |
| font / family | `brand.font.family.base`, `brand.font.family.heading` |
| font / weight | `brand.font.weight.regular`, `.medium`, `.bold` |

Клиентский цвет вне палитры → прямое hex в `brand.*`, палитру `sys.*` не трогаем.

### SEM / Tokens (префикс `sem.*`)
Семантический слой, мост Brand ↔ компоненты. **2 Modes: light / dark.** Компоненты сидят только здесь.

| Группа | Токены | Маппинг |
|--------|--------|---------|
| bg / page | `sem.bg.page.default` | light: `sys.color.neutral.0` / dark: `sys.color.neutral.900` |
| bg / surface | `sem.bg.surface.default` | light: `sys.color.neutral.50` / dark: `sys.color.neutral.800` |
| text | `sem.text.default`, `sem.text.brand` | `.brand` → `brand.color.primary` |
| border | `sem.border.default` | |
| action / primary | `sem.action.primary.bg`, `.text` | `.bg` → `brand.color.primary` |
| action / secondary | `sem.action.secondary.bg`, `.text` | |
| action / tertiary | `sem.action.tertiary.bg`, `.text` | |
| field | `sem.field.border`, `sem.field.bg` | |
| link | `sem.link.default` | → `brand.color.primary` |
| icon | `sem.icon.default`, `sem.icon.brand` | |
| status | `sem.status.success`, `.warning`, `.danger` | |
| overlay | `sem.overlay.default` | |
| focus | `sem.focus.ring` | |

### CMP / Optional (префикс `cmp.*`)
Алиасит Semantic. Слот заложен, **в v1 не раздувать**.

- `cmp.button.primary.bg` → `sem.action.primary.bg`
- `cmp.card.default.bg` → `sem.bg.surface.default`

## 7. Naming convention

Формат: `[layer].[group].[role].[variant].[state]`

| Правило | Пример |
|---------|--------|
| Префикс слоя обязателен | `sys`, `brand`, `sem`, `cmp` |
| Только latin lowercase | `sem.action.primary.bg` |
| Точка как разделитель | `brand.color.primary` |
| CSS: точка → дефис | `sem-bg-page-default` |
| Dart: точка → camelCase | `semBgPageDefault` |

Плохие имена: `BlueMain`, `FinalPrimary`, `ButtonBlue`, `ClientRed2`.

## 8. Figma: файловая структура

| Файл | Содержимое |
|------|-----------|
| **PD DS / Foundations** | Коллекции `SYS / Primitives` и `SEM / Tokens`, light/dark modes, typography rules, spacing/layout primitives |
| **PD DS / Core Components** | Buttons, inputs, selects, tabs, chips, badges, modals, drawers, toasts, nav, account, storefront atoms. Все на `sem.*` |
| **PD DS / Marketing Blocks** | Hero presets, promo banners, feature grids, CTA, FAQ, image/text sections. Только preset-based вариативность |
| **PD DS / Page Templates** | Home, Listing, Detail, Checkout, Account. Собраны из section instances, не нарисованы вручную |
| **PD Client / [BrandName] / Validation** | Один файл на клиента — brand mapping, assets, validation screens, edge cases |

### Структура страниц Core Library

```
FOUNDATION
  Tokens          — визуализация всех 4 коллекций
  Typography      — все суббренды рядом
  Icons           — общий сет PayDigital, slot под клиента

WEB COMPONENTS (Next.js / PWA)
  Components — Web    — мастера, всегда paydigital·light
  Patterns — Web      — карточка товара, хедер, форма оплаты

FLUTTER COMPONENTS (только Travel)
  Components — Flutter — отдельные мастера, 44pt touch targets
  Patterns — Flutter   — экраны Travel mobile

BRAND SHOWCASE (просмотр тем, Modes переключаются здесь)
  PayDigital      — зонтичный бренд, эталон
  Travel          — web + Flutter рядом, light + dark
  Market          — клиентские витрины, light + dark
```

## 9. Client validation file

### Зачем
- Не тащить клиентов в core modes
- Проверять тему клиента на живой системе до кода
- Хранить approved assets и статус
- Показывать тему продукту / аккаунту / разработке
- Staging/QA пространство

### Страницы файла

| Страница | Содержимое |
|----------|-----------|
| `00 Cover` | Client name, product scope, theme version, owner, date, status |
| `01 Theme Mapping` | Логотип, primary/secondary/accent, base font, heading font, weight mapping, dark enabled, notes |
| `02 Assets` | Approved logo, icon set, banners, illustrations, hero media, dark assets |
| `03 Light / Core Screens` | Homepage, listing, detail, checkout, account |
| `04 Dark / Core Screens` | Те же экраны в dark |
| `05 Edge Cases` | Long titles, empty states, disabled buttons, contrast problems, weird logo on backgrounds |
| `06 Review Notes` | Issues list, open questions, review rounds, approval status |

### Как маппить тему
В client file локально создаётся `BRAND / Mapping`:
```
brand.color.primary   = #005BFF
brand.color.secondary = #7C3AED
brand.color.accent    = #F59E0B
brand.font.family.base    = Inter
brand.font.family.heading = Onest
```

Semantic роли маппятся на brand values:
```
sem.action.primary.bg → brand.color.primary
sem.text.brand        → brand.color.primary
sem.link.default      → brand.color.primary
```

Работа без detach: только content overrides, asset slots, allowed variant switches, brand mapping variables.

## 10. Иерархия компонентов

### Уровень 1: Base components
Button, Input, Select, Tabs, Badge, Modal, Card, IconButton, Drawer, Toast, Chip.

### Уровень 2: Product / UI blocks
Product card, Tariff card, Balance card, Transaction row, Search bar, Payment method selector, Checkout summary.

### Уровень 3: Marketing / page sections
Hero A, Hero B, Promo strip, Feature grid, FAQ block, CTA section.

### Page template
Screen-level template, собранный из section instances:
```
Header instance → Hero instance → Feature grid instance →
Product grid instance → FAQ instance → Footer instance
```

## 11. Dark theme strategy

### Подход: Semi-automated dark theme
Системная база dark mode + ограниченный набор client overrides.

| Тип | Чья ответственность |
|-----|-------------------|
| Page bg, surface bg, default text, borders | Всегда системные (`sys.*`) |
| Primary in dark, accent in dark, selected highlights, subtle brand surfaces | Клиентские overrides через `brand.*` |

Полностью ручная dark-вселенная на каждого клиента не нужна — дорого, плохо масштабируется.

### Открыто
- Dark theme — обязательная для всех, опциональная или premium?
- Производная от light полуавтоматически или полностью ручная?

## 12. Responsive strategy (v1, минимальная)

### Что закладываем
- **Typography**: mobile/desktop distinctions на ключевые размеры (`sys.type.size.h1.mobile/desktop`)
- **Layout**: container max width, mobile/desktop paddings/gutters (`sys.layout.padding.mobile/desktop`)
- **Spacing**: единая system scale (`sys.space.*`)

### Что НЕ делаем в v1
- Responsive modes на всё подряд
- Клиентскую кастомизацию spacing

## 13. Целевая модель в коде

### Архитектура
- Один Next.js кодбейс, multi-tenant storefront (рекомендация, не подтверждено разработкой)
- Общая component library
- Theme config на клиента (JSON)
- Контент и блоки через Strapi
- CSS Variables для runtime theming

### Pipeline
1. Токены спроектированы в Figma
2. Экспортируются в JSON schema (Tokens Studio → Git)
3. JSON маппится в CSS Variables через Style Dictionary
4. Компоненты используют semantic tokens через CSS variables
5. Tenant config задаёт brand values
6. Strapi отдаёт content / layout / media

### Определение клиента в рантайме
Не решено: домен / subdomain / route / tenant id. Уточнить с разработкой.

### Загрузка темы
Не решено: на сервере до рендера / во время SSR / после загрузки. Критично для UX (flash of wrong theme).

## 14. Strapi и токены

- Strapi уже используется для контента
- Токены задаём мы за клиента, не клиент сам
- Хранение темы в Strapi **отложено** до стабилизации token model

### Последовательность
1. Стабилизировать token model в Figma + Git (JSON)
2. Проверить на 2–3 реальных клиентах
3. Согласовать token-to-code pipeline (Style Dictionary)
4. Решить, что из theme config уходит в Strapi

### Strapi на старте управляет
Контент, порядок и состав блоков, feature flags, media assets, preset choice.

### Strapi позже может хранить
Primary, secondary, accent, font family, dark enabled, hero preset, logo ref, assets refs. Рекомендуемый формат: Collection Type `ClientTheme` с flat JSON полем токенов.

## 15. Принятые решения

1. Одна платформенная дизайн-система, не две
2. 1 бренд клиента = 1 тема на все продукты
3. Controlled white-label, не page builder
4. Слои: system → brand → semantic → components
5. Semantic layer обязателен
6. В ядре Figma modes только light/dark
7. Клиенты не хранятся как modes в core DS
8. Каждый клиент — отдельный validation file
9. Компоненты и экраны не detach
10. Marketing variability = preset variants
11. v1 scope: color, typography family/weight, dark theme
12. Radius/spacing — system-owned, без client customization в v1
13. Responsive: typography mobile/desktop, layout paddings/gutters, spacing scale
14. Дизайн — owner системы, но тема не должна зависеть только от дизайнеров
15. Успех v1: 80% экранов на общих компонентах, синк токенов без ручного ада

## 16. Открытые вопросы

### По продукту / бизнесу
- [ ] Нужен ли master brand поверх клиента?
- [ ] Может ли Travel визуально отделиться от Market?
- [ ] Dark theme — обязательная для всех или premium?
- [ ] Какие hero/layout presets нужны в v1?
- [ ] Планируются ли RTL-рынки в горизонте 12–18 месяцев?
- [ ] Готовность запрещать sales произвольную кастомизацию core UI?

### По разработке
- [ ] Один кодбейс multi-tenant или несколько фронтов?
- [ ] Как определяется клиент (домен / subdomain / route / tenant id)?
- [ ] Когда подгружается тема (SSR / runtime)?
- [ ] Theme config как JSON / tenant config?
- [ ] Theming через CSS variables — готовы?
- [ ] Формат token sync (JSON / Style Dictionary / custom)?
- [ ] Где хранить theme config (код / JSON / Strapi / tenant registry)?

### По дизайн-системе
- [ ] Визуальные отличия Travel vs Market (какие токены разные)
- [ ] Кастомизация тем клиентов во Flutter — насколько нужна
- [ ] Компоненты Travel для Flutter — отдельная библиотека потом?
- [ ] Style Dictionary config — CSS + Dart выходы
- [ ] Механизм применения CSS Variables в Next.js

## 17. План реализации

| Этап | Что делаем |
|------|-----------|
| 1 | Зафиксировать правила: что токен, что можно/нельзя, workflow |
| 2 | Собрать PD DS / Foundations: SYS + SEM, light/dark, typography, spacing |
| 3 | Собрать PD DS / Core Components: минимальный обязательный набор |
| 4 | Собрать PD DS / Marketing Blocks: только критичные presets |
| 5 | Собрать PD DS / Page Templates: 5 key screens |
| 6 | Сделать 2–3 demo brands (neutral, bright, stress-test) |
| 7 | Сделать первый Client / [Brand] / Validation на реальном клиенте |
| 8 | Прогнать ещё 2–3 реальных клиента, допилить архитектуру |
| 9 | Согласовать token-to-code pipeline с разработкой |
