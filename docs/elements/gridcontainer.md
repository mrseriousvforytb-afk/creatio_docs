# crt.GridContainer

---

## Пример

Сетка из 4 равных колонок с полным набором стилевых свойств:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "GridContainer_j4y5q7m",
  "values": {
    "type": "crt.GridContainer",
    "columns": [
      "minmax(32px, 1fr)",
      "minmax(32px, 1fr)",
      "minmax(32px, 1fr)",
      "minmax(32px, 1fr)"
    ],
    "rows": "minmax(max-content, 32px)",
    "gap": { "columnGap": "large", "rowGap": "none" },
    "items": [],
    "fitContent": true,
    "visible": true,
    "color": "transparent",
    "borderRadius": "large",
    "padding": {
      "top": "medium",
      "right": "medium",
      "bottom": "medium",
      "left": "medium"
    },
    "alignItems": "stretch"
  },
  "parentName": "FlexContainer_oahsujb",
  "propertyName": "items",
  "index": 0
}
```

Дочерние элементы размещаются внутри сетки через `layoutConfig` с указанием колонки, строки и размера ячейки:

```json
{
  "operation": "insert",
  "name": "Button_Save",
  "values": {
    "layoutConfig": {
      "column": 1,
      "row": 1,
      "colSpan": 3,
      "rowSpan": 1
    },
    "type": "crt.Button",
    "caption": "#ResourceString(Button_caption)#",
    "color": "default"
  },
  "parentName": "GridContainer_j4y5q7m",
  "propertyName": "items",
  "index": 0
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.GridContainer"` | да | -- | Тип элемента |
| `columns` | `string[]` | да | -- | Определения колонок CSS Grid. Каждый элемент массива -- одна колонка |
| `rows` | `string` | нет | -- | Определение строк CSS Grid |
| `gap` | [`GapConfig`](#gap) | нет | -- | Промежутки между колонками и строками |
| `items` | `[]` | да | `[]` | Дочерние элементы |
| `fitContent` | `boolean` | нет | -- | Автоподгонка под содержимое |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `color` | `string` | нет | -- | Цвет фона. Принимает имена токенов (`"transparent"`) или HEX (`"#FDAB06"`) |
| `borderRadius` | [`BorderRadius`](#borderradius) | нет | -- | Скругление углов |
| `padding` | [`PaddingConfig`](#padding) | нет | -- | Внутренние отступы контейнера |
| `justifyItems` | [`AlignValue`](#justifyitems--alignitems) | нет | -- | Горизонтальное выравнивание дочерних элементов внутри ячеек |
| `alignItems` | [`AlignValue`](#justifyitems--alignitems) | нет | -- | Вертикальное выравнивание дочерних элементов внутри ячеек |
| `styles` | `Record<string, string>` | нет | -- | Произвольные CSS-стили контейнера |
| `layoutConfig` | [`LayoutConfig`](#layoutconfig) | нет | -- | Размеры самого контейнера (minWidth, maxWidth, width, height) |

<details markdown>
<summary><b>columns</b> -- паттерны определения колонок</summary>

Каждый элемент массива `columns` задает одну колонку через CSS-функцию `minmax()`:

```javascript
// 4 равные колонки (стандарт для формы)
["minmax(32px, 1fr)", "minmax(32px, 1fr)", "minmax(32px, 1fr)", "minmax(32px, 1fr)"]

// 3 широкие колонки
["minmax(64px, 1fr)", "minmax(64px, 1fr)", "minmax(64px, 1fr)"]

// Одна колонка (вертикальная раскладка)
["minmax(32px, 1fr)"]

// Фиксированная + гибкая колонка
["298px", "minmax(64px, 1fr)"]
```

Количество элементов в массиве = количество колонок в сетке. Значение `1fr` означает равное распределение свободного пространства.

</details>

<details markdown>
<summary><b>rows</b></summary>

Строка с CSS-определением высоты строк. Типичное значение:

```json
"rows": "minmax(max-content, 32px)"
```

`max-content` позволяет строке растянуться под содержимое, но не меньше 32px.

</details>

<details markdown>
<summary><b>gap</b></summary>

Промежутки между ячейками сетки:

| Свойство | Тип | Допустимые значения |
|----------|-----|---------------------|
| `columnGap` | `string` | `"large"`, `"medium"`, `"small"` |
| `rowGap` | `string` \| `number` | `"none"`, `"small"`, `0` |

```json
"gap": { "columnGap": "large", "rowGap": "none" }
```

</details>

<details markdown>
<summary><b>padding</b></summary>

Внутренние отступы контейнера. Принимает именованные значения, числа (px) или строки:

| Значение | |
|----------|-|
| `"none"` | Нулевой отступ |
| `"small"` | Маленький |
| `"medium"` | Средний |
| `"large"` | Большой |

Можно задать разные значения для каждой стороны:

```json
"padding": {
  "top": "medium",
  "right": "medium",
  "bottom": "medium",
  "left": "medium"
}
```

Допускается смешивать типы значений:

```json
"padding": {
  "top": "6px",
  "right": 6,
  "bottom": "small",
  "left": "large"
}
```

</details>

<details markdown>
<summary><b>borderRadius</b></summary>

| Значение | |
|----------|-|
| `"none"` | Без скругления |
| `"small"` | Маленькое скругление |
| `"medium"` | Среднее скругление |
| `"large"` | Большое скругление |

</details>

<details markdown>
<summary><b>justifyItems / alignItems</b></summary>

Управление выравниванием дочерних элементов внутри ячеек сетки:

| Значение | justifyItems (горизонталь) | alignItems (вертикаль) |
|----------|----------------------------|------------------------|
| `"start"` | Прижать к левому краю | Прижать к верхнему краю |
| `"end"` | Прижать к правому краю | Прижать к нижнему краю |
| `"center"` | По центру горизонтально | По центру вертикально |
| `"stretch"` | Растянуть на всю ширину ячейки | Растянуть на всю высоту ячейки |

</details>

<details markdown>
<summary><b>layoutConfig</b></summary>

Размеры самого контейнера (в пикселях):

| Параметр | Тип | Описание |
|----------|-----|----------|
| `minWidth` | `number` | Минимальная ширина |
| `maxWidth` | `number` | Максимальная ширина |
| `width` | `number` | Фиксированная ширина |
| `height` | `number` | Фиксированная высота |

```json
"layoutConfig": {
  "minWidth": 368,
  "maxWidth": 368
}
```

> Не путайте с `layoutConfig` дочерних элементов -- у них `layoutConfig` содержит `column`, `row`, `colSpan`, `rowSpan` для позиционирования внутри сетки.

</details>

<details markdown>
<summary><b>styles</b></summary>

Произвольные CSS-свойства в формате `Record<string, string>`:

```json
"styles": { "overflow-x": "hidden" }
```

Используйте для случаев, когда стандартных свойств контейнера недостаточно.

</details>

<details markdown>
<summary><b>Размещение дочерних элементов (layoutConfig ребенка)</b></summary>

Дочерние элементы, вставляемые в `items` контейнера, используют собственный `layoutConfig` для позиционирования в сетке:

| Параметр | Тип | Описание |
|----------|-----|----------|
| `column` | `number` | Номер колонки (начиная с 1) |
| `row` | `number` | Номер строки (начиная с 1) |
| `colSpan` | `number` | Сколько колонок занимает элемент |
| `rowSpan` | `number` | Сколько строк занимает элемент |

```json
{
  "operation": "insert",
  "name": "Input_Name",
  "values": {
    "layoutConfig": {
      "column": 1,
      "row": 1,
      "colSpan": 2,
      "rowSpan": 1
    },
    "type": "crt.Input",
    "label": "#ResourceString(Input_Name_label)#"
  },
  "parentName": "GridContainer_j4y5q7m",
  "propertyName": "items",
  "index": 0
}
```

В сетке из 4 колонок элемент с `column: 1, colSpan: 2` займет первые две колонки.

</details>

---

## Дополнительные примеры

**Одна колонка с кастомными стилями:**
```json
{
  "operation": "insert",
  "name": "GridContainer_wkid0zt",
  "values": {
    "type": "crt.GridContainer",
    "rows": "minmax(max-content, 24px)",
    "columns": ["minmax(32px, 1fr)"],
    "gap": { "columnGap": "large", "rowGap": 0 },
    "styles": { "overflow-x": "hidden" },
    "items": []
  }
}
```

**Изменение существующего контейнера через merge:**
```json
{
  "operation": "merge",
  "name": "TabContainer_MainTabs",
  "values": {
    "columns": [
      "minmax(64px, 1fr)",
      "minmax(64px, 1fr)",
      "minmax(64px, 1fr)"
    ],
    "gap": { "columnGap": "large", "rowGap": "none" },
    "alignItems": "stretch"
  }
}
```

---

## Ограничения

Следующие стандартные возможности CSS Grid **не поддерживаются** в `crt.GridContainer`:

| Что не работает | Описание |
|-----------------|----------|
| `grid-line-name` | Именованные линии сетки |
| `fit-content()` | Ограничение размера трека. Браузер рендерит одну колонку 32px вне зависимости от аргумента |
| `repeat()` | Компактная запись повторяющихся колонок. Нужно перечислять каждую колонку явно |

Вместо `repeat(4, minmax(32px, 1fr))` необходимо записывать все 4 элемента массива вручную.

---

## Частые ошибки

- **Используют `repeat()` в `columns`** -- функция не поддерживается, колонки не отрисуются корректно. Перечисляйте каждую колонку отдельным элементом массива.

- **Путают `layoutConfig` контейнера и дочерних элементов** -- у контейнера `layoutConfig` задает размеры (`minWidth`, `maxWidth`), а у дочерних элементов -- позицию в сетке (`column`, `row`, `colSpan`, `rowSpan`).

- **Забывают `items: []`** -- свойство обязательное, даже если дочерних элементов пока нет. Без него контейнер может не отрисоваться.

---

## Соглашения об именовании

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя контейнера | `GridContainer_<hash>` | `GridContainer_j4y5q7m` |
| Семантическое имя | `GridContainer_<назначение>` | `GridContainer_GeneralInfo` |
| Системные контейнеры | Без префикса | `SideAreaProfileContainer`, `TopAreaProfileContainer` |

---

## См. также

- [crt.FlexContainer](flexcontainer.md) -- контейнер на основе Flexbox (для линейных раскладок)
- [Структура viewConfigDiff](../page/view-config.md)
