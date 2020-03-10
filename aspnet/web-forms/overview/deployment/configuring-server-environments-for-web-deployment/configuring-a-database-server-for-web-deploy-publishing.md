---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configuración de un servidor de base de datos para la publicación de Web Deploy | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo configurar un servidor de base de datos SQL Server 2008 R2 para admitir la publicación y la implementación web. Las tareas descritas en este tema son Co...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: ade3c1ba1c470092f512436f39b8831458408c2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515245"
---
# <a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configurar un servidor de base de datos para la publicación de la implementación web

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo configurar un servidor de base de datos SQL Server 2008 R2 para admitir la publicación y la implementación web.
> 
> Las tareas que se describen en este tema son comunes a cada&#x2014;escenario de implementación. no importa si los servidores web están configurados para usar el servicio de agente remoto de la herramienta de implementación web de IIS (Web deploy), el controlador de web deploy o la implementación sin conexión o la aplicación se ejecuta en un único servidor web o en una granja de servidores. La forma de implementar la base de datos puede cambiar según los requisitos de seguridad y otras consideraciones. Por ejemplo, puede implementar la base de datos con o sin datos de ejemplo, y puede implementar asignaciones de roles de usuario o configurarlas manualmente después de la implementación. Sin embargo, la manera de configurar el servidor de base de datos sigue siendo la misma.

No tiene que instalar ningún producto o herramienta adicional para configurar un servidor de base de datos para admitir la implementación web. Suponiendo que el servidor de base de datos y el servidor Web se ejecutan en equipos diferentes, solo tiene que hacer lo siguiente:

- Permita que SQL Server se comuniquen mediante TCP/IP.
- Permitir el tráfico SQL Server a través de los firewalls.
- Conceda a la cuenta de equipo del servidor Web un SQL Server inicio de sesión.
- Asigne el inicio de sesión de la cuenta de equipo a los roles de base de datos necesarios.
- Proporcione a la cuenta que ejecutará la implementación un SQL Server los permisos inicio de sesión y creador de la base de datos.
- Para admitir implementaciones de repetición, asigne el inicio de sesión de la cuenta de implementación al rol de base de datos **db\_Owner** .

En este tema se muestra cómo realizar cada uno de estos procedimientos. En las tareas y los tutoriales de este tema se supone que va a comenzar con una instancia predeterminada de SQL Server 2008 R2 que se ejecuta en Windows Server 2008 R2. Antes de continuar, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido a un dominio.
- El servidor tiene una dirección IP estática.
- SQL Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles están instaladas.

La instancia de SQL Server solo debe incluir el rol **motor de base de datos Services** , que se incluye automáticamente en cualquier SQL Server instalación. Sin embargo, para facilitar la configuración y el mantenimiento, se recomienda incluir las herramientas de **Administración: básicas** y **las herramientas de administración:** funciones de servidor completas.

