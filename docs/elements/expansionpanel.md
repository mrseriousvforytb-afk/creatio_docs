# crt.ExpansionPanel

---

## Пример

Сворачиваемая панель с заголовком, стрелкой-переключателем и контейнером для контента:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "ExpansionPanel_b800djm",
  "values": {
    "type": "crt.ExpansionPanel",
    "title": "#ResourceString(ExpansionPanel_b800djm_title)#",
    "toggleType": "default",
    "togglePosition": "before",
    "expanded": false,
    "labelColor": "auto",
    "fullWidthHeader": false,
    "titleWidth": 20,
    "padding": {
      "top": "small",
      "bottom": "small",
      "left": "none",
      "right": "none"
    },
    "fitContent": true,
    "visible": true,
    "alignItems": "stretch",
    "tools": [],
    "items": [],
    "layoutConfig": {
      "column": 1,
      "row": 5,
      "colSpan": 3,
      "rowSpan": 1
    }
  },
  "parentName": "GeneralInfoTabContainer",
  "propertyName": "items",
  "index": 5
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.ExpansionPanel"` | да | — | Тип элемента |
| `title` | `string` | да | — | Заголовок панели. [`#ResourceString(...)#`](../page/view-config.md) — ссылка на ресурсы страницы |
| `tools` | `array` | да | `[]` | Элементы тулбара в заголовке |
| `items` | `array` | да | `[]` | Контент панели (тело) |
| `toggleType` | [`ToggleType`](#toggletype) | нет | `"default"` | Стиль стрелки-переключателя |
| `togglePosition` | [`TogglePosition`](#toggleposition) | нет | `"before"` | Позиция стрелки |
| `expanded` | `boolean` \| `"$Attr"` | нет | `true` | Развёрнута ли панель. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `labelColor` | `string` | нет | `"auto"` | Цвет текста заголовка |
| `fullWidthHeader` | `boolean` | нет | `false` | Заголовок на всю ширину |
| `titleWidth` | `number` | нет | `50` | Ширина заголовка в процентах |
| `description` | `string` | нет | — | Описание в шапке панели |
| `ariaLabel` | `string` | нет | = `title` | Текст рядом со стрелкой (accessibility) |
| `extraStyles` | `object` | нет | — | Дополнительные стили для заголовка и стрелки |
| `padding` | `PaddingConfig` | нет | — | Отступы |
| `fitContent` | `boolean` | нет | — | Подгонка под контент |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `alignItems` | `string` | нет | — | Выравнивание дочерних элементов |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в GridContainer |
| `styles` | `Record<string, string>` | нет | — | Пользовательские CSS-стили |

<details markdown>
<summary><b>toggleType</b></summary>

| Значение | Описание |
|----------|----------|
| `"default"` | Красная стрелка с фоном |
| `"material"` | Синяя стрелка без фона |

</details>

<details markdown>
<summary><b>togglePosition</b></summary>

| Значение | Описание |
|----------|----------|
| `"before"` | Стрелка перед заголовком |
| `"after"` | Стрелка после заголовка |

</details>

<details markdown>
<summary><b>padding</b></summary>

Объект с отступами по сторонам. Допустимые значения каждого свойства: `"none"`, `"small"`, `"medium"`, `"large"`.

```json
"padding": {
  "top": "small",
  "bottom": "small",
  "left": "none",
  "right": "none"
}
```

</details>

---

## Структура дочерних элементов

ExpansionPanel автоматически ожидает два дочерних контейнера: тулбар в заголовке и контейнер с контентом.

```
ExpansionPanel_b800djm
  +-- tools[0] --> ExpansionPanel_Header_Tools   (тулбар в шапке)
  +-- items[0] --> ExpansionPanel_Content_Items   (тело панели)
        +-- GridContainer_wkid0zt                 (грид для размещения контента)
```

### Тулбар заголовка

Контейнер `tools` размещается в правой части шапки панели. Сюда добавляются кнопки действий (например, «Добавить», «Настроить»).

```json
{
  "operation": "insert",
  "name": "ExpansionPanel_Header_Tools",
  "values": {
    "type": "crt.FlexContainer",
    "direction": "row",
    "items": []
  },
  "parentName": "ExpansionPanel_b800djm",
  "propertyName": "tools",
  "index": 0
}
```

### Контейнер контента

Контейнер `items` — основное тело панели. Обычно внутри размещается `crt.GridContainer` для табличной раскладки полей.

```json
{
  "operation": "insert",
  "name": "ExpansionPanel_Content_Items",
  "values": {
    "type": "crt.GridContainer",
    "columns": ["minmax(32px, 1fr)", "minmax(32px, 1fr)"],
    "rows": "minmax(max-content, 32px)",
    "gap": { "columnGap": "large", "rowGap": "none" },
    "items": [],
    "visible": true,
    "color": "transparent",
    "borderRadius": "none",
    "padding": { "top": "none", "right": "none", "bottom": "none", "left": "none" }
  },
  "parentName": "ExpansionPanel_b800djm",
  "propertyName": "items",
  "index": 0
}
```

Внутрь `ExpansionPanel_Content_Items` добавляются поля, кнопки и другие элементы как обычные `insert`-операции с `parentName: "ExpansionPanel_Content_Items"`.

---

## Управление состоянием

Свойство `expanded` можно привязать к атрибуту для программного управления раскрытием/сворачиванием.

**viewModelConfigDiff** — атрибут:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "IsPanelExpanded": { "value": true }
  }
}
```

**viewConfigDiff** — привязка:
```json
{
  "operation": "merge",
  "name": "ExpansionPanel_b800djm",
  "values": {
    "expanded": "$IsPanelExpanded"
  }
}
```

**handlers** — управление из кода:
```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    // Сворачиваем панель, если запись не в статусе "Черновик"
    const status = await request.$context.PDS_Status_abc123;
    request.$context.IsPanelExpanded = status?.displayValue === "Draft";
    return await next?.handle(request);
  }
}
```

> Если `expanded` задано как литерал (`expanded: true`), изменить состояние из кода **нельзя**. Используйте привязку `"$AttributeName"`.

---

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя панели | `ExpansionPanel_<hash>` | `ExpansionPanel_b800djm` |
| Тулбар заголовка | `ExpansionPanel_Header_Tools` | — |
| Контейнер контента | `ExpansionPanel_Content_Items` | — |

</details>

---

## Частые ошибки

- **Забыли дочерние контейнеры** — ExpansionPanel не отрисует контент, если не добавить `ExpansionPanel_Header_Tools` (в `tools`) и `ExpansionPanel_Content_Items` (в `items`). Оба контейнера обязательны, даже если `tools` остаётся пустым.

- **`titleWidth: 50` обрезает длинный заголовок** — по умолчанию заголовку отводится 50% ширины. Если текст не помещается, уменьшите значение (например, `titleWidth: 20`) или установите `fullWidthHeader: true`.

- **Литерал вместо привязки для `expanded`** — если задать `expanded: false` как литерал, программно развернуть панель не получится. Для динамического управления используйте `"$AttributeName"`.

- **`toggleType: "material"` незаметен на светлом фоне** — синяя стрелка без фона менее контрастна. Убедитесь, что пользователь понимает, что панель сворачиваемая.

---

## См. также

- [crt.FlexContainer](flexcontainer.md) — контейнер с гибкой раскладкой
- [crt.GridContainer](gridcontainer.md) — контейнер с табличной раскладкой
- [crt.TabPanel](tabpanel.md) — вкладки как альтернатива сворачиваемым панелям
