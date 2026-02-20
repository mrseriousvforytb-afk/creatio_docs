# crt.Button

Кнопка действия. Самый частый интерактивный элемент — встречается 400+ раз в стандартных страницах Creatio.

---

## Минимальный пример

```json
{
  "operation": "insert",
  "name": "Button_Save",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(SaveButton_caption)#",
    "color": "primary",
    "clicked": { "request": "crt.SaveNotCloseRequest" }
  },
  "parentName": "ActionButtonsContainer",
  "propertyName": "items",
  "index": 0
}
```

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.Button"` | да | — | Тип элемента |
| `caption` | `string` | да | — | Текст кнопки. Обычно `#ResourceString(...)#` |
| `color` | `ButtonColor` | да | — | Цветовая схема |
| `disabled` | `boolean \| "$Attr"` | да | `false` | Заблокирована. Поддерживает привязку к атрибуту |
| `size` | `ButtonSize` | да | — | Размер |
| `iconPosition` | `IconPosition` | да | — | Режим отображения иконки/текста |
| `visible` | `boolean \| "$Attr"` | нет | `true` | Видимость. Поддерживает привязку к атрибуту |
| `icon` | `string \| null` | нет | `null` | Идентификатор иконки |
| `clicked` | `ClickedConfig` | нет | — | Конфигурация обработчика клика |
| `clickMode` | `"default"` | нет | — | Всегда `"default"`, если указан |
| `shape` | `"default" \| "rounded"` | нет | `"default"` | Форма кнопки |
| `displayType` | `string` | нет | — | Вариант отображения |
| `title` | `string` | нет | — | Tooltip при наведении |
| `tooltip` | `string` | нет | — | Расширенный tooltip |
| `ariaLabel` | `string` | нет | = `caption` | Значение `aria-label` |
| `layoutConfig` | `LayoutConfig` | нет | — | Позиция в GridContainer |

### color — цветовая схема

| Значение | Описание | Когда использовать |
|----------|----------|--------------------|
| `"primary"` | Зелёная | Главное действие (Сохранить) |
| `"accent"` | Синяя | Вторичное действие (Открыть, Настроить) |
| `"default"` | Серая | Нейтральное действие (Обновить, тулбар грида) |
| `"outline"` | С рамкой | Менее заметное действие |
| `"warn"` | Красная | Деструктивное действие (Удалить, Отклонить) |

### size — размер

| Значение | Описание |
|----------|----------|
| `"large"` | Большая (основные действия страницы) |
| `"medium"` | Средняя (тулбар грида) |
| `"small"` | Маленькая |

### iconPosition — режим иконки

| Значение | Описание |
|----------|----------|
| `"only-text"` | Только текст |
| `"only-icon"` | Только иконка (нужен `icon`) |
| `"left-icon"` | Иконка слева от текста |
| `"right-icon"` | Иконка справа от текста |

### Доступные иконки

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

---

## Обработка клика (clicked)

Свойство `clicked` определяет, какой request отправляется при нажатии:

```json
"clicked": {
  "request": "crt.SaveNotCloseRequest"
}
```

С параметрами:

```json
"clicked": {
  "request": "crt.OpenPageRequest",
  "params": {
    "schemaName": "Notifications_ListPage"
  }
}
```

### Типичные request-ы для кнопок

| Request | Действие | Параметры |
|---------|---------|-----------|
| `crt.SaveNotCloseRequest` | Сохранить без закрытия | — |
| `crt.OpenPageRequest` | Открыть страницу | `{ schemaName }` |
| `crt.RunBusinessProcessRequest` | Запустить БП | `{ processName, processRunType, ... }` |
| `crt.LoadDataRequest` | Перезагрузить данные | `{ config: { loadType }, dataSourceName }` |
| `crt.UploadFileRequest` | Загрузить файл | `{ viewElementName }` |
| `crt.ChangeStateRequest` | Сменить стадию | `{ newState: "GUID" }` |
| `crt.ButtonStepDone` | Завершить шаг workflow | — |
| Кастомный request | Своя логика | любые |

---

## Примеры

### Кнопка с иконкой и текстом

```json
{
  "operation": "insert",
  "name": "Button_Settings",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(Button_Settings_caption)#",
    "color": "accent",
    "disabled": false,
    "size": "large",
    "iconPosition": "left-icon",
    "icon": "gear-button-icon",
    "clicked": {
      "request": "crt.OpenPageRequest",
      "params": { "schemaName": "Notifications_ListPage" }
    }
  },
  "parentName": "MainHeader",
  "propertyName": "items",
  "index": 2
}
```

### Кнопка-иконка (тулбар грида)

