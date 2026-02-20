# crt.DateTimePicker

---

## Пример

Поле выбора даты и времени, привязанное к атрибуту модели:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "DateTimePicker_t4493zs",
  "values": {
    "type": "crt.DateTimePicker",
    "pickerType": "datetime",
    "label": "$Resources.Strings.PDS_Date_cndt1qv",
    "labelPosition": "auto",
    "control": "$PDS_Date_cndt1qv"
  },
  "parentName": "FlexContainer_45mm66n",
  "propertyName": "items",
  "index": 0
}
```

Поле даты в режиме только для чтения с явной позицией в сетке:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "DateTimePicker_LeRegistrationDate",
  "values": {
    "layoutConfig": { "column": 1, "row": 1, "colSpan": 1, "rowSpan": 1 },
    "type": "crt.DateTimePicker",
    "pickerType": "date",
    "label": "$Resources.Strings.DateTimeAttribute_02voahx",
    "labelPosition": "auto",
    "control": "$DateTimeAttribute_02voahx",
    "visible": true,
    "readonly": true,
    "placeholder": "",
    "tooltip": ""
  },
  "parentName": "SideAreaProfileContainer",
  "propertyName": "items",
  "index": 0
}
```

Минимальная конфигурация — только обязательные свойства:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "DateTimePicker_4etgqv9",
  "values": {
    "type": "crt.DateTimePicker",
    "pickerType": "date",
    "label": "$Resources.Strings.PDS_Deadline_u331crw",
    "labelPosition": "auto",
    "control": "$PDS_Deadline_u331crw"
  },
  "parentName": "FlexContainer_8eak4li",
  "propertyName": "items",
  "index": 3
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.DateTimePicker"` | да | — | Тип элемента |
| `pickerType` | [`PickerType`](#pickertype) | да | — | Режим выбора: дата, дата-время или время |
| `label` | `string` | да | — | Подпись поля. [`#ResourceString(...)#`](../page/view-config.md) — ссылка на ресурсы |
| `labelPosition` | `"auto"` \| `"above"` | да | — | Расположение подписи |
| `control` | `string` | да | — | Привязка к атрибуту: `"$AttributeName"` |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `readonly` | `boolean` \| `"$Attr"` | нет | `false` | Только чтение. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `disabled` | `boolean` \| `"$Attr"` | нет | `false` | Заблокировано для редактирования |
| `placeholder` | `string` | нет | `""` | Текст-заполнитель при пустом значении |
| `tooltip` | `string` | нет | `""` | Всплывающая подсказка при наведении |
| `ariaLabel` | `string` | нет | — | Значение `aria-label` для accessibility |
| `multiYearSelector` | `boolean` | нет | `false` | Показывать селектор мульти-года при клике на год |
| `twelvehour` | `boolean` | нет | `false` | 12-часовой формат времени. По умолчанию 24-часовой |
| `startView` | [`StartView`](#startview) | нет | `"month"` | Начальный вид при открытии календаря |
| `mode` | [`DisplayMode`](#displaymode) | нет | `"auto"` | Режим отображения календаря |
| `timeInterval` | `number` | нет | `1` | Интервал поиска в мс |
| `preventSameDateTimeSelection` | `boolean` | нет | `false` | Запретить повторный выбор текущей даты/времени |
| `rowModeSizePx` | `number` | нет | `320` | Ширина поля в пикселях |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в GridContainer |

<details markdown>
<summary><b>pickerType</b></summary>

| Значение | Описание |
|----------|----------|
| `"date"` | Только дата (самый распространённый режим) |
| `"datetime"` | Дата и время |
| `"time"` | Только время (используется редко) |

</details>

<details markdown>
<summary><b>startView</b></summary>

Определяет, какой вид откроется при клике на поле.

| Значение | Описание |
|----------|----------|
| `"month"` | Вид месяца (по умолчанию) |
| `"year"` | Вид года |
| `"hour"` | Вид часов |
| `"multi-year"` | Мульти-год |

</details>

<details markdown>
<summary><b>displayMode</b></summary>

| Значение | Описание |
|----------|----------|
| `"auto"` | Автоматический (по умолчанию) |
| `"portrait"` | Вертикальная ориентация |
| `"landscape"` | Горизонтальная ориентация |

</details>

---

## Горячие клавиши

При открытом календаре доступна навигация с клавиатуры:

| Клавиша | Действие |
|---------|----------|
| `LEFT_ARROW` | Предыдущий день |
| `RIGHT_ARROW` | Следующий день |
| `UP_ARROW` | Предыдущая неделя |
| `DOWN_ARROW` | Следующая неделя |
| `HOME` | Первый день месяца |
| `END` | Последний день месяца |
| `PAGE_UP` | Предыдущий месяц |
| `PAGE_DOWN` | Следующий месяц |
| `ALT + PAGE_UP` | Предыдущий год |
| `ALT + PAGE_DOWN` | Следующий год |
| `ENTER` | Выбрать выделенный элемент |

---

## Частые ошибки

- **Не указан `pickerType`** — свойство обязательное. Без него компонент не будет знать, какой режим выбора использовать.

- **`readonly` задан литералом, а нужно менять динамически** — если `readonly: true` записан напрямую в `viewConfigDiff`, его нельзя изменить из обработчика. Используйте привязку `"$IsFieldReadonly"` и управляйте атрибутом через `request.$context`.

- **Путают `readonly` и `disabled`** — `readonly` позволяет фокус и копирование значения, `disabled` полностью блокирует элемент (нельзя ни кликнуть, ни скопировать).

- **Используют `"time"` без проверки** — режим `pickerType: "time"` используется крайне редко. Убедитесь, что колонка в модели данных имеет тип Time, а не DateTime.

---

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `DateTimePicker_<semanticName>` | `DateTimePicker_LeRegistrationDate` |
| Имя элемента (альт.) | `DateTimePicker_<hash>` | `DateTimePicker_t4493zs` |

</details>

---

## См. также

- [crt.Input](input.md) — текстовое поле ввода
- [crt.NumberInput](numberinput.md) — числовое поле ввода
- [Атрибуты и привязка данных](../page/viewmodel-config.md)
