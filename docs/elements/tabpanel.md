# crt.TabPanel / crt.CardToggleTabPanel / crt.TabContainer

Табы (вкладки) -- основной способ организации контента на Freedom UI страницах. В Creatio используются три связанных типа элементов:

- **`crt.TabPanel`** -- стандартная панель вкладок (toggle-режим, боковая панель)
- **`crt.CardToggleTabPanel`** -- панель вкладок в карточке записи (FormPage). Используется в центральной области страницы
- **`crt.TabContainer`** -- отдельная вкладка внутри любой из панелей

---

## Обзор

### Как устроены табы на странице

На типичной FormPage присутствует `CardToggleTabPanel` -- он содержит основные вкладки карточки (общая информация, хронология, лента и т.д.). Каждая вкладка -- это `TabContainer`, вложенный в панель.

Иерархия элементов:

```
CardToggleTabPanel (crt.CardToggleTabPanel)
  +-- TabContainer_GeneralInfoTabContainer (crt.TabContainer) -- "Основное"
  |     +-- GridContainer_* / FlexContainer_* -- контейнеры с полями
  +-- TabContainer_TimelineTabContainer (crt.TabContainer) -- "Хронология"
  +-- TabContainer_FeedTabContainer (crt.TabContainer) -- "Лента"
```

> Важно: `CardToggleTabPanel` **всегда предустановлен** на FormPage. Вы не создаёте его через `insert` -- только модифицируете через `merge`.

### crt.TabPanel vs crt.CardToggleTabPanel

| | `crt.TabPanel` | `crt.CardToggleTabPanel` |
|---|---|---|
| Где используется | Боковые toggle-панели, кастомные области | Центральная часть FormPage |
| Операции | `insert`, `merge` | Только `merge` (предустановлен) |
| Режим | `mode: "toggle"` -- сворачивается | Всегда развёрнут |
| Управление кнопками | Через `crt.ButtonToggleGroup` | Встроенные табы-заголовки |

---

## Свойства CardToggleTabPanel

Применяются через `merge` к существующему элементу `CardToggleTabPanel`.

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `styleType` | `"default"` | `"default"` | Стиль отображения табов |
| `bodyBackgroundColor` | `"primary-contrast-500"` \| `"auto"` | `"auto"` | Цвет фона области содержимого |
| `selectedTabTitleColor` | `string` | `"auto"` | Цвет заголовка выбранной вкладки |
| `tabTitleColor` | `string` | `"auto"` | Цвет заголовков невыбранных вкладок |
| `underlineSelectedTabColor` | `string` | `"auto"` | Цвет подчёркивания активной вкладки |
| `headerBackgroundColor` | `string` | `"auto"` | Цвет фона шапки с заголовками вкладок |
| `layoutConfig` | `{ maxWidth, minWidth }` | -- | Ограничения размера панели |

### Пример: настройка стилей

```json
{
  "operation": "merge",
  "name": "CardToggleTabPanel",
  "values": {
    "styleType": "default",
    "bodyBackgroundColor": "primary-contrast-500",
    "selectedTabTitleColor": "auto",
    "tabTitleColor": "auto",
    "underlineSelectedTabColor": "auto",
    "headerBackgroundColor": "auto"
  }
}
```

### Пример: ограничение ширины

Фиксированная ширина панели -- полезно, когда рядом располагается боковая панель:

```json
{
  "operation": "merge",
  "name": "CardToggleTabPanel",
  "values": {
    "layoutConfig": {
      "maxWidth": 568,
      "minWidth": 568
    }
  }
}
```

---

## Свойства TabContainer

`crt.TabContainer` -- отдельная вкладка внутри панели. Может быть как предустановленной (модифицируется через `merge`), так и добавленной кастомно (через `insert`).

| Свойство | Тип | Описание |
|----------|-----|----------|
| `type` | `"crt.TabContainer"` | Тип элемента |
| `caption` | `string` | Заголовок вкладки. `#ResourceString(...)#` для локализации |
| `items` | `array` | Вложенные элементы (контейнеры, поля, гриды) |
| `tools` | `array` | Элементы в заголовке вкладки (кнопки, метки) |
| `icon` | `string` | Иконка вкладки (для toggle-панелей) |
| `iconSize` | `string` | Размер иконки: `"large"`, `"medium"`, `"small"` и др. |
| `visible` | `boolean` \| `"$Attr"` | Видимость вкладки. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `badge` | `object` \| `"$Attr"` | Индикатор уведомления на вкладке |
| `padding` | `object` | Внутренние отступы: `{ left, right, top, bottom }` |
| `layoutConfig` | `object` | Позиция в сетке: `{ column, row, colSpan, rowSpan }` |

