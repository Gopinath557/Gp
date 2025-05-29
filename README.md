CREATE DATABASE EsevaCopApp;
CREATE TABLE Citizens (
    citizen_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100),
    aadhaar_no VARCHAR(12) UNIQUE,
    phone VARCHAR(15),
    email VARCHAR(100),
    address TEXT
);
USE EsevaCopApp;
CREATE TABLE PoliceStations (
    station_id INT AUTO_INCREMENT PRIMARY KEY,
    station_name VARCHAR(100),
    location VARCHAR(100)
);
CREATE TABLE PoliceOfficers (
    officer_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    badge_number VARCHAR(50) UNIQUE,
    rank VARCHAR(50),
    phone VARCHAR(15),
    station_id INT,
    FOREIGN KEY (station_id) REFERENCES PoliceStations(station_id)
);
CREATE TABLE Complaints (
    complaint_id INT AUTO_INCREMENT PRIMARY KEY,
    citizen_id INT,
    complaint_type VARCHAR(100),
    description TEXT,
    complaint_date DATE,
    status VARCHAR(30) DEFAULT 'Pending',
    assigned_officer_id INT,
    FOREIGN KEY (citizen_id) REFERENCES Citizens(citizen_id),
    FOREIGN KEY (assigned_officer_id) REFERENCES PoliceOfficers(officer_id)
);
CREATE TABLE FIRs (
    fir_id INT AUTO_INCREMENT PRIMARY KEY,
    complaint_id INT,
    fir_date DATE,
    details TEXT,
    status VARCHAR(30) DEFAULT 'Open',
    FOREIGN KEY (complaint_id) REFERENCES Complaints(complaint_id)
);
INSERT INTO PoliceStations (station_name, location)
VALUES ('Central Police Station', 'Downtown');
INSERT INTO PoliceOfficers (name, badge_number, rank, phone, station_id)
VALUES ('Inspector Ravi', 'BN1234', 'Inspector', '9876543210', 1);
INSERT INTO Citizens (full_name, aadhaar_no, phone, email, address)
VALUES ('Amit Kumar', '123456789012', '9123456780', 'amit@example.com', 'Sector 15, New Delhi');
INSERT INTO Complaints (citizen_id, complaint_type, description, complaint_date, assigned_officer_id)
VALUES (1, 'Theft', 'Mobile phone stolen from bus stop', CURDATE(), 1);
INSERT INTO FIRs (complaint_id, fir_date, details)
VALUES (1, CURDATE(), 'Theft FIR registered under IPC 379');
UPDATE Complaints
SET status = 'In Progress'
WHERE complaint_id = 1;
UPDATE FIRs
SET status = 'Closed'
WHERE fir_id = 1;
SELECT c.complaint_id, c.complaint_type, c.status, c.complaint_date, p.name AS officer
FROM Complaints c
JOIN PoliceOfficers p ON c.assigned_officer_id = p.officer_id
WHERE c.citizen_id = 1;
SELECT f.fir_id, f.fir_date, f.details, f.status
FROM FIRs f
JOIN Complaints c ON f.complaint_id = c.complaint_id
WHERE c.complaint_id = 1;
