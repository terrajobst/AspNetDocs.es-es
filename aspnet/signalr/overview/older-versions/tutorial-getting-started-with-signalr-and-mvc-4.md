---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introducción con Signalr 1. x y MVC 4 | Microsoft Docs'
author: bradygaster
description: Use ASP.NET Signalr y ASP.NET MVC 4 para compilar una aplicación de chat en tiempo real.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468073"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Tutorial: Introducción con Signalr 1. x y MVC 4

por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tutorial se muestra cómo usar ASP.NET Signalr para crear una aplicación de chat en tiempo real. Agregará Signalr a una aplicación de MVC 4 y creará una vista de chat para enviar y mostrar mensajes.

## <a name="overview"></a>Información general

En este tutorial se presenta el desarrollo de aplicaciones web en tiempo real con ASP.NET Signalr y ASP.NET MVC 4. En el tutorial se usa el mismo código de la aplicación de chat que [signalr introducción tutorial](tutorial-getting-started-with-signalr.md), pero se muestra cómo agregarlo a una aplicación de MVC 4 basada en la plantilla de Internet.

En este tema, aprenderá las siguientes tareas de desarrollo de Signalr:

- Agregar la biblioteca de Signalr a una aplicación de MVC 4.
- Crear una clase de concentrador para enviar contenido a los clientes.
- Uso de la biblioteca de jQuery de Signalr en una página web para enviar mensajes y Mostrar actualizaciones desde el concentrador.

En la captura de pantalla siguiente se muestra la aplicación de chat completada que se ejecuta en un explorador.

