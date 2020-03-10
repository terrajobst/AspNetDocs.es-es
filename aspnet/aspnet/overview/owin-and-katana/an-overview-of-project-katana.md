---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Información general de Project Katana | Microsoft Docs
author: howarddierking
description: El marco de trabajo de ASP.NET ha estado en torno a más de diez años y la plataforma ha habilitado el desarrollo de innumerables sitios web y servicios. Como icaciones Web...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 1f28db822930cdfd2ebf4cf9bb27d173f4aa4201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500347"
---
# <a name="an-overview-of-project-katana"></a>Información general del proyecto Katana

por [Howard Dierking](https://github.com/howarddierking)

> El marco de trabajo de ASP.NET ha estado en torno a más de diez años y la plataforma ha habilitado el desarrollo de innumerables sitios web y servicios. A medida que evolucionan las estrategias de desarrollo de aplicaciones Web, el marco de trabajo ha podido evolucionar en paso con tecnologías como ASP.NET MVC y ASP.NET Web API. A medida que el desarrollo de aplicaciones web lleva su siguiente paso evolutivo hacia el mundo de la informática en la nube, Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) proporciona el conjunto subyacente de componentes a las aplicaciones de ASP.net, lo que les permite ser flexible, portátil, ligero y proporcionar un mejor rendimiento; de otro modo, Project [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) cloud optimiza las aplicaciones ASP.net.

## <a name="why-katana--why-now"></a>¿Por qué Katana?

 Independientemente de si se trata de un marco de trabajo para desarrolladores o de un producto de usuario final, es importante comprender las motivaciones subyacentes para crear el producto, y parte de eso incluye saber con qué se ha creado el producto. ASP.NET se creó originalmente con dos clientes en mente.   
  
**El primer grupo de clientes era desarrollador de ASP clásico.** En ese momento, ASP era una de las tecnologías principales para crear sitios web dinámicos controlados por datos y aplicaciones mediante la interseción de marcado y el script de servidor. El script del servidor en tiempo de ejecución de ASP proporcionado con un conjunto de objetos que abstraeba aspectos básicos del protocolo HTTP y el servidor Web subyacentes, y proporcionaba acceso a servicios adicionales, como la administración de la sesión y el estado de la aplicación, la memoria caché, etc. Aunque las eficaces aplicaciones ASP clásicas convirtieron en un reto administrar a medida que aumentaran el tamaño y la complejidad. Esto era en gran medida debido a la falta de estructura encontrada en los entornos de scripting junto con la duplicación de código resultante de la intercalación de código y marcado. Con el fin de aprovechar al máximo las ventajas del ASP clásico mientras se abordan algunos de sus desafíos, ASP.NET aprovechó la organización del código proporcionada por los lenguajes orientados a objetos del .NET Framework al mismo tiempo que conserva el modelo de programación del lado servidor. en qué desarrolladores clásicos de ASP han crecido.

**El segundo grupo de clientes de destino para ASP.NET era desarrolladores de aplicaciones empresariales de Windows.** A diferencia de los desarrolladores de ASP clásico, que estaban acostumbrados a escribir el marcado HTML y el código para generar más marcado HTML, los desarrolladores de WinForms (como los desarrolladores de VB6 anteriores) estaban acostumbrados a una experiencia en tiempo de diseño que incluía un lienzo y un amplio conjunto de usuarios controles de interfaz. La primera versión de ASP.NET, también conocida como "formularios Web Forms", proporcionó una experiencia similar en tiempo de diseño junto con un modelo de eventos del servidor para los componentes de la interfaz de usuario y un conjunto de características de la infraestructura (como ViewState) para crear una experiencia de desarrollador perfecta entre la programación del lado cliente y el servidor. Los formularios Web Forms han ocultado de forma eficaz la naturaleza sin estado de la web en un modelo de eventos con estado conocido para los desarrolladores de WinForms.

### <a name="challenges-raised-by-the-historical-model"></a>Desafíos generados por el modelo histórico

