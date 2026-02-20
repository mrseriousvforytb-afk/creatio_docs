# Creatio Freedom UI Documentation — Project Instructions

## Purpose
User-facing documentation for Creatio Freedom UI development.
Target audience: **Creatio integrators and developers** (реальные команды разработки).

This is a separate docs repository. The **library source code** lives in
`C:\git\Current_Projects\Creatio_UI` — never modify files there from this project.

## Language
- Documentation text: **Russian** (the audience is Russian-speaking integrators)
- ALL code examples, property names, type names: **English**
- Creatio-specific terms stay as-is: `viewConfigDiff`, `$context`, `HandlerChainService`, etc.

## Repository Structure

### Crawlers (tools, not content)
```
crawlers/                        # Crawler scripts + their state files
├── crawl-academy.js             # Academy crawler
├── crawl-customerfx.js          # CustomerFX crawler
├── crawl-community.js           # Community crawler (puppeteer-based)
├── academy-urls.json            # Cached URLs for academy
├── academy-progress.json        # Crawl progress for academy
├── community-urls.json          # Cached URLs for community (created on first run)
└── community-progress.json      # Crawl progress for community (created on first run)
```

### Raw Sources (DO NOT MODIFY)
```
sources/                         # All crawled raw data
├── academy/                     # ~130 md files crawled from academy.creatio.com
├── customerfx/                  # ~12 md files crawled from customerfx.com
└── community/                   # Posts crawled from community.creatio.com
```

These are **read-only source data**. The crawlers populate them.
**Never edit files in `sources/` manually.**

### Documentation Output (TO BE CREATED)
```
docs/                           # Final user-facing documentation
├── index.md                    # Table of contents, navigation
├── getting-started/            # Quick start, prerequisites
│   ├── page-structure.md       # What is a Freedom UI page schema
│   └── first-customization.md  # Step-by-step first page edit
│
├── concepts/                   # Core concepts (must understand first)
│   ├── schema-structure.md     # viewConfigDiff, viewModelConfigDiff, modelConfigDiff, handlers
│   ├── attributes.md           # Attribute system, binding types, $context
│   ├── handler-chain.md        # Request → Handler chain, lifecycle (5 stages)
│   ├── data-sources.md         # PDS, EntityDataSource, dependencies, filters
│   └── element-properties.md   # Boolean vs "$Attr" duality, layoutConfig, control binding
│
├── elements/                   # UI element reference (one file per element type)
│   ├── index.md                # Element catalog by category
│   ├── button.md               # crt.Button — full property reference + examples
│   ├── input.md                # crt.Input
│   ├── combobox.md             # crt.ComboBox
│   ├── datetimepicker.md       # crt.DateTimePicker
│   ├── numberinput.md          # crt.NumberInput
│   ├── checkbox.md             # crt.Checkbox
│   ├── datagrid.md             # crt.DataGrid (+ crt.List features)
│   ├── flexcontainer.md        # crt.FlexContainer
│   ├── gridcontainer.md        # crt.GridContainer
│   ├── expansionpanel.md       # crt.ExpansionPanel
│   ├── feed.md                 # crt.Feed
│   ├── tabpanel.md             # crt.TabPanel / crt.TabContainer
│   ├── label.md                # crt.Label
│   ├── filelist.md             # crt.FileList / crt.AttachmentList
│   ├── menuitem.md             # crt.MenuItem
│   └── other.md                # Less common: RichTextEditor, PhoneInput, EmailInput, etc.
│
├── data/                       # Working with data
│   ├── crud-operations.md      # Declarative (SaveDataRequest) + sdk.Model CRUD
│   ├── loading.md              # LoadDataRequest, load types, pagination
│   ├── filters.md              # FilterGroup, sdk.ComparisonType, filter injection
│   └── embedded-models.md      # Non-bound collections for dropdowns
│
├── handlers/                   # Request handling
│   ├── lifecycle.md            # 5-stage lifecycle: Init → Resume → Change → Pause → Destroy
│   ├── common-requests.md      # SaveDataRequest, RunBusinessProcess, OpenPage, ShowDialog, etc.
│   ├── custom-handlers.md      # Writing your own handlers, dispatching via HandlerChainService
│   └── sidebar-requests.md     # CloseSidebar, OpenSidebarRequest, SidebarLoadDataRequest
│
├── services/                   # Platform services
│   ├── requests.md             # Complete request type reference table
│   ├── messages.md             # ServerChannel, WebSocket subscriptions
│   ├── dialogs.md              # ShowDialog, ShowToast, confirmation patterns
│   ├── lookups.md              # OpenSelectionWindow, multi-select lookup
│   ├── files.md                # File download, zip archive
│   ├── rights.md               # checkUserInRole, permission checks
│   └── sys-settings.md         # SysSettingsService, SysValuesService
│
├── converters/                 # Value converters
│   ├── built-in.md             # crt.ToBoolean, InvertBooleanValue, IsEqual, etc.
│   └── custom.md               # Creating custom converters (Angular remote module)
│
├── validators/                 # Field validators
│   ├── built-in.md             # crt.Required, MinLength, MaxLength, Min, Max, EmptyOrWhiteSpace
│   └── custom.md               # Creating custom validators (Angular remote module)
│
├── remote-modules/             # Angular-based extensions
│   ├── overview.md             # What are remote modules, Module Federation, project setup
│   ├── view-elements.md        # @CrtViewElement, @CrtInput, @CrtOutput, @CrtValidationInput
│   ├── designer-integration.md # @CrtInterfaceDesignerItem, toolbar config
│   ├── assets.md               # Fonts, icons, images in remote modules
│   └── localization.md         # @ngx-translate, SysValuesService, localizeMetadata
│
├── patterns/                   # Practical recipes
│   ├── visibility-toggle.md    # Boolean → Attribute conversion, controlling visibility
│   ├── field-dependencies.md   # Reactive field logic via HandleViewModelAttributeChangeRequest
│   ├── datagrid-reload.md      # Reloading grids, injecting filters, selection state
│   ├── dynamic-validators.md   # enable/disableAttributeValidator at runtime
│   ├── save-workflow.md        # Save interception, validation before save, HasUnsavedData
│   └── process-integration.md  # RunBusinessProcess, processRunType, parameterMappings
│
└── reference/                  # Quick-reference tables
    ├── request-types.md        # Complete table of all crt.* request types
    ├── data-value-types.md     # Creatio dataValueType codes (4=Int, 10=Lookup, etc.)
    ├── naming-conventions.md   # Element naming, DataSource naming, attribute naming patterns
    └── keyboard-shortcuts.md   # DateTimePicker keyboard shortcuts
```

