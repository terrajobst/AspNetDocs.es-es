---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: Registrar detalles de error con ELMAHC#() | Microsoft Docs
author: rick-anderson
description: Los módulos y controladores de registro de errores (ELMAH) ofrecen otro enfoque para registrar errores en tiempo de ejecución en un entorno de producción. ELMAH es un error de código abierto gratuito...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 5018023eced23e7a70eab90e649f85862c548940
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74570479"
---
# <a name="logging-error-details-with-elmah-c"></a>Registrar detalles de error con ELMAH (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Los módulos y controladores de registro de errores (ELMAH) ofrecen otro enfoque para registrar errores en tiempo de ejecución en un entorno de producción. ELMAH es una biblioteca de registro de errores de código abierto y gratuita que incluye características como el filtrado de errores y la posibilidad de ver el registro de errores de una página web, como una fuente RSS o de descargarlo como un archivo delimitado por comas. Este tutorial le guía a través de la descarga y configuración de ELMAH.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](logging-error-details-with-asp-net-health-monitoring-cs.md) se examinó ASP. El sistema de supervisión de estado de la red, que ofrece una biblioteca de la caja para grabar una amplia gama de eventos Web. Muchos desarrolladores usan la supervisión de estado para registrar y enviar por correo electrónico los detalles de las excepciones no controladas. Sin embargo, hay algunos puntos problemáticos con este sistema. En primer lugar, es la falta cualquier tipo de interfaz de usuario para ver la información sobre los eventos registrados. Si desea ver un resumen de los 10 últimos errores, o ver los detalles de un error que se produjo la semana pasada, debe echar un vistazo a la base de datos, examinar la bandeja de entrada de correo electrónico o crear una página web que muestre información de la tabla `aspnet_WebEvent_Events`.

Otro punto problemático se centra en la complejidad del seguimiento de estado. Dado que la supervisión de estado se puede usar para registrar una gran cantidad de eventos diferentes, y dado que hay una variedad de opciones para indicar cómo y cuándo se registran los eventos, la configuración correcta del sistema de supervisión de estado puede ser una tarea muy onerosa. Por último, hay problemas de compatibilidad. Dado que la supervisión de estado se agregó por primera vez al .NET Framework en la versión 2,0, no está disponible para las aplicaciones web anteriores compiladas con la versión 1. x de ASP.NET. Además, la clase `SqlWebEventProvider`, que usamos en el tutorial anterior para registrar los detalles del error en una base de datos, solo funciona con bases de datos de Microsoft SQL Server. Tendrá que crear una clase de proveedor de registro personalizada en caso de que necesite registrar los errores en un almacén de datos alternativo, como un archivo XML o una base de datos de Oracle.

