## Схема
```mermaid
sequenceDiagram
    participant Front(App)
    participant Dobs
    participant EsiaService
    participant Esia
    Front(App)->>Dobs: Получить Url для аутентификации в ЕСИА
    Note right of Front(App): Передаётся url для редиректа в конце процесса (к примеру about)
    Dobs->>EsiaService: Получить Url для аутентификации в ЕСИА
    Note right of Dobs: Передаётся url для редиректа в бэк с токеном доступа к данным в ЕСИА
    EsiaService->>Dobs: Url аутентификации
    Dobs->>Front(App): Url аутентификации
    
    Front(App)->>Esia: Aутентификация в ЕСИА
    Note right of Front(App): Используется Url полученный на предыдущем шаге
    Esia->>Front(App): Страница для авторизации в ЕСИА
    Front(App)->>Esia: Данные для аутентификации в ЕСИА
    Esia->>Front(App): Запрос на доступ к данным
    Front(App)->>Esia: Разрешение или запрет на доступ к данным
    Esia->>Front(App): Редирект на наш бэк с токеном доступа к данным ЕСИА или ошибкой
    Note right of Dobs: В url добавляются параметры: токен доступа или ошибка

    Front(App)->>Dobs: Отрабатывает редирект
    Note right of Front(App): передаются параметры: токен доступа или ошибка
    Dobs-->>Front(App): Если есть ошибка то в ответ редиректим на {Url из начала процесса}?error={текст ошибки}
    Dobs->>EsiaService: Получить информацию о пользователе из ЕСИА
    Note right of Dobs: передается токен доступа
    EsiaService->>Esia: Получить информацию о пользователе из ЕСИА
    Note right of EsiaService: передается токен доступа
    Esia-->>EsiaService: Ошибка
    EsiaService-->>Dobs: Ошибка
    Dobs-->>Front(App): Если есть ошибка то в ответ редиректим на {Url из начала процесса}?error=esia-error
    Esia->>EsiaService: Информация о пользователе
    EsiaService->>Dobs: Информация о пользователе
    Dobs->>Front(App): Редирект на Url из начала процесса```
