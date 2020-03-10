---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Introducción con Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Use ASP.NET Signalr para crear una aplicación de chat en tiempo real en una página HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505753"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a>Tutorial: Introducción con Signalr 1. x

por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tutorial se muestra cómo usar SignalR para crear una aplicación de chat en tiempo real. Agregará Signalr a una aplicación Web ASP.NET vacía y creará una página HTML para enviar y mostrar mensajes.

## <a name="overview"></a>Información general

En este tutorial se presenta el desarrollo de Signalr al mostrar cómo crear una sencilla aplicación de chat basada en explorador. Agregará la biblioteca de Signalr a una aplicación Web de ASP.NET vacía, creará una clase de concentrador para enviar mensajes a los clientes y creará una página HTML que permita a los usuarios enviar y recibir mensajes de chat. Para ver un tutorial similar que muestra cómo crear una aplicación de chat en MVC 4 mediante una vista de MVC, vea [Introducción con signalr y MVC 4](index.md).

> [!NOTE]
> En este tutorial se usa la versión Release (1. x) de Signalr. Para más información sobre los cambios entre Signalr 1. x y 2,0, consulte [actualización de proyectos de signalr 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).

Signalr es una biblioteca .NET de código abierto para compilar aplicaciones web que requieren interacción con el usuario activo o actualizaciones de datos en tiempo real. Algunos ejemplos son las aplicaciones sociales, los juegos multiusuario, la colaboración empresarial y las aplicaciones de noticias, el tiempo o las actualizaciones financieras. A menudo se denominan aplicaciones en tiempo real.

Signalr simplifica el proceso de creación de aplicaciones en tiempo real. Incluye una biblioteca de servidores de ASP.NET y una biblioteca de cliente de JavaScript para facilitar la administración de las conexiones de cliente-servidor y la instalación de actualizaciones de contenido en los clientes. Puede Agregar la biblioteca de Signalr a una aplicación de ASP.NET existente para obtener funcionalidad en tiempo real.

En el tutorial se muestran las siguientes tareas de desarrollo de Signalr:

- Adición de la biblioteca de Signalr a una aplicación Web de ASP.NET.
- Crear una clase de concentrador para enviar contenido a los clientes.
- Uso de la biblioteca de jQuery de Signalr en una página web para enviar mensajes y Mostrar actualizaciones desde el concentrador.

En la captura de pantalla siguiente se muestra la aplicación de chat que se ejecuta en un explorador. Cada usuario nuevo puede publicar comentarios y ver los comentarios agregados una vez que el usuario se une al chat.

![Instancias de chat](tutorial-getting-started-with-signalr/_static/image1.png)

Sección

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examinar el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

En esta sección se muestra cómo crear una aplicación Web de ASP.NET vacía, agregar Signalr y crear la aplicación de chat.

Requisitos previos:

- Visual Studio 2010 SP1 o 2012. Si no tiene Visual Studio, consulte descargas de [ASP.net](https://www.asp.net/downloads) para obtener la herramienta gratuita de desarrollo de visual Studio 2012 Express.
- [Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941). En Visual Studio 2012, este instalador agrega nuevas características de ASP.NET, incluidas las plantillas de Signalr a Visual Studio. Para Visual Studio 2010 SP1, no hay un instalador disponible, pero puede completar el tutorial mediante la instalación del paquete de NuGet Signalr como se describe en los pasos de configuración.

En los pasos siguientes se usa Visual Studio 2012 para crear una aplicación Web vacía de ASP.NET y agregar la biblioteca de Signalr:

1. En Visual Studio, cree una aplicación Web vacía de ASP.NET.

    ![Crear Web vacío](tutorial-getting-started-with-signalr/_static/image2.png)
2. Para abrir la **consola del administrador de paquetes** , seleccione **herramientas | Administrador de paquetes NuGet | Consola del administrador de paquetes**. Escriba el siguiente comando en la ventana de la consola:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Este comando instala la versión más reciente de Signalr 1. x.
3. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar | Clase**. Asigne a la nueva clase el nombre **ChatHub**.
4. En **Explorador de soluciones** expanda el nodo scripts. Las bibliotecas de scripts para jQuery y Signalr están visibles en el proyecto.

    ![Referencias de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. Reemplace el código de la clase **ChatHub** por el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar | Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **clase de aplicación global** y haga clic en **Agregar**.

    ![Agregar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. Agregue las siguientes instrucciones de `using` después de las instrucciones de `using` proporcionadas en la clase Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Agregue la siguiente línea de código en el método `Application_Start` de la clase global para registrar la ruta predeterminada de los concentradores Signalr.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto y, a continuación, haga clic en **Agregar | Nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione página HTML y haga clic en **Agregar**.
10. En **Explorador de soluciones**, haga clic con el botón secundario en la página HTML que acaba de crear y haga clic en **establecer como página de inicio**.
11. Reemplace el código predeterminado de la página HTML por el código siguiente.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Guarde todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración. La página HTML se carga en una instancia del explorador y solicita un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr/_static/image5.png)
2. Escriba un nombre de usuario.
3. Copie la dirección URL de la línea de dirección del explorador y Úsela para abrir dos instancias más del explorador. En cada instancia del explorador, escriba un nombre de usuario único.
4. En cada instancia del explorador, agregue un comentario y haga clic en **Enviar**. Los comentarios deben mostrarse en todas las instancias del explorador.

    > [!NOTE]
    > Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor. El centro difunde los comentarios a todos los usuarios actuales. Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.

    En la captura de pantalla siguiente se muestra la aplicación de chat que se ejecuta en tres instancias del explorador, todas ellas actualizadas cuando una instancia envía un mensaje:

    ![Exploradores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución. Hay un archivo de script denominado **hubs** que la biblioteca de signalr genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor.

    ![Script de concentrador generado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examinar el código

La aplicación Signalr Conversation muestra dos tareas básicas de desarrollo de Signalr: crear un concentrador como el objeto de coordinación principal en el servidor y usar la biblioteca de jQuery de Signalr para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código, la clase **ChatHub** se deriva de la clase **Microsoft. Aspnet. signalr. Hub** . Derivar de la clase **Hub** es una manera útil de compilar una aplicación signalr. Puede crear métodos públicos en la clase de concentrador y, a continuación, acceder a esos métodos llamándolos desde scripts de jQuery en una página web.

En el código de chat, los clientes llaman al método **ChatHub. Send** para enviar un mensaje nuevo. El concentrador, a su vez, envía el mensaje a todos los clientes mediante una llamada a **clients. All. broadcastMessage**.

El método **send** muestra varios conceptos de Hub:

- Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.
- Use la propiedad dinámica **Microsoft. Aspnet. signalr. Hub. clients** para tener acceso a todos los clientes conectados a este concentrador.
- Llame a una función de jQuery en el cliente (por ejemplo, la función `broadcastMessage`) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr y jQuery

En la página HTML del ejemplo de código se muestra cómo usar la biblioteca de jQuery de Signalr para comunicarse con un concentrador Signalr. Las tareas esenciales del código consisten en declarar un proxy para que haga referencia al centro, declarando una función a la que el servidor puede llamar para enviar contenido a los clientes e iniciando una conexión para enviar mensajes al centro.

El código siguiente declara un proxy para un concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> En jQuery, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas Camel. En el ejemplo de código C# se hace referencia a la clase **ChatHub** de jQuery como **ChatHub**.

En el código siguiente se describe cómo crear una función de devolución de llamada en el script. La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente. Las dos líneas que codifican en HTML el contenido antes de mostrarse son opcionales y muestran una forma sencilla de evitar la inserción de scripts.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

En el código siguiente se muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, la pasa una función para controlar el evento de clic en el botón **Enviar** de la página HTML.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecute el controlador de eventos.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que Signalr es un marco para compilar aplicaciones web en tiempo real. También ha aprendido varias tareas de desarrollo de Signalr: Cómo agregar Signalr a una aplicación de ASP.NET, cómo crear una clase de concentrador y cómo enviar y recibir mensajes desde el centro.

Puede hacer que la aplicación de ejemplo de este tutorial u otras aplicaciones de Signalr estén disponibles a través de Internet mediante su implementación en un proveedor de hospedaje. Microsoft ofrece hospedaje web gratuito para un máximo de 10 sitios web en una [cuenta de prueba gratuita de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para ver un tutorial sobre cómo implementar la aplicación Signalr de ejemplo, vea [publicar el ejemplo de signalr introducción como un sitio web de Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Para obtener información detallada sobre cómo implementar un proyecto Web de Visual Studio en un sitio web de Windows Azure, vea [implementar una aplicación de ASP.net en un sitio web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Nota: actualmente no se admite el transporte de WebSocket para sitios web de Windows Azure. Cuando el transporte de WebSocket no está disponible, Signalr usa los otros transportes disponibles, tal y como se describe en la sección transportes del [tema Introducción a signalr](index.md).

Para obtener más información sobre los conceptos avanzados de los avances de Signalr, visite los siguientes sitios para ver el código fuente y los recursos de Signalr:

- [Proyecto signalr](http://signalr.net)
- [Github y ejemplos de signalr](https://github.com/SignalR/SignalR)
- [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)
