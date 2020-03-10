---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hospedar OWIN en un rol de trabajo de Azure | Microsoft Docs
author: MikeWasson
description: En este tutorial se muestra cómo autohospedar OWIN en un rol de trabajo de Microsoft Azure. Open Web Interface for .NET (OWIN) define una abstracción entre el servidor Web .NET...
ms.author: riande
ms.date: 04/11/2014
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 59d2e0d549427093f8a2424b17af81169b78ef30
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472399"
---
# <a name="host-owin-in-an-azure-worker-role"></a>Hospedar OWIN en un rol de trabajo de Azure

por [Mike Wasson](https://github.com/MikeWasson)

> En este tutorial se muestra cómo autohospedar OWIN en un rol de trabajo de Microsoft Azure.
>
> [Open Web Interface for .net](http://owin.org/) (OWIN) define una abstracción entre los servidores Web .net y las aplicaciones Web. OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS, por ejemplo, dentro de un rol de trabajo de Azure.
>
> En este tutorial, aprenderá a hospedar automáticamente una aplicación OWIN dentro de un rol de trabajo de Microsoft Azure. Para más información sobre los roles de trabajo, consulte [modelos de ejecución de Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - [SDK de Azure para .NET 2,3](https://azure.microsoft.com/downloads/)
> - [Microsoft. Owin. Selfhost 2.1.0](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)

## <a name="create-a-microsoft-azure-project"></a>Creación de un proyecto de Microsoft Azure

Inicie Visual Studio con privilegios de administrador. Se necesitan privilegios de administrador para depurar la aplicación localmente, mediante el emulador de proceso de Azure.

En el menú **archivo** , haga clic en **nuevo**y, a continuación, haga clic en **proyecto**. En **plantillas instaladas**, en C#visual, haga clic en **nube** y, a continuación, haga clic en **servicio en la nube de Windows Azure**. Asigne al proyecto el nombre "AzureApp" y haga clic en **Aceptar**.

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

En el cuadro de diálogo **nuevo servicio en la nube de Windows Azure** , haga doble clic en **rol de trabajo**. Deje el nombre predeterminado ("WorkerRole1"). En este paso se agrega un rol de trabajo a la solución. Haga clic en **Aceptar**.

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

La solución de Visual Studio que se crea contiene dos proyectos:

- &quot;AzureApp&quot; define los roles y la configuración de la aplicación de Azure.
- &quot;WorkerRole1&quot; contiene el código para el rol de trabajo.

En general, una aplicación de Azure puede contener varios roles, aunque en este tutorial se usa un único rol.

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a>Agregar los paquetes de autohospedario OWIN

En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet**y luego en **consola del administrador de paquetes**.

En la ventana Package Manager Console, escriba el siguiente comando:

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Agregar un punto de conexión HTTP

En Explorador de soluciones, expanda el proyecto AzureApp. Expanda el nodo roles, haga clic con el botón secundario en WorkerRole1 y seleccione **propiedades**.

![](host-owin-in-an-azure-worker-role/_static/image6.png)

Haga clic en **Extremos** y, a continuación, en **Agregar extremo**.

En la lista desplegable **Protocolo** , seleccione "http". En **puerto público** y **puerto privado**, escriba 80. Estos números de puerto pueden ser diferentes. El puerto público es lo que los clientes usan cuando envían una solicitud al rol.

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a>Creación de la clase de inicio OWIN

En Explorador de soluciones, haga clic con el botón derecho en el proyecto WorkerRole1 y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `Startup`.

Reemplace todo el código reutilizable por lo siguiente:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

El método de extensión `UseWelcomePage` agrega una página HTML simple a la aplicación para comprobar que el sitio funciona.

## <a name="start-the-owin-host"></a>Inicio del host OWIN

Abra el archivo WorkerRole.cs. Esta clase define el código que se ejecuta cuando se inicia y se detiene el rol de trabajo.

Agregue la siguiente instrucción using:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

Agregue un miembro **IDisposable** a la clase `WorkerRole`:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

En el método `OnStart`, agregue el código siguiente para iniciar el host:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

El método **webapp. Start** inicia el host OWIN. El nombre de la clase `Startup` es un parámetro de tipo para el método. Por Convención, el host llamará al método `Configure` de esta clase.

Invalide el `OnStop` para desechar la instancia de la *aplicación\_* :

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

Este es el código completo de WorkerRole.cs:

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

Compile la solución y presione F5 para ejecutar la aplicación localmente en el emulador de proceso de Azure. En función de la configuración del firewall, es posible que deba permitir el emulador a través del firewall.

El emulador de proceso asigna una dirección IP local al extremo. Puede encontrar la dirección IP visualizando la interfaz de usuario del emulador de proceso. Haga clic con el botón derecho en el icono del emulador en el área de notificación de la barra de tareas y seleccione **Mostrar interfaz de usuario del emulador de proceso**.

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

Busque la dirección IP en implementaciones de servicio, implementación [id], detalles del servicio. Abra un explorador Web y vaya a http:\/\/*Dirección*, donde *Dirección* es la dirección IP asignada por el emulador de proceso; por ejemplo, `http://127.0.0.1:80`. Debería ver la página de bienvenida de OWIN:

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a>Implementar en Azure

En este paso, debe tener una cuenta de Azure. Si aún no tiene una, puede crear una cuenta de evaluación gratuita en un par de minutos. Para obtener más información, consulte [evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

En Explorador de soluciones, haga clic con el botón secundario en el proyecto AzureApp. Seleccione **Publicar**.

![](host-owin-in-an-azure-worker-role/_static/image12.png)

Si no ha iniciado sesión en su cuenta de Azure, haga clic en **iniciar sesión**.

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

Una vez iniciada la sesión, elija una suscripción y haga clic en **siguiente**.

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

Escriba un nombre para el servicio en la nube y elija una región. Haga clic en **Crear**.

![](host-owin-in-an-azure-worker-role/_static/image17.png)

Haga clic en **Publicar**.

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

La ventana registro de actividad de Azure muestra el progreso de la implementación. Cuando se implemente la aplicación, vaya a `http://appname.cloudapp.net/`, donde *appname* es el nombre del servicio en la nube.

## <a name="additional-resources"></a>Recursos adicionales

- [Información general del proyecto Katana](an-overview-of-project-katana.md)
- [Proyecto Katana en GitHub](https://github.com/aspnet/AspNetKatana/)
