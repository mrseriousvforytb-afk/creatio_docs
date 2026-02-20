# crt.MenuItem

Пункт меню. Не является самостоятельным элементом — всегда дочерний для другого компонента (DataGrid `bulkActions`, AttachmentList `rowToolbarItems`, Button `items`).

---

## Пример

Массовое действие для DataGrid — добавление тега:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "DataGrid_rry62l9_AddTagsBulkAction",
  "values": {
    "type": "crt.MenuItem",
    "caption": "Add tag",
    "icon": "tag-icon",
    "clicked": {
      "request": "crt.AddTagsInRecordsRequest",
      "params": { "dataGridName": "DataGrid_rry62l9" }
    }
  },
  "parentName": "DataGrid_rry62l9",
  "propertyName": "bulkActions",
  "index": 0
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.MenuItem"` | да | — | Тип элемента |
| `caption` | `string` | да | — | Текст пункта меню. `#ResourceString(...)#` или строка |
| `icon` | `string` | нет | — | Идентификатор иконки |
| `clicked` | [`ClickedConfig`](#clicked) | да | — | Действие при клике |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `items` | `MenuItem[]` | нет | — | Вложенное подменю |

### Clicked

Структура `ClickedConfig` — та же, что у [crt.Button](button.md#clicked):

```json
"clicked": {
  "request": "crt.DeleteRecordsRequest",
  "params": { "dataGridName": "DataGrid_rry62l9" }
}
```

---

<details markdown>
<summary><b>Контексты использования</b></summary>

MenuItem размещается в трёх контекстах. Отличается только `parentName` и `propertyName`:

**1. DataGrid — массовые действия (`bulkActions`):**
```json
{
  "parentName": "DataGrid_rry62l9",
  "propertyName": "bulkActions",
  "index": 0
}
```

**2. AttachmentList — действия над строкой (`rowToolbarItems`):**
```json
{
  "parentName": "AttachmentList",
  "propertyName": "rowToolbarItems"
}
```

**3. Button — выпадающее меню (`items`):**
```json
{
  "parentName": "Button_Actions",
  "propertyName": "items",
  "index": 0
}
```

</details>

<details markdown>
<summary><b>Примеры: все типовые массовые действия DataGrid</b></summary>

**Удаление тега:**
```json
{
  "operation": "insert",
  "name": "DataGrid_rry62l9_RemoveTagsBulkAction",
  "values": {
    "type": "crt.MenuItem",
    "caption": "Remove tag",
    "icon": "delete-button-icon",
    "clicked": {
      "request": "crt.RemoveTagsInRecordsRequest",
      "params": { "dataGridName": "DataGrid_rry62l9" }
    }
  },
  "parentName": "DataGrid_rry62l9",
  "propertyName": "bulkActions",
  "index": 1
}
```

**Экспорт в Excel:**
```json
{
  "operation": "insert",
  "name": "DataGrid_rry62l9_ExportToExcelBulkAction",
  "values": {
    "type": "crt.MenuItem",
    "caption": "Export to Excel",
    "icon": "export-button-icon",
    "clicked": {
      "request": "crt.ExportDataGridToExcelRequest",
      "params": { "dataGridName": "DataGrid_rry62l9" }
    }
  },
  "parentName": "DataGrid_rry62l9",
  "propertyName": "bulkActions",
  "index": 2
}
```

**Удаление записей:**
```json
{
  "operation": "insert",
  "name": "DataGrid_rry62l9_DeleteBulkAction",
  "values": {
    "type": "crt.MenuItem",
    "caption": "Delete",
    "icon": "delete-button-icon",
    "clicked": {
      "request": "crt.DeleteRecordsRequest",
      "params": { "dataGridName": "DataGrid_rry62l9" }
    }
  },
  "parentName": "DataGrid_rry62l9",
  "propertyName": "bulkActions",
  "index": 3
}
```

</details>

<details markdown>
<summary><b>Известные request-ы для MenuItem</b></summary>

| Request | Описание | Контекст | Параметры |
|---------|----------|----------|-----------|
| `crt.AddTagsInRecordsRequest` | Добавить тег | DataGrid bulkActions | `{ "dataGridName": "..." }` |
| `crt.RemoveTagsInRecordsRequest` | Удалить тег | DataGrid bulkActions | `{ "dataGridName": "..." }` |
| `crt.ExportDataGridToExcelRequest` | Экспорт в Excel | DataGrid bulkActions | `{ "dataGridName": "..." }` |
| `crt.DeleteRecordsRequest` | Удалить выделенные записи | DataGrid bulkActions | `{ "dataGridName": "..." }` |
| `crt.DeleteRecordRequest` | Удалить одну запись | rowToolbarItems | `{ "recordId": "$current_row.Id", "dataGridName": "..." }` |
| `crt.ViewFileRequest` | Просмотр файла | rowToolbarItems | `{ "Id": "$current_row.Id" }` |

</details>

---

## Частые ошибки

- **Используют MenuItem как самостоятельный элемент** — MenuItem не имеет собственного отображения. Он работает только внутри `bulkActions`, `rowToolbarItems` или `items` другого элемента.
- **Забыли `dataGridName` в params** — большинство request-ов требуют явного указания имени грида. Без него действие не привязывается к конкретному списку.
- **Неправильный `propertyName`** — для DataGrid это `"bulkActions"`, для AttachmentList — `"rowToolbarItems"`, для Button — `"items"`. Путаница приведёт к тому, что MenuItem не отобразится.

---

## См. также

- [crt.Button](button.md) — кнопка с выпадающим меню через `items`
- [crt.AttachmentList](filelist.md) — список файлов с `rowToolbarItems`
