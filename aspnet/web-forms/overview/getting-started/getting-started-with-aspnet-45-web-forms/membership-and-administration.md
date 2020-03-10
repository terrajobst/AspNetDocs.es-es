---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Pertenencia y administración | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: ab00bc90bfc767d06e747be6dfb973245b5aae88
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439933"
---
# <a name="membership-and-administration"></a>Pertenencia y administración

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial se muestra cómo actualizar la aplicación de ejemplo Wingtip Toys para agregar un rol personalizado y usar ASP.NET Identity. También muestra cómo implementar una página de administración desde la que el usuario con un rol personalizado puede Agregar y quitar productos del sitio Web.

[ASP.net Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) es el sistema de pertenencia que se usa para compilar la aplicación Web ASP.net y está disponible en ASP.net 4,5. ASP.NET Identity se usa en la plantilla de proyecto Visual Studio 2013 Web Forms, así como las plantillas para la [aplicación de una sola página](../../../../single-page-application/index.md)de [ASP.NET MVC](../../../../mvc/index.md), [ASP.net web API](../../../../web-api/index.md)y ASP.net. También puede instalar específicamente el sistema ASP.NET Identity mediante NuGet cuando empiece con una aplicación Web vacía. Sin embargo, en esta serie de tutoriales, se usa el projecttemplate de **formularios Web Forms**, que incluye el sistema ASP.net Identity. ASP.NET Identity facilita la integración de datos de perfil específicos del usuario con los datos de la aplicación. Además, ASP.NET Identity permite elegir el modelo de persistencia para perfiles de usuario en la aplicación. Puede almacenar los datos en una base de datos SQL Server u otro almacén de datos, incluidos los almacenes de datos *NoSQL* , como las tablas Azure Storage de Windows.

Este tutorial se basa en el tutorial anterior titulado "Checkout and Payment with PayPal" en la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Temas que se abordarán:

- Cómo usar el código para agregar un rol personalizado y un usuario a la aplicación.
- Cómo restringir el acceso a la página y la carpeta de administración.
- Cómo proporcionar la navegación para el usuario que pertenece al rol personalizado.
- Cómo usar el enlace de modelos para rellenar un control [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) con categorías de productos.
- Cómo cargar un archivo en la aplicación Web mediante el control [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) .
- Cómo usar los controles de validación para implementar la validación de entrada.
- Cómo agregar y quitar productos de la aplicación.

## <a name="these-features-are-included-in-the-tutorial"></a>Estas características se incluyen en el tutorial:

- ASP.NET Identity
- Configuración y autorización
- Enlace de modelos
- Validación discreta

Los formularios Web Forms de ASP.NET proporcionan funcionalidades de pertenencia. Mediante el uso de la plantilla predeterminada, dispone de funcionalidad de pertenencia integrada que puede usar inmediatamente cuando se ejecuta la aplicación. En este tutorial se muestra cómo usar ASP.NET Identity para agregar un rol personalizado y asignar un usuario a ese rol. Obtendrá información sobre cómo restringir el acceso a la carpeta de administración. Agregará una página a la carpeta de administración que permite a un usuario con un rol personalizado agregar y quitar productos, así como obtener una vista previa de un producto una vez agregado.

## <a name="adding-a-custom-role"></a>Agregar un rol personalizado

Con ASP.NET Identity, puede Agregar un rol personalizado y asignar un usuario a ese rol mediante código.

1. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *lógica* y cree una nueva clase.
2. Asigne a la nueva clase el nombre *RoleActions.CS*.
3. Modifique el código para que aparezca de la manera siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. En **Explorador de soluciones**, abra el archivo *global.asax.CS* .
5. Modifique el archivo *global.asax.CS* agregando el código resaltado en amarillo para que aparezca de la manera siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Observe que `AddUserAndRole` está subrayado en rojo. Haga doble clic en el código AddUserAndRole.  
   Se subrayará la letra "A" al principio del método resaltado.
7. Mantenga el mouse sobre la letra "A" y haga clic en la interfaz de usuario que le permite generar un código auxiliar de método para el método `AddUserAndRole`. 

    ![Pertenencia y administración: generar código auxiliar de método](membership-and-administration/_static/image1.png)
8. Haga clic en la opción titulada:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Abra el archivo *RoleActions.CS* desde la carpeta *lógica* .  
   El método de `AddUserAndRole` se ha agregado al archivo de clase.
