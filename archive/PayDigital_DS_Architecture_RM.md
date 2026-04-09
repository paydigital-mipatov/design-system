# PayDigital Design System Architecture README / Project Prompt

> Этот документ — рабочий мастер-промпт и проектный README по архитектуре дизайн-системы PayDigital в Figma и её дальнейшей связи с разработкой.  
> Его цель: зафиксировать контекст, принятые решения, открытые вопросы, правила системы, структуру файлов и токенов, а также план реализации так, чтобы этот файл можно было передать в другой чат, новой команде, дизайнеру или разработчику без потери смысла.

---

## 0. Как использовать этот README

Этот документ можно использовать в четырёх режимах:

1. **Как проектный README** — чтобы быстро понять, что строим, зачем и по каким правилам.
2. **Как промпт для нового чата** — можно вставлять целиком или частями, чтобы продолжить работу без потери контекста.
3. **Как основу для дизайн- и техсинков** — по нему можно обсуждать решение с продактом и разработчиками.
4. **Как инструкцию к внедрению** — по шагам собирать архитектуру в Figma и готовить её к коду.

---

# 1. Контекст проекта

## 1.1. Продукты

У компании есть несколько продуктов и множество клиентов.

### Продукт 1
Витрина цифровых товаров:
- подписки на ChatGPT
- покупка аккаунтов
- другие цифровые товары

### Продукт 2
Travel app:
- eSIM
- пополняемая карта для оплаты за рубежом

## 1.2. Платформенная цель

Планируется:
- разработать одну или две универсальные витрины,
- переводить текущих клиентов на эти универсальные варианты,
- управлять бэкендом / контентом / блоками через **Strapi**,
- использовать **Next.js** на фронтенде.

## 1.3. Брендинг и клиенты

Сейчас:
- у компании уже есть **10+ клиентов**,
- ранее упоминалось, что клиентов с витринами цифровых товаров уже **20+**, в будущем может быть **50+**,
- у клиентов уже есть свои кастомные темы.

Цель:
- **свести текущие разрозненные темы в единый framework**,
- не потерять white-label возможности,
- не превратить систему в хаос ручных исключений.

## 1.4. Предпочтительная модель бренда

Принятая рабочая цель:
- **1 бренд клиента = 1 общая тема на все продукты**

То есть в идеале один и тот же клиент получает единый визуальный язык для:
- витрины цифровых товаров,
- travel,
- и, возможно, позже других продуктов.

---

# 2. Главная задача

Нужно спроектировать архитектуру дизайн-системы в Figma, которая:

- поддерживает **multi-brand white-label**,
- выдерживает **light/dark theme**,
- масштабируется на десятки клиентов,
- не требует detatch компонентов и экранов,
- позволяет валидировать каждую клиентскую тему в отдельном файле,
- потом может быть перенесена в код через **машиночитаемую token architecture**,
- в идеале связана с **Next.js** и позже со **Strapi**.

---

# 3. Короткий итог решения

## 3.1. Что строим

Не две отдельные дизайн-системы.  
Не page builder.  
Не library на каждого клиента.

Строим:

- **одну платформенную дизайн-систему**
- с **общим system foundation**
- с **brand token layer**
- с **semantic token layer**
- с **общими core-компонентами**
- с **preset-вариативностью marketing-блоков**
- с **light/dark modes**
- с **отдельным validation file на каждого клиента**

## 3.2. Что это по сути

Это **white-label платформа с контролируемой кастомизацией**, а не полный конструктор интерфейсов.

---

# 4. Бриф-вопросы и статус по ним

Ниже — список ключевых бриф-вопросов с текущим статусом.

---

## 4.1. Что первично: единый framework или независимые темы?

**Вопрос:**  
Что для компании первично:
- один общий framework темизации,
- или независимые темы, которые потом как-то объединят?

**Решение / ответ:**  
У клиентов уже есть свои кастомные темы.  
Сейчас задача — **свести их в единый framework**.

**Статус:** принято.

**Влияние:**  
Это означает, что архитектура должна быть не "чисто greenfield", а уметь:
- переваривать существующие темы,
- нормализовывать их,
- маппить в системные правила.

---

## 4.2. Модель бренда: один клиент = одна тема на все продукты?

**Вопрос:**  
Нужно ли стремиться к модели:
- 1 клиент = 1 общая тема на все продукты,
- или допускать отдельные темы по продуктам?

**Решение / ответ:**  
Принята рабочая модель:
- **1 бренд = 1 общая тема на все продукты**

Travel потенциально может позже визуально отстроиться, но на текущем этапе думаем в единую бренд-рамку.

**Статус:** принято как рабочее решение.

**Подвешенный риск:**  
Travel может позже сильнее отделиться.

**Влияние:**  
Сейчас разумно строить:
- общую token architecture,
- общий semantic layer,
- с возможностью позже добавить product-specific aliases, если travel реально разойдётся с digital storefront.

---

## 4.3. Есть ли master brand платформы поверх бренда клиента?

**Вопрос:**  
Должен ли быть заметен PayDigital как master brand поверх клиента, или продукт почти полностью white-label?

**Ответ:**  
Пока неясно. Сейчас от master brand есть только логотип.

**Статус:** подвешено.

**Что уточнить:** у продукта / бизнеса.  
См. раздел с вопросами команде.

**Влияние:**  
Если master brand станет значимым:
- могут появиться системные ограничения на white-label,
- часть UI-ролей может стать platform-owned и недоступной для кастомизации,
- может понадобиться отдельный брендовый слой платформы поверх клиентского.

