---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ejecutar scripts de Windows PowerShell desde archivos de proyecto de MSBuild | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación. Puede ejecutar un script de forma local (es decir, en la
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 7b09c07b8b7c2a61ca534f7a66a929593f3d04ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441451"
---
# <a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Ejecutar scripts de Windows PowerShell desde archivos de proyecto de MSBuild

por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación. Puede ejecutar un script de forma local (en otras palabras, en el servidor de compilación) o de forma remota, como en un servidor Web de destino o un servidor de base de datos.
> 
> Hay muchas razones por las que podría querer ejecutar un script de Windows PowerShell posterior a la implementación. Por ejemplo, puedes:
> 
> - Agregue un origen de eventos personalizado al registro.
> - Genere un directorio del sistema de archivos para las cargas.
> - Limpie los directorios de compilación.
> - Escribir entradas en un archivo de registro personalizado.
> - Enviar correos electrónicos que invitan a los usuarios a una aplicación web recién aprovisionada.
> - Cree cuentas de usuario con los permisos adecuados.
> - Configure la replicación entre instancias de SQL Server.
> 
> En este tema se muestra cómo ejecutar scripts de Windows PowerShell tanto de forma local como remota desde un destino personalizado en un archivo de proyecto de Microsoft Build Engine (MSBuild).

Este tema forma parte de una serie de tutoriales basados en los requisitos de implementación empresarial de una empresa ficticia denominada Fabrikam, Inc. En esta serie de tutoriales se&#x2014;usa una solución de ejemplo de la&#x2014; [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar una aplicación web con un nivel de complejidad realista, como una aplicación ASP.NET MVC 3, un servicio Windows Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación que se encuentra en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto dividido que se describe en [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en el&#x2014;que el proceso de compilación se controla mediante dos archivos de proyecto que contiene instrucciones de compilación que se aplican a cada entorno de destino y uno que contiene la configuración de compilación e implementación específica del entorno. En tiempo de compilación, el archivo de proyecto específico del entorno se combina en el archivo de proyecto independiente del entorno para formar un conjunto completo de instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Para ejecutar un script de Windows PowerShell como parte de un proceso de implementación automatizado o de un solo paso, debe completar estas tareas de alto nivel:

- Agregue el script de Windows PowerShell a la solución y al control de código fuente.
- Cree un comando que invoque el script de Windows PowerShell.
- Escapa cualquier carácter XML reservado en el comando.
- Cree un destino en el archivo de proyecto de MSBuild personalizado y use la tarea **exec** para ejecutar el comando.

En este tema se muestra cómo llevar a cabo estos procedimientos. En las tareas y los tutoriales de este tema se supone que ya está familiarizado con las propiedades y los destinos de MSBuild, y que comprende cómo usar un archivo de proyecto de MSBuild personalizado para impulsar un proceso de compilación e implementación. Para obtener más información, vea [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Crear y agregar scripts de Windows PowerShell

Las tareas de este tema usan un script de Windows PowerShell de ejemplo denominado **LogDeploy. PS1** para ilustrar cómo ejecutar scripts desde MSBuild. El script **LogDeploy. PS1** contiene una función simple que escribe una entrada de una sola línea en un archivo de registro:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]

El script **LogDeploy. PS1** acepta dos parámetros. El primer parámetro representa la ruta de acceso completa al archivo de registro al que desea agregar una entrada y el segundo parámetro representa el destino de implementación que desea grabar en el archivo de registro. Al ejecutar el script, se agrega una línea al archivo de registro con este formato:

[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]

Para que el script **LogDeploy. PS1** esté disponible en MSBuild, debe:

- Agregue el script al control de código fuente.
- Agregue el script a la solución en Visual Studio 2010.

No es necesario implementar el script con el contenido de la solución, independientemente de si planea ejecutar el script en el servidor de compilación o en un equipo remoto. Una opción consiste en agregar el script a una carpeta de soluciones. En el ejemplo de Contact Manager, como quiere usar el script de Windows PowerShell como parte del proceso de implementación, tiene sentido agregar el script a la carpeta de la solución de publicación.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

El contenido de las carpetas de soluciones se copia en los servidores de compilación como material de origen. Sin embargo, no forman parte de ningún resultado del proyecto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Ejecutar un script de Windows PowerShell en el servidor de compilación

En algunos escenarios, puede que desee ejecutar scripts de Windows PowerShell en el equipo que compila los proyectos. Por ejemplo, puede usar un script de Windows PowerShell para limpiar las carpetas de compilación o escribir entradas en un archivo de registro personalizado.

En términos de sintaxis, la ejecución de un script de Windows PowerShell desde un archivo de proyecto de MSBuild es igual que ejecutar un script de Windows PowerShell desde un símbolo del sistema normal. Debe invocar el archivo ejecutable de PowerShell. exe y usar el modificador **– Command** para proporcionar los comandos que desea que ejecute Windows PowerShell. (En Windows PowerShell V2, también puede usar el modificador **– File** ). El comando debe tomar este formato:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]

