# Modelo Relacional - Sistema Salto Innova

## Diagrama Conceptual

```
Empresas ─┬─< Reuniones
          ├─< HistorialEmpresa
          ├─< EmpresaTags
          └─< EmpresaInstrumentos

UnidadesAtencion ─┬─< Reuniones
                  └─< FuncionarioUnidades ─> Funcionarios

Funcionarios ─┬─< Reuniones
              └─< FuncionarioUnidades ─> UnidadesAtencion
```

## 1. Empresas

Tabla principal para gestionar PYMEs. Almacena información crítica para seguimiento y métricas.

| Campo               | Tipo                                                               | Restricciones                       | Descripción                     |
| ------------------- | ------------------------------------------------------------------ | ----------------------------------- | ------------------------------- |
| **id_empresa**      | INT                                                                | PK, AUTO_INCREMENT                  | Identificador único             |
| rut                 | VARCHAR(12)                                                        | UNIQUE, NOT NULL                    | RUT de la empresa               |
| nombre_comercial    | VARCHAR(200)                                                       | NOT NULL                            | Nombre de la empresa            |
| razon_social        | VARCHAR(200)                                                       |                                     | Razón social oficial            |
| rubro               | VARCHAR(100)                                                       |                                     | Sector/industria                |
| direccion           | VARCHAR(300)                                                       |                                     | Dirección física                |
| telefono            | VARCHAR(20)                                                        |                                     | Teléfono de contacto            |
| email               | VARCHAR(100)                                                       |                                     | Email principal                 |
| fecha_registro      | DATETIME                                                           | NOT NULL, DEFAULT CURRENT_TIMESTAMP | Fecha de alta en el sistema     |
| estado              | ENUM('pre-cliente', 'cliente', 'inactivo', 'cerrado', 'expandido') | NOT NULL, DEFAULT 'pre-cliente'     | Estado actual                   |
| etapa_incubadora    | ENUM('ninguna', 'idea', 'validacion', 'ejecucion', 'asesoria')     | DEFAULT 'ninguna'                   | Etapa en módulo incubadora      |
| responsable_empresa | VARCHAR(200)                                                       |                                     | Nombre del responsable/contacto |
| cargo_responsable   | VARCHAR(100)                                                       |                                     | Cargo del contacto              |
| observaciones       | TEXT                                                               |                                     | Notas adicionales               |
| created_at          | DATETIME                                                           | DEFAULT CURRENT_TIMESTAMP           |                                 |
| updated_at          | DATETIME                                                           | ON UPDATE CURRENT_TIMESTAMP         |                                 |

**Índices:**

- `idx_rut` en (rut)
- `idx_estado` en (estado)
- `idx_etapa` en (etapa_incubadora)

---

## 2. HistorialEmpresa

Registro histórico anual para métricas de evolución. Permite análisis multi-año.

| Campo              | Tipo                                                                                              | Restricciones                       | Descripción                  |
| ------------------ | ------------------------------------------------------------------------------------------------- | ----------------------------------- | ---------------------------- |
| **id_historial**   | INT                                                                                               | PK, AUTO_INCREMENT                  | Identificador único          |
| **id_empresa**     | INT                                                                                               | FK → Empresas(id_empresa), NOT NULL | Empresa asociada             |
| anio               | YEAR                                                                                              | NOT NULL                            | Año del registro             |
| facturacion        | DECIMAL(15,2)                                                                                     |                                     | Facturación anual (USD)      |
| cantidad_empleados | INT                                                                                               |                                     | Cantidad de empleados        |
| area_innovacion    | ENUM('marketing', 'finanzas', 'tics', 'comercial', 'modelo_negocio', 'economia_circular', 'otro') |                                     | Área principal de innovación |
| observaciones      | TEXT                                                                                              |                                     | Notas del año                |
| fecha_registro     | DATETIME                                                                                          | DEFAULT CURRENT_TIMESTAMP           | Cuándo se registró este dato |

**Restricciones:**

