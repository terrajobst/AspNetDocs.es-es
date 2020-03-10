---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Aplicaciones de Signalr de pruebas unitarias | Microsoft Docs
author: bradygaster
description: En este artículo se describe cómo usar las características de pruebas unitarias de Signalr 2,0.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: 2cf2e88f141d89971439dc1fc4979849f8dded47
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467305"
---
# <a name="unit-testing-signalr-applications"></a>Pruebas unitarias de aplicaciones de SignalR

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe el uso de las características de pruebas unitarias de Signalr 2.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2
>
>
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Aplicaciones de Signalr de pruebas unitarias

Puede usar las características de pruebas unitarias de Signalr 2 para crear pruebas unitarias para la aplicación Signalr. Signalr 2 incluye la interfaz [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) , que se puede usar para crear un objeto ficticio para simular los métodos de concentrador para las pruebas.

En esta sección, agregará pruebas unitarias para la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md) con [xUnit.net](https://github.com/xunit/xunit) y [MOQ](https://github.com/Moq/moq4).

XUnit.net se usará para controlar la prueba; MOQ se usará para crear un objeto [ficticio](http://en.wikipedia.org/wiki/Mock_object) para las pruebas. Si lo desea, puede usar otros marcos ficticios. [NSubstitute](http://nsubstitute.github.io/) también es una buena elección. En este tutorial se muestra cómo configurar el objeto ficticio de dos maneras: en primer lugar, mediante un objeto `dynamic` (introducido en .NET Framework 4) y el segundo, mediante una interfaz.

### <a name="contents"></a>Contenido

Este tutorial contiene las siguientes secciones.

- [Pruebas unitarias con Dynamic](#dynamic)
- [Pruebas unitarias por tipo](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Pruebas unitarias con Dynamic

En esta sección, agregará una prueba unitaria para la aplicación creada en el [Introducción tutorial](../getting-started/tutorial-getting-started-with-signalr.md) con un objeto dinámico.

1. Instale la [extensión xUnit Runner](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) para Visual Studio 2013.
2. Complete el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md)o descargue la aplicación completada de la galería de [código de MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Si usa la versión de descarga de la aplicación Introducción, abra la **consola del administrador de paquetes** y haga clic en **restaurar** para agregar el paquete de signalr al proyecto.

    ![Restaurar paquetes](unit-testing-signalr-applications/_static/image1.png)
4. Agregue un proyecto a la solución para la prueba unitaria. Haga clic con el botón derecho en la solución en **Explorador de soluciones** y seleccione **Agregar**, **nuevo proyecto..** .. En el **C#** nodo, seleccione el nodo **Windows** . Seleccione **biblioteca de clases**. Asigne al nuevo proyecto el nombre **TestLibrary** y haga clic en **Aceptar**.

    ![Crear biblioteca de pruebas](unit-testing-signalr-applications/_static/image2.png)
5. Agregue una referencia en el proyecto de biblioteca de pruebas al proyecto SignalRChat. Haga clic con el botón derecho en el proyecto **TestLibrary** y seleccione **Agregar**, **referencia..** .. Seleccione el nodo **proyectos** en el nodo de la **solución** y compruebe **SignalRChat**. Haga clic en **Aceptar**.

    ![Agregar referencia de proyecto](unit-testing-signalr-applications/_static/image3.png)
6. Agregue los paquetes Signalr, MOQ y XUnit al proyecto **TestLibrary** . En la **consola del administrador de paquetes**, establezca la lista desplegable **proyecto predeterminado** en **TestLibrary**. Ejecute los siguientes comandos en la ventana de la consola:

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalación de paquetes](unit-testing-signalr-applications/_static/image4.png)
7. Cree el archivo de prueba. Haga clic con el botón derecho en el proyecto **TestLibrary** y haga clic en **Agregar...** , **clase**. Asigne a la nueva clase el nombre **tests.CS**.
8. Reemplace el contenido de Tests.cs por el código siguiente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    En el código anterior, se crea un cliente de prueba mediante el `Mock` objeto de la biblioteca [MOQ](https://github.com/Moq/moq4) , de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en signalr 2,1, asigna `dynamic` para el parámetro de tipo). La interfaz de `IHubCallerConnectionContext` es el objeto proxy con el que se invocan métodos en el cliente. A continuación, se define la función `broadcastMessage` para el cliente ficticio, de modo que la clase `ChatHub` pueda llamarla. A continuación, el motor de pruebas llama al método `Send` de la clase `ChatHub`, que a su vez llama a la función de `broadcastMessage` ficticia.
9. Para compilar la solución, presione **F6**.
10. Ejecute la prueba unitaria. En Visual Studio, seleccione **probar**, **Windows**, **Explorador de pruebas**. En la ventana Explorador de pruebas, haga clic con el botón secundario en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image5.png)
11. Compruebe que la prueba se superó comprobando el panel inferior en la ventana del explorador de pruebas. La ventana mostrará que la prueba se ha superado.

    ![Prueba superada](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Pruebas unitarias por tipo

En esta sección, agregará una prueba para la aplicación creada en el [Introducción tutorial](../getting-started/tutorial-getting-started-with-signalr.md) mediante una interfaz que contiene el método que se va a probar.

1. Complete los pasos del 1-7 en el tutorial de [pruebas unitarias con Dynamic](#dynamic) .
2. Reemplace el contenido de Tests.cs por el código siguiente.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    En el código anterior, se crea una interfaz que define la firma del método `broadcastMessage` para el que el motor de pruebas creará un cliente ficticio. A continuación, se crea un cliente ficticio mediante el `Mock` objeto, de tipo [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (en signalr 2,1, asigna `dynamic` para el parámetro de tipo). La interfaz de `IHubCallerConnectionContext` es el objeto proxy con el que se invocan métodos en el cliente.

    A continuación, la prueba crea una instancia de `ChatHub`y, a continuación, crea una versión ficticia del método `broadcastMessage`, que a su vez se invoca llamando al método `Send` en el concentrador.
3. Para compilar la solución, presione **F6**.
4. Ejecute la prueba unitaria. En Visual Studio, seleccione **probar**, **Windows**, **Explorador de pruebas**. En la ventana Explorador de pruebas, haga clic con el botón secundario en **HubsAreMockableViaDynamic** y seleccione **ejecutar pruebas seleccionadas**.

    ![Explorador de pruebas](unit-testing-signalr-applications/_static/image7.png)
5. Compruebe que la prueba se superó comprobando el panel inferior en la ventana del explorador de pruebas. La ventana mostrará que la prueba se ha superado.

    ![Prueba superada](unit-testing-signalr-applications/_static/image8.png)
