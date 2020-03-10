---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Implementar una compilación específica | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo implementar paquetes web y scripts de base de datos desde una compilación anterior específica en un nuevo destino, como un estándar de ensayo o de producción...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6bede6b36c24ade928ab052e14daec1e017bd0b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519577"
---
# <a name="deploying-a-specific-build"></a>Implementar una compilación concreta

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo implementar paquetes web y scripts de base de datos desde una compilación anterior específica en un nuevo destino, como un entorno de ensayo o de producción.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el que el&#x2014;proceso de compilación e implementación está controlado por dos archivos de proyecto que contienen instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Hasta ahora, los temas de este tutorial se han centrado en cómo compilar, empaquetar e implementar aplicaciones web y bases de datos como parte de un proceso de un solo paso o automatizado. Sin embargo, en algunos escenarios comunes, querrá seleccionar los recursos que implemente desde una lista de compilaciones en una carpeta de entrega. Es decir, es posible que la compilación más reciente no sea la compilación que desea implementar.

Considere el escenario de integración continua (CI) descrito en el tema anterior, [creación de una definición de compilación que admite la implementación](creating-a-build-definition-that-supports-deployment.md). Ha creado una definición de compilación en Team Foundation Server (TFS) 2010. Cada vez que un desarrollador comprueba el código en TFS, Team Build compilará el código, creará paquetes web y scripts de base de datos como parte del proceso de compilación, ejecutará cualquier prueba unitaria e implementará los recursos en un entorno de prueba. En función de la Directiva de retención que haya configurado al crear la definición de compilación, TFS conservará un cierto número de compilaciones anteriores.

![](deploying-a-specific-build/_static/image1.png)

Ahora, supongamos que ha realizado pruebas de comprobación y validación en una de estas compilaciones en el entorno de prueba y está listo para implementar la aplicación en un entorno de ensayo. Mientras tanto, los desarrolladores pueden haber protegido código nuevo. No desea volver a compilar la solución e implementarla en el entorno de ensayo, y no desea implementar la compilación más reciente en el entorno de ensayo. En su lugar, desea implementar la compilación específica que ha comprobado y validado en los servidores de prueba.

Para ello, debe indicar al Microsoft Build Engine (MSBuild) dónde encontrar los paquetes web y los scripts de base de datos generados por una compilación específica.

## <a name="overriding-the-outputroot-property"></a>Invalidación de la propiedad OutputRoot

En la [solución de ejemplo](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), el archivo *Publish. proj* declara una propiedad denominada **OutputRoot**. Como sugiere su nombre, esta es la carpeta raíz que contiene todo lo que genera el proceso de compilación. En el archivo *Publish. proj* , puede ver que la propiedad **OutputRoot** hace referencia a la ubicación raíz para todos los recursos de implementación.

> [!NOTE]
> **OutputRoot** es un nombre de propiedad que se usa con frecuencia. Los C# archivos de proyecto de Visual y Visual Basic también declaran esta propiedad para almacenar la ubicación raíz para todos los resultados de compilación.

[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]

Si desea que el archivo de proyecto implemente paquetes web y scripts de base de&#x2014;datos desde una ubicación diferente, como las&#x2014;salidas de una compilación anterior de TFS, solo tiene que reemplazar la propiedad **OutputRoot** . Debe establecer el valor de la propiedad en la carpeta de compilación correspondiente en el servidor de Team Build. Si estaba ejecutando MSBuild desde la línea de comandos, puede especificar un valor para **OutputRoot** como argumento de la línea de comandos:

[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]

En la práctica, sin embargo, también podría querer omitir el destino&#x2014;de compilación no hay ningún punto para compilar la solución si no tiene previsto usar los resultados de la compilación. Para ello, puede especificar los destinos que desea ejecutar desde la línea de comandos:

[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]

Sin embargo, en la mayoría de los casos, querrá crear la lógica de implementación en una definición de compilación de TFS. Esto permite a los usuarios con el permiso **compilaciones de cola** desencadenar la implementación desde cualquier instalación de Visual Studio con una conexión al servidor de TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Crear una definición de compilación para implementar compilaciones específicas

En el procedimiento siguiente se describe cómo crear una definición de compilación que permita a los usuarios desencadenar implementaciones en un entorno de ensayo con un solo comando.

