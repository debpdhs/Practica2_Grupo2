## INTEGRANTES.
| Nombre | Cargo | URL GitHub |
|---|:---:|---:|
| Daniel Alquinga | :technologist: Desarrollador | https://github.com/superdavi/Practica2_Grupo2 |
| Daniel Baldeon | :technologist: Desarrollador | https://github.com/debpdhs/Practica2_Grupo2.git |
| Bryan Miño | :technologist: Desarrollador | https://github.com/bmiomi/tarea2-grupo2.git |
| Wilson Segovia | :technologist: Desarrollador | https://github.com/segoviawilson/Practica2_Grupo2.git |
| Leonardo Tuguminago | :technologist: Desarrollador | https://github.com/Tuguminago/Practica2_Grupo2.git |

# 1. Despliegue de PGADMIN con docker compose utilizando imágenes diferentes a las de las prácticas

Este proyecto implementa el docker compose de una base de datos PostgreSQL y presenta su interfaz gráfica PGADMIN.

## 1.1. Arquitectura del Sistema

- **Postgres 15.6**: Base de datos PostgreSQL y su version 15.6
- **PGAdmin 4.8.10**: Interfaz web para administración de la base de datos
- **Volúmenes**: Gestionados por Docker y almacenados en ./data/postgres:/var/lib/postgresql/data y ./data/pgadmin:/var/lib/pgadmin

# 2. Configuración e Instalación

### PASO 1: 📂 Estructura de Archivos

```bash
    tree -a

    Proyecto PGAdmin
    |
    |____ .env
    |____ README.md
    |____ compose.yaml
```

**Salida Esperada**

<img width="886" height="213" alt="image" src="https://github.com/user-attachments/assets/30092990-b98f-4862-9a16-e63e90436568" />

### PASO 2: Creación del directorio para guardar data portable

---

```bash
mkdir -p data/postgres data/pgadmin
```

**Salida Esperada**

<img width="886" height="213" alt="image" src="https://github.com/user-attachments/assets/94ccd115-6d8b-4aee-b0b5-80aaa1fdca27" />

**Explicación:**

- Permite la portabilidad de datos en dockers de bases de datos
    
### PASO 3: Listar y ver directorios creados

```bash
    tree -a

    Proyecto PGAdmin
    |
    |____ compose.yaml
    |____ data
            |____ pgadmin
            |____ postgres
    |____ .env
    |____ README.md
```
**Salida Esperada**

<img width="725" height="443" alt="directorio" src="https://github.com/user-attachments/assets/9e45efc1-708f-47d5-87a2-1a2215eb4e75" />

**Explicación:**

- **Directorio**: Revisar el directorio creado y revisar los archivos que crearán el docker de PGAdmin

### PASO 4: Dar permisos 777 a carpeta pgadmin y postgres

```bash
    sudo chmod 777 data/pgadmin
    sudo chmod 777 data/postgres
```

**Salida Esperada**

<img width="725" height="443" alt="permisos chmod" src="https://github.com/user-attachments/assets/d7e3c045-7748-4835-9d6e-55b2b396d78a" />

**Explicación:**

- **Permisos requeridos**: Se debe dar permisos de escritura, lectura y ejecución a esta carpeta caso contrario no se ejecutará el docker porque necesita esos permisos para ejecutarse sin problema

### PASO 5: Despliegue del Contenedor Docker PostgreSQL y PGAdmin

```bash
docker compose up -d
```

### Paso 5.1. Salida Esperada

<img width="676" height="597" alt="despliegue contendor" src="https://github.com/user-attachments/assets/c3dffb42-5f0a-45e6-9dbc-80f6ecdb2394" />

**Explicación:**

- **PGAdmin Container**: Proporciona interfaz web conectada al contenedor PostgreSQL
- **Ports**: PostgreSQL en puerto 5432, PGAdmin en puerto 8080
  
### Paso 5.2. Verificar Estado Up del Contendor

```bash
docker ps -a
```

<img width="1215" height="142" alt="listar los contendores" src="https://github.com/user-attachments/assets/5495871a-0490-47da-9926-addd2e48825e" />

### PASO 5: Ingreso al Portal del Servidor PGAdmin

```bash
http://localhost:8080
```
<img width="1215" height="142" alt="listar los contendores" src="https://github.com/user-attachments/assets/21b4ab97-51b8-48c0-9f07-68fff205b5bf" />

### PASO 6: Credenciales de Ingreso


```bash
usuario: grupo2.mdmq@gmail.com
password: DV353rhfU3
```


### PASO 7: Revisamos la Estructura de Base de Datos


