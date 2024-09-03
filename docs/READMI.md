# Proyecto: Gestión de Veterinaria

Este proyecto es una base de datos para la gestión de una clínica veterinaria. La base de datos permite registrar información sobre los clientes, sus mascotas, visitas médicas, servicios ofrecidos, y las relaciones entre estas entidades. Está diseñada para facilitar el seguimiento de las visitas de las mascotas y los servicios que se les han proporcionado.

## Estructura de la Base de Datos

La base de datos `veterinaria` contiene las siguientes tablas:

1. **Clientes**: Información de los clientes de la veterinaria.
2. **Mascotas**: Información de las mascotas de cada cliente.
3. **Visitas**: Registro de cada visita de una mascota a la veterinaria.
4. **Servicios**: Lista de servicios que ofrece la veterinaria.
5. **Visitas_Servicios**: Relación entre las visitas y los servicios proporcionados, representando una relación de muchos a muchos entre las visitas y los servicios.

## Restaurar la Base de Datos

Para restaurar la base de datos, sigue estos pasos:

1. Abre tu terminal o consola de comandos.
2. Conéctate a tu servidor MySQL utilizando las credenciales adecuadas (usuario y contraseña).
   ```bash
   mysql -u usuario -p

####CREAR LA BASE DE DATOS#####

CREATE DATABASE IF NOT EXISTS veterinaria;
USE veterinaria;

####Estructura de la base de datos###
-- Clientes
CREATE TABLE Clientes (
    id_cliente INT PRIMARY KEY AUTO_INCREMENT,
    nombre VARCHAR(50) NOT NULL,
    apellido VARCHAR(50) NOT NULL,
    telefono VARCHAR(20) NOT NULL,
    direccion TEXT,
    email VARCHAR(100) UNIQUE,
    fecha_registro DATE 
);

-- Mascotas
CREATE TABLE Mascotas (
    id_mascota INT PRIMARY KEY AUTO_INCREMENT,
    nombre_mascota VARCHAR(50) NOT NULL,
    especie ENUM('Perro', 'Gato', 'Otro') NOT NULL,
    raza VARCHAR(50),
    fecha_nacimiento DATE,
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES Clientes(id_cliente)
);

-- Visitas
CREATE TABLE Visitas (
    id_visita INT PRIMARY KEY AUTO_INCREMENT,
    id_mascota INT,
    fecha_visita DATE NOT NULL,
    motivo_visita TEXT,
    diagnostico TEXT,
    tratamiento TEXT,
    FOREIGN KEY (id_mascota) REFERENCES Mascotas(id_mascota)
);

-- Servicios
CREATE TABLE Servicios (
    id_servicio INT PRIMARY KEY AUTO_INCREMENT,
    nombre_servicio VARCHAR(50) NOT NULL,
    precio DECIMAL(10,2) NOT NULL
);

-- Visitas_Servicios (relación muchos a muchos entre Visitas y Servicios)
CREATE TABLE Visitas_Servicios (
    id_visita_servicio INT PRIMARY KEY AUTO_INCREMENT,
    id_visita INT,
    id_servicio INT,
    cantidad INT DEFAULT 1,
    FOREIGN KEY (id_visita) REFERENCES Visitas(id_visita),
    FOREIGN KEY (id_servicio) REFERENCES Servicios(id_servicio)
);

INSERT INTO Clientes (nombre, apellido, telefono, direccion, email, fecha_registro)
VALUES 
    ('María', 'López', '5551234567', 'Av. Siempreviva 123', 'maria@example.com', '2023-11-22'),
    ('Pedro', 'García', '5559876543', 'Calle Sesamo 456', 'pedro@example.com', '2024-01-05'),
    ('Ana', 'Rodríguez', '5553216549', 'Calle de las Flores 789', 'ana@example.com', '2023-09-15');

INSERT INTO Mascotas (nombre_mascota, especie, raza, fecha_nacimiento, id_cliente)
VALUES
    ('Lucas', 'Perro', 'Labrador', '2022-03-10', 1),
    ('Minina', 'Gato', 'Siamés', '2021-05-20', 2),
    ('Rocky', 'Perro', 'Italiano', '2020-11-05', 3);

INSERT INTO Visitas (id_mascota, fecha_visita, motivo_visita, diagnostico, tratamiento)
VALUES
    (1, '2023-12-01', 'Vacunación', 'Todo en orden', 'Vacuna antirrábica'),
    (2, '2024-02-15', 'Vómitos', 'Gastroenteritis', 'Dieta blanda y medicamentos'),
    (3, '2023-10-20', 'Revisión rutinaria', 'Saludable', 'Ninguno');

INSERT INTO Servicios (nombre_servicio, precio)
VALUES
    ('Consulta básica', 25.00),
    ('Vacunación', 30.00),
    ('Esterilización', 150.00);

INSERT INTO Visitas_Servicios (id_visita, id_servicio, cantidad)
VALUES
    (1, 2, 1),  -- Visita 1: Vacunación (1 dosis)
    (2, 1, 1),  -- Visita 2: Consulta básica
    (3, 1, 1);  -- Visita 3: Consulta básica


#####Instrucciones relevantes

-Asegúrate de tener instalado MySQL Server y MySQL Workbench o un cliente SQL similar para gestionar la base de datos.
-Las tablas están relacionadas mediante claves foráneas para asegurar la integridad referencial.
-Los tipos de datos han sido seleccionados para optimizar el almacenamiento y la búsqueda de información.
-La tabla Visitas_Servicios permite una relación de muchos a muchos entre visitas y servicios, permitiendo múltiples servicios por visita.

#### Conclusion ###


Este proyecto de gestión de veterinaria proporciona una solución eficiente para administrar la información de clientes, mascotas, visitas médicas y servicios en una clínica veterinaria. La estructura de la base de datos está diseñada para mantener la integridad de los datos y facilitar el acceso y la gestión de la información. Con estas herramientas, la clínica puede mejorar la calidad de su servicio al cliente, optimizar la administración de sus recursos y ofrecer un mejor seguimiento de la salud de las mascotas.
