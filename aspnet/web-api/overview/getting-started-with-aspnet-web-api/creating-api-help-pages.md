---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creando páginas de ayuda para ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: En este tutorial con código se muestra cómo crear páginas de ayuda para ASP.NET Web API en ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448621"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a>Crear páginas de ayuda para ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este tutorial con código se muestra cómo crear páginas de ayuda para ASP.NET Web API en ASP.NET 4. x.

Al crear una API Web, a menudo resulta útil crear una página de ayuda, de modo que otros desarrolladores sepan cómo llamar a la API. Puede crear toda la documentación de forma manual, pero es mejor generarla lo máximo posible. Para facilitar esta tarea, ASP.NET Web API proporciona una biblioteca para la generación automática de páginas de ayuda en tiempo de ejecución.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Creación de páginas de ayuda de API

Instale [ASP.net and Web Tools actualización 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Esta actualización integra las páginas de ayuda en la plantilla de proyecto de Web API.

A continuación, cree un nuevo proyecto de ASP.NET MVC 4 y seleccione la plantilla de proyecto de API Web. La plantilla de proyecto crea un controlador de API de ejemplo denominado `ValuesController`. La plantilla también crea las páginas de ayuda de la API. Todos los archivos de código de la página de ayuda se colocan en la carpeta áreas del proyecto.

![](creating-api-help-pages/_static/image2.png)

Al ejecutar la aplicación, la Página principal contiene un vínculo a la página de ayuda de la API. En la Página principal, la ruta de acceso relativa es/Help.

![](creating-api-help-pages/_static/image3.png)

Este vínculo le lleva a una página de Resumen de la API.

![](creating-api-help-pages/_static/image4.png)

La vista de MVC de esta página se define en areas/HelpPage/views/help/index. cshtml. Puede editar esta página para modificar el diseño, la introducción, el título, los estilos, etc.

La parte principal de la página es una tabla de API, agrupadas por controlador. Las entradas de la tabla se generan dinámicamente, mediante la interfaz **IApiExplorer** . (Más adelante hablaré sobre esta interfaz). Si agrega un nuevo controlador de API, la tabla se actualiza automáticamente en tiempo de ejecución.

La columna "API" muestra el método HTTP y el URI relativo. La columna "Descripción" contiene documentación para cada API. Inicialmente, la documentación es simplemente texto de marcador de posición. En la siguiente sección, le mostraré cómo agregar documentación de comentarios XML.

Cada API tiene un vínculo a una página con información más detallada, incluidos los cuerpos de solicitud y respuesta de ejemplo.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Agregar páginas de ayuda a un proyecto existente

Puede agregar páginas de ayuda a un proyecto de API Web existente mediante el administrador de paquetes NuGet. Esta opción es útil para empezar a partir de una plantilla de proyecto diferente de la plantilla de "Web API".

En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana de la [consola del administrador de paquetes](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , escriba uno de los siguientes comandos:

Para una **C#** aplicación: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Para una aplicación **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Hay dos paquetes, uno para C# y otro para Visual Basic. Asegúrese de usar el que coincida con su proyecto.

Este comando instala los ensamblados necesarios y agrega las vistas de MVC para las páginas de ayuda (ubicadas en la carpeta areas/HelpPage). Tendrá que agregar manualmente un vínculo a la página de ayuda. El URI es/Help. Para crear un vínculo en una vista de Razor, agregue lo siguiente:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Además, asegúrese de registrar áreas. En el archivo global. asax, agregue el siguiente código al método de **Inicio de\_** de la aplicación, si no está ya presente:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Adición de documentación de API

De forma predeterminada, las páginas de ayuda de tienen cadenas de marcador de posición para la documentación de. Puede usar los [comentarios de documentación XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para crear la documentación. Para habilitar esta característica, abra las áreas de archivo/HelpPage/App\_Start/HelpPageConfig. CS y quite la marca de comentario de la siguiente línea:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Ahora, habilite la documentación XML. En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **propiedades**. Seleccione la página **compilación** .

![](creating-api-help-pages/_static/image6.png)

En **salida**, compruebe el **archivo de documentación XML**. En el cuadro de edición, escriba "App\_Data/XmlDocument. xml".

![](creating-api-help-pages/_static/image7.png)

A continuación, abra el código del controlador de API de `ValuesController`, que se define en/Controllers/ValuesController.cs. Agregue algunos comentarios de documentación a los métodos del controlador. Por ejemplo:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Sugerencia: Si coloca el símbolo de intercalación en la línea que está encima del método y escribe tres barras diagonales, Visual Studio inserta automáticamente los elementos XML. Después, puede rellenar los espacios en blanco.

Ahora vuelva a compilar y ejecutar la aplicación y vaya a las páginas de ayuda. Las cadenas de documentación deben aparecer en la tabla de API.

![](creating-api-help-pages/_static/image8.png)

La página de ayuda Lee las cadenas del archivo XML en tiempo de ejecución. (Al implementar la aplicación, asegúrese de implementar el archivo XML).

## <a name="under-the-hood"></a>Una mirada al interior

Las páginas de ayuda se compilan sobre la clase **ApiExplorer** , que forma parte del marco de la API Web. La clase **ApiExplorer** proporciona materias primas para crear una página de ayuda. Para cada API, **ApiExplorer** contiene una **ApiDescription** que describe la API. Para este propósito, una "API" se define como la combinación del método HTTP y el URI relativo. Por ejemplo, estas son algunas API distintas:

- OBTENER/api/Products
- GET /api/Products/{id}
- POST/api/Products

Si una acción de controlador admite varios métodos HTTP, el **ApiExplorer** trata cada método como una API distinta.

Para ocultar una API de **ApiExplorer**, agregue el atributo **ApiExplorerSettings** a la acción y establezca *IgnoreApi* en true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

También puede Agregar este atributo al controlador para excluir todo el controlador.

La clase ApiExplorer obtiene las cadenas de documentación de la interfaz **IDocumentationProvider** . Como vimos anteriormente, la biblioteca de páginas de ayuda proporciona un **IDocumentationProvider** que obtiene documentación de cadenas de documentación XML. El código se encuentra en/Areas/HelpPage/XmlDocumentationProvider.cs. Puede obtener documentación de otro origen escribiendo su propio **IDocumentationProvider**. Para conectarlo, llame al método de extensión **SetDocumentationProvider** , definido en **HelpPageConfigurationExtensions**

**ApiExplorer** llama automáticamente a la interfaz **IDocumentationProvider** para obtener cadenas de documentación para cada API. Los almacena en la propiedad **Documentation** de los objetos **ApiDescription** y **ApiParameterDescription** .

## <a name="next-steps"></a>Pasos siguientes

No está limitado a las páginas de ayuda que se muestran aquí. De hecho, **ApiExplorer** no se limita a crear páginas de ayuda. Yao Huangt ha escrito algunas excelentes entradas de blog para que pueda pensar de la caja:

- [Agregar un cliente de prueba simple a ASP.NET Web API página de ayuda](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Cómo funciona ASP.NET Web API página de ayuda en servicios autohospedados](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generación en tiempo de diseño de la página de ayuda (o cliente) para ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizaciones de la página de ayuda avanzada](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
