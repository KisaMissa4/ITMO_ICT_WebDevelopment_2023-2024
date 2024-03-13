## Основные Данные (Data)

- **mountain**: Объект, хранящий информацию о выбранной горе.
- **routes**: Массив маршрутов, связанных с этой горой.

## Методы (Methods)

### fetchMountainDetails

```javascript
fetchMountainDetails()
{
    const mountainId = this.$route.params.id;
    api.get(`mountains/${mountainId}/`)
        .then(response => {
            this.mountain = response.data;
            this.fetchRoutes(this.mountain.name);
        })
        .catch(error => {
            console.error("There was an error fetching the mountain details:", error);
        });
}
```

Этот метод выполняет запрос к API для получения деталей о горе по её идентификатору. После успешного получения данных о
горе, метод вызывает `fetchRoutes` для загрузки связанных маршрутов.

### fetchRoutes

```javascript
fetchRoutes(mountainName)
{
    api.get('routes/', {params: {search: mountainName}})
        .then(response => {
            this.routes = response.data;
        })
        .catch(error => {
            console.error("There was an error fetching the routes:", error);
        });
}
```

`fetchRoutes` запрашивает список маршрутов, связанных с горой. Он использует имя горы как параметр поиска для фильтрации
маршрутов.

## Жизненный Цикл (Lifecycle Hook)

### mounted

```javascript
mounted()
{
    this.fetchMountainDetails();
}
```

При монтировании компонента вызывается метод `fetchMountainDetails` для загрузки деталей о горе и связанных с ней
маршрутов.

## Описание Шаблона (Template)

- В разделе **Детали Горы** отображается информация о выбранной горе, включая её название, высоту и местоположение.
- В разделе **Список Маршрутов** представлен список всех маршрутов, связанных с этой горой.