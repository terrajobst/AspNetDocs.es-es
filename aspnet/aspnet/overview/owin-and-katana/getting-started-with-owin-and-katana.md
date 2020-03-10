---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introducción con OWIN y Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472447"
---
# <a name="getting-started-with-owin-and-katana"></a>Introducción a OWIN y Katana

por [Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface for .net (OWIN)](http://owin.org/) define una abstracción entre los servidores Web .net y las aplicaciones Web. Al desacoplar el servidor Web de la aplicación, OWIN facilita la creación de middleware para el desarrollo web de .NET. Además, OWIN facilita el traslado de aplicaciones web a otros hosts&#8212;, por ejemplo, autohospedado en un servicio de Windows u otro proceso.

OWIN es una especificación propiedad de la comunidad, no una implementación. El proyecto Katana es un conjunto de componentes OWIN de código abierto desarrollados por Microsoft. Para obtener información general sobre OWIN y Katana, vea [información general de Project Katana](an-overview-of-project-katana.md). En este artículo, entraremos en el código para comenzar.

En este tutorial se usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), pero también puede usar Visual Studio 2012. Algunos de los pasos son diferentes en Visual Studio 2012, que se indican a continuación.

## <a name="host-owin-in-iis"></a>Hospedar OWIN en IIS

En esta sección, hospedaremos OWIN en IIS. Esta opción ofrece la flexibilidad y la composición de una canalización OWIN junto con el conjunto de características de IIS. Con esta opción, la aplicación OWIN se ejecuta en la canalización de solicitudes ASP.NET.

En primer lugar, cree un nuevo proyecto de aplicación Web ASP.NET. (En Visual Studio 2012, use el tipo de proyecto aplicación Web vacía de ASP.NET).

![](getting-started-with-owin-and-katana/_static/image1.png)

En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** .

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Agregar paquetes NuGet

A continuación, agregue los paquetes de NuGet necesarios. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Agregar una clase de inicio

A continuación, agregue una clase de inicio OWIN. En Explorador de soluciones, haga clic con el botón derecho en el proyecto, seleccione **Agregar**y, a continuación, seleccione **nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **clase de inicio Owin**. Para obtener más información sobre la configuración de la clase startup, vea [detección de clases de inicio de OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Agregue el código siguiente al método `Startup1.Configuration`:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Este código agrega una parte simple de middleware a la canalización OWIN, implementada como una función que recibe una instancia de **Microsoft. OWIN. IOwinContext** . Cuando el servidor recibe una solicitud HTTP, la canalización OWIN invoca el middleware. El middleware establece el tipo de contenido de la respuesta y escribe el cuerpo de la respuesta.

> [!NOTE]
> La plantilla de la clase de inicio OWIN está disponible en Visual Studio 2013. Si usa Visual Studio 2012, solo tiene que agregar una nueva clase vacía denominada `Startup1`y pegar en el código siguiente:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Ejecutar la aplicación

Presione F5 para comenzar la depuración. Visual Studio abrirá una ventana del explorador para `http://localhost:*port*/`. La página debería tener un aspecto similar al siguiente:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Servicio OWIN de autohospedaje en una aplicación de consola

Es fácil convertir esta aplicación del hospedaje de IIS en un proceso personalizado. Con el hospedaje de IIS, IIS actúa como el servidor HTTP y como el proceso que hospeda el servicio. Con el autohospedaje, la aplicación crea el proceso y usa la clase **HttpListener** como servidor http.

En Visual Studio, cree una nueva aplicación de consola. En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Agregue al proyecto una clase de `Startup1` de la parte 1 de este tutorial. No es necesario modificar esta clase.

Implemente el método de `Main` de la aplicación como se indica a continuación.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Al ejecutar la aplicación de consola, el servidor empieza a escuchar `http://localhost:9000`. Si navega a esta dirección en un explorador Web, verá la página "Hello World".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Agregar diagnósticos OWIN

El paquete Microsoft. Owin. Diagnostics contiene software intermedio que detecta las excepciones no controladas y muestra una página HTML con los detalles del error. Esta página funciona de manera muy similar a la página de error ASP.NET, que a veces se denomina "[pantalla amarilla de muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Al igual que YSOD, la página de error Katana es útil durante el desarrollo, pero se recomienda deshabilitarla en modo de producción.

Para instalar el paquete de diagnósticos en el proyecto, escriba el siguiente comando en la ventana de la consola del administrador de paquetes:

`install-package Microsoft.Owin.Diagnostics –Pre`

Cambie el código del método `Startup1.Configuration` como se indica a continuación:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Ahora, use CTRL + F5 para ejecutar la aplicación sin depuración, de modo que Visual Studio no se interrumpa en la excepción. La aplicación se comporta igual que antes, hasta que se navega a `http://localhost/fail`, momento en el que la aplicación produce la excepción. El middleware de la página de error detectará la excepción y mostrará una página HTML con información sobre el error. Puede hacer clic en las fichas para ver las variables de la pila, la cadena de consulta, las cookies, el encabezado de solicitud y el entorno OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Pasos siguientes

- [Detección de la clase de inicio OWIN](owin-startup-class-detection.md)
- [Usar OWIN para autohospedar ASP.NET Web API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Usar OWIN para autohospedar Signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
