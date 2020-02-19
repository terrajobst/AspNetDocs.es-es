---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatice todo (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: e741a753a36ebdaefbff8eee0b38911785c716ac
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457172"
---
# <a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatice todo (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener una introducción al libro electrónico, vea [el primer capítulo](introduction.md).

Los tres primeros patrones que veremos en realidad se aplican a cualquier proyecto de desarrollo de software, pero especialmente a los proyectos en la nube. Este patrón trata sobre la automatización de tareas de desarrollo. Es un tema importante porque los procesos manuales son lentos y propensos a errores. automatizar tantas como sea posible ayuda a configurar un flujo de trabajo rápido, confiable y ágil. Es de gran importancia para el desarrollo en la nube, ya que puede automatizar fácilmente muchas tareas que son difíciles o imposibles de automatizar en un entorno local. Por ejemplo, puede configurar entornos de prueba completos, como nuevo servidor Web y máquinas virtuales de back-end, bases de datos, almacenamiento de blobs (almacenamiento de archivos), colas, etc.

## <a name="devops-workflow"></a>Flujo de trabajo DevOps

Cada vez que oye el término "DevOps". El término desarrollado fuera de un reconocimiento que tiene que integrar tareas de desarrollo y operaciones para desarrollar software de forma eficaz. El tipo de flujo de trabajo que desea habilitar es aquél en el que puede desarrollar una aplicación, implementarla, aprender del uso de producción de ti, cambiarla en respuesta a lo que ha aprendido y repetir el ciclo de forma rápida y confiable.

Algunos equipos de desarrollo en la nube correctos se implementan varias veces al día en un entorno activo. El equipo de Azure usó para implementar una actualización principal cada 2-3 meses, pero ahora publica actualizaciones secundarias cada 2-3 días y versiones principales cada 2-3 semanas. Entrar en esa cadencia realmente le ayuda a responder a los comentarios de los clientes.

Para ello, tiene que habilitar un ciclo de desarrollo e implementación que sea repetible, confiable, predecible y que tenga un tiempo de ciclo inferior.

![Flujo de trabajo DevOps](automate-everything/_static/image1.png)

En otras palabras, el período de tiempo entre el momento en que se tiene una idea para una característica y el momento en que los clientes la usan y el envío de comentarios deben ser lo más cortos posible. Los tres primeros patrones: automatizar todo, el control de código fuente, la integración continua y la entrega, son todos los procedimientos recomendados que se recomiendan para habilitar ese tipo de proceso.

## <a name="azure-management-scripts"></a>Scripts de administración de Azure

En la [Introducción a este libro electrónico](introduction.md), vio la consola basada en Web, la portal de administración de Azure. El portal de administración de le permite supervisar y administrar todos los recursos que ha implementado en Azure. Es una forma sencilla de crear y eliminar servicios como aplicaciones web y máquinas virtuales, configurar esos servicios, supervisar el funcionamiento del servicio, etc. Es una herramienta excelente, pero su uso es un proceso manual. Si va a desarrollar una aplicación de producción de cualquier tamaño, especialmente en un entorno de equipo, se recomienda que pase por la interfaz de usuario del portal para aprender y explorar Azure y, a continuación, automatizar los procesos que va a realizar repetidamente.

Casi todo lo que puede hacer manualmente en el portal de administración o desde Visual Studio también se puede hacer mediante una llamada a la API de administración de REST. Puede escribir scripts mediante [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), o puede usar un marco de código abierto como [chef](http://www.opscode.com/chef/) o [posición](http://puppetlabs.com/puppet/what-is-puppet)de trabajo. También puede usar la herramienta de línea de comandos bash en un entorno Mac o Linux. Azure tiene API de scripting para todos esos entornos diferentes y tiene una [API de administración de .net](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) en caso de que desee escribir código en lugar de script.

Para la aplicación Fix it, hemos creado algunos scripts de Windows PowerShell que automatizan los procesos de creación de un entorno de prueba e implementando el proyecto en ese entorno, y revisaremos parte del contenido de esos scripts.

## <a name="environment-creation-script"></a>Script de creación de entorno

El primer script que veremos se denomina *New-AzureWebsiteEnv. PS1*. Crea un entorno de Azure en el que puede implementar la aplicación Fix it para realizar pruebas. Las tareas principales que realiza este script son las siguientes:

- Cree una aplicación web.
- Cree una cuenta de almacenamiento. (Necesario para los blobs y las colas, como verá en los capítulos posteriores).
- Cree un servidor de SQL Database y dos bases de datos: una base de datos de aplicación y una base de datos de pertenencia.
- Almacene la configuración en Azure que la aplicación usará para acceder a la cuenta de almacenamiento y a las bases de datos.
- Cree archivos de configuración que se usarán para automatizar la implementación.

### <a name="run-the-script"></a>Ejecute el script.

> [!NOTE]
> En esta parte del capítulo se muestran ejemplos de scripts y los comandos que se especifican para ejecutarlos. Esta es una demostración y no proporciona todo lo que necesita saber para ejecutar los scripts. Para obtener instrucciones paso a paso sobre cómo hacerlo, consulte [Apéndice: aplicación de ejemplo de corrección](the-fix-it-sample-application.md#deploybase).

Para ejecutar un script de PowerShell que administra los servicios de Azure, tiene que instalar la consola de Azure PowerShell y configurarla para que funcione con su suscripción de Azure. Una vez configurado, puede ejecutar el script de creación corregir entorno de TI con un comando como este:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

El parámetro `Name` especifica el nombre que se va a usar al crear la base de datos y las cuentas de almacenamiento, y el parámetro `SqlDatabasePassword` especifica la contraseña de la cuenta de administrador que se creará para SQL Database. Hay otros parámetros que puede usar para examinarlos más adelante.

![Ventana de PowerShell](automate-everything/_static/image2.png)

Una vez finalizado el script, puede ver en el portal de administración lo que se ha creado. Encontrará dos bases de datos:

![Bases de datos](automate-everything/_static/image3.png)

Una cuenta de almacenamiento:

![Cuenta de almacenamiento](automate-everything/_static/image4.png)

Y una aplicación web:

![Sitio web](automate-everything/_static/image5.png)

En la pestaña **configurar** de la aplicación Web, puede ver que tiene la configuración de la cuenta de almacenamiento y las cadenas de conexión de SQL Database configuradas para la aplicación Fix it.

![appSettings y connectionStrings](automate-everything/_static/image6.png)

La carpeta *Automation* ahora también contiene un archivo *&lt;Websitename&gt;. pubxml* . Este archivo almacena la configuración que MSBuild usará para implementar la aplicación en el entorno de Azure que se acaba de crear. Por ejemplo:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Como puede ver, el script ha creado un entorno de prueba completo y todo el proceso se realiza en unos 90 segundos.

Si otra persona de su equipo quiere crear un entorno de prueba, solo puede ejecutar el script. No solo es rápido, sino que también pueden estar seguros de que usan un entorno idéntico al que está usando. No podía estar tan seguro de que si todos estuvieran configurando elementos manualmente mediante la interfaz de usuario del portal de administración.

### <a name="a-look-at-the-scripts"></a>Un vistazo a los scripts

En realidad, hay tres scripts que realizan este trabajo. Se llama a una desde la línea de comandos y se usan automáticamente las otras dos para realizar algunas de las tareas:

- *New-AzureWebSiteEnv. PS1* es el script principal.

    - *New-AzureStorage. PS1* crea la cuenta de almacenamiento.
    - *New-AzureSql. PS1* crea las bases de datos.

### <a name="parameters-in-the-main-script"></a>Parámetros en el script principal

El script principal, *New-AzureWebSiteEnv. PS1*, define varios parámetros:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Se requieren dos parámetros:

- Nombre de la aplicación web que crea el script. (También se utiliza para la dirección URL: `<name>.azurewebsites.net`).
- La contraseña para el nuevo usuario administrativo del servidor de bases de datos que crea el script.

Los parámetros opcionales le permiten especificar la ubicación del centro de datos (el valor predeterminado es "oeste de EE. UU."), el nombre del administrador del servidor de bases de datos (el valor predeterminado es "dbUser") y una regla de Firewall para el servidor de base de datos.

### <a name="create-the-web-app"></a>Creación de la aplicación web

Lo primero que hace el script es crear la aplicación Web mediante una llamada al cmdlet `New-AzureWebsite`, pasándole los valores de parámetro de ubicación y nombre de la aplicación web:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Creación de la cuenta de almacenamiento

Después, el script principal ejecuta el script *New-AzureStorage. PS1* , especificando " *&lt;Websitename&gt;* Storage" para el nombre de la cuenta de almacenamiento y la misma ubicación del centro de datos que la aplicación Web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*New-AzureStorage. PS1* llama al cmdlet `New-AzureStorageAccount` para crear la cuenta de almacenamiento y devuelve los valores de nombre de cuenta y clave de acceso. La aplicación necesitará estos valores para tener acceso a los blobs y las colas de la cuenta de almacenamiento.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Es posible que no siempre quiera crear una nueva cuenta de almacenamiento. podría mejorar el script agregando un parámetro que, opcionalmente, lo dirija para usar una cuenta de almacenamiento existente.

### <a name="create-the-databases"></a>Crear las bases de datos

Después, el script principal ejecuta el script de creación de la base de datos, *New-AzureSql. PS1*, después de configurar los nombres predeterminados de las reglas de firewall y de base de datos:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

El script de creación de base de datos recupera la dirección IP del equipo de desarrollo y establece una regla de Firewall para que el equipo de desarrollo pueda conectarse al servidor y administrarlo. El script de creación de la base de datos pasa por varios pasos para configurar las bases de datos:

- Crea el servidor mediante el cmdlet `New-AzureSqlDatabaseServer`.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Crea reglas de Firewall para permitir que el equipo de desarrollo administre el servidor y permitir que la aplicación web se conecte a él. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Crea un contexto de base de datos que incluye el nombre del servidor y las credenciales, mediante el cmdlet `New-AzureSqlDatabaseServerContext`.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` es una función del script que llama al cmdlet `ConvertTo-SecureString` para cifrar la contraseña y devuelve un objeto `PSCredential`, el mismo tipo que devuelve el cmdlet `Get-Credential`.
- Crea la base de datos de la aplicación y la base de datos de pertenencia mediante el cmdlet `New-AzureSqlDatabase`.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Llama a una función definida localmente para crear una cadena de conexión para cada base de datos. La aplicación usará estas cadenas de conexión para tener acceso a las bases de datos. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString es una función definida en el script que crea la cadena de conexión a partir de los valores de parámetro que se le proporcionan.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Devuelve una tabla hash con el nombre del servidor de bases de datos y las cadenas de conexión.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

La aplicación Fix it usa bases de datos de pertenencia y de aplicación independientes. También es posible colocar datos de pertenencia y de aplicación en una sola base de datos.

### <a name="store-app-settings-and-connection-strings"></a>Almacenar la configuración de la aplicación y las cadenas de conexión

Azure tiene una característica que permite almacenar la configuración y las cadenas de conexión que invalidan automáticamente lo que se devuelve a la aplicación cuando intenta leer las colecciones `appSettings` o `connectionStrings` en el archivo Web. config. Se trata de una alternativa a la aplicación de [transformaciones Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) al implementar. Para obtener más información, consulte [almacenamiento de datos confidenciales en Azure](source-control.md#appsettings) más adelante en este libro electrónico.

El script de creación de entorno almacena en Azure todos los valores `appSettings` y `connectionStrings` que la aplicación necesita para acceder a la cuenta de almacenamiento y a las bases de datos cuando se ejecuta en Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) es un marco de telemetría que se muestra en el capítulo de [supervisión y telemetría](monitoring-and-telemetry.md) . El script de creación de entorno también reinicia la aplicación web para asegurarse de que recoge la nueva configuración de Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparación para la implementación

Al final del proceso, el script de creación del entorno llama a dos funciones para crear archivos que usará el script de implementación.

Una de estas funciones crea un perfil de publicación *(&lt;websitename&gt;archivo. pubxml* ). El código llama a la API de REST de Azure para obtener la configuración de publicación y guarda la información en un archivo *. publishsettings* . A continuación, usa la información de ese archivo junto con un archivo de plantilla (*pubxml. template*) para crear el archivo *. pubxml* que contiene el perfil de publicación. En este proceso de dos pasos se simula lo que hace en Visual Studio: descargar un archivo *. publishsettings* e importarlo para crear un perfil de publicación.

La otra función usa otro archivo de plantilla (website-Environment. template) para crear un archivo *website-Environment. XML* que contenga la configuración que el script de implementación usará junto con el archivo *. pubxml* .

### <a name="troubleshooting-and-error-handling"></a>Solución de problemas y control de errores

Los scripts son similares a los programas: pueden producir errores y, cuando se desea saber todo lo que se puede hacer sobre el error y lo que lo causó. Por esta razón, el script de creación del entorno cambia el valor de la variable `VerbosePreference` de `SilentlyContinue` a `Continue` para que se muestren todos los mensajes detallados. También cambia el valor de la variable `ErrorActionPreference` de `Continue` a `Stop`, de modo que el script se detenga incluso cuando detecte errores de no terminación:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Antes de realizar cualquier trabajo, el script almacena la hora de inicio para que pueda calcular el tiempo transcurrido cuando se hace:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Una vez completado el trabajo, el script muestra el tiempo transcurrido:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Y para cada operación de clave el script escribe mensajes detallados, por ejemplo:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script de implementación

Qué hace el script *New-AzureWebsiteEnv. PS1* para la creación del entorno, el script *Publish-AzureWebsite. PS1* hace para la implementación de la aplicación.

El script de implementación obtiene el nombre de la aplicación web del archivo *website-Environment. XML* creado por el script de creación del entorno.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Obtiene la contraseña de usuario de implementación del archivo *. publishsettings* :

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Ejecuta el comando de [msbuild](http://msbuildbook.com/) que compila e implementa el proyecto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Y si ha especificado el parámetro `Launch` en la línea de comandos, llama al cmdlet `Show-AzureWebsite` para abrir el explorador predeterminado en la dirección URL del sitio Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Puede ejecutar el script de implementación con un comando como este:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Cuando haya terminado, el explorador se abrirá con el sitio que se ejecuta en la nube en la `<websitename>.azurewebsites.net` dirección URL.

![Corregir la aplicación de ti implementada en Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Resumen

Con estos scripts puede estar seguro de que los mismos pasos siempre se ejecutarán en el mismo orden con las mismas opciones. Esto ayuda a garantizar que todos los desarrolladores del equipo no pierdan nada o dispongan de nada o implementen algo personalizado en su propio equipo que realmente no funcione de la misma manera en el entorno de otro miembro del equipo o en producción.

De forma similar, puede automatizar la mayoría de las funciones de administración de Azure que puede realizar en el portal de administración, mediante la API de REST, scripts de Windows PowerShell, una API de lenguaje .NET o una utilidad bash que se puede ejecutar en Linux o Mac.

En el [siguiente capítulo](source-control.md) veremos el código fuente y explicaremos por qué es importante incluir los scripts en el repositorio de código fuente.

## <a name="resources"></a>Recursos

- [Instale y configure Windows PowerShell para Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Explica cómo instalar los cmdlets de Azure PowerShell y cómo instalar el certificado que necesita en el equipo para administrar su cuenta de Azure. Este es un buen punto de partida, ya que también tiene vínculos a recursos para el aprendizaje de PowerShell.
- [Centro de scripts de Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal de WindowsAzure.com a recursos para el desarrollo de scripts que administran servicios de Azure, con vínculos a tutoriales de introducción, documentación de referencia de cmdlets y código fuente y scripts de ejemplo
- [Scripter de fin de semana: introducción con Azure y PowerShell](https://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). En un blog dedicado a Windows PowerShell, esta publicación proporciona una excelente introducción al uso de PowerShell para las funciones de administración de Azure.
- [Instale y configure la interfaz de la línea de comandos multiplataforma de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Tutorial de introducción para una plataforma de scripting de Azure que funciona en Mac y Linux, así como en sistemas Windows.
- [Sección herramientas de línea de comandos del tema descarga de herramientas y SDK de Azure](https://azure.microsoft.com/downloads/). Página del portal para obtener documentación y descargas relacionadas con las herramientas de línea de comandos para Azure.
- [Automatización de todo con las bibliotecas de administración de Azure y .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman presenta la API de administración de .NET para Azure.
- [Uso de scripts de Windows PowerShell para publicar en entornos de prueba y desarrollo](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentación de MSDN en la que se explica cómo usar los scripts de publicación que Visual Studio genera automáticamente para proyectos Web.
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Extensión de Visual Studio que agrega compatibilidad de idioma para Windows PowerShell en Visual Studio.

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Siguiente](source-control.md)
