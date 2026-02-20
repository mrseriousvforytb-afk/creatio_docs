# crt.ComboBox

Выпадающий список (поле-справочник) для выбора значения из связанной сущности. Встречается на всех FormPage-страницах. Всегда сопровождается дочерним элементом `crt.ComboboxSearchTextAction` для добавления записей прямо из поля.

---

## Пример

ComboBox с кнопкой "Добавить запись" в выпадающем списке:

**viewConfigDiff** -- сам ComboBox:
```json
{
  "operation": "insert",
  "name": "ComboBox_jmwpcxh",
  "values": {
    "layoutConfig": { "column": 1, "row": 3, "colSpan": 1, "rowSpan": 1 },
    "type": "crt.ComboBox",
    "label": "$Resources.Strings.LookupAttribute_gzza1dr",
    "labelPosition": "auto",
    "control": "$LookupAttribute_gzza1dr",
    "listActions": [],
    "showValueAsLink": true,
    "controlActions": [],
    "visible": true,
    "readonly": false,
    "placeholder": "",
    "tooltip": ""
  },
  "parentName": "SideAreaProfileContainer",
  "propertyName": "items",
  "index": 2
}
```

**viewConfigDiff** -- дочернее действие (insert отдельной операцией):
```json
{
  "operation": "insert",
  "name": "addRecord_ghwrzaa",
  "values": {
    "code": "addRecord",
    "type": "crt.ComboboxSearchTextAction",
    "icon": "combobox-add-new",
    "caption": "#ResourceString(addRecord_ghwrzaa_caption)#",
    "clicked": {
      "request": "crt.CreateRecordFromLookupRequest",
      "params": {}
    }
  },
  "parentName": "ComboBox_jmwpcxh",
  "propertyName": "listActions",
  "index": 0
}
```