---

## 4.4. Насколько кастомизация — конкурентное преимущество?

**Вопрос:**  
Нужен controlled customization или почти конструктор?

**Ответ:**  
Кастомизация важна. Разрешается менять:
- цвета,
- шрифты,
- иконки,
- иллюстрации,
- баннеры,
- контент,
- часть marketing/hero блоков,
- включение/выключение отдельных блоков.

Но при этом нет цели идти в полный page-builder chaos machine.

**Статус:** принято.

**Вывод:**  
Модель решения:
- **контролируемая кастомизация через разрешённые параметры и preset-варианты**.

---

## 4.5. Одна DS или две?

**Вопрос:**  
Нужна одна дизайн-система на оба продукта или две?

**Ответ:**  
Предпочтение — **одна**.

**Статус:** принято.

**Влияние:**  
Нужны:
- общий foundation,
- общий semantic layer,
- общие core-компоненты,
- при необходимости позже — product-specific слой поверх.

---

## 4.6. Что будет ядром системы?

**Вопрос:**  
Что является центром решения?

**Ответ:**  
Ядро:
- **витрина**
- **личный кабинет**

**Статус:** принято.

**Влияние:**  
Приоритизация компонентов и шаблонов страниц должна идти от этих сценариев.

---

## 4.7. Что можно кастомизировать, а что нет?

**Вопрос:**  
Есть ли whitelist параметров или sales будут продавать всё подряд?

**Ответ:**  
Разрешены:
- лого,
- фирменные цвета,
- шрифты,
- иконки,
- иллюстрации,
- баннеры,
- контент,
- выключение/включение предусмотренных блоков,
- некоторые preset-варианты marketing/hero блоков.

Не планируется:
- свободная перестройка core UI,
- page builder,
- произвольная архитектура страниц на клиента.

**Статус:** принято как рабочая граница.

---

## 4.8. Нужно ли разделять brand tokens и system tokens?

**Вопрос:**  
Нужно ли разделить:
- client brand tokens,
- и platform/system tokens, которые клиент не трогает?

**Ответ ассистента:**  
Да, это **обязательно**.

**Статус:** принято как архитектурное правило.

**Влияние:**  
Без этого система развалится:
- компоненты начнут зависеть от raw client colors,
- бренд будет ломать продукт,
- поддержка десятков клиентов станет хаосом.

---

## 4.9. Нужен ли режим "почти без кастома"?

**Вопрос:**  
Нужны ли theme presets / быстрый onboarding?

**Ответ:**  
Да, нужен.

**Статус:** принято.

**Влияние:**  
Архитектура должна поддерживать:
- preset themes,
- быстрый старт клиента с минимальным набором параметров:
  - логотип,
  - primary,
  - secondary/accent,
  - шрифт,
  - maybe preset hero style.

---

## 4.10. Какие параметры токенизировать в v1?

**Ответ пользователя:**
В v1 точно:
- color
- typography family
- typography weight
- dark theme

Не в v1:
- border radius customization
- spacing customization

**Статус:** принято.

**Влияние:**  
Система должна содержать radius/spacing как system primitives,  
но **не открывать их как client customization** в первой версии.

---

## 4.11. Нужны ли foundation и semantic layer одновременно?

**Вопрос:**  
Только foundation или foundation + semantic?

**Ответ ассистента:**  
Нужны **оба слоя**.

**Статус:** принято.

**Пояснение:**  
- foundation/primitives = сырые базовые значения,
- semantic = смысловые роли (`button.primary.bg`, `text.default`, `bg.surface`).

Без semantic слоя multi-brand white-label система будет кривой.

---

## 4.12. Нужен ли component-level token layer в v1?

**Ответ:**  
Не как обязательный полный слой.  
Но **место под него надо заложить**.

**Статус:** принято.

**Практика:**  
В v1:
- компоненты сидят на semantic tokens,
- component-specific aliases можно добавить позже, если реально понадобится.

---

## 4.13. Нужно ли различать global / brand / product / mode?

**Ответ:**  
Да, логически различать надо:
- system/global
- brand/client
- product (позже, если реально понадобится)
- mode (light/dark)

**Статус:** принято.

---

## 4.14. Полная переносимость токенов в код важна?

**Ответ пользователя:**  
Да, критично важна.

**Статус:** принято.

**Влияние:**  
Архитектура Figma не должна быть чисто "дизайнерской".  
Она должна быть совместима с:
- JSON token representation,
- code mapping,
- CSS variables / theme config,
- и, вероятно, с отдельным export/sync pipeline.

---

## 4.15. Dark theme обязательна?

**Ответ:**  
Dark theme — **опциональна**.

Но пользователь ожидает, что уже со старта могут быть разные dark themes.

**Статус:** частично принято, частично подвешено по реализации.

**Рекомендация ассистента:**  
Не делать полностью вручную уникальную dark тему на каждого клиента.  
Лучше:
- системная dark база,
- плюс ограниченный набор client overrides.

**Что ещё не решено окончательно:**  
Будет ли dark:
- полуавтоматически производной от light,
- или полностью ручной.

---

## 4.16. Что управляется через Strapi?

**Ответ пользователя:**  
Сейчас предполагается:
- контент
- порядок и состав блоков
- layout
- feature flags

Но по теме и ещё части деталей уверенности пока нет.

**Статус:** подвешено.

**Рекомендация:**  
На старте не засовывать в Strapi сырой хаос токенов.  
Сначала стабилизировать token architecture, потом решать, что именно из theme config уходит в CMS.

---

## 4.17. Кто должен управлять темой?

