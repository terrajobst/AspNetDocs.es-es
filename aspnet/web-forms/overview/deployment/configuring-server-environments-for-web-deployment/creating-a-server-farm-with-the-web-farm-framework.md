---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Crear una granja de servidores con el marco de granja de servidores Web | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo usar el marco de granja de servidores Web (WFF) 2,0 para crear y configurar una granja de servidores web a partir de una colección de servidores.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 204996514bed336e60ab77f184a923f04e7e2bba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78517501"
---
# <a name="creating-a-server-farm-with-the-web-farm-framework"></a>Crear una granja de servidores con Web Farm Framework

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo usar el marco de granja de servidores Web (WFF) 2,0 para crear y configurar una granja de servidores web a partir de una colección de servidores.

WFF le permite sincronizar productos y componentes de la plataforma web, aplicaciones Web, sitios web y valores de configuración en varios servidores web con equilibrio de carga. En escenarios en los que se necesita más de un servidor Web, como entornos de ensayo y producción, esto puede simplificar enormemente la implementación y el proceso de configuración. Puede implementar una aplicación web en un solo servidor&#x2014;el *servidor*&#x2014;principal y WFF replicará automáticamente la aplicación web en todos los demás servidores Web de la granja de servidores.

## <a name="understanding-the-web-farm-framework"></a>Descripción del marco de granja de servidores Web

Puede usar WFF 2,0 para aprovisionar, administrar e implementar contenido en un grupo de servidores Web. Una implementación de WFF consta de tres roles de servidor clave:

- El *servidor de controlador*. Este servidor se usa para crear y configurar granjas de servidores de WFF. El servidor de controlador administra la sincronización de componentes de plataforma web, opciones de configuración y aplicaciones entre los servidores Web en una granja de servidores. Instale WFF 2,0 en el servidor del controlador y el servidor del controlador, a su vez, instalará el agente WFF en cada uno de los servidores de una granja de servidores. El servidor del controlador no pertenece conceptualmente a ninguna granja de servidores de WFF y un solo servidor de controlador puede administrar varias granjas de servidores. En este escenario, se usa un único servidor de controlador de WFF para crear y administrar la granja de servidores de ensayo y la granja de servidores de producción.
- El *servidor principal*. Cada granja de servidores de WFF incluye un único servidor principal. Cuando se instalan componentes de plataforma web o se implementan aplicaciones en el servidor principal, el WFF sincroniza los cambios en todos los demás servidores de la granja de servidores.
- El *servidor secundario*. Cada granja de servidores de WFF incluye uno o varios servidores secundarios. Los cambios que realice en el servidor principal se replican en todos los servidores secundarios de la granja de servidores.

Esto muestra cómo se relacionan estos roles de servidor con los entornos de ensayo y producción de Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

En este escenario, el entorno de ensayo y el entorno de producción se configuran como granjas de servidores de WFF. Un único servidor de controlador de WFF administra ambas granjas de servidores. En cada granja de servidores, los cambios que se realicen en el servidor principal se replican en cada servidor secundario.

Antes de empezar a configurar los entornos de ensayo y producción, le recomendamos que lea estos artículos para familiarizarse con los conceptos clave de WFF 2,0:

- [Información general del marco de granja de servidores Web 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configuración de una granja de servidores con el marco de granja Web 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisitos del sistema y de la plataforma para el marco de granja de servidores Web 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Información general sobre tareas

Para completar las tareas y los tutoriales de este tema, necesitará al menos tres servidores&#x2014;, un controlador de WFF, un servidor web principal para la granja de servidores y uno o varios servidores Web secundarios para la granja de servidores. Puede agregar más servidores secundarios a una granja de servidores de WFF en cualquier momento. En un nivel alto, para crear y configurar una granja de servidores de WFF para el entorno de ensayo o de producción, deberá:

- Cree un servidor de controlador mediante la instalación de Internet Information Services (IIS) 7,5 y WFF 2,0.
- Prepare los servidores principales y secundarios mediante la creación de una cuenta de administrador común y la configuración de las excepciones de Firewall.
- Configure la granja de servidores mediante el administrador de IIS en el servidor de controlador.
- Configure el equilibrio de carga mediante el enrutamiento de solicitud de aplicaciones (ARR) de IIS o una tecnología alternativa de equilibrio de carga.

En las tareas y los tutoriales de este tema se supone que está empezando con las compilaciones de servidor limpio que ejecutan Windows Server 2008 R2. Antes de empezar, para cada servidor, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido a un dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información acerca de cómo unir equipos a un dominio, consulte [unir equipos al dominio e inicio de sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre la configuración de direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).

## <a name="create-the-wff-controller-server"></a>Crear el servidor de controlador de WFF

Para crear un servidor de controlador de WFF, deberá instalar IIS 7 o posterior y WFF 2,0 o posterior. En segundo plano, WFF usa la herramienta de implementación web de IIS (Web Deploy) 2. x para sincronizar los servidores de la granja. Si usa el instalador de plataforma web para instalar WFF, el instalador descargará e instalará Web Deploy automáticamente.

**Para crear el servidor de controlador de WFF**

1. Descargue e instale el [instalador de plataforma web](https://go.microsoft.com/?linkid=9739157).
2. En la parte superior de la ventana **instalador de plataforma Web 3,0** , haga clic en **productos**.
3. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **servidor**.
4. En la fila **configuración recomendada de IIS 7** , haga clic en **Agregar**.
5. En el <strong>marco de granja de servidores Web 2.</strong> <em>x</em> , haga clic en <strong>Agregar</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Haga clic en **Instalar**. Tenga en cuenta que el instalador de plataforma web ha agregado la herramienta de implementación web, junto con otras dependencias, a la lista de instalación.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Revise los términos de licencia y, si da su consentimiento a los términos **, haga clic en Acepto.**
8. Una vez completada la instalación, haga clic en **Finalizar**y, a continuación, cierre la ventana **instalador de plataforma web 3,0** .

## <a name="configure-the-primary-and-secondary-servers"></a>Configurar los servidores principales y secundarios

Antes de crear una granja de servidores de WFF, debe completar algunas tareas de preparación en los servidores web que compondrán la granja:

- Agregue excepciones de Firewall para permitir que las características **principales de redes**, **administración remota**y **uso compartido de archivos e impresoras** se comuniquen con el servidor de controlador de WFF.
- Cree una cuenta de dominio (por ejemplo, **FABRIKAM\stagingfarm**) en Active Directory y agréguela al grupo de administradores locales en cada servidor. Usará esta cuenta como cuenta de administrador de la granja de servidores al crear la granja de servidores.

Para obtener más información sobre cómo configurar estas excepciones de firewall en firewall de Windows, vea [requisitos del sistema y de la plataforma para el marco de granja de servidores Web 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805128). En el caso de otros sistemas de firewall, consulte la documentación del producto.

Puede usar el siguiente procedimiento para agregar una cuenta de dominio al grupo de administradores local en Windows Server 2008 R2. Debe realizar este procedimiento en cada servidor que desee agregar a la granja&#x2014;de servidores en otras palabras, agregar la misma cuenta de dominio al grupo de administradores locales en el servidor principal y en cada servidor secundario.

**Para agregar una cuenta de dominio al grupo de administradores locales**

1. En el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en **Administrador del servidor**.
2. En la ventana de **Administrador del servidor** , en el panel de vista de árbol, expanda **configuración**, expanda **usuarios y grupos locales**y, a continuación, haga clic en **grupos**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. En el panel **grupos** , haga doble clic en **administradores**.
4. En el cuadro de diálogo **propiedades de administradores** , haga clic en **Agregar**.
5. En el cuadro de diálogo **Seleccionar usuarios, equipos, cuentas de servicio o grupos** , escriba (o examine) la cuenta de dominio (por ejemplo, **FABRIKAM\stagingfarm**) y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. En el cuadro de diálogo **propiedades de administradores** , haga clic en **Aceptar**.

Los servidores ya están listos para agregarse a una granja de servidores. En el caso del servidor principal, puede configurar el servidor para cumplir los requisitos de la aplicación antes o después de crear la granja&#x2014;de servidores en ambos casos, el WFF sincronizará los servidores mediante la implementación de los mismos productos, componentes o configuraciones en los servidores secundarios. Por motivos de simplicidad, en este tutorial se supone que configurará el servidor principal cuando haya terminado de crear la granja de servidores.

## <a name="create-the-wff-server-farm"></a>Crear la granja de servidores de WFF

En este momento, todos los servidores están listos para agregarse a una granja de servidores de WFF:

- Ha instalado WFF en el servidor de controlador.
- Ha configurado las excepciones de firewall en los servidores Web principales y secundarios.
- Ha agregado una cuenta de dominio al grupo de administradores local en los servidores web principal y secundario.

El siguiente paso consiste en crear la granja de servidores en WFF. Puede hacerlo desde el administrador de IIS en el servidor de controlador de WFF.

**Para crear una granja de servidores de WFF**

1. En el servidor de controlador de WFF, en el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en **Administrador de Internet Information Services (IIS)** .
2. En el panel **conexiones** , expanda el nodo de servidor local, haga clic con el botón secundario en **granjas de servidores**y, a continuación, haga clic en **crear granja**de servidores.
3. En el cuadro de diálogo **crear granja de servidores** , escriba un nombre descriptivo para la granja de servidores (por ejemplo, **granja provisional**) y, a continuación, seleccione **aprovisionar granja**de servidores.
4. Escriba el nombre de usuario y la contraseña de la cuenta de dominio que ha agregado al grupo de administradores local en cada servidor.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Haga clic en **Siguiente**.
6. En la página **agregar servidores** , escriba el nombre de dominio completo (FQDN) del servidor principal, seleccione **servidor principal**y, a continuación, haga clic en **Agregar**.
7. En este momento, WFF intentará ponerse en contacto con el servidor principal con las credenciales proporcionadas. Si la conexión se realiza correctamente, el servidor principal se agregará a la tabla en la página **agregar servidores** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Es posible que haya observado que el **servidor está disponible para el equilibrio de carga** está seleccionado de forma predeterminada. WFF usa el módulo de IIS ARR para implementar el equilibrio de carga y, por tanto, distribuir las solicitudes entre los servidores Web de la granja de servidores. En la mayoría de los escenarios, solo desactive el **servidor está disponible para la opción de equilibrio de carga** si desea utilizar en su lugar una solución de equilibrio de carga de otro fabricante.
8. En la página **agregar servidores** , escriba el FQDN del primer servidor secundario y, a continuación, haga clic en **Agregar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Repita el paso 7 para los servidores secundarios adicionales de la granja y, a continuación, haga clic en **Finalizar**.

La granja de servidores de WFF ya está en funcionamiento. Todos los productos o componentes de la plataforma web que se instalan en el servidor principal, así como las aplicaciones web o el contenido que se implementa en el servidor principal, se aprovisionarán automáticamente en todos los servidores secundarios.

WFF es un tema amplio y complejo, y puede obtener más información en el sitio web de [Microsoft Web Farm Framework 2,0 for IIS 7](https://go.microsoft.com/?linkid=9805129) . Sin embargo, para el momento, hay dos áreas de características que debe tener en cuenta:

- El *aprovisionamiento de aplicaciones* es el proceso que replica el contenido del servidor principal, como las opciones de configuración y las aplicaciones Web, en todos los servidores secundarios de la granja de servidores. Por ejemplo, si implementa la solución de ejemplo Contact Manager en el servidor de ensayo principal, el proceso de aprovisionamiento de aplicaciones de WFF implementará esta solución en todos los servidores de ensayo secundarios. De forma predeterminada, el proceso de aprovisionamiento de aplicaciones se ejecuta cada 30 segundos.
- El *aprovisionamiento de plataforma* es el proceso que sincroniza los productos y componentes de la plataforma web del servidor principal con todos los servidores secundarios de la granja de servidores. Por ejemplo, si instala ASP.NET MVC 3 en el servidor de ensayo principal, el proceso de aprovisionamiento de plataforma usará el instalador de plataforma web para instalar ASP.NET MVC 3 en todos los servidores de ensayo secundarios. De forma predeterminada, el proceso de aprovisionamiento de la plataforma se ejecuta cada cinco minutos.

Puede administrar la configuración básica de aprovisionamiento de aplicaciones y plataformas desde el administrador de IIS en el servidor de controlador de WFF.

**Explore la configuración de aprovisionamiento de aplicaciones y plataformas**

1. En el administrador de IIS, en el panel **conexiones** , seleccione la granja de servidores.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. En el panel **granja de servidores** , haga doble clic en aprovisionamiento de **aplicaciones**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Como puede ver, la granja de servidores está configurada actualmente para sincronizar el contenido web y los valores de configuración entre el servidor principal y los servidores secundarios cada 30 segundos.
4. Haga clic en **atrás**y, a continuación, haga doble clic en **aprovisionamiento de plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Como puede ver, la granja de servidores está configurada actualmente para sincronizar los productos y componentes de la plataforma web entre el servidor principal y los servidores secundarios cada cinco minutos.
6. Haga clic en **Atrás**.
7. Para obligar a la granja de servidores a sincronizar los productos de la plataforma web inmediatamente, en el panel **acciones** , haga clic en **aprovisionar plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > El aprovisionamiento de la plataforma puede tardar algún tiempo. El proceso del instalador se ejecuta en segundo plano en los servidores secundarios de la granja de servidores.
8. Una vez que haya permitido el tiempo suficiente para que se complete el proceso de aprovisionamiento, puede comprobar que los productos y componentes que ha agregado al servidor principal se han replicado en los servidores secundarios. Por ejemplo, puede iniciar sesión en un servidor secundario y usar la ventana de **Administrador del servidor** para comprobar que se ha instalado el rol de servidor Web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. También puede consultar la lista programas instalados para comprobar que se han agregado varios componentes de la plataforma Web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurar el equilibrio de carga

Al crear una granja de servidores Web, debe configurar algún tipo de equilibrio de carga para distribuir las solicitudes HTTP entre los servidores Web. Podría ser el equilibrio de carga de red de Windows Server 2008, IIS ARR o una solución de equilibrio de carga basada en software o en hardware de terceros.

WFF está diseñado para integrarse estrechamente con IIS ARR. Para aprovechar esta integración, debe instalar el módulo ARR en el servidor de controlador de WFF. A continuación, dirige todo el tráfico web al servidor del controlador, normalmente mediante la configuración de registros del sistema de nombres de dominio (DNS). El servidor del controlador distribuirá después las solicitudes entrantes entre los servidores de la granja, en función de la disponibilidad del servidor y de otros criterios.

> [!NOTE]
> No tiene que usar ARR con WFF; puede configurar WFF para que funcione con soluciones de equilibrio de carga de terceros. Para obtener más información, vea [información general sobre el marco de granja de servidores Web 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805126).

El equilibrio de carga con ARR es un tema complejo, la mayoría de los cuales queda fuera del ámbito de este tutorial. Sin embargo, puede usar el siguiente procedimiento para instalar el módulo ARR y empezar a trabajar con el equilibrio de carga.

**Para configurar el equilibrio de carga en el servidor de controlador de WFF**

1. En el servidor de controlador de WFF, inicie el instalador de plataforma Web.
2. En la parte superior de la ventana **instalador de plataforma Web 3,0** , haga clic en **productos**.
3. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **servidor**.
4. En la fila **enrutamiento de solicitud de aplicación 2,5** , haga clic en **Agregar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Haga clic en **instalar**y siga las instrucciones de la ventana de instalación de la **plataforma web** .
6. Una vez completada la instalación, inicie el administrador de IIS y, en el panel **conexiones** , haga clic en el nodo de la granja de servidores. Observe que se han agregado varios iconos nuevos al panel **granja de servidores** .

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. En el panel **Granja de servidores**, haga doble clic en **Equilibrio de carga**.
8. En el panel **equilibrio de carga** , seleccione un algoritmo de equilibrio de carga (por ejemplo, la **solicitud menos actual**).

    > [!NOTE]
    > Para obtener más información sobre los algoritmos de equilibrio de carga y otras opciones de configuración, vea [módulo de enrutamiento de solicitud de aplicación](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. En el panel **acciones** , haga clic en **aplicar**.

Ya ha configurado el equilibrio de carga básico para los servidores de la granja de servidores. Si dirige todo el tráfico de la granja de servidores web al servidor del controlador, las solicitudes se distribuirán entre los servidores de la granja según la disponibilidad y el algoritmo de equilibrio de carga que haya seleccionado.

Para obtener más información sobre cómo configurar el equilibrio de carga con ARR, vea [módulo de enrutamiento de solicitud de aplicación](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Supervisión de la granja de servidores

Puede supervisar el estado de la granja de servidores en cualquier momento a través del administrador de IIS en el servidor de controlador. En el panel **conexiones** , expanda la granja de servidores y, a continuación, haga clic en **servidores**. El panel central mostrará un resumen de cada servidor de la granja junto con un registro de seguimiento de la actividad reciente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusión

La granja de servidores de WFF ahora debe estar en funcionamiento. Puede configurar el servidor principal para que admita cualquier enfoque de implementación que&#x2014;prefiera. Consulte la sección de lecturas&#x2014;adicionales para obtener más información y la configuración se replicará en cada servidor secundario de la granja de servidores.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre todos los aspectos de la configuración y el uso de WFF, vea el sitio web de [Microsoft Web Farm Framework 2,0 para IIS 7](https://go.microsoft.com/?linkid=9805129) .

> [!div class="step-by-step"]
> [Anterior](configuring-a-database-server-for-web-deploy-publishing.md)
> [Siguiente](configuring-deployment-properties-for-a-target-environment.md)