**viewModelConfigDiff** -- привязка атрибута к источнику данных:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "LookupAttribute_gzza1dr": {
      "modelConfig": {
        "path": "PDS.UsrMerchantType"
      }
    }
  }
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.ComboBox"` | да | -- | Тип элемента |
| `label` | `string` | да | -- | Подпись поля. `$Resources.Strings.X` или [`#ResourceString(...)#`](../page/view-config.md) |
| `labelPosition` | [`LabelPosition`](#labelposition) | да | -- | Расположение подписи |
| `control` | `string` | да | -- | Привязка к данным: `"$AttrName"`. См. [Привязка атрибутов](#привязка-атрибутов) |
| `listActions` | `ListAction[]` | да | `[]` | Действия в выпадающем списке. Заполняется через отдельные insert-операции |
| `showValueAsLink` | `boolean` | да | -- | Отображать значение как кликабельную ссылку на связанную запись |
| `controlActions` | `any[]` | да | `[]` | Действия контрола. Всегда пустой массив `[]` |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) и [конвертеры](../page/converters.md) |
| `readonly` | `boolean` | нет | `false` | Только чтение -- пользователь не может менять значение |
| `disabled` | `boolean` | нет | `false` | Неактивное состояние |
| `placeholder` | `string` | нет | `""` | Текст-подсказка при пустом значении |
| `tooltip` | `string` | нет | `""` | Всплывающая подсказка при наведении |
| `isAddAllowed` | `boolean` | нет | -- | Разрешить добавление новых записей из выпадающего списка |
| `ariaLabel` | `string` | нет | = `label` | Значение `aria-label` для доступности |
| `debounceTime` | `number` | нет | `500` | Задержка поиска в мс при вводе текста |
| `layoutConfig` | `LayoutConfig` | нет | -- | Позиция в контейнере. Поддерживает `max-width` |

<details markdown>
<summary><b>labelPosition</b></summary>

| Значение | Описание |
|----------|----------|
| `"auto"` | Платформа выбирает расположение автоматически |
| `"above"` | Подпись над полем |

</details>

<details markdown>
<summary><b>Паттерны видимости (visible)</b></summary>

| Паттерн | Тип | Описание |
|---------|-----|----------|
| `true` / `false` | Литерал | Статическая видимость. Нельзя изменить из кода |
| `"$IsFieldVisible"` | Привязка к атрибуту | Управляется через `request.$context` |
| `"$CardState \| crt.IsEqual : 'edit'"` | Конвертер | Показать только в режиме редактирования |

</details>

<details markdown>
<summary><b>Паттерны привязки control</b></summary>

| Паттерн | Источник | Пример |
|---------|----------|--------|
| `$PDS_<Field>_<hash>` | Основной источник данных страницы | `$PDS_Type_j7ivbgg` |
| `$<DetailDS>_<Field>_<hash>` | Источник данных детали | `$AccountDetailDS_Purpose_vgyea1w` |
| `$LookupAttribute_<hash>` | Атрибут-справочник | `$LookupAttribute_gzza1dr` |

</details>

---

## ComboboxSearchTextAction

Дочерний элемент, который добавляет кнопку "Создать запись" в выпадающий список ComboBox. Вставляется **отдельной операцией** в `viewConfigDiff`, указывая `parentName` на родительский ComboBox.

### Связь родитель-потомок

```
ComboBox_jmwpcxh                         (crt.ComboBox)
  └─ listActions[0] → addRecord_ghwrzaa  (crt.ComboboxSearchTextAction)
```

### Свойства ComboboxSearchTextAction

| Свойство | Значение | Описание |
|----------|----------|----------|
| `code` | `"addRecord"` | Всегда `"addRecord"` |
| `type` | `"crt.ComboboxSearchTextAction"` | Тип дочернего действия |
| `icon` | `"combobox-add-new"` | Иконка "+" в выпадающем списке |
| `caption` | `#ResourceString(...)#` | Локализованный текст кнопки |
| `clicked.request` | `"crt.CreateRecordFromLookupRequest"` | Открывает страницу создания связанной записи |
| `clicked.params` | `{}` | Всегда пустой объект |

### Пример вставки

```json
{
  "operation": "insert",
  "name": "addRecord_849vl09",
  "values": {
    "code": "addRecord",
    "type": "crt.ComboboxSearchTextAction",
    "icon": "combobox-add-new",
    "caption": "#ResourceString(addRecord_849vl09_caption)#",
    "clicked": {
      "request": "crt.CreateRecordFromLookupRequest",
      "params": {}
    }
  },
  "parentName": "ComboBox_tqcf28h",
  "propertyName": "listActions",
  "index": 0
}
```

> Обрати внимание: `listActions` в самом ComboBox всегда пустой массив `[]`. Дочерний элемент добавляется отдельным insert-ом с `propertyName: "listActions"`.

---

## Привязка атрибутов

ComboBox связывается с данными через атрибут в `viewModelConfigDiff`. Свойство `control` в `viewConfigDiff` ссылается на имя атрибута с префиксом `$`.

**viewModelConfigDiff:**
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "PDS_Type_j7ivbgg": {
      "modelConfig": {
        "path": "PDS.Type"
      }
    }
  }
}
```

**viewConfigDiff** -- привязка:
```json
{
  "operation": "insert",
  "name": "ComboBox_j7ivbgg",
  "values": {
    "type": "crt.ComboBox",
    "control": "$PDS_Type_j7ivbgg",
    "...": "..."
  },
  "parentName": "GeneralInfoContainer",
  "propertyName": "items"
}
```

Паттерн `modelConfig.path`: `<DataSourceName>.<FieldName>`, где:

- `PDS` -- основной источник данных страницы (Page Data Source)
- `AccountDetailDS` -- источник данных детали
- `BillingAccountDS` -- именованный источник данных

### Управление значением из handler-а

```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    // Чтение текущего значения (возвращает объект { value, displayValue })
    const type = await request.$context.PDS_Type_j7ivbgg;
    console.log(type?.displayValue); // "Customer"

    // Установка значения
    request.$context.PDS_Type_j7ivbgg = {
      value: "00000000-0000-0000-0000-000000000001",
      displayValue: "Partner"
    };

    return await next?.handle(request);
  }
}
```

> Значение Lookup-поля -- это всегда объект `{ value: GUID, displayValue: string }`, а не скаляр.

---

## Частые ошибки

- **`listActions` заполнен прямо в ComboBox** -- так не работает. Дочерний `crt.ComboboxSearchTextAction` вставляется **отдельной insert-операцией** с `parentName` на ComboBox и `propertyName: "listActions"`.

- **Литеральная видимость `visible: true/false`** -- если задана литералом, изменить из кода нельзя. Для динамического управления замените на привязку `"$AttrName"`.

- **`controlActions` заполнен** -- свойство `controlActions` всегда должно быть пустым массивом `[]`. Действия добавляются только через `listActions`.

- **Забыли `showValueAsLink`** -- если поле ссылается на справочник и нужен переход на запись по клику, установите `showValueAsLink: true`.

- **`readonly: true` с `isAddAllowed: true`** -- это допустимая комбинация: пользователь не может изменить текущее значение, но может создать новую запись из выпадающего списка.

---

## См. также

- [crt.Input](input.md) -- текстовое поле ввода
- [Атрибуты и привязка данных](../page/viewmodel-config.md)
- [Конвертеры](../page/converters.md)

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `ComboBox_<hash>` | `ComboBox_vgyea1w` |
| Имя действия | `addRecord_<hash>` | `addRecord_ghwrzaa` |
| Привязка control | `$<DataSource>_<Field>_<hash>` | `$PDS_Type_j7ivbgg` |
| Атрибут-справочник | `$LookupAttribute_<hash>` | `$LookupAttribute_gzza1dr` |
| Label | `$Resources.Strings.<DS>_<Field>_<hash>` | `$Resources.Strings.PDS_Type_j7ivbgg` |

</details>
