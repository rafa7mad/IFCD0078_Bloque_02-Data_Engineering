# Azure Data Explorer (ADX)

## Actividad 1 - Principales servicios de Azure orientado a Datos

### IFCD0078 - Data Engineering

**Alumno:** Rafael Velasco

**Servicio Azure seleccionado:** Azure Data Explorer

**Repositorio:** IFCD0078_Bloque_02-Data_Engineering

**Fecha:** Junio 2026

---

# Índice

1. Introducción
2. ¿Qué es Azure Data Explorer?
3. Características principales
4. Arquitectura de Azure Data Explorer
5. Creación y configuración del servicio
6. Lenguaje Kusto Query Language (KQL)
7. Caso práctico 1: Monitorización IoT
8. Caso práctico 2: Análisis de logs y telemetría
9. Comparación con otros servicios Azure
10. Conclusiones
11. Referencias

---

## 1. Introducción

Texto...

---

## 2. ¿Qué es Azure Data Explorer?

Azure Data Explorer (ADX) es un servicio de análisis de datos totalmente administrado de Microsoft Azure diseñado para recopilar, almacenar y analizar grandes volúmenes de datos en tiempo casi real. Está especialmente orientado al procesamiento de registros (logs), telemetría, eventos y datos procedentes de dispositivos IoT, permitiendo obtener información valiosa con baja latencia y gran capacidad de escalado.

La Figura 1 muestra el flujo de trabajo general de Azure Data Explorer. El servicio permite crear bases de datos, ingerir datos desde múltiples orígenes como Azure Event Hubs, Azure Blob Storage o Azure IoT Hub, procesarlos mediante el motor analítico de ADX y consultarlos utilizando Kusto Query Language (KQL). Los resultados pueden visualizarse posteriormente mediante herramientas como Power BI o aplicaciones personalizadas.

Gracias a su arquitectura escalable y a su capacidad para analizar millones de eventos por segundo, Azure Data Explorer es una solución ampliamente utilizada en escenarios de monitorización de infraestructuras, análisis de telemetría, observabilidad, ciberseguridad y análisis de datos IoT.

![Flujo de trabajo de Azure Data Explorer](images/ADX_workflow.png)

*Figura 1. Flujo de trabajo de Azure Data Explorer. Fuente: Microsoft Learn.*

### Capacidades principales

- Ingesta de datos en tiempo real y por lotes.
- Consultas rápidas mediante Kusto Query Language (KQL).
- Escalado automático para grandes volúmenes de datos.
- Integración con Power BI, Azure Monitor y Microsoft Fabric.
- Soporte para escenarios de IoT, telemetría y análisis de registros.


## 3. Características principales

Azure Data Explorer incorpora diversas características que lo convierten en una plataforma eficiente para el análisis de grandes volúmenes de datos. Su diseño está orientado a escenarios donde es necesario recopilar, almacenar y consultar información con una latencia muy baja y una alta capacidad de escalado.

| Característica                    | Descripción                                                                                        |
| --------------------------------- | -------------------------------------------------------------------------------------------------- |
| Análisis en tiempo real           | Permite consultar datos pocos segundos después de su ingesta.                                      |
| Alta escalabilidad                | Puede escalar horizontalmente para procesar grandes volúmenes de información.                      |
| Servicio PaaS                     | Microsoft administra la infraestructura, reduciendo las tareas de mantenimiento.                   |
| Kusto Query Language (KQL)        | Lenguaje de consultas optimizado para explorar y analizar datos rápidamente.                       |
| Ingesta flexible                  | Admite datos procedentes de Azure Event Hubs, IoT Hub, Blob Storage, Data Factory y otras fuentes. |
| Integración con Azure             | Se integra con servicios como Power BI, Azure Monitor, Microsoft Sentinel y Microsoft Fabric.      |
| Optimizado para telemetría y logs | Diseñado específicamente para analizar eventos, registros y datos IoT.                             |

### Ventajas de Azure Data Explorer

* Consultas rápidas sobre grandes volúmenes de datos.
* Capacidad para trabajar con datos históricos y datos en tiempo real.
* Escalado automático según la carga de trabajo.
* Amplio ecosistema de integración con servicios de Azure.
* Soporte para visualización y creación de paneles interactivos.

Estas características hacen que Azure Data Explorer sea una solución especialmente adecuada para escenarios de monitorización, observabilidad, análisis de telemetría, ciberseguridad y análisis de datos procedentes de dispositivos IoT.


## 4. Arquitectura de Azure Data Explorer

![Arquitectura de Azure Data Explorer](images/ADX_data_ingestion.png)

*Figura 2. Arquitectura de ingesta y consumo de datos en Azure Data Explorer. Fuente: Microsoft Learn.*

La arquitectura de Azure Data Explorer está diseñada para procesar grandes volúmenes de datos procedentes de múltiples fuentes y ponerlos a disposición de los usuarios para su análisis en tiempo casi real. El servicio admite tanto cargas de datos por lotes (batch) como flujos continuos de eventos (streaming).

Las fuentes de datos pueden incluir registros de aplicaciones, telemetría de dispositivos IoT, archivos almacenados en Azure Data Lake Storage o información procedente de aplicaciones empresariales. Estos datos son ingeridos mediante servicios como Azure Event Hubs, Azure IoT Hub, Apache Kafka o Azure Data Factory.

Una vez almacenados en Azure Data Explorer, los datos pueden ser consultados mediante Kusto Query Language (KQL) para realizar análisis, generar informes o entrenar modelos de aprendizaje automático. Los resultados pueden consumirse desde herramientas como Power BI, Grafana o aplicaciones personalizadas.

### Componentes principales

* **Fuentes de datos:** aplicaciones, dispositivos IoT, registros y archivos.
* **Servicios de ingesta:** Event Hubs, IoT Hub, Kafka y Azure Data Factory.
* **Motor de Azure Data Explorer:** almacenamiento y procesamiento analítico.
* **Herramientas de análisis:** KQL, Azure Synapse Analytics y Azure Machine Learning.
* **Visualización:** Power BI, Grafana y aplicaciones web.