**El resultado neto era un modelo de programación de desarrollo y en tiempo de ejecución completo y rico en características.** Sin embargo, con esa característica, la riqueza ha surgido un par de retos importantes. En primer lugar, el marco de trabajo era **monolítico**, con unidades de funcionalidad dispares lógicamente separadas en el mismo ensamblado System. Web. dll (por ejemplo, los objetos http principales con el marco de trabajo de formularios Web Forms). En segundo lugar, ASP.NET se incluyó como parte del .NET Framework mayor, lo que significaba que el **tiempo entre las versiones era el orden de los años.** Esto dificultaba que ASP.NET mantenerse en el ritmo de todos los cambios que se producen en un desarrollo web que evolucione rápidamente. Por último, System. Web. dll se ha acoplado de varias maneras diferentes a una opción de hospedaje web concreta: Internet Information Services (IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Pasos evolutivas: ASP.NET MVC y ASP.NET Web API

Y en el desarrollo web se estaba produciendo un gran número de cambios. Las aplicaciones web se desarrollaron cada vez más como una serie de componentes pequeños y centrados en lugar de marcos de gran tamaño. El número de componentes, así como la frecuencia con la que se lanzaron, aumentó a una velocidad cada vez más rápida. Era evidente que mantener el ritmo de la web requeriría que los marcos estuvieran más pequeños, desacoplados y más centrados, en lugar de más grandes y con características enriquecidas, por lo que el **equipo de ASP.net llevó a cabo varios pasos evolutivas para habilitar ASP.net como una familia de componentes Web conectables en lugar de un único marco**.

Uno de los cambios tempranos fue el aumento de la popularidad del modelo de diseño conocido del controlador de vista de modelos (MVC) gracias a los marcos de desarrollo web como Ruby on Rails. Este estilo de creación de aplicaciones web dio al desarrollador mayor control sobre el marcado de la aplicación, al tiempo que conservaba la separación del marcado y la lógica de negocios, que era uno de los puntos de venta iniciales de ASP.NET. Para satisfacer la demanda de este estilo de desarrollo de aplicaciones Web, Microsoft ha tenido la oportunidad de posicionarse mejor para el futuro mediante el **desarrollo de ASP.NET MVC fuera de banda** (sin incluirlo en el .NET Framework). ASP.NET MVC se lanzó como una descarga independiente. Esto dio al equipo de ingeniería la flexibilidad de proporcionar actualizaciones con mucha más frecuencia de lo que había sido posible anteriormente.

Otro cambio importante en el desarrollo de aplicaciones web era el cambio desde páginas web dinámicas generadas por el servidor hasta el marcado inicial estático con secciones dinámicas de la página generadas a partir de scripts de cliente que se comunican **con las API Web de back-end a través de solicitudes Ajax**. Este cambio arquitectónico ayudó al aumento de las API Web y al desarrollo del marco de ASP.NET Web API. Como en el caso de ASP.NET MVC, el lanzamiento de ASP.NET Web API proporcionó otra oportunidad para evolucionar ASP.NET como un marco más modular. El equipo de ingeniería aprovechó las ventajas de la oportunidad y **se creó ASP.net web API tal que no tenía dependencias en ninguno de los tipos de marco de trabajo principales que se encuentran en System. Web. dll**. Esto ha habilitado dos cosas: en primer lugar, significaba que ASP.NET Web API podía evolucionar de forma completamente independiente (y podría continuar la iteración rápidamente porque se entrega a través de NuGet). En segundo lugar, dado que no había dependencias externas con System. Web. dll y, por lo tanto, ninguna dependencia de IIS, ASP.NET Web API incluía la capacidad de ejecutarse en un host personalizado (por ejemplo, una aplicación de consola, un servicio de Windows, etc.)

### <a name="the-future-a-nimble-framework"></a>El futuro: un marco de trabajo más ágil

Al desacoplar los componentes del marco de trabajo entre sí y, a continuación, publicarlos en NuGet, los marcos de trabajo ahora pueden **iterar más de forma independiente y más rápidamente**. Además, la eficacia y flexibilidad de la funcionalidad de autohospedaje de Web API resultó muy atractiva para los desarrolladores que querían un **pequeño host ligero** para sus servicios. Resultó tan atractivo, de hecho, que otros marcos de trabajo también querían esta capacidad, y esto surgió un nuevo desafío en que cada marco de trabajo se ejecutaba en su propio proceso de host en su propia dirección base y necesitaba ser administrado (iniciado, detenido, etc.) de forma independiente. Una aplicación web moderna generalmente admite el servicio de archivos estáticos, la generación de páginas dinámicas, la API Web y las notificaciones de extracción y en tiempo real más recientes. Se espera que cada uno de estos servicios se deben ejecutar y administrar de forma independiente simplemente no es realista.

Lo que era necesario era una abstracción de hospedaje única que permitiría a un desarrollador crear una aplicación a partir de una variedad de componentes y marcos de trabajo diferentes y, a continuación, ejecutar la aplicación en un host auxiliar.

## <a name="the-open-web-interface-for-net-owin"></a>La interfaz web abierta para .NET (OWIN)

 Inspirado en los beneficios logrados por el [bastidor](http://rack.github.io/) de la comunidad de Ruby, varios miembros de la comunidad de .net se han establecido para crear una abstracción entre los servidores web y los componentes del marco. Dos objetivos de diseño para la abstracción OWIN fueron que era sencillo y que tomaba las mínimas dependencias posibles en otros tipos de Marcos. Estos dos objetivos ayudan a garantizar:

- Los nuevos componentes pueden desarrollarse y consumirse más fácilmente.
- Las aplicaciones se pueden migrar más fácilmente entre hosts y, potencialmente, plataformas o sistemas operativos completos.

La abstracción resultante consta de dos elementos básicos. El primero es el Diccionario de entorno. Esta estructura de datos es responsable de almacenar todo el estado necesario para procesar una solicitud y respuesta HTTP, así como cualquier estado del servidor pertinente. El Diccionario de entorno se define de la siguiente manera:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Un servidor Web compatible con OWIN es responsable de rellenar el Diccionario de entorno con datos como los flujos de cuerpo y las colecciones de encabezados para una solicitud y respuesta HTTP. Después, es responsabilidad de los componentes de la aplicación o del marco de trabajo rellenar o actualizar el diccionario con valores adicionales y escribir en la secuencia del cuerpo de la respuesta.

Además de especificar el tipo para el Diccionario de entorno, la especificación OWIN define una lista de pares de clave-valor de diccionario principales. Por ejemplo, en la tabla siguiente se muestran las claves de diccionario necesarias para una solicitud HTTP:

| Nombre de clave | Descripción del valor |
| --- | --- |
| `"owin.RequestBody"` | Flujo con el cuerpo de la solicitud, si existe. Stream. NULL se puede usar como marcador de posición si no hay ningún cuerpo de solicitud. Consulte el cuerpo de la [solicitud](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | `IDictionary<string, string[]>` de encabezados de solicitud. Consulte [encabezados](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | `string` que contiene el método de solicitud HTTP de la solicitud (por ejemplo, `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | `string` que contiene la ruta de acceso de la solicitud. La ruta de acceso debe ser relativa a la "raíz" del delegado de aplicación; vea [rutas](http://owin.org/html/owin.html#5-3-paths)de acceso. |
| `"owin.RequestPathBase"` | `string` que contiene la parte de la ruta de acceso de la solicitud correspondiente a la "raíz" del delegado de la aplicación; vea [rutas](http://owin.org/html/owin.html#5-3-paths)de acceso. |
| `"owin.RequestProtocol"` | `string` que contiene el nombre del protocolo y la versión (por ejemplo, `"HTTP/1.0"` o `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | `string` que contiene el componente de cadena de consulta del URI de la solicitud HTTP, sin el "?" inicial (por ejemplo, `"foo=bar&baz=quux"`). El valor puede ser una cadena vacía. |
| `"owin.RequestScheme"` | `string` que contiene el esquema URI utilizado para la solicitud (por ejemplo, `"http"`, `"https"`); vea [esquema de URI](http://owin.org/html/owin.html#5-1-uri-scheme). |

El segundo elemento clave de OWIN es el delegado de la aplicación. Se trata de una firma de función que actúa como la interfaz principal entre todos los componentes de una aplicación OWIN. La definición del delegado de aplicación es la siguiente:

`Func<IDictionary<string, object>, Task>;`

A continuación, el delegado de la aplicación es simplemente una implementación del tipo de delegado Func, donde la función acepta el Diccionario de entorno como entrada y devuelve una tarea. Este diseño tiene varias implicaciones para los desarrolladores:

- Hay un número muy pequeño de dependencias de tipo necesarias para poder escribir componentes OWIN. Esto aumenta en gran medida la accesibilidad de OWIN a los desarrolladores.
- El diseño asincrónico permite que la abstracción sea eficaz con el control de los recursos informáticos, especialmente en operaciones con un uso intensivo de e/s.
- Dado que el delegado de la aplicación es una unidad atómica de ejecución y como el Diccionario de entorno se realiza como un parámetro en el delegado, los componentes OWIN se pueden encadenar fácilmente juntos para crear canalizaciones complejas de procesamiento de HTTP.

Desde la perspectiva de la implementación, OWIN es una especificación ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Su objetivo no es el marco Web siguiente, sino una especificación sobre cómo interactúan los marcos web y los servidores Web.

Si ha investigado [Owin](http://owin.org/) o [Katana](https://github.com/aspnet/AspNetKatana/wiki), es posible que también haya observado el [paquete NuGet Owin](http://nuget.org/packages/Owin) y Owin. dll. Esta biblioteca contiene una sola interfaz, [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), que formaliza y codifica la secuencia de inicio que se describe en la [sección 4](http://owin.org/html/owin.html#4-application-startup) de la especificación OWIN. Aunque no es necesario para compilar servidores OWIN, la interfaz [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) proporciona un punto de referencia concreto y lo usan los componentes del proyecto Katana.

## <a name="project-katana"></a>Project Katana

Mientras que las especificaciones [Owin](http://owin.org/html/owin.html) y *Owin. dll* son trabajos de código abierto de la comunidad y de la comunidad, el proyecto [Katana](https://github.com/aspnet/AspNetKatana/wiki) representa el conjunto de componentes OWIN que, mientras que el código abierto sigue siendo código abierto, se compila y lanza a Microsoft. Estos componentes incluyen ambos componentes de la infraestructura, como hosts y servidores, así como componentes funcionales, como componentes de autenticación y enlaces a marcos como [signalr](../../../signalr/index.md) y [ASP.net web API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). El proyecto tiene los tres objetivos de alto nivel siguientes: 

- **Portátiles** : los componentes deben poder sustituirse fácilmente por los nuevos componentes a medida que estén disponibles. Esto incluye todos los tipos de componentes, desde el marco de trabajo hasta el servidor y el host. La implicación de este objetivo es que los marcos de terceros pueden ejecutarse sin problemas en los servidores de Microsoft, mientras que los marcos de trabajo de Microsoft pueden ejecutarse potencialmente en servidores y hosts de terceros.
- **Modular/flexible**: a diferencia de muchos marcos de trabajo que incluyen una gran cantidad de características que están activadas de forma predeterminada, los componentes de proyecto de Katana deben ser pequeños y centrados, lo que le ofrece control sobre el desarrollador de la aplicación para determinar qué componentes usar en su aplicación.
- **Ligero/con rendimiento y escalable** : mediante la división de la noción tradicional de un marco de trabajo en un conjunto de componentes pequeños y centrados que se agregan explícitamente por el desarrollador de la aplicación, una aplicación Katana resultante puede consumir menos recursos informáticos y, como resultado, controlar más carga, que con otros tipos de servidores y marcos. A medida que los requisitos de la aplicación demandan más características de la infraestructura subyacente, se pueden agregar a la canalización OWIN, pero esa debe tomar una decisión explícita sobre la parte del desarrollador de la aplicación. Además, la posibilidad de sustituir los componentes de nivel inferior significa que, a medida que estén disponibles, se pueden introducir nuevos servidores de alto rendimiento para mejorar el rendimiento de las aplicaciones OWIN sin interrumpir esas aplicaciones.

## <a name="getting-started-with-katana-components"></a>Introducción con componentes Katana

Una vez que se presentó por primera vez, un aspecto del marco de trabajo de [node. js](http://nodejs.org/) que dibujó la atención de las personas de forma inmediata era la simplicidad con la que podría crear y ejecutar un servidor Web. Si los objetivos de Katana se encontraban en la luz de [node. js](http://nodejs.org/), es posible que se resuman indicando que Katana aporta muchas de las ventajas de [node. js](http://nodejs.org/) (y marcos de trabajo como ti) sin obligar a que el desarrollador destaque todo lo que conoce sobre el desarrollo de aplicaciones Web ASP.net. Para que esta instrucción contenga el valor true, la introducción al proyecto Katana debe ser igual de sencilla a [node. js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Creando "Hola mundo!"

Una diferencia importante entre el desarrollo de JavaScript y .NET es la presencia (o ausencia) de un compilador. Como tal, el punto inicial de un servidor Katana sencillo es un proyecto de Visual Studio. Sin embargo, podemos empezar con los tipos de proyecto más mínimos: la aplicación Web ASP.NET vacía.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

A continuación, se instalará el paquete NuGet [Microsoft. Owin. host. SystemWeb](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) en el proyecto. Este paquete proporciona un servidor OWIN que se ejecuta en la canalización de solicitudes de ASP.NET. Se puede encontrar en la [Galería de NuGet](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) y se puede instalar mediante el cuadro de diálogo Administrador de paquetes de Visual Studio o la consola del administrador de paquetes con el siguiente comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Al instalar el paquete de `Microsoft.Owin.Host.SystemWeb`, se instalarán algunos paquetes adicionales como dependencias. Una de estas dependencias es `Microsoft.Owin`, una biblioteca que proporciona varios tipos y métodos auxiliares para desarrollar aplicaciones OWIN. Podemos usar esos tipos para escribir rápidamente el siguiente servidor "Hello World".

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Este servidor web muy sencillo se puede ejecutar ahora con el comando **F5** de Visual Studio e incluye compatibilidad completa con la depuración.

## <a name="switching-hosts"></a>Cambiar hosts

De forma predeterminada, el ejemplo anterior de "Hello World" se ejecuta en la canalización de solicitudes ASP.NET, que usa System. Web en el contexto de IIS. Esto puede Agregar un valor enorme, ya que nos permite beneficiarse de la flexibilidad y la composición de una canalización OWIN con las capacidades de administración y la madurez general de IIS. Sin embargo, puede haber casos en los que no se necesiten las ventajas proporcionadas por IIS y el deseo de un host más pequeño y ligero. ¿Qué se necesita para ejecutar nuestro servidor web simple fuera de IIS y System. Web?

Para ilustrar el objetivo de portabilidad, el paso de un host de servidor Web a un host de línea de comandos requiere simplemente agregar las dependencias de servidor y host nuevas a la carpeta de salida del proyecto y, a continuación, iniciar el host. En este ejemplo, se hospedará el servidor Web en un host de Katana denominado `OwinHost.exe` y se usará el servidor basado en HttpListener de Katana. De forma similar a los demás componentes de Katana, estos se adquirirán desde NuGet con el siguiente comando:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Desde la línea de comandos, podemos desplazarse a la carpeta raíz del proyecto y simplemente ejecutar el `OwinHost.exe` (que se instaló en la carpeta Tools de su paquete NuGet respectivo). De forma predeterminada, `OwinHost.exe` está configurado para buscar el servidor basado en HttpListener y, por tanto, no se necesita ninguna configuración adicional. Al navegar en un explorador Web a `http://localhost:5000/` se muestra la aplicación que se ejecuta ahora a través de la consola de.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Arquitectura de Katana

 La arquitectura de componentes de Katana divide una aplicación en cuatro capas lógicas, como se muestra a continuación: *host, servidor, middleware* y *aplicación*. La arquitectura de componentes se factoriza de tal forma que las implementaciones de estas capas se pueden sustituir fácilmente, en muchos casos, sin necesidad de volver a compilar la aplicación.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>administrador de flujos de trabajo

 El host es responsable de:

- Administrar el proceso subyacente.
- Orquestar el flujo de trabajo que da como resultado la selección de un servidor y la construcción de una canalización OWIN a través de la cual se administrarán las solicitudes.

  En la actualidad, hay 3 opciones de hospedaje principales para las aplicaciones basadas en Katana:  
  
**IIS/ASP. net**: mediante los tipos estándar HttpModule y HttpHandler, las CANALIZAciones OWIN se pueden ejecutar en IIS como parte de un flujo de solicitud ASP.net. La compatibilidad con el hospedaje de ASP.NET se habilita al instalar el paquete NuGet Microsoft. AspNet. host. SystemWeb en un proyecto de aplicación Web. Además, dado que IIS actúa como host y como servidor, la distinción entre el servidor y el host OWIN se reparte en este paquete de NuGet, lo que significa que si se usa el host SystemWeb, un desarrollador no puede sustituir una implementación de servidor alternativa.  
  
**Host personalizado**: el conjunto de componentes de Katana ofrece a los desarrolladores la capacidad de hospedar aplicaciones en su propio proceso personalizado, ya sea una aplicación de consola, un servicio de Windows, etc. Esta funcionalidad es similar a la funcionalidad de autohospedaje proporcionada por Web API. En el ejemplo siguiente se muestra un host personalizado del código de la API Web:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

La configuración de autohospedaje para una aplicación Katana es similar:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Una diferencia importante entre la API Web y los ejemplos de autohospedaje de Katana es que el código de configuración de la API Web no se encuentra en el ejemplo Katana Self-host. Para habilitar la portabilidad y composición, Katana separa el código que inicia el servidor del código que configura la canalización de procesamiento de solicitudes. El código que configura la API Web se incluye en el inicio de la clase, que se especifica además como el parámetro de tipo en WebApplication. Start.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

La clase startup se tratará con más detalle más adelante en el artículo. Sin embargo, el código necesario para iniciar un proceso de autohospedaje Katana es sorprendentemente similar al código que puede estar usando hoy en ASP.NET Web API aplicaciones de autohospedaje.

**OwinHost. exe**: mientras que algunos querrán escribir un proceso personalizado para ejecutar aplicaciones Web de Katana, muchos preferirían simplemente iniciar un ejecutable pregenerado que pueda iniciar un servidor y ejecutar su aplicación. En este escenario, el conjunto de componentes de Katana incluye `OwinHost.exe`. Cuando se ejecuta desde el directorio raíz de un proyecto, este archivo ejecutable iniciará un servidor (usa el servidor HttpListener de forma predeterminada) y usa convenciones para buscar y ejecutar la clase de inicio del usuario. Para un control más granular, el ejecutable proporciona una serie de parámetros de línea de comandos adicionales.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Servidor

 Aunque el host es responsable de iniciar y mantener el proceso en el que se ejecuta la aplicación, la responsabilidad del servidor es abrir un socket de red, escuchar solicitudes y enviarlas a través de la canalización de los componentes OWIN especificados por el usuario (como usted es posible que ya se haya observado que esta canalización se especifica en la clase de inicio del desarrollador de la aplicación). Actualmente, el proyecto Katana incluye dos implementaciones de servidor: 

- **Microsoft. Owin. host. SystemWeb**: como se mencionó anteriormente, IIS junto con la canalización ASP.net actúa como host y como servidor. Por lo tanto, al elegir esta opción de hospedaje, IIS administra los problemas de nivel de host, como la activación de procesos y escucha las solicitudes HTTP. En el caso de las aplicaciones Web de ASP.NET, envía las solicitudes a la canalización ASP.NET. El host de SystemWeb de Katana registra un HttpModule y HttpHandler de ASP.NET para interceptar las solicitudes a medida que fluyen a través de la canalización HTTP y las envían a través de la canalización OWIN especificada por el usuario.
- **Microsoft. Owin. host. HttpListener**: como su nombre indica, este servidor de Katana usa la clase HttpListener del .NET Framework para abrir un socket y enviar solicitudes a una canalización Owin especificada por el desarrollador. Esta es actualmente la selección de servidor predeterminada para la API de autohospedaje Katana y para OwinHost. exe.

## <a name="middlewareframework"></a>Middleware/marco de trabajo

 Como se mencionó anteriormente, cuando el servidor acepta una solicitud de un cliente, es responsable de pasarlo a través de una canalización de los componentes OWIN, que se especifican mediante el código de inicio del desarrollador. Estos componentes de canalización se conocen como middleware.  
 En un nivel muy básico, un componente de middleware OWIN simplemente necesita implementar el delegado de la aplicación OWIN para que se pueda llamar.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Sin embargo, para simplificar el desarrollo y la composición de los componentes de middleware, Katana admite un puñado de convenciones y tipos auxiliares para los componentes de middleware. Lo más común es la `OwinMiddleware` clase. Un componente de middleware personalizado creado con esta clase tendría un aspecto similar al siguiente: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Esta clase se deriva de `OwinMiddleware`, implementa un constructor que acepta una instancia del siguiente middleware en la canalización como uno de sus argumentos y, a continuación, lo pasa al constructor base. Los argumentos adicionales que se usan para configurar el middleware también se declaran como parámetros de constructor después del siguiente parámetro de middleware.   
  
En tiempo de ejecución, el middleware se ejecuta a través del método `Invoke` invalidado. Este método toma un único argumento de tipo `OwinContext`. Este objeto de contexto lo proporciona el paquete NuGet `Microsoft.Owin` descrito anteriormente y proporciona acceso fuertemente tipado a la solicitud, la respuesta y el Diccionario de entorno, junto con algunos tipos auxiliares adicionales.   
  
La clase de middleware se puede agregar fácilmente a la canalización OWIN en el código de inicio de la aplicación de la siguiente manera:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Dado que la infraestructura de Katana simplemente crea una canalización de componentes de middleware OWIN, y dado que los componentes simplemente necesitan admitir el delegado de la aplicación para participar en la canalización, los componentes de middleware pueden abarcar la complejidad de los registradores simples hasta marcos de trabajo completos como ASP.NET, Web API o [signalr](../../../signalr/index.md). Por ejemplo, si agrega ASP.NET Web API a la canalización OWIN anterior, es necesario agregar el siguiente código de Inicio:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

La infraestructura de Katana creará la canalización de los componentes de middleware según el orden en el que se agregaron al objeto IAppBuilder en el método de configuración. En nuestro ejemplo, LoggerMiddleware puede controlar todas las solicitudes que fluyen a través de la canalización, independientemente de cómo se controlen en última instancia esas solicitudes. Esto permite escenarios eficaces en los que un componente de middleware (por ejemplo, un componente de autenticación) puede procesar solicitudes para una canalización que incluye varios componentes y marcos (por ejemplo, ASP.NET Web API, Signalr y un servidor de archivos estático).
 
## <a name="applications"></a>Aplicaciones

Como se muestra en los ejemplos anteriores, OWIN y el proyecto Katana no se deben considerar como un nuevo modelo de programación de aplicaciones, sino como una abstracción para desacoplar los modelos de programación de aplicaciones y los marcos de trabajo del servidor y la infraestructura de hospedaje. Por ejemplo, al compilar aplicaciones de API Web, el marco de trabajo del desarrollador seguirá usando el marco de ASP.NET Web API, independientemente de si la aplicación se ejecuta en una canalización OWIN mediante componentes del proyecto Katana. El único lugar en el que el código relacionado con OWIN será visible para el desarrollador de la aplicación será el código de inicio de la aplicación, donde el desarrollador crea la canalización OWIN. En el código de inicio, el desarrollador registrará una serie de instrucciones UseXx, normalmente una para cada componente de middleware que procesará las solicitudes entrantes. Esta experiencia tendrá el mismo efecto que registrar los módulos HTTP en el entorno de System. Web actual. Normalmente, un middleware de marco mayor, como ASP.NET Web API o [signalr](../../../signalr/index.md) , se registrará al final de la canalización. Los componentes de middleware transversales, como los para la autenticación o el almacenamiento en caché, se registran normalmente al principio de la canalización para que procesen las solicitudes de todos los marcos y componentes registrados más adelante en la canalización. Esta separación de los componentes de middleware entre sí y de los componentes de la infraestructura subyacente permite que los componentes evolucionen a velocidades diferentes, a la vez que garantizan que el sistema global siga siendo estable.

## <a name="components--nuget-packages"></a>Componentes: paquetes NuGet

Al igual que muchas bibliotecas y marcos actuales, los componentes del proyecto Katana se entregan como un conjunto de paquetes NuGet. Para la próxima versión 2,0, el gráfico de dependencias del paquete Katana tiene el siguiente aspecto. (Haga clic en la imagen para ver una vista más grande).

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Casi todos los paquetes del proyecto Katana dependen, directa o indirectamente, del paquete Owin. Es posible que recuerde que este es el paquete que contiene la interfaz IAppBuilder, que proporciona una implementación concreta de la secuencia de inicio de la aplicación descrita en la sección 4 de la especificación OWIN. Además, muchos de los paquetes dependen de Microsoft. Owin, que proporciona un conjunto de tipos de aplicación auxiliar para trabajar con solicitudes y respuestas HTTP. El resto del paquete se puede clasificar como paquetes de infraestructura de hospedaje (servidores o hosts) o middleware. Los paquetes y las dependencias que son externos al proyecto Katana se muestran en color naranja.

La infraestructura de hospedaje de Katana 2,0 incluye los servidores basados en SystemWeb y HttpListener, el paquete OwinHost para ejecutar aplicaciones OWIN con OwinHost. exe y el paquete Microsoft. Owin. Hosting para autohospedar aplicaciones OWIN en un host personalizado (por ejemplo, aplicación de consola, servicio de Windows, etc.)

En el caso de Katana 2,0, los componentes de middleware se centran principalmente en proporcionar diferentes formas de autenticación. Se proporciona un componente adicional de middleware para el diagnóstico, que habilita la compatibilidad con una página de inicio y error. A medida que OWIN crece en la abstracción de hospedaje de facto, el ecosistema de componentes de middleware, tanto desarrollados por Microsoft como de terceros, también aumentará en número.

## <a name="conclusion"></a>Conclusión

 Desde el principio, el objetivo del proyecto Katana no ha sido crear y, por tanto, obligar a los desarrolladores a aprender aún otro marco Web. En su lugar, el objetivo ha sido crear una abstracción para ofrecer a los desarrolladores de aplicaciones Web de .NET la opción más adecuada que antes. Al dividir las capas lógicas de una pila de aplicación web típica en un conjunto de componentes reemplazables, el proyecto Katana permite que los componentes de toda la pila mejoren la velocidad de los componentes. Al compilar todos los componentes en torno a la abstracción OWIN simple, Katana permite que los marcos y las aplicaciones creadas sobre ellos sean portátiles a través de una variedad de servidores y hosts diferentes. Al colocar el desarrollador en el control de la pila, Katana garantiza que el desarrollador realiza la elección más reciente sobre la ligera o la forma en que debe ser su pila Web.  

## <a name="for-more-information-about-katana"></a>Para obtener más información acerca de Katana

- El proyecto Katana en GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Vídeo: [el proyecto Katana-OWIN para ASP.net](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), Howard Dierking.

## <a name="acknowledgements"></a>Reconocimientos

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (Twitter [@RickAndMSFT](http://twitter.com/RickAndMSFT) ) Rick es un redactor de programación Senior para Microsoft que se centra en Azure y MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (Twitter [@shanselman](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (Twitter [@jongalloway](https://twitter.com/jongalloway) )
