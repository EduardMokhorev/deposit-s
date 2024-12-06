---
sidebar_position: 3
---

# DS-1 - Создание заявки на депозит

Примечание:

Мы должны подтверждать депозитные заявки, аналогично процессу в Белгазпромбанке. Депозит не создается немедленно при оформлении; подтверждение происходит в будние дни и в рабочее время. Без подтверждения от сотрудников депозиты не предоставляются.

Может показаться, что процесс не полностью описан, но это описание первой точки входа для создания заявки на депозит. Дальнейшие шаги будут разработаны в последующих спринтах.

Паттерн Outbox для Kafka включает сохранение сообщений в лестнице (Outbox) перед их публикацией в тему Kafka. Этот метод гарантирует сохранность сообщений в случае сбоя процесса публикации и обеспечивает доставку сообщений.




```plantuml

@startuml

title DS.1 Открытие депозита для авторизованного пользователя (отправка заявки на депозит)

skinparam maxMessageSize 200

actor Пользователь

boundary Браузер

control APIGateway

control depositService

entity Outbox

queue deposit_for_approval

Пользователь -> Браузер: Нажал кнопку "Отправить заявку "

note right

  в форме заявки есть

  ключ идемпотентности

end note

Браузер ->> APIGateway: POST api/v1/deposit-service/deposit-orders

APIGateway -> APIGateway: Извлечение clientId из токена

APIGateway -> depositService: POST v1/deposit-service/deposit-orders

alt Строка с таким же idempotency_key найдена

depositService --> APIGateway: CODE 409 Conflict

APIGateway --> Браузер: CODE 409 Conflict  

Браузер --> Пользователь: Сообщение "Заявка уже была ранее принята в работу"

else Строка с таким же idempotency_key не найдена

depositService -> depositService: Сохранение данных в deposit_order



depositService -> depositService:  Подтверждение сохранения данных

par Отправка подтверждения Пользователю

depositService --> APIGateway: CODE 201 Created + Body

APIGateway --> Браузер: CODE 201 Created + Body

Браузер --> Пользователь: Сообщение "Заявка успешно создана"

else Отправка в Kafka

depositService ->deposit_for_approval: Отправка сообщения по заявке в Kafka

alt Сообщение не сохранено в Kafka

depositService -> Outbox:Сохранение данных в Outbox

end

end

end

loop каждые 30 минут, шедулер запускает процедуру для отправки сообщение в топик

depositService -> depositService: Запуск процедуры proc_outbox_send_deposit_orders

depositService -> deposit_for_approval: Отправка заявлений на создание депозита в кафку

alt Заявка отправлена успешно

depositService ->Outbox: Удалить запись по заявке из таблицы Outbox

Outbox -> Outbox: Изменение статуса записи на отправлено

else не удалось отправить

Outbox -> Outbox: артибут в бд отвечающий за кол-во попыток инкрементируем

note right

  инкрементируем мы для себя,

  работать с проблемными записями будет другая процедура (которая будет бить тревогу)

  и она будет реализована позже.

end note

end

end

@enduml

```

