---
title: Tutorial de Django - Django Basics
subtitle: 2 - Modelo de Datos
layout: post
image: /img/ddbb.png
tags: [python, django]
date: 2017-04-23 20:59:54
---

## Filosofía del modelo de datos en Django

Una de las características de Django que más me llamó la atención fue la aplicación del llamado "DRY Principle" para la capa de persistencia. Se trata de definir el modelo de datos en un solo lugar y automáticamente derivar de ahí todo lo demás.

Veremos el alcance de este modelo en este y sucesivos posts.

## Definición del modelo de datos en nuestro proyecto.

Llegados a este punto ya deberíamos tener una idea básica de lo que queremos implementar en nuestro proyecto. Inicialmente, he pensado en una aplicación que presente al usuario un formulario para subir ficheros al servidor. 

Estos ficheros podrían ser datasets que trataríamos con la excelente libreria **python-pandas** para extraer datos relevantes de los mismos.... pero será otra historia, vamos paso a paso.

Vamos a simplificar al máximo nuestro modelo. Esto nos facilitará la vida para entender los apartados siguientes. Inicialmente, implementamos la clase que define nuestro modelo de la forma siguiente:

```python
class Research (models.Model):
	description = models.CharField(max_length=150)
	date_time = models.DateField()
	
```
En principo solo se realiza una clase (tabla), a la que llamaremos Research, con dos campos que hemos instanciado con su correspondiente tipo de datos (CharField y DateField). Django utilizará el tipo de datos asociado a cada campo para tres cosas:

1. Determinar el tipo de datos que almacenará el campo en la base de datos.
2. El widget HTML que se usará por defecto cuando se renderize el campo en un formulario. **(~Aquí me surge la siguiente duda ¿No supone esto cierto acoplamiento entre las capas Modelo y Vista? _Conforme evolucione la aplicación lo comprobaremos_~)**
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
<a name="part2"></a> 

### Managers.  
Vamos a pararnos aquí un poco, inicialmente diremos que los **Managers**  suministran a los modelos las operaciones para ejecutar operaciones de consulta sobre la base de datos.

¿Cómo? Haciendo uso de relaciones de herencia propias de la [metodología programación orientada a objetos](https://es.wikipedia.org/wiki/Programaci%C3%B3n_orientada_a_objetos). 

A grandes rasgos:

a) Existe definida en Django una clase llamada **Manager()** donde se implementan las operaciones de consulta “típicas” a una base de datos (como pueden ser extraer todos los objetos de una tabla).
    
b) Cada vez que se crea un Modelo, Django asigna por defecto al mismo la interfaz Manager() de forma que esta nueva clase que define el Modelo pueda hacer uso de las operaciones de consulta definidas en dicha interfaz.

Así, nuestra clase hereda por defecto un atributo llamado **objects** que podemos utilizar para instanciar objetos desde la base de datos.


Veamos su funcionamiento con el siguente código autoexplicativo haciendo uso del **shell de Django**

[Django Docs - Shell de Django](https://docs.djangoproject.com/en/1.11/intro/tutorial02/#playing-with-the-api)

```bash
$ python manage.py shell
Python 3.5.2 (default, Sep 14 2016, 11:28:32) 
[GCC 6.2.1 20160901 (Red Hat 6.2.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
##Primero importamos nuestro modelo y las funciones necesarias
>>> from cube.models import Research
>>> from django.utils import timezone
##Instanciamos un nuevo objeto Research y lo insertamos en la base de datos
>>> r = Research(description="test01", date_time=timezone.now())
>>> r.save()
>>> r.id
1
##Seguidamente hacemos uso del atributo objects para consultar la base de datos. En este caso extremos todos los objetos del tipo Research
>>> Research.objects.all()
<QuerySet [<Research: Research object>]>
## o el objeto Research con id=1 
>>> Research.objects.get(pk=1)
<Research: Research object>

```

El objeto objects solo es accesible desde la clase del Modelo, no desde sus instancias. En el ejemplo anterior sería incorrecta la llamada r.objects.

Por supuesto, podríamos crear _nuestros propios Managers_ heredando de los ya existentes, ampliando o modificando, si nuestras necesidades de consulta no están definidas en la clase Manager().

### Métodos 
En las clases que definen nuestros Modelos podemos definir **métodos** que son funciones para mejorar la comprensión de los propios modelos o extaer información de los mismos que pudiera sernos de utilidad.

Por ejemplo, en la documentación oficial de Django recomienda definir el método __str__ , entre otras cosas, para establecer  la representación que nos devuelve el Modelo al mostrar sus instancias en consola. Hemos visto que las consultas realizadas en el shell nos devuelven una representación de los objetos Research muy genérica que pudiera no sernos del todo útil (- Research: Research object -). Podemos cambiar este comportamiento definiendo en nuestro modelo el método __str__.

```python
def __str__(self):
	return self.description
```
Ahora, nuestras consultas al shell serán mucho más amigables.

```bash
>>> Research.objects.all()
<QuerySet [<Research: test01>]>
>>> r = Research.objects.get(pk=1)
>>> r
<Research: test01>
```
El método __str__ es predefinido de Python. Igualmente podemos añadir métodos personalizados, por ejemplo uno que nos devuelva si el estudio se ha realizado en los últimos 7 días. Nuestro modelo quedaría de la forma siguiente:

```python
import datetime
from django.db import models
from django.utils import timezone

# Create your models here.

class Research (models.Model):
	description = models.CharField(default="test", max_length=150)
	date_time = models.DateTimeField("Fecha Estudio")
	
	def __str__(self):
		return self.description

	def is_recent (self):
		#Return true if research hans been executed in last 7 days 
		return self.date_time >= timezone.now() - datetime.timedelta(days=7)

```
Si ejecutamos el nuevo método en el shell

```bash
$ python manage.py shell
Python 3.5.2 (default, Sep 14 2016, 11:28:32) 
[GCC 6.2.1 20160901 (Red Hat 6.2.1-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> from cube.models import Research
>>> r = Research.objects.get(pk=1)
>>> r
<Research: test01>
>>> r.is_recent()
True
```

---

## Referencias
[1 - docs.djangoproject.com- Topic Guides ](https://docs.djangoproject.com/en/1.11/topics/db/models/)

[2 - docs.djangoproject.com - Tutorial](https://docs.djangoproject.com/en/1.11/intro/tutorial02/)





