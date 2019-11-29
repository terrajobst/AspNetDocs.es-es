---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementar la herencia con el Entity Framework en una aplicación ASP.NET MVC (8 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9507cba71b976825257cc9948d54f2651355959d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595317"
---
# <a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementar la herencia con el Entity Framework en una aplicación ASP.NET MVC (8 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En el tutorial anterior se controlan las excepciones de simultaneidad. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar la herencia para eliminar el código redundante. En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos. No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Herencia de tabla por jerarquía y de tabla por tipo

En la programación orientada a objetos, puede usar la herencia para facilitar el trabajo con clases relacionadas. Por ejemplo, las clases `Instructor` y `Student` del modelo de datos `School` comparten varias propiedades, lo que da como resultado código redundante:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`. Puede crear una `Person` clase base que contenga solo esas propiedades compartidas y, a continuación, hacer que las entidades `Instructor` y `Student` hereden de esa clase base, como se muestra en la siguiente ilustración:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Esta estructura de herencia se puede representar de varias formas en la base de datos. Puede tener una tabla de `Person` que incluya información acerca de los estudiantes y los instructores en una sola tabla. Algunas de las columnas solo se pueden aplicar a los instructores (`HireDate`), algunas solo a los estudiantes (`EnrollmentDate`), algunas a (`LastName`, `FirstName`). Normalmente, tendría una columna *discriminadora* para indicar qué tipo representa cada fila. Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.

![Tabla por hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Este patrón de generación de una estructura de herencia de entidades a partir de una tabla de base de datos única se denomina herencia de *tabla por jerarquía* (TPH).

Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia. Por ejemplo, podría tener solo los campos nombre en la tabla `Person` y tener tablas `Instructor` y `Student` independientes con los campos de fecha.

![Tabla por type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Este patrón de creación de una tabla de base de datos para cada clase de entidad se denomina herencia de *tabla por tipo* (TPT).

Los patrones de herencia TPH suelen ofrecer un mejor rendimiento en el Entity Framework que los patrones de herencia de TPT, ya que los patrones de TPT pueden dar lugar a consultas complejas de combinación. Este tutorial muestra cómo implementar la herencia de TPH. Para ello, realice los pasos siguientes:

- Cree una clase de `Person` y cambie las clases `Instructor` y `Student` para que se deriven de `Person`.
- Agregue código de asignación de modelo a base de datos a la clase de contexto de base de datos.
- Cambie `InstructorID` y `StudentID` referencias en el proyecto a `PersonID`.

## <a name="creating-the-person-class"></a>Crear la clase Person

 Nota: no podrá compilar el proyecto después de crear las clases siguientes hasta que actualice los controladores que usa estas clases. 

En la carpeta *Models* , cree *Person.CS* y reemplace el código de plantilla por el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

En *instructor.CS*, derive la clase `Instructor` de la clase `Person` y quite los campos clave y nombre. El código tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Realice cambios similares en *Student.CS*. La clase `Student` será similar a la del ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Agregar el tipo de entidad person al modelo

En *SchoolContext.CS*, agregue una propiedad `DbSet` para el tipo de entidad `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía. Como verá, cuando se vuelva a crear la base de datos, tendrá una tabla de `Person` en lugar de las tablas `Student` y `Instructor`.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Cambiar InstructorID y StudentID a PersonID

En *SchoolContext.CS*, en la instrucción de asignación de instructor-Course, cambie `MapRightKey("InstructorID")` a `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Este cambio no es necesario; simplemente cambia el nombre de la columna InstructorID en la tabla de combinación de varios a varios. Si dejó el nombre como InstructorID, la aplicación seguiría funcionando correctamente. Este es el *SchoolContext.CS*completado:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

A continuación, debe cambiar `InstructorID` a `PersonID` y `StudentID` a `PersonID` en todo el proyecto, ***excepto*** en los archivos de migración con marca de tiempo de la carpeta *Migrations* . Para ello, encontrará y abrirá solo los archivos que se deben cambiar y, a continuación, realizará un cambio global en los archivos abiertos. El único archivo de la carpeta *Migrations* que debe cambiar es *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Empiece por cerrar todos los archivos abiertos en Visual Studio.
2. Haga clic en **Buscar y reemplazar: Buscar todos los archivos** en el menú **edición** y busque todos los archivos del proyecto que contengan `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Abra cada archivo en la ventana **resultados** de la búsqueda ***excepto*** en la &lt;marca de tiempo&gt; *\_. CS* en la carpeta *Migrations* ; para ello, haga doble clic en una línea para cada archivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Abra el cuadro **de diálogo reemplazar en archivos** y cambie **Buscar en** **todos los documentos abiertos**.
5. Use el cuadro **de diálogo reemplazar en archivos** para cambiar todos los `InstructorID` a `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Busque todos los archivos del proyecto que contengan `StudentID`.
7. Abra cada archivo en la ventana **resultados** de la búsqueda ***excepto*** en la &lt;marca de tiempo&gt;\_los archivos de migración *\*. CS* en la carpeta *Migrations* ; para ello, haga doble clic en una línea para cada archivo.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Abra el cuadro **de diálogo reemplazar en archivos** y cambie **Buscar en** **todos los documentos abiertos**.
9. Utilice el cuadro **de diálogo reemplazar en archivos** para cambiar todos los `StudentID` a `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Generar el proyecto.

(Tenga en cuenta que esto muestra una *desventaja* del patrón de `classnameID` para asignar nombres a las claves principales. Si hubiera llamado ID. de clave principal sin prefijar el nombre de clase, *no* sería necesario cambiar el nombre ahora).

## <a name="create-and-update-a-migrations-file"></a>Crear y actualizar un archivo de migraciones

En la consola del administrador de paquetes (PMC), escriba el siguiente comando:

`Add-Migration Inheritance`

Ejecute el comando `Update-Database` en el PMC. El comando producirá un error en este momento porque tenemos datos existentes que las migraciones no saben cómo controlar. Obtiene el siguiente error:

*La instrucción ALTER TABLE está en conflicto con la restricción FOREIGN KEY "FK\_DBO. Departamento\_DBO. Person\_PersonID ". El conflicto se produjo en la base de datos "ContosoUniversity", tabla "dbo. Person ", columna" PersonID ".*

Abra *migraciones\&lt; timestamp&gt;\_inheritance.CS* y reemplace el método `Up` por el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Vuelva a ejecutar el comando `update-database`.

> [!NOTE]
> Es posible obtener otros errores al migrar datos y realizar cambios en el esquema. Si obtiene errores de migración que no se pueden resolver, puede continuar con el tutorial cambiando la cadena de conexión en el archivo *Web. config* o eliminando la base de datos. El enfoque más sencillo es cambiar el nombre de la base de datos en el archivo *Web. config* . Por ejemplo, cambie el nombre de la base de datos a CU\_test como se muestra en el ejemplo siguiente:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Con una base de datos nueva, no hay ningún dato para migrar y es mucho más probable que el comando `update-database` se complete sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, vea [quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si adopta este enfoque para continuar con el tutorial, omita el paso de implementación que se encuentra al final de este tutorial, ya que el sitio implementado recibirá el mismo error cuando ejecute migraciones automáticamente. Si quiere solucionar un error de migración, el mejor recurso es uno de los foros de Entity Framework o StackOverflow.com.

## <a name="testing"></a>Pruebas

Ejecute el sitio y pruebe varias páginas. Todo funciona igual que antes.

En **Explorador de servidores,** expanda **SchoolContext** y, a continuación, **tablas**y verá que las tablas **Student** e **instructor** se han reemplazado por una tabla **Person** . Expanda la tabla **Person** y verá que tiene todas las columnas que solía haber en las tablas **Student** e **instructor** .

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

En el diagrama siguiente se muestra la estructura de la nueva base de datos School:

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Resumen

La herencia de tabla por jerarquía se ha implementado ahora para las clases `Person`, `Student`y `Instructor`. Para obtener más información sobre esta y otras estructuras de herencia, consulte herencia de las [estrategias de asignación](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) en el blog de Morteza manavi. En el siguiente tutorial, verá algunas maneras de implementar el repositorio y los patrones de unidad de trabajo.

Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
