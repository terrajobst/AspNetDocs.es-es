---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Desconectar aplicaciones web con Web Deploy | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo poner una aplicación web sin conexión mientras dure una implementación automatizada mediante el cuand Web de Internet Information Services (IIS)...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: ba60664a0c3daa0650cd7e7cfc4ab9da08df3440
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77075143"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Desconectar aplicaciones web con Web Deploy

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo poner una aplicación web sin conexión mientras dure una implementación automatizada mediante la herramienta de implementación web de Internet Information Services (IIS) (Web Deploy). Los usuarios que navegan a la aplicación web se redirigen a una *aplicación\_archivo. htm sin conexión* hasta que se completa la implementación.

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

En muchos escenarios, querrá dejar sin conexión una aplicación web mientras realiza cambios en los componentes relacionados, como bases de datos o servicios Web. Normalmente, en IIS y ASP.NET, esto se consigue colocando un archivo denominado *App\_offline. htm* en la carpeta raíz del sitio web o la aplicación Web de IIS. La *aplicación\_archivo. htm sin conexión* es un archivo HTML estándar y normalmente contiene un mensaje sencillo que le advierte al usuario de que el sitio no está disponible temporalmente debido al mantenimiento. Mientras la *aplicación\_archivo. htm sin conexión* se encuentra en la carpeta raíz del sitio web, IIS redirigirá automáticamente las solicitudes al archivo. Cuando haya terminado de hacer las actualizaciones, quite la *aplicación\_archivo. htm sin conexión* y el sitio web reanudará el servicio de solicitudes como de costumbre.

Cuando use Web Deploy para realizar implementaciones automatizadas o de un solo paso en un entorno de destino, es posible que desee incorporar agregar y quitar la *aplicación\_archivo. htm sin conexión* en el proceso de implementación. Para ello, deberá completar estas tareas de alto nivel:

- En el archivo de proyecto de Microsoft Build Engine (MSBuild) que se usa para controlar el proceso de implementación, cree un destino de MSBuild que copie una *aplicación\_archivo. htm sin conexión* en el servidor de destino antes de que se inicie cualquier tarea de implementación.
- Agregue otro destino de MSBuild que quite la *aplicación\_archivo. htm sin conexión* del servidor de destino cuando se hayan completado todas las tareas de implementación.
- En el proyecto de aplicación Web, cree un archivo *. WPP. targets* que garantice que una *aplicación\_archivo. htm sin conexión* se agrega al paquete de implementación cuando se invoca web deploy.

En este tema se muestra cómo llevar a cabo estos procedimientos. En las tareas y los tutoriales de este tema se supone que ya ha creado una solución que contiene al menos un proyecto de aplicación web y que usa un archivo de proyecto personalizado para controlar el proceso de implementación como se describe en [implementación web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Como alternativa, puede usar la solución de ejemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) para seguir los ejemplos del tema.

## <a name="adding-an-app_offline-file-to-a-web-application-project"></a>Agregar una aplicación\_archivo sin conexión a un proyecto de aplicación Web

La primera tarea que debe completar es agregar una *aplicación\_archivo sin conexión* al proyecto de aplicación web:

- Para evitar que el archivo interfiera con el proceso de desarrollo (no quiere que la aplicación esté sin conexión de forma permanente), debe llamarlo otra cosa que no sea *App\_offline. htm*. Por ejemplo, puede asignar un nombre a la aplicación de archivo *\_offline-template. htm*.
- Para evitar que el archivo se implemente tal cual, debe establecer la acción de compilación en **ninguno**.

**Para agregar una aplicación\_archivo sin conexión a un proyecto de aplicación Web**

