# Design System — PayDigital
_Последнее обновление: апрель 2026_

## Продукт и бренд

PayDigital — зонтичный бренд
├── Market — витрина цифровых товаров (Next.js, PWA)
│   └── Клиенты с кастомными темами, до 50+
└── Travel — eSIM + карта (Next.js web + Flutter mobile)

Компоненты единые пока не решено иначе.

## Стек

- Фронт: Next.js
- CMS: Strapi (уже используется)
- Мобайл: Flutter (только Travel)
- Дизайн: Figma + Tokens Studio Pro
- Токен пайплайн: Tokens Studio → Git → JSON → 
  Style Dictionary → CSS Variables (web) + Dart ThemeExtension (Flutter)
- Тема в рантайме: CSS Variables в <head> при загрузке страницы

## Архитектура токенов — 4 коллекции Figma Variables

### Primitive
Наша палитра, сырые значения. Клиент не трогает.
color.blue.500, color.gray.100, font.family.inter

### Brand
N Modes — один на клиента. Внутри каждого Mode:
- brand.primary.light → прямое hex значение
- brand.primary.dark  → прямое hex значение
- brand.font.family   → одно значение, без light/dark
- brand.font.weight.regular / bold

Клиентский цвет вне нашей палитры → прямое hex, палитру не трогаем.

### Semantic (мост между осями Brand и Theme)
2 Modes: light / dark

color.brand.primary  → light: Brand[client].primary.light
                        dark:  Brand[client].primary.dark
color.bg.surface     → light: color.gray.100 / dark: color.gray.900
color.bg.elevated
color.text.primary
color.text.secondary
color.border.default
font.family.base     → Brand[client].font.family
font.weight.regular
font.weight.bold

### Component (зарезервировано, алиасит Semantic)
button.bg.default    → color.brand.primary
button.bg.hover
button.bg.disabled
button.radius        → 8px (потом переопределяется)
card.bg              → color.bg.surface
card.radius          → 12px
input.border.default → color.border.default
input.border.focus   → color.brand.primary

## Naming convention

Формат: категория.подкатегория.роль.состояние
Правила: только latin lowercase, точка как разделитель
Трансформация: точка → дефис в CSS, точка → camelCase в Dart

## Структура страниц Core Library (Figma)

FOUNDATION
  🎨 Tokens          — визуализация всех 4 коллекций
  📐 Typography      — все три суббренда рядом
  🖼 Icons           — общий сет PayDigital, Slot под клиента

WEB COMPONENTS (Next.js / PWA)
  🧩 Components — Web    — мастера, всегда paydigital·light
  📖 Patterns — Web      — карточка товара, хедер, форма оплаты

FLUTTER COMPONENTS (только Travel)
  📱 Components — Flutter — отдельные мастера, 44pt touch targets
  📖 Patterns — Flutter   — экраны Travel mobile

BRAND SHOWCASE (только для просмотра тем, Modes переключаются здесь)
  🏢 PayDigital      — зонтичный бренд, эталон
  ✈️ Travel          — web + Flutter рядом, light + dark
  🛍 Market          — клиентские витрины, light + dark

## Strapi — токены

- Strapi уже используется
- Токены задаём мы за клиента, не клиент сам
- Схема данных: НЕ СПРОЕКТИРОВАНА → задача для Claude Code

## Открытые вопросы

- [ ] Схема данных токенов в Strapi
- [ ] Визуальные отличия Travel vs Market (какие токены разные)
- [ ] Кастомизация тем клиентов во Flutter — насколько нужна
- [ ] Компоненты Travel для Flutter — отдельная библиотека потом?
- [ ] Style Dictionary config — CSS + Dart выходы
- [ ] Механизм применения CSS Variables в Next.js
