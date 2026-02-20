# crt.FlexContainer

Контейнер на основе Flexbox-макета. Самый распространённый контейнер в Freedom UI -- используется для группировки элементов в строку или колонку с управлением отступами, выравниванием и переносом.

---

## Пример

Колоночный контейнер с фоном, скруглением и отступами:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "FlexContainer_oahsujb",
  "values": {
    "type": "crt.FlexContainer",
    "direction": "column",
    "items": [],
    "fitContent": true,
    "visible": true,
    "color": "primary",
    "borderRadius": "medium",
    "padding": {
      "top": "medium",
      "right": "medium",
      "bottom": "medium",
      "left": "medium"
    },
    "alignItems": "stretch",
    "justifyContent": "start",
    "gap": "medium",
    "wrap": "nowrap"
  },
  "parentName": "CardContentContainer",
  "propertyName": "items",
  "index": 0
}
```

Строчный контейнер с переносом дочерних элементов:

```json
{
  "operation": "insert",
  "name": "FlexContainer_Tags",
  "values": {
    "type": "crt.FlexContainer",
    "direction": "row",
    "items": [],
    "fitContent": true,
    "color": "transparent",
    "borderRadius": "none",
    "padding": { "top": "none", "right": "none", "bottom": "none", "left": "none" },
    "alignItems": "stretch",
    "justifyContent": "start",
    "gap": "small",
    "wrap": "wrap"
  },
  "parentName": "FlexContainer_Content",
  "propertyName": "items",
  "index": 0
}
```

Минимальный контейнер -- достаточно `type`, `direction`, `items`:

```json
{
  "operation": "insert",
  "name": "FlexContainer_Row",
  "values": {
    "type": "crt.FlexContainer",
    "direction": "row",
    "items": [],
    "fitContent": true
  },
  "parentName": "CardContentContainer",
  "propertyName": "items",
  "index": 0
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.FlexContainer"` | да | -- | Тип элемента |
| `direction` | [`Direction`](#direction) | да | -- | Направление раскладки |
| `items` | `[]` | да | `[]` | Дочерние элементы. Всегда пустой массив -- дочерние элементы добавляются отдельными `insert`-операциями с `parentName` |
| `fitContent` | `boolean` | нет | `false` | Автоподгонка размера по содержимому. На практике почти всегда `true` |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `color` | [`Color`](#color) | нет | -- | Цвет фона |
| `borderRadius` | [`SizePreset`](#sizepreset) | нет | `"none"` | Скругление углов |
| `padding` | [`PaddingConfig`](#padding) | нет | -- | Внутренние отступы по четырём сторонам |
| `alignItems` | [`AlignItems`](#alignitems) | нет | `"stretch"` | Выравнивание по поперечной оси |
| `justifyContent` | [`JustifyContent`](#justifycontent) | нет | `"start"` | Выравнивание по главной оси |
| `gap` | [`SizePreset`](#sizepreset) | нет | `"none"` | Промежуток между дочерними элементами |
| `wrap` | `"wrap"` \| `"nowrap"` | нет | `"nowrap"` | Перенос элементов при нехватке места |
| `layoutConfig` | `LayoutConfig` | нет | -- | Позиция в GridContainer или `min-width` для spacer-ов |

<details markdown>
<summary><b>direction</b></summary>

| Значение | Описание |
|----------|----------|
| `"row"` | Горизонтально, слева направо |
| `"column"` | Вертикально, сверху вниз |
| `"row-reverse"` | Горизонтально, справа налево |
| `"column-reverse"` | Вертикально, снизу вверх |

На практике используются почти исключительно `"row"` и `"column"`.

</details>

<details markdown>
<summary><b>color</b></summary>

| Значение | Описание |
|----------|----------|
| `"primary"` | Основной фон (белый/серый в зависимости от темы) |
| `"transparent"` | Без фона |
| `"primary-contrast-500"` | Контрастный фон |
| Hex-код | Произвольный цвет, например `"#FDAB06"` |

</details>

<details markdown>
<summary><b>sizePreset</b></summary>

Применимо к `borderRadius`, `gap` и значениям внутри `padding`:

| Значение | |
|----------|-|
| `"none"` | Нет |
| `"small"` | Маленький |
| `"medium"` | Средний |
| `"large"` | Большой |

</details>

<details markdown>
<summary><b>padding</b></summary>

Объект с четырьмя сторонами. Каждая сторона принимает `SizePreset` **или** пиксельное значение:

```json
{
  "top": "medium",
  "right": "large",
  "bottom": "6px",
  "left": 6
}
```

Числовое значение и строка с `px` -- эквивалентны (`6` = `"6px"`).

Одинаковые отступы со всех сторон:

```json
{ "top": "medium", "right": "medium", "bottom": "medium", "left": "medium" }
```

Без отступов:

```json
{ "top": "none", "right": "none", "bottom": "none", "left": "none" }
```

</details>

<details markdown>
<summary><b>alignItems</b></summary>

Выравнивание дочерних элементов по **поперечной** оси (для `direction: "row"` -- по вертикали, для `"column"` -- по горизонтали):

| Значение | Описание |
|----------|----------|
| `"stretch"` | Растянуть на всю высоту/ширину (по умолчанию) |
| `"center"` | По центру |
| `"flex-start"` | К началу |
| `"flex-end"` | К концу |

</details>

<details markdown>
<summary><b>justifyContent</b></summary>

Выравнивание дочерних элементов по **главной** оси (для `direction: "row"` -- по горизонтали, для `"column"` -- по вертикали):

| Значение | Описание |
|----------|----------|
| `"start"` | К началу (по умолчанию) |
| `"center"` | По центру |
| `"end"` | К концу |
| `"space-between"` | Равномерно, первый и последний прижаты к краям |

</details>

---

## Spacer (пустой контейнер)

Пустой `FlexContainer` с `layoutConfig.min-width` используется как распорка -- фиксированный отступ между элементами:

```json
{
  "operation": "insert",
  "name": "FlexContainer_Spacer",
  "values": {
    "layoutConfig": { "min-width": "28" },
    "type": "crt.FlexContainer",
    "direction": "row",
    "items": [],
    "fitContent": true
  },
  "parentName": "FlexContainer_Row",
  "propertyName": "items",
  "index": 0
}
```

Значение `min-width` задаётся строкой в пикселях (без суффикса `px`).

---

## Управление видимостью

Через привязку к атрибуту можно показывать/скрывать контейнер вместе со всеми дочерними элементами.

**viewModelConfigDiff** -- атрибут:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "IsDetailsVisible": { "value": true }
  }
}
```

**viewConfigDiff** -- привязка:
```json
{
  "operation": "merge",
  "name": "FlexContainer_Details",
  "values": {
    "visible": "$IsDetailsVisible"
  }
}
```

**handlers** -- управление:
```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    const type = await request.$context.PDS_Type_abc123;
    // Показываем блок деталей только для определённого типа
    request.$context.IsDetailsVisible = type?.displayValue === "Service";
    return await next?.handle(request);
  }
}
```

> Если `visible` задано литералом (`true` / `false`), изменить его из кода нельзя. Для динамической видимости используйте привязку `"$AttributeName"`.

---

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `FlexContainer_<hash>` | `FlexContainer_oahsujb` |
| Семантическое имя | `FlexContainer_<semanticName>` | `FlexContainer_DataGridDetails` |
| Известные контейнеры | Без префикса | `CardContentContainer`, `MainHeader` |

Известные контейнеры (`CardContentContainer`, `MainHeader` и др.) -- это элементы из базовых схем страниц. Их имена не меняются и используются как `parentName` при вставке кастомных элементов.

</details>

---

## Частые ошибки

- **Забыли `items: []`** -- свойство обязательное. Без него контейнер не отрисуется или выдаст ошибку.

- **Дочерние элементы внутри `items`** -- элементы **не** вставляются в массив `items` напрямую. Каждый дочерний элемент добавляется отдельной `insert`-операцией с `parentName`, указывающим на имя контейнера, и `"propertyName": "items"`.

- **`fitContent` не указан** -- без `fitContent: true` контейнер может занять больше места, чем нужно, растягиваясь на всё доступное пространство.

- **Путаница `alignItems` и `justifyContent`** -- `alignItems` действует по **поперечной** оси, `justifyContent` -- по **главной**. Для `direction: "row"` главная ось -- горизонтальная; для `"column"` -- вертикальная.

- **Литерал вместо привязки в `visible`** -- если задано `"visible": true`, значение нельзя изменить из handler-а. Используйте `"visible": "$AttrName"`.

---

## См. также

- [crt.GridContainer](gridcontainer.md) -- контейнер на основе CSS Grid с фиксированной сеткой
- [Атрибуты и привязки](../page/viewmodel-config.md) -- механизм `"$Attr"` для динамических свойств
