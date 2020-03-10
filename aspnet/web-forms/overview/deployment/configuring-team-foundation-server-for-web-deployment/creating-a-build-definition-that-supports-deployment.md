---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Crear una definición de compilación que admita la implementación | Microsoft Docs
author: jrjlee
description: Si desea realizar cualquier tipo de compilación en Team Foundation Server (TFS) 2010, debe crear una definición de compilación dentro del proyecto de equipo. Este tema des...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e11c91a824446572aaf0b3bc6954b9b8ffb4eaff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78489013"
---
# <a name="creating-a-build-definition-that-supports-deployment"></a>Crear una definición de compilación que admita la implementación

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Si desea realizar cualquier tipo de compilación en Team Foundation Server (TFS) 2010, debe crear una definición de compilación dentro del proyecto de equipo. En este tema se describe cómo crear una nueva definición de compilación en TFS y cómo controlar la implementación web como parte del proceso de compilación en Team Build.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el que el&#x2014;proceso de compilación e implementación está controlado por dos archivos de proyecto que contienen instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Una definición de compilación es el mecanismo que controla cómo y cuándo se producen las compilaciones para los proyectos de equipo en TFS. Cada definición de compilación especifica:

- Lo que desea compilar, como los archivos de solución de Visual Studio o los archivos de proyecto de Microsoft Build Engine personalizado (MSBuild).
- Los criterios que determinan cuándo debe tener lugar una compilación, como los desencadenadores manuales, la integración continua (CI) o las protecciones validadas.
- Ubicación a la que Team Build debe enviar salidas de compilación, incluidos los artefactos de implementación como paquetes web y scripts de base de datos.
- La cantidad de tiempo que se debe conservar cada compilación.
- Otros parámetros del proceso de compilación.

> [!NOTE]
> Para obtener más información sobre las definiciones de compilación, vea [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx).

En este tema se muestra cómo crear una definición de compilación que usa CI para que se desencadene una compilación cuando un desarrollador protege contenido nuevo. Si la compilación se realiza correctamente, el servicio de compilación ejecuta un archivo de proyecto personalizado para implementar la solución en un entorno de prueba.

Cuando se desencadena una compilación, es necesario que se produzcan estas acciones:

- En primer lugar, Team Build debe compilar la solución. Como parte de este proceso, Team Build invocará la canalización de publicación web (WPP) para generar paquetes de implementación web para cada uno de los proyectos de aplicación Web de la solución. Team Build también ejecutará todas las pruebas unitarias asociadas a la solución.
- Si se produce un error en la compilación de la solución, Team Build no debe realizar ninguna acción más. Los errores de las pruebas unitarias se deben tratar como un error de compilación.
- Si la compilación de la solución se realiza correctamente, Team Build debe ejecutar el archivo de proyecto personalizado que controla la implementación de la solución. Como parte de este proceso, Team Build invocará la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para instalar las aplicaciones web empaquetadas en los servidores Web de destino e invocará la utilidad VSDBCMD. exe para ejecutar la creación de la base de datos. scripts en los servidores de bases de datos de destino.

Esto muestra el proceso:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

La solución de ejemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) incluye un archivo de proyecto de MSBuild personalizado, *Publish. proj*, que puede ejecutar desde MSBuild o Team Build. Tal y como se describe en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), este archivo de proyecto define la lógica que implementa los paquetes web y las bases de datos en un entorno de destino. El archivo incluye lógica que omite el proceso de compilación y empaquetado si se está ejecutando en Team Build, lo que solo se ejecuta en las tareas de implementación. Esto se debe a que al automatizar la implementación de esta manera, normalmente querrá asegurarse de que la solución se compila correctamente y de que pasa todas las pruebas unitarias antes de que se inicie el proceso de implementación.

En la sección siguiente se explica cómo implementar este proceso mediante la creación de una nueva definición de compilación.

> [!NOTE]
> Este procedimiento&#x2014;en el que un proceso automatizado único genera, prueba e implementa una solución&#x2014;es probablemente más adecuado para la implementación en entornos de prueba. En el caso de entornos de ensayo y producción, es mucho más probable que desee implementar contenido desde una compilación anterior que ya haya comprobado y validado en un entorno de prueba. Este enfoque se describe en el tema siguiente, [implementar una compilación específica](deploying-a-specific-build.md).

### <a name="who-performs-this-procedure"></a>¿Quién realiza este procedimiento?

Normalmente, un administrador de TFS realiza este procedimiento. En algunos casos, un jefe de equipo de desarrolladores puede asumir la responsabilidad de la colección de proyectos de equipo en TFS. Para crear una nueva definición de compilación, debe ser miembro del grupo administradores de compilación de la **colección de proyectos** para la colección de proyectos de equipo que contiene la solución.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Crear una definición de compilación para el CI y la implementación

En el procedimiento siguiente se describe cómo crear una definición de compilación que desencadenador de elementos de integración. Si la compilación se realiza correctamente, la solución se implementa con la lógica en un archivo de proyecto de MSBuild personalizado.

**Para crear una definición de compilación para el CI y la implementación**

