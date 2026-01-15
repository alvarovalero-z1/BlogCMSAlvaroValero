# CMS Server API

API REST completa para la gestión de blogs, posts y etiquetas con autenticación por tokens, desarrollada con Django REST Framework y documentación automática con Swagger.

## 🚀 Características

- **API REST completa** con Django REST Framework
- **Autenticación por tokens** segura
- **Documentación automática** con Swagger UI y ReDoc
- **Gestión de blogs, posts y etiquetas**
- **Permisos granulares** (público para lectura, privado para escritura)
- **Filtros y búsquedas** avanzadas
- **Tests completos** con pytest
- **Import/Export** de datos

## 📋 Requisitos

- Python 3.12+ (recomendado)
- Django 5.2+
- SQLite3 (base de datos incluida)
- pip (gestor de paquetes Python)

## 🛠️ Instalación

### 1. Clonar el repositorio
```bash
git clone https://github.com/alvarovalero-z1/BlogCMSAlvaroValero.git
cd ProyectoAlvaroValero
```

### 2. Crear entorno virtual
```bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
```

### 3. Instalar dependencias
```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt  # Opcional: para desarrollo y testing
```

### 4. Configurar variables de entorno
Copia el archivo de ejemplo y configura tus variables:
```bash
cp .env.example .env
```

Genera una SECRET_KEY segura:
```bash
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

Edita el archivo `.env` y actualiza `SECRET_KEY` con el valor generado.

### 5. Configurar base de datos SQLite3
```bash
python manage.py migrate
```

### 6. Crear superusuario (opcional)
```bash
python manage.py createsuperuser
```

### 7. Ejecutar servidor
```bash
python manage.py runserver
```

El servidor estará disponible en http://localhost:8000/

## 📚 Documentación de la API

### URLs de documentación
- **Swagger UI**: http://localhost:8000/api/docs/
- **ReDoc**: http://localhost:8000/api/redoc/
- **Esquema OpenAPI**: http://localhost:8000/api/schema/

### Endpoints principales
- **API Base**: http://localhost:8000/cms/api/
- **Admin**: http://localhost:8000/admin/

## 🔐 Autenticación

La API utiliza autenticación por tokens. Para usar endpoints protegidos:

1. **Registrar usuario**:
```bash
POST /cms/api/auth/register/
{
    "username": "usuario",
    "password": "contraseña123",
    "password_confirm": "contraseña123"
}
```

2. **Iniciar sesión**:
```bash
POST /cms/api/auth/login/
{
    "username": "usuario",
    "password": "contraseña123"
}
```

3. **Usar token**:
```bash
Authorization: Token tu_token_aqui
```

## 📖 Uso de la API

### Blogs
```bash
# Listar blogs (público)
GET /cms/api/blogs/

# Crear blog (requiere autenticación)
POST /cms/api/blogs/
{
    "title": "Mi Blog",
    "description": "Descripción del blog"
}

# Obtener blog específico
GET /cms/api/blogs/{id}/

# Actualizar blog (solo propietario)
PUT /cms/api/blogs/{id}/

# Eliminar blog (solo propietario)
DELETE /cms/api/blogs/{id}/
```

### Posts
```bash
# Listar posts (público)
GET /cms/api/posts/

# Crear post (requiere autenticación)
POST /cms/api/posts/
{
    "title": "Mi Post",
    "content": "Contenido del post",
    "blog": 1,
    "tags": [1, 2]
}

# Obtener post específico
GET /cms/api/posts/{id}/

# Actualizar post (solo propietario)
PUT /cms/api/posts/{id}/

# Eliminar post (solo propietario)
DELETE /cms/api/posts/{id}/
```

### Tags
```bash
# Listar tags (público)
GET /cms/api/tags/

# Crear tag (requiere autenticación)
POST /cms/api/tags/
{
    "name": "tecnología"
}

# Obtener tag específico
GET /cms/api/tags/{id}/

# Actualizar tag
PUT /cms/api/tags/{id}/

# Eliminar tag
DELETE /cms/api/tags/{id}/
```

## 🧪 Testing

### Prerequisitos
Asegúrate de tener instalado `requirements-dev.txt`:
```bash
pip install -r requirements-dev.txt
```

### Ejecutar tests
```bash
# Todos los tests
pytest

# Tests con cobertura
pytest --cov=CMSServer

