# Прочие элементы

Менее распространённые элементы Freedom UI. Каждый описан кратко — основные свойства и пример.

---

## crt.EmailInput

Специализированное поле ввода email с встроенной валидацией формата.

**Свойства:**

| Свойство | Тип | Обязательное | Описание |
|----------|-----|:---:|---|
| `type` | `"crt.EmailInput"` | да | Тип элемента |
| `label` | `string` | да | Подпись поля |
| `labelPosition` | `"above"` \| `"auto"` | да | Позиция подписи |
| `control` | `string` | да | Привязка данных |
| `visible` | `boolean` \| `"$Attr"` | нет | Видимость |
| `readonly` | `boolean` | нет | Только чтение |
| `placeholder` | `string` | нет | Подсказка |
| `tooltip` | `string` | нет | Tooltip |
| `layoutConfig` | `LayoutConfig` | нет | Позиция в сетке |

**Пример:**
```json
{
  "operation": "insert",
  "name": "EmailInput",
  "values": {
    "type": "crt.EmailInput",
    "label": "$Resources.Strings.Email",
    "labelPosition": "above",
    "control": "$Email",
    "visible": "$IsEmailVisible",
    "readonly": true,
    "placeholder": "#ResourceString(EmailInput_placeholder)#"
  },
  "parentName": "LoginEmailFlexContainer",
  "propertyName": "items",
  "index": 2
}
```

---

## crt.PhoneInput

Специализированное поле ввода телефона с форматированием номера.

**Свойства:**

| Свойство | Тип | Обязательное | Описание |
|----------|-----|:---:|---|
| `type` | `"crt.PhoneInput"` | да | Тип элемента |
| `label` | `string` | да | Подпись поля |
| `labelPosition` | `"above"` | да | Позиция подписи |
| `control` | `string` | да | Привязка данных |
| `visible` | `boolean` | нет | Видимость |
| `placeholder` | `string` | нет | Подсказка |
| `tooltip` | `string` | нет | Tooltip |
| `layoutConfig` | `LayoutConfig` | нет | Позиция в сетке |

**Пример:**
```json
{
  "operation": "insert",
  "name": "MobilePhone",
  "values": {
    "layoutConfig": { "column": 1, "row": 5, "colSpan": 1, "rowSpan": 1 },
    "type": "crt.PhoneInput",
    "label": "$Resources.Strings.StringAttribute_2sfe3yg",
    "labelPosition": "above",
    "control": "$StringAttribute_2sfe3yg",
    "visible": true,
    "placeholder": "#ResourceString(MobilePhone_placeholder)#"
  },
  "parentName": "GridContainer_yko36dq",
  "propertyName": "items",
  "index": 4
}
```

---

## crt.RichTextEditor

Редактор форматированного текста (HTML) с поддержкой вложенных файлов.

**Свойства:**

| Свойство | Тип | Обязательное | Описание |
|----------|-----|:---:|---|
| `type` | `"crt.RichTextEditor"` | да | Тип элемента |
| `label` | `string` | да | Подпись поля |
| `labelPosition` | `"above"` | да | Позиция подписи |
| `control` | `string` | да | Привязка данных |
| `filesStorage` | `FilesStorageConfig` | нет | Конфигурация хранения файлов |
| `visible` | `boolean` | нет | Видимость |
| `readonly` | `boolean` | нет | Только чтение |
| `placeholder` | `string` | нет | Подсказка |
| `toolbarDisplayMode` | `null` | нет | Режим тулбара (`null` = по умолчанию) |
| `layoutConfig` | `LayoutConfig` | нет | Позиция (обычно `colSpan: 4`) |

**Конфигурация filesStorage:**
```json
"filesStorage": {
  "masterRecordColumnValue": "$Id",
  "entitySchemaName": "SysFile",
  "recordColumnName": "RecordId"
}
```

