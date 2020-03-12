---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introducción a ASP.NET Web API 2 (C#)-ASP.net 4. x
author: MikeWasson
description: Tutorial con código. Use ASP.NET Web API para crear una API Web que devuelva una lista de productos.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084058"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a>Introducción a ASP.NET Web API 2 (C#)

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

En este tutorial, usará ASP.NET Web API para crear una API Web que devuelva una lista de productos.

HTTP no es solo para servir páginas Web. HTTP es también una plataforma eficaz para la creación de API que exponen servicios y datos. HTTP es simple, flexible y omnipresente. Casi cualquier plataforma que se pueda considerar tiene una biblioteca HTTP, por lo que los servicios HTTP pueden llegar a una amplia gama de clientes, incluidos exploradores, dispositivos móviles y aplicaciones de escritorio tradicionales.

ASP.NET Web API es un marco para crear API Web sobre la .NET Framework. 

## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- API Web 2

Consulte [creación de una API Web con ASP.net Core y Visual Studio para Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) para obtener una versión más reciente de este tutorial.

## <a name="create-a-web-api-project"></a>Creación de un proyecto de API Web

En este tutorial, usará ASP.NET Web API para crear una API Web que devuelva una lista de productos. La Página Web de front-end utiliza jQuery para mostrar los resultados.

![](tutorial-your-first-web-api/_static/image1.png)

Inicie Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** . O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web ASP.net**. Asigne al proyecto el nombre "ProductsApp" y haga clic en **Aceptar**.

![](tutorial-your-first-web-api/_static/image2.png)

En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** . En &quot;agregar carpetas y referencias principales para&quot;, consulte **API Web**. Haga clic en **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> También puede crear un proyecto de API Web con la plantilla de&quot; Web API de &quot;. La plantilla de API Web usa ASP.NET MVC para proporcionar páginas de ayuda de la API. Estoy usando la plantilla vacía para este tutorial porque quiero mostrar la API Web sin MVC. En general, no es necesario conocer ASP.NET MVC para usar Web API.

## <a name="adding-a-model"></a>Agregar un modelo

Un *modelo* es un objeto que representa los datos de la aplicación. ASP.NET Web API puede serializar automáticamente el modelo a JSON, XML o a algún otro formato y, a continuación, escribir los datos serializados en el cuerpo del mensaje de respuesta HTTP. Siempre que un cliente pueda leer el formato de serialización, puede deserializar el objeto. La mayoría de los clientes pueden analizar XML o JSON. Además, el cliente puede indicar el formato que desea estableciendo el encabezado Accept en el mensaje de solicitud HTTP.

Comencemos por crear un modelo simple que representa un producto.

Si el Explorador de soluciones no está visible, haga clic en el menú **Ver** y seleccione **Explorador de soluciones**. En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models. En el menú contextual, seleccione **Agregar** y, después, haga clic en **Clase**.

![](tutorial-your-first-web-api/_static/image4.png)

Asigne a la clase el nombre &quot;&quot;del producto. Agregue las siguientes propiedades a la clase `Product`.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Agregar un controlador

En la API Web, un *controlador* es un objeto que controla las solicitudes HTTP. Vamos a agregar un controlador que puede devolver una lista de productos o un único producto especificado por identificador.

> [!NOTE]
> Si ha usado ASP.NET MVC, ya está familiarizado con los controladores. Los controladores de API Web son similares a los controladores de MVC, pero heredan la clase **ApiController** en lugar de la clase de **controlador** .

En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta Controllers. Seleccione **Agregar** y, luego, **Controlador**.

![](tutorial-your-first-web-api/_static/image5.png)

En el cuadro de diálogo **Agregar scaffold**, seleccione **Controlador de API Web: en blanco**. Haga clic en **Agregar**.

![](tutorial-your-first-web-api/_static/image6.png)

En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre &quot;ProductsController&quot;. Haga clic en **Agregar**.

![](tutorial-your-first-web-api/_static/image7.png)

El scaffolding crea un archivo denominado ProductsController.cs en la carpeta Controllers.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> No es necesario colocar los controladores en una carpeta denominada Controllers. El nombre de la carpeta es simplemente una manera cómoda de organizar los archivos de código fuente.

Si este archivo no está abierto, haga doble clic en el archivo para abrirlo. Reemplace el código de este archivo por lo siguiente:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Para que el ejemplo sea sencillo, los productos se almacenan en una matriz fija dentro de la clase de controlador. Por supuesto, en una aplicación real, realizaría una consulta en una base de datos o usaría algún otro origen de datos externo.

El controlador define dos métodos que devuelven productos:

- El método `GetAllProducts` devuelve la lista completa de productos como un tipo de **&gt;de producto IEnumerable&lt;** .
- El método `GetProduct` busca un único producto por su identificador.

Eso es todo. Tiene una API Web en funcionamiento. Cada método en el controlador corresponde a uno o varios URI:

| Método de controlador | URI |
| --- | --- |
| GetAllProducts | /api/products |
| GetProduct | *identificador* de/API/Products/ |

En el caso del método `GetProduct`, el *identificador* del identificador URI es un marcador de posición. Por ejemplo, para obtener el producto con el identificador 5, el URI es `api/products/5`.

Para obtener más información sobre cómo la API Web enruta las solicitudes HTTP a los métodos de controlador, consulte [enrutamiento en ASP.net web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Llamar a la API Web con JavaScript y jQuery

En esta sección, agregaremos una página HTML que usa AJAX para llamar a la API Web. Usaremos jQuery para realizar las llamadas AJAX y también para actualizar la página con los resultados.

En Explorador de soluciones, haga clic con el botón derecho en el proyecto, seleccione **Agregar**y, a continuación, seleccione **nuevo elemento**.

![](tutorial-your-first-web-api/_static/image9.png)

En el cuadro de diálogo **Agregar nuevo elemento** , seleccione el nodo **Web** en **C#visual**y, a continuación, seleccione el elemento de **página HTML** . Asigne a la página el nombre &quot;&quot;index. html.

![](tutorial-your-first-web-api/_static/image10.png)

Reemplace todo el código de este archivo por lo siguiente:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Existen varias formas de obtener jQuery. En este ejemplo, usé la [red CDN de Microsoft Ajax](../../../ajax/cdn/overview.md). También puede descargarlo desde [http://jquery.com/](http://jquery.com/)y la plantilla de proyecto ASP.net "Web API" también incluye jQuery.

### <a name="getting-a-list-of-products"></a>Obtención de una lista de productos

Para obtener una lista de productos, envíe una solicitud HTTP GET a &quot;&quot;/API/Products.

La función [getJSON](http://api.jquery.com/jQuery.getJSON/) de jQuery envía una solicitud Ajax. La respuesta contiene una matriz de objetos JSON. La función `done` especifica una devolución de llamada a la que se llama si la solicitud se realiza correctamente. En la devolución de llamada, actualizamos el DOM con la información del producto.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Obtención de un producto por identificador

Para obtener un producto por identificador, envíe una solicitud HTTP GET a &quot;*ID* . de/API/Products/&quot;, donde *ID* es el identificador del producto.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Seguimos llamando `getJSON` para enviar la solicitud de AJAX, pero esta vez colocamos el identificador en el URI de solicitud. La respuesta de esta solicitud es una representación JSON de un único producto.

## <a name="running-the-application"></a>Ejecutar la aplicación

Presione F5 para iniciar la depuración de la aplicación. La página web debería tener un aspecto similar al siguiente:

![](tutorial-your-first-web-api/_static/image11.png)

Para obtener un producto por identificador, escriba el identificador y haga clic en buscar:

![](tutorial-your-first-web-api/_static/image12.png)

Si escribe un identificador no válido, el servidor devuelve un error HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Usar F12 para ver la solicitud y la respuesta HTTP

Cuando se trabaja con un servicio HTTP, puede ser muy útil ver los mensajes de solicitud y respuesta HTTP. Puede hacerlo con las herramientas de desarrollo F12 de Internet Explorer 9. En Internet Explorer 9, presione **F12** para abrir las herramientas. Haga clic en la pestaña **red** y presione **Iniciar captura**. Ahora vuelva a la página web y presione **F5** para volver a cargar la Página Web. Internet Explorer capturará el tráfico HTTP entre el explorador y el servidor Web. La vista resumen muestra todo el tráfico de red de una página:

![](tutorial-your-first-web-api/_static/image14.png)

Busque la entrada del URI relativo "API/Products/". Seleccione esta entrada y haga clic en **ir a la vista detallada**. En la vista de detalle, hay pestañas para ver los encabezados y cuerpos de solicitud y respuesta. Por ejemplo, si hace clic en la pestaña **encabezados de solicitud** , puede ver que el cliente solicitó &quot;&quot; Application/JSON en el encabezado Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Si hace clic en la pestaña cuerpo de respuesta, puede ver cómo se serializó la lista de productos en JSON. Otros exploradores tienen una funcionalidad similar. Otra herramienta útil es [Fiddler](http://www.fiddler2.com/fiddler2/), un proxy de depuración web. Puede usar Fiddler para ver el tráfico HTTP y también para crear solicitudes HTTP, lo que le proporciona un control total sobre los encabezados HTTP de la solicitud.

## <a name="see-this-app-running-on-azure"></a>Vea esta aplicación en ejecución en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación Web activa? Puede implementar una versión completa de la aplicación en su cuenta de Azure simplemente haciendo clic en el botón siguiente.

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si aún no tiene una cuenta, tiene las siguientes opciones:

- [Abra una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : su suscripción a MSDN le proporciona créditos todos los meses que puede usar para los servicios de Azure de pago.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener un ejemplo más completo de un servicio HTTP que admite acciones POST, PUT y DELETE y escrituras en una base de datos, consulte [uso de Web API 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Para obtener más información sobre cómo crear aplicaciones web fluidas y receptivas sobre un servicio HTTP, vea aplicación de una [sola página de ASP.net](../../../single-page-application/index.md).
- Para obtener información sobre cómo implementar un proyecto Web de Visual Studio en Azure App Service, consulte [creación de una aplicación Web de ASP.net en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
