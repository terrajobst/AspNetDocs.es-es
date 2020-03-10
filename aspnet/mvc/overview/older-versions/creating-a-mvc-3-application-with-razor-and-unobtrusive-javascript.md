---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creación de una aplicación de MVC 3 con JavaScript discreto y discreto | Microsoft Docs
author: microsoft
description: La aplicación Web de ejemplo de lista de usuarios muestra lo sencillo que es crear aplicaciones ASP.NET MVC 3 mediante el motor de vistas de Razor. La aplicación de ejemplo s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435001"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Crear una aplicación de MVC 3 con Razor y JavaScript discreto

por [Microsoft](https://github.com/microsoft)

> La aplicación Web de ejemplo de lista de usuarios muestra lo sencillo que es crear aplicaciones ASP.NET MVC 3 mediante el motor de vistas de Razor. La aplicación de ejemplo muestra cómo usar el nuevo motor de vista de Razor con ASP.NET MVC versión 3 y Visual Studio 2010 para crear un sitio web de lista de usuarios ficticios que incluye funcionalidad como la creación, visualización, edición y eliminación de usuarios.
> 
> En este tutorial se describen los pasos que se realizaron para compilar la aplicación de ejemplo de lista de usuarios ASP.NET MVC 3. Hay disponible un proyecto de C# Visual Studio con y código fuente de VB para acompañar este tema: [Descargar](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). Si tiene alguna pregunta sobre este tutorial, publíquelos en el foro de [MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Información general

La aplicación que va a compilar es un sitio web de lista de usuarios simple. Los usuarios pueden escribir, ver y actualizar la información del usuario.

![Sitio de ejemplo](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

Puede descargar el proyecto de VB C# y completado [aquí](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Creación de la aplicación Web

Para iniciar el tutorial, abra Visual Studio 2010 y cree un nuevo proyecto con la plantilla *aplicación web MVC 3 ASP.net* . Asigne a la aplicación el nombre &quot;Mvc3Razor&quot;.

[![nuevo proyecto de MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

En el cuadro de diálogo **nuevo ASP.NET MVC 3 Project** , seleccione **aplicación de Internet**, seleccione el motor de vistas de Razor y, a continuación, haga clic en **Aceptar**.

![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

En este tutorial no se usará el proveedor de pertenencia a ASP.NET, por lo que puede eliminar todos los archivos asociados con el inicio de sesión y la pertenencia. En **Explorador de soluciones**, quite los siguientes archivos y directorios:

- *Controllers\AccountController*
- *Models\AccountModels*
- *\\Views\Shared _LogOnPartial*
- *Views\Account* (y todos los archivos de este directorio)

![SOLN exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Edite el archivo <em>\_layout. cshtml</em> y reemplace el marcado dentro del elemento `<div>` denominado `logindisplay` con el mensaje <em>&quot;</em>inicio de sesión deshabilitado&quot;. En el ejemplo siguiente se muestra el nuevo marcado:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Agregar el modelo

En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *modelos* , seleccione **Agregar**y, a continuación, haga clic en **clase**.

![Nueva clase MDL de usuario](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Asigne a la clase el nombre `UserModel`. Reemplace el contenido del archivo *UserModel* por el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La clase `UserModel` representa a los usuarios. Cada miembro de la clase se anota con el atributo [required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) del espacio de nombres [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) . Los atributos del espacio de nombres [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) proporcionan validación automática del lado cliente y del servidor para las aplicaciones Web.

Abra la clase `HomeController` y agregue una directiva `using` para poder tener acceso a las clases `UserModel` y `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Justo después de la declaración de `HomeController`, agregue el siguiente comentario y la referencia a una clase de `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La clase `Users` es un almacén de datos simplificado y en memoria que usará en este tutorial. En una aplicación real usaría una base de datos para almacenar información de usuario. Las primeras líneas del archivo `HomeController` se muestran en el ejemplo siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compile la aplicación para que el modelo de usuario estará disponible para el Asistente para scaffolding en el paso siguiente.

## <a name="creating-the-default-view"></a>Crear la vista predeterminada

El siguiente paso consiste en agregar un método de acción y una vista para mostrar los usuarios.

Elimine el archivo *Views\Home\Index* existente. Creará un nuevo archivo de *Índice* para mostrar los usuarios.

En la clase `HomeController`, reemplace el contenido del método `Index` por el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Haga clic con el botón secundario dentro del método `Index` y, a continuación, haga clic en **Agregar vista**.

![Agregar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Seleccione la opción **crear una vista fuertemente tipada** . En **clase de datos de vista**, seleccione **Mvc3Razor. Models. UserModel**. (Si no ve **Mvc3Razor. Models. UserModel** en el cuadro **Ver clase de datos** , debe compilar el proyecto). Asegúrese de que el motor de vista está establecido en **Razor**. Establezca **ver contenido** en **lista** y, a continuación, haga clic en **Agregar**.

![Agregar vista de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nueva vista scaffolding automáticamente los datos de usuario que se pasan a la vista `Index`. Examine el archivo *Views\Home\Index* recién generado. Los vínculos **crear nuevos**, **Editar**, **detalles**y **eliminar** no funcionan, pero el resto de la página es funcional. Ejecute la página. Verá una lista de usuarios.

![Página de índice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Abra el archivo *index. cshtml* y reemplace el marcado `ActionLink` de **Edit**, **Details**y **Delete** con el código siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

El nombre de usuario se utiliza como identificador para buscar el registro seleccionado en los vínculos de **edición**, **detalles**y **eliminación** .

## <a name="creating-the-details-view"></a>Crear la vista de detalles

El siguiente paso consiste en agregar un método de acción de `Details` y una vista para mostrar los detalles del usuario.

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Agregue el siguiente método de `Details` al controlador Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Haga clic con el botón derecho en el método `Details` y seleccione <strong>Agregar vista</strong>. Compruebe que el cuadro <strong>Ver clase de datos</strong> contiene <strong>Mvc3Razor. Models. UserModel</strong><em>.</em> Establezca <strong>ver contenido</strong> en <strong>detalles</strong> y, a continuación, haga clic en <strong>Agregar</strong>.

![Agregar vista de detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Ejecute la aplicación y seleccione un vínculo de detalles. El scaffolding automático muestra cada propiedad del modelo.

![Detalles](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Crear la vista de edición

Agregue el siguiente método de `Edit` al controlador Home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Agregue una vista como en los pasos anteriores, pero establezca **ver contenido** en **Editar**.

![Agregar vista de edición](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Ejecute la aplicación y edite el nombre y el apellido de uno de los usuarios. Si infringe cualquier restricción `DataAnnotation` que se haya aplicado a la clase `UserModel`, al enviar el formulario, verá errores de validación generados por el código del servidor. Por ejemplo, si cambia el nombre &quot;Ana&quot; a &quot;un&quot;, al enviar el formulario, se muestra el siguiente error en el formulario:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

En este tutorial, va a tratar el nombre de usuario como clave principal. Por lo tanto, no se puede cambiar la propiedad del nombre de usuario. En el archivo *Edit. cshtml* , justo después de la instrucción `Html.BeginForm`, establezca el nombre de usuario en un campo oculto. Esto hace que la propiedad se pase en el modelo. En el fragmento de código siguiente se muestra la ubicación de la instrucción `Hidden`:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Reemplace el `TextBoxFor` y `ValidationMessageFor` marcado para el nombre de usuario por una llamada a `DisplayFor`. El método `DisplayFor` muestra la propiedad como un elemento de solo lectura. En el siguiente ejemplo se muestra el marcado completado. Las llamadas `TextBoxFor` y `ValidationMessageFor` originales se comentan con los caracteres de comentario de inicio y de finalización de Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Habilitar la validación del lado cliente

Para habilitar la validación del lado cliente en ASP.NET MVC 3, debe establecer dos marcas y debe incluir tres archivos JavaScript.

Abra el archivo *Web. config* de la aplicación. Compruebe `that ClientValidationEnabled` y `UnobtrusiveJavaScriptEnabled` se establecen en true en la configuración de la aplicación. El siguiente fragmento del archivo *Web. config* raíz muestra la configuración correcta:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Si se establece `UnobtrusiveJavaScriptEnabled` en true, se habilita la validación de cliente discreta y discreta de Ajax. Cuando se usa la validación discreta, las reglas de validación se convierten en atributos HTML5. Los nombres de atributo HTML5 solo pueden contener letras minúsculas, números y guiones.

Si se establece `ClientValidationEnabled` en true, se habilita la validación del lado cliente. Al establecer estas claves en el archivo *Web. config* de la aplicación, se habilita la validación del cliente y el JavaScript discreto para toda la aplicación. También puede habilitar o deshabilitar esta configuración en vistas individuales o en métodos de controlador mediante el código siguiente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

También debe incluir varios archivos JavaScript en la vista representada. Una manera fácil de incluir JavaScript en todas las vistas es agregarlos al archivo *Views\Shared\\_Layout. cshtml* . Reemplace el elemento `<head>` del archivo *\_layout. cshtml* por el código siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

Los dos primeros scripts de jQuery se hospedan en Microsoft Ajax Content Delivery Network (CDN). Al aprovechar la red CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento del primer acierto de las aplicaciones.

Ejecute la aplicación y haga clic en un vínculo de edición. Vea el origen de la página en el explorador. El origen del explorador muestra muchos atributos con el formato `data-val` (para la validación de datos). Cuando está habilitada la validación de cliente y el JavaScript discreto, los campos de entrada con una regla de validación de cliente contienen el `data-val="true"` atributo para desencadenar la validación de cliente discreta. Por ejemplo, el campo `City` del modelo se redecoraba con el atributo [required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , lo que da como resultado el código HTML que se muestra en el ejemplo siguiente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Para cada regla de validación de cliente, se agrega un atributo que tiene el formulario `data-val-rulename="message"`. Con el ejemplo de campo `City` mostrado anteriormente, la regla de validación de cliente necesaria genera el `data-val-required` atributo y el mensaje &quot;el campo City es necesario&quot;. Ejecute la aplicación, edite uno de los usuarios y desactive el campo `City`. Al salir del campo, verá un mensaje de error de validación del lado cliente.

![Ciudad necesaria](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Del mismo modo, para cada parámetro de la regla de validación de cliente, se agrega un atributo que tiene el formato `data-val-rulename-paramname=paramvalue`. Por ejemplo, la propiedad `FirstName` se anota con el atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) y especifica una longitud mínima de 3 y una longitud máxima de 8. La regla de validación de datos denominada `length` tiene el nombre de parámetro `max` y el valor de parámetro 8. A continuación se muestra el código HTML que se genera para el campo `FirstName` cuando se edita uno de los usuarios:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Para obtener más información sobre la validación discreta del cliente, consulte la entrada sobre la [validación de cliente discreta en ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) en el blog de Brad Wilson.

> [!NOTE]
> En ASP.NET MVC 3 beta, a veces es necesario enviar el formulario para iniciar la validación del lado cliente. Esto puede cambiarse para la versión final.

## <a name="creating-the-create-view"></a>Crear la vista

El siguiente paso consiste en agregar un método de acción de `Create` y una vista para permitir al usuario crear un nuevo usuario. Agregue el siguiente método de `Create` al controlador Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Agregue una vista como en los pasos anteriores, pero establezca **ver contenido** en **crear**.

![Crear vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Ejecute la aplicación, seleccione el vínculo **crear** y agregue un nuevo usuario. El método `Create` aprovecha automáticamente la validación del lado cliente y del lado servidor. Intente escribir un nombre de usuario que contenga espacios en blanco, como &quot;Ben X&quot;. Al salir del campo Nombre de usuario, se muestra un error de validación del lado cliente (`White space is not allowed`).

## <a name="add-the-delete-method"></a>Agregar el método Delete

Para completar el tutorial, agregue el siguiente método de `Delete` al controlador Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Agregue una vista `Delete` como en los pasos anteriores, estableciendo **ver contenido** en **eliminar**.

![Eliminar vista](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

Ahora tiene una aplicación ASP.NET MVC 3 sencilla pero totalmente funcional con validación.
