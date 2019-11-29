---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Configuración de un servidor web para la publicación de Web Deploy (implementación sin conexión) | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo configurar un servidor Web de IIS para que admita la publicación e implementación web sin conexión. Al trabajar con Internet Information Services (I...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: f93cf11085fb19afb97b71aca8f638bd88fe658b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621096"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Configurar un servidor web para la publicación de la implementación web (implementación sin conexión)

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar un servidor Web de IIS para que admita la publicación e implementación web sin conexión.
> 
> Cuando trabaja con la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy) 2,0 o una versión posterior, existen tres enfoques principales que puede usar para obtener sus aplicaciones o sitios en un servidor Web. Puede:
> 
> - Use el *servicio agente remoto de web deploy*. Este enfoque requiere menos configuración del servidor Web, pero debe proporcionar las credenciales de un administrador del servidor local para poder implementar cualquier cosa en el servidor.
> - Use el *controlador de web deploy*. Este enfoque es mucho más complejo y requiere más esfuerzo inicial para configurar el servidor Web. Sin embargo, si usa este enfoque, puede configurar IIS para permitir que los usuarios que no son administradores realicen la implementación. El controlador de Web Deploy solo está disponible en la versión 7 o posterior de IIS.
> - Use la *implementación sin conexión*. Este enfoque requiere la menor configuración del servidor Web, pero un administrador del servidor debe copiar manualmente el paquete Web en el servidor e importarlo a través del administrador de IIS.
> 
> Para obtener más información sobre las características clave, las ventajas y las desventajas de estos enfoques, consulte [elección del enfoque adecuado para la implementación web](choosing-the-right-approach-to-web-deployment.md).

Sí, si la infraestructura de red o las restricciones de seguridad impiden la implementación remota. Lo más probable es que sea el caso de los entornos de producción orientados a Internet, donde los servidores&#x2014;web están aislados físicamente o mediante firewalls y&#x2014;subredes del resto de la infraestructura de servidor.

Obviamente, este enfoque es menos deseable si las aplicaciones web se actualizan de forma periódica. Si su infraestructura lo permite, puede que desee considerar la posibilidad de habilitar la implementación remota, mediante el controlador de Web Deploy o el servicio del agente remoto Web Deploy.

## <a name="task-overview"></a>Información general sobre tareas

Para configurar el servidor web para que admita la importación y la implementación sin conexión de paquetes Web, deberá hacer lo siguiente:

- Instale IIS 7,5 y la configuración recomendada de IIS 7.
- Instale Web Deploy 2,1 o posterior.
- Cree un sitio web de IIS para hospedar el contenido implementado.
- Deshabilite el servicio de Deployment Agent Web.

Para hospedar la solución de ejemplo específicamente, también necesitará:

- Instale el .NET Framework 4,0.
- Instale ASP.NET MVC 3.

