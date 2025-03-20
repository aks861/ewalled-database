CREATE Table users (
 id SERIAL PRIMARY KEY,
 nama VARCHAR NOT NULL,
 username VARCHAR(15) UNIQUE NOT NULL,
 email VARCHAR UNIQUE,
 password VARCHAR NOT NULL,
 avatar VARCHAR,
 phone_no VARCHAR UNIQUE NOT NULL 
 );


INSERT INTO users (nama, username, email, password, avatar, phone_no) VALUES
('Joko', 'joko', 'joko@example.com', 'password123', 'avatar1.png', '08123456789'),
('Chelsea Immanuela', 'chelsea', 'chelsea@example.com', 'chelsea123', 'avatar2.png', '08223344556'),
('Danny Satria', 'dannys', 'dannys@example.com', 'danny456', 'avatar3.png', '08332255677'),
('Tito Pandhito', 'pandhito', 'pandhito@example.com', 'panditho789', 'avatar4.png', '08445566778'),
('Haji Koko', 'koko', 'koko@example.com', 'koko789', 'avatar5.png', '08556677889');


CREATE TYPE transaction_type AS ENUM ('Topup', 'Transfer');

CREATE Table transaction (
 id SERIAL PRIMARY KEY,
 amount VARCHAR NOT NULL,
 type transaction_type NOT NULL,
 description VARCHAR,
 datetime TIMESTAMP DEFAULT CURRENT_TIMESTAMP NOT NULL,
 transaction_no VARCHAR UNIQUE NOT NULL,
 sender_id INT REFERENCES users(id),
 recipient_id INT NOT NULL REFERENCES users(id),
 status VARCHAR(20)
 );

 INSERT INTO transaction (amount, type, description,transaction_no, sender_id, recipient_id, status) VALUES
('75000', 'Transfer', 'Kopi tuku', 'TXN1001', 1, 2, 'Success'),
('75000', 'Transfer', 'Fore', 'TXN1002', 3, 1, 'Success'),
('75000', 'Transfer', 'Calf', 'TXN1003', 4, 1, 'Success'),
('1000000', 'Transfer', 'transfer bukber', 'TXN1004', 1, 3, 'Success'),
('75000', 'Transfer', 'kopken', 'TXN1005', 1, 4, 'Success'),
('75000', 'Transfer', 'tomoro', 'TXN1006', 5, 1, 'Success'),
('1000000', 'Topup', 'Topup gopay', 'TXN2001', NULL, 1, 'Success'),
('1000000', 'Topup', 'Topup bank', 'TXN2002', NULL, 3, 'Success'),
('1000000', 'Topup', 'Topup shopee', 'TXN2003', NULL, 5, 'Success');

-- SELECT * FROM users;
-- SELECT * FROM transaction;

-- POVnya user id 1, Joko
EXPLAIN ANALYZE SELECT 
    t.datetime AS "Date & Time", 
    t.type,
    CASE 
        WHEN t.sender_id = 1 THEN recipient.nama
        ELSE sender.nama
	END AS "From / To",
	t.description,
    CASE 
        WHEN t.type = 'Transfer' THEN CONCAT('- ', t.amount)
        ELSE CONCAT('+ ', t.amount)
    END AS "Amount"
FROM transaction t
LEFT JOIN users sender ON t.sender_id = sender.id
LEFT JOIN users recipient ON t.recipient_id = recipient.id
WHERE sender.id = 1 OR recipient.id = 1;
