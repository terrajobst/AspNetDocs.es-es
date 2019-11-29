---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementación de la funcionalidad CRUD básica con el Entity Framework en la aplicación ASP.NET MVC (2 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: eba146d2975e0dcf243facf7c205c4acfe40b6f6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595328"
---
# <a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementación de la funcionalidad CRUD básica con el Entity Framework en la aplicación ASP.NET MVC (2 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En el tutorial anterior, ha creado una aplicación MVC que almacena y muestra los datos mediante el Entity Framework y SQL Server LocalDB. En este tutorial, revisará y personalizará el código CRUD (crear, leer, actualizar, eliminar) que el scaffolding de MVC crea automáticamente en los controladores y las vistas.

> [!NOTE]
> Es una práctica habitual implementar el modelo de repositorio con el fin de crear una capa de abstracción entre el controlador y la capa de acceso a datos. Para simplificar estos tutoriales, no implementará un repositorio hasta un tutorial posterior de esta serie.

En este tutorial, creará las páginas web siguientes:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Crear una página de detalles

El código con scaffolding de la página Students `Index` dejó la propiedad `Enrollments`, porque esa propiedad contiene una colección. En la página `Details` mostrará el contenido de la colección en una tabla HTML.

 En *Controllers\StudentController.CS*, el método de acción de la vista `Details` usa el método `Find` para recuperar una única entidad `Student`. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 El valor de clave se pasa al método como `id` parámetro y procede de los datos de ruta del hipervínculo de **detalles** de la página de índice. 

1. Abra *Views\Student\Details.cshtml*. Cada campo se muestra mediante una aplicación auxiliar de `DisplayFor`, como se muestra en el ejemplo siguiente: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Después del campo de `EnrollmentDate` y inmediatamente antes de la etiqueta de cierre de `fieldset`, agregue código para mostrar una lista de inscripciones, como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Este código recorre en bucle las entidades en la propiedad de navegación `Enrollments`. Para cada entidad `Enrollment` en la propiedad, muestra el título del curso y la calificación. El título del curso se recupera de la entidad `Course` almacenada en la propiedad de navegación `Course` de la entidad `Enrollments`. Todos estos datos se recuperan de la base de datos automáticamente cuando sea necesario. (En otras palabras, se usa la carga diferida aquí. No especificó la *carga diligente* para la propiedad de navegación `Courses`, por lo que la primera vez que intente obtener acceso a esa propiedad, se enviará una consulta a la base de datos para recuperar los datos. Puede leer más información sobre la carga diferida y la carga diligente en el tutorial [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) más adelante en esta serie).
3. Ejecute la página. para ello, seleccione la pestaña **Students** y haga clic en un vínculo de **detalles** para Alexander Carson. Verá la lista de cursos y calificaciones para el alumno seleccionado:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Actualización de la página crear

1. En *Controllers\StudentController.CS*, reemplace el método de acción `HttpPost``Create` por el código siguiente para agregar un bloque de `try-catch` y el [atributo de enlace](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) al método con scaffolding: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Este código agrega la entidad `Student` creada por el enlazador de modelos MVC de ASP.NET al conjunto de entidades `Students` y, a continuación, guarda los cambios en la base de datos. (El*enlazador de modelos* hace referencia a la funcionalidad de ASP.NET MVC que facilita el trabajo con los datos enviados por un formulario; un enlazador de modelos convierte los valores de formulario enviados a tipos CLR y los pasa al método de acción en los parámetros. En este caso, el enlazador de modelos crea una instancia de una entidad `Student` para usar los valores de propiedad de la colección `Form`).

    El atributo `ValidateAntiForgeryToken` ayuda a evitar los ataques [de falsificación de solicitudes entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.cshtml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Ejecute la página. para ello, seleccione la pestaña **Students** y haga clic en **crear nuevo**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    La validación de datos funciona de forma predeterminada. Escriba los nombres y una fecha no válida y haga clic en **crear** para ver el mensaje de error.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    En el código resaltado siguiente se muestra la comprobación de validación del modelo.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Cambie la fecha por un valor válido como 9/1/2005 y haga clic en **crear** para ver que el nuevo estudiante aparece en la página de **Índice** .

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Actualización de la página Editar publicación

En *Controllers\StudentController.CS*, el método `Edit` `HttpGet` (el que no tiene el atributo `HttpPost`) utiliza el método `Find` para recuperar la entidad `Student` seleccionada, como se vio en el método `Details`. No es necesario cambiar este método.

Sin embargo, reemplace el método de acción `HttpPost` `Edit` por el código siguiente para agregar un bloque de `try-catch` y el [atributo de enlace](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Este código es similar a lo que vio en el método de `Create` de `HttpPost`. Sin embargo, en lugar de agregar la entidad creada por el enlazador de modelos al conjunto de entidades, este código establece una marca en la entidad que indica que se ha cambiado. Cuando se llama al método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , la marca [modificada](https://msdn.microsoft.com/library/system.data.entitystate.aspx) hace que el Entity Framework cree instrucciones SQL para actualizar la fila de la base de datos. Se actualizarán todas las columnas de la fila de base de datos, incluidas las que el usuario no ha cambiado, y se omiten los conflictos de simultaneidad. (Aprenderá a controlar la simultaneidad en un tutorial posterior de esta serie).

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Estados de la entidad y los métodos Attach y SaveChanges

El contexto de la base de datos realiza el seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos, y esta información determina lo que ocurre cuando se llama al método `SaveChanges`. Por ejemplo, cuando se pasa una nueva entidad al método [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , el estado de esa entidad se establece en `Added`. A continuación, cuando se llama al método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , el contexto de la base de datos emite un comando de SQL `INSERT`.

Una entidad puede estar en uno de los[siguientes Estados](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. La entidad no existe todavía en la base de datos. El método `SaveChanges` debe emitir una instrucción `INSERT`.
- `Unchanged`. No es necesario hacer nada con esta entidad mediante el método `SaveChanges`. Al leer una entidad de la base de datos, la entidad empieza con este estado.
- `Modified`. Se han modificado algunos o todos los valores de propiedad de la entidad. El método `SaveChanges` debe emitir una instrucción `UPDATE`.
- `Deleted`. La entidad se ha marcado para su eliminación. El método `SaveChanges` debe emitir una instrucción `DELETE`.
- `Detached`. El contexto de base de datos no está realizando el seguimiento de la entidad.

En una aplicación de escritorio, los cambios de estado normalmente se establecen de forma automática. En un tipo de aplicación de escritorio, Lee una entidad y realiza cambios en algunos de sus valores de propiedad. Esto hace que su estado de entidad cambie automáticamente a `Modified`. Después, al llamar a `SaveChanges`, el Entity Framework genera una instrucción SQL `UPDATE` que solo actualiza las propiedades reales que ha cambiado.

La naturaleza desconectada de Web Apps no permite esta secuencia continua. El [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) que lee una entidad se elimina después de representar una página. Cuando se llama al método de acción de `Edit` `HttpPost`, se realiza una nueva solicitud y se tiene una nueva instancia de [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), por lo que debe establecer manualmente el estado de la entidad en `Modified.`, cuando se llama a `SaveChanges`, el Entity Framework actualiza todas las columnas de la fila de la base de datos, porque el contexto no tiene ninguna manera de saber qué propiedades ha cambiado.

Si desea que la instrucción SQL `Update` actualice solo los campos que el usuario ha cambiado realmente, puede guardar los valores originales de alguna manera (como los campos ocultos) para que estén disponibles cuando se llame al método `Edit` `HttpPost`. Después, puede crear una entidad de `Student` con los valores originales, llamar al método `Attach` con esa versión original de la entidad, actualizar los valores de la entidad con los nuevos valores y, a continuación, llamar a `SaveChanges.` para obtener más información, consulte [Estados de entidad y SaveChanges](https://msdn.microsoft.com/data/jj592676) y [datos locales](https://msdn.microsoft.com/data/jj592872) en el centro para desarrolladores de datos de MSDN.

El código de *Views\Student\Edit.cshtml* es similar al que vio en *Create. cshtml*y no se requiere ningún cambio.

Para ejecutar la página, seleccione la pestaña **estudiantes** y, a continuación, haga clic en un hipervínculo de **edición** .

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Cambie algunos de los datos y haga clic en **Guardar**. Verá los datos modificados en la página de índice.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Actualización de la página eliminar

En *Controllers\StudentController.CS*, el código de plantilla para el método `Delete` `HttpGet` usa el método `Find` para recuperar la entidad `Student` seleccionada, como se vio en los métodos `Details` y `Edit`. Pero para implementar un mensaje de error personalizado cuando se produce un error en la llamada a `SaveChanges`, agregará funcionalidad a este método y su vista correspondiente.

Como se vio para las operaciones de actualización y creación, las operaciones de eliminación requieren dos métodos de acción. El método al que se llama en respuesta a una solicitud GET muestra una vista que proporciona al usuario una oportunidad para aprobar o cancelar la operación de eliminación. Si el usuario la aprueba, se crea una solicitud POST. Cuando esto sucede, se llama al método de `Delete` de `HttpPost` y, a continuación, ese método realiza realmente la operación de eliminación.

Agregará un bloque `try-catch` al método `HttpPost` `Delete` para controlar los errores que puedan producirse cuando se actualice la base de datos. Si se produce un error, el método de `Delete` de `HttpPost` llama al método de `Delete` `HttpGet` y le pasa un parámetro que indica que se ha producido un error. A continuación, el método `HttpGet Delete` vuelve a mostrar la página de confirmación junto con el mensaje de error, lo que proporciona al usuario una oportunidad para cancelar o volver a intentarlo.

1. Reemplace el método de acción de `HttpGet` `Delete` por el código siguiente, que administra los informes de errores: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Este código acepta un parámetro booleano [opcional](https://msdn.microsoft.com/library/dd264739.aspx) que indica si se llamó después de un error al guardar los cambios. Este parámetro se `false` cuando se llama al método `Delete` `HttpGet` sin un error anterior. Cuando lo llama el método `HttpPost` `Delete` en respuesta a un error de actualización de base de datos, el parámetro se `true` y se pasa un mensaje de error a la vista.
2. Reemplace el `HttpPost` `Delete` método de acción (denominado `DeleteConfirmed`) por el código siguiente, que realiza la operación de eliminación real y detecta cualquier error de actualización de la base de datos.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Este código recupera la entidad seleccionada y, a continuación, llama al método [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) para establecer el estado de la entidad en `Deleted`. Cuando se llama a `SaveChanges`, se genera un comando SQL `DELETE`. También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding denominado `HttpPost` `Delete` método `DeleteConfirmed` para dar al método `HttpPost` una firma única. (CLR requiere que los métodos sobrecargados tengan parámetros de método diferentes). Ahora que las firmas son únicas, puede ceñirse a la Convención MVC y usar el mismo nombre para los métodos `HttpPost` y `HttpGet` DELETE.

     Si la mejora del rendimiento en una aplicación de gran volumen es una prioridad, puede evitar una consulta SQL innecesaria para recuperar la fila reemplazando las líneas de código que llaman a los métodos `Find` y `Remove` con el código siguiente, tal como se muestra en el resaltado amarillo:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Este código crea una instancia de una entidad `Student` utilizando únicamente el valor de clave principal y, a continuación, establece el estado de la entidad en `Deleted`. Eso es todo lo que necesita Entity Framework para eliminar la entidad.

     Como se indicó, el método de `Delete` `HttpGet` no elimina los datos. La realización de una operación de eliminación en respuesta a una solicitud GET (o, en ese caso, realizar cualquier operación de edición, operación de creación o cualquier otra operación que cambie los datos) crea un riesgo de seguridad. Para obtener más información, vea [ASP.NET MVC Tip #46: no use los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) en el blog de Stephen Walther.
3. En *Views\Student\Delete.cshtml*, agregue un mensaje de error entre el encabezado `h2` y el encabezado `h3`, como se muestra en el ejemplo siguiente:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Ejecute la página. para ello, seleccione la pestaña **Students** y haga clic en un hipervínculo **Delete** :

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Haga clic en **Eliminar**. Se mostrará la página de índice sin el estudiante eliminado. (Verá un ejemplo del código de control de errores en acción en el tutorial de [control de simultaneidad](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) más adelante en esta serie).

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Asegurarse de que las conexiones de base de datos no quedan abiertas

Para asegurarse de que las conexiones de la base de datos están cerradas correctamente y de que se han liberado los recursos que contienen, debe ver que se ha desechado la instancia de contexto. Esa es la razón por la que el código con scaffolding proporciona un método [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) al final de la clase `StudentController` en *StudentController.CS*, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

La clase de `Controller` base ya implementa la interfaz de `IDisposable`, por lo que este código simplemente agrega una invalidación al método `Dispose(bool)` para desechar explícitamente la instancia de contexto.

## <a name="summary"></a>Resumen

Ahora tiene un conjunto completo de páginas que realizan operaciones CRUD simples para entidades `Student`. Usó aplicaciones auxiliares de MVC para generar elementos de interfaz de usuario para los campos de datos. Para obtener más información sobre las aplicaciones auxiliares de MVC, vea [representar un formulario mediante aplicaciones auxiliares de HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (la página es para MVC 3, pero sigue siendo relevante para MVC 4).

En el siguiente tutorial expandirá la funcionalidad de la página de índice agregando ordenación y paginación.

Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [Siguiente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