**Ответ пользователя:**  
Пока кажется, что дизайнеры.

**Ответ ассистента:**  
Не только дизайнеры. Иначе система не масштабируется.

**Принятое рабочее решение:**  
- дизайн владеет системой, правилами и QA,
- тему могут инициировать / заводить и другие роли по разрешённым параметрам,
- дизайн ревьюит нестандартные кейсы.

**Статус:** принято как целевая модель.

---

## 4.18. Должен ли клиент запускаться без релиза фронта?

**Ответ пользователя:**  
В идеале да, но если это сильно усложняет — можно и без этого.

**Статус:** подвешено с уклоном в "желательно".

**Влияние:**  
Очень сильно влияет на:
- runtime theming / config-driven approach,
- хранение tenant config,
- multi-tenant фронт,
- то, где живёт theme config.

---

## 4.19. Один кодбейс или несколько?

**Ответ пользователя:**  
Неизвестно.

**Рекомендация ассистента:**  
Под этот кейс почти наверняка лучше:
- **один кодбейс multi-tenant**
- + общая component library
- + theme config на клиента.

**Статус:** рекомендация, нужно подтвердить с разработкой.

---

## 4.20. Что делать с нативным travel app?

**Ответ пользователя:**  
Будет отдельное мобильное приложение.

**Рекомендация ассистента:**  
Нужно:
- одна общая token architecture,
- но не одна компонентная библиотека на web и native.

То есть:
- общие foundation / brand / часть semantic,
- разные component libraries под web и mobile native.

**Статус:** рабочая рекомендация.

---

## 4.21. Нужна ли responsive token strategy?

**Ответ пользователя:**  
Не было уверенности.

**Ответ ассистента:**  
Да, но минимально.

**Принятое решение:**  
В v1 заложить минимум:
- typography mobile/desktop distinctions,
- layout paddings/gutters,
- spacing scale как system primitive.

**Статус:** принято.

---

## 4.22. Нужна ли RTL-поддержка?

**Ответ пользователя:**  
Неизвестно, надо уточнить.

**Статус:** подвешено.

**Зафиксированный вопрос команде:**  
Нужно отдельно уточнить у продукта и разработки, планируется ли RTL-поддержка (арабский, иврит и т.п.).  
Это уже вынесено как важный будущий вопрос.

---

## 4.23. Кто владеет системой?

**Ответ пользователя:**  
Хочется, чтобы дизайн.

**Рекомендация ассистента:**  
Да:
- дизайн как owner системы,
- но с синхронизацией с фронтендом,
- и с жёсткими правилами кастомизации.

**Статус:** принято как рабочее решение.

---

## 4.24. Что считается успехом v1?

**Ответ пользователя:**  
Критерии успеха:
- 80% экранов живут на общих компонентах
- токены синкаются с кодом без ручного ада

**Статус:** принято.

---

# 5. Открытые вопросы, которые ещё не закрыты

Ниже только те вопросы, по которым нет финального решения и которые реально влияют на архитектуру.

## 5.1. Вопросы по продукту / бизнесу
- Нужен ли master brand поверх клиента?
- Может ли Travel позже реально визуально отделиться от Digital?
- Нужна ли dark theme всем или это premium customization?
- Какие hero/layout presets реально нужны в v1?
- Планируются ли RTL-рынки в горизонте 12–18 месяцев?

## 5.2. Вопросы по Strapi / runtime
- Должен ли новый клиент запускаться без нового релиза фронта?
- Должен ли Strapi хранить theme config или только контент/блоки?
- Кто реально будет заводить нового клиента в процессе?

## 5.3. Вопросы по разработке
- Один кодбейс multi-tenant или несколько фронтов?
- Как фронт определяет клиента (домен / subdomain / route / tenant id)?
- Когда подгружается тема?
- Возможен ли theme config как JSON / tenant config?
- Готовы ли разработчики делать theming через CSS variables?
- Какой формат token sync им удобен?

---

# 6. Вопросы команде и на что они влияют

Ниже — готовый список вопросов для команды.

---

## 6.1. Вопросы продакту

### 1. Планируются ли RTL-рынки в горизонте 12–18 месяцев?
**Почему спрашиваем:**  
RTL влияет на архитектуру компонентов, layout rules и потенциально на токены / правила directionality.

**Если ответ "да/возможно":**
- нельзя проектировать компоненты так, будто левый-to-правый мир вечный,
- придётся учитывать зеркалирование layout позже.

---

### 2. Должен ли Travel в будущем визуально отделиться от Digital, или пока это один брендовый язык?
**Почему спрашиваем:**  
Это влияет на:
- необходимость product-specific layer,
- расщепление библиотек/паттернов,
- глубину унификации.

**Если Travel пойдёт отдельно:**
- нужен отдельный product alias layer,
- но foundation лучше всё равно оставить общим.

---

### 3. Какие блоки на витрине реально должны быть вариативны в v1?
**Почему спрашиваем:**  
Нужно не раздувать архитектуру под всё подряд.

**Влияет на:**
- состав Marketing Blocks,
- набор preset-вариантов,
- структуру полей в Strapi.

---

### 4. Какие hero/layout preset-варианты реально нужны в v1?
**Почему спрашиваем:**  
Hero и marketing blocks — главный источник вариативности и хаоса.

**Влияет на:**
- объём работы в Figma,
- объём логики в коде,
- поля конфигурации в CMS.

---

### 5. Dark theme — обязательная для всех, опциональная или premium?
**Почему спрашиваем:**  
Это влияет на:
- объём работ,
- глубину dark token architecture,
- обязательность validation в dark mode для каждого клиента.

