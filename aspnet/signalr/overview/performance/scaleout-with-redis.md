---
uid: signalr/overview/performance/scaleout-with-redis
title: Signalr ampliación con Redis | Microsoft Docs
author: bradygaster
description: Las versiones de software que se usan en este tema Visual Studio 2013 versiones anteriores de .NET 4,5 Signalr, versión 2 de este tema, para obtener información acerca de las versiones anteriores de...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467779"
---
# <a name="signalr-scaleout-with-redis"></a>Escalabilidad horizontal de SignalR con Redis

por [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2,4
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

En este tutorial, usará [Redis](http://redis.io/) para distribuir los mensajes a través de una aplicación signalr implementada en dos instancias de IIS independientes.

Redis es un almacén de clave-valor en memoria. También admite un sistema de mensajería con un modelo de publicación/suscripción. El backplane de ReDiS ReDiS usa la característica pub/sub para reenviar mensajes a otros servidores.

![](scaleout-with-redis/_static/image1.png)

En este tutorial, usará tres servidores:

- Dos servidores que ejecutan Windows, que se usarán para implementar una aplicación de Signalr.
- Un servidor que ejecuta Linux, que usará para ejecutar Redis. En el caso de las capturas de pantallas de este tutorial, usé Ubuntu 12,04 TLS.

Si no tiene tres servidores físicos para usar, puede crear máquinas virtuales en Hyper-V. Otra opción es crear máquinas virtuales en Azure.

Aunque en este tutorial se usa la implementación de Redis oficial, también hay un [Puerto de Windows de Redis](https://github.com/MSOpenTech/redis) desde MSOpenTech. La instalación y configuración son diferentes, pero, de lo contrario, los pasos son los mismos.

> [!NOTE]
>
> Signalr ampliación con Redis no admite clústeres de Redis.

## <a name="overview"></a>Información general

Antes de llegar al tutorial detallado, aquí se muestra una introducción rápida de lo que hará.

1. Instale Redis e inicie el servidor de Redis.
2. Agregue estos paquetes de NuGet a la aplicación:

    - [Microsoft. AspNet. Signalr](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft. AspNet. Signalr. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. Cree una aplicación Signalr.
4. Agregue el código siguiente a Startup.cs para configurar el backplane:

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a>Ubuntu en Hyper-V

Con Windows Hyper-V, puede crear fácilmente una máquina virtual de Ubuntu en Windows Server.

Descargue el ISO de Ubuntu desde [http://www.ubuntu.com](http://www.ubuntu.com/).

En Hyper-V, agregue una nueva máquina virtual. En el paso **Conectar disco duro virtual** , seleccione **crear un disco duro virtual**.

![](scaleout-with-redis/_static/image2.png)

En el paso **Opciones de instalación** , seleccione Archivo de **imagen (. ISO)** , haga clic en **examinar**y busque el ISO de instalación de Ubuntu.

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a>Instalación de Redis

Siga los pasos descritos en [http://redis.io/download](http://redis.io/download) para descargar y compilar Redis.

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

Esto crea los archivos binarios de Redis en el directorio de `src`.

De forma predeterminada, Redis no requiere una contraseña. Para establecer una contraseña, edite el archivo de `redis.conf`, que se encuentra en el directorio raíz del código fuente. (Hacer una copia de seguridad del archivo antes de editarlo) Agregue la siguiente directiva a `redis.conf`:

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

Ahora inicie el servidor de Redis:

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

Abra el puerto 6379, que es el puerto predeterminado en el que escucha Redis. (Puede cambiar el número de puerto en el archivo de configuración).

## <a name="create-the-signalr-application"></a>Creación de la aplicación Signalr

Cree una aplicación de Signalr siguiendo uno de estos tutoriales:

- [Introducción con Signalr 2,0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Introducción con Signalr 2,0 y MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

A continuación, modificaremos la aplicación de chat para admitir ampliación con Redis. En primer lugar, agregue el `Microsoft.AspNet.SignalR.StackExchangeRedis` paquete NuGet al proyecto. En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

A continuación, abra el archivo Startup.cs. Agregue el código siguiente al método de **configuración** :

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- "servidor" es el nombre del servidor que ejecuta Redis.
- *Port* es el número de Puerto
- "contraseña" es la contraseña que definió en el archivo Redis. conf.
- "AppName" es cualquier cadena. Signalr crea un canal pub/sub de Redis con este nombre.

Por ejemplo:

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a>Implementación y ejecución de la aplicación

Preparar las instancias de Windows Server para implementar la aplicación Signalr.

Agregue el rol de IIS. Incluya características de "desarrollo de aplicaciones", incluido el protocolo WebSocket.

![](scaleout-with-redis/_static/image5.png)

Incluya también el servicio de administración de (que aparece en "herramientas de administración").

![](scaleout-with-redis/_static/image6.png)

**Instale Web Deploy 3,0.** Al ejecutar el administrador de IIS, se le pedirá que instale la plataforma web de Microsoft, o bien puede [descargar el instalador](https://go.microsoft.com/fwlink/?LinkId=255386). En el instalador de plataforma, busque Web Deploy e instale Web Deploy 3,0

![](scaleout-with-redis/_static/image7.png)

Compruebe que el servicio de administración web se está ejecutando. Si no es así, inicie el servicio. (Si no ve servicio de administración web en la lista de servicios de Windows, asegúrese de que ha instalado el servicio de administración de al agregar el rol de IIS).

De forma predeterminada, el servicio de administración web escucha en el puerto TCP 8172. En firewall de Windows, cree una nueva regla de entrada para permitir el tráfico TCP en el puerto 8172. Para obtener más información, consulte [configuración de reglas de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). (Si hospeda las máquinas virtuales en Azure, puede hacerlo directamente en el Azure Portal. Consulte [configuración de puntos de conexión en una máquina virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/)).

Ahora está listo para implementar el proyecto de Visual Studio desde el equipo de desarrollo en el servidor. En Explorador de soluciones, haga clic con el botón secundario en la solución y haga clic en **publicar**.

Para obtener documentación más detallada sobre la implementación web, vea [mapa de contenido de implementación web para Visual Studio y ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).

Si implementa la aplicación en dos servidores, puede abrir cada instancia en una ventana del explorador independiente y ver que cada una de ellas recibe mensajes Signalr del otro. (Por supuesto, en un entorno de producción, los dos servidores se colocarían detrás de un equilibrador de carga).

![](scaleout-with-redis/_static/image8.png)

Si tiene curiosidad por ver los mensajes que se envían a Redis, puede usar el cliente **de Redis-CLI** , que se instala con Redis.

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
