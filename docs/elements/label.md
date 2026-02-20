# crt.Label

Статический текст: заголовки, описания, информационные надписи на странице.

---

## Пример

Заголовок секции:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Label_izx6smh",
  "values": {
    "type": "crt.Label",
    "caption": "#MacrosTemplateString(#ResourceString(Label_izx6smh_caption)#)#",
    "labelType": "headline-3",
    "labelThickness": "semibold",
    "labelEllipsis": false,
    "labelColor": "auto",
    "labelBackgroundColor": "transparent",
    "labelTextAlign": "start",
    "visible": true
  },
  "parentName": "FlexContainer_c9855nx",
  "propertyName": "items",
  "index": 0
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.Label"` | да | — | Тип элемента |
| `caption` | `string` | да | — | Текст. Всегда через [макрос-обёртку](#caption) |
| `labelType` | [`LabelType`](#labeltype) | нет | `"body-1"` | Типографический стиль |
| `labelThickness` | [`LabelThickness`](#labelthickness) | нет | `"regular"` | Толщина шрифта |
| `labelEllipsis` | `boolean` | нет | `false` | Обрезать текст с многоточием при переполнении |
| `labelColor` | `string` | нет | `"auto"` | Цвет текста |
| `labelBackgroundColor` | `string` | нет | `"transparent"` | Цвет фона |
| `labelTextAlign` | [`LabelTextAlign`](#labeltextalign) | нет | `"start"` | Выравнивание текста |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в GridContainer |

<details markdown>
<summary><b>labelType</b></summary>

| Значение | Описание |
|----------|----------|
| `"headline-3"` | Заголовок секции |
| `"body-1"` | Основной текст |
| `"caption"` | Мелкий вспомогательный текст |

</details>

<details markdown>
<summary><b>labelThickness</b></summary>

| Значение | Описание |
|----------|----------|
| `"regular"` | Обычный |
| `"semibold"` | Полужирный |
| `"bold"` | Жирный |

</details>

<details markdown>
<summary><b>labelTextAlign</b></summary>

| Значение | Описание |
|----------|----------|
| `"start"` | По левому краю |
| `"center"` | По центру |
| `"end"` | По правому краю |

</details>

---

## Caption

Текст Label-а **всегда** оборачивается в двойной макрос:

```
"#MacrosTemplateString(#ResourceString(Label_hash_caption)#)#"
```

- Внутренний `#ResourceString(...)#` — ссылка на строку ресурсов страницы.
- Внешний `#MacrosTemplateString(...)#` — обработка макросов (подстановка значений, если в строке есть динамические вставки).

> Это отличает Label от Button и других элементов, где caption использует только `#ResourceString(...)#` без внешней обёртки.

---

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `Label_<hash>` | `Label_izx6smh` |
| Строка ресурсов | `Label_<hash>_caption` | `Label_izx6smh_caption` |

</details>

---

## Частые ошибки

- **Забыли внешний `#MacrosTemplateString(...)#`** — caption не будет обрабатывать макросы. Используйте полный двойной шаблон даже для статического текста.
- **Привязка `"$Attr"` в caption** — Label не поддерживает привязку атрибутов в caption. Для динамического текста задайте значение строки ресурсов программно или используйте Input с `readonly`.

---

## См. также

- [crt.Button](button.md) — кнопка с caption без макрос-обёртки
- [viewConfigDiff](../page/view-config.md) — структура визуальной конфигурации
