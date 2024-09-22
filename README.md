-- Create tables for revenue and expenditure
CREATE TABLE revenue (
    id SERIAL PRIMARY KEY,
    invoice_number VARCHAR(255) UNIQUE NOT NULL,
    invoice_date DATE NOT NULL,
    customer_name VARCHAR(255) NOT NULL,
    amount NUMERIC NOT NULL,
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW()
);

CREATE TABLE expenditure (
    id SERIAL PRIMARY KEY,
    invoice_number VARCHAR(255) UNIQUE NOT NULL,
    invoice_date DATE NOT NULL,
    vendor_name VARCHAR(255) NOT NULL,
    amount NUMERIC NOT NULL,
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW()
);

-- Insert some sample data
INSERT INTO revenue (invoice_number, invoice_date, customer_name, amount) VALUES
    ('INV-001', '2023-01-01', 'Customer A', 1000),
    ('INV-002', '2023-02-01', 'Customer B', 1500),
    ('INV-003', '2023-03-01', 'Customer C', 2000);

INSERT INTO expenditure (invoice_number, invoice_date, vendor_name, amount) VALUES
    ('EXP-001', '2023-01-10', 'Vendor A', 500),
    ('EXP-002', '2023-02-10', 'Vendor B', 750),
    ('EXP-003', '2023-03-10', 'Vendor C', 1000);

-- Calculate total revenue for a given period
SELECT SUM(amount) AS total_revenue
FROM revenue
WHERE invoice_date BETWEEN '2023-01-01' AND '2023-03-31';

-- Calculate total expenditure for a given period
SELECT SUM(amount) AS total_expenditure
FROM expenditure
WHERE invoice_date BETWEEN '2023-01-01' AND '2023-03-31';

-- Calculate net income for a given period
SELECT SUM(revenue.amount) - SUM(expenditure.amount) AS net_income
FROM revenue
JOIN expenditure ON revenue.invoice_date = expenditure.invoice_date
WHERE revenue.invoice_date BETWEEN '2023-01-01' AND '2023-03-31';
