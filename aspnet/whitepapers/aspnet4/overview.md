---
uid: whitepapers/aspnet4/overview
title: Información general sobre el desarrollo web de ASP.NET 4 y Visual Studio 2010 | Microsoft Docs
author: rick-anderson
description: En este documento se proporciona información general sobre muchas de las nuevas características de ASP.NET que se incluyen en the.NET Framework 4 y en Visual Studio 2010.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: ecde48f6bd88ee5f569bfeb8b70c26a50bc869c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511435"
---
# <a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Información general sobre la implementación web de ASP.NET 4 y Visual Studio 2010

> En este documento se proporciona información general sobre muchas de las nuevas características de ASP.NET que se incluyen en the.NET Framework 4 y en Visual Studio 2010.
> 
> [Descargue estas notas del producto](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)

**Contenido**

**[Servicios principales](#0.2__Toc253429238 "_Toc253429238")**  
[Refactorización del archivo Web. config](#0.2__Toc253429239 "_Toc253429239")  
[Almacenamiento en caché de resultados extensible](#0.2__Toc253429240 "_Toc253429240")  
[Iniciar automáticamente aplicaciones Web](#0.2__Toc253429241 "_Toc253429241")  
[Redireccionamiento permanente de una página](#0.2__Toc253429242 "_Toc253429242")  
[Reducir el estado de sesión](#0.2__Toc253429243 "_Toc253429243")  
[Expandir el intervalo de direcciones URL permitidas](#0.2__Toc253429244 "_Toc253429244")  
[Validación de solicitudes extensible](#0.2__Toc253429245 "_Toc253429245")  
[Extensibilidad de almacenamiento en caché de objetos y caché de objetos](#0.2__Toc253429246 "_Toc253429246")  
[Extensible HTML, URL y codificación de encabezado HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Supervisión del rendimiento de aplicaciones individuales en un único proceso de trabajo](#0.2__Toc253429248 "_Toc253429248")  
[Compatibilidad con múltiples versiones](#0.2__Toc253429249 "_Toc253429249")

**[Compatible](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery incluido con formularios Web Forms y MVC](#0.2__Toc253429251 "_Toc253429251")  
[Compatibilidad con Content Delivery Network](#0.2__Toc253429252 "_Toc253429252")  
[Scripts explícitos de ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Formularios Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Establecer etiquetas meta con las propiedades Page. MetaKeywords y Page. Description](#0.2__Toc253429257 "_Toc253429257")  
[Habilitar el estado de vista de controles individuales](#0.2__Toc253429258 "_Toc253429258")  
[Cambios en las capacidades del explorador](#0.2__Toc253429259 "_Toc253429259")  
[Enrutamiento en ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Establecer identificadores de cliente](#0.2__Toc253429261 "_Toc253429261")  
[Conservar la selección de fila en los controles de datos](#0.2__Toc253429262 "_Toc253429262")  
[Control chart de ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtrar datos con el control QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Expresiones de código HTML codificado](#0.2__Toc253429265 "_Toc253429265")  
[Cambios de plantilla de proyecto](#0.2__Toc253429266 "_Toc253429266")  
[Mejoras de CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ocultar los elementos div alrededor de los campos ocultos](#0.2__Toc253429268 "_Toc253429268")  
[Representar una tabla externa para controles con plantilla](#0.2__Toc253429269 "_Toc253429269")  
[Mejoras del control ListView](#0.2__Toc253429270 "_Toc253429270")  
[Mejoras en el control CheckBoxList y RadioButtonList](#0.2__Toc253429271 "_Toc253429271")  
[Mejoras en el control de menú](#0.2__Toc253429272 "_Toc253429272")  
[Controles Wizard y CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[MVC DE ASP.NET](#0.2__Toc253429274 "_Toc253429274")**  
[Compatibilidad con áreas](#0.2__Toc253429275 "_Toc253429275")  
[Compatibilidad con la validación de atributos de anotación de datos](#0.2__Toc253429276 "_Toc253429276")  
[Aplicaciones auxiliares con plantilla](#0.2__Toc253429277 "_Toc253429277")

**[datos dinámicos](#0.2__Toc253429278 "_Toc253429278")**  
[Habilitar datos dinámicos para proyectos existentes](#0.2__Toc253429279 "_Toc253429279")  
[Sintaxis de control declarativo de DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Plantillas de entidad](#0.2__Toc253429281 "_Toc253429281")  
[Nuevas plantillas de campo para direcciones URL y direcciones de correo electrónico](#0.2__Toc253429282 "_Toc253429282")  
[Crear vínculos con el control DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Compatibilidad con la herencia en el modelo de datos](#0.2__Toc253429284 "_Toc253429284")  
[Compatibilidad con relaciones de varios a varios (solo Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nuevos atributos para controlar la visualización y compatibilidad de enumeraciones](#0.2__Toc253429286 "_Toc253429286")  
[Compatibilidad mejorada con filtros](#0.2__Toc253429287 "_Toc253429287")

**[Mejoras en el desarrollo web de Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Compatibilidad mejorada con CSS](#0.2__Toc253429289 "_Toc253429289")  
[Fragmentos de código HTML y JavaScript](#0.2__Toc253429290 "_Toc253429290")  
[Mejoras de IntelliSense para JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Implementación de aplicaciones web con Visual Studio 2010](#0.2__Toc253429292 "_Toc253429292")**  
[Empaquetado Web](#0.2__Toc253429293 "_Toc253429293")  
[Transformación de Web. config](#0.2__Toc253429294 "_Toc253429294")  
[Implementación de base de datos](#0.2__Toc253429295 "_Toc253429295")  
[Publicación con un solo clic para aplicaciones Web](#0.2__Toc253429296 "_Toc253429296")  
[Recursos](#0.2__Toc253429297 "_Toc253429297")

**[Ausencia](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Servicios principales

ASP.NET 4 presenta una serie de características que mejoran los servicios principales de ASP.NET, como el almacenamiento en caché de resultados y el estado de sesión.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refactorización del archivo Web. config

El archivo `Web.config` que contiene la configuración de una aplicación web ha crecido considerablemente en las últimas versiones de la .NET Framework a medida que se han agregado nuevas características, como Ajax, enrutamiento e integración con IIS 7. Esto ha hecho más difícil configurar o iniciar nuevas aplicaciones web sin una herramienta como Visual Studio. En. .NET Framework 4, los elementos de configuración principales se han pasado al archivo `machine.config` y las aplicaciones heredan ahora esta configuración. Esto permite que el archivo de `Web.config` de las aplicaciones de ASP.NET 4 esté vacío o contenga solo las siguientes líneas, que especifican para Visual Studio qué versión del marco de trabajo tiene como destino la aplicación:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Almacenamiento en caché de resultados extensible

Desde el lanzamiento de ASP.NET 1,0, el almacenamiento en caché de resultados ha habilitado a los desarrolladores para que almacenen la salida generada de páginas, controles y respuestas HTTP en memoria. En las solicitudes web posteriores, ASP.NET puede servir contenido más rápidamente mediante la recuperación de la salida generada de la memoria en lugar de volver a generar la salida desde el principio. Sin embargo, este enfoque tiene una limitación: el contenido generado siempre tiene que almacenarse en la memoria y en los servidores que están experimentando mucho tráfico, la memoria consumida por el almacenamiento en caché de resultados puede competir con peticiones de memoria procedentes de otras partes de una aplicación Web.

ASP.NET 4 agrega un punto de extensibilidad al almacenamiento en caché de resultados que le permite configurar uno o varios proveedores de caché de resultados personalizados. Los proveedores de caché de resultados pueden utilizar cualquier mecanismo de almacenamiento para conservar el contenido HTML. Esto permite crear proveedores de caché de resultados personalizados para diversos mecanismos de persistencia, que pueden incluir discos locales o remotos, almacenamiento en la nube y motores de caché distribuida.

Se crea un proveedor de caché de resultados personalizado como una clase que se deriva del nuevo tipo *System. Web. Caching. OutputCacheProvider* . Después, puede configurar el proveedor en el archivo de `Web.config` mediante la subsección *proveedores* nuevos del elemento *OutputCache* , como se muestra en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample2.xml)]

De forma predeterminada en ASP.NET 4, todas las respuestas HTTP, páginas representadas y controles usan la memoria caché de resultados en memoria, tal como se muestra en el ejemplo anterior, donde el atributo *defaultProvider* se establece en AspNetInternalProvider. Puede cambiar el proveedor de caché de resultados predeterminado que se usa para una aplicación web especificando un nombre de proveedor diferente para *defaultProvider*.

Además, puede seleccionar diferentes proveedores de caché de resultados por control y por solicitud. La manera más fácil de elegir un proveedor de caché de resultados diferente para los diferentes controles de usuario web es hacerlo mediante declaración con el nuevo atributo *providerName* en una directiva de control, como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Especificar un proveedor de caché de resultados diferente para una solicitud HTTP requiere un poco más de trabajo. En lugar de especificar mediante declaración el proveedor, invalide el nuevo método *GetOuputCacheProviderName* en el archivo `Global.asax` para especificar mediante programación el proveedor que se va a usar para una solicitud concreta. En el ejemplo siguiente se muestra cómo hacerlo.

[!code-csharp[Main](overview/samples/sample4.cs)]

Con la incorporación de la extensibilidad del proveedor de caché de resultados a ASP.NET 4, ahora puede conseguir estrategias de almacenamiento en caché de resultados más agresivas y más inteligentes para los sitios Web. Por ejemplo, ahora es posible almacenar en caché las "10 principales" páginas de un sitio en memoria, mientras se almacenan en caché las páginas que obtienen menos tráfico en el disco. Como alternativa, puede almacenar en caché cada combinación de variación en una página representada, pero usar una caché distribuida para que el consumo de memoria se descargue de los servidores front-end web.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Iniciar automáticamente aplicaciones Web

Algunas aplicaciones Web necesitan cargar grandes cantidades de datos o realizar un procesamiento de inicialización costoso antes de atender la primera solicitud. En las versiones anteriores de ASP.NET, en estas situaciones tenía que diseñar enfoques personalizados para "reactivar" una aplicación ASP.NET y, a continuación, ejecutar el código de inicialización durante la *aplicación\_método Load* en el archivo `Global.asax`.

Una nueva característica de escalabilidad denominada *Inicio automático* que se dirige directamente a este escenario está disponible cuando ASP.net 4 se ejecuta en IIS 7,5 en Windows Server 2008 R2. La característica de inicio automático proporciona un enfoque controlado para iniciar un grupo de aplicaciones, inicializar una aplicación ASP.NET y, a continuación, aceptar solicitudes HTTP.

> [!NOTE] 
> 
> Módulo de preparación de aplicaciones de IIS para IIS 7,5
> 
> El equipo de IIS ha publicado la primera versión de prueba beta del módulo de preparación de la aplicación para IIS 7,5. Esto hace que la preparación de las aplicaciones sea aún más fácil que las descritas anteriormente. En lugar de escribir código personalizado, especifique las direcciones URL de los recursos que se ejecutarán antes de que la aplicación web acepte solicitudes de la red. Esta preparación se produce durante el inicio del servicio IIS (si configuró el grupo de aplicaciones de IIS como *AlwaysRunning*) y cuando se recicla un proceso de trabajo de IIS. Durante el reciclaje, el proceso de trabajo de IIS anterior continúa ejecutando solicitudes hasta que el proceso de trabajo recién generado esté totalmente preparado, de modo que las aplicaciones no experimenten ninguna interrupción u otros problemas debidos a memorias caché no previstas. Tenga en cuenta que este módulo funciona con cualquier versión de ASP.NET, a partir de la versión 2,0.
> 
> Para obtener más información, consulte preparación de la [aplicación](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) en el sitio Web de IIS.net. Para ver un tutorial que muestra cómo usar la característica de preparación, consulte [Introducción con el módulo de preparación de aplicaciones de IIS 7,5](https://www.iis.net/learn/manage) en el sitio Web de IIS.net.

Para usar la característica de inicio automático, un administrador de IIS establece un grupo de aplicaciones en IIS 7,5 para que se inicie automáticamente mediante la configuración siguiente en el archivo de `applicationHost.config`:

[!code-xml[Main](overview/samples/sample5.xml)]

Dado que un único grupo de aplicaciones puede contener varias aplicaciones, se especifica que las aplicaciones individuales se inicien automáticamente mediante la configuración siguiente en el archivo de `applicationHost.config`:

[!code-xml[Main](overview/samples/sample6.xml)]

Cuando un servidor de IIS 7,5 se inicia en frío o cuando se recicla un grupo de aplicaciones individual, IIS 7,5 usa la información del archivo `applicationHost.config` para determinar qué aplicaciones web deben iniciarse automáticamente. Para cada aplicación marcada para inicio automático, IIS 7.5 envía una solicitud a ASP.NET 4 para iniciar la aplicación en un estado en el que la aplicación no acepta temporalmente solicitudes HTTP. Cuando se encuentra en este estado, ASP.NET crea instancias del tipo definido por el atributo *serviceAutoStartProvider* (como se muestra en el ejemplo anterior) y llama a su punto de entrada público.

Para crear un tipo de inicio automático administrado con el punto de entrada necesario, implemente la interfaz *IProcessHostPreloadClient* , tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](overview/samples/sample7.cs)]

Una vez que el código de inicialización se ejecuta en el método *preload* y el método devuelve, la aplicación ASP.net está lista para procesar solicitudes.

Con la adición del inicio automático a IIS 5 y ASP.NET 4, ahora tiene un enfoque bien definido para realizar una inicialización de la aplicación costosa antes de procesar la primera solicitud HTTP. Por ejemplo, puede usar la nueva característica de inicio automático para inicializar una aplicación y, a continuación, indicar a un equilibrador de carga que la aplicación se ha inicializado y está lista para aceptar el tráfico HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redireccionamiento permanente de una página

Es una práctica común en las aplicaciones web mover páginas y otro contenido en torno a lo largo del tiempo, lo que puede conducir a una acumulación de vínculos obsoletos en motores de búsqueda. En ASP.NET, los desarrolladores han controlado tradicionalmente las solicitudes a las direcciones URL antiguas mediante el método *Response. Redirect* para reenviar una solicitud a la nueva dirección URL. Sin embargo, el método de *redireccionamiento* emite una respuesta HTTP 302 encontrada (redirección temporal), lo que da como resultado un viaje de ida y vuelta http adicional cuando los usuarios intentan obtener acceso a las direcciones URL anteriores.

ASP.NET 4 agrega un nuevo método auxiliar de *RedirectPermanent* que facilita la emisión de respuestas permanentes http 301, como en el ejemplo siguiente:

[!code-csharp[Main](overview/samples/sample8.cs)]

Los motores de búsqueda y otros agentes de usuario que reconozcan las redirecciones permanentes almacenarán la nueva dirección URL que está asociada con el contenido, lo que elimina el viaje de ida y vuelta innecesario realizado por el explorador para las redirecciones temporales.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Reducir el estado de sesión

ASP.NET proporciona dos opciones predeterminadas para almacenar el estado de sesión en una granja de servidores Web: un proveedor de estado de sesión que invoca un servidor de estado de sesión fuera de proceso y un proveedor de estado de sesión que almacena los datos en una base de datos de Microsoft SQL Server. Dado que ambas opciones implican el almacenamiento de la información de estado fuera del proceso de trabajo de una aplicación Web, el estado de sesión se debe serializar antes de enviarse al almacenamiento remoto. En función de la cantidad de información que un desarrollador guarde en el estado de sesión, el tamaño de los datos serializados puede aumentar de forma bastante grande.

ASP.NET 4 presenta una nueva opción de compresión para ambos tipos de proveedores de estado de sesión fuera de proceso. Cuando la opción de configuración *compressionEnabled* que se muestra en el ejemplo siguiente se establece en *true*, ASP.net comprimirá (y descomprime) el estado de sesión serializada mediante el .NET Framework clase *System. IO. Compression. GZipStream* .

[!code-xml[Main](overview/samples/sample9.xml)]

Con la simple adición del nuevo atributo al archivo `Web.config`, las aplicaciones con ciclos de CPU de reserva en servidores Web pueden obtener reducciones sustanciales en el tamaño de los datos serializados de estado de sesión.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Expandir el intervalo de direcciones URL permitidas

ASP.NET 4 presenta nuevas opciones para expandir el tamaño de las direcciones URL de la aplicación. Las versiones anteriores de ASP.NET limitaban la longitud de la ruta de acceso de la dirección URL a 260 caracteres, según el límite de la ruta de acceso de archivo NTFS. En ASP.NET 4, tiene la opción de aumentar (o disminuir) este límite según sea adecuado para las aplicaciones, mediante dos nuevos atributos de configuración de *httpRuntime* . En el ejemplo siguiente se muestran estos nuevos atributos.

[!code-xml[Main](overview/samples/sample10.xml)]

Para permitir rutas de acceso más largas o más cortas (la parte de la dirección URL que no incluye el protocolo, el nombre del servidor y la cadena de consulta), modifique el atributo *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* . Para permitir cadenas de consulta más largas o más cortas, modifique el valor del atributo *[maxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* .

ASP.NET 4 también le permite configurar los caracteres que usa la comprobación de caracteres de dirección URL. Cuando ASP.NET encuentra un carácter no válido en la parte de la ruta de acceso de una dirección URL, rechaza la solicitud y emite un error HTTP 400. En versiones anteriores de ASP.NET, las comprobaciones de caracteres de dirección URL se limitaban a un conjunto fijo de caracteres. En ASP.NET 4, puede personalizar el conjunto de caracteres válidos mediante el nuevo atributo *requestPathInvalidCharacters* del elemento de configuración *httpRuntime* , tal como se muestra en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample11.xml)]

De forma predeterminada, el atributo *requestPathInvalidCharacters* define ocho caracteres como no válidos. (En la cadena que se asigna a *requestPathInvalidCharacters* de forma predeterminada, los caracteres menor que (&lt;), mayor que (&gt;) y comercial (&amp;) se codifican, porque el archivo de `Web.config` es un archivo XML.) Puede personalizar el conjunto de caracteres no válidos según sea necesario.

> [!NOTE]
> Nota ASP.NET 4 siempre rechaza las rutas de dirección URL que contienen caracteres en el intervalo ASCII de 0x00 a 0x1F, ya que son caracteres de dirección URL no válidos, tal como se define en RFC 2396 de IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). En las versiones de Windows Server que ejecutan IIS 6 o versiones posteriores, el controlador de dispositivo de protocolo http. sys rechaza automáticamente las direcciones URL con estos caracteres.

<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validación de solicitudes extensible

La validación de solicitudes de ASP.NET busca los datos de la solicitud HTTP de entrada para las cadenas que se usan normalmente en los ataques de scripting entre sitios (XSS). Si se encuentran posibles cadenas XSS, la validación de la solicitud marca la cadena sospechosa y devuelve un error. La validación de la solicitud integrada solo devuelve un error cuando encuentra las cadenas más comunes que se usan en los ataques XSS. Los intentos anteriores de hacer que la validación XSS sea más agresiva dieron como resultado demasiados falsos positivos. Sin embargo, es posible que los clientes deseen que la validación de solicitudes sea más agresiva o que, por el contrario, quiera relajar intencionadamente las comprobaciones de XSS para páginas específicas o para tipos específicos de solicitudes.

En ASP.NET 4, la característica de validación de solicitudes se ha hecho extensible para que pueda usar la lógica de validación de solicitudes personalizada. Para extender la validación de solicitudes, cree una clase que derive del nuevo tipo *System. Web. util. RequestValidator* y configure la aplicación (en la sección *httpRuntime* del archivo `Web.config`) para usar el tipo personalizado. En el ejemplo siguiente se muestra cómo configurar una clase de validación de solicitud personalizada:

[!code-xml[Main](overview/samples/sample12.xml)]

El nuevo atributo *requestValidationType* requiere una cadena de identificador de tipo de .NET Framework estándar que especifique la clase que proporciona la validación de solicitudes personalizada. Para cada solicitud, ASP.NET invoca el tipo personalizado para procesar cada parte de los datos de la solicitud HTTP de entrada. La dirección URL de entrada, todos los encabezados HTTP (tanto las cookies como los encabezados personalizados) y el cuerpo de la entidad están disponibles para su inspección mediante una clase de validación de solicitud personalizada como la que se muestra en el ejemplo siguiente:

[!code-csharp[Main](overview/samples/sample13.cs)]

En los casos en los que no desea inspeccionar una parte de los datos HTTP entrantes, la clase solicitud-validación puede revertir para permitir la ejecución de la validación de la solicitud predeterminada de ASP.NET con solo llamar a *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Extensibilidad de almacenamiento en caché de objetos y caché de objetos

Desde su primera versión, ASP.NET ha incluido una eficaz memoria caché de objetos en memoria (*System. Web. Caching. cache*). La implementación de la memoria caché ha sido tan popular que se ha usado en aplicaciones que no son Web. Sin embargo, es extraño que una aplicación Windows Forms o WPF incluya una referencia a `System.Web.dll` solo para poder usar la memoria caché de objetos ASP.NET.

Para que el almacenamiento en caché esté disponible para todas las aplicaciones, el .NET Framework 4 introduce un nuevo ensamblado, un nuevo espacio de nombres, algunos tipos base y una implementación de almacenamiento en caché concreta. El nuevo ensamblado de `System.Runtime.Caching.dll` contiene una nueva API de almacenamiento en caché en el espacio de nombres *System. Runtime. Caching* . El espacio de nombres contiene dos conjuntos de clases principales:

- Tipos abstractos que proporcionan la base para compilar cualquier tipo de implementación de caché personalizada.
- Una implementación concreta de la memoria caché de objetos en memoria (la clase *System. Runtime. Caching. MemoryCache* ).

La nueva clase *MemoryCache* se modela estrechamente en la memoria caché de ASP.net y comparte gran parte de la lógica interna del motor de caché con ASP.net. Aunque las API de almacenamiento en caché públicas en *System. Runtime. Caching* se han actualizado para admitir el desarrollo de memorias caché personalizadas, si ha usado el objeto de *caché* ASP.net, encontrará conceptos conocidos en las nuevas API.

Una explicación detallada de la nueva clase *MemoryCache* y de las API base compatibles requeriría un documento completo. Sin embargo, en el ejemplo siguiente se ofrece una idea de cómo funciona la nueva API de caché. El ejemplo se ha escrito para una aplicación Windows Forms, sin ninguna dependencia en `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Extensible HTML, URL y codificación de encabezado HTTP

En ASP.NET 4, puede crear rutinas de codificación personalizadas para las siguientes tareas comunes de codificación de texto:

- Codificación HTML.
- Codificación de dirección URL.
- Codificación de atributos HTML.
- Codificar encabezados HTTP salientes.

Puede crear un codificador personalizado mediante la derivación del nuevo tipo *System. Web. util. HttpEncoder* y, a continuación, configurando ASP.net para usar el tipo personalizado en la sección *httpRuntime* del archivo `Web.config`, como se muestra en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample15.xml)]

Después de configurar un codificador personalizado, ASP.NET llama automáticamente a la implementación de la codificación personalizada cada vez que se llama a los métodos de codificación pública de las clases *System. Web. HttpUtility* o *System. Web. HttpServerUtility* . Esto permite que una parte de un equipo de desarrollo web cree un codificador personalizado que implemente una codificación agresiva de caracteres, mientras que el resto del equipo de desarrollo web sigue usando las API de codificación de ASP.NET públicas. Al configurar un codificador personalizado de forma centralizada en el elemento *httpRuntime* , se garantiza que todas las llamadas de codificación de texto de las API de codificación ASP.net pública se enrutan a través del codificador personalizado.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Supervisión del rendimiento de aplicaciones individuales en un único proceso de trabajo

Con el fin de aumentar el número de sitios web que se pueden hospedar en un solo servidor, muchos proveedores de hospedaje ejecutan varias aplicaciones de ASP.NET en un único proceso de trabajo. Sin embargo, si varias aplicaciones usan un único proceso de trabajo compartido, es difícil que los administradores del servidor identifiquen una aplicación individual que está experimentando problemas.

ASP.NET 4 aprovecha la nueva funcionalidad de supervisión de recursos introducida por CLR. Para habilitar esta funcionalidad, puede Agregar el siguiente fragmento de código XML al archivo de configuración de `aspnet.config`.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Tenga en cuenta que el archivo `aspnet.config` está en el directorio donde está instalado el .NET Framework. No es el archivo de `Web.config`.

Cuando se ha habilitado la característica *appDomainResourceMonitoring* , hay dos nuevos contadores de rendimiento disponibles en la categoría de rendimiento "aplicaciones ASP.net": *% de tiempo de procesador administrado* y *memoria administrada utilizada*. Estos dos contadores de rendimiento usan la nueva característica de administración de recursos de dominio de aplicación CLR para realizar el seguimiento del tiempo de CPU Estimado y el uso de memoria administrada de las aplicaciones ASP.NET individuales. Como resultado, con ASP.NET 4, los administradores ahora tienen una vista más detallada del consumo de recursos de las aplicaciones individuales que se ejecutan en un único proceso de trabajo.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Compatibilidad con múltiples versiones (multi-targeting)

Puede crear una aplicación que tenga como destino una versión específica de la .NET Framework. En ASP.NET 4, un nuevo atributo en el elemento *Compilation* del archivo `Web.config` le permite tener como destino el .NET Framework 4 y versiones posteriores. Si el destino es explícitamente el .NET Framework 4, y si incluye elementos opcionales en el archivo `Web.config` como las entradas de *System. CodeDom*, estos elementos deben ser correctos para el .NET Framework 4. (Si no tiene como destino explícita el .NET Framework 4, la versión de .NET Framework de destino se deduce de la falta de una entrada en el archivo de `Web.config`).

En el ejemplo siguiente se muestra el uso del atributo *targetFramework* en el elemento *Compilation* del archivo `Web.config`.

[!code-xml[Main](overview/samples/sample17.xml)]

Tenga en cuenta lo siguiente sobre cómo establecer como destino una versión específica de la .NET Framework:

- En un grupo de aplicaciones de .NET Framework 4, el sistema de compilación ASP.NET asume el .NET Framework 4 como destino si el archivo `Web.config` no incluye el atributo *targetFramework* o si falta el archivo de `Web.config`. (Es posible que tenga que realizar cambios de codificación en la aplicación para que se ejecute en el .NET Framework 4).
- Si incluye el atributo *targetFramework* y si el elemento *System. CodeDom* se define en el archivo `Web.config`, este archivo debe contener las entradas correctas para el .NET Framework 4.
- Si usa el comando *del compilador aspnet\_* para precompilar la aplicación (por ejemplo, en un entorno de compilación), debe usar la versión correcta del comando de *compilador ASPNET\_* para la plataforma de destino. Use el compilador que se incluye con el .NET Framework 2,0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) para compilar el .NET Framework 3,5 y versiones anteriores. Use el compilador que se incluye con el .NET Framework 4 para compilar aplicaciones creadas con ese marco o con versiones posteriores.
- En tiempo de ejecución, el compilador utiliza los ensamblados de .NET Framework más recientes que están instalados en el equipo (y, por tanto, en la GAC). Si se realiza una actualización más adelante en el marco (por ejemplo, se instala una versión hipotética 4,1), podrá usar las características de la versión más reciente del marco de trabajo aunque el atributo *targetFramework* tenga como destino una versión anterior (como 4,0). (Sin embargo, en tiempo de diseño en Visual Studio 2010 o cuando se usa el comando de *compilador aspnet\_* , el uso de las características más recientes del marco producirá errores del compilador).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery incluido con formularios Web Forms y MVC

Las plantillas de Visual Studio para formularios Web Forms y MVC incluyen la biblioteca jQuery de código abierto. Al crear un nuevo sitio web o proyecto, se crea una carpeta scripts que contiene los tres archivos siguientes:

- jQuery-1.4.1. js: la versión de unminified legible de la biblioteca de jQuery.
- jQuery-14.1. min. js: la versión reducida de la biblioteca jQuery.
- jQuery-1.4.1-vsdoc. js: el archivo de documentación de IntelliSense para la biblioteca de jQuery.

Incluya la versión unminified de jQuery al desarrollar una aplicación. Incluya la versión reducida de jQuery para las aplicaciones de producción.

Por ejemplo, en la página de formularios Web Forms siguiente se muestra cómo puede usar jQuery para cambiar el color de fondo de los controles de cuadro de texto ASP.NET a amarillo cuando tienen el foco.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Compatibilidad con Content Delivery Network

Microsoft Ajax Content Delivery Network (CDN) le permite agregar fácilmente scripts de ASP.NET AJAX y jQuery a las aplicaciones Web. Por ejemplo, puede empezar a usar la biblioteca de jQuery simplemente agregando una `<script>` etiqueta a la página que apunte a Ajax.microsoft.com como se indica a continuación:

[!code-html[Main](overview/samples/sample19.html)]

Aprovechando CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento de las aplicaciones Ajax. El contenido de la red CDN de Microsoft Ajax se almacena en caché en los servidores ubicados en todo el mundo. Además, CDN de Microsoft Ajax permite que los exploradores reutilicen los archivos JavaScript almacenados en memoria caché para los sitios web que se encuentran en dominios diferentes.

Microsoft Ajax Content Delivery Network admite SSL (HTTPS) en caso de que tenga que servir una página web con el Capa de sockets seguros.

Implementar una reserva cuando la red CDN no está disponible. Pruebe la reserva.

Para obtener más información acerca de la red CDN de Microsoft Ajax, visite el siguiente sitio web:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

El ScriptManager ASP.NET es compatible con la red CDN de Microsoft Ajax. Simplemente estableciendo una propiedad, la propiedad EnableCdn, puede recuperar todos los archivos JavaScript de ASP.NET Framework desde la red CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Después de establecer la propiedad EnableCdn en el valor true, el marco de trabajo de ASP.NET recuperará todos los archivos JavaScript de ASP.NET Framework de la red CDN, incluidos todos los archivos JavaScript que se usan para la validación y UpdatePanel. Establecer esta propiedad puede tener un impacto drástico en el rendimiento de la aplicación Web.

Puede establecer la ruta de acceso de la red CDN para sus propios archivos JavaScript mediante el atributo WebResource. La nueva propiedad CdnPath especifica la ruta de acceso a la red CDN que se usa al establecer la propiedad EnableCdn en el valor true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scripts explícitos de ScriptManager

En el pasado, si usaba el ScriptManger de ASP.NET, era necesario cargar toda la biblioteca de Ajax ASP.NET de monolítica. Al aprovechar la nueva propiedad ScriptManager. AjaxFrameworkMode, puede controlar exactamente qué componentes de la biblioteca ASP.NET AJAX se cargan y cargar solo los componentes de la biblioteca de Ajax ASP.NET que necesite.

La propiedad ScriptManager. AjaxFrameworkMode se puede establecer en los valores siguientes:

- Habilitado: especifica que el control ScriptManager incluye automáticamente el archivo de script MicrosoftAjax. js, que es un archivo de script combinado de todos los scripts principales de Framework (comportamiento heredado).
- Deshabilitado: especifica que todas las características de script de Microsoft Ajax están deshabilitadas y que el control ScriptManager no hace referencia a ningún script automáticamente.
- Explicit: especifica que se incluirán explícitamente las referencias de script a un archivo de script básico de .NET Framework que la página requiera y que se incluirán las referencias a las dependencias que cada archivo de script requiere.

Por ejemplo, si establece la propiedad AjaxFrameworkMode en el valor Explicit, puede especificar los scripts de componentes AJAX de ASP.NET concretos que necesita:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>formularios Web Forms

Los formularios Web Forms han sido una característica principal de ASP.NET desde la versión de ASP.NET 1,0. En esta área se han realizado muchas mejoras en ASP.NET 4, entre las que se incluyen las siguientes:

- La capacidad de establecer etiquetas *meta* .
- Más control sobre el estado de vista.
- Formas más sencillas de trabajar con las funciones del explorador.
- Compatibilidad con el uso del enrutamiento ASP.NET con formularios Web Forms.
- Más control sobre los identificadores generados.
- La capacidad de conservar las filas seleccionadas en los controles de datos.
- Mayor control sobre el HTML representado en los controles *FormView* y *ListView* .
- Compatibilidad de filtrado con controles de origen de datos.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Establecer etiquetas meta con las propiedades Page. MetaKeywords y Page. Description

ASP.net 4 agrega dos propiedades a la clase *Page* , *MetaKeywords* y *Description*. Estas dos propiedades representan las etiquetas *meta* correspondientes en la página, tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Estas dos propiedades funcionan de la misma forma que la propiedad *title* de la página. Siguen estas reglas:

1. Si no hay etiquetas *meta* en el elemento de *encabezado* que coincidan con los nombres de propiedad (es decir, name = "keywords" para *Page. MetaKeywords* y name = "Description" para *Page. Description*, lo que significa que no se han establecido estas propiedades), las etiquetas *meta* se agregarán a la página cuando se represente.
2. Si ya hay etiquetas *meta* con estos nombres, estas propiedades actúan como métodos GET y set para el contenido de las etiquetas existentes.

Puede establecer estas propiedades en tiempo de ejecución, lo que le permite obtener el contenido de una base de datos o de otro origen, y que le permite establecer las etiquetas dinámicamente para describir para qué sirve una página determinada.

También puede establecer las propiedades *Keywords* y *Description* en la directiva *@ Page* en la parte superior del marcado de la página de formularios Web Forms, como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Esto invalidará el contenido de la etiqueta *meta* (si existe) ya declarado en la página.

El contenido de la etiqueta *meta* de descripción se usa para mejorar las vistas previas de las listas de búsqueda en Google. (Para obtener más información, vea [mejorar fragmentos de código con una meta Description Makeover](https://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) en el blog central de Google Webmaster). Google y Windows Live Search no usan el contenido de las palabras clave para nada, pero podrían ser otros motores de búsqueda. Para obtener más información, consulte [consejos sobre palabras clave meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) en el sitio web de la guía del motor de búsqueda.

Estas nuevas propiedades son una característica sencilla, pero le ahorran de la necesidad de agregarlas manualmente o de escribir su propio código para crear las etiquetas *meta* .

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Habilitar el estado de vista de controles individuales

De forma predeterminada, el estado de vista está habilitado para la página, con el resultado de que cada control de la página puede almacenar el estado de vista incluso si no es necesario para la aplicación. Los datos de estado de vista se incluyen en el marcado que genera una página y aumenta la cantidad de tiempo que se tarda en enviar una página al cliente y volver a publicarla. Almacenar más Estados de vista de los necesarios puede causar una degradación significativa del rendimiento. En versiones anteriores de ASP.NET, los desarrolladores podían deshabilitar el estado de vista de los controles individuales para reducir el tamaño de página, pero tenía que hacerlo explícitamente para los controles individuales. En ASP.NET 4, los controles de servidor Web incluyen una propiedad *ViewStateMode* que permite deshabilitar el estado de vista de forma predeterminada y, a continuación, habilitarlo solo para los controles que lo requieran en la página.

La propiedad *ViewStateMode* toma una enumeración que tiene tres valores: *Enabled*, *Disabled*y *inherit*. *Enabled* habilita el estado de vista para ese control y para todos los controles secundarios que se establecen para *heredar* o que no tienen nada establecido. *Deshabilitado* deshabilita el estado de vista y *inherit* especifica que el control usa la configuración *ViewStateMode* del control primario.

En el ejemplo siguiente se muestra cómo funciona la propiedad *ViewStateMode* . El marcado y el código de los controles de la página siguiente incluyen valores para la propiedad *ViewStateMode* :

[!code-aspx[Main](overview/samples/sample25.aspx)]

Como puede ver, el código deshabilita el estado de vista del control PlaceHolder1. El control Label1 secundario hereda este valor de propiedad (*inherit* es el valor predeterminado de *ViewStateMode* para los controles) y, por tanto, no guarda ningún estado de vista. En el control PlaceHolder2, *ViewStateMode* se establece en *Enabled*, por lo que Label2 hereda esta propiedad y guarda el estado de vista. Cuando se carga la página por primera vez, la propiedad *texto* de ambos controles *etiqueta* se establece en la cadena "[DynamicValue]".

El efecto de esta configuración es que, cuando se carga la página por primera vez, se muestra el siguiente resultado en el explorador:

Deshabilitado `: [DynamicValue]`

Habilitado:`[DynamicValue]`

Sin embargo, después de un postback, se muestra el siguiente resultado:

Deshabilitado `: [DeclaredValue]`

Habilitado:`[DynamicValue]`

El control Label1 (cuyo valor *ViewStateMode* se establece en *Disabled*) no ha conservado el valor en el que se estableció en el código. Sin embargo, el control Label2 (cuyo valor *ViewStateMode* se establece en *Enabled*) ha conservado su estado.

También puede establecer *ViewStateMode* en la directiva *@ Page* , como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample26.aspx)]

La clase *Page* es simplemente otro control; actúa como control primario de todos los demás controles de la página. El valor predeterminado de *ViewStateMode* está *habilitado* para las instancias de *Page*. Dado que los controles se *heredan*de forma predeterminada, los controles heredarán el valor de propiedad *habilitado* a menos que establezca *ViewStateMode* en el nivel de página o de control.

El valor de la propiedad *ViewStateMode* determina si el estado de vista se mantiene solo si la propiedad *EnableViewState* está establecida en *true*. Si la propiedad *EnableViewState* está establecida en *false*, el estado de vista no se mantendrá aunque *ViewStateMode* esté establecido en *habilitado*.

Un buen uso para esta característica es con controles de *ContentPlaceHolder* en páginas maestras, donde puede establecer *ViewStateMode* en *deshabilitado* para la página maestra y, a continuación, habilitarla individualmente para los controles de *ContentPlaceHolder* que a su vez contienen controles que requieren el estado de vista.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Cambios en las capacidades del explorador

ASP.NET determina las capacidades del explorador que utiliza un usuario para examinar el sitio mediante una característica denominada *funciones del explorador*. Las funciones del explorador se representan mediante el objeto *HttpBrowserCapabilities* (expuesto por la propiedad *request. Browser* ). Por ejemplo, puede utilizar el objeto *HttpBrowserCapabilities* para determinar si el tipo y la versión del explorador actual admiten una versión concreta de JavaScript. O bien, puede usar el objeto *HttpBrowserCapabilities* para determinar si la solicitud se originó en un dispositivo móvil.

El objeto *HttpBrowserCapabilities* está controlado por un conjunto de archivos de definición del explorador. Estos archivos contienen información sobre las capacidades de exploradores concretos. En ASP.NET 4, estos archivos de definición del explorador se han actualizado para contener información acerca de los exploradores y dispositivos incorporados recientemente, como Google Chrome, la investigación en smartphones de Motion BlackBerry y Apple iPhone.

En la lista siguiente se muestran los nuevos archivos de definición del explorador:

- *BlackBerry. Browser*
- *Chrome. Browser*
- *Default. Browser*
- *Firefox. Browser*
- *puerta de enlace. Browser*
- *Generic. Browser*
- *IE. Browser*
- *Iemobile. Browser*
- *iPhone. Browser*
- *Opera. Browser*
- *Safari. Browser*

#### <a name="using-browser-capabilities-providers"></a>Usar proveedores de funciones del explorador

En la versión 3,5 de ASP.NET Service Pack 1, puede definir las capacidades que tiene un explorador de las siguientes maneras:

- En el nivel de equipo, se crea o se actualiza un archivo XML `.browser` en la siguiente carpeta:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Después de definir la capacidad del explorador, ejecute el siguiente comando desde el símbolo del sistema de Visual Studio para volver a generar el ensamblado de capacidades del explorador y agregarlo a la GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- En el caso de una aplicación individual, se crea un archivo de `.browser` en la carpeta de `App_Browsers` de la aplicación.

Estos enfoques requieren la modificación de los archivos XML y, para los cambios en el nivel de equipo, se debe reiniciar la aplicación después de ejecutar el proceso ASPNET\_regbrowsers. exe.

ASP.NET 4 incluye una característica denominada proveedores de *funciones del explorador*. Como sugiere su nombre, esto le permite compilar un proveedor que, a su vez, le permite usar su propio código para determinar las capacidades del explorador.

En la práctica, los desarrolladores no suelen definir funciones personalizadas del explorador. Los archivos del explorador son difíciles de actualizar, el proceso para actualizarlos es bastante complicado y la sintaxis XML para archivos `.browser` puede ser compleja de usar y definir. Lo que haría que este proceso sea mucho más fácil es si hubiera una sintaxis de definición de explorador común, o una base de datos que contuviera definiciones de explorador actualizadas, o incluso un servicio web para dicha base de datos. La nueva característica de proveedores de capacidades del explorador hace que estos escenarios sean posibles y prácticos para desarrolladores de terceros.

Hay dos enfoques principales para usar la nueva característica de proveedor de funciones del explorador de ASP.NET 4: extender la funcionalidad de definición del explorador ASP.NET o reemplazarla por completo. En las secciones siguientes se describe primero cómo reemplazar la funcionalidad y cómo ampliarla.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Reemplazar la funcionalidad de las funciones del explorador de ASP.NET

Para reemplazar por completo la funcionalidad de definición del explorador de ASP.NET, siga estos pasos:

1. Cree una clase de proveedor que derive de *HttpCapabilitiesProvider* y que invalide el método *GetBrowserCapabilities* , como en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    El código de este ejemplo crea un nuevo objeto *HttpBrowserCapabilities* , especificando solo la funcionalidad denominada browser y estableciendo esa capacidad en MyCustomBrowser.
2. Registre el proveedor con la aplicación. 

    Para usar un proveedor con una aplicación, debe agregar el atributo *Provider* a la sección *browserCaps* en los archivos `Web.config` o `Machine.config`. (También puede definir los atributos de proveedor en un elemento de *Ubicación* para directorios específicos de la aplicación, como en una carpeta para un dispositivo móvil específico). En el ejemplo siguiente se muestra cómo establecer el atributo de *proveedor* en un archivo de configuración:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Otra manera de registrar la nueva definición de funcionalidad del explorador es usar el código, como se muestra en el ejemplo siguiente:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Este código debe ejecutarse en la *aplicación\_* evento de inicio del archivo de `Global.asax`. Cualquier cambio en la clase *BrowserCapabilitiesProvider* debe aparecer antes de que se ejecute cualquier código de la aplicación, con el fin de asegurarse de que la memoria caché permanece en un estado válido para el objeto *HttpCapabilitiesBase* resuelto.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Almacenar en caché el objeto HttpBrowserCapabilities

El ejemplo anterior tiene un problema, que es que el código se ejecutaría cada vez que se invoca el proveedor personalizado para obtener el objeto *HttpBrowserCapabilities* . Esto puede ocurrir varias veces durante cada solicitud. En el ejemplo, el código del proveedor no hace mucho. Sin embargo, si el código del proveedor personalizado realiza un trabajo significativo para obtener el objeto *HttpBrowserCapabilities* , esto puede afectar al rendimiento. Para evitar que esto suceda, puede almacenar en caché el objeto *HttpBrowserCapabilities* . Siga estos pasos:

1. Cree una clase que derive de *HttpCapabilitiesProvider*, como la del ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    En el ejemplo, el código genera una clave de caché mediante una llamada a un método BuildCacheKey personalizado y obtiene el período de tiempo que se va a almacenar en la memoria caché llamando a un método GetCacheTime personalizado. Después, el código agrega el objeto *HttpBrowserCapabilities* resuelto a la memoria caché. El objeto se puede recuperar de la memoria caché y reutilizarlo en solicitudes posteriores que hagan uso del proveedor personalizado.
2. Registre el proveedor con la aplicación como se describe en el procedimiento anterior.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Extender la funcionalidad de las funciones del explorador de ASP.NET

En la sección anterior se describe cómo crear un nuevo objeto *HttpBrowserCapabilities* en ASP.net 4. También puede ampliar la funcionalidad de las funciones del explorador de ASP.NET agregando nuevas definiciones de capacidades del explorador a las que ya están en ASP.NET. Puede hacerlo sin usar las definiciones de explorador XML. En el procedimiento siguiente se muestra cómo.

1. Cree una clase que derive de *HttpCapabilitiesEvaluator* y que invalide el método *GetBrowserCapabilities* , como se muestra en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    En primer lugar, este código usa la funcionalidad de las funciones del explorador de ASP.NET para intentar identificar el explorador. Sin embargo, si no se identifica ningún explorador en función de la información definida en la solicitud (es decir, si la propiedad *Browser* del objeto *HttpBrowserCapabilities* es la cadena "Unknown"), el código llama al proveedor personalizado (MyBrowserCapabilitiesEvaluator) para identificar el explorador.
2. Registre el proveedor con la aplicación como se describe en el ejemplo anterior.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Ampliar la funcionalidad de las funciones del explorador agregando nuevas funcionalidades a las definiciones de funcionalidades existentes

Además de crear un proveedor de definición de explorador personalizado y de crear dinámicamente nuevas definiciones de explorador, puede extender las definiciones existentes del explorador con capacidades adicionales. Esto le permite usar una definición que está próxima a lo que desea, pero que solo tiene algunas funcionalidades. Para hacer esto, siga los pasos siguientes:

1. Cree una clase que derive de *HttpCapabilitiesEvaluator* y que invalide el método *GetBrowserCapabilities* , como se muestra en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    El código de ejemplo amplía la clase *HttpCapabilitiesEvaluator* de ASP.net existente y obtiene el objeto *HttpBrowserCapabilities* que coincide con la definición de solicitud actual mediante el código siguiente:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Después, el código puede Agregar o modificar una funcionalidad para este explorador. Hay dos maneras de especificar una nueva funcionalidad del explorador:

    - Agregue un par clave-valor al objeto *IDictionary* que expone la propiedad *Capabilities* del objeto *HttpCapabilitiesBase* . En el ejemplo anterior, el código agrega una funcionalidad denominada multitoque con un valor de *true*.
    - Establezca las propiedades existentes del objeto *HttpCapabilitiesBase* . En el ejemplo anterior, el código establece la propiedad *frames* en *true*. Esta propiedad es simplemente un descriptor de acceso para el objeto *IDictionary* que expone la propiedad *Capabilities* . 

        > [!NOTE]
        > Nota Este modelo se aplica a cualquier propiedad de *HttpBrowserCapabilities*, incluidos los adaptadores de control.
2. Registre el proveedor con la aplicación como se describe en el procedimiento anterior.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Enrutamiento en ASP.NET 4

ASP.NET 4 agrega compatibilidad integrada para usar el enrutamiento con formularios Web Forms. El enrutamiento le permite configurar una aplicación para que acepte direcciones URL de solicitud que no se asignan a archivos físicos. En su lugar, puede usar el enrutamiento para definir direcciones URL que son significativas para los usuarios y que pueden ayudar con la optimización del motor de búsqueda (SEO) de la aplicación. Por ejemplo, la dirección URL de una página que muestra las categorías de producto en una aplicación existente podría ser similar al ejemplo siguiente:

[!code-console[Main](overview/samples/sample36.cmd)]

Mediante el enrutamiento, puede configurar la aplicación para que acepte la siguiente dirección URL para presentar la misma información:

[!code-console[Main](overview/samples/sample37.cmd)]

El enrutamiento está disponible a partir de ASP.NET 3,5 SP1. (Para ver un ejemplo de cómo usar el enrutamiento en ASP.NET 3,5 SP1, consulte la entrada [uso del enrutamiento con WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Título de esta entrada.") en el blog de Phil Haack). Sin embargo, ASP.NET 4 incluye algunas características que facilitan el uso del enrutamiento, entre las que se incluyen las siguientes:

- La clase *PageRouteHandler* , que es un controlador http sencillo que se usa al definir rutas. La clase pasa los datos a la página a la que se enruta la solicitud.
- Las nuevas propiedades *HttpRequest. RequestContext* y *Page. RouteData* (que es un proxy para el objeto *HttpRequest. RequestContext. RouteData* ). Estas propiedades facilitan el acceso a la información que se pasa desde la ruta.
- Los nuevos generadores de expresiones siguientes, que se definen en *System. Web. Compilation. RouteUrlExpressionBuilder* y *System. Web. Compilation. RouteValueExpressionBuilder*:
- *RouteUrl*, que proporciona una manera sencilla de crear una dirección URL que corresponde a una dirección URL de ruta dentro de un control de servidor ASP.net.
- *RouteValue*, que proporciona una manera sencilla de extraer información del objeto *RouteContext* .
- La clase *RouteParameter* , que facilita el paso de los datos contenidos en un objeto *RouteContext* a una consulta de un control de origen de datos (similar a [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Enrutamiento para páginas de formularios Web Forms

En el ejemplo siguiente se muestra cómo definir una ruta de formularios Web Forms mediante el nuevo método *MapPageRoute* de la clase *Route* :

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 presenta el método *MapPageRoute* . El ejemplo siguiente es equivalente a la definición de SearchRoute que se muestra en el ejemplo anterior, pero usa la clase *PageRouteHandler* .

[!code-csharp[Main](overview/samples/sample39.cs)]

El código del ejemplo asigna la ruta a una página física (en la primera ruta, a `~/search.aspx`). La primera definición de ruta también especifica que el parámetro denominado searchterm debe extraerse de la dirección URL y pasarse a la página.

El método *MapPageRoute* admite las siguientes sobrecargas de método:

- *MapPageRoute (String routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (String routeName, String routeUrl, String physicalFile, bool checkPhysicalUrlAccess, valores predeterminados de RouteValueDictionary)*
- *MapPageRoute (cadena routeName, cadena routeUrl, cadena physicalFile, bool checkPhysicalUrlAccess, valores predeterminados de RouteValueDictionary, restricciones de RouteValueDictionary)*

El parámetro *checkPhysicalUrlAccess* especifica si la ruta debe comprobar los permisos de seguridad de la página física que se va a enrutar (en este caso, Search. aspx) y los permisos en la dirección URL de entrada (en este caso, Search/{searchterm}). Si el valor de *checkPhysicalUrlAccess* es *false*, solo se comprueban los permisos de la dirección URL entrante. Estos permisos se definen en el archivo de `Web.config` con opciones como las siguientes:

[!code-xml[Main](overview/samples/sample40.xml)]

En la configuración de ejemplo, se deniega el acceso a la página física `search.aspx` para todos los usuarios excepto los que están en el rol de administrador. Cuando el parámetro *checkPhysicalUrlAccess* se establece en *true* (que es su valor predeterminado), solo se permite que los usuarios administradores tengan acceso a la dirección URL/Search/{searchterm}, ya que la página física Search. aspx está restringida a los usuarios de ese rol. Si *checkPhysicalUrlAccess* se establece en *false* y el sitio se configura como se muestra en el ejemplo anterior, todos los usuarios autenticados pueden tener acceso a la dirección URL/Search/{searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Leer información de enrutamiento en una página de formularios Web Forms

En el código de la página física de formularios Web Forms, puede tener acceso a la información que el enrutamiento ha extraído de la dirección URL (u otra información que otro objeto haya agregado al objeto *RouteData* ) mediante el uso de dos nuevas propiedades: *HttpRequest. RequestContext* y *Page. RouteData*. (*Page. RouteData* ajusta *HttpRequest. RequestContext. RouteData*). En el ejemplo siguiente se muestra cómo usar *Page. RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

El código extrae el valor que se pasó para el parámetro searchterm, tal como se define en la ruta de ejemplo anterior. Considere la siguiente dirección URL de solicitud:

[!code-console[Main](overview/samples/sample42.cmd)]

Cuando se realiza esta solicitud, la palabra "Scott" se representaría en la página `search.aspx`.

#### <a name="accessing-routing-information-in-markup"></a>Obtener acceso a la información de enrutamiento en el marcado

El método descrito en la sección anterior muestra cómo obtener datos de ruta en el código en una página de formularios Web Forms. También puede utilizar expresiones en el marcado que proporcionan acceso a la misma información. Los generadores de expresiones son una manera eficaz y elegante de trabajar con código declarativo. (Para obtener más información, consulte la entrada [rápida con los creadores de expresiones personalizadas](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) en el blog de Phil Haack).

ASP.NET 4 incluye dos nuevos generadores de expresiones para el enrutamiento de formularios Web Forms. En el ejemplo siguiente se muestra cómo utilizarlos.

[!code-aspx[Main](overview/samples/sample43.aspx)]

En el ejemplo, la expresión *RouteUrl* se usa para definir una dirección URL que se basa en un parámetro de ruta. Esto evita tener que codificar de forma rígida la dirección URL completa en el marcado y permite cambiar la estructura de la dirección URL más adelante sin necesidad de realizar ningún cambio en este vínculo.

En función de la ruta definida anteriormente, este marcado genera la siguiente dirección URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET funciona automáticamente en la ruta correcta (es decir, genera la dirección URL correcta) basándose en los parámetros de entrada. También puede incluir un nombre de ruta en la expresión, que le permite especificar la ruta que se va a usar.

En el ejemplo siguiente se muestra cómo usar la expresión *RouteValue* .

[!code-aspx[Main](overview/samples/sample45.aspx)]

Cuando se ejecuta la página que contiene este control, se muestra el valor "Scott" en la etiqueta.

La expresión *RouteValue* facilita el uso de los datos de ruta en el marcado y evita tener que trabajar con la sintaxis Page. RouteData ["x"] en el marcado.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Usar datos de ruta para parámetros de control de origen de datos

La clase *RouteParameter* permite especificar los datos de ruta como un valor de parámetro para las consultas en un control de origen de datos. Funciona de manera [muy similar a la](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) clase, como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample46.aspx)]

En este caso, se usará el valor del parámetro de ruta searchterm para el parámetro @companyname en la instrucción *Select* .

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Establecer identificadores de cliente

La nueva propiedad *ClientIDMode* se dirige a un problema de larga duración en ASP.net, es decir, cómo los controles crean el atributo *ID* para los elementos que representan. Conocer el atributo *ID* de los elementos representados es importante si la aplicación incluye el script de cliente que hace referencia a estos elementos.

El atributo *ID* de HTML que se representa para los controles de servidor Web se genera en función de la propiedad *ClientID* del control. Hasta ASP.NET 4, el algoritmo para generar el atributo *ID* de la propiedad *ClientID* ha sido concatenar el contenedor de nomenclatura (si existe) con el identificador, y en el caso de los controles repetidos (como en los controles de datos), para agregar un prefijo y un número secuencial. Aunque esto siempre garantiza que los identificadores de los controles de la página son únicos, el algoritmo ha provocado que los identificadores de control no sean predecibles y, por lo tanto, son difíciles de hacer referencia en el script de cliente.

La nueva propiedad *ClientIDMode* permite especificar más exactamente cómo se genera el identificador de cliente para los controles. Puede establecer la propiedad *ClientIDMode* para cualquier control, incluido para la página. Los valores posibles son los siguientes:

- *AutoID* : es equivalente al algoritmo para generar los valores de la propiedad *ClientID* que se usó en versiones anteriores de ASP.net.
- *Static* : especifica que el valor de *ClientID* será el mismo que el identificador sin concatenar los identificadores de los contenedores de nomenclatura primarios. Esto puede ser útil en los controles de usuario Web. Dado que un control de usuario web puede ubicarse en páginas diferentes y en distintos controles de contenedor, puede ser difícil escribir script de cliente para los controles que usan el algoritmo *AutoID* porque no se pueden predecir los valores de identificador.
- *Predicción* : esta opción se usa principalmente en controles de datos que usan plantillas repetidas. Concatena las propiedades de identificador de los contenedores de nomenclatura del control, pero los valores de *ClientID* generados no contienen cadenas como "ctlxxx". Esta configuración funciona junto con la propiedad *ClientIDRowSuffix* del control. La propiedad *ClientIDRowSuffix* se establece en el nombre de un campo de datos y el valor de ese campo se usa como sufijo para el valor de *ClientID* generado. Normalmente, se usaría la clave principal de un registro de datos como el valor de *ClientIDRowSuffix* .
- *Inherit* : este valor es el comportamiento predeterminado de los controles; Especifica que la generación de IDENTIFICADOres de un control es igual que su elemento primario.

Puede establecer la propiedad *ClientIDMode* en el nivel de página. Define el valor predeterminado de *ClientIDMode* para todos los controles de la página actual.

El valor predeterminado de *ClientIDMode* en el nivel de página es *AutoID*y el valor de *ClientIDMode* predeterminado en el nivel de control es *heredar*. Como resultado, si no se establece esta propiedad en cualquier parte del código, todos los controles tendrán como valor predeterminado el algoritmo *AutoID* .

Establezca el valor de nivel de página en la directiva *@ Page* , tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample47.aspx)]

También puede establecer el valor de *ClientIDMode* en el archivo de configuración, ya sea en el nivel de equipo (equipo) o en el nivel de aplicación. Define el valor predeterminado de *ClientIDMode* para todos los controles de todas las páginas de la aplicación. Si establece el valor en el nivel de equipo, se define la configuración predeterminada de *ClientIDMode* para todos los sitios web de ese equipo. En el ejemplo siguiente se muestra la configuración de *ClientIDMode* en el archivo de configuración:

[!code-xml[Main](overview/samples/sample48.xml)]

Como se indicó anteriormente, el valor de la propiedad *ClientID* se deriva del contenedor de nomenclatura para el elemento primario de un control. En algunos escenarios, como cuando se usan páginas maestras, los controles pueden acabar con identificadores como los del siguiente código HTML representado:

[!code-html[Main](overview/samples/sample49.html)]

Aunque el elemento de *entrada* que se muestra en el marcado (desde un control de *cuadro de texto* ) es solo dos contenedores de nomenclatura en profundidad en la página (los controles de *ContentPlaceHolder* anidados), debido a la forma en que se procesan las páginas maestras, el resultado final es un identificador de control como el siguiente:

[!code-console[Main](overview/samples/sample50.cmd)]

Se garantiza que este identificador es único en la página, pero es innecesariamente largo para la mayoría de los propósitos. Imagine que desea reducir la longitud del identificador representado y tener más control sobre cómo se genera el identificador. (Por ejemplo, desea eliminar los prefijos "ctlxxx"). La manera más sencilla de lograrlo es establecer la propiedad *ClientIDMode* como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample51.aspx)]

En este ejemplo, la propiedad *ClientIDMode* se establece en *static* para el elemento *NamingPanel* más externo y se establece en *predecible* para el elemento *NamingControl* interno. Esta configuración da como resultado el siguiente marcado (el resto de la página y se supone que la página maestra es la misma que en el ejemplo anterior):

[!code-html[Main](overview/samples/sample52.html)]

La configuración *estática* tiene el efecto de restablecer la jerarquía de nomenclatura de cualquier control dentro del elemento *NamingPanel* más externo y de eliminar los identificadores de *ContentPlaceHolder* y *masterpage* del identificador generado. (El atributo *Name* de los elementos representados no se ve afectado, por lo que la funcionalidad ASP.net normal se conserva para los eventos, el estado de vista, etc.) Un efecto secundario del restablecimiento de la jerarquía de nomenclatura es que incluso si mueve el marcado de los elementos *NamingPanel* a un control *ContentPlaceHolder* diferente, los identificadores de cliente representados siguen siendo los mismos.

> [!NOTE]
> Tenga en cuenta que depende de usted asegurarse de que los identificadores de control presentados son únicos. Si no es así, puede interrumpir cualquier funcionalidad que requiera identificadores únicos para elementos HTML individuales, como la función *Document. getElementById* de cliente.

#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Crear identificadores de cliente predecibles en controles enlazados a datos

Los valores de *ClientID* que se generan para los controles de un control de lista enlazado a datos mediante el algoritmo heredado pueden ser largos y no son realmente predecibles. La funcionalidad de *ClientIDMode* puede ayudarle a tener más control sobre cómo se generan estos identificadores.

El marcado en el ejemplo siguiente incluye un control *ListView* :

[!code-aspx[Main](overview/samples/sample53.aspx)]

En el ejemplo anterior, las propiedades *ClientIDMode* y *RowClientIDRowSuffix* se establecen en el marcado. La propiedad *ClientIDRowSuffix* solo se puede usar en controles enlazados a datos y su comportamiento difiere en función del control que se use. Las diferencias son las siguientes:

- Control *GridView* : puede especificar el nombre de una o varias columnas del origen de datos, que se combinan en tiempo de ejecución para crear los identificadores de cliente. Por ejemplo, si establece *RowClientIDRowSuffix* en "ProductName, ProductID", los identificadores de control de los elementos representados tendrán un formato similar al siguiente:

- [!code-console[Main](overview/samples/sample54.cmd)]

- Control *ListView* : puede especificar una única columna en el origen de datos que se anexe al identificador de cliente. Por ejemplo, si establece *ClientIDRowSuffix* en "ProductName", los identificadores de control representados tendrán un formato similar al siguiente:

- [!code-console[Main](overview/samples/sample55.cmd)]

- En este caso, el objeto 1 final se deriva del ID. de producto del elemento de datos actual.

- Control *Repeater* : este control no admite la propiedad *ClientIDRowSuffix* . En un control *Repeater* , se usa el índice de la fila actual. Cuando se usa ClientIDMode = "predicción" con un control *Repeater* , se generan identificadores de cliente con el siguiente formato:

- [!code-console[Main](overview/samples/sample56.cmd)]

- El valor 0 final es el índice de la fila actual.

Los controles *FormView* y *DetailsView* no muestran varias filas, por lo que no admiten la propiedad *ClientIDRowSuffix* .

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Conservar la selección de fila en los controles de datos

Los controles *GridView* y *ListView* pueden permitir a los usuarios seleccionar una fila. En versiones anteriores de ASP.NET, la selección se basaba en el índice de fila de la página. Por ejemplo, si selecciona el tercer elemento de la Página 1 y después se desplaza a la página 2, se seleccionará el tercer elemento de esa página.

La selección persistente se admitía inicialmente solo en proyectos de datos dinámicos en el .NET Framework 3,5 SP1. Cuando esta característica está habilitada, el elemento seleccionado actual se basa en la clave de datos del elemento. Esto significa que si selecciona la tercera fila de la Página 1 y se desplaza a la página 2, no se selecciona nada en la página 2. Al retroceder a la Página 1, la tercera fila sigue estando seleccionada. La selección persistente ahora es compatible con los controles *GridView* y *ListView* en todos los proyectos mediante el uso de la propiedad *EnablePersistedSelection* , como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Control chart de ASP.NET

El control *Chart* de ASP.net expande las ofertas de visualización de datos en el .NET Framework. Con el control *Chart* , puede crear páginas ASP.Nets que tengan gráficos intuitivos y visualmente atractivos para un análisis estadístico o financiero complejo. El control *Chart* de ASP.net se presentó como un complemento a la versión de .NET Framework versión 3,5 SP1 y forma parte de la versión .NET Framework 4.

El control incluye las siguientes características:

- 35 tipos de gráfico distintos.
- Un número ilimitado de áreas de gráfico, títulos, leyendas y anotaciones.
- Gran variedad de opciones de apariencia para todos los elementos de gráfico.
- compatibilidad 3D para la mayoría de los tipos de gráficos.
- Etiquetas de datos inteligentes que pueden ajustarse automáticamente a los puntos de datos.
- Franjas, quiebres de escala y escala logarítmica.
- Más de 50 fórmulas financieras y estadísticas para la transformación y el análisis de datos.
- Enlace sencillo y manipulación de los datos del gráfico.
- Compatibilidad con formatos de datos comunes, como fechas, horas y monedas.
- Compatibilidad con la interactividad y la personalización orientada a eventos, incluidos los eventos de clic de cliente mediante Ajax.
- Administración de estados.
- Transmisión de flujo binario.

Las siguientes figuras muestran ejemplos de gráficos financieros producidos por el control de gráfico ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: ejemplos de control de gráfico de ASP.NET

Para obtener más ejemplos de cómo usar el control de gráfico ASP.NET, descargue el código de ejemplo de la página [de ejemplos de entorno para controles de Microsoft Chart](https://go.microsoft.com/fwlink/?LinkId=128300) en el sitio web de MSDN. Puede encontrar más ejemplos de contenido de la comunidad en el [Foro de control de gráficos](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Agregar el control Chart a una página ASP.NET

En el ejemplo siguiente se muestra cómo agregar un control *Chart* a una página ASP.net mediante el marcado. En el ejemplo, el control *Chart* genera un gráfico de columnas para los puntos de datos estáticos.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Uso de gráficos 3D

El control *Chart* contiene una colección *ChartAreas* , que puede contener objetos *ChartArea* que definen características de las áreas del gráfico. Por ejemplo, para usar 3D para un área de gráfico, use la propiedad *Area3DStyle* como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample59.aspx)]

En la siguiente ilustración se muestra un gráfico 3D con cuatro series del tipo de gráfico de *barras* .

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3:3-gráfico de barras D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Usar quiebres de escala y escalas logarítmicas

Los quiebres de escala y las escalas logarítmicas son dos maneras adicionales de agregar sofisticación al gráfico. Estas características son específicas de cada eje en un área de gráfico. Por ejemplo, para usar estas características en el eje Y principal de un área de gráfico, use las propiedades *AxisY. IsLogarithmic* y *ScaleBreakStyle* en un objeto *ChartArea* . En el fragmento de código siguiente se muestra cómo usar quiebres de escala en el eje Y principal.

[!code-aspx[Main](overview/samples/sample60.aspx)]

En la siguiente ilustración se muestra el eje Y con quiebres de escala habilitados.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: quiebres de escala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrar datos con el control QueryExtender

Una tarea muy común para los desarrolladores que crean páginas web controladas por datos es filtrar los datos. Tradicionalmente, esto se ha realizado compilando cláusulas *Where* en controles de origen de datos. Este enfoque puede ser complicado y, en algunos casos, la sintaxis *Where* no le permite aprovechar toda la funcionalidad de la base de datos subyacente.

Para facilitar el filtrado, se ha agregado un nuevo control *QueryExtender* en ASP.net 4. Este control se puede Agregar a los controles *EntityDataSource* o *LinqDataSource* para filtrar los datos devueltos por estos controles. Dado que el control *QueryExtender* se basa en LINQ, el filtro se aplica en el servidor de base de datos antes de que los datos se envíen a la página, lo que da lugar a operaciones muy eficaces.

El control *QueryExtender* admite una variedad de opciones de filtro. En las secciones siguientes se describen estas opciones y se proporcionan ejemplos de cómo usarlas.

#### <a name="search"></a>Buscar

Para la opción de búsqueda, el control *QueryExtender* realiza una búsqueda en los campos especificados. En el ejemplo siguiente, el control utiliza el texto que se especifica en el control TextBoxSearch y busca su contenido en el `ProductName` y `Supplier.CompanyName` columnas de los datos devueltos por el control *LinqDataSource* .

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervalo

La opción de intervalo es similar a la opción de búsqueda, pero especifica un par de valores para definir el intervalo. En el ejemplo siguiente, el control *QueryExtender* busca en el `UnitPrice` columna de los datos devueltos por el control *LinqDataSource* . El intervalo se lee desde los controles TextBoxFrom y TextBoxto en la página.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

La opción expresión de propiedad permite definir una comparación con un valor de propiedad. Si la expresión se evalúa como *true*, se devuelven los datos que se están examinando. En el ejemplo siguiente, el control *QueryExtender* filtra los datos comparando los datos de la columna `Discontinued` con el valor del control CheckBoxDiscontinued de la página.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Por último, puede especificar una expresión personalizada para usarla con el control *QueryExtender* . Esta opción le permite llamar a una función de la página que define la lógica de filtro personalizada. En el ejemplo siguiente se muestra cómo especificar una expresión personalizada mediante declaración en el control *QueryExtender* .

[!code-aspx[Main](overview/samples/sample64.aspx)]

En el ejemplo siguiente se muestra la función personalizada invocada por el control *QueryExtender* . En este caso, en lugar de usar una consulta de base de datos que incluya una cláusula *Where* , el código usa una consulta LINQ para filtrar los datos.

[!code-csharp[Main](overview/samples/sample65.cs)]

En estos ejemplos se muestra solo una expresión que se usa en el control *QueryExtender* cada vez. Sin embargo, puede incluir varias expresiones en el control *QueryExtender* .

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expresiones de código HTML codificado

Algunos sitios de ASP.NET (especialmente con ASP.NET MVC) dependen en gran medida del uso de `<%`= sintaxis de `expression %>` (a menudo denominada "fragmentos de código") para escribir texto en la respuesta. Cuando se usan expresiones de código, es fácil olvidarse de codificar en HTML el texto, si el texto procede de los datos proporcionados por el usuario, puede dejar páginas abiertas a un ataque XSS (scripting entre sitios).

ASP.NET 4 presenta la siguiente sintaxis nueva para expresiones de código:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Esta sintaxis utiliza la codificación HTML de forma predeterminada cuando se escribe en la respuesta. Esta nueva expresión se traduce eficazmente a lo siguiente:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Por ejemplo, &lt;%: Request ["UserInput"]%&gt; realiza la codificación HTML en el valor de *Request ["UserInput"]* .

El objetivo de esta característica es que sea posible reemplazar todas las instancias de la sintaxis anterior con la nueva sintaxis, de modo que no se le obligue a decidir en cada paso cuál de ellas usar. Sin embargo, hay casos en los que el texto que se está generando está destinado a ser HTML o ya está codificado, en cuyo caso esto podría dar lugar a la codificación doble.

En estos casos, ASP.NET 4 introduce una nueva interfaz, *IHtmlString*, junto con una implementación concreta, *HtmlString*. Las instancias de estos tipos permiten indicar que el valor devuelto ya está correctamente codificado (o examinado de otro modo) para que se muestre como HTML, por lo que el valor no debe volver a codificarse en HTML. Por ejemplo, el siguiente código no debe ser (y no es) codificado en HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Los métodos auxiliares de ASP.NET MVC 2 se han actualizado para que funcionen con esta nueva sintaxis para que no estén codificados con dobleidad, pero solo cuando se ejecuta ASP.NET 4. Esta nueva sintaxis no funciona cuando se ejecuta una aplicación con ASP.NET 3,5 SP1.

Tenga en cuenta que esto no garantiza la protección contra ataques XSS. Por ejemplo, el código HTML que usa valores de atributo que no están entre comillas puede contener datos proporcionados por el usuario que siguen siendo susceptibles. Tenga en cuenta que la salida de los controles de ASP.NET y las aplicaciones auxiliares de ASP.NET MVC siempre incluye valores de atributo entre comillas, que es el enfoque recomendado.

Del mismo modo, esta sintaxis no realiza la codificación de JavaScript, como cuando se crea una cadena de JavaScript basada en la entrada del usuario.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Cambios de plantilla de proyecto

En versiones anteriores de ASP.NET, al usar Visual Studio para crear un nuevo proyecto de sitio web o proyecto de aplicación Web, los proyectos resultantes contienen solo una página default. aspx, un archivo `Web.config` predeterminado y la carpeta `App_Data`, como se muestra en la siguiente ilustración:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio también admite un tipo de proyecto de sitio Web vacío, que no contiene ningún archivo, como se muestra en la ilustración siguiente:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

El resultado es que para el principiante, hay muy pocas instrucciones sobre cómo crear una aplicación Web de producción. Por lo tanto, ASP.NET 4 introduce tres nuevas plantillas, una para un proyecto de aplicación Web vacío y otra para una aplicación web y un proyecto de sitio Web.

#### <a name="empty-web-application-template"></a>Plantilla de aplicación Web vacía

Como sugiere su nombre, la plantilla de aplicación Web vacía es un proyecto de aplicación web desactivado. Seleccione esta plantilla de proyecto en el cuadro de diálogo nuevo proyecto de Visual Studio, como se muestra en la ilustración siguiente:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Haga clic para ver la imagen de tamaño completo](overview/_static/image8.png))

Cuando se crea una aplicación Web de ASP.NET vacía, Visual Studio crea el siguiente diseño de carpeta:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Esto es similar al diseño de sitio Web vacío de versiones anteriores de ASP.NET, con una excepción. En Visual Studio 2010, la aplicación Web vacía y los proyectos de sitio web vacíos contienen el siguiente archivo de `Web.config` mínimo que contiene la información usada por Visual Studio para identificar el marco de trabajo de destino del proyecto:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sin esta propiedad *targetFramework* , Visual Studio tiene como destino el .NET Framework 2,0 para mantener la compatibilidad al abrir aplicaciones antiguas.

#### <a name="web-application-and-web-site-project-templates"></a>Plantillas de proyecto de sitio web y aplicación Web

Las otras dos plantillas de proyecto nuevas que se incluyen con Visual Studio 2010 contienen cambios importantes. En la ilustración siguiente se muestra el diseño del proyecto que se crea al crear un nuevo proyecto de aplicación Web. (El diseño de un proyecto de sitio web es prácticamente idéntico).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

El proyecto incluye una serie de archivos que no se crearon en versiones anteriores. Además, el nuevo proyecto de aplicación web se configura con la funcionalidad de pertenencia básica, lo que le permite empezar a proteger rápidamente el acceso a la nueva aplicación. Debido a esta inclusión, el archivo de `Web.config` del nuevo proyecto incluye entradas que se usan para configurar la pertenencia, los roles y los perfiles. En el ejemplo siguiente se muestra el archivo de `Web.config` para un nuevo proyecto de aplicación Web. (En este caso, *roleManager* está deshabilitado).

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Haga clic para ver la imagen de tamaño completo](overview/_static/image14.png))

El proyecto también contiene un segundo archivo `Web.config` en el directorio `Account`. El segundo archivo de configuración proporciona una manera de proteger el acceso a la página ChangePassword. aspx para los usuarios que no tienen la sesión iniciada. En el ejemplo siguiente se muestra el contenido del segundo archivo de `Web.config`.

![](overview/_static/image15.png)

Las páginas creadas de forma predeterminada en las plantillas de proyecto nuevas también contienen más contenido que en versiones anteriores. El proyecto contiene una página maestra y un archivo CSS predeterminados, y la página predeterminada (default. aspx) está configurada para usar la página maestra de forma predeterminada. El resultado es que, al ejecutar la aplicación web o el sitio web por primera vez, la página predeterminada (Inicio) ya es funcional. De hecho, es similar a la página predeterminada que se ve al iniciar una nueva aplicación MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Haga clic para ver la imagen de tamaño completo](overview/_static/image18.png))

La intención de estos cambios en las plantillas de proyecto es proporcionar instrucciones sobre cómo empezar a crear una nueva aplicación Web. Con el marcado semánticamente correcto, estricto y compatible con XHTML 1,0 y con el diseño que se especifica mediante CSS, las páginas de las plantillas representan los procedimientos recomendados para compilar las aplicaciones Web de ASP.NET 4. Las páginas predeterminadas también tienen un diseño de dos columnas que se puede personalizar fácilmente.

Por ejemplo, Imagine que para una nueva aplicación web desea cambiar algunos de los colores e insertar el logotipo de la empresa en lugar del logotipo de la aplicación My ASP.NET. Para ello, cree un nuevo directorio en `Content` para almacenar la imagen del logotipo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Para agregar la imagen a la página, abra el archivo `Site.Master`, busque donde se define el texto de la aplicación My ASP.NET y reemplácelo por un elemento *Image* cuyo atributo *src* esté establecido en la nueva imagen de logotipo, como en el ejemplo siguiente:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Haga clic para ver la imagen de tamaño completo](overview/_static/image22.png))

Después, puede ir al archivo site. CSS y modificar las definiciones de clase CSS para cambiar el color de fondo de la página, así como el del encabezado.

El resultado de estos cambios es que puede mostrar una página principal personalizada con muy poco esfuerzo:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Haga clic para ver la imagen de tamaño completo](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Mejoras de CSS

Una de las principales áreas de trabajo en ASP.NET 4 ha sido ayudar a representar HTML que es compatible con los estándares HTML más recientes. Esto incluye los cambios en el modo en que los controles de servidor Web de ASP.NET usan estilos CSS.

#### <a name="compatibility-setting-for-rendering"></a>Configuración de compatibilidad para la representación

De forma predeterminada, cuando una aplicación web o un sitio web tiene como destino el .NET Framework 4, el atributo *controlRenderingCompatibilityVersion* del elemento *pages* se establece en "4,0". Este elemento se define en el archivo de `Web.config` de nivel de equipo y, de forma predeterminada, se aplica a todas las aplicaciones de ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

El valor de *controlRenderingCompatibility* es una cadena, que permite nuevas definiciones de versiones posibles en futuras versiones. En la versión actual, se admiten los siguientes valores para esta propiedad:

- "3.5". Este valor indica la representación y el marcado heredados. El marcado representado por los controles es 100% compatible con versiones anteriores y se respeta el valor de la propiedad *xhtmlConformance* .
- "4.0". Si la propiedad tiene esta configuración, los controles de servidor Web de ASP.NET hacen lo siguiente:
- La propiedad *xhtmlConformance* siempre se trata como "STRICT". Como resultado, los controles representan el marcado XHTML 1,0 STRICT.
- Deshabilitar los controles que no son de entrada ya no representa estilos no válidos.
- los elementos *div* alrededor de los campos ocultos están ahora con estilo para que no interfieran con las reglas CSS creadas por el usuario.
- Los controles de menú representan el marcado que es semánticamente correcto y cumple con las directrices de accesibilidad.
- Los controles de validación no representan estilos en línea.
- Los controles que anteriormente representaban Border = "0" (controles que derivan del control de *tabla* ASP.net y el control de *imagen* ASP.net) ya no representan este atributo.

#### <a name="disabling-controls"></a>Deshabilitar controles

En ASP.NET 3,5 SP1 y versiones anteriores, el marco de trabajo representa el atributo *Disabled* en el marcado HTML de cualquier control cuya propiedad *enabled esté* establecida en *false*. Sin embargo, según la especificación HTML 4,01, solo los elementos de *entrada* deben tener este atributo.

En ASP.NET 4, puede establecer la propiedad *controlRenderingCompatibilityVersion* en "3,5", como en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample70.xml)]

Puede crear un marcado para un control *etiqueta* como el siguiente, lo que deshabilita el control:

[!code-aspx[Main](overview/samples/sample71.aspx)]

El control *etiqueta* presentaría el siguiente código HTML:

[!code-html[Main](overview/samples/sample72.html)]

En ASP.NET 4, puede establecer el *controlRenderingCompatibilityVersion* en "4,0". En ese caso, solo los controles que representan elementos de *entrada* representarán un atributo *deshabilitado* cuando la propiedad *Enabled* del control esté establecida en *false*. Los controles que no representan elementos de *entrada* HTML representan en su lugar un atributo de *clase* que hace referencia a una clase CSS que se puede utilizar para definir un aspecto deshabilitado para el control. Por ejemplo, el control de *etiqueta* que se muestra en el ejemplo anterior generaría el siguiente marcado:

[!code-html[Main](overview/samples/sample73.html)]

El valor predeterminado de la clase especificada para este control es "aspNetDisabled". Sin embargo, puede cambiar este valor predeterminado estableciendo la propiedad estática *DisabledCssClass* static de la clase *WebControl* . Para los desarrolladores de controles, el comportamiento que se va a utilizar para un control concreto también se puede definir mediante la propiedad *SupportsDisabledAttribute* .

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ocultar los elementos div alrededor de los campos ocultos

ASP.NET 2,0 y versiones posteriores representan campos ocultos específicos del sistema (como el elemento *oculto* que se usa para almacenar información de estado de vista) dentro del elemento *div* para cumplir con el estándar XHTML. Sin embargo, esto puede producir un problema cuando una regla CSS afecta a los elementos *div* de una página. Por ejemplo, puede dar lugar a que aparezca una línea de un píxel en la página alrededor de los elementos *div* ocultos. En ASP.NET 4, los elementos *div* que encierran los campos ocultos generados por ASP.net agregan una referencia de clase CSS como en el ejemplo siguiente:

[!code-html[Main](overview/samples/sample74.html)]

Después, puede definir una clase CSS que se aplique solo a los elementos *ocultos* generados por ASP.net, como en el ejemplo siguiente:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Representar una tabla externa para controles con plantilla

De forma predeterminada, los siguientes controles de servidor Web ASP.NET que admiten plantillas se ajustan automáticamente en una tabla externa que se usa para aplicar estilos en línea:

- *FormView*
- *Inicio de sesión*
- *PasswordRecovery*
- *ChangePassword*
- *Asistente*
- *CreateUserWizard*

Se ha agregado una nueva propiedad denominada *RenderOuterTable* a estos controles que permite quitar la tabla externa del marcado. Por ejemplo, considere el ejemplo siguiente de un control *FormView* :

[!code-aspx[Main](overview/samples/sample76.aspx)]

Este marcado representa el siguiente resultado en la página, que incluye una tabla HTML:

[!code-html[Main](overview/samples/sample77.html)]

Para evitar que se represente la tabla, puede establecer la propiedad *RenderOuterTable* del control *FormView* , como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample78.aspx)]

En el ejemplo anterior se representa el siguiente resultado, sin los elementos *TABLE*, *TR*y *TD* :

> Contenido

Esta mejora puede facilitar el estilo del contenido del control con CSS, ya que el control no representa ninguna etiqueta inesperada.

> [!NOTE]
> Tenga en cuenta que este cambio deshabilita la compatibilidad con la función de formato automático en el diseñador de Visual Studio 2010, porque ya no hay un elemento de *tabla* que pueda hospedar atributos de estilo generados por la opción de formato automático.

<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Mejoras del control ListView

El control *ListView* se ha facilitado para su uso en ASP.net 4. La versión anterior del control requiere que especifique una plantilla de diseño que contenía un control de servidor con un identificador conocido. En el marcado siguiente se muestra un ejemplo típico de cómo usar el control *ListView* en ASP.net 3,5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

En ASP.NET 4, el control *ListView* no requiere una plantilla de diseño. El marcado que se muestra en el ejemplo anterior se puede reemplazar por el marcado siguiente:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Mejoras en el control CheckBoxList y RadioButtonList

En ASP.NET 3,5, puede especificar el diseño de *CheckBoxList* y *RadioButtonList* con las dos opciones siguientes:

- *Flujo*. El control representa los elementos de *intervalo* para que contengan su contenido.
- *Tabla*. El control representa un elemento de *tabla* para que contenga su contenido.

En el ejemplo siguiente se muestra el marcado de cada uno de estos controles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

De forma predeterminada, los controles representan HTML similar al siguiente:

[!code-html[Main](overview/samples/sample82.html)]

Dado que estos controles contienen listas de elementos, para representar el código HTML correcto semánticamente, deben representar su contenido mediante elementos de lista HTML (*Li*). Esto hace que sea más fácil para los usuarios que leen páginas web mediante la tecnología de asistencia, y facilita el estilo de los controles mediante CSS.

En ASP.NET 4, los controles *CheckBoxList* y *RadioButtonList* admiten los siguientes valores nuevos para la propiedad *RepeatLayout* :

- *OrderedList* : el contenido se representa como elementos *Li* dentro de un elemento *OL* .
- *UnorderedList* : el contenido se representa como elementos *Li* dentro de un elemento *UL* .

En el ejemplo siguiente se muestra cómo utilizar estos nuevos valores.

[!code-aspx[Main](overview/samples/sample83.aspx)]

El marcado anterior genera el siguiente código HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Nota Si establece *RepeatLayout* en *OrderedList* o *UnorderedList*, la propiedad *RepeatDirection* ya no se puede usar y producirá una excepción en tiempo de ejecución si la propiedad se ha establecido en el marcado o el código. La propiedad no tendría ningún valor porque el diseño visual de estos controles se define con CSS en su lugar.

<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Mejoras en el control de menú

Antes de ASP.NET 4, el control de *menú* representó una serie de tablas HTML. Esto dificultaba la aplicación de los estilos CSS fuera de la configuración de las propiedades insertadas y tampoco cumplía los estándares de accesibilidad.

En ASP.NET 4, el control ahora representa HTML usando el marcado semántico que consta de una lista sin ordenar y elementos de lista. En el ejemplo siguiente se muestra el marcado en una página de ASP.NET para el control de *menú* .

[!code-aspx[Main](overview/samples/sample85.aspx)]

Cuando se representa la página, el control genera el siguiente código HTML (el código de *onclick* se ha omitido para mayor claridad):

[!code-html[Main](overview/samples/sample86.html)]

Además de las mejoras de representación, la navegación con el teclado del menú se ha mejorado mediante la administración del foco. Cuando el control de *menú* obtiene el foco, puede usar las teclas de dirección para navegar por los elementos. El control de *menú* también asocia ahora los roles y atributos de las aplicaciones de Internet enriquecidas (Aria) a SIG para mejorar[la](http://www.w3.org/TR/wai-aria-practices/#menu "Instrucciones de ARIA de menú")accesibilidad.

Los estilos del control de menú se representan en un bloque de estilo en la parte superior de la página, en lugar de en línea con los elementos HTML representados. Si desea tener un control total sobre el estilo del control, puede establecer la nueva propiedad *IncludeStyleBlock* en *false*, en cuyo caso no se emite el bloque Style. Una manera de usar esta propiedad es usar la característica de formato automático en el diseñador de Visual Studio para establecer la apariencia del menú. Después, puede ejecutar la página, abrir el origen de la página y, a continuación, copiar el bloque de estilo representado en un archivo CSS externo. En Visual Studio, deshaga el estilo y establezca *IncludeStyleBlock* en *false*. El resultado es que la apariencia del menú se define usando estilos en una hoja de estilos externa.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Controles Wizard y CreateUserWizard

El *Asistente para* ASP.net y los controles *CreateUserWizard* admiten plantillas que permiten definir el código HTML que representan. (*CreateUserWizard* se deriva del *Asistente*). En el ejemplo siguiente se muestra el marcado para un control *CreateUserWizard* completamente plantilla:

[!code-aspx[Main](overview/samples/sample87.aspx)]

El control presenta HTML similar al siguiente:

[!code-html[Main](overview/samples/sample88.html)]

En ASP.NET 3,5 SP1, aunque puede cambiar el contenido de la plantilla, aún tiene un control limitado sobre la salida del control del *Asistente* . En ASP.NET 4, puede crear una plantilla de *LayoutTemplate* e insertar controles de *marcador de posición* (mediante nombres reservados) para especificar cómo desea que se represente el control del *Asistente* . Esto se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample89.aspx)]

El ejemplo contiene los siguientes marcadores de posición con nombre en el elemento *LayoutTemplate* :

- *headerPlaceholder* : en tiempo de ejecución, esto se reemplaza por el contenido del elemento *HeaderTemplate* .
- *sideBarPlaceholder* : en tiempo de ejecución, esto se reemplaza por el contenido del elemento *SideBarTemplate* .
- *wizardStepPlaceHolder* : en tiempo de ejecución, esto se reemplaza por el contenido del elemento *WizardStepTemplate* .
- *navigationPlaceholder* : en tiempo de ejecución, esto se sustituye por las plantillas de navegación que haya definido.

El marcado del ejemplo que usa marcadores de posición representa el siguiente código HTML (sin el contenido realmente definido en las plantillas):

[!code-html[Main](overview/samples/sample90.html)]

El único HTML que ahora no está definido por el usuario es un elemento *span* . (Se prevé que en futuras versiones, incluso el elemento *span* no se representará). Esto le proporciona control total sobre prácticamente todo el contenido generado por el control del *Asistente* .

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC se presentó como un marco de trabajo de complementos a ASP.NET 3,5 SP1 en marzo de 2009. Visual Studio 2010 incluye ASP.NET MVC 2, que incluye nuevas características y capacidades.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Compatibilidad con áreas

Las áreas permiten agrupar controladores y vistas en secciones de una aplicación grande en aislamiento relativo desde otras secciones. Cada área se puede implementar como un proyecto de ASP.NET MVC independiente al que se puede hacer referencia desde la aplicación principal. Esto ayuda a administrar la complejidad al compilar una aplicación grande y permite que varios equipos trabajen juntos en una sola aplicación.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Compatibilidad con la validación de atributos de anotación de datos

Los atributos de *DataAnnotations* permiten adjuntar la lógica de validación a un modelo mediante atributos de metadatos. Los atributos de *DataAnnotations* se introdujeron en ASP.net datos dinámicos en ASP.net 3,5 SP1. Estos atributos se han integrado en el enlazador de modelos predeterminado y proporcionan un medio basado en metadatos para validar la entrada del usuario.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Aplicaciones auxiliares con plantilla

Las aplicaciones auxiliares con plantilla permiten asociar automáticamente plantillas de edición y visualización con tipos de datos. Por ejemplo, puede usar una aplicación auxiliar de plantilla para especificar que un elemento de la interfaz de usuario del selector de fecha se represente automáticamente para un valor *System. DateTime* . Esto es similar a las plantillas de campo en ASP.NET datos dinámicos.

Los métodos auxiliares *HTML. EditorFor* y *HTML. DisplayFor* tienen compatibilidad integrada para la representación de tipos de datos estándar, así como objetos complejos con varias propiedades. También personalizan la representación permitiéndole aplicar atributos de anotación de datos como *displayName* y *ScaffoldColumn* al objeto *ViewModel* .

A menudo, desea personalizar la salida de las aplicaciones auxiliares de la interfaz de usuario aún más y tener un control completo sobre lo que se genera. Los métodos auxiliares *HTML. EditorFor* y *HTML. DisplayFor* admiten esto mediante un mecanismo de plantillas que permite definir plantillas externas que pueden invalidar y controlar la salida representada. Las plantillas se pueden representar individualmente para una clase.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Datos dinámicos

Datos dinámicos se presentó en la versión de .NET Framework 3,5 SP1 en mediados de 2008. Esta característica proporciona muchas mejoras para la creación de aplicaciones controladas por datos, entre las que se incluyen las siguientes:

- Una experiencia de RAD para compilar rápidamente un sitio web controlado por datos.
- Validación automática que se basa en las restricciones definidas en el modelo de datos.
- La capacidad de cambiar fácilmente el marcado que se genera para los campos en los controles *GridView* y *DetailsView* mediante el uso de plantillas de campo que forman parte de su proyecto datos dinámicos.

> [!NOTE]
> Nota para obtener más información, vea la [documentación de datos dinámicos](https://msdn.microsoft.com/library/cc488545.aspx) en MSDN Library.

En el caso de ASP.NET 4, datos dinámicos se ha mejorado para ofrecer a los desarrolladores aún más capacidad para crear rápidamente sitios Web controlados por datos.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Habilitar datos dinámicos para proyectos existentes

Datos dinámicos características incluidas en el .NET Framework 3,5 SP1 incorporaban nuevas características, como las siguientes:

- Plantillas de campo: proporcionan plantillas basadas en el tipo de datos para los controles enlazados a datos. Las plantillas de campo proporcionan una forma más sencilla de personalizar la apariencia de los controles de datos que el uso de campos de plantilla para cada campo.
- Validación: datos dinámicos permite usar atributos en clases de datos para especificar la validación de escenarios comunes, como campos obligatorios, comprobación de intervalo, comprobación de tipos, coincidencia de patrones mediante expresiones regulares y validación personalizada. Los controles de datos exigen la validación.

Sin embargo, estas características tenían los siguientes requisitos:

- La capa de acceso a datos debía basarse en Entity Framework o LINQ to SQL.
- Los únicos controles de origen de datos admitidos para estas características eran los controles *EntityDataSource* o *LinqDataSource* .
- Las características requieren un proyecto web que se haya creado con las plantillas de entidades datos dinámicos o datos dinámicos para tener todos los archivos necesarios para admitir la característica.

Un objetivo importante de la compatibilidad de datos dinámicos en ASP.NET 4 es habilitar la nueva funcionalidad de datos dinámicos para cualquier aplicación de ASP.NET. En el ejemplo siguiente se muestra el marcado de los controles que pueden aprovechar la funcionalidad de datos dinámicos en una página existente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

En el código de la página, debe agregarse el código siguiente para habilitar la compatibilidad de datos dinámicos con estos controles:

[!code-csharp[Main](overview/samples/sample92.cs)]

Cuando el control *GridView* está en modo de edición, datos dinámicos valida automáticamente si los datos especificados tienen el formato correcto. Si no es así, se muestra un mensaje de error.

Esta funcionalidad también proporciona otras ventajas, como la posibilidad de especificar valores predeterminados para el modo de inserción. Sin datos dinámicos, para implementar un valor predeterminado para un campo, debe asociar a un evento, buscar el control (mediante *FindControl*) y establecer su valor. En ASP.NET 4, la llamada a *EnableDynamicData* admite un segundo parámetro que le permite pasar valores predeterminados para cualquier campo del objeto, como se muestra en este ejemplo:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintaxis de control declarativo de DynamicDataManager

El control *DynamicDataManager* se ha mejorado para que pueda configurarlo mediante declaración, como con la mayoría de los controles de ASP.net, en lugar de solo en el código. El marcado del control *DynamicDataManager* es similar al ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Este marcado permite datos dinámicos comportamiento del control GridView1 al que se hace referencia en la sección *datacontrols* del control *DynamicDataManager* .

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Plantillas de entidad

Las plantillas de entidad ofrecen una nueva forma de personalizar el diseño de los datos sin necesidad de crear una página personalizada. Las plantillas de página usan el control *FormView* (en lugar del control *DetailsView* , como se usa en las plantillas de página de versiones anteriores de datos dinámicos) y el control *DynamicEntity* para representar plantillas de entidad. Esto le proporciona más control sobre el marcado que se representa mediante datos dinámicos.

En la siguiente lista se muestra el nuevo diseño del directorio de proyecto que contiene plantillas de entidad:

[!code-console[Main](overview/samples/sample95.cmd)]

El directorio `EntityTemplate` contiene plantillas para mostrar los objetos del modelo de datos. De forma predeterminada, los objetos se representan mediante el `Default.ascx` plantilla, que proporciona el marcado que tiene el mismo aspecto que el marcado creado por el control *DetailsView* usado por datos dinámicos en ASP.net 3,5 SP1. En el ejemplo siguiente se muestra el marcado del control `Default.ascx`:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Las plantillas predeterminadas se pueden editar para cambiar la apariencia y el funcionamiento de todo el sitio. Hay plantillas para las operaciones de visualización, edición e inserción. Se pueden agregar nuevas plantillas en función del nombre del objeto de datos para cambiar la apariencia y el funcionamiento de un solo tipo de objeto. Por ejemplo, puede Agregar la siguiente plantilla:

[!code-console[Main](overview/samples/sample97.cmd)]

La plantilla podría contener el marcado siguiente:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Las nuevas plantillas de entidad se muestran en una página mediante el nuevo control *DynamicEntity* . En tiempo de ejecución, este control se reemplaza por el contenido de la plantilla de entidad. El marcado siguiente muestra el control *FormView* en la plantilla de página `Detail.aspx` que utiliza la plantilla de entidad. Observe el elemento *DynamicEntity* en el marcado.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Nuevas plantillas de campo para direcciones URL y direcciones de correo electrónico

ASP.NET 4 presenta dos nuevas plantillas de campo integradas, `EmailAddress.ascx` y `Url.ascx`. Estas plantillas se utilizan para los campos que están marcados como *EmailAddress* o *URL* con el atributo *DataType* . En el caso de los objetos *EmailAddress* , el campo se muestra como un hipervínculo que se crea mediante el protocolo *mailto:* . Cuando los usuarios hacen clic en el vínculo, se abre el cliente de correo electrónico del usuario y se crea un mensaje de esqueleto. Los objetos con el tipo de *dirección URL* se muestran como hipervínculos ordinarios.

En el ejemplo siguiente se muestra cómo se marcan los campos.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Crear vínculos con el control DynamicHyperLink

Datos dinámicos usa la nueva característica de enrutamiento que se agregó en el .NET Framework 3,5 SP1 para controlar las direcciones URL que los usuarios finales ven cuando acceden al sitio Web. El nuevo control *DynamicHyperLink* facilita la creación de vínculos a las páginas de un sitio datos dinámicos. En el ejemplo siguiente se muestra cómo usar el control *DynamicHyperLink* :

[!code-aspx[Main](overview/samples/sample101.aspx)]

Este marcado crea un vínculo que apunta a la página de lista de la tabla de `Products` basada en las rutas definidas en el archivo de `Global.asax`. El control utiliza automáticamente el nombre de tabla predeterminado en el que se basa la página datos dinámicos.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Compatibilidad con la herencia en el modelo de datos

Tanto el Entity Framework como el LINQ to SQL admiten la herencia en sus modelos de datos. Un ejemplo de esto podría ser una base de datos que tiene una tabla `InsurancePolicy`. También puede contener `CarPolicy` y `HousePolicy` tablas que tengan los mismos campos que `InsurancePolicy` y, a continuación, agregar más campos. Datos dinámicos se ha modificado para comprender los objetos heredados en el modelo de datos y para admitir la técnica scaffolding para las tablas heredadas.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Compatibilidad con relaciones de varios a varios (solo Entity Framework)

El Entity Framework tiene una amplia compatibilidad para las relaciones de varios a varios entre las tablas, que se implementa mediante la exposición de la relación como una colección en un objeto *entidad* . Se han agregado nuevas plantillas de campo `ManyToMany.ascx` y `ManyToMany_Edit.ascx` para proporcionar compatibilidad con la visualización y la edición de los datos implicados en las relaciones de varios a varios.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nuevos atributos para controlar la visualización y compatibilidad de enumeraciones

Se ha agregado *DisplayAttribute* para proporcionar un mayor control sobre cómo se muestran los campos. El atributo *displayName* de versiones anteriores de datos dinámicos permite cambiar el nombre que se usa como título de un campo. La nueva clase *DisplayAttribute* permite especificar más opciones para mostrar un campo, como el orden en que se muestra un campo y si un campo se utilizará como filtro. El atributo también proporciona control independiente del nombre usado para las etiquetas en un control *GridView* , el nombre utilizado en un control *DetailsView* , el texto de ayuda para el campo y la marca de agua utilizada para el campo (si el campo acepta la entrada de texto).

Se ha agregado la clase *EnumDataTypeAttribute* para permitir asignar campos a las enumeraciones. Al aplicar este atributo a un campo, se especifica un tipo de enumeración. Datos dinámicos usa la nueva plantilla de campo de `Enumeration.ascx` para crear la interfaz de usuario para mostrar y editar los valores de enumeración. La plantilla asigna los valores de la base de datos a los nombres de la enumeración.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Compatibilidad mejorada con filtros

Datos dinámicos 1,0 se incluye con filtros integrados para columnas booleanas y columnas de clave externa. Los filtros no le permiten especificar si se mostraban o en qué orden se mostraban. El nuevo atributo *DisplayAttribute* soluciona ambos problemas, ya que le permite controlar si una columna se muestra como filtro y en qué orden se mostrará.

Una mejora adicional es que se ha vuelto a escribir la compatibilidad de filtrado[para usar la nueva](#0.2__QueryExtender "_QueryExtender") característica de formularios Web Forms. Esto le permite crear filtros sin necesidad de conocer el control de origen de datos con el que se usarán los filtros. Junto con estas extensiones, los filtros también se han convertido en controles de plantilla, lo que permite agregar otros nuevos. Por último, la clase *DisplayAttribute* mencionada anteriormente permite que se invalide el filtro predeterminado, de la misma manera que *UIHint* permite que se invalide la plantilla de campo predeterminada para una columna.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Mejoras en el desarrollo web de Visual Studio 2010

El desarrollo web en Visual Studio 2010 se ha mejorado para una mayor compatibilidad con CSS, una mayor productividad a través de fragmentos de código de marcado HTML y ASP.NET y el nuevo JavaScript de IntelliSense dinámico.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilidad mejorada con CSS

El diseñador de Visual Web Developer de Visual Studio 2010 se ha actualizado para mejorar el cumplimiento de los estándares de CSS 2,1. El diseñador conserva mejor la integridad del origen HTML y es más eficaz que en versiones anteriores de Visual Studio. En el capó, también se han realizado mejoras arquitectónicas que permitirán mejorar las futuras mejoras en la representación, el diseño y la capacidad de servicio.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Fragmentos de código HTML y JavaScript

En el editor HTML, IntelliSense completa automáticamente los nombres de etiqueta. La característica de fragmentos de código de IntelliSense autocompleta las etiquetas completas y mucho más. En Visual Studio 2010, se admiten fragmentos de código de IntelliSense para C# JavaScript, junto con Visual Basic, que se admitían en versiones anteriores de Visual Studio.

Visual Studio 2010 incluye más de 200 fragmentos de código que le ayudarán a Autocompletar etiquetas HTML y ASP.NET comunes, incluidos los atributos necesarios (como runat = "Server") y los atributos comunes específicos de una etiqueta (como *ID*, *DataSourceID*, *ControlToValidate*y *Text*).

Puede descargar fragmentos de código adicionales o puede escribir sus propios fragmentos de código que encapsulan los bloques de marcado que usted o su equipo usan para tareas comunes.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Mejoras de IntelliSense para JavaScript

En Visual 2010, IntelliSense para JavaScript se ha rediseñado para proporcionar una experiencia de edición incluso más enriquecida. IntelliSense ahora reconoce los objetos generados dinámicamente por métodos como *registerNamespace* y por técnicas similares usadas por otros marcos de trabajo de JavaScript. El rendimiento se ha mejorado para analizar bibliotecas grandes de script y para mostrar IntelliSense con pocos o ningún retraso de procesamiento. La compatibilidad se ha aumentado drásticamente para admitir casi todas las bibliotecas de terceros y para admitir diversos estilos de codificación. Ahora, los comentarios de la documentación se analizan a medida que escribe y se aprovechan de forma inmediata con IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Implementación de aplicaciones web con Visual Studio 2010

Cuando los desarrolladores de ASP.NET implementan una aplicación Web, suelen encontrar problemas como los siguientes:

- La implementación en un sitio de hospedaje compartido requiere tecnologías como FTP, lo que puede ser lento. Además, debe realizar manualmente tareas como la ejecución de scripts SQL para configurar una base de datos y debe cambiar la configuración de IIS, como la configuración de una carpeta de directorio virtual como una aplicación.
- En un entorno empresarial, además de implementar los archivos de aplicación Web, los administradores con frecuencia deben modificar los archivos de configuración de ASP.NET y la configuración de IIS. Los administradores de bases de datos deben ejecutar una serie de scripts SQL para que se ejecute la base de datos de la aplicación. Estas instalaciones hacen un uso intensivo de trabajo, a menudo tardan horas en completarse y deben documentarse cuidadosamente.

Visual Studio 2010 incluye tecnologías que abordan estos problemas y que le permiten implementar aplicaciones web sin problemas. Una de estas tecnologías es la herramienta de implementación web de IIS (MsDeploy. exe).

Las características de implementación web en Visual Studio 2010 incluyen las siguientes áreas principales:

- Empaquetado Web
- Transformación de Web. config
- Implementación de base de datos
- Publicación con un solo clic para aplicaciones Web

En las secciones siguientes se proporcionan detalles acerca de estas características.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empaquetado Web

Visual Studio 2010 usa la herramienta MSDeploy para crear un archivo comprimido (. zip) para la aplicación, al que se hace referencia como un *paquete web*. El archivo de paquete contiene metadatos acerca de la aplicación más el siguiente contenido:

- Configuración de IIS, que incluye la configuración del grupo de aplicaciones, la configuración de la página de error, etc.
- Contenido web real, que incluye páginas web, controles de usuario, contenido estático (imágenes y archivos HTML), etc.
- SQL Server los datos y los esquemas de la base de datos.
- Certificados de seguridad, componentes para instalar en la GAC, configuración del registro, etc.

Un paquete Web se puede copiar en cualquier servidor y, a continuación, instalar manualmente mediante el administrador de IIS. Como alternativa, para la implementación automatizada, el paquete se puede instalar mediante comandos de la línea de comandos o mediante las API de implementación.

Visual Studio 2010 proporciona tareas y destinos de MSBuild integrados para crear paquetes Web. Para obtener más información, vea [información general sobre la implementación de proyectos de aplicación Web de ASP.net](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) en el sitio web de MSDN y [10 + 20 razones por las que debe crear un paquete web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformación de Web. config

En la implementación de aplicaciones Web, Visual Studio 2010 presenta la [transformación de documentos XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), que es una característica que permite transformar un archivo `Web.config` de la configuración de desarrollo a la configuración de producción. La configuración de la transformación se especifica en los archivos de transformación denominados `web.debug.config`, `web.release.config`, etc. (Los nombres de estos archivos coinciden con las configuraciones de MSBuild). Un archivo de transformación incluye solo los cambios que debe realizar en un archivo de `Web.config` implementado. Los cambios se especifican mediante la sintaxis simple.

En el ejemplo siguiente se muestra una parte de un archivo `web.release.config` que se puede producir para la implementación de la configuración de lanzamiento. La palabra clave Replace en el ejemplo especifica que durante la implementación, el nodo *ConnectionString* del archivo `Web.config` se reemplazará por los valores que se enumeran en el ejemplo.

[!code-xml[Main](overview/samples/sample102.xml)]

Para obtener más información, vea [Sintaxis de transformación de Web. config para la implementación de proyectos de aplicación web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) en el sitio web de MSDN <a id="0.2_a"></a> y[Web Deployment: transformación de Web. config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Implementación de base de datos

Un paquete de implementación de Visual Studio 2010 puede incluir dependencias en bases de datos de SQL Server. Como parte de la definición del paquete, debe proporcionar la cadena de conexión de la base de datos de origen. Al crear el paquete Web, Visual Studio 2010 crea scripts SQL para el esquema de la base de datos y, opcionalmente, para los datos y, a continuación, los agrega al paquete. También puede proporcionar scripts SQL personalizados y especificar la secuencia en la que deben ejecutarse en el servidor. En el momento de la implementación, se proporciona una cadena de conexión adecuada para el servidor de destino. a continuación, el proceso de implementación usa esta cadena de conexión para ejecutar los scripts que crean el esquema de la base de datos y agregan los datos.

Además, mediante la publicación con un solo clic, puede configurar la implementación para publicar la base de datos directamente cuando la aplicación se publica en un sitio de hospedaje compartido remoto. Para obtener más información, consulte [Cómo: implementar una base de datos con un proyecto de aplicación web](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) en el sitio web de MSDN y la [implementación de bases de datos con vs 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publicación con un solo clic para aplicaciones Web

Visual Studio 2010 también le permite usar el servicio de administración remota de IIS para publicar una aplicación web en un servidor remoto. Puede crear un perfil de publicación para su cuenta de hospedaje o para servidores de prueba o servidores de ensayo. Cada perfil puede guardar las credenciales adecuadas de forma segura. Después, puede realizar la implementación en cualquiera de los servidores de destino con un solo clic mediante la barra de herramientas publicar con un solo clic Web. Con Visual Studio 2010, también puede publicar mediante la línea de comandos de MSBuild. Esto le permite configurar el entorno de Team Build para incluir la publicación en un modelo de integración continua.

Para obtener más información, consulte [Cómo: implementar un proyecto de aplicación Web mediante la publicación con un solo clic y Web deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) en el sitio web de MSDN y [Web 1: haga clic en publicar con vs 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) en el blog de Vishal Joshi. Para ver presentaciones en vídeo sobre la implementación de aplicaciones web en Visual Studio 2010, consulte [VS 2010 para vistas previas de Desarrollador Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Recursos

Los siguientes sitios web proporcionan información adicional sobre ASP.NET 4 y Visual Studio 2010.

- [ASP.net 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) : la documentación oficial de ASP.net 4 en el sitio web de MSDN.
- [https://www.asp.net/](https://www.asp.net/) : el sitio web del equipo de ASP.net.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) y [ASP.net datos dinámicos mapa de contenido](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) : recursos en línea en el sitio de grupo de ASP.net y en la documentación oficial de ASP.net datos dinámicos.
- [https://www.asp.net/ajax/](../../ajax/index.md) : el recurso Web principal para el desarrollo de ASP.NET AJAX.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) : el blog del equipo de Visual Web Developer, que incluye información sobre las características de visual Studio 2010.
- [ASP.net webstack](https://github.com/aspnet/AspNetWebStack) : el recurso Web principal para las versiones de vista previa de ASP.net.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, las compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y eventos que se describen aquí son ficticios y no tienen ninguna asociación con ninguna empresa, organización, producto, nombre de dominio y correo electrónico reales. la dirección, el logotipo, la persona, el lugar o el evento está pensado o debe deducirse.

© 2009 Microsoft Corporation. All rights reserved.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
