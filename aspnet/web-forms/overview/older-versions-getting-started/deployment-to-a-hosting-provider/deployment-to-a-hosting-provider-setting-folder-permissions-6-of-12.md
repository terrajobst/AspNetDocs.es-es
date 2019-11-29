---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: establecimiento de permisos de carpeta-6 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 85a77a196cf3458bbb2e6308838a846936cd070b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633498"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: establecimiento de permisos de carpeta: 6 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general del

En este tutorial, establecerá permisos de carpeta para la carpeta *Elmah* en el sitio Web implementado para que la aplicación pueda crear archivos de registro en esa carpeta.

Al probar una aplicación web en Visual Studio con el Servidor de desarrollo de Visual Studio (Cassini), la aplicación se ejecuta bajo su identidad. Lo más probable es que sea un administrador en el equipo de desarrollo y que tenga autoridad completa para realizar cualquier acción en cualquier archivo de cualquier carpeta. Pero cuando una aplicación se ejecuta en IIS, se ejecuta con la identidad definida para el grupo de aplicaciones al que está asignado el sitio. Normalmente, se trata de una cuenta definida por el sistema que tiene permisos limitados. De forma predeterminada, tiene permisos de lectura y ejecución en los archivos y carpetas de la aplicación Web, pero no tiene acceso de escritura.

Esto se convierte en un problema si la aplicación crea o actualiza archivos, lo que es una necesidad común de las aplicaciones Web. En la aplicación contoso University, Elmah crea archivos XML en la carpeta *Elmah* con el fin de guardar los detalles sobre los errores. Incluso si no usa algo como Elmah, el sitio podría permitir que los usuarios carguen archivos o realicen otras tareas que escriben datos en una carpeta de su sitio.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Probar el registro de errores y los informes

Para ver cómo la aplicación no funciona correctamente en IIS (aunque lo hizo cuando lo probó en Visual Studio), puede producirse un error que normalmente se registrará en Elmah y, a continuación, abrir el registro de errores Elmah para ver los detalles. Si Elmah no pudo crear un archivo XML y almacenar los detalles del error, verá un informe de errores vacío.

Abra un explorador y vaya a `http://localhost/ContosoUniversity`y, a continuación, solicite una dirección URL no válida como *Studentsxxx. aspx*. Verá una página de error generada por el sistema en lugar de la página *GenericErrorPage. aspx* porque el valor `customErrors` en el archivo Web. config es "RemoteOnly" y está ejecutando IIS localmente:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Ahora ejecute *Elmah. axd* para ver el informe de errores. Verá una página de registro de errores vacía porque Elmah no pudo crear un archivo XML en la carpeta *Elmah* :

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Establecer el permiso de escritura en la carpeta Elmah

Puede establecer permisos de carpeta manualmente o puede convertirlo en una parte automática del proceso de implementación. Hacer que sea automático requiere código MSBuild complejo y, puesto que solo tiene que hacerlo la primera vez que implementa, en este tutorial solo se muestra cómo hacerlo manualmente. (Para obtener información sobre cómo hacer esta parte del proceso de implementación, consulte [configuración de permisos de carpeta en la publicación web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) en el blog de Sayed Hashimi).

En el **Explorador de Windows**, vaya a *C:\inetpub\wwwroot\ContosoUniversity*. Haga clic con el botón derecho en la carpeta *Elmah* , seleccione **propiedades**y, a continuación, seleccione la pestaña **seguridad** .

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Si no ve **DefaultAppPool** en la lista **nombres de grupos o usuarios** , es probable que use algún otro método que el especificado en este tutorial para configurar IIS y ASP.net 4 en el equipo. En ese caso, averigüe qué identidad usa el grupo de aplicaciones asignado a la aplicación contoso University y conceda permiso de escritura a esa identidad. Vea los vínculos acerca de las identidades del grupo de aplicaciones al final de este tutorial).

Haga clic en **Editar**. En el cuadro de diálogo **permisos de Elmah** , seleccione **DefaultAppPool**y, a continuación, active la casilla **escribir** en la columna **permitir** .

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Haga clic en **Aceptar** en ambos cuadros de diálogo.

## <a name="retesting-error-logging-and-reporting"></a>Reprobar el registro de errores y los informes

Realice una prueba de forma que se produzca un error de la misma manera (solicite una dirección URL incorrecta) y ejecute la página **registro de errores** . Esta vez el error aparece en la página.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

También necesita permiso de escritura en la carpeta de datos de la\_de la *aplicación* porque tiene SQL Server Compact archivos de base de datos en esa carpeta y desea poder actualizar los datos de esas bases de datos. En ese caso, sin embargo, no tiene que hacer nada más, ya que el proceso de implementación establece automáticamente el permiso de escritura en la carpeta *\_de datos* de la aplicación.

Ya ha completado todas las tareas necesarias para que contoso University funcione correctamente en IIS en el equipo local. En el siguiente tutorial, hará que el sitio esté disponible públicamente mediante su implementación en un proveedor de hospedaje.

## <a name="more-information"></a>Más información

En este ejemplo, el motivo por el que Elmah no pudo guardar los archivos de registro era bastante obvio. Puede usar el seguimiento de IIS en los casos en los que la causa del problema no sea tan obvia; consulte [solución de problemas de solicitudes con error mediante el seguimiento en IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en el sitio de IIS.net.

Para obtener más información sobre cómo conceder permisos a las identidades del grupo de aplicaciones, vea [identidades del grupo de aplicaciones](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) y [contenido seguro en IIS a través de las ACL del sistema de archivos](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) en el sitio de IIS.net.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
