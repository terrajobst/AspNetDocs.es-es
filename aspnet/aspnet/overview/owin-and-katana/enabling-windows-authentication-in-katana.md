---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Habilitación de la autenticación de Windows en Katana | Microsoft Docs
author: MikeWasson
description: 'En este artículo se muestra cómo habilitar la autenticación de Windows en Katana. Abarca dos escenarios: usar IIS para hospedar Katana y usar HttpListener para autohospedar Kat (...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500299"
---
# <a name="enabling-windows-authentication-in-katana"></a>Habilitar la autenticación de Windows en Katana

por [Mike Wasson](https://github.com/MikeWasson)

> En este artículo se muestra cómo habilitar la autenticación de Windows en Katana. Abarca dos escenarios: usar IIS para hospedar Katana y usar HttpListener para autohospedar Katana en un proceso personalizado. Gracias a Barry Dorrans, David Paspartúson y Chris Ross para revisar este artículo.

Katana es la implementación de Microsoft de [OWIN](http://owin.org/), la interfaz web abierta para .net. Puede leer una introducción a OWIN y Katana [aquí](an-overview-of-project-katana.md). La arquitectura OWIN tiene varias capas:

- Host: administra el proceso en el que se ejecuta la canalización OWIN.
- Servidor: abre un socket de red y escucha las solicitudes.
- Middleware: procesa la solicitud y la respuesta HTTP.

Katana proporciona actualmente dos servidores, que admiten la autenticación integrada de Windows:

- **Microsoft. Owin. host. SystemWeb**. Utiliza IIS con la canalización ASP.NET.
- **Microsoft. Owin. host. HttpListener**. Usa [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Este servidor es actualmente la opción predeterminada al autohospedar Katana.

> [!NOTE]
> Katana no proporciona actualmente middleware OWIN para la autenticación de Windows, porque esta funcionalidad ya está disponible en los servidores.

## <a name="windows-authentication-in-iis"></a>Autenticación de Windows en IIS

Con Microsoft. Owin. host. SystemWeb, puede simplemente habilitar la autenticación de Windows en IIS.

Comencemos por crear una nueva aplicación de ASP.NET, mediante la plantilla de proyecto "aplicación Web vacía de ASP.NET".

![](enabling-windows-authentication-in-katana/_static/image1.png)

A continuación, agregue paquetes NuGet. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Ahora, agregue una clase denominada `Startup` con el código siguiente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Eso es todo lo que necesita para crear una aplicación "Hello World" para OWIN, que se ejecuta en IIS. Presione F5 para depurar la aplicación. Debe ver "Hello World!" en la ventana del explorador.

![](enabling-windows-authentication-in-katana/_static/image2.png)

A continuación, habilitaremos la autenticación de Windows en IIS Express. En el menú **Ver** , seleccione **propiedades**. Haga clic en el nombre del proyecto en Explorador de soluciones para ver las propiedades del proyecto.

En la ventana **propiedades** , establezca **autenticación anónima** en **deshabilitada** y establezca **autenticación de Windows** en **habilitado**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Al ejecutar la aplicación desde Visual Studio, IIS Express requerirá las credenciales de Windows del usuario. Puede verlo usando [Fiddler](http://fiddler2.com/home) u otra herramienta de depuración http. Este es un ejemplo de respuesta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Los encabezados WWW-Authenticate en esta respuesta indican que el servidor es compatible con el protocolo [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que usa Kerberos o NTLM.

Posteriormente, al implementar la aplicación en un servidor, siga [estos pasos](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar la autenticación de Windows en IIS en ese servidor.

## <a name="windows-authentication-in-httplistener"></a>Autenticación de Windows en HttpListener

Si usa Microsoft. Owin. host. HttpListener para autohospedar Katana, puede habilitar la autenticación de Windows directamente en la instancia de **HttpListener** .

En primer lugar, cree una nueva aplicación de consola. A continuación, agregue paquetes NuGet. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Ahora, agregue una clase denominada `Startup` con el código siguiente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Esta clase implementa el mismo ejemplo "Hello World" de antes, pero también establece la autenticación de Windows como esquema de autenticación.

Dentro de la función `Main`, inicie la canalización OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Puede enviar una solicitud en Fiddler para confirmar que la aplicación usa la autenticación de Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Temas relacionados

[Información general del proyecto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Descripción de la autenticación de formularios OWIN en MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