Una alternativa al sistema de supervisión de estado son los módulos y controladores de registro de errores (ELMAH), un sistema de registro de errores de código abierto gratuito creado por [Atif Aziz](http://www.raboof.com/). La diferencia más notable entre los dos sistemas es la capacidad de ELAMH para mostrar una lista de errores y los detalles de un error específico de una página web y como una fuente RSS. ELMAH es más fácil de configurar que la supervisión de estado porque solo registra errores. Además, ELMAH incluye compatibilidad con las aplicaciones ASP.NET 1. x, ASP.NET 2,0 y ASP.NET 3,5, y se distribuye con una variedad de proveedores de origen de registro.

Este tutorial le guía a través de los pasos necesarios para agregar ELMAH a una aplicación ASP.NET. Comencemos.

> [!NOTE]
> El sistema de supervisión de estado y ELMAH tienen sus propios conjuntos de ventajas y desventajas. Recomiendo que pruebe ambos sistemas y decida lo que mejor se adapte a sus necesidades.

## <a name="adding-elmah-to-an-aspnet-web-application"></a>Agregar ELMAH a una aplicación Web de ASP.NET

Integrar ELMAH en una aplicación ASP.NET nueva o existente es un proceso sencillo y sencillo que tarda menos de cinco minutos. En pocas palabras, implica cuatro pasos sencillos:

1. Descargue ELMAH y agregue el ensamblado `Elmah.dll` a la aplicación Web.
2. Registre los módulos y el controlador HTTP de ELMAH en `Web.config`,
3. Especifique las opciones de configuración de ELMAH y
4. Cree la infraestructura de origen del registro de errores, si es necesario.

Vamos a examinar cada uno de estos cuatro pasos, de uno en uno.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Paso 1: descargar los archivos de proyecto ELMAH y agregar`Elmah.dll`a la aplicación Web

ELMAH 1,0 BETA 3 (compilación 10617), la versión más reciente en el momento de la escritura, se incluye en la descarga disponible con este tutorial. Como alternativa, puede visitar el [sitio web de ELMAH](https://code.google.com/p/elmah/) para obtener la versión más reciente o descargar el código fuente. Extraiga la descarga de ELMAH en una carpeta en el escritorio y busque el archivo de ensamblado ELMAH (`Elmah.dll`).

> [!NOTE]
> El archivo de `Elmah.dll` se encuentra en la carpeta `Bin` de la descarga, que tiene subcarpetas para diferentes versiones de .NET Framework y para las compilaciones de versión y depuración. Use la versión de lanzamiento para la versión de Framework adecuada. Por ejemplo, si va a compilar una aplicación Web de ASP.NET 3,5, copie el archivo de `Elmah.dll` de la carpeta `Bin\net-3.5\Release`.

Después, abra Visual Studio y agregue el ensamblado al proyecto haciendo clic con el botón derecho en el nombre del sitio web en el Explorador de soluciones y eligiendo Agregar referencia en el menú contextual. Aparecerá el cuadro de diálogo Agregar referencia. Vaya a la pestaña examinar y elija el archivo de `Elmah.dll`. Esta acción agrega el archivo de `Elmah.dll` a la carpeta de `Bin` de la aplicación Web.

> [!NOTE]
> El tipo de proyecto de aplicación web (WAP) no muestra la carpeta `Bin` en el Explorador de soluciones. En su lugar, muestra estos elementos en la carpeta referencias.

El ensamblado de `Elmah.dll` incluye las clases utilizadas por el sistema ELMAH. Estas clases se dividen en una de estas tres categorías:

- **Módulos http** : un módulo HTTP es una clase que define controladores de eventos para `HttpApplication` eventos, como el evento `Error`. ELMAH incluye varios módulos HTTP, los tres más alemanes: 

    - `ErrorLogModule`: registra las excepciones no controladas en un origen de registro.
    - `ErrorMailModule`: envía los detalles de una excepción no controlada en un mensaje de correo electrónico.
    - `ErrorFilterModule`: aplica filtros especificados por el desarrollador para determinar qué excepciones se registran y cuáles se omiten.
- **Controladores http** : un controlador HTTP es una clase que es responsable de generar el marcado para un tipo determinado de solicitud. ELMAH incluye controladores HTTP que representan los detalles del error como una página web, como una fuente RSS, o como un archivo delimitado por comas (CSV).
- **Orígenes de registro de errores** : fuera del cuadro ELMAH, puede registrar errores en la memoria, en una base de datos de Microsoft SQL Server, en una base de datos de Microsoft Access, en una base de datos de Oracle, en un archivo XML, en una base de datos de SQLite o en una base de datos de vista dB. Al igual que el sistema de supervisión de estado, la arquitectura de ELMAH se creó con el modelo de proveedor, lo que significa que puede crear y integrar sin problemas sus propios proveedores de origen de registro personalizados, si es necesario.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Paso 2: registro del módulo y el controlador HTTP de ELMAH

Aunque el archivo de `Elmah.dll` contiene los módulos HTTP y el controlador necesarios para registrar automáticamente las excepciones no controladas y mostrar los detalles de los errores de una página web, estos deben registrarse explícitamente en la configuración de la aplicación Web. El módulo HTTP `ErrorLogModule`, una vez registrado, se suscribe al evento `Error` del `HttpApplication`. Cada vez que se genera este evento, el `ErrorLogModule` registra los detalles de la excepción en un origen de registro especificado. Veremos cómo definir el proveedor de origen de registro en la sección siguiente, "configuración de ELMAH". El generador de controladores HTTP `ErrorLogPageFactory` es responsable de generar el marcado al ver el registro de errores de una página web.

La sintaxis específica para registrar módulos y controladores HTTP depende del servidor Web que está potenciando el sitio. En el caso del Servidor de desarrollo de ASP.NET y la versión 6,0 de Microsoft IIS y versiones anteriores, los módulos y controladores HTTP se registran en las secciones `<httpModules>` y `<httpHandlers>`, que aparecen en el elemento `<system.web>`. Si usa IIS 7,0, deben registrarse en las secciones `<modules>` y `<handlers>` del elemento de `<system.webServer>`. Afortunadamente, puede definir los módulos y controladores HTTP en *ambos* lugares independientemente del servidor Web que se use. Esta opción es la más portátil, ya que permite usar la misma configuración en los entornos de desarrollo y producción, independientemente del servidor Web que se use.

Para empezar, registre el módulo HTTP `ErrorLogModule` y el controlador HTTP `ErrorLogPageFactory` en la sección `<httpModules>` y `<httpHandlers>` de `<system.web>`. Si la configuración ya define estos dos elementos, simplemente incluya el elemento `<add>` para el módulo y el controlador HTTP de ELMAH.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

A continuación, registre el módulo y el controlador HTTP de ELMAH en el elemento `<system.webServer>`. Como antes, si este elemento no está ya presente en la configuración, agréguelo.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

De forma predeterminada, IIS 7 se queja si los módulos y controladores HTTP están registrados en la sección `<system.web>`. El atributo `validateIntegratedModeConfiguration` del elemento `<validation>` indica a IIS 7 que suprima estos mensajes de error.

Tenga en cuenta que la sintaxis para registrar el controlador HTTP `ErrorLogPageFactory` incluye un atributo `path`, que se establece en `elmah.axd`. Este atributo informa a la aplicación Web de que si llega una solicitud para una página llamada `elmah.axd`, el controlador HTTP `ErrorLogPageFactory` debe procesar la solicitud. Veremos el `ErrorLogPageFactory` controlador HTTP en acción más adelante en este tutorial.

### <a name="step-3-configuring-elmah"></a>Paso 3: configuración de ELMAH

ELMAH busca sus opciones de configuración en el archivo de `Web.config` del sitio web en una sección de configuración personalizada denominada `<elmah>`. Para usar una sección personalizada en `Web.config` debe definirse primero en el elemento `<configSections>`. Abra el archivo `Web.config` y agregue el marcado siguiente al `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> Si está configurando ELMAH para una aplicación ASP.NET 1. x, quite el atributo `requirePermission="false"` de los elementos `<section>` anteriores.

La sintaxis anterior registra la sección de `<elmah>` personalizada y sus subsecciones: `<security>`, `<errorLog>`, `<errorMail>`y `<errorFilter>`.

A continuación, agregue la sección `<elmah>` a `Web.config`. Esta sección debe aparecer en el mismo nivel que el elemento `<system.web>`. Dentro de la sección `<elmah>` Agregue las secciones `<security>` y `<errorLog>` como las siguientes:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

El atributo `allowRemoteAccess` de la sección `<security>` indica si se permite el acceso remoto. Si este valor se establece en 0, la página web del registro de errores solo se puede ver de forma local. Si este atributo se establece en 1, la Página Web de registro de errores está habilitada para los visitantes remotos y locales. Por ahora, vamos a deshabilitar la Página Web de registro de errores para visitantes remotos. Permitiremos el acceso remoto más adelante, después de que tengamos la oportunidad de analizar los problemas de seguridad que conlleva.

En la sección `<errorLog>` se define el origen del registro de errores, que indica dónde se registran los detalles del error. es similar a la sección `<providers>` del sistema de supervisión de estado. La sintaxis anterior especifica la clase `SqlErrorLog` como el origen del registro de errores, que registra los errores en una base de datos Microsoft SQL Server especificada por el valor del atributo `connectionStringName`.

> [!NOTE]
> ELMAH incluye proveedores de registro de errores adicionales que se pueden usar para registrar errores en un archivo XML, una base de datos de Microsoft Access, una base de datos de Oracle y otros almacenes de datos. Consulte el archivo de `Web.config` de ejemplo que se incluye con la descarga de ELMAH para obtener información sobre cómo usar estos proveedores de registro de errores alternativos.

### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Paso 4: crear la infraestructura de origen de registro de errores

El proveedor de `SqlErrorLog` de ELMAH registra los detalles del error en una base de datos de Microsoft SQL Server especificada. El proveedor de `SqlErrorLog` espera que esta base de datos tenga una tabla denominada `ELMAH_Error` y tres procedimientos almacenados: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`y `ELMAH_LogError`. La descarga de ELMAH incluye un archivo llamado `SQLServer.sql` en la carpeta `db` que contiene el T-SQL para crear esta tabla y estos procedimientos almacenados. Deberá ejecutar estas instrucciones en la base de datos para usar el proveedor de `SqlErrorLog`.

Las **figuras 1** y **2** muestran el explorador de bases de datos en Visual Studio una vez agregados los objetos de base de datos necesarios para el proveedor de `SqlErrorLog`.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Figura 1**: el proveedor de `SqlErrorLog` registra los errores en la tabla `ELMAH_Error`

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Figura 2**: el proveedor de `SqlErrorLog` usa tres procedimientos almacenados

## <a name="elmah-in-action"></a>ELMAH en acción

En este punto se ha agregado ELMAH a la aplicación Web, se ha registrado el `ErrorLogModule` módulo HTTP y el controlador HTTP `ErrorLogPageFactory`, se han especificado las opciones de configuración de ELMAH en `Web.config`y se han agregado los objetos de base de datos necesarios para el proveedor de registro de errores de `SqlErrorLog`. Ahora estamos listos para ver ELMAH en acción. Visite el sitio web de reseñas de libros y visite una página que genera un error en tiempo de ejecución, como `Genre.aspx?ID=foo`, o una página no existente, como `NoSuchPage.aspx`. Lo que se ve al visitar estas páginas depende de la configuración de `<customErrors>` y de si se va a visitar de forma local o remota. (Consulte el [tutorial *Mostrar una página de error personalizada* ](displaying-a-custom-error-page-cs.md) para obtener un actualizador de este tema).

ELMAH no afecta al contenido que se muestra al usuario cuando se produce una excepción no controlada. simplemente registra sus detalles. Se puede tener acceso a este registro de errores desde la página web `elmah.axd` desde la raíz del sitio web, como `http://localhost/BookReviews/elmah.axd`. (Este archivo no existe físicamente en el proyecto, pero cuando una solicitud entra en `elmah.axd` el tiempo de ejecución lo envía al controlador HTTP `ErrorLogPageFactory`, que genera el marcado que se devuelve al explorador).

> [!NOTE]
> También puede usar la página `elmah.axd` para indicar a ELMAH que genere un error de prueba. Al visitar `elmah.axd/test` (como en, `http://localhost/BookReviews/elmah.axd/test`), ELMAH produce una excepción de tipo `Elmah.TestException`, que tiene el mensaje de error: "se trata de una excepción de prueba que se puede pasar por alto sin problemas".

En la **figura 3** se muestra el registro de errores al visitar `elmah.axd` desde el entorno de desarrollo.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Figura 3**: `Elmah.axd` muestra el registro de errores de una página web  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-elmah-cs/_static/image7.png))

