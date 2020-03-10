---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Uso de Web API con ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Tutorial con código paso a paso para agregar la API Web a una aplicación de formularios de ASP.NET para ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448537"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Usar Web API con Formularios Web Forms de ASP.NET

por [Mike Wasson](https://github.com/MikeWasson)

Este tutorial le guiará por los pasos necesarios para agregar la API Web a una aplicación de formularios Web Forms tradicional de ASP.NET en ASP.NET 4. x. 

## <a name="overview"></a>Información general

Aunque ASP.NET Web API está empaquetado con ASP.NET MVC, es fácil agregar API Web a una aplicación de formularios Web Forms de ASP.NET tradicional.

Para usar Web API en una aplicación de formularios Web Forms, hay dos pasos principales:

- Agregue un controlador de API Web que se derive de la clase **ApiController** .
- Agregue una tabla de rutas al método de **Inicio de\_** de la aplicación.

## <a name="create-a-web-forms-project"></a>Crear un proyecto de formularios Web Forms

Inicie Visual Studio y seleccione **nuevo proyecto** en la página de **Inicio** . O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **ASP.NET Web Forms Application**. Escriba un nombre para el proyecto y haga clic en **Aceptar**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Crear el modelo y el controlador

En este tutorial se usan las mismas clases de modelo y controlador que en el tutorial de [Introducción](tutorial-your-first-web-api.md) .

En primer lugar, agregue una clase de modelo. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar clase**. Asigne a la clase el nombre Product y agregue la siguiente implementación:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Después, agregue un controlador de API Web al proyecto., un *controlador* es el objeto que controla las solicitudes HTTP para la API Web.

En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto. Seleccione **Agregar nuevo elemento**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

En **plantillas instaladas**, expanda  **C# visual** y seleccione **Web**. A continuación, en la lista de plantillas, seleccione **clase de controlador de API Web**. Asigne al controlador el nombre "ProductsController" y haga clic en **Agregar**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

El Asistente para **Agregar nuevo elemento** creará un archivo denominado ProductsController.cs. Elimine los métodos incluidos en el asistente y agregue los siguientes métodos:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Para obtener más información sobre el código de este controlador, vea el tutorial de [Introducción](tutorial-your-first-web-api.md) .

## <a name="add-routing-information"></a>Agregar información de enrutamiento

A continuación, agregaremos una ruta de URI para que los URI del formulario &quot;&quot;/API/Products/se enruten al controlador.

En **Explorador de soluciones**, haga doble clic en global. asax para abrir el archivo de código subyacente global.asax.cs. Agregue la siguiente instrucción **using** .

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

A continuación, agregue el siguiente código al método de **Inicio de\_** de la aplicación:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Para obtener más información acerca de las tablas de enrutamiento, consulte [enrutamiento en ASP.net web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Agregar AJAX del lado cliente

Eso es todo lo que necesita para crear una API Web a la que puedan acceder los clientes. Ahora vamos a agregar una página HTML que usa jQuery para llamar a la API.

Asegúrese de que la página maestra (por ejemplo, *site. Master*) incluye un `ContentPlaceHolder` con `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Abra el archivo default. aspx. Reemplace el texto reutilizable que se encuentra en la sección contenido principal, como se muestra a continuación:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

A continuación, agregue una referencia al archivo de origen de jQuery en la sección `HeaderContent`:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Nota: puede agregar fácilmente la referencia de script arrastrando y colocando el archivo de **Explorador de soluciones** en la ventana del editor de código.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Debajo de la etiqueta de script de jQuery, agregue el siguiente bloque de script:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Cuando se carga el documento, este script realiza una solicitud AJAX a &quot;API/Products&quot;. La solicitud devuelve una lista de productos en formato JSON. El script agrega la información del producto a la tabla HTML.

Al ejecutar la aplicación, debe tener el siguiente aspecto:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
