🚛 Fleet Maintenance & Fuel Audit System
A robust, console-based Java Spring Boot application designed to manage vehicle fleets, track fuel expenditures, monitor maintenance milestones, and perform financial audits using MySQL and Spring JDBC.

🌟 Key Features
Fleet Dashboard & Registration: Register vehicles with initial readings and track current operational statuses (ACTIVE, MAINTENANCE, DECOMMISSIONED).
Fuel Tracking & Efficiency: Log fuel purchases (litres, cost, odometer) and automatically calculate vehicle mileage (km/L) between fuel stops.
Maintenance Management: Log service records and repair costs. Automatically flags vehicles due for service when distance since the last service exceeds $5,000\text{ km}$.
Financial & Expenditure Audits: Aggregates overall fleet operational costs (Fuel + Service) per vehicle using SQL subqueries.
Interactive CLI Interface: Command-line menu built with Spring Boot CommandLineRunner for real-time interaction.

🛠️ Tech Stack
Language: Java 21
Framework: Spring Boot 4.1.2
Database: MySQL 8.x
Data Access: Spring JDBC (JdbcTemplate)
Build Automation: Apache Maven

🏗️ Architecture & Project Structure
The project follows a layered software design pattern ensuring clean separation of concerns:
com.example.demo
├── config/        # Spring bean definitions (JdbcTemplate, DataSource)
├── console/       # Interactive terminal menu (CommandLineRunner UI)
├── dao/           # Data Access Layer using SQL queries & JdbcTemplate
├── model/         # Domain Entities (Vehicle, FuelEntry, ServiceRecord)
├── rowmapper/     # Custom ResultSet to Java object mappers
└── service/       # Core business logic and transaction boundaries (@Transactional)

📊 Database Schema Design
The fleet_audit database consists of three relational tables bound by strict Foreign Keys:
CREATE DATABASE fleet_audit;
USE fleet_audit;

CREATE TABLE vehicles (
    reg_number VARCHAR(20) PRIMARY KEY,
    model VARCHAR(100) NOT NULL,
    current_odometer INT NOT NULL,
    last_service_odometer INT NOT NULL,
    status VARCHAR(30) NOT NULL
);

CREATE TABLE fuel_entries (
    entry_id INT AUTO_INCREMENT PRIMARY KEY,
    reg_number VARCHAR(20) NOT NULL,
    litres DECIMAL(8,2) NOT NULL,
    cost DECIMAL(10,2) NOT NULL,
    odometer_reading INT NOT NULL,
    fuel_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (reg_number) REFERENCES vehicles(reg_number)
);

CREATE TABLE service_records (
    service_id INT AUTO_INCREMENT PRIMARY KEY,
    reg_number VARCHAR(20) NOT NULL,
    service_type VARCHAR(100) NOT NULL,
    service_cost DECIMAL(10,2) NOT NULL,
    service_odometer INT NOT NULL,
    service_date DATE NOT NULL,
    FOREIGN KEY (reg_number) REFERENCES vehicles(reg_number)
);

⚙️ Configuration & Setup
Prerequisites
JDK 21 installed
MySQL Server (v8.0+) running on port 3306
1. Database Setup
Create the fleet_audit database in your MySQL instance:
CREATE DATABASE fleet_audit;
2. Application Properties Configuration
Ensure your src/main/resources/application.properties matches your local database setup:
```properties
spring.application.name=fleet-audit


spring.datasource.url=jdbc:mysql://localhost:3306/fleet_audit
spring.datasource.username=root
spring.datasource.password=${DB_PASSWORD:root}
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver


spring.sql.init.mode=never
```


🚀 Running the Application
Option A: Via Terminal (Maven)
# Clone the repository
git clone https://github.com/Pranali-Londhe/Fleet-Maintenance-Fuel-Audit-System.git

# Navigate to root directory
cd Fleet-Maintenance-Fuel-Audit-System

# Run using Maven
mvn spring-boot:run
Option B: Via IDE (IntelliJ IDEA / Eclipse)
Import the project as a Maven project.
Run com.example.demo.DemoApplication.java directly.

🖥️ Application Console Preview
====================================
 FLEET MAINTENANCE & FUEL AUDIT
====================================
1. Register Vehicle
2. View Fleet Dashboard
3. Add Fuel Entry
4. Calculate Fuel Mileage
5. Add Service Record
6. View Maintenance Alerts
7. View Cost Audit
8. View Service History
0. Exit
Enter your choice: 

