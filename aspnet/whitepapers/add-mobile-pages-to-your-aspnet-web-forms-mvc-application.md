---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Cómo: agregar páginas móviles a la aplicación de formularios Web Forms o MVC de ASP.NET | Microsoft Docs'
author: rick-anderson
description: En este artículo se describen las distintas formas de servir páginas optimizadas para dispositivos móviles de la aplicación ASP.NET Web Forms/MVC y se sugiere la arquitectura y...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 63c555358d06a9506bb5c8c993800c3307108192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78462217"
---
# <a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Cómo: agregar páginas móviles a la aplicación de formularios Web Forms o MVC de ASP.NET

> **Se aplica a**
> 
> - ASP.NET Web Forms versión 4,0
> - ASP.NET MVC versión 3,0
> 
> **Resumen**
> 
> En este artículo se describen las distintas formas de atender las páginas optimizadas para dispositivos móviles desde la aplicación Web Forms/MVC de ASP.NET y se sugieren problemas de diseño y arquitectura que se deben tener en cuenta a la hora de dirigirse a una amplia gama de dispositivos. En este documento también se explica por qué los controles móviles de ASP.NET de ASP.NET 2,0 a 3,5 ahora están obsoletos y se describen algunas alternativas modernas.

## <a name="contents"></a>Contenido

- Información general
- Opciones de arquitectura
- Detección de exploradores y dispositivos
- Cómo pueden las aplicaciones de formularios Web Forms de ASP.NET presentar páginas específicas para dispositivos móviles
- Cómo pueden las aplicaciones MVC de ASP.NET presentar páginas específicas para móviles
- Recursos adicionales

