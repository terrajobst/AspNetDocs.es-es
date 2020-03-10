---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Creacíón de una base de datos | Microsoft Docs
author: microsoft
description: En el paso 2 se muestran los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b0aa7c8cdf741f44e09ed18e2b2f73fe6bf786ae
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469303"
---
# <a name="create-a-database"></a>Crear una base de datos

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 2 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 2 se muestran los pasos para crear la base de datos que contiene todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner paso 2: crear la base de datos

Usaremos una base de datos para almacenar todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.

En los pasos siguientes se muestra cómo crear la base de datos con la edición gratuita de SQL Server Express (que puede instalar fácilmente mediante la versión 2 de la [instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)). Todo el código que escribiremos funciona con SQL Server Express y con el SQL Server completo.

### <a name="creating-a-new-sql-server-express-database"></a>Crear una nueva base de datos de SQL Server Express

Comenzaremos haciendo clic con el botón derecho en nuestro proyecto web y, a continuación, seleccionaremos el comando de menú **agregar&gt;nuevo elemento** :

![](create-a-database/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar nuevo elemento" de Visual Studio. Filtraremos por la categoría "Data" (datos) y seleccionaremos la plantilla de elemento "SQL Server Database":

![](create-a-database/_static/image2.png)

Asignaremos el nombre "NerdDinner. MDF" y aceptaremos la base de datos de SQL Server Express. Visual Studio nos preguntará si queremos agregar este archivo al directorio de datos de \App\_(que es un directorio ya configurado con las ACL de seguridad de lectura y escritura):

![](create-a-database/_static/image3.png)

Haremos clic en "sí" y la nueva base de datos se creará y se agregará a nuestro Explorador de soluciones:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Crear tablas dentro de nuestra base de datos

Ahora tenemos una nueva base de datos vacía. Vamos a agregarle algunas tablas.

Para ello, desplazaremos a la ventana de pestaña "Explorador de servidores" dentro de Visual Studio, lo que nos permite administrar las bases de datos y los servidores. SQL Server Express las bases de datos almacenadas en la carpeta \App\_data de nuestra aplicación se mostrarán automáticamente en el Explorador de servidores. Opcionalmente, puede usar el icono "conectar con base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar más bases de datos de SQL Server (tanto locales como remotas) a la lista también:

![](create-a-database/_static/image5.png)

Agregaremos dos tablas a nuestra base de datos NerdDinner: una para almacenar nuestras cenas y la otra para realizar el seguimiento de las aceptaciones RSVP. Podemos crear nuevas tablas haciendo clic con el botón derecho en la carpeta "tablas" de nuestra base de datos y eligiendo el comando de menú "Agregar nueva tabla":

![](create-a-database/_static/image6.png)

Se abrirá un diseñador de tablas que nos permite configurar el esquema de la tabla. En nuestra tabla "cenas", agregaremos 10 columnas de datos:

![](create-a-database/_static/image7.png)

Queremos que la columna "DinnerID" sea una clave principal única para la tabla. Para configurarlo, haga clic con el botón derecho en la columna "DinnerID" y elija el elemento de menú "establecer clave principal":

![](create-a-database/_static/image8.png)

Además de convertir DinnerID en una clave principal, también queremos configurarla como una columna de "identidad" cuyo valor se incrementa automáticamente a medida que se agregan nuevas filas de datos a la tabla (lo que significa que la primera fila de cena insertada tendrá un DinnerID de 1, la segunda fila insertada). tendrá un DinnerID de 2, etc.).

Para ello, seleccione la columna "DinnerID" y, a continuación, use el editor de "propiedades de columna" para establecer la propiedad "(is Identity)" en la columna en "Yes". Usaremos los valores predeterminados de la identidad estándar (empieza en 1 y el incremento 1 en cada fila nueva cena):

![](create-a-database/_static/image9.png)

Después, guardaremos la tabla escribiendo Ctrl-S o usando el comando **de menú guardar archivo&gt;** . Esto le pedirá que asigne un nombre a la tabla. Lo denominaremos "cenas":

![](create-a-database/_static/image10.png)

La nueva tabla de cenas se mostrará en la base de datos en el explorador de servidores.

A continuación, repetiremos los pasos anteriores y crearemos una tabla "RSVP". Esta tabla contiene tres columnas. La columna RsvpID se configurará como la clave principal y también se convertirá en una columna de identidad:

![](create-a-database/_static/image11.png)

Lo guardaremos y le asignaremos el nombre "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configurar una relación de clave externa entre tablas

Ahora tenemos dos tablas en nuestra base de datos. Nuestro último paso de diseño de esquema será configurar una relación "uno a varios" entre estas dos tablas, de modo que podamos asociar cada fila de cena con cero o más filas RSVP que se apliquen a ella. Para ello, se configura la columna "DinnerID" de la tabla RSVP para que tenga una relación de clave externa con la columna "DinnerID" de la tabla "cenas".

Para ello, abriremos la tabla RSVP en el diseñador de tablas haciendo doble clic en ella en el explorador de servidores. A continuación, seleccionaremos la columna "DinnerID" que contiene, hacemos clic con el botón derecho y elegiremos "relaciones..." comando del menú contextual:

![](create-a-database/_static/image12.png)

Se abrirá un cuadro de diálogo que se puede usar para configurar las relaciones entre las tablas:

![](create-a-database/_static/image13.png)

Haremos clic en el botón "agregar" para agregar una nueva relación al cuadro de diálogo. Una vez que se ha agregado una relación, expandiremos el nodo de vista de árbol de las tablas y las especificaciones de columna en la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, hacer clic en el botón "..." situado a su derecha:

![](create-a-database/_static/image14.png)

Al hacer clic en "..." el botón abrirá otro cuadro de diálogo que nos permite especificar qué tablas y columnas participan en la relación, así como permitirnos asignar un nombre a la relación.

Cambiaremos la tabla de la clave principal a "cenas" y seleccionaremos la columna "DinnerID" dentro de la tabla cenas como clave principal. La tabla RSVP será la tabla de clave externa y la RSVP. La columna DinnerID se asociará como clave externa:

![](create-a-database/_static/image15.png)

Ahora, cada fila de la tabla RSVP se asociará a una fila de la tabla cena. SQL Server mantendrá la integridad referencial para nosotros, y nos impedimos agregar una nueva fila RSVP si no señala a una fila cena válida. También nos impedirá eliminar una fila de cena si todavía hay filas RSVP que hacen referencia a ella.

### <a name="adding-data-to-our-tables"></a>Agregar datos a las tablas

Vamos a terminar agregando algunos datos de ejemplo a nuestra tabla de cenas. Podemos agregar datos a una tabla haciendo clic con el botón derecho en él en el Explorador de servidores y eligiendo el comando "Mostrar datos de tabla":

![](create-a-database/_static/image16.png)

Agregaremos algunas filas de datos de cenas que podremos usar más adelante a medida que comenzamos a implementar la aplicación:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Paso siguiente

Hemos terminado de crear la base de datos. Vamos a crear ahora clases de modelo que se pueden usar para realizar consultas y actualizarlas.

> [!div class="step-by-step"]
> [Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)
