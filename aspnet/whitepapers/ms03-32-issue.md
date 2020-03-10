---
uid: whitepapers/ms03-32-issue
title: Corrección del error "la aplicación de servidor no está disponible" después de aplicar la actualización de seguridad para IE | Microsoft Docs
author: rick-anderson
description: En este documento se describe la revisión que corrige un problema con la actualización de seguridad de MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1,0 que se ejecutan en Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: e0b6776cbfe22e341ac7105f03daac5074b480fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463459"
---
# <a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Corregir el error "Aplicación de servidor no disponible" después de aplicar la actualización de seguridad de Internet Explorer

> En este documento se describe la revisión que corrige un problema con la actualización de seguridad de MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1,0 que se ejecutan en Windows XP Professional.
> 
> Se aplica a ASP.NET 1,0 y Windows XP Professional.

Microsoft identificó un problema con la actualización de seguridad de MS03-32 para la revisión de seguridad de Internet Explorer y ASP.NET 1,0 que se ejecuta en Windows XP. Esta revisión se puede instalar manualmente o mediante la obtención de actualizaciones críticas recientes del sitio Windows Update.

El síntoma de este problema es que, después de instalar la revisión en un equipo con Windows XP, todas las solicitudes a las aplicaciones ASP.NET que se ejecutan en el servidor Web de IIS 5,1 local generan un mensaje de error que indica "aplicación de servidor no disponible". Las solicitudes a los servidores web remotos no se ven afectadas.

Este problema solo afecta a las instalaciones que ejecutan ASP.NET 1,0 en Windows XP. No afecta a las máquinas que ejecutan Windows 2000 o Windows Server 2003. Tampoco afecta a las máquinas que ejecutan Windows XP con ASP.NET 1,1 instalado.

Tenga en cuenta que este problema **no es** un error de seguridad con ASP.net. **No** se abre ni permite ningún ataque malintencionado contra una aplicación o un servidor ASP.net. En su lugar, es un error funcional causado por el propio parche.

Estamos trabajando en una solución permanente para este problema. Mientras tanto, puede ejecutar el siguiente archivo por lotes como una solución para el problema. El archivo por lotes hace lo siguiente:

1. Detiene los servicios de estado de IIS y ASP.NET
2. Elimina y vuelve a crear la cuenta de ASPNET con una contraseña temporal conocida.
3. Usa el comando de `runas` de Windows para iniciar un ejecutable que crea un perfil de usuario de ASPNET.
4. Vuelve a registrar ASP.NET. Esto crea una nueva contraseña aleatoria para la cuenta y aplica la configuración predeterminada de control de acceso de ASP.NET para ella.
5. Reinicia el servicio IIS

El archivo por lotes contiene una contraseña temporal codificada de "<strong>1pass\@Word</strong>", que se le pedirá que escriba para el comando runas cuando se ejecute el archivo por lotes. Una vez completado el comando runas, la contraseña de la cuenta ASPNET se vuelve a crear con un valor aleatorio seguro. Tenga en cuenta que se puede producir un error en el archivo por lotes si la contraseña codificada no cumple los requisitos de complejidad de contraseñas de su entorno. Si ese es el caso, puede cambiarlo a otro valor que sea adecuado para su entorno.

*> [!IMPORTANT]* Si ha agregado valores de configuración de control de acceso personalizados o permisos de cuenta de base de datos para la cuenta de ASPNET, deberá volver a crearlos una vez completado este archivo por lotes. Esto se debe a que, cuando se vuelve a crear la cuenta, se obtiene un nuevo identificador de seguridad (SID).

*> [!IMPORTANT]* Si está ejecutando el proceso de trabajo ASP.NET con una cuenta personalizada que no sea la cuenta ASPNET, no debe ejecutar este archivo por lotes. En su lugar, debe iniciar sesión de forma interactiva o utilizar el comando runas con esa cuenta, que creará un perfil de usuario para esa cuenta.

El archivo por lotes se incluye en el archivo de extracción automática que aparece a continuación. Para usarla:

1. Debe ejecutarse como una cuenta con privilegios de administrador.
2. [Descargar y abrir el archivo ejecutable autoextraíble](ms03-32-issue/_static/fixup1.exe)
3. Extraiga el contenido en c:\
4. Seleccione ejecutar... en el menú Inicio, escriba `cmd.exe`
5. En las ventanas de comandos abiertas, escriba `c:\fixup.cmd`.
6. Cuando se le solicite, escriba <strong>1pass\@Word</strong> como contraseña.
7. Si previamente ha personalizado la configuración de control de acceso o los permisos de la cuenta de base de datos para la cuenta de ASPNET, deberá volver a aplicar esta configuración ahora.

Muchas disculpas por las molestias que esto ha causado. Publicaremos información adicional a medida que esté disponible.

En la matriz siguiente se detallan las plataformas y versiones afectadas por este problema.

| .NET Framework | Plataforma | Qué |
| --- | --- | --- |
| Versión 1.0 | Windows 2000 Professional | No |
| Versión 1.0 | Windows 2000 Server | No |
| Versión 1.0 | Windows XP Professional | Sí |
| Versión 1.0 | Windows Server 2003 | No |
| Versión 1.0 | Windows XP Home con Cassini | No |
| Versión 1.1 | Windows 2000 Professional | No |
| Versión 1.1 | Windows 2000 Server | No |
| Versión 1.1 | Windows XP Professional | No |
| Versión 1.1 | Windows Server 2003 | No |
| Versión 1.1 | Windows XP Home con Cassini | No |

Gracias,   
 El equipo de ASP.NET