---

### 6. Есть ли platform/master brand, который должен быть виден поверх клиента?
**Почему спрашиваем:**  
Это влияет на границы white-label:
- что owned by platform,
- что owned by client.

---

### 7. Есть ли готовность жёстко запрещать sales продавать произвольную кастомизацию core UI?
**Почему спрашиваем:**  
Если такой готовности нет, система умрёт.

**Влияет на:**
- governance,
- ограничения в system docs,
- то, насколько вообще можно строить controlled system.

---

## 6.2. Вопросы разработке

### 1. Один кодбейс multi-tenant или несколько фронтов?
**Почему спрашиваем:**  
Это определяет основную техмодель реализации.

**Предпочтительная рекомендация:**  
Один кодбейс multi-tenant.

---

### 2. Может ли новый клиент подключаться без отдельного релиза фронта?
**Почему спрашиваем:**  
Это определяет, насколько theme/config-driven будет решение.

**Влияет на:**
- runtime theming,
- tenant config storage,
- integration with Strapi or config files.

---

### 3. Как определяется клиент?
Варианты:
- домен,
- subdomain,
- route,
- tenant id.

**Почему спрашиваем:**  
Нужно понимать, как подхватывать тему и контент на конкретного клиента.

---

### 4. Когда подгружается тема?
Варианты:
- на сервере до рендера,
- во время server-side render,
- после загрузки страницы в браузере.

**Почему спрашиваем:**  
Это влияет на:
- UX,
- риск мигания неправильной темы,
- runtime theming strategy.

---

### 5. Можно ли хранить theme config как JSON / tenant config?
**Почему спрашиваем:**  
Это влияет на перенос токенов из Figma в код и на возможность конфигурировать тему без переписывания компонентов.

---

### 6. Готовы ли вы реализовывать theming через CSS variables?
**Почему спрашиваем:**  
Под white-label multi-tenant storefront это обычно самый практичный вариант.

---

### 7. Какой формат sync токенов вам удобен?
Варианты:
- JSON
- Style Dictionary
- custom token schema
- Tokens Studio export
- собственный pipeline

**Почему спрашиваем:**  
Figma не должна жить отдельно от кода.

---

### 8. Где предполагается хранить theme config?
Варианты:
- в коде,
- в отдельном JSON,
- в БД,
- в Strapi,
- в tenant config registry.

**Почему спрашиваем:**  
Это влияет на source of truth и процесс запуска клиента.

---

### 9. Как будут поддерживаться light/dark и client overrides?
**Почему спрашиваем:**  
Это влияет на:
- модель токенов,
- структуру theme config,
- CSS variable strategy.

---

### 10. Нужны ли очень быстрые white-label запуски на разных доменах?
**Почему спрашиваем:**  
Это влияет на:
- multi-tenant architecture,
- доменную конфигурацию,
- runtime theme resolution.

---

# 7. Базовая модель решения

## 7.1. Что предлагается построить

Предлагается сделать:

### One Platform Design System
С такой структурой:

1. **System primitives**
2. **Brand tokens**
3. **Semantic tokens**
4. **Core components**
5. **Marketing presets**
6. **Page templates**
7. **Client validation files**

---

## 7.2. Ключевой принцип

Клиент **не меняет систему**.  
Клиент меняет только **разрешённые брендовые параметры и ассеты**.

Это критично.

---

## 7.3. Что разрешено кастомизировать

Разрешённая кастомизация:
- логотип
- primary / secondary / accent colors
- шрифтовые семьи
- font weight mapping
- dark theme participation
- иконки
- иллюстрации
- баннеры
- контент
- выбор preset marketing / hero block
- включение / выключение разрешённых блоков
- assets in slots

---

## 7.4. Что кастомизировать нельзя

### Нельзя:
- менять core layout architecture
- ломать структуру core-компонентов
- произвольно собирать страницы как в page builder
- добавлять ad hoc клиентские overrides в ядро DS
- заводить каждого клиента как mode в core system
- привязывать компоненты напрямую к raw brand hex
- делать detatch экранов как стандартный workflow
- превращать marketing customization в уникальный набор компонентов под клиента

---

## 7.5. Почему так

Потому что иначе вы получаете:
- помойку в Figma,
- несинхрон с кодом,
- дорогое сопровождение,
- невозможность масштабироваться на 20–50 клиентов.

---

# 8. Архитектурные принципы

## 8.1. Разделение слоёв
Система обязана различать:
- system
- brand
- semantic
- components
- page templates
- client validation

---

## 8.2. Semantic layer обязателен
Никакой компонент не должен сидеть напрямую на:
- `brand.color.primary`,
- или raw hex.

Компоненты сидят только на:
- `sem.*`

---

## 8.3. Core modes = only light/dark
В ядре:
- только `light`
- только `dark`

Не делать client modes в core DS.

---

## 8.4. Клиенты валидируются отдельно
Каждый клиент живёт в отдельном **validation file**.  
Не в core modes.

---

## 8.5. Marketing variability = presets, not freeform chaos
Hero и marketing sections делаются как:
- preset variants
- slots
- conditional options

Но не как произвольный конструктор.

---

## 8.6. Не detatch
Валидация и клиентские файлы должны работать на:
- instances
- section instances
- templates
- variables / mappings

А не на ручной перекраске и detatch.

---

## 8.7. Figma ≠ единственный source of truth
Figma — место проектирования и проверки.  
Но итоговая система должна быть переносима в код.

---

