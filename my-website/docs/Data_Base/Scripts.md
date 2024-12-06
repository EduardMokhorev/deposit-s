---
sidebar_position: 2
---

# Создание таблиц
СУБД PosrgreSQL

```Plain Text
CREATE TABLE IF NOT EXISTS "deposit_product" (
    "id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "is_active" BOOLEAN NOT NULL,
    "deposit_title_description" VARCHAR(60) NOT NULL,
    "min_amount" BIGINT NOT NULL,
    "max_amount" BIGINT NOT NULL,
    "currency_id" BIGINT NOT NULL,
    "interest_rate" NUMERIC(4,2) NOT NULL,
    "is_add_possible" BOOLEAN NOT NULL,
    "is_capitalisation" BOOLEAN NOT NULL,
    "is_auto_renewal" BOOLEAN NOT NULL,
    "min_duration" BIGINT NOT NULL,
    "max_duration" BIGINT NOT NULL
);

CREATE TABLE IF NOT EXISTS "currency" (
    "id" SERIAL PRIMARY KEY,
    "title" VARCHAR(50) NOT NULL,
    "iso_num" VARCHAR(3) NOT NULL
);

CREATE TABLE IF NOT EXISTS "deposit_agreement" (
    "id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "agreement_number" VARCHAR(20) NOT NULL,
    "deposit_product_id" UUID NOT NULL,
    "interest_rate" NUMERIC(4,2) NOT NULL,
    "start_date" DATE NOT NULL,
    "end_date" DATE NOT NULL,
    "initial_amount" NUMERIC(19) NOT NULL,
    "currency_id" BIGINT NOT NULL,
    "client_account_id" BIGINT NOT NULL,
    "is_auto_renewal" BOOLEAN NOT NULL,
    "status" VARCHAR(20) NOT NULL,
    "is_add_possible" BOOLEAN NOT NULL,
    "is_capitalisation" BOOLEAN NOT NULL,
    "client_id" UUID NOT NULL,
    "account_perc_id" BIGINT NOT NULL
);

CREATE TABLE IF NOT EXISTS "deposit_orders" (
    "id" UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    "client_id" UUID NOT NULL,
    "idempotency_key" UUID NOT NULL,
    "status" VARCHAR(255) NOT NULL,
    "open_date" DATE NOT NULL,
    "deposit_product_id" BIGINT NOT NULL,
    "account_from_id" BIGINT NOT NULL,
    "sum" BIGINT NOT NULL,
    "currency_id" BIGINT NOT NULL,
    "deposit_period" BIGINT NOT NULL,
    "deposit_rate" NUMERIC(4,2) NOT NULL
);
```



создание индексов

```Plain Text
-- Добавление индексов к таблице "deposit_product"
-- индекс на вшений ключ идентификатор валюты. 
CREATE INDEX idx_currency_id_deposit_product ON deposit_product (currency_id);

-- Добавление индексов к таблице "deposit_agreement"
--айди продукта
CREATE INDEX idx_deposit_product_id_deposit_agreement ON deposit_agreement (deposit_product_id);

-- Добавление индексов к таблице "deposit_orders"
CREATE INDEX idx_client_id_deposit_orders ON deposit_orders (client_id);
CREATE INDEX idx_deposit_product_id_deposit_orders ON deposit_orders (deposit_product_id);
CREATE INDEX idx_currency_id_deposit_orders ON deposit_orders (currency_id);
```