En este tema se muestra cómo realizar cada uno de estos procedimientos. En las tareas y los tutoriales de este tema se da por supuesto que se está iniciando una compilación de servidor limpio que ejecuta Windows Server 2008 R2. Antes de continuar, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido a un dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información acerca de cómo unir equipos a un dominio, consulte [unir equipos al dominio e inicio de sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre la configuración de direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Instalar productos y componentes

Esta sección le guiará a través de la instalación de los productos y componentes necesarios en el servidor Web. Antes de empezar, se recomienda ejecutar Windows Update para asegurarse de que el servidor está totalmente actualizado.

En este caso, debe instalar estos elementos:

- **Configuración recomendada de IIS 7**. Esto habilita el rol de **servidor Web (IIS)** en el servidor web e instala el conjunto de módulos y componentes de IIS necesarios para hospedar una aplicación ASP.net.
- **.NET Framework 4,0**. Esto es necesario para ejecutar aplicaciones que se compilaron en esta versión del .NET Framework.
- **Herramienta de implementación Web 2,1 o posterior**. Esto instala Web Deploy (y su archivo ejecutable subyacente, MSDeploy. exe) en el servidor. Web Deploy se integra con IIS y le permite importar y exportar paquetes Web.
- **ASP.NET MVC 3**. Esto instala los ensamblados que necesita para ejecutar aplicaciones MVC 3.

> [!NOTE]
> En este tutorial se describe el uso del instalador de plataforma web para instalar y configurar varios componentes. Aunque no tiene que usar el instalador de plataforma web, simplifica el proceso de instalación mediante la detección automática de las dependencias y la garantía de que siempre obtiene las versiones más recientes del producto. Para obtener más información, consulte [Instalador de plataforma web de Microsoft 3,0](https://go.microsoft.com/?linkid=9805118).

**Para instalar los productos y componentes necesarios**

1. Descargue e instale el [instalador de plataforma web](https://go.microsoft.com/?linkid=9805118).
2. Una vez completada la instalación, el instalador de plataforma web se iniciará automáticamente.

    > [!NOTE]
    > Ahora puede iniciar el instalador de plataforma web en cualquier momento desde el menú **Inicio** . Para ello, en el menú **Inicio** , haga clic en **todos los programas**y, a continuación, haga clic en **instalador de plataforma web de Microsoft**.
3. En la parte superior de la ventana **instalador de plataforma Web 3,0** , haga clic en **productos**.
4. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **marcos de trabajo**.
5. En la fila **Microsoft .NET Framework 4** , si el .NET Framework no está ya instalado, haga clic en **Agregar**.

    > [!NOTE]
    > Es posible que ya haya instalado el .NET Framework 4,0 a través de Windows Update. Si ya hay instalado un producto o componente, el instalador de plataforma web lo indicará reemplazando el botón **Agregar** con el texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. En la fila **ASP.NET MVC 3 (Visual Studio 2010)** , haga clic en **Agregar**.
7. En el panel de navegación, haga clic en **servidor**.
8. En la fila **configuración recomendada de IIS 7** , haga clic en **Agregar**.
9. En la fila **herramienta de implementación Web 2,1** , haga clic en **Agregar**.
10. Haga clic en **Instalar**. El instalador de plataforma web mostrará una lista de productos&#x2014;junto con las dependencias asociadas&#x2014;que se van a instalar y le pedirá que acepte los términos de licencia.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Revise los términos de licencia y, si da su consentimiento a los términos **, haga clic en Acepto.**
12. Una vez completada la instalación, haga clic en **Finalizar**y, a continuación, cierre la ventana **instalador de plataforma web 3,0** .

Si instaló el .NET Framework 4,0 antes de instalar IIS, deberá ejecutar la herramienta de registro de [IIS de ASP.net](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) para registrar la versión más reciente de ASP.net con IIS. Si no lo hace, descubrirá que IIS servirá contenido estático (como archivos HTML) sin problemas, pero devolverá el **error HTTP 404,0 – no encontrado** al intentar examinar el contenido de ASP.net. Puede usar el procedimiento siguiente para asegurarse de que ASP.NET 4,0 está registrado.

**Para registrar ASP.NET 4,0 con IIS**

1. Haga clic en **Inicio**y, a continuación, escriba **símbolo del sistema**.
2. En los resultados de la búsqueda, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**.
3. En la ventana del símbolo del sistema, navegue hasta el directorio **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**
4. Escriba este comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Si planea hospedar aplicaciones Web de 64 bits en cualquier momento, debe registrar también la versión de 64 bits de ASP.NET con IIS. Para ello, en la ventana del símbolo del sistema, navegue hasta el directorio **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**
6. Escriba este comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Como práctica recomendada, vuelva a usar Windows Update en este momento para descargar e instalar las actualizaciones disponibles para los nuevos productos y componentes que ha instalado.

## <a name="configure-the-iis-website"></a>Configurar el sitio web de IIS

Para poder implementar contenido web en el servidor, debe crear y configurar un sitio web de IIS para hospedar el contenido. Web Deploy solo puede implementar paquetes Web en un sitio web de IIS existente; no puede crear el sitio Web. En un nivel alto, deberá completar estas tareas:

- Cree una carpeta en el sistema de archivos para hospedar el contenido.
- Cree un sitio web de IIS para servir el contenido y asócielo a la carpeta local.
- Conceda permisos de lectura a la identidad del grupo de aplicaciones en la carpeta local.

Aunque no hay nada que le impida implementar contenido en el sitio web predeterminado en IIS, este enfoque no se recomienda para otros escenarios de prueba o demostración. Para simular un entorno de producción, debe crear un nuevo sitio web de IIS con la configuración específica de los requisitos de la aplicación.

**Para crear y configurar un sitio web de IIS**

1. En el sistema de archivos local, cree una carpeta para almacenar el contenido (por ejemplo, **C:\DemoSite**).
2. En el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en Administrador de **Internet Information Services (IIS)** .
3. En el administrador de IIS, en el panel **conexiones** , expanda el nodo del servidor (por ejemplo, **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Haga clic con el botón secundario en el nodo **sitios** y, a continuación, haga clic en **Agregar sitio web**.
5. En el cuadro **nombre del sitio** , escriba un nombre para el sitio web de IIS (por ejemplo, **DemoSite**).
6. En el cuadro **ruta de acceso física** , escriba (o busque) la ruta de acceso a la carpeta local (por ejemplo, **C:\DemoSite**).
7. En el cuadro **Puerto** , escriba el número de puerto en el que desea hospedar el sitio web (por ejemplo, **85**).

    > [!NOTE]
    > Los números de puerto estándar son 80 para HTTP y 443 para HTTPS. Sin embargo, si hospeda este sitio web en el puerto 80, deberá detener el sitio web predeterminado para poder acceder a su sitio.
8. Deje en blanco el cuadro **nombre de host** , a menos que desee configurar un registro del sistema de nombres de dominio (DNS) para el sitio web y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > En un entorno de producción, probablemente querrá hospedar el sitio web en el puerto 80 y configurar un encabezado de host, junto con los registros DNS coincidentes. Para obtener más información sobre cómo configurar los encabezados de host en IIS 7, vea [configurar un encabezado de host para un sitio web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Para obtener más información sobre el rol de servidor DNS en Windows Server 2008 R2, consulte [Introducción](https://technet.microsoft.com/library/cc770392.aspx) al servidor DNS y [servidor DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. En el panel **acciones** , en **Editar sitio**, haga clic en **enlaces**.
10. En el cuadro de diálogo **enlaces de sitios** , haga clic en **Agregar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. En el cuadro de diálogo **Agregar enlace de sitio** , establezca la **dirección IP** y el **Puerto** para que coincidan con la configuración de sitio existente.
12. En el cuadro **nombre de host** , escriba el nombre del servidor Web (por ejemplo, **PROWEB1**) y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > El primer enlace de sitio le permite tener acceso al sitio localmente mediante la dirección IP y el puerto o el `http://localhost:85`. El segundo enlace de sitio le permite tener acceso al sitio desde otros equipos del dominio mediante el nombre de equipo (por ejemplo, http://proweb1:85).
13. En el cuadro de diálogo **enlaces de sitios** , haga clic en **cerrar**.
14. En el panel **conexiones** , haga clic en **grupos de aplicaciones**.
15. En el panel **grupos de aplicaciones** , haga clic con el botón secundario en el nombre del grupo de aplicaciones y, a continuación, haga clic en **configuración básica**. De forma predeterminada, el nombre del grupo de aplicaciones coincidirá con el nombre del sitio web (por ejemplo, **DemoSite**).
16. En la lista **.NET Framework versión** , seleccione **.NET Framework v 4.0.30319**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > La solución de ejemplo requiere .NET Framework 4,0. Esto no es un requisito para Web Deploy en general.

Para que el sitio web atienda el contenido, la identidad del grupo de aplicaciones debe tener permisos de lectura en la carpeta local que almacena el contenido. En IIS 7,5, los grupos de aplicaciones se ejecutan con una identidad de grupo de aplicaciones única de forma predeterminada (a diferencia de las versiones anteriores de IIS, donde los grupos de aplicaciones normalmente se ejecutaban con la cuenta de servicio de red). La identidad del grupo de aplicaciones no es una cuenta de usuario real y no aparece en ninguna lista de usuarios o&#x2014;grupos, sino que se crea dinámicamente cuando se inicia el grupo de aplicaciones. Cada identidad del grupo de aplicaciones se agrega al grupo de seguridad local de **IIS\_IUSRS** como un elemento oculto.

Para conceder permisos a una identidad del grupo de aplicaciones en un archivo o carpeta, tiene dos opciones:

- Asigne permisos directamente a la identidad del grupo de aplicaciones, con el formato <strong>IIS AppPool\</strong ><em>[nombre del grupo de aplicaciones]</em>(por ejemplo, <strong>IIS AppPool\DemoSite</strong>).
- Asigne permisos al grupo **IUSRS de IIS\_** .

El enfoque más común consiste en asignar permisos al grupo local de **IIS\_IUSRS** , ya que este enfoque permite cambiar los grupos de aplicaciones sin tener que volver a configurar los permisos del sistema de archivos. En el procedimiento siguiente se usa este enfoque basado en grupos.

> [!NOTE]
> Para obtener más información sobre las identidades del grupo de aplicaciones en IIS 7,5, vea [identidades del grupo de aplicaciones](https://go.microsoft.com/?linkid=9805123).

**Para configurar permisos de carpeta para un sitio web de IIS**

1. En el explorador de Windows, vaya a la ubicación de la carpeta local.
2. Haga clic con el botón secundario en la carpeta y, a continuación, haga clic en **propiedades**.
3. En la pestaña **seguridad** , haga clic en **Editar**y, a continuación, haga clic en **Agregar**.
4. Haga clic en **ubicaciones**. En el cuadro de diálogo **ubicaciones** , seleccione el servidor local y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. En el cuadro de diálogo **Seleccionar usuarios o grupos** , escriba **IIS\_IUSRS**, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.
6. En el cuadro de diálogo <strong>permisos de</strong><em>[nombre de carpeta]</em> , observe que el nuevo grupo tiene asignados los permisos <strong>leer &amp; ejecutar</strong>, <strong>Mostrar el contenido</strong>de la carpeta y <strong>leer</strong> de forma predeterminada. Déjelo sin cambios y haga clic en <strong>Aceptar</strong>.
7. Haga clic en <strong>Aceptar</strong> para cerrar el cuadro de diálogo<strong>propiedades</strong> de <em>[nombre de carpeta]</em>.

## <a name="disable-the-remote-agent-service"></a>Deshabilitar el servicio de agente remoto

Al instalar Web Deploy, el servicio de Deployment Agent Web se instala y se inicia automáticamente. Este servicio le permite implementar y publicar paquetes web desde una ubicación remota. No utilizará la funcionalidad de implementación remota en este escenario, por lo que debe detener y deshabilitar el servicio.

> [!NOTE]
> No es necesario detener el servicio del agente remoto para poder importar e implementar un paquete Web manualmente. Sin embargo, es recomendable detener y deshabilitar el servicio si no tiene previsto usarlo.

Puede detener y deshabilitar un servicio de varias maneras, mediante el uso de varias utilidades de línea de comandos o cmdlets de Windows PowerShell. Este procedimiento describe un enfoque sencillo basado en la interfaz de usuario.

**Para detener y deshabilitar el servicio de agente remoto**

1. En el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en **servicios**.
2. En la consola servicios, busque la fila **servicio de Deployment Agent web** .

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Haga clic con el botón secundario en **servicio de Deployment Agent web**y, a continuación, haga clic en **propiedades**.
4. En el cuadro de diálogo **propiedades del servicio de Deployment Agent web** , haga clic en **detener**.
5. En la lista **tipo de inicio** , seleccione **deshabilitado**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Conclusión

En este momento, el servidor web está listo para la implementación de paquetes Web sin conexión. Antes de intentar importar paquetes Web en un sitio web de IIS, puede que desee comprobar estos puntos clave:

- ¿Ha registrado ASP.NET 4,0 con IIS?
- ¿La identidad del grupo de aplicaciones tiene acceso de lectura a la carpeta de origen para el sitio web?
- ¿Ha detenido el servicio Web Deployment Agent?

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [Siguiente](configuring-a-database-server-for-web-deploy-publishing.md)
