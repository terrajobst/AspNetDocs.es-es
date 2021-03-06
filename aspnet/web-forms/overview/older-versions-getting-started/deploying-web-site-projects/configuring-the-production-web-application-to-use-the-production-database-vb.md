---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Configurar la aplicación Web de producción para usar la base de datos de producción (VB) | Microsoft Docs
author: rick-anderson
description: Como se explicó en los tutoriales anteriores, no es raro que la información de configuración difiera entre los entornos de desarrollo y de producción. Este es...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 7fe4f545a76992ad687827af447d9a9e95bea73f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78516625"
---
# <a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Configurar la aplicación web de producción para usar la base de datos de producción (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Como se explicó en los tutoriales anteriores, no es raro que la información de configuración difiera entre los entornos de desarrollo y de producción. Esto es especialmente cierto para las aplicaciones web controladas por datos, ya que las cadenas de conexión de base de datos difieren entre los entornos de desarrollo y de producción. En este tutorial se exploran las formas de configurar el entorno de producción para incluir la cadena de conexión adecuada con más detalle.

## <a name="introduction"></a>Introducción

Las aplicaciones web controladas por datos suelen usar una base de datos diferente cuando están en desarrollo que en producción. En el caso de las aplicaciones hospedadas por un proveedor de host web y desarrolladas de forma local, la base de datos de desarrollo reside normalmente en el equipo del desarrollador mientras la base de datos de producción se hospeda en un servidor de base de datos en la instalación de la empresa. La implementación de una aplicación web controlada por datos implica copiar la base de datos de desarrollo en el servidor de base de datos de producción. En el tutorial anterior, hemos examinado las formas de llevar a cabo este paso.

La aplicación web utiliza la información de una *cadena de conexión* para establecer una conexión con la base de datos. La cadena de conexión, que normalmente se almacena en `Web.config`, especifica el nombre del servidor de base de datos, el nombre de la base de datos, el contexto de seguridad y otra información. Dado que la base de datos utilizada por la aplicación web depende de si la aplicación web se está ejecutando en los entornos de desarrollo o de producción, las cadenas de conexión deben ser diferentes entre los dos entornos.

No es raro que la información de configuración difiera entre los entornos de desarrollo y de producción. Las *diferencias de configuración comunes entre el tutorial de desarrollo y producción* describen técnicas para mantener información de configuración independiente entre estos dos entornos, así como una breve descripción de las cadenas de conexión a bases de datos. En este tutorial se exploran las formas de configurar el entorno de producción para incluir la cadena de conexión adecuada con más detalle.

## <a name="examining-the-connection-string-information"></a>Examinar la información de la cadena de conexión

