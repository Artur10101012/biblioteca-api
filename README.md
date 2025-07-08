# 📚 API RESTful de Biblioteca

¡Bienvenido a la API RESTful para gestión de una biblioteca!

Este proyecto está desarrollado en **Django 5.2.4** con **Django REST Framework** y utiliza **PostgreSQL** como base de datos.

Permite gestionar libros, autores, géneros y calificaciones de usuarios, así como generar reportes visuales basados en los datos.

---

## 🔧 Requisitos y Tecnologías

### Principales librerías usadas:

- Django 5.2.4
- Django REST Framework
- djangorestframework-simplejwt
- psycopg2-binary
- pandas
- matplotlib
- seaborn

Consulta el listado completo de versiones y licencias [aquí](#-licencias).

---

## 🚀 Instalación

### 1. Crear entorno virtual

```bash
python -m venv venv
```

- Activar en Windows:
  ```bash
  venv\Scripts\activate
  ```
- Activar en Linux/macOS:
  ```bash
  source venv/bin/activate
  ```

---

### 2. Instalar dependencias

Si no contás con `requirements.txt`, instalá manualmente:

```bash
pip install django djangorestframework psycopg2-binary pandas matplotlib seaborn
```

---

## 🛠️ Configuración de Base de Datos

### Crear base de datos y usuario

Ejecutar en pgAdmin o consola:

```sql
CREATE DATABASE biblioteca;
CREATE USER biblioteca_user WITH PASSWORD 'biblioteca_pass';
GRANT ALL PRIVILEGES ON DATABASE biblioteca TO biblioteca_user;
```

### Configurar Django

Editar `biblioteca/settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'biblioteca',
        'USER': 'postgres',
        'PASSWORD': '123456789',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

### Aplicar migraciones

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 📚 Funcionalidades del Proyecto

✅ Registrar y gestionar:
- Libros
- Autores
- Géneros
- Calificaciones de usuarios

✅ Consultar listados y detalles

✅ Generar reportes gráficos con datos reales de la base

✅ Recomendaciones de libros por género según promedio de calificaciones

---

## 🔐 Pruebas en Postman

### Registro de usuario

- Endpoint: `POST /api/registro/`
- Body (JSON):

```json
{
  "username": "arturoaquino",
  "email": "arturoaquino@gmail.com",
  "password": "123456789"
}

```
![image](https://github.com/user-attachments/assets/8d7079f6-9418-448a-8c80-1d5bf409f92f)

---

### Login

- Endpoint: `POST /api/login/`
- Body (JSON):

```json
{
  "username": "arturoaquino",
  "password": "123456789"
}
```

![image](https://github.com/user-attachments/assets/e7bb5d6d-4b02-41e4-bb10-cd01ae47c51d)


✅ Obtendrás tokens JWT. Usá el **access token** para autenticación en endpoints protegidos:

```
Authorization: Bearer <access_token>
```

---

### Crear género

- Endpoint: `POST /api/generos/`

Body:

```json
{
  "nombre": "Aventura"
}
```

![image](https://github.com/user-attachments/assets/c67a4134-b768-4257-b4c0-ca521d9f13ca)


---

### Crear autor

- Endpoint: `POST /api/autores/`

Body:

```json
{
  "nombre": "Gabriel García Márquez",
  "nacionalidad": "Colombiana"
}
```
![image](https://github.com/user-attachments/assets/bd50854e-7b35-41e4-87bd-313676dff3f1)

---

### Crear libro

- Endpoint: `POST /api/libros/`

Body:

```json
{
  "titulo": "Cien años de soledad",
  "autor": 1,
  "genero": 1,
  "fecha_publicacion": "1967-05-30",
  "isbn": "1234567890",
  "url": "http://ejemplo.com/libro"
}
```
![image](https://github.com/user-attachments/assets/9663c16a-f65a-4d15-a3df-821ac1532998)


---

### Listar todos los libros

- Endpoint: `GET /api/libros/`

---

![image](https://github.com/user-attachments/assets/dccd90d3-3a48-4266-9571-69f52afef575)


## 📈 Generación de Reportes

### Script

```bash
python manage.py generar_reportes
```

Genera automáticamente 10 gráficos en PNG, guardados en la carpeta `reporte_graficos`.

**Algunos reportes:**
- Libros por género
- Libros por autor
- Promedios de calificación
- Histograma de calificaciones
- Promedio de calificación por usuario

---

## 💡 Recomendaciones por Género

Ejecutá:

```bash
python manage.py recomendar_por_genero Programación
```

✅ El sistema devuelve los libros mejor calificados en ese género.

**Búsqueda flexible:**  
- No distingue mayúsculas/minúsculas.
- No requiere tildes (acentos).

Ejemplo:

```bash
python manage.py recomendar_por_genero Programación
python manage.py recomendar_por_genero programacion
python manage.py recomendar_por_genero PROGRAMACION
```

---

## 📄 Licencias

Listado de paquetes utilizados:

```
asgiref                       3.8.1
contourpy                     1.3.2
cycler                        0.12.1
Django                        5.2.4
djangorestframework           3.16.0
djangorestframework_simplejwt 5.5.0
fonttools                     4.58.4
Jinja2                        3.1.6
kiwisolver                    1.4.8
license                       0.1a3
MarkupSafe                    3.0.2
matplotlib                    3.10.3
numpy                         2.3.1
packaging                     25.0
pandas                        2.3.0
pillow                        11.3.0
pip                           24.3.1
psycopg2-binary               2.9.10
PyJWT                         2.9.0
pyparsing                     3.2.3
python-dateutil               2.9.0.post0
pytz                          2025.2
seaborn                       0.13.2
six                           1.17.0
sqlparse                      0.5.3
tzdata                        2025.2
Unidecode                     1.4.0
```

---

© Proyecto educativo desarrollado para fines de aprendizaje y evaluación. Licencia MIT.