Типичный паттерн для кнопок обновления и добавления в тулбаре DataGrid:

```json
{
  "operation": "insert",
  "name": "GridDetailRefreshBtn_zj8rxqc",
  "values": {
    "type": "crt.Button",
    "icon": "reload-icon",
    "iconPosition": "only-icon",
    "color": "default",
    "size": "medium",
    "clicked": {
      "request": "crt.LoadDataRequest",
      "params": {
        "config": { "loadType": "reload" },
        "dataSourceName": "FileList_fiaa8zeDS"
      }
    }
  },
  "parentName": "FlexContainer_oa5bbe6",
  "propertyName": "items",
  "index": 1
}
```

### Кнопка запуска бизнес-процесса

```json
{
  "operation": "insert",
  "name": "Button_Publish",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(Button_Publish_caption)#",
    "color": "accent",
    "disabled": false,
    "size": "large",
    "iconPosition": "only-text",
    "clicked": {
      "request": "crt.RunBusinessProcessRequest",
      "params": {
        "processName": "PublishProcess",
        "processRunType": "ForTheSelectedPage",
        "saveAtProcessStart": true,
        "showNotification": true,
        "recordIdProcessParameterName": "Questionnaire"
      }
    }
  },
  "parentName": "FlexContainer_3qfzk6d",
  "propertyName": "items",
  "index": 0
}
```

### Деструктивная кнопка (warn)

```json
{
  "operation": "insert",
  "name": "Button_Reject",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(Button_Reject_caption)#",
    "color": "warn",
    "disabled": false,
    "size": "large",
    "iconPosition": "left-icon",
    "icon": "close-button-icon",
    "visible": "$IsRejectButtonVisible",
    "clicked": {
      "request": "crt.ChangeStateRequest",
      "params": { "newState": "c2b39b62-67f8-427d-99f9-f8557884815a" }
    }
  },
  "parentName": "FlexContainer_3qfzk6d",
  "propertyName": "items",
  "index": 3
}
```

---

## Динамическое управление кнопкой

Чтобы менять видимость или доступность кнопки в runtime:

**Шаг 1.** Добавьте атрибут в `viewModelConfigDiff`:

```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "IsPublishVisible": { "value": false }
  }
}
```

**Шаг 2.** Привяжите свойство кнопки к атрибуту в `viewConfigDiff`:

```json
{
  "operation": "merge",
  "name": "Button_Publish",
  "values": {
    "visible": "$IsPublishVisible"
  }
}
```

**Шаг 3.** Управляйте из handler-а:

```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    const status = await request.$context.PDS_Status_abc123;
    request.$context.IsPublishVisible = status?.displayValue === "Draft";
    return await next?.handle(request);
  }
}
```

> Важно: Если свойство задано как литерал (`visible: true`), его **нельзя** изменить из кода. Обязательно замените на привязку `"$AttributeName"`.

---

## Кастомный обработчик клика

Если встроенных request-ов недостаточно, создайте свой:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Button_Import",
  "values": {
    "type": "crt.Button",
    "caption": "#ResourceString(Button_Import_caption)#",
    "color": "accent",
    "clicked": { "request": "usr.ImportDuplicates" }
  },
  "parentName": "ActionButtonsContainer",
  "propertyName": "items"
}
```

**handlers:**
```javascript
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
}
```

---

## Соглашения об именовании

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя кнопки | `Button_<Действие>` | `Button_Save`, `Button_Publish` |
| Кнопка добавления в гриде | `GridDetailAddBtn_<hash>` | `GridDetailAddBtn_0amajnl` |
| Кнопка обновления в гриде | `GridDetailRefreshBtn_<hash>` | `GridDetailRefreshBtn_zj8rxqc` |
| Атрибут видимости | `Is<Name>Visible` | `IsPublishVisible` |
| Атрибут доступности | `IsButton<Name>Disabled` | `IsButtonDoneHoldDisabled` |

---

## Частые ошибки

- **Забыли `next?.handle(request)`** в кастомном handler-е — цепочка обработчиков прервётся, стандартная логика не выполнится.
- **`visible: true` вместо привязки** — если позже нужно скрыть кнопку из кода, литеральное значение не изменить. Сразу используйте `"$IsButtonVisible"`.
- **`icon` без `iconPosition`** — иконка не отобразится. Укажите `"only-icon"`, `"left-icon"` или `"right-icon"`.

---

## См. также

- [Структура страницы — viewConfigDiff](../page/view-config.md)
- [Атрибуты и привязки](../page/viewmodel-config.md)
- [Обработчики](../page/handlers.md)
- [MenuItem](menuitem.md) — элементы выпадающего меню кнопки
