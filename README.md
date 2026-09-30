# 🗄️ Arquitectura de Base de Datos - Sistema de Tienda

## 📝 Sobre el Proyecto
Este repositorio contiene el diseño relacional y la lógica de backend para el sistema de inventario de una tienda de zapatillas. El objetivo es demostrar la correcta normalización de datos y la automatización de procesos mediante la programación en base de datos.

## 🛠️ Tecnologías Aplicadas
* **Oracle SQL:** Definición de la estructura de datos (DDL), creación de tablas, llaves primarias y foráneas para garantizar la integridad referencial.
* **PL/SQL:** Desarrollo de procedimientos almacenados (Stored Procedures) para encapsular la lógica de negocio. Se incluye un procedimiento robusto para procesar ventas que incluye:
  * Manejo de excepciones (`EXCEPTION`).
  * Validaciones de stock en tiempo real.
  * Prevención de errores de datos huérfanos.

## 📂 Archivos del Repositorio
* `tienda_schema.sql`: Script principal ejecutable con la estructura de tablas y el código del procedimiento almacenado.