Por ejemplo:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]

Si la ruta de acceso al script incluye espacios, debe escribir la ruta de archivo entre comillas simples precedidas de una y comercial. No puede usar comillas dobles porque ya las ha utilizado para incluir el comando:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]

Existen algunas consideraciones adicionales al invocar este comando desde MSBuild. En primer lugar, debe incluir la marca **– Interactive** para asegurarse de que el script se ejecuta de manera silenciosa. A continuación, debe incluir la marca **– ExecutionPolicy** con un valor de argumento adecuado. Esto especifica la Directiva de ejecución que Windows PowerShell aplicará al script y le permite invalidar la Directiva de ejecución predeterminada, lo que puede impedir la ejecución del script. Puede elegir entre estos valores de argumento:

- Un valor de **Unrestricted** permitirá a Windows PowerShell ejecutar el script, independientemente de si el script está firmado.
- Un valor de **RemoteSigned** permitirá a Windows PowerShell ejecutar scripts sin firmar que se crearon en el equipo local. Sin embargo, los scripts que se crearon en otro lugar deben estar firmados. (En la práctica, es muy poco probable que haya creado un script de Windows PowerShell localmente en un servidor de compilación).
- Un valor de **AllSigned** permitirá a Windows PowerShell ejecutar solo scripts firmados.

La Directiva de ejecución predeterminada está **restringida**, lo que impide que Windows PowerShell ejecute cualquier archivo de script.

Por último, debe aplicar un carácter de escape a los caracteres XML reservados que se producen en el comando de Windows PowerShell:

- Reemplazar comillas simples por **&amp;apos;**
- Reemplace las comillas dobles por **&amp;quot;**
- Reemplazar y comercial con **&amp;amp;**

- Cuando realice estos cambios, el comando tendrá un aspecto similar al siguiente:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]

En el archivo del proyecto de MSBuild personalizado, puede crear un nuevo destino y usar la tarea **exec** para ejecutar este comando:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]

En este ejemplo, tenga en cuenta lo siguiente:

- Cualquier variable, como los valores de parámetro y la ubicación del ejecutable de Windows PowerShell, se declaran como propiedades de MSBuild.
- Se incluyen condiciones para permitir que los usuarios invaliden estos valores desde la línea de comandos.
- La propiedad **MSDeployComputerName** se declara en otro lugar del archivo de proyecto.

Al ejecutar este destino como parte del proceso de compilación, Windows PowerShell ejecutará el comando y escribirá una entrada de registro en el archivo especificado.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Ejecutar un script de Windows PowerShell en un equipo remoto

Windows PowerShell es capaz de ejecutar scripts en equipos remotos a través de [administración remota de Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Para ello, debe usar el cmdlet [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) . Esto le permite ejecutar el script en uno o varios equipos remotos sin copiar el script en los equipos remotos. Los resultados se devuelven al equipo local desde el que se ejecutó el script.

> [!NOTE]
> Antes de usar el cmdlet **Invoke-Command** para ejecutar scripts de Windows PowerShell en un equipo remoto, debe configurar un agente de escucha de WinRM para aceptar mensajes remotos. Para ello, ejecute el comando **WinRM quickconfig** en el equipo remoto. Para obtener más información, vea [instalación y configuración de administración remota de Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).

En una ventana de Windows PowerShell, usaría esta sintaxis para ejecutar el script **LogDeploy. PS1** en un equipo remoto:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]

> [!NOTE]
> Hay varias maneras de usar **Invoke-Command** para ejecutar un archivo de script, pero este enfoque es el más sencillo cuando se necesita proporcionar valores de parámetro y administrar rutas de acceso con espacios.

Cuando ejecute este comando desde un símbolo del sistema, debe invocar el ejecutable de Windows PowerShell y usar el parámetro **– Command** para proporcionar las instrucciones:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]

Como antes, debe proporcionar algunos modificadores adicionales y escapar cualquier carácter XML reservado cuando ejecute el comando desde MSBuild:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]

Por último, como antes, puede usar la tarea **exec** dentro de un destino MSBuild personalizado para ejecutar el comando:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]

Al ejecutar este destino como parte del proceso de compilación, Windows PowerShell ejecutará el script en el equipo especificado en el argumento **– ComputerName** .

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo ejecutar un script de Windows PowerShell desde un archivo de proyecto de MSBuild. Puede usar este enfoque para ejecutar un script de Windows PowerShell, ya sea de forma local o en un equipo remoto, como parte de un proceso de compilación e implementación automatizado o de un solo paso.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre la firma de scripts de Windows PowerShell y la administración de directivas de ejecución, consulte [ejecutar scripts de Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Para obtener instrucciones sobre cómo ejecutar comandos de Windows PowerShell desde un equipo remoto, vea [ejecutar comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).

Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, vea [Descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [Descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Siguiente](troubleshooting-the-packaging-process.md)