10. Modifique el archivo *RoleActions.CS* quitando el `NotImplementedException` y agregando el código resaltado en amarillo para que aparezca de la manera siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

En primer lugar, el código anterior establece un contexto de base de datos para la base de datos de pertenencia. La base de datos de pertenencia también se almacena como un archivo *. MDF* en la carpeta *App\_Data* . Podrá ver esta base de datos una vez que el primer usuario haya iniciado sesión en esta aplicación Web. 

> [!NOTE] 
> 
> Si desea almacenar los datos de pertenencia junto con los datos del producto, puede considerar la posibilidad de usar el mismo **DbContext** que usó para almacenar los datos del producto en el código anterior.

 La palabra clave *Internal* es un modificador de acceso para tipos (como clases) y miembros de tipo (como métodos o propiedades). Solo se puede tener acceso a los tipos o miembros internos en los archivos contenidos en el mismo ensamblado *(archivo. dll* ). Al compilar la aplicación, se crea un archivo de ensamblado *(. dll*) que contiene el código que se ejecuta al ejecutar la aplicación. 

Un objeto `RoleStore`, que proporciona la administración de roles, se crea basándose en el contexto de la base de datos.

> [!NOTE] 
> 
> Tenga en cuenta que cuando se crea el objeto de `RoleStore`, se usa un tipo de `IdentityRole` genérico. Esto significa que el `RoleStore` solo puede contener objetos `IdentityRole`. Además, mediante el uso de genéricos, los recursos de la memoria se administran mejor.

Después, el objeto de `RoleManager`, se crea basándose en el objeto `RoleStore` que acaba de crear. el objeto `RoleManager` expone una API relacionada con el rol que se puede usar para guardar automáticamente los cambios en el `RoleStore`. El `RoleManager` solo puede contener objetos `IdentityRole` porque el código utiliza el tipo genérico `<IdentityRole>`.

Llame al método `RoleExists` para determinar si el rol "canEdit" está presente en la base de datos de pertenencia. En caso contrario, se crea el rol.

La creación del objeto `UserManager` parece ser más complicada que el control `RoleManager`, pero es prácticamente igual. Simplemente se codifica en una línea en lugar de en varias. Aquí, el parámetro que está pasando crea instancias de como un nuevo objeto incluido en el paréntesis.

A continuación, cree el usuario "canEditUser" mediante la creación de un nuevo objeto de `ApplicationUser`. A continuación, si crea correctamente el usuario, agregue el usuario al nuevo rol.

> [!NOTE] 
> 
> El control de errores se actualizará durante el tutorial "control de errores de ASP.NET" más adelante en esta serie de tutoriales.

