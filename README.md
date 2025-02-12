# Logistics---Transporation
Third Most Important Sql query

-- Create the Vehicles table
CREATE TABLE Vehicles (
    vehicle_id INT PRIMARY KEY AUTO_INCREMENT,
    license_plate VARCHAR(20) UNIQUE NOT NULL,
    vehicle_type ENUM('Truck', 'Van', 'Bike') NOT NULL,
    capacity DECIMAL(10,2) NOT NULL,  -- Capacity in tons or kg
    status ENUM('Available', 'In Transit', 'Under Maintenance') DEFAULT 'Available'
);

-- Create the Drivers table
CREATE TABLE Drivers (
    driver_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(15) UNIQUE NOT NULL,
    license_number VARCHAR(50) UNIQUE NOT NULL,
    experience INT CHECK (experience >= 0)  -- Experience in years
);

-- Create the Routes table
CREATE TABLE Routes (
    route_id INT PRIMARY KEY AUTO_INCREMENT,
    source VARCHAR(255) NOT NULL,
    destination VARCHAR(255) NOT NULL,
    distance DECIMAL(10,2) NOT NULL,  -- Distance in km
    estimated_time TIME NOT NULL  -- Estimated travel time
);

-- Create the Shipments table
CREATE TABLE Shipments (
    shipment_id INT PRIMARY KEY AUTO_INCREMENT,
    sender VARCHAR(255) NOT NULL,
    receiver VARCHAR(255) NOT NULL,
    weight DECIMAL(10,2) CHECK (weight > 0),  -- Weight in kg or tons
    status ENUM('Pending', 'In Transit', 'Delivered', 'Cancelled') DEFAULT 'Pending',
    route_id INT,
    FOREIGN KEY (route_id) REFERENCES Routes(route_id) ON DELETE SET NULL
);

-- Create the Deliveries table
CREATE TABLE Deliveries (
    delivery_id INT PRIMARY KEY AUTO_INCREMENT,
    shipment_id INT,
    vehicle_id INT,
    driver_id INT,
    departure_time TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    arrival_time TIMESTAMP NULL,
    status ENUM('On Route', 'Delivered', 'Delayed') DEFAULT 'On Route',
    FOREIGN KEY (shipment_id) REFERENCES Shipments(shipment_id) ON DELETE CASCADE,
    FOREIGN KEY (vehicle_id) REFERENCES Vehicles(vehicle_id) ON DELETE SET NULL,
    FOREIGN KEY (driver_id) REFERENCES Drivers(driver_id) ON DELETE SET NULL
);

