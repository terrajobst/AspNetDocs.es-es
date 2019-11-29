---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Implementación web de ASP.NET con Visual Studio: transformaciones del archivo Web. config | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621790"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Implementación web de ASP.NET con Visual Studio: transformaciones del archivo Web. config

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

En este tutorial se muestra cómo automatizar el proceso de cambiar el archivo *Web. config* cuando se implementa en entornos de destino diferentes. La mayoría de las aplicaciones tienen opciones de configuración en el archivo *Web. config* que deben ser diferentes cuando se implementa la aplicación. Automatizar el proceso de realizar estos cambios evita tener que hacerlo manualmente cada vez que implemente, lo que sería tedioso y propenso a errores.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformaciones de Web. config frente a parámetros de Web Deploy

Hay dos maneras de automatizar el proceso de cambiar la configuración del archivo *Web. config* : las [transformaciones Web. config](https://msdn.microsoft.com/library/dd465326.aspx) y [los parámetros de web deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un archivo de transformación de *Web. config* contiene el marcado XML que especifica cómo cambiar el archivo *Web. config* cuando se implementa. Puede especificar diferentes cambios para configuraciones de compilación específicas y para perfiles de publicación específicos. Las configuraciones de compilación predeterminadas son Debug y release, y puede crear configuraciones de compilación personalizadas. Un perfil de publicación corresponde normalmente a un entorno de destino. (Encontrará más información sobre los perfiles de publicación en el tutorial [implementación en IIS como entorno de prueba](deploying-to-iis.md) ).

Web Deploy parámetros se pueden usar para especificar muchos tipos diferentes de valores que se deben configurar durante la implementación, incluida la configuración que se encuentra en los archivos *Web. config* . Cuando se usa para especificar los cambios en el archivo *Web. config* , Web deploy parámetros son más complejos de configurar, pero resultan útiles cuando no se conoce el valor que se va a establecer hasta que se implemente. Por ejemplo, en un entorno empresarial, podría crear un *paquete de implementación* y asignarlo a una persona del Departamento de TI para que lo instale en producción, y esa persona tenga que poder escribir cadenas de conexión o contraseñas que no conozca.

En el escenario que cubre esta serie de tutoriales, sabe de antemano todo lo que hay que hacer en el archivo *Web. config* , por lo que no necesita usar parámetros de web deploy. Configurará algunas transformaciones que difieren en función de la configuración de compilación usada y otras diferencias según el perfil de publicación que se use.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especificar la configuración de Web. config en Azure

Si la configuración del archivo *Web. config* que desea cambiar está en el `<connectionStrings>` o el elemento `<appSettings>`, y si está implementando en Web Apps en Azure App Service, tiene otra opción para automatizar los cambios durante la implementación. Puede especificar la configuración que desea que se aplique en Azure en la pestaña **configurar** de la página del portal de administración de la aplicación web (Desplácese hacia abajo hasta la sección configuración de la **aplicación** y **cadenas de conexión** ). Al implementar el proyecto, Azure aplica automáticamente los cambios. Para obtener más información, vea [sitios web de Windows Azure: Cómo funcionan las cadenas de aplicación y las cadenas de conexión](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Archivos de transformación predeterminados

En **Explorador de soluciones**, expanda *Web. config* para ver los archivos de transformación *Web. Debug. config* y *Web. Release. config* que se crean de forma predeterminada para las dos configuraciones de compilación predeterminadas.

![Web. config_transform_files](web-config-transformations/_static/image1.png)

Puede crear archivos de transformación para configuraciones de compilación personalizadas haciendo clic con el botón secundario en el archivo Web. config y eligiendo **Agregar transformaciones de configuración** en el menú contextual. En este tutorial no es necesario hacerlo y la opción de menú está deshabilitada, porque no ha creado ninguna configuración de compilación personalizada.

Más adelante creará tres archivos de transformación más, uno para los perfiles de publicación de prueba, de ensayo y de producción. Un ejemplo típico de una configuración que se administraría en un archivo de transformación de Perfil de publicación porque depende del entorno de destino es un punto de conexión de WCF que es diferente para la prueba y la producción. Creará archivos de transformación de Perfil de publicación en los tutoriales posteriores después de crear los perfiles de publicación con los que se dirigen.

## <a name="disable-debug-mode"></a>Deshabilitar el modo de depuración

Un ejemplo de un valor de configuración que depende de la configuración de compilación en lugar del entorno de destino es el `debug` atributo. En el caso de una compilación de versión, normalmente se desea deshabilitar la depuración, independientemente del entorno en el que se vaya a implementar. Por lo tanto, de forma predeterminada, las plantillas de proyecto de Visual Studio crean archivos de transformación *Web. Release. config* con código que quita el atributo `debug` del elemento `compilation`. Este es el *archivo Web. Release. config*predeterminado: además de algunos códigos de transformación de ejemplo que se marcan como comentarios, incluye el código del elemento `compilation` que quita el atributo `debug`:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

El atributo `xdt:Transform="RemoveAttributes(debug)"` especifica que desea quitar el atributo `debug` del elemento `system.web/compilation` en el archivo *Web. config* implementado. Esto se realizará cada vez que implemente una versión de lanzamiento.

## <a name="limit-error-log-access-to-administrators"></a>Limitar el acceso al registro de errores a los administradores

Si se produce un error mientras se ejecuta la aplicación, la aplicación muestra una página de error genérica en lugar de la página de errores generada por el sistema y usa el [paquete de NuGet Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para el registro de errores y los informes. El elemento `customErrors` del archivo *Web. config* de la aplicación especifica la página de error:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver la página de error, cambie temporalmente el atributo `mode` del elemento `customErrors` de "RemoteOnly" a "ON" y ejecute la aplicación desde Visual Studio. Producir un error solicitando una dirección URL no válida, como *Studentsxxx. aspx*. En lugar de la página de error "no se encuentra el recurso que genera IIS", verá la página *GenericErrorPage. aspx* .

![Página de error](web-config-transformations/_static/image2.png)

Para ver el registro de errores, reemplace todo lo que haya en la dirección URL después del número de puerto con *elmah. axd* (por ejemplo, `http://localhost:51130/elmah.axd`) y presione ENTRAR:

![Página ELMAH](web-config-transformations/_static/image3.png)

Cuando haya terminado, no olvide volver a establecer el `customErrors` elemento en el modo "RemoteOnly".

En el equipo de desarrollo, es conveniente permitir el acceso gratuito a la página de registro de errores, pero en producción sería un riesgo para la seguridad. En el caso del sitio de producción, desea agregar una regla de autorización que restringe el acceso al registro de errores a los administradores y asegurarse de que la restricción funciona también en pruebas y ensayo. Por lo tanto, se trata de otro cambio que se desea implementar cada vez que se implementa una compilación de versión, por lo que pertenece al archivo *Web. Release. config* .

Abra *Web. Release. config* y agregue un nuevo elemento `location` inmediatamente antes de la etiqueta de cierre `configuration`, como se muestra aquí.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

El valor del atributo `Transform` de "Insert" hace que este elemento `location` se agregue como elemento relacionado a cualquier elemento de `location` existente en el archivo *Web. config* . (Ya existe un elemento `location` que especifica las reglas de autorización para la página **Actualizar créditos** ).

Ahora puede obtener una vista previa de la transformación para asegurarse de que la codifique correctamente.

En **Explorador de soluciones**, haga clic con el botón derecho en *Web. Release. config* y haga clic en **vista previa de transformación**.

![Menú de transformación vista previa](web-config-transformations/_static/image4.png)

Se abre una página que muestra el archivo *Web. config* de desarrollo a la izquierda y el aspecto que tendrá el archivo *Web. config* implementado a la derecha, con los cambios resaltados.

![Vista previa de la transformación de depuración](web-config-transformations/_static/image5.png)

![Vista previa de la transformación de ubicación](web-config-transformations/_static/image6.png)

(En la versión preliminar, es posible que observe algunos cambios adicionales para los que no escribió transformaciones: estos suelen implicar la eliminación de los espacios en blanco que no afectan a la funcionalidad).

Al probar el sitio después de la implementación, también probará para comprobar que la regla de autorización es efectiva.

> [!NOTE] 
> 
> **Nota de seguridad** Nunca muestre los detalles del error al público en una aplicación de producción, o bien almacene esa información en una ubicación pública. Los atacantes pueden usar la información de error para detectar vulnerabilidades en un sitio. Si usa ELMAH en su propia aplicación, configure ELMAH para minimizar los riesgos de seguridad. El ejemplo de ELMAH de este tutorial no debe considerarse una configuración recomendada. Es un ejemplo que se eligió para ilustrar cómo controlar una carpeta en la que la aplicación debe poder crear archivos. Para obtener más información, vea [proteger el punto de conexión de ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un valor de configuración que se controlará en los archivos de transformación de Perfil de publicación.

Un escenario común es tener los valores del archivo *Web. config* que deben ser diferentes en cada entorno en el que se implemente. Por ejemplo, una aplicación que llama a un servicio WCF puede necesitar un punto de conexión diferente en entornos de prueba y de producción. La aplicación contoso University también incluye una configuración de este tipo. Esta configuración controla un indicador visible en las páginas de un sitio que indica el entorno en el que se encuentra, como desarrollo, prueba o producción. El valor de configuración determina si la aplicación anexará "(dev)" o "(test)" al encabezado principal de la página maestra *site. Master* :

![Indicador de entorno](web-config-transformations/_static/image7.png)

El indicador de entorno se omite cuando la aplicación se ejecuta en ensayo o en producción.

Las páginas web de Contoso University leen un valor que se establece en `appSettings` en el archivo *Web. config* para determinar el entorno en el que se ejecuta la aplicación:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

El valor debe ser "Test" en el entorno de prueba y "Prod" para almacenamiento provisional y producción.

El siguiente código de un archivo de transformación implementará esta transformación:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

El valor del atributo `xdt:Transform` "SetAttributes" indica que el propósito de esta transformación es cambiar los valores de atributo de un elemento existente en el archivo *Web. config* . El valor del atributo `xdt:Locator` "match (Key)" indica que el elemento que se va a modificar es el que `key` atributo coincide con el atributo `key` especificado aquí. El único otro atributo del elemento `add` es `value`y eso es lo que se cambiará en el archivo *Web. config* implementado. El código que se muestra aquí hace que el atributo `value` del elemento `Environment` `appSettings` se establezca en "Test" en el archivo *Web. config* que se implementa.

Esta transformación pertenece a los archivos de transformación de Perfil de publicación, que todavía no ha creado. Creará y actualizará los archivos de transformación que implementan este cambio al crear los perfiles de publicación para los entornos de prueba, ensayo y producción. Lo hará en los tutoriales [implementar en IIS](deploying-to-iis.md) e [implementar en producción](deploying-to-production.md) .

> [!NOTE]
> Dado que esta configuración se encuentra en el elemento `<appSettings>`, tiene otra alternativa para especificar la transformación al implementar en Web Apps en Azure App Service vea [especificar la configuración de Web. config en Azure](#watransforms) anteriormente en este tema.

## <a name="setting-connection-strings"></a>Establecer cadenas de conexión

Aunque el archivo de transformación predeterminado contiene un ejemplo que muestra cómo actualizar una cadena de conexión, en la mayoría de los casos no es necesario configurar transformaciones de cadena de conexión, ya que puede especificar cadenas de conexión en el perfil de publicación. Lo hará en los tutoriales [implementar en IIS](deploying-to-iis.md) e [implementar en producción](deploying-to-production.md) .

## <a name="summary"></a>Resumen

Ya ha hecho todo lo posible con las transformaciones de *Web. config* antes de crear los perfiles de publicación, y ha visto una vista previa de lo que se incluirá en el archivo Web. config implementado.

![Vista previa de la transformación de ubicación](web-config-transformations/_static/image8.png)

En el siguiente tutorial, se encargará de las tareas de configuración de implementación que requieren el establecimiento de las propiedades del proyecto.

## <a name="more-information"></a>Más información

Para obtener más información acerca de los temas que se tratan en este tutorial, consulte [usar transformaciones de Web. config para cambiar la configuración en el archivo Web. config de destino o el archivo app. config durante la implementación](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) en el mapa de contenido de implementación web para Visual Studio y ASP.net.

> [!div class="step-by-step"]
> [Anterior](preparing-databases.md)
> [Siguiente](project-properties.md)
