## DP-900 - Lab 01-03 PostgreSQL

# Azure Database for PostgreSQL

## Provisionar un recurso de Azure Database for PostgreSql

### Paso 1: Acceso al portal de Azure

- Ingresamos al portal de Azure y iniciamos sesión con nuestras credenciales.

![010_azure_portal](images/010_azure_portal.JPG)

<br>

### Paso 2: Crear recurso de Azure Database for PostgreSql

- En el portal, seleccionamos Crear un recurso.
- Buscamos Azure Database for PostgreSQL y seleccionamos la opción Azure Database for PostgreSQL.
- Hacemos clic en Crear.

![020_azure_postgresql](images/020_azure_postgresql.jpg)

<br>

### Paso 3: Configuración del recurso

- Rellenamos los campos con las opciones que deseemos, como:
    - Nombre del servidor.
    - Tipo de suscripción.
    - Grupo de recursos (puede ser uno nuevo o existente).
    - Ubicación (región).
    - Versión de PostgreSQL.

![030_basic_postgresql](images/030_basic_postgresql.jpg)

- Establecemos el tipo de autenticación que queramos (por ejemplo, autenticación SQL).
- En la sección de redes, agregamos nuestra IP actual para permitir el acceso desde nuestra ubicación.
- Finalmente, revisamos toda la configuración y hacemos clic en Revisar y crear.

![031_basic_postgresql_auth](images/031_basic_postgresql_auth.jpg)

![032_networking_postgresql](images/032_networking_postgresql.jpg)

![033_review_create](images/033_review_create.jpg)

<br>

### Paso 4: Implementación y acceso al recurso

- Esperamos a que se complete la creación del recurso.
- Una vez creado, podemos acceder a él haciendo clic en el botón Ir al recurso.

![040_postgresql_fsinprogress](images/040_postgresql_fsinprogress.jpg)

![041_postgresql_fscomplete](images/041_postgresql_fscomplete.JPG)

<br>

### Opciones para administrar Azure Database for PostgreSQL

![050_serverpostgresql](images/050_serverpostgresql.JPG)

### Settings

### - Databases
Podemos añadir o eliminar bases de datos.
Muestra dos bases de datos del sistema que MySQL utiliza internamente para y otra para el usuario por defecto.
- Usuarios y permisos.
- Configuración del servidor.
- Información de rendimiento.
- Metadatos del sistema.

![051_databases](images/051_databases.JPG)

<br>

### - Server Parameters
PostgreSQL tiene muchísimos parámetros configurables relacionados con:
- Memoria
- Planificador de consultas
- Logging
- Replicación

![052_servers_parameters](images/052_servers_parameters.JPG)


### - Backup and Restore

Azure Database for PostgreSQL incorpora mecanismos automáticos de copia de seguridad y restauración administrados por la plataforma. Estas funcionalidades permiten recuperar los datos ante errores, pérdidas de información o incidencias operativas, mejorando la disponibilidad y la protección de la base de datos.

Conceptos relacionados:
- Copias de seguridad
- Recuperación de datos
- Disponibilidad
- Continuidad del servicio

![053_backup_restore](images/053_backup_restore.JPG)

### Monitoring 

### - Metrics

La sección Metrics permite supervisar el estado y rendimiento del servidor PostgreSQL mediante indicadores como uso de CPU, memoria, almacenamiento, conexiones activas y operaciones de entrada/salida. Estas métricas ayudan a identificar problemas de rendimiento y a optimizar los recursos utilizados.

Conceptos relacionados:
- Monitorización
- Rendimiento
- Observabilidad
- Administración de recursos

![054_metrics](images/054_metrics.JPG)

### Activity Log

El registro de actividad almacena las operaciones realizadas sobre el recurso, como la creación del servidor, modificaciones de configuración, actualizaciones y eliminaciones. Esta información resulta útil para auditoría, seguimiento de cambios y resolución de incidencias.

Conceptos relacionados:
- Auditoría
- Trazabilidad
- Administración de recursos
- Seguridad

![055_activity_log](images/055_activity_log.JPG)

<br>

## Conexion con CLI al servidor 

### Nos conectamos al servidor mediante CLI

> </>Bash
> 
> psql "host=serverpostgresql20260621.postgres.database.azure.com port=5432 dbname=postgres user=serveradmin password=Scooby123+ sslmode=require" 

![060_cli_conexion](images/060_cli_conexion.JPG)

<br>

### Creamos una base de datos de práctica nueva

> </> SQL
> 
> CREATE DATABASE laboratorio;

![061_cli_create](images/061_cli_create.jpg)

### Cambiamos a la base de datos creada

> </> SQL> 
> 
> \connect laboratorio

![062_cli_connect](images/062_cli_connect.jpg)

<br>

### Crear una tabla

> </> SQL> 
> 
> CREATE TABLE productos (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    precio DECIMAL(10,2)
> ); 

![063_cli_create_table](images/063_cli_create_table.jpg)

<br>

### Insertar datos

> </> SQL
> 
> INSERT INTO productos VALUES
> (1,'Portatil',850.00),
> (2,'Monitor',220.00),
> (3,'Teclado',45.00);

![064_cli_insert](images/064_cli_insert.jpg)

<br>

### Consultar los datos

> </> SQL
> 
> SELECT * FROM productos;

![065_cli_select](images/065_cli_select.jpg)

<br>

### Consulta con filtro

> </> SQL> 
> SELECT nombre, precio 
> FROM productos
> WHERE precio > 100;

![066_cli_select_where](images/066_cli_select_where.jpg)

<br>

## Conexión al servidor PostgreSQL con Visual Studio Code

También podemos conectarnos a través de Visual Studio Code

![067_connect_vscode](images/067_connect_vscode.jpg)

### Consultar los datos

Seleccionamos una New Query.

![068_vscode_newquery](images/068_vscode_newquery_l.jpg)


Insertamos el código SQL

> </> SQL
> 
> SELECT * FROM productos;

![069_vscode_select](images/069_vscode_select.JPG)

<br>

## Limpieza, eliminar el recurso

### En el portal de Azure, accedemos a grupo de recursos

![070_resource_groups](images/070_resource_groups.jpg)

### Seleccione «Eliminar grupo de recursos», confirme la eliminación introduciendo el nombre del grupo y, a continuación, seleccione «Eliminar».

![071_rgmysql](images/071_rgmysql_delete.jpg)

![072_rgmysql_delete](images/072_rgmysql_delete.jpg)

### Comprobamos que el recuso ha sido eliminado

![073_resource_groups_f](images/073_resource_groups_f.JPG)