# 9. Архитектура файлов в Figma

Ниже — рекомендуемая файловая структура.

---

## 9.1. `PD DS / Foundations`
Файл с токенами и основами системы.

### Содержит:
- system primitives
- semantic tokens
- light/dark modes
- typography rules
- spacing/layout primitives
- docs pages по naming и mapping

### Назначение:
- источник базовой логики токенов

---

## 9.2. `PD DS / Core Components`
Файл системных компонентов.

### Содержит:
- buttons
- inputs
- selects
- tabs
- chips
- badges
- modals
- drawers
- toasts
- nav primitives
- account primitives
- basic storefront blocks

### Правило:
Все компоненты используют semantic tokens.

---

## 9.3. `PD DS / Marketing Blocks`
Отдельный файл для маркетинговых секций.

### Содержит:
- hero presets
- promo banners
- feature grids
- text/media blocks
- cta sections
- faq blocks
- image/text sections
- logos strips

### Правило:
Вся вариативность тут должна быть:
- preset-based,
- controlled,
- without turning into builder.

---

## 9.4. `PD DS / Page Templates`
Файл шаблонов экранов.

### Минимальный набор страниц:
- Home
- Listing / Catalog
- Detail / Offer
- Checkout
- Account Main
- Account Transactions / Orders
- Auth / Login / OTP / Registration if relevant
- Support / FAQ if relevant

### Важно:
Это не нарисованные вручную фреймы.  
Это **template screens, composed from section instances**.

---

## 9.5. `PD Client / [BrandName] / Validation`
Отдельный файл на клиента.

### Содержит:
- brand mapping
- local brand variables
- approved assets
- validation screens
- light/dark checks
- edge cases
- notes / status

### Это и есть основной рабочий формат для ближайших 10–15 клиентов.

---

# 10. Структура Variable Collections в Figma

Ниже — целевая модель collections.

---

## 10.1. Коллекция: `SYS / Primitives`

### Назначение
Содержит системные базовые значения.  
Не является клиентской кастомизацией.

### Основные группы:
- `color / neutral`
- `color / status`
- `space`
- `radius`
- `font / family`
- `font / weight`
- `type / size`
- `type / lineHeight`
- `layout`

### Пример структуры:
- `sys.color.neutral.0`
- `sys.color.neutral.50`
- `sys.color.neutral.100`
- `sys.color.neutral.900`
- `sys.color.success.500`
- `sys.space.16`
- `sys.radius.8`
- `sys.font.weight.medium`
- `sys.type.size.h1.mobile`
- `sys.type.size.h1.desktop`
- `sys.layout.padding.mobile`
- `sys.layout.padding.desktop`

---

## 10.2. Коллекция: `BRAND / Mapping`

### Назначение
Содержит брендовые значения клиента.

### Где живёт
В рабочей модели v1 — **локально в client validation file**.

### Modes:
- `light`
- `dark`

### Основные группы:
- `color / core`
- `font / family`
- `font / weight`
- `theme / options`

### Примеры:
- `brand.color.primary`
- `brand.color.secondary`
- `brand.color.accent`
- `brand.font.family.base`
- `brand.font.family.heading`
- `brand.font.weight.regular`
- `brand.font.weight.medium`
- `brand.font.weight.bold`

---

## 10.3. Коллекция: `SEM / Tokens`

### Назначение
Главный semantic слой, на котором сидят компоненты.

### Modes:
- `light`
- `dark`

### Основные группы:
- `bg / page`
- `bg / surface`
- `text`
- `icon`
- `border`
- `action / primary`
- `action / secondary`
- `action / tertiary`
- `field`
- `link`
- `status / success`
- `status / warning`
- `status / danger`
- `overlay`
- `focus`

### Примеры:
- `sem.bg.page.default`
- `sem.bg.surface.default`
- `sem.text.default`
- `sem.text.brand`
- `sem.border.default`
- `sem.action.primary.bg`
- `sem.action.primary.text`
- `sem.field.border`
- `sem.link.default`

---

## 10.4. Коллекция: `CMP / Optional`
На v1 можно не наполнять полноценно.

### Назначение
Будущий слой component aliases, если позже понадобится.

### Пример:
- `cmp.button.primary.bg`
- `cmp.card.default.bg`

### Статус:
Архитектурный слот заложить, но в v1 не раздувать.

---

# 11. Naming conventions

## 11.1. Общее правило
Имена должны быть:
- скучными,
- жёсткими,
- предсказуемыми,
- машиночитаемыми.

### Формат:
`[layer].[group].[role].[variant].[state]`

Не всегда все части нужны, но логика должна быть одинаковой.

---

## 11.2. Примеры

### System
- `sys.color.neutral.0`
- `sys.font.weight.medium`
- `sys.type.size.h2.desktop`

### Brand
- `brand.color.primary`
- `brand.font.family.base`

### Semantic
- `sem.bg.page.default`
- `sem.text.default`
- `sem.action.primary.bg`
- `sem.field.border`

---

## 11.3. Чего не делать
Плохие названия:
- `BlueMain`
- `FinalPrimary`
- `ButtonBlue`
- `ClientRed2`
- `Header text black maybe`

Так нельзя.

---

# 12. Modes в Figma

## 12.1. Что использовать
В ядре системы:
- только `light`
- только `dark`

---

## 12.2. Что не использовать
Нельзя использовать modes для:
- клиентов
- travel/digital
- mobile/desktop как глобальный mode на всю систему

Это очень быстро превращает файл в болото.

---

