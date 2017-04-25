---
layout: post
title: Tutorial de Django - Django Basics
subtitle: 1 - Introducción, entorno, proyecto y aplicación.
image: /img/container-tree-01.png
tags: [python, django]
date: 2017-04-21 21:41:27
---

## Introducción

**Objetivo:** Implementar una aplicación web, desde cero, haciendo uso del framework **Django** a partir de la documentación oficial.

**Django; ideas básicas:** Patrón MVC. Orientación a objetos en todos los niveles del patrón. Vistas basadas en classes (Class Based View). Modelo de datos y persistencia basado en las clases Modelo de la aplicación (DRY Principle).

## Entorno
Fedora Linux 25. Python 3.5. Django 1.11. Entorno de desarrollo virtual con venv.
[link](https://elpesodeloslunes.wordpress.com/2017/04/08/entornos-de-desarrollo-virtuales-con-python-3/).

## Proyecto y aplicación.
No se ha definido el objetivo de la aplicación. Designaremos con nombres genéricos al proyecto (al que llamaremos **container**) y a la aplicación (a la que llamaremos **cube**), por analogía con la descripción que ofrece la documentación oficial de Django. 
>" [...] Un proyecto puede contener aplicaciones múltiples. Una aplicación puede estar en varios proyectos."


El repositorio github del proyecto será el siguiente: [container repo](https://github.com/).

### Creando el proyecto **container**
```
$ django-admin startproject container
```
<img src="https://aalvarezg.github.io/img/container-tree-01.png" width="200"
 style="display: block; margin-left: auto; margin-right: auto;"/>

### Creando la aplicación **cube**
En el directorio raiz container, donde se encuentra el archivo manage.py, ejecutamos

```
$ python manage.py startapp cube
```

<img src="https://aalvarezg.github.io/img/container-tree-02.png" width="400"
 style="display: block; margin-left: auto; margin-right: auto;"/>



### El archivo settings.py
Este archivo de configuración se encuentra en el directorio raiz del proyecto (en nuestro caso container/settings.py). Como hemos visto anteriormente es el lugar donde establecer y modificar las distintas configuraciones de nuestro proyecto.

En el apartado INSTALLED_APPS mantenemos las aplicaciones instaladas por defecto con Django (admin, auth, contenttypes, sessions, messages y static files) y añadimos la nuestra (cube) para que Django la reconozca.

```
INSTALLED_APPS = [
    'cube.apps.CubeConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```


#### Configurando el sistema gestor de base de datos
Inicialmente utilizaremos SQLite, la base de datos instalada por defecto en Django. Bastará con dejar la configuración ya establecida en el archivo container/settings.py

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
Para inicializar las base de datos y generar las tablas necesarias de las distintas aplicaciones ejecutamos

```
$ python manage.py migrate
```
---

## Referencias
[1 - docs.djangoproject.com - Tutorial](https://docs.djangoproject.com/en/1.11/intro/tutorial01/)
