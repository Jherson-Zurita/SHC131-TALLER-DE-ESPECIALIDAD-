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

## 📋 Requisitos Previos
- Python 3.8 o superior
- PostgreSQL 12 o superior
- Node.js y npm instalados (para la interfaz gráfica)
- Librerías necesarias incluidas en `requirements.txt`

---

## ⚙️ Instalación de Dependencias
Abre una terminal en el directorio del proyecto y ejecuta:
```bash
pip install -r requirements.txt
npm install
```

---

## 🚀 Ejecución
Para iniciar la interfaz gráfica:
```bash
npm start
```

---

# 📖 Guía de Uso

El proceso debe seguirse en orden para garantizar una configuración correcta.

## Paso 1: Conectar a la Base de Datos
- Rellena los datos de conexión (**Host, Puerto, Base de Datos, Usuario, Contraseña y Schema**).  
- Haz clic en **"Conectar"**.  
- Si la conexión es exitosa, el estado cambiará a **"Conectado"**.

## Paso 2: Configurar la Encriptación
- Selecciona el **Tipo de Encriptación** que deseas.  
- Introduce una **Clave Secreta** segura (mínimo 8 caracteres) y confírmala.  
- El indicador de estado confirmará que las claves coinciden y son válidas.

## Paso 3: Crear las Funciones Base
- Con la conexión activa y la clave válida, haz clic en **"Crear Funciones Base"**.  
- Esto instalará en tu base de datos toda la lógica de encriptación, ofuscación y auditoría.

## Paso 4: Generar la Auditoría
- La lista de tablas del esquema se cargará automáticamente.  
- Selecciona las tablas que deseas auditar marcando las casillas correspondientes.  
- Haz clic en **"Generar Auditoría"**.  
- El script creará las tablas de auditoría ofuscadas y los triggers para cada tabla seleccionada.

## Paso 5: Guardar la Configuración (¡CRÍTICO!)
- Haz clic en **"Guardar Configuración de Encriptación"**.  
- Esto creará el archivo **configuracion_encriptacion.txt** en la misma carpeta.  
- Guarda este archivo en un lugar extremadamente seguro.

---

## ⚠️ ¡MUY IMPORTANTE!
- La seguridad de tu auditoría depende de la **Clave Secreta**.  
- Si pierdes la clave secreta, **tus datos de auditoría encriptados serán permanentemente irrecuperables**.  
- Trata el archivo **configuracion_encriptacion.txt** como un documento confidencial.
