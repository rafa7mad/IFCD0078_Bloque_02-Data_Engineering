# Introducción al análisis de datos de Microsoft Azure en Azure

El crecimiento exponencial de los datos en los últimos años está impulsando la transformación digital de empresas y otras organizaciones, ya que les permite tomar de decisiones de forma rápida e informada a través del análisis de datos. Microsoft Azure proporciona varios servicios que puede combinar para crear soluciones de análisis a gran escala que aprovechen las tecnologías y técnicas más recientes para la ingesta, el almacenamiento, el modelado y la visualización de datos. Esta ruta de aprendizaje lo ayuda a prepararse para la certificación [Azure Data Fundamentals](https://learn.microsoft.com/es-es/certifications/azure-data-fundamentals?azure-portal=true).

### Requisitos previos
Antes de iniciar esta ruta de aprendizaje, debe tener un conocimiento fundamental de los conceptos básicos de los datos, los datos relacionales y los datos no relacionales.

<br>

---

## Exploración de los aspectos básicos del análisis a gran escala

Las organizaciones usan plataformas de análisis para crear soluciones de análisis de datos a gran escala que generan conclusiones e impulsan el éxito. Microsoft proporciona varias tecnologías que puede combinar para crear una solución de análisis de datos a gran escala.

Objetivos de aprendizaje
En este módulo aprenderá a:
- Descripción de la arquitectura de un almacenamiento de datos.
- Describir las características clave de las canalizaciones de ingesta de datos.
- Identificar tipos comunes de almacén de datos analíticos y servicios de Azure relacionados.
- Uso de Microsoft Fabric para ingerir y analizar datos.


### Requisitos previos

Antes de iniciar este módulo, debe tener un conocimiento conceptual de los datos y las bases de datos, y estar familiarizado con los servicios de Microsoft Azure para cargas de trabajo de datos como Azure Storage, Azure SQL Database y Azure Cosmos DB.

<br>

---

### Introducción

Antes de explorar cómo se crean las soluciones de análisis a gran escala, vamos a presentar las dos principales plataformas de análisis de Azure que encontrará a lo largo de este módulo.

Microsoft Fabric es Microsoft plataforma unificada de análisis de software como servicio (SaaS). Ofrece ingeniería de datos, almacenamiento de datos, análisis en tiempo real, ciencia de datos y Power BI juntos en un único área de trabajo basada en explorador sobre una capa de almacenamiento compartida denominada OneLake. No administra servidores ni clústeres, crea áreas de trabajo y elementos y Microsoft ejecuta la infraestructura.

Azure Databricks es una plataforma de análisis en la nube basada en Apache Spark. Está optimizado para la ingeniería de datos a gran escala, la ciencia de datos y el análisis de SQL a través de formatos abiertos de almacén de lago de datos. Utiliza Delta Lake —un formato de almacenamiento de código abierto que añade transacciones, aplicación de esquemas y control de versiones sobre los archivos Parquet—, lo que te proporciona la fiabilidad propia de un lakehouse a escala. Databricks se ejecuta como un servicio administrado dentro de tu suscripción de Azure y es una opción habitual para los equipos que necesitan flujos de trabajo centrados en el código con Spark y basados en cuadernos interactivos.

### Análisis a gran escala

Las soluciones de análisis de datos a gran escala combinan el almacenamiento de datos convencional que se usa para admitir inteligencia empresarial (BI) con técnicas usadas para el análisis de macrodatos. Normalmente, una solución de almacenamiento de datos convencional implica copiar datos de almacenes de datos transaccionales en una base de datos relacional con un esquema optimizado para consultar y crear modelos multidimensionales. Las soluciones de procesamiento de macrodatos se usan con grandes volúmenes de datos en varios formatos, que se cargan o capturan por lotes en flujos en tiempo real y se almacenan en un lago de datos desde el que los motores de procesamiento distribuidos como Apache Spark los procesan.

<br>

![11_large-scale-analytics.png](images/11_large-scale-analytics.png)

La combinación de almacenamiento de lago de datos flexible y análisis de SQL de almacenamiento de datos ha llevado a la aparición de un diseño de análisis a gran escala a menudo denominado almacenamiento de datos.

> ! Nota: 
> 
> Reconocemos que a diferentes personas les gusta aprender de diferentes maneras. Puede optar por completar este módulo en formato basado en vídeo o puede leer el contenido como texto e imágenes. El texto contiene más detalle que los vídeos, por lo que, en algunos casos, es posible que desee hacer referencia a él como material complementario para la presentación de vídeo.

<br>

---

### Descripción de la arquitectura de un almacenamiento de datos

1/2/7

https://learn.microsoft.com/es-es/training/modules/examine-components-of-modern-data-warehouse/2-describe-warehousing?pivots=text







<br>

---