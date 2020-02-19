---
uid: mvc/overview/performance/bundling-and-minification
title: Agrupar y Minificación | Microsoft Docs
author: Rick-Anderson
description: La agrupación y la minificación son dos técnicas que puede usar en ASP.NET 4,5 para mejorar el tiempo de carga de la solicitud. La agrupación y minificación mejora el tiempo de carga de reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 61bfe5dbac04b57e1461183b66ead2f01fe0734c
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457770"
---
# <a name="bundling-and-minification"></a>Unión y minificación

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> La agrupación y la minificación son dos técnicas que puede usar en ASP.NET 4,5 para mejorar el tiempo de carga de la solicitud. La agrupación y minificación mejora el tiempo de carga al reducir el número de solicitudes al servidor y reducir el tamaño de los activos solicitados (como CSS y JavaScript).

La mayoría de los exploradores principales actuales limitan el número de [conexiones simultáneas](http://www.browserscope.org/?category=network) por cada nombre de host a seis. Esto significa que, mientras se procesan seis solicitudes, el explorador pondrá en cola las solicitudes adicionales de recursos de un host. En la imagen siguiente, las pestañas red de herramientas de desarrollo F12 de IE muestran el tiempo necesario para los recursos que requiere la vista acerca de de una aplicación de ejemplo.

![B/M](bundling-and-minification/_static/image1.png)

Las barras grises muestran la hora en que el explorador pone en cola la solicitud a la espera del límite de seis conexiones. La barra amarilla es el tiempo de solicitud hasta el primer byte, es decir, el tiempo que se tarda en enviar la solicitud y recibir la primera respuesta del servidor. Las barras azules muestran el tiempo necesario para recibir los datos de respuesta del servidor. Puede hacer doble clic en un recurso para obtener información detallada sobre el control de tiempo. Por ejemplo, en la siguiente imagen se muestran los detalles de tiempo para cargar el archivo */scripts/MyScripts/JavaScript6.js* .

![](bundling-and-minification/_static/image2.png)

En la imagen anterior se muestra el evento de **Inicio** , que proporciona la hora a la que se puso en cola la solicitud debido a que el explorador limita el número de conexiones simultáneas. En este caso, la solicitud se puso en cola durante 46 milisegundos a la espera de que se completara otra solicitud.

## <a name="bundling"></a>Unión

La Unión es una nueva característica de ASP.NET 4,5 que facilita la combinación o agrupación de varios archivos en un único archivo. Puede crear CSS, JavaScript y otros paquetes. Menos archivos significa menos solicitudes HTTP y esto puede mejorar el rendimiento de la carga de la primera página.

En la imagen siguiente se muestra la misma vista de control de tiempo de la vista acerca de que se mostró anteriormente, pero esta vez con la agrupación y la minificación habilitada.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minificación

Minificación realiza una variedad de diferentes optimizaciones de código en scripts o CSS, como quitar los comentarios y los espacios en blanco innecesarios, y acortar los nombres de variable a un carácter. Considere la siguiente función de JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Después de minificación, la función se reduce a lo siguiente:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Además de quitar los comentarios y los espacios en blanco innecesarios, se ha cambiado el nombre de los siguientes parámetros y nombres de variable (abreviado) de la manera siguiente:

| **Original** | **Cambia** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impacto de la agrupación y Minificación

En la tabla siguiente se muestran varias diferencias importantes entre enumerar todos los recursos individualmente y usar la Unión y minificación (B/M) en el programa de ejemplo.

|  | **Usar B/M** | **Sin B/M** | **Cambio** |
| --- | --- | --- | --- |
| **Solicitudes de archivo** | 9 | 34 | 256% |
| **KB enviados** | 3,26 | 11.92 | 266% |
| **KB recibidos** | 388.51 | 530 | 36% |
| **Tiempo de carga** | 510 MS | 780 MS | 53% |

Los bytes enviados tuvieron una reducción significativa en la agrupación, ya que los exploradores son bastante detallados con los encabezados HTTP que se aplican en las solicitudes. La reducción de bytes recibidos no es tan grande porque los archivos más grandes (*scripts\\jQuery-UI-1.8.11. min. js* y *scripts\\jQuery-1.7.1. min. js*) ya se reducida. Nota: los intervalos en el programa de ejemplo usaban la herramienta [Fiddler](http://www.fiddler2.com/fiddler2/) para simular una red lenta. (En el menú **reglas** de Fiddler, seleccione **rendimiento** y, a continuación, **simular velocidades de módem**).

## <a name="debugging-bundled-and-minified-javascript"></a>Depuración de JavaScript agrupado y reducida

Es fácil depurar el código JavaScript en un entorno de desarrollo (donde el [elemento de compilación](https://msdn.microsoft.com/library/s10awwz0.aspx) en el archivo *Web. config* se establece en `debug="true"`) porque los archivos JavaScript no están agrupados ni reducida. También puede depurar una compilación de versión en la que los archivos de JavaScript estén agrupados y reducida. Con las herramientas de desarrollo F12 de IE, puede depurar una función de JavaScript incluida en un paquete de reducida con el siguiente enfoque:

1. Seleccione la pestaña **script** y, a continuación, seleccione el botón **iniciar depuración** .
2. Seleccione el paquete que contiene la función de JavaScript que desea depurar mediante el botón activos.  
    ![](bundling-and-minification/_static/image4.png)
3. Formatee el reducida JavaScript seleccionando el **botón configuración** ![](bundling-and-minification/_static/image5.png)y, a continuación, seleccionando **dar formato a JavaScript**.
4. En el cuadro de entrada **script de búsqueda** , seleccione el nombre de la función que desea depurar. En la imagen siguiente, se especificó **AddAltToImg** en el cuadro de entrada **script de búsqueda** .  
    ![](bundling-and-minification/_static/image6.png)

Para obtener más información sobre la depuración con las herramientas de desarrollo F12, vea el artículo de MSDN [sobre el uso del herramientas de desarrollo F12 para depurar errores de JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controlar la agrupación y Minificación

La agrupación y minificación está habilitada o deshabilitada estableciendo el valor del atributo debug en el [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) en el archivo *Web. config* . En el XML siguiente, `debug` se establece en true para que la agrupación y minificación esté deshabilitada.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Para habilitar la agrupación y la minificación, establezca el valor de `debug` en "false". Puede invalidar la configuración de *Web. config* con la propiedad `EnableOptimizations` en la clase `BundleTable`. En el código siguiente se habilita la agrupación y la minificación, y se reemplaza cualquier valor en el archivo *Web. config* .

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A menos que `EnableOptimizations` sea `true` o el atributo Debug del [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) del archivo *Web. config* esté establecido en `false`, los archivos no se agruparán ni se reducida. Además, no se usará la versión. min de los archivos, se seleccionarán las versiones de depuración completas. `EnableOptimizations` invalida el atributo debug en el [elemento Compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) del archivo *Web. config*

## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Usar la Unión y Minificación con formularios Web Forms y páginas web de ASP.NET

- En el caso de las páginas web, vea la entrada de blog [Agregar optimización web a un sitio de Web pages](https://docs.microsoft.com/archive/blogs/rickandy/adding-web-optimization-to-a-web-pages-site).
- Para los formularios Web Forms, vea la entrada de blog [Agregar agrupación y minificación a formularios Web Forms](https://docs.microsoft.com/archive/blogs/rickandy/adding-bundling-and-minification-to-web-forms).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Uso de la Unión y Minificación con ASP.NET MVC

En esta sección se creará un proyecto de ASP.NET MVC para examinar la Unión y la minificación. En primer lugar, cree un nuevo proyecto de Internet de ASP.NET MVC denominado **MvcBM** sin cambiar ninguno de los valores predeterminados.

Abra el *\\de la aplicación \_inicio\\archivo BundleConfig.CS* y examine el método `RegisterBundles` que se usa para crear, registrar y configurar agrupaciones. En el código siguiente se muestra una parte del método `RegisterBundles`.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

En el código anterior se crea una nueva agrupación de JavaScript denominada *~/bundles/jQuery* que incluye todo lo adecuado (es decir, Debug o reducida, pero no). *vsdoc*) en la carpeta *scripts* que coincide con la cadena comodín "~/scripts/jQuery-{version}.js". En el caso de ASP.NET MVC 4, esto significa que, con una configuración de depuración, el archivo *jQuery-1.7.1. js* se agregará a la agrupación. En una configuración de versión, se agregará *jQuery-1.7.1. min. js* . El marco de trabajo de agrupación sigue varias convenciones comunes, como:

- Selección de un archivo ". min" para el lanzamiento cuando *FileX. min. js* y *FileX. js* existan.
- Seleccionar la versión no ". min" para la depuración.
- Se omiten los archivos "-vsdoc" (como *jQuery-1.7.1-vsdoc. js*), que solo se usan en IntelliSense.

La `{version}` coincidencia de caracteres comodín mostrada anteriormente se usa para crear automáticamente un grupo de jQuery con la versión adecuada de jQuery en la carpeta *scripts* . En este ejemplo, el uso de un carácter comodín proporciona las siguientes ventajas:

- Permite usar NuGet para actualizar a una versión más reciente de jQuery sin cambiar el código de unión anterior o las referencias de jQuery en las páginas de vista.
- Selecciona automáticamente la versión completa de las configuraciones de depuración y la versión ". min" para las compilaciones de versión.

## <a name="using-a-cdn"></a>Usar una red CDN

 El código siguiente reemplaza el paquete de jQuery local por un paquete de jQuery de CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

En el código anterior, se solicitará jQuery desde la red CDN en modo de versión y la versión de depuración de jQuery se capturará localmente en modo de depuración. Cuando se usa una red CDN, debe tener un mecanismo de reserva en caso de que se produzca un error en la solicitud de CDN. El siguiente fragmento de marcado del final del archivo de diseño muestra un script agregado para solicitar jQuery en caso de que se produzca un error en la red CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Crear un paquete

La clase [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `Include` método toma una matriz de cadenas, donde cada cadena es una ruta de acceso virtual a un recurso. El siguiente código del método `RegisterBundles` del\\de la *aplicación \_inicio\\archivo BundleConfig.CS* muestra cómo se agregan varios archivos a un paquete:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

La clase [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` método se proporciona para agregar todos los archivos de un directorio (y, opcionalmente, todos los subdirectorios) que coinciden con un patrón de búsqueda. A continuación se muestra la clase de [paquete](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) `IncludeDirectory` API:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

En las vistas se hace referencia a las agrupaciones mediante el método Render (`Styles.Render` para CSS y `Scripts.Render` para JavaScript). El siguiente marcado de las *vistas\\Compartidos\\\_archivo layout. cshtml* muestra cómo las vistas de proyecto de internet de ASP.net predeterminadas hacen referencia a las agrupaciones CSS y JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Observe que los métodos de representación toman una matriz de cadenas, por lo que puede agregar varios paquetes en una línea de código. Por lo general, querrá usar los métodos de representación que crean el HTML necesario para hacer referencia al recurso. Puede usar el método `Url` para generar la dirección URL para el recurso sin el marcado necesario para hacer referencia al recurso. Supongamos que desea usar el nuevo atributo [Async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) de HTML5. En el código siguiente se muestra cómo hacer referencia a Modernizr con el método `Url`.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Usar el carácter comodín "\*" para seleccionar archivos

La ruta de acceso virtual especificada en el método `Include` y el patrón de búsqueda en el método `IncludeDirectory` puede aceptar un carácter comodín "\*" como prefijo o sufijo en el último segmento de ruta de acceso. La cadena de búsqueda no distingue mayúsculas de minúsculas. El método `IncludeDirectory` tiene la opción de buscar en subdirectorios.

Considere un proyecto con los siguientes archivos JavaScript:

- *Scripts\\Common\\AddAltToImg. js*
- *Scripts\\Common\\ToggleDiv. js*
- *Scripts\\Common\\ToggleImg. js*
- *Scripts\\Common\\Sub1\\ToggleLinks. js*

![DIR imag](bundling-and-minification/_static/image7.png)

En la tabla siguiente se muestran los archivos agregados a un paquete mediante el carácter comodín, como se muestra a continuación:

| **Call** | **Archivos agregados o excepción generados** |
| --- | --- |
| Include ("~/Scripts/Common/\*. js") | *AddAltToImg. js*, *ToggleDiv. js*, *ToggleImg. js* |
| Include ("~/Scripts/Common/T\*. js") | Excepción de patrón no válida. El carácter comodín solo se permite en el prefijo o sufijo. |
| Incluya ("~/Scripts/Common/\*OG.\*") | Excepción de patrón no válida. Solo se permite un carácter comodín. |
| Include ("~/Scripts/Common/T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| Include ("~/Scripts/Common/\*") | Excepción de patrón no válida. Un segmento de carácter comodín puro no es válido. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv. js*, *ToggleImg. js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv. js*, *ToggleImg. js*, *ToggleLinks. js* |

Agregar explícitamente cada archivo a una agrupación suele ser preferible a la carga de archivos con comodín por los siguientes motivos:

- La adición de scripts de forma predeterminada para cargarlos en orden alfabético, que normalmente no es lo que se desea. Con frecuencia, los archivos CSS y JavaScript deben agregarse en un orden específico (no alfabético). Puede mitigar este riesgo agregando una implementación de [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) personalizada, pero agregar explícitamente cada archivo es menos propenso a errores. Por ejemplo, podría agregar nuevos recursos a una carpeta en el futuro, lo que podría requerir que modifique la implementación de [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) .
- La visualización de archivos específicos agregados a un directorio mediante la carga de caracteres comodín puede incluirse en todas las vistas que hacen referencia a ese lote. Si el script específico de la vista se agrega a un paquete, es posible que obtenga un error de JavaScript en otras vistas que hagan referencia a la agrupación.
- Los archivos CSS que importan otros archivos hacen que los archivos importados se carguen dos veces. Por ejemplo, el código siguiente crea una agrupación con la mayoría de los archivos CSS de tema de la interfaz de usuario de jQuery cargados dos veces. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  El selector de carácter comodín "\*. css" incluye en cada archivo CSS de la carpeta, incluido el *contenido\\temas\\base\\jQuery. UI. All. CSS* File. El archivo *jQuery. UI. All. CSS* importa otros archivos CSS.

## <a name="bundle-caching"></a>Agrupación en caché

Agrupaciones establezca el encabezado Expires de HTTP un año a partir de la creación de la agrupación. Si navega a una página que se ha visto anteriormente, Fiddler muestra que IE no realiza una solicitud condicional para la agrupación, es decir, no hay solicitudes HTTP GET procedentes de IE para las agrupaciones y ninguna respuesta HTTP 304 del servidor. Puede forzar que IE realice una solicitud condicional para cada paquete con la tecla F5 (lo que da como resultado una respuesta HTTP 304 para cada paquete). Puede forzar una actualización completa mediante ^ F5 (lo que da como resultado una respuesta HTTP 200 para cada paquete).

En la imagen siguiente se muestra la pestaña **almacenamiento en caché** del panel de respuesta Fiddler:

![imagen de almacenamiento en caché de Fiddler](bundling-and-minification/_static/image8.png)

Solicitud   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 es para la agrupación **AllMyScripts** y contiene una cadena de consulta Pair **v = R0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La cadena de consulta **v** tiene un token de valor que es un identificador único que se usa para el almacenamiento en caché. Siempre que el paquete no cambie, la aplicación ASP.NET solicitará el paquete **AllMyScripts** con este token. Si cambia algún archivo de la agrupación, el marco de optimización de ASP.NET generará un nuevo token, lo que garantiza que las solicitudes del explorador para la agrupación obtendrán el paquete más reciente.

Si ejecuta las herramientas de desarrollo de IE9 F12 y navega a una página cargada anteriormente, IE muestra incorrectamente las solicitudes GET condicionales realizadas a cada agrupación y el servidor devuelve HTTP 304. Puede leer por qué IE9 tiene problemas para determinar si se realizó una solicitud condicional en la entrada de blog [mediante redes CDN y expira para mejorar el rendimiento del sitio web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS, CoffeeScript, SCSS y Sass.

El marco de trabajo de agrupación y minificación proporciona un mecanismo para procesar lenguajes intermedios como [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [less](http://www.dotlesscss.org/) o [CoffeeScript](http://coffeescript.org/)y aplicar transformaciones como minificación al grupo resultante. Por ejemplo, para agregar archivos [. Less](http://www.dotlesscss.org/) al proyecto de MVC 4:

1. Cree una carpeta para el menor contenido. En el ejemplo siguiente se usa el *contenido\\* carpeta.
2. Agregue **el paquete NuGet** [. Less](http://www.dotlesscss.org/) sin punto a su proyecto.  
    ![instalación sin punto de NuGet](bundling-and-minification/_static/image9.png)
3. Agregue una clase que implemente la interfaz [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) . Para la transformación. Less, agregue el código siguiente al proyecto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Cree una agrupación de menos archivos con la `LessTransform` y la transformación [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) . Agregue el código siguiente al método `RegisterBundles` en el archivo *App\\_Start\\BundleConfig.CS* .

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Agregue el código siguiente a cualquier vista que haga referencia a la agrupación LESS.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Consideraciones de agrupación

Una buena Convención que se debe seguir al crear paquetes es incluir "bundle" como prefijo en el nombre de la agrupación. Esto impedirá un posible [conflicto de enrutamiento](https://forums.asp.net/post/5012037.aspx).

Una vez que se actualiza un archivo en una agrupación, se genera un nuevo token para el parámetro de cadena de consulta de agrupación y se debe descargar el paquete completo la próxima vez que un cliente solicite una página que contenga la agrupación. En el marcado tradicional en el que cada activo se muestra individualmente, solo se descargaría el archivo modificado. Los recursos que cambian con frecuencia pueden no ser buenos candidatos para la agrupación.

La agrupación y minificación mejoran principalmente el tiempo de carga de la primera solicitud de página. Una vez que se ha solicitado una página web, el explorador almacena en caché los recursos (JavaScript, CSS e imágenes), por lo que la Unión y minificación no proporcionarán ningún aumento del rendimiento al solicitar la misma página o páginas en el mismo sitio que soliciten los mismos recursos. Si no establece el encabezado Expires correctamente en sus activos y no usa la agrupación y minificación, la heurística de actualización de exploradores marcará los recursos obsoletos después de unos días y el explorador requerirá una solicitud de validación para cada activo. En este caso, la agrupación y la minificación proporcionan un aumento del rendimiento después de la primera solicitud de la página. Para obtener más información, consulte el blog [uso de redes CDN y expira para mejorar el rendimiento del sitio web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitación del explorador de seis conexiones simultáneas por cada nombre de host se puede mitigar mediante una [red CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Dado que la red CDN tendrá un nombre de host diferente al del sitio de hospedaje, las solicitudes de recursos de la red CDN no contarán en el límite de seis conexiones simultáneas para el entorno de hospedaje. Una red CDN también puede proporcionar un almacenamiento en caché de paquetes común y las ventajas de almacenamiento en caché perimetral.

Las agrupaciones se deben particionar por páginas que las necesiten. Por ejemplo, la plantilla predeterminada de MVC de ASP.NET para una aplicación de Internet crea un paquete de validación de jQuery independiente de jQuery. Dado que las vistas predeterminadas creadas no tienen ninguna entrada y no envían valores, no incluyen el paquete de validación.

El espacio de nombres `System.Web.Optimization` se implementa en *System. Web. Optimization. dll*. Aprovecha la biblioteca webgrasa (*webgrasa. dll*) para las funcionalidades de minificación, que a su vez usa *Antlr3. Runtime. dll*.

*Utilizo Twitter para hacer entradas rápidas y compartir vínculos. Mi identificador de Twitter es*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Recursos adicionales

- Vídeo:[agrupar y optimizar](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) mediante [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Agregar optimización web a un sitio de Web pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Agregar agrupación y minificación a formularios Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicaciones de rendimiento de la agrupación y minificación en la exploración Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) mediante el [@frystyk](https://twitter.com/frystyk) de [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen)
- El [uso de redes CDN y expira para mejorar el rendimiento del sitio web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) por Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimizar RTT (tiempos de ida y vuelta)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Colaboradores

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
