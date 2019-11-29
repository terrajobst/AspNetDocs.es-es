---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: Mostrar una página de error personalizadaC#() | Microsoft Docs
author: rick-anderson
description: ¿Qué ve el usuario cuando se produce un error de tiempo de ejecución en una aplicación Web de ASP.NET? La respuesta depende de cómo la configuración de&gt; de &lt;customErrors del sitio Web...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: c1ff4c112b9a489b8fb9ef3443663cd71eda7965
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625489"
---
# <a name="displaying-a-custom-error-page-c"></a>Mostrar una página de error personalizado (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> ¿Qué ve el usuario cuando se produce un error de tiempo de ejecución en una aplicación Web de ASP.NET? La respuesta depende de la configuración de&gt; de &lt;customErrors del sitio Web. De forma predeterminada, a los usuarios se les muestra una pantalla amarilla informativa de que se produce un error en tiempo de ejecución. En este tutorial se muestra cómo personalizar esta configuración para mostrar una página de error personalizada estéticamente agradable que coincida con la apariencia y el funcionamiento del sitio.

## <a name="introduction"></a>Introducción

En un mundo perfecto, no habrá errores en tiempo de ejecución. Los programadores escriben código con nary un error y con una validación de entrada de usuario sólida, y los recursos externos, como los servidores de base de datos y los servidores de correo electrónico, nunca se desconectan. Por supuesto, los errores de realidad son inevitables. Las clases de la .NET Framework señalan un error iniciando una excepción. Por ejemplo, al llamar al método Open de un objeto SqlConnection, se establece una conexión con la base de datos especificada por una cadena de conexión. Sin embargo, si la base de datos está inactiva o si las credenciales de la cadena de conexión no son válidas, el método Open produce una `SqlException`. Las excepciones se pueden controlar mediante el uso de bloques `try/catch/finally`. Si el código de un bloque `try` produce una excepción, el control se transfiere al bloque catch adecuado, en el que el desarrollador puede intentar recuperarse del error. Si no hay ningún bloque catch coincidente, o si el código que produjo la excepción no está en un bloque try, la excepción percolates la pila de llamadas en la búsqueda de bloques de `try/catch/finally`.

