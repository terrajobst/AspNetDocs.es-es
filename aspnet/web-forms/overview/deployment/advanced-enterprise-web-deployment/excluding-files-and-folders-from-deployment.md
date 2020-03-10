---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Excluir archivos y carpetas de la implementación | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo puede excluir archivos y carpetas de un paquete de implementación web al compilar y empaquetar un proyecto de aplicación Web.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: a262ce43d7199fb1015d54d0b7c213857c360946
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438421"
---
# <a name="excluding-files-and-folders-from-deployment"></a>Excluir archivos y carpetas de la implementación

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo puede excluir archivos y carpetas de un paquete de implementación web al compilar y empaquetar un proyecto de aplicación Web.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="overview"></a>Información general

Al compilar un proyecto de aplicación web en Visual Studio 2010, la canalización de publicación web (WPP) le permite ampliar este proceso de compilación empaquetando la aplicación web compilada en un paquete web que se puede implementar. Después, puede usar la herramienta de implementación web (Web Deploy) de Internet Information Services (IIS) para implementar este paquete Web en un servidor Web de IIS remoto, o bien importar el paquete Web manualmente a través del administrador de IIS. Este proceso de empaquetado se explica en [compilar y empaquetar proyectos de aplicación web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

¿Cómo puede controlar lo que se incluye en el paquete Web? La configuración del proyecto en Visual Studio, a través del archivo de proyecto subyacente, proporciona un control suficiente para muchos escenarios. Sin embargo, en algunos casos es posible que desee personalizar el contenido del paquete Web en entornos de destino específicos. Por ejemplo, puede que desee incluir una carpeta para los archivos de registro al implementar la aplicación en un entorno de prueba, pero excluir la carpeta al implementar la aplicación en un entorno de ensayo o de producción. En este tema se muestra cómo hacerlo.

## <a name="what-gets-included-by-default"></a>¿Qué se incluye de forma predeterminada?

Al configurar las propiedades del proyecto de aplicación web en Visual Studio, la lista **elementos para implementar** en la página **Web de empaquetar/publicar** permite especificar lo que se desea incluir en el paquete de implementación web. De forma predeterminada, se establece en **solo los archivos necesarios para ejecutar esta aplicación**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Cuando elija **solo los archivos necesarios para ejecutar esta aplicación**, WPP intentará determinar qué archivos se deben agregar al paquete Web. Esto incluye:

- Todas las salidas de compilación del proyecto.
- Cualquier archivo marcado con una acción de compilación de **contenido**.

> [!NOTE]
> La lógica que determina los archivos que se van a incluir está contenida en este archivo:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft. Web. Publishing. OnlyFilesToRunTheApp. targets*

## <a name="excluding-specific-files-and-folders"></a>Exclusión de archivos y carpetas específicos

En algunos casos, querrá tener un control más preciso sobre qué archivos y carpetas se implementan. Si sabe qué archivos desea excluir con anterioridad y la exclusión se aplica a todos los entornos de destino, puede simplemente establecer la **acción de compilación** de cada archivo en **ninguno**.

**Para excluir archivos específicos de la implementación**

1. En la ventana **Explorador de soluciones** , haga clic con el botón secundario en el archivo y, a continuación, haga clic en **propiedades**.
2. En la ventana **propiedades** , en la fila **acción de compilación** , seleccione **ninguno**.

Sin embargo, este enfoque no siempre es práctico. Por ejemplo, puede que desee variar qué archivos y carpetas se incluyen según el entorno de destino y desde fuera de Visual Studio. Por ejemplo, en la solución de ejemplo Contact Manager, eche un vistazo al contenido del proyecto ContactManager. Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- La carpeta Internal contiene algunos scripts SQL que el programador usa para crear, quitar y rellenar las bases de datos locales con fines de desarrollo. No se debe implementar nada en esta carpeta en un entorno de ensayo o de producción.
- La carpeta scripts contiene varios archivos JavaScript. Muchos de estos archivos se incluyen únicamente para admitir la depuración o proporcionar IntelliSense en Visual Studio. Algunos de estos archivos no se deben implementar en entornos de ensayo o de producción. Sin embargo, puede que desee implementarlos en un entorno de prueba de desarrollador para facilitar la solución de problemas.

Aunque podría manipular los archivos de proyecto para excluir archivos y carpetas específicos, hay una manera más fácil. WPP incluye un mecanismo para excluir archivos y carpetas mediante la creación de listas de elementos denominadas **ExcludeFromPackageFolders** y **ExcludeFromPackageFiles**. Puede extender este mecanismo agregando sus propios elementos a estas listas. Para ello, debe completar estos pasos de alto nivel:

1. Cree un archivo de proyecto personalizado denominado *[nombre de proyecto]. WPP. targets* en la misma carpeta que el archivo de proyecto.

    > [!NOTE]
    > El archivo *. WPP. targets* debe ir en la misma carpeta que el archivo&#x2014;de proyecto de aplicación Web, por ejemplo, *ContactManager. Mvc. csproj*&#x2014;en lugar de en la misma carpeta que los archivos de proyecto personalizados que se usan para controlar el proceso de compilación e implementación.
2. En el archivo *. WPP. targets* , agregue un elemento **ItemGroup** .
3. En el elemento **ItemGroup** , agregue los elementos **ExcludeFromPackageFolders** y **ExcludeFromPackageFiles** para excluir archivos y carpetas específicos, según sea necesario.

Esta es la estructura básica de este archivo *. WPP. targets* :

[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]

Tenga en cuenta que cada elemento incluye un elemento de metadatos de elemento denominado **FromTarget**. Este es un valor opcional que no afecta al proceso de compilación; simplemente sirve para indicar por qué se omitieron determinados archivos o carpetas si alguien revisa los registros de compilación.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Exclusión de archivos y carpetas de un paquete Web

En el procedimiento siguiente se muestra cómo agregar un archivo *. WPP. targets* a un proyecto de aplicación web y cómo usar el archivo para excluir archivos y carpetas específicos del paquete web al compilar el proyecto.

**Para excluir archivos y carpetas de un paquete de implementación web**

1. Abra la solución en Visual Studio 2010.
2. En la ventana de **Explorador de soluciones** , haga clic con el botón secundario en el nodo de proyecto de aplicación web (por ejemplo, **ContactManager. Mvc**), seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento**.
3. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la plantilla de **archivo XML** .
4. En el cuadro **nombre** , escriba *[nombre del proyecto] * * *. WPP. targets** (por ejemplo, **ContactManager. Mvc. WPP. targets**) y, a continuación, haga clic en **Agregar**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Si agrega un nuevo elemento al nodo raíz de un proyecto, el archivo se crea en la misma carpeta que el archivo de proyecto. Para comprobarlo, abra la carpeta en el explorador de Windows.
5. En el archivo, agregue un elemento **Project** y un elemento **ItemGroup** :

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Si desea excluir carpetas del paquete Web, agregue un elemento **ExcludeFromPackageFolders** al elemento **ItemGroup** :

   1. En el atributo **include** , proporcione una lista separada por punto y coma de las carpetas que desea excluir.
   2. En el elemento de metadatos **FromTarget** , proporcione un valor significativo para indicar por qué se excluyen las carpetas, como el nombre del archivo *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Si desea excluir archivos del paquete Web, agregue un elemento **ExcludeFromPackageFiles** al elemento **ItemGroup** :

   1. En el atributo **include** , proporcione una lista separada por puntos y comas de los archivos que desea excluir.
   2. En el elemento de metadatos **FromTarget** , proporcione un valor significativo para indicar por qué se excluyen los archivos, como el nombre del archivo *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. El archivo *[nombre de proyecto]. WPP. targets* ahora debe ser similar al siguiente:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Guarde y cierre el archivo *[nombre del proyecto]. WPP. targets* .

La próxima vez que compile y empaquete el proyecto de aplicación Web, WPP detectará automáticamente el archivo *. WPP. targets* . Los archivos y las carpetas especificados no se incluirán en el paquete Web.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo excluir archivos y carpetas específicos al compilar un paquete Web, mediante la creación de un archivo personalizado *. WPP. targets* en la misma carpeta que el archivo de proyecto de aplicación Web.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el uso de archivos de proyecto de Microsoft Build Engine personalizado (MSBuild) para controlar el proceso de implementación, vea [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre el proceso de empaquetado e implementación, vea [compilar y empaquetar proyectos de aplicación web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configurar parámetros para la implementación de paquetes web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)e [implementar paquetes web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Anterior](deploying-membership-databases-to-enterprise-environments.md)
> [Siguiente](taking-web-applications-offline-with-web-deploy.md)