**Пример:**
```json
{
  "operation": "insert",
  "name": "RichTextEditor_ys50lfg",
  "values": {
    "layoutConfig": { "column": 1, "row": 3, "colSpan": 4, "rowSpan": 1 },
    "type": "crt.RichTextEditor",
    "label": "$Resources.Strings.AnnouncementDS_Description_z9qfzmc",
    "labelPosition": "above",
    "control": "$AnnouncementDS_Description_z9qfzmc",
    "filesStorage": {
      "masterRecordColumnValue": "$Id",
      "entitySchemaName": "SysFile",
      "recordColumnName": "RecordId"
    },
    "visible": true,
    "readonly": false,
    "toolbarDisplayMode": null
  },
  "parentName": "GridContainer_MainData",
  "propertyName": "items",
  "index": 6
}
```

> ⚠️ `filesStorage.entitySchemaName` — всегда `"SysFile"` в стандартных страницах. Обычно занимает несколько колонок (`colSpan: 4`).

---

## crt.EntityStageProgressBar

Визуальный индикатор стадий/статусов для DCM-процессов. Показывает текущую стадию и позволяет переходить между ними.

**Свойства:**

| Свойство | Тип | Обязательное | Описание |
|----------|-----|:---:|---|
| `type` | `"crt.EntityStageProgressBar"` | да | Тип элемента |
| `saveOnChange` | `boolean` | да | Автосохранение при смене стадии |
| `askUserToChangeSchema` | `boolean` | да | Подтверждение перед сменой схемы |
| `entityName` | `string` | да | Имя сущности бэкенда |
| `visible` | `boolean` | нет | Видимость |

**Пример:**
```json
{
  "operation": "insert",
  "name": "EntityStageProgressBar_tcihgr0",
  "values": {
    "type": "crt.EntityStageProgressBar",
    "saveOnChange": true,
    "askUserToChangeSchema": true,
    "entityName": "Task",
    "visible": true
  },
  "parentName": "FlexContainer_header",
  "propertyName": "items",
  "index": 0
}
```

> ⚠️ Размещается в шапке страницы. `entityName` должен иметь настроенные стадии в DCM.

---

## crt.TagSelect

Компонент выбора тегов для записей.

**Свойства:**

| Свойство | Тип | Обязательное | Описание |
|----------|-----|:---:|---|
| `type` | `"crt.TagSelect"` | да | Тип элемента |
| `tagInRecordSourceSchemaName` | `string` | да | Сущность связи тег-запись |
| `recordId` | `"$Id"` | да | Ссылка на текущую запись |
| `visible` | `boolean` | нет | Видимость |
| `label` | `string` | нет | Подпись |

**Пример:**
```json
{
  "operation": "insert",
  "name": "TagSelect_abc123",
  "values": {
    "type": "crt.TagSelect",
    "tagInRecordSourceSchemaName": "TagInRecord",
    "visible": true,
    "recordId": "$Id"
  }
}
```

---

## crt.ApprovalList

Специализированная таблица для визирования (approval workflow). Расширяет паттерн DataGrid свойствами визирования.

**Свойства:**

| Свойство | Тип | Описание |
|----------|-----|---|
| `type` | `"crt.ApprovalList"` | Тип элемента |
| `masterRecordColumnValue` | `"$Id"` | Ссылка на родительскую запись |
| `recordColumnName` | `"RecordId"` | Поле FK в сущности визирования |
| `entityName` | `string` | Основная сущность (напр. `"Lead"`) |
| `approvalEntityName` | `string` | Сущность визирования (напр. `"SysApproval"`) |
| `items` | `"$ApprovalList_hash"` | Привязка к коллекции |
| `primaryColumnName` | `string` | Уникальный идентификатор колонки |
| `columns` | `Column[]` | Определения колонок (как в DataGrid) |
| `features` | `object` | Обычно `{ editable: false }` |
| `visible` | `boolean` | Видимость |

**Типичные колонки:** VisaOwner (визирующий), Objective (цель), Comment, SetBy (кем установлено), SetDate, Status.

