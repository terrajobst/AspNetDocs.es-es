---
uid: web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
title: Host ASP.NET Web API 2 en un rol de trabajo de Azure-ASP.NET 4. x
author: MikeWasson
description: 'Tutorial: hospede ASP.NET Web API en un rol de trabajo de Azure, usando OWIN para autohospedar el marco de API Web.'
ms.author: riande
ms.date: 04/02/2014
ms.custom: seoapril2019
ms.assetid: 6980ee2e-d6b0-4a08-8fb6-ab96362dd0e3
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/host-aspnet-web-api-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: ec9904e0bff090be0f504036ae73977cfca0cb31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448411"
---
# <a name="host-aspnet-web-api-2-in-an-azure-worker-role"></a>Host ASP.NET Web API 2 en un rol de trabajo de Azure

por [Mike Wasson](https://github.com/MikeWasson)

> En este tutorial se muestra cómo hospedar ASP.NET Web API en un rol de trabajo de Azure, usando OWIN para autohospedar el marco de API Web.
>
> [Open Web Interface for .net](http://owin.org/) (OWIN) define una abstracción entre los servidores Web .net y las aplicaciones Web. OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS, por ejemplo, dentro de un rol de trabajo de Azure.
>
> En este tutorial, usará el paquete Microsoft. Owin. host. HttpListener, que proporciona un servidor HTTP que se usa para aplicaciones OWIN de autohospedar.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - [SDK de Azure para .NET 2,3](https://azure.microsoft.com/downloads/)

## <a name="create-a-microsoft-azure-project"></a>Creación de un proyecto de Microsoft Azure

Inicie Visual Studio con privilegios de administrador. Se necesitan privilegios de administrador para depurar la aplicación localmente, mediante el emulador de proceso de Azure.

En el menú **archivo** , haga clic en **nuevo**y, a continuación, haga clic en **proyecto**. En **plantillas instaladas**, en C#visual, haga clic en **nube** y, a continuación, haga clic en **servicio en la nube de Windows Azure**. Asigne al proyecto el nombre "AzureApp" y haga clic en **Aceptar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image2.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image1.png)

En el cuadro de diálogo **nuevo servicio en la nube de Windows Azure** , haga doble clic en **rol de trabajo**. Deje el nombre predeterminado ("WorkerRole1"). En este paso se agrega un rol de trabajo a la solución. Haga clic en **Aceptar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image4.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image3.png)

La solución de Visual Studio que se crea contiene dos proyectos:

- &quot;AzureApp&quot; define los roles y la configuración de la aplicación de Azure.
- &quot;WorkerRole1&quot; contiene el código para el rol de trabajo.

En general, una aplicación de Azure puede contener varios roles, aunque en este tutorial se usa un único rol.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-web-api-and-owin-packages"></a>Incorporación de la API Web y los paquetes OWIN

En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet**y luego en **consola del administrador de paquetes**.

En la ventana Package Manager Console, escriba el siguiente comando:

[!code-console[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a>Agregar un punto de conexión HTTP

En Explorador de soluciones, expanda el proyecto AzureApp. Expanda el nodo roles, haga clic con el botón secundario en WorkerRole1 y seleccione **propiedades**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image6.png)

Haga clic en **Extremos** y, a continuación, en **Agregar extremo**.

En la lista desplegable **Protocolo** , seleccione "http". En **puerto público** y **puerto privado**, escriba 80. Estos números de puerto pueden ser diferentes. El puerto público es lo que los clientes usan cuando envían una solicitud al rol.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image8.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image7.png)

## <a name="configure-web-api-for-self-host"></a>Configuración de la API Web para el autohospedaje

En Explorador de soluciones, haga clic con el botón derecho en el proyecto WorkerRole1 y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `Startup`.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image9.png)

Reemplace todo el código reutilizable de este archivo por lo siguiente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample2.cs)]

## <a name="add-a-web-api-controller"></a>Incorporación de un controlador de API Web

