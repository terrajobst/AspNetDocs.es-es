---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 7 | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 18d4b44c5e23fd6942c3adf48a33a5602e6df6d0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78488527"
---
# <a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introducción con Entity Framework 4,0 Database First y ASP.NET 4 formularios Web Forms, parte 7

por [Tom Dykstra](https://github.com/tdykstra)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante el Entity Framework 4,0 y Visual Studio 2010. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md) .

## <a name="using-stored-procedures"></a>Utilizar procedimientos almacenados

En el tutorial anterior, implementó un patrón de herencia de tabla por jerarquía. En este tutorial se muestra cómo usar procedimientos almacenados para obtener más control sobre el acceso a la base de datos.

El Entity Framework le permite especificar que debe utilizar procedimientos almacenados para el acceso a la base de datos. Para cualquier tipo de entidad, puede especificar un procedimiento almacenado que se utilizará para crear, actualizar o eliminar entidades de ese tipo. Después, en el modelo de datos, puede Agregar referencias a los procedimientos almacenados que puede usar para realizar tareas como la recuperación de conjuntos de entidades.

El uso de procedimientos almacenados es un requisito común para el acceso a la base de datos. En algunos casos, es posible que un administrador de bases de datos requiera que todo el acceso a la base de datos pase por procedimientos almacenados por motivos de seguridad. En otros casos, puede que desee crear lógica de negocios en algunos de los procesos que utiliza el Entity Framework al actualizar la base de datos. Por ejemplo, cada vez que se elimina una entidad, puede que desee copiarla en una base de datos de archivo. O, cada vez que se actualiza una fila, es posible que desee escribir una fila en una tabla de registro que registre quién realizó el cambio. Puede realizar estos tipos de tareas en un procedimiento almacenado al que se llama cada vez que el Entity Framework elimina una entidad o actualiza una entidad.

Como en el tutorial anterior, no se crearán páginas nuevas. En su lugar, cambiará la forma en que el Entity Framework tiene acceso a la base de datos para algunas de las páginas que ya ha creado.

En este tutorial, creará procedimientos almacenados en la base de datos para insertar entidades `Student` y `Instructor`. Los agregará al modelo de datos y especificará que los Entity Framework deberían utilizarlos para agregar `Student` y `Instructor` entidades a la base de datos. También creará un procedimiento almacenado que puede usar para recuperar entidades `Course`.

## <a name="creating-stored-procedures-in-the-database"></a>Crear procedimientos almacenados en la base de datos

(Si utiliza el archivo *School. MDF* del proyecto disponible para su descarga con este tutorial, puede omitir esta sección porque los procedimientos almacenados ya existen).

En **Explorador de servidores**, expanda *School. MDF*, haga clic con el botón secundario en **procedimientos almacenados**y seleccione **Agregar nuevo procedimiento almacenado**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copie las siguientes instrucciones SQL y péguelas en la ventana procedimiento almacenado, reemplazando el procedimiento almacenado esqueleto.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` entidades tienen cuatro propiedades: `PersonID`, `LastName`, `FirstName`y `EnrollmentDate`. La base de datos genera automáticamente el valor de identificador y el procedimiento almacenado acepta parámetros para los otros tres. El procedimiento almacenado devuelve el valor de la clave de registro de la nueva fila para que el Entity Framework pueda realizar un seguimiento de la versión de la entidad que mantiene en la memoria.

Guarde y cierre la ventana del procedimiento almacenado.

Cree un `InsertInstructor` procedimiento almacenado de la misma manera, mediante las siguientes instrucciones SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Cree también `Update` procedimientos almacenados para las entidades `Student` y `Instructor`. (La base de datos ya tiene un procedimiento almacenado `DeletePerson` que funcionará para las entidades `Instructor` y `Student`).

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

En este tutorial, asignará las tres funciones, INSERT, Update y DELETE, para cada tipo de entidad. La Entity Framework versión 4 le permite asignar solo una o dos de estas funciones a procedimientos almacenados sin asignar las demás, con una excepción: Si asigna la función de actualización pero no la función de eliminación, el Entity Framework producirá una excepción cuando intento de eliminar una entidad. En la versión 3,5 de Entity Framework, no tenía esta gran flexibilidad en la asignación de procedimientos almacenados: Si asigna una función, era necesario asignar los tres.

Para crear un procedimiento almacenado que lea en lugar de los datos de actualizaciones, cree uno que seleccione todas `Course` entidades, mediante las siguientes instrucciones SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Agregar los procedimientos almacenados al modelo de datos

Ahora, los procedimientos almacenados se definen en la base de datos, pero se deben agregar al modelo de datos para que estén disponibles para el Entity Framework. Abra *SchoolModel. edmx*, haga clic con el botón secundario en la superficie de diseño y seleccione **Actualizar modelo desde base de datos**. En la pestaña **Agregar** del cuadro de diálogo **Elija los objetos de base de datos** , expanda **procedimientos almacenados**, seleccione los procedimientos almacenados recién creados y el `DeletePerson` procedimiento almacenado y, a continuación, haga clic en **Finalizar**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Asignar los procedimientos almacenados

En el diseñador de modelos de datos, haga clic con el botón secundario en la entidad `Student` y seleccione **asignación de procedimiento almacenado**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Aparece la ventana detalles de la **asignación** , en la que puede especificar procedimientos almacenados que el Entity Framework debe usar para insertar, actualizar y eliminar entidades de este tipo.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Establezca la función **Insert** en **InsertStudent**. En la ventana se muestra una lista de parámetros de procedimientos almacenados, cada uno de los cuales debe estar asignado a una propiedad de entidad. Dos de estos se asignan automáticamente porque los nombres son los mismos. No hay ninguna propiedad de entidad denominada `FirstName`, por lo que debe seleccionar manualmente `FirstMidName` en una lista desplegable que muestra las propiedades de la entidad disponibles. (Esto se debe a que ha cambiado el nombre de la propiedad `FirstName` por `FirstMidName` en el primer tutorial).

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

En la misma ventana detalles de la **asignación** , asigne la función `Update` al procedimiento almacenado `UpdateStudent` (Asegúrese de especificar `FirstMidName` como el valor del parámetro para `FirstName`, como hizo con el `Insert` procedimiento almacenado) y la función `Delete` en el procedimiento almacenado `DeletePerson`.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Siga el mismo procedimiento para asignar los procedimientos almacenados de inserción, actualización y eliminación para los instructores a la entidad `Instructor`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

En el caso de los procedimientos almacenados que leen en lugar de actualizar datos, se usa la ventana **Explorador de modelos** para asignar el procedimiento almacenado al tipo de entidad que devuelve. En el diseñador de modelos de datos, haga clic con el botón secundario en la superficie de diseño y seleccione **Explorador de modelos**. Abra el nodo **SchoolModel. Store** y, a continuación, abra el nodo **procedimientos almacenados** . A continuación, haga clic con el botón secundario en el `GetCourses` procedimiento almacenado y seleccione **Agregar importación de función**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

En el cuadro de diálogo **Agregar importación de función** , en **devuelve una colección de** **entidades**seleccionadas y, a continuación, seleccione `Course` como el tipo de entidad devuelto. Cuando haya terminado, haga clic en **Aceptar**. Guarde y cierre el archivo *. edmx* .

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Usar procedimientos almacenados INSERT, Update y DELETE

Los procedimientos almacenados para insertar, actualizar y eliminar datos los utiliza el Entity Framework automáticamente después de agregarlos al modelo de datos y asignarlos a las entidades adecuadas. Ahora puede ejecutar la página *StudentsAdd. aspx* y cada vez que cree un estudiante nuevo, el Entity Framework utilizará el `InsertStudent` procedimiento almacenado para agregar la nueva fila a la tabla de `Student`.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Ejecute la página Students *. aspx* y el nuevo estudiante aparecerá en la lista.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Cambie el nombre para comprobar que la función de actualización funciona y, a continuación, elimine el estudiante para comprobar que funciona la función de eliminación.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Usar procedimientos almacenados Select

El Entity Framework no ejecuta automáticamente procedimientos almacenados como `GetCourses`y no se pueden usar con el control `EntityDataSource`. Para usarlos, se les llama desde el código.

Abra el archivo *InstructorsCourses.aspx.CS* . El método `PopulateDropDownLists` usa una consulta LINQ to Entities para recuperar todas las entidades Course para que pueda recorrer en bucle la lista y determinar a qué se asigna un instructor y cuáles están sin asignar:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Reemplácelo por el código siguiente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La página ahora usa el `GetCourses` procedimiento almacenado para recuperar la lista de todos los cursos. Ejecute la página para comprobar que funciona como antes.

(Es posible que las propiedades de navegación de las entidades recuperadas por un procedimiento almacenado no se rellenen automáticamente con los datos relacionados con dichas entidades, en función de la configuración predeterminada de `ObjectContext`. Para obtener más información, vea [cargar objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx) en MSDN Library).

En el siguiente tutorial, aprenderá a usar la funcionalidad de datos dinámicos para que sea más fácil programar y probar reglas de validación y formato de datos. En lugar de especificar en cada una de las reglas de página web como cadenas de formato de datos y si se requiere o no un campo, puede especificar dichas reglas en los metadatos del modelo de datos y se aplican automáticamente en todas las páginas.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-8.md)
