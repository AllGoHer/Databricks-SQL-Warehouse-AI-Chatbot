# Databricks-SQL-Warehouse-AI-Chatbot

![image](https://github.com/user-attachments/assets/efa59c46-985c-46d8-bbfa-39449d18621d)  ![image](https://github.com/user-attachments/assets/d2bed958-3f73-4ae7-817d-afe5168d68ff)


## 🎯 Descripción del Proyecto

Este proyecto demuestra la construcción de un Data Warehouse moderno desde cero utilizando Databricks SQL Warehouse. En lugar de escribir pipelines ETL complejos en Python, se adopta el paradigma ELT (Extract, Load, Transform), donde los datos crudos se cargan primero y las transformaciones ocurren directamente en el motor SQL del Warehouse.

El objetivo del proyecto es la implementación de un Esquema Estrella (Star Schema) utilizando datos simulados de una aerolínea, aplicando técnicas de modelado dimensional, rastreo de cambios históricos (SCD), y configurando las capas de rendimiento y gobernanza de Databricks.

## 🏗️ Arquitectura del Modelo de Datos (Star Schema)

El diseño central de este proyecto separa los datos transaccionales (Hechos) de los datos descriptivos (Dimensiones), optimizando las consultas analíticas y evitando la redundancia.


![image](https://github.com/user-attachments/assets/f39a0677-b67d-49e9-b394-176c91a4303a)


  ┌─────────────────────┐                  ┌─────────────────────────┐                  ┌─────────────────┐
  │   DIM PASSENGERS    │                  │      FACT_BOOKINGS      │                  │   DIM AIRPORTS  │
  ├─────────────────────┤                  ├─────────────────────────┤                  ├─────────────────┤
  │ PK: passenger_id    │  ────────▶      │ FK: passenger_id        │                  │ PK: airport_id  │
  │ name                │                  │ FK: airport_id          │     ◀────────   │ city            │
  │ gender              │                  │ FK: flight_id           │                  │ country         │
  │ nationality         │                  │ PK: booking_id          │                  └─────────────────┘
  └─────────────────────┘                  │ amount                  │
                                           │ booking_date            │
                                           └─────────────────────────┘

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

