# ✅ Desplegar Django en Vercel

En este repositorio encontraras la información y el código para que aprendas a como desplegar tu proyecto de Django a Vercel.

> Si te gusta el repositorio puedes regalarme una estrella.
> [Video de YouTube](https://youtu.be/eQJFHfFn-sk)

![App Screenshot](https://i.imgur.com/8oH8kyD.png)


[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)

## Instrucciónes
Acá encontraras las instrucciones de cada uno de los archivos y las modificaciones que deberas de realizar para que todo funciones correctamente. Además si desearas conectar a base de datos tu proyecto también encontraras como realizarlo.

### requirements.txt

```python
Django
whitenoise
```

### settings.py

```python
#Imports
import os

# ALLOWED_HOSTS
ALLOWED_HOSTS = ['.vercel.app', 'localhost', '127.0.0.1']

# WSGI_APPLICATION
WSGI_APPLICATION = 'VercelDeploy.wsgi.app'

# Static files (CSS, JavaScript, Images)
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')

STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

```

### urls.py

```python
#Imports
...
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    ...
]


urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)

```

### wsgi.py

```python
...

app = application
```


### vercel.json

```json
{
    "builds" : [
        {
            "src": "<NombreDeTuProyecto>/wsgi.py",
            "use": "@vercel/python"
        }
    ],
    "routes": [
        {
            "src": "/(.*)",
            "dest": "<NombreDeTuProyecto>/wsgi.py"
        }
    ]
}
```

Con estas configuraciones es más que suficiente para hacer que tu proyecto funcione totalmente con vercel. 

> Nunca olvides agregar tu .gitignore

## Adicional

Si deseas conectar tu base de datos por ejemplo con postgres deberás de utilizar la siguiente configuraciones.

### settings.py

```python
...

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME':  "TU BASE DE DATOS",
        'USER':  "TU USUARIO DE BASE DE DATOS",
        'PASSWORD':  "TU CONTRASEÑA DE BASE DE DATOS",
        'HOST':  "EL HOST DE TU BASE DE DATOS",
        'PORT':     "EL PUERTO DE TU BASE DE DATOS",
    }
}

...
```

### requirements.txt

```txt
Django
whitenoise

asgiref
gunicorn
psycopg2-binary
sqlparse    
```

### vercel.json

```json

{
    "builds" : [
        {
            "src": "VercelDeploy/wsgi.py",
            "use": "@vercel/python"
        },
        {
            "src": "build.sh",
            "use": "@vercel/static-build",
            "config": {
                "distDir": "staticfiles"
            }
        }
    ],
    "routes": [
        {
            "src": "/(.*)",
            "dest": "VercelDeploy/wsgi.py"
        }
    ]
}

```

### build.sh

```bash

#!/usr/bin/env bash

echo "Construyendo aplicación..."
python3 -m pip install -r requirements.txt

echo "Migrando Base de Datos..."
python3 manage.py makemigrations --noinput
python3 manage.py migrate --noinput

echo "Recopilando archivos estáticos..."
python3 manage.py collectstatic --noinput

```
## Autor

- [@estuardodev](https://www.github.com/estuardodev)

