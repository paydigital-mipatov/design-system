# PayDigital Design System

Дизайн-система для white-label платформы PayDigital — единая основа для продуктов Market (витрина цифровых товаров) и Travel (eSIM + карта).

## Что это

Одна платформенная дизайн-система с контролируемой кастомизацией:
- 4-уровневая архитектура токенов (SYS → BRAND → SEM → CMP)
- Light/dark modes
- Поддержка 50+ клиентских тем без хаоса
- Пайплайн: Figma (Tokens Studio) → Git → JSON → Style Dictionary → CSS Variables / Dart

## Структура репозитория

```
design-system/
├── tokens/                     # JSON-токены (SYS, Brand, SEM, CMP)
├── components/                 # Спецификации UI-компонентов
├── guidelines/                 # Правила использования
├── design-system-context.md    # Полный технический референс
├── CLAUDE.md                   # Инструкции для Claude Code
└── README.md                   # Этот файл
```

## Документация

| Файл | Для кого | Что внутри |
|------|---------|-----------|
| [design-system-context.md](design-system-context.md) | Дизайнеры, разработчики, продакт | Архитектура, принципы, токены, Figma-структура, кастомизация, план |
| [CLAUDE.md](CLAUDE.md) | Claude Code | Компактные правила для работы с кодом |

## Ключевые принципы

- Компоненты сидят только на семантических токенах (`sem.*`)
- Клиент меняет только разрешённые брендовые параметры
- В ядре Figma только modes light/dark, клиенты — отдельные validation files
- Marketing blocks — presets, не произвольный конструктор
- Экраны и компоненты не detach

## Быстрый старт

1. Прочитай [design-system-context.md](design-system-context.md) — полный контекст
2. Изучи токены в `/tokens`
3. Смотри компоненты в `/components`
