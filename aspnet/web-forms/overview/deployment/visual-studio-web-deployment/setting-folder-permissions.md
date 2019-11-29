---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Implementación web de ASP.NET con Visual Studio: configuración de permisos de carpeta | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 410525bb2e3f6e5a0be6d7d6b33fb3a40509041a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614938"
---
# <a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Implementación web de ASP.NET con Visual Studio: configuración de permisos de carpeta

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

En este tutorial, establecerá permisos de carpeta para la carpeta *Elmah* en el sitio Web implementado para que la aplicación pueda crear archivos de registro en esa carpeta.

Al probar una aplicación web en Visual Studio con el Servidor de desarrollo de Visual Studio (Cassini) o IIS Express, la aplicación se ejecuta bajo su identidad. Lo más probable es que sea un administrador en el equipo de desarrollo y que tenga autoridad completa para realizar cualquier acción en cualquier archivo de cualquier carpeta. Pero cuando una aplicación se ejecuta en IIS, se ejecuta con la identidad definida para el grupo de aplicaciones al que está asignado el sitio. Normalmente, se trata de una cuenta definida por el sistema que tiene permisos limitados. De forma predeterminada, tiene permisos de lectura y ejecución en los archivos y carpetas de la aplicación Web, pero no tiene acceso de escritura.

Esto se convierte en un problema si la aplicación crea o actualiza archivos, lo que es una necesidad común de las aplicaciones Web. En la aplicación contoso University, Elmah crea archivos XML en la carpeta *Elmah* con el fin de guardar los detalles sobre los errores. Incluso si no usa algo como Elmah, el sitio podría permitir que los usuarios carguen archivos o realicen otras tareas que escriben datos en una carpeta de su sitio.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Probar el registro y los informes de errores

Para ver cómo la aplicación no funciona correctamente en IIS (aunque lo hizo cuando lo probó en Visual Studio), puede producirse un error que normalmente se registrará en Elmah y, a continuación, abrir el registro de errores Elmah para ver los detalles. Si Elmah no pudo crear un archivo XML y almacenar los detalles del error, verá un informe de errores vacío.

Abra un explorador y vaya a `http://localhost/ContosoUniversity`y, a continuación, solicite una dirección URL no válida como *Studentsxxx. aspx*. Verá una página de error generada por el sistema en lugar de la página *GenericErrorPage. aspx* porque el valor `customErrors` en el archivo Web. config es "RemoteOnly" y está ejecutando IIS localmente:

![Página de error de HTTP 404](setting-folder-permissions/_static/image1.png)

Ahora ejecute *Elmah. axd* para ver el informe de errores. Después de iniciar sesión con las credenciales de la cuenta de administrador (&quot;admin&quot; y &quot;devpwd&quot;), verá una página de registro de errores vacía porque Elmah no pudo crear un archivo XML en la carpeta *Elmah* :

![Registro de errores vacío](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Establecer el permiso de escritura en la carpeta Elmah

Puede establecer permisos de carpeta manualmente o puede convertirlo en una parte automática del proceso de implementación. Si lo hace, se requiere un código MSBuild complejo y, puesto que solo tiene que hacerlo la primera vez que implemente, los pasos siguientes le indicarán cómo hacerlo manualmente. (Para obtener información sobre cómo hacer esta parte del proceso de implementación, consulte [configuración de permisos de carpeta en la publicación web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) en el blog de Sayed Hashimi).

1. En el **Explorador de archivos**, vaya a *C:\inetpub\wwwroot\ContosoUniversity*. Haga clic con el botón derecho en la carpeta *Elmah* , seleccione **propiedades**y, a continuación, seleccione la pestaña **seguridad** .
2. Haga clic en **Editar**.
3. En el cuadro de diálogo **permisos de Elmah** , seleccione **DefaultAppPool**y, a continuación, active la casilla **escribir** en la columna **permitir** .

    ![Permisos para la carpeta ELMAH](setting-folder-permissions/_static/image3.png)

    (Si no ve **DefaultAppPool** en la lista **nombres de grupos o usuarios** , es probable que use algún otro método que el especificado en este tutorial para configurar IIS y ASP.net 4 en el equipo. En ese caso, averigüe qué identidad usa el grupo de aplicaciones asignado a la aplicación contoso University y conceda permiso de escritura a esa identidad. Vea los vínculos acerca de las identidades del grupo de aplicaciones al final de este tutorial). Haga clic en **Aceptar** en ambos cuadros de diálogo.

## <a name="retest-error-logging-and-reporting"></a>Reprobar el registro de errores y los informes

Realice una prueba de forma que se produzca un error de la misma manera (solicite una dirección URL incorrecta) y ejecute la página **registro de errores** . Esta vez el error aparece en la página.

![Página de registro de errores de ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Resumen

Ya ha completado todas las tareas necesarias para que contoso University funcione correctamente en IIS en el equipo local. En el siguiente tutorial, hará que el sitio esté disponible públicamente mediante su implementación en Azure.

## <a name="more-information"></a>Más información

En este ejemplo, el motivo por el que Elmah no pudo guardar los archivos de registro era bastante obvio. Puede usar el seguimiento de IIS en los casos en los que la causa del problema no sea tan obvia; consulte [solución de problemas de solicitudes con error mediante el seguimiento en IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) en el sitio de IIS.net.

Para obtener más información sobre cómo conceder permisos a las identidades del grupo de aplicaciones, vea [identidades del grupo de aplicaciones](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) y [contenido seguro en IIS a través de las ACL del sistema de archivos](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) en el sitio de IIS.net.

> [!div class="step-by-step"]
> [Anterior](deploying-to-iis.md)
> [Siguiente](deploying-to-production.md)
