---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Diferencias principales entre IIS y el Servidor de desarrollo de ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Al probar localmente una aplicación de ASP.NET, es probable que esté usando el servidor Web de desarrollo de ASP.NET. Sin embargo, lo más probable es que el sitio web de producción sea Pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 880bb403e671446a77d7eebccf578a1dc714d1f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586520"
---
# <a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Diferencias principales entre IIS y el Servidor de desarrollo de ASP.NET (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Al probar localmente una aplicación de ASP.NET, es probable que esté usando el servidor Web de desarrollo de ASP.NET. Sin embargo, lo más probable es que el sitio web de producción esté basado en IIS. Hay algunas diferencias entre el modo en que estos servidores web controlan las solicitudes y estas diferencias pueden tener consecuencias importantes. En este tutorial se exploran algunas de las diferencias en alemán.

## <a name="introduction"></a>Introducción

Cada vez que un usuario visita una aplicación ASP.NET, su explorador envía una solicitud al sitio Web. El software del servidor Web recoge esa solicitud, que se coordina con el tiempo de ejecución de ASP.NET para generar y devolver el contenido del recurso solicitado. [**I** Internet S ervices ( IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) son un conjunto de servicios que proporcionan una funcionalidad común basada en Internet para servidores Windows. IIS es el servidor Web que se usa con más frecuencia para aplicaciones ASP.NET en entornos de producción. lo más probable es que el proveedor de host web use el software de servidor web para servir a la aplicación ASP.NET. IIS también se puede usar como software de servidor Web en el entorno de desarrollo, aunque esto implica la instalación de IIS y su configuración correcta.

El Servidor de desarrollo de ASP.NET es una opción de servidor Web alternativa para el entorno de desarrollo; se incluye con y se integra en Visual Studio. A menos que la aplicación web se haya configurado para usar IIS, el Servidor de desarrollo de ASP.NET se inicia automáticamente y se usa como el servidor Web la primera vez que visita una página web desde Visual Studio. Las aplicaciones Web de demostración que creamos en el tutorial [*determinar qué archivos deben implementarse*](determining-what-files-need-to-be-deployed-vb.md) fueron las aplicaciones web basadas en sistema de archivos que no se han configurado para usar IIS. Por lo tanto, al visitar cualquiera de estos sitios web desde Visual Studio, se usa el Servidor de desarrollo de ASP.NET.

En un mundo perfecto, los entornos de desarrollo y producción serían idénticos. Sin embargo, como analizamos en el tutorial anterior, no es raro que los entornos tengan opciones de configuración diferentes. El uso de un software de servidor web diferente en los entornos agrega otra variable que se debe tener en cuenta al implementar una aplicación. En este tutorial se describen las diferencias principales entre IIS y el Servidor de desarrollo de ASP.NET. Debido a estas diferencias, hay escenarios en los que el código que se ejecuta de forma insegura en el entorno de desarrollo inicia una excepción o se comporta de manera diferente cuando se ejecuta en producción.

## <a name="security-context-differences"></a>Diferencias de contexto de seguridad

Siempre que el software del servidor Web controla una solicitud entrante, asocia esa solicitud a un contexto de seguridad determinado. El sistema operativo usa esta información de contexto de seguridad para determinar las acciones permitidas por la solicitud. Por ejemplo, una página de ASP.NET podría incluir código que registra un mensaje en un archivo en disco. Para que esta página de ASP.NET se ejecute sin errores, el contexto de seguridad debe tener los permisos de nivel de sistema de archivo adecuados, es decir, permisos de escritura en ese archivo.

El Servidor de desarrollo de ASP.NET asocia las solicitudes entrantes con el contexto de seguridad del usuario que ha iniciado sesión actualmente. Si ha iniciado sesión en el escritorio como administrador, las páginas de ASP.NET atendidas por el Servidor de desarrollo de ASP.NET tendrán los mismos derechos de acceso que un administrador. Sin embargo, las solicitudes de ASP.NET controladas por IIS están asociadas a una cuenta de equipo específica. De forma predeterminada, la cuenta de equipo de servicio de red se usa en las versiones 6 y 7 de IIS, aunque es posible que el proveedor de host Web haya configurado una cuenta única para cada cliente. Lo más probable es que el proveedor de hosts web tenga permisos limitados para esta cuenta de equipo. El resultado neto es que puede tener páginas web que se ejecutan sin errores en el entorno de desarrollo, pero que generan excepciones relacionadas con la autorización cuando se hospedan en el entorno de producción.

Para mostrar este tipo de error en acción he creado una página en el sitio web de reseñas de libros que crea un archivo en el disco que almacena la fecha y la hora más recientes que ha visto el *ASP.NET 3,5 en la revisión de 24 horas* . Para continuar, abra la página `~/Tech/TYASP35.aspx` y agregue el código siguiente al controlador de eventos `Page_Load`:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> El [método`File.WriteAllText`](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crea un nuevo archivo si no existe y, a continuación, escribe el contenido especificado en él. Si el archivo ya existe, se sobrescribe el contenido existente.

A continuación, visite la página de análisis de *ASP.NET 3,5 en 24 horas* en el entorno de desarrollo mediante el servidor de desarrollo de ASP.net. Suponiendo que ha iniciado sesión en el equipo con una cuenta que tiene los permisos adecuados para crear y modificar un archivo de texto en el directorio raíz de la aplicación Web, la revisión del libro aparece igual que antes, pero cada vez que se visita la página, la fecha y la hora y la dirección IP del usuario se almacenan en el archivo de `LastTYASP35Access.txt`. Apunte el explorador a este archivo; debería ver un mensaje similar al que se muestra en la figura 1.

[![el archivo de texto contiene la última fecha y hora en que se Visitó la revisión del libro&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figura 1**: el archivo de texto contiene la última fecha y hora en que se Visitó la revisión del libro ([haga clic para ver la imagen a tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))

Implemente la aplicación web en el entorno de producción y, a continuación, visite la página de análisis de la agenda *de ASP.NET 3,5 en 24 horas* . En este momento, debería ver la página de revisión del libro como normal o el mensaje de error que se muestra en la figura 2. Algunos proveedores de host web conceden permisos de escritura a la cuenta de máquina anónima ASP.NET, en cuyo caso la página funcionará sin errores. Sin embargo, si el proveedor de host web prohíbe el acceso de escritura para la cuenta anónima, se genera una [excepción`UnauthorizedAccessException`](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) cuando la página de `TYASP35.aspx` intenta escribir la fecha y hora actuales en el archivo `LastTYASP35Access.txt`.

[![la cuenta de equipo predeterminada usada por IIS no tiene permisos para escribir en el sistema de archivos](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figura 2**: la cuenta de equipo predeterminada usada por IIS no tiene permisos para escribir en el sistema de archivos ([haga clic para ver la imagen de tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))

La buena noticia es que la mayoría de los proveedores de host web tienen algún tipo de herramienta de permisos que le permite especificar los permisos del sistema de archivos en el sitio Web. Conceda a la cuenta de ASP.NET anónima acceso de escritura en el directorio raíz y, a continuación, vuelva a visitar la página de revisión del libro. (Si es necesario, póngase en contacto con su proveedor de host web para obtener ayuda sobre cómo conceder permisos de escritura a la cuenta de ASP.NET predeterminada). Esta vez, la página debe cargarse sin errores y el archivo de `LastTYASP35Access.txt` se debe crear correctamente.

Lo más importante es que, dado que el Servidor de desarrollo de ASP.NET funciona en un contexto de seguridad diferente que IIS, es posible que las páginas de ASP.NET lean o escriban en el sistema de archivos, lean o escriban en el registro de eventos de Windows o lean o escriban en las ventanas.  el registro funcionará como se espera en el desarrollo, pero generará excepciones cuando se produzca en producción. Al compilar una aplicación web que se implementará en un entorno de hospedaje web compartido, no lea ni escriba en el registro de eventos o en el registro de Windows. Tenga también en cuenta las páginas de ASP.NET que leen o escriben en el sistema de archivos, ya que puede que tenga que conceder privilegios de lectura y escritura en las carpetas adecuadas en el entorno de producción.

## <a name="differences-on-serving-static-content"></a>Diferencias en el servicio de contenido estático

Otra diferencia principal entre IIS y el Servidor de desarrollo de ASP.NET es cómo controlan las solicitudes de contenido estático. Cada solicitud que entra en el Servidor de desarrollo de ASP.NET, ya sea para una página ASP.NET, una imagen o un archivo JavaScript, se procesa mediante el tiempo de ejecución de ASP.NET. De forma predeterminada, IIS solo invoca el tiempo de ejecución de ASP.NET cuando una solicitud entra en un recurso ASP.NET, como una página Web ASP.NET, un servicio Web, etc. IIS recupera las solicitudes de contenido estático (imágenes, archivos CSS, archivos JavaScript, archivos PDF, archivos ZIP y similares) sin la implicación del tiempo de ejecución de ASP.NET. (Es posible indicar a IIS que trabaje con el tiempo de ejecución de ASP.NET al servir contenido estático; consulte la sección "realización de autenticación basada en formularios y autenticación de direcciones URL en archivos estáticos con IIS 7" de este tutorial para obtener más información).

El tiempo de ejecución de ASP.NET realiza una serie de pasos para generar el contenido solicitado, incluida la autenticación (identificación del solicitante) y la autorización (determinar si el solicitante tiene permiso para ver el contenido solicitado). Una forma popular de autenticación es la *autenticación basada en formularios*, en la que un usuario se identifica escribiendo sus credenciales (normalmente un nombre de usuario y una contraseña) en los cuadros de texto de una página web. Tras validar sus credenciales, el sitio Web almacena una cookie de *vale de autenticación* en el explorador del usuario, que se envía con cada solicitud subsiguiente al sitio web y es lo que se usa para autenticar al usuario. Además, es posible especificar reglas de *autorización de URL* que determinan lo que los usuarios pueden o no pueden acceder a una carpeta determinada. Muchos sitios web de ASP.NET usan la autenticación basada en formularios y la autorización de URL para admitir cuentas de usuario y para definir partes del sitio a las que solo pueden tener acceso los usuarios autenticados o los usuarios que pertenecen a un rol determinado.

> [!NOTE]
> Para un examen exhaustivo de ASP. La autenticación basada en formularios de la red, la autorización de direcciones URL y otras características relacionadas con la cuenta de usuario, asegúrese de consultar los [tutoriales de seguridad de los sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Considere la posibilidad de usar un sitio web que admita cuentas de usuario mediante la autorización basada en formularios y que tenga una carpeta que, mediante la autorización de direcciones URL, esté configurada para permitir solo usuarios autenticados. Imagine que esta carpeta contiene páginas de ASP.NET y archivos PDF y que la intención es que solo los usuarios autenticados puedan ver estos archivos PDF.

¿Qué ocurre si un visitante intenta ver uno de estos archivos PDF escribiendo la dirección URL directamente en la barra de direcciones del explorador? Para averiguarlo, vamos a crear una nueva carpeta en el sitio de revisiones del libro, agregar algunos archivos PDF y configurar el sitio para que use la autorización de URL para prohibir a los usuarios anónimos que visiten esta carpeta. Si descarga la aplicación de demostración, verá que he creado una carpeta llamada `PrivateDocs` y agregado un archivo PDF de mis [tutoriales de seguridad de sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (How). La carpeta `PrivateDocs` también contiene un archivo `Web.config` que especifica las reglas de autorización de dirección URL para denegar a los usuarios anónimos:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Por último, configuré la aplicación web para usar la autenticación basada en formularios actualizando el archivo Web. config en el directorio raíz, reemplazando:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Por:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Con el Servidor de desarrollo de ASP.NET, visite el sitio y escriba la dirección URL directa de uno de los archivos PDF en la barra de direcciones del explorador. Si descargó el sitio web asociado a este tutorial, la dirección URL debería tener un aspecto similar al siguiente: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Al escribir esta dirección URL en la barra de direcciones, el explorador envía una solicitud al Servidor de desarrollo de ASP.NET del archivo. El Servidor de desarrollo de ASP.NET entrega la solicitud al tiempo de ejecución de ASP.NET para su procesamiento. Dado que aún no hemos iniciado sesión, y como el `Web.config` en la carpeta `PrivateDocs` está configurado para denegar el acceso anónimo, el tiempo de ejecución de ASP.NET redirige automáticamente a la página de inicio de sesión, `Login.aspx` (consulte la figura 3). Cuando se redirige al usuario a la página de inicio de sesión, ASP.NET incluye un parámetro QueryString `ReturnUrl` que indica la página que el usuario estaba intentando ver. Después de iniciar sesión correctamente, el usuario puede volver a esta página.

[![usuarios no autorizados se redirigen automáticamente a la página de inicio de sesión](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figura 3**: los usuarios no autorizados se redirigen automáticamente a la página de inicio de sesión ([haga clic para ver la imagen a tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))

Ahora veamos cómo se comporta en producción. Implemente la aplicación y escriba la dirección URL directa a uno de los PDF de la carpeta `PrivateDocs` en producción. Esto le pedirá al explorador que envíe una solicitud de IIS para el archivo. Dado que se solicita un archivo estático, IIS recupera y devuelve el archivo sin invocar el tiempo de ejecución de ASP.NET. Como resultado, no se ha realizado ninguna comprobación de autorización de URL; el contenido del PDF supuestamente privado es accesible para cualquier persona que conozca la dirección URL directa del archivo.

[![usuarios anónimos pueden descargar los archivos PDF privados escribiendo la dirección URL directa del archivo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figura 4**: los usuarios anónimos pueden descargar los archivos PDF privados escribiendo la dirección URL directa del archivo ([haga clic para ver la imagen de tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))

### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Realizar la autenticación basada en formularios y la autenticación de direcciones URL en archivos estáticos con IIS 7

Hay un par de técnicas que puede usar para proteger el contenido estático de usuarios no autorizados. IIS 7 presentó la *canalización integrada*, que marries el flujo de trabajo de IIS con el flujo de trabajo del tiempo de ejecución de ASP.net. En pocas palabras, puede indicar a IIS que invoque los módulos de autenticación y autorización de ASP.NET en tiempo de ejecución todas las solicitudes entrantes (incluido el contenido estático, como los archivos PDF). Póngase en contacto con su proveedor de host web para averiguar cómo configurar su sitio web para usar la canalización integrada.

Una vez que IIS se ha configurado para usar la canalización integrada, agregue el siguiente marcado al archivo `Web.config` en el directorio raíz:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Este marcado indica a IIS 7 que use los módulos de autenticación y autorización basados en ASP.NET. Vuelva a implementar la aplicación y, a continuación, vuelva a visitar el archivo PDF. Esta vez, cuando IIS controla la solicitud, proporciona a la lógica de autenticación y autorización del tiempo de ejecución de ASP.NET una oportunidad para inspeccionar la solicitud. Dado que solo los usuarios autenticados tienen autorización para ver el contenido de la carpeta `PrivateDocs`, el visitante anónimo se redirige automáticamente a la página de inicio de sesión (consulte la figura 3).

> [!NOTE]
> Si el proveedor de host web todavía usa IIS 6, no puede usar la característica de canalización integrada. Una solución alternativa consiste en colocar los documentos privados en una carpeta que prohíba el acceso HTTP (como `App_Data`) y, a continuación, crear una página para prestar servicio a estos documentos. Se puede llamar a esta página `GetPDF.aspx`y se le pasa el nombre del PDF a través de un parámetro QueryString. En la página `GetPDF.aspx` se comprobaría primero que el usuario tiene permiso para ver el archivo y, en ese caso, usaría el método [`Response.WriteFile(filePath)`](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) para devolver el contenido del archivo PDF solicitado al cliente que realiza la solicitud. Esta técnica también funcionaría en IIS 7 si no desea habilitar la canalización integrada.

## <a name="summary"></a>Resumen

Las aplicaciones web en un entorno de producción se hospedan mediante el software del servidor Web de IIS de Microsoft. En el entorno de desarrollo, sin embargo, la aplicación se puede hospedar mediante IIS o el Servidor de desarrollo de ASP.NET. Idealmente, se debe usar el mismo software de servidor Web en ambos entornos, ya que el uso de software diferente agrega otra variable en la combinación. Sin embargo, la facilidad de uso de la Servidor de desarrollo de ASP.NET la convierte en una opción atractiva en el entorno de desarrollo. La buena noticia es que solo hay algunas diferencias fundamentales entre IIS y el Servidor de desarrollo de ASP.NET, y si conoce estas diferencias, puede tomar medidas para garantizar que la aplicación funciona y funciona de la misma manera, independientemente del entorno.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Integración de ASP.NET con IIS 7,0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Usar la autenticación de foros de ASP.net con todos los tipos de contenido en IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (vídeo)
- [Servidores Web en Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Anterior](common-configuration-differences-between-development-and-production-vb.md)
> [Siguiente](deploying-a-database-vb.md)