Si la excepción se propaga hasta el tiempo de ejecución de ASP.NET sin controlarse, se genera el [evento`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) de la [clase`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)y se muestra la *Página de error* configurada. De forma predeterminada, ASP.NET muestra una página de error affectionately a la que se hace referencia como la [pantalla amarilla de muerte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Hay dos versiones de YSOD: una muestra los detalles de la excepción, un seguimiento de la pila y otra información útil para los desarrolladores que depuran la aplicación (vea la **Ilustración 1**). el otro simplemente indica que se produjo un error en tiempo de ejecución (vea la **figura 2**).

Los detalles de la excepción YSOD son muy útiles para los desarrolladores que depuran la aplicación, pero que muestran un YSOD para los usuarios finales es una buena y no profesional. En su lugar, los usuarios finales deben ir a una página de error que mantenga la apariencia y el funcionamiento del sitio con más Prose descriptivos que describan la situación. La buena noticia es que es muy fácil crear una página de error personalizada. Este tutorial comienza con una mirada a ASP. Páginas de error diferentes de la red. A continuación, se muestra cómo configurar la aplicación web para mostrar a los usuarios una página de error personalizada en caso de error.

### <a name="examining-the-three-types-of-error-pages"></a>Examinar los tres tipos de páginas de error

Cuando se produce una excepción no controlada en una aplicación ASP.NET se muestra uno de los tres tipos de páginas de error:

- La pantalla de detalles de la excepción color amarillo de la página de errores de muerte,
- La pantalla amarilla del error en tiempo de ejecución de la página de error de muerte, o
- Página de error personalizada

La página de errores a la que están familiarizados los desarrolladores son los detalles de la excepción YSOD. De forma predeterminada, esta página se muestra a los usuarios que visitan localmente y, por lo tanto, son la página que se ve cuando se produce un error al probar el sitio en el entorno de desarrollo. Como su nombre implica, los detalles de la excepción YSOD proporcionan detalles sobre la excepción, el tipo, el mensaje y el seguimiento de la pila. Lo que es más, si la excepción la ha generado el código de la clase de código subyacente de la página de ASP.NET y si la aplicación está configurada para la depuración, los detalles de la excepción YSOD también mostrarán esta línea de código (y algunas líneas de código por encima y por debajo).

En la **figura 1** se muestra la página de detalles de la excepción YSOD. Observe la dirección URL en la ventana de dirección del explorador: `http://localhost:62275/Genre.aspx?ID=foo`. Recuerde que en la página `Genre.aspx` se enumeran las revisiones del libro en un género determinado. Requiere que se pase `GenreId` valor (un `uniqueidentifier`) a través de la cadena de acceso; por ejemplo, la dirección URL adecuada para ver las revisiones de ficción es `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Si se pasa un valor no`uniqueidentifier` a través de QueryString (como "foo"), se produce una excepción.

> [!NOTE]
> Para reproducir este error en la aplicación Web de demostración disponible para su descarga, puede visitar `Genre.aspx?ID=foo` directamente o hacer clic en el vínculo "generar un error de tiempo de ejecución" en `Default.aspx`.

Observe la información de la excepción que se muestra en la **figura 1**. El mensaje de excepción "error de conversión al convertir de una cadena de caracteres a uniqueidentifier" está presente en la parte superior de la página. También se muestra el tipo de la excepción, `System.Data.SqlClient.SqlException`. También hay el seguimiento de la pila.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Figura 1**: la YSOD de detalles de la excepción incluye información sobre la excepción  
 ([Haga clic para ver la imagen de tamaño completo](displaying-a-custom-error-page-cs/_static/image3.png))

El otro tipo de YSOD es el error de tiempo de ejecución YSOD y se muestra en la **figura 2**. El YSOD de error en tiempo de ejecución informa al visitante de que se ha producido un error en tiempo de ejecución, pero no incluye información sobre la excepción que se produjo. (Sin embargo, proporciona instrucciones sobre cómo ver los detalles de los errores mediante la modificación del archivo de `Web.config`, que forma parte de lo que hace que una apariencia de YSOD sea no profesional).

De forma predeterminada, se muestra el error de tiempo de ejecución YSOD a los usuarios que visitan de forma remota (a través de http://www.yoursite.com), tal y como prueba la dirección URL en la barra de direcciones del explorador en la **figura 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Existen dos pantallas de YSOD diferentes porque los desarrolladores están interesados en conocer los detalles del error, pero dicha información no debe mostrarse en un sitio activo, ya que puede revelar posibles vulnerabilidades de seguridad u otra información confidencial para cualquier persona que visite su visitante.

> [!NOTE]
> Si está siguiendo y usa DiscountASP.NET como su host Web, es posible que observe que el error de tiempo de ejecución YSOD no se muestra al visitar el sitio activo. Esto se debe a que DiscountASP.NET tiene sus servidores configurados para mostrar los detalles de la excepción YSOD de forma predeterminada. La buena noticia es que puede invalidar este comportamiento predeterminado agregando una `<customErrors>` sección al archivo de `Web.config`. En la sección "configuración de la página de error" se examina la sección `<customErrors>` en detalle.

[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**Figura 2**: el error de tiempo de ejecución YSOD no incluye detalles de error  
 ([Haga clic para ver la imagen de tamaño completo](displaying-a-custom-error-page-cs/_static/image6.png))

El tercer tipo de página de error es la página de error personalizada, que es una página web que se crea. La ventaja de una página de error personalizada es que tiene control total sobre la información que se muestra al usuario junto con la apariencia de la página. la página de error personalizada puede usar la misma página maestra y los mismos estilos que las demás páginas. La sección "uso de una página de error personalizada" le guía a través de la creación de una página de error personalizada y su configuración para que se muestre en el caso de una excepción no controlada. La **figura 3** ofrece un adelanto en esta página de error personalizada. Como puede ver, la apariencia de la página de error es mucho más profesional que una de las pantallas amarillas de muerte que se muestran en las figuras 1 y 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Figura 3**: una página de error personalizada ofrece una apariencia y funcionamiento más adaptados  
 ([Haga clic para ver la imagen de tamaño completo](displaying-a-custom-error-page-cs/_static/image9.png))

Dedique un momento a inspeccionar la barra de direcciones del explorador en la **figura 3**. Tenga en cuenta que la barra de direcciones muestra la dirección URL de la página de errores personalizada (`/ErrorPages/Oops.aspx`). En las figuras 1 y 2, las pantallas amarillas de muerte se muestran en la misma página en la que se originó el error (`Genre.aspx`). A la página de error personalizada se le pasa la dirección URL de la página donde se produjo el error a través de `aspxerrorpath` parámetro QueryString.

## <a name="configuring-which-error-page-is-displayed"></a>Configurar la página de error que se muestra

La visualización de las tres páginas de error posibles se basa en dos variables:

- La información de configuración en la sección `<customErrors>`, y
- Si el usuario visita el sitio de forma local o remota.

La [sección`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx) en `Web.config` tiene dos atributos que afectan a la página de error que se muestra: `defaultRedirect` y `mode`. El atributo `defaultRedirect` es opcional. Si se proporciona, especifica la dirección URL de la página de error personalizada e indica que se debe mostrar la página de error personalizada en lugar del error de tiempo de ejecución YSOD. El atributo `mode` es obligatorio y acepta uno de estos tres valores: `On`, `Off`o `RemoteOnly`. Estos valores tienen el siguiente comportamiento:

