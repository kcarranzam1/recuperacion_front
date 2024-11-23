# Proyecto de recuperacion de curso de desarrollo Web


## Estructura de la base de datos

```
CREATE TABLE Kv_TiposEvento (
    idTipoEvento INT PRIMARY KEY IDENTITY(1,1),
    nombre VARCHAR(50) NOT NULL,
    descripcion TEXT NULL
);

SELECT * FROM Kv_TiposEvento;


CREATE TABLE Kv_RolesAsistentes (
    idRol INT PRIMARY KEY IDENTITY(1,1),
    nombre VARCHAR(50) NOT NULL,
    descripcion TEXT NULL
);

CREATE TABLE Kv_EventosCorporativos (
    idEvento INT PRIMARY KEY IDENTITY(1,1),
    nombre VARCHAR(100) NOT NULL,
    fecha DATE NOT NULL,
    idTipoEvento INT NOT NULL,
    ubicacion VARCHAR(200) NOT NULL,
    descripcion TEXT NULL,
    FOREIGN KEY (idTipoEvento) REFERENCES Kv_TiposEvento(idTipoEvento) ON DELETE CASCADE
);

CREATE TABLE Kv_AsistentesEvento (
    idAsistente INT PRIMARY KEY IDENTITY(1,1),
    idEvento INT NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    correoElectronico VARCHAR(100) NOT NULL,
    telefono VARCHAR(15) NULL,
    idRol INT NOT NULL,
    FOREIGN KEY (idEvento) REFERENCES Kv_EventosCorporativos(idEvento) ON DELETE CASCADE,
    FOREIGN KEY (idRol) REFERENCES Kv_RolesAsistentes(idRol) ON DELETE CASCADE
);

SELECT * FROM Kv_RolesAsistentes;

INSERT INTO Kv_RolesAsistentes (nombre, descripcion)
VALUES 
    ('Asistente', 'Persona que asiste al evento'),
    ('Ponente', 'Persona que presenta en el evento'),
    ('Organizador', 'Persona encargada de organizar el evento');

```

## Descripción General
Esta aplicación permite gestionar eventos corporativos, asignar roles a asistentes y categorizar los eventos según tipos. El proyecto incluye funcionalidades para crear, editar y eliminar eventos, asistentes, tipos de eventos y roles.

---

## Estructura de la Base de Datos

La base de datos está diseñada en *SQL Server* con las siguientes tablas:

### 1. tipos_eventos
- id: Identificador único (PK).
- descripcion: Texto que describe el tipo de evento (e.g., conferencia, seminario).

### 2. roles_data
- id: Identificador único (PK).
- descripcion: Descripción del rol asignado (e.g., moderador, coordinador).

### 3. eventos
- id: Identificador único (PK).
- nombre: Nombre del evento.
- fecha: Fecha del evento.
- tipo_evento_id: Relación con el tipo de evento (FK).

### 4. asistentes
- id: Identificador único (PK).
- evento_id: Relación con el evento asociado (FK).
- nombre: Nombre del asistente.
- rol_id: Relación con el rol asignado al asistente (FK).

Las relaciones son las siguientes:
- Cada *evento* pertenece a un *tipo de evento*.
- Cada *asistente* está asociado a un *evento* y tiene un *rol* asignado.

---

## Arquitectura de la Aplicación

### Frontend
- *Framework:* Vue.js
- *Estructura:* Toda la lógica del frontend está desarrollada en el archivo principal App.vue.
- *Librerías usadas:* Axios para solicitudes HTTP.
- *Funcionalidades principales:*
  - Gestión de eventos: Crear, editar y eliminar eventos.
  - Gestión de asistentes: Agregar asistentes a eventos, asignarles roles y eliminarlos.
  - Gestión de tipos de eventos y roles: CRUD básico.

### Backend
- *Framework:* Express.js
- *Conexión con la base de datos:* SQL Server mediante el paquete mssql.
- *Endpoints RESTful:*
  - CRUD para eventos, asistentes, roles y tipos de eventos.
  - Validaciones básicas y manejo de errores.
  - Uso de relaciones para evitar eliminaciones conflictivas (e.g., no eliminar roles usados por asistentes).

### Base de Datos
- *Gestor:* SQL Server.
- *Conexión al backend:* Configurada mediante el paquete mssql en Node.js.
- *Modelo relacional:* Se aplican restricciones de integridad referencial con claves foráneas.