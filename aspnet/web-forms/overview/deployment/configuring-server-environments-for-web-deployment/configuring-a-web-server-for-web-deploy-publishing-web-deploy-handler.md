---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: Configuración de un servidor web para la publicación de Web Deploy (controlador de Web Deploy) | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo configurar un servidor Web de Internet Information Services (IIS) para admitir la publicación e implementación web mediante el uso de la Web Deploy ha...
ms.author: riande
ms.date: 01/29/2017
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: baaebd32f08d3c6b861572c5c5a16ec0fb70aaf0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589040"
---
# <a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Configurar un servidor web para la publicación de la implementación web (controlador de implementación web)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar un servidor Web de Internet Information Services (IIS) para admitir la publicación e implementación web mediante el controlador de Web Deploy de IIS.
> 
> Cuando trabaja con Web Deploy 2,0 o posterior, hay tres enfoques principales que puede usar para obtener sus aplicaciones o sitios en un servidor Web. Puede:
> 
> - Use el *servicio agente remoto de web deploy*. Este enfoque requiere menos configuración del servidor Web, pero debe proporcionar las credenciales de un administrador del servidor local para poder implementar cualquier cosa en el servidor.
> - Use el *controlador de web deploy*. Este enfoque es mucho más complejo y requiere más esfuerzo inicial para configurar el servidor Web. Sin embargo, si usa este enfoque, puede configurar IIS para permitir que los usuarios que no son administradores realicen la implementación. El controlador de Web Deploy solo está disponible en la versión 7 o posterior de IIS.
> - Use la *implementación sin conexión*. Este enfoque requiere la menor configuración del servidor Web, pero un administrador del servidor debe copiar manualmente el paquete Web en el servidor e importarlo a través del administrador de IIS.
> 
> Para obtener más información sobre las características clave, las ventajas y las desventajas de estos enfoques, consulte [elección del enfoque adecuado para la implementación web](choosing-the-right-approach-to-web-deployment.md).

Sí, si desea permitir que los usuarios que no son administradores implementen contenido en sitios web de IIS específicos. Este enfoque suele ser conveniente en estos tipos de escenarios:

- Entornos de ensayo o de producción, en los que es improbable que la persona o la cuenta de servicio que desencadena la implementación remota tenga acceso a las credenciales de un administrador del servidor.
- Entornos hospedados, donde desea proporcionar a los usuarios remotos la capacidad de actualizar sus sitios web sin concederles control total sobre los servidores Web (o el acceso a los sitios web de otros usuarios).

En escenarios de desarrollo o prueba, o en organizaciones más pequeñas, la implementación de contenido con credenciales de administrador de servidor suele ser menos polémico. En estos casos, la configuración de los servidores web para admitir la implementación mediante el [servicio del agente remoto de web deploy](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) ofrece un enfoque más sencillo.

## <a name="task-overview"></a>Información general sobre tareas

Para configurar el servidor web para que acepte e implemente paquetes web desde un equipo remoto mediante el enfoque de controlador de Web Deploy, necesitará:

- Cree o elija una cuenta de usuario de dominio (el "usuario sin privilegios de administrador") cuyas credenciales usará para realizar las implementaciones.
- Instale IIS 7,5, incluido el servicio de administración web y el módulo de autenticación básica.
- Instale Web Deploy 2,1 o posterior.
- Configure el servicio de administración web para permitir las conexiones remotas e iniciar el servicio.
- Cree un sitio web de IIS para hospedar el contenido implementado.
- Conceda permisos de usuario sin privilegios de administrador en el sitio web del administrador de IIS.
- Asegúrese de que las reglas de delegación del servicio de administración web permiten al servicio agregar y cambiar el contenido del sitio web mediante la cuenta de usuario que no es de administrador.
- Configure los firewalls para permitir las conexiones entrantes en el puerto 8172.

Para hospedar la solución de ejemplo ContactManager específicamente, también necesitará:

- Instale el .NET Framework 4,0.
- Instale ASP.NET MVC 3.

En este tema se muestra cómo realizar cada uno de estos procedimientos. En las tareas y los tutoriales de este tema se da por supuesto que se está iniciando una compilación de servidor limpio que ejecuta Windows Server 2016. Antes de continuar, asegúrese de que:

- Windows Server 2016
- El servidor está unido a un dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información acerca de cómo unir equipos a un dominio, consulte [unir equipos al dominio e inicio de sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre la configuración de direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="install-products-and-components"></a>Instalar productos y componentes

Esta sección le guiará a través de la instalación de los productos y componentes necesarios en el servidor Web. Antes de empezar, se recomienda ejecutar Windows Update para asegurarse de que el servidor está totalmente actualizado.

En este caso, debe instalar estos elementos:

- **Configuración recomendada de IIS 7**. Esto habilita el rol de **servidor Web (IIS)** en el servidor web e instala el conjunto de módulos y componentes de IIS necesarios para hospedar una aplicación ASP.net.
- **IIS: servicio de administración**de. Esto instala el servicio de administración web (WMSvc) en IIS. Este servicio permite la administración remota de sitios web de IIS y expone el punto de conexión del controlador de Web Deploy a los clientes.
- **IIS: autenticación básica**. Esto instala el módulo de autenticación básica de IIS. Esto permite al servicio de administración web (WMSvc) autenticar las credenciales proporcionadas.
- **Herramienta de implementación Web 2,1 o posterior**. Esto instala Web Deploy (y su archivo ejecutable subyacente, MSDeploy. exe) en el servidor. Como parte de este proceso, instala el controlador de Web Deploy y lo integra con el servicio de administración web.
- **.NET Framework 4,0**. Esto es necesario para ejecutar aplicaciones que se compilaron en esta versión del .NET Framework.
- **ASP.NET MVC 3**. Esto instala los ensamblados que necesita para ejecutar aplicaciones MVC 3.

> [!NOTE]
> En este tutorial se describe el uso del instalador de plataforma web para instalar y configurar varios componentes. Aunque no tiene que usar el instalador de plataforma web, simplifica el proceso de instalación mediante la detección automática de las dependencias y la garantía de que siempre obtiene las versiones más recientes del producto. Para obtener más información, vea [instalador de plataforma web de Microsoft](https://go.microsoft.com/?linkid=9805118).

**Para instalar los productos y componentes necesarios**

1. Descargue e instale el [instalador de plataforma web](https://go.microsoft.com/?linkid=9805118).
2. Una vez completada la instalación, el instalador de plataforma web se iniciará automáticamente.

    > [!NOTE]
    > Ahora puede iniciar el instalador de plataforma web en cualquier momento desde el menú **Inicio** . Para ello, en el menú **Inicio** , haga clic en **todos los programas**y, a continuación, haga clic en **instalador de plataforma web de Microsoft**.
3. En la parte superior de la ventana **instalador de plataforma web** , haga clic en **productos**.
4. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **marcos de trabajo**.
5. En la fila **Microsoft .NET Framework 4** , si el .NET Framework no está ya instalado, haga clic en **Agregar**.

    > [!NOTE]
    > Es posible que ya haya instalado el .NET Framework 4,0 a través de Windows Update. Si ya hay instalado un producto o componente, el instalador de plataforma web lo indicará reemplazando el botón **Agregar** con el texto **instalado**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. En la fila **ASP.NET MVC 3 (Visual Studio 2010)** , haga clic en **Agregar**.
7. En el panel de navegación, haga clic en **servidor**.
8. En la fila **configuración recomendada de IIS 7** , haga clic en **Agregar**.
9. En la fila **herramienta de implementación Web 2,1** , haga clic en **Agregar**.
10. En la fila **IIS: autenticación básica** , haga clic en **Agregar**.
11. En la fila **IIS: servicio de administración** , haga clic en **Agregar**.
12. Haga clic en **Instalar**. El instalador de plataforma web mostrará una lista de productos&#x2014;junto con las dependencias asociadas&#x2014;que se van a instalar y le pedirá que acepte los términos de licencia.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Revise los términos de licencia y, si da su consentimiento a los términos **, haga clic en Acepto.**
14. Una vez completada la instalación, haga clic en **Finalizar**y, a continuación, cierre la ventana **instalador de plataforma web** .

Si instaló el .NET Framework 4,0 antes de instalar IIS, deberá ejecutar la herramienta de registro de [IIS de ASP.net](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (ASPNET\_regiis. exe) para registrar la versión más reciente de ASP.net con IIS. Si no lo hace, descubrirá que IIS servirá contenido estático (como archivos HTML) sin problemas, pero devolverá el **error HTTP 404,0 – no encontrado** al intentar examinar el contenido de ASP.net. Puede usar el procedimiento siguiente para asegurarse de que ASP.NET 4,0 está registrado.

**Para registrar ASP.NET 4,0 con IIS**

1. Haga clic en **Inicio**y, a continuación, escriba **símbolo del sistema**.
2. En los resultados de la búsqueda, haga clic con el botón secundario en **símbolo del sistema**y, a continuación, haga clic en **Ejecutar como administrador**.
3. En la ventana del símbolo del sistema, navegue hasta el directorio **%WINDIR%\Microsoft.NET\Framework\v4.0.30319**
4. Escriba este comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Si planea hospedar aplicaciones Web de 64 bits en cualquier momento, debe registrar también la versión de 64 bits de ASP.NET con IIS. Para ello, en la ventana del símbolo del sistema, navegue hasta el directorio **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319**
6. Escriba este comando y, a continuación, presione ENTRAR:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Como práctica recomendada, vuelva a usar Windows Update en este momento para descargar e instalar las actualizaciones disponibles para los nuevos productos y componentes que ha instalado.

## <a name="configure-the-web-management-service"></a>Configurar el servicio de administración web

Ahora que ha instalado todo lo que necesita, el siguiente paso es configurar el servicio de administración web en IIS. En un nivel alto, deberá completar estas tareas:

- Habilitar la autenticación básica en el nivel de servidor.
- Configure el servicio de administración web para que acepte conexiones remotas.
- Inicie el servicio de administración de Web.
- Compruebe que las reglas de delegación del servicio de administración web necesarias estén vigentes.

**Para configurar el servicio de administración web**

1. En el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en Administrador de **Internet Information Services (IIS)** .
2. En el administrador de IIS, en el panel **conexiones** , haga clic en el nodo del servidor (por ejemplo, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. En el panel central, en **IIS**, haga doble clic en **autenticación**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image20.png)
4. Haga clic con el botón secundario en **autenticación básica**y, a continuación, haga clic en **Habilitar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. En el panel **conexiones** , vuelva a hacer clic en el nodo del servidor para volver a la configuración de nivel superior.
6. En el panel central, en **Administración**, haga doble clic en **servicio de administración**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. En el panel central, seleccione **Habilitar conexiones remotas**.

    > [!NOTE]
    > Si el servicio de administración web ya se está ejecutando, deberá detenerlo en primer lugar.
8. En el panel **acciones** , haga clic en **iniciar** para iniciar el servicio de administración web.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Si se le pide que guarde la configuración, haga clic en **sí**.

    > [!NOTE]
    > También es posible que desee configurar el servicio para que se inicie automáticamente. Para ello, abra la consola servicios, haga clic con el botón secundario en **servicio de administración web**y, a continuación, haga clic en **propiedades**. En la lista desplegable **tipo de inicio** , seleccione **automático**y, a continuación, haga clic en **Aceptar**.
10. En el panel **conexiones** , vuelva a hacer clic en el nodo del servidor para volver a la configuración de nivel superior.
11. En el panel central, en **Administración**, haga doble clic en **delegación del servicio de administración**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Compruebe que el panel central contiene un conjunto de reglas.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Estas reglas permiten a los usuarios del servicio de administración web autorizado usar varios proveedores de Web Deploy. Por ejemplo, para implementar aplicaciones web y contenido en IIS mediante el controlador de Web Deploy, debe haber una regla de delegación que permita a todos los usuarios del servicio de administración web autenticado usar los proveedores **contentPath** y **iisApp** (la última regla que se puede ver en la captura de pantalla).

    Si instaló productos y componentes en el orden descrito en este tema, la versión más reciente de Web Deploy debe agregar automáticamente todas las reglas de delegación necesarias al servicio de administración web. Si la página de delegación del servicio de administración no muestra ninguna regla, deberá crearlas usted mismo. Para obtener instrucciones sobre cómo hacerlo, vea [configurar el controlador de implementación web](https://go.microsoft.com/?linkid=9805124).
13. En el panel **conexiones** , vuelva a hacer clic en el nodo del servidor para volver a la configuración de nivel superior.

## <a name="create-and-configure-an-iis-website"></a>Crear y configurar un sitio web de IIS

Para poder implementar contenido web en el servidor, debe crear y configurar un sitio web de IIS para hospedar el contenido. Web Deploy solo puede implementar paquetes Web en un sitio web de IIS existente; no puede crear el sitio Web. También debe realizar una pequeña configuración adicional para permitir que la cuenta que no sea de administrador implemente contenido de forma remota. En un nivel alto, deberá completar estas tareas:

- Cree una carpeta en el sistema de archivos para hospedar el contenido.
- Cree un sitio web de IIS para servir el contenido y asócielo a la carpeta local.
- Conceda permisos de lectura a la identidad del grupo de aplicaciones en la carpeta local.
- Conceda los permisos de IIS necesarios a la cuenta de dominio que va a implementar la aplicación Web.

Aunque no hay nada que le impida implementar contenido en el sitio web predeterminado en IIS, este enfoque no se recomienda para otros escenarios de prueba o demostración. Para simular un entorno de producción, debe crear un nuevo sitio web de IIS con la configuración específica de los requisitos de la aplicación.

**Para crear un sitio web de IIS**

1. En el sistema de archivos local, cree una carpeta para almacenar el contenido (por ejemplo, **C:\DemoSite**).
2. En el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en Administrador de **Internet Information Services (IIS)** .
3. En el administrador de IIS, en el panel **conexiones** , expanda el nodo del servidor (por ejemplo, **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Haga clic con el botón secundario en el nodo **sitios** y, a continuación, haga clic en **Agregar sitio web**.
5. En el cuadro **nombre del sitio** , escriba un nombre para el sitio web de IIS (por ejemplo, **DemoSite**).
6. En el cuadro **ruta de acceso física** , escriba (o busque) la ruta de acceso a la carpeta local (por ejemplo, **C:\DemoSite**).
7. En el cuadro **Puerto** , escriba el número de puerto en el que desea hospedar el sitio web (por ejemplo, **85**).

    > [!NOTE]
    > Los números de puerto estándar son 80 para HTTP y 443 para HTTPS. Sin embargo, si hospeda este sitio web en el puerto 80, deberá detener el sitio web predeterminado para poder acceder a su sitio.
8. Deje en blanco el cuadro **nombre de host** , a menos que desee configurar un registro del sistema de nombres de dominio (DNS) para el sitio web y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > En un entorno de producción, probablemente querrá hospedar el sitio web en el puerto 80 y configurar un encabezado de host, junto con los registros DNS coincidentes. Para obtener más información sobre cómo configurar los encabezados de host en IIS 7, vea [configurar un encabezado de host para un sitio web (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Para obtener más información sobre el rol de servidor DNS en Windows Server, consulte [Introducción](https://technet.microsoft.com/library/cc770392.aspx) al servidor DNS y [servidor DNS](https://technet.microsoft.com/windowsserver/dd448607).
9. En el panel **acciones** , en **Editar sitio**, haga clic en **enlaces**.
10. En el cuadro de diálogo **enlaces de sitios** , haga clic en **Agregar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. En el cuadro de diálogo **Agregar enlace de sitio** , establezca la **dirección IP** y el **Puerto** para que coincidan con la configuración de sitio existente.
12. En el cuadro **nombre de host** , escriba el nombre del servidor Web (por ejemplo, **STAGEWEB1**) y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > El primer enlace de sitio le permite tener acceso al sitio localmente mediante la dirección IP y el puerto o el `http://localhost:85`. El segundo enlace de sitio le permite tener acceso al sitio desde otros equipos del dominio mediante el nombre de equipo (por ejemplo, http://stageweb1:85).
13. En el cuadro de diálogo **enlaces de sitios** , haga clic en **cerrar**.
14. En el panel **conexiones** , haga clic en **grupos de aplicaciones**.
15. En el panel **grupos de aplicaciones** , haga clic con el botón secundario en el nombre del grupo de aplicaciones y, a continuación, haga clic en **configuración básica**. De forma predeterminada, el nombre del grupo de aplicaciones coincidirá con el nombre del sitio web (por ejemplo, **DemoSite**).
16. En la lista **versión de .net CLR** , seleccione **.net CLR v 4.0.30319**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image21.png)

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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. En el cuadro de diálogo **Seleccionar usuarios o grupos** , escriba **IIS\_IUSRS**, haga clic en **Comprobar nombres**y, a continuación, haga clic en **Aceptar**.
6. En el cuadro de diálogo <strong>permisos de</strong><em>[nombre de carpeta]</em> , observe que el nuevo grupo tiene asignados los permisos <strong>leer &amp; ejecutar</strong>, <strong>Mostrar el contenido</strong>de la carpeta y <strong>leer</strong> de forma predeterminada. Déjelo sin cambios y haga clic en <strong>Aceptar</strong>.
7. Haga clic en <strong>Aceptar</strong> para cerrar el cuadro de diálogo<strong>propiedades</strong> de <em>[nombre de carpeta]</em>.

Como tarea final, debe conceder los permisos adecuados al usuario que no sea administrador cuyas credenciales usará para implementar el contenido. Este usuario requiere permisos para implementar contenido de forma remota en el sitio Web.

**Para configurar los permisos del sitio web de IIS para un usuario de dominio que no sea de administrador**

1. En el administrador de IIS, en el panel **conexiones** , haga clic con el botón secundario en el nodo de su sitio web (por ejemplo, **DemoSite**), elija **implementar**y, a continuación, haga clic en **configurar web deploy publicación**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. En el cuadro de diálogo **configurar publicación de web deploy** , a la derecha de la lista **Seleccione un usuario para dar permisos de publicación** , haga clic en el botón de puntos suspensivos.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. En el cuadro de diálogo **permitir usuario** , escriba el dominio y el nombre de usuario de la cuenta que desea usar para implementar el contenido y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. En el cuadro de diálogo **configurar publicación de web deploy** , haga clic en **configuración**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Esta operación realiza dos funciones clave en un solo paso. En primer lugar, concede al usuario permiso para modificar el sitio web de forma remota a través del servicio de administración web, de acuerdo con las reglas de delegación que examinó en la sección anterior. En segundo lugar, concede al usuario control total de la carpeta de origen para el sitio web, lo que permite al usuario agregar, modificar y establecer permisos en el contenido del sitio Web.
5. En el cuadro de diálogo **configurar publicación de web deploy** , haga clic en **cerrar**.

## <a name="configure-firewall-exceptions"></a>Configurar excepciones de Firewall

De forma predeterminada, el servicio de administración web de IIS escucha en el puerto TCP 8172. Si el Firewall de Windows está habilitado en el servidor Web, deberá crear una nueva regla de entrada para permitir el tráfico TCP en el puerto 8172 (de forma predeterminada, se permite todo el tráfico saliente en el Firewall de Windows). Si usa un firewall de terceros, debe crear reglas para permitir el tráfico.

| Direction | Desde el puerto | A Puerto | Tipo de puerto |
| --- | --- | --- | --- |
| Entrada | Cualquiera | 8172 | TCP |
| Salida | 8172 | Cualquiera | TCP |

Para obtener más información sobre la configuración de reglas en firewall de Windows, consulte [configuración de reglas de Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). En el caso de los firewalls de terceros, consulte la documentación del producto.

## <a name="conclusion"></a>Conclusión

El servidor web debería estar ahora preparado para aceptar implementaciones remotas en el controlador de Web Deploy a través del servicio de administración web. Antes de intentar implementar una aplicación web en el servidor, puede que desee comprobar estos puntos clave:

- ¿Ha habilitado la autenticación básica en el nivel de servidor en IIS?
- ¿Ha habilitado las conexiones remotas al servicio de administración web?
- ¿Ha iniciado el servicio de administración web?
- ¿Existen reglas de delegación del servicio de administración?
- ¿La identidad del grupo de aplicaciones tiene acceso de lectura a la carpeta de origen para el sitio web?
- ¿La cuenta de usuario que no es de administrador tiene permisos de nivel de sitio en IIS?
- ¿El Firewall permite las conexiones entrantes al servidor en el puerto TCP 8172?

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre cómo configurar archivos de proyecto de Microsoft Build Engine personalizado (MSBuild) para implementar paquetes Web en el controlador de Web Deploy, consulte [configuración de las propiedades de implementación para un entorno de destino](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
> [Siguiente](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