El registro de errores de la **figura 3** contiene seis entradas de error. Cada entrada incluye el código de Estado HTTP (404 o 500, para estos errores), el tipo, la descripción, el nombre del usuario que inició sesión cuando se produjo el error y la fecha y la hora. Al hacer clic en el vínculo detalles, se muestra una página que incluye el mismo mensaje de error que se muestra en la pantalla de detalles del error en color amarillo (consulte la **figura 4**) junto con los valores de las variables de servidor en el momento del error (consulte la **figura 5**). También puede ver el XML sin formato en el que se guardan los detalles del error, lo que incluye información adicional, como los valores del encabezado HTTP POST.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Figura 4**: visualización de los detalles del error YSOD  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Figura 5**: exploración de los valores de la colección de variables de servidor en el momento del error  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-elmah-cs/_static/image13.png))

La implementación de ELMAH en el sitio web de producción conlleva:

- Copiar el archivo de `Elmah.dll` en la carpeta de `Bin` en producción,
- Copiar los valores de configuración específicos de ELMAH en el archivo `Web.config` utilizado en producción, y
- Agregar la infraestructura de origen del registro de errores a la base de datos de producción.

Hemos explorado técnicas para copiar archivos del desarrollo a la producción en los tutoriales anteriores. Quizás la manera más sencilla de obtener la infraestructura de origen de registro de errores en la base de datos de producción es usar SQL Server Management Studio para conectarse a la base de datos de producción y, a continuación, ejecutar el archivo de script de `SqlServer.sql`, que creará la tabla y los procedimientos almacenados necesarios.