### Пример: добавление новой вкладки

```json
{
  "operation": "insert",
  "name": "TabContainer_CustomTab",
  "values": {
    "type": "crt.TabContainer",
    "caption": "#ResourceString(CustomTab_caption)#",
    "items": [],
    "tools": []
  },
  "parentName": "CardToggleTabPanel",
  "propertyName": "items",
  "index": 2
}
```

> `index` определяет позицию вкладки. `0` -- первая, `1` -- вторая и т.д.

### Пример: вкладка с layoutConfig

Когда TabContainer используется внутри GridContainer, задаётся позиция в сетке:

```json
{
  "operation": "insert",
  "name": "TabContainer_GeneralInfoTabContainer",
  "values": {
    "type": "crt.TabContainer",
    "items": [],
    "layoutConfig": {
      "column": 1,
      "row": 1,
      "colSpan": 1,
      "rowSpan": 1
    }
  },
  "parentName": "CardToggleTabPanel",
  "propertyName": "items",
  "index": 0
}
```

### Пример: индикатор уведомления (badge)

Индикатор (точка) отображается в углу заголовка вкладки -- например, чтобы привлечь внимание к непрочитанным элементам.

**Постоянно видимый индикатор:**
```json
{
  "operation": "merge",
  "name": "TabContainer_FeedTabContainer",
  "values": {
    "badge": {
      "visible": true
    }
  }
}
```

**Индикатор, привязанный к атрибуту:**
```json
{
  "operation": "merge",
  "name": "TabContainer_FeedTabContainer",
  "values": {
    "badge": "$HasUnreadFeedItems"
  }
}
```

Значение атрибута `HasUnreadFeedItems` управляет видимостью индикатора. Логику изменения атрибута описывайте в `handlers`.

---

## Свойства TabPanel (toggle-режим)

`crt.TabPanel` используется в связке с `crt.ButtonToggleGroup` для создания сворачиваемых боковых панелей с вкладками.

| Свойство | Тип | По умолчанию | Описание |
|----------|-----|---|---|
| `type` | `"crt.TabPanel"` | -- | Тип элемента |
| `mode` | `"toggle"` | -- | Режим панели |
| `fitContent` | `boolean` | -- | Подгонка по содержимому |
| `styleType` | `"default"` | `"default"` | Стиль отображения |
| `bodyBackgroundColor` | `string` | `"primary-contrast-500"` | Цвет фона |
| `layoutConfig` | `{ minWidth, maxWidth }` | -- | Ограничения ширины |
| `selectedTabTitleColor` | `string` | `"auto"` | Цвет выбранного заголовка |
| `tabTitleColor` | `string` | `"auto"` | Цвет заголовков |
| `underlineSelectedTabColor` | `string` | `"auto"` | Цвет подчёркивания |
| `headerBackgroundColor` | `string` | `"auto"` | Фон шапки |
| `allowToggleClose` | `boolean` | `true` | Показывать кнопку закрытия панели |
| `isToggleTabHeaderVisible` | `boolean` | `true` | Показывать заголовки вкладок |

### ButtonToggleGroup -- кнопки управления

`crt.ButtonToggleGroup` -- элемент-компаньон, который рендерит кнопки для переключения вкладок `TabPanel`. Размещается отдельно от самой панели (например, в шапке страницы).

| Свойство | Тип | Описание |
|----------|-----|----------|
| `for` | `string` | Имя связанного `TabPanel` |
| `type` | `"crt.ButtonToggleGroup"` | Тип элемента |
| `direction` | `"row"` \| `"column"` | Направление кнопок |
| `gap` | `"none"` \| `"small"` \| `"medium"` \| `"large"` | Отступ между кнопками |
| `size` | `"default"` \| `"small"` \| `"medium"` \| `"large"` | Размер кнопок |
| `shape` | `"default"` \| `"rounded"` | Форма кнопок |
| `contentAlign` | `"start"` \| `"center"` \| `"end"` | Выравнивание текста |
| `allowUntoggle` | `boolean` | Закрывать панель при повторном клике |
| `selectedTab` | `{ value: "TabContainerName" }` | Вкладка по умолчанию |
| `toggleViewMode` | `"button"` \| `"dropdown"` | Режим отображения: кнопки или выпадающий список |
| `badgeConfig` | `{ color, offset }` | Настройки индикаторов уведомлений |

<details markdown>
<summary><b>Пример: toggle-панель с двумя вкладками</b></summary>

