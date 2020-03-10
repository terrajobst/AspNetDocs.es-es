---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de solo código-8 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: e4d094ef84a747c36ce05ddb0e3d1ce0391d5605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78455077"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: implementar una actualización de solo código-8 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

Después de la implementación inicial, el trabajo de mantenimiento y desarrollo del sitio web continúa y antes de que se desee implementar una actualización. Este tutorial le guiará por el proceso de implementación de una actualización en el código de la aplicación. Esta actualización no implica un cambio en la base de datos; en el siguiente tutorial, verá qué es diferente de la implementación de un cambio en la base de datos.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Realización de un cambio de código

Como ejemplo sencillo de una actualización de la aplicación, agregará a la página de **instructores** una lista de cursos impartidos por el instructor seleccionado.

Si ejecuta la página **instructores** , observará que hay vínculos **seleccionados** en la cuadrícula, pero que no hacen nada más que hacer que el fondo de la fila se convierta en gris.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Ahora agregará el código que se ejecuta cuando se hace clic en el vínculo **seleccionar** y muestra una lista de los cursos impartidos por el instructor seleccionado.

En *instructors. aspx*, agregue el siguiente marcado inmediatamente después del control **ErrorMessageLabel** `Label`:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Ejecute la página y seleccione un instructor. Verá una lista de los cursos impartidos por ese instructor.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Implementar la actualización de código en el entorno de prueba

La implementación en el entorno de prueba es una cuestión sencilla de ejecutar la publicación con un solo clic de nuevo. Para que este proceso sea más rápido, puede usar la barra de herramientas de **publicación de un solo clic** .

En el menú **Ver** , elija **barras de herramientas** y, después, **haga clic en publicación web**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

En **Explorador de soluciones**, seleccione el proyecto ContosoUniversity.

en la barra de herramientas de **publicación de un solo clic** , elija el perfil de publicación de **prueba** y, a continuación, haga clic en **publicar web** (el icono con flechas que apuntan a la izquierda y a la derecha)

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio implementa la aplicación actualizada y el explorador se abre automáticamente en la Página principal. Ejecute la página Instructors y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalmente también realizaría pruebas de regresión (es decir, probar el resto del sitio para asegurarse de que el nuevo cambio no interrumpió ninguna funcionalidad existente). Pero para este tutorial omitirá ese paso y continuará con la implementación de la actualización en producción.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedir la reimplementación del estado de la base de datos inicial en producción

En una aplicación real, los usuarios interactúan con el sitio de producción después de la implementación inicial y las bases de datos se rellenan con datos en directo. Por lo tanto, no desea volver a implementar la base de datos de pertenencia en su estado inicial, lo que eliminaría todos los datos activos. Dado que las bases de datos de SQL Server Compact son archivos de la carpeta *app\_Data* , debe evitar esto cambiando la configuración de implementación para que no se implementen los archivos de la carpeta *\_Data* .

Abra la ventana **propiedades del proyecto** para el proyecto ContosoUniversity y seleccione la pestaña **empaquetar/publicar web** . Asegúrese de que el cuadro desplegable **configuración** tiene **activa (versión)** o **versión** seleccionada, y seleccione **excluir archivos de la carpeta\_de datos de la aplicación**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

En caso de que decida implementar una compilación de depuración en el futuro, es una buena idea realizar el mismo cambio para la configuración de compilación de depuración: cambie la **configuración** a **depurar** y, a continuación, seleccione **excluir archivos de la carpeta de\_de datos de la aplicación**.

Guarde y cierre la pestaña **empaquetar/publicar web** .

> [!NOTE] 
> 
> [!IMPORTANT]
> Asegúrese de que no tiene seleccionados **quitar archivos adicionales** en el destino en los perfiles de publicación. Si selecciona esta opción, el proceso de implementación eliminará las bases de datos que tiene en los datos de\_de la aplicación en el sitio implementado y eliminará la aplicación\_carpeta de datos.

## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedir el acceso de los usuarios al sitio de producción durante la actualización

El cambio que está implementando ahora es un cambio sencillo en una sola página. Pero a veces se implementan cambios mayores y, en ese caso, el sitio puede comportarse de forma extraña si un usuario solicita una página antes de que finalice la implementación. Para evitarlo, puede usar una *aplicación\_archivo. htm sin conexión* . Cuando se coloca un archivo con el nombre *app\_offline. htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación. Por lo tanto, para evitar el acceso durante la implementación, coloque *app\_offline. htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quite *App\_offline. htm*.

En **Explorador de soluciones**, haga clic con el botón secundario en la solución (no en uno de los proyectos) y seleccione **nueva carpeta de soluciones**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Asigne a la carpeta el nombre *SolutionFiles*.

En la nueva carpeta, cree una página HTML denominada *app\_offline. htm*. Reemplace el contenido existente por el marcado siguiente:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Puede copiar la *aplicación\_archivo. htm sin conexión* en el sitio mediante una conexión FTP o la utilidad **Administrador de archivos** en el panel de control del proveedor de hospedaje. En este tutorial, usará el administrador de **archivos**.

Abra el panel de control y seleccione **Administrador de archivos** como hizo en el tutorial [implementación del entorno de producción](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Seleccione **contosouniversity.com** y, a continuación, **wwwroot** para acceder a la carpeta raíz de la aplicación y, a continuación, haga clic en **cargar**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

En el cuadro de diálogo **Cargar archivo** , seleccione la *aplicación\_archivo. htm sin conexión* y, a continuación, haga clic en **cargar**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Vaya a la dirección URL del sitio. Verá que la *aplicación\_página offline. htm* aparece ahora en lugar de la Página principal.

[![App_offline. htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Ahora está listo para realizar la implementación en producción.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Implementación de la actualización del código en el entorno de producción

En la barra de herramientas de **publicación en Web** , elija el perfil de publicación de **producción** y haga clic en **publicar web**.

Visual Studio implementa la aplicación actualizada y abre el explorador en la Página principal del sitio. Se muestra la *aplicación\_archivo. htm sin conexión* . Antes de que pueda realizar pruebas para comprobar que la implementación se ha realizado correctamente, debe quitar la *aplicación\_archivo. htm sin conexión* .

Vuelva a la aplicación **Administrador de archivos** en el panel de control. Seleccione **contosouniversity.com** y **wwwroot**, seleccione **App\_offline. htm**y, a continuación, haga clic en **eliminar**.

[![Deleting_app_offline. htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

En el explorador, abra la página de instructores en el sitio público y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Ahora ha implementado una actualización de la aplicación que no implica un cambio en la base de datos. En el siguiente tutorial se muestra cómo implementar un cambio en la base de datos.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