1. Abra la solución en Visual Studio 2010.
2. En la ventana de **Explorador de soluciones** , haga clic con el botón secundario en el proyecto de aplicación Web, seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento**.
3. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **página html**.
4. En el cuadro **nombre** , escriba **App\_offline-template. htm**y, a continuación, haga clic en **Agregar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Agregue un HTML sencillo para informar a los usuarios de que la aplicación no está disponible y, a continuación, guarde el archivo. No incluya ninguna etiqueta en el servidor (por ejemplo, cualquier etiqueta que tenga el prefijo "ASP:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. En la ventana **Explorador de soluciones** , haga clic con el botón secundario en el nuevo archivo y, a continuación, haga clic en **propiedades**.
7. En la ventana **propiedades** , en la fila **acción de compilación** , seleccione **ninguno**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-app_offline-file"></a>Implementación y eliminación de una aplicación\_archivo sin conexión

El siguiente paso consiste en modificar la lógica de implementación para copiar el archivo en el servidor de destino al inicio del proceso de implementación y quitarlo al final.

> [!NOTE]
> En el procedimiento siguiente se supone que está utilizando un archivo de proyecto de MSBuild personalizado para controlar el proceso de implementación, como se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Si va a implementar Direct desde Visual Studio, deberá usar un enfoque diferente. Sayed Ibrahim Hashimi describe uno de estos métodos en [Cómo poner la aplicación web sin conexión durante la publicación](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).

Para implementar una *aplicación\_archivo sin conexión* en un sitio web de IIS de destino, debe invocar MSDeploy. exe con el [proveedor de **contentPath** web deploy](https://technet.microsoft.com/library/dd569034(WS.10).aspx). El proveedor de **contentPath** admite rutas de acceso de directorio físicas y rutas de acceso de aplicaciones o sitios web de IIS, lo que la convierte en la opción ideal para sincronizar un archivo entre una carpeta de proyecto de Visual Studio y una aplicación Web de IIS. Para implementar el archivo, el comando MSDeploy debe ser similar a este:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]

Para quitar el archivo del sitio de destino al final del proceso de implementación, el comando MSDeploy debe ser similar a este:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]

Para automatizar estos comandos como parte de un proceso de compilación e implementación, debe integrarlos en el archivo de proyecto de MSBuild personalizado. En el procedimiento siguiente se describe cómo hacerlo.

**Para implementar y eliminar una aplicación\_archivo sin conexión**

1. En Visual Studio 2010, abra el archivo de proyecto de MSBuild que controla el proceso de implementación. En la solución de ejemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , este es el archivo *Publish. proj* .
2. En el elemento de **proyecto** raíz, cree un nuevo elemento **PropertyGroup** para almacenar las variables de la *aplicación\_implementación sin conexión* :

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. La propiedad **SourceRoot** se define en otra parte del archivo *Publish. proj* . Indica la ubicación de la carpeta raíz para el contenido de origen en relación con la ruta&#x2014;de acceso actual, en otras palabras, con respecto a la ubicación del archivo *Publish. proj* .
4. El proveedor de **contentPath** no aceptará las rutas de acceso relativas al archivo, por lo que debe obtener una ruta de acceso absoluta al archivo de código fuente antes de poder implementarlo. Puede usar la tarea [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) para hacerlo.
5. Agregue un nuevo elemento de **destino** denominado **GetAppOfflineAbsolutePath**. Dentro de este destino, use la tarea **ConvertToAbsolutePath** para obtener una ruta de acceso absoluta a la *aplicación\_archivo de plantilla sin conexión* en la carpeta del proyecto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Este destino toma la ruta de acceso relativa a la *aplicación\_archivo de plantilla sin conexión* en la carpeta del proyecto y lo guarda en una nueva propiedad como una ruta de acceso de archivo absoluta. El atributo **BeforeTargets** especifica que desea que este destino se ejecute antes que el destino **DeployAppOffline** , que creará en el paso siguiente.
7. Agregue un nuevo destino denominado **DeployAppOffline**. Dentro de este destino, invoque el comando MSDeploy. exe que implementa la *aplicación\_archivo sin conexión* en el servidor Web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. En este ejemplo, la propiedad **ContactManagerIisPath** se define en otra parte del archivo de proyecto. Esto es simplemente una ruta de acceso de la aplicación de IIS, con el formato *[nombre del sitio web de IIS]/[nombre de la aplicación]* . La inclusión de una condición en el destino permite a los usuarios cambiar la *aplicación\_implementación sin conexión* de manera activa o inactiva cambiando un valor de propiedad o proporcionando un parámetro de línea de comandos.
9. Agregue un nuevo destino denominado **DeleteAppOffline**. Dentro de este destino, invoque el comando MSDeploy. exe que quita la *aplicación\_archivo sin conexión* del servidor Web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. La tarea final es invocar estos nuevos destinos en los puntos adecuados durante la ejecución del archivo de proyecto. Puede hacerlo de varias maneras. Por ejemplo, en el archivo *Publish. proj* , la propiedad **FullPublishDependsOn** especifica una lista de destinos que se deben ejecutar en orden cuando se invoca el destino predeterminado de **FullPublish** .
11. Modifique el archivo de proyecto de MSBuild para invocar los destinos **DeployAppOffline** y **DeleteAppOffline** en los puntos adecuados del proceso de publicación.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Al ejecutar el archivo de proyecto de MSBuild personalizado, la *aplicación\_archivo sin conexión* se implementará en el servidor inmediatamente después de una compilación correcta. Después se eliminará del servidor una vez que se hayan completado todas las tareas de implementación.

## <a name="adding-an-app_offline-file-to-deployment-packages"></a>Agregar una aplicación\_archivo sin conexión a paquetes de implementación

En función de cómo configure la implementación, el contenido existente en la aplicación&#x2014;Web de IIS de destino como la *aplicación\_archivo* &#x2014;. htm sin conexión se puede eliminar automáticamente al implementar un paquete Web en el destino. Para asegurarse de que la *aplicación\_archivo. htm sin conexión* permanece en su lugar mientras dure la implementación, debe incluir el archivo en el propio paquete de implementación web además de implementar el archivo directamente al inicio del proceso de implementación.

- Si ha seguido las tareas anteriores de este tema, habrá agregado la *aplicación\_archivo. htm sin conexión* al proyecto de aplicación web en un nombre de archivo diferente (hemos usado *App\_offline-template. htm*) y habrá establecido la acción de compilación en **ninguno**. Estos cambios son necesarios para evitar que el archivo interfiera con el desarrollo y la depuración. Como resultado, debe personalizar el proceso de empaquetado para asegurarse de que la *aplicación\_archivo. htm sin conexión* se incluye en el paquete de implementación web.

La canalización de publicación web (WPP) usa una lista de elementos denominada **FilesForPackagingFromProject** para compilar una lista de archivos que deben incluirse en el paquete de implementación web. Puede personalizar el contenido de los paquetes Web agregando sus propios elementos a esta lista. Para ello, debe completar estos pasos de alto nivel:

1. Cree un archivo de proyecto personalizado denominado *[nombre de proyecto]. WPP. targets* en la misma carpeta que el archivo de proyecto.

    > [!NOTE]
    > El archivo *. WPP. targets* debe ir en la misma carpeta que el archivo&#x2014;de proyecto de aplicación Web, por ejemplo, *ContactManager. Mvc. csproj*&#x2014;en lugar de en la misma carpeta que los archivos de proyecto personalizados que se usan para controlar el proceso de compilación e implementación.
2. En el archivo *. WPP. targets* , cree un nuevo destino de MSBuild que se ejecute *antes* que el destino **CopyAllFilesToSingleFolderForPackage** . Este es el destino WPP que genera una lista de elementos que se van a incluir en el paquete.
3. En el nuevo destino, cree un elemento **ItemGroup** .
4. En el elemento **ItemGroup** , agregue un elemento **FilesForPackagingFromProject** y especifique la *aplicación\_archivo. htm sin conexión* .

El archivo *. WPP. targets* debe ser similar a este:

[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]

Estos son los puntos clave de la nota en este ejemplo:

- El atributo **BeforeTargets** inserta este destino en WPP especificando que se debe ejecutar inmediatamente antes del destino **CopyAllFilesToSingleFolderForPackage** .
- El elemento **FilesForPackagingFromProject** usa el valor de metadatos **DestinationRelativePath** para cambiar el nombre del archivo de *App\_offline-template. htm* a *App\_offline. htm* a medida que se agrega a la lista.

En el procedimiento siguiente se muestra cómo agregar este archivo *. WPP. targets* a un proyecto de aplicación Web.

**Para agregar un archivo. WPP. targets a un paquete de implementación web**

1. Abra la solución en Visual Studio 2010.
2. En la ventana de **Explorador de soluciones** , haga clic con el botón secundario en el nodo de proyecto de aplicación web (por ejemplo, **ContactManager. Mvc**), seleccione **Agregar**y, a continuación, haga clic en **nuevo elemento**.
3. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la plantilla de **archivo XML** .
4. En el cuadro **nombre** , escriba *[nombre del proyecto] * * *. WPP. targets** (por ejemplo, **ContactManager. Mvc. WPP. targets**) y, a continuación, haga clic en **Agregar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Si agrega un nuevo elemento al nodo raíz de un proyecto, el archivo se crea en la misma carpeta que el archivo de proyecto. Para comprobarlo, abra la carpeta en el explorador de Windows.
5. En el archivo, agregue el marcado de MSBuild que se ha descrito anteriormente.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Guarde y cierre el archivo *[nombre del proyecto]. WPP. targets* .

La próxima vez que compile y empaquete el proyecto de aplicación Web, WPP detectará automáticamente el archivo *. WPP. targets* . La *aplicación\_archivo offline-template. htm* se incluirá en el paquete de implementación web resultante como *App\_offline. htm*.

> [!NOTE]
> Si se produce un error en la implementación, la *aplicación\_archivo. htm sin conexión* permanecerá en su lugar y la aplicación permanecerá sin conexión. Este suele ser el comportamiento deseado. Para volver a poner la aplicación en línea, puede eliminar la *aplicación\_archivo. htm sin conexión* del servidor Web. Como alternativa, si corrige los errores y ejecuta una implementación correcta, se quitará la *aplicación\_archivo. htm sin conexión* .

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo poner una aplicación web sin conexión mientras dure una implementación; para ello, se publica una *aplicación\_archivo. htm sin conexión* en el servidor de destino al inicio del proceso de implementación y se quita al final. También se describe cómo incluir una *aplicación\_archivo. htm sin conexión* en un paquete de implementación web.

## <a name="further-reading"></a>Lecturas adicionales

Para obtener más información sobre el proceso de empaquetado e implementación, vea [compilar y empaquetar proyectos de aplicación web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configurar parámetros para la implementación de paquetes web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)e [implementar paquetes web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Si publica las aplicaciones web directamente desde Visual Studio, en lugar de usar el enfoque de archivo de proyecto de MSBuild personalizado descrito en estos tutoriales, deberá usar un enfoque ligeramente diferente para poner la aplicación sin conexión durante la publicación. Procese.

> [!div class="step-by-step"]
> [Anterior](excluding-files-and-folders-from-deployment.md)
> [Siguiente](running-windows-powershell-scripts-from-msbuild-project-files.md)