### <a name="viewing-the-error-details-page-on-production"></a>Visualización de la página de detalles del error en producción

Después de implementar el sitio en producción, visite el sitio web de producción y genere una excepción no controlada. Como en el entorno de desarrollo, ELMAH no tiene ningún efecto en la página de error que se muestra cuando se produce una excepción no controlada. en su lugar, simplemente registra el error. Si intenta visitar la página de registro de errores (`elmah.axd`) desde el entorno de producción, recibirá la página prohibido que se muestra en la **figura 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Figura 6**: de forma predeterminada, los visitantes remotos no pueden ver la Página Web de registro de errores  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-elmah-cs/_static/image16.png))

Recuerde que en la sección `<security>` de la configuración de ELMAH, establecemos el atributo `allowRemoteAccess` en 0, lo que prohíbe a los usuarios remotos ver el registro de errores. Es importante prohibir a los visitantes anónimos que vean el registro de errores, ya que los detalles del error pueden revelar vulnerabilidades de seguridad u otra información confidencial. Si decide establecer este atributo en 1 y habilitar el acceso remoto al registro de errores, es importante bloquear el `elmah.axd` ruta de acceso para que solo los visitantes autorizados puedan acceder a él. Esto puede lograrse agregando un elemento de `<location>` al archivo de `Web.config`.

