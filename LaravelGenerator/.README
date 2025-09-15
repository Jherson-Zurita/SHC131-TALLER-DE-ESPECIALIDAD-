# 🐳 Generador Laravel desde Docker

Este es un script de **Python** con una interfaz gráfica (GUI) creada con **Tkinter** que automatiza la creación de componentes **Model-View-Controller (MVC)** para proyectos **Laravel**.  
La herramienta se conecta a tu entorno de **Docker** para detectar automáticamente contenedores de bases de datos (MySQL, PostgreSQL, MariaDB) y proyectos Laravel en ejecución, generando el código boilerplate necesario a partir de la estructura de tus tablas.

---

## ✨ Características Principales

### 🖥️ Interfaz Gráfica Intuitiva  
Facilita la selección de contenedores, bases de datos y tablas sin necesidad de usar la línea de comandos.

### 🔍 Detección Automática en Docker  
- Encuentra contenedores que ejecutan servicios de bases de datos (**MySQL, PostgreSQL, MariaDB**).  
- Localiza tus proyectos Laravel buscando el archivo `artisan` dentro de cualquier contenedor en ejecución.

### ⚡ Generación de Código Completa  
- **Modelos:** Crea modelos de Eloquent con las propiedades `$table`, `$primaryKey` y `$fillable` preconfiguradas.  
- **Controladores:** Genera **Resource Controllers** con todos los métodos CRUD (`index`, `create`, `store`, `show`, `edit`, `update`, `destroy`).  
- **Rutas:** Añade automáticamente las rutas de tipo `Route::resource` al archivo `routes/web.php`.  
- **Vistas Blade:** Genera vistas básicas pero funcionales para las operaciones CRUD (listado, crear, editar, ver).

### 🧠 Escritura Inteligente de Archivos  
- Si tu proyecto Laravel está en un volumen montado (**bind mount**), los archivos se escriben directamente en tu máquina host.  
- Si no, los archivos se crean dentro del contenedor usando `docker exec`.  

### 🛡️ Prevención de Errores  
Muestra un diálogo de confirmación detallado si los archivos a generar ya existen, evitando sobrescrituras accidentales.

---

## 📋 Requisitos Previos

Asegúrate de tener lo siguiente instalado y en ejecución:

- **Python 3.11.5** o superior.  
- **Docker Desktop** o **Docker Engine** (el servicio de Docker debe estar activo).  
- Tus proyectos (base de datos y aplicación Laravel) deben estar corriendo en contenedores de Docker.

---

## 🚀 Instalación y Uso

1. **Clona o descarga el script**  
   Guarda el archivo `generator3.py` en tu máquina local.

2. **Crea un archivo de dependencias**  
   En el mismo directorio donde guardaste el script, crea un archivo llamado `requirements.txt` con el siguiente contenido:  

   ```plaintext
   docker==7.1.0
   mysql-connector-python==9.4.0
   psycopg2-binary==2.9.9