## 12.3. Почему клиенты не modes
Потому что:
- клиент — это не архитектурное состояние системы,
- а операционная сущность / tenant,
- количество клиентов растёт,
- каждый новый token придётся руками заполнять для всех client modes,
- это ломает масштабируемость.

---

# 13. Почему нужен validation file на клиента

## 13.1. Что это такое
**Client validation file** — это отдельный Figma-файл клиента, который:
- подключён к core DS libraries,
- содержит локальные brand variables,
- маппит бренд клиента на system/semantic layer,
- показывает эталонные страницы и edge cases,
- не требует detatch.

---

## 13.2. Зачем он нужен
Он нужен, чтобы:
- не тащить клиентов в core modes,
- проверять тему клиента на живой системе,
- хранить approved assets и status,
- показывать тему продукту/аккаунту/разработке,
- иметь staging/QA пространство до кода.

---

## 13.3. Что в нём должно быть

### Страницы:
1. `00 Cover`
2. `01 Theme Mapping`
3. `02 Assets`
4. `03 Light / Core Screens`
5. `04 Dark / Core Screens`
6. `05 Edge Cases`
7. `06 Review Notes`

---

## 13.4. Как это работает
- В core DS компоненты используют `sem.*`.
- В client file локально задаются `brand.*`.
- Semantic роли маппятся на бренд.
- Эталонные экраны собраны из instances и templates.
- Theme validation происходит без detatch.

---

# 14. Как не detatch экраны

## 14.1. Главное правило
Нельзя делать workflow вида:
- нарисовали homepage,
- detatch,
- перекрасили руками под клиента.

Это тупик.

---

## 14.2. Как надо
Экраны собираются из:
- component instances,
- section instances,
- page templates.

---

## 14.3. Какие уровни компонентов должны быть

### Уровень 1. Base components
- Button
- Input
- Select
- Tabs
- Badge
- Modal
- Card
- IconButton
и т.д.

### Уровень 2. Product/UI blocks
- Product card
- Tariff card
- Balance card
- Transaction row
- Search bar
- Payment method selector
- Checkout summary

### Уровень 3. Marketing/page sections
- Hero A
- Hero B
- Promo strip
- Feature grid
- FAQ block
- CTA section

---

## 14.4. Что такое page template
Это screen-level template, собранный из section instances.

### Пример Home:
- Header instance
- Hero instance
- Feature grid instance
- Product grid instance
- FAQ instance
- Footer instance

---

# 15. Responsive token strategy (минимум для v1)

## 15.1. Нужна ли?
Да, но минимально.

## 15.2. Что закладываем
### Typography
- mobile/desktop distinctions на ключевые размеры

### Layout
- container max width
- mobile/desktop paddings
- mobile/desktop gutters

### Spacing
- единая system scale

## 15.3. Что не делаем
Не делаем:
- responsive modes на всё подряд
- клиентскую кастомизацию spacing в v1

---

# 16. Dark theme strategy

## 16.1. Что не стоит делать
Не стоит делать полностью ручную отдельную dark-вселенную для каждого клиента в v1.

Это:
- дорого,
- трудно,
- плохо масштабируется.

---

## 16.2. Рекомендуемая стратегия
### Semi-automated dark theme
- системная база dark mode,
- плюс ограниченный набор client overrides.

### Всегда системные:
- page bg
- surface bg
- default text
- borders

### Потенциально клиентские override:
- primary in dark
- accent in dark
- selected highlights
- subtle brand surfaces

---

## 16.3. Что ещё надо уточнить
Нужно решить с продуктом:
- всем ли клиентам нужна dark theme,
- является ли она частью стандарта или кастомной опцией.

---

# 17. Базовая реализация в Figma — пошагово

## Этап 1. Зафиксировать правила
Нужно описать:
- что такое system token,
- что такое brand token,
- что такое semantic token,
- что можно кастомизировать,
- что нельзя,
- какие экраны validation,
- как проходит approval.

---

## Этап 2. Собрать `PD DS / Foundations`
Создать:
- `SYS / Primitives`
- `SEM / Tokens`

Заполнить:
- neutral scale
- status colors
- spacing
- radius
- type scale
- layout primitives
- light/dark semantic tokens

---

## Этап 3. Собрать `PD DS / Core Components`
Минимальный обязательный набор:
- Button
- Input
- Field
- Tabs
- Card
- Modal
- Nav
- Badge
- core account/storefront atoms

Все они должны ссылаться только на semantic tokens.

---

## Этап 4. Собрать `PD DS / Marketing Blocks`
Сначала только критичные:
- Hero A/B
- Banner strip
- Feature grid
- Promo cards
- FAQ
- CTA

Не делать 30 блоков сразу.

---

## Этап 5. Собрать `PD DS / Page Templates`
Нужны хотя бы:
- Home
- Listing
- Detail
- Checkout
- Account

---

## Этап 6. Сделать 2–3 demo brands
Не реальные клиенты сразу все подряд, а:
- neutral demo,
- bright demo,
- stress-test demo.

Нужно, чтобы проверить устойчивость системы.

---

## Этап 7. Сделать первый `Client / [Brand] / Validation`
Внутри:
- brand mapping
- light/dark
- assets
- core screens
- edge cases

---

## Этап 8. Прогнать ещё 2–3 реальных клиента
Только после этого финально допиливать архитектуру.

Это важно:  
система покажет слабые места только на реальных брендах.

---

## Этап 9. Зафиксировать export/sync модель с разработкой
После стабилизации token architecture:
- выбрать JSON schema,
- выбрать подход к token export,
- согласовать theme config structure,
- согласовать storage и runtime resolution.

