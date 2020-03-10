---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introducción | Microsoft Docs
author: Rick-Anderson
description: WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use Visual Studio o Visual Studio Code. Esta guía...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: bb863f8605e6f8faca3b285607b63a3e88e83012
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440245"
---
# <a name="getting-started"></a>Introducción

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE[](~/includes/rp.md)]

> > [!NOTE] 
> > 
> > WebMatrix ya no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) o [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Esta guía y aplicación proporciona información general sobre ASP.NET Web Pages (versión 2 o posterior) y sintaxis Razor, un marco ligero para la creación de sitios web dinámicos. También incluye WebMatrix, una herramienta para crear páginas y sitios.
> 
> **Nivel**: nuevo en ASP.NET Web pages.  
> **Aptitudes asumidas**: HTML, hojas de estilos en cascada (CSS) básicas.
> 
> Lo que aprenderá en el primer tutorial del conjunto:
> 
> - Qué es ASP.NET Web Pages tecnología y para qué sirve.
> - Qué es WebMatrix.
> - Cómo instalar todo.
> - Cómo crear un sitio web mediante WebMatrix.
>   
> 
> Características y tecnologías descritas:
> 
> - Instalador de plataforma web de Microsoft.
> - WebMatrix.
> - páginas *. cshtml*
>   
> 
> Mike Pope escribió originalmente este tutorial. Tom FitzMacken lo actualizó para Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3

## <a name="what-should-you-know"></a>¿Qué debe saber?

Estamos asumiendo que está familiarizado con:

- **HTML**. No se requiere ningún conocimiento exhaustivo. No se explica HTML, pero tampoco usamos nada complejo. Proporcionaremos vínculos a tutoriales HTML en los que creemos que son útiles.
- **Hojas de estilos en cascada (CSS)** . Igual que con HTML.
- **Ideas básicas de bases de datos**. Si ha usado una hoja de cálculo para datos y ha ordenado y filtrado los datos, este es el nivel de conocimientos que se suele asumir para este conjunto de tutoriales.

También se da por hecho que está interesado en aprender la programación básica. ASP.NET Web Pages usar un lenguaje de programación C#denominado. No tiene que tener ningún fondo en la programación, sino solo un interés en él. Si alguna vez ha escrito JavaScript en una página web antes, tiene mucha información de fondo.

Tenga en cuenta que si está familiarizado con la programación, es posible que este tutorial se haya establecido inicialmente lentamente mientras ponemos a los nuevos programadores a la velocidad. A medida que se exponen los primeros tutoriales, sin embargo, habrá una programación menos básica para explicar y los elementos se moverán a lo largo de un clip más rápido.

## <a name="what-do-you-need"></a>¿Qué necesita?

Aquí está lo que necesita:

- Un equipo que ejecuta Windows 8, Windows 7, Windows Server 2008 o Windows Server 2012.
- Una conexión activa A Internet.
- Privilegios de administrador (necesario para el proceso de instalación).

## <a name="what-is-aspnet-web-pages"></a>¿Qué es ASP.NET Web Pages?

ASP.NET Web Pages es un marco de trabajo que puede usar para crear páginas web dinámicas. Una página web HTML simple es estática; su contenido viene determinado por el marcado HTML fijo que se encuentra en la página. Las páginas dinámicas como las creadas con ASP.NET Web Pages permiten crear el contenido de la página sobre la marcha, mediante el uso de código.

Las páginas dinámicas permiten realizar todo tipo de cosas. Puede solicitar a un usuario la entrada mediante un formulario y, a continuación, cambiar lo que se muestra en la página o su aspecto. Puede tomar información de un usuario, guardarla en una base de datos y, a continuación, enumerarla más adelante. Puede enviar correo electrónico desde su sitio. Puede interactuar con otros servicios en la web (por ejemplo, un servicio de asignación) y generar páginas que integren información de esos orígenes.

## <a name="what-is-webmatrix"></a>¿Qué es WebMatrix?

WebMatrix es una herramienta que integra un editor de páginas web, una utilidad de base de datos, un servidor web para probar páginas y características para publicar su sitio web en Internet. WebMatrix es gratuito y es fácil de instalar y fácil de usar. (También funciona para páginas HTML sin formato, así como para otras tecnologías como PHP).

En realidad, no *tiene* que usar WebMatrix para trabajar con ASP.NET Web pages. Puede crear páginas mediante un editor de texto, por ejemplo, y probar páginas mediante un servidor Web al que tenga acceso. Sin embargo, WebMatrix hace que sea muy fácil, por lo que estos tutoriales utilizarán WebMatrix a lo largo del proceso.

## <a name="about-these-tutorials"></a>Acerca de estos tutoriales

Este conjunto de tutoriales es una introducción al uso de ASP.NET Web Pages. En este conjunto de tutoriales de introducción hay 9 tutoriales en total. Forma parte de una serie de conjuntos de tutoriales que le lleva de ASP.NET Web Pages inexperto a crear sitios Web reales y de aspecto profesional.

Este primer tutorial se centra en mostrarle los aspectos básicos sobre cómo trabajar con ASP.NET Web Pages. Cuando haya terminado, puede trabajar con conjuntos de tutoriales adicionales que recogen dónde termina este y que exploran las páginas web con más detalle.

Nos facilitaremos deliberadamente las explicaciones detalladas. Y cada vez que se muestra algo, en este tutorial se establecerá siempre el modo en que creemos que es más fácil de entender. En el tutorial más adelante, profundizar en más profundidad y mostrar enfoques más eficientes o más flexibles (también más divertido). Sin embargo, estos tutoriales requieren que comprenda primero los conceptos básicos.

El conjunto de tutoriales que acaba de empezar cubre estas características de ASP.NET Web Pages:

- Introducción y obtención de todo instalado. (Esto es lo que se encuentra en el tutorial que está leyendo).
- Aspectos básicos de la programación de ASP.NET Web Pages.
- Crear una base de datos.
- Crear y procesar un formulario de entrada de usuario.
- Agregar, actualizar y eliminar datos en la base de datos.

## <a name="what-will-you-create"></a>¿Qué va a crear?

Este conjunto de tutoriales y los subsiguientes giran en torno a un sitio web en el que puede enumerar las películas que le gusten. Podrá escribir películas, editarlas y enumerarlas. Estas son algunas de las páginas que creará en este conjunto de tutoriales. La primera muestra la página de lista de películas que creará:

![Aplicación de película finalizado mostrando una lista de películas](getting-started/_static/image1.png)

Y esta es la página que le permite agregar nueva información de la película a su sitio:

![Aplicación de película finalizada que muestra la página agregar película](getting-started/_static/image2.png)

En el tutorial siguiente se establecen las compilaciones en este conjunto y se agrega más funcionalidad, como la carga de imágenes, la creación de un inicio de sesión, el envío de correo electrónico y la integración con redes sociales.

## <a name="see-this-app-running-on-azure"></a>Vea esta aplicación en ejecución en Azure

¿Desea ver el sitio terminado que se ejecuta como una aplicación Web activa? Puede implementar una versión completa de la aplicación en su cuenta de Azure simplemente haciendo clic en el botón siguiente.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Necesita una cuenta de Azure para implementar esta solución en Azure. Si aún no tiene una cuenta, tiene las siguientes opciones:

- [Abra una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.
- [Activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : su suscripción a MSDN le proporciona créditos todos los meses que puede usar para los servicios de Azure de pago.

## <a name="installing-everything"></a>Instalación de todo

Puede instalar todo mediante el instalador de plataforma web de Microsoft. En efecto, se instala el instalador y, a continuación, se usa para instalar todo lo demás.

Para usar páginas web, debe tener instalado como mínimo Windows XP con SP3, o Windows Server 2008 o posterior.

En la [Página páginas web](../../../index.md) del sitio web de ASP.net, haga clic en **instalar**.

![Sitio web de ASP.NET que muestra &quot;botón&quot; instalar WebMatrix](getting-started/_static/image3.png)

Se le pedirá que acepte los términos de licencia y la declaración de privacidad antes de instalar WebMatrix.

![aceptar término para comenzar la instalación](getting-started/_static/image4.png)

Haga clic en **Ejecutar** para iniciar la instalación. (Si desea guardar el instalador, haga clic en **Guardar** y, a continuación, ejecute el instalador desde la carpeta donde lo guardó).

![](getting-started/_static/image5.png)

Aparecerá el instalador de plataforma web, preparado para instalar WebMatrix. Haga clic en **Instalar**.

![](getting-started/_static/image6.png)

El proceso de instalación Averigua lo que tiene que instalar en el equipo e inicia el proceso. En función de lo que se deba instalar exactamente, el proceso puede tardar desde unos minutos hasta varios minutos. Seleccione Acepto para aceptar los **términos de licencia** .

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Cuando haya terminado, el proceso de instalación podrá iniciar WebMatrix automáticamente. Si no es así, en Windows, en el menú **Inicio** , inicie **Microsoft WebMatrix**.

Al iniciar WebMatrix por primera vez, se le ofrece la oportunidad de iniciar sesión en Microsoft Azure con el cuenta de Microsoft. Al iniciar sesión, recibirá 10 aplicaciones web gratuitas a través de Azure. Estas aplicaciones web gratuitas proporcionan una manera cómoda de probar sus aplicaciones. Si aún no tiene una cuenta de Azure, pero tiene una suscripción a MSDN, puede [activar las ventajas de la suscripción a MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). De lo contrario, puede crear una cuenta de evaluación gratuita en un par de minutos. Para obtener más información, consulte [Evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

No tiene que iniciar sesión en este momento para continuar con este tutorial. Si no inicia sesión ahora, tendrá la opción de iniciar sesión más tarde. En el último [tema](publishing.md) de esta serie de tutoriales se explica cómo implementar el sitio web en Azure. por lo tanto, debe iniciar sesión para completar el tema.

En este punto, inicie sesión con el cuenta de Microsoft o seleccione **no ahora** en la esquina inferior derecha.

![Iniciar sesión](getting-started/_static/image7.png)

Para empezar, creará un sitio web en blanco y agregará una página. En un tutorial posterior de este conjunto, reproducirá una de las plantillas de sitio web integradas.

En la ventana Inicio, haga clic en **nuevo**.

![Pantalla de inicio de WebMatrix](getting-started/_static/image8.png)

Las plantillas son archivos y páginas creados previamente para diferentes tipos de sitios Web. Para ver todas las plantillas que están disponibles de forma predeterminada, seleccione la opción de la galería de plantillas.

![Seleccionar la galería de plantillas](getting-started/_static/image9.png)

En la ventana de **Inicio rápido** , seleccione **sitio vacío** en el grupo **ASP.net** y asigne al nuevo sitio el nombre "WebPagesMovies".

![Ventana de Inicio rápido WebMatrix con la plantilla de sitio vacía seleccionada](getting-started/_static/image10.png)

Haga clic en **Siguiente**.

Si ha iniciado sesión en el cuenta de Microsoft, se le ofrecerá la oportunidad de crear el sitio en Azure. Según el nombre del sitio, se sugiere el nombre predeterminado de **WebPagesMovies.azurewebsites.net** ; sin embargo, el signo de exclamación indica que este nombre no está disponible en Windows Azure. Para simplificar, seleccione **omitir** para omitir la creación del sitio web en Azure en este momento. Más adelante en esta serie, publicaremos el sitio en Azure.

![crear sitio de Azure](getting-started/_static/image11.png)

WebMatrix crea y abre el sitio:

![Nuevo sitio de WebPagesMovies abierto en WebMatrix](getting-started/_static/image12.png)

En la parte superior, hay una barra de herramientas de acceso rápido y una cinta de opciones. En la parte inferior izquierda, verá el selector de área de trabajo en el que puede cambiar entre las tareas (**sitio**, **archivos**, **bases de datos**e **informes**). A la derecha está el panel de contenido del editor y de los informes. Y en la parte inferior, ocasionalmente verá una barra de notificación para los mensajes.

Aprenderá más sobre WebMatrix y sus características a medida que avance en estos tutoriales.

## <a name="creating-a-web-page"></a>Crear una página web

Para familiarizarse con WebMatrix y ASP.NET Web Pages, creará una página simple.

En el selector de área de trabajo, seleccione el área de trabajo **archivos** . Esta área de trabajo le permite trabajar con archivos y carpetas. En el panel izquierdo se muestra la estructura de archivos del sitio. La cinta de opciones cambia para mostrar las tareas relacionadas con archivos.

![Área de trabajo de archivo en WebMatrix](getting-started/_static/image13.png)

En la cinta de opciones, haga clic en la flecha situada debajo de **nuevo** y luego haga clic en **nuevo archivo**.

![Usar el comando &quot;New&quot; de la cinta de opciones para crear un nuevo archivo](getting-started/_static/image14.png)

WebMatrix muestra una lista de tipos de archivo. Seleccione **CSHTML**y, en el cuadro **nombre** , escriba "HelloWorld". Una página CSHTML es una página ASP.NET Web Pages.

![Creación de una nueva página CSHTML denominada HelloWorld. CSHTML](getting-started/_static/image15.png)

Haga clic en **Aceptar**.

WebMatrix crea la página y la abre en el editor.

![La nueva página HelloWorld en el editor de WebMatrix](getting-started/_static/image16.png)

Como puede ver, la página contiene principalmente marcado HTML normal, excepto un bloque en la parte superior que tiene este aspecto:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Eso es para agregar código, como veremos en breve.

Observe que las distintas partes de la página &mdash; los nombres de elemento, atributos y texto, además del bloque en la parte superior, están en colores diferentes. Esto se conoce como *resaltado de sintaxis*y facilita la tarea de mantener todo claro. Es una de las características que facilita el trabajo con páginas web en WebMatrix.

Agregue contenido para los elementos `<head>` y `<body>` como en el ejemplo siguiente. (Si lo desea, simplemente copie el siguiente bloque y reemplace toda la página existente con este código).

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

En la barra de herramientas de acceso rápido o en el menú **archivo** , haga clic en **Guardar**.

![Botón Guardar en la barra de herramientas de acceso rápido de WebMatrix](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Probar la página

En el área de trabajo **archivos** , haga clic con el botón secundario en la página *HelloWorld. cshtml* y, a continuación, haga clic en **iniciar en el explorador**.

![Ejecutar una página con el botón ejecutar de la cinta WebMatrix](getting-started/_static/image18.png)

WebMatrix inicia un servidor Web integrado (IIS Express) que puede usar para probar las páginas del equipo. (Sin IIS Express en WebMatrix, tendría que publicar la página en un servidor Web en algún lugar antes de probarlo). La página se muestra en el explorador predeterminado.

![&quot;página de&quot; de Hola mundo que se ejecuta en el explorador](getting-started/_static/image19.png)

Tenga en cuenta que al probar una página en WebMatrix, la dirección URL en el explorador es similar a `http://localhost:33651/HelloWorld.cshtml.` el nombre *localhost* hace referencia a un servidor local, lo que significa que la página es atendida por un servidor Web que se encuentra en su propio equipo. Como se indicó, WebMatrix incluye un programa de servidor Web denominado IIS Express que se ejecuta cuando se inicia una página.

El número después del *host local* (por ejemplo, *localhost: 33651*) hace referencia a un *número de Puerto* del equipo. Es el número del "canal" que IIS Express usa para este sitio web concreto. El número de puerto se selecciona aleatoriamente desde el intervalo de 1024 a 65536 al crear el sitio, y es diferente para cada sitio que cree. (Al probar su propio sitio, el número de Puerto casi seguro será un número distinto de 33561). Mediante el uso de un puerto diferente para cada sitio web, IIS Express puede mantenerse directo de los sitios a los que está hablando.

Más adelante, cuando publique el sitio en un servidor web público, ya no verá *localhost* en la dirección URL. En ese momento, verá una dirección URL más habitual, como `http://myhostingsite/mywebsite/HelloWorld.cshtml` o cualquier otra. Más adelante en esta serie de tutoriales aprenderá a publicar un sitio.

## <a name="adding-some-server-side-code"></a>Agregar código de servidor

Cierre el explorador y vuelva a la página de WebMatrix.

Agregue una línea al bloque de código para que tenga un aspecto similar al código siguiente:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Se trata de un poco de código Razor. Probablemente, es evidente que obtiene la fecha y hora actuales y coloca ese valor en una *variable* denominada `currentDateTime`. En el siguiente tutorial, obtendrá más información sobre sintaxis Razor.

En el cuerpo de la página, después del elemento `<p>Hello World!</p>`, agregue lo siguiente:

[!code-html[Main](getting-started/samples/sample4.html)]

Este código obtiene el valor que se coloca en la variable `currentDateTime` en la parte superior y lo inserta en el marcado de la página. El carácter `@` marca el código de ASP.NET Web Pages en la página.

Vuelva a ejecutar la página (WebMatrix guarda los cambios antes de que se ejecute la página). Esta vez, verá la fecha y la hora en la página.

![&quot;página de&quot; de Hola mundo que se ejecuta en el explorador con una presentación de tiempo generada dinámicamente](getting-started/_static/image20.png)

Espere unos instantes y, a continuación, actualice la página en el explorador. Se actualiza la presentación de fecha y hora.

En el explorador, examine el origen de la página. Tiene el siguiente formato:

[!code-html[Main](getting-started/samples/sample5.html)]

Observe que el bloque de `@{ }` en la parte superior no está ahí. Tenga en cuenta también que la presentación de fecha y hora muestra una cadena de caracteres real (`1/18/2012 2:49:50 PM` o cualquier otra), no `@currentDateTime` como tenía en la página *. cshtml* . Lo que sucedió aquí es que, cuando se ejecutó la página, ASP.NET procesó todo el código (muy poco en este caso) que se marcó con `@`. El código produce la salida y la salida se insertó en la página.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Este es el ASP.NET Web Pages

Al leer que ASP.NET Web Pages genera contenido Web dinámico, lo que ha visto aquí es la idea. La página que acaba de crear contiene el mismo código HTML que ha visto antes. También puede contener código que puede realizar todo tipo de tareas. En este ejemplo, hizo la tarea trivial de obtener la fecha y la hora actuales. Como vio, puede intercalar código con el HTML para generar la salida en la página. Cuando alguien solicita una página *. cshtml* en el explorador, ASP.net procesa la página mientras todavía está en manos del servidor Web. ASP.NET inserta la salida del código (si existe) en la página como HTML. Cuando se realiza el procesamiento del código, ASP.NET envía la página resultante al explorador. Todo el explorador obtiene el código HTML. Este es un diagrama:

![Flujo conceptual de cómo ASP.NET genera HTML dinámicamente](getting-started/_static/image21.png)

La idea es sencilla, pero hay muchas tareas interesantes que el código puede realizar, y hay muchas maneras interesantes en las que puede agregar dinámicamente contenido HTML a la página. Y las páginas de ASP.NET *. cshtml* , como cualquier página HTML, también pueden incluir código que se ejecuta en el propio explorador (código de JavaScript y jQuery). En este tutorial se explorarán todas estas cosas y en las subsiguientes.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial de esta serie, explorará ASP.NET Web Pages programación un poco más.

## <a name="additional-resources"></a>Recursos adicionales

[Cree un sitio web de ASP.net desde cero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Este es un tutorial que trata sobre el uso específico de WebMatrix (no ASP.NET Web Pages). Incluye un poco más detalles sobre algunas de las características adicionales de WebMatrix que no trataremos en este conjunto de tutoriales.

> [!div class="step-by-step"]
> [Siguiente](intro-to-web-pages-programming.md)
