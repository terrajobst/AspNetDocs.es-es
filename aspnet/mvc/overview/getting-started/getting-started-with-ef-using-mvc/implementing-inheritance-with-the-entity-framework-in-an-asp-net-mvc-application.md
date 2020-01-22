---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: implementación de la herencia con EF en una aplicación ASP.NET MVC 5'
description: En este tutorial se muestra cómo implementar la herencia en el modelo de datos.
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 73a01ed47b0935a1a9734c197377470defb1fe36
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519393"
---
# <a name="tutorial-implement-inheritance-with-ef-in-an-aspnet-mvc-5-app"></a>Tutorial: implementación de la herencia con EF en una aplicación ASP.NET MVC 5

En el tutorial anterior se controlan las excepciones de simultaneidad. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar la [herencia](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) para facilitar la [reutilización de código](http://en.wikipedia.org/wiki/Code_reuse). En este tutorial, cambiará las clases `Instructor` y `Student` para que deriven de una clase base `Person` que contenga propiedades como `LastName`, que son comunes tanto para los instructores como para los alumnos. No tendrá que agregar ni cambiar ninguna página web, sino que cambiará parte del código y esos cambios se reflejarán automáticamente en la base de datos.

En este tutorial ha:

> [!div class="checklist"]
> * Aprenda a asignar la herencia a la base de datos
> * Creación de la clase Person
> * Actualiza Instructor y Student
> * Agregar una persona al modelo
> * Crear y actualizar migraciones
> * Prueba la implementación
> * Implementar en Azure

## <a name="prerequisites"></a>Requisitos previos

* [Administrar la simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="map-inheritance-to-database"></a>Asigna la herencia a la base de datos

Las clases `Instructor` y `Student` del modelo de datos `School` tienen varias propiedades que son idénticas:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Imagine que quiere eliminar el código redundante de las propiedades que comparten las entidades `Instructor` y `Student`. O que quiere escribir un servicio que pueda dar formato a los nombres sin preocuparse de si el nombre es de un instructor o de un alumno. Puede crear una `Person` clase base que contenga solo esas propiedades compartidas y, a continuación, hacer que las entidades `Instructor` y `Student` hereden de esa clase base, como se muestra en la siguiente ilustración:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Esta estructura de herencia se puede representar de varias formas en la base de datos. Puede tener una tabla de `Person` que incluya información acerca de los estudiantes y los instructores en una sola tabla. Algunas de las columnas solo se pueden aplicar a los instructores (`HireDate`), algunas solo a los estudiantes (`EnrollmentDate`), algunas a (`LastName`, `FirstName`). Normalmente, tendría una columna *discriminadora* para indicar qué tipo representa cada fila. Por ejemplo, en la columna discriminadora podría aparecer "Instructor" para los instructores y "Student" para los alumnos.

![Tabla por hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Este patrón de generación de una estructura de herencia de entidades a partir de una tabla de base de datos única se denomina herencia de *tabla por jerarquía* (TPH).

Una alternativa consiste en hacer que la base de datos se parezca más a la estructura de herencia. Por ejemplo, podría tener solo los campos nombre en la tabla `Person` y tener tablas `Instructor` y `Student` independientes con los campos de fecha.

![Tabla por type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Este patrón de creación de una tabla de base de datos para cada clase de entidad se denomina herencia de *tabla por tipo* (TPT).

Y todavía otra opción pasa por asignar todos los tipos no abstractos a tablas individuales. Todas las propiedades de una clase, incluidas las propiedades heredadas, se asignan a columnas de la tabla correspondiente. Este patrón se denomina herencia de tabla por clase concreta (TPC). Si ha implementado la herencia de TPC para las clases `Person`, `Student`y `Instructor` como se mostró anteriormente, las tablas `Student` y `Instructor` tendrían un aspecto diferente después de implementar la herencia que antes.

Los patrones de herencia de TPC y TPH suelen ofrecer un mejor rendimiento en el Entity Framework que los patrones de herencia de TPT, ya que los patrones de TPT pueden dar lugar a consultas de combinación complejas.

Este tutorial muestra cómo implementar la herencia de TPH. TPH es el patrón de herencia predeterminado en el Entity Framework, por lo que todo lo que tiene que hacer es crear una clase `Person`, cambiar las clases `Instructor` y `Student` para que se deriven de `Person`, agregar la nueva clase a la `DbContext`y crear una migración. (Para obtener información sobre cómo implementar los otros patrones de herencia, consulte [asignación de la herencia de tabla por tipo (TPT)](https://msdn.microsoft.com/data/jj591617#2.5) y [asignación de la herencia de tabla por conjunto (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) en la documentación de MSDN Entity Framework).

## <a name="create-the-person-class"></a>Creación de la clase Person

En la carpeta *Models* , cree *Person.CS* y reemplace el código de plantilla por el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="update-instructor-and-student"></a>Actualiza Instructor y Student

Ahora, actualice *instructor.CS* y *Student.CS* para que hereden los valores de *Person.SC*.

En *instructor.CS*, derive la clase `Instructor` de la clase `Person` y quite los campos clave y nombre. El código tendrá un aspecto similar al ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Realice cambios similares en *Student.CS*. La clase `Student` será similar a la del ejemplo siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-person-to-the-model"></a>Agregar una persona al modelo

En *SchoolContext.CS*, agregue una propiedad `DbSet` para el tipo de entidad `Person`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Esto es todo lo que Entity Framework necesita para configurar la herencia de tabla por jerarquía. Como verá, cuando se actualice la base de datos, tendrá una tabla de `Person` en lugar de las tablas `Student` y `Instructor`.

## <a name="create-and-update-migrations"></a>Crear y actualizar migraciones

En la consola del administrador de paquetes (PMC), escriba el siguiente comando:

`Add-Migration Inheritance`

Ejecute el comando `Update-Database` en el PMC. El comando producirá un error en este momento porque tenemos datos existentes que las migraciones no saben cómo controlar. Obtendrá un mensaje de error similar al siguiente:

> *No se pudo quitar el objeto ' DBO. Instructor ' porque la restricción de clave externa hace referencia a él.*

Abra *migraciones\&lt; timestamp&gt;\_inheritance.CS* y reemplace el método `Up` por el código siguiente:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Este código se encarga de las siguientes tareas de actualización de la base de datos:

- Quita las restricciones de la clave externa y los índices que apuntan a la tabla Student.
- Cambia el nombre de la tabla Instructor por Person y realiza los cambios necesarios para que pueda almacenar datos de los alumnos:

    - Agrega EnrollmentDate que acepta valores NULL para alumnos.
    - Agrega la columna discriminadora para indicar si una fila es para un alumno o para un instructor.
    - Hace que HireDate admita un valor NULL, puesto que las filas de alumnos no dispondrán de fechas de contratación.
    - Agrega un campo temporal que se usará para actualizar las claves externas que apuntan a los alumnos. Al copiar estudiantes en la tabla Person, obtendrán nuevos valores de clave principal.
- Copia datos de la tabla Student a la tabla Person. Esto hace que los alumnos se asignen a nuevos valores de clave principal.
- Corrige los valores de clave externa correspondientes a los alumnos.
- Vuelve a crear las restricciones y los índices de la clave externa, pero ahora los dirige a la tabla Person.

(Si hubiera usado el GUID en lugar de un número entero como tipo de clave principal, los valores de la clave principal de alumno no tendrían que cambiar y algunos de estos pasos se podrían haber omitido).

Vuelva a ejecutar el comando `update-database`.

(En un sistema de producción, realizaría los cambios correspondientes en el método Down en caso de que alguna vez tuviera que usarlo para volver a la versión anterior de la base de datos. En este tutorial no usará el método Down).

> [!NOTE]
> Es posible obtener otros errores al migrar datos y realizar cambios en el esquema. Si obtiene errores de migración que no se pueden resolver, puede continuar con el tutorial cambiando la cadena de conexión en el archivo *Web. config* o eliminando la base de datos. El enfoque más sencillo es cambiar el nombre de la base de datos en el archivo *Web. config* . Por ejemplo, cambie el nombre de la base de datos a ContosoUniversity2 como se muestra en el ejemplo siguiente:
>
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
>
> Con una base de datos nueva, no hay ningún dato para migrar y es mucho más probable que el comando `update-database` se complete sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, vea [quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Si adopta este enfoque para continuar con el tutorial, omita el paso de implementación al final de este tutorial o realice la implementación en un nuevo sitio y base de datos. Si implementa una actualización en el mismo sitio en el que ya se ha implementado, EF recibirá el mismo error cuando ejecute las migraciones automáticamente. Si quiere solucionar un error de migración, el mejor recurso es uno de los foros de Entity Framework o StackOverflow.com.

## <a name="test-the-implementation"></a>Prueba la implementación

Ejecute el sitio y pruebe varias páginas. Todo funciona igual que antes.

En **Explorador de servidores,** expanda **Connections\SchoolContext de datos** y, a continuación, **tablas**y verá que las tablas **Student** e **instructor** se han reemplazado por una tabla **Person** . Expanda la tabla **Person** y verá que tiene todas las columnas que solía haber en las tablas **Student** e **instructor** .

Haga clic con el botón derecho en la tabla Person y después haga clic en **Mostrar datos de tabla** para ver la columna discriminadora.

En el diagrama siguiente se muestra la estructura de la nueva base de datos School:

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>Implementar en Azure

En esta sección, es necesario haber completado la sección implementación opcional de **la aplicación en Azure** de la [parte 3, ordenación, filtrado y paginación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) de esta serie de tutoriales. Si tuviera errores de migración que resolvió eliminando la base de datos del proyecto local, omita este paso. o bien, cree un nuevo sitio y una base de datos e implemente en el nuevo entorno.

1. En Visual Studio, haga clic con el botón derecho en el proyecto, en el **Explorador de soluciones** y seleccione **Publicar** en el menú contextual.

2. Haga clic en **Publicar**.

    La aplicación web se abre en el explorador predeterminado.

3. Pruebe la aplicación para comprobar que funciona.

    La primera vez que se ejecuta una página que tiene acceso a la base de datos, el Entity Framework ejecuta todas las migraciones `Up` métodos necesarios para actualizar la base de datos con el modelo de datos actual.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en el [acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obtener más información sobre esta y otras estructuras de herencia, consulte [patrón de herencia de TPT](https://msdn.microsoft.com/data/jj618293) y patrón de [herencia TPH](https://msdn.microsoft.com/data/jj618292) en MSDN. En el siguiente tutorial, aprenderá a controlar una serie de escenarios de Entity Framework relativamente avanzados.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Aprendido para asignar la herencia a la base de datos
> * Creado la clase Person
> * Actualizado Instructor y Student
> * Persona agregada al modelo
> * Migraciones creadas y actualizadas
> * Probado la implementación
> * Implementado en Azure

Vaya al siguiente artículo para obtener información acerca de los temas que son útiles para tener en cuenta cuando va más allá de los aspectos básicos del desarrollo de aplicaciones Web de ASP.NET que usan Code First de Entity Framework.
> [!div class="nextstepaction"]
> [Escenarios avanzados de Entity Framework](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
