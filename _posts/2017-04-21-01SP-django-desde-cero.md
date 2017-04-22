---
layout: post
title:  Django desde cero
image: /img/container-tree-01.png
tags: [python, django]
date: 2017-04-21 21:41:27
---

1.Introducción, entorno, proyecto y aplicación
==============================================

## Introducción

**Objetivo:** Implementar una aplicación web con el framework **Django** desde cero, partiendo de la documentación oficial.


**Django; ideas básicas:** Patrón MVC. Orientación a objetos en todos los niveles del patrón. Vistas basadas en classes (Class Based View). Modelo de datos y persistencia basado en las clases Modelo de la aplicación.

## Entorno
Fedora Linux 25. Python 3.5. Django 1.11. Entorno de desarrollo virtual con venv.
[link]({{ site.baseurl }}{% link https://elpesodeloslunes.wordpress.com/2017/04/08/entornos-de-desarrollo-virtuales-con-python-3/ %}).

## Proyecto y aplicación.
No se ha definido el objetivo de la aplicación. Designaremos con nombres genéricos al proyecto (al que llamaremos **container**) y a la aplicación (a la que llamaremos **cube**), por analogía con la descripción que ofrece la documentación oficial de Django. 
>" [...] Un proyecto puede contener aplicaciones múltiples. Una aplicación puede estar en varios proyectos."


El repositorio github del proyecto será el siguiente: [container repo](https://github.com/).

### Creando el proyecto **container**
```bash
$ django-admin startproject container
```
<img src="img/container-tree-01.png" width="400"
 style="display: block; margin-left: auto; margin-right: auto;"/>

### Creando la aplicación **cube**
En el directorio raiz container (donde se encuentra el archivo manage.py) ejecutamos
```bash
$ python manage.py startapp cube
```
<img src="img/container-tree-02.png" width="400"
 style="display: block; margin-left: auto; margin-right: auto;"/>

### Configurando la base de datos
Inicialmente utilizaremos SQLite; por lo que nos bastará con la configuración por defecto en el archivo container/settings.py

Dejaremos, igualmente, las aplicaciones instaladas por defecto con Django (admin, auth, contenttypes, sessions, messages y static files)

Para inicializar la base de datos y generar las tablas necesarias de las distintas aplicaciones ejecutamos
```bash
$ python manage.py migrate
```