**Пример:**
```json
{
  "operation": "insert",
  "name": "ApprovalList_pb69gpr",
  "values": {
    "type": "crt.ApprovalList",
    "masterRecordColumnValue": "$Id",
    "recordColumnName": "RecordId",
    "features": { "editable": false },
    "entityName": "Lead",
    "approvalEntityName": "SysApproval",
    "items": "$ApprovalList_pb69gpr",
    "primaryColumnName": "ApprovalList_pb69gprDS_Id",
    "columns": [
      { "code": "ApprovalList_pb69gprDS_VisaOwner", "dataValueType": 10, "width": 175 },
      { "code": "ApprovalList_pb69gprDS_Objective", "dataValueType": 30, "width": 222 },
      { "code": "ApprovalList_pb69gprDS_Status", "dataValueType": 10, "width": 125 }
    ]
  },
  "parentName": "GridContainer_Approval",
  "propertyName": "items",
  "index": 1
}
```

<details markdown>
<summary><b>Отличия от DataGrid</b></summary>

| Аспект | DataGrid | ApprovalList |
|--------|----------|-------------|
| Дополнительные свойства | — | `masterRecordColumnValue`, `recordColumnName`, `entityName`, `approvalEntityName` |
| Редактирование | Настраивается | Обычно `{ editable: false }` |
| Bulk actions | Поддерживаются | Не используются |
| Выделение строк | Поддерживается | Не используется |

</details>

---

## crt.Summaries / crt.SummaryItem

Компоненты агрегации — отображают количество, суммы и другие агрегированные значения. Привязываются к DataGrid через `_designOptions.modelName`.

**Свойства Summaries:**

| Свойство | Тип | Описание |
|----------|-----|---|
| `type` | `"crt.Summaries"` | Тип элемента |
| `items` | `[]` | Дочерние SummaryItem |
| `visible` | `boolean` | Видимость |
| `expanded` | `"$Attr"` \| `boolean` | Развёрнутость |
| `_designOptions` | `{ modelName: string }` | Привязка к DataSource |

**Функции агрегации:** `"count"`, `"sum"`, `"avg"`, `"min"`, `"max"`.

**Пример:**
```json
{
  "operation": "insert",
  "name": "Summaries_9cc315d",
  "values": {
    "type": "crt.Summaries",
    "items": [],
    "visible": true,
    "_designOptions": { "modelName": "DataGrid_vmojr1oDS" }
  },
  "parentName": "FlexContainer_TopArea",
  "propertyName": "items",
  "index": 0
}
```

---

## crt.QuickFilter / crt.SearchFilter

Фильтры для списковых страниц.

**QuickFilter** — toggle/dropdown фильтры с конвертером `crt.QuickFilterAttributeConverter`.

**SearchFilter** — текстовый поиск. Предустановлен на списковых страницах, обычно перемещается через `move`.

**Пример QuickFilter:**
```json
{
  "operation": "insert",
  "name": "QuickFilter_ShowAll",
  "values": {
    "type": "crt.QuickFilter",
    "config": {
      "caption": "#ResourceString(QuickFilter_ShowAll_caption)#",
      "hint": "",
      "defaultValue": false
    },
    "expose": [{
      "attribute": "QuickFilter_ShowAll_Items",
      "converters": [{
        "converter": "crt.QuickFilterAttributeConverter",
        "args": [{
          "target": { "viewAttributeName": "Items" },
          "customFilter": {
            "filterType": "exclude",
            "filters": [{ "columnOperand": "Status", "operationType": 4 }]
          }
        }]
      }]
    }]
  },
  "parentName": "MainFilterContainer",
  "propertyName": "items",
  "index": 0
}
```

<details markdown>
<summary><b>Типы фильтров QuickFilter</b></summary>

| Тип | Параметр target | Описание |
|-----|----------------|----------|
| Toggle (boolean) | `viewAttributeName` + `customFilter` | Включить/выключить фильтр |
| Диапазон дат | `filterColumnStart` | Фильтр по дате |
| По колонке | `filterColumn` | Фильтр по конкретному полю |

</details>

**Перемещение SearchFilter:**
```json
{
  "operation": "move",
  "name": "SearchFilter",
  "parentName": "RightFilterContainer",
  "propertyName": "items",
  "index": 0
}
```
