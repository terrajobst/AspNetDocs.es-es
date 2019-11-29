---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Implementación web de ASP.NET con Visual Studio: implementación de archivos adicionales | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: eaa3141c22980f0c816e2f33b5597ac9fe69c23c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594905"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Implementación web de ASP.NET con Visual Studio: implementación de archivos adicionales

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

En este tutorial se muestra cómo extender la canalización de publicación Web de Visual Studio para realizar una tarea adicional durante la implementación. La tarea consiste en copiar archivos adicionales que no están en la carpeta del proyecto en el sitio web de destino.

En este tutorial, copiará un archivo adicional: *robots. txt*. Quiere implementar este archivo en el entorno de ensayo, pero no en el de producción. En [el tutorial implementación de producción](deploying-to-production.md) , agregó este archivo al proyecto y configuró el perfil de publicación de producción para excluirlo. En este tutorial, verá un método alternativo para controlar esta situación, uno que será útil para cualquier archivo que desee implementar pero que no quiera incluir en el proyecto.

## <a name="move-the-robotstxt-file"></a>Traslado del archivo robots. txt

Para prepararse para un método diferente de control de *robots. txt*, en esta sección del tutorial, mueva el archivo a una carpeta que no esté incluida en el proyecto y elimine *robots. txt* del entorno de ensayo. Es necesario eliminar el archivo del almacenamiento provisional para que pueda comprobar que el nuevo método de implementación del archivo en ese entorno funciona correctamente.

1. En **Explorador de soluciones**, haga clic con el botón secundario en el archivo *robots. txt* y haga clic en **excluir del proyecto**.
2. Con el explorador de archivos de Windows, cree una nueva carpeta en la carpeta de la solución y asígnele el nombre *archivo*.
3. Mueva el archivo *robots. txt* de la carpeta de proyecto *ContosoUniversity* a la carpeta de *archivos* .

    ![Carpeta de archivos.](deploying-extra-files/_static/image1.png)
4. Con la herramienta FTP, elimine el archivo *robots. txt* del sitio web de ensayo.

    Como alternativa, puede seleccionar **quitar archivos adicionales en el destino** en **Opciones de publicación de archivos** en la pestaña **configuración** del perfil de publicación de ensayo y volver a publicar en el almacenamiento provisional.

## <a name="update-the-publish-profile-file"></a>Actualizar el archivo de Perfil de publicación

Solo necesita *robots. txt* en el almacenamiento provisional, por lo que el único Perfil de publicación que necesita actualizar para implementarlo es provisional.

1. En Visual Studio, Abra *staging. pubxml*.
2. Al final del archivo, antes de la etiqueta de cierre `</Project>`, agregue el siguiente marcado:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Este código crea un nuevo *destino* que recopilará los archivos adicionales que se van a implementar. Un destino se compone de una o más tareas que MSBuild ejecutará en función de las condiciones que especifique.

    El atributo `Include` especifica que la carpeta en la que se van a buscar los archivos es *Extrafiles*, que se encuentra en el mismo nivel que la carpeta del proyecto. MSBuild recopilará todos los archivos de esa carpeta y de forma recursiva de las subcarpetas (el asterisco doble especifica subcarpetas recursivas). Con este código, podría colocar varios archivos, y archivos en subcarpetas dentro de la carpeta de *archivos* de programa, y se implementarán todos.

    El elemento `DestinationRelativePath` especifica que las carpetas y los archivos deben copiarse en la carpeta raíz del sitio web de destino, en la misma estructura de archivos y carpetas que se encuentran en la carpeta de *archivos* . Si desea copiar la carpeta de *archivos de metaarchivos* en sí, el valor `DestinationRelativePath` sería *extrafiles\%(RecursiveDir)% (nombre de archivo)% (extensión)* .
3. Al final del archivo, antes de la etiqueta de cierre `</Project>`, agregue el marcado siguiente que especifica cuándo se debe ejecutar el nuevo destino.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Este código hace que se ejecute el nuevo destino de `CustomCollectFiles` cada vez que se ejecuta el destino que copia los archivos en la carpeta de destino. Existe un destino independiente para la creación de paquetes e implementaciones, y el nuevo destino se inserta en ambos destinos en caso de que decida realizar la implementación mediante un paquete de implementación en lugar de publicarlo.

    El archivo *. pubxml* ahora es similar al ejemplo siguiente:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Guarde y cierre el archivo *staging. pubxml* .

## <a name="publish-to-staging"></a>Publicar en el almacenamiento provisional

Mediante el uso de la publicación con un solo clic o la línea de comandos, publique la aplicación mediante el perfil de almacenamiento provisional.

Si usa la publicación con un solo clic, puede comprobar en la ventana de **vista previa** que se copiará el archivo *robots. txt* . De lo contrario, use la herramienta FTP para comprobar que el archivo *robots. txt* se encuentra en la carpeta raíz del sitio web después de la implementación.

## <a name="summary"></a>Resumen

Esto completa esta serie de tutoriales sobre la implementación de una aplicación Web de ASP.NET en un proveedor de hospedaje de terceros. Para obtener más información sobre cualquiera de los temas descritos en estos tutoriales, consulte el [mapa de contenido de implementación de ASP.net](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Más información

Si sabe cómo trabajar con archivos de MSBuild, puede automatizar muchas otras tareas de implementación escribiendo código en archivos *. pubxml* (para tareas específicas del perfil) o el archivo Project *. WPP. targets* (para las tareas que se aplican a todos los perfiles). Para obtener más información sobre los archivos *. pubxml* y *. WPP. targets* , vea [Cómo: editar la configuración de implementación en archivos de Perfil de publicación (. pubxml) y el archivo. WPP. targets en proyectos Web de Visual Studio](https://msdn.microsoft.com/library/ff398069). Para obtener una introducción básica a código de MSBuild, vea **la anatomía de un archivo de proyecto** en [la serie de implementación empresarial: Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Para obtener información sobre cómo trabajar con archivos de MSBuild para realizar tareas para sus propios escenarios, vea este libro: [en el Microsoft Build Engine: usar MSBuild y Team Foundation Build](http://msbuildbook.com) de Sayed Ibraham Hashimi y William Bartholomew.

## <a name="acknowledgements"></a>Agradecimientos

Me gustaría agradecer a las siguientes personas que realizaran importantes contribuciones al contenido de esta serie de tutoriales:

- [Alberto poblacion, MVP &amp; MCT, España](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, MVP de desarrollo de plataforma de datos, Estados Unidos
- Mittal duras, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italia](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](command-line-deployment.md)
> [Siguiente](troubleshooting.md)