La configuración siguiente solo permite a los usuarios del rol de administrador tener acceso a la Página Web de registro de errores:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> El rol de administrador y los tres usuarios del sistema (Scott, Jisun y Alice) se agregaron en el [tutorial *configuración de un sitio web que usa servicios de aplicación* ](configuring-a-website-that-uses-application-services-cs.md). Los usuarios Scott y Jisun son miembros del rol de administrador. Para obtener más información sobre la autenticación y la autorización, consulte los [tutoriales de seguridad de los sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Ahora, los usuarios remotos pueden ver el registro de errores en el entorno de producción. Consulte las **figuras 3**, **4**y **5** para obtener capturas de pantalla de la Página Web de registro de errores. Sin embargo, si un usuario anónimo o no administrador intenta ver la página del registro de errores, se le redirigirá automáticamente a la página de inicio de sesión (`Login.aspx`), como se muestra en la **figura 7** .

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Figura 7**: los usuarios no autorizados se redirigen automáticamente a la página de inicio de sesión  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Registrar errores mediante programación

El módulo HTTP de `ErrorLogModule` de ELMAH registra automáticamente las excepciones no controladas en el origen de registro especificado. Como alternativa, puede registrar un error sin tener que generar una excepción no controlada mediante el uso de la clase `ErrorSignal` y su método `Raise`. Se pasa al método `Raise` un objeto `Exception` y se registra como si esa excepción se hubiera producido y se ha alcanzado el tiempo de ejecución de ASP.NET sin ser controlado. La diferencia, sin embargo, es que la solicitud continúa ejecutándose con normalidad después de que se haya llamado al método `Raise`, mientras que una excepción iniciada no controlada interrumpe la ejecución normal de la solicitud y hace que el tiempo de ejecución de ASP.NET muestre la página de error configurada.

La clase `ErrorSignal` es útil en situaciones en las que hay alguna acción que puede producir un error, pero su error no es catastrófico en la operación general que se realiza. Por ejemplo, un sitio web puede contener un formulario que toma la entrada del usuario, la almacena en una base de datos y, a continuación, envía al usuario un correo electrónico que les informa de que se han procesado los datos. ¿Qué ocurre si la información se guarda correctamente en la base de datos, pero se produce un error al enviar el mensaje de correo electrónico? Una opción sería producir una excepción y enviar al usuario a la página de error. Sin embargo, esto puede confundir al usuario a pensar que la información que escribió no se guardó. Otro enfoque sería registrar el error relacionado con el correo electrónico, pero no modificar la experiencia del usuario de ningún modo. Aquí es donde es útil la clase `ErrorSignal`.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>Notificación de error por correo electrónico

Junto con los errores de registro en una base de datos, ELMAH también puede configurarse para enviar por correo electrónico los detalles del error a un destinatario especificado. Esta funcionalidad la proporciona la `ErrorMailModule` módulo HTTP; por lo tanto, debe registrar este módulo HTTP en `Web.config` para poder enviar detalles de error por correo electrónico.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

A continuación, especifique información sobre el correo electrónico de error en la sección `<errorMail>` del elemento de `<elmah>`, que indica el remitente y el destinatario del correo electrónico, el asunto y si el correo electrónico se envía de forma asincrónica.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Una vez establecida la configuración anterior, cuando se produce un error en tiempo de ejecución, se envía un correo electrónico a support@example.com con los detalles del error. El correo electrónico de error de ELMAH incluye la misma información que se muestra en la Página Web de detalles del error, es decir, el mensaje de error, el seguimiento de la pila y las variables de servidor (consulte las **figuras 4** y **5**). El correo electrónico de error también incluye la pantalla de detalles de la excepción amarillo de contenido de muerte como datos adjuntos (`YSOD.html`).

En la **figura 8** se muestra el correo electrónico de error de ELMAH generado al visitar `Genre.aspx?ID=foo`. Mientras que la **figura 8** muestra solo el mensaje de error y el seguimiento de la pila, las variables de servidor se incluyen más abajo en el cuerpo del correo electrónico.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Figura 8**: puede configurar ELMAH para enviar detalles de error por correo electrónico.  
([Haga clic para ver la imagen de tamaño completo](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Registrar solo los errores de interés

De forma predeterminada, ELMAH registra los detalles de cada excepción no controlada, incluidos 404 y otros errores HTTP. Puede indicar a ELMAH que omita estos u otros tipos de errores mediante el filtrado de errores. La lógica de filtrado se realiza mediante el módulo HTTP de `ErrorFilterModule` de ELMAH, que tendrá que registrarse en `Web.config` para poder usar la lógica de filtrado. Las reglas de filtrado se especifican en la sección `<errorFilter>`.

El marcado siguiente indica a ELMAH que no registre los errores 404.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> No olvide que, para usar el filtrado de errores, debe registrar el `ErrorFilterModule` módulo HTTP.

El elemento `<equal>` dentro de la sección `<test>` se conoce como una aserción. Si la aserción se evalúa como true, el error se filtra desde el registro de ELMAH. Hay otras aserciones disponibles, entre las que se incluyen: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`, etc. También puede combinar aserciones mediante los operadores booleanos `<and>` y `<or>`. Además, incluso puede incluir una expresión de JavaScript simple como una aserción o escribir sus propias aserciones en C# o Visual Basic.

Para más información sobre las funcionalidades de filtrado de errores de ELMAH, consulte la [sección filtrado de errores](https://code.google.com/p/elmah/wiki/ErrorFiltering) en el [wiki de elmah](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Resumen

ELMAH proporciona un mecanismo sencillo pero eficaz para registrar errores en una aplicación Web ASP.NET. Al igual que el sistema de supervisión de estado de Microsoft, ELMAH puede registrar errores en una base de datos y enviar los detalles del error a un desarrollador por correo electrónico. A diferencia del sistema de supervisión de estado, ELMAH incluye compatibilidad incorporada para una gama más amplia de almacenes de datos de registro de errores, entre los que se incluyen: Microsoft SQL Server, Microsoft Access, Oracle, archivos XML y muchos otros. Además, ELMAH ofrece un mecanismo integrado para ver el registro de errores y los detalles sobre un error específico de una página web `elmah.axd`. La página `elmah.axd` también puede presentar información de error como una fuente RSS o como un archivo de valores separados por comas (CSV), que puede leer con Microsoft Excel. También puede indicar a ELMAH que filtre los errores del registro mediante aserciones declarativas o de programación. Y ELMAH se pueden usar con aplicaciones de la versión 1. x de ASP.NET.

Cada aplicación implementada debe tener algún mecanismo para registrar automáticamente las excepciones no controladas y enviar notificaciones al equipo de desarrollo. Si esto se consigue mediante la supervisión de estado o ELMAH es secundario. En otras palabras, en realidad no importa si usa la supervisión de estado o ELMAH; Evalúe ambos sistemas y, a continuación, elija el que mejor se adapte a sus necesidades. Lo fundamental es que se ponga en marcha algún mecanismo para registrar excepciones no controladas en el entorno de producción.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [ELMAH: módulos y controladores de registro de errores](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Página del proyecto ELMAH](https://code.google.com/p/elmah/) (código fuente, ejemplos, wiki)
- [Conexión de ELMAH en una aplicación web para detectar excepciones no controladas](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vídeo)
- [Páginas de registro de errores de seguridad](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Uso de módulos y controladores HTTP para crear componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Tutoriales de seguridad de sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [Siguiente](precompiling-your-website-cs.md)
