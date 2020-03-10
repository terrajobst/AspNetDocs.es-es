---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms ASP.NET con las Entity Framework 4,0 y Visual Studio 2010...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: fd88b32ad2a65e5b4c7ead15f0d6dc5dc6e97e75
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511681"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Introducción con Entity Framework 4,0 Database First y formularios Web Forms de ASP.NET 4

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. La aplicación de ejemplo es un sitio web de una Universidad ficticia de contoso. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.
> 
> En el tutorial se muestran C#ejemplos de. El [ejemplo descargable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) contiene código en C# y Visual Basic.
> 
> ## <a name="database-first"></a>Database First
> 
> Hay tres maneras de trabajar con datos en el Entity Framework: *Database First*, *Model First*y *code First*. Este tutorial es para Database First. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir el mejor para su escenario, consulte [Entity Framework flujos de trabajo de desarrollo](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>formularios Web Forms
> 
> En esta serie de tutoriales se usa el modelo de formularios Web Forms de ASP.NET y se supone que sabe cómo trabajar con formularios Web Forms de ASP.NET en Visual Studio. Si no lo hace, vea [Introducción con formularios Web Forms de ASP.NET 4,5](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si prefiere trabajar con el marco de ASP.NET MVC, consulte [Introducción con el Entity Framework mediante ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. El tutorial no se ha probado con versiones posteriores de Visual Studio. Hay muchas diferencias en las selecciones de menú, los cuadros de diálogo y las plantillas. |
> | .NET 4 | .NET 4,5 es compatible con versiones anteriores de .NET 4, pero el tutorial no se ha probado con .NET 4,5. |
> | Entity Framework 4 | El tutorial no se ha probado con versiones posteriores de Entity Framework. A partir de Entity Framework 5, EF utiliza de forma predeterminada el `DbContext API` que se presentó con EF 4,1. El control EntityDataSource se diseñó para usar la API de `ObjectContext`. Para obtener información sobre cómo usar el control EntityDataSource con la API de `DbContext`, consulte [esta entrada de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [Entity Framework de ASP.net](https://forums.asp.net/1227.aspx), en el [Foro de Entity Framework y LINQ to Entities](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o en [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Información general

La aplicación que se va a compilar en estos tutoriales es un sitio web sencillo de la Universidad.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. A continuación se muestran algunas de las pantallas que va a crear.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Creación de la aplicación Web

Para iniciar el tutorial, abra Visual Studio y, a continuación, cree un nuevo proyecto de aplicación Web de ASP.NET mediante la plantilla de **aplicación web ASP.net** :

[![Image01](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Esta plantilla crea un proyecto de aplicación web que ya incluye una hoja de estilos y páginas maestras:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Abra el archivo *site. Master* y cambie "My ASP.net Application" por "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Busque el control de *menú* denominado `NavigationMenu` y reemplácelo por el marcado siguiente, que agrega los elementos de menú para las páginas que va a crear.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Abra la página *default. aspx* y cambie el control `Content` denominado `BodyContent` por este:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Ahora tiene una página principal sencilla con vínculos a las distintas páginas que va a crear:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Crear la base de datos

Para estos tutoriales, usará el diseñador de modelos de datos Entity Framework para crear automáticamente el modelo de datos basado en una base de datos existente (a menudo denominado enfoque *de base de datos primero* ). Una alternativa que no se trata en esta serie de tutoriales es crear el modelo de datos manualmente y, a continuación, hacer que el diseñador genere scripts que crean la base de datos (el enfoque del *modelo primero* ).

En el caso del método Database-First que se usa en este tutorial, el paso siguiente consiste en agregar una base de datos al sitio. La manera más fácil es descargar primero el proyecto que se incluye en este tutorial. A continuación, haga clic con el botón derecho en la carpeta *App\_Data* , seleccione **Add Existing Item (Agregar elemento existente**) y seleccione el archivo de base de datos *School. MDF* del proyecto descargado.

Una alternativa es seguir las instrucciones de [creación de la base de datos de ejemplo School](https://msdn.microsoft.com/library/bb399731.aspx). Tanto si descarga la base de datos como si la crea, copie el archivo *School. MDF* de la carpeta siguiente a la carpeta de datos de la aplicación *\_* de la aplicación:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Esta ubicación del archivo *. MDF* supone que está usando SQL Server 2008 Express).

Si crea la base de datos a partir de un script, realice los pasos siguientes para crear un diagrama de base de datos:

1. En **Explorador de servidores**, expanda **conexiones de datos**, *School. MDF*, haga clic con el botón secundario en **diagramas de base**de datos y seleccione **Agregar nuevo diagrama**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Seleccione todas las tablas y, a continuación, haga clic en **Agregar**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server crea un diagrama de base de datos que muestra las tablas, las columnas de las tablas y las relaciones entre las tablas. Puede mover las tablas para organizarlas de la forma que desee.
3. Guarde el diagrama como "SchoolDiagram" y ciérrelo.

Si descarga el archivo *School. MDF* que se incluye en este tutorial, puede ver el diagrama de base de datos haciendo doble clic en **SchoolDiagram** en **diagramas de base de datos** en **Explorador de servidores**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

El diagrama tiene un aspecto similar al siguiente (las tablas pueden estar en ubicaciones diferentes de las que se muestran aquí):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Crear el modelo de datos de Entity Framework

Ahora puede crear un modelo de datos de Entity Framework a partir de esta base de datos. Puede crear el modelo de datos en la carpeta raíz de la aplicación, pero para este tutorial lo colocará en una carpeta denominada *Dal* (para el nivel de acceso a datos).

En **Explorador de soluciones**, agregue una carpeta de proyecto denominada *Dal* (Asegúrese de que se encuentra en el proyecto, no en la solución).

Haga clic con el botón secundario en la carpeta *Dal* y seleccione **Agregar** y **nuevo elemento**. En **plantillas instaladas**, seleccione **datos**, seleccione la plantilla **ADO.NET Entity Data Model** , asígnele el nombre *SchoolModel. edmx*y, a continuación, haga clic en **Agregar**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Esto iniciará el Asistente para Entity Data Model. En el primer paso del asistente, la opción **generar desde la base de datos** está seleccionada de forma predeterminada. Haga clic en **Siguiente**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

En el paso **elegir la conexión de datos** , deje los valores predeterminados y haga clic en **siguiente**. La base de datos School está seleccionada de forma predeterminada y la configuración de conexión se guarda en el archivo *Web. config* como **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

En el paso del asistente **Elija los objetos de base de datos** , seleccione todas las tablas excepto `sysdiagrams` (que se creó para el diagrama que generó anteriormente) y, a continuación, haga clic en **Finalizar**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Una vez finalizada la creación del modelo, Visual Studio muestra una representación gráfica de los objetos de Entity Framework (entidades) que corresponden a las tablas de base de datos. (Al igual que con el diagrama de base de datos, la ubicación de los elementos individuales puede ser diferente de lo que se ve en esta ilustración. Puede arrastrar los elementos para que coincidan con la ilustración si lo desea).

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Exploración del modelo de datos de Entity Framework

Puede ver que el diagrama de la entidad es muy similar al diagrama de base de datos, con un par de diferencias. Una diferencia es la adición de símbolos al final de cada asociación que indican el tipo de asociación (las relaciones entre las tablas se denominan asociaciones de entidad en el modelo de datos):

- Una asociación de uno a cero o uno está representada por "1" y "0.. 1".

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    En este caso, una entidad `Person` puede estar asociada o no a una entidad `OfficeAssignment`. Una entidad `OfficeAssignment` debe estar asociada a una entidad `Person`. Es decir, un instructor puede asignarse o no a una oficina y cualquier oficina puede asignarse a un solo instructor.
- Una asociación uno a varios está representada por "1" y "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    En este caso, una entidad `Person` puede tener asociados `StudentGrade` entidades. Una entidad `StudentGrade` debe estar asociada a una entidad `Person`. `StudentGrade` entidades representan realmente los cursos inscritos en esta base de datos; Si un estudiante está inscrito en un curso y todavía no hay ningún grado de calificación, la propiedad `Grade` es NULL. Es decir, es posible que un estudiante no esté inscrito todavía en ningún curso, que se inscriba en un curso o que se inscriba en varios cursos. Cada calificación de un curso inscrito se aplica solo a un estudiante.
- Una asociación de varios a varios se representa mediante "\*" y "\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    En este caso, una entidad `Person` puede tener asociados `Course` entidades, y la operación inversa también se cumple: una entidad `Course` puede tener asociados `Person` entidades. En otras palabras, un instructor puede enseñar varios cursos y un curso puede ser impartido por varios instructores. (En esta base de datos, esta relación se aplica solo a los instructores; no vincula a los estudiantes con los cursos. Los estudiantes están vinculados a los cursos en la tabla StudentGrades).

Otra diferencia entre el diagrama de base de datos y el modelo de datos es la sección **propiedades de navegación** adicionales para cada entidad. Una propiedad de navegación de una entidad hace referencia a las entidades relacionadas. Por ejemplo, la propiedad `Courses` de una entidad `Person` contiene una colección de todas las entidades `Course` que están relacionadas con esa `Person` entidad.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Otra diferencia entre la base de datos y el modelo de datos es la ausencia de la tabla de asociación `CourseInstructor` que se utiliza en la base de datos para vincular las tablas `Person` y `Course` en una relación de varios a varios. Las propiedades de navegación permiten obtener entidades de `Course` relacionadas de la entidad `Person` y las entidades `Person` relacionadas de la entidad `Course`, por lo que no es necesario representar la tabla de asociación en el modelo de datos.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Para los fines de este tutorial, supongamos que la columna `FirstName` de la tabla `Person` contiene realmente el nombre y el segundo nombre de una persona. Desea cambiar el nombre del campo para que refleje esto, pero es posible que el administrador de la base de datos (DBA) no desee cambiar la base de datos. Puede cambiar el nombre de la propiedad `FirstName` en el modelo de datos, sin cambiar su equivalente en la base de datos.

En el diseñador, haga clic con el botón secundario en **FirstName** en la entidad `Person` y, a continuación, seleccione **cambiar nombre**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Escriba el nombre nuevo "FirstMidName". Esto cambia la manera en que se hace referencia a la columna en el código sin cambiar la base de datos.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

El explorador de modelos proporciona otra manera de ver la estructura de la base de datos, la estructura del modelo de datos y la asignación entre ellas. Para verlo, haga clic con el botón secundario en un área en blanco de Entity Designer y, a continuación, haga clic en **Explorador de modelos**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

El panel **Explorador de modelos** muestra una vista de árbol. (El panel **Explorador de modelos** se puede acoplar con el panel **Explorador de soluciones** ). El nodo **SchoolModel** representa la estructura del modelo de datos y el nodo **SchoolModel. Store** representa la estructura de la base de datos.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Expanda **SchoolModel. Store** para ver las tablas, expanda **tablas o vistas** para ver las tablas y, a continuación, expanda **Course** para ver las columnas de una tabla.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Expanda **SchoolModel**, expanda **tipos de entidad**y, a continuación, expanda el nodo **Course** para ver las entidades y las propiedades de las entidades.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

En el diseñador o en el panel **Explorador de modelos** , puede ver cómo el Entity Framework relaciona los objetos de los dos modelos. Haga clic con el botón secundario en la entidad `Person` y seleccione **asignación de tabla**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Se abrirá la ventana detalles de la **asignación** . Tenga en cuenta que esta ventana le permite ver que la columna de base de datos `FirstName` está asignada a `FirstMidName`, que es el nombre al que ha cambiado en el modelo de datos.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

El Entity Framework usa XML para almacenar información sobre la base de datos, el modelo de datos y las asignaciones entre ellos. El archivo *SchoolModel. edmx* es realmente un archivo XML que contiene esta información. El diseñador representa la información en un formato gráfico, pero también puede ver el archivo como XML; para ello, haga clic con el botón secundario en el archivo *. edmx* en **Explorador de soluciones**, haga clic en **abrir con**y seleccione **Editor XML (texto)** . (El diseñador de modelos de datos y un editor XML son solo dos formas diferentes de abrir y trabajar con el mismo archivo, por lo que no puede hacer que el diseñador abra y abrir el archivo en un editor XML al mismo tiempo).

Ahora ha creado un sitio web, una base de datos y un modelo de datos. En el siguiente tutorial, comenzará a trabajar con datos mediante el modelo de datos y el control ASP.NET `EntityDataSource`.

> [!div class="step-by-step"]
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-2.md)
