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

<img width="1275" height="705" alt="listar los contendores" src="https://github.com/user-attachments/assets/5495871a-0490-47da-9926-addd2e48825e" />

### PASO 6: Ingreso al Portal del Servidor PGAdmin

```bash
http://localhost:8080
```
<img width="1275" height="705" alt="listar los contendores" src="https://github.com/user-attachments/assets/21b4ab97-51b8-48c0-9f07-68fff205b5bf" />

### PASO 7: Credenciales de Ingreso


```bash
usuario: grupo2.mdmq@gmail.com
password: DV353rhfU3
```
**Salida Esperada**

<img width="1275" height="705" alt="ingresar pgadmin" src="https://github.com/user-attachments/assets/a7ce96cd-fec1-44a3-9ea2-03253bb58bc7" />

### PASO 8: Despliegue de PGAdmin

<img width="1275" height="705" alt="despliegue" src="https://github.com/user-attachments/assets/668c5124-8840-4d01-be98-ad2da48c6ef7" />

### PASO 9  

### Configuración Adicional



**Archivo .env**

```env
POSTGRES_USER=grupo2
POSTGRES_PW=P5xi9i2n8l
POSTGRES_DB=dbGrupo2
PGADMIN_MAIL=grupo2.mdmq@gmail.com
PGADMIN_PW=DV353rhfU3
```

### Comandos Útiles

```bash
# Ver contenedores en ejecución
docker ps

# Ver logs de un contenedor
docker logs postgres_db

# Detener todos los contenedores
docker stop postgres_db pgadmin4

# Eliminar contenedores
docker rm postgres_db pgadmin4

```


# 3. Conclusiones

- Implementación Exitosa: Se logró configurar un sistema distribuido utilizando Docker, separando la base de datos (PostgreSQL) de la interfaz de administración (PGAdmin) en contenedores independientes.
- Uso de Docker Compose: Creacion de un solo archivo donde se incluyen todas las sentencias para la creación de uno o más docker con sus configuraciones .
- Separación de Configuración: El archivo .env, centraliza las variables sensibles, mejorando la seguridad y facilitando el despliegue en diferentes ambientes.

# 4. Recomendaciones

 - Es importante verificar los permisos de las carpetas ya que estas muchas veces no cuentan con permisos de lectura, escritura y ejecución, por lo que hay que darles permisos con los comandos CHMOD y las variables del 777.
 - Revisar los puertos habilitados para la ejecución del contenedor, evitando conflictos al momento del despliegue.
 - Se utilizaron volumnenes para hacerle portable el contenedor debido a que Bind Mount no permite portabilidad.
 - Con Docker Compose puedes levantar, administrar y escalar múltiples contenedores con un solo archivo (docker-compose.yml), en lugar de ejecutar manualmente docker run para cada servicio.