Para ver ejemplos de código descargables que muestran las técnicas de estas notas del producto para formularios Web Forms de ASP.NET y MVC, consulte [Mobile Apps sitios de & con ASP.net](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Información general

Dispositivos móviles: smartphones, teléfonos de características y tabletas: continúe creciendo en popularidad como medio para tener acceso a la Web. Para muchos desarrolladores web y empresas orientadas a la web, esto significa que es cada vez más importante proporcionar una excelente experiencia de exploración para los visitantes que usan esos dispositivos.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Cómo las versiones anteriores de ASP.NET eran compatibles con los exploradores móviles

ASP.NET Versions 2,0 to 3,5 incluye *ASP.NET Mobile Controls*: un conjunto de controles de servidor para dispositivos móviles en el ensamblado *System. Web. Mobile. dll* y el espacio de nombres *System. Web. UI. MobileControls* . El ensamblado todavía está incluido en ASP.NET 4, pero está en desuso. Se recomienda a los desarrolladores que migren a enfoques más modernos, como los descritos en este documento.

La razón por la que los controles móviles ASP.NET se han marcado como obsoletos es que su diseño está orientado en torno a los teléfonos móviles que eran comunes en torno a 2005 y versiones anteriores. Los controles están diseñados principalmente para representar el marcado WML o cHTML (en lugar de HTML normal) para los exploradores WAP de esa era. No obstante, WAP, WML y cHTML ya no son relevantes para la mayoría de los proyectos, porque HTML ahora se ha convertido en el lenguaje de marcado omnipresente para exploradores móviles y de escritorio.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Los desafíos de admitir dispositivos móviles hoy en día

Aunque los exploradores móviles ahora son compatibles con HTML de un momento más universal, se enfrentarán a muchos desafíos a la hora de crear excelentes experiencias de exploración móvil:

- ***Tamaño de pantalla*** : los dispositivos móviles varían drásticamente en el formulario, y sus pantallas suelen ser mucho menores que los monitores de escritorio. Por lo tanto, puede que tenga que diseñar diseños de página completamente diferentes para ellos.
- ***Métodos de entrada*** : algunos dispositivos tienen teclados, otros tienen lápiz óptico y otros usan la función táctil. Es posible que tenga que tener en cuenta varios mecanismos de navegación y métodos de entrada de datos.
- ***Compatibilidad con estándares*** : muchos exploradores móviles no son compatibles con los estándares HTML, CSS o JavaScript más recientes.
- ***Ancho de banda*** : el rendimiento de la red de datos móviles varía de forma natural y algunos usuarios finales tienen tarifas que se cobran por megabyte.

No hay ninguna solución que sea de un solo tamaño. la aplicación tendrá que buscar y comportarse de forma diferente según el dispositivo que tiene acceso a ella. En función del nivel de soporte técnico móvil que desee, puede ser un desafío mayor para los desarrolladores web que en el caso de las "guerras del explorador".

Los desarrolladores que se aproximan a la compatibilidad con exploradores móviles por primera vez suelen pensar que solo es importante admitir los smartphones más recientes y más sofisticados (por ejemplo, Windows Phone 7, iPhone o Android), quizás porque los desarrolladores suelen ser de su propiedad. dispositivos. Sin embargo, los teléfonos más baratos siguen siendo extremadamente populares y sus propietarios los usan para explorar la web, especialmente en los países en los que los teléfonos móviles son más fáciles de obtener que una conexión de banda ancha. Su empresa deberá decidir qué gama de dispositivos se admiten si tiene en cuenta a sus clientes probables. Si va a crear un catálogo en línea para un spa de estado de lujo, puede tomar una decisión empresarial únicamente para dirigirse a smartphones avanzados, mientras que si está creando un sistema de reserva de vales para una cine, es probable que tenga que tener en cuenta los visitantes con características menos eficaces. teléfonos.

## <a name="architectural-options"></a>Opciones de arquitectura

Antes de obtener los detalles técnicos específicos de los formularios Web Forms o MVC de ASP.NET, tenga en cuenta que los desarrolladores web en general tienen tres opciones principales para admitir exploradores móviles:

1. ***No hacer nada:*** Puede simplemente crear una aplicación web estándar y orientada al escritorio y basarse en exploradores móviles para representarla de forma aceptable. 

    - **Ventaja**: es la opción más barata de implementar y mantener: sin trabajo adicional
    - **Desventaja**: ofrece la peor experiencia del usuario final: 

        - Los smartphones más recientes pueden presentar el código HTML, así como un explorador de escritorio, pero los usuarios todavía se verán obligados a hacer zoom y desplazarse horizontal y verticalmente para consumir el contenido en una pantalla pequeña. Esto está lejos del óptimo.
        - Los dispositivos y los teléfonos de características más antiguos pueden no representar el marcado de forma satisfactoria.
        - Incluso en los dispositivos de tableta más recientes (cuyas pantallas pueden ser tan grandes como las pantallas de portátiles), se aplican reglas de interacción diferentes. La entrada táctil funciona mejor con botones y vínculos más grandes que se reparten más allá y no hay ninguna manera de mantener el cursor del mouse sobre un menú emergente.
2. ***Resolver el problema en el cliente* :** con un uso cuidadoso de CSS y la [mejora progresiva](http://en.wikipedia.org/wiki/Progressive_enhancement) , puede crear marcado, estilos y scripts que se adapten a cualquier explorador que los ejecute. Por ejemplo, con [las consultas de medios CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), puede crear un diseño de varias columnas que se convierte en un diseño de una sola columna en los dispositivos cuyas pantallas son más estrechas que el umbral seleccionado. 

    - **Ventaja**: optimiza la representación del dispositivo específico en uso, incluso en el caso de dispositivos futuros desconocidos según la pantalla y las características de entrada que tengan.
    - **Ventaja**: le permite compartir fácilmente la lógica del lado servidor en todos los tipos de dispositivos: duplicación mínima de código o esfuerzo
    - **Desventaja**: los dispositivos móviles son tan diferentes de los dispositivos de escritorio que realmente desea que las páginas móviles sean completamente diferentes de las páginas de escritorio, mostrando información diferente. Estas variaciones pueden ser ineficaces o imposibles de lograr de forma sólida a través de CSS, en especial teniendo en cuenta cómo los dispositivos más antiguos incoherentes interpretan las reglas de CSS. Esto es especialmente cierto en el caso de los atributos CSS 3.
    - **Desventaja**: no proporciona compatibilidad con la lógica del lado servidor y los flujos de trabajo de distintos dispositivos. Por ejemplo, no puede implementar un flujo de trabajo de retirada de carro de la compra simplificado para usuarios móviles por medio de CSS únicamente.
    - **Desventaja**: uso de ancho de banda ineficaz. Es posible que el servidor tenga que transmitir el marcado y los estilos que se aplican a todos los dispositivos posibles, aunque el dispositivo de destino solo usará un subconjunto de esa información.
3. ***Solucione el problema en el servidor* :** Si el servidor sabe qué dispositivo está accediendo a él, o al menos las características de ese dispositivo, como el tamaño de pantalla y el método de entrada, y si se trata de un dispositivo móvil, puede ejecutar una lógica diferente y generar un marcado HTML diferente. 

    - **Ventaja:** Flexibilidad máxima. No hay ningún límite en cuanto a cuánto puede variar la lógica del lado servidor para dispositivos móviles u optimizar el marcado para el diseño deseado específico del dispositivo.
    - **Ventaja:** Uso eficaz del ancho de banda. Solo necesita transmitir el marcado y la información de estilo que el dispositivo de destino va a usar.
    - **Desventaja:** A veces obliga a repetir el trabajo o el código (por ejemplo, creando copias similares pero ligeramente diferentes de las páginas de formularios Web Forms o las vistas de MVC). Siempre que sea posible, se factoriza la lógica común en un nivel o servicio subyacente, pero es posible que algunas partes del código o el marcado de la interfaz de usuario se dupliquen y se mantengan en paralelo.
    - **Desventaja:** La detección de dispositivos no es trivial. Requiere una lista o una base de datos de tipos de dispositivo conocidos y sus características (que puede que no siempre estén bien actualizadas) y no se garantiza que coincidan con precisión con cada solicitud entrante. En este documento se describen algunas opciones y sus problemas más adelante.

Para obtener los mejores resultados, la mayoría de los desarrolladores tienen que combinar las opciones (2) y (3). Las diferencias estilísticas secundarias se adaptan mejor al cliente mediante CSS o incluso JavaScript, mientras que las principales diferencias en los datos, el flujo de trabajo o el marcado se implementan de forma más eficaz en el código del lado servidor.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Este documento se centra en las técnicas del lado servidor

Dado que los formularios Web Forms y MVC de ASP.NET son principalmente tecnologías del lado servidor, estas notas del producto se centrarán en las técnicas de servidor que permiten generar diferentes marcado y lógica para los exploradores móviles. Por supuesto, también puede combinarlo con cualquier técnica de cliente (por ejemplo, consultas de medios CSS 3, JavaScript de mejora progresiva), pero esto es más importante para el diseño web que el desarrollo de ASP.NET.

## <a name="browser-and-device-detection"></a>Detección de exploradores y dispositivos

El requisito previo de clave para todas las técnicas de servidor para admitir dispositivos móviles es saber qué dispositivo usa el visitante. De hecho, incluso mejor que conocer el fabricante y el número de modelo de ese dispositivo está conociendo las *características* del dispositivo. Las características pueden incluir:

- ¿Es un dispositivo móvil?
- Método de entrada (mouse/teclado, táctil, teclado, joystick,...)
- Tamaño de la pantalla (físicamente y en píxeles)
- Formatos de datos y medios admitidos
- Etc.

Es mejor tomar decisiones en función de las características que el número de modelo, ya que, de este modo, estará mejor equipado para controlar futuros dispositivos.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Mediante ASP. Compatibilidad con la detección integrada del explorador de la red

Los desarrolladores de formularios Web Forms de ASP.NET y MVC pueden detectar inmediatamente características importantes de un explorador visitante mediante la inspección de las propiedades del objeto *request. Browser* . Por ejemplo, vea

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...y muchos otros

En segundo plano, la plataforma ASP.NET coincide con el encabezado HTTP de *agente de usuario* (UA) entrante con expresiones regulares en un conjunto de archivos XML de definición de explorador. De forma predeterminada, la plataforma incluye definiciones para muchos dispositivos móviles comunes y puede Agregar archivos de definición de explorador personalizados para los demás usuarios que desee reconocer. Para obtener más información, vea la página de MSDN [ASP.net controles de servidor Web y capacidades del explorador](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Usar la base de datos de dispositivos WURFL a través de 51Degrees.mobi Foundation

While ASP. La compatibilidad con la detección integrada del explorador de la red será suficiente para muchas aplicaciones. hay dos casos principales en los que es posible que no sea suficiente:

- ***Quiere reconocer los dispositivos más recientes***(sin crear manualmente los archivos de definición del explorador). Tenga en cuenta que los archivos de definición del explorador de .NET 4 no son lo suficientemente recientes para reconocer Windows Phone 7, teléfonos Android, exploradores de Operating Mobile o Apple iPad.
- ***Necesita información más detallada sobre las funcionalidades del dispositivo***. Es posible que tenga que conocer el método de entrada de un dispositivo (por ejemplo, Touch vs teclado) o los formatos de audio que admite el explorador. Esta información no está disponible en los archivos de definición de explorador estándar.

El [proyecto de *archivo de recursos universal inalámbrico* (wurfl)](http://wurfl.sourceforge.net/) mantiene información más actualizada y detallada acerca de los dispositivos móviles que se usan hoy en día.

La gran noticia para los desarrolladores de .NET es que ASP. La característica de detección de exploradores de red es extensible, por lo que es posible mejorarla para solucionar estos problemas. Por ejemplo, puede Agregar la biblioteca de código abierto [*51Degrees.mobi Foundation*](https://github.com/51Degrees/dotNET-Device-Detection) Library al proyecto. Es un proveedor de funciones de ASP.NET o IHttpModule (utilizable en formularios Web Forms y aplicaciones MVC) que lee directamente los datos de WURFL y los enlaza a ASP. Mecanismo de detección de explorador integrado de la red. Una vez que haya instalado el módulo, *request. Browser* contendrá una información mucho más precisa y detallada: reconocerá correctamente muchos de los dispositivos mencionados anteriormente y mostrará sus capacidades (incluidas otras funcionalidades como el método de entrada). Consulte la documentación del proyecto para obtener más detalles.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Cómo pueden las aplicaciones de Web Forms presentar páginas específicas para dispositivos móviles

De forma predeterminada, este es el modo en que se muestra una nueva aplicación de formularios Web Forms en dispositivos móviles comunes:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Claramente, ninguno de los diseños tiene un aspecto muy descriptivo: esta página se diseñó para un monitor grande, orientado a horizontal, no para una pequeña pantalla orientada verticalmente. ¿Qué puede hacer al respecto?

Como se explicó anteriormente en este documento, hay muchas maneras de adaptar las páginas para dispositivos móviles. Algunas técnicas se basan en el servidor y otras se ejecutan en el cliente.

### <a name="creating-a-mobile-specific-master-page"></a>Crear una página maestra específica para dispositivos móviles

En función de sus requisitos, puede usar los mismos formularios Web para todos los visitantes, pero tener dos páginas maestras independientes: una para los visitantes del escritorio, otra para los visitantes móviles. Esto le ofrece la flexibilidad de cambiar la hoja de estilos CSS o el marcado HTML de nivel superior para adaptarse a los dispositivos móviles, sin obligarle a duplicar la lógica de la página.

Esto es fácil de hacer. Por ejemplo, puede Agregar un controlador de PreInit como el siguiente a un formulario web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Ahora, cree una página maestra denominada Mobile. Master en la carpeta de nivel superior de la aplicación y se usará cuando se detecte un dispositivo móvil. Si es necesario, la página maestra móvil puede hacer referencia a una hoja de estilos CSS específica del móvil. Los visitantes del escritorio seguirán viendo la página maestra predeterminada, no la del móvil.

### <a name="creating-independent-mobile-specific-web-forms"></a>Creación de formularios Web Forms específicos para móviles independientes

Para obtener la máxima flexibilidad, puede profundizar mucho más que simplemente tener páginas maestras independientes para diferentes tipos de dispositivos. Puede implementar dos *conjuntos totalmente independientes de páginas de formularios Web Forms* : uno para los exploradores de escritorio y otro para los móviles. Esto funciona mejor si desea presentar información o flujos de trabajo muy diferentes a los visitantes móviles. En el resto de esta sección se describe detalladamente este enfoque.

Suponiendo que ya tiene una aplicación de formularios Web Forms diseñada para los exploradores de escritorio, la manera más fácil de proceder es crear una subcarpeta denominada "móvil" dentro del proyecto y crear las páginas móviles allí. Puede crear un subsitio completo, con sus propias páginas maestras, hojas de estilos y páginas, con las mismas técnicas que usaría para cualquier otra aplicación de formularios Web Forms. No tiene que generar necesariamente un equivalente móvil para *cada* página del sitio de escritorio; puede elegir qué subconjunto de funcionalidad tiene sentido para los visitantes móviles.

Las páginas móviles pueden compartir recursos estáticos comunes (como imágenes, JavaScript o archivos CSS) con las páginas normales, si así lo desea. Dado que la carpeta "móvil" *no* se marcará como una aplicación independiente cuando se hospeda en IIS (es simplemente una subcarpeta simple de páginas de formularios Web Forms), también compartirá la misma configuración, los mismos datos de sesión y otras infraestructuras que las páginas de escritorio.

> [!NOTE]
> Dado que este enfoque normalmente implica la duplicación de código (es probable que las páginas móviles compartan algunas similitudes con las páginas de escritorio), es importante factorizar cualquier lógica de negocios común o el código de acceso a datos en una capa o servicio compartido subyacente. De lo contrario, se duplicará el esfuerzo de crear y mantener la aplicación.

#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Redirigir a los visitantes móviles a las páginas móviles

A menudo es conveniente redirigir a los visitantes móviles a las páginas móviles solo en la *primera* solicitud de su sesión de exploración (y no en todas las solicitudes de su sesión), porque:

- Después, puede permitir que los visitantes móviles accedan fácilmente a las páginas de escritorio si lo desean: solo tiene que colocar un vínculo en la página maestra que vaya a "versión de escritorio". El visitante no se redirigirá de nuevo a una página móvil, ya que ya no es la primera solicitud de su sesión.
- Evita el riesgo de interferir con las solicitudes de los recursos dinámicos compartidos entre los elementos móviles y de escritorio del sitio (por ejemplo, si tiene un formulario web común que los componentes de escritorio y móviles del sitio se muestran en un IFRAME, o algunos controladores Ajax).

Para ello, puede colocar la lógica de redirección en una **sesión\_** método de inicio. Por ejemplo, agregue el método siguiente al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configuración de la autenticación de formularios para respetar las páginas móviles

Tenga en cuenta que la autenticación mediante formularios realiza determinadas suposiciones sobre dónde puede redirigir a los visitantes durante y después del proceso de autenticación:

- Cuando un usuario debe autenticarse, la autenticación de formularios los redirigirá a la página de inicio de sesión de escritorio, independientemente de si es un usuario de escritorio o móvil (porque solo tiene un concepto de *una* dirección URL de inicio de sesión). Suponiendo que desea aplicar estilo a la página de inicio de sesión móvil de forma diferente, debe mejorar la página de inicio de sesión de escritorio para que redirija a los usuarios móviles a una página de inicio de sesión móvil independiente. Por ejemplo, agregue el código siguiente al código subyacente de la página de inicio de sesión de **escritorio** : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Después de que un usuario inicie sesión correctamente, la autenticación de formularios redirigirá de forma predeterminada a la Página principal del escritorio (porque solo tiene un concepto de *una* página predeterminada). Debe mejorar la página de inicio de sesión móvil para que redirija a la Página principal móvil después de un inicio de sesión correcto. Por ejemplo, agregue el código siguiente al código subyacente de la página de inicio de sesión **móvil** : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  En este código se supone que la página tiene un control de servidor de inicio de sesión denominado LoginUser, como en la plantilla de proyecto predeterminada.

### <a name="working-with-output-caching"></a>Trabajar con el almacenamiento en caché de resultados

Si usa el almacenamiento en caché de resultados, tenga en cuidado que, de forma predeterminada, es posible que un usuario de escritorio visite una determinada dirección URL (lo que hace que su salida se almacene en caché), seguida de un usuario móvil que recibe la salida del escritorio almacenado en caché. Esta advertencia se aplica tanto si solo está modificando la página maestra por tipo de dispositivo, como si implementa formularios Web Forms totalmente independientes por tipo de dispositivo.

Para evitar el problema, puede indicar a ASP.NET que varíe la entrada de la memoria caché según si el visitante usa un dispositivo móvil. Agregue un parámetro VaryByCustom a la declaración OutputCache de la página como se indica a continuación:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

A continuación, defina *isMobileDevice* como parámetro de caché personalizado agregando la siguiente invalidación de método al archivo global.asax.CS:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Esto garantizará que los visitantes móviles de la página no reciban la salida previamente colocada en la memoria caché por un visitante del escritorio.

### <a name="a-working-example"></a>Un ejemplo funcional

Para ver estas técnicas en acción, descargue los [ejemplos de código de este documento](https://docs.microsoft.com/aspnet/mobile/overview). La aplicación de ejemplo de formularios Web Forms redirige automáticamente a los usuarios móviles a un conjunto de páginas específicas de dispositivos móviles en una subcarpeta denominada móvil. El marcado y el estilo de esas páginas están optimizados mejor para los exploradores móviles, como puede ver en las siguientes capturas de pantallas:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Para obtener más sugerencias sobre cómo optimizar el marcado y CSS para exploradores móviles, vea la sección "aplicar estilos a páginas móviles para exploradores móviles" más adelante en este documento.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Cómo pueden las aplicaciones MVC de ASP.NET presentar páginas específicas para móviles

Dado que el patrón modelo-vista-controlador separa la lógica de la aplicación (en los controladores) de la lógica de presentación (en vistas), puede elegir cualquiera de los métodos siguientes para controlar la compatibilidad con dispositivos móviles en el código del servidor:

1. ***utilizan los mismos controladores y vistas para los exploradores de escritorio y móviles, pero representan las vistas con diseños de Razor diferentes en función del tipo de dispositivo *.** Esta opción funciona mejor si muestra datos idénticos en todos los dispositivos, pero simplemente desea proporcionar diferentes hojas de estilos CSS o cambiar algunos elementos HTML de nivel superior para dispositivos móviles.
2. ***Use los mismos controladores para los exploradores de escritorio y móviles, pero represente vistas diferentes según el tipo de dispositivo***. Esta opción funciona mejor si se muestran aproximadamente los mismos datos y se proporcionan los mismos flujos de trabajo para los usuarios finales, pero se desea representar un marcado HTML muy diferente para adaptarse al dispositivo que se está usando.
3. ***crear áreas independientes para exploradores móviles y de escritorio, implementando controladores independientes y vistas para cada *.** Esta opción funciona mejor si se muestran pantallas muy diferentes, que contienen información diferente y que conducen al usuario a través de diferentes flujos de trabajo optimizados para su tipo de dispositivo. Puede significar alguna repetición del código, pero puede minimizarlo mediante la división de la lógica común en una capa o servicio subyacente.

Si desea tomar la **primera** opción y variar solo el diseño de Razor por tipo de dispositivo, es muy fácil. Solo tiene que modificar el archivo \_ViewStart. cshtml de la siguiente manera:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Ahora puede crear un diseño específico para móviles denominado \_LayoutMobile. cshtml con una estructura de página y reglas CSS optimizadas para dispositivos móviles.

Si desea que la **segunda** opción represente vistas totalmente diferentes según el tipo de dispositivo del visitante, consulte la [entrada de blog de Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

El resto de este documento se centra en la **tercera** opción: crear controladores *y* vistas independientes para dispositivos móviles, por lo que puede controlar exactamente qué subconjunto de funcionalidad se ofrece para los visitantes móviles.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Configuración de un área móvil en la aplicación ASP.NET MVC

Puede Agregar un área denominada "móvil" a una aplicación existente de ASP.NET MVC de la manera normal: haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones y, a continuación, elija Agregar área de à. Después, puede agregar controladores y vistas como lo haría con cualquier otra área dentro de una aplicación ASP.NET MVC. Por ejemplo, agregue al área móvil un nuevo controlador denominado HomeController para que actúe como una página principal para los visitantes móviles.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Asegurarse de que la dirección URL/Mobile llegue a la Página principal móvil

Si desea que la dirección URL/Mobile llegue a la acción de índice en HomeController dentro del área móvil, deberá realizar dos pequeños cambios en la configuración de enrutamiento. En primer lugar, actualice la clase MobileAreaRegistration para que HomeController sea el controlador predeterminado en el área móvil, tal como se muestra en el código siguiente:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Esto significa que la Página principal móvil ahora se encontrará en/Mobile, en lugar de/Mobile/Home, ya que "Home" es ahora el nombre del controlador predeterminado implícitamente para las páginas móviles.

A continuación, tenga en cuenta que al agregar un segundo HomeController a la aplicación (es decir, el móvil, además del escritorio existente), habrá interrumpido la Página principal de escritorio normal. Se producirá un error que indica*que se encontraron varios tipos que coinciden con el controlador denominado ' Home '* ". Para resolver este error, actualice la configuración de enrutamiento de nivel superior (en Global.asax.cs) para especificar que el HomeController de escritorio debe tener prioridad cuando haya ambigüedad:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Ahora el error desaparecerá y la dirección URL http:\/\/*yoursite*/llegará a la Página principal del escritorio y http:\/\/*yoursite*/Mobile/llegarán a la Página principal de Mobile.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Redirigir visitantes móviles a su área móvil

Hay muchos puntos de extensibilidad diferentes en ASP.NET MVC, por lo que hay muchas formas posibles de inyectar la lógica de redirección. Una opción ordenada es crear un atributo de filtro, [RedirectMobileDevicesToMobileArea], que realiza una redirección si se cumplen las condiciones siguientes:

1. Es la primera solicitud de la sesión del usuario (es decir, Session. IsNewSession es igual a true)
2. La solicitud proviene de un explorador móvil (es decir, Request. Browser. IsMobileDevice es igual a true)
3. El usuario todavía no está solicitando un recurso en el área móvil (es decir, la parte de la *ruta de acceso* de la dirección URL no comienza por/Mobile)

El ejemplo descargable incluido en estas notas del producto incluye una implementación de esta lógica. Se implementa como un filtro de autorización, que se deriva de AuthorizeAttribute, lo que significa que puede funcionar correctamente incluso si se usa el almacenamiento en caché de resultados (de lo contrario, si un visitante del escritorio accede primero a una dirección URL determinada, la salida del escritorio se podría almacenar en caché y, a continuación, atender a visitantes móviles posteriores).

Como filtro, puede elegir aplicarlo a controladores y acciones específicos, por ejemplo,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… o bien, puede aplicarlo a todos los controladores y acciones como un *filtro global* de MVC 3 en el archivo global.asax.CS:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

En el ejemplo descargable también se muestra cómo puede crear subclases de este atributo que se redirigen a ubicaciones específicas dentro de su área móvil. Esto significa que, por ejemplo, puede:

- Registre un filtro global como se muestra arriba que envía a los visitantes móviles a la página de inicio móvil de forma predeterminada.
- Aplique también un filtro especial [RedirectMobileDevicesToMobileProductPage] a una acción "ver producto" que lleve a los visitantes móviles a la versión móvil de cualquier página del producto que haya solicitado.
- Aplique también otras subclases especiales del filtro a acciones específicas, redirigiendo a los visitantes móviles a la página móvil equivalente

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Configuración de la autenticación de formularios para respetar las páginas móviles

Si utiliza la autenticación de formularios, debe tener en cuenta que cuando un usuario necesita iniciar sesión, redirige automáticamente al usuario a una dirección URL de "Inicio de sesión" específica, que de forma predeterminada es **/account/Logon**. Esto significa que los usuarios móviles se pueden redirigir a la acción "iniciar sesión" del escritorio.

Para evitar este problema, agregue la lógica a la acción "iniciar sesión" del escritorio para que redirija a los usuarios móviles de nuevo a una acción de inicio de sesión móvil. Si está usando la plantilla de aplicación de ASP.NET MVC predeterminada, actualice la acción de inicio de sesión de AccountController como se indica a continuación:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… y, a continuación, implemente una acción adecuada de "Inicio de sesión" específica de Mobile en un controlador denominado AccountController en su área móvil.

### <a name="working-with-output-caching"></a>Trabajar con el almacenamiento en caché de resultados

Si utiliza el filtro [OutputCache], debe forzar que la entrada de la caché varíe según el tipo de dispositivo. Por ejemplo, escriba:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

A continuación, agregue el método siguiente al archivo Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Esto garantizará que los visitantes móviles de la página no reciban la salida previamente colocada en la memoria caché por un visitante del escritorio.

### <a name="a-working-example"></a>Un ejemplo funcional

Para ver estas técnicas en acción, descargue los [ejemplos asociados de código de este documento](https://docs.microsoft.com/aspnet/mobile/overview). El ejemplo incluye una aplicación de ASP.NET MVC 3 (versión candidata) mejorada para admitir dispositivos móviles mediante los métodos descritos anteriormente.

## <a name="further-guidance-and-suggestions"></a>Instrucciones y sugerencias adicionales

La siguiente explicación se aplica a los desarrolladores de Web Forms y MVC que usan las técnicas descritas en este documento.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Mejora de la lógica de redirección mediante 51Degrees.mobi Foundation

La lógica de redirección que se muestra en este documento puede ser perfectamente suficiente para la aplicación, pero no funcionará si necesita deshabilitar sesiones o con exploradores móviles que rechacen las cookies (no pueden tener sesiones), ya que no sabrá si una solicitud determinada es el primero de ese visitante.

Ya aprendió cómo la base de 51Degrees.mobi de código abierto puede mejorar la precisión de ASP. Detección del explorador de la red. También tiene una capacidad integrada para redirigir a los visitantes móviles a ubicaciones específicas configuradas en Web. config. Es capaz de trabajar sin depender de las sesiones de ASP.NET (y, por lo tanto, de las cookies) mediante el almacenamiento de un registro temporal de los valores hash de los encabezados HTTP y las direcciones IP de los visitantes, de modo que sepa si cada solicitud es la primera de un determinado visitador.

El siguiente elemento que se agrega a la sección fiftyOne del archivo Web. config redirigirá la primera solicitud de un dispositivo móvil detectado a la página ~/Mobile/Default.aspx. *No* se redirigirán las solicitudes a las páginas de la carpeta móvil, independientemente del tipo de dispositivo. Si el dispositivo móvil ha estado inactivo durante un período de 20 minutos o más, el dispositivo se olvidará y las solicitudes subsiguientes se tratarán como nuevas para los propósitos de la redirección.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Para obtener más información, consulte la [documentación de 51Degrees.mobi Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> *Puede* usar la característica de redirección de 51Degrees.mobi Foundation en aplicaciones ASP.NET MVC, pero tendrá que definir la configuración de redireccionamiento en términos de direcciones URL sin formato, no en términos de enrutamiento o mediante la colocación de filtros MVC en acciones. Esto se debe a que, en el momento de la escritura, 51Degrees.mobi Foundation no reconoce filtros ni rutas.

### <a name="disabling-transcoders-and-proxy-servers"></a>Deshabilitar transcodificadores y servidores proxy

Los operadores de red móvil tienen dos objetivos generales en su enfoque para la Internet móvil:

1. Proporcione tanto contenido relevante como sea posible
2. Maximice el número de clientes que pueden compartir el ancho de banda de red de radio limitado

Dado que la mayoría de las páginas web se diseñaron para pantallas de gran tamaño de escritorio y conexiones de banda ancha de línea fija rápidas, muchos operadores utilizan *transcodificadores* o *servidores proxy* que modifican dinámicamente el contenido Web. Pueden modificar el marcado HTML o CSS para adaptarse a pantallas más pequeñas (especialmente para "teléfonos de características" que carecen de la capacidad de procesamiento para administrar diseños complejos) y pueden volver a comprimir las imágenes (lo que reduce significativamente su calidad) para mejorar la velocidad de entrega de páginas.

Pero si ha realizado el esfuerzo de generar una versión optimizada para dispositivos móviles de su sitio, es probable que no desee que el operador de red interfiera más. Puede Agregar la siguiente línea a la página\_evento Load en cualquier formulario Web ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

O bien, para un controlador ASP.NET MVC, puede Agregar la siguiente invalidación de método para que se aplique a todas las acciones de ese controlador:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

El mensaje HTTP resultante informa a los transcodificadores y proxies compatibles con W3C de no modificar el contenido. Por supuesto, no hay ninguna garantía de que los operadores de red móvil respeten este mensaje.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Aplicar estilos a páginas móviles para exploradores móviles

Queda fuera del ámbito de este documento describir con gran detalle qué tipos de marcado HTML funcionan correctamente o qué técnicas de diseño web maximizan la facilidad de uso en determinados dispositivos. Depende de usted encontrar un diseño suficientemente sencillo, optimizado para una pantalla de tamaño móvil, sin usar trucos de posicionamiento HTML o CSS no confiables. Una técnica importante que merece la pena mencionar, sin embargo, es la *etiqueta meta*de la ventanilla.

Algunos exploradores móviles modernos, en un esfuerzo muestran páginas Web diseñadas para monitores de escritorio, representan la página en un lienzo virtual, también denominado "ventanilla" (por ejemplo, la ventanilla virtual tiene un ancho de 980 píxeles en el iPhone y 850 píxeles de ancho en Opera Mobile de forma predeterminada) y, a continuación, escale el resultado hacia abajo para ajustarse a los píxeles físicos de la pantalla. Después, el usuario puede acercar y desplazarse por la ventanilla. Esto tiene la ventaja de que permite que el explorador muestre la página en su diseño previsto, pero también tiene la desventaja de que fuerza el zoom y la panorámica, lo que no resulta cómodo para el usuario. Si está diseñando para móviles, es mejor diseñar una pantalla estrecha para que no sea necesario hacer zoom o desplazamiento horizontal.

Una manera de indicar al explorador móvil el ancho que debe tener la ventanilla a través de la etiqueta meta no estándar de la *ventanilla* . Por ejemplo, si agrega lo siguiente a la sección principal de la página,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… a continuación, los exploradores de smartphone diseñarán la página en un lienzo virtual de ancho de 480 píxeles. Esto significa que si los elementos HTML definen su ancho en términos porcentuales, los porcentajes se interpretarán con respecto a este ancho de 480 píxeles, no al ancho de la ventanilla predeterminado. Como resultado, es menos probable que el usuario tenga que hacer zoom y panorámica horizontalmente, lo que mejora considerablemente la experiencia de exploración móvil.

Si desea que el ancho de la ventanilla coincida con los píxeles físicos del dispositivo, puede especificar lo siguiente:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Para que esto funcione correctamente, no debe forzar explícitamente que los elementos superen ese ancho (por ejemplo, mediante el uso de un atributo de *ancho* o una propiedad de CSS); de lo contrario, el explorador se verá obligado a usar una ventanilla mayor independientemente de. Vea también: [más detalles sobre la etiqueta viewport no estándar](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

La mayoría de los smartphones modernos admiten la *doble orientación*: se pueden mantener en modo vertical u horizontal. Por lo tanto, es importante no hacer suposiciones sobre el ancho de pantalla en píxeles. Ni siquiera Supongamos que el ancho de la pantalla es fijo, ya que el usuario puede volver a orientar el dispositivo mientras está en la página.

Los dispositivos de Windows Mobile y BlackBerry más antiguos también pueden aceptar las siguientes etiquetas meta en el encabezado de página para informar de que el contenido se ha optimizado para dispositivos móviles y, por tanto, no debe transformarse.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Recursos adicionales

Para obtener una lista de emuladores y simuladores de dispositivos móviles que puede usar para probar la aplicación Web de ASP.NET móvil, consulte la página [simular dispositivos móviles populares para pruebas](../mobile/device-simulators.md).

## <a name="credits"></a>Créditos

- Autor principal: Steven Sanderson
- Revisores/escritores de contenido adicionales: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
