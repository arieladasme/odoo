[![Build Status](https://runbot.odoo.com/runbot/badge/flat/1/master.svg)](https://runbot.odoo.com/runbot)
[![Tech Doc](https://img.shields.io/badge/master-docs-875A7B.svg?style=flat&colorA=8F8F8F)](https://www.odoo.com/documentation/master)
[![Help](https://img.shields.io/badge/master-help-875A7B.svg?style=flat&colorA=8F8F8F)](https://www.odoo.com/forum/help-1)
[![Nightly Builds](https://img.shields.io/badge/master-nightly-875A7B.svg?style=flat&colorA=8F8F8F)](https://nightly.odoo.com/)

## Getting started with Odoo

For a standard installation please follow the <a href="https://www.odoo.com/documentation/master/administration/install/install.html">Setup instructions</a>
from the documentation.

To learn the software, we recommend the <a href="https://www.odoo.com/slides">Odoo eLearning</a>, or <a href="https://www.odoo.com/page/scale-up-business-game">Scale-up</a>, the <a href="https://www.odoo.com/page/scale-up-business-game">business game</a>. Developers can start with <a href="https://www.odoo.com/documentation/master/developer/howtos.html">the developer tutorials</a>

# Guía de Instalación de Odoo 18 en Entorno Local

Esta guía te ayudará a configurar Odoo 18 en tu entorno de desarrollo local, partiendo desde la creación del entorno virtual de Python.

## Requisitos Previos

- Python 3.10+
- PostgreSQL 12+
- Git
- Node.js y npm

## Configuración del Entorno Virtual

### Paso 1: Crear y activar el entorno virtual

```bash
# Navega a tu directorio de Odoo
cd ruta/a/tu/repositorio/odoo18

# Crea un entorno virtual
python -m venv venv

# Activa el entorno virtual
# En Windows:
venv\Scripts\activate
# En macOS/Linux:
source venv/bin/activate
```

### Paso 2: Instalar dependencias de Python

```bash
# Actualiza pip
pip install --upgrade pip

# Instala las dependencias de Odoo
pip install -r requirements.txt
```

> **Nota**: Este proceso puede tardar varios minutos dependiendo de tu conexión a internet y la potencia de tu equipo.

## Configuración de PostgreSQL

### Paso 3: Configurar usuario de base de datos

```bash
# Conecta a PostgreSQL como superusuario (postgres)
# En Windows (usando psql):
psql -U postgres

# En Linux:
sudo -u postgres psql

# Ejecuta estos comandos SQL:
CREATE USER odoo WITH PASSWORD 'tu_contraseña';
ALTER USER odoo WITH CREATEDB;
\q
```

## Configuración de Odoo

### Paso 4: Crear archivo de configuración

Crea un archivo `odoo.conf` en la raíz del proyecto con el siguiente contenido:

```
[options]
addons_path = ./addons
data_dir = ./data
admin_passwd = admin
db_host = localhost
db_port = 5432
db_user = odoo
db_password = tu_contraseña
http_port = 8069
```

> **Importante**: Asegúrate de cambiar `tu_contraseña` por la contraseña que estableciste para el usuario de PostgreSQL.

### Paso 5: Crear directorio para datos

```bash
# Crea el directorio para almacenar los datos de Odoo
mkdir data
```

## Configuración de VS Code

### Paso 6: Configurar VS Code para depuración

Crea una carpeta `.vscode` en la raíz del proyecto si no existe, y dentro crea un archivo `launch.json`:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Odoo",
      "type": "python",
      "request": "launch",
      "program": "${workspaceFolder}/odoo-bin",
      "args": ["-c", "${workspaceFolder}/odoo.conf"],
      "console": "integratedTerminal"
    }
  ]
}
```

## Iniciar Odoo

### Paso 7: Ejecutar Odoo

Tienes dos opciones para iniciar Odoo:

#### Opción 1: Usando el depurador de VS Code

Presiona F5 o haz clic en el botón de depuración en VS Code.

#### Opción 2: Desde la terminal

```bash
# Asegúrate de que el entorno virtual esté activado
python odoo-bin -c odoo.conf
```

### Paso 8: Acceder a Odoo

Abre tu navegador y ve a [http://localhost:8069](http://localhost:8069)

En la primera ejecución, deberás crear una base de datos con los siguientes datos:

- Nombre de la base de datos: elige un nombre para tu instancia
- Email: tu correo electrónico
- Contraseña: establece una contraseña segura para el usuario admin
- Idioma: selecciona español si lo prefieres
- País: selecciona tu país

## Desarrollo de Módulos Personalizados

### Paso 9: Configurar carpeta para módulos personalizados

```bash
# Crea una carpeta para tus módulos personalizados
mkdir custom_addons
```

Actualiza el archivo `odoo.conf` para incluir la nueva ruta de addons:

```
[options]
addons_path = ./addons,./custom_addons
# El resto de la configuración se mantiene igual
```

### Paso 10: Crear un nuevo módulo

```bash
# Usa el scaffolding de Odoo para crear un nuevo módulo
python odoo-bin scaffold mi_modulo ./custom_addons
```

## Consejos para el Desarrollo

### Actualización de módulos

```bash
# Actualiza un módulo específico
python odoo-bin -c odoo.conf -u mi_modulo

# Actualiza varios módulos
python odoo-bin -c odoo.conf -u mi_modulo,otro_modulo

# Actualiza todos los módulos instalados
python odoo-bin -c odoo.conf -u all
```

### Modo desarrollador

Para activar características de desarrollo:

```bash
# Modo de desarrollo con recarga automática
python odoo-bin -c odoo.conf --dev=all
```

### Extensiones recomendadas para VS Code

- **Python**: Soporte para Python
- **XML Tools**: Formateo y validación de XML
- **PostgreSQL**: Gestión de bases de datos
- **Odoo Snippets**: Snippets para desarrollo en Odoo
- **Git Graph**: Visualización de historial de Git

## Solución de Problemas Comunes

### Error de conexión a PostgreSQL

Verifica que:

- PostgreSQL esté en ejecución
- Las credenciales en odoo.conf sean correctas
- El usuario tenga permisos para crear bases de datos

### Problemas con dependencias de Python

Si hay errores al instalar dependencias:

```bash
# Instala herramientas de desarrollo (Linux)
sudo apt-get install build-essential python3-dev libxml2-dev libxslt1-dev libldap2-dev libsasl2-dev libssl-dev

# En Windows, puede ser necesario instalar Visual C++ Build Tools
```

### Errores al cargar módulos

Verifica que:

- La ruta en addons_path sea correcta
- Los módulos tengan la estructura correcta
- No haya errores de sintaxis en los archivos Python o XML

---

## Notas de Seguridad

> **Advertencia**: La configuración proporcionada es para entornos de desarrollo local. Para entornos de producción, asegúrate de:
>
> - Cambiar la contraseña de administrador
> - Configurar HTTPS
> - Limitar el acceso a la base de datos
> - Seguir las recomendaciones de seguridad de Odoo
