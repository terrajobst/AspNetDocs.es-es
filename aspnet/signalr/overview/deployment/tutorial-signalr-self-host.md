---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: autohospedaje de Signalr | Microsoft Docs'
author: bradygaster
description: En este tutorial se muestra cómo crear un servidor de Signalr con autohospedado y cómo conectarse a él con un cliente de JavaScript. Versiones de software usadas en el tutorial...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578574"
---
# <a name="tutorial-signalr-self-host"></a>Tutorial: autohospedaje de Signalr

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[Descargar proyecto completado](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> En este tutorial se muestra cómo crear un servidor de Signalr con autohospedado y cómo conectarse a él con un cliente de JavaScript.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Uso de Visual Studio 2012 con este tutorial
>
>
> Para usar Visual Studio 2012 con este tutorial, haga lo siguiente:
>
> - Actualice el [Administrador de paquetes](http://docs.nuget.org/docs/start-here/installing-nuget) a la versión más reciente.
> - Instale el [instalador de plataforma web](https://www.microsoft.com/web/downloads/platform.aspx).
> - En el instalador de plataforma web, busque e instale **ASP.NET and Web Tools 2013,1 para Visual Studio 2012**. Se instalarán las plantillas de Visual Studio para las clases de Signalr como **Hub**.
> - Algunas plantillas (como la **clase de inicio OWIN**) no estarán disponibles; para ello, use un archivo de clase en su lugar.
>
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Información general del

Un servidor de Signalr se hospeda normalmente en una aplicación ASP.NET en IIS, pero también puede ser autohospedado (por ejemplo, en una aplicación de consola o un servicio de Windows) mediante la biblioteca de autohospedaje. Esta biblioteca, al igual que la de Signalr 2, se basa en OWIN ([Open web interface para .net](http://owin.org)). OWIN define una abstracción entre servidores Web .NET y aplicaciones Web. OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS.

Entre los motivos para no hospedar en IIS se incluyen:

- Entornos en los que IIS no está disponible o deseable, como una granja de servidores existente sin IIS.
- La sobrecarga de rendimiento de IIS debe evitarse.
- La funcionalidad de signalr se agregará a una aplicación existente que se ejecuta en un servicio de Windows, un rol de trabajo de Azure u otro proceso.

Si una solución se está desarrollando como autohospedada por motivos de rendimiento, se recomienda probar también la aplicación hospedada en IIS para determinar la ventaja de rendimiento.

Este tutorial contiene las siguientes secciones:

- [Crear el servidor](#server)
- [Acceso al servidor con un cliente de JavaScript](#js)

<a id="server"></a>

## <a name="creating-the-server"></a>Crear el servidor

En este tutorial, creará un servidor que se hospeda en una aplicación de consola, pero el servidor se puede hospedar en cualquier tipo de proceso, como un servicio de Windows o un rol de trabajo de Azure. Para ver el código de ejemplo para hospedar un servidor de Signalr en un servicio de Windows, consulte [signalr Autohospedaje en un servicio de Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).

1. Abra Visual Studio 2013 con privilegios de administrador. Seleccione **archivo**, **nuevo proyecto**. Seleccione **Windows** en el **nodo C# visual** en el panel **plantillas** y seleccione la plantilla **aplicación de consola** . Asigne al nuevo proyecto el nombre "SignalRSelfHost" y haga clic en **Aceptar**.

    ![](tutorial-signalr-self-host/_static/image1.png)
2. Abra la consola del administrador de paquetes NuGet seleccionando **herramientas** > **Administrador de paquetes Nuget** > **consola del administrador de paquetes**.
3. En la consola del administrador de paquetes, escriba el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    Este comando agrega las bibliotecas de autohospedamiento Signalr 2 al proyecto.
4. En la consola del administrador de paquetes, escriba el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    Este comando agrega la biblioteca Microsoft. Owin. CORS al proyecto. Esta biblioteca se usará para la compatibilidad entre dominios, que es necesaria para las aplicaciones que hospedan Signalr y un cliente de páginas web en dominios diferentes. Puesto que va a hospedar el servidor de Signalr y el cliente web en distintos puertos, esto significa que entre dominios debe estar habilitado para la comunicación entre estos componentes.
5. Reemplace el contenido de Program.cs por el código siguiente.

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    El código anterior incluye tres clases:

    - **Programa**, incluido el método **Main** que define la ruta de acceso principal de la ejecución. En este método, se inicia una aplicación Web de tipo **Startup** en la dirección URL especificada (`http://localhost:8080`). Si se requiere seguridad en el punto de conexión, se puede implementar SSL. Consulte [Cómo: configurar un puerto con un certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obtener más información.
    - **Startup**, la clase que contiene la configuración para el servidor de signalr (la única configuración que usa este tutorial es la llamada a `UseCors`) y la llamada a `MapSignalR`, que crea rutas para cualquier objeto de concentrador en el proyecto.
    - **MyHub**, la clase de concentrador de signalr que la aplicación proporcionará a los clientes. Esta clase tiene un único método, **send**, al que los clientes llamarán para difundir un mensaje a todos los demás clientes conectados.
6. Compile y ejecute la aplicación. La dirección en la que se ejecuta el servidor debería mostrarse en una ventana de consola.

    ![](tutorial-signalr-self-host/_static/image2.png)
7. Si se produce un error en la ejecución con la excepción `System.Reflection.TargetInvocationException was unhandled`, deberá reiniciar Visual Studio con privilegios de administrador.
8. Detenga la aplicación antes de continuar con la siguiente sección.

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a>Acceso al servidor con un cliente de JavaScript

En esta sección, usará el mismo cliente de JavaScript del tutorial de [Introducción](../getting-started/tutorial-getting-started-with-signalr.md). Solo realizaremos una modificación en el cliente, que es definir explícitamente la dirección URL del centro. Con una aplicación autohospedada, es posible que el servidor no esté necesariamente en la misma dirección que la dirección URL de conexión (debido a los servidores proxy inversos y los equilibradores de carga), por lo que la dirección URL debe definirse explícitamente.

1. En **Explorador de soluciones**, haga clic con el botón derecho en la solución y seleccione **Agregar**, **nuevo proyecto**. Seleccione el nodo **Web** y seleccione la plantilla **aplicación Web ASP.net** . Asigne al proyecto el nombre "JavascriptClient" y haga clic en **Aceptar**.

    ![](tutorial-signalr-self-host/_static/image3.png)
2. Seleccione la plantilla **vacía** y deje las opciones restantes desactivadas. Seleccione **crear proyecto**.

    ![](tutorial-signalr-self-host/_static/image4.png)
3. En la consola del administrador de paquetes, seleccione el proyecto "JavascriptClient" en la lista desplegable **proyecto predeterminado** y ejecute el siguiente comando:

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    Este comando instala las bibliotecas Signalr y JQuery que necesitará en el cliente.
4. Haga clic con el botón derecho en el proyecto y seleccione **Agregar**, **nuevo elemento**. Seleccione el nodo **Web** y la página HTML. Asigne el nombre **default. html**a la página.

    ![](tutorial-signalr-self-host/_static/image5.png)
5. Reemplace el contenido de la nueva página HTML por el código siguiente. Compruebe que las referencias del script aquí coinciden con los scripts de la carpeta scripts del proyecto.

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    El código siguiente (resaltado en el ejemplo de código anterior) es la adición que ha realizado al cliente usado en el tutorial de introducción (además de actualizar el código a Signalr versión 2 beta). Esta línea de código establece explícitamente la dirección URL de conexión base para Signalr en el servidor.

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. Haga clic con el botón derecho en la solución y seleccione **establecer proyectos de inicio..** .. Seleccione el botón de radio **proyectos de inicio múltiples** y establezca la **acción** de ambos proyectos en **iniciar**.

    ![](tutorial-signalr-self-host/_static/image6.png)
7. Haga clic con el botón derecho en "default. html" y seleccione **establecer como página de inicio**.
8. Ejecute la aplicación. Se iniciará el servidor y la página. Es posible que tenga que volver a cargar la página web (o seleccionar **continuar** en el depurador) si la página se carga antes de que se inicie el servidor.
9. En el explorador, proporcione un nombre de usuario cuando se le solicite. Copie la dirección URL de la página en otra pestaña o ventana del explorador y proporcione un nombre de usuario diferente. Podrá enviar mensajes desde un panel del explorador al otro, como en el tutorial de Introducción.