![Instancias de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Sección

- [Configurar el proyecto](#setup)
- [Ejecutar el ejemplo](#run)
- [Examinar el código](#code)
- [Pasos siguientes](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Configurar el proyecto

Requisitos previos:

- Visual Studio 2010 SP1, Visual Studio 2012 o Visual Studio 2012 Express. Si no tiene Visual Studio, consulte descargas de [ASP.net](https://www.asp.net/downloads) para obtener la herramienta gratuita de desarrollo de visual Studio 2012 Express.
- Para Visual Studio 2010, instale [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

En esta sección se muestra cómo crear una aplicación ASP.NET MVC 4, agregar la biblioteca Signalr y crear la aplicación de chat.

1. 1. En Visual Studio, cree una aplicación ASP.NET MVC 4, asígnele el nombre SignalRChat y haga clic en Aceptar.

        > [!NOTE]
        > En VS 2010, seleccione **.NET Framework 4** en el control desplegable versión de Framework. El código de signalr se ejecuta en las versiones 4 y 4,5 de .NET Framework.

        ![Crear web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Seleccione la plantilla aplicación de Internet, desactive la opción para **crear un proyecto de prueba unitaria**y haga clic en Aceptar.

         ![Crear sitio de Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Abra las **herramientas > el administrador de paquetes NuGet > consola del administrador de paquetes** y ejecute el siguiente comando. Este paso agrega al proyecto un conjunto de archivos de script y referencias de ensamblado que habilitan la funcionalidad de Signalr.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. En **Explorador de soluciones** expanda la carpeta scripts. Tenga en cuenta que las bibliotecas de scripts de Signalr se han agregado al proyecto.

         ![Referencias de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto, seleccione **Agregar | Nueva carpeta**y agregue una nueva carpeta denominada **hubs**.
      6. Haga clic con el botón secundario en la carpeta **hubs** , haga clic en **Agregar |** Y cree una nueva C# clase denominada **ChatHub.CS**. Usará esta clase como concentrador de servidor de Signalr que envía mensajes a todos los clientes.

> [!NOTE]
> Si usa Visual Studio 2012 y ha instalado la [actualización ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), puede usar la nueva plantilla de elemento signalr para crear la clase hub. Para ello, haga clic con el botón secundario en la carpeta **hubs** , haga clic en **Agregar | Nuevo elemento**, seleccione **clase de concentrador de signalr (V1)** y asigne a la clase el nombre **ChatHub.CS**.

1. Reemplace el código de la clase **ChatHub** por el código siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Abra el archivo **global. asax** del proyecto y agregue una llamada al método `RouteTable.Routes.MapHubs();` como la primera línea de código en el método `Application_Start`. Este código registra la ruta predeterminada para los concentradores de Signalr y se debe llamar antes de registrar otras rutas. El método `Application_Start` completado es similar al del ejemplo siguiente.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Edite la clase `HomeController` que se encuentra en **Controllers/HomeController. CS** y agregue el método siguiente a la clase. Este método devuelve la vista de **chat** que creará en un paso posterior.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Haga clic con el botón derecho en el método `Chat` que acaba de crear y haga clic en **Agregar vista** para crear un nuevo archivo de vista.
5. En el cuadro de diálogo **Agregar vista** , asegúrese de que la casilla está activada para **usar un diseño o una página maestra** (desactive las demás casillas) y, a continuación, haga clic en **Agregar**.

    ![Agregar una vista](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Edite el nuevo archivo de vista denominado **chat. cshtml**. Después de la etiqueta &lt;H2&gt;, pegue la siguiente sección de &lt;div&gt; y el bloque de código `@section scripts` en la página. Este script permite a la página enviar mensajes de chat y mostrar mensajes del servidor. El código completo de la vista de chat aparece en el siguiente bloque de código.

    > [!IMPORTANT]
    > Al agregar Signalr y otras bibliotecas de scripts al proyecto de Visual Studio, el administrador de paquetes puede instalar versiones de los scripts que son más recientes que las versiones que se muestran en este tema. Asegúrese de que las referencias de script en el código coinciden con las versiones de las bibliotecas de scripts instaladas en el proyecto.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Guarde todo** para el proyecto.

<a id="run"></a>

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Presione F5 para ejecutar el proyecto en modo de depuración.
2. En la línea de dirección del explorador, anexe **/Home/chat** a la dirección URL de la página predeterminada del proyecto. La página de chat se carga en una instancia del explorador y solicita un nombre de usuario.

    ![Escribir nombre de usuario](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Escriba un nombre de usuario.
4. Copie la dirección URL de la línea de dirección del explorador y Úsela para abrir dos instancias más del explorador. En cada instancia del explorador, escriba un nombre de usuario único.
5. En cada instancia del explorador, agregue un comentario y haga clic en **Enviar**. Los comentarios deben mostrarse en todas las instancias del explorador.

    > [!NOTE]
    > Esta sencilla aplicación de chat no mantiene el contexto de discusión en el servidor. El centro difunde los comentarios a todos los usuarios actuales. Los usuarios que se unan al chat más adelante verán mensajes agregados desde el momento en que se unen.
6. En la captura de pantalla siguiente se muestra la aplicación de chat que se ejecuta en un explorador.

    ![Exploradores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. En **Explorador de soluciones**, inspeccione el nodo **documentos de script** para la aplicación en ejecución. Este nodo está visible en modo de depuración si usa Internet Explorer como explorador. Hay un archivo de script denominado **hubs** que la biblioteca de signalr genera dinámicamente en tiempo de ejecución. Este archivo administra la comunicación entre el script de jQuery y el código del lado servidor. Si utiliza un explorador que no sea Internet Explorer, también puede tener acceso al archivo de **concentradores** dinámicos desplazándose directamente a él, por ejemplo http://mywebsite/signalr/hubs.

    ![Script de concentrador generado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Examinar el código

La aplicación Signalr Conversation muestra dos tareas básicas de desarrollo de Signalr: crear un concentrador como el objeto de coordinación principal en el servidor y usar la biblioteca de jQuery de Signalr para enviar y recibir mensajes.

### <a name="signalr-hubs"></a>Concentradores de SignalR

En el ejemplo de código, la clase **ChatHub** se deriva de la clase **Microsoft. Aspnet. signalr. Hub** . Derivar de la clase **Hub** es una manera útil de compilar una aplicación signalr. Puede crear métodos públicos en la clase de concentrador y, a continuación, acceder a esos métodos llamándolos desde scripts de jQuery en una página web.

En el código de chat, los clientes llaman al método **ChatHub. Send** para enviar un mensaje nuevo. El concentrador, a su vez, envía el mensaje a todos los clientes mediante una llamada a **clients. All. addNewMessageToPage**.

El método **send** muestra varios conceptos de Hub:

- Declare los métodos públicos en un concentrador para que los clientes puedan llamarlos.
- Use la propiedad **Microsoft. Aspnet. signalr. Hub. clients** para tener acceso a todos los clientes conectados a este concentrador.
- Llame a una función de jQuery en el cliente (por ejemplo, la función `addNewMessageToPage`) para actualizar los clientes.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>Signalr y jQuery

El archivo de vista de **chat. cshtml** en el ejemplo de código muestra cómo usar la biblioteca de jQuery de signalr para comunicarse con un concentrador de signalr. Las tareas esenciales del código son la creación de una referencia al proxy generado automáticamente para el concentrador, la declaración de una función a la que el servidor puede llamar para insertar contenido en los clientes y el inicio de una conexión para enviar mensajes al centro.

El código siguiente declara un proxy para un concentrador.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> En jQuery, la referencia a la clase de servidor y sus miembros está en mayúsculas y minúsculas Camel. En el ejemplo de código C# se hace referencia a la clase **ChatHub** de jQuery como **ChatHub**. Si desea hacer referencia a la clase `ChatHub` en jQuery con la grafía Pascal convencional tal como lo C#haría en, edite el archivo de clase ChatHub.cs. Agregue una instrucción `using` para hacer referencia al espacio de nombres `Microsoft.AspNet.SignalR.Hubs`. A continuación, agregue el atributo `HubName` a la clase `ChatHub`, por ejemplo `[HubName("ChatHub")]`. Por último, actualice la referencia de jQuery a la clase `ChatHub`.

En el código siguiente se muestra cómo crear una función de devolución de llamada en el script. La clase hub en el servidor llama a esta función para enviar actualizaciones de contenido a cada cliente. La llamada opcional a la función `htmlEncode` muestra una forma de codificar en HTML el contenido del mensaje antes de mostrarlo en la página, como una manera de evitar la inserción de scripts.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

En el código siguiente se muestra cómo abrir una conexión con el centro. El código inicia la conexión y, a continuación, le pasa una función para controlar el evento de clic en el botón **Enviar** de la página de chat.

> [!NOTE]
> Este enfoque garantiza que la conexión se establece antes de que se ejecute el controlador de eventos.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Pasos siguientes

Ha aprendido que Signalr es un marco para compilar aplicaciones web en tiempo real. También ha aprendido varias tareas de desarrollo de Signalr: Cómo agregar Signalr a una aplicación de ASP.NET, cómo crear una clase de concentrador y cómo enviar y recibir mensajes desde el centro.

Para obtener más información sobre los conceptos avanzados de los avances de Signalr, visite los siguientes sitios para ver el código fuente y los recursos de Signalr:

- [Proyecto signalr](http://signalr.net)
- [Github y ejemplos de signalr](https://github.com/SignalR/SignalR)
- [Wiki de signalr](https://github.com/SignalR/SignalR/wiki)