- `On`: indica que la página de error personalizada o el error de tiempo de ejecución YSOD se muestra a todos los visitantes, independientemente de si son locales o remotos.
- `Off`: especifica que el YSOD de detalles de la excepción se muestra a todos los visitantes, independientemente de si son locales o remotos.
- `RemoteOnly`: indica que se muestra la página de error personalizada o el error de tiempo de ejecución YSOD a los visitantes remotos, mientras que los detalles de la excepción YSOD se muestran a los visitantes locales.

A menos que especifique lo contrario, ASP.NET actúa como si hubiera establecido el atributo de modo en `RemoteOnly` y no hubiera especificado un valor de `defaultRedirect`. En otras palabras, el comportamiento predeterminado es que el YSOD de detalles de la excepción se muestra a los visitantes locales mientras se muestra el error de tiempo de ejecución YSOD a los visitantes remotos. Puede invalidar este comportamiento predeterminado agregando una sección `<customErrors>` a la `Web.config file.` de la aplicación Web.

## <a name="using-a-custom-error-page"></a>Uso de una página de error personalizada

Cada aplicación Web debe tener una página de error personalizada. Proporciona una alternativa más profesional al YSOD de errores en tiempo de ejecución, es fácil de crear y configurar la aplicación para que use la página de error personalizada solo tarda unos minutos. El primer paso es crear la página de error personalizada. He agregado una nueva carpeta a la aplicación de revisiones del libro denominada `ErrorPages` y agregada a una nueva página de ASP.NET denominada `Oops.aspx`. Haga que la página use la misma página maestra que el resto de las páginas del sitio para que herede automáticamente la misma apariencia y funcionamiento.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Figura 4**: creación de una página de error personalizada

A continuación, dedique unos minutos a crear el contenido de la página de error. He creado una página de error personalizada bastante sencilla con un mensaje que indica que se ha producido un error inesperado y un vínculo a la Página principal del sitio.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Figura 5**: diseño de la página de error personalizada  
 ([Haga clic para ver la imagen de tamaño completo](displaying-a-custom-error-page-cs/_static/image14.png))

Con la página de error completada, configure la aplicación web para que use la página de error personalizada en lugar del error de tiempo de ejecución YSOD. Esto se consigue especificando la dirección URL de la página de error en el atributo `defaultRedirect` de la sección `<customErrors>`. Agregue el marcado siguiente al archivo de `Web.config` de la aplicación:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

