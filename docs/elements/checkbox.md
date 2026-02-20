# crt.Checkbox

Булево поле-чекбокс. Привязывается к логическому (`boolean`) полю объекта через свойство `control`.

> Обратите внимание на регистр типа: `"crt.Checkbox"` — заглавная **C**, строчная **b**.

---

## Пример

Чекбокс "Выпущен" на странице редактирования:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Checkbox_IsReleased",
  "values": {
    "type": "crt.Checkbox",
    "label": "$Resources.Strings.PDS_IsReleased_7ztqh8t",
    "labelPosition": "right",
    "control": "$PDS_IsReleased_7ztqh8t",
    "visible": true,
    "readonly": false,
    "placeholder": "",
    "tooltip": ""
  },
  "parentName": "FlexContainer_45mm66n",
  "propertyName": "items",
  "index": 1
}
```

**viewModelConfigDiff** — атрибут для булева поля:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "PDS_IsReleased_7ztqh8t": {
      "modelConfig": { "path": "PDS.IsReleased" }
    }
  }
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.Checkbox"` | да | — | Тип элемента |
| `label` | `string` | да | — | Подпись поля. [`$Resources.Strings.*`](../page/view-config.md) — ссылка на ресурсы |
| `labelPosition` | [`LabelPosition`](#labelposition) | да | `"right"` | Расположение подписи |
| `control` | `string` | да | — | Привязка к булеву полю. Формат: `"$PDS_FieldName_hash"` |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `readonly` | `boolean` \| `"$Attr"` | нет | `false` | Только чтение. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `disabled` | `boolean` \| `"$Attr"` | нет | `false` | Заблокирован для редактирования |
| `inversed` | `boolean` | нет | `false` | Инвертирует визуальный стиль чекбокса |
| `placeholder` | `string` | нет | `""` | Плейсхолдер (обычно пустой для чекбокса) |
| `tooltip` | `string` | нет | `""` | Всплывающая подсказка при наведении |
| `ariaLabel` | `string` | нет | — | Значение `aria-label` для accessibility |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в [GridContainer](./gridcontainer.md) |

<details markdown>
<summary><b>labelPosition</b></summary>

| Значение | Описание |
|----------|----------|
| `"right"` | Подпись справа от чекбокса (по умолчанию, используется практически всегда) |
| `"left"` | Подпись слева |
| `"above"` | Подпись сверху |
| `"auto"` | Автоматическое размещение |
| `"hidden"` | Подпись скрыта |

> В реальных страницах Creatio `labelPosition` для чекбокса всегда равен `"right"` — подпись отображается после чекбокса.

</details>

<details markdown>
<summary><b>layoutConfig</b></summary>

Позиция элемента внутри `GridContainer`. Задаётся объектом:

```json
"layoutConfig": {
  "column": 3,
  "row": 2,
  "colSpan": 1,
  "rowSpan": 1
}
```

| Свойство | Тип | Описание |
|----------|-----|----------|
| `column` | `number` | Номер колонки (начинается с 1) |
| `row` | `number` | Номер строки (начинается с 1) |
| `colSpan` | `number` | Сколько колонок занимает |
| `rowSpan` | `number` | Сколько строк занимает |

</details>

<details markdown>
<summary><b>События</b></summary>

| Событие | Описание |
|---------|----------|
| `valueChange` | Срабатывает при изменении значения чекбокса |

</details>

---

## Управление чекбоксом

Через привязку к атрибутам можно динамически управлять свойствами `visible` и `readonly`.

**viewModelConfigDiff** — атрибуты:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "IsCheckboxVisible": { "value": true }
  }
}
```

**viewConfigDiff** — привязка:
```json
{
  "operation": "merge",
  "name": "Checkbox_IsReleased",
  "values": {
    "visible": "$IsCheckboxVisible"
  }
}
```

**handlers** — переключение видимости:
```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    const status = await request.$context.PDS_Status_abc123;
    // Показываем чекбокс только для черновиков
    request.$context.IsCheckboxVisible = status?.displayValue === "Draft";
    return await next?.handle(request);
  }
}
```

> Если свойство задано как литерал (`visible: true`), его **нельзя** изменить из кода. Обязательно замените на привязку `"$AttributeName"`.

---

## Реакция на изменение значения

Чтобы выполнить логику при переключении чекбокса, используйте `HandleViewModelAttributeChangeRequest`:

```javascript
{
  request: "crt.HandleViewModelAttributeChangeRequest",
  handler: async (request, next) => {
    if (request.attributeName === "PDS_IsReleased_7ztqh8t") {
      const isReleased = await request.$context.PDS_IsReleased_7ztqh8t;
      // Например, блокируем другие поля при включённом чекбоксе
      request.$context.IsNameReadonly = isReleased;
    }
    return await next?.handle(request);
  }
}
```

---

<details markdown>
<summary><b>Скрытый чекбокс с layoutConfig</b></summary>

Чекбокс внутри `GridContainer` со скрытой видимостью:

```json
{
  "operation": "insert",
  "name": "Checkbox_rjajaf1",
  "values": {
    "layoutConfig": { "column": 3, "row": 2, "colSpan": 1, "rowSpan": 1 },
    "type": "crt.Checkbox",
    "label": "$Resources.Strings.AnnouncementDS_IsAllDay_fr1klmk",
    "labelPosition": "right",
    "control": "$AnnouncementDS_IsAllDay_fr1klmk",
    "visible": false,
    "readonly": false,
    "placeholder": "",
    "tooltip": ""
  },
  "parentName": "GridContainer_MainData",
  "propertyName": "items",
  "index": 5
}
```

Скрытые чекбоксы часто используются как вспомогательные — значение существует в модели, но UI-элемент не отображается. Управление значением происходит через handler-ы.

</details>

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `Checkbox_<hash>` | `Checkbox_rjajaf1`, `Checkbox_jdx6jt7` |
| Привязка control | Булевы поля (`Is*`, `Has*`) | `$PDS_IsReleased_7ztqh8t`, `$PDS_IsPlanTask_l9g4i21` |
| Атрибут видимости | `Is<Name>Visible` | `IsCheckboxVisible` |

</details>

---

## Частые ошибки

- **Неверный регистр типа** — `"crt.CheckBox"` или `"crt.checkbox"` не сработает. Правильно: `"crt.Checkbox"`.

- **Забыли `labelPosition: "right"`** — для чекбокса подпись должна быть справа. Если указать `"left"` или `"above"`, подпись будет отображаться непривычно для пользователя.

- **Привязка к нелогическому полю** — `control` должен ссылаться на булево поле объекта. Привязка к строковому или числовому полю приведёт к некорректному поведению.

- **Литерал вместо привязки** — если задать `visible: true` литералом, потом нельзя скрыть чекбокс из handler-а. Используйте `"$AttrName"` для динамического управления.

---

## См. также

- [Атрибуты и привязка данных](../page/viewmodel-config.md)
- [Обработчики (handlers)](../page/handlers.md)
- [GridContainer](./gridcontainer.md) — контейнер с `layoutConfig`