## Source Data Map

Each documentation file should be based on **analysis files** from the library repo
and **raw academy/customerfx data** from this repo. Here's the mapping:

### Analysis files (in C:\git\Current_Projects\Creatio_UI)

| Analysis File                         | → Docs Section(s)                                    |
|---------------------------------------|-------------------------------------------------------|
| `PageStructure/analysis.md`           | `concepts/schema-structure.md`, `getting-started/`    |
| `Attributes/analysis.md`             | `concepts/attributes.md`, `concepts/element-properties.md` |
| `Handlers/analysis.md`               | `handlers/*`, `concepts/handler-chain.md`             |
| `Services/analysis.md`               | `services/*`                                          |
| `DataLoading/analysis.md`            | `data/*`, `concepts/data-sources.md`                  |
| `Filters/analysis.md`                | `data/filters.md`                                     |
| `Converters/analysis.md`             | `converters/*`                                        |
| `Validators/analysis.md`             | `validators/*`                                        |
| `RemoteModules/analysis.md`          | `remote-modules/*`                                    |
| `Elements/*/analysis.md` (23 files)  | `elements/*` (one doc per element type)               |
| `CheatSheets/*` (4 files)            | `reference/*`                                         |

### Academy sources (in this repo)

| Academy Directory                                                                          | Key Content                       |
|--------------------------------------------------------------------------------------------|-----------------------------------|
| `sources/academy/platform-customization/freedom-ui/page-customization-basics/`             | Page structure, element config    |
| `sources/academy/platform-customization/freedom-ui/page-customization-basics/references/`  | Element property references       |
| `sources/academy/front-end-development/freedom-ui/remote-module/`                          | Remote modules, converters, validators |
| `sources/academy/front-end-development/freedom-ui/freedom-ui-page/`                        | Page schema, handlers, DevKit    |
| `sources/academy/data-operations-front-end*.md`                                            | CRUD, data loading               |
| `sources/academy/schema-freedomui-references.md`                                           | Schema token comments, structure  |

### CustomerFX sources (in this repo)

| File                                                          | Key Content                               |
|---------------------------------------------------------------|-------------------------------------------|
| `sources/customerfx/detecting-unsaved-data-changes.md`        | HasUnsavedData pattern                    |
| `sources/customerfx/multi-select-lookup.md`                   | Multi-select lookup window                |
| `sources/customerfx/adding-lookup-field-filter.md`            | Lookup filtering                          |
| `sources/customerfx/executing-process-with-collection.md`     | RunBusinessProcess with JSON.stringify    |

## Writing Guidelines

### Documentation Format

Each docs file should follow this structure:
```markdown
# Заголовок на русском

Краткое описание — 1-2 предложения, что это и зачем нужно.

---

## Ключевые концепции

Объяснение основных понятий. Не предполагай, что читатель знаком с внутренней архитектурой.

## Справочник свойств / API

Таблицы свойств, типов, значений по умолчанию.

## Примеры

Реальные JSON/JS примеры из Creatio страниц. Каждый пример должен быть:
- Минимальным (только то, что нужно для демонстрации)
- С комментариями на русском
- С указанием, где этот код размещается (viewConfigDiff, handler, etc.)

## Частые ошибки / Подводные камни

Что может пойти не так, неочевидные моменты.

## См. также

Ссылки на связанные страницы документации.
```

### Style Rules