- `UNIQUE(id_empresa, anio)` - Un registro por empresa por año

**Índices:**

- `idx_empresa_anio` en (id_empresa, anio)

---

## 3. Funcionarios

Usuarios del sistema con diferentes roles y permisos.

| Campo              | Tipo                                                | Restricciones               | Descripción              |
| ------------------ | --------------------------------------------------- | --------------------------- | ------------------------ |
| **id_funcionario** | INT                                                 | PK, AUTO_INCREMENT          | Identificador único      |
| username           | VARCHAR(50)                                         | UNIQUE, NOT NULL            | Usuario de login         |
| password_hash      | VARCHAR(255)                                        | NOT NULL                    | Contraseña encriptada    |
| nombre_completo    | VARCHAR(200)                                        | NOT NULL                    | Nombre del funcionario   |
| email              | VARCHAR(100)                                        | UNIQUE, NOT NULL            | Email corporativo        |
| rol                | ENUM('admin', 'coordinador', 'asesor', 'recepcion') | NOT NULL, DEFAULT 'asesor'  | Rol/permisos             |
| activo             | BOOLEAN                                             | DEFAULT TRUE                | Usuario activo           |
| fecha_alta         | DATETIME                                            | DEFAULT CURRENT_TIMESTAMP   |                          |
| ultimo_login       | DATETIME                                            |                             | Último acceso al sistema |
| created_at         | DATETIME                                            | DEFAULT CURRENT_TIMESTAMP   |                          |
| updated_at         | DATETIME                                            | ON UPDATE CURRENT_TIMESTAMP |                          |

**Índices:**

- `idx_username` en (username)
- `idx_email` en (email)

---

## 4. UnidadesAtencion

Sedes o lugares físicos/virtuales donde Salto Innova atiende empresas.

| Campo                | Tipo                      | Restricciones             | Descripción                           |
| -------------------- | ------------------------- | ------------------------- | ------------------------------------- |
| **id_unidad**        | INT                       | PK, AUTO_INCREMENT        | Identificador único                   |
| nombre               | VARCHAR(200)              | UNIQUE, NOT NULL          | Nombre de la unidad                   |
| tipo                 | ENUM('fisica', 'virtual') | NOT NULL                  | Tipo de unidad                        |
| direccion            | VARCHAR(300)              |                           | Dirección física (si aplica)          |
| ciudad               | VARCHAR(100)              | NOT NULL                  | Ciudad (ej: Salto, Guaviyu de Arapey) |
| telefono             | VARCHAR(20)               |                           | Teléfono de contacto                  |
| email                | VARCHAR(100)              |                           | Email de la unidad                    |
| horario_atencion     | VARCHAR(200)              |                           | Horarios de atención                  |
| capacidad_simultanea | INT                       |                           | Reuniones simultáneas posibles        |
| activo               | BOOLEAN                   | DEFAULT TRUE              | Unidad operativa                      |
| observaciones        | TEXT                      |                           | Notas adicionales                     |
| created_at           | DATETIME                  | DEFAULT CURRENT_TIMESTAMP |                                       |

**Datos ejemplo:**

- Sede Central - Salto (física)
- Biblioteca Salto (física)
- Guaviyu de Arapey (física)
- Atención Virtual (virtual)

**Índices:**

- `idx_ciudad` en (ciudad)
- `idx_tipo_activo` en (tipo, activo)

---

## 5. FuncionarioUnidades

Relación muchos-a-muchos: funcionarios pueden atender en múltiples unidades.

| Campo              | Tipo     | Restricciones                               | Descripción                  |
| ------------------ | -------- | ------------------------------------------- | ---------------------------- |
| **id_func_unidad** | INT      | PK, AUTO_INCREMENT                          | Identificador único          |
| **id_funcionario** | INT      | FK → Funcionarios(id_funcionario), NOT NULL | Funcionario asignado         |
| **id_unidad**      | INT      | FK → UnidadesAtencion(id_unidad), NOT NULL  | Unidad de atención           |
| fecha_asignacion   | DATETIME | DEFAULT CURRENT_TIMESTAMP                   | Cuándo fue asignado          |
| es_responsable     | BOOLEAN  | DEFAULT FALSE                               | Si es responsable de la sede |

