# crt.Input

Текстовое поле ввода -- самый распространённый элемент формы в Creatio Freedom UI.
Используется для однострочного и многострочного ввода текста.

---

## Пример

Простое текстовое поле с привязкой к полю DataSource:

**viewConfigDiff:**
```json
{
  "operation": "insert",
  "name": "Input_0w33uwq",
  "values": {
    "type": "crt.Input",
    "multiline": false,
    "label": "$Resources.Strings.SysUserProfileDS_FullName_udfiku2",
    "labelPosition": "auto",
    "control": "$SysUserProfileDS_FullName_udfiku2"
  },
  "parentName": "LoginEmailFlexContainer",
  "propertyName": "items",
  "index": 0
}
```

Поле привязано к атрибуту `SysUserProfileDS_FullName_udfiku2` через префикс `$`. Атрибут автоматически создаётся в [`viewModelConfigDiff`](../page/viewmodel-config.md) при привязке к DataSource.

---

## Свойства

| Свойство | Тип | Обязательное | По умолчанию | Описание |
|----------|-----|:---:|---|---|
| `type` | `"crt.Input"` | да | -- | Тип элемента |
| `multiline` | `boolean` | да | -- | `false` = однострочное, `true` = многострочное поле |
| `label` | `string` | да | -- | Подпись поля. `$Resources.Strings.X` -- ссылка на ресурс страницы |
| `labelPosition` | [`LabelPosition`](#labelposition) | да | -- | Расположение подписи |
| `control` | `string` | да | -- | Привязка данных: `$DataSource_Field_hash`. См. [атрибуты](../page/viewmodel-config.md) |
| `visible` | `boolean` \| `"$Attr"` | нет | `true` | Видимость. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `readonly` | `boolean` \| `"$Attr"` | нет | `false` | Только чтение. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `disabled` | `boolean` \| `"$Attr"` | нет | `false` | Блокировка поля. Поддерживает [привязку к атрибуту](../page/viewmodel-config.md) |
| `placeholder` | `string` | нет | -- | Текст-подсказка в пустом поле. [`#ResourceString(...)#`](../page/view-config.md) |
| `tooltip` | `string` | нет | -- | Всплывающая подсказка при наведении |
| `inputType` | [`InputType`](#inputtype) | нет | `"text"` | Тип ввода |
| `appearance` | [`Appearance`](#appearance) | нет | `"legacy"` | Визуальный стиль |
| `autocomplete` | `string` | нет | `"off"` | Автозаполнение браузера |
| `rowModeSizePx` | `number` | нет | `320` | Ширина поля (px), при которой layout переключается с вертикального на горизонтальный |
| `layoutConfig` | `LayoutConfig` | нет | -- | Позиция в GridContainer |

<details markdown>
<summary><b>labelPosition</b></summary>

| Значение | Описание |
|----------|----------|
| `"auto"` | Автоматическое размещение (по умолчанию) |
| `"above"` | Подпись над полем |
| `"left"` | Подпись слева от поля |
| `"right"` | Подпись справа от поля |
| `"hidden"` | Подпись скрыта |

</details>

<details markdown>
<summary><b>inputType</b></summary>

| Значение | Описание |
|----------|----------|
| `"text"` | Обычный текст (по умолчанию) |
| `"password"` | Маскированный ввод (пароль) |

</details>

<details markdown>
<summary><b>appearance</b></summary>

| Значение | Описание |
|----------|----------|
| `"legacy"` | Только нижняя граница (по умолчанию) |
| `"outline"` | Рамка вокруг поля |

</details>

---

## Многострочный режим

Свойство `multiline` определяет режим поля:

- `false` -- однострочный ввод (стандартный `<input>`)
- `true` -- многострочный ввод (аналог `<textarea>`)

```json
{
  "operation": "insert",
  "name": "Input_Description",
  "values": {
    "type": "crt.Input",
    "multiline": true,
    "label": "$Resources.Strings.PDS_Description_abc123",
    "labelPosition": "above",
    "control": "$PDS_Description_abc123"
  },
  "parentName": "GridContainer_main",
  "propertyName": "items",
  "index": 2
}
```

---

## Управление полем

Через привязку к атрибутам можно управлять свойствами `visible`, `readonly` и `disabled`.

**viewModelConfigDiff** -- атрибуты:
```json
{
  "operation": "merge",
  "path": ["attributes"],
  "values": {
    "IsNameVisible": { "value": true },
    "IsNameReadonly": { "value": false }
  }
}
```

**viewConfigDiff** -- привязка:
```json
{
  "operation": "merge",
  "name": "Input_0w33uwq",
  "values": {
    "visible": "$IsNameVisible",
    "readonly": "$IsNameReadonly"
  }
}
```

**handlers** -- управление при инициализации:
```javascript
{
  request: "crt.HandleViewModelInitRequest",
  handler: async (request, next) => {
    const status = await request.$context.PDS_Status_abc123;
    // Скрываем поле, если статус не "Черновик"
    request.$context.IsNameVisible = status?.displayValue === "Draft";
    // Блокируем редактирование для активных записей
    request.$context.IsNameReadonly = status?.displayValue === "Active";
    return await next?.handle(request);
  }
}
```

> Если свойство задано как литерал (`visible: true`), его **нельзя** изменить из кода. Обязательно замените на привязку `"$AttributeName"`.

---

## Чтение и запись значения

**Чтение** -- через `await` (значение может быть загружено асинхронно):
```javascript
const name = await request.$context.SysUserProfileDS_FullName_udfiku2;
```

**Запись** -- прямое присвоение:
```javascript
request.$context.SysUserProfileDS_FullName_udfiku2 = "Новое значение";
```

---

## Реакция на изменение поля

Основной реактивный механизм -- `crt.HandleViewModelAttributeChangeRequest`. Срабатывает при каждом изменении значения атрибута.

```javascript
handlers: [
  {
    request: "crt.HandleViewModelAttributeChangeRequest",
    handler: async (request, next) => {
      if (request.attributeName === "PDS_Name_abc123") {
        const name = await request.$context.PDS_Name_abc123;

        // Обновляем зависимые поля
        request.$context.PDS_DisplayName_def456 = name?.toUpperCase();

        // Управляем видимостью другого поля
        request.$context.IsDescriptionVisible = !!name;
      }
      // Обязательно вызываем следующий обработчик в цепочке
      return await next?.handle(request);
    }
  },
  // ... другие обработчики
]
```

> Фильтрация по `request.attributeName` обязательна -- без неё обработчик будет срабатывать на **каждое** изменение **любого** атрибута на странице.

---

## Динамическое управление валидацией

Валидаторы можно включать и отключать в runtime через методы `$context`:

```javascript
{
  request: "crt.HandleViewModelAttributeChangeRequest",
  handler: async (request, next) => {
    if (request.attributeName === "PDS_Type_abc123") {
      const type = await request.$context.PDS_Type_abc123;
      const requiresDescription = type?.displayValue === "Custom";

      if (requiresDescription) {
        // Включаем обязательность поля "Описание"
        request.$context.enableAttributeValidator(
          'PDS_Description_def456', 'required'
        );
      } else {
        // Отключаем обязательность и очищаем значение
        request.$context.disableAttributeValidator(
          'PDS_Description_def456', 'required'
        );
        request.$context.PDS_Description_def456 = "";
      }
    }
    return await next?.handle(request);
  }
}
```

> При отключении валидатора рекомендуется также очищать значение поля, чтобы не сохранять невалидные данные.

---

<details markdown>
<summary><b>Соглашения об именовании</b></summary>

| Что | Паттерн | Пример |
|-----|---------|--------|
| Имя элемента | `Input_<hash>` | `Input_0w33uwq` |
| Привязка control (DataSource) | `$<DataSource>_<Field>_<hash>` | `$SysUserProfileDS_FullName_udfiku2` |
| Привязка control (строковый атрибут) | `$StringAttribute_<hash>` | `$StringAttribute_14qqemi` |
| Атрибут видимости | `Is<Name>Visible` | `IsNameVisible` |
| Атрибут readonly | `Is<Name>Readonly` | `IsNameReadonly` |

</details>

---

## Частые ошибки

- **Забыли `next?.handle(request)`** в обработчике `HandleViewModelAttributeChangeRequest` -- цепочка обработчиков прервётся, другие поля перестанут реагировать на изменения.

- **Нет фильтрации по `request.attributeName`** -- обработчик срабатывает на изменение каждого атрибута страницы, что приводит к бесконечным циклам и падению производительности.

- **`visible`/`readonly` заданы литералом** (`true`/`false`) вместо привязки `"$Attr"` -- значение невозможно изменить из кода. Заменяйте на `"$IsFieldVisible"` заранее.

- **При отключении валидатора не очищают поле** -- в результате сохраняется невалидное значение, которое прошло проверку при активном валидаторе, но стало некорректным после его отключения.

---

## См. также

- [crt.Button](button.md) -- кнопки
- [viewModelConfigDiff -- атрибуты](../page/viewmodel-config.md)
- [handlers -- обработчики](../page/handlers.md)
- [view-config -- конфигурация UI](../page/view-config.md)
