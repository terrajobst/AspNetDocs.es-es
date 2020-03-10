---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service-ASP.NET 4. x
author: Rick-Anderson
description: En este tutorial se muestra cómo el código puede almacenar y obtener acceso a información segura de forma segura. Lo más importante es que nunca almacene contraseñas u otros...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8356a90611f791779cc4ff4730038d82cd76242f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472147"
---
# <a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Prácticas recomendadas para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> En este tutorial se muestra cómo el código puede almacenar y obtener acceso a información segura de forma segura. Lo más importante es que nunca almacene contraseñas u otros datos confidenciales en el código fuente, y no debe usar secretos de producción en el modo de desarrollo y pruebas.
> 
> El código de ejemplo es una aplicación de consola de WebJob simple y una aplicación ASP.NET MVC que necesita acceso a una cadena de conexión de base de datos password, Twilio, Google y SendGrid Secure Keys.
> 
> La configuración local y PHP también se mencionan.

- [Trabajar con contraseñas en el entorno de desarrollo](#pwd)
- [Trabajar con cadenas de conexión en el entorno de desarrollo](#con)
- [Aplicaciones de consola de webjobs](#wj)
- [Implementación de secretos en Azure](#da)
- [Notas para la instalación local y PHP](#not)
- [Recursos adicionales](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Trabajar con contraseñas en el entorno de desarrollo

Con frecuencia, los tutoriales muestran datos confidenciales en el código fuente, con la salvedad de que nunca debe almacenar datos confidenciales en el código fuente. Por ejemplo, mi [aplicación de ASP.NET MVC 5 con SMS y el tutorial de correo electrónico 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) muestra lo siguiente en el archivo *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

El archivo *Web. config* es código fuente, por lo que estos secretos nunca deben almacenarse en ese archivo. Afortunadamente, el elemento `<appSettings>` tiene un atributo `file` que le permite especificar un archivo externo que contiene valores de configuración de aplicaciones confidenciales. Puede trasladar todos los secretos a un archivo externo, siempre y cuando el archivo externo no se proteja en el árbol de origen. Por ejemplo, en el marcado siguiente, el archivo *AppSettingsSecrets. config* contiene todos los secretos de la aplicación:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

El marcado del archivo externo (*AppSettingsSecrets. config* en este ejemplo) es el mismo marcado que se encuentra en el archivo *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

El tiempo de ejecución de ASP.NET combina el contenido del archivo externo con el marcado en &lt;elemento&gt; appSettings. El tiempo de ejecución omite el atributo de archivo si no se encuentra el archivo especificado.

> [!WARNING]
> Seguridad: no agregue el archivo *Secrets. config* al proyecto o lo proteja en el control de código fuente. De forma predeterminada, Visual Studio establece el `Build Action` en `Content`, lo que significa que se implementa el archivo. Para obtener más información, consulte [¿por qué no se implementan todos los archivos de la carpeta del proyecto?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Aunque puede usar cualquier extensión para el archivo *Secrets. config* , es mejor mantenerlo *. config*, ya que los archivos de configuración no son atendidos por IIS. Observe también que el archivo *AppSettingsSecrets. config* tiene dos niveles de directorio en el archivo *Web. config* , por lo que está completamente fuera del directorio de la solución. Desplazando el archivo fuera del directorio de la solución, &quot;git Add \*&quot; no lo agregará al repositorio.

<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Trabajar con cadenas de conexión en el entorno de desarrollo

Visual Studio crea nuevos proyectos de ASP.NET que usan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB se creó específicamente para el entorno de desarrollo. No requiere una contraseña, por lo que no es necesario hacer nada para evitar que se protejan los secretos en el código fuente. Algunos equipos de desarrollo usan las versiones completas de SQL Server (u otro DBMS) que requieran una contraseña.

Puede usar el atributo `configSource` para reemplazar el marcado de `<connectionStrings>` completo. A diferencia de la `<appSettings>` `file` atributo que combina el marcado, el atributo `configSource` reemplaza el marcado. En el marcado siguiente se muestra el `configSource` atributo en el archivo *Web. config* :

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Si usa el atributo `configSource` como se mostró anteriormente para trasladar las cadenas de conexión a un archivo externo, y hace que Visual Studio cree un nuevo sitio web, no podrá detectar que está usando una base de datos y no tendrá la opción de configurar la base de datos al publicar en Azure desde Visual Studio. Si usa el atributo `configSource`, puede usar PowerShell para crear e implementar el sitio web y la base de datos, o puede crear el sitio web y la base de datos en el portal antes de publicar. El script [New-AzureWebsitewithDB. PS1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) creará un nuevo sitio web y una base de datos.

> [!WARNING]
> Seguridad: a diferencia del archivo *AppSettingsSecrets. config* , el archivo de cadenas de conexión externas debe estar en el mismo directorio que el archivo *Web. config* raíz, por lo que tendrá que tomar precauciones para asegurarse de no protegerlo en el repositorio de origen.

> [!NOTE]
> **Advertencia de seguridad en el archivo de secretos:** Un procedimiento recomendado es no usar secretos de producción en pruebas y desarrollo. El uso de contraseñas de producción en pruebas o desarrollo pierde esos secretos.

<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplicaciones de consola de webjobs

El archivo *app. config* usado por una aplicación de consola no es compatible con rutas de acceso relativas, pero admite rutas de acceso absolutas. Puede usar una ruta de acceso absoluta para sacar los secretos del directorio del proyecto. El marcado siguiente muestra los secretos del archivo *C:\secrets\AppSettingsSecrets.config* y los datos no confidenciales en el archivo *app. config* .

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Implementación de secretos en Azure

Al implementar la aplicación web en Azure, no se implementará el archivo *AppSettingsSecrets. config* (eso es lo que desea). Puede ir al Portal de administración de [Azure](https://azure.microsoft.com/services/management-portal/) y establecerlos manualmente para ello:

1. Vaya a [https://portal.azure.com](https://portal.azure.com)e inicie sesión con sus credenciales de Azure.
2. Haga clic en **examinar &gt; Web Apps**y, a continuación, haga clic en el nombre de la aplicación Web.
3. Haga clic en **toda la configuración &gt; configuración**de la aplicación.

Los valores de configuración de la **aplicación** y de la **cadena de conexión** invalidan la misma configuración en el archivo *Web. config* . En nuestro ejemplo, no implementamos esta configuración en Azure, pero si estas claves estuviesen en el archivo *Web. config* , la configuración que se muestra en el portal tendría prioridad.

Un procedimiento recomendado es seguir un [flujo de trabajo de DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) y usar [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (u otro marco como [chef](http://www.opscode.com/chef/) o [posición libre](http://puppetlabs.com/puppet/what-is-puppet)) para automatizar la configuración de estos valores en Azure. El siguiente script de PowerShell usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) para exportar los secretos cifrados en el disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

En el script anterior, ' nombre ' es el nombre de la clave secreta, como '&quot;FB\_AppSecret&quot; o ' TwitterSecret '. Puede ver el archivo ". Credential" creado por el script en el explorador. El siguiente fragmento de código comprueba cada uno de los archivos de credenciales y establece los secretos de la aplicación web con nombre:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Seguridad: no incluya contraseñas u otros secretos en el script de PowerShell, lo que impide el uso de un script de PowerShell para implementar datos confidenciales. El cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) proporciona un mecanismo seguro para obtener una contraseña. El uso de un indicador de la interfaz de usuario puede impedir la pérdida de una contraseña.

### <a name="deploying-db-connection-strings"></a>Implementación de cadenas de conexión de base de BD

Las cadenas de conexión de base de BD se tratan de forma similar a la configuración de la aplicación Si implementa la aplicación web desde Visual Studio, se configurará automáticamente la cadena de conexión. Puede comprobarlo en el portal. La forma recomendada para establecer la cadena de conexión es con PowerShell. Para obtener un ejemplo de un script de PowerShell, crea un sitio web y una base de datos y establece la cadena de conexión en el sitio web, descargue [New-AzureWebsitewithDB. PS1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) de la [biblioteca de scripts de Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Notas para PHP

Dado que los pares clave-valor para la configuración de la **aplicación** y las **cadenas de conexión** se almacenan en variables de entorno en Azure App Service, los desarrolladores que usan cualquier marco de aplicación web (como php) pueden recuperar fácilmente estos valores. Vea los [sitios web de Microsoft Azure en Stefan Schackow: Cómo funcionan las cadenas de aplicación y las cadenas de conexión](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) en la entrada del blog, que muestra un fragmento de código php para leer la configuración de la aplicación y las cadenas de conexión.

## <a name="notes-for-on-premises-servers"></a>Notas para servidores locales

Si va a implementar en servidores web locales, puede ayudar a proteger los secretos mediante [el cifrado de las secciones de configuración de los archivos de configuración](https://msdn.microsoft.com/library/ff647398.aspx). Como alternativa, puede usar el mismo enfoque recomendado para Azure websites: mantener la configuración de desarrollo en los archivos de configuración y usar valores de variables de entorno para la configuración de producción. Sin embargo, en este caso, tiene que escribir código de aplicación para una funcionalidad que sea automática en Azure websites: recuperar la configuración de las variables de entorno y usar los valores en lugar de los valores del archivo de configuración, o bien usar la configuración del archivo de configuración cuando no se encuentran las variables de entorno.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

Para obtener un ejemplo de un script de PowerShell que crea una aplicación web + Database, establece la cadena de conexión y la configuración de la aplicación, descargue [New-AzureWebsitewithDB. PS1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) de la [biblioteca de scripts de Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Vea los [sitios web de Microsoft Azure de Stefan Schackow: Cómo funcionan las cadenas de aplicación y las cadenas de conexión](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)

Especial gracias a Barry Dorrans ( [@blowdart](https://twitter.com/blowdart) ) y Carlos Farre para revisarlo.
