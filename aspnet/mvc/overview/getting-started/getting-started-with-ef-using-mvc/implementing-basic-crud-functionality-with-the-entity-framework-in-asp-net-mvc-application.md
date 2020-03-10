---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: 'Tutorial: implementación de la funcionalidad CRUD con la Entity Framework en ASP.NET MVC | Microsoft Docs'
description: Revise y personalice el código de creación, lectura, actualización y eliminación (CRUD) que el scaffolding de MVC crea automáticamente en los controladores y las vistas.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 9ed388543dd54d209ff2a0b92df4f7659962582c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471151"
---
# <a name="tutorial-implement-crud-functionality-with-the-entity-framework-in-aspnet-mvc"></a>Tutorial: implementación de la funcionalidad CRUD con la Entity Framework en ASP.NET MVC

En el [tutorial anterior](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), ha creado una aplicación MVC que almacena y muestra los datos mediante el Entity Framework (EF) 6 y SQL Server LocalDB. En este tutorial, revisará y personalizará el código de creación, lectura, actualización y eliminación (CRUD) que el scaffolding de MVC crea automáticamente en los controladores y vistas.

> [!NOTE]
> Es una práctica habitual implementar el modelo de repositorio con el fin de crear una capa de abstracción entre el controlador y la capa de acceso a datos. Para que estos tutoriales sean sencillos y se centren en la enseñanza de cómo usar EF 6, no usan repositorios. Para obtener información sobre cómo implementar repositorios, consulte el [mapa de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

Estos son algunos ejemplos de las páginas web que se crean:

![Captura de pantalla de la página de detalles del alumno.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Captura de pantalla de la página de creación de Student.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Captura de pantalla de la página de eliminación de estudiante.](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

En este tutorial va a:

> [!div class="checklist"]
> * Crear una página de detalles
> * Actualizar la página Create
> * Actualización del método de edición HttpPost
> * Actualizar la página Delete
> * Cerrar conexiones de bases de datos
> * Controlar transacciones

## <a name="prerequisites"></a>Requisitos previos

* [Crear el modelo de datos de Entity Framework](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

## <a name="create-a-details-page"></a>Crear una página de detalles

El código con scaffolding de la página Students `Index` dejó la propiedad `Enrollments`, porque esa propiedad contiene una colección. En la página `Details`, mostrará el contenido de la colección en una tabla HTML.

En *Controllers\StudentController.CS*, el método de acción de la vista `Details` usa el método [Find](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) para recuperar una sola entidad `Student`.

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

El valor de clave se pasa al método como `id` parámetro y procede de los *datos de ruta* del hipervínculo de **detalles** de la página de índice.

### <a name="tip-route-data"></a>Sugerencia: **datos de ruta**

Los datos de ruta son datos que el enlazador de modelos encontró en un segmento de dirección URL especificado en la tabla de enrutamiento. Por ejemplo, la ruta predeterminada especifica `controller`, `action`y `id` segmentos:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

En la dirección URL siguiente, la ruta predeterminada asigna `Instructor` como `controller`, `Index` como `action` y 1 como `id`; Estos son los valores de los datos de ruta.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

`?courseID=2021` es un valor de cadena de consulta. El enlazador de modelos también funcionará si pasa el `id` como un valor de cadena de consulta:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Las direcciones URL se crean mediante `ActionLink` instrucciones en la vista de Razor. En el código siguiente, el parámetro `id` coincide con la ruta predeterminada, por lo que `id` se agrega a los datos de ruta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

En el código siguiente, `courseID` no coincide con un parámetro en la ruta predeterminada, por lo que se agrega como una cadena de consulta.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]

### <a name="to-create-the-details-page"></a>Para crear la página de detalles

1. Abra *Views\Student\Details.cshtml*.

   Cada campo se muestra mediante una aplicación auxiliar de `DisplayFor`, como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]

2. Después del campo de `EnrollmentDate` y inmediatamente antes de la etiqueta de cierre de `</dl>`, agregue el código resaltado para mostrar una lista de inscripciones, como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Si la sangría de código no es correcta después de pegar el código, presione **ctrl**+**K**, **Ctrl**+**D** para darle formato.

    Este código recorre en bucle las entidades en la propiedad de navegación `Enrollments`. Para cada entidad `Enrollment` en la propiedad, muestra el título del curso y la calificación. El título del curso se recupera de la entidad `Course` almacenada en la propiedad de navegación `Course` de la entidad `Enrollments`. Todos estos datos se recuperan de la base de datos automáticamente cuando sea necesario. En otras palabras, se usa la carga diferida aquí. No especificó la *carga diligente* para la propiedad de navegación `Courses`, por lo que las inscripciones no se recuperaron en la misma consulta que obtuvieron los estudiantes. En su lugar, la primera vez que intenta tener acceso a la propiedad de navegación `Enrollments`, se envía una nueva consulta a la base de datos para recuperar los datos. Puede leer más información sobre la carga diferida y la carga diligente en el tutorial [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) más adelante en esta serie.

3. Abra el programa (**Ctrl**+**F5**) para abrir la página de detalles, seleccione la pestaña **Students** y, a continuación, haga clic en el vínculo **Details** para Alexander Carson. (Si presiona **Ctrl**+**F5** mientras el archivo *details. cshtml* está abierto, recibirá un error http 400. Esto se debe a que Visual Studio intenta ejecutar la página de detalles, pero no se ha alcanzado desde un vínculo que especifica el estudiante que se va a mostrar. Si esto ocurre, quite "Student/details" de la dirección URL e inténtelo de nuevo, o bien cierre el explorador, haga clic con el botón secundario en el proyecto y haga clic en **ver** > **vista en el explorador**).

    Verá la lista de cursos y calificaciones para el alumno seleccionado.

4. Cierre el explorador.

## <a name="update-the-create-page"></a>Actualizar la página Create

1. En *Controllers\StudentController.CS*, reemplace el método de acción `Create` <xref:System.Web.Mvc.HttpPostAttribute> por el código siguiente. Este código agrega un bloque `try-catch` y quita `ID` del atributo <xref:System.Web.Mvc.BindAttribute> para el método scaffolding:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Este código agrega la entidad `Student` creada por el enlazador de modelos MVC de ASP.NET al conjunto de entidades `Students` y, a continuación, guarda los cambios en la base de datos. El *enlazador de modelos* hace referencia a la funcionalidad de ASP.NET MVC que facilita el trabajo con los datos enviados por un formulario. un enlazador de modelos convierte los valores de formulario enviados a tipos CLR y los pasa al método de acción en los parámetros. En este caso, el enlazador de modelos crea una instancia de una entidad `Student` para usar los valores de propiedad de la colección `Form`.

    Quitó `ID` del atributo de enlace porque `ID` es el valor de clave principal que SQL Server establecerá automáticamente cuando se inserte la fila. La entrada del usuario no establece el valor de `ID`.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgery-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>ADVERTENCIA de seguridad: el atributo `ValidateAntiForgeryToken` ayuda a evitar los ataques [de falsificación de solicitudes entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) . Requiere una instrucción `Html.AntiForgeryToken()` correspondiente en la vista, que se verá más adelante.

    El atributo `Bind` es una manera de protegerse frente a la *publicación excesiva* en escenarios de creación. Por ejemplo, supongamos que la entidad `Student` incluye una propiedad `Secret` que no desea que establezca esta página web.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    Incluso si no tiene un campo de `Secret` en la página web, un pirata informático podría usar una herramienta como [Fiddler](http://fiddler2.com/home)o escribir algo de JavaScript para publicar un valor de `Secret` Form. Sin el atributo <xref:System.Web.Mvc.BindAttribute> que limita los campos que el enlazador de modelos usa cuando crea una instancia de `Student`<em>,</em> el enlazador de modelos tomará ese `Secret` valor de formulario y lo utilizará para crear la instancia de la entidad de `Student`. Después, el valor que el hacker haya especificado para el campo de formulario `Secret` se actualizaría en la base de datos. En la imagen siguiente se muestra la herramienta Fiddler agregando el campo `Secret` (con el valor "sobrepost") a los valores del formulario expuesto.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

    Después, el valor "OverPost" se agregaría correctamente a la propiedad `Secret` de la fila insertada, aunque no hubiera previsto que la página web pudiera establecer esa propiedad.

    Es mejor usar el parámetro `Include` con el atributo `Bind` en los campos de la *lista de permitidos* . También es posible usar el parámetro `Exclude` para los campos de *lista negra* que desee excluir. La razón `Include` es más segura es que cuando se agrega una nueva propiedad a la entidad, el nuevo campo no se protege automáticamente mediante una lista de `Exclude`.

    Para evitar la publicación en escenarios de edición, se lee primero la entidad de la base de datos y, a continuación, se llama a `TryUpdateModel`, pasando una lista de propiedades permitidas explícitas. Este es el método que se usa en estos tutoriales.

    Una manera alternativa de evitar la publicación más adecuada por muchos desarrolladores es usar los modelos de vista en lugar de las clases de entidad con el enlace de modelos. Incluya en el modelo de vista solo las propiedades que quiera actualizar. Una vez finalizado el enlazador de modelos de MVC, copie las propiedades del modelo de vista en la instancia de la entidad y, opcionalmente, use una herramienta como [automapper](http://automapper.org/). Use dB. Entrada en la instancia de la entidad para establecer su estado en sin cambios y, a continuación, establezca la propiedad ("PropertyName"). IsModified en true en cada propiedad de entidad incluida en el modelo de vista. Este método funciona tanto en escenarios de edición como de creación.

    Aparte del atributo `Bind`, el bloque de `try-catch` es el único cambio que se ha realizado en el código con scaffolding. Si se detecta una excepción derivada de <xref:System.Data.DataException> mientras se guardan los cambios, se muestra un mensaje de error genérico. En ocasiones, las excepciones <xref:System.Data.DataException> se deben a algo externo a la aplicación y no a un error de programación, por lo que se recomienda al usuario que vuelva a intentarlo. Aunque no se ha implementado en este ejemplo, en una aplicación de producción de calidad se debería registrar la excepción. Para obtener más información, vea la sección **Registro para obtener información** de [Supervisión y telemetría (creación de aplicaciones de nube reales con Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    El código de *Views\Student\Create.cshtml* es similar al que se vio en *details. cshtml*, salvo que se usan `EditorFor` y `ValidationMessageFor` aplicaciones auxiliares para cada campo en lugar de `DisplayFor`. Este es el código pertinente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create. cshtml* también incluye `@Html.AntiForgeryToken()`, que funciona con el `ValidateAntiForgeryToken` atributo en el controlador para evitar ataques de [falsificación de solicitudes entre sitios](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) .

    No se requieren cambios en *Create. cshtml*.

2. Ejecute la página iniciando el programa, seleccionando la pestaña **estudiantes** y haciendo clic en **crear nuevo**.

3. Escriba los nombres y una fecha no válida y haga clic en **crear** para ver el mensaje de error.

    Esta es la validación del lado servidor que se obtiene de forma predeterminada. En un tutorial posterior, verá cómo agregar atributos que generan código para la validación del lado cliente. En el código resaltado siguiente se muestra la comprobación de validación del modelo en el método **Create** .

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]

4. Cambie la fecha por un valor válido y haga clic en **Crear** para ver el alumno nuevo en la página **Index**.

5. Cierre el explorador.

## <a name="update-httppost-edit-method"></a>Actualizar el método de edición HttpPost

1. Reemplace el método de acción <xref:System.Web.Mvc.HttpPostAttribute> `Edit` por el código siguiente:

   [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

   > [!NOTE]
   > En *Controllers\StudentController.CS*, el método `HttpGet Edit` (el que no tiene el atributo `HttpPost`) usa el método `Find` para recuperar la entidad `Student` seleccionada, como se vio en el método `Details`. No es necesario cambiar este método.

   Estos cambios implementan un procedimiento recomendado de seguridad para evitar la [publicación](#overpost)excesiva, el scaffolding generó un atributo `Bind` y agregó la entidad creada por el enlazador de modelos al conjunto de entidades con una marca modificada. Ya no se recomienda el código porque el atributo `Bind` borra los datos preexistentes de los campos que no aparecen en el parámetro `Include`. En el futuro, el scaffolding del controlador de MVC se actualizará para que no genere `Bind` atributos para los métodos de edición.

   El nuevo código lee la entidad existente y llama <xref:System.Web.Mvc.Controller.TryUpdateModel%2A> para actualizar los campos de la entrada del usuario en los datos del formulario expuesto. El seguimiento de cambios automático del Entity Framework establece la marca [EntityState. Modified](<xref:System.Data.EntityState.Modified>) en la entidad. Cuando se llama al método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , la marca de <xref:System.Data.EntityState.Modified> hace que el Entity Framework cree instrucciones SQL para actualizar la fila de base de datos. Los [conflictos de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) se omiten y se actualizan todas las columnas de la fila de la base de datos, incluidas las que el usuario no ha cambiado. (Un tutorial posterior muestra cómo controlar los conflictos de simultaneidad y, si solo desea que los campos individuales se actualicen en la base de datos, puede establecer la entidad en [EntityState. Unchanged](<xref:System.Data.EntityState.Unchanged>) y establecer campos individuales en [EntityState. Modified](<xref:System.Data.EntityState.Modified>)).

   Para evitar la publicación excesiva, los campos que desea que se puedan actualizar en la página de edición aparecen en la lista de permitidos en el `TryUpdateModel` parámetros. Actualmente no se está protegiendo ningún campo adicional, pero enumerar los campos que quiere que el enlazador de modelos enlace garantiza que, si en el futuro agrega campos al modelo de datos, se protejan automáticamente hasta que los agregue aquí de forma explícita.

   Como resultado de estos cambios, la firma de método del método de edición HttpPost es igual que el método de edición HttpGet; por lo tanto, ha cambiado el nombre del método EditPost.

   > [!TIP]
   >
   > **Estados de la entidad y los métodos Attach y SaveChanges**
   >
   > El contexto de la base de datos realiza el seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos, y esta información determina lo que ocurre cuando se llama al método `SaveChanges`. Por ejemplo, cuando se pasa una nueva entidad al método [Add](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) , el estado de esa entidad se establece en `Added`. A continuación, cuando se llama al método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) , el contexto de la base de datos emite un comando de SQL `INSERT`.
   >
   > Una entidad puede estar en uno de los siguientes [Estados](xref:System.Data.EntityState):
   >
   > - `Added`Operador La entidad no existe todavía en la base de datos. El método `SaveChanges` debe emitir una instrucción `INSERT`.
   > - `Unchanged`Operador No es necesario hacer nada con esta entidad mediante el método `SaveChanges`. Al leer una entidad de la base de datos, la entidad empieza con este estado.
   > - `Modified`Operador Se han modificado algunos o todos los valores de propiedad de la entidad. El método `SaveChanges` debe emitir una instrucción `UPDATE`.
   > - `Deleted`Operador La entidad se ha marcado para su eliminación. El método `SaveChanges` debe emitir una instrucción `DELETE`.
   > - `Detached`Operador El contexto de base de datos no está realizando el seguimiento de la entidad.
   >
   > En una aplicación de escritorio, los cambios de estado normalmente se establecen de forma automática. En un tipo de aplicación de escritorio, Lee una entidad y realiza cambios en algunos de sus valores de propiedad. Esto hace que su estado de entidad cambie automáticamente a `Modified`. Después, al llamar a `SaveChanges`, el Entity Framework genera una instrucción SQL `UPDATE` que solo actualiza las propiedades reales que ha cambiado.
   >
   > La naturaleza desconectada de Web Apps no permite esta secuencia continua. El [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) que lee una entidad se elimina después de representar una página. Cuando se llama al método de acción de `Edit` `HttpPost`, se realiza una nueva solicitud y se tiene una nueva instancia de [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), por lo que debe establecer manualmente el estado de la entidad en `Modified.`, cuando se llama a `SaveChanges`, el Entity Framework actualiza todas las columnas de la fila de la base de datos, porque el contexto no tiene ninguna manera de saber qué propiedades ha cambiado.
   >
   > Si desea que la instrucción SQL `Update` actualice solo los campos que el usuario ha cambiado realmente, puede guardar los valores originales de alguna manera (como los campos ocultos) para que estén disponibles cuando se llame al método `Edit` `HttpPost`. Después, puede crear una entidad de `Student` con los valores originales, llamar al método `Attach` con esa versión original de la entidad, actualizar los valores de la entidad con los nuevos valores y, a continuación, llamar a `SaveChanges.` para obtener más información, consulte [Estados de entidad y SaveChanges](/ef/ef6/saving/change-tracking/entity-state) y [datos locales](/ef/ef6/querying/local-data).

   El código HTML y Razor en *Views\Student\Edit.cshtml* es similar al que vimos en *Create. cshtml*y no se requieren cambios.

2. Ejecute la página iniciando el programa, seleccionando la pestaña **Students** y haciendo clic en un hipervínculo de **edición** .

3. Cambie algunos de los datos y haga clic en **Guardar**. Verá los datos modificados en la página de índice.

4. Cierre el explorador.

## <a name="update-the-delete-page"></a>Actualizar la página Delete

En *Controllers\StudentController.CS*, el código de plantilla para el método `Delete` <xref:System.Web.Mvc.HttpGetAttribute> usa el método `Find` para recuperar la entidad `Student` seleccionada, como se vio en los métodos `Details` y `Edit`. Pero para implementar un mensaje de error personalizado cuando se produce un error en la llamada a `SaveChanges`, agregará funcionalidad a este método y su vista correspondiente.

Como se vio para las operaciones de actualización y creación, las operaciones de eliminación requieren dos métodos de acción. El método al que se llama en respuesta a una solicitud GET muestra una vista que proporciona al usuario una oportunidad para aprobar o cancelar la operación de eliminación. Si el usuario la aprueba, se crea una solicitud POST. Cuando esto sucede, se llama al método de `Delete` de `HttpPost` y, a continuación, ese método realiza realmente la operación de eliminación.

Agregará un bloque `try-catch` al método <xref:System.Web.Mvc.HttpPostAttribute> `Delete` para controlar los errores que puedan producirse cuando se actualice la base de datos. Si se produce un error, el método de `Delete` de <xref:System.Web.Mvc.HttpPostAttribute> llama al método de `Delete` <xref:System.Web.Mvc.HttpGetAttribute> y le pasa un parámetro que indica que se ha producido un error. A continuación, el método de `Delete` de <xref:System.Web.Mvc.HttpGetAttribute> vuelve a mostrar la página de confirmación junto con el mensaje de error, lo que proporciona al usuario una oportunidad para cancelar o volver a intentarlo.

1. Reemplace el método de acción de <xref:System.Web.Mvc.HttpGetAttribute> `Delete` por el código siguiente, que administra los informes de errores:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Este código acepta un [parámetro opcional](https://msdn.microsoft.com/library/dd264739.aspx) que indica si se llamó al método después de un error al guardar los cambios. Este parámetro se `false` cuando se llama al método `Delete` `HttpGet` sin un error anterior. Cuando lo llama el método `HttpPost` `Delete` en respuesta a un error de actualización de base de datos, el parámetro se `true` y se pasa un mensaje de error a la vista.

2. Reemplace el <xref:System.Web.Mvc.HttpPostAttribute> `Delete` método de acción (denominado `DeleteConfirmed`) por el código siguiente, que realiza la operación de eliminación real y detecta cualquier error de actualización de la base de datos.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

    Este código recupera la entidad seleccionada y, a continuación, llama al método [Remove](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) para establecer el estado de la entidad en `Deleted`. Cuando se llama a `SaveChanges`, se genera un comando SQL `DELETE`. También ha cambiado el nombre del método de acción de `DeleteConfirmed` a `Delete`. El código con scaffolding denominado `HttpPost` `Delete` método `DeleteConfirmed` para dar al método `HttpPost` una firma única. (CLR requiere que los métodos sobrecargados tengan parámetros de método diferentes). Ahora que las firmas son únicas, puede ceñirse a la Convención MVC y usar el mismo nombre para los métodos `HttpPost` y `HttpGet` DELETE.

    Si la mejora del rendimiento en una aplicación de gran volumen es una prioridad, puede evitar una consulta SQL innecesaria para recuperar la fila reemplazando las líneas de código que llaman a los métodos `Find` y `Remove` con el código siguiente:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

    Este código crea una instancia de una entidad `Student` utilizando únicamente el valor de clave principal y, a continuación, establece el estado de la entidad en `Deleted`. Eso es todo lo que necesita Entity Framework para eliminar la entidad.

    Como se indicó, el método de `Delete` `HttpGet` no elimina los datos. La realización de una operación de eliminación en respuesta a una solicitud GET (o, en ese caso, realizar cualquier operación de edición, operación de creación o cualquier otra operación que cambie los datos) crea un riesgo de seguridad. Para obtener más información, vea [ASP.NET MVC Tip #46: no use los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) en el blog de Stephen Walther.

3. En *Views\Student\Delete.cshtml*, agregue un mensaje de error entre el encabezado `h2` y el encabezado `h3`, como se muestra en el ejemplo siguiente:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

4. Ejecute la página iniciando el programa, seleccionando la pestaña **estudiantes** y haciendo clic en un hipervínculo **eliminar** .

5. Elija **eliminar** en la página que indica ¿está **seguro de que desea eliminarlo?** .

    La página de índice se muestra sin el estudiante eliminado. (Verá un ejemplo del código de control de errores en acción en el [tutorial de simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)).

## <a name="close-database-connections"></a>Cerrar conexiones de bases de datos

Para cerrar las conexiones de base de datos y liberar los recursos que contienen tan pronto como sea posible, elimine la instancia de contexto cuando haya terminado con ella. Esa es la razón por la que el código con scaffolding proporciona un método [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) al final de la clase `StudentController` en *StudentController.CS*, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

La clase de `Controller` base ya implementa la interfaz de `IDisposable`, por lo que este código simplemente agrega una invalidación al método `Dispose(bool)` para desechar explícitamente la instancia de contexto.

## <a name="handle-transactions"></a>Controlar transacciones

De forma predeterminada, Entity Framework implementa las transacciones de manera implícita. En escenarios en los que se realizan cambios en varias filas o tablas y, a continuación, se llama a `SaveChanges`, el Entity Framework se asegura automáticamente de que todos los cambios se realicen correctamente o se produzca un error. Si primero se realizan algunos cambios y después se produce un error, los cambios se revierten automáticamente. En escenarios en los que necesita más control&mdash;por ejemplo, si desea incluir operaciones realizadas fuera de Entity Framework en una transacción&mdash;vea [trabajar con transacciones](/ef/ef6/saving/transactions).

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Ahora tiene un conjunto completo de páginas que realizan operaciones CRUD simples para entidades `Student`. Usó aplicaciones auxiliares de MVC para generar elementos de interfaz de usuario para los campos de datos. Para obtener más información sobre las aplicaciones auxiliares de MVC, vea [representar un formulario mediante aplicaciones auxiliares HTML](/previous-versions/aspnet/dd410596(v=vs.98)) (el artículo es para MVC 3, pero sigue siendo pertinente para MVC 5).

Los vínculos a otros recursos de EF 6 se pueden encontrar en [ASP.net Data Access (recursos recomendados)](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Se creó una página de detalles
> * Actualizado la página Create
> * Se actualizó el método de edición HttpPost
> * Actualizado la página Delete
> * Cerrado conexiones de bases de datos
> * Transacciones controladas

Avance al siguiente artículo para aprender a agregar ordenación, filtrado y paginación al proyecto.
> [!div class="nextstepaction"]
> [Ordenar, filtrar y paginar](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
