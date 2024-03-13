### Данные (Data)

- **routes**: Массив для хранения списка маршрутов.
- **mountains**: Массив для хранения списка гор.
- **dialog**: Управляет отображением модального окна для формы.
- **currentRoute**: Объект для хранения данных текущего редактируемого или добавляемого маршрута.
- **formTitle**: Заголовок формы в модальном окне.
- **valid**: Статус валидации формы.
- **serverError**: Сообщение об ошибке от сервера.
- **headers**: Заголовки столбцов таблицы маршрутов.

### Методы (Methods)

#### Общие HTTP-запросы

- **fetchData**: Универсальный метод для выполнения HTTP-запросов.
    - `endpoint`: URL-адрес для запроса.
    - `dataProperty`: Свойство компонента для сохранения полученных данных.

#### Запросы к API

- **fetchRoutes**: Загружает список маршрутов.
- **fetchMountains**: Загружает список гор.

#### Управление Модальным Окном

- **openDialogForNewRoute**: Открывает модальное окно для добавления нового маршрута.
- **editRoute**: Открывает модальное окно для редактирования существующего маршрута.
- **deleteRoute**: Удаляет выбранный маршрут.
- **closeDialog**: Закрывает модальное окно.
- **saveRoute**: Сохраняет добавленный или отредактированный маршрут.

### Жизненный Цикл (Lifecycle Hooks)

- **mounted**: Вызывается при инициализации компонента для загрузки данных о маршрутах и горах.

## Подробное Описание Методов

### fetchData

```javascript
async
fetchData(endpoint, dataProperty)
{
    try {
        const response = await api.get(endpoint);
        this[dataProperty] = response.data;
    } catch (error) {
        console.error(`There was an error fetching data from ${endpoint}:`, error);
    }
}
```

Метод `fetchData` обобщает логику выполнения HTTP-GET запросов и сохранения ответа в соответствующем свойстве данных
компонента.

### Работа с Модальным Окном

```javascript
openDialogForNewRoute()
{
    this.currentRoute = {};
    this.formTitle = 'New Route';
    this.dialog = true;
}
editRoute(route)
{
    this.currentRoute = Object.assign({}, route);
    this.formTitle = `Edit ${route.name}`;
    this.dialog = true;
}
```

`openDialogForNewRoute` и `editRoute` устанавливают необходимые данные для модального окна и открывают его. При
редактировании существующего маршрута данные копируются, чтобы избежать прямой модификации объекта в массиве `routes`.

### Удаление Маршрута

```javascript
async
deleteRoute(routeId)
{
    try {
        await api.delete(`routes/${routeId}/`);
        await this.fetchRoutes();
    } catch (error) {
        this.serverError = error.response.data;
    }
}
```

`deleteRoute` отправляет запрос на удаление маршрута и обновляет список маршрутов.

### Сохранение Маршрута

```javascript
async
saveRoute()
{
    this.serverError = null;
    try {
        if (this.currentRoute.id) {
            await api.patch(`routes/${this.currentRoute.id}/`, this.currentRoute);
        } else {
            await api.post('routes/', this.currentRoute);
        }


        this.dialog = false;
        await this.fetchRoutes();
    } catch (error) {
        this.serverError = error.response.data;
    }
}
```

Метод `saveRoute` определяет, нужно ли создать новый маршрут или обновить существующий, и выполняет соответствующий
HTTP-запрос.