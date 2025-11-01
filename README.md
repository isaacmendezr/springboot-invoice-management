# Spring Boot Invoice Management System

<em>A comprehensive electronic invoicing platform built with Spring Boot, featuring multi-role authentication, client and product management, and invoice generation with PDF/XML export capabilities.</em>

---

## Table of Contents

- [Spring Boot Invoice Management System](#spring-boot-invoice-management-system)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Architecture](#architecture)
  - [Features](#features)
  - [Technology Stack](#technology-stack)
  - [Database Model](#database-model)
  - [Project Structure](#project-structure)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Installation](#installation)
    - [Database Setup](#database-setup)
    - [Configuration](#configuration)
    - [Usage](#usage)
  - [API Endpoints](#api-endpoints)
  - [License](#license)
  - [Contact](#contact)

---

## Overview

**Spring Boot Invoice Management System** is a web-based application designed to streamline the electronic invoicing process for small and medium-sized businesses. The system provides a complete solution for managing suppliers, clients, products, and invoices with role-based access control.

The application supports two main user roles:
- **Administrators**: Manage supplier accounts and their activation status
- **Suppliers**: Manage their own clients, products, and generate invoices

Key capabilities include:
- User registration and authentication
- Client and product management per supplier
- Real-time invoice generation with automatic calculations
- Export invoices to PDF and XML formats
- Supplier profile management
- Administrative control panel

---

## Architecture

**For Backend/Web Application:**
- **Framework:** Spring Boot 3.2.4
- **Language:** Java 21
- **Template Engine:** Thymeleaf
- **Database:** MySQL
- **ORM:** Spring Data JPA (Hibernate)
- **Design Pattern:** Layered Architecture (MVC)
- **Packaging:** WAR (Web Application Archive)

The application follows a three-tier architecture:
1. **Presentation Layer**: Thymeleaf templates with controllers
2. **Business Logic Layer**: Service classes with transactional operations
3. **Data Access Layer**: JPA repositories and entities

---

## Features

| Category | Description |
| :-------- | :----------- |
| 🔐 **Authentication** | Role-based login system (Administrator/Supplier) with session management and user state validation (Active/Inactive/Pending) |
| 👥 **User Management** | Supplier registration with automatic user account creation, administrator approval workflow, and profile updates |
| 📊 **Client Management** | CRUD operations for clients, search by name, assignment to specific suppliers |
| 📦 **Product Catalog** | Product management per supplier with name, description, and pricing |
| 🧾 **Invoice Generation** | Real-time invoice creation with automatic total calculations, item quantity adjustments, and client validation |
| 📄 **Export Capabilities** | Generate invoices in PDF format (iText7) and XML format with complete invoice details |
| 🎯 **Admin Panel** | Supplier activation/deactivation, sortable supplier lists by status |
| 🔍 **Search & Filter** | Search clients by name, filter products by supplier, view invoices by supplier |

---

## Technology Stack

**Core Dependencies:**
- **Spring Boot Starter Web** - MVC framework and embedded Tomcat server
- **Spring Boot Starter Data JPA** - Database operations and ORM
- **Spring Boot Starter Thymeleaf** - Server-side template engine
- **Spring Boot Starter Validation** - Jakarta Bean Validation
- **MySQL Connector** - MySQL database driver
- **iText7 Core** - PDF generation library
- **Spring Boot DevTools** - Hot reload for development

**Configuration:**
- **Server Port** - 8080 (default)
- **Database URL** - `jdbc:mysql://localhost:3306/Facturacion`
- **JPA Show SQL** - Enabled for debugging
- **Naming Strategy** - PhysicalNamingStrategyStandardImpl (preserves exact column names)

---

## Database Model

The application uses a relational database with the following entities:

**Core Entities:**
- **Usuario (User)**: Authentication and authorization (id, username, password, state, role)
- **Proveedor (Supplier)**: Supplier information with 1:1 relationship to Usuario (id/tax-id, name, email, phone, address)
- **Cliente (Client)**: Client records linked to suppliers (id/tax-id, name, email, phone, address)
- **Producto (Product)**: Product catalog per supplier (id, name, description, price)
- **Factura (Invoice)**: Invoice headers (id, date, total, supplier_id, client_id)
- **DetalleFactura (Invoice Detail)**: Line items (id, invoice_id, product_id, quantity, unit_price, subtotal)

**Relationships:**
- Usuario ↔ Proveedor: One-to-One
- Proveedor → Cliente: One-to-Many
- Proveedor → Producto: One-to-Many
- Proveedor → Factura: One-to-Many
- Cliente → Factura: One-to-Many
- Factura → DetalleFactura: One-to-Many
- Producto → DetalleFactura: One-to-Many

---

## Project Structure

```sh
springboot-invoice-management/
├── DB/
│   ├── Facturacion_Schema.sql       # Database schema
│   └── Facturacion_Data.sql         # Sample data
├── Facturacion/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/eif209/facturacion/
│   │   │   │   ├── FacturacionApplication.java
│   │   │   │   ├── data/                    # Repositories
│   │   │   │   │   ├── ClienteRepository.java
│   │   │   │   │   ├── DetallefacturaRepository.java
│   │   │   │   │   ├── FacturaRepository.java
│   │   │   │   │   ├── ProductoRepository.java
│   │   │   │   │   ├── ProveedorRepository.java
│   │   │   │   │   └── UsuarioRepository.java
│   │   │   │   ├── dto/                     # Data Transfer Objects
│   │   │   │   │   └── ProveedorRegistroDTO.java
│   │   │   │   ├── logic/                   # Domain entities & services
│   │   │   │   │   ├── Cliente.java
│   │   │   │   │   ├── Detallefactura.java
│   │   │   │   │   ├── Factura.java
│   │   │   │   │   ├── Producto.java
│   │   │   │   │   ├── Proveedor.java
│   │   │   │   │   ├── Usuario.java
│   │   │   │   │   └── Service.java
│   │   │   │   └── presentation/            # Controllers
│   │   │   │       ├── about/
│   │   │   │       ├── app/
│   │   │   │       ├── clientes/
│   │   │   │       ├── facturar/
│   │   │   │       ├── facturas/
│   │   │   │       ├── login/
│   │   │   │       ├── perfil/
│   │   │   │       ├── productos/
│   │   │   │       ├── proveedores/
│   │   │   │       └── registro/
│   │   │   └── resources/
│   │   │       ├── application.properties
│   │   │       ├── static/
│   │   │       │   ├── css/
│   │   │       │   │   └── style.css
│   │   │       │   └── images/
│   │   │       └── templates/
│   │   │           ├── fragments/
│   │   │           │   ├── header.html
│   │   │           │   └── footer.html
│   │   │           └── presentation/
│   │   │               ├── about/
│   │   │               ├── clientes/
│   │   │               ├── facturar/
│   │   │               ├── facturas/
│   │   │               ├── login/
│   │   │               ├── perfil/
│   │   │               ├── productos/
│   │   │               ├── proveedores/
│   │   │               └── registro/
│   │   └── test/
│   ├── pom.xml
│   ├── mvnw
│   └── mvnw.cmd
└── README.md
```

---

## Getting Started

### Prerequisites

- **Java Development Kit (JDK)** 21 or higher
- **Apache Maven** 3.8+ (or use included Maven wrapper)
- **MySQL Server** 8.0+
- **Git** (for cloning the repository)

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/isaacmendezr/springboot-invoice-management.git
   cd springboot-invoice-management
   ```

2. Navigate to the project directory:
   ```sh
   cd Facturacion
   ```

### Database Setup

1. Start your MySQL server and create the database:
   ```sql
   CREATE DATABASE IF NOT EXISTS Facturacion;
   ```

2. Run the schema script:
   ```sh
   mysql -u root -p Facturacion < ../DB/Facturacion_Schema.sql
   ```

3. (Optional) Load sample data:
   ```sh
   mysql -u root -p Facturacion < ../DB/Facturacion_Data.sql
   ```

   **Sample credentials after loading data:**
   - Admin: `admin` / `password`
   - Supplier: `proveedor1` / `password`

### Configuration

Update the database credentials in `src/main/resources/application.properties` if needed:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/Facturacion
spring.datasource.username=root
spring.datasource.password=YourPassword
```

### Usage

1. Run the application using Maven:
   ```sh
   ./mvnw spring-boot:run
   ```

   Or on Windows:
   ```sh
   mvnw.cmd spring-boot:run
   ```

2. Access the application:
   ```
   http://localhost:8080
   ```

3. Login with your credentials or register as a new supplier

4. **Administrator workflow:**
   - Login with admin credentials
   - Navigate to "Proveedores" (Suppliers)
   - Activate or deactivate supplier accounts

5. **Supplier workflow:**
   - Register a new account (status: Pending)
   - Wait for admin activation
   - Login and manage clients, products
   - Create invoices and export to PDF/XML

---

## API Endpoints

| Method | Endpoint | Description | Access |
|--------|----------|-------------|--------|
| GET | `/` | Redirect to login | Public |
| GET | `/presentation/login/show` | Login page | Public |
| POST | `/presentation/login/login` | Authenticate user | Public |
| GET | `/presentation/login/logout` | Logout user | Authenticated |
| GET | `/presentation/registro/show` | Registration form | Public |
| POST | `/presentation/registro/register` | Register new supplier | Public |
| GET | `/presentation/proveedores/show` | List suppliers | Admin |
| GET | `/proveedores/activar/{id}` | Activate supplier | Admin |
| GET | `/proveedores/desactivar/{id}` | Deactivate supplier | Admin |
| GET | `/presentation/clientes/show` | List clients | Supplier |
| POST | `/presentation/clientes/add` | Create client | Supplier |
| POST | `/presentation/clientes/search` | Search clients | Supplier |
| GET | `/presentation/productos/show` | List products | Supplier |
| POST | `/presentation/productos/add` | Create product | Supplier |
| GET | `/presentation/facturar/show` | Invoice creation page | Supplier |
| POST | `/presentation/facturar/searchClient` | Search client for invoice | Supplier |
| POST | `/presentation/facturar/searchProduct` | Add product to invoice | Supplier |
| POST | `/presentation/facturar/guardar` | Save invoice | Supplier |
| GET | `/presentation/facturas/show` | List invoices | Supplier |
| POST | `/presentation/facturas/pdf` | Generate PDF | Supplier |
| POST | `/presentation/facturas/xml` | Generate XML | Supplier |
| GET | `/presentation/perfil/show` | Supplier profile | Supplier |
| POST | `/presentation/perfil/configurar` | Update profile | Supplier |

---

## License

Academic project developed for educational purposes.

---

## Contact

**Developer:** Isaac Méndez  
**Repository:** [https://github.com/isaacmendezr/springboot-invoice-management](https://github.com/isaacmendezr/springboot-invoice-management)
