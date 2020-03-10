---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET acceso denegado a directorios IIS | Microsoft Docs
author: rick-anderson
description: En estas notas del producto se describe lo que debe hacer si una solicitud a la aplicación ASP.NET devuelve el error "denegado el acceso al directorio DirectoryName. No se pudo...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78518575"
---
# <a name="aspnet-denied-access-to-iis-directories"></a>Acceso denegado de ASP.NET a los directorios de IIS

> En estas notas del producto se describe lo que debe hacer si una solicitud a la aplicación ASP.NET devuelve el error "denegado el acceso al directorio *directoryname* . No se pudo iniciar la supervisión de los cambios de directorio. "
> 
> Se aplica a ASP.NET 1,0 y ASP.NET 1,1.

ASP.NET v1 RTM ahora se ejecuta con una cuenta de Windows con menos privilegios registrada como la cuenta "ASPNET" en un equipo local.

En algunos sistemas bloqueados, es posible que esta cuenta no tenga de forma predeterminada acceso de lectura a los directorios de contenido de un sitio web, el directorio raíz de la aplicación o el directorio raíz del sitio Web. En este caso, recibirá el siguiente error al solicitar páginas de una aplicación web determinada:

![](denied-access-to-iis-directories/_static/image1.jpg)

Para solucionarlo, deberá cambiar los permisos de seguridad en los directorios correspondientes.

En concreto, ASP.NET requiere acceso de lectura, ejecución y lista para la cuenta de ASPNET para la raíz del sitio web (por ejemplo: c:\Inetpub\wwwroot o cualquier directorio de sitio alternativo que haya configurado en IIS), el directorio de contenido y el directorio raíz de la aplicación. para supervisar los cambios del archivo de configuración. La raíz de la aplicación corresponde a la ruta de acceso de la carpeta asociada al directorio virtual de la aplicación en la herramienta de administración de IIS (inetmgr).

Por ejemplo, considere la siguiente jerarquía de aplicación en la carpeta wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

En este ejemplo, la cuenta ASPNET necesita los permisos de lectura definidos anteriormente para el contenido en el directorio MyApp y wwwroot. También se puede usar una sola ACL heredada en la carpeta raíz para ambos directorios, si están anidados.

Para agregar permisos a un directorio, realice los pasos siguientes:

- Mediante el explorador de Windows, navegue hasta el directorio
- Haga clic con el botón derecho en la carpeta del directorio y elija "propiedades".
- Vaya a la pestaña "seguridad" en el cuadro de diálogo de propiedades.
- Haga clic en el botón "agregar" y escriba el nombre de la máquina seguido del nombre de la cuenta ASPNET. Por ejemplo, en un equipo denominado "WebDev", escribiría webdev\ASPNET y aceptaría "OK".
- Asegúrese de que la cuenta de ASPNET tiene activadas las casillas de verificación "leer &amp; ejecutar", "Mostrar el contenido de la carpeta" y "leer".
- Presione Aceptar para descartar el cuadro de diálogo y guardar los cambios.

![](denied-access-to-iis-directories/_static/image2.jpg)

Si lo desea, estos cambios se pueden automatizar mediante scripts o la herramienta "cacls. exe" que se incluye con Windows. Para obtener más información sobre la cuenta de ASPNET, consulte el [documento de preguntas más frecuentes](https://go.microsoft.com/fwlink/?LinkId=5828).

Si una aplicación web determinada se basa en tener permisos de escritura o modificación en una carpeta o un archivo determinados, esto se puede conceder siguiendo el mismo procedimiento y comprobando las casillas "escribir" y/o "modificar".

En los equipos que permiten a todos los usuarios o al grupo de usuarios tener acceso de lectura a estos directorios (que es la configuración predeterminada), no se producirán problemas y no se requerirán los pasos anteriores.