---

# 18. Реализация client validation file — инструкция

Ниже — конкретная схема клиентского validation file.

---

## 18.1. Название файла
`PD Client / [BrandName] / Validation`

---

## 18.2. Страницы файла

### `00 Cover`
Содержит:
- client name
- product scope
- theme version
- owner
- date
- status

---

### `01 Theme Mapping`
Содержит:
- логотип
- primary
- secondary
- accent
- base font
- heading font
- weight mapping
- dark enabled
- notes

---

### `02 Assets`
Содержит:
- approved logo
- approved icon set
- approved banners
- approved illustrations
- hero media
- dark assets if relevant

---

### `03 Light / Core Screens`
Содержит:
- homepage
- listing
- detail
- checkout
- account

---

### `04 Dark / Core Screens`
Содержит те же экраны в dark.

---

### `05 Edge Cases`
Содержит:
- long titles
- empty states
- disabled buttons
- form validation
- weird logo on backgrounds
- image fit issues
- contrast problem zones

---

### `06 Review Notes`
Содержит:
- issues list
- open questions
- review rounds
- approval status
- final notes

---

## 18.3. Как маппить тему клиента
В client file локально создаётся `BRAND / Mapping`.

### Пример:
- `brand.color.primary = #005BFF`
- `brand.color.secondary = #7C3AED`
- `brand.color.accent = #F59E0B`
- `brand.font.family.base = Inter`
- `brand.font.family.heading = Onest`

Далее semantic роли маппятся на brand values:
- `sem.action.primary.bg -> brand.color.primary`
- `sem.text.brand -> brand.color.primary`
- `sem.link.default -> brand.color.primary`

---

## 18.4. Как работать без detatch
- Использовать page templates и section instances.
- Менять только:
  - content overrides
  - asset slots
  - allowed variant switches
  - brand mapping variables

Не detatch страницы и не перекрашивать руками.

---

# 19. Что делать нельзя

Это критичный раздел. Ниже — список запретов.

## Нельзя:
1. Делать клиентов как modes в core DS.
2. Привязывать компоненты напрямую к brand hex / raw colors.
3. Держать клиентские исключения прямо в core components file.
4. Делать detatch экранов как стандартный способ кастомизации.
5. Делать page-builder chaos machine под видом дизайн-системы.
6. Разрешать sales продавать произвольные изменения core UI без system review.
7. Раздувать component token layer в v1.
8. Заводить отдельную library на каждого клиента.
9. Хранить theme logic только в Figma без мысли о коде.
10. Впихивать в Strapi сырой хаос токенов до стабилизации модели.

---

# 20. Как превратить это в решение в разработке

Ниже — целевая логика, а не окончательная implementation spec. Окончательные детали нужно согласовать с разработкой.

---

## 20.1. Целевая техмодель

### Предпочтительная архитектура:
- **один Next.js кодбейс**
- **multi-tenant storefront**
- **общая component library**
- **theme config на клиента**
- **контент и блоки через Strapi**
- **токены в машиночитаемом виде (JSON / token schema)**

---

## 20.2. Какой слой чем должен быть

### Figma
Для:
- проектирования,
- проверки,
- документации,
- theme validation,
- demo / QA.

### JSON / token schema
Для:
- экспорта токенов,
- хранения token relationships,
- использования в code pipeline.

### Code
Для:
- rendering,
- CSS variables / theme objects,
- runtime theme application,
- component styling.

### Strapi
Для:
- контента,
- структуры страниц,
- включения/выключения блоков,
- preset selections,
- media refs,
- возможно позже части client theme metadata.

---

## 20.3. Рекомендуемая реализация theming в коде

### Предпочтительно:
- design tokens as JSON
- CSS variables for runtime theming
- tenant theme config
- semantic mapping in UI layer

### Почему:
Это самый практичный путь для white-label multi-tenant storefront.

---

## 20.4. Какой может быть pipeline

### Базовый сценарий:
1. Токены спроектированы в Figma
2. Экспортируются / фиксируются в JSON schema
3. JSON маппится в code theme structure
4. Компоненты используют semantic tokens через CSS variables / token references
5. Tenant config задаёт brand values и feature choices
6. Strapi отдаёт content/layout/media choices

---

## 20.5. Что делать со Strapi

### На старте Strapi должен управлять:
- контентом
- порядком и составом блоков
- feature flags
- media assets
- preset choice

### Позже Strapi может хранить:
- `primary`
- `secondary`
- `accent`
- `font family`
- `dark enabled`
- `hero preset`
- `logo ref`
- `assets refs`

Но не надо тащить это туда до тех пор, пока token model не стабилизирована.

---

## 20.6. Что уточнить с разработкой про SSR / SSG / ISR
Нужно понять:
- как рендерятся страницы,
- когда подгружается тема,
- можно ли запускать нового клиента без нового релиза,
- как избежать flash of wrong theme,
- как theme config попадает в рантайм.

---

# 21. Технические решения, принятые в результате обсуждения

Ниже — consolidated list решений, которые можно считать принятыми в текущем контексте.

## Принято:
1. Строится **одна платформенная дизайн-система**, не две.
2. Целевая модель — **1 бренд клиента = 1 тема на все продукты**.
3. Основа — **controlled white-label**, не page builder.
4. Нужны отдельные слои:
   - system
   - brand
   - semantic
5. **Semantic layer обязателен**.
6. В ядре Figma используются только modes:
   - `light`
   - `dark`