En este caso, no quiere que la definición de compilación cree realmente nada&#x2014;que quiera que ejecute la lógica de implementación en el archivo de proyecto personalizado. El archivo *Publish. proj* incluye una lógica condicional que omite el destino de **compilación** si el archivo se está ejecutando en Team Build. Para ello, evalúa la propiedad integrada **BuildingInTeamBuild** , que se establece automáticamente en **true** si se ejecuta el archivo de proyecto en Team Build. Como resultado, puede omitir el proceso de compilación y simplemente ejecutar el archivo de proyecto para implementar una compilación existente.

**Para crear una definición de compilación para desencadenar la implementación manualmente**

1. En Visual Studio 2010, en la ventana de **Team Explorer** , expanda el nodo del proyecto de equipo, haga clic con el botón secundario en **compilaciones**y, a continuación, haga clic en **nueva definición de compilación**.

    ![](deploying-a-specific-build/_static/image2.png)
2. En la pestaña **General** , asigne un nombre a la definición de compilación (por ejemplo, **DeployToStaging**) y una descripción opcional.
3. En la pestaña **desencadenador** , seleccione **manual: las protecciones no desencadenan una nueva compilación**.
4. En la pestaña **valores predeterminados de compilación** , en el cuadro **Copiar la salida de la compilación en la siguiente carpeta de entrega** , escriba la ruta de acceso UNC (Convención de nomenclatura universal) de la carpeta de entrega (por ejemplo, **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. En la pestaña **proceso** , en la lista desplegable **archivo del proceso de compilación** , deje seleccionado **DefaultTemplate. Xaml** . Se trata de una de las plantillas de proceso de compilación predeterminadas que se agregan a todos los proyectos de equipo nuevos.
6. En la tabla **parámetros del proceso de compilación** , haga clic en la fila **elementos para compilar** y, a continuación, haga clic en el botón de **puntos suspensivos** .

    ![](deploying-a-specific-build/_static/image4.png)
7. En el cuadro de diálogo **elementos para compilar** , haga clic en **Agregar**.
8. En la lista desplegable de **elementos de tipo** , seleccione **archivos de proyecto de MSBuild**.
9. Vaya a la ubicación del archivo de proyecto personalizado con el que controla el proceso de implementación, seleccione el archivo y, a continuación, haga clic en **Aceptar**.

    ![](deploying-a-specific-build/_static/image5.png)
10. En el cuadro de diálogo **elementos para compilar** , haga clic en **Aceptar**.
11. En la tabla **parámetros del proceso de compilación** , expanda la sección **Opciones avanzadas** .
12. En la fila **argumentos de MSBuild** , especifique la ubicación del archivo de proyecto específico del entorno y agregue un marcador de posición para la ubicación de la carpeta de compilación:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Tendrá que invalidar el valor de **OutputRoot** cada vez que pone en cola una compilación. Esto se trata en el procedimiento siguiente.
13. Haga clic en **Guardar**.

Al desencadenar una compilación, debe actualizar la propiedad **OutputRoot** para que apunte a la compilación que desea implementar.

**Para implementar una compilación específica a partir de una definición de compilación**

1. En la ventana **Team Explorer** , haga clic con el botón secundario en la definición de compilación y, a continuación, haga clic en **poner nueva compilación en cola**.

    ![](deploying-a-specific-build/_static/image7.png)
2. En el cuadro de diálogo **cola de compilación** , en la pestaña **parámetros** , expanda la sección **Opciones avanzadas** .
3. En la fila **argumentos de MSBuild** , reemplace el valor de la propiedad **OutputRoot** por la ubicación de la carpeta de compilación. Por ejemplo:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Asegúrese de incluir una barra diagonal al final de la ruta de acceso a la carpeta de compilación.
4. Haga clic en **cola**.

Al poner en cola la compilación, el archivo de proyecto implementará los scripts y paquetes Web de la base de datos desde la carpeta de entrega de compilación especificada en la propiedad **OutputRoot** .

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo publicar recursos de implementación, como paquetes web y scripts de base de datos, desde una compilación anterior específica mediante el modelo de implementación de archivos de proyecto dividido. Se explicó cómo invalidar la propiedad **OutputRoot** y cómo incorporar la lógica de implementación en una definición de compilación de TFS.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre cómo crear definiciones de compilación, vea [crear una definición de compilación básica](https://msdn.microsoft.com/library/ms181716.aspx) y [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx). Para obtener más información sobre las compilaciones en cola, vea [poner en cola una compilación](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-build-definition-that-supports-deployment.md)
> [Siguiente](configuring-permissions-for-team-build-deployment.md)
