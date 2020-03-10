---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Crear una base de datos | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lea y escriba en una base de datos.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 995db714ce6365415d06dc458aee84a31c7c8fd6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78469675"
---
# <a name="creating-a-database"></a>Crear una base de datos

por [Scott Hanselman](https://github.com/shanselman)

> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe en una base de datos. Visite el [centro de aprendizaje de MVC de ASP.net](../../../index.md) para buscar otros tutoriales y ejemplos de ASP.NET MVC.

En esta sección vamos a crear una nueva base de datos de SQL Express que usaremos para almacenar y recuperar los datos de la película. En el IDE de Visual Web Developer, seleccione Vista | Explorador de servidores. Haga clic con el botón derecho en conexiones de datos y haga clic en agregar conexión...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

En el cuadro de diálogo elegir origen de datos, seleccione Microsoft SQL Server y seleccione continuar.

![](getting-started-with-mvc-part4/_static/image2.png)

En el cuadro de diálogo Agregar conexión, escriba ".\SQLEXPRESS" como nombre del servidor y escriba "Movies" como nombre de la nueva base de datos.

[![cuadro de diálogo Agregar conexión](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Haga clic en aceptar y se le preguntará si desea crear la base de datos. Seleccione Sí.

[¿![crear películas?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Ahora tiene una base de datos vacía en Explorador de servidores.

![Agregar nueva tabla](getting-started-with-mvc-part4/_static/image7.png)

Haga clic con el botón derecho en tablas y haga clic en agregar tabla. Aparecerá el Diseñador de tablas. Agregue columnas para ID, title, ReleaseDate, Genre y price. Haga clic con el botón derecho en la columna ID y haga clic en establecer clave principal. Este es el aspecto de mis áreas de diseño.

[Editor de tablas de ![base de datos](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Además, seleccione la columna ID. y, en propiedades de columna, cambie "Especificación de identidad" a "sí".

[![IsIdentity-propiedades de la columna](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Cuando haya terminado, haga clic en el icono guardar en la barra de herramientas o seleccione Archivo | Guarde en el menú y asigne un nombre a la tabla "**Movie**" (singular). Tenemos una base de datos y una tabla.

[![elegir nombre](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Vuelva a Explorador de servidores, haga clic con el botón secundario en la tabla de películas y seleccione "Mostrar datos de tabla". Escriba algunas películas para que nuestra base de datos tenga algunos datos.

[![edición de tabla de base de datos](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Creación de un modelo

Ahora, vuelva a la Explorador de soluciones en el lado derecho del IDE y haga clic con el botón derecho en la carpeta modelos y seleccione Agregar | Nuevo elemento.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vamos a crear un modelo de entidad a partir de la nueva base de datos. Esto agregará un conjunto de clases a nuestro proyecto que facilitan la consulta y manipulación de los datos de nuestra base de datos. Seleccione el nodo de datos en el lado izquierdo del cuadro de diálogo y, a continuación, seleccione la plantilla ADO.NET Entity Data Model Item. Asígnele el nombre movies. edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Haga clic en el botón "Agregar". A continuación, se iniciará el "Asistente para Entity Data Model".

En el nuevo cuadro de diálogo que aparece, seleccione generar desde la base de datos. Dado que se acaba de crear una base de datos, solo hay que indicar al Entity Framework sobre la nueva base de datos y su tabla. Haga clic en siguiente para guardar la conexión de la base de datos en la configuración de la aplicación Web. Ahora, active la casilla tablas y películas y haga clic en finalizar.

[Asistente para Entity Data Model de ![](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Ahora podemos ver la nueva tabla de películas en el Entity Framework Designer y acceder a ella desde el código.

[Películas de ![: Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

En la superficie de diseño, puede ver una clase "Movie". Esta clase se asigna a la tabla "Movie" de nuestra base de datos y cada propiedad dentro de ella se asigna a una columna con la tabla. Cada instancia de una clase "Movie" se corresponderá con una fila de la tabla "Movie".

Si no le gustan las convenciones de nomenclatura y asignación predeterminadas utilizadas por el Entity Framework, puede utilizar el diseñador de Entity Framework para cambiarlos o personalizarlos. En esta aplicación se usarán los valores predeterminados y solo se guardará el archivo tal cual.

Ahora, vamos a trabajar con algunos datos reales.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part3.md)
> [Siguiente](getting-started-with-mvc-part5.md)
