# CLAUDE.md — PayDigital Design System

Этот файл читается автоматически при каждом запуске Claude Code.

## Продукт

PayDigital — зонтичный бренд с двумя продуктами:
- **Market** — витрина цифровых товаров (Next.js, PWA), клиенты с кастомными темами (до 50+)
- **Travel** — eSIM + карта (Next.js web + Flutter mobile)

Компоненты единые, пока не решено иначе.

## Стек

- Фронт: Next.js
- CMS: Strapi
- Мобайл: Flutter (только Travel)
- Дизайн: Figma + Tokens Studio Pro
- Токен пайплайн: Tokens Studio → Git → JSON → Style Dictionary → CSS Variables (web) + Dart ThemeExtension (Flutter)
- Тема в рантайме: CSS Variables в `<head>` при загрузке страницы

## Архитектура токенов — 4 коллекции (Figma Variable Collections)

| Коллекция | Префикс | Описание |
|-----------|---------|----------|
| **SYS / Primitives** | `sys.*` | Системная палитра, клиент не трогает. `sys.color.neutral.100`, `sys.color.status.success.500`, `sys.space.16`, `sys.radius.8` |
| **BRAND / Mapping** | `brand.*` | Брендовые значения клиента. 2 Modes: light/dark. `brand.color.primary`, `brand.color.secondary`, `brand.color.accent`, `brand.font.family.base`, `brand.font.family.heading` |
| **SEM / Tokens** | `sem.*` | Семантический слой, мост Brand ↔ компоненты. 2 Modes: light/dark. `sem.bg.page.default`, `sem.action.primary.bg`, `sem.text.default` |
| **CMP / Optional** | `cmp.*` | Алиасит Semantic. Слот заложен, в v1 не раздувать. `cmp.button.primary.bg`, `cmp.card.default.bg` |

Компоненты сидят **только** на `sem.*` токенах. Привязка напрямую к `brand.*` или raw hex запрещена.

## Scope кастомизации клиента в v1

Токенизировано и кастомизируемо:
- color (primary, secondary, accent)
- typography family (base, heading)
- typography weight (regular, medium, bold)
- dark theme (on/off + ограниченные overrides)

Системное, клиент НЕ трогает в v1:
- spacing, radius — system-owned (`sys.*`)

## Naming convention

- Формат: `[layer].[group].[role].[variant].[state]`
- Префикс слоя обязателен: `sys`, `brand`, `sem`, `cmp`
- Только latin lowercase, точка как разделитель
- CSS: точка → дефис (`sem-bg-page-default`)
- Dart: точка → camelCase (`semBgPageDefault`)

## Структура репозитория

```
design-system/
├── tokens/                     # JSON-токены (SYS, Brand, SEM, CMP)
├── components/                 # Спецификации UI-компонентов
├── guidelines/                 # Правила использования
├── design-system-context.md    # Полный технический референс
└── CLAUDE.md                   # Этот файл
```

Полный контекст проекта (архитектура, принципы, Figma-структура, план) — в `design-system-context.md`.

## Правила работы

- Токены в формате JSON, совместимом с Tokens Studio
- Имена по convention: `[layer].[group].[role].[variant].[state]`
- Не трогать `sys.*` токены при добавлении клиентских тем
- Клиентский цвет вне палитры → прямое hex в `brand.*`, палитру (`sys.*`) не трогаем
- Компоненты ссылаются только на `sem.*`, никогда на `brand.*` или raw hex
- Light/dark — modes внутри коллекций BRAND и SEM, а не суффиксы в именах
- Marketing blocks — только preset variants, не freeform конструктор
- Компоненты и экраны не detach в Figma
- Spacing и radius — system-owned (`sys.*`), не кастомизируемы клиентом в v1

## Strapi и токены

Strapi уже используется для контента. Хранение темы клиента в Strapi — **отложено** до стабилизации token model.

Последовательность:
1. Стабилизировать token model в Figma + Git (JSON)
2. Проверить на 2–3 реальных клиентах
3. Согласовать token-to-code pipeline (Style Dictionary)
4. Решить, что из theme config уходит в Strapi (рекомендация: flat JSON field)

## Открытые задачи

- [ ] Style Dictionary config — CSS + Dart выходы
- [ ] Механизм применения CSS Variables в Next.js
- [ ] Визуальные отличия Travel vs Market
- [ ] Стабилизация token model → затем схема токенов в Strapi
- [ ] Согласование с разработкой: multi-tenant, runtime theming, SSR
