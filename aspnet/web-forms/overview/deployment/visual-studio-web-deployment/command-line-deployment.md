---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Implementación web de ASP.NET con Visual Studio: implementación de la línea de comandos | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74634208"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Implementación web de ASP.NET con Visual Studio: implementación desde la línea de comandos

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

En este tutorial se muestra cómo invocar la canalización de publicación Web de Visual Studio desde la línea de comandos. Esto resulta útil en escenarios en los que desea [automatizar el proceso de implementación](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) en lugar de hacerlo manualmente en Visual Studio, normalmente mediante un [sistema de control de versiones de código fuente](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Realizar un cambio en la implementación

Actualmente en la página acerca de se muestra el código de plantilla.

![Página acerca de con código de plantilla](command-line-deployment/_static/image1.png)

Lo reemplazará por código que muestra un resumen de la inscripción de estudiantes.

Abra la página *About. aspx* , elimine todo el marcado dentro del elemento `MainContent` `Content` e inserte el marcado siguiente en su lugar:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Ejecute el proyecto y seleccione la página **About** .

![Página About](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Implementar para probar mediante la línea de comandos

No va a implementar otro cambio en la base de datos, por lo que debe deshabilitar la implementación de la base de datos dbDacFx para la base de datos ASPNET-ContosoUniversity. Abra el Asistente para **publicación web** y, en cada uno de los tres perfiles de publicación, desactive la casilla **Actualizar base de datos** en la pestaña **configuración** .

En la página de inicio de Windows 8, busque **símbolo del sistema para desarrolladores para VS2012**.

Haga clic con el botón derecho en el icono de **símbolo del sistema para desarrolladores de VS2012** y haga clic en **Ejecutar como administrador**.

Escriba el siguiente comando en el símbolo del sistema y reemplace la ruta de acceso al archivo de solución por la ruta de acceso al archivo de solución:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild compila la solución y la implementa en el entorno de prueba.

![Salida de la línea de comandos](command-line-deployment/_static/image3.png)

Abra un explorador y vaya a `http://localhost/ContosoUniversity`y, a continuación, haga clic en la página **About** para comprobar que la implementación se realizó correctamente.

Si no ha creado ningún estudiante en la prueba, verá una página vacía en el encabezado de **estadísticas del cuerpo del alumno** . Vaya a la página **Students** , haga clic en **Agregar Student**y agregue algunos estudiantes y, a continuación, vuelva a la página **About** para ver las estadísticas de los estudiantes.

![Página acerca de en el entorno de prueba](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opciones de la línea de comandos de clave

El comando que ha especificado ha pasado la ruta de acceso del archivo de solución y dos propiedades a MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Implementación de la solución en comparación con la implementación de proyectos individuales

Al especificar el archivo de solución, se generan todos los proyectos de la solución. Si tiene varios proyectos web en la solución, se aplica el siguiente comportamiento de MSBuild:

- Las propiedades que se especifican en la línea de comandos se pasan a todos los proyectos. Por lo tanto, cada proyecto web debe tener un perfil de publicación con el nombre que especifique. Si especifica `/p:PublishProfile=Test`, cada proyecto web debe tener un perfil de publicación denominado *Test*.
- Es posible que publique correctamente un proyecto cuando no se haya compilando otro. Para obtener más información, vea el subproceso de stackoverflow [msbuild genera un error con dos paquetes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Si especifica un proyecto individual en lugar de una solución, tendrá que agregar un parámetro que especifique la versión de Visual Studio. Si usa Visual Studio 2012, la línea de comandos sería similar al ejemplo siguiente:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

El número de versión de Visual Studio 2010 es 10,0. Para obtener más información, vea [compatibilidad de proyectos de Visual Studio y VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) en el blog de Sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Especificar el perfil de publicación

Puede especificar el perfil de publicación por nombre o la ruta de acceso completa al archivo *. pubxml* , como se muestra en el ejemplo siguiente:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Métodos de publicación Web compatibles con la publicación en la línea de comandos

Se admiten tres métodos de publicación para la publicación de la línea de comandos:

- `MSDeploy`-publicar mediante Web Deploy.
- `Package`-publicar creando un paquete de Web Deploy. Tiene que instalar el paquete por separado desde el comando de MSBuild que lo crea.
- `FileSystem`: publicar mediante la copia de archivos en una carpeta especificada.

### <a name="specifying-the-build-configuration-and-platform"></a>Especificar la configuración de compilación y la plataforma

La configuración de compilación y la plataforma deben establecerse en Visual Studio o en la línea de comandos. Los perfiles de publicación incluyen propiedades que se denominan `LastUsedBuildConfiguration` y `LastUsedPlatform`, pero no puede establecer estas propiedades para determinar cómo se compila el proyecto. Para obtener más información, vea [msbuild: How to Set the Configuration Property en el](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) blog de Sayed Hashimi.

## <a name="deploy-to-staging"></a>Implementar en el almacenamiento provisional

Para implementar en Azure, debe agregar la contraseña a la línea de comandos. Si ha guardado la contraseña en el perfil de publicación en Visual Studio, se almacenará en formato cifrado en el archivo. *pubxml. User* . MSBuild no tiene acceso a ese archivo al realizar una implementación de línea de comandos, por lo que tiene que pasar la contraseña en un parámetro de línea de comandos.

1. Copie la contraseña que necesita del archivo *. publishsettings* que descargó anteriormente para el sitio web de ensayo. La contraseña es el valor del atributo `userPWD` del elemento `publishProfile` Web Deploy.

    ![Web Deploy contraseña](command-line-deployment/_static/image5.png)
2. En la página de inicio de Windows 8, busque **símbolo del sistema para desarrolladores de VS2012**y haga clic en el icono para abrir el símbolo del sistema. (No tiene que abrirlo como administrador esta vez porque no está implementando en IIS en el equipo local).
3. Escriba el siguiente comando en el símbolo del sistema y reemplace la ruta de acceso al archivo de solución por la ruta de acceso al archivo de solución y la contraseña por la contraseña:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Tenga en cuenta que esta línea de comandos incluye un parámetro adicional: `/p:AllowUntrustedCertificate=true`. Cuando se escribe este tutorial, se debe establecer la propiedad `AllowUntrustedCertificate` al publicar en Azure desde la línea de comandos. Cuando se lance la corrección para este error, no necesitará ese parámetro.
4. Abra un explorador y vaya a la dirección URL del sitio de ensayo y, a continuación, haga clic en la página **acerca** de para comprobar que la implementación se realizó correctamente.

    Como vimos anteriormente para el entorno de prueba, es posible que tenga que crear algunos estudiantes para ver las estadísticas en la página **acerca** de.

## <a name="deploy-to-production"></a>Implementación en producción

El proceso de implementación en producción es similar al proceso de ensayo.

1. Copie la contraseña que necesita del archivo *. publishsettings* que descargó anteriormente para el sitio web de producción.
2. Abra **símbolo del sistema para desarrolladores para VS2012**.
3. Escriba el siguiente comando en el símbolo del sistema y reemplace la ruta de acceso al archivo de solución por la ruta de acceso al archivo de solución y la contraseña por la contraseña:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Para un sitio de producción real, si también se ha producido un cambio en la base de datos, normalmente se copiará la *aplicación\_archivo. htm sin conexión* en el sitio antes de la implementación y se eliminará después de la implementación correcta.
4. Abra un explorador y vaya a la dirección URL del sitio de ensayo y, a continuación, haga clic en la página **acerca** de para comprobar que la implementación se realizó correctamente.

## <a name="summary"></a>Resumen

Ahora ha implementado una actualización de la aplicación mediante la línea de comandos.

![Página acerca de en el entorno de prueba](command-line-deployment/_static/image6.png)

En el siguiente tutorial, verá un ejemplo de cómo extender la canalización de publicación Web. En el ejemplo se muestra cómo implementar archivos que no están incluidos en el proyecto.

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-update.md)
> [Siguiente](deploying-extra-files.md)