La cadena de conexión utilizada por la aplicación Web de revisiones del libro se almacena en el archivo de configuración de la aplicación, `Web.config`. `Web.config` incluye una sección especial para almacenar cadenas de conexión, con el nombre acertado [&lt;connectionstrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). El archivo de `Web.config` para el sitio web de revisiones del libro tiene una cadena de conexión definida en esta sección denominada `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

Cadena de conexión: Data Source = .\SQLEXPRESS; AttachDbFilename = | DataDirectory | \Reviews.MDF; Integrated Security = true; Instancia de usuario = true: se compone de un número de opciones y valores, con pares de opción/valor delimitados por un punto y coma y cada opción y valor delimitados por un signo igual. Las cuatro opciones utilizadas en esta cadena de conexión son:

- `Data Source`: especifica la ubicación del servidor de base de datos y el nombre de la instancia del servidor de base de datos (si existe). El valor, `.\SQLEXPRESS`, es un ejemplo en el que hay un servidor de base de datos y un nombre de instancia. El punto especifica que el servidor de base de datos está en el mismo equipo que la aplicación; el nombre de la instancia es `SQLEXPRESS`.
- `AttachDbFilename`: especifica la ubicación del archivo de base de datos. El valor contiene el marcador de posición `|DataDirectory|`, que se resuelve en la ruta de acceso completa de la carpeta Application s `App_Data` en tiempo de ejecución.
- `Integrated Security`: un valor booleano que indica si se debe usar un nombre de usuario o contraseña especificados al conectarse a la base de datos (false) o las credenciales de la cuenta de Windows actual (true).
- `User Instance`-una opción de configuración específica de las ediciones de SQL Server Express que indica si se permite a los usuarios no administrativos en el equipo local adjuntar y conectarse a una base de datos de SQL Server Express Edition. Consulte [SQL Server Express instancias de usuario](https://msdn.microsoft.com/library/ms254504.aspx) para obtener más información sobre esta configuración.

Las opciones de cadena de conexión permitidas dependen de la base de datos a la que se está conectando y del proveedor de base de datos [ADO.net](http://ADO.NET) que se está usando. Por ejemplo, la cadena de conexión para conectarse a una base de datos de Microsoft SQL Server es diferente de la que se utiliza para conectarse a una base de datos de Oracle. Del mismo modo, la conexión a una base de datos de Microsoft SQL Server mediante el proveedor SqlClient utiliza una cadena de conexión diferente que cuando se usa el proveedor OLE-DB.

Puede compilar la cadena de conexión de base de datos a mano mediante un sitio como [connectionStrings.com](http://www.connectionstrings.com/) como un recurso para las opciones disponibles. Sin embargo, un enfoque más sencillo consiste en agregar la base de datos al Explorador de servidores en Visual Studio y, a continuación, obtener la cadena de conexión de la ventana Propiedades. Vamos a usar esta última técnica para construir la cadena de conexión al servidor de base de datos de producción.

Abra Visual Studio y vaya a la ventana Explorador de servidores (en Visual Web Developer, esta ventana se denomina Explorador de bases de datos). Haga clic con el botón derecho en la opción conexiones de datos y elija la opción Agregar conexión en el menú contextual. Se abrirá el asistente que se muestra en la figura 1. Elija el origen de datos adecuado y haga clic en continuar.

[![elija Agregar una nueva base de datos al Explorador de servidores](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Figura 1**: agregar una nueva base de datos a la explorador de servidores ([haga clic para ver la imagen de tamaño completo](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))

A continuación, especifique la información de conexión de base de datos (consulte la figura 2). Al suscribirse a la empresa de hospedaje Web, debe haber proporcionado información sobre cómo conectarse a la base de datos: el nombre del servidor de base de datos, el nombre de la base de datos, el nombre de usuario y la contraseña que se usarán para conectarse a la base de datos, etc. Después de escribir esta información, haga clic en Aceptar para completar este asistente y agregar la base de datos al Explorador de servidores.

[![especificar la información de conexión a la base de datos](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Figura 2**: especificación de la información de conexión a la base de datos ([haga clic para ver la imagen de tamaño completo](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))

La base de datos del entorno de producción debería aparecer ahora en el Explorador de servidores. Seleccione la base de datos de la Explorador de servidores y vaya a la ventana Propiedades. Allí encontrará una propiedad denominada cadena de conexión con la cadena de conexión de la base de datos. Suponiendo que está usando una base de datos Microsoft SQL Server en producción y el proveedor SqlClient, la cadena de conexión debe ser similar a la siguiente:

<strong>Origen de datos =<em>ServerName</em>; Initial Catalog =<em>DatabaseName</em>; Persist Security info = true; ID. de usuario =<em>nombre</em>de usuario; Contraseña =*contraseña</strong>*

Donde *ServerName*, *DatabaseName*, *username*y *password* son con los valores del nombre del servidor de base de datos, el nombre de la base de datos y el nombre de usuario y la contraseña que le proporcionó su empresa de host Web.

## <a name="deploying-the-book-reviews-web-application"></a>Implementación de la aplicación web Reviews Book

En el tutorial anterior paso a paso, se copia la base de datos de desarrollo en el entorno de producción, pero no se exploró la implementación de la aplicación controlada por datos. En este momento, el entorno de producción contiene la base de datos, pero usa la versión de la aplicación de revisiones del libro con revisiones estáticas. Necesitamos implementar la nueva aplicación controlada por datos en el servidor de producción junto con la información de configuración actualizada.

Dedique un momento a implementar la aplicación controlada por datos desde el entorno de desarrollo a la producción. Este proceso se trató en detalle en los tutoriales anteriores. Si necesita un actualizador, consulte los tutoriales *implementar el sitio web mediante un cliente FTP* o *implementar el sitio web mediante Visual Studio* . Deberá asegurarse de que la cadena de conexión de base de datos de producción sea la que se usa en el entorno de producción, lo que significa que se debe implementar un archivo de `Web.config` alternativo. En concreto, este elemento modificado `Web.config` File s `<connectionStrings>` debe contener la cadena de conexión de la base de datos de producción y debe ser similar al siguiente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Tenga en cuenta que la cadena de conexión del elemento de `<connectionStrings>` tiene el mismo nombre (`ReviewsConnectionString`), pero ahora contiene la cadena de conexión de base de datos de producción en lugar de la cadena de conexión de base de datos de desarrollo.

A menos que tenga un flujo de trabajo de implementación más formalizado, modifique manualmente el archivo de `Web.config` para utilizar la cadena de conexión de la base de datos de producción antes de la implementación (recordar para revertirla a usar la cadena de conexión de base de datos de desarrollo después) o mantener un archivo de `Web.config` independiente con la información de configuración del entorno de producción que se carga en el entorno de

> [!NOTE]
> Si implementa accidentalmente un archivo `Web.config` que contiene la cadena de conexión de la base de datos de desarrollo, se producirá un error cuando la aplicación en producción intente conectarse a la base de datos. Este error se manifiesta como un `SqlException` con un mensaje que informa de que el servidor no se encontró o no estaba accesible.

Una vez que el sitio se haya implementado en producción, visite el sitio de producción a través del explorador. Debería ver y disfrutar de la misma experiencia del usuario que al ejecutar localmente la aplicación controlada por datos. Por supuesto, al visitar el sitio web de producción, el sitio está basado en el servidor de base de datos de producción, mientras que al visitar el sitio web en el entorno de desarrollo se utiliza la base de datos en desarrollo. En la figura 3 se muestra la página *Enséñele Yourself ASP.NET 3,5 in 24 hours* Review del sitio web en el entorno de producción (tenga en cuenta la dirección URL en la barra de direcciones del explorador).

[![la aplicación controlada por datos ya está disponible en producción.](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Figura 3**: la aplicación controlada por datos ya está disponible en producción. ([Haga clic para ver la imagen de tamaño completo](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))

### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Almacenar cadenas de conexión en un archivo de configuración independiente

Una técnica común para mantener información de configuración independiente en los entornos de desarrollo y producción es tener dos versiones de la `Web.config`: una para el entorno de desarrollo y otra para la producción. En el momento de la implementación, se puede copiar la versión de `Web.config` adecuada en el entorno de producción. Idealmente, este proceso se automatizaría como parte del flujo de trabajo de implementación.

En lugar de mantener dos archivos de `Web.config` independientes, puede proporcionar, opcionalmente, diferencias más granulares. Los elementos que componen el archivo de `Web.config` pueden definirse en archivos de configuración externos a los que se hace referencia en el archivo de `Web.config`. En pocas palabras, puede tener un archivo `Web.config` para ambos entornos que hagan referencia a un archivo databaseConnectionStrings. config, que contendría las cadenas de conexión utilizadas por la aplicación y sería única para cada entorno. Me veo que la separación de la información de configuración diferente en archivos independientes proporciona un tidier y un archivo de `Web.config` más sencillo y describe claramente las diferencias de configuración entre los entornos de desarrollo y de producción.

Para usar esta técnica, empiece por crear una nueva carpeta en la aplicación web denominada `ConfigSections`. A continuación, agregue dos archivos a esta nueva carpeta denominada databaseConnectionStrings. dev. config y databaseConnectionStrings. Production. config. A continuación, copie el elemento `<connectionStrings>` de `Web.config` en los archivos databaseConnectionStrings. dev. config y databaseConnectionStrings. Production. config y, a continuación, modifique la cadena de conexión en el archivo databaseConnectionStrings. Production. config para que especifique la cadena de conexión de la base de datos de producción. Por ejemplo, el archivo databaseConnectionStrings. dev. config debe contener solo el elemento `<connectionStrings>` con una cadena de conexión que haga referencia a la base de datos de desarrollo:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Del mismo modo, el archivo databaseConnectionStrings. Production. config debe contener solo un elemento `<connectionStrings>`, pero uno que tenga la cadena de conexión de la base de datos de producción.

Realice una copia del archivo databaseConnectionStrings. dev. config y asígnele el nombre databaseConnectionStrings. config.

> [!NOTE]
> Puede asignar un nombre a un archivo de configuración distinto de databaseConnectionStrings. config, si lo desea, como `connectionStrings.config` o `dbInfo.config`. Sin embargo, asegúrese de asignar un nombre al archivo con una extensión `.config` como `.config` los archivos son, de forma predeterminada, no servidas por el motor ASP.NET. Si asigna otro nombre al archivo, como `connectionStrings.txt`, un usuario podría apuntar a su explorador a [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) y ver el contenido del archivo.

En este momento, la carpeta `ConfigSections` debe contener tres archivos (consulte la figura 4). Los archivos databaseConnectionStrings. dev. config y databaseConnectionStrings. Production. config contienen las cadenas de conexión para los entornos de desarrollo y producción, respectivamente. El archivo databaseConnectionStrings. config contiene la información de la cadena de conexión que usará la aplicación web en tiempo de ejecución. Por consiguiente, el archivo databaseConnectionStrings. config debe ser idéntico al archivo databaseConnectionStrings. dev. config en el entorno de desarrollo, mientras que en producción el archivo databaseConnectionStrings. config debe ser idéntico a databaseConnectionStrings. Production. config.

[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Figura 4**: ConfigSections ([haga clic para ver la imagen de tamaño completo](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))

Ahora necesitamos indicar a `Web.config` que use el archivo databaseConnectionStrings. config para el almacén de cadenas de conexión. Abra `Web.config` y reemplace el elemento `<connectionStrings>` existente por lo siguiente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

El atributo `configSource` especifica una ruta de acceso física relativa al archivo de `Web.config`. Si el archivo de `.config` externo está en el mismo directorio que `Web.config`, establezca este atributo en el nombre de archivo del archivo de `.config`. Si se encuentra en un subdirectorio, como es el caso de databaseConnectionStrings. config, especifique la subcarpeta con una barra diagonal inversa para delimitar los nombres de carpeta y archivo, como ConfigSections\databaseConnectionStrings.config.

Con esta modificación, los entornos de desarrollo y producción contienen el mismo archivo de `Web.config`. Ahora la única diferencia es el archivo databaseConnectionStrings. config. Copie el archivo databaseConnectionStrings. Production. config en producción y cambie su nombre a databaseConnectionStrings. config. Si, en el futuro, hay cambios en la cadena de conexión de la base de datos de producción, tendrá que hacerlo en el archivo databaseConnectionStrings. Production. config y, a continuación, cargar ese archivo en producción, cambiando el nombre de databaseConnectionStrings. config.

> [!NOTE]
> Puede especificar la información de cualquier `Web.config` elemento en un archivo independiente y usar el atributo `configSource` para hacer referencia a ese archivo desde `Web.config`.

## <a name="summary"></a>Resumen

Las aplicaciones controladas por datos suelen utilizar bases de datos diferentes en los entornos de desarrollo y producción. Por lo tanto, las cadenas de conexión de base de datos almacenadas en la configuración de la aplicación web deben ser únicas por entorno. En este tutorial, hemos examinado cómo determinar la cadena de conexión de la base de datos de producción y las maneras de mantener la información de la cadena de conexión única en los dos entornos.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Cadenas de conexión y archivos de configuración](https://msdn.microsoft.com/library/ms254494.aspx)
- [Información de cadenas de configuración de base de datos @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Quitar la configuración del archivo Web. config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentación técnica del elemento&gt; de &lt;connectionStrings](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-vb.md)
> [Siguiente](configuring-a-website-that-uses-application-services-vb.md)
