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

## Архитектура токенов — 4 коллекции

| Коллекция | Описание |
|-----------|----------|
| **Primitive** | Сырая палитра, клиент не трогает. `color.blue.500`, `color.gray.100` |
| **Brand** | N Modes (один на клиента). `brand.primary.light/dark`, `brand.font.family` |
| **Semantic** | Мост Brand ↔ Theme. 2 Modes: light/dark. `color.brand.primary`, `color.bg.surface` |
| **Component** | Алиасит Semantic. `button.bg.default`, `card.radius` |

## Naming convention

- Формат: `категория.подкатегория.роль.состояние`
- Только latin lowercase, точка как разделитель
- CSS: точка → дефис (`color-bg-surface`)
- Dart: точка → camelCase (`colorBgSurface`)

## Структура репозитория

```
design-system/
├── tokens/              # JSON-токены (Primitive, Brand, Semantic, Component)
├── components/          # Спецификации UI-компонентов
├── guidelines/          # Правила использования
├── design-system-context.md  # Полный контекст проекта
└── CLAUDE.md            # Этот файл
```

## Правила работы

- Токены всегда в формате JSON, совместимом с Tokens Studio
- Имена только по naming convention выше
- Не трогать Primitive токены при добавлении клиентских тем
- Клиентский цвет вне нашей палитры → прямое hex в Brand, палитру не трогаем

## Открытые задачи

- [ ] Схема данных токенов в Strapi
- [ ] Style Dictionary config — CSS + Dart выходы
- [ ] Механизм применения CSS Variables в Next.js
- [ ] Визуальные отличия Travel vs Market