**Restricciones:**

- `UNIQUE(id_funcionario, id_unidad)` - Evitar asignaciones duplicadas

**Índices:**

- `idx_funcionario` en (id_funcionario)
- `idx_unidad` en (id_unidad)

---

## 6. TiposReunion

Catálogo de tipos de reunión del sistema.

| Campo                | Tipo         | Restricciones      | Descripción                         |
| -------------------- | ------------ | ------------------ | ----------------------------------- |
| **id_tipo_reunion**  | INT          | PK, AUTO_INCREMENT | Identificador único                 |
| nombre               | VARCHAR(100) | UNIQUE, NOT NULL   | Nombre del tipo                     |
| descripcion          | TEXT         |                    | Descripción detallada               |
| requiere_seguimiento | BOOLEAN      | DEFAULT FALSE      | Si genera recordatorios automáticos |

**Datos iniciales:**

- Primer contacto
- Seguimiento
- Derivación/Postulación a instrumento
- Asesoría
- Autodiagnóstico

---

## 7. Reuniones

Registro de todas las reuniones con empresas. Núcleo del seguimiento.

| Campo               | Tipo                                                         | Restricciones                                | Descripción                           |
| ------------------- | ------------------------------------------------------------ | -------------------------------------------- | ------------------------------------- |
| **id_reunion**      | INT                                                          | PK, AUTO_INCREMENT                           | Identificador único                   |
| **id_empresa**      | INT                                                          | FK → Empresas(id_empresa), NOT NULL          | Empresa involucrada                   |
| **id_funcionario**  | INT                                                          | FK → Funcionarios(id_funcionario), NOT NULL  | Funcionario a cargo                   |
| **id_tipo_reunion** | INT                                                          | FK → TiposReunion(id_tipo_reunion), NOT NULL | Tipo de reunión                       |
| **id_unidad**       | INT                                                          | FK → UnidadesAtencion(id_unidad)             | Unidad donde se realiza               |
| fecha_hora          | DATETIME                                                     | NOT NULL                                     | Fecha y hora programada               |
| modalidad           | ENUM('presencial', 'telefonica', 'online', 'en_empresa')     | NOT NULL                                     | Cómo se realiza                       |
| duracion_minutos    | INT                                                          |                                              | Duración estimada/real                |
| estado              | ENUM('programada', 'realizada', 'cancelada', 'reprogramada') | NOT NULL, DEFAULT 'programada'               | Estado actual                         |
| agenda              | TEXT                                                         |                                              | Temas a tratar                        |
| acta                | TEXT                                                         |                                              | Resumen de lo tratado                 |
| acuerdos            | TEXT                                                         |                                              | Compromisos acordados                 |
| id_google_calendar  | VARCHAR(255)                                                 |                                              | ID del evento en Google Calendar      |
| proximo_contacto    | DATE                                                         |                                              | Fecha sugerida para siguiente reunión |
| created_at          | DATETIME                                                     | DEFAULT CURRENT_TIMESTAMP                    |                                       |
| updated_at          | DATETIME                                                     | ON UPDATE CURRENT_TIMESTAMP                  |                                       |

**Índices:**

- `idx_empresa_fecha` en (id_empresa, fecha_hora)
- `idx_funcionario_fecha` en (id_funcionario, fecha_hora)
- `idx_unidad_fecha` en (id_unidad, fecha_hora)
- `idx_estado` en (estado)

---

## 8. Tags

Sistema de etiquetado para categorizar problemas/necesidades.

