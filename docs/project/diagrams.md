# Диаграммы

## Диаграмма последовательности

Диаграмма показывает сценарий UC-01 "Получение персональных рекомендаций".

```mermaid
sequenceDiagram
    actor Viewer as Зритель
    participant System as Сервис рекомендаций
    participant DB as Хранилище
    participant Model as ML-модель

    Viewer->>System: Открыть раздел "Рекомендации"
    System->>DB: Загрузить оценки зрителя
    DB-->>System: История оценок
    alt Оценок нет
        System->>DB: Запросить популярные фильмы
        DB-->>System: Популярные фильмы
        System-->>Viewer: Показать популярные фильмы
    else Оценки есть
        System->>Model: Передать историю оценок
        Model-->>System: Фильмы и оценки соответствия
        System->>System: Убрать уже оценённые фильмы
        System-->>Viewer: Показать рекомендации
    end
```

## Диаграмма классов

```mermaid
classDiagram
    class User {
        +int id
        +string name
    }
    class Movie {
        +int id
        +string title
        +int year
        +string[] genres
        +float averageRating
    }
    class Rating {
        +int value
        +datetime ratedAt
    }
    class Recommendation {
        +float score
        +string reason
        +string modelVersion
    }

    User "1" --> "*" Rating : ставит
    Movie "1" --> "*" Rating : получает
    User "1" --> "*" Recommendation : получает
    Recommendation "*" --> "1" Movie : предлагает
```