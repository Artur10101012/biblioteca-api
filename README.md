# 📚 Proyecto Sistema de Registro y Valoración de Libros

✅ **Versiones de las herramientas**

```python
Python: 3.10+
Django: 4.2+
PostgreSQL: 14+
Pandas: 1.5+
Matplotlib: 3.6+
Seaborn: 0.12+
Scikit-learn: 1.2+
Virtualenv: 20+

⚙️ Instalaciones y Configuración
# Crear entorno virtual
python -m venv venv

# Activar entorno (Windows)
venv\Scripts\activate

# Activar entorno (Linux/macOS)
source venv/bin/activate


# Configura la base de datos PostgreSQL en settings.py:
```SQL
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'libreria_db',
        'USER': 'postgres',
        'PASSWORD': 'tu_password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```
📖 Explicación del programa
El sistema permite:

✅ Registrar libros (título, autor, género, año, calificación)
✅ Consultar libros registrados
✅ Obtener sugerencias basadas en género y calificaciones promedio
✅ Generar gráficos para análisis exploratorio de datos

La arquitectura se basa en un modelo Django Book, vistas para manejo CRUD y scripts de análisis de datos en Python.

La arquitectura se basa en un modelo Django Book, vistas para manejo CRUD y scripts de análisis de datos en Python.

✅ Crear Autor
```python
from django.db import models

class Author(models.Model):
    name = models.CharField(max_length=255)

    def __str__(self):
        return self.name
```
Vista para registrar autor
```python
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
import json
from .models import Author

def add_author(request):
    if request.method == 'POST':
        data = json.loads(request.body)
        author = Author.objects.create(name=data['name'])
        return JsonResponse({'message': 'Autor creado', 'id': author.id})
```
Ejemplo de petición Postman:
![image](https://github.com/user-attachments/assets/7ba07caa-2dc1-44bd-a1e2-07665aa1816c)

✅ Crear Género
```python
from django.db import models

class Genre(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name
```
Vista para registrar género
```python
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
import json
from .models import Genre

def add_genre(request):
    if request.method == 'POST':
        data = json.loads(request.body)
        genre = Genre.objects.create(name=data['name'])
        return JsonResponse({'message': 'Género creado', 'id': genre.id})
```
Ejemplo de petición Postman:
![image](https://github.com/user-attachments/assets/f7b2e182-1cc1-4479-8856-838539921384)

✅ Registrar Libro
```python
from django.http import JsonResponse
from django.views.decorators.csrf import csrf_exempt
import json
from .models import Book

@csrf_exempt
def add_book(request):
    if request.method == 'POST':
        data = json.loads(request.body)
        book = Book.objects.create(
            title=data['title'],
            author=data['author'],
            genre=data['genre'],
            year=data['year'],
            rating=data['rating']
        )
        return JsonResponse({'message': 'Libro creado', 'id': book.id})
```
Ejemplo Postman:
![image](https://github.com/user-attachments/assets/5932ba8c-1e60-4c55-8d70-33fdfc8e1266)

✅ Listado de Libros
Endpoint para obtener listado
```python
from django.http import JsonResponse
from .models import Book

def list_books(request):
    books = Book.objects.all().values()
    return JsonResponse(list(books), safe=False)
```
![image](https://github.com/user-attachments/assets/aef1ac14-2543-48af-a278-be56b27db4d7)

✅ Valorar Libro
Vista para actualizar calificación
```python
from django.views.decorators.csrf import csrf_exempt
import json
from django.http import JsonResponse
from .models import Book

@csrf_exempt
def rate_book(request, book_id):
    if request.method == 'POST':
        data = json.loads(request.body)
        book = Book.objects.get(id=book_id)
        book.rating = data['rating']
        book.save()
        return JsonResponse({'message': 'Calificación actualizada', 'rating': book.rating})
```
![image](https://github.com/user-attachments/assets/644594f3-e433-4b4a-8529-e31002b63ee4)

📝 Documentación Script
Para leer registros y generar análisis, se utiliza un script Python con Pandas. Ejemplo de lectura:
```python
import pandas as pd

# Conexión a PostgreSQL
import psycopg2

conn = psycopg2.connect(
    dbname='libreria_db',
    user='postgres',
    password='tu_password',
    host='localhost',
    port='5432'
)

query = "SELECT * FROM libros_book;"
df = pd.read_sql(query, conn)

print(df.head())
```
📊 Generar y Explicar Gráficos
Para visualizar los datos se crearon gráficos en Google Colab o Google Drive utilizando Python, Pandas, Seaborn y Matplotlib.

Ejemplo de preguntas y gráficos
¿Cómo se distribuyen las calificaciones?
![image](https://github.com/user-attachments/assets/46ac5f8a-1f7f-4991-bd98-247d0052054f)

¿Cuál es el promedio de calificación por usuario?
![image](https://github.com/user-attachments/assets/72d3c8d3-1b3e-4fc7-8495-9b835697bb59)

¿Cuántos libros hay por género?
![image](https://github.com/user-attachments/assets/b7ad53f5-7a02-4860-805e-c15d11abfcf8)

¿Qué autores tienen más libros?
![image](https://github.com/user-attachments/assets/ac54fee6-4cea-4e8e-ab5f-ee2c05a32266)

¿Qué libros tienen mejor promedio de calificación?
![image](https://github.com/user-attachments/assets/1f71571c-52b6-4f1e-8017-1071c0a3bac2)

¿Cuál es la valoración media por género?
![image](https://github.com/user-attachments/assets/887b850b-bc85-4d78-afad-58d74ecf8762)

¿Cuáles son los libros más calificados?
![image](https://github.com/user-attachments/assets/8dfc5e73-714b-4e73-b5da-731fb1c34cb7)

¿Cuáles son los autores con más calificaciones?
![image](https://github.com/user-attachments/assets/366fe945-7413-425a-8870-054ca8244add)

¿Cómo han evolucionado las publicaciones por año?
![image](https://github.com/user-attachments/assets/116e7644-5da6-4258-9aaf-96b4e9521fe9)


⚖️ Licencias
✅ Python: PSF License
✅ Django: BSD License
✅ PostgreSQL: PostgreSQL License
✅ Pandas: BSD License
✅ Matplotlib: PSF-based License
✅ Seaborn: BSD License
✅ Scikit-learn: BSD License

Este proyecto se distribuye bajo la licencia MIT. Ver archivo LICENSE para más detalles.
