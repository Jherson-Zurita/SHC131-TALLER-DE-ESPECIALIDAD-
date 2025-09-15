# 🛡️ Generador de Auditoría PostgreSQL con Encriptación y Ofuscación

Esta es una herramienta de escritorio con interfaz gráfica (GUI) construida en **Python** y **Tkinter**, diseñada para implementar un sistema de auditoría altamente seguro en bases de datos **PostgreSQL**.

El script crea triggers que registran cada cambio (INSERT, UPDATE, DELETE) en las tablas seleccionadas, aplicando además **encriptación de datos** y **ofuscación del esquema**, de modo que los registros de auditoría sólo sean legibles por quien posea la clave secreta.

---

## 🔐 Características de Seguridad

- **Encriptación de Datos en Reposo**: Cada campo capturado por los triggers (valores nuevos, valores antiguos, usuario, fecha, tipo de operación) se encripta antes de almacenarse en las tablas de auditoría.
- **Ofuscación del Esquema**: Los nombres de tablas y columnas de auditoría no son predecibles; se generan mediante hashes (por ejemplo, `aud_a3b8c1...`) para evitar inferencias del propósito por observadores del esquema.
- **Control Total sobre la Clave Secreta**: La clave de encriptación la defines tú. No se almacena en texto plano en la base de datos; se pasa como parámetro a las funciones de desencriptación.
- **Soporte para Múltiples Algoritmos**:
  - **AES**: encriptación simétrica reversible (cuando necesites recuperar datos originales).
  - **MD5, SHA-256, SHA-512**: algoritmos de hashing unidireccionales para integridad o pseudo-identificadores.

---

## ✨ Funcionalidades Principales

- **Interfaz Gráfica Intuitiva** con Tkinter para configurar todo sin escribir SQL manualmente.
- **Generación Automática de Funciones y Triggers**: crea funciones como `encrypt_field`, `decrypt_field`, el generador de triggers `aud_trigger`, y otras utilidades necesarias.
- **Creación de Tablas de Auditoría Ofuscadas** y triggers asociados para cada tabla seleccionada.
- **Herramientas de Desencriptación**: el script puede generar las consultas `SELECT` necesarias para desencriptar los registros usando la clave secreta (ejecutables desde la interfaz).
- **Exportación de Configuración Segura**: guarda un archivo `configuracion_encriptacion.txt` con la información crítica (clave, algoritmo, tablas afectadas). Este archivo es tu llave maestra: trátalo con extrema precaución.

---

## 📋 Requisitos Previos

- **Python 3.x** (recomendado 3.8+).
- Acceso a un servidor **PostgreSQL**.
- Un usuario de PostgreSQL con permisos suficientes para:
  - `CREATE EXTENSION pgcrypto`
  - `CREATE SCHEMA`, `CREATE TABLE`, `CREATE FUNCTION`
  - `CREATE TRIGGER` en las tablas a auditar

---

## 🚀 Instalación

1. **Guardar el script**  
   Descarga o guarda el archivo `generadorfunctionencript.py` en una carpeta de tu equipo.

2. **Crear `requirements.txt`**  
   En la misma carpeta crea un archivo `requirements.txt` con este contenido:

   ```text
   psycopg2-binary==2.9.9
   ```
3. **Instalar las dependencias**
   Abre una terminal en esa carpeta y ejecuta:
   
