---
sidebar_position: 3
---

# Наполнение БД тестовыми данными
СУБД PosrgreSQL

SQL запрос для наполнения Базы данных (результат исполнения см. рисунок 1-4)
СУБД PosrgreSQL

```Plain Text
INSERT INTO currency (title, iso_num) VALUES
('US Dollar', 'USD'),
('Euro', 'EUR'),
('British Pound', 'GBP'),
('Japanese Yen', 'JPY'),
('Australian Dollar', 'AUD'),
('Canadian Dollar', 'CAD'),
('Swiss Franc', 'CHF'),
('Chinese Yuan', 'CNY'),
('Indian Rupee', 'INR'),
('Brazilian Real', 'BRL'),
('South African Rand', 'ZAR');



INSERT INTO deposit_product (is_active, deposit_title_description, min_amount, max_amount, currency_id, interest_rate, is_add_possible, is_capitalisation, is_auto_renewal, min_duration, max_duration) VALUES
(true, 'Regular Savings', 500, 10000, 1, 2.50, true, true, true, 30, 365),
(true, 'Fixed Deposit', 1000, 50000, 2, 3.00, false, false, false, 90, 730),
(true, 'Children''s Savings', 100, 5000, 1, 2.75, true, true, true, 60, 365),
(true, 'Senior Citizens Scheme', 10000, 100000, 3, 3.25, true, true, true, 180, 730),
(true, 'Student Savings', 50, 3000, 5, 2.25, true, true, true, 30, 365),
(true, 'Retirement Plan', 5000, 50000, 4, 3.50, true, true, true, 365, 1095),
(true, 'Holiday Fund', 200, 2000, 6, 2.00, true, false, false, 60, 365),
(true, 'Emergency Fund', 1000, 10000, 7, 2.75, true, true, true, 30, 365),
(true, 'Dream Home Savings', 50000, 500000, 10, 4.00, true, true, true, 365, 1825),
(true, 'Wedding Fund', 1000, 10000, 8, 3.25, true, true, true, 90, 730),
(true, 'Vacation Savings', 300, 3000, 11, 2.75, true, true, true, 60, 365);



INSERT INTO deposit_agreement (agreement_number, deposit_product_id, interest_rate, start_date, end_date, initial_amount, currency_id, client_account_id, is_auto_renewal, status, is_add_possible, is_capitalisation, client_id, account_perc_id) VALUES
('AGREEMENT001', 'b04d8979-dc7c-46a8-ba94-2c8a9888a302', 2.50, '2024-01-15', '2025-01-15', 1000, 1, 12345, true, 'ACTIVE', true, true, '550e8400-e29b-41d4-a716-446655440000', 54321),
('AGREEMENT002', '0d7eae8f-1f99-4d4a-a174-9c7f38a9bc63', 3.00, '2024-03-20', '2025-03-20', 5000, 2, 54321, false, 'CLOSED', false, false, '0d7eae8f-1f99-4d4a-a174-9c7f38a9bc63', 67890),
('AGREEMENT003', 'a2acb352-a946-445b-a80e-2ac0b5f24009', 3.25, '2024-02-10', '2025-08-10', 20000, 3, 98765, true, 'ACTIVE', true, true, 'ea8b595c-49b3-4b6a-b2a1-c4f6d0f7b9e1', 24680),
('AGREEMENT004', '5cfdfb8f-7571-4adf-bb7c-6f9c7e2d27d6', 2.25, '2024-04-05', '2025-04-05', 1500, 5, 13579, true, 'ACTIVE', true, true, '5cfdfb8f-7571-4adf-bb7c-6f9c7e2d27d6', 80246),
('AGREEMENT005', 'a2acb352-a946-445b-a80e-2ac0b5f24009', 3.50, '2024-06-15', '2025-06-15', 10000, 4, 24680, true, 'ACTIVE', true, true, 'c7eac9a1-9cbb-4e30-8a4a-0c595b78d3cc', 35791),
('AGREEMENT006', 'a2acb352-a946-445b-a80e-2ac0b5f24009', 2.00, '2024-03-01', '2025-03-01', 500, 6, 67890, false, 'CLOSED', false, false, 'f4b3ebe5-0d7c-4c7e-8b3f-7e4d9f1b5c9a', 46802),
('AGREEMENT007', 'b04d8979-dc7c-46a8-ba94-2c8a9888a302', 2.75, '2024-07-20', '2025-07-20', 3000, 7, 35791, true, 'ACTIVE', true, true, '7f2f0d5a-2c99-4b65-bc7c-6ee6e1a4bf0a', 91572),
('AGREEMENT008', '4d7f6a3e-9a8b-46aa-b9d1-6e8c9e4f2c7d', 4.00, '2024-09-10', '2025-09-10', 60000, 10, 46802, true, 'ACTIVE', true, true, '4d7f6a3e-9a8b-46aa-b9d1-6e8c9e4f2c7d', 12345),
('AGREEMENT010', 'b04d8979-dc7c-46a8-ba94-2c8a9888a302', 2.75, '2024-10-01', '2025-10-01', 2500, 11, 46802, true, 'ACTIVE', true, true, '9f3c5b9e-1a7b-4f9d-b5c7-8a2e3c4f5b7a', 67890),
('AGREEMENT011', 'b04d8979-dc7c-46a8-ba94-2c8a9888a302', 2.75, '2024-08-15', '2025-08-15', 8000, 1, 35791, true, 'ACTIVE', true, true, '0b3c7e5a-6d9f-4c8b-a7f3-5e2b1c4d7a9f', 80246);



INSERT INTO deposit_orders (client_id, idempotency_key, status, open_date, deposit_product_id, account_from_id, sum, currency_id, deposit_period, deposit_rate) VALUES
('550e8400-e29b-41d4-a716-446655440000', '550e8400-e29b-41d4-a716-446655440000', 'ACTIVE', '2024-12-01', 1, 12345, 5000, 1, 90, 2.75),
('0d7eae8f-1f99-4d4a-a174-9c7f38a9bc63', '0d7eae8f-1f99-4d4a-a174-9c7f38a9bc63', 'PENDING', '2024-12-01', 2, 54321, 10000, 2, 180, 3.25),
('ea8b595c-49b3-4b6a-b2a1-c4f6d0f7b9e1', 'ea8b595c-49b3-4b6a-b2a1-c4f6d0f7b9e1', 'ACTIVE', '2024-12-01', 4, 98765, 20000, 3, 365, 2.50),
('5cfdfb8f-7571-4adf-bb7c-6f9c7e2d27d6', '5cfdfb8f-7571-4adf-bb7c-6f9c7e2d27d6', 'CLOSED', '2024-12-01', 5, 13579, 3000, 5, 60, 2.25),
('c7eac9a1-9cbb-4e30-8a4a-0c595b78d3cc', 'c7eac9a1-9cbb-4e30-8a4a-0c595b78d3cc', 'ACTIVE', '2024-12-01', 6, 24680, 15000, 4, 365, 3.50),
('f4b3ebe5-0d7c-4c7e-8b3f-7e4d9f1b5c9a', 'f4b3ebe5-0d7c-4c7e-8b3f-7e4d9f1b5c9a', 'PENDING', '2024-12-01', 7, 35791, 8000, 7, 90, 2.75),
('7f2f0d5a-2c99-4b65-bc7c-6ee6e1a4bf0a', '7f2f0d5a-2c99-4b65-bc7c-6ee6e1a4bf0a', 'ACTIVE', '2024-12-01', 8, 46802, 25000, 10, 180, 4.00),
('4d7f6a3e-9a8b-46aa-b9d1-6e8c9e4f2c7d', '4d7f6a3e-9a8b-46aa-b9d1-6e8c9e4f2c7d', 'PENDING', '2024-12-01', 9, 12345, 10000, 11, 365, 2.75),
('9f3c5b9e-1a7b-4f9d-b5c7-8a2e3c4f5b7a', '9f3c5b9e-1a7b-4f9d-b5c7-8a2e3c4f5b7a', 'ACTIVE', '2024-12-01', 10, 46802, 5000, 10, 90, 2.75),
('0b3c7e5a-6d9f-4c8b-a7f3-5e2b1c4d7a9f', '0b3c7e5a-6d9f-4c8b-a7f3-5e2b1c4d7a9f', 'ACTIVE', '2024-12-01', 11, 35791, 12000, 1, 180, 2.75);


```
