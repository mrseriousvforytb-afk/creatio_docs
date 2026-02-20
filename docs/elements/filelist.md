# crt.AttachmentList

Список прикреплённых файлов. Предустановлен на FormPage — добавлять не нужно, только настраивать через `merge`.

---

## Пример

Настройка колонок и действий над строками:

**viewConfigDiff:**
```json
{
  "operation": "merge",
  "name": "AttachmentList",
  "values": {
    "columns": [
      {
        "id": "2dcbae86-956f-45df-b80c-0091145be7f4",
        "code": "AttachmentListDS_Name",
        "caption": "#ResourceString(AttachmentListDS_Name)#",
        "dataValueType": 28,
        "width": 200
      }
    ],
    "rowToolbarItems": [
      {
        "type": "crt.MenuItem",
        "caption": "#ResourceString(Preview)#",
        "icon": "open-button-icon",
        "clicked": {
          "request": "crt.ViewFileRequest",
          "params": { "Id": "$current_row.Id" }
        }
      },
      {
        "type": "crt.MenuItem",
        "caption": "#ResourceString(Delete)#",
        "icon": "delete-button-icon",
        "clicked": {
          "request": "crt.DeleteRecordRequest",
          "params": {
            "recordId": "$current_row.Id",
            "dataGridName": "AttachmentList"
          }
        }
      }
    ]
  }
}
```

---

## Свойства

| Свойство | Тип | Описание |
|----------|-----|----------|
| `columns` | `Column[]` | Колонки списка файлов. Формат аналогичен [DataGrid](datagrid.md) |
| `rowToolbarItems` | [`MenuItem[]`](menuitem.md) | Действия в контекстном меню строки |

### Column

| Свойство | Тип | Описание |
|----------|-----|----------|
| `id` | `string` | UUID колонки |
| `code` | `string` | Код колонки — `AttachmentListDS_*` или `PDS_*` |
| `caption` | `string` | Заголовок. `#ResourceString(...)#` |
| `dataValueType` | `number` | Тип данных (28 = Text, 7 = Float, 10 = Lookup, 4 = Integer) |
| `width` | `number` | Ширина колонки в пикселях |

### rowToolbarItems

Каждый элемент массива — [crt.MenuItem](menuitem.md). Для ссылки на текущую строку используйте `$current_row.Id`.

---

<details markdown>
<summary><b>Пример: несколько колонок с метаданными файлов</b></summary>

```json
{
  "operation": "merge",
  "name": "AttachmentList",
  "values": {
    "columns": [
      {
        "code": "PDS_FileName",
        "caption": "#ResourceString(PDS_FileName)#",
        "dataValueType": 28
      },
      {
        "code": "PDS_FileSize",
        "caption": "#ResourceString(PDS_FileSize)#",
        "dataValueType": 7
      },
      {
        "code": "PDS_FileOwner",
        "caption": "#ResourceString(PDS_FileOwner)#",
        "dataValueType": 10
      },
      {
        "code": "PDS_FileDate",
        "caption": "#ResourceString(PDS_FileDate)#",
        "dataValueType": 4
      }
    ]
  }
}
```

</details>

<details markdown>
<summary><b>Известные request-ы для rowToolbarItems</b></summary>

| Request | Описание | Параметры |
|---------|----------|-----------|
| `crt.ViewFileRequest` | Просмотр файла | `{ "Id": "$current_row.Id" }` |
| `crt.DeleteRecordRequest` | Удаление записи | `{ "recordId": "$current_row.Id", "dataGridName": "AttachmentList" }` |

</details>

---

## Частые ошибки

- **Используют `insert` вместо `merge`** — AttachmentList уже существует на FormPage. Операция `insert` создаст дубликат или ошибку. Всегда используйте `"operation": "merge"`.
- **Забыли `dataGridName` в `DeleteRecordRequest`** — без указания имени грида платформа не знает, из какого списка удалять запись.
- **Путают `$current_row.Id` с `$Id`** — в контексте rowToolbarItems доступ к текущей строке только через паттерн `$current_row.*`.

---

## См. также

- [crt.MenuItem](menuitem.md) — элементы меню для `rowToolbarItems`
- [crt.Button](button.md) — `clicked` и `ClickedConfig`