La próxima vez que se inicie la aplicación, el usuario denominado "canEditUser" se agregará como el rol denominado "canEdit" de la aplicación. Más adelante en este tutorial, iniciará sesión como el usuario "canEditUser" para mostrar las capacidades adicionales que agregará durante este tutorial. Para obtener detalles de la API sobre ASP.NET Identity, vea el [espacio de nombres Microsoft. Aspnet. Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Para obtener más información acerca de cómo inicializar el sistema ASP.NET Identity, vea [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Restringir el acceso a la página de administración

La aplicación de ejemplo Wingtip Toys permite que los usuarios anónimos y los usuarios que han iniciado sesión vean y compren productos. Sin embargo, el usuario que ha iniciado sesión y que tiene el rol personalizado "canEdit" puede tener acceso a una página restringida para poder agregar y quitar productos.

#### <a name="add-an-administration-folder-and-page"></a>Agregar una página y una carpeta de administración

Después, creará una carpeta denominada *admin* para el usuario "canEditUser" que pertenece al rol personalizado de la aplicación de ejemplo Wingtip Toys.

1. Haga clic con el botón derecho en el nombre del proyecto (**Wingtip Toys**) en **Explorador de soluciones** y seleccione **Agregar** -&gt; **nueva carpeta**.
2. Asigne un nombre a la nueva carpeta *admin*.
3. Haga clic con el botón secundario en la carpeta *admin* y seleccione **Agregar** -&gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
4. Seleccione el grupo plantillas <strong>Web</strong> de <strong>Visual C#</strong> -&gt; a la izquierda. En la lista central, seleccione <strong>Web Forms con la página maestra</strong>, asígnele el nombre <em>AdminPage. aspx</em><strong>y, a continuación, seleccione</strong> <strong>Agregar</strong>.
5. Seleccione el archivo *site. Master* como la página maestra y, a continuación, elija **Aceptar**.

#### <a name="add-a-webconfig-file"></a>Agregar un archivo Web. config

Al agregar un archivo *Web. config* a la carpeta *admin* , puede restringir el acceso a la página contenida en la carpeta.

1. Haga clic con el botón secundario en la carpeta *admin* y seleccione **Agregar** -&gt; **nuevo elemento**.  
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En la lista de plantillas C# web visuales, seleccione <strong>archivo de configuración Web</strong>en la lista central, acepte el nombre predeterminado de <em>Web. config</em><strong>y, a continuación, seleccione</strong> <strong>Agregar</strong>.
3. Reemplace el contenido XML del archivo *Web.config* por el siguiente:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Guarde el archivo *Web.config* . El archivo *Web. config* especifica que solo el usuario que pertenece al rol "canEdit" de la aplicación puede tener acceso a la página contenida en la carpeta *admin* .

### <a name="including-custom-role-navigation"></a>Incluir la navegación de roles personalizados

Para permitir que el usuario del rol "canEdit" personalizado navegue hasta la sección de administración de la aplicación, debe agregar un vínculo a la página *site. Master* . Solo los usuarios que pertenecen al rol "canEdit" podrán ver el vínculo de **Administrador** y acceder a la sección de administración.

1. En Explorador de soluciones, busque y abra la página *site. Master* .
2. Para crear un vínculo para el usuario con el rol "canEdit", agregue el marcado resaltado en amarillo al siguiente lista sin ordenar `<ul>` elemento de modo que la lista aparezca de la manera siguiente:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Abra el archivo *site.Master.CS* . Haga que el vínculo de **Administrador** solo sea visible para el usuario "canEditUser" agregando el código resaltado en amarillo al controlador de `Page_Load`. El controlador de `Page_Load` aparecerá de la siguiente manera:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Cuando se carga la página, el código comprueba si el usuario que ha iniciado sesión tiene el rol "canEdit". Si el usuario pertenece al rol "canEdit", el elemento span que contiene el vínculo a la página *AdminPage. aspx* (y, por consiguiente, el vínculo dentro del intervalo) se hace visible.

### <a name="enabling-product-administration"></a>Habilitar la administración de productos

Hasta ahora, ha creado el rol "canEdit" y ha agregado un usuario "canEditUser", una carpeta de administración y una página de administración. Ha establecido derechos de acceso para la página y la carpeta de administración, y ha agregado un vínculo de navegación para el usuario del rol "canEdit" a la aplicación. A continuación, agregará marcado a la página *AdminPage. aspx* y el código al archivo de código subyacente *AdminPage.aspx.CS* que permitirá al usuario con el rol "canEdit" agregar y quitar productos.

1. En **Explorador de soluciones**, abra el archivo *AdminPage. aspx* de la carpeta *admin* .
2. Reemplace el marcado existente por lo siguiente:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Después, abra el archivo de código subyacente *AdminPage.aspx.CS* haciendo clic con el botón secundario en *AdminPage. aspx* y haciendo clic en **Ver código**.
4. Reemplace el código existente en el archivo de código subyacente *AdminPage.aspx.CS* por el código siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

En el código que especificó para el archivo de código subyacente de *AdminPage.aspx.CS* , una clase llamada `AddProducts` realiza el trabajo real de agregar productos a la base de datos. Esta clase todavía no existe, por lo que la creará ahora.

1. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *lógica* y, a continuación, seleccione **Agregar** -&gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el grupo plantillas de **código** de **Visual C#**  -&gt; a la izquierda. A continuación, seleccione **clase**en la lista central y asígnele el nombre *AddProducts.CS*.   
   Se muestra el nuevo archivo de clase.
3. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

La página *AdminPage. aspx* permite al usuario que pertenece al rol "canEdit" agregar y quitar productos. Cuando se agrega un nuevo producto, se validan los detalles del producto y, a continuación, se escriben en la base de datos. El nuevo producto está disponible inmediatamente para todos los usuarios de la aplicación Web.

#### <a name="unobtrusive-validation"></a>Validación discreta

Los detalles del producto que el usuario proporciona en la página *AdminPage. aspx* se validan mediante controles de validación (`RequiredFieldValidator` y `RegularExpressionValidator`). Estos controles utilizan automáticamente la validación discreta. La validación discreta permite a los controles de validación usar JavaScript para la lógica de validación del lado cliente, lo que significa que la página no requiere un viaje al servidor que se va a validar. De forma predeterminada, la validación discreta se incluye en el archivo *Web. config* en función de la configuración siguiente:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Expresiones regulares

El precio del producto en la página *AdminPage. aspx* se valida mediante un control **RegularExpressionValidator** . Este control valida si el valor del control de entrada asociado (el cuadro de texto "AddProductPrice") coincide con el patrón especificado por la expresión regular. Una expresión regular es una notación de coincidencia de patrones que permite buscar y hacer coincidir rápidamente patrones de caracteres concretos. El control **RegularExpressionValidator** incluye una propiedad denominada `ValidationExpression` que contiene la expresión regular utilizada para validar la entrada de precios, como se muestra a continuación:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload (control)

Además de los controles de entrada y validación, ha agregado el control **FileUpload** a la página *AdminPage. aspx* . Este control proporciona la capacidad de cargar archivos. En este caso, solo se permiten cargar archivos de imagen. En el archivo de código subyacente (*AdminPage.aspx.CS*), cuando se hace clic en el `AddProductButton`, el código comprueba la propiedad `HasFile` del control **FileUpload** . Si el control tiene un archivo y se permite el tipo de archivo (basado en la extensión de archivo), la imagen se guarda en la carpeta *images* y en la carpeta *images/Thumbs* de la aplicación.

#### <a name="model-binding"></a>Enlace de modelos

Anteriormente en esta serie de tutoriales se usaba el enlace de modelos para rellenar un control **ListView** , un control **FormsView** , un control **GridView** y un control **DetailView** . En este tutorial, se usa el enlace de modelos para rellenar un control **DropDownList** con una lista de categorías de productos.

El marcado que agregó al archivo *AdminPage. aspx* contiene un control **DropDownList** denominado `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

El enlace de modelos se usa para rellenar este **DropDownList** estableciendo el atributo de `ItemType` y el atributo de `SelectMethod`. El atributo `ItemType` especifica que se utiliza el tipo de `WingtipToys.Models.Category` al rellenar el control. Este tipo se definió al principio de esta serie de tutoriales mediante la creación de la clase `Category` (que se muestra a continuación). La clase `Category` se encuentra en la carpeta *Models* dentro del archivo *Category.CS* .

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

El atributo `SelectMethod` del control **DropDownList** especifica que se usa el método `GetCategories` (que se muestra a continuación) que se incluye en el archivo de código subyacente (*AdminPage.aspx.CS*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Este método especifica que se usa una interfaz de `IQueryable` para evaluar una consulta en un tipo de `Category`. El valor devuelto se usa para rellenar el **DropDownList** en el marcado de la página (*AdminPage. aspx*).

El texto que se muestra para cada elemento de la lista se especifica estableciendo el atributo `DataTextField`. El atributo `DataTextField` usa la `CategoryName` de la clase `Category` (mostrada anteriormente) para mostrar cada categoría en el control **DropDownList** . El valor real que se pasa cuando se selecciona un elemento en el control **DropDownList** se basa en el atributo `DataValueField`. El atributo `DataValueField` se establece en el `CategoryID` como definido en la clase `Category` (se muestra arriba).

### <a name="how-the-application-will-work"></a>Cómo funcionará la aplicación

Cuando el usuario que pertenece al rol "canEdit" navega a la página por primera vez, el control `DropDownAddCategory`**DropDownList** se rellena como se describió anteriormente. El control `DropDownRemoveProduct`**DropDownList** también se rellena con productos que utilizan el mismo enfoque. El usuario que pertenece al rol "canEdit" selecciona el tipo de categoría y agrega los detalles del producto (**nombre**, **Descripción**, **precio**y **archivo de imagen**). Cuando el usuario que pertenece al rol "canEdit" hace clic en el botón **Agregar producto** , se desencadena el controlador de eventos `AddProductButton_Click`. El controlador de eventos `AddProductButton_Click` ubicado en el archivo de código subyacente (*AdminPage.aspx.CS*) comprueba el archivo de imagen para asegurarse de que coincide con los tipos de archivo permitidos *(. gif*, *. png*, *. JPEG*o *. jpg*). Después, el archivo de imagen se guarda en una carpeta de la aplicación de ejemplo Wingtip Toys. A continuación, se agrega el nuevo producto a la base de datos. Para realizar la adición de un nuevo producto, se crea una nueva instancia de la clase `AddProducts` y se denomina Products. La clase `AddProducts` tiene un método denominado `AddProduct`y el objeto Products llama a este método para agregar productos a la base de datos.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Si el código agrega correctamente el nuevo producto a la base de datos, la página se recarga con el valor de la cadena de consulta `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Cuando se recarga la página, la cadena de consulta se incluye en la dirección URL. Al volver a cargar la página, el usuario que pertenece al rol "canEdit" puede ver inmediatamente las actualizaciones en los controles **DropDownList** en la página *AdminPage. aspx* . Además, al incluir la cadena de consulta con la dirección URL, la página puede mostrar un mensaje de confirmación al usuario que pertenece al rol "canEdit".

Cuando se vuelve a cargar la página *AdminPage. aspx* , se llama al evento `Page_Load`.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

El controlador de eventos `Page_Load` comprueba el valor de la cadena de consulta y determina si se muestra un mensaje de operación correcta.

## <a name="running-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver cómo puede Agregar, eliminar y actualizar elementos en el carro de la compra. El total del carro de la compra reflejará el costo total de todos los artículos en el carro de la compra.

1. En Explorador de soluciones, presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
   El explorador se abre y muestra la página *default. aspx* .
2. Haga clic en el vínculo **iniciar sesión** en la parte superior de la página. 

    ![Pertenencia y administración: vínculo de inicio de sesión](membership-and-administration/_static/image2.png)

   Se muestra la página *login. aspx* .
3. Use el nombre de usuario y la contraseña siguientes:  
   Nombre de usuario: canEditUser@wingtiptoys.com  
   Contraseña: PA $ $word 1 

    ![Pertenencia y administración: Página de inicio de sesión](membership-and-administration/_static/image3.png)
4. Haga clic en el botón **iniciar sesión** situado cerca de la parte inferior de la página.
5. En la parte superior de la página siguiente, seleccione el vínculo **admin (Administrador** ) para navegar a la página *AdminPage. aspx* . 

    ![Pertenencia y administración: vínculo de administrador](membership-and-administration/_static/image4.png)
6. Para probar la validación de entrada, haga clic en el botón **Agregar producto** sin agregar los detalles del producto. 

    ![Pertenencia y administración: Página de administración](membership-and-administration/_static/image5.png)

   Observe que se muestran los mensajes de campo necesarios.
7. Agregue los detalles de un nuevo producto y, a continuación, haga clic en el botón **Agregar producto** . 

    ![Pertenencia y administración: agregar producto](membership-and-administration/_static/image6.png)
8. Seleccione **productos** en el menú de navegación superior para ver el nuevo producto que ha agregado. 

    ![Pertenencia y administración: Mostrar nuevo producto](membership-and-administration/_static/image7.png)
9. Haga clic en el vínculo **Administrador** para volver a la página de administración.
10. En la sección **quitar producto** de la página, seleccione el nuevo producto que ha agregado en **DropDownListBox**.
11. Haga clic en el botón **quitar producto** para quitar el nuevo producto de la aplicación. 

    ![Pertenencia y administración: quitar producto](membership-and-administration/_static/image8.png)
12. Seleccione **productos** en el menú de navegación superior para confirmar que se ha quitado el producto.
13. Haga clic en **Cerrar sesión** en el modo de administración existente.   
    Observe que el panel de navegación superior ya no muestra el elemento de menú **Administrador** .

## <a name="summary"></a>Resumen

En este tutorial, ha agregado un rol personalizado y un usuario que pertenece al rol personalizado, acceso restringido a la página y la carpeta de administración, y ha proporcionado la navegación para el usuario que pertenece al rol personalizado. Usó el enlace de modelos para rellenar un control **DropDownList** con datos. Ha implementado el control **FileUpload** y los controles de validación. Además, ha aprendido a agregar y quitar productos de una base de datos. En el siguiente tutorial, aprenderá a implementar el enrutamiento ASP.NET.

## <a name="additional-resources"></a>Recursos adicionales

[Web. config: elemento authorization](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Implementación de una aplicación de formularios Web Forms de ASP.NET segura con suscripción, OAuth y SQL Database en un sitio web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](checkout-and-payment-with-paypal.md)
> [Siguiente](url-routing.md)
