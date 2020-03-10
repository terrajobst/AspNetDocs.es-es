---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: transformaciones del archivo Web. config-3 de 12 | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web de ASP.NET que incluye una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 9e7902bcf8a16c154aee1a982824bfaedeea7d9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515509"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Implementación de una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: transformaciones del archivo Web. config-3 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> En esta serie de tutoriales se muestra cómo implementar (publicar) un proyecto de aplicación Web ASP.NET que incluye una base de datos SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación Web. Para obtener una introducción a la serie, vea [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación que se introdujeron después de la versión RC de Visual Studio 2012, muestra cómo implementar SQL Server ediciones distintas de SQL Server Compact y muestra cómo realizar la implementación en Azure App Service Web Apps, vea [implementación web de ASP.net con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Información general

En este tutorial se muestra cómo automatizar el proceso de cambiar el archivo *Web. config* cuando se implementa en entornos de destino diferentes. La mayoría de las aplicaciones tienen opciones de configuración en el archivo *Web. config* que deben ser diferentes cuando se implementa la aplicación. Automatizar el proceso de realizar estos cambios evita tener que hacerlo manualmente cada vez que implemente, lo que sería tedioso y propenso a errores.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformaciones de Web. config frente a parámetros de Web Deploy

Hay dos maneras de automatizar el proceso de cambiar la configuración del archivo *Web. config* : las [transformaciones Web. config](https://msdn.microsoft.com/library/dd465326.aspx) y [los parámetros de web deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un archivo de transformación de *Web. config* contiene el marcado XML que especifica cómo cambiar el archivo *Web. config* cuando se implementa. Puede especificar diferentes cambios para configuraciones de compilación específicas y para perfiles de publicación específicos. Las configuraciones de compilación predeterminadas son Debug y release, y puede crear configuraciones de compilación personalizadas. Un perfil de publicación corresponde normalmente a un entorno de destino. (Encontrará más información sobre los perfiles de publicación en el tutorial [implementación en IIS como entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) ).

Web Deploy parámetros se pueden usar para especificar muchos tipos diferentes de valores que se deben configurar durante la implementación, incluida la configuración que se encuentra en los archivos *Web. config* . Cuando se usa para especificar los cambios en el archivo *Web. config* , Web deploy parámetros son más complejos de configurar, pero resultan útiles cuando no se conoce el valor que se va a establecer hasta que se implemente. Por ejemplo, en un entorno empresarial, podría crear un *paquete de implementación* y asignarlo a una persona del Departamento de TI para que lo instale en producción, y esa persona tenga que poder escribir cadenas de conexión o contraseñas que no conozca.

En el escenario que se trata en este tutorial, sabrá todo lo que se debe hacer en el archivo *Web. config* , por lo que no necesita usar parámetros de web deploy. Configurará algunas transformaciones que difieren en función de la configuración de compilación usada y otras diferencias según el perfil de publicación que se use.

## <a name="creating-transformation-files-for-publish-profiles"></a>Crear archivos de transformación para perfiles de publicación

En **Explorador de soluciones**, expanda *Web. config* para ver los archivos de transformación *Web. Debug. config* y *Web. Release. config* que se crean de forma predeterminada para las dos configuraciones de compilación predeterminadas.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Puede crear archivos de transformación para configuraciones de compilación personalizadas haciendo clic con el botón secundario en el archivo Web. config y eligiendo **Agregar transformaciones de configuración** en el menú contextual, pero para este tutorial no es necesario hacerlo.

Necesita dos archivos de transformación más, para configurar los cambios relacionados con el destino de implementación en lugar de la configuración de compilación. Un ejemplo típico de este tipo de configuración es un punto de conexión de WCF que es diferente para la prueba y la producción. En los tutoriales posteriores, creará perfiles de publicación denominados prueba y producción, por lo que necesita un archivo *Web. test. config* y un archivo *Web. Production. config* .

Los archivos de transformación que están vinculados a perfiles de publicación se deben crear manualmente. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto ContosoUniversity y seleccione **Abrir carpeta en el explorador de Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

En el **Explorador de Windows**, seleccione el archivo *Web. Release. config* , copie el archivo y, a continuación, pegue dos copias de él. Cambie el nombre de estas copias *Web. Production. config* y *Web. test. config*y cierre el **Explorador de Windows**.

En **Explorador de soluciones**, haga clic en **Actualizar** para ver los nuevos archivos.

Seleccione los nuevos archivos, haga clic con el botón secundario y, a continuación, haga clic en **incluir en el proyecto** en el menú contextual.

![Incluir archivos de configuración de producción y de prueba en el proyecto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Para evitar que estos archivos se implementen, selecciónelos en **Explorador de soluciones**y, a continuación, en la ventana **propiedades** , cambie la propiedad **acción de compilación** de **contenido** a **ninguno**. (Los archivos de transformación basados en configuraciones de compilación no se pueden implementar automáticamente).

Ahora está listo para especificar transformaciones *Web. config* en los archivos de transformación *Web. config* .

## <a name="limiting-error-log-access-to-administrators"></a>Limitar el acceso al registro de errores a los administradores

Si se produce un error mientras se ejecuta la aplicación, la aplicación muestra una página de error genérica en lugar de la página de errores generada por el sistema y usa el [paquete de NuGet Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para el registro de errores y los informes. El elemento `customErrors` del archivo *Web. config* especifica la página de error:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Para ver la página de error, cambie temporalmente el atributo `mode` del elemento `customErrors` de "RemoteOnly" a "ON" y ejecute la aplicación desde Visual Studio. Producir un error solicitando una dirección URL no válida, como *Studentsxxx. aspx*. En lugar de una página de error "página no encontrada" generada por IIS, verá la página *GenericErrorPage. aspx* .

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Para ver el registro de errores, reemplace todo lo que aparece en la dirección URL después del número de puerto con *elmah. axd* (en el ejemplo de la captura de pantalla, `http://localhost:51130/elmah.axd`) y presione ENTRAR:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Cuando haya terminado, no olvide volver a establecer el `customErrors` elemento en el modo "RemoteOnly".

En el equipo de desarrollo, es conveniente permitir el acceso gratuito a la página de registro de errores, pero en producción sería un riesgo para la seguridad. En el caso del sitio de producción, puede Agregar una regla de autorización que restringe el acceso al registro de errores solo a los administradores mediante la configuración de una transformación en el archivo *Web. Production. config* .

Abra *Web. Production. config* y agregue un nuevo elemento de `location` inmediatamente después de la etiqueta de apertura `configuration`, como se muestra aquí. (Asegúrese de que agrega solo el elemento `location` y no el marcado circundante que se muestra aquí solo para proporcionar algún contexto).

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

El valor del atributo `Transform` de "Insert" hace que este elemento `location` se agregue como elemento relacionado a cualquier elemento de `location` existente en el archivo *Web. config* . (Ya existe un elemento `location` que especifica las reglas de autorización para la página **Actualizar créditos** ). Al probar el sitio de producción después de la implementación, probará para comprobar que esta regla de autorización es efectiva.

No es necesario restringir el acceso al registro de errores en el entorno de prueba, por lo que no tiene que agregar este código al archivo *Web. test. config* .

> [!NOTE] 
> 
> **Nota de seguridad** Nunca muestre los detalles del error al público en una aplicación de producción, o bien almacene esa información en una ubicación pública. Los atacantes pueden usar la información de error para detectar vulnerabilidades en un sitio. Si usa ELMAH en su propia aplicación, asegúrese de investigar las maneras en las que se puede configurar ELMAH para minimizar los riesgos de seguridad. El ejemplo de ELMAH de este tutorial no debe considerarse una configuración recomendada. Es un ejemplo que se eligió para ilustrar cómo controlar una carpeta en la que la aplicación debe poder crear archivos.

## <a name="setting-an-environment-indicator"></a>Establecimiento de un indicador de entorno

Un escenario común es tener los valores del archivo *Web. config* que deben ser diferentes en cada entorno en el que se implemente. Por ejemplo, una aplicación que llama a un servicio WCF puede necesitar un punto de conexión diferente en entornos de prueba y de producción. La aplicación contoso University también incluye una configuración de este tipo. Esta configuración controla un indicador visible en las páginas de un sitio que indica el entorno en el que se encuentra, como desarrollo, prueba o producción. El valor de configuración determina si la aplicación anexará "(dev)" o "(test)" al encabezado principal de la página maestra *site. Master* :

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

El indicador de entorno se omite cuando la aplicación se ejecuta en producción.

Las páginas web de Contoso University leen un valor que se establece en `appSettings` en el archivo *Web. config* para determinar el entorno en el que se ejecuta la aplicación:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

El valor debe ser "Test" en el entorno de prueba y "Prod" en el entorno de producción.

Abra *Web. Production. config* y agregue un elemento `appSettings` inmediatamente antes de la etiqueta de apertura del elemento `location` que agregó anteriormente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

El valor del atributo `xdt:Transform` "SetAttributes" indica que el propósito de esta transformación es cambiar los valores de atributo de un elemento existente en el archivo *Web. config* . El valor del atributo `xdt:Locator` "match (Key)" indica que el elemento que se va a modificar es el que `key` atributo coincide con el atributo `key` especificado aquí. El único otro atributo del elemento `add` es `value`y eso es lo que se cambiará en el archivo *Web. config* implementado. Este código hace que el atributo `value` del elemento `Environment` `appSettings` se establezca en "Prod" en el archivo *Web. config* que se implementa en producción.

A continuación, aplique el mismo cambio al archivo *Web. test. config* , excepto establezca el `value` en "Test" en lugar de "Prod". Cuando haya terminado, la sección `appSettings` en *Web. test. config* será similar al ejemplo siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Deshabilitar el modo de depuración

En el caso de una compilación de versión, no desea que la depuración esté habilitada, independientemente del entorno en el que vaya a realizar la implementación. De forma predeterminada, el archivo de transformación *Web. Release. config* se crea automáticamente con el código que quita el atributo `debug` del elemento `compilation`:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

El atributo `Transform` hace que el atributo `debug` se omita en el archivo *Web. config* implementado siempre que se implemente una compilación de versión.

Esta misma transformación está en los archivos de transformación de producción y de prueba porque se crearon copiando el archivo de transformación de la versión. No es necesario que se duplique, por lo que debe abrir cada uno de esos archivos, quitar el elemento de **compilación** y guardar y cerrar cada archivo.

## <a name="setting-connection-strings"></a>Establecer cadenas de conexión

En la mayoría de los casos no es necesario configurar transformaciones de cadena de conexión, ya que se pueden especificar cadenas de conexión en el perfil de publicación. Pero hay una excepción cuando se implementa una base de datos de SQL Server Compact y se usa Migraciones de Entity Framework Code First para actualizar la base de datos en el servidor de destino. En este escenario, tiene que especificar una cadena de conexión adicional que se utilizará en el servidor para actualizar el esquema de la base de datos. Para configurar esta transformación, agregue un elemento **&lt;connectionstrings&gt;** inmediatamente después de la etiqueta Open **&lt;Configuration&gt;** en los archivos de transformación *Web. test. config* y *Web. Production. config* :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

El atributo `Transform` especifica que esta cadena de conexión se agregará al elemento *connectionStrings* en el archivo *Web. config* implementado. (El proceso de publicación crea esta cadena de conexión adicional automáticamente si no existe, pero de forma predeterminada el atributo **providerName** se establece en `System.Data.SqlClient`, que no funciona para SQL Server Compact. Al agregar manualmente la cadena de conexión, se evita que el proceso de implementación cree un elemento de cadena de conexión con un nombre de proveedor incorrecto).

Ahora ha especificado todas las transformaciones de *Web. config* que necesita para implementar la aplicación contoso University en test y Production. En el siguiente tutorial, se encargará de las tareas de configuración de implementación que requieren el establecimiento de las propiedades del proyecto.

## <a name="more-information"></a>Más información

Para obtener más información sobre los temas que se tratan en este tutorial, vea el escenario de transformación de Web. config en [mapa de contenido de implementación de ASP.net](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