# Tests específicos
pytest CMSServer/tests/tests_models.py
```

### Tests disponibles
- **tests_models.py**: Tests de modelos
- **tests_permissions.py**: Tests de permisos
- **tests_mixins.py**: Tests de mixins
- **tests_urls.py**: Tests de URLs
- **tests_utils.py**: Tests de utilidades

## 🔧 Configuración

### Variables de entorno

El proyecto usa variables de entorno para configuración sensible. Sigue estos pasos:

1. **Copia el archivo de ejemplo**:
```bash
cp .env.example .env
```

2. **Genera una SECRET_KEY segura**:
```bash
python manage.py shell -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"
```

3. **Edita el archivo `.env`** con tus valores:

```env
SECRET_KEY=tu_secret_key_generada_aqui
DEBUG=1
DATABASE_URL=sqlite:///db.sqlite3
ALLOWED_HOSTS=localhost,127.0.0.1
DJANGO_SETTINGS_MODULE=ProyectoAlvaroValero.settings
```

**Nota**: El archivo `.env` no se commitea al repositorio por seguridad. Usa `.env.example` como referencia.

**Importante**: Si ejecutas `runserver` directamente (sin docker-compose), necesitarás instalar `python-dotenv`:
```bash
pip install python-dotenv
```

Y añadir estas líneas al inicio de `settings.py` (después de `import os`):
```python
from dotenv import load_dotenv
load_dotenv()  # Carga las variables del archivo .env
```
## 📦 Estructura del proyecto

```
ProyectoAlvaroValero/
├── CMSServer/                 # App principal
│   ├── models.py             # Modelos de datos
│   ├── views.py              # ViewSets con documentación
│   ├── serializers.py        # Serializers con ejemplos
│   ├── urls.py               # URLs de la API
│   ├── permissions.py        # Permisos personalizados
│   ├── mixins.py             # Mixins reutilizables
│   ├── utils.py              # Utilidades
│   └── tests/                # Tests
├── ProyectoAlvaroValero/     # Configuración del proyecto
│   ├── settings.py           # Configuración
│   └── urls.py                # URLs principales
├── .env.example              # Plantilla de variables de entorno
├── .gitignore                # Archivos ignorados por git
├── requirements.txt          # Dependencias de producción
├── requirements-dev.txt      # Dependencias de desarrollo
├── pytest.ini                # Configuración de tests
└── README.md                 # Este archivo

```

## 🚀 Despliegue

### Desarrollo
```bash
python manage.py runserver
```

### Producción
```bash
# Instalar dependencias
pip install -r requirements.txt

# Aplicar migraciones
python manage.py migrate

# Recopilar archivos estáticos
python manage.py collectstatic
```

## 🐛 Solución de Problemas

### Error: "Couldn't import Django"
- Asegúrate de tener el entorno virtual activado
- Verifica que instalaste las dependencias: `pip install -r requirements.txt`

### Error: "SECRET_KEY not found"
- Verifica que creaste el archivo `.env` desde `.env.example`
- Asegúrate de que el archivo `.env` está en la raíz del proyecto
- Si usas `runserver` directamente, instala `python-dotenv` y añade `load_dotenv()` en `settings.py`

### Error: "ModuleNotFoundError"
- Instala las dependencias: `pip install -r requirements.txt`
- Si usas herramientas de desarrollo: `pip install -r requirements-dev.txt`

### Variables de entorno no se cargan
- Si usas `runserver` directamente (sin docker-compose), instala `python-dotenv`:
  ```bash
  pip install python-dotenv
  ```
- Añade al inicio de `settings.py` (después de `import os`):
  ```python
  from dotenv import load_dotenv
  load_dotenv()
  ```
- O exporta manualmente las variables antes de ejecutar:
  ```bash
  export SECRET_KEY=$(grep SECRET_KEY .env | cut -d '=' -f2)
  python manage.py runserver
  ```

### Error al ejecutar tests
- Asegúrate de tener instalado `requirements-dev.txt`
- Verifica que la base de datos está migrada: `python manage.py migrate`

```

## 📊 Características técnicas

### Tecnologías utilizadas
- **Django 5.2**: Framework web
- **Django REST Framework 3.16**: API REST
- **drf-spectacular 0.29**: Documentación automática
- **pytest**: Testing
- **TinyMCE**: Editor de texto enriquecido
- **django-import-export**: Importación/exportación de datos

### Permisos
- **Lectura**: Pública para todos los recursos
- **Escritura**: Solo usuarios autenticados
- **Propiedad**: Solo el propietario puede editar/eliminar

### Filtros disponibles
- **Posts por blog**: `?blog=1`
- **Posts por tag**: `?tags=1,2`
- **Búsqueda**: `?search=texto`


## 👨‍💻 Autor

**Álvaro Valero**
- GitHub: [@alvarovalero-z1](https://github.com/alvarovalero-z1/BlogCMSAlvaroValero)


## 🆘 Soporte

Si tienes problemas o preguntas:

Revisa la [documentación de la API](http://localhost:8000/api/docs/)



---
