# Databricks-SQL-Warehouse-AI-Chatbot

![image](https://github.com/user-attachments/assets/efa59c46-985c-46d8-bbfa-39449d18621d)  ![image](https://github.com/user-attachments/assets/d2bed958-3f73-4ae7-817d-afe5168d68ff)


## 🎯 Descripción del Proyecto

Este proyecto demuestra la construcción de un Data Warehouse moderno desde cero utilizando Databricks SQL Warehouse. En lugar de escribir pipelines ETL complejos en Python, se adopta el paradigma ELT (Extract, Load, Transform), donde los datos crudos se cargan primero y las transformaciones ocurren directamente en el motor SQL del Warehouse.

El objetivo del proyecto es la implementación de un Esquema Estrella (Star Schema) utilizando datos simulados de una aerolínea, aplicando técnicas de modelado dimensional, rastreo de cambios históricos (SCD), y configurando las capas de rendimiento y gobernanza de Databricks.

## 🏗️ Arquitectura del Modelo de Datos (Star Schema)

El diseño central de este proyecto separa los datos transaccionales (Hechos) de los datos descriptivos (Dimensiones), optimizando las consultas analíticas y evitando la redundancia.


![image](https://github.com/user-attachments/assets/f39a0677-b67d-49e9-b394-176c91a4303a)

🧠 **Cómo leer este diagrama (La regla del 1 a Muchos)**

La magia del Star Schema es que separa los datos en dos tipos de tablas:

**1. Las tablas de los bordes (Dimensiones):**
Representan las "entidades" del negocio. Son descriptivas y cambian muy poco en el tiempo.

Tabla DIM_PASSENGERS: Aquí solo guardas el nombre y género del pasajero.
Tabla DIM_AIRPORTS: Aquí guardas la ciudad y el país del aeropuerto.
Nota: Ambas tienen una PK (Primary Key). Es su identificador único (ej. P0001). Esa PK no se repite nunca en su propia tabla.

**2. La tabla del centro (Tabla de Hechos):**
Representa un evento o transacción del negocio. Son datos numéricos que cambian constantemente.

Tabla FACT_BOOKINGS: Aquí guardas que el pasajero P0001 compró un vuelo en el aeropuerto A001 por $850.
Las flechas (FK - Foreign Keys): En lugar de escribir "Juan Pérez" en la tabla de hechos, solo guardas el ID (P0001). Esto evita duplicar nombres largos y ahorra millones en costos de almacenamiento y procesamiento en la nube.

________________________________________________________________________________________________________________________________________________________________________________________________________________
🧠 Habilidades de Ingeniería de Datos Demostradas
________________________________________________________________________________________________________________________________________________________________________________________________________________

Este proyecto no es solo "escribir SELECTs". Abarca el ciclo de vida completo del Data Warehousing moderno:

1. Modelado Dimensional (Star Schema)
Diseño e implementación de tablas de hechos transaccionales (fact_bookings) y tablas de dimensiones (dim_passengers, dim_airports).
Separación estricta de atributos descriptivos para evitar anomalías de actualización y redudancia de datos.
2. Gestión de Cambios Lenta (Slowly Changing Dimensions - SCD)
Conceptos aprendidos: SCD Tipo 1 (Sobrescribir datos antiguos) vs. SCD Tipo 2 (Guardar historial de cambios añadiendo nuevas filas).
Implementación en Databricks: Uso de MERGE INTO para aplicar estas técnicas sobre tablas Delta Lake subyacentes sin romper la integridad de los reportes.
3. Ingesta Moderna: Auto CDC & Streaming Tables
El problema clásico: Los pipelines ETL tradicionales exigen escribir código para detectar qué cambió en la base de datos origen.
La solución moderna: Implementación conceptual de Change Data Capture (CDC) nativo de Databricks y Streaming Tables, que automatizan la ingesta incremental sin código personalizado, permitiendo que la capa de análisis esté siempre actualizada en tiempo real.
4. Rendimiento y Optimización de Consultas (Query Profiling)
Query Caching: Entendimiento de cómo el Databricks SQL Warehouse guarda en caché los resultados de consultas repetitivas para ahorrar costos de cómputo.
Query Profiling: Análisis de planes de ejecución para identificar cuellos de botella en Joins complejos y optimizar el rendimiento del Warehouse.
5. Gobernanza y Automatización
Alertas: Configuración de notificaciones basadas en condiciones de negocio (ej. "Envíame una alerta si un job falla o si los ingresos bajan un 20%").
Query Parameters: Creación de consultas dinámicas reutilizables que permiten a los analistas cambiar fechas o regiones sin modificar el código SQL.
Databricks Jobs: Programación de la ejecución de pipelines SQL para actualizar el Warehouse automáticamente.
6. Interfaz de Lenguaje Natural (Genie / AI Chatbot)
Exploración de cómo integrar asistentes de IA (como Databricks Genie) para permitir que los stakeholders empresariales hagan preguntas complejas sobre el Star Schema en inglés plano, democratizando el acceso a los datos.

 ![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

![image]()

