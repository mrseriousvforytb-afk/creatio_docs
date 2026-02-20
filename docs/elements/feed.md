# crt.Feed

---

## Пример

Лента активностей на стандартной FormPage уже существует — достаточно merge-операции для настройки:

**viewConfigDiff:**
```json
{
  "operation": "merge",
  "name": "Feed",
  "values": {
    "dataSourceName": "PDS",
    "entitySchemaName": "Activity"
  }
}
```

На кастомной странице, где Feed отсутствует, используется insert с полным набором свойств:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Feed_AccountRelation",
  "values": {
    "type": "crt.Feed",
    "feedType": "Record",
    "primaryColumnValue": "$Id",
    "cardState": "$CardState",
    "allowExternalPost": false,
    "entitySchemaName": "AccountRelation",
    "dataSourceName": "AccountRelationDS",
    "visible": true,
    "showBlankSlate": true
  },
  "parentName": "FeedTabContainer",
  "propertyName": "items",
  "index": 0
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.Feed"` | да | — | Тип элемента |
| `dataSourceName` | `string` | да | — | Источник данных. Обычно `"PDS"` (основной источник данных страницы) |
| `entitySchemaName` | `string` | да | — | Имя схемы объекта, к записям которого привязана лента |
| `feedType` | [`FeedType`](#feedtype) | нет | `"Record"` | Тип ленты: записи объекта или пользователя |
| `primaryColumnValue` | `string` | нет | `"$Id"` | Привязка к первичному ключу записи |
| `cardState` | `string` | нет | `"$CardState"` | Привязка к состоянию карточки |
| `allowExternalPost` | `boolean` | нет | `false` | Разрешить сообщения от внешних пользователей |
| `isReadOnly` | `boolean` \| `"$Attr"` | нет | `false` | Режим "только чтение". Когда `true` — доступен только Like |
| `showBlankSlate` | `boolean` | нет | — | Показывать заглушку при отсутствии записей |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция и размер (column, colSpan, row, rowSpan) |

<details markdown>
<summary><b>feedType</b></summary>

| Значение | Описание |
|----------|----------|
| `"Record"` | Лента сообщений конкретной записи (контакт, обращение и т.д.) |
| `"User"` | Лента сообщений конкретного пользователя |

</details>

---

## Ключевые особенности

### Feed уже есть на стандартных FormPage

На страницах, созданных через стандартный шаблон FormPage, элемент `Feed` предустановлен как вкладка. Его **не нужно вставлять** — только модифицировать через `merge`:

```json
{
  "operation": "merge",
  "name": "Feed",
  "values": {
    "dataSourceName": "PDS",
    "entitySchemaName": "Task"
  }
}
```

Другие примеры `entitySchemaName` для merge-операций:

```json
{ "entitySchemaName": "Release" }
{ "entitySchemaName": "Claim" }
{ "entitySchemaName": "Contact" }
```

### Полная вставка на кастомной странице

Если страница создана без стандартного шаблона, Feed нужно вставить целиком. В этом случае укажите все ключевые свойства:

- `feedType` — тип ленты (`"Record"` для привязки к записи)
- `primaryColumnValue` — привязка к `$Id` записи
- `cardState` — привязка к `$CardState` (управляет поведением ленты при создании/редактировании)
- `entitySchemaName` — должен совпадать с основной сущностью страницы

### Связь с crt.FeedComposer

`crt.FeedComposer` — сестринский элемент, отвечающий за ввод и отправку сообщений в ленту. На стандартных FormPage он также предустановлен вместе с Feed.

### Режим "только чтение"

Когда `isReadOnly: true`, пользователь может только ставить Like на существующие сообщения. Создание и редактирование сообщений недоступно. Поддерживает привязку к атрибуту:

```json
{
  "operation": "merge",
  "name": "Feed",
  "values": {
    "isReadOnly": "$IsFeedReadOnly"
  }
}
```

---

## Частые ошибки

- **`entitySchemaName` не совпадает с сущностью страницы** — лента не покажет записи. Значение должно соответствовать объекту, к которому привязана страница.

- **Используют `insert` вместо `merge` на стандартной FormPage** — элемент Feed уже существует на шаблонных страницах. Попытка `insert` приведёт к дублированию или ошибке. Используйте `merge` и обращайтесь к элементу по имени `"Feed"`.

- **Забыли `primaryColumnValue` при insert** — без привязки к `$Id` лента не знает, к какой записи отображать сообщения.

- **Забыли `cardState` при insert** — без привязки к `$CardState` лента может некорректно работать при создании новой записи (когда карточка ещё не сохранена).

---

## См. также

- [crt.Button](button.md) — кнопки и обработка кликов
- [crt.TabPanel](tabpanel.md) — вкладки, на которых обычно размещается Feed
- [Handlers](../page/handlers.md) — обработчики запросов