El marcado anterior configura la aplicación para mostrar los detalles de la excepción YSOD a los usuarios que visitan localmente, mientras se usa la página de error personalizada, vaya. aspx para los usuarios que visitan de forma remota. Para ver esto en acción, implemente el sitio web en el entorno de producción y, a continuación, visite la página Genre. aspx en el sitio activo con un valor de cadena de consulta no válido. Debería ver la página de error personalizada (consulte la **figura 3**).

Para comprobar que la página de error personalizada solo se muestra a los usuarios remotos, visite la página `Genre.aspx` con una cadena de consulta no válida desde el entorno de desarrollo. Todavía debería ver los detalles de la excepción YSOD (consulte la **figura 1**). El valor `RemoteOnly` garantiza que los usuarios que visitan el sitio en el entorno de producción ven la página de error personalizada mientras los desarrolladores que trabajan localmente continúan viendo los detalles de la excepción.

## <a name="notifying-developers-and-logging-error-details"></a>Notificar a los desarrolladores y registrar los detalles del error

Los errores que se producen en el entorno de desarrollo fueron causados por el desarrollador sentado en su equipo. Se muestra la información de la excepción en los detalles de la excepción YSOD y sabe qué pasos estaba realizando cuando se produjo el error. Pero cuando se produce un error en producción, el desarrollador no tiene conocimiento de que se ha producido un error a menos que el usuario final que visita el sitio lleve a cabo el tiempo necesario para notificar el error. Y, incluso si el usuario sale de su camino para avisar al equipo de desarrollo de que se ha producido un error, sin conocer el tipo de excepción, el mensaje y el seguimiento de la pila, puede ser difícil diagnosticar la causa del error, dejar que lo solucione por sí solo.

Por estas razones, es primordial que cualquier error en el entorno de producción se registre en algún almacén persistente (por ejemplo, una base de datos) y que los desarrolladores se les avise de este error. La página de error personalizada puede parecer un buen lugar para realizar este registro y notificación. Desafortunadamente, la página de error personalizada no tiene acceso a los detalles del error y, por lo tanto, no se puede usar para registrar esta información. La buena noticia es que hay varias formas de interceptar los detalles del error y de registrarlos, y los tres tutoriales siguientes exploran este tema con más detalle.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Usar diferentes páginas de error personalizadas para los distintos Estados de error HTTP

Cuando una página ASP.NET inicia una excepción y no se controla, la excepción percolates hasta el tiempo de ejecución de ASP.NET, que muestra la página de error configurada. Si una solicitud entra en el motor de ASP.NET pero no se puede procesar por alguna razón; quizás el archivo solicitado no se encuentra o se han deshabilitado los permisos de lectura para el archivo; a continuación, el motor de ASP.NET genera una `HttpException`. Esta excepción, al igual que las excepciones que se producen desde páginas ASP.NET, se propaga hasta el tiempo de ejecución, lo que provoca que se muestre la página de error adecuada.

Lo que esto significa para la aplicación web en producción es que si un usuario solicita una página que no se encuentra, verá la página de error personalizada. En la **Ilustración 6** se muestra un ejemplo de este tipo. Dado que la solicitud es para una página no existente (`NoSuchPage.aspx`), se produce una `HttpException` y se muestra la página de error personalizada (observe la referencia a `NoSuchPage.aspx` en el parámetro de cadena de consulta de `aspxerrorpath`).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Figura 6**: el tiempo de ejecución de ASP.net muestra la página de error configurada como respuesta a una solicitud no válida ([haga clic para ver la imagen de tamaño completo](displaying-a-custom-error-page-cs/_static/image17.png))

