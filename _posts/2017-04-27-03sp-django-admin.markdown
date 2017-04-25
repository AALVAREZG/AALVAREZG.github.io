---
title: Tutorial de Django - Django Basics
subtitle: 2 - Modelo de Datos
layout: post
image: /img/ddbb.png
tags: [python, django]
date: 2017-04-23 20:59:54
---

## Filosofía del modelo de datos en Django

Una de las características de Django que más me llamó la atención fue la utilización del llamado "DRY Principle" para la capa de persistencia. Se trata de definir el modelo de datos en un solo lugar y automaticamente derivar de ahí todo lo demás.

Veremos el alcance de esta implementación a lo largo del ejemplo.

## Definición del modelo de datos en nuestro proyecto.

Llegados a este punto ya deberíamos tener una idea básica de lo que queremos implementar en nuestro proyecto. Inicialmente, he pensado en una aplicación que presente al usuario un formulario para subir ficheros al servidor. 

Estos ficheros podrían ser datasets que trataríamos con la excelente libreria **python-pandas** para extraer datos relevantes de los mismos.... pero será otra historia, vamos paso a paso.

En una implementación inicial, nuestro modelo quedaría de la forma siguiente:

```python
class Research (models.Model):
	date_time = models.DateField()
	ip_address = models.CharField(max_length=30)
	system = models.CharField(max_length=40)
	browser = models.CharField(max_length=30)
	input_file = models.FileField(upload_to='uploads/%Y%m%d%h%M%s/')
	output_file = models.CharField(max_length=50)
```
En principo solo se implementa una clase (tabla), a la que llamaremos Research, con seis campos que hemos instanciado con su correspondiente tipo de datos (DateField, CharField y FileField). Django utilizará el tipo de datos asociado a cada campo para tres cosas:

1. Determinar el tipo de datos que almacenará el campo en la base de datos.
2. El widget HTML que se usará por defecto cuando se renderize el campo en un formulario. **(~Aquí me surge la siguiente duda ¿No supone esto cierto acoplamiento entre las capas Modelo y Vista? _Mas adelante lo comprobaremos_~)**
3. Determinar los requisitos mínimos de validación, tanto en las vistas generadas por Django's admin como en los formularios generados automáticamente. **(~Idem de lo anterior con respecto al acomplamiento~)**

## Clave primaria
Por defecto **Django** añade automáticamente a cada modelo un campo de tipo autonumérico que se utilizará como clave primaria. Este comportamiento puede cambiarse en el archivo de configuración.

## Activando el modelo
Django gestiona los cambios del modelo de datos mediante las llamadas **migraciones**. Este método permite trasladar los cambios del modelo a lo largo del ciclo de vida de nuestra aplicación sin necesidad de modificar o borrar la base de datos o tablas en nuestro sistema gestor de BBDD.
L@s chic@s de Django lo explican mejor que yo:

>Migrations are very powerful and let you change your models over time, as you develop your project, without the need to delete your database or tables and make new ones - it specializes in upgrading your database live, without losing data. 
>**Three-step guide to making model changes:**

>  * Change your models (in models.py).
>  * Run python manage.py makemigrations to create migrations for those changes
>  * Run python manage.py migrate to apply those changes to the database.

Así, si ejecutamos 

```
python manage.py makemigrations cube
```

 deberíamos ver el siguiente resultado

```
$ python manage.py makemigrations cube
Migrations for 'cube':
  cube/migrations/0001_initial.py
    - Create model Research
```
Podemos comprobar que se ha creado el archivo de la migración en el directorio migrations. Para consultarlo ejecutamos:

```
 $ python manage.py sqlmigrate polls 0001
```
Por último trasladamos los cambios a la base de datos ejecutando el comando:

```
$ python manage.py migrate
```

---

## Referencias
[1 - docs.djangoproject.com- Topic Guides ](https://docs.djangoproject.com/en/1.11/topics/db/models/)

[2 - docs.djangoproject.com - Tutorial](https://docs.djangoproject.com/en/1.11/intro/tutorial02/)