7. Клиенты **не хранятся как modes** в core DS.
8. Каждый клиент проверяется в отдельном **validation file**.
9. Компоненты и экраны **не detatch**.
10. Marketing variability делается через **preset variants** и разрешённые if-ветки.
11. В v1 токенизируются:
   - color
   - typography family
   - typography weight
   - dark theme
12. Radius/spacing пока остаются system-owned, без client customization.
13. Responsive strategy нужна минимальная:
   - typography mobile/desktop
   - layout paddings/gutters
   - spacing scale
14. В идеале theme config должен быть переносим в код.
15. Дизайн — owner системы.
16. Тема не должна зависеть только от дизайнеров в операционке.
17. Целевой успех v1:
   - 80% экранов на общих компонентах
   - синк токенов с кодом без ручного ада

---

# 22. Подвешенные решения

## Не закрыто:
1. Нужен ли master brand поверх клиента?
2. Пойдёт ли Travel позже в отдельное визуальное направление?
3. Насколько обязательна dark theme для всех клиентов?
4. Должен ли новый клиент запускаться без релиза?
5. Должен ли Strapi хранить theme config?
6. Один кодбейс multi-tenant — финально подтверждено ли это разработкой?
7. Нужна ли RTL-поддержка в будущем?
8. Какая точная схема token export / sync будет выбрана?
9. Какой exact runtime theming / config loading подход будет у фронта?

---

# 23. Рекомендованный набор следующих шагов

## Шаг 1. Согласовать с продактом и разработкой открытые вопросы
Минимум:
- RTL
- master brand
- travel separation
- client launch without release
- theme config storage
- one codebase vs several fronts

---

## Шаг 2. Зафиксировать архитектурный one-pager
Сделать краткий internal doc:
- что можно кастомизировать,
- что нельзя,
- слои токенов,
- validation workflow.

---

## Шаг 3. Собрать `PD DS / Foundations`
Создать:
- `SYS / Primitives`
- `SEM / Tokens`

---

## Шаг 4. Собрать минимальный `Core Components`
Без фанатизма, только критичные.

---

## Шаг 5. Собрать `Marketing Blocks`
Только критичные preset-блоки.

---

## Шаг 6. Собрать `Page Templates`
Минимум 5 key screens.

---

## Шаг 7. Собрать первый client validation file
На реальном клиенте.

---

## Шаг 8. Проверить ещё 2–3 клиента
И только потом допиливать архитектуру.

---

## Шаг 9. Согласовать token-to-code pipeline
После проверки на реальных клиентах.

---

# 24. Краткий проектный промпт для нового чата

Ниже — компактная версия этого README, если нужно быстро продолжить в другом чате.

---

## PROMPT START

Мы проектируем архитектуру дизайн-системы PayDigital для multi-brand white-label storefront platform.

Контекст:
- Есть минимум 2 продукта: digital storefront и travel (eSIM + card).
- Есть много клиентов со своими темами.
- Цель — свести текущие кастомные темы в единый framework.
- Предпочтительная модель: 1 клиент = 1 общая тема на все продукты.
- White-label кастомизация важна, но полный page builder не нужен.
- Разрешено кастомизировать: цвета, шрифты, иконки, иллюстрации, баннеры, контент, включение/выключение разрешённых блоков, preset-hero/marketing variants.
- Нельзя кастомизировать core layout architecture и core UI structure.

Архитектурные решения:
- Делаем 1 платформенную дизайн-систему, не 2 отдельные.
- Нужны слои:
  - system primitives
  - brand tokens
  - semantic tokens
  - core components
  - marketing presets
  - page templates
  - client validation files
- Semantic layer обязателен.
- В core DS modes только light/dark.
- Клиентов нельзя хранить как modes в ядре.
- Каждый клиент живёт в отдельном validation file.
- Компоненты и экраны нельзя detatch.
- Components must use semantic tokens only.

Figma структура:
- `PD DS / Foundations`
- `PD DS / Core Components`
- `PD DS / Marketing Blocks`
- `PD DS / Page Templates`
- `PD Client / [BrandName] / Validation`

Variable collections:
- `SYS / Primitives`
- `BRAND / Mapping`
- `SEM / Tokens`
- optionally `CMP / Optional`

v1 scope:
- tokenized: color, typography family, typography weight, dark theme
- not client-customized yet: spacing, radius
- minimal responsive strategy: typography mobile/desktop + layout paddings/gutters + spacing scale

Client validation file:
- `00 Cover`
- `01 Theme Mapping`
- `02 Assets`
- `03 Light / Core Screens`
- `04 Dark / Core Screens`
- `05 Edge Cases`
- `06 Review Notes`

Open questions to resolve with product/dev:
- RTL in future?
- master brand?
- travel visual separation later?
- can new client launch without release?
- should Strapi store theme config?
- one multi-tenant codebase?
- JSON/theme config/CSS variables pipeline?

Goal of v1:
- 80% screens on shared components
- tokens synced with code without manual hell

## PROMPT END

---

# 25. Финальный вывод

Под текущие вводные правильная стратегия такая:

- **одна DS**
- **одно системное ядро**
- **brand layer отдельно**
- **semantic layer как центр**
- **light/dark only in core**
- **клиенты валидируются отдельно**
- **никакого detatch-ада**
- **никаких client modes в ядре**
- **никакого page-builder chaos**
- **потом перевод в JSON/token pipeline и Next.js**
- **Strapi — в первую очередь для контента и структуры, theme metadata позже и аккуратно**

Это взрослая, масштабируемая и не идиотская модель.  
Она не идеальна навсегда, но для вашего этапа она самая здравая.
