---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Autenticar a los usuarios con la autenticación de Windows (VB) | Microsoft Docs
author: microsoft
description: Aprenda a usar la autenticación de Windows en el contexto de una aplicación MVC. Aprenderá a habilitar la autenticación de Windows en el co Web de la aplicación...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: aa64b1f9ef6461a81611ca066310dca2d545baa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506245"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Autenticar a los usuarios con la autenticación de Windows (VB)

por [Microsoft](https://github.com/microsoft)

> Aprenda a usar la autenticación de Windows en el contexto de una aplicación MVC. Aprenderá a habilitar la autenticación de Windows en el archivo de configuración Web de la aplicación y cómo configurar la autenticación con IIS. Por último, aprenderá a usar el atributo [Authorize] para restringir el acceso a las acciones de controlador a determinados usuarios o grupos de Windows.

El objetivo de este tutorial es explicar cómo puede aprovechar las ventajas de las características de seguridad integradas en Internet Information Services para proteger las vistas en las aplicaciones MVC. Aprenderá a permitir que solo los usuarios o usuarios de Windows que sean miembros de determinados grupos de Windows puedan invocar las acciones de controlador.

El uso de la autenticación de Windows tiene sentido cuando se crea un sitio web de empresa interno (un sitio de intranet) y se desea que los usuarios puedan usar sus nombres de usuario y contraseñas estándar de Windows al acceder al sitio Web. Si va a crear un sitio web orientado al exterior (un sitio web de Internet), considere la posibilidad de usar la autenticación de formularios.

#### <a name="enabling-windows-authentication"></a>Habilitación de la autenticación de Windows

Al crear una nueva aplicación ASP.NET MVC, la autenticación de Windows no está habilitada de forma predeterminada. La autenticación mediante formularios es el tipo de autenticación predeterminado habilitado para las aplicaciones MVC. Debe habilitar la autenticación de Windows modificando el archivo de configuración Web (Web. config) de la aplicación MVC. Busque la sección&gt; de autenticación de &lt;y modifíquela para usar Windows en lugar de la autenticación de formularios de la siguiente manera:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Cuando se habilita la autenticación de Windows, el servidor Web se convierte en responsable de la autenticación de los usuarios. Normalmente, hay dos tipos diferentes de servidores web que se usan al crear e implementar una aplicación ASP.NET MVC.

En primer lugar, al desarrollar una aplicación MVC, se usa el servidor Web de desarrollo de ASP.NET incluido con Visual Studio. De forma predeterminada, el servidor Web de desarrollo de ASP.NET ejecuta todas las páginas en el contexto de la cuenta de Windows actual (la cuenta que usó para iniciar sesión en Windows).

El servidor Web de desarrollo de ASP.NET también admite la autenticación NTLM. Para habilitar la autenticación NTLM, haga clic con el botón secundario en el nombre del proyecto en la ventana de Explorador de soluciones y seleccione Propiedades. Después, seleccione la pestaña web y active la casilla NTLM (vea la figura 1).

**Figura 1: habilitación de la autenticación NTLM para el servidor Web de desarrollo de ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

En el caso de una aplicación Web de producción, se usa IIS como servidor Web. IIS admite varios tipos de autenticación, entre los que se incluyen:

- Autenticación básica: se define como parte del protocolo HTTP 1,0. Envía nombres de usuario y contraseñas en texto no cifrado (codificados en Base64) a través de Internet. -Autenticación implícita: envía un hash de una contraseña, en lugar de la propia contraseña, a través de Internet. -Autenticación integrada de Windows (NTLM): el mejor tipo de autenticación que se usa en entornos de intranet mediante Windows. -Autenticación de certificado: habilita la autenticación mediante un certificado del lado cliente. El certificado se asigna a una cuenta de usuario de Windows.

> [!NOTE] 
> 
> Para obtener información general más detallada de estos tipos diferentes de autenticación, consulte [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

Puede usar el administrador de Internet Information Services para habilitar un tipo determinado de autenticación. Tenga en cuenta que todos los tipos de autenticación no están disponibles en el caso de todos los sistemas operativos. Además, si usa IIS 7,0 con Windows Vista, deberá habilitar los distintos tipos de autenticación de Windows antes de que aparezcan en el administrador de Internet Information Services. Abra el **Panel de control, programas, programas y características, Active o desactive las características de Windows**y expanda el nodo Internet Information Services (consulte la figura 2).

**Figura 2: habilitación de las características de Windows IIS**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Con Internet Information Services, puede habilitar o deshabilitar distintos tipos de autenticación. Por ejemplo, la figura 3 ilustra la deshabilitación de la autenticación anónima y la habilitación de la autenticación integrada de Windows (NTLM) al usar IIS 7,0.

**Ilustración 3: habilitar la autenticación integrada de Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorización de usuarios y grupos de Windows

Después de habilitar la autenticación de Windows, puede usar el atributo &lt;autorizar&gt; para controlar el acceso a los controladores o acciones de controlador. Este atributo se puede aplicar a un controlador MVC completo o a una acción de controlador determinada.

Por ejemplo, el controlador Home de la lista 1 expone tres acciones denominadas index (), CompanySecrets () y StephenSecrets (). Cualquier persona puede invocar la acción index (). Sin embargo, solo los miembros del grupo de administradores locales de Windows pueden invocar la acción CompanySecrets (). Por último, solo el usuario de dominio de Windows denominado Stephen (en el dominio Redmond) puede invocar la acción StephenSecrets ().

**Lista 1: Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Debido al control de cuentas de usuario (UAC) de Windows, cuando se trabaja con Windows Vista o Windows Server 2008, el grupo de administradores locales se comportará de forma diferente a otros grupos. El &lt;autorizar&gt; atributo no reconocerá correctamente a un miembro del grupo de administradores locales a menos que modifique la configuración de UAC del equipo.

Exactamente lo que ocurre cuando se intenta invocar una acción de controlador sin tener los permisos adecuados depende del tipo de autenticación habilitada. De forma predeterminada, cuando se usa el Servidor de desarrollo de ASP.NET, simplemente se obtiene una página en blanco. La página se sirve con un estado de respuesta HTTP **no autorizado de 401** .

Por otro lado, si usa IIS con la autenticación anónima deshabilitada y la autenticación básica habilitada, seguirá obteniendo un símbolo del sistema de inicio de sesión cada vez que solicite la página protegida (consulte la figura 4).

**Figura 4: cuadro de diálogo Inicio de sesión de autenticación básica**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Resumen

En este tutorial se explica cómo puede usar la autenticación de Windows en el contexto de una aplicación ASP.NET MVC. Aprendió a habilitar la autenticación de Windows en el archivo de configuración Web de la aplicación y cómo configurar la autenticación con IIS. Por último, ha aprendido a usar el atributo &lt;autorizar&gt; para restringir el acceso a las acciones de controlador a determinados usuarios o grupos de Windows.

> [!div class="step-by-step"]
> [Anterior](authenticating-users-with-forms-authentication-vb.md)
> [Siguiente](preventing-javascript-injection-attacks-vb.md)
