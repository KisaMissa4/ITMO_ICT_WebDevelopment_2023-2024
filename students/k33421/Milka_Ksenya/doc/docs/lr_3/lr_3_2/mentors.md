# Mentors API

### URL

`/api/mentors`

### Methods

- `GET`: Получение списка гидов или деталей об одном гиде.
- `POST`: Создание нового гида.
- `PUT`/`PATCH`: Обновление данных гида.
- `DELETE`: Удаление гида.

### Authentication

- Требуется аутентификация для доступа ко всем методам.
- Только владельцы профиля или администраторы могут обновлять или удалять профили.

### Filters and Search

- Фильтрация по сертификации (`certification`) и годам опыта (`years_of_experience`).
- Поиск по электронной почте (`user__email`), имени (`user__first_name`), фамилии (`user__last_name`).

## Endpoints

### List mentors

- **URL**: `/api/mentors/`
- **Method**: `GET`
- **Query Parameters**:
    - `certification`: Фильтр по сертификации.
    - `years_of_experience`: Фильтр по годам опыта.
    - `search`: Строка для поиска.

### Create Guide

- **URL**: `/api/mentors/`
- **Method**: `POST`
- **Body Parameters**:
    - `certification`: String (Сертификация)
    - `years_of_experience`: Integer (Годы опыта)
    - ... (другие поля)

### Retrieve Guide

- **URL**: `/api/mentors/{id}/`
- **Method**: `GET`
- **URL Parameters**:
    - `id`: ID гида

### Update Guide

- **URL**: `/api/mentors/{id}/`
- **Method**: `PUT`/`PATCH`
- **URL Parameters**:
    - `id`: ID гида
- **Body Parameters**:
    - ... (поля для обновления)

### Delete Guide

- **URL**: `/api/mentors/{id}/`
- **Method**: `DELETE`
- **URL Parameters**:
    - `id`: ID гида

## Examples

### Пример запроса на получение списка гидов

GET `/api/mentors/`

### Пример ответа

```json
[
  {
    "id": 1,
    "certification": "Certified Mountain Guide",
    "years_of_experience": 5,
    ...
  },
  ...
]
```

### Пример запроса на создание гида

POST /api/mentors/
Content-Type: application/json

```json
{
  "certification": "Advanced Guide",
  "years_of_experience": 10,
  ...
}
```

### Пример ответа

```json
{
  "id": 2,
  "certification": "Advanced Guide",
  "years_of_experience": 10,
  ...
}

```