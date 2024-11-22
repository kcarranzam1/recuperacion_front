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