1. En Visual Studio 2010, en la ventana de **Team Explorer** , expanda el nodo del proyecto de equipo, haga clic con el botón secundario en **compilaciones**y, a continuación, haga clic en **nueva definición de compilación**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. En la pestaña **General** , asigne un nombre a la definición de compilación (por ejemplo, **DeployToTest**) y una descripción opcional.
3. En la pestaña **desencadenador** , seleccione los criterios en los que desea desencadenar una nueva compilación. Por ejemplo, si desea compilar la solución e implementarla en el entorno de prueba cada vez que un desarrollador proteja el nuevo código, seleccione **integración continua**.
4. En la pestaña **valores predeterminados de compilación** , en el cuadro **Copiar la salida de la compilación en la siguiente carpeta de entrega** , escriba la ruta de acceso UNC (Convención de nomenclatura universal) de la carpeta de entrega (por ejemplo, **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Esta ubicación de destino almacena varias compilaciones, en función de la Directiva de retención que configure. Si desea publicar artefactos de implementación de una compilación específica en un entorno de ensayo o de producción, aquí es donde los encontrará.
5. En la pestaña **proceso** , en la lista desplegable **archivo del proceso de compilación** , deje seleccionado **DefaultTemplate. Xaml** . Se trata de una de las plantillas de proceso de compilación predeterminadas que se agregan a todos los proyectos de equipo nuevos.
6. En la tabla **parámetros del proceso de compilación** , haga clic en la fila **elementos para compilar** y, a continuación, haga clic en el botón de **puntos suspensivos** .

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. En el cuadro de diálogo **elementos para compilar** , haga clic en **Agregar**.
8. Busque la ubicación del archivo de solución y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. En el cuadro de diálogo **elementos para compilar** , haga clic en **Agregar**.
10. En la lista desplegable de **elementos de tipo** , seleccione **archivos de proyecto de MSBuild**.
11. Vaya a la ubicación del archivo de proyecto personalizado con el que controla el proceso de implementación, seleccione el archivo y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Ahora, el cuadro de diálogo **elementos para compilar** debe mostrar dos elementos. Haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. En la pestaña **proceso** , en la tabla **parámetros del proceso de compilación** , expanda la sección **Opciones avanzadas** .
14. En la fila **argumentos de MSBuild** , agregue cualquier argumento de la línea de comandos de MSBuild que requieran los elementos que se van a compilar. En el escenario de la solución Contact Manager, estos argumentos son obligatorios:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. En este ejemplo:

    1. Los argumentos **DeployOnBuild = true** y **DeployTarget = Package** son obligatorios al compilar la solución Contact Manager. Esto indica a MSBuild que cree paquetes de implementación web después de compilar cada proyecto de aplicación Web, tal como se describe en [compilar y empaquetar proyectos de aplicación web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. El argumento **TargetEnvPropsFile** es necesario al compilar el archivo *Publish. proj* . Esta propiedad indica la ubicación del archivo de configuración específico del entorno, como se describe en [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. En la pestaña **Directiva de retención** , configure el número de compilaciones de cada tipo que desea conservar según sea necesario.
17. Haga clic en **Guardar**.

## <a name="queue-a-build"></a>Poner en cola una compilación

En este punto, ha creado al menos una nueva definición de compilación. El proceso de compilación que definió se ejecutará ahora según los desencadenadores que haya especificado en la definición de compilación.

Si ha configurado la definición de compilación para usar CI, puede probar la definición de compilación de dos maneras:

- Proteja algún contenido del proyecto de equipo para desencadenar una compilación automática.
- Poner en cola una compilación manualmente.

**Para poner en cola una compilación manualmente**

1. En la ventana **Team Explorer** , haga clic con el botón secundario en la definición de compilación y, a continuación, haga clic en **poner nueva compilación en cola**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. En el cuadro de diálogo **poner compilación en cola** , revise las propiedades de compilación y, a continuación, haga clic en **cola**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Para revisar el progreso y el resultado de una compilación&#x2014;, independientemente de si se desencadenó de forma manual&#x2014;o automática, haga doble clic en la definición de compilación en la ventana de **Team Explorer** . Se abrirá una pestaña del **Explorador de compilaciones** .

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Desde aquí, puede solucionar errores en las compilaciones. Si hace doble clic en una compilación individual, puede ver información de Resumen y hacer clic en los archivos de registro detallados.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Puede usar esta información para solucionar errores en las compilaciones y resolver los problemas antes de intentar otra compilación.

> [!NOTE]
> Es probable que se produzcan errores en las compilaciones que ejecutan la lógica de implementación hasta que se haya concedido al servidor de compilación los permisos necesarios en el entorno de destino. Para obtener más información, vea [configurar permisos para la implementación de Team Build](configuring-permissions-for-team-build-deployment.md).

## <a name="monitor-the-build-process"></a>Supervisar el proceso de compilación

TFS proporciona una amplia gama de funcionalidades para ayudarle a supervisar el proceso de compilación. Por ejemplo, TFS puede enviarle un correo electrónico o mostrar alertas en el área de notificación de la barra de tareas cuando se complete una compilación. Para obtener más información, vea [Ejecutar y supervisar compilaciones](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo crear una definición de compilación en TFS. La definición de compilación está configurada para CI, por lo que el proceso de compilación se ejecuta cada vez que un desarrollador protege el contenido en el proyecto de equipo. La definición de compilación ejecuta un archivo de proyecto de MSBuild personalizado para implementar paquetes web y scripts de base de datos en un entorno de servidor de destino.

Para que una implementación automatizada se realice correctamente como parte de un proceso de compilación, deberá conceder los permisos adecuados a la cuenta de servicio de compilación en los servidores Web de destino y en el servidor de base de datos de destino. En el tema final de este tutorial, [configuración de permisos para la implementación de Team Build](configuring-permissions-for-team-build-deployment.md), se describe cómo identificar y configurar los permisos necesarios para la implementación automatizada desde un servidor de Team Build.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre cómo crear definiciones de compilación, vea [crear una definición de compilación básica](https://msdn.microsoft.com/library/ms181716.aspx) y [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx). Para obtener más información sobre las compilaciones en cola, vea [poner en cola una compilación](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-a-tfs-build-server-for-web-deployment.md)
> [Siguiente](deploying-a-specific-build.md)
