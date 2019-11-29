---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: chat en tiempo real con Signalr 2 y MVC 5 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo usar ASP.NET Signalr 2 para crear una aplicación de chat en tiempo real. Puede Agregar Signalr a una aplicación de MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600483"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>Tutorial: chat en tiempo real con Signalr 2 y MVC 5

En este tutorial se muestra cómo usar ASP.NET Signalr 2 para crear una aplicación de chat en tiempo real. Puede Agregar Signalr a una aplicación de MVC 5 y crear una vista de chat para enviar y mostrar mensajes.

En este tutorial ha:

> [!div class="checklist"]
> * Configurar el proyecto
> * Ejecutar el ejemplo
> * Examinar el código

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **desarrollo web y ASP.net** .

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección se muestra cómo usar Visual Studio 2017 y Signalr 2 para crear una aplicación ASP.NET MVC 5 vacía, agregar la biblioteca de Signalr y crear la aplicación de chat.

1. En Visual Studio, cree una C# aplicación ASP.net cuyo destino sea .NET Framework 4,5, asígnele el nombre SignalRChat y haga clic en Aceptar.

    ![Crear Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. En **nueva aplicación Web de ASP.net-SignalRMvcChat**, seleccione **MVC** y, a continuación, seleccione **cambiar autenticación**.

1. En **cambiar autenticación**, seleccione **sin autenticación** y haga clic en **Aceptar**.

    ![Seleccione sin autenticación](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. En **nueva aplicación Web de ASP.net-SignalRMvcChat**, seleccione **Aceptar**.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-SignalRChat**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .

1. Asigne a la clase el nombre *ChatHub* y agréguelo al proyecto.

    En este paso se crea el archivo de clase *ChatHub.CS* y se agrega un conjunto de archivos de script y referencias de ensamblado que admiten signalr en el proyecto.

1. Reemplace el código del nuevo archivo de clase *ChatHub.CS* con este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **agregar** **clase**de > .

1. Asigne a la nueva clase el nombre *Startup* y agréguela al proyecto.

1. Reemplace el código del archivo de clase *Startup.CS* por este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. En **Explorador de soluciones**, seleccione **controllers** > **HomeController.CS**.

1. Agregue este método a *HomeController.CS*.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    Este método devuelve la vista de **chat** que se crea en un paso posterior.

1. En **Explorador de soluciones**, haga clic con el botón derecho en **vistas** > **Inicio**y seleccione **Agregar** >  **vista**.

1. En **Agregar vista**, asigne a la nueva vista el nombre **chat** y seleccione **Agregar**.

1. Reemplace el contenido de **chat. cshtml** con este código:

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. En **Explorador de soluciones**, expanda **scripts**.

    Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.

    > [!IMPORTANT]
    > Es posible que el administrador de paquetes haya instalado una versión posterior de los scripts de Signalr.

1. Compruebe que las referencias de script en el bloque de código corresponden a las versiones de los archivos de script del proyecto.

    Haga referencia al script desde el bloque de código original:

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. Si no coinciden, actualice el archivo *. cshtml* .

1. En la barra de menús, seleccione **archivo** > **guardar todo**.

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. En la barra de herramientas, active la **depuración de scripts** y, después, seleccione el botón reproducir para ejecutar el ejemplo en modo de depuración.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. Cuando se abra el explorador, escriba un nombre para la identidad de chat.

1. Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.

1. En cada explorador, escriba un nombre único.

1. Ahora, agregue un comentario y seleccione **Enviar**. Repítalo en los otros exploradores. Los comentarios aparecen en tiempo real.

    > [!NOTE]
    > Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor. El centro difunde los comentarios a todos los usuarios actuales. Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.

    Vea cómo se ejecuta la aplicación de chat en tres exploradores diferentes. Cuando Tom, Anand y Susan envían mensajes, todos los exploradores se actualizan en tiempo real:

    ![Los tres exploradores muestran el mismo historial de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución. Hay un archivo de script denominado *hubs* que la biblioteca de signalr genera en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.

    ![script de hubs generados automáticamente en el nodo documentos de script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>Examinar el código

La aplicación Signalr Conversation muestra dos tareas básicas de desarrollo de Signalr. Muestra cómo crear un centro. El servidor utiliza ese concentrador como el objeto de coordinación principal. El concentrador usa la biblioteca de jQuery de Signalr para enviar y recibir mensajes.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentradores de signalr en ChatHub.cs

En el ejemplo de código, la clase `ChatHub` se deriva de la clase `Microsoft.AspNet.SignalR.Hub`. La derivación de la clase `Hub` es una manera útil de compilar una aplicación Signalr. Puede crear métodos públicos en la clase de concentrador y, a continuación, tener acceso a esos métodos llamándolos desde scripts en una página web.

En el código de chat, los clientes llaman al método `ChatHub.Send` para enviar un mensaje nuevo. El concentrador, a su vez, envía el mensaje a todos los clientes mediante una llamada a `Clients.All.addNewMessageToPage`.

El método `Send` muestra varios conceptos de Hub:

* Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.

* Use la propiedad dinámica `Microsoft.AspNet.SignalR.Hub.Clients` para comunicarse con todos los clientes conectados a este concentrador.

* Llame a una función en el cliente (como la función `addNewMessageToPage`) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>Signalr y jQuery chat. cshtml

El archivo de vista de *chat. cshtml* en el ejemplo de código muestra cómo usar la biblioteca de jQuery de signalr para comunicarse con un concentrador de signalr.  El código lleva a cabo muchas tareas importantes. Crea una referencia al proxy generado automáticamente para el concentrador, declara una función a la que el servidor puede llamar para insertar contenido en los clientes e inicia una conexión para enviar mensajes al centro.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros está en camelCase. En el ejemplo de código C# se hace referencia a la clase `ChatHub` en JavaScript como `chatHub`.

En este bloque de código, se crea una función de devolución de llamada en el script.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente. La llamada opcional a la función `htmlEncode` muestra una forma de codificar en HTML el contenido del mensaje antes de mostrarlo en la página. Es una manera de evitar la inyección de scripts.

Este código abre una conexión con el centro.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> Este enfoque garantiza que se establece una conexión antes de que se ejecute el controlador de eventos.

El código inicia la conexión y, a continuación, le pasa una función para controlar el evento de clic en el botón **Enviar** de la página de chat.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información acerca de Signalr, consulte los siguientes recursos:

* [Proyecto signalr](http://signalr.net)

* [GitHub y ejemplos de signalr](https://github.com/SignalR/SignalR)

* [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Configurar el proyecto
> * Ejecutar el ejemplo
> * Examinó el código

Vaya al siguiente artículo para obtener información sobre cómo crear una aplicación web que use ASP.NET Signalr 2 para proporcionar funcionalidad de mensajería de alta frecuencia.
> [!div class="nextstepaction"]
> [Aplicación web con mensajería de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md)