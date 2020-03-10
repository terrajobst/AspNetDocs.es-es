---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: chat en tiempo real con Signalr 2 | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Puede Agregar Signalr a una aplicación Web de ASP.NET vacía.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431569"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>Tutorial: chat en tiempo real con Signalr 2

En este tutorial se muestra cómo usar Signalr para crear una aplicación de chat en tiempo real. Agrega Signalr a una aplicación Web ASP.NET vacía y crea una página HTML para enviar y mostrar mensajes.

En este tutorial va a:

> [!div class="checklist"]
> * Configuración del proyecto
> * Ejecución del ejemplo
> * Examen del código

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) con la carga de trabajo de **ASP.NET y desarrollo web**.

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección se muestra cómo usar Visual Studio 2017 y Signalr 2 para crear una aplicación Web de ASP.NET vacía, agregar Signalr y crear la aplicación de chat.

1. En Visual Studio, cree una aplicación Web ASP.NET.

    ![Crear Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. En la ventana **New ASP.net Project-SignalRChat** , deje **vacío** seleccionado y haga clic en **Aceptar**.

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-SignalRChat**, seleccione **instalado** > **Visual C#**  > **Web** > **signalr** y, a continuación, seleccione **clase de concentrador de signalr (V2)** .

1. Asigne a la clase el nombre *ChatHub* y agréguelo al proyecto.

    En este paso se crea el archivo de clase *ChatHub.CS* y se agrega un conjunto de archivos de script y referencias de ensamblado que admiten signalr en el proyecto.

1. Reemplace el código del nuevo archivo de clase *ChatHub.CS* con este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **nuevo elemento**.

1. En **Agregar nuevo elemento-SignalRChat** , seleccione **instalado** > **Visual C#**  > **Web** y, a continuación, seleccione **clase de inicio OWIN**.

1. Asigne a la clase el nombre *Startup* y agréguela al proyecto.

1. Reemplace el código predeterminado en la clase de *Inicio* con este código:

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Agregar** > **página HTML**.

1. Asigne un nombre al nuevo *Índice* de página y seleccione **Aceptar**.

1. En **Explorador de soluciones**, haga clic con el botón secundario en la página HTML que creó y seleccione **establecer como página de inicio**.

1. Reemplace el código predeterminado de la página HTML por este código:

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. En **Explorador de soluciones**, expanda **scripts**.

    Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.

    > [!IMPORTANT]
    > Es posible que el administrador de paquetes haya instalado una versión posterior de los scripts de Signalr.

1. Compruebe que las referencias de script en el bloque de código corresponden a las versiones de los archivos de script del proyecto.

    Haga referencia al script desde el bloque de código original:

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. Si no coinciden, actualice el archivo *. html* .

1. En la barra de menús, seleccione **archivo** > **guardar todo**.

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. En la barra de herramientas, active la **depuración de scripts** y, después, seleccione el botón reproducir para ejecutar el ejemplo en modo de depuración.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image3.png)

1. Cuando se abra el explorador, escriba un nombre para la identidad de chat.

1. Copie la dirección URL del explorador, abra otros dos exploradores y pegue las direcciones URL en las barras de direcciones.

1. En cada explorador, escriba un nombre único.

1. Ahora, agregue un comentario y seleccione **Enviar**. Repítalo en los otros exploradores. Los comentarios aparecen en tiempo real.

    > [!NOTE]
    > Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor. El centro difunde los comentarios a todos los usuarios actuales. Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.

    Vea cómo se ejecuta la aplicación de chat en tres exploradores diferentes. Cuando Tom, Anand y Susan envían mensajes, todos los exploradores se actualizan en tiempo real:

    ![Los tres exploradores muestran el mismo historial de chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución. Hay un archivo de script denominado *hubs* que la biblioteca de signalr genera en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.

    ![script de hubs generados automáticamente en el nodo documentos de script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>Examinar el código

La aplicación SignalRChat muestra dos tareas básicas de desarrollo de Signalr. Muestra cómo crear un centro. El servidor utiliza ese concentrador como el objeto de coordinación principal. El concentrador usa la biblioteca de jQuery de Signalr para enviar y recibir mensajes.

### <a name="signalr-hubs-in-the-chathubcs"></a>Concentradores de signalr en ChatHub.cs

En el ejemplo de código anterior, la clase `ChatHub` se deriva de la clase `Microsoft.AspNet.SignalR.Hub`. La derivación de la clase `Hub` es una manera útil de compilar una aplicación Signalr. Puede crear métodos públicos en la clase de concentrador y, a continuación, usar esos métodos llamándolos desde scripts en una página web.

En el código de chat, los clientes llaman al método `ChatHub.Send` para enviar un mensaje nuevo. A continuación, el concentrador envía el mensaje a todos los clientes mediante una llamada a `Clients.All.broadcastMessage`.

El método `Send` muestra varios conceptos de Hub:

* Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.

* Use la propiedad dinámica `Microsoft.AspNet.SignalR.Hub.Clients` para comunicarse con todos los clientes conectados a este concentrador.

* Llame a una función en el cliente (como la función `broadcastMessage`) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>Signalr y jQuery en index. html

En la página *index. html* del ejemplo de código se muestra cómo usar la biblioteca de jQuery de signalr para comunicarse con un concentrador signalr. El código lleva a cabo muchas tareas importantes. Declara un proxy para que haga referencia al concentrador, declara una función a la que el servidor puede llamar para insertar contenido en los clientes e inicia una conexión para enviar mensajes al centro.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> En JavaScript, la referencia a la clase de servidor y sus miembros deben ser camelCase. En el ejemplo de código C# se hace referencia a la clase *ChatHub* en JavaScript como `chatHub`.

En este bloque de código, se crea una función de devolución de llamada en el script.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente. Las dos líneas que codifican en HTML el contenido antes de mostrarse son opcionales y muestran una buena manera de evitar la inserción de scripts.

Este código abre una conexión con el centro.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> Este enfoque garantiza que el código establezca una conexión antes de que se ejecute el controlador de eventos.

El código inicia la conexión y, a continuación, la pasa una función para controlar el evento de clic en el botón **Enviar** de la página HTML.

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información acerca de Signalr, consulte los siguientes recursos:

* [Proyecto signalr](http://signalr.net)

* [Github y ejemplos de signalr](https://github.com/SignalR/SignalR)

* [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial:

> [!div class="checklist"]
> * Configuración del proyecto
> * Ejecutar el ejemplo
> * Examinó el código

Avance al siguiente artículo para aprender a usar Signalr y MVC 5.
> [!div class="nextstepaction"]
> [Signalr 2 y MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)