De forma predeterminada, todos los tipos de errores hacen que se muestre la misma página de error personalizada. Sin embargo, puede especificar una página de error personalizada diferente para un código de Estado HTTP específico mediante `<error>` elementos secundarios de la sección `<customErrors>`. Por ejemplo, para que se muestre una página de error diferente en caso de un error de página no encontrada, que tiene un código de Estado HTTP de 404, actualice la sección `<customErrors>` para incluir el marcado siguiente:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

Con este cambio, siempre que un usuario visite de forma remota un recurso ASP.NET que no existe, se le redirigirá a la página de error personalizada `404.aspx` en lugar de `Oops.aspx`. Como se muestra en la **figura 7** , la página `404.aspx` puede incluir un mensaje más específico que la página de error general personalizada.

> [!NOTE]
> Consulte [las páginas de error de 404, una vez más](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) para obtener instrucciones sobre cómo crear páginas de errores de 404 efectivas.

[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)

**Figura 7**: la página de Error personalizada 404 muestra un mensaje más dirigido que `Oops.aspx`  
([Haga clic para ver la imagen de tamaño completo](displaying-a-custom-error-page-cs/_static/image20.png)) 

Como sabe que la página de `404.aspx` solo se alcanza cuando el usuario realiza una solicitud de una página que no se ha encontrado, puede mejorar esta página de error personalizada para incluir la funcionalidad que ayude al usuario a resolver este tipo de error específico. Por ejemplo, podría crear una tabla de base de datos que asigne direcciones URL no válidas conocidas a direcciones URL correctas y, a continuación, hacer que la página de error personalizada de `404.aspx` ejecute una consulta en esa tabla y sugiera páginas a las que el usuario podría estar intentando llegar.

> [!NOTE]
> La página de error personalizada solo se muestra cuando se realiza una solicitud a un recurso administrado por el motor de ASP.NET. Como hemos comentado en las [principales diferencias entre IIS y el](core-differences-between-iis-and-the-asp-net-development-server-cs.md) tutorial de servidor de desarrollo de ASP.net, el servidor web puede controlar ciertas solicitudes. De forma predeterminada, el servidor Web de IIS procesa las solicitudes de contenido estático, como imágenes y archivos HTML, sin invocar el motor de ASP.NET. Por lo tanto, si el usuario solicita un archivo de imagen que no existe, recibirá el mensaje de error predeterminado 404 de IIS en lugar de ASP. Página de error configurada en la red.

## <a name="summary"></a>Resumen

Cuando se produce una excepción no controlada en una aplicación ASP.NET, se muestra al usuario una de las tres páginas de error: la pantalla de detalles de la excepción en amarillo; Error en tiempo de ejecución de la pantalla amarilla de muerte; o una página de error personalizada. La página de error que se muestra depende de la configuración de `<customErrors>` de la aplicación y de si el usuario está visitando localmente o de forma remota. El comportamiento predeterminado es mostrar los detalles de la excepción YSOD a los visitantes locales y el error de tiempo de ejecución YSOD a los visitantes remotos.

Aunque el YSOD de errores en tiempo de ejecución oculta información de errores potencialmente confidenciales del usuario que visita el sitio, se rompe de la apariencia y el funcionamiento del sitio y hace que la aplicación mire con errores. Un mejor enfoque consiste en usar una página de error personalizada, que implica la creación y el diseño de la página de error personalizada y la especificación de su dirección URL en el atributo `defaultRedirect` de la sección `<customErrors>`. Incluso puede tener varias páginas de error personalizadas para los distintos Estados de error HTTP.

La página de error personalizada es el primer paso en una estrategia integral de control de errores para un sitio web en producción. Avisar al desarrollador del error y registrar sus detalles también son pasos importantes. Los tres tutoriales siguientes exploran las técnicas de notificación de errores y registro.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Páginas de error, una vez más](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Instrucciones de diseño de excepciones](https://msdn.microsoft.com/library/ms229014.aspx)
- [Páginas de error fáciles de obtener](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Controlar y producir excepciones](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Usar correctamente páginas de error personalizadas en ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Anterior](strategies-for-database-development-and-deployment-cs.md)
> [Siguiente](processing-unhandled-exceptions-cs.md)
