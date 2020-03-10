---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: Novedades de ASP.NET 4,5 y Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: En este documento se describen las nuevas características y mejoras que se introducen en ASP.NET 4,5. También se describen las mejoras que se realizan para el desarrollo web...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 32fbf7c25b00f3f0796c4c3fdd38ca2a86c89199
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422737"
---
# <a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>Novedades de ASP.NET 4.5 y Visual Studio 2012

> En este documento se describen las nuevas características y mejoras que se introducen en ASP.NET 4,5. También se describen las mejoras que se realizan para el desarrollo web en Visual Studio 2012. Este documento se publicó originalmente el 29 de febrero de 2012.

- [Tiempo de ejecución y marco de ASP.NET Core](#_Toc318097372)

    - [Lectura y escritura asincrónicas de solicitudes y respuestas HTTP](#_Toc318097373)
    - [Mejoras en el control de HttpRequest](#_Toc318097374)
    - [Vaciado de una respuesta de forma asincrónica](#_Toc318097375)
    - [Compatibilidad con los módulos y controladores asincrónicos basados en *tareas*y *Await*](#_Toc318097376)
    - [Módulos HTTP asíncronos](#_Toc318097377)
    - [Controladores HTTP asincrónicos](#_Toc318097378)
    - [Nuevas características de validación de solicitudes de ASP.NET](#_Toc318097379)
    - [Validación de solicitud diferida ("diferida")](#_Toc318097380)
    - [Compatibilidad con solicitudes no validadas](#_Toc318097381)
    - [Biblioteca AntiXSS](#_Toc318097382)
    - [Compatibilidad con el protocolo WebSockets](#_Toc318097383)
    - [Unión y minificación](#_Toc318097384)
    - [Mejoras de rendimiento para el hospedaje Web](#_Toc_perf)

        - [Factores clave de rendimiento](#_Toc_perf_1)
        - [Requisitos para las nuevas características de rendimiento](#_Toc_perf_2)
        - [Compartir ensamblados comunes](#_Toc_perf_3)
        - [Uso de la compilación JIT de varios núcleos para un inicio más rápido](#_Toc_perf_4)
        - [Optimización de la recolección de elementos no utilizados para optimizar la memoria](#_Toc_perf_5)
        - [Captura previa de aplicaciones Web](#_Toc_perf_6)
- [Formularios Web Forms ASP.NET](#_Toc318097385)

    - [Controles de datos fuertemente tipados](#_Toc318097386)
    - [Enlace de modelos](#_Toc318097387)

        - [Seleccionar datos](#_Toc318097388)
        - [Proveedores de valores](#_Toc318097389)
        - [Filtrar por valores de un control](#_Toc318097390)
    - [Expresiones de enlace de datos codificadas en HTML](#_Toc318097391)
    - [Validación discreta](#_Toc318097392)
    - [Actualizaciones de HTML5](#_Toc318097393)
- [ASP.NET MVC 4](#_Toc318097394)
- [ASP.NET Web Pages 2](#_Toc318097395)
- [Candidato de versión comercial de Visual Studio 2012](#_Toc318097396)

    - [Uso compartido de proyectos entre Visual Studio 2010 y Visual Studio 2012 Release Candidate (compatibilidad de proyectos)](#project-compatibility)
    - [Cambios de configuración en las plantillas de sitio web de ASP.NET 4,5](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Compatibilidad nativa en IIS 7 para el enrutamiento de ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor de HTML](#_Toc318097397)

        - [Tareas inteligentes](#_Toc318097398)
        - [Compatibilidad con WAI-ARIA](#_Toc318097399)
        - [Nuevos fragmentos de código HTML5](#_Toc318097400)
        - [Extraer a control de usuario](#_Toc318097401)
        - [IntelliSense para código de fragmentos en atributos](#_Toc318097402)
        - [Cambio de nombre automático de la etiqueta coincidente al cambiar el nombre de una etiqueta de apertura o de cierre](#_Toc318097403)
        - [Generación de controladores de eventos](#_Toc318097404)
        - [Sangría inteligente](#_Toc318097405)
        - [Finalización automática de instrucciones de reducción automática](#_Toc318097406)
    - [Editor de JavaScript](#_Toc318097407)

        - [Esquematización de código](#_Toc318097408)
        - [Coincidencia de llaves](#_Toc318097409)
        - [Ir a definición](#_Toc318097410)
        - [Compatibilidad con ECMAScript5](#_Toc318097411)
        - [IntelliSense para DOM](#_Toc318097412)
        - [Sobrecargas de firma de VSDOC](#_Toc318097413)
        - [Referencias implícitas](#_Toc318097414)
    - [Editor CSS](#_Toc318097415)

        - [Finalización automática de instrucciones de reducción automática](#_Toc318097416)
        - [Sangría jerárquica.](#_Toc318097417)
        - [Compatibilidad con la piratería de CSS](#_Toc318097418)
        - [Esquemas específicos del proveedor (-moz-,-WebKit)](#_Toc318097419)
        - [Compatibilidad con comentarios y quitar comentarios](#_Toc318097420)
        - [Selector de colores](#_Toc318097421)
        - [Fragmentos de código](#_Toc318097422)
        - [Regiones personalizadas](#_Toc318097423)
    - [Inspector de página](#_Toc318097424)
    - [Publicación](#_Toc318097425)

        - [Perfiles de publicación](#_Toc318097426)
        - [ASP.NET precompilar y fusionar mediante combinación](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Ausencia](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Tiempo de ejecución y marco de ASP.NET Core

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Lectura y escritura asincrónicas de solicitudes y respuestas HTTP

ASP.NET 4 presentó la capacidad de leer una entidad de solicitud HTTP como una secuencia mediante el método *HttpRequest. GetBufferlessInputStream* . Este método proporcionó acceso de transmisión por secuencias a la entidad de solicitud. Sin embargo, se ejecutó sincrónicamente, que asocia un subproceso para la duración de una solicitud.

ASP.NET 4,5 admite la capacidad de leer flujos de forma asincrónica en una entidad de solicitud HTTP y la capacidad de vaciar de forma asincrónica. ASP.NET 4,5 también ofrece la posibilidad de almacenar en búfer una entidad de solicitud HTTP, lo que facilita la integración con controladores HTTP de bajada, como controladores de páginas. aspx y controladores de ASP.NET MVC.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Mejoras en el control de HttpRequest

La referencia de secuencia devuelta por ASP.NET 4,5 desde *HttpRequest. GetBufferlessInputStream* admite métodos de lectura sincrónicos y asincrónicos. El objeto de *secuencia* devuelto desde *GetBufferlessInputStream* ahora implementa los métodos BeginRead y EndRead. Los métodos de *flujo* asincrónico permiten leer de forma asincrónica la entidad de solicitud en fragmentos, mientras que ASP.net libera el subproceso actual entre cada iteración de un bucle de lectura asincrónico.

ASP.NET 4,5 también ha agregado un método complementario para leer la entidad solicitud de una manera almacenada en búfer: *HttpRequest. GetBufferedInputStream*. Esta nueva sobrecarga funciona como *GetBufferlessInputStream*, lo que admite lecturas sincrónicas y asincrónicas. Sin embargo, a medida que lee, *GetBufferedInputStream* también copia los bytes de la entidad en los búferes internos de ASP.net para que los módulos y controladores de nivel inferior sigan teniendo acceso a la entidad de solicitud. Por ejemplo, si algún código de nivel superior de la canalización ya ha leído la entidad solicitud mediante *GetBufferedInputStream*, todavía puede usar *HttpRequest. Form* o *HttpRequest. files*. Esto le permite realizar el procesamiento asincrónico en una solicitud (por ejemplo, transmitir por secuencias una carga de archivos grandes a una base de datos), pero seguir ejecutando las páginas. aspx y los controladores ASP.NET de MVC después.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Vaciado de una respuesta de forma asincrónica

El envío de respuestas a un cliente HTTP puede tardar un tiempo considerable cuando el cliente está lejos o tiene una conexión de ancho de banda bajo. Normalmente ASP.NET almacena en búfer los bytes de respuesta tal como los crea una aplicación. A continuación, ASP.NET realiza una operación de envío única de los búferes acumulados al final del procesamiento de la solicitud.

Si la respuesta almacenada en búfer es grande (por ejemplo, al transmitir un archivo grande a un cliente), debe llamar periódicamente a *HttpResponse. Flush* para enviar la salida almacenada en búfer al cliente y mantener el uso de memoria bajo control. Sin embargo, dado que *Flush* es una llamada sincrónica, la llamada iterativa a *Flush* todavía consume un subproceso para la duración de las solicitudes de ejecución prolongada.

ASP.NET 4,5 agrega compatibilidad para realizar vaciados de forma asincrónica mediante los métodos *BeginFlush* y *EndFlush* de la clase *HttpResponse* . Con estos métodos, puede crear módulos asincrónicos y controladores asincrónicos que envíen datos incrementalmente a un cliente sin necesidad de utilizar subprocesos del sistema operativo. En entre las llamadas a *BeginFlush* y *EndFlush* , ASP.net libera el subproceso actual. Esto reduce considerablemente el número total de subprocesos activos necesarios para admitir descargas HTTP de ejecución prolongada.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Compatibilidad con los módulos y controladores asincrónicos basados en *tareas* y *Await*

El .NET Framework 4 presentó un concepto de programación asincrónica denominado *tarea*. Las tareas se representan mediante el tipo de *tarea* y los tipos relacionados en el espacio de nombres *System. Threading. Tasks* . El .NET Framework 4,5 se basa en esto con mejoras del compilador que facilitan el trabajo con objetos de *tarea* . En el .NET Framework 4,5, los compiladores admiten dos nuevas palabras clave: *Await* y *Async*. La palabra clave *Await* es una abreviatura sintáctica para indicar que un fragmento de código debe esperar de forma asincrónica en algún otro fragmento de código. La palabra clave *Async* representa una sugerencia que se puede usar para marcar métodos como métodos asincrónicos basados en tareas.

La combinación de *Await*, *Async*y el objeto de *tarea* hace que sea mucho más fácil escribir código asincrónico en .net 4,5. ASP.NET 4,5 admite estas simplificaciones con nuevas API que permiten escribir módulos HTTP asincrónicos y controladores HTTP asincrónicos mediante las nuevas mejoras del compilador.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Módulos HTTP asíncronos

Supongamos que desea realizar el trabajo asincrónico dentro de un método que devuelve un objeto de *tarea* . En el ejemplo de código siguiente se define un método asincrónico que realiza una llamada asincrónica para descargar la Página principal de Microsoft. Observe el uso de la palabra clave *Async* en la Signatura del método y la llamada *Await* a *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Eso es todo lo que tiene que escribir: el .NET Framework controlará automáticamente el desenredado de la pila de llamadas mientras espera a que se complete la descarga, así como la restauración automática de la pila de llamadas una vez finalizada la descarga.

Supongamos ahora que desea usar este método asincrónico en un módulo HTTP ASP.NET asincrónico. ASP.NET 4,5 incluye un método auxiliar (*EventHandlerTaskAsyncHelper*) y un nuevo tipo de delegado (*TaskEventHandler*) que puede usar para integrar métodos asincrónicos basados en tareas con el modelo de programación asincrónico anterior expuesto por la canalización http ASP.net. En este ejemplo se muestra cómo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Controladores HTTP asincrónicos

El enfoque tradicional para escribir controladores asincrónicos en ASP.NET es implementar la interfaz *IHttpAsyncHandler* . ASP.NET 4,5 presenta el tipo base asincrónico *HttpTaskAsyncHandler* del que puede derivar, lo que facilita la escritura de controladores asincrónicos.

El tipo *HttpTaskAsyncHandler* es abstracto y requiere invalidar el método *ProcessRequestAsync* . Internamente, ASP.NET se encarga de integrar la firma devuelta (un objeto de *tarea* ) de *ProcessRequestAsync* con el modelo de programación asincrónico anterior que usa la canalización ASP.net.

En el ejemplo siguiente se muestra cómo se puede usar *Task* y *Await* como parte de la implementación de un controlador http asincrónico:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Nuevas características de validación de solicitudes de ASP.NET

De forma predeterminada, ASP.NET realiza la validación de solicitudes: examina las solicitudes para buscar marcado o script en campos, encabezados, cookies, etc. Si se detecta alguno, ASP.NET produce una excepción. Esto actúa como la primera línea de defensa frente a posibles ataques de scripting entre sitios.

ASP.NET 4,5 facilita la lectura selectiva de datos de solicitud no validados. ASP.NET 4,5 también integra la popular biblioteca de AntiXSS, que antes era una biblioteca externa.

A menudo, los desarrolladores han solicitado la posibilidad de desactivar selectivamente la validación de solicitudes para sus aplicaciones. Por ejemplo, si la aplicación es software de foro, es posible que desee permitir que los usuarios envíen comentarios y publicaciones de foros con formato HTML, pero asegúrese de que la validación de solicitudes está comprobando todo lo demás.

ASP.NET 4,5 presenta dos características que facilitan el trabajo selectivo con la entrada no validada: la validación de la solicitud diferida ("diferida") y el acceso a los datos de solicitud no validados.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Validación de solicitud diferida ("diferida")

De forma predeterminada, en ASP.NET 4,5, todos los datos de solicitud están sujetos a la validación de solicitudes. Sin embargo, puede configurar la aplicación para diferir la validación de solicitudes hasta que realmente tenga acceso a los datos de la solicitud. (Esto se conoce a veces como validación de solicitudes diferidas, basándose en términos como la carga diferida para determinados escenarios de datos). Puede configurar la aplicación para usar la validación diferida en el archivo Web. config estableciendo el atributo *requestValidationMode* en 4,5 en el elemento *httpRUntime* , como en el ejemplo siguiente:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Cuando el modo de validación de solicitudes se establece en 4,5, la validación de solicitudes solo se desencadena para un valor de solicitud específico y solo cuando el código tiene acceso a ese valor. Por ejemplo, si el código obtiene el valor de request. Form ["Forum\_post"], la validación de solicitudes solo se invoca para ese elemento en la colección de formularios. No se valida ningún otro elemento de la colección de *formularios* . En versiones anteriores de ASP.NET, se desencadenó la validación de solicitudes para toda la colección de solicitudes cuando se tenía acceso a cualquier elemento de la colección. El nuevo comportamiento facilita que los distintos componentes de la aplicación examinen diferentes partes de los datos de la solicitud sin activar la validación de solicitudes en otras partes.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Compatibilidad con solicitudes no validadas

La validación de solicitud diferida no soluciona el problema de omitir selectivamente la validación de solicitudes. La llamada a request. Form ["Forum\_post"] sigue desencadenando la validación de solicitudes para ese valor de solicitud específico. Sin embargo, es posible que desee tener acceso a este campo sin desencadenar la validación porque desea permitir el marcado en ese campo.

Para permitir esto, ASP.NET 4,5 ahora admite el acceso no validado a los datos de solicitud. ASP.NET 4,5 incluye una nueva propiedad de colección no *validada* en la clase *HttpRequest* . Esta colección proporciona acceso a todos los valores comunes de datos de solicitud, como *Form*, *QueryString*, *cookies*y *URL*.

En el ejemplo del foro, para poder leer datos de solicitudes no validados, primero debe configurar la aplicación para que use el nuevo modo de validación de solicitudes:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Después, puede usar la propiedad *HttpRequest. unvalidated* para leer el valor de formulario no validado:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]

> [!WARNING]
> Seguridad: *use datos de solicitud sin validar con cuidado.* ASP.NET 4,5 agregó las colecciones y propiedades de solicitud no validadas para que sea más fácil acceder a datos de solicitud no validados muy específicos. Sin embargo, todavía debe realizar la validación personalizada en los datos de la solicitud sin procesar para asegurarse de que el texto peligroso no se represente en los usuarios.

<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteca AntiXSS

Debido a la popularidad de la biblioteca AntiXSS de Microsoft, ASP.NET 4,5 ahora incorpora rutinas de codificación básicas de la versión 4,0 de dicha biblioteca.

Las rutinas de codificación se implementan mediante el tipo *AntiXssEncoder* en el nuevo espacio de nombres *System. Web. Security. AntiXss* . Puede usar el tipo *AntiXssEncoder* directamente llamando a cualquiera de los métodos de codificación estáticos que se implementan en el tipo. Sin embargo, el enfoque más sencillo para usar las nuevas rutinas AntiXSS es configurar una aplicación de ASP.NET para que use la clase *AntiXssEncoder* de forma predeterminada. Para ello, agregue el siguiente atributo al archivo Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Cuando el atributo *encoderType* está establecido para usar el tipo *AntiXssEncoder* , toda la codificación de salida en ASP.net utiliza automáticamente las nuevas rutinas de codificación.

Estas son las partes de la biblioteca AntiXSS externa que se han incorporado en ASP.NET 4,5:

- *HtmlEncode*, *HtmlFormUrlEncode*y *HtmlAttributeEncode*
- *XmlAttributeEncode* y *XmlEncode*
- *UrlEncode* y *UrlPathEncode* (New)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Compatibilidad con el protocolo WebSockets

El protocolo WebSockets es un protocolo de red basado en estándares que define cómo establecer comunicaciones bidireccionales seguras y en tiempo real entre un cliente y un servidor a través de HTTP. Microsoft ha trabajado con los cuerpos de estándares IETF y W3C para ayudar a definir el protocolo. El protocolo WebSockets es compatible con cualquier cliente (no solo con exploradores), con Microsoft que invierte recursos sustanciales que admiten el protocolo WebSockets en los sistemas operativos cliente y móvil.

El protocolo WebSockets facilita enormemente la creación de transferencias de datos de ejecución prolongada entre un cliente y un servidor. Por ejemplo, la escritura de una aplicación de chat es mucho más sencilla, ya que puede establecer una conexión de ejecución prolongada verdadera entre un cliente y un servidor. No tiene que recurrir a soluciones alternativas como el sondeo periódico o el sondeo de HTTP Long para simular el comportamiento de un socket.

ASP.NET 4,5 e IIS 8 incluyen compatibilidad con WebSockets de bajo nivel, lo que permite a los desarrolladores de ASP.NET usar API administradas para leer y escribir asincrónicamente datos de cadena y binarios en un objeto WebSockets. Para ASP.NET 4,5, hay un nuevo espacio de nombres *System. Web. WebSockets* que contiene tipos para trabajar con el protocolo WebSockets.

Un cliente de explorador establece una conexión de WebSockets mediante la creación de un objeto DOM *WebSocket* que apunta a una dirección URL en una aplicación ASP.net, como en el ejemplo siguiente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Puede crear puntos de conexión de WebSockets en ASP.NET con cualquier tipo de módulo o controlador. En el ejemplo anterior, se usaba un archivo. ashx, ya que los archivos. ashx son una forma rápida de crear un controlador.

Según el protocolo WebSockets, una aplicación de ASP.NET acepta la solicitud WebSockets de un cliente indicando que la solicitud se debe actualizar desde una solicitud HTTP GET a una solicitud WebSockets. Por ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

El método *AcceptWebSocketRequest* acepta un delegado de función porque ASP.net desenreda la solicitud HTTP actual y, a continuación, transfiere el control al delegado de función. Conceptualmente, este enfoque es similar al uso de *System. Threading. Thread*, donde se define un delegado de inicio del subproceso en el que se realiza el trabajo en segundo plano.

Después de que ASP.NET y el cliente hayan completado correctamente un protocolo de enlace de WebSockets, ASP.NET llamará al delegado y la aplicación WebSockets comenzará a ejecutarse. En el ejemplo de código siguiente se muestra una aplicación de eco sencilla que usa la compatibilidad integrada de WebSockets en ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

La compatibilidad en .NET 4,5 para la palabra clave *Await* y las operaciones asincrónicas basadas en tareas es una opción natural para escribir aplicaciones de WebSockets. En el ejemplo de código se muestra que una solicitud WebSockets se ejecuta completamente de forma asincrónica dentro de ASP.NET. La aplicación espera de forma asincrónica que se envíe un mensaje desde un cliente mediante una llamada a *Await Socket. ReceiveAsync*. De forma similar, puede enviar un mensaje asincrónico a un cliente mediante una llamada a *Await Socket. SendAsync*.

En el explorador, una aplicación recibe mensajes de WebSockets a través de una función de *mensaje* . Para enviar un mensaje desde un explorador, se llama al método *send* del tipo DOM de *WebSocket* , tal como se muestra en este ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

En el futuro, podríamos publicar actualizaciones en esta funcionalidad que abstraen parte de la codificación de bajo nivel que se requiere en esta versión para aplicaciones WebSockets.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Unión y minificación

La agrupación permite combinar archivos CSS y JavaScript individuales en una agrupación que se puede tratar como un solo archivo. Minificación condensa los archivos JavaScript y CSS quitando el espacio en blanco y otros caracteres que no son necesarios. Estas características funcionan con formularios Web Forms, ASP.NET MVC y Web pages.

Los paquetes se crean mediante la clase de paquete o una de sus clases secundarias, ScriptBundle y StyleBundle. Después de configurar una instancia de una agrupación, la agrupación se pone a disposición de las solicitudes entrantes simplemente agregándola a una instancia global de BundleCollection. En las plantillas predeterminadas, la configuración de la agrupación se realiza en un archivo BundleConfig. Esta configuración predeterminada crea agrupaciones para todos los scripts principales y archivos CSS que usan las plantillas.

Se hace referencia a las agrupaciones desde las vistas mediante uno de los dos métodos auxiliares posibles. Con el fin de admitir la representación de un marcado diferente para una agrupación en el modo de depuración y versión, las clases ScriptBundle y StyleBundle tienen el método auxiliar, render. En el modo de depuración, render generará el marcado para cada recurso de la agrupación. En el modo de versión, render generará un único elemento de marcado para todo el paquete. La alternancia entre el modo de depuración y versión se puede realizar modificando el atributo Debug del elemento Compilation en Web. config como se muestra a continuación:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Además, la habilitación o deshabilitación de la optimización se puede establecer directamente a través de la propiedad BundleTable. EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Cuando los archivos están agrupados, se ordenan primero alfabéticamente (tal como se muestran en **Explorador de soluciones**). A continuación, se organizan de modo que se carguen primero las bibliotecas conocidas y sus extensiones personalizadas (como jQuery, MooTools y Dojo). Por ejemplo, el orden final de la agrupación de la carpeta scripts, como se muestra arriba, será:

1. jquery-1.6.2.js
2. jquery-ui.js
3. jquery.tools.js
4. a.js

Los archivos CSS también se ordenan alfabéticamente y, a continuación, se reorganizan de modo que RESET. CSS y Normalize. CSS preceden a cualquier otro archivo. La ordenación final de la agrupación de la carpeta de estilos mostrada anteriormente será la siguiente:

1. reset.css
2. contenido. CSS
3. Forms. CSS
4. globals.css
5. menú. CSS
6. styles.css

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Mejoras de rendimiento para el hospedaje Web

Las .NET Framework 4,5 y Windows 8 presentan características que pueden ayudarle a lograr una mejora significativa en el rendimiento de las cargas de trabajo Web-Server. Esto incluye una reducción (hasta 35%) tanto en el momento de inicio como en la superficie de memoria de los sitios de hospedaje web que usan ASP.NET.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Factores clave de rendimiento

Idealmente, todos los sitios web deben estar activos y en memoria para garantizar una respuesta rápida a la solicitud siguiente, siempre que proceda. Entre los factores que pueden afectar a la capacidad de respuesta del sitio se incluyen:

- El tiempo que tarda un sitio en reiniciarse después de que se recicla un grupo de aplicaciones. Este es el tiempo que se tarda en iniciar un proceso de servidor web para el sitio cuando los ensamblados de sitio ya no están en la memoria. (Los ensamblados de la plataforma todavía están en memoria, ya que se usan en otros sitios). Esta situación se conoce como "sitio en frío, Inicio de marco cálido" o simplemente "Inicio de sitio en frío".
- Cuánta memoria ocupa el sitio. Los términos de esto son "consumo de memoria por sitio" o "espacio de trabajo no compartido".

Las nuevas mejoras de rendimiento se centran en ambos factores.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisitos para las nuevas características de rendimiento

Los requisitos para las nuevas características se pueden dividir en estas categorías:

- Mejoras que se ejecutan en el .NET Framework 4.
- Mejoras que requieren el .NET Framework 4,5 pero se pueden ejecutar en cualquier versión de Windows.
- Mejoras que solo están disponibles con .NET Framework 4,5 que se ejecutan en Windows 8.

El rendimiento aumenta con cada nivel de mejora que se puede habilitar.

Algunas de las mejoras .NET Framework 4,5 aprovechan las características de rendimiento más amplias que se aplican también a otros escenarios.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Compartir ensamblados comunes

**Requisito**: SDK de .NET Framework 4 y Visual Studio 11 Developer Preview

Los distintos sitios de un servidor suelen usar los mismos ensamblados auxiliares (por ejemplo, ensamblados de una aplicación de Starter Kit o de ejemplo). Cada sitio tiene su propia copia de estos ensamblados en su directorio bin. Aunque el código de objeto de los ensamblados es idéntico, son ensamblados físicamente independientes, por lo que cada ensamblado debe leerse por separado durante el inicio del sitio en frío y mantenerse por separado en la memoria.

La nueva funcionalidad de interning resuelve esta ineficacia y reduce los requisitos de RAM y el tiempo de carga. El interning permite a Windows mantener una única copia de cada ensamblado en el sistema de archivos, y los ensamblados individuales de las carpetas bin del sitio se reemplazan por vínculos simbólicos a la única copia. Si un sitio individual necesita una versión distinta del ensamblado, el vínculo simbólico se reemplaza con la nueva versión del ensamblado y solo se ve afectado ese sitio.

El uso compartido de los ensamblados mediante vínculos simbólicos requiere una nueva herramienta llamada ASPNET\_Intern. exe, que le permite crear y administrar el almacén de ensamblados de internd. Se proporciona como parte del SDK de Visual Studio 11 Developer Preview. (Sin embargo, funcionará en un sistema que tenga solo el .NET Framework 4 instalado, suponiendo que haya instalado la [actualización](https://support.microsoft.com/kb/2468871)más reciente).

Para asegurarse de que todos los ensamblados válidos se han internó, ejecute ASPNET\_Intern. exe periódicamente (por ejemplo, una vez a la semana como tarea programada). Un uso típico es el siguiente:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Para ver todas las opciones, ejecute la herramienta sin argumentos.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Uso de la compilación JIT de varios núcleos para un inicio más rápido

**Requisito**: .NET Framework 4,5

En el inicio de un sitio en frío, no solo los ensamblados deben leerse desde el disco, pero el sitio debe estar compilado JIT. Para un sitio complejo, esto puede Agregar retrasos importantes. Una nueva técnica de uso general en el .NET Framework 4,5 reduce estos retrasos mediante la propagación de la compilación JIT entre núcleos de procesador disponibles. Lo hace tanto como sea lo antes posible mediante el uso de la información recopilada durante los lanzamientos anteriores del sitio. Esta funcionalidad se implementa mediante el método [System. Runtime. ProfileOptimization. StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) .

La compilación JIT con varios núcleos está habilitada de forma predeterminada en ASP.NET, por lo que no es necesario hacer nada para aprovechar esta característica. Si desea deshabilitar esta característica, realice la siguiente configuración en el archivo Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Optimización de la recolección de elementos no utilizados para optimizar la memoria

**Requisito**: .NET Framework 4,5

Una vez que se ejecuta un sitio, el uso del montón del recolector de elementos no utilizados (GC) puede ser un factor importante en el consumo de memoria. Al igual que cualquier recolector de elementos no utilizados, el .NET Framework GC realiza un equilibrio entre el tiempo de CPU (frecuencia y la importancia de las colecciones) y el consumo de memoria (espacio adicional que se usa para los objetos nuevos, liberados o disponibles). En las versiones anteriores, hemos proporcionado instrucciones sobre cómo configurar el GC para lograr el equilibrio correcto (por ejemplo, vea [ASP.net 2.0/3.5 Shared Hosting Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

En el .NET Framework 4,5, en lugar de varios valores de configuración independientes, hay disponible una opción de configuración definida por la carga de trabajo que habilita todas las opciones de configuración de GC recomendadas anteriormente, así como nuevas optimizaciones que ofrecen un rendimiento adicional para cada sitio. espacio de trabajo.

Para habilitar la optimización de memoria de GC, agregue la siguiente configuración al archivo Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Si está familiarizado con la guía anterior de los cambios en Aspnet. config, tenga en cuenta que esta configuración reemplaza la configuración anterior; por ejemplo, no es necesario establecer gcServer, gcConcurrent, etc. No es necesario quitar la configuración anterior).

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>Captura previa de aplicaciones Web

**Requisito**: .NET Framework 4,5 que se ejecuta en Windows 8

En varias versiones, Windows ha incluido una tecnología conocida como [captura previa](http://en.wikipedia.org/wiki/Prefetcher) que reduce el costo de lectura del disco de inicio de la aplicación. Dado que el inicio en frío es un problema principalmente en el caso de las aplicaciones cliente, esta tecnología no se ha incluido en Windows Server, que incluye solo los componentes que son esenciales para un servidor. La captura previa ahora está disponible en la versión más reciente de Windows Server, donde puede optimizar el lanzamiento de sitios web individuales.

En Windows Server, la captura previa no está habilitada de forma predeterminada. Para habilitar y configurar el captura previa para el hospedaje Web de alta densidad, ejecute el siguiente conjunto de comandos en la línea de comandos:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Después, para integrar el precargador con las aplicaciones de ASP.NET, agregue lo siguiente al archivo Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controles de datos fuertemente tipados

En ASP.NET 4,5, Web Forms incluye algunas mejoras para trabajar con datos. La primera mejora son los controles de datos fuertemente tipados. En el caso de los controles de formularios Web Forms en versiones anteriores de ASP.NET, se muestra un valor enlazado a datos mediante *eval* y una expresión de enlace de datos:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Para el enlace de datos bidireccional, se usa *BIND*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

En tiempo de ejecución, estas llamadas usan la reflexión para leer el valor del miembro especificado y, a continuación, muestran el resultado en el marcado. Este enfoque facilita el enlace de datos con datos arbitrarios y no formados.

Sin embargo, las expresiones de enlace de datos como esta no admiten características como IntelliSense para nombres de miembro, navegación (como ir a definición) o comprobación en tiempo de compilación para estos nombres.

Para solucionar este problema, ASP.NET 4,5 agrega la capacidad de declarar el tipo de datos de los datos a los que está enlazado un control. Para ello, use la nueva propiedad *itemType* . Al establecer esta propiedad, dos nuevas variables con tipo están disponibles en el ámbito de las expresiones de enlace de datos: *Item* y *BindItem*. Dado que las variables están fuertemente tipadas, obtendrá todas las ventajas de la experiencia de desarrollo de Visual Studio.

En el caso de expresiones de enlace de datos bidireccionales, use la variable *BindItem* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]

La mayoría de los controles del marco de formularios Web Forms ASP.NET que admiten el enlace de datos se han actualizado para admitir la propiedad *itemType* .

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Enlace de modelos

El enlace de modelos extiende el enlace de datos en los controles de formularios Web Forms de ASP.NET para trabajar con el acceso a datos centrado en el código. Incorpora conceptos del control *ObjectDataSource* y del enlace de modelos en ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Seleccionar datos

Para configurar un control de datos para que use el enlace de modelos para seleccionar datos, establezca la propiedad *SelectMethod* del control en el nombre de un método en el código de la página. El control de datos llama al método en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos. No es necesario llamar explícitamente al método *DataBind* .

En el ejemplo siguiente, el control *GridView* está configurado para usar un método denominado *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Cree el método *GetCategories* en el código de la página. Para una operación Select sencilla, el método no necesita ningún parámetro y debe devolver un objeto *IEnumerable* o *IQueryable* . Si se establece la nueva propiedad *itemType* (que permite expresiones de enlace de datos fuertemente tipadas, como se explica en [controles de datos fuertemente tipados](#_Toc318097386) ), se deben devolver las versiones genéricas de estas interfaces: *IEnumerable&lt;T&gt;* o *IQueryable&lt;t&gt;* , con el parámetro *t* que coincide con el tipo de la propiedad *itemType* (por ejemplo, *IQueryable&lt;categoría&gt;* ).

En el ejemplo siguiente se muestra el código de un método *GetCategories* . En este ejemplo se usa el modelo de Code First de Entity Framework con la base de datos de ejemplo Northwind. El código garantiza que la consulta devuelva detalles de los productos relacionados para cada categoría por medio del método *include* . (Esto garantiza que el elemento *TemplateField* en el marcado muestra el recuento de productos de cada categoría sin necesidad de una [Select n + 1](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)).

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Cuando se ejecuta la página, el control *GridView* llama automáticamente al método *GetCategories* y representa los datos devueltos mediante los campos configurados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Dado que el método SELECT devuelve un objeto *IQueryable* , el control *GridView* puede manipular más la consulta antes de ejecutarla. Por ejemplo, el control *GridView* puede Agregar expresiones de consulta para ordenar y paginar en el objeto *IQueryable* devuelto antes de ejecutarse, de modo que el proveedor LINQ subyacente Realice esas operaciones. En este caso, Entity Framework garantizará que las operaciones se realicen en la base de datos.

En el ejemplo siguiente se muestra el control *GridView* modificado para permitir la ordenación y paginación:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Ahora, cuando se ejecuta la página, el control puede asegurarse de que solo se muestra la página de datos actual y que está ordenada por la columna seleccionada:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Para filtrar los datos devueltos, los parámetros se deben agregar al método Select. Estos parámetros se rellenarán con el enlace del modelo en tiempo de ejecución y se pueden usar para modificar la consulta antes de devolver los datos.

Por ejemplo, suponga que desea permitir que los usuarios filtren los productos escribiendo una palabra clave en la cadena de consulta. Puede Agregar un parámetro al método y actualizar el código para usar el valor del parámetro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Este código incluye una expresión *Where* si se proporciona un valor para la *palabra clave* y, a continuación, devuelve los resultados de la consulta.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Proveedores de valores

El ejemplo anterior no era específico de dónde proviene el valor del parámetro de *palabra clave* . Para indicar esta información, puede usar un atributo de parámetro. En este ejemplo, puede usar la clase *QueryStringAttribute* que se encuentra en el espacio de nombres *System. Web. ModelBinding* :

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Esto indica al enlace de modelos que intente enlazar un valor de la cadena de consulta al parámetro de *palabra clave* en tiempo de ejecución. (Esto podría implicar realizar la conversión de tipos, aunque no en este caso). Si no se puede proporcionar un valor y el tipo no admite valores NULL, se produce una excepción.

Los orígenes de los valores de estos métodos se conocen como proveedores de valores y los atributos de parámetro que indican qué proveedor de valor se va a usar se conocen como atributos de proveedor de valores. Los formularios Web Forms incluirán proveedores de valores y los atributos correspondientes para todos los orígenes típicos de los datos proporcionados por el usuario en una aplicación de formularios Web Forms, como la cadena de consulta, las cookies, los valores de formulario, los controles, el estado de vista, el estado de la sesión y las propiedades del perfil. También puede escribir proveedores de valores personalizados.

De forma predeterminada, el nombre del parámetro se utiliza como clave para buscar un valor en la colección de proveedores de valores. En el ejemplo, el código buscará un valor de cadena de consulta denominado palabra clave (por ejemplo, ~/default.aspx? keyword = chef). Puede especificar una clave personalizada pasándola como argumento al atributo Parameter. Por ejemplo, para usar el valor de la variable de cadena de consulta denominada q, podría hacer lo siguiente:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Si este método está en el código de la página, los usuarios pueden filtrar los resultados pasando una palabra clave mediante la cadena de consulta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

El enlace de modelos realiza muchas tareas que, de otro modo, tendría que codificar manualmente: leer el valor, comprobar si hay un valor null, intentar convertirlo al tipo adecuado, comprobar si la conversión se realizó correctamente y, por último, usar el valor en el misma. El enlace de modelos da como resultado mucho menos código y en la capacidad de reutilizar la funcionalidad en toda la aplicación.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtrar por valores de un control

Supongamos que desea extender el ejemplo para permitir que el usuario elija un valor de filtro en una lista desplegable. Agregue la siguiente lista desplegable al marcado y configúrela para obtener sus datos de otro método mediante la propiedad *SelectMethod* :

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Normalmente también agregaría un elemento *EmptyDataTemplate* al control *GridView* para que el control muestre un mensaje si no se encuentran productos coincidentes:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

En el código de página, agregue el nuevo método de selección para la lista desplegable:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Por último, actualice el método *GetProducts* SELECT para tomar un nuevo parámetro que contenga el identificador de la categoría seleccionada en la lista desplegable:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Ahora, cuando se ejecuta la página, los usuarios pueden seleccionar una categoría en la lista desplegable y el control *GridView* se vuelve a enlazar automáticamente para mostrar los datos filtrados. Esto es posible porque el enlace de modelos realiza un seguimiento de los valores de los parámetros de los métodos SELECT y detecta si un valor de parámetro ha cambiado después de un postback. En ese caso, el enlace de modelos obliga al control de datos asociado a volver a enlazar los datos.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expresiones de enlace de datos codificadas en HTML

Ahora puede codificar en HTML el resultado de las expresiones de enlace de datos. Agregue dos puntos (:) al final del prefijo &lt;% # que marca la expresión de enlace de datos:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validación discreta

Ahora puede configurar los controles de validador integrados para usar JavaScript discreto para la lógica de validación del lado cliente. Esto reduce considerablemente la cantidad de código JavaScript insertado en el marcado de la página y reduce el tamaño total de la página. Puede configurar JavaScript discreto para los controles de validador de cualquiera de estas maneras:

- Globalmente agregando la siguiente configuración al elemento *&lt;appSettings&gt;* en el archivo Web. config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Globalmente estableciendo la propiedad estática *System. Web. UI. ValidationSettings. UnobtrusiveValidationMode* en *UnobtrusiveValidationMode. WebForms* (normalmente en el método de inicio de la *aplicación\_* en el archivo global. asax).
- Individualmente para una página estableciendo la nueva propiedad *UnobtrusiveValidationMode* de la clase *Page* en *UnobtrusiveValidationMode. WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Actualizaciones de HTML5

Se han realizado algunas mejoras en los controles de servidor de formularios Web Forms para aprovechar las nuevas características de HTML5:

- La propiedad *textmode* del control *TextBox* se ha actualizado para admitir los nuevos tipos de entrada HTML5 como *email*, *DateTime*, etc.
- El control *FileUpload* admite ahora varias cargas de archivos desde exploradores compatibles con esta característica de HTML5.
- Los controles de validador ahora admiten la validación de elementos de entrada de HTML5.
- Los nuevos elementos HTML5 que tienen atributos que representan una dirección URL ahora admiten runat = "Server". Como resultado, puede usar las convenciones de ASP.NET en rutas de dirección URL, como el operador ~ para representar la raíz de la aplicación (por ejemplo, &lt;vídeo runat = "Server" src = "~/myVideo.wmv"/&gt;).
- El control *UpdatePanel* se ha corregido para admitir la publicación de los campos de entrada de HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 beta se incluye ahora con Visual Studio 11 beta. ASP.NET MVC es un marco de trabajo para desarrollar aplicaciones web muy comprobables y fáciles de mantener aprovechando el patrón modelo-vista-controlador (MVC). ASP.NET MVC 4 facilita la compilación de aplicaciones para la web móvil e incluye ASP.NET Web API, lo que le ayuda a crear servicios HTTP que pueden tener acceso a cualquier dispositivo. Para obtener más información, vea las notas de la [versión de ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>ASP.NET Web Pages 2

Entre las nuevas características se incluyen las siguientes:

- Plantillas de sitio nuevas y actualizadas.
- Agregar la validación del lado servidor y del lado cliente mediante la aplicación auxiliar de *validación* .
- La capacidad de registrar scripts mediante un administrador de activos.
- Habilitación de inicios de sesión desde Facebook y otros sitios mediante OAuth y OpenID.
- Agregar asignaciones mediante la aplicación auxiliar *Maps* .
- Ejecutar aplicaciones de páginas web en paralelo.
- Páginas de representación para dispositivos móviles.

Para obtener más información sobre estas características y ejemplos de código de página completa, vea [las principales características de Web Pages 2 beta](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 beta

En esta sección se proporciona información sobre las mejoras para el desarrollo web en Visual Web Developer 11 beta y Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Uso compartido de proyectos entre Visual Studio 2010 y Visual Studio 2012 Release Candidate (compatibilidad de proyectos)

Hasta la versión candidata para lanzamiento de Visual Studio 2012, al abrir un proyecto existente en una versión más reciente de Visual Studio se inicia el Asistente para conversión. Esto forzó una actualización del contenido (recursos) de un proyecto y una solución a nuevos formatos que no eran compatibles con versiones anteriores. Por lo tanto, después de la conversión no se pudo abrir el proyecto en la versión anterior de Visual Studio.

Muchos clientes nos han comentado que este no era el enfoque correcto. En Visual Studio 11 beta, ahora se admite el uso compartido de proyectos y soluciones con Visual Studio 2010 SP1. Esto significa que si abre un proyecto de 2010 en Visual Studio 2012 Release Candidate, podrá abrir el proyecto en Visual Studio 2010 SP1.

> [!NOTE]
> Algunos tipos de proyectos no se pueden compartir entre Visual Studio 2010 SP1 y Visual Studio 2012 Release Candidate. Estos incluyen algunos proyectos anteriores (como los proyectos de ASP.NET MVC 2) o proyectos para fines especiales (como los proyectos de instalación).

Al abrir un proyecto Web de Visual Studio 2010 SP1 por primera vez en Visual Studio 11 beta, se agregan las siguientes propiedades al archivo de proyecto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation y OldToolsVersion se usan en el proceso que actualiza el archivo de proyecto. No tienen ningún impacto en el trabajo con el proyecto en Visual Studio 2010.

VisualStudioVersion es una nueva propiedad utilizada por MSBuild 4,5 que indica la versión de Visual Studio para el proyecto actual. Dado que esta propiedad no existía en MSBuild 4,0 (la versión de MSBuild que Visual Studio 2010 SP1 USA), se inserta un valor predeterminado en el archivo de proyecto.

La propiedad VSToolsPath se usa para determinar el archivo. targets correcto que se va a importar desde la ruta de acceso representada por el valor MSBuildExtensionsPath32.

También hay algunos cambios relacionados con los elementos de importación. Estos cambios son necesarios para admitir la compatibilidad entre ambas versiones de Visual Studio.

> [!NOTE]
> Si un proyecto se comparte entre Visual Studio 2010 SP1 y Visual Studio 11 beta en dos equipos diferentes, y si el proyecto incluye una base de datos local en la carpeta app\_Data, debe asegurarse de que la versión de SQL Server que usa la base de datos esté instalada en ambos equipos.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Cambios de configuración en las plantillas de sitio web de ASP.NET 4,5

Se han realizado los siguientes cambios en el archivo *Web. config* predeterminado para el sitio que se crea mediante plantillas de sitio web en Visual Studio 2012 Release Candidate:

- En el elemento `<httpRuntime>`, el atributo `encoderType` se establece ahora de forma predeterminada para usar los tipos AntiXSS que se agregaron a ASP.NET. Para obtener más información, consulte [AntiXSS Library](#_Toc318097382).
- También en el elemento `<httpRuntime>`, el atributo `requestValidationMode` se establece en "4,5". Esto significa que, de forma predeterminada, la validación de solicitudes está configurada para usar la validación diferida ("diferida"). Para obtener más información, consulte [nuevas características de validación de solicitudes de ASP.net](#_Toc318097379).
- El elemento `<modules>` de la sección `<system.webServer>` no contiene un atributo `runAllManagedModulesForAllRequests`. (Su valor predeterminado es false). Esto significa que si usa una versión de IIS 7 que no se ha actualizado a SP1, es posible que tenga problemas con el enrutamiento en un nuevo sitio. Para obtener más información, consulte [compatibilidad nativa en IIS 7 para el enrutamiento de ASP.net](#Native_Support_In_IIS7_For_ASPNET_Routine).

Estos cambios no afectan a las aplicaciones existentes. Sin embargo, pueden representar una diferencia en el comportamiento entre los sitios Web existentes y los nuevos que cree para ASP.NET 4,5 con las nuevas plantillas.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Compatibilidad nativa en IIS 7 para el enrutamiento de ASP.NET

Esto no es un cambio en ASP.NET como tal, sino un cambio en las plantillas para los nuevos proyectos de sitios web que pueden afectar al usuario si está trabajando con una versión de IIS 7 a la que no se ha aplicado la actualización de SP1.

En ASP.NET, puede Agregar la siguiente opción de configuración a las aplicaciones para admitir el enrutamiento:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Cuando **runAllManagedModulesForAllRequests** es true, una dirección url como `http://mysite/myapp/home` va a ASP.net, aunque no haya ninguna extensión *. aspx*, *. Mvc*o similar en la dirección URL.

Una actualización realizada en IIS 7 hace que la configuración de **runAllManagedModulesForAllRequests** sea innecesaria y admita el enrutamiento de ASP.net de forma nativa. (Para obtener información acerca de la actualización, consulte el artículo Soporte técnico de Microsoft [hay disponible una actualización que permite que determinados controladores de iis 7,0 o iis 7,5 controlen las solicitudes cuyas direcciones URL no finalizan con un punto](https://support.microsoft.com/kb/980368)).

Si el sitio web se ejecuta en IIS 7 y se ha actualizado IIS, no es necesario establecer **runAllManagedModulesForAllRequests** en true. De hecho, no se recomienda establecerlo en true porque agrega sobrecarga de procesamiento innecesaria para la solicitud. Cuando este valor es true, todas las solicitudes, incluidas las de *. htm*, *. jpg*y otros archivos estáticos, también pasan por la canalización de solicitudes ASP.net.

Si crea un nuevo sitio web de ASP.NET 4,5 con las plantillas que se proporcionan en Visual Studio 2012 RC, la configuración del sitio web no incluye el valor **runAllManagedModulesForAllRequests** . Esto significa que, de forma predeterminada, el valor es false.

Si después ejecuta el sitio web en Windows 7 sin SP1 instalado, IIS 7 no incluirá la actualización necesaria. Como consecuencia, el enrutamiento no funcionará y verá errores. Si tiene un problema en el que el enrutamiento no funciona, puede hacer lo siguiente:

- Actualice Windows 7 a SP1, con lo que se agregará la actualización a IIS 7.
- Instale la actualización que se describe en el Soporte técnico de Microsoft artículo enumerado anteriormente.
- Establezca **runAllManagedModulesForAllRequests** en true en el archivo Web. config de ese sitio Web. Tenga en cuenta que esto agregará una sobrecarga a las solicitudes.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor de HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tareas inteligentes

En Vista de diseño, las propiedades complejas de los controles de servidor suelen tener asociados asistentes y cuadros de diálogo para facilitar su establecimiento. Por ejemplo, puede usar un cuadro de diálogo especial para agregar un origen de datos a un control *Repeater* o Agregar columnas a un control *GridView* .

Sin embargo, este tipo de ayuda de la interfaz de usuario para propiedades complejas no está disponible en la vista Código fuente. Por lo tanto, Visual Studio 11 incluye tareas inteligentes para la vista Código fuente. Las tareas inteligentes son accesos directos contextuales para las características de C# uso frecuente en los editores de y Visual Basic.

En el caso de los controles de formularios Web Forms de ASP.NET, las tareas inteligentes aparecen en etiquetas de servidor como un pequeño glifo cuando el punto de inserción está dentro del elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

El tarea inteligente se expande al hacer clic en el glifo o al presionar CTRL +. (punto), al igual que en los editores de código. A continuación, muestra los accesos directos que son similares a las tareas inteligentes de Vista de diseño.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Por ejemplo, en la tarea inteligente de la ilustración anterior se muestran las opciones de las tareas de GridView. Si elige editar columnas, se muestra el cuadro de diálogo siguiente:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Al rellenar el cuadro de diálogo, se establecen las mismas propiedades que se pueden establecer en Vista de diseño. Al hacer clic en aceptar, el marcado del control se actualiza con la nueva configuración:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Compatibilidad con WAI-ARIA

La escritura de sitios web accesibles es cada vez más importante. El [estándar de accesibilidad WAI-ARIA](http://www.w3.org/WAI/intro/aria) define cómo los desarrolladores deben escribir sitios web accesibles. Este estándar ahora es totalmente compatible con Visual Studio.

Por ejemplo, el atributo *role* ahora tiene IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

El estándar WAI-ARIA también introduce atributos que tienen el prefijo *aria,* que permiten agregar semántica a un documento HTML5. Visual Studio también es totalmente compatible con los siguientes atributos *de Aria* :

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Nuevos fragmentos de código HTML5

Para que sea más rápido y fácil escribir marcado HTML5 de uso común, Visual Studio incluye una serie de fragmentos de código. Un ejemplo es el fragmento de código de vídeo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Para invocar el fragmento de código, presione la tecla TAB dos veces cuando el elemento esté seleccionado en IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Esto genera un fragmento de código que se puede personalizar.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extraer a control de usuario

En páginas web de gran tamaño, puede ser una buena idea trasladar partes individuales a controles de usuario. Esta forma de refactorización puede ayudar a aumentar la legibilidad de la página y a simplificar la estructura de la página.

Para facilitar esta tarea, al editar páginas de formularios Web Forms en la vista Código fuente, ahora puede seleccionar texto en una página, hacer clic con el botón secundario en él y, a continuación, elegir extraer a control de usuario:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense para código de fragmentos en atributos

Visual Studio siempre proporcionó IntelliSense para los elementos de código del lado servidor en cualquier página o control. Ahora Visual Studio también incluye IntelliSense para el código de los atributos HTML.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Esto facilita la creación de expresiones de enlace de datos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>Cambio de nombre automático de la etiqueta coincidente al cambiar el nombre de una etiqueta de apertura o de cierre

Si cambia el nombre de un elemento HTML (por ejemplo, cambia una etiqueta *div* para que sea una etiqueta de *encabezado* ), la etiqueta de apertura o cierre correspondiente también cambia en tiempo real.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Esto ayuda a evitar el error en el que se olvida de cambiar una etiqueta de cierre o de cambiar la incorrecta.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Generación de controladores de eventos

Visual Studio incluye ahora características en la vista Código fuente para ayudarle a escribir controladores de eventos y enlazarlos manualmente. Si está editando un nombre de evento en la vista Código fuente, IntelliSense muestra &lt;crear nuevo&gt;de eventos, que creará un controlador de eventos en el código de la página que tiene la firma correcta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

De forma predeterminada, el controlador de eventos usará el identificador del control para el nombre del método de control de eventos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

El controlador de eventos resultante tendrá el siguiente aspecto (en este caso, C#en):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Sangría inteligente

Cuando presione ENTRAR mientras se encuentra dentro de un elemento HTML vacío, el editor colocará el punto de inserción en el lugar correcto:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Si presiona entrar en esta ubicación, la etiqueta de cierre se desplaza hacia abajo y se aplica una sangría para que coincida con la etiqueta de apertura. También se aplica la sangría al punto de inserción:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Finalización automática de instrucciones de reducción automática

La lista de IntelliSense en Visual Studio ahora filtra según lo que escriba para que solo muestre las opciones relevantes:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense también filtra según el caso de título de las palabras individuales en la lista de IntelliSense. Por ejemplo, si escribe "dL", se muestran las opciones DL y ASP: DataList:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Esta característica hace que sea más rápido obtener la finalización de instrucciones de los elementos conocidos.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>editor de JavaScript

El editor de JavaScript de Visual Studio 2012 Release Candidate es totalmente nuevo y mejora considerablemente la experiencia de trabajar con JavaScript en Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Esquematización de código

Ahora se crean automáticamente las regiones de esquematización para todas las funciones, lo que permite contraer partes del archivo que no son pertinentes para el foco actual.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Coincidencia de llaves

Al colocar el punto de inserción en una llave de apertura o de cierre, el editor resalta la coincidencia.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Ir a definición

El comando ir a definición permite saltar al origen de una función o variable.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Compatibilidad con ECMAScript5

El editor es compatible con la nueva sintaxis y las API de ECMAScript5, la versión más reciente del estándar que describe el lenguaje JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense para DOM

Se ha mejorado IntelliSense para las API de DOM, con compatibilidad con muchas API de HTML5 nuevas, como *querySelector*, Dom Storage, mensajería entre documentos y *Canvas*. Ahora, IntelliSense de DOM está controlado por un único archivo de JavaScript simple, en lugar de por una definición de biblioteca de tipos nativa. Esto hace que sea fácil extender o reemplazar.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Sobrecargas de firma de VSDOC

Ahora se pueden declarar comentarios de IntelliSense detallados para las sobrecargas independientes de las funciones de JavaScript mediante el nuevo elemento *&lt;signature&gt;* , como se muestra en este ejemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Referencias implícitas

Ahora puede Agregar archivos de JavaScript a una lista central que se incluirá implícitamente en la lista de archivos a los que hace referencia cualquier archivo o bloque de JavaScript determinado, lo que significa que obtendrá IntelliSense para su contenido. Por ejemplo, puede Agregar archivos de jQuery a la lista central de archivos y obtendrá IntelliSense para las funciones de jQuery en cualquier bloque de archivo de JavaScript, tanto si ha hecho referencia a él explícitamente (mediante///&lt;referencia/&gt;) como si no.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Editor CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Finalización automática de instrucciones de reducción automática

La lista de IntelliSense para CSS ahora filtra según las propiedades y los valores de CSS admitidos por el esquema seleccionado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

IntelliSense también admite búsquedas de casos de título:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Aplicación de sangría jerárquica

El editor de CSS usa la sangría para mostrar las reglas jerárquicas, lo que proporciona una visión general de cómo se organizan lógicamente las reglas en cascada. En el ejemplo siguiente, el #list un selector es un elemento secundario en cascada de List y, por lo tanto, se aplica sangría.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

En el ejemplo siguiente se muestra una herencia más compleja:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

Las reglas principales determinan la sangría de una regla. La sangría jerárquica está habilitada de forma predeterminada, pero puede deshabilitarla en el cuadro de diálogo Opciones (herramientas, opciones en la barra de menús):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>Compatibilidad con la piratería de CSS

El análisis de cientos de archivos CSS del mundo real muestra que los ataques de CSS son muy comunes y ahora Visual Studio es compatible con los más utilizados. Esta compatibilidad incluye IntelliSense y la validación de la propiedad Star (\*) y el carácter de subrayado (\_) hackers:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

También se admiten ataques de selector típicos, por lo que la sangría jerárquica se mantiene incluso cuando se aplican. Un selector típico que se usa para tener como destino Internet Explorer 7 es anteponer un selector con *\*: First-Child + HTML*. El uso de esa regla mantendrá la sangría jerárquica:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Esquemas específicos del proveedor (-moz-,-WebKit)

CSS3 introduce muchas propiedades implementadas por diferentes exploradores en distintos momentos. Anteriormente, Esto obligó a los desarrolladores a codificar para exploradores específicos mediante la sintaxis específica del proveedor. Estas propiedades específicas del explorador se incluyen ahora en IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Compatibilidad con comentarios y quitar comentarios

Ahora puede comentar y quitar el comentario de las reglas de CSS usando los mismos métodos abreviados que se usan en el editor de código (Ctrl + K, C para comentar y Ctrl + K, sin comentarios).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Selector de colores

En versiones anteriores de Visual Studio, IntelliSense para los atributos relacionados con el color constaba de una lista desplegable de valores de color con nombre. La lista se ha reemplazado por un selector de colores con todas las características.

Al escribir un valor de color, el selector de colores se muestra automáticamente y muestra una lista de los colores usados previamente seguidos de una paleta de colores predeterminada. Puede seleccionar un color mediante el mouse o el teclado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

La lista se puede expandir en un selector de colores completo. El selector permite controlar el canal alfa convirtiendo automáticamente cualquier color en RGBA cuando se mueve el control deslizante opacidad:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Fragmentos de código

Los fragmentos de código en el editor de CSS facilitan y agilizan la creación de estilos entre exploradores. Muchas propiedades CSS3 que requieren una configuración específica del explorador se han realizado ahora en fragmentos de código.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Los fragmentos de código CSS admiten escenarios avanzados (como las consultas de medios CSS3) escribiendo el símbolo de arroba (@), que muestra la lista de IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Al seleccionar @media valor y presionar la tecla Tab, el editor de CSS inserta el siguiente fragmento de código:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Como en el caso de los fragmentos de código, puede crear sus propios fragmentos de código CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Regiones personalizadas

Las regiones de código con nombre, que ya están disponibles en el editor de código, ahora están disponibles para la edición de CSS. Esto le permite agrupar fácilmente los bloques de estilo relacionados.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Cuando se contrae una región, se muestra el nombre de la región:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspector de página

Inspector de página es una herramienta que representa una página web (HTML, formularios Web Forms, ASP.NET MVC o páginas web) en el IDE de Visual Studio y le permite examinar el código fuente y la salida resultante. En el caso de las páginas de ASP.NET, Inspector de página le permite determinar qué código del servidor ha generado el marcado HTML que se representa en el explorador.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Para obtener más información acerca de Inspector de página, consulte los siguientes tutoriales:

- Usar Inspector de página en [MVC de ASP.net](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Usar Inspector de página en [formularios Web Forms de ASP.net](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publicación

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Publicar los perfiles

En Visual Studio 2010, la publicación de información para los proyectos de aplicación web no se almacena en el control de versiones y no está diseñada para compartir con otros usuarios. En la versión candidata para lanzamiento de Visual Studio 2012, se ha cambiado el formato del perfil de publicación. Se ha convertido en un artefacto de equipo y ahora es fácil de aprovecharse de las compilaciones basadas en MSBuild. La información de configuración de compilación está en el cuadro de diálogo publicar para que pueda cambiar fácilmente las configuraciones de compilación antes de la publicación.

Los perfiles de publicación se almacenan en la carpeta PublishProfiles. La ubicación de la carpeta depende del lenguaje de programación que use:

- C#: Properties\PublishProfiles
- Visual Basic: mis Project\PublishProfiles

Cada perfil es un archivo de MSBuild. Durante la publicación, este archivo se importa en el archivo MSBuild del proyecto. En Visual Studio 2010, si desea realizar cambios en el proceso de publicación o paquete, debe colocar las personalizaciones en un archivo denominado **projectname**. WPP. targets. Esto se sigue admitiendo, pero ahora puede colocar sus personalizaciones en el perfil de publicación. De este modo, las personalizaciones se utilizarán solo para ese perfil.

Ahora también puede aprovechar los perfiles de publicación de MSBuild. Para ello, use el comando siguiente al compilar el proyecto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

El valor Project. csproj es la ruta de acceso del proyecto y ProfileName es el nombre del perfil que se va a publicar. Como alternativa, en lugar de pasar el nombre del perfil para la propiedad *PublishProfile* , puede pasar la ruta de acceso completa al perfil de publicación.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>ASP.NET precompilar y fusionar mediante combinación

En el caso de los proyectos de aplicación Web, Visual Studio 2012 Release Candidate agrega una opción en la página empaquetar/publicar propiedades web que le permite compilar y fusionar mediante combinación el contenido del sitio al publicar o empaquetar el proyecto. Para ver estas opciones, haga clic con el botón derecho en el proyecto en Explorador de soluciones, elija propiedades y, a continuación, elija la página de propiedades empaquetar/publicar web. En la ilustración siguiente se muestra la opción precompilar esta aplicación antes de publicar.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Cuando se selecciona esta opción, Visual Studio precompila la aplicación cada vez que se publica o empaqueta la aplicación Web. Si desea controlar cómo está precompilado el sitio o cómo se combinan los ensamblados, haga clic en el botón Opciones avanzadas para configurar esas opciones.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

El servidor Web predeterminado para probar proyectos web en Visual Studio ahora es IIS Express. El Servidor de desarrollo de Visual Studio sigue siendo una opción para el servidor Web local durante el desarrollo, pero IIS Express es ahora el servidor recomendado. La experiencia del uso de IIS Express en Visual Studio 11 beta es muy similar a su uso en Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, las compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y eventos que se describen aquí son ficticios y no tienen ninguna asociación con ninguna empresa, organización, producto, nombre de dominio y correo electrónico reales. la dirección, el logotipo, la persona, el lugar o el evento está pensado o debe deducirse.

© 2012 Microsoft Corporation. All rights reserved.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