A continuación, agregue una clase de controlador de API Web. Haga clic con el botón derecho en el proyecto WorkerRole1 y seleccione **agregar** / **clase**. Asigne a la clase el nombre TestController. Reemplace todo el código reutilizable de este archivo por lo siguiente:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample3.cs)]

Para simplificar, este controlador solo define dos métodos GET que devuelven texto sin formato.

## <a name="start-the-owin-host"></a>Inicio del host OWIN

Abra el archivo WorkerRole.cs. Esta clase define el código que se ejecuta cuando se inicia y se detiene el rol de trabajo.

Agregue la siguiente instrucción using:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample4.cs)]

Agregue un miembro **IDisposable** a la clase `WorkerRole`:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample5.cs)]

En el método `OnStart`, agregue el código siguiente para iniciar el host:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample6.cs?highlight=5)]

El método **webapp. Start** inicia el host OWIN. El nombre de la clase `Startup` es un parámetro de tipo para el método. Por Convención, el host llamará al método `Configure` de esta clase.

Invalide el `OnStop` para desechar la instancia de la *aplicación\_* :

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample7.cs)]

Este es el código completo de WorkerRole.cs:

[!code-csharp[Main](host-aspnet-web-api-in-an-azure-worker-role/samples/sample8.cs)]

Compile la solución y presione F5 para ejecutar la aplicación localmente en el emulador de proceso de Azure. En función de la configuración del firewall, es posible que deba permitir el emulador a través del firewall.

> [!NOTE]
> Si recibe una excepción como la siguiente, consulte [esta entrada de blog](https://blogs.msdn.com/b/praburaj/archive/2013/11/20/fileloadexception-on-microsoft-owin-when-running-on-worker-role.aspx) para obtener una solución alternativa. "No se pudo cargar el archivo o ensamblado ' Microsoft. Owin, version = 2.0.2.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' o una de sus dependencias. La definición del manifiesto del ensamblado encontrado no coincide con la referencia de ensamblado. (Excepción de HRESULT: 0x80131040) "

El emulador de proceso asigna una dirección IP local al extremo. Puede encontrar la dirección IP visualizando la interfaz de usuario del emulador de proceso. Haga clic con el botón derecho en el icono del emulador en el área de notificación de la barra de tareas y seleccione **Mostrar interfaz de usuario del emulador de proceso**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image11.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image10.png)

Busque la dirección IP en implementaciones de servicio, implementación [id], detalles del servicio. Abra un explorador Web y vaya a la<em>dirección</em>http:///Test/1, donde <em>Dirección</em> es la dirección IP asignada por el emulador de proceso. por ejemplo, `http://127.0.0.1:80/test/1`. Debería ver la respuesta del controlador de API Web:

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image12.png)

## <a name="deploy-to-azure"></a>Implementar en Azure

En este paso, debe tener una cuenta de Azure. Si aún no tiene una, puede crear una cuenta de evaluación gratuita en un par de minutos. Para obtener más información, consulte [evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

En Explorador de soluciones, haga clic con el botón secundario en el proyecto AzureApp. Seleccione **Publicar**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image13.png)

Si no ha iniciado sesión en su cuenta de Azure, haga clic en **iniciar sesión**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image15.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image14.png)

Una vez iniciada la sesión, elija una suscripción y haga clic en **siguiente**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image17.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image16.png)

Escriba un nombre para el servicio en la nube y elija una región. Haga clic en **Crear**.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image18.png)

Haga clic en **Publicar**.

[![](host-aspnet-web-api-in-an-azure-worker-role/_static/image20.png)](host-aspnet-web-api-in-an-azure-worker-role/_static/image19.png)

La ventana registro de actividad de Azure muestra el progreso de la implementación. Cuando se implemente la aplicación, vaya a http://appname.cloudapp.net/test/1.

![](host-aspnet-web-api-in-an-azure-worker-role/_static/image21.png)

## <a name="additional-resources"></a>Recursos adicionales

- [Información general del proyecto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)
- [Proyecto Katana en GitHub](https://github.com/aspnet/AspNetKatana)
