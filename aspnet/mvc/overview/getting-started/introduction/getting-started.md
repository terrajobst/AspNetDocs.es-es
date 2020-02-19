---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introducción con MVC 5 de ASP.NET | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: c74daa37f68dda641cae97d3b0c19718f62d474d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456392"
---
# <a name="getting-started-with-aspnet-mvc-5"></a>Introducción a ASP.NET MVC 5

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

En este tutorial se enseñan los conceptos básicos de la creación de una aplicación web MVC 5 ASP.NET con [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). El código fuente final del tutorial se encuentra en [GitHub](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Este tutorial fue escrito por [Scott Guthrie](https://weblogs.asp.net/scottgu/) (Twitter[@scottgu](https://twitter.com/scottgu) ), [scott Hanselman](http://www.hanselman.com/blog/) (Twitter: [@shanselman](https://twitter.com/shanselman) ) y [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )

Necesita una cuenta de Azure para implementar esta aplicación en Azure:

- Puede [abrir una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.
- Puede [activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Su suscripción a MSDN le proporciona crédito todos los meses que puede utilizar para servicios de Azure de pago.

## <a name="get-started"></a>Introducción

Empiece por [instalar Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). A continuación, abra Visual Studio.

Visual Studio es un IDE o un entorno de desarrollo integrado. Del mismo modo que usa Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. En Visual Studio, hay una lista a lo largo de la parte inferior que muestra varias opciones disponibles. También hay un menú que proporciona otra manera de realizar tareas en el IDE. Por ejemplo, en lugar de seleccionar **nuevo proyecto** en la **página Inicio**, puede usar la barra de menús y seleccionar **archivo** > **nuevo proyecto**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Creación de la primera aplicación

En la **página Inicio**, seleccione **nuevo proyecto**. En el cuadro de diálogo **nuevo proyecto** , seleccione la categoría **Visual C#**  a la izquierda, después **Web**y, a continuación, seleccione la plantilla de proyecto **aplicación Web de ASP.net (.NET Framework)** . Asigne al proyecto el nombre "MvcMovie" y, después, elija **Aceptar**.

![](getting-started/_static/image2.png)

En el cuadro de diálogo **nueva aplicación Web de ASP.net** , elija **MVC** y, a continuación, elija **Aceptar**.

![](getting-started/_static/image3.png)

Visual Studio usó una plantilla predeterminada para el proyecto ASP.NET MVC que acaba de crear, por lo que tiene una aplicación en funcionamiento en este momento sin hacer nada. Se trata de una "Hola mundo" simple. Project, y es un buen lugar para iniciar la aplicación.

![](getting-started/_static/image4.png)

Pulse **F5** para iniciar la depuración. Cuando presione **F5**, Visual Studio se iniciará [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecutará la aplicación Web. A continuación, Visual Studio inicia un explorador y abre la Página principal de la aplicación. Tenga en cuenta que la barra de direcciones del explorador indica `localhost:port#` y no algo como `example.com`. Esto se debe a que `localhost` siempre apunta a su propio equipo local, que en este caso está ejecutando la aplicación que acaba de crear. Cuando Visual Studio ejecuta un proyecto Web, se usa un puerto aleatorio para el servidor Web. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

![](getting-started/_static/image5.png)

De forma predeterminada, esta plantilla predeterminada le proporciona `Home`, `Contact`y `About` páginas. En la imagen siguiente no se muestran los vínculos **Inicio**, **acerca**de y **contacto** . En función del tamaño de la ventana del explorador, es posible que tenga que hacer clic en el icono de navegación para ver estos vínculos.

![](getting-started/_static/image6.png)

La aplicación también proporciona compatibilidad para registrarse e iniciar sesión. El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre la aplicación ASP.NET MVC y cambie parte del código.

Para obtener una lista de los tutoriales actuales, consulte [los artículos recomendados de MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Vea esta aplicación en ejecución en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación Web activa? Puede implementar una versión completa de la aplicación en su cuenta de Azure simplemente haciendo clic en el botón siguiente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si aún no tiene una cuenta, utilice una de las siguientes opciones para crear una:

- [Abra una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) : su suscripción a Visual Studio le ofrece créditos cada mes que puede usar para servicios de Azure de pago.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