> [!NOTE]
> Para obtener más información acerca de cómo unir equipos a un dominio, consulte [unir equipos al dominio e inicio de sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre la configuración de direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Para obtener más información sobre la instalación de SQL Server, consulte [instalación de SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).

## <a name="enable-remote-access-to-sql-server"></a>Habilitación del acceso remoto a SQL Server

SQL Server usa TCP/IP para comunicarse con equipos remotos. Si el servidor de base de datos y el servidor Web se encuentran en equipos diferentes, debe:

- Configure SQL Server opciones de red para permitir la comunicación a través de TCP/IP.
- Configure los firewalls de hardware o software para permitir el tráfico TCP (y, en algunos casos, el tráfico del Protocolo de datagramas de usuario (UDP)) en los puertos que utiliza la instancia de SQL Server.

Para habilitar SQL Server para comunicarse a través de TCP/IP, utilice Administrador de configuración de SQL Server para cambiar la configuración de red de la instancia de SQL Server.

**Para habilitar SQL Server para comunicarse mediante TCP/IP**

1. En el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft SQL Server 2008 R2**, haga clic en **herramientas de configuración**y, a continuación, haga clic en **Administrador de configuración de SQL Server**.
2. En el panel vista de árbol, expanda **SQL Server configuración de red**y, a continuación, haga clic en **protocolos de MSSQLServer**.

   > [!NOTE]
   > Si ha instalado varias instancias de SQL Server, verá un elemento <strong>protocolos para</strong><em>[nombre de instancia]</em> para cada instancia. Debe configurar las opciones de red en cada instancia.
3. En el panel de detalles, haga clic con el botón secundario en la fila **TCP/IP** y, a continuación, haga clic en **Habilitar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. En el cuadro de diálogo de **ADVERTENCIA** , haga clic en **Aceptar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Deberá reiniciar el servicio MSSQLSERVER para que la nueva configuración de red surta efecto. Puede hacerlo en un símbolo del sistema, desde la consola de servicios o desde SQL Server Management Studio. En este procedimiento, usará SQL Server Management Studio.
6. Cierre el Administrador de configuración de SQL Server.
7. En el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft SQL Server 2008 R2**y, a continuación, haga clic en **SQL Server Management Studio**.
8. En el cuadro de diálogo **conectar con el servidor** , en el cuadro **nombre del servidor** , escriba el nombre del servidor de base de datos y, a continuación, haga clic en **conectar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. En el panel **Explorador de objetos** , haga clic con el botón secundario en el nodo del servidor principal (por ejemplo, **TESTDB1**) y, a continuación, haga clic en **reiniciar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. En el cuadro de diálogo **Management Studio de Microsoft SQL Server** , haga clic en **sí**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Cuando se haya reiniciado el servicio, cierre SQL Server Management Studio.

Para permitir el tráfico SQL Server a través de un firewall, primero debe saber qué puertos usa la instancia de SQL Server. Esto dependerá de cómo se haya creado y configurado la instancia de SQL Server:

- Una *instancia predeterminada* de SQL Server escucha las solicitudes (y responde a ellas) en el puerto TCP 1433.
- Una *instancia con nombre* de SQL Server escucha las solicitudes (y responde a ellas) en un puerto TCP asignado dinámicamente.
- Si el servicio SQL Server Browser está habilitado, los clientes pueden consultar el servicio en el puerto UDP 1434 para averiguar qué puerto TCP usar para una instancia SQL Server determinada. Sin embargo, este servicio suele deshabilitarse por motivos de seguridad.

Suponiendo que esté usando una instancia predeterminada de SQL Server, debe configurar el firewall para permitir el tráfico.

| Dirección | Desde el puerto | A Puerto | Tipo de puerto |
| --- | --- | --- | --- |
| Entrada | Cualquiera | 1433 | TCP |
| Salida | 1433 | Cualquiera | TCP |

> [!NOTE]
> Técnicamente, un equipo cliente usará un puerto TCP asignado aleatoriamente entre 1024 y 5000 para comunicarse con SQL Server, y puede restringir las reglas de firewall en consecuencia. Para obtener más información sobre SQL Server puertos y firewalls, consulte [números de puerto TCP/IP necesarios para comunicarse con SQL a través de un firewall](https://go.microsoft.com/?linkid=9805125) y [Cómo: configurar un servidor para que escuche en un puerto TCP específico (Administrador de configuración de SQL Server)](https://msdn.microsoft.com/library/ms177440.aspx).

En la mayoría de los entornos de Windows Server, es probable que tenga que configurar Firewall de Windows en el servidor de base de datos. De forma predeterminada, Firewall de Windows permite todo el tráfico saliente a menos que una regla lo prohíba específicamente. Para permitir que el servidor Web alcance la base de datos, debe configurar una regla de entrada que permita el tráfico TCP en el número de puerto que usa la instancia de SQL Server. Si utiliza una instancia predeterminada de SQL Server, puede usar el siguiente procedimiento para configurar esta regla.

**Para configurar el Firewall de Windows para permitir la comunicación con una instancia de SQL Server predeterminada**

1. En el servidor de base de datos, en el menú **Inicio** , seleccione **herramientas administrativas**y, a continuación, haga clic en **firewall de Windows con seguridad avanzada**.
2. En el panel de vista de árbol, haga clic en **reglas de entrada**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. En el panel **acciones** , en **reglas de entrada**, haga clic en **nueva regla**.
4. En el Asistente para nueva regla de entrada, en la página **tipo de regla** , seleccione **Puerto**y, a continuación, haga clic en **siguiente**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. En la página **Protocolo y puertos** , asegúrese de **que TCP** está seleccionado y, en el cuadro **puertos locales específicos** , escriba **1433**y, a continuación, haga clic en **siguiente**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. En la página **acción** , deje **la opción permitir la conexión** seleccionada y haga clic en **siguiente**.
7. En la página **perfil** , deje **el dominio** seleccionado, desactive las casillas **privado** y **público** y, a continuación, haga clic en **siguiente**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. En la página **nombre** , asigne a la regla un nombre descriptivo apropiado (por ejemplo, **SQL Server instancia predeterminada – acceso a la red**) y, a continuación, haga clic en **Finalizar**.

Para obtener más información sobre la configuración de Firewall de Windows para SQL Server, especialmente si necesita comunicarse con SQL Server a través de puertos no estándar o dinámicos, consulte [How to: Configure a Windows Firewall for motor de base de datos Access](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurar inicios de sesión y permisos de base de datos

Cuando se implementa una aplicación web en Internet Information Services (IIS), la aplicación se ejecuta con la identidad del grupo de aplicaciones. En un entorno de dominio, las identidades del grupo de aplicaciones usan la cuenta del equipo del servidor en el que se ejecutan para tener acceso a los recursos de red. Las cuentas de equipo tienen la forma <em>[nombre de dominio]</em><strong>\</strong ><em>[nombre de equipo]</em> <strong>$</strong> &#x2014;por ejemplo, <strong>FABRIKAM\TESTWEB1 $</strong>. Para permitir que la aplicación web tenga acceso a una base de datos a través de la red, debe:

- Agregue un inicio de sesión para la cuenta de equipo del servidor Web a la instancia de SQL Server.
- Asigne el inicio de sesión de la cuenta de equipo a los roles de base de datos necesarios (normalmente **db\_DataReader** y **dB\_** .

Si la aplicación web se ejecuta en una granja de servidores, en lugar de un solo servidor, deberá repetir estos procedimientos para cada servidor Web de la granja de servidores.

> [!NOTE]
> Para obtener más información sobre las identidades del grupo de aplicaciones y el acceso a los recursos de red, vea [identidades del grupo de aplicaciones](https://go.microsoft.com/?linkid=9805123).

Puede abordar estas tareas de varias maneras. Para crear el inicio de sesión, puede:

- Cree el inicio de sesión manualmente en el servidor de base de datos mediante Transact-SQL o SQL Server Management Studio.
- Use un proyecto de servidor SQL Server 2008 en Visual Studio para crear e implementar el inicio de sesión.

Un inicio de sesión de SQL Server es un objeto de nivel de servidor, en lugar de un objeto de nivel de base de datos, por lo que no depende de la base de datos que desea implementar. Como tal, puede crear el inicio de sesión en cualquier momento y el enfoque más sencillo es crear el inicio de sesión manualmente en el servidor de base de datos antes de empezar a implementar las bases de datos. Puede usar el siguiente procedimiento para crear un inicio de sesión en SQL Server Management Studio.

**Para crear un inicio de sesión SQL Server para la cuenta de equipo del servidor Web**

1. En el servidor de base de datos, en el menú **Inicio** , seleccione **todos los programas**, haga clic en **Microsoft SQL Server 2008 R2**y, a continuación, haga clic en **SQL Server Management Studio**.
2. En el cuadro de diálogo **conectar con el servidor** , en el cuadro **nombre del servidor** , escriba el nombre del servidor de base de datos y, a continuación, haga clic en **conectar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. En el panel **Explorador de objetos** , haga clic con el botón secundario en **seguridad**, seleccione **nuevo**y, a continuación, haga clic en **Inicio de sesión**.
4. En el cuadro de diálogo **Inicio de sesión-nuevo** , en el cuadro **nombre de inicio de sesión** , escriba el nombre de la cuenta de equipo del servidor Web (por ejemplo, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Haga clic en **Aceptar**.

En este momento, el servidor de base de datos está listo para la publicación de Web Deploy. Sin embargo, las soluciones que implemente no funcionarán hasta que asigne el inicio de sesión de la cuenta de equipo a los roles de base de datos necesarios. La asignación del inicio de sesión a los roles de base de datos requiere mucho más pensamiento, ya que no se pueden asignar roles hasta después de haber implementado la base de datos. Para asignar el inicio de sesión de la cuenta de equipo a los roles de base de datos necesarios, puede:

- Asigne los roles de base de datos al inicio de sesión manualmente, después de haber implementado la base de datos por primera vez.
- Use un script posterior a la implementación para asignar los roles de base de datos al inicio de sesión.

Para obtener más información sobre la automatización de la creación de inicios de sesión y asignaciones de roles de base de datos, vea [implementar pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Como alternativa, puede usar el siguiente procedimiento para asignar manualmente el inicio de sesión de la cuenta de equipo a los roles de base de datos necesarios. Recuerde que no puede realizar este *procedimiento hasta que haya* implementado la base de datos.

**Para asignar roles de base de datos al inicio de sesión de la cuenta de equipo de servidor Web**

1. Abra SQL Server Management Studio como antes.
2. En el panel **Explorador de objetos** , expanda el nodo **seguridad** , expanda el nodo **inicios de sesión** y, a continuación, haga doble clic en el inicio de sesión de la cuenta de equipo (por ejemplo, **FABRIKAM\TESTWEB1 $** ).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. En el cuadro de diálogo **propiedades de inicio de sesión** , haga clic en asignación de **usuarios**.
4. En la tabla **usuarios asignados a este inicio de sesión** , seleccione el nombre de la base de datos (por ejemplo, **ContactManager**).
5. En la lista **pertenencia al rol de la base de datos para:** *[nombre de la base de datos]* , seleccione los permisos necesarios. En el caso de la solución de ejemplo Contact Manager, debe seleccionar los roles **db\_DataReader** y **dB\_** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Haga clic en **Aceptar**.

Aunque la asignación manual de roles de base de datos suele ser más adecuada para entornos de prueba, es menos conveniente para implementaciones automatizadas o de un solo clic en entornos de ensayo o producción. Puede encontrar más información sobre cómo automatizar este tipo de tarea mediante scripts posteriores a la implementación en la [implementación de pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Para obtener más información sobre proyectos de servidor y proyectos de base de datos, vea [Visual Studio 2010 SQL Server proyectos de base de datos](https://msdn.microsoft.com/library/ff678491.aspx).

## <a name="configure-permissions-for-the-deployment-account"></a>Configurar permisos para la cuenta de implementación

Si la cuenta que va a usar para ejecutar la implementación no es un administrador de SQL Server, también deberá crear un inicio de sesión para esta cuenta. Para crear la base de datos, la cuenta debe ser miembro del rol de servidor **dbcreator** o tener permisos equivalentes.

> [!NOTE]
> Al usar Web Deploy o VSDBCMD para implementar una base de datos, puede usar credenciales de Windows o credenciales de SQL Server (si la instancia de SQL Server está configurada para admitir la autenticación de modo mixto). En el procedimiento siguiente se supone que desea utilizar las credenciales de Windows, pero no hay nada que le impida especificar una SQL Server nombre de usuario y contraseña en la cadena de conexión al configurar la implementación.

**Para configurar permisos para la cuenta de implementación**

1. Abra SQL Server Management Studio como antes.
2. En el panel **Explorador de objetos** , haga clic con el botón secundario en **seguridad**, seleccione **nuevo**y, a continuación, haga clic en **Inicio de sesión**.
3. En el cuadro de diálogo **Inicio de sesión-nuevo** , en el cuadro **nombre de inicio de sesión** , escriba el nombre de la cuenta de implementación (por ejemplo, **FABRIKAM\matt**).
4. En el panel **seleccionar una página** , haga clic en **roles de servidor**.
5. Seleccione **dbcreator**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Para admitir las implementaciones posteriores, también tendrá que agregar la cuenta de implementación al rol de **propietario** de la base de datos\_en la base de datos después de la primera implementación. Esto se debe a que en las implementaciones posteriores se está modificando el esquema de una base de datos existente, en lugar de crear una nueva base de datos. Como se describe en la sección anterior, no se puede Agregar un usuario a un rol de base de datos hasta que se haya creado la base de datos, por razones obvias.

**Para asignar el inicio de sesión de la cuenta de implementación al rol de base de datos DB\_Owner**

1. Abra SQL Server Management Studio como antes.
2. En la ventana de **Explorador de objetos** , expanda el nodo **seguridad** , expanda el nodo **inicios de sesión** y, a continuación, haga doble clic en el inicio de sesión de la cuenta de equipo (por ejemplo, **FABRIKAM\matt**).
3. En el cuadro de diálogo **propiedades de inicio de sesión** , haga clic en asignación de **usuarios**.
4. En la tabla **usuarios asignados a este inicio de sesión** , seleccione el nombre de la base de datos (por ejemplo, **ContactManager**).
5. En la lista **pertenencia al rol de la base de datos para:** *[nombre de la base de datos]* , seleccione el rol de **propietario de\_dB** .

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Haga clic en **Aceptar**.

## <a name="conclusion"></a>Conclusión

El servidor de base de datos ahora debe estar preparado para aceptar implementaciones de bases de datos remotas y permitir que los servidores Web de IIS remotos tengan acceso a las bases de datos. Antes de intentar implementar y usar bases de datos, puede que desee comprobar estos puntos clave:

- ¿Ha configurado SQL Server para aceptar conexiones TCP/IP remotas?
- ¿Ha configurado los firewalls para permitir el tráfico SQL Server?
- ¿Ha creado un inicio de sesión de cuenta de equipo para cada servidor Web que vaya a tener acceso a SQL Server?
- ¿La implementación de la base de datos incluye un script para crear asignaciones de roles de usuario, o bien debe crearlos manualmente después de implementar la base de datos por primera vez?
- ¿Ha creado un inicio de sesión para la cuenta de implementación y lo ha agregado al rol de servidor **dbcreator** ?

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre la implementación de proyectos de base de datos, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener instrucciones sobre la creación de pertenencias a roles de base de datos mediante la ejecución de un script posterior a la implementación, vea [implementar miembros de roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obtener instrucciones sobre cómo cumplir los desafíos de implementación únicos que suponen las bases de datos de pertenencia, vea [implementar bases de datos de pertenencia en entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Siguiente](creating-a-server-farm-with-the-web-farm-framework.md)
