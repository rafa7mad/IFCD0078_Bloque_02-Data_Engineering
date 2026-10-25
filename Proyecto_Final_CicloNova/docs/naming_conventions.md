# CicloNova — Convenciones de nomenclatura

> **Proyecto:** Data Warehouse CicloNova · SQL Server · Arquitectura Medallion (Bronze → Silver → Gold)  
> **Documento:** `docs/naming_conventions.md`  
> **Referencia principal:** apartado **4.3. Convenciones de nombres** del ejercicio «Ejemplo de un modelo estrella».

## 1. Objetivo

Establecer reglas consistentes para nombrar objetos, columnas y archivos, de modo que sea sencillo reconocer su función, localizar su origen y mantener el Data Warehouse. Las reglas de la **sección 2** proceden del apartado 4.3 del material del ejercicio; las convenciones adicionales de las **secciones 3–5** organizan los elementos que hemos añadido durante la práctica.

## 2. Convenciones originales del ejercicio

| Elemento | Regla | Ejemplo |
|---|---|---|
| General | `snake_case`, minúsculas, en inglés y sin palabras reservadas | `customer_number` |
| Tablas Bronze y Silver | `<sistema_origen>_<entidad>`, conservando el nombre de la entidad de origen | `crm_cust_info` |
| Objetos Gold | `<categoría>_<entidad>`, con nombres orientados al negocio | `dim_customers`, `fact_sales` |
| Prefijos de Gold | `dim_` (dimensión), `fact_` (hechos), `report_` (informe) | `report_sales_monthly` |
| Claves subrogadas | `<entidad>_key` | `customer_key` |
| Columnas técnicas | Prefijo `dwh_` | `dwh_create_date` |
| Procedimientos de carga | `load_<capa>` | `load_bronze`, `load_silver` |

### 2.1. Aplicación a los esquemas SQL

Los esquemas identifican la etapa del procesamiento:

```sql
bronze.crm_cust_info
silver.crm_cust_info
gold.dim_customers
gold.dim_products
gold.fact_sales
```

Así, `silver.crm_cust_info` y `gold.dim_customers` pertenecen a capas diferentes y cumplen funciones distintas: el primero conserva la entidad transformada del CRM; el segundo expone una dimensión para análisis.

### 2.2. Objetos Gold

| Prefijo | Función | Objeto utilizado en CicloNova |
|---|---|---|
| `dim_` | Describe entidades del negocio | `gold.dim_customers`, `gold.dim_products` |
| `fact_` | Recoge eventos y métricas cuantificables | `gold.fact_sales` |
| `report_` | Prepararía una vista orientada a un informe concreto | `gold.report_customers` (**ejercicio propuesto; no se presenta como implementado**) |

**Importante:** en este proyecto los tres objetos Gold principales son **vistas SQL**. Los nombres `dim_` y `fact_` indican su papel dimensional, no que necesariamente sean tablas físicas.

## 3. Convenciones aplicadas a procedimientos y automatización

### 3.1. Procedimientos almacenados

Se utiliza el prefijo `load_` junto con el nombre de la capa y el esquema correspondiente:

```sql
EXEC bronze.load_bronze;
EXEC silver.load_silver;
```

El orden de procesamiento es **Bronze → Silver**; Gold se consulta a través de las vistas una vez finalizadas las cargas. Los procedimientos incorporan manejo de errores mediante `TRY...CATCH` y propagación con `THROW`.

### 3.2. SQL Server Agent

La automatización configurada en la práctica se identifica de forma descriptiva:

| Elemento | Nombre utilizado | Criterio |
|---|---|---|
| Trabajo de SQL Server Agent | `DWH - Carga diaria` | Indica propósito y frecuencia |
| Archivo de documentación/script del trabajo | `scripts/automation/job_carga_diaria.sql` | `job_` + descripción en `snake_case` |
| Programación | Diaria a las **06:00** | Configuración operativa, no parte del nombre |

Para futuras automatizaciones conviene mantener nombres legibles y que indiquen el propósito del proceso, sin introducir nuevos prefijos obligatorios que no figuren en el ejercicio.

## 4. Convenciones para archivos y repositorio

Se conserva la estructura de carpetas indicada en el apartado 4.2 del ejercicio, ampliada para las evidencias, la automatización y Power BI:

