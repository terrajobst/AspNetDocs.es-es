---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: agregar una vista de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447883"
---
# <a name="part-4-adding-an-admin-view"></a>Parte 4: adición de una vista de administración

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Agregar una vista de administración

Ahora se va a activar el lado del cliente y se agregará una página que puede consumir datos del controlador de administración. La página permitirá a los usuarios crear, editar o eliminar productos mediante el envío de solicitudes AJAX al controlador.

En Explorador de soluciones, expanda la carpeta Controllers y abra el archivo denominado HomeController.cs. Este archivo contiene un controlador de MVC. Agregue un método denominado `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

El método **HttpRouteUrl** crea el URI para la API Web y lo almacenamos en el contenedor de vistas para más adelante.

A continuación, coloque el cursor de texto dentro del método de acción `Admin`, haga clic con el botón derecho y seleccione **Agregar vista**. Se abrirá el cuadro de diálogo **Agregar vista** .

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

En el cuadro de diálogo **Agregar vista** , asigne a la vista el nombre "admin". Active la casilla de verificación **crear una vista fuertemente tipada**. En **clase de modelo**, seleccione "Product (ProductStore. Models)". Deje las demás opciones como valores predeterminados.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Al hacer clic en **Agregar** , se agrega un archivo denominado admin. cshtml en vistas/Inicio. Abra este archivo y agregue el siguiente código HTML. Este código HTML define la estructura de la página, pero aún no se ha conectado ninguna funcionalidad.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Crear un vínculo a la página de administración

En Explorador de soluciones, expanda la carpeta vistas y, a continuación, expanda la carpeta compartida. Abra el archivo denominado \_layout. cshtml. Busque el elemento **UL** con ID = "MENU" y un vínculo de acción para la vista de administración:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> En el proyecto de ejemplo, he realizado algunos cambios cosméticos, como reemplazar la cadena "su logotipo aquí". Estos no afectan a la funcionalidad de la aplicación. Puede descargar el proyecto y comparar los archivos.

Ejecute la aplicación y haga clic en el vínculo "admin" que aparece en la parte superior de la Página principal. La página de administración debería tener un aspecto similar al siguiente:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

En este momento, la página no hace nada. En la siguiente sección, usaremos Knockout. js para crear una interfaz de usuario dinámica.

## <a name="add-authorization"></a>Agregar autorización

La página de administración es actualmente accesible para cualquier persona que visite el sitio. Vamos a cambiar esto para restringir el permiso a los administradores.

Comience agregando un rol de administrador y un usuario administrador. En Explorador de soluciones, expanda la carpeta filters y abra el archivo denominado InitializeSimpleMembershipAttribute.cs. Busque el constructor de `SimpleMembershipInitializer`. Después de la llamada a **WebSecurity. InitializeDatabaseConnection**, agregue el código siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Se trata de una manera rápida y desfasada de agregar el rol "administrador" y crear un usuario para el rol.

En Explorador de soluciones, expanda la carpeta Controllers y abra el archivo HomeController.cs. Agregue el atributo **Authorize** al método `Admin`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Abra el archivo AdminController.cs y agregue el atributo **Authorize** a toda la clase `AdminController`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> Tanto MVC como la API Web definen ambos atributos de **autorización** , en diferentes espacios de nombres. MVC usa **System. Web. Mvc. AuthorizeAttribute**, mientras que la API Web usa **System. Web. http. AuthorizeAttribute**.

Ahora solo los administradores pueden ver la página de administración. Además, si envía una solicitud HTTP al controlador de administración, la solicitud debe contener una cookie de autenticación. De lo contrario, el servidor envía una respuesta HTTP 401 (no autorizado). Puede verlo en Fiddler mediante el envío de una solicitud GET a `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-3.md)
> [Siguiente](using-web-api-with-entity-framework-part-5.md)
