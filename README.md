# Databricks-SQL-Warehouse-AI-Chatbot

![image](https://github.com/user-attachments/assets/efa59c46-985c-46d8-bbfa-39449d18621d)  ![image](https://github.com/user-attachments/assets/d2bed958-3f73-4ae7-817d-afe5168d68ff)

________________________________________________________________________________________________________________________________________________________________________________________________________________
## 🎯 Descripción del Proyecto
________________________________________________________________________________________________________________________________________________________________________________________________________________

Este proyecto demuestra la construcción de un Data Warehouse moderno desde cero utilizando Databricks SQL Warehouse. En lugar de escribir pipelines ETL complejos en Python, se adopta el paradigma ELT (Extract, Load, Transform), donde los datos crudos se cargan primero y las transformaciones ocurren directamente en el motor SQL del Warehouse.

El objetivo del proyecto es la implementación de un Esquema Estrella (Star Schema) utilizando datos simulados de una aerolínea, aplicando técnicas de modelado dimensional, rastreo de cambios históricos (SCD), y configurando las capas de rendimiento y gobernanza de Databricks.

________________________________________________________________________________________________________________________________________________________________________________________________________________
## 🏗️ Arquitectura del Modelo de Datos (Star Schema)
________________________________________________________________________________________________________________________________________________________________________________________________________________

El diseño central de este proyecto separa los datos transaccionales (Hechos) de los datos descriptivos (Dimensiones), optimizando las consultas analíticas y evitando la redundancia.


![image](https://github.com/user-attachments/assets/f39a0677-b67d-49e9-b394-176c91a4303a)

 **Cómo leer este diagrama (La regla del 1 a Muchos)**

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

### 📊 Diccionario de Datos (Data Dictionary)

| Tabla | Tipo | Clave Primaria (PK) | Descripción del Negocio |
| :--- | :--- | :--- | :--- |
| **dim_airports** | Dimensión | `airport_id` | Catálogo maestro de aeropuertos, ubicación y país. |
| **dim_passengers** | Dimensión | `passenger_id` | Perfil demográfico de los pasajeros de la aerolínea. |
| **fact_bookings** | Hecho | `booking_id` | Tabla transaccional. Registra cada reserva con su monto, fecha y las claves foráneas. |


________________________________________________________________________________________________________________________________________________________________________________________________________________
## 🧠 Habilidades de Ingeniería de Datos Demostradas
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

________________________________________________________________________________________________________________________________________________________________________________________________________________
## 🚀 DESARROLLO
________________________________________________________________________________________________________________________________________________________________________________________________________________

1.	Creamos una carpeta archivo llamado SQL_Warehouse.
2.	Ahora hacemos click en create y seleccionamos query y se abrirá la siguiente ventana.

 ![image](https://github.com/user-attachments/assets/85eddf30-33e4-4d13-b7a0-bfaca9195bf1)

 3.	Ahora crearemos catálogo.

Código:

         CREATE CATALOG sql_warehouse

![image](https://github.com/user-attachments/assets/37a1c559-41f9-45d1-954d-da46ebfce4fd)

4.	Luego creamos un esquema.

Código:

         CREATE SCHEMA sql_warehouse.stg


![image](https://github.com/user-attachments/assets/706e1b2d-2351-42f5-b086-ad95ad17bc7a)

Ahora escogemos sql_warehouse.


![image](https://github.com/user-attachments/assets/25f56140-bd40-43c6-90d5-cefd87f47d70)

Luego desplegamos la pestaña default y escogemos storage (stg)

![image](https://github.com/user-attachments/assets/945d67de-46d2-4e86-8bb5-b400b1925712)

![image](https://github.com/user-attachments/assets/0f147838-ca96-4622-bf83-05c27197f0c6)

Luego creamos una tabla y la ejecutamos

Código:
 
        CREATE TABLE products
        (
            product_id INT,
            product_name STRING,
            price DECIMAL(5,2),
            catetgory STRING
        );
        INSERT INTO products VALUES
        (1, 'Product A', 10.99, 'Category X'),
        (2, 'Product B', 15.99, 'Category Y'),
        (3, 'Product C', 20.99, 'Category X'),
        (4, 'Product D', 25.99, 'Category Z')


![image](https://github.com/user-attachments/assets/51cd5906-418d-4184-bec4-1c0ad8233bc1)

Ahora verificaremos la creación de la tabla.

Código:

        SELECT * FROM products; 

![image](https://github.com/user-attachments/assets/a4c28e41-6610-4260-badb-6db644bf45b0)

![image](https://github.com/user-attachments/assets/2b15ec42-650d-4425-abd2-113d32e5c811)

•	PARAMETROS DE CONSULTA

Primero hay que entender una regla fundamental de las consultas parametrizadas en Databricks SQL:

Regla de oro
Los parámetros (:parametro) sirven para sustituir:
•	Valores (100, 'USA', fechas, etc.) 
•	Algunas veces identificadores usando IDENTIFIER() 
Pero no puedes construir libremente nombres de columnas o tablas dentro del SQL como si fuera Python.


Estructura de consulta parametrizada.

Código:

        SELECT * FROM products
        WHERE product_id = :product_id AND price > :price 


![image](https://github.com/user-attachments/assets/f1b4289a-2682-45b9-abe9-ed45e6d993f9)

•	Parametrización de objetos.

Código:
           
        SELECT :col1
        FROM sql_warehouse.stg.products
        WHERE Identifier(:col_product_id) = :product_id AND price > :price 


![image](https://github.com/user-attachments/assets/327d36aa-a512-4263-b4af-dbe5423886f6)

•	INSERTAR OBJETOS DE BASE DE DATOS

Código: 

        SELECT IDENTIFIER(:col1)
        FROM IDENTIFIER(:full_table_name)
        WHERE IDENTIFIER(:col_product_id) = :product_id AND price > :price 

![image](https://github.com/user-attachments/assets/14165283-6029-441d-a283-6bef6aee5235)

•	FRAGMENTO DE CONSULTA / QUERY SNIPPETS.

1.	Creamos una nueva query llamada query snippets y, luego hacemos click en los 3 puntos y al desplegarse la pestaña selecionamos view y luego se desplegara otra ventana y selecionamos query snippets, el cual, abrirá una nueva venta de query snippets.


![image](https://github.com/user-attachments/assets/3ec932dd-bdf4-4aa7-890b-d1a8d9c5c95f)

![image](https://github.com/user-attachments/assets/9bc50ffc-5c9f-4a83-a73f-db0379428a57)

Ahora creamos un query snippet con los siguientes datos.

![image](https://github.com/user-attachments/assets/6367aab8-e55a-4c6e-8444-8ba24a656405)

![image](https://github.com/user-attachments/assets/aa7b9a1c-2504-459c-994c-73aee5525186)

Luego volvemos a nuestro query original y hacemos la siguiente consulta.

![image](https://github.com/user-attachments/assets/31dade00-45ad-489f-9388-b4cbe65dfcaf)

La consulta de fragmento es dinamico, asi es que, podemos agregar más variables
adentro. Entonces volvemos a query snippets y cambiamos la variable.

![image](https://github.com/user-attachments/assets/ce1a5330-537d-42a4-bb80-ef61cb4352d2)

![image](https://github.com/user-attachments/assets/72c6db05-85d0-4453-aa73-f0ee69087650)

Ahora al escribir WHERE podremos ver este cambio.

![image](https://github.com/user-attachments/assets/bb36d794-6819-46a7-9325-f3b04dff67e0)

Nota: ${} es un marcador de posición ejemplo.

![image](https://github.com/user-attachments/assets/25b1fa39-8252-4e15-bfb7-9b5706bb36ac)

Volvemos a nuestra query principal y al escribir WHERE se desplegara la opción {} producto_where, lo seleccionamos y veremos los resultados.

![image](https://github.com/user-attachments/assets/74db15bc-bb0e-4d46-97ec-b5083295a091)

![image](https://github.com/user-attachments/assets/a44d8474-7147-4b88-a981-9a56b6b0cbdd)

Ahora creamos un nuevo query snippets

![image](https://github.com/user-attachments/assets/385196cd-1c1b-4250-8949-5d5249051092)

Luego regresamos a la pestaña de query snippet y con tan solo escribir case se desplegará una ventana donde aparece case_snippet y con tan solo darle click completara todo el código.

![image](https://github.com/user-attachments/assets/637c2929-c4a8-43a5-b0c8-04531b49f336)

![image](https://github.com/user-attachments/assets/f77b7676-fab8-4f55-8c1f-4e4f84e2321f)

**BACKBONE**

Ahora regresamos a catálogos y desplegamos sql_warehouse y seleccionamos stg, dentro del panel hacemos click en crear y, en la ventana emergente hacemos click en Table.

![image](https://github.com/user-attachments/assets/faf5e872-27dd-4467-9e4a-880090d06c12)

![image](https://github.com/user-attachments/assets/4ded1cb8-35d9-4ce9-a3e7-ff18aa3145a3)

Y ahí subimos los siguientes 3 archivos.

•	Table fact_bookings.
•	Tabla dim_airports.
•	Tabla dim_passengers.


![image](https://github.com/user-attachments/assets/981763ba-f0aa-4a87-acf3-431000d2b574)


Ahora creamos un esquema nuevo en sql_warehouse llamado enr.

![image](https://github.com/user-attachments/assets/d35e3ebb-bb1b-4d53-b53b-d43b5e7149e3)

Luego en workspace / sql_warehouse creamos una nueva consulta (query) llamada streaming tables.

![image](https://github.com/user-attachments/assets/2173b2ce-df58-45db-8b20-1cddc14838c9)


Y creamos una tabla con el siguiente código.

Código:

        CREATE OR REFRESH STREAMING TABLE sql_warehouse.stg.strean_passengers
        AS SELECT * FROM STREAM(sql_warehouse.stg.dim_passengers);


![image](https://github.com/user-attachments/assets/7234b037-e000-4c56-ab1a-19f5ba9c2e18)

Ahora hacemos la consulta.
Código:

        SELECT * FROM sql_warehouse.stg.strean_passengers

        
![image](https://github.com/user-attachments/assets/3acf556f-ac18-4637-965b-68410ef210a4)


Ahora hacemos los mismos pasos para crear la tabla de aeropuertos

Código:

        CREATE OR REFRESH STREAMING TABLE sql_warehouse.stg.strean_airports
        AS SELECT * FROM STREAM(sql_warehouse.stg.dim_airports);


![image](https://github.com/user-attachments/assets/91fd40ac-0bb3-4641-bb91-901f80d6ee88)

Y también hacemos lo mismo para reservas.

Código:

        CREATE OR REFRESH STREAMING TABLE sql_warehouse.stg.strean_booking
        AS SELECT * FROM STREAM(sql_warehouse.stg.dim_booking);


![image](https://github.com/user-attachments/assets/1850126a-3425-428d-ac7c-546100438e08)


 ## 🔥 SCDs 🔥

Creamos un nuevo query  llamado SCDs.

Vamos a la columna principal de Databricks y seleccionamos la pestaña Job & Pipeline y creamos un nuevo pipeline (SCDs) haciendo click en el botón crear y de la ventana emergente seleccionamos ETL Pipeline.

![image](https://github.com/user-attachments/assets/e544ce24-afd0-4b9e-bf2b-f2fae5959840)

Ahora configuraremos que este en sql_warehouse y en el esquema enr.

![image](https://github.com/user-attachments/assets/a2977456-11ce-4abe-ac35-17c2680dc3f3)

![image](https://github.com/user-attachments/assets/7a08f722-8f6a-4b29-be32-7561669f196a)

Y guardamos (save).

Y se abrirá una nueva ventana donde aparecerá SCDs y ahí hacemos click en Edit pipeline.

![image](https://github.com/user-attachments/assets/52780d27-238b-4dd8-bbd1-85f0f59badca)

Y ahí pasamos el siguiente código:

Código:

        CREATE OR REFRESH STREAMING TABLE DimAirports;

        CREATE FLOW flow1
        AS AUTO CDC INTO 
            DimAirports
        FROM stream(stg.strean_airports)
        KEYS (airport_id)
        SEQUENCE BY airport_id
        STORED AS SCD TYPE 2;

        CREATE OR REFRESH STREAMING TABLE DimPassengers;

        CREATE FLOW flow2
        AS AUTO CDC INTO
            DimPassengers
        FROM stream(stg.strean_passengers)
            KEYS (passenger_id)
            SEQUENCE BY passenger_id
            STORED AS SCD TYPE 2;

Y luego corremos el pipeline (Run pipeline)

![image](https://github.com/user-attachments/assets/bae79445-9406-4d24-b858-3e70fa8ff930)


Ahora haremos una consulta sobre la tabla airports.

Código:

        SELECT * FROM sql_warehouse.enr.dimairports


![image](https://github.com/user-attachments/assets/a9a7082b-3458-4585-9c3b-2fed9e058aa6)

