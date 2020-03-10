---
uid: whitepapers/side-by-side-with-10
title: ASP.NET ejecución en paralelo de .NET Framework 1,0 y 1,1 | Microsoft Docs
author: rick-anderson
description: En estas notas del producto se describe cómo instalar .NET 1,0 y .NET 1,1 en el equipo, lo que permite que una aplicación Web ASP.NET se ejecute en cualquiera de las versiones de fotogramas...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513835"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Ejecución en paralelo de ASP.NET de .NET Framework 1.0 y 1.1

> En estas notas del producto se describe cómo instalar .NET 1,0 y .NET 1,1 en el equipo, lo que permite que una aplicación Web ASP.NET se ejecute en cualquier versión del marco.
> 
> Se aplica a ASP.NET 1,0 y ASP.NET 1,1.

En ASP.NET, se dice que las aplicaciones se ejecutan en paralelo cuando están instaladas en el mismo equipo, pero utilizan versiones diferentes de la .NET Framework. En el siguiente tema se describe cómo configurar aplicaciones de ASP.NET para la ejecución en paralelo y se proporcionan pasos detallados para:

- [Mantenimiento de la asignación de la aplicación web a .NET Framework versión 1,0 durante la instalación](#1)
- [Asignar una aplicación web a una versión específica de la .NET Framework](#2)
- [Buscar la versión del .NET Framework que utiliza un sitio web](#3)

Tradicionalmente, cuando se actualiza un componente o una aplicación en un equipo, se quita la versión anterior y se reemplaza por la versión más reciente. Si la nueva versión no es compatible con la versión anterior, normalmente interrumpe otras aplicaciones que usan el componente o la aplicación. El .NET Framework proporciona compatibilidad con la ejecución en paralelo, que permite instalar varias versiones de un ensamblado o una aplicación en el mismo equipo al mismo tiempo. Dado que se pueden instalar varias versiones simultáneamente, las aplicaciones administradas pueden seleccionar qué versión usar sin afectar a las aplicaciones que usan una versión diferente.

De forma predeterminada, durante la instalación de la versión 1,1 de .NET Framework, todas las aplicaciones de ASP.NET existentes se reconfiguran automáticamente para usar la versión más reciente del .NET Framework. Si no desea que las aplicaciones de ASP.NET tengan como valor predeterminado .NET Framework 1,1, haga clic [aquí](#1) para obtener información sobre cómo evitar esto durante la instalación.

Si actualiza el servidor Web a .NET Framework 1,1 y desea que una o varias aplicaciones web se ejecuten .NET Framework 1,0, debe actualizar la asignación de scripts de Internet Information Services (IIS). La asignación de script es el mecanismo para asignar la extensión de archivo. aspx para una aplicación web específica a una versión de la .NET Framework. Haga clic [aquí](#2) para obtener información sobre cómo asignar una aplicación web a una versión específica de la .NET Framework.

Puede usar Internet Information Manager o la herramienta de registro de IIS de ASP.NET (ASPNET\_regiis. exe) para averiguar qué .NET Framework versión está ejecutando una aplicación web determinada. Haga clic [aquí](#3) para obtener información sobre cómo buscar la versión del .NET Framework que utiliza un sitio Web.

Una consideración de importación al migrar a .NET Framework 1,1 es que cada versión de la .NET Framework utiliza su propio archivo Machine. config. Como resultado, si un administrador web ha realizado cambios en el archivo Machine. config, esos cambios se deben migrar al archivo Machine. config de la .NET Framework 1,1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Mantenimiento de la asignación de la aplicación web a .NET Framework 1,0 durante la instalación

De forma predeterminada, todas las aplicaciones de ASP.NET existentes se reconfiguran automáticamente durante la instalación para usar la versión más reciente de la .NET Framework. Con la versión más reciente de la .NET Framework, las aplicaciones pueden aprovechar al máximo las mejoras y las nuevas características incluidas en la nueva versión. Al mismo tiempo, el administrador web, que podría querer un control granular sobre qué aplicaciones se actualizan, puede impedir la reasignación automática de todas las aplicaciones de ASP.NET existentes durante la instalación del .NET Framework.

Para evitar la reasignación automática de toda la aplicación ASP.NET a la versión más reciente de la .NET Framework, el administrador web puede usar la opción de línea de comandos/noaspupgrade con el programa de instalación Dotnetfx. exe.

**Para evitar la reasignación total de la aplicación ASP.NET a la versión más reciente**

1. Vaya a **Inicio**.
2. Haga clic en **Ejecutar**.
3. Escriba **cmd**.
4. Haga clic en **Aceptar**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. En el símbolo del sistema, escriba la siguiente línea para iniciar la instalación del .NET Framework: **Dotnetfx. exe/c: "Install/noaspupgrade?** .  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Haga clic en **sí** en el programa de instalación de Microsoft .NET Framework 1,1. Se iniciará el proceso de instalación del .NET Framework 1,1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Asignar una aplicación web a una versión específica de la .NET Framework

Cada versión del .NET Framework incluye una versión de la herramienta de registro de IIS de ASP.NET (ASPNET\_regiis. exe). Esta herramienta permite a los administradores especificar que una aplicación web se ejecutará en una versión determinada del .NET Framework. Esto se conoce como la asignación de una aplicación web a una versión de la .NET Framework. Los administradores deben seleccionar el ASPNET\_regiis. exe que se corresponda con la versión de la .NET Framework que se asociará con la aplicación Web. Por ejemplo, un administrador que desee especificar que un sitio web use .NET Framework 1,1 debe usar el ASPNET\_regiis. exe que viene con .NET Framework 1,1.

ASPNET\_regiis. exe para la versión 1,0 se encuentra en:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\_regiis

ASPNET\_regiis. exe para la versión 1, 1 se encuentra en:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis

ASPNET\_regiis. exe proporciona dos opciones para la asignación de scripts a una aplicación web:

- **-s** establece el mapa de script en la ruta de acceso y en sus directorios secundarios.
- **-SN** establece el mapa de script solo en la ruta de acceso.

La ruta de acceso define la ruta de metadatos de IIS de la aplicación Web, que se define con el formato W3SVC/ROOT/{WebSiteNumber}/{nombre de la aplicación\_}. Por ejemplo, para una aplicación web denominada portal ubicada en el sitio web predeterminado, la ruta de acceso de la metabase es W3SVC/1/ROOT/portal.

![](side-by-side-with-10/_static/image4.gif)

Nota también puede usar una herramienta denominada editor de metabase para obtener la ruta de la metabase. Puede descargar esta herramienta desde el sitio de Soporte técnico de Microsoft en [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Ejecute ASPNET\_regiis. exe-s W3SVC/1/ROOT/portal para actualizar la asignación de script IIS del portal y su subaplicación.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Ejecute ASPNET\_regiis. exe-SN W3SVC/1/ROOT/portal para actualizar el mapa de scripts de IIS del portal, sin afectar a las aplicaciones en el portal? subdirectorios.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Buscar la versión de .NET Framework que usa una aplicación Web

Un administrador puede usar el Service Manager de Internet para buscar qué versión del .NET Framework ejecuta un sitio Web. Las distintas versiones de sistema operativo inician Internet Service Manager de forma diferente. Para iniciar Service Manager, siga los pasos que se indican a continuación.

**Para iniciar Internet Service Manager**

1. Vaya a **Inicio**.
2. Haga clic en **Ejecutar**.
3. Escriba **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. En el Service Manager de Internet, seleccione la aplicación Web cuya versión de .NET Framework desea conocer.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Haga clic con el botón derecho en la aplicación web y haga clic en **propiedades.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. En la ventana de propiedades, seleccione **configuración.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. En la tabla asignación de aplicaciones, seleccione **. aspx**y haga clic en **Editar**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. En el cuadro de texto **ejecutable** , desplácese por el directorio de la versión. Si el directorio de versión es v. 1.1.4322, la aplicación se asigna a .NET Framework 1,1. Por el contrario, si el directorio de versiones es la versión 1.0.3705, la aplicación se asigna a .NET Framework 1,0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