1. **Пиши для практиков** — интеграторы хотят решить задачу, а не изучать теорию
2. **Начинай с "зачем"** — каждый раздел начинается с проблемы, которую он решает
3. **Код > текст** — если можно показать примером, показывай примером
4. **Один файл = одна тема** — не смешивай Button и DataGrid в одном файле
5. **Примеры из реальных страниц** — берём из analysis.md, где указаны реальные файлы-источники
6. **Таблицы для справочных данных** — свойства, типы, значения всегда в таблицах
7. **Предупреждения выделяй** — используй `> ⚠️ Важно:` для подводных камней
8. **Ссылки между страницами** — используй relative markdown links: `[Атрибуты](../concepts/attributes.md)`
9. **Не дублируй контент** — если что-то описано в `concepts/attributes.md`, в `elements/button.md` дай ссылку
10. **Кодовые примеры** — всегда указывай язык: ```json, ```javascript, ```typescript

### What NOT to Do

- НЕ копируй текст из Academy дословно — переписывай своими словами
- НЕ пиши абстрактную теорию без примеров
- НЕ создавай файлы-заглушки ("TODO: написать позже") — пиши сразу или не создавай
- НЕ модифицируй файлы в `sources/` — это raw source data
- НЕ модифицируй файлы в `C:\git\Current_Projects\Creatio_UI` — это другой проект
- НЕ пиши документацию по Classic UI (только Freedom UI)
- НЕ включай internal implementation details библиотеки Creatio_UI

### Code Examples Format

**viewConfigDiff element:**
```json
// viewConfigDiff — добавление кнопки
{
  "operation": "insert",
  "name": "Button_Save",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(SaveButton_caption)#",
    "color": "primary",
    "clicked": { "request": "crt.SaveNotCloseRequest" }
  },
  "parentName": "ActionButtonsContainer",
  "propertyName": "items",
  "index": 0
}
```

**Handler:**
```javascript
// handlers — обработчик инициализации страницы
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    // Бизнес-логика при открытии страницы
    const status = await request.$context.PDS_Status_abc123;
    request.$context.IsSaveButtonVisible = status?.value === "active-guid";

    // Обязательно вызываем следующий обработчик в цепочке
    return await next?.handle(request);
  }
}
```

**viewModelConfigDiff attribute:**
```json
// viewModelConfigDiff — определение атрибута с валидатором
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "PDS_Name_abc123": {
      "modelConfig": { "path": "PDS.Name" },
      "validators": {
        "crt.Required": { "type": "crt.Required", "params": {} }
      }
    }
  }
}
```

## Verification

Before considering any documentation file complete:
1. All code examples must be syntactically valid JSON/JS/TS
2. All property names must match actual Creatio API (check against analysis files)
3. All cross-references must point to existing files
4. Russian text must be grammatically correct
5. No English-only sections (except code blocks)

## Priority Order for Writing Docs

**Phase 1: Foundation** (write these first)
1. `docs/index.md` — navigation hub
2. `docs/concepts/schema-structure.md` — without this, nothing else makes sense
3. `docs/concepts/attributes.md` — core data binding mechanism
4. `docs/concepts/handler-chain.md` — core logic mechanism

**Phase 2: Element Reference** (most-used elements first)
5. `docs/elements/index.md` — catalog
6. `docs/elements/button.md`
7. `docs/elements/input.md`
8. `docs/elements/combobox.md`
9. `docs/elements/datagrid.md`
10. `docs/elements/flexcontainer.md` + `gridcontainer.md`

**Phase 3: Data & Handlers**
11. `docs/data/crud-operations.md`
12. `docs/data/filters.md`
13. `docs/handlers/lifecycle.md`
14. `docs/handlers/common-requests.md`

**Phase 4: Services & Patterns**
15. `docs/services/requests.md`
16. `docs/services/messages.md`
17. `docs/patterns/visibility-toggle.md`
18. `docs/patterns/field-dependencies.md`

**Phase 5: Advanced**
19. `docs/converters/*`
20. `docs/validators/*`
21. `docs/remote-modules/*`
22. `docs/reference/*`

## Relationships Between Repos

```
Creatio_Docs (THIS REPO)
├── crawlers/              ← Crawler scripts + state files
├── sources/               ← Raw crawled data (read-only)
│   ├── academy/
│   ├── customerfx/
│   └── community/
├── docs/                  ← User-facing documentation (output)
├── node_modules/          ← Dependencies (cheerio, turndown, puppeteer)
└── .claude/               ← Project instructions
                               ↑ reads analysis files from ↓

Creatio_UI (SEPARATE REPO — DO NOT MODIFY)
├── PageStructure/analysis.md
├── Attributes/analysis.md
├── Handlers/analysis.md
├── Services/analysis.md
├── DataLoading/analysis.md
├── Filters/analysis.md
├── Converters/analysis.md
├── Validators/analysis.md
├── RemoteModules/analysis.md
├── Elements/*/analysis.md (23 files)
├── CheatSheets/ (4 files)
└── src/                   ← Library TypeScript source code
```

**Key principle:** Analysis files are the **knowledge base**.
Documentation files are the **user-facing presentation** of that knowledge.
They are NOT copies — docs restructure, explain, and add context that analysis files lack.
