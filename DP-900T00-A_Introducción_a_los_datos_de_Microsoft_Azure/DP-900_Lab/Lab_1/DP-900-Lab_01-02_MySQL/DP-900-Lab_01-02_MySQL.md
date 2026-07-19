## DP-900 Lab 01-02 MySQL

# Azure Database for MySQL

## Provisionar un recurso de Azure Database for MySQL

### Paso 1: Crear un nuevo recurso

- Ingresa al portal de Azure.
- Selecciona + Crear un recurso en la esquina superior izquierda.
- En el buscador, escribe Azure Database for MySQL.
- Selecciona la opción que aparece y haz clic en Crear.

![010_azure_portal](images/010_azure_portal.JPG)

![020_azure_mysql](images/020_azure_mysql.jpg)

<br>

### Paso 2: Seleccionar tipo de recurso

- Revisa las opciones disponibles para Azure Database for MySQL.
- En Tipo de recurso, selecciona Servidor flexible.
- Haz clic en Creacion rapida o avanzado(nuestro caso avanzada).


![030_flexible_server](images/030_flexible_server.jpg)

<br>

### Paso 3: Configurar la base de datos
En la página Crear base de datos SQL, introduce los siguientes valores:
- Suscripción: selecciona tu suscripción activa de Azure.
- Grupo de recursos: crea un nuevo grupo con un nombre que elijas.
- Nombre del servidor: escribe un nombre único para el servidor.
- Región: selecciona una ubicación cercana a ti.
- Versión de MySQL: déjala sin cambios.
- Tipo de carga de trabajo: selecciona para proyectos de desarrollo o hobby.
- Computación + almacenamiento: déjalo sin cambios.
- Zona de disponibilidad: déjalo sin cambios.
- Habilitar alta disponibilidad: déjalo sin cambios.
- Nombre de usuario del administrador: ingresa el nombre de usuario que quieras usar.
- Contraseña y Confirmar contraseña: establece una contraseña segura.

Luego, selecciona Siguiente: Redes.

![040_basics_server](images/040_basics_server.jpg)

![041_basics_server](images/041_basics_server.jpg)

![042_basics_server](images/042_basics_server.jpg)

<br>

### Paso 4: Configurar reglas de firewall

- En la sección Reglas de firewall, selecciona + Agregar dirección IP del cliente actual para permitir el acceso desde tu equipo.

![050_redes_ip](images/050_redes_ip.jpg)

<br>

### Paso 5: Revisar y crear

- Selecciona Revisar + Crear para validar la configuración.
- Luego, haz clic en Crear para iniciar la implementación.

![060_review_create](images/060_review_create.jpg)

![061_mysqlflexibleserver](images/061_mysqlflexibleserver.jpg)

<br>

### Paso 6: Esperar a la implementación

- Espera a que la implementación se complete.
- Cuando termine, selecciona Ir al recurso para acceder al servidor MySQL recién creado.

![062_mysqlflexibleserver_complete](images/062_mysqlflexibleserver_complete.jpg)

<br>

### Paso 7: Explorar y administrar el recurso

- Desde la página del recurso, puedes revisar todas las opciones para administrar tu Azure Database for MySQL.

![070_serverazuremysql](images/070_serverazuremysql.jpg)

### Opciones para administrar Azure Database for MySQL

### Settings

### - Databases

Podemos añadir o eliminar bases de datos.
Muestra las cuatro bases de datos del sistema que MySQL utiliza internamente para:
- Usuarios y permisos.
- Configuración del servidor.
- Información de rendimiento.
- Metadatos del sistema.

![071_databases](images/071_databases.jpg)

### - Server Parameters

Permiten configurar el comportamiento del motor MySQL mediante parámetros de administración como conexiones máximas, memoria, tiempos de espera y registro de actividad.
Concepto relacionado:
- Administración de bases de datos
- Configuración del motor de base de datos

![072_servers_parameters](images/072_servers_parameters.jpg)

### - Backup and Restore

Azure Database for MySQL incluye mecanismos de copia de seguridad y restauración administrados por la plataforma para facilitar la recuperación ante errores o pérdidas de datos.
Concepto relacionado:
- Disponibilidad
- Recuperación ante desastres
- Copias de seguridad

![073_backup_restore](images/073_backup_restore.jpg)

### Monitoring 

### - Metrics

Sirve para monitorizar el rendimiento mediante métricas como:
- CPU
- Memoria
- Conexiones
- IOPS
- Almacenamiento

![074_activity_log](images/074_activity_log.jpg)

### Activity Log

Muestra todas las operaciones realizadas sobre el recurso:
- Creación del servidor.
- Cambios de configuración.
- Eliminaciones.
- Actualizaciones.

![075_metrics](images/075_metrics.jpg)

<br>

## Conexion con CLI al servidor 

### Nos conectamos al servidor mediante CLI en Automation

> </>Bash
> 
> mysql -h serverazuremysql260619.mysql.database.azure.com -u serveradmin -p

![080_cli_conexion](images/080_cli_conexion.jpg)

<br>

### Creamos una base de datos de práctica nueva

> </> SQL
> 
> CREATE DATABASE laboratorio;

![081_cli_create](images/081_cli_create.jpg)

### Seleccionar la base de datos

> </> SQL> 
> 
> USE laboratorio;

![082_cli_use](images/082_cli_use.jpg)

<br>

### Crear una tabla

> </> SQL> 
> 
> CREATE TABLE productos (
    id INT PRIMARY KEY,
    nombre VARCHAR(50),
    precio DECIMAL(10,2)
> ); 

![083_cli_create_table](images/083_cli_create_table.jpg)

<br>

### Insertar datos

> </> SQL
> 
> INSERT INTO productos VALUES
> (1,'Portatil',850.00),
> (2,'Monitor',220.00),
> (3,'Teclado',45.00);

![084_cli_insert](images/084_cli_insert.jpg)

<br>

### Consultar los datos

> </> SQL
> 
> SELECT * FROM productos;

![085_cli_select](images/085_cli_select.jpg)

<br>

### Consulta con filtro

> </> SQL> 
> SELECT nombre, precio 
> FROM productos
> WHERE precio > 100;

![086_cli_select_where](images/086_cli_select_where.jpg)

<br>

## Limpieza, eliminar el recurso

### En el portal de Azure, accedemos a grupo de recursos

![090_resource_groups](images/090_resource_groups.jpg)

### Seleccione «Eliminar grupo de recursos», confirme la eliminación introduciendo el nombre del grupo y, a continuación, seleccione «Eliminar».

![091_rgmysql](images/091_rgmysql.jpg)

![092_rgmysql_delete](images/092_rgmysql_delete.jpg)

### Comprobamos que el recuso ha sido eliminado

![093_resource_groups_f](images/093_resource_groups_f.JPG)