```text
dwh_project/                      # En GitHub: Proyecto_Final_CicloNova/
├── datasets/
│   ├── source_crm/
│   └── source_erp/
├── docs/
│   ├── architecture.md
│   ├── data_lineage.md
│   ├── data_model.md
│   ├── data_catalog.md
│   └── naming_conventions.md
├── images/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   ├── automation/
│   └── powerbi/
├── powerbi/
│   └── CicloNova_Dashboard.pbix
├── scripts/
│   ├── init_database.sql
│   ├── bronze/
│   │   ├── ddl_bronze.sql
│   │   └── proc_load_bronze.sql
│   ├── silver/
│   │   ├── ddl_silver.sql
│   │   └── proc_load_silver.sql
│   ├── gold/
│   │   └── ddl_gold.sql
│   └── automation/
│       └── job_carga_diaria.sql
├── tests/
│   ├── quality_checks_silver.sql
│   └── quality_checks_gold.sql
└── README.md
```

Esta estructura es la **organización acordada** para el proyecto; no debe interpretarse como una comprobación independiente de que todos los archivos estén publicados ya.

| Tipo de archivo | Convención | Ejemplo |
|---|---|---|
| Creación de estructuras | `ddl_<capa>.sql` | `ddl_silver.sql` |
| Procedimientos | `proc_load_<capa>.sql` | `proc_load_bronze.sql` |
| Pruebas de calidad | `quality_checks_<capa>.sql` | `quality_checks_gold.sql` |
| Documentación | Minúsculas, `snake_case`, extensión `.md` | `data_lineage.md` |
| Capturas de Power BI | Prefijo utilizado durante la práctica `FPBI_`, número correlativo y descripción | `FPBI_016_Dashboard_Final.JPG` |
| Archivo de Power BI | Nombre identificable del proyecto | `CicloNova_Dashboard.pbix` |

El nombre exacto de las capturas existentes debe respetarse en los enlaces Markdown, **incluidas las mayúsculas y la extensión** (`.JPG` frente a `.jpg`). GitHub diferencia mayúsculas y minúsculas en las rutas.

## 5. Convenciones en Power BI

En el modelo importado las vistas aparecen con nombres como **`gold fact_sales`**, **`gold dim_customers`** y **`gold dim_products`**. Estos son los nombres visibles dentro de Power BI; no sustituyen los identificadores SQL con punto (`gold.fact_sales`).

### 5.1. Medidas DAX

Las medidas se nombran en lenguaje de negocio, con espacios y acentos cuando mejoran su lectura:

```dax
Ventas Totales = SUM('gold fact_sales'[sales_amount])

Unidades Vendidas = SUM('gold fact_sales'[quantity])

Pedidos = DISTINCTCOUNT('gold fact_sales'[order_number])

Clientes con Compras = DISTINCTCOUNT('gold fact_sales'[customer_key])

Ticket Medio = DIVIDE([Ventas Totales], [Pedidos], 0)
```

| Ámbito | Ejemplo | Motivo |
|---|---|---|
| Nombre de objeto SQL | `gold.fact_sales` | Identifica esquema y vista |
| Nombre de tabla en Power BI | `'gold fact_sales'` | Nombre que Power BI utiliza en las expresiones DAX de esta práctica |
| Nombre de columna | `sales_amount` | Sigue la convención técnica del origen |
| Nombre de medida | `Ventas Totales` | Facilita su interpretación por el usuario del informe |
| Texto de visual | `Evolución de ventas` | Expresa el análisis, no la implementación |

## 6. Reglas de mantenimiento y comprobación

1. **Consistencia:** no mezclar convenciones para un mismo tipo de objeto sin justificación.
2. **Trazabilidad:** mantener la entidad original al pasar de Bronze a Silver; usar nombres de negocio en Gold.
3. **Claves:** utilizar el sufijo `_key` para las claves dimensionales y de relación.
4. **Vocabulario:** reservar `dim_`, `fact_` y `report_` para sus respectivas funciones analíticas.
5. **Sincronización:** cuando cambie una vista Gold, revisar `data_model.md`, `data_catalog.md` y las medidas DAX afectadas.
6. **Enlaces:** comprobar las rutas relativas y la capitalización de los archivos desde GitHub.
7. **Documentación fiel:** distinguir los objetos efectivamente implementados de los ejercicios opcionales o mejoras futuras.

### Ejemplo de enlace relativo

Desde este documento, que se encuentra en `docs/`, el script de creación de Gold se enlaza así:

```markdown
[DDL de Gold](../scripts/gold/ddl_gold.sql)
```

## 7. Referencias cruzadas

- [Arquitectura Medallion](architecture.md)
- [Linaje de datos](data_lineage.md)
- [Modelo dimensional](data_model.md)
- [Catálogo de datos](data_catalog.md)
- [Script Gold](../scripts/gold/ddl_gold.sql)
- [README principal](../README.md)

---

> **Nota de alcance:** las convenciones de la sección 2 proceden de la documentación de clase. Los apartados de automatización, capturas, repositorio y DAX describen las decisiones organizativas tomadas para la implementación CicloNova.
