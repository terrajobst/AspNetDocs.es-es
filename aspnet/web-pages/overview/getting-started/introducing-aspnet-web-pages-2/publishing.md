---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Presentación de ASP.NET Web Pages: publicación de un sitio mediante WebMatrix | Microsoft Docs'
author: Rick-Anderson
description: Este tutorial es la última instalación del conjunto de tutoriales que presenta ASP.NET Web Pages y Microsoft WebMatrix. Describe cómo publicar el sitio t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 49a841dbda183bf1d59153b83f694c9f517e0b94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78514387"
---
# <a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Presentación de ASP.NET Web Pages: publicación de un sitio mediante WebMatrix

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial es la última instalación del conjunto de tutoriales que presenta ASP.NET Web Pages y Microsoft WebMatrix. Describe cómo publicar su sitio en Internet para que otros usuarios puedan trabajar con él. Se supone que ha completado la serie a través [de la creación de un aspecto coherente para los sitios de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Aprenderá a publicar su sitio mediante:
> 
> - Microsoft Azure
> - Empresa de hospedaje Web

## <a name="about-publishing-your-site"></a>Acerca de la publicación de un sitio

Hasta ahora, ha hecho todo su trabajo en un equipo local, incluida la prueba de las páginas. Para ejecutar las páginas<em>. cshtml</em> , ha usado el servidor Web integrado en WebMatrix, es decir, IIS Express. Pero, por supuesto, nadie puede ver el sitio que ha creado excepto usted. Para que otros usuarios puedan trabajar con su sitio, debe publicarlo en Internet.

A menos que ya tenga acceso a un servidor web público, la publicación significa que debe tener una cuenta con una *plataforma en la nube* o un *proveedor de hospedaje*. Una plataforma en la nube, como Microsoft Azure, proporciona una infraestructura a petición para las aplicaciones. Un proveedor de hospedaje es una compañía que posee servidores Web accesibles públicamente y que le alquilará espacio para su sitio. Los planes de hospedaje se ejecutan desde unos dólares al mes (o incluso gratis) para sitios pequeños hasta muchos cientos de dólares al mes para sitios web comerciales de gran volumen.

> [!NOTE]
> Puede tener acceso a un servidor web público a través del proveedor de servicios Internet (ISP) que usa para obtener el servicio de Internet en casa. Sin embargo, el proveedor de hospedaje debe admitir ASP.NET Web Pages. Muchos ISP no, pero siempre merece la pena comprobar.

En este tutorial, proporcionaremos una visión general de cómo publicar. No es práctico proporcionar detalles exactos para todo, ya que el proceso difiere un poco en cada proveedor de hospedaje. Pero obtendrá una buena idea de cómo funciona el proceso.

Este tutorial contiene cuatro secciones:

1. [Configuración de la página predeterminada](#defaultpage)
2. Publicación (elija una de las siguientes opciones)  
 a. [Publicar el sitio en Microsoft Azure](#azure)  
 b. [Publicación de su sitio en una empresa de hospedaje Web](#host)
3. [Actualización del sitio activo: volver a publicar](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configuración de la página predeterminada

Cuando un usuario navega a la dirección base del sitio web, se muestra al usuario la página predeterminada del sitio. Por ejemplo, si *default. htm* se establece como la página predeterminada del sitio en `www.contoso.com`, desplazarse a `www.contoso.com` es igual que navegar a `www.contoso.com/Default.htm`.

Actualmente, el sitio usa **default. cshtml** como la página predeterminada. Esta página es correcta para la página predeterminada, pero en este tutorial no ha agregado ningún contenido a esa página para que muestre una página en blanco. Abra default. cshtml y reemplace el contenido por el código siguiente.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Ahora su sitio está listo para la publicación. En primer lugar, verá cómo implementar el sitio en Azure y cómo implementarlo en una empresa de hospedaje Web. Ambas opciones funcionan para el sitio web y solo tiene que seguir una de las opciones de implementación.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publicar el sitio en Microsoft Azure

En este tutorial se mostrará primero cómo implementar el sitio en Microsoft Azure. Al iniciar sesión con un cuenta de Microsoft, puede crear hasta 10 sitios gratuitos en Azure. Estos sitios gratuitos proporcionan una manera cómoda de probar los sitios. Siempre puede eliminar este sitio de ejemplo más adelante para evitar el uso de todos los sitios gratuitos. Puede crear una cuenta de evaluación gratuita en pocos minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

En la cinta WebMatrix, haga clic en el botón **publicar** .

![Botón ' publicar ' en la cinta WebMatrix](publishing/_static/image1.png)

Se muestra el cuadro de diálogo **publicar el sitio** . Si no ha iniciado sesión en el cuenta de Microsoft, el cuadro de diálogo contendrá un vínculo Introducción a **Azure** . Haga clic en este vínculo.

![Publicar el sitio](publishing/_static/image2.png)

Si no ha iniciado sesión en un cuenta de Microsoft, volverá a tener la oportunidad de iniciar sesión. Debe iniciar sesión en una cuenta de Microsoft para publicar el sitio en Azure.

![Iniciar sesión](publishing/_static/image3.png)

Después de iniciar sesión en el cuenta de Microsoft, el cuadro de diálogo contiene vínculos para crear un nuevo sitio en Azure o conectarse a uno de los sitios existentes en Azure.

![Crear nuevo sitio](publishing/_static/image4.png)

Seleccione **crear un nuevo sitio**.

Si ha llamado al proyecto **WebPagesMovies**, el nombre predeterminado para el sitio será **webpagesmovies.azurewebsites.net**. Lo más probable es que este nombre predeterminado no esté disponible, como se indica en el signo de exclamación rojo.

![nombre del sitio web predeterminado](publishing/_static/image5.png)

Cambie el nombre del sitio a algo que esté disponible y seleccione una ubicación cercana a su ubicación.

![nombre de sitio cambiado](publishing/_static/image6.png)

Haga clic en **Aceptar**.

WebMatrix proporciona una prueba para determinar si el servidor es compatible con el sitio.

![prueba de compatibilidad](publishing/_static/image7.png)

Seleccione **Continuar**.

Se muestran los resultados de la prueba de compatibilidad.

![resultado de la compatibilidad](publishing/_static/image8.png)

Seleccione **Continuar**.

WebMatrix muestra los archivos y las bases de datos que se van a publicar en el sitio. Como es la primera vez que publica el sitio, se muestran todos los archivos. Puede desproteger un archivo que no está listo para publicarse. En las publicaciones posteriores, solo se mostrarán los archivos que han cambiado. Vea [actualizar el sitio activo:](#update)volver a publicar.

![Publish Preview](publishing/_static/image9.png)

Seleccione **Continuar**.

Una vez que el sitio se ha implementado en Azure, se muestra un mensaje que indica que la implementación se ha completado.

![Publicación completa](publishing/_static/image10.png)

El sitio y la base de datos se han publicado en Azure y ahora están disponibles para el público. Haga clic en el vínculo del mensaje que indica que la publicación se ha completado y ahora verá el sitio implementado. Usted o cualquier persona con acceso a Internet puede Agregar o modificar registros en la base de datos.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publicación de su sitio en una empresa de hospedaje Web

Si decide no publicar en Azure, puede publicar su sitio en una empresa de hospedaje Web.

Haga clic en el vínculo **buscar hospedaje web** .

![Botón "buscar hospedaje web" del cuadro de diálogo Configuración de publicación](publishing/_static/image12.png)

Vaya a una página del sitio de Microsoft que muestra los proveedores de hospedaje que admiten ASP.NET.

![Página en el sitio de Microsoft que muestra los proveedores de hospedaje](publishing/_static/image13.png)

Obviamente, puede ser difícil saber exactamente qué características de hospedaje puede necesitar a largo plazo. Estos son algunos de los aspectos que se deben tener en cuenta:

- En el caso del sitio de WebPagesMovies, no es necesario tener un complemento independiente para SQL Server, lo que a menudo cuesta más. En su sitio, está usando SQL Server Compact Edition, que es independiente. Sin embargo, es posible que necesite SQL Server acceso para algún trabajo de sitio web futuro que realice. Si cree que podría, asegúrese de que puede Agregar SQL Server funcionalidad más adelante.
- Compruebe si el proveedor de hospedaje es compatible con el protocolo de publicación Web Deploy. Puede publicar mediante el protocolo FTP, pero es más conveniente utilizar Web Deploy.

Algunos sitios ofrecen un período de evaluación gratuita. Una evaluación gratuita es una buena manera de probar la publicación y el hospedaje mientras sigue experimentando WebMatrix y ASP.NET Web Pages.

Elija una que desee. En este tutorial, hemos seleccionado DiscountASP.NET, porque mientras creamos el tutorial, esa empresa tenía una promoción que permite a los usuarios hospedar un sitio gratis durante unos meses.

> [!NOTE]
> La elección de un proveedor de hospedaje para este tutorial no debe interpretarse como una aprobación de la empresa sobre otros. Pero tuvimos que elegir uno para la ilustración y DiscountASP.NET es una de las muchas empresas que admiten ASP.NET Web Pages y el protocolo de Web Deploy para la publicación.

Normalmente, una vez que se ha registrado con el proveedor de hospedaje, la empresa le envía un correo electrónico que contiene un nombre de usuario y una contraseña, la dirección URL del servidor Web, etc. Si la empresa de hospedaje admite Web Deploy protocolo, puede que le envíe un archivo que contenga la configuración de publicación o que le permita descargar uno. Un archivo de configuración de publicación simplifica el proceso.

Cuando se haya registrado y esté listo para publicar, haga clic en el botón **publicar** en la cinta WebMatrix. Se muestra el cuadro de diálogo **configuración de publicación** .

Si el proveedor de hospedaje le envió un archivo de configuración de publicación, haga clic en el vínculo **importar configuración de publicación** e importe el archivo. Si no tiene un archivo de configuración de publicación, rellene los campos con los valores que la empresa de hospedaje le envió por correo electrónico. Este es el aspecto que podría tener el cuadro de diálogo **configuración de publicación** cuando haya terminado:

![Configuración de publicación rellenada en el cuadro de diálogo "configuración de publicación"](publishing/_static/image14.png)

Haga clic en **validar conexión**. Si todo es correcto, el cuadro de diálogo informa de que se **ha conectado correctamente**, lo que significa que puede comunicarse con el servidor del proveedor de hospedaje.

![Mensaje de operación correcta si la configuración de publicación es correcta](publishing/_static/image15.png)

Si hay algún problema, WebMatrix hace lo mejor para indicarle cuál es el problema:

![Mensaje de error si hay un problema con la configuración de publicación](publishing/_static/image16.png)

Haga clic en **Guardar** para guardar la configuración. WebMatrix ofrece una prueba para asegurarse de que puede comunicarse correctamente con el sitio de hospedaje:

![Oferta de mensajes para realizar una prueba del proceso de publicación](publishing/_static/image17.png)

Haga clic en **Sí**. WebMatrix carga algunos archivos de ejemplo en el proveedor de hospedaje. Cuando se realiza la prueba de compatibilidad, WebMatrix informa de los resultados:

![Resultados de la prueba de publicación](publishing/_static/image18.png)

Si está listo, continúe y haga clic en **continuar** para iniciar el proceso de publicación para real. WebMatrix averigua qué archivos se encuentran en su sitio y ya están en el servidor host (ahora mismo, ninguno) y le proporciona una vista previa del proceso de publicación:

![Vista previa de los archivos que se cargarán en el proceso de publicación](publishing/_static/image19.png)

La lista de archivos que se van a publicar incluye las páginas web que ha creado como *movies. cshtml*. La lista también incluye archivos para las aplicaciones auxiliares que ha instalado, los archivos para ejecutar SQL Server Compact Edition para la base de datos, etc. Como resultado, el proceso de publicación inicial puede ser considerable.

Haga clic en **Continuar**. WebMatrix copia los archivos en el servidor del proveedor de hospedaje. Cuando haya terminado, los resultados se mostrarán en la barra de estado:

![Mensaje de la barra de estado cuando el proceso de publicación ha finalizado correctamente](publishing/_static/image20.png)

Para ver el sitio activo, haga clic en el vínculo de la barra de estado. Agregue *películas* a la dirección URL y verá el archivo *movies. cshtml* que creó:

![El sitio activo que muestra la página películas](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Actualización del sitio activo: volver a publicar

Una vez que haya publicado su sitio (en Azure o en una empresa de hospedaje web), hay dos copias de él &mdash; la versión del equipo y la versión del proveedor de servicios. Probablemente querrá seguir desarrollando el sitio (si no hay nada más, como parte del siguiente conjunto de tutoriales). Cuando lo haga, tendrá que volver a publicar el sitio para copiar los cambios del equipo en el proveedor de servicios. El proceso de publicación en WebMatrix puede determinar qué archivos han cambiado en el sitio y publicar solo esos archivos.

Para ver cómo funciona la republicación, abra el sitio de *movies. cshtml* , realice algunos cambios pequeños y, a continuación, guarde el archivo. Por ejemplo, cambie el título a `Movies - Updated`.

Haga clic en el botón **publicar** en la cinta de opciones. WebMatrix determina lo que ha cambiado y muestra una vista previa de los archivos que se van a publicar.

![Cuadro de diálogo "publicar" que muestra los archivos modificados listos para volver a publicar](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> De forma predeterminada, WebMatrix solo publica la base de datos (archivo *. sdf* ) la primera vez que publica el sitio. Una vez que se publica el sitio y los usuarios interactúan con el sitio web, la base de datos del sitio activo suele tener los datos reales del sitio. Tendrá que tener mucho cuidado de no sobrescribir la base de datos activa con el archivo *. sdf* que se encuentra en el equipo, que normalmente solo contiene datos de prueba. Por ese motivo, la publicación de advertencias **sobrescribirá las bases de datos remotas**y por qué la casilla de *WebPagesMovies. sdf* está desactivada de forma predeterminada.

Haga clic en **Continuar**. WebMatrix publica los archivos modificados y muestra un mensaje de operación correcta, como lo hacía la primera vez que publicó.

Vaya al sitio activo (puede hacer clic en el vínculo del mensaje de operación correcta si todavía se está mostrando) y comprobar que el cambio se ha publicado.

> [!TIP] 
> 
> **Editar archivos de forma remota**
> 
> Como alternativa a cambiar el sitio y volver a publicarlo, puede editar los archivos remotos directamente en WebMatrix. En este escenario, se abre un archivo que se encuentra en el proveedor de servicios y WebMatrix descarga una copia del mismo para que pueda editarlo. Cada vez que se guarda el archivo, WebMatrix envía los cambios al sitio.
> 
> La edición remota es una forma sencilla de realizar cambios en el sitio activo. Sin embargo, los cambios que realice de esta manera no se sincronizan con los archivos del sitio local. Para sincronizar los archivos locales con el sitio remoto, puede descargar los archivos remotos. Este proceso funciona de forma muy similar a la publicación, excepto en orden inverso.
> 
> Aquí no se describe más sobre las funciones de edición remota y descarga remota de WebMatrix. Son bastante útiles si varias personas tienen que trabajar en el mismo sitio en equipos diferentes. Para más información, consulte [publicación y edición de un sitio remoto con WebMatrix 2 beta](https://go.microsoft.com/fwlink/?LinkId=251591).

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.net WebMatrix ASP.NET Web pages Forum](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un excelente lugar para publicar preguntas y obtener respuestas.

> [!div class="step-by-step"]
> [Anterior](layouts.md)
