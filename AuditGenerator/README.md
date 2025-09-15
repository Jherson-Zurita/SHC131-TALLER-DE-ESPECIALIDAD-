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
   ```bash
   pip install -r requirements.txt
   ```
4. **Ejecutar la aplicación**
   ```bash
   python generadorfunctionencript.py
    ```
   
## 📖 Guía de Uso (paso a paso)

Sigue el orden indicado para asegurar que todo quede correctamente instalado y funcional.

---

### 🧩 Paso 1 — Conectar a la Base de Datos

- Rellena los datos de conexión: **Host**, **Puerto**, **Base de Datos**, **Usuario**, **Contraseña** y **Schema**.
- Haz clic en **"Conectar"**.
- Si la conexión es correcta, el estado cambiará a **"Conectado"**.

---

### 🔐 Paso 2 — Configurar la Encriptación

- Selecciona el **Tipo de Encriptación** (`AES`, `SHA-256`, etc.) desde el menú.
- Introduce una **Clave Secreta segura** (mínimo 8 caracteres) y confírmala.
- Verifica que el indicador de estado confirme que las claves **coinciden** y que son **válidas**.

---

### 🛠️ Paso 3 — Crear las Funciones Base

Con la conexión activa y la clave validada, haz clic en **"Crear Funciones Base"**. Esto:

- Instalará `pgcrypto` si falta:  
  ```sql
  CREATE EXTENSION IF NOT EXISTS pgcrypto;
  ```
  - Creará funciones auxiliares como:  
  - `encrypt_field(text, key)`  
  - `decrypt_field(bytea, key)`  
  - `hash_name(text)`  
  - `Lógica de ofuscación` 

- Generará utilidades para nombres ofuscados de tablas/columnas (ej. SHA-256 truncado).

---

### 📊 Paso 4 — Generar la Auditoría

- La lista de tablas del esquema se cargará automáticamente en la interfaz.
- Marca las casillas de las tablas que deseas auditar.
- Haz clic en **"Generar Auditoría"**.

Para cada tabla seleccionada, el script:

- Generará un nombre de tabla de auditoría ofuscado (ej. `aud_<hash>`).
- Creará la tabla de auditoría con columnas ofuscadas y campos clave:
  - `fecha`, `operacion`, `usuario`, `datos_old`, `datos_new`
- Creará la función trigger que:
  - Captura `OLD` y `NEW` según la operación
  - Serializa los datos en JSON
  - Encripta los campos con la clave y algoritmo seleccionado
  - Inserta el registro en la tabla de auditoría
- Creará el `CREATE TRIGGER` que asocia la función a la tabla original (`AFTER INSERT/UPDATE/DELETE`).

---

### 🔎 Paso 5 — Desencriptación y Consultas

- La interfaz puede generar un query `SELECT` que, usando la **Clave Secreta**, desencripta los valores para su lectura.
- Estas consultas deben ejecutarse con la clave correcta.  
  Si la clave no coincide, la desencriptación fallará y los datos seguirán siendo ininteligibles.

---

### 💾 Paso 6 — Guardar la Configuración (¡CRÍTICO!)

Haz clic en **"Guardar Configuración de Encriptación"**.  
Se generará un archivo `configuracion_encriptacion.txt` que contiene:

- Algoritmo seleccionado  
- Fecha de creación  
- Hashable mapping (`tabla original -> tabla de auditoría ofuscada`)

> ⚠️ **Nota:** NO almacenes la clave en texto plano si no lo deseas.  
> Si decides guardarla, hazlo en un contenedor cifrado o gestor de secretos.

Guarda ese archivo en un lugar **extremadamente seguro**:  
Es la **llave maestra** para recuperar los datos de auditoría.

---

## ⚠️ Advertencias Críticas

- Si pierdes la clave secreta, los datos encriptados serán **irrecuperables**.
- Trata `configuracion_encriptacion.txt` como evidencia sensible y protégela con controles de acceso físico y/o digital.
- Antes de ejecutar en producción, **prueba el flujo completo** en un entorno de staging.
- Otorga **sólo los permisos mínimos necesarios** al usuario que ejecutará la creación de funciones/triggers.
- Documenta y controla los accesos a la clave.  
  Considera usar un gestor de secretos como **Vault**, **AWS Secrets Manager**, etc.

---

## 📄 Licencia y Responsabilidad

Este script se entrega **tal cual**.  
Revisa y audita el código antes de usarlo en entornos críticos.

> 🛡️ El autor no asume responsabilidad por pérdidas derivadas de un mal uso, pérdida de claves o mala configuración.
