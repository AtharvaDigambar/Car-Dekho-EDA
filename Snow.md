-- Create Database
CREATE DATABASE IF NOT EXISTS DELIVERY_TRACKING_DB;

USE DELIVERY_TRACKING_DB;


-- Create Routes Table
CREATE TABLE Routes (
    route_id INT PRIMARY KEY,
    route_name VARCHAR(100)
);


-- Create Drivers Table
CREATE TABLE Drivers (
    driver_id INT PRIMARY KEY,
    driver_name VARCHAR(100)
);


-- Create Deliveries Table
CREATE TABLE Deliveries (
    delivery_id INT PRIMARY KEY,
    route_id INT,
    driver_id INT,
    delivery_time DECIMAL(6,2),
    status ENUM('Completed', 'Delayed', 'Cancelled'),

    FOREIGN KEY (route_id) REFERENCES Routes(route_id),
    FOREIGN KEY (driver_id) REFERENCES Drivers(driver_id)
);


-- Insert Routes
INSERT INTO Routes (route_id, route_name) VALUES
(1, 'Route A'),
(2, 'Route B');


-- Insert Drivers
INSERT INTO Drivers (driver_id, driver_name) VALUES
(1, 'John'),
(2, 'Mike'),
(3, 'David'),
(4, 'Robert');


-- Insert Deliveries
INSERT INTO Deliveries
(delivery_id, route_id, driver_id, delivery_time, status)
VALUES
(301, 1, 1, 42.50, 'Completed'),
(302, 1, 2, 39.00, 'Completed'),
(303, 1, 3, 46.00, 'Completed'),
(304, 1, 4, 55.00, 'Delayed'),
(305, 2, 1, 61.00, 'Completed'),
(306, 2, 2, 58.50, 'Completed'),
(307, 2, 3, 63.50, 'Completed'),
(308, 2, 4, 70.00, 'Cancelled');


-- Required Query
SELECT
    r.route_name,
    dr.driver_name,
    d.delivery_time,
    ROUND(
        AVG(d.delivery_time) OVER (
            PARTITION BY d.route_id
        ),
        2
    ) AS avg_route_time
FROM Deliveries d
JOIN Routes r
    ON d.route_id = r.route_id
JOIN Drivers dr
    ON d.driver_id = dr.driver_id
WHERE d.status = 'Completed'
ORDER BY
    r.route_name,
    d.delivery_time,
    dr.driver_name;
