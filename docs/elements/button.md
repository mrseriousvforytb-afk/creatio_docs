# crt.Button

---

## Пример

Кнопка с кастомным request-ом и обработчиком:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Button_Import",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(Button_Import_caption)#",
    "color": "accent",
    "disabled": false,
    "size": "large",
    "iconPosition": "left-icon",
    "icon": "upload-button-icon",
    "visible": "$IsImportButtonVisible",
    "clicked": { "request": "usr.ImportDuplicates" }
  },
  "parentName": "ActionButtonsContainer",
  "propertyName": "items",
  "index": 0
}
```

**handlers:**
```javascript
handlers: [
  {
    request: "usr.ImportDuplicates",
    handler: async (request, next) => {
      const selection = await request.$context.DataGrid_abc123_SelectionState;
      const httpService = new sdk.HttpClientService();
      await httpService.post(
        "rest/CustomService/ImportDuplicates",
        selection.selected
      );
      return await next?.handle(request);
    }
  },
  // ... другие обработчики
]
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.Button"` | да | — | Тип элемента |
| `caption` | `string` | да | — | Текст кнопки. [`#ResourceString(...)#`](../page/view-config.md) — ссылка на ресурсы страницы |
| `color` | [`ButtonColor`](#color) | да | — | Цветовая схема |
| `disabled` | `boolean` \| `"$Attr"` | да | `false` | Неактивна. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `size` | [`ButtonSize`](#size) | да | — | Размер |
| `iconPosition` | [`IconPosition`](#iconposition) | да | — | Режим отображения иконки/текста |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `icon` | `string` \| `null` | нет | `null` | Идентификатор иконки |
| `clicked` | [`ClickedConfig`](#clicked) | нет | — | Действие при клике |
| `clickMode` | `"default"` | нет | — | Всегда `"default"`, если указан |
| `shape` | `"default"` \| `"rounded"` | нет | `"default"` | Форма кнопки |
| `displayType` | `string` | нет | — | Вариант отображения |
| `title` | `string` | нет | — | Tooltip при наведении |
| `tooltip` | `string` | нет | — | Расширенный tooltip |
| `ariaLabel` | `string` | нет | = `caption` | Значение `aria-label` |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в GridContainer |

<details markdown>
<summary><b>color</b></summary>

| Значение | Цвет |
|----------|------|
| `"primary"` | Зелёная |
| `"accent"` | Синяя |
| `"default"` | Серая |
| `"outline"` | С рамкой |
| `"warn"` | Красная |

</details>

<details markdown>
<summary><b>size</b></summary>

| Значение | |
|----------|-|
| `"large"` | Большая |
| `"medium"` | Средняя |
| `"small"` | Маленькая |

</details>

<details markdown>
<summary><b>iconPosition</b></summary>

| Значение | |
|----------|-|
| `"only-text"` | Только текст |
| `"only-icon"` | Только иконка |
| `"left-icon"` | Иконка слева |
| `"right-icon"` | Иконка справа |

</details>

<details markdown>
<summary><b>Доступные иконки</b></summary>

| Иконка | Назначение |
|--------|-----------|
| `pencil-button-icon` | Редактировать |
| `checkmark-icon` | Подтвердить |
| `close-button-icon` | Закрыть / Отменить |
| `reload-icon` | Обновить |
| `upload-button-icon` | Загрузить файл |
| `download-button-icon` | Скачать |
| `gear-button-icon` | Настройки |
| `email-button-icon` | Отправить email |
| `flag-button-icon` | Пометить |
| `tag-icon` | Тег |

</details>

---

## Clicked

Свойство `clicked` определяет, какой request отправляется при нажатии.

**Вызов базового request-а:**
```json
"clicked": {
  "request": "crt.SaveNotCloseRequest"
}
```

> Полный список базовых request-ов и их параметров — [Справочник request-ов](../page/handlers.md)

**Вызов кастомного request-а:**
```json
"clicked": {
  "request": "usr.ImportDuplicates"
}
```

Для кастомного request-а нужен обработчик в секции `handlers` — см. [Пример](#пример) выше.

---

## Управление кнопкой

Через привязку к атрибутам можно управлять двумя свойствами: `visible` и `disabled`.

**viewModelConfigDiff** — атрибуты:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "IsImportVisible": { "value": false },
    "IsImportDisabled": { "value": true }
  }
}
```

**viewConfigDiff** — привязка:
```json
{
  "operation": "merge",
  "name": "Button_Import",
  "values": {
    "visible": "$IsImportVisible",
    "disabled": "$IsImportDisabled"
  }
}
```

**handlers** — управление:
```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    const status = await request.$context.PDS_Status_abc123;
    request.$context.IsImportVisible = status?.displayValue === "Draft";
    request.$context.IsImportDisabled = false;
    return await next?.handle(request);
  }
}
```

> ⚠️ Если свойство задано как литерал (`visible: true`), его **нельзя** изменить из кода. Обязательно замените на привязку `"$AttributeName"`.

---

## Обработчик клика

Если базовых request-ов недостаточно — создайте свой.

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Button_Export",
  "values": {
    "...": "...",
    "clicked": { "request": "usr.ExportRecords" }
  },
  "parentName": "ActionButtonsContainer",
  "propertyName": "items"
}
```

**handlers:**
```javascript
handlers: [
  {
    request: "usr.ExportRecords",
    handler: async (request, next) => {
      const name = await request.$context.PDS_Name_abc123;
      console.log("Exporting: " + name);
      return await next?.handle(request);
    }
  },
  // ... другие обработчики
]
```

---

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя кнопки | `Button_<Действие>` | `Button_Save`, `Button_Publish` |
| Кнопка добавления в гриде | `GridDetailAddBtn_<hash>` | `GridDetailAddBtn_0amajnl` |
| Кнопка обновления в гриде | `GridDetailRefreshBtn_<hash>` | `GridDetailRefreshBtn_zj8rxqc` |
| Атрибут видимости | `Is<Name>Visible` | `IsPublishVisible` |
| Атрибут доступности | `IsButton<Name>Disabled` | `IsButtonDoneHoldDisabled` |

</details>

---

## Частые ошибки

- **Забыли `next?.handle(request)`** в кастомном handler-е — цепочка обработчиков прервётся, стандартная логика не выполнится.

---

## См. также

<!-- TODO: ссылки будут добавлены по мере наполнения -->