| Campo      | Tipo                                                   | Restricciones      | Descripción                 |
| ---------- | ------------------------------------------------------ | ------------------ | --------------------------- |
| **id_tag** | INT                                                    | PK, AUTO_INCREMENT | Identificador único         |
| nombre     | VARCHAR(100)                                           | UNIQUE, NOT NULL   | Nombre del tag              |
| categoria  | ENUM('problema', 'necesidad', 'oportunidad', 'riesgo') |                    | Categoría del tag           |
| color      | VARCHAR(7)                                             |                    | Color hex para UI (#RRGGBB) |
| activo     | BOOLEAN                                                | DEFAULT TRUE       | Tag disponible              |

**Datos ejemplo:**

- Financiamiento
- Marketing digital
- Transformación digital
- Exportación
- RR.HH.
- Legalidad

---

## 9. EmpresaTags

Relación muchos-a-muchos entre empresas y tags.

| Campo              | Tipo     | Restricciones                       | Descripción         |
| ------------------ | -------- | ----------------------------------- | ------------------- |
| **id_empresa_tag** | INT      | PK, AUTO_INCREMENT                  | Identificador único |
| **id_empresa**     | INT      | FK → Empresas(id_empresa), NOT NULL | Empresa             |
| **id_tag**         | INT      | FK → Tags(id_tag), NOT NULL         | Tag asignado        |
| **id_funcionario** | INT      | FK → Funcionarios(id_funcionario)   | Quién asignó el tag |
| fecha_asignacion   | DATETIME | DEFAULT CURRENT_TIMESTAMP           | Cuándo se asignó    |
| observaciones      | TEXT     |                                     | Contexto del tag    |

**Restricciones:**

- `UNIQUE(id_empresa, id_tag)` - Evitar tags duplicados

**Índices:**

- `idx_empresa` en (id_empresa)
- `idx_tag` en (id_tag)

---

## 10. Programas

Programas de apoyo disponibles en Salto Innova.

| Campo           | Tipo                                                                   | Restricciones      | Descripción                  |
| --------------- | ---------------------------------------------------------------------- | ------------------ | ---------------------------- |
| **id_programa** | INT                                                                    | PK, AUTO_INCREMENT | Identificador único          |
| nombre          | VARCHAR(200)                                                           | UNIQUE, NOT NULL   | Nombre del programa          |
| descripcion     | TEXT                                                                   |                    | Descripción detallada        |
| tipo            | ENUM('incubadora', 'innovacion', 'financiero', 'capacitacion', 'otro') | NOT NULL           | Categoría                    |
| fecha_inicio    | DATE                                                                   |                    | Inicio del programa          |
| fecha_fin       | DATE                                                                   |                    | Fin del programa (si aplica) |
| activo          | BOOLEAN                                                                | DEFAULT TRUE       | Programa disponible          |
| poa_vinculado   | VARCHAR(100)                                                           |                    | Código POA si corresponde    |

---

## 11. Instrumentos

Instrumentos específicos dentro de programas (ej: subsidios, cursos, etc).

| Campo              | Tipo          | Restricciones                         | Descripción                   |
| ------------------ | ------------- | ------------------------------------- | ----------------------------- |
| **id_instrumento** | INT           | PK, AUTO_INCREMENT                    | Identificador único           |
| **id_programa**    | INT           | FK → Programas(id_programa), NOT NULL | Programa al que pertenece     |
| nombre             | VARCHAR(200)  | NOT NULL                              | Nombre del instrumento        |
| descripcion        | TEXT          |                                       | Detalles del instrumento      |
| requisitos         | TEXT          |                                       | Requisitos de aplicación      |
| monto_disponible   | DECIMAL(15,2) |                                       | Monto económico (si aplica)   |
| fecha_apertura     | DATE          |                                       | Desde cuándo se puede aplicar |
| fecha_cierre       | DATE          |                                       | Hasta cuándo se puede aplicar |
| activo             | BOOLEAN       | DEFAULT TRUE                          | Instrumento disponible        |

---

## 12. EmpresaInstrumentos

Registro de postulaciones/derivaciones de empresas a instrumentos.

| Campo                      | Tipo                                                                   | Restricciones                               | Descripción              |
| -------------------------- | ---------------------------------------------------------------------- | ------------------------------------------- | ------------------------ |
| **id_empresa_instrumento** | INT                                                                    | PK, AUTO_INCREMENT                          | Identificador único      |
| **id_empresa**             | INT                                                                    | FK → Empresas(id_empresa), NOT NULL         | Empresa postulante       |
| **id_instrumento**         | INT                                                                    | FK → Instrumentos(id_instrumento), NOT NULL | Instrumento solicitado   |
| **id_funcionario**         | INT                                                                    | FK → Funcionarios(id_funcionario)           | Funcionario que gestionó |
| fecha_postulacion          | DATETIME                                                               | DEFAULT CURRENT_TIMESTAMP                   | Fecha de postulación     |
| estado                     | ENUM('en_proceso', 'aprobado', 'rechazado', 'desistido', 'finalizado') | NOT NULL, DEFAULT 'en_proceso'              | Estado actual            |
| monto_aprobado             | DECIMAL(15,2)                                                          |                                             | Si fue aprobado, monto   |
| fecha_resolucion           | DATE                                                                   |                                             | Fecha de decisión        |
| observaciones              | TEXT                                                                   |                                             | Notas del proceso        |

**Índices:**

- `idx_empresa` en (id_empresa)
- `idx_instrumento` en (id_instrumento)
- `idx_estado` en (estado)

---

## 13. Recordatorios

Sistema automatizado de seguimiento y notificaciones.

| Campo               | Tipo                                                                  | Restricciones                               | Descripción                    |
| ------------------- | --------------------------------------------------------------------- | ------------------------------------------- | ------------------------------ |
| **id_recordatorio** | INT                                                                   | PK, AUTO_INCREMENT                          | Identificador único            |
| **id_reunion**      | INT                                                                   | FK → Reuniones(id_reunion)                  | Reunión relacionada (opcional) |
| **id_empresa**      | INT                                                                   | FK → Empresas(id_empresa), NOT NULL         | Empresa objetivo               |
| **id_funcionario**  | INT                                                                   | FK → Funcionarios(id_funcionario), NOT NULL | Funcionario responsable        |
| tipo                | ENUM('reunion_proxima', 'seguimiento', 'documento_pendiente', 'otro') | NOT NULL                                    | Tipo de recordatorio           |
| medio               | ENUM('email', 'whatsapp', 'ambos')                                    | NOT NULL, DEFAULT 'email'                   | Canal de envío                 |
| fecha_envio         | DATETIME                                                              | NOT NULL                                    | Cuándo enviar                  |
| mensaje             | TEXT                                                                  | NOT NULL                                    | Contenido del recordatorio     |
| estado              | ENUM('pendiente', 'enviado', 'fallido', 'cancelado')                  | NOT NULL, DEFAULT 'pendiente'               | Estado de envío                |
| fecha_envio_real    | DATETIME                                                              |                                             | Cuándo se envió realmente      |
| error_mensaje       | TEXT                                                                  |                                             | Si falló, motivo               |
| created_at          | DATETIME                                                              | DEFAULT CURRENT_TIMESTAMP                   |                                |

**Índices:**

- `idx_fecha_estado` en (fecha_envio, estado)
- `idx_empresa` en (id_empresa)

---

## 14. Autodiagnosticos

Formularios de autodiagnóstico completados por empresas.

| Campo                  | Tipo     | Restricciones                       | Descripción                      |
| ---------------------- | -------- | ----------------------------------- | -------------------------------- |
| **id_autodiagnostico** | INT      | PK, AUTO_INCREMENT                  | Identificador único              |
| **id_empresa**         | INT      | FK → Empresas(id_empresa), NOT NULL | Empresa que completó             |
| fecha_realizado        | DATETIME | DEFAULT CURRENT_TIMESTAMP           | Cuándo se completó               |
| respuestas_json        | JSON     |                                     | Respuestas en formato JSON       |
| puntaje_total          | INT      |                                     | Score calculado (0-100)          |
| areas_oportunidad      | TEXT     |                                     | Áreas identificadas para mejorar |
| recomendaciones        | TEXT     |                                     | Recomendaciones automáticas      |
| revisado_por           | INT      | FK → Funcionarios(id_funcionario)   | Funcionario que revisó           |
| fecha_revision         | DATETIME |                                     | Cuándo fue revisado              |

**Índices:**

- `idx_empresa` en (id_empresa)

---

## 15. AuditLog

Registro de auditoría para seguridad y trazabilidad.

| Campo              | Tipo                                                  | Restricciones                     | Descripción                   |
| ------------------ | ----------------------------------------------------- | --------------------------------- | ----------------------------- |
| **id_log**         | INT                                                   | PK, AUTO_INCREMENT                | Identificador único           |
| **id_funcionario** | INT                                                   | FK → Funcionarios(id_funcionario) | Usuario que realizó la acción |
| tabla_afectada     | VARCHAR(50)                                           | NOT NULL                          | Tabla modificada              |
| id_registro        | INT                                                   |                                   | ID del registro afectado      |
| accion             | ENUM('INSERT', 'UPDATE', 'DELETE', 'LOGIN', 'LOGOUT') | NOT NULL                          | Tipo de acción                |
| datos_anteriores   | JSON                                                  |                                   | Estado previo (UPDATE/DELETE) |
| datos_nuevos       | JSON                                                  |                                   | Estado nuevo (INSERT/UPDATE)  |
| ip_address         | VARCHAR(45)                                           |                                   | IP del usuario                |
| timestamp          | DATETIME                                              | DEFAULT CURRENT_TIMESTAMP         | Momento de la acción          |

**Índices:**

- `idx_funcionario_fecha` en (id_funcionario, timestamp)
- `idx_tabla` en (tabla_afectada)

---

## Relaciones Principales (Foreign Keys)

### Claves Foráneas Detalladas

```sql
-- Tabla: HistorialEmpresa
ALTER TABLE HistorialEmpresa
ADD CONSTRAINT fk_historial_empresa
FOREIGN KEY (id_empresa) REFERENCES Empresas(id_empresa)
ON DELETE CASCADE;

-- Tabla: Reuniones
ALTER TABLE Reuniones
ADD CONSTRAINT fk_reunion_empresa
FOREIGN KEY (id_empresa) REFERENCES Empresas(id_empresa);

ALTER TABLE Reuniones
ADD CONSTRAINT fk_reunion_funcionario
FOREIGN KEY (id_funcionario) REFERENCES Funcionarios(id_funcionario);

ALTER TABLE Reuniones
ADD CONSTRAINT fk_reunion_tipo
FOREIGN KEY (id_tipo_reunion) REFERENCES TiposReunion(id_tipo);

ALTER TABLE Reuniones
ADD CONSTRAINT fk_reunion_unidad
FOREIGN KEY (id_unidad) REFERENCES UnidadesAtencion(id_unidad);

-- Tabla: EmpresaTags
ALTER TABLE EmpresaTags
ADD CONSTRAINT fk_empresatag_empresa
FOREIGN KEY (id_empresa) REFERENCES Empresas(id_empresa)
ON DELETE CASCADE;

ALTER TABLE EmpresaTags
ADD CONSTRAINT fk_empresatag_tag
FOREIGN KEY (id_tag) REFERENCES Tags(id_tag)
ON DELETE CASCADE;

-- Tabla: EmpresaInstrumentos
ALTER TABLE EmpresaInstrumentos
ADD CONSTRAINT fk_empresainst_empresa
FOREIGN KEY (id_empresa) REFERENCES Empresas(id_empresa)
ON DELETE CASCADE;

ALTER TABLE EmpresaInstrumentos
ADD CONSTRAINT fk_empresainst_programa
FOREIGN KEY (id_programa) REFERENCES Programas(id_programa);

ALTER TABLE EmpresaInstrumentos
ADD CONSTRAINT fk_empresainst_instrumento
FOREIGN KEY (id_instrumento) REFERENCES Instrumentos(id_instrumento);

-- Tabla: FuncionarioUnidades
ALTER TABLE FuncionarioUnidades
ADD CONSTRAINT fk_funcunidad_funcionario
FOREIGN KEY (id_funcionario) REFERENCES Funcionarios(id_funcionario)
ON DELETE CASCADE;

ALTER TABLE FuncionarioUnidades
ADD CONSTRAINT fk_funcunidad_unidad
FOREIGN KEY (id_unidad) REFERENCES UnidadesAtencion(id_unidad)
ON DELETE CASCADE;

-- Tabla: Recordatorios
ALTER TABLE Recordatorios
ADD CONSTRAINT fk_recordatorio_empresa
FOREIGN KEY (id_empresa) REFERENCES Empresas(id_empresa)
ON DELETE CASCADE;

ALTER TABLE Recordatorios
ADD CONSTRAINT fk_recordatorio_funcionario
FOREIGN KEY (id_funcionario) REFERENCES Funcionarios(id_funcionario);

-- Tabla: Autodiagnosticos
ALTER TABLE Autodiagnosticos
ADD CONSTRAINT fk_autodiag_empresa
FOREIGN KEY (id_empresa) REFERENCES Empresas(id_empresa)
ON DELETE CASCADE;

-- Tabla: AuditLog
ALTER TABLE AuditLog
ADD CONSTRAINT fk_auditlog_funcionario
FOREIGN KEY (id_funcionario) REFERENCES Funcionarios(id_funcionario);
```

---

## Índices Recomendados

```sql
-- Performance para búsquedas frecuentes
CREATE INDEX idx_empresas_rut ON Empresas(rut);
CREATE INDEX idx_empresas_estado ON Empresas(estado);
CREATE INDEX idx_empresas_etapa ON Empresas(etapa_incubadora);

-- Reuniones por fecha
CREATE INDEX idx_reuniones_fecha ON Reuniones(fecha_hora);
CREATE INDEX idx_reuniones_empresa ON Reuniones(id_empresa);
CREATE INDEX idx_reuniones_funcionario ON Reuniones(id_funcionario);
CREATE INDEX idx_reuniones_unidad ON Reuniones(id_unidad);

-- Búsqueda rápida de recordatorios pendientes
CREATE INDEX idx_recordatorios_pendiente ON Recordatorios(estado, fecha_envio);

-- Auditoría
CREATE INDEX idx_auditlog_funcionario_fecha ON AuditLog(id_funcionario, timestamp);
CREATE INDEX idx_auditlog_tabla ON AuditLog(tabla_afectada);

-- Historial
CREATE INDEX idx_historial_empresa_anio ON HistorialEmpresa(id_empresa, anio);

-- Tags
CREATE INDEX idx_empresatags_empresa ON EmpresaTags(id_empresa);
CREATE INDEX idx_empresatags_tag ON EmpresaTags(id_tag);

-- Unidades de atención
CREATE INDEX idx_unidades_ciudad ON UnidadesAtencion(ciudad);
CREATE INDEX idx_funcunidad_funcionario ON FuncionarioUnidades(id_funcionario);
```

---

## Consultas Comunes (Ejemplos)

### 1. Empresas que requieren seguimiento

Identifica clientes sin reuniones en 3 meses.

```sql
SELECT e.*, MAX(r.fecha_hora) as ultima_reunion
FROM Empresas e
LEFT JOIN Reuniones r ON e.id_empresa = r.id_empresa
WHERE e.estado = 'cliente'
GROUP BY e.id_empresa
HAVING ultima_reunion < DATE_SUB(NOW(), INTERVAL 3 MONTH)
   OR ultima_reunion IS NULL;
```

### 2. Métricas de evolución anual

```sql
SELECT e.nombre_comercial,
       h1.facturacion as facturacion_2024,
       h2.facturacion as facturacion_2025,
       ((h2.facturacion - h1.facturacion) / h1.facturacion * 100) as crecimiento_porcentual
FROM Empresas e
JOIN HistorialEmpresa h1 ON e.id_empresa = h1.id_empresa AND h1.anio = 2024
JOIN HistorialEmpresa h2 ON e.id_empresa = h2.id_empresa AND h2.anio = 2025
WHERE h1.facturacion > 0;
```

### 3. Carga de trabajo por funcionario

```sql
SELECT f.nombre_completo,
       COUNT(r.id_reunion) as reuniones_programadas,
       COUNT(DISTINCT r.id_empresa) as empresas_atendidas
FROM Funcionarios f
LEFT JOIN Reuniones r ON f.id_funcionario = r.id_funcionario
WHERE r.estado = 'programada'
  AND r.fecha_hora >= NOW()
GROUP BY f.id_funcionario
ORDER BY reuniones_programadas DESC;
```

### 4. Recordatorios pendientes de envío

```sql
SELECT r.*, e.nombre_comercial, f.nombre_completo, f.email
FROM Recordatorios r
JOIN Empresas e ON r.id_empresa = e.id_empresa
JOIN Funcionarios f ON r.id_funcionario = f.id_funcionario
WHERE r.estado = 'pendiente'
  AND r.fecha_envio <= NOW()
ORDER BY r.fecha_envio ASC;
```

### 5. Carga por unidad de atención

```sql
SELECT u.nombre as unidad,
       COUNT(r.id_reunion) as reuniones_programadas,
       AVG(r.duracion_minutos) as duracion_promedio
FROM UnidadesAtencion u
LEFT JOIN Reuniones r ON u.id_unidad = r.id_unidad
WHERE r.estado = 'programada'
  AND r.fecha_hora >= NOW()
GROUP BY u.id_unidad
ORDER BY reuniones_programadas DESC;
```

### 6. Funcionarios por unidad

```sql
SELECT u.nombre as unidad,
       COUNT(fu.id_funcionario) as total_funcionarios,
       SUM(CASE WHEN fu.es_responsable THEN 1 ELSE 0 END) as responsables
FROM UnidadesAtencion u
LEFT JOIN FuncionarioUnidades fu ON u.id_unidad = fu.id_unidad
WHERE u.activo = TRUE
GROUP BY u.id_unidad;
```

---

## Consideraciones de Diseño

### Seguridad

- **Passwords**: Usar bcrypt/argon2 con salt (mínimo 10 rounds)
- **Datos sensibles**: Encriptar RUT, facturación en reposo
- **Auditoría completa**: Tabla AuditLog registra todas las operaciones críticas

### Performance

- Índices en campos de búsqueda frecuente (RUT, fechas, estados)
- Particionamiento por año en HistorialEmpresa si crece mucho
- Cache para catálogos (TiposReunion, Tags, Programas)

### Escalabilidad

- Diseño modular permite agregar nuevas tablas sin afectar existentes
- JSON para datos semi-estructurados (respuestas autodiagnóstico)
- Sistema de tags flexible para categorización futura

### Historial y Métricas

- HistorialEmpresa permite análisis temporal
- AuditLog asegura trazabilidad completa
- Soft deletes en tablas críticas (campo `activo`)

---

## Notas Finales

1. **Motor recomendado**: MySQL 8.0+ o PostgreSQL 14+ (ambos soportan JSON nativo)
2. **Collation**: `utf8mb4_unicode_ci` para soporte completo de caracteres español
3. **Timezone**: Configurar servidor en UTC, convertir en aplicación según usuario
4. **Backups**: Diarios automáticos con retención de 30 días mínimo
5. **Testing**: Poblar con datos de prueba representativos del contexto uruguayo
6. **Unidades de atención**: Reflejan sedes físicas (Salto, Guaviyu de Arapey, Biblioteca) y virtuales

---

_Modelo generado: 4 de febrero de 2026_  
_Basado en: Entrevistas stakeholders, SRS, y documentación Reto IV_