<img width="1275" height="705" alt="revisamos la estructura de la bd" src="https://github.com/user-attachments/assets/dc74e0d5-f413-47cd-9795-c7bc27ee2c70" />

### PASO 8: Scritp init.sql

```sql
-- Crear tabla de propietario
CREATE TABLE propietario (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cedula VARCHAR(15) NOT NULL,
    nombre VARCHAR(100) NOT NULL,
    telefono VARCHAR(20),
    direccion VARCHAR(150)
);

-- Crear tabla de vehiculo
CREATE TABLE vehiculo (
    id INT AUTO_INCREMENT PRIMARY KEY,
    placa VARCHAR(8) NOT NULL,
    marca VARCHAR(50) NOT NULL,
    modelo VARCHAR(50) NOT NULL,
    anio INT NOT NULL,
    propietario_id INT,
    FOREIGN KEY (propietario_id) REFERENCES propietario(id)
);

-- Insertar registros en propietario
INSERT INTO propietario (cedula, nombre, telefono, direccion) VALUES
('1111111111', 'Daniel Alquinga', '0992222222', 'ABC'),
('1222222222', 'Leonardo Tuguminago', '0993333333', 'DEF'),
('1333333333', 'Bryan Miño', '0994444444', 'GHI'),
('1444444444', 'Wilson Segovia', '0995555555', 'JKL'),
('1555555555', 'Daniel Baldeon', '0996666666', 'MNO');

-- Insertar registros en vehiculo
INSERT INTO vehiculo (placa, marca, modelo, anio, propietario_id) VALUES
('PBB-5555', 'Hyundai', 'i10', 2018, 1),
('PBB-2222', 'Mazda', 'Cx3', 2012, 2),
('PBB-3333', 'Chevrolet', 'Aveo', 2017, 3),
('PBB-9999', 'Nissan', 'Sentra', 2019, 4),
('PCC-4444', 'Toyota', '4Runner', 2022, 5);
```

**Explicación:**

- **Tabla propietario**: Almacena información personal de los dueños de vehículos
- **Tabla vehiculo**: Contiene datos de los vehículos con referencia al propietario
- **Relación**: Clave foránea entre vehículo y propietario (1:N)
- **Datos de prueba**: 5 propietarios y 5 vehículos para testing
  

### Configuración Adicional



**Archivo .env**

```env
MYSQL_ROOT_PASSWORD=admin123
MYSQL_DATABASE=dbVehiculos
MYSQL_USER=usuario
MYSQL_PASSWORD=clave123
```

### Comandos Útiles

```bash
# Ver contenedores en ejecución
docker ps

# Ver logs de un contenedor
docker logs db-mysql-vehiculos

# Conectar a MySQL directamente
docker exec -it db-mysql-vehiculos mysql -u root -p

# Detener todos los contenedores
docker stop db-mysql-vehiculos web_phpmyadmin_vehiculos

# Eliminar contenedores
docker rm db-mysql-vehiculos web_phpmyadmin_vehiculos

# Eliminar red
docker network rm netw-vehiculos
```


# 3. Conclusiones

**Logros Alcanzados**

- Implementación Exitosa: Se logró configurar un sistema distribuido utilizando Docker, separando la base de datos (MySQL) de la interfaz de administración (phpMyAdmin) en contenedores independientes.
- Gestión Eficiente de Redes: La creación de la red personalizada netw-vehiculos, permitió la comunicación segura entre contenedores, eliminando la necesidad de exponer servicios innecesarios al host.
- Persistencia de Datos Garantizada: El uso de volúmenes Docker (mysql_data), asegura que la información de propietarios y vehículos se mantenga intacta entre reinicios del sistema.
- Automatización de Inicialización: El script init.sql, automatiza la creación de tablas y datos de prueba, reduciendo errores manuales y garantizando consistencia en diferentes entornos.
- Separación de Configuración: El archivo .env, centraliza las variables sensibles, mejorando la seguridad y facilitando el despliegue en diferentes ambientes.

**Beneficios Obtenidos**

- Portabilidad: El sistema puede ejecutarse en cualquier máquina con Docker instalado
- Escalabilidad: Fácil agregar nuevos servicios o réplicas de contenedores
- Mantenibilidad: Cada servicio se actualiza independientemente
- Aislamiento: Los fallos en un contenedor no afectan a otros servicios
- Reproducibilidad: El entorno se puede recrear exactamente en cualquier momento


# 4. Recomendaciones.

 - Es necesario crear primero la red Docker y luego el contenedor, ya que este inconveniente se presento al momento de ejecutar la tarea. 
 - Se debe validar que la versión de MySQL sea compatible con phpMyAdmin, ya que una incompatibilidad entre ambas versiones podría causar errores al intentar levantar el contenedor.
