# Proyecto Django con MySQL y Operaciones CRUD

## Objetivo

El objetivo de este proyecto es configurar un entorno de desarrollo Django para conectarse a una base de datos MySQL y realizar operaciones CRUD (Crear, Leer, Actualizar y Borrar) utilizando el ORM de Django.

## Instrucciones

A continuación, se describen los pasos seguidos para el desarrollo de la actividad solicitada.

### 1. Instalación de dependencias

Si bien el ejercicio solicita utilizar MySQL, se utilizó PostreSQL. Para conectar Django con PostgreSQL, se instalautilizó el conector `psycopg2`.

```bash
pip install psycopg2
```

### 2. Creación de un proyecto Django

Se creó un nuevo proyecto de Django llamado `mi_proyecto`. a través de:

```bash
django-admin startproject mi_proyecto
```

### 3. Configuración de la conexión a MySQL

Utilizando PGAdmin , se crea una base de datos llamada `crud_db` y se edita el archivo `mi_proyecto/settings.py` para configurar la conexión a la base de datos MySQL. 

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'crud_db',          # Nombre de la base de datos
        'USER': 'postgres',         # Usuario de MySQL
        'PASSWORD': 'contraseña',   # Contraseña
        'HOST': 'localhost',        # Servidor de MySQL
        'PORT': '5432',             # Puerto por defecto
    }
}
```

### 4. Creación de una aplicación Django

Se crea una nueva aplicación llamada `productos` y se agrega al `INSTALLED_APPS` en `mi_proyecto/settings.py`.

```bash
python manage.py startapp productos
```

```python
# mi_proyecto/settings.py
INSTALLED_APPS = [
    # ...
    'productos',
]
```

### 5. Creación del modelo en Django (ORM)

En el archivo `productos/models.py`, se define el modelo `Producto`.

```python
from django.db import models

class Producto(models.Model):
    nombre = models.CharField(max_length=100, unique=True)
    descripcion = models.TextField()
    precio = models.DecimalField(max_digits=10, decimal_places=2)
    stock = models.IntegerField()
    fecha_creacion = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre
```

### 6. Aplicación de migraciones

Se generan y aplican las migraciones para crear la tabla `Producto` en la base de datos.

```bash
python manage.py makemigrations
python manage.py migrate
```

### 7. Operaciones CRUD con el ORM de Django

Se utiliza la consola interactiva de Django (`shell`) para realizar las operaciones CRUD.

```bash
python manage.py shell
```

#### a) Crear un producto

Se importa el modelo `Producto`, se crea una nueva instancia y se guarda en la base de datos.

```python
from productos.models import Producto
p_crear = Producto(nombre='Monitor 4K', descripcion='Monitor de alta resolución para diseño gráfico', precio=750.00, stock=15)
p_crear.save()
```

#### b) Consultar el producto

Se consulta el producto recién creado.

```python
from productos.models import Producto
p_consultar = Producto.objects.get(nombre='Monitor 4K')
print(f'Nombre: {p_consultar.nombre}, Descripción: {p_consultar.descripcion}, Precio: {p_consultar.precio}, Stock: {p_consultar.stock}')
```

#### c) Modificar el producto

Se modifica el precio del producto y se guardan los cambios.

```python
from productos.models import Producto
p_modificar = Producto.objects.get(nombre='Teclado Mecánico')
p_modificar.precio = 135.50
p_modificar.save()
```

#### d) Eliminar el producto

Se elimina el producto de la base de datos.

```python
from productos.models import Producto
p_eliminar = Producto.objects.get(nombre='Teclado Mecánico')
p_eliminar.delete()
```
