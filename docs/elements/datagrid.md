# crt.DataGrid

Таблица данных (грид/реестр) для отображения и редактирования коллекций записей связанной сущности. Один из самых сложных элементов Freedom UI -- встречается в **196+ схемах**.

`crt.DataGrid` используется как деталь на форме записи. Для реестра раздела (list page) платформа использует `crt.List`, который разделяет ту же структуру `features`.

---

## Пример

Редактируемый грид с мультивыбором, привязкой к DataSource и кнопками действий:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "DataGrid_Claims",
  "values": {
    "type": "crt.DataGrid",
    "items": "$DataGrid_zc3vbv7",
    "primaryColumnName": "DataGrid_zc3vbv7DS_Id",
    "columns": [
      {
        "id": "f1a2b3c4-d5e6-7890-abcd-ef1234567890",
        "code": "DataGrid_zc3vbv7DS_Title",
        "caption": "#ResourceString(DataGrid_zc3vbv7DS_Title)#",
        "dataValueType": 28
      },
      {
        "id": "a1b2c3d4-e5f6-7890-abcd-ef0987654321",
        "code": "DataGrid_zc3vbv7DS_Merchant",
        "caption": "#ResourceString(DataGrid_zc3vbv7DS_Merchant)#",
        "dataValueType": 10,
        "width": 125
      },
      {
        "id": "b2c3d4e5-f6a7-8901-bcde-f12345678901",
        "code": "DataGrid_zc3vbv7DS_LossSum",
        "caption": "#ResourceString(DataGrid_zc3vbv7DS_LossSum)#",
        "dataValueType": 32,
        "width": 158
      }
    ],
    "features": {
      "rows": {
        "selection": { "enable": true, "multiple": true },
        "numeration": true,
        "toolbar": false
      },
      "columns": { "adding": false },
      "editable": {
        "enable": true,
        "itemsCreation": false,
        "floatingEditPanel": false
      }
    },
    "visible": true,
    "fitContent": true,
    "placeholder": false,
    "activeRow": "$DataGrid_zc3vbv7_ActiveRow",
    "selectionState": "$DataGrid_zc3vbv7_SelectionState",
    "_selectionOptions": { "attribute": "DataGrid_zc3vbv7_SelectionState" },
    "layoutConfig": { "max-height": "843" },
    "bulkActions": []
  },
  "parentName": "FlexContainer_DataGridDetails",
  "propertyName": "items",
  "index": 1
}
```

**modelConfigDiff** -- DataSource и зависимость от родительской записи:
```json
[
  {
    "operation": "merge",
    "path": ["dataSources"],
    "values": {
      "DataGrid_zc3vbv7DS": {
        "type": "crt.EntityDataSource",
        "scope": "viewElement",
        "config": {
          "entitySchemaName": "Claim",
          "attributes": {
            "Title": { "path": "Title" },
            "Merchant": { "path": "Merchant" },
            "LossSum": { "path": "LossSum" }
          }
        }
      }
    }
  },
  {
    "operation": "merge",
    "path": ["dependencies"],
    "values": {
      "DataGrid_zc3vbv7DS": [
        {
          "attributePath": "Case",
          "relationPath": "PDS.Id"
        }
      ]
    }
  }
]
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.DataGrid"` | да | -- | Тип элемента |
| `items` | `"$DataGrid_<hash>"` | да | -- | Привязка к атрибуту коллекции. Имя атрибута совпадает с именем элемента |
| `primaryColumnName` | `string` | да | -- | Колонка-идентификатор записи. Формат: `DataGrid_<hash>DS_Id` |
| `columns` | [`Column[]`](#определение-колонки) | да | -- | Массив определений колонок |
| `features` | [`FeaturesConfig`](#features) | нет | см. ниже | Конфигурация строк, колонок, редактирования |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `fitContent` | `boolean` | нет | `false` | Автоподбор высоты под содержимое |
| `placeholder` | `boolean` | нет | `true` | Показывать заглушку при пустом гриде |
| `activeRow` | `"$DataGrid_<hash>_ActiveRow"` | нет | -- | Привязка к атрибуту активной строки |
| `selectionState` | `"$DataGrid_<hash>_SelectionState"` | нет | -- | Привязка к атрибуту состояния выделения |
| `_selectionOptions` | `{ attribute: string }` | нет | -- | Ссылка на атрибут выделения (дублирует `selectionState`) |
| `bulkActions` | `MenuItem[]` | нет | `[]` | Массовые действия над выбранными записями. См. [Bulk Actions](#bulk-actions) |
| `layoutConfig` | `{ "max-height"?: string }` | нет | -- | Ограничение высоты грида в пикселях |

---

## Определение колонки

Каждый элемент массива `columns` описывает одну колонку грида:

| Свойство | Тип | Обязательное | Описание |
|----------|-----|:---:|---|
| `id` | `string (UUID)` | нет | Уникальный идентификатор колонки |
| `code` | `string` | да | Код поля. Формат: `DataGrid_<hash>DS_<FieldName>` |
| `caption` | `string` | нет | Заголовок. `#ResourceString(DataGrid_<hash>DS_<Field>)#` |
| `dataValueType` | `number` | да | Код типа данных Creatio. См. [Типы данных](#типы-данных-datavaluetype) |
| `width` | `number` | нет | Ширина колонки в пикселях. Если не задана -- автоматически |
| `path` | `string` | нет | Путь для вложенных атрибутов (например, lookup-поля) |

**Пример колонки:**
```json
{
  "id": "0421ad17-a170-7de0-2db1-cc331fc38087",
  "code": "DataGrid_rry62l9DS_IsPush",
  "caption": "#ResourceString(DataGrid_rry62l9DS_IsPush)#",
  "dataValueType": 12,
  "width": 330
}
```

<details markdown>
<summary><b>Типы данных (dataValueType)</b></summary>

| Код | Тип | Описание |
|-----|-----|----------|
| 4 | Integer | Целое число |
| 7 | Date | Дата / Дата и время |
| 10 | Lookup | Справочник (FK) |
| 12 | Boolean | Логическое (чекбокс) |
| 28 | ShortText | Короткий текст |
| 30 | LongText | Длинный / многострочный текст |
| 32 | Decimal | Дробное число / Деньги |
| 43 | Blob | GUID / Бинарные данные |

</details>

---

## Features

Объект `features` управляет поведением заголовка, ячеек, строк, колонок и редактирования. Все свойства опциональны -- если `features` не задан, используются значения по умолчанию.

> `crt.DataGrid` и `crt.List` используют одну и ту же структуру `features`.

```
features: {
  header:   { ... },   // Заголовок грида
  cells:    { ... },   // Ячейки
  rows:     { ... },   // Строки
  columns:  { ... },   // Колонки
  editable: { ... }    // Редактирование
}
```

### header

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `visible` | `boolean` | `true` | Показывать заголовок грида |

### cells

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `selection` | `boolean` | `true` | Подсветка выбранной ячейки. При `true` можно копировать текст из ячейки и редактировать значение (если `editable.enable = true`) |

### rows

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `selection` | `boolean` \| `object` | `false` | Выделение строк. См. [Конфигурация выделения](#конфигурация-выделения-строк) |
| `numeration` | `boolean` | `true` | Автонумерация строк |
| `toolbar` | `boolean` | `true` | Показывать тулбар строки |
| `padding` | `object` | -- | Отступы: `{ top, right, bottom, left }` |

<details markdown>
<summary><b>Конфигурация выделения строк</b></summary>

Свойство `rows.selection` принимает `boolean` или объект с тремя полями:

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `enable` | `boolean` | `false` | Включить выделение строк |
| `multiple` | `boolean` | = `enable` | Множественный выбор |
| `selectAll` | `boolean` | = `multiple` | Чекбокс "Выбрать все" в заголовке |

Все четыре варианта ниже эквивалентны (включают выделение с мультивыбором и "Выбрать все"):

```json
"selection": true
```
```json
"selection": { "enable": true }
```
```json
"selection": { "enable": true, "multiple": true }
```
```json
"selection": { "enable": true, "multiple": true, "selectAll": true }
```

**Только одиночный выбор** (без чекбокса "Выбрать все"):
```json
"selection": { "enable": true, "multiple": false }
```

</details>

### columns

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `adding` | `boolean` | `true` | Добавление колонок в грид / отображение действия "Скрыть" в меню колонки |
| `dragAndDrop` | `boolean` | `true` | Изменение порядка колонок перетаскиванием |
| `resizing` | `boolean` | `true` | Изменение ширины колонок |
| `sorting` | `boolean` | `true` | Сортировка по клику на заголовок |
| `toolbar` | `boolean` | `true` | Меню действий колонки (Закрепить, Скрыть, Свойства) |

### editable

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `enable` | `boolean` | `true` | Включить редактирование в гриде |
| `itemsCreation` | `boolean` | = `enable` | Добавление новых записей через грид (Shift+Enter) |
| `floatingEditPanel` | `boolean` | -- | Плавающая панель редактирования |

> Редактирование требует, чтобы `cells.selection` был `true`.

<details markdown>
<summary><b>Пример: только просмотр (без редактирования)</b></summary>

```json
"features": {
  "rows": { "selection": false, "numeration": false, "toolbar": false },
  "columns": { "adding": false },
  "editable": { "enable": false, "itemsCreation": false }
}
```

</details>

<details markdown>
<summary><b>Пример: редактирование без добавления записей</b></summary>

```json
"features": {
  "rows": { "selection": false, "numeration": false, "toolbar": false },
  "columns": { "adding": false },
  "editable": {
    "enable": true,
    "itemsCreation": false,
    "floatingEditPanel": false
  }
}
```

</details>

---

## DataSource

Каждый DataGrid требует определение DataSource в секции `modelConfigDiff`. DataSource связывает грид с серверной сущностью (таблицей).

### Определение DataSource

```json
{
  "operation": "merge",
  "path": ["dataSources"],
  "values": {
    "DataGrid_rry62l9DS": {
      "type": "crt.EntityDataSource",
      "scope": "viewElement",
      "config": {
        "entitySchemaName": "NotificationSetting",
        "attributes": {
          "Contact": { "path": "Contact" },
          "IsPush": { "path": "IsPush" },
          "IsEmail": { "path": "IsEmail" }
        }
      }
    }
  }
}
```

### Свойства DataSource

| Свойство | Значение | Описание |
|----------|----------|----------|
| `type` | `"crt.EntityDataSource"` | Всегда `EntityDataSource` для грида |
| `scope` | `"viewElement"` | Грид имеет собственный DataSource (не общий с PDS страницы). Для основного реестра раздела используется `"page"` |
| `config.entitySchemaName` | `string` | Имя серверной сущности (объекта) |
| `config.attributes` | `Record<string, {path}>` | Маппинг атрибутов грида на колонки сущности |

> Имя DataSource строится по шаблону: имя элемента + суффикс `DS`. Например: `DataGrid_rry62l9` -> `DataGrid_rry62l9DS`.

---

## Dependencies (связь с родительской записью)

Секция `dependencies` в `modelConfigDiff` создаёт фильтр, связывающий DataGrid с основной записью страницы. Без неё грид загрузит **все** записи сущности.

```json
{
  "operation": "merge",
  "path": ["dependencies"],
  "values": {
    "DataGrid_6p4wtcvDS": [
      {
        "attributePath": "ParentTask",
        "relationPath": "PDS.Id"
      }
    ]
  }
}
```

| Свойство | Описание |
|----------|----------|
| `attributePath` | FK-поле в дочерней сущности (та, что в гриде) |
| `relationPath` | Ссылка на ID родительской записи. `"PDS.Id"` -- ID записи текущей страницы |

**Результат:** платформа автоматически добавляет фильтр `WHERE Claim.ParentTask = <текущий Id страницы>` при загрузке данных грида.

> Ключ в объекте `dependencies` -- имя DataSource грида (с суффиксом `DS`).

---

## Bulk Actions

Массовые действия отображаются при выделении строк. Определяются как дочерние элементы `crt.MenuItem` внутри свойства `bulkActions` грида.

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "DataGrid_rry62l9_ExportBulkAction",
  "values": {
    "type": "crt.MenuItem",
    "caption": "Export to Excel",
    "icon": "export-button-icon",
    "clicked": {
      "request": "crt.ExportDataGridToExcelRequest",
      "params": {
        "dataSourceName": "DataGrid_rry62l9DS",
        "filters": "$DataGrid_rry62l9 | crt.ToCollectionFilters : 'DataGrid_rry62l9' : $DataGrid_rry62l9_SelectionState | crt.SkipIfSelectionEmpty : $DataGrid_rry62l9_SelectionState"
      }
    }
  },
  "parentName": "DataGrid_MyPersonalSettings",
  "propertyName": "bulkActions",
  "index": 0
}
```

### Встроенные request-ы массовых действий

| Request | Описание |
|---------|----------|
| `crt.AddTagsInRecordsRequest` | Добавить теги к выбранным записям |
| `crt.RemoveTagsInRecordsRequest` | Удалить теги у выбранных записей |
| `crt.ExportDataGridToExcelRequest` | Экспорт в Excel |
| `crt.DeleteRecordsRequest` | Удалить выбранные записи |

> Параметр `filters` в bulk actions использует конвейер конвертеров: `crt.ToCollectionFilters` формирует фильтр по выделению, а `crt.SkipIfSelectionEmpty` предотвращает выполнение при пустом выделении.

---

## Handler-паттерны

### Перезагрузка грида

Используйте `sdk.HandlerChainService` для программной перезагрузки данных грида:

```javascript
{
  request: "crt.HandleViewModelAttributeChangeRequest",
  handler: async (request, next) => {
    if (request.attributeName === "Id") {
      const handlerChain = sdk.HandlerChainService.instance;
      await handlerChain.process({
        type: "crt.LoadDataRequest",
        $context: request.$context,
        config: {
          loadType: "reload",
          useLastLoadParameters: true
        },
        dataSourceName: "DataGrid_rry62l9DS"
      });
    }
    return await next?.handle(request);
  }
}
```

Ключевые параметры `crt.LoadDataRequest`:

| Параметр | Значение | Описание |
|----------|----------|----------|
| `dataSourceName` | `"DataGrid_<hash>DS"` | Имя DataSource грида |
| `config.loadType` | `"reload"` | Полная перезагрузка данных |
| `config.useLastLoadParameters` | `true` | Сохранить текущие фильтры, сортировку, пагинацию |

<details markdown>
<summary><b>Перезагрузка по серверному сообщению (ServerChannel)</b></summary>

Подписка на WebSocket-сообщения для динамической перезагрузки:

```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    // Подписка на серверные сообщения
    Terrasoft.ServerChannel.on("message", async (event, message) => {
      if (message.Header.Sender === "ReloadSchema") {
        const bodyData = Ext.decode(message.Body);
        const handlerChain = sdk.HandlerChainService.instance;
        await handlerChain.process({
          type: "crt.LoadDataRequest",
          $context: request.$context,
          config: {
            loadType: "reload",
            useLastLoadParameters: true
          },
          dataSourceName: bodyData.schemaName
        });
      }
    });
    return await next?.handle(request);
  }
}
```

</details>

### Инъекция фильтров

Перехват `crt.LoadDataRequest` для добавления динамических фильтров перед загрузкой данных:

```javascript
{
  request: "crt.LoadDataRequest",
  handler: async (request, next) => {
    if (request.dataSourceName === "DataGrid_abc123DS") {
      // Получаем значение lookup-поля со страницы
      const account = await request.$context.LookupAttribute_j2wjc1n;

      const filterGroup = new sdk.FilterGroup();

      if (account) {
        // Фильтр по связанной сущности
        await filterGroup.addSchemaColumnFilterWithParameter(
          sdk.ComparisonType.Equal,
          "[AccountRelation:LegalEntity:Id].Merchant",
          account.value
        );
      } else {
        // "Пустой" фильтр -- ничего не вернёт
        await filterGroup.addSchemaColumnFilterWithParameter(
          sdk.ComparisonType.Equal,
          "Id",
          "00000000-0000-0000-0000-000000000000"
        );
      }

      // Оборачиваем FilterGroup для передачи в параметры
      const filterGroupWrap = Object.assign({}, filterGroup);
      filterGroupWrap.items = filterGroup.items;
      request.parameters.push({
        type: "filter",
        value: filterGroupWrap
      });
    }
    return await next?.handle(request);
  }
}
```

> Фильтр с нулевым GUID (`00000000-...`) -- стандартный приём для отображения пустого грида, когда условие фильтрации не выполнено.

### Чтение состояния выделения

Доступ к выделенным строкам из handler-а (например, для кастомной кнопки):

```javascript
{
  request: "usr.ProcessSelected",
  handler: async (request, next) => {
    // Получаем объект состояния выделения
    const selection = await request.$context.DataGrid_bqzet1p_SelectionState;

    // selection.selected -- массив ID выделенных записей
    const httpService = new sdk.HttpClientService();
    await httpService.post(
      "rest/CustomService/ProcessRecords",
      selection.selected
    );

    return await next?.handle(request);
  }
}
```

### Чтение активной строки

```javascript
// Получить ID активной (текущей) строки
const activeRow = await request.$context.DataGrid_hash_ActiveRow;
```

### Отслеживание изменений в гриде

Реакция на добавление/удаление строк через `HandleViewModelAttributeChangeRequest`:

```javascript
{
  request: "crt.HandleViewModelAttributeChangeRequest",
  handler: async (request, next) => {
    if (request.attributeName === "DataGrid_kpu5uad") {
      const rows = await request.$context.DataGrid_kpu5uad;

      // Проверяем, есть ли записи
      if (rows && rows[0]) {
        request.$context.HasLinkedRecords = true;
      } else {
        request.$context.HasLinkedRecords = false;
      }

      // Доступ к атрибутам строк грида
      const merchantIds = rows.map(
        row => row.attributes.DataGrid_kpu5uadDS_Merchant?.value
      );

      // Загрузка связанных данных через sdk.Model
      if (merchantIds.length > 0) {
        const model = await sdk.Model.create("Account");
        const filter = new sdk.FilterGroup();
        await filter.addSchemaColumnInFilterWithParameters(
          sdk.ComparisonType.Equal, "Id", merchantIds
        );
        const results = await model.load({
          attributes: ["Id", "SalesManager.Contact"],
          parameters: [
            { type: sdk.ModelParameterType.Filter, value: filter }
          ]
        });
      }
    }
    return await next?.handle(request);
  }
}
```

> Атрибут грида (например, `DataGrid_kpu5uad`) -- это массив объектов-строк. У каждой строки есть `.attributes` с данными колонок.

---

## Соглашения об именовании

| Сущность | Паттерн | Пример |
|----------|---------|--------|
| Имя элемента | `DataGrid_<hash>` | `DataGrid_rry62l9` |
| DataSource | `DataGrid_<hash>DS` | `DataGrid_rry62l9DS` |
| Primary column | `DataGrid_<hash>DS_Id` | `DataGrid_rry62l9DS_Id` |
| Код колонки | `DataGrid_<hash>DS_<Field>` | `DataGrid_rry62l9DS_IsPush` |
| Атрибут активной строки | `DataGrid_<hash>_ActiveRow` | `DataGrid_rry62l9_ActiveRow` |
| Атрибут выделения | `DataGrid_<hash>_SelectionState` | `DataGrid_rry62l9_SelectionState` |

> `<hash>` -- автоматически сгенерированный идентификатор. При создании через дизайнер Freedom UI генерируется автоматически.

---

## Частые ошибки

- **Забыли `dependencies`** -- грид загружает ВСЕ записи сущности вместо записей, связанных с текущей записью страницы.

- **`next?.handle(request)` не вызван** в handler-е `crt.LoadDataRequest` -- данные не загрузятся. Фильтр-хендлер должен модифицировать `request.parameters` и обязательно передать управление дальше.

- **Несовпадение имён** DataSource и элемента. Имя DataSource = имя элемента + `DS`. Если в `dataSourceName` указано имя без суффикса `DS` -- reload не сработает.

- **`editable.enable: true` без `cells.selection: true`** -- редактирование ячеек не будет работать. Для редактирования необходима возможность выделения ячеек.

- **Литерал вместо привязки `"$Attr"`** для `visible` -- если задано `"visible": true`, значение нельзя изменить программно. Используйте `"visible": "$IsGridVisible"` и управляйте через атрибут.

- **Фильтр в bulk actions** -- если скопировали `filters` из другого грида и не заменили `DataGrid_<hash>` и `DataGrid_<hash>_SelectionState` на актуальные значения, массовые действия будут работать с неправильным набором записей.

- **Перезагрузка через `handlerChain.process` без `$context`** -- если не передать `$context` в объект request-а, `LoadDataRequest` не найдёт нужный DataSource.

---

## См. также

- [Атрибуты и привязка](../page/viewmodel-config.md) -- `$DataGrid_<hash>`, `$DataGrid_<hash>_ActiveRow`
- [Обработчики (handlers)](../page/handlers.md) -- `crt.LoadDataRequest`, `HandleViewModelAttributeChangeRequest`
- [crt.Button](button.md) -- кнопки действий для грида (Add, Refresh)
- [Фильтры](../page/filters.md) -- `sdk.FilterGroup`, `sdk.ComparisonType`