**ButtonToggleGroup** (кнопки в шапке):
```json
{
  "operation": "insert",
  "name": "ButtonToggleGroup_custom",
  "values": {
    "for": "TabPanel_custom",
    "fitContent": true,
    "type": "crt.ButtonToggleGroup",
    "direction": "row",
    "gap": "medium",
    "size": "default",
    "shape": "rounded",
    "contentAlign": "center",
    "allowUntoggle": false,
    "selectedTab": {
      "value": "TabContainer_Notes"
    },
    "toggleViewMode": "button"
  },
  "parentName": "MainHeaderBottom",
  "propertyName": "items",
  "index": 1
}
```

**TabPanel** (область содержимого):
```json
{
  "operation": "insert",
  "name": "TabPanel_custom",
  "values": {
    "type": "crt.TabPanel",
    "items": [],
    "mode": "toggle",
    "fitContent": true,
    "styleType": "default",
    "bodyBackgroundColor": "primary-contrast-500",
    "layoutConfig": {
      "minWidth": 368,
      "maxWidth": 368
    },
    "allowToggleClose": true,
    "isToggleTabHeaderVisible": true
  },
  "parentName": "CenterContainer",
  "propertyName": "items",
  "index": 1
}
```

**Вкладки** (TabContainer внутри TabPanel):
```json
[
  {
    "operation": "insert",
    "name": "TabContainer_Notes",
    "values": {
      "type": "crt.TabContainer",
      "caption": "#ResourceString(NotesTab_caption)#",
      "items": [],
      "tools": [],
      "icon": "notes-button-icon",
      "iconSize": "large"
    },
    "parentName": "TabPanel_custom",
    "propertyName": "items",
    "index": 0
  },
  {
    "operation": "insert",
    "name": "TabContainer_Files",
    "values": {
      "type": "crt.TabContainer",
      "caption": "#ResourceString(FilesTab_caption)#",
      "items": [],
      "tools": []
    },
    "parentName": "TabPanel_custom",
    "propertyName": "items",
    "index": 1
  }
]
```

</details>

---

## Удаление вкладок

Удаление предустановленных вкладок -- частая задача при кастомизации FormPage. Нужно удалить **и саму вкладку, и её контейнер**.

### Пример: удаление вкладки "Основное"

```json
[
  {
    "operation": "remove",
    "name": "GeneralInfoTab"
  },
  {
    "operation": "remove",
    "name": "GeneralInfoTabContainer"
  }
]
```

> Важно: удаляйте **оба** элемента -- `*Tab` (заголовок вкладки) и `*TabContainer` (содержимое). Если удалить только один, на странице останутся артефакты.

### Пример: удаление вкладки "Хронология"

```json
[
  {
    "operation": "remove",
    "name": "TimelineTab"
  },
  {
    "operation": "remove",
    "name": "TimelineTabContainer"
  }
]
```

### Скрытие вместо удаления

Если вкладку нужно скрывать условно (а не убирать навсегда), используйте привязку `visible` к атрибуту:

```json
{
  "operation": "merge",
  "name": "TabContainer_GeneralInfoTabContainer",
  "values": {
    "visible": "$IsGeneralTabVisible"
  }
}
```

---

## Частые ошибки

- **Попытка `insert` для `CardToggleTabPanel`** -- этот элемент предустановлен на FormPage, используйте только `merge`. Операция `insert` создаст дубликат и сломает разметку.

- **Удаление только Tab без TabContainer** (или наоборот) -- останутся пустые заголовки или скрытый контент. Всегда удаляйте парой: `*Tab` + `*TabContainer`.

- **Неправильный `parentName` при добавлении вкладки** -- новые `TabContainer` добавляются в `parentName: "CardToggleTabPanel"` (или имя вашего `TabPanel`), а не в корень `viewConfigDiff`.

- **Забыли `tools: []`** при создании нового TabContainer -- без этого свойства могут не отрисовываться элементы в заголовке вкладки (кнопки, метки).

- **`index` больше количества существующих вкладок** -- вкладка добавится последней, но если рассчитываете на конкретную позицию, проверьте текущее количество табов на странице.

---

## См. также

- [crt.FlexContainer](flexcontainer.md) -- контейнер для содержимого вкладок
- [crt.ExpansionPanel](expansionpanel.md) -- сворачиваемая панель (альтернатива табам)
- [Структура viewConfigDiff](../page/view-config.md) -- операции insert, merge, remove
- [Привязка к атрибутам](../page/viewmodel-config.md) -- управление видимостью через `$Attr`
