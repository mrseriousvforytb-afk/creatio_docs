# crt.NumberInput

Числовое поле ввода для целых и дробных значений.

---

## Пример

Числовое поле с минимальной конфигурацией:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "NumberInput_2k6l4yz",
  "values": {
    "type": "crt.NumberInput",
    "label": "$Resources.Strings.BanksDS_ExternalId_qa6xnax",
    "labelPosition": "above",
    "control": "$BanksDS_ExternalId_qa6xnax",
    "layoutConfig": { "column": 3, "row": 1, "colSpan": 1, "rowSpan": 1 }
  },
  "parentName": "GridContainer_cpyy1gs",
  "propertyName": "items",
  "index": 1
}
```

Числовое поле с управлением видимостью, placeholder и tooltip:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "NumberInput_gf2dcxn",
  "values": {
    "type": "crt.NumberInput",
    "label": "$Resources.Strings.PDS_Priority_tk97om7",
    "labelPosition": "above",
    "control": "$PDS_Priority_tk97om7",
    "visible": "$IsPriorityVisible",
    "readonly": false,
    "placeholder": "#ResourceString(Priority_placeholder)#",
    "tooltip": "#ResourceString(Priority_tooltip)#"
  },
  "parentName": "FlexContainer_8eak4li",
  "propertyName": "items",
  "index": 1
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.NumberInput"` | да | — | Тип элемента |
| `label` | `string` | да | — | Заголовок поля. [`$Resources.Strings.*`](../page/view-config.md) — ссылка на ресурсы страницы |
| `labelPosition` | [`LabelPosition`](#labelposition) | да | `"auto"` | Расположение заголовка |
| `control` | `string` | да | — | Привязка к данным: `"$PDS_FieldName_hash"` или `"$AttributeName"` |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `readonly` | `boolean` | нет | `false` | Только для чтения |
| `disabled` | `boolean` | нет | `false` | Заблокировано для редактирования |
| `placeholder` | `string` | нет | `""` | Текст-подсказка внутри пустого поля |
| `tooltip` | `string` | нет | `""` | Всплывающая подсказка при наведении |
| `min` | `number` | нет | — | Минимальное допустимое значение |
| `max` | `number` | нет | — | Максимальное допустимое значение |
| `decimalPrecision` | `number` | нет | — | Количество знаков после запятой |
| `autocomplete` | `string` | нет | `"off"` | Автозаполнение браузером |
| `rowModeSizePx` | `number` | нет | `320` | Ширина поля в пикселях |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в GridContainer |

<details markdown>
<summary><b>labelPosition</b></summary>

| Значение | Описание |
|----------|----------|
| `"auto"` | Автоматическое размещение (по умолчанию) |
| `"left"` | Заголовок слева от поля |
| `"right"` | Заголовок справа от поля |
| `"above"` | Заголовок над полем |
| `"hidden"` | Заголовок скрыт |

</details>

<details markdown>
<summary><b>min / max / decimalPrecision</b></summary>

Свойства `min`, `max` и `decimalPrecision` позволяют ограничить диапазон и формат вводимых значений.

```json
{
  "type": "crt.NumberInput",
  "label": "$Resources.Strings.PDS_Discount_abc123",
  "control": "$PDS_Discount_abc123",
  "labelPosition": "above",
  "min": 0,
  "max": 100,
  "decimalPrecision": 2
}
```

- `min` / `max` — значения, выходящие за диапазон, будут скорректированы при потере фокуса.
- `decimalPrecision` — если не указано, формат определяется типом колонки в модели данных (integer vs float).

</details>

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `NumberInput_<hash>` | `NumberInput_2k6l4yz` |

</details>

---

## Частые ошибки

- **`visible: true` вместо `"$Attr"`** — если задать литерал `true`, видимость нельзя будет изменить из handler-а. Используйте привязку `"$AttributeName"`, если планируете управлять видимостью динамически.

- **Не указан `decimalPrecision` для дробных полей** — без явного указания точность определяется моделью данных. Если колонка целочисленная, дробная часть будет отброшена, даже если пользователь ввёл десятичное число.

- **Пустые `placeholder` и `tooltip`** — если значение не нужно, лучше не указывать свойство вовсе, чем передавать пустую строку.

---

## См. также

- [crt.Button](button.md)
- [Каталог элементов](index.md)
- [viewConfigDiff](../page/view-config.md)
- [viewModelConfigDiff](../page/viewmodel-config.md)
