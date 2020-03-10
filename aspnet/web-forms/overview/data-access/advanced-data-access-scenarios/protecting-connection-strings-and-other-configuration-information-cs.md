---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Proteger las cadenas de conexión y otra informaciónC#de configuración () | Microsoft Docs
author: rick-anderson
description: Una aplicación ASP.NET normalmente almacena información de configuración en un archivo Web. config. Algunos de estos datos son confidenciales y garantizan la protección. Por def...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 15970bee1e990d3a139673efe12486e08f79814c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421075"
---
# <a name="protecting-connection-strings-and-other-configuration-information-c"></a>Proteger las cadenas de conexión y otros datos de configuración (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) o [Descargar PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Una aplicación ASP.NET normalmente almacena información de configuración en un archivo Web. config. Algunos de estos datos son confidenciales y garantizan la protección. De forma predeterminada, este archivo no se servirá al visitante de un sitio web, pero un administrador o un hacker puede tener acceso al sistema de archivos del servidor Web y ver el contenido del archivo. En este tutorial aprendemos que ASP.NET 2,0 nos permite proteger la información confidencial mediante el cifrado de las secciones del archivo Web. config.

## <a name="introduction"></a>Introducción

La información de configuración de las aplicaciones de ASP.NET normalmente se almacena en un archivo XML denominado `Web.config`. En el transcurso de estos tutoriales hemos actualizado el `Web.config` unos pocos veces. Al crear el `Northwind` conjunto de datos con tipo en el [primer tutorial](../introduction/creating-a-data-access-layer-cs.md), por ejemplo, la información de la cadena de conexión se agregó automáticamente a `Web.config` en la sección `<connectionStrings>`. Más adelante, en las [páginas maestras y](../introduction/master-pages-and-site-navigation-cs.md) en el tutorial de navegación del sitio, actualizamos de forma manual `Web.config`, agregando un elemento `<pages>` que indica que todas las páginas de ASP.net del proyecto deben usar el tema `DataWebControls`.

Dado que `Web.config` puede contener datos confidenciales, como cadenas de conexión, es importante que el contenido de `Web.config` se mantenga seguro y esté oculto de los visores no autorizados. De forma predeterminada, las solicitudes HTTP a un archivo con la extensión de `.config` se administran mediante el motor ASP.NET, que devuelve el mensaje *este tipo de página no es atendido* que se muestra en la figura 1. Esto significa que los visitantes no pueden ver el contenido del `Web.config` archivo s simplemente escribiendo http://www.YourServer.com/Web.config en la barra de direcciones del explorador.

[![visitar el archivo Web. config a través de un explorador devuelve un mensaje este tipo de página no es atendido](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Figura 1**: al visitar `Web.config` a través de un explorador se devuelve un mensaje que indica que este tipo de página no es atendido ([haga clic para ver la imagen de tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))

Pero ¿qué ocurre si un atacante puede encontrar algún otro tipo de ataque que le permita ver el contenido del `Web.config` archivo? ¿Qué puede hacer un atacante con esta información y qué pasos se pueden llevar a cabo para proteger aún más la información confidencial dentro de `Web.config`? Afortunadamente, la mayoría de las secciones de `Web.config` no contienen información confidencial. ¿Qué daño puede suponer un atacante si conoce el nombre del tema predeterminado que usan las páginas de ASP.NET?

No obstante, algunas secciones `Web.config` contienen información confidencial que puede incluir cadenas de conexión, nombres de usuario, contraseñas, nombres de servidor, claves de cifrado, etc. Esta información se encuentra normalmente en las siguientes secciones `Web.config`:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

En este tutorial, veremos técnicas para proteger la información de configuración confidencial. Como veremos, la versión 2,0 de .NET Framework incluye un sistema de configuraciones protegidas que permite cifrar y descifrar las secciones de configuración seleccionadas mediante programación.

> [!NOTE]
> En este tutorial se concluye con una mirada a las recomendaciones de Microsoft para conectarse a una base de datos desde una aplicación de ASP.NET. Además de cifrar las cadenas de conexión, puede ayudar a proteger el sistema asegurándose de que se está conectando a la base de datos de manera segura.

## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Paso 1: explorar las opciones de configuración protegida de ASP.NET 2,0 s

ASP.NET 2,0 incluye un sistema de configuración protegida para cifrar y descifrar la información de configuración. Esto incluye métodos en el .NET Framework que se pueden usar para cifrar o descifrar la información de configuración mediante programación. El sistema de configuración protegida utiliza el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite a los desarrolladores elegir qué implementación criptográfica se usa.

El .NET Framework se suministra con dos proveedores de configuración protegidos:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) : usa el [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) asimétrico para el cifrado y el descifrado.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) : usa la API de protección de datos de Windows [(DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) para el cifrado y el descifrado.

Dado que el sistema de configuración protegida implementa el modelo de diseño del proveedor, es posible crear su propio proveedor de configuración protegida y conectarlo a la aplicación. Vea [implementar un proveedor de configuración protegida](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) para obtener más información sobre este proceso.

Los proveedores de RSA y DPAPI usan claves para sus rutinas de cifrado y descifrado, y estas claves se pueden almacenar en el nivel de equipo o de usuario. Las claves de nivel de equipo son ideales para los escenarios en los que la aplicación web se ejecuta en su propio servidor dedicado o si hay varias aplicaciones en un servidor que necesitan compartir información cifrada. Las claves de nivel de usuario son una opción más segura en entornos de hospedaje compartidos donde otras aplicaciones del mismo servidor no deben ser capaces de descifrar las secciones de configuración protegidas de la aplicación.

En este tutorial, nuestros ejemplos usarán el proveedor de DPAPI y las claves de nivel de equipo. En concreto, veremos el cifrado de la sección `<connectionStrings>` en `Web.config`, aunque se puede usar el sistema de configuración protegida para cifrar la mayoría de las `Web.config` sección. Para obtener información sobre el uso de claves de nivel de usuario o el uso del proveedor de RSA, consulte los recursos en la sección lecturas adicionales al final de este tutorial.

> [!NOTE]
> Los proveedores `RSAProtectedConfigurationProvider` y `DPAPIProtectedConfigurationProvider` se registran en el archivo `machine.config` con los nombres de proveedor `RsaProtectedConfigurationProvider` y `DataProtectionConfigurationProvider`, respectivamente. Al cifrar o descifrar la información de configuración, es necesario proporcionar el nombre de proveedor adecuado (`RsaProtectedConfigurationProvider` o `DataProtectionConfigurationProvider`) en lugar del nombre de tipo real (`RSAProtectedConfigurationProvider` y `DPAPIProtectedConfigurationProvider`). Puede encontrar el archivo de `machine.config` en la carpeta `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`.

## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Paso 2: cifrado y descifrado de las secciones de configuración mediante programación

Con unas pocas líneas de código, se puede cifrar o descifrar una sección de configuración concreta mediante un proveedor especificado. El código, como veremos en breve, solo tiene que hacer referencia mediante programación a la sección de configuración adecuada, llamar a su método `ProtectSection` o `UnprotectSection` y, a continuación, llamar al método `Save` para conservar los cambios. Además, el .NET Framework incluye una utilidad de línea de comandos útil que puede cifrar y descifrar la información de configuración. Exploraremos esta utilidad de línea de comandos en el paso 3.

Para ilustrar la protección de la información de configuración mediante programación, cree una página ASP.NET que incluya botones para cifrar y descifrar la sección `<connectionStrings>` en `Web.config`.

Para empezar, abra la página `EncryptingConfigSections.aspx` de la carpeta `AdvancedDAL`. Arrastre un control TextBox desde el cuadro de herramientas hasta el diseñador, estableciendo su propiedad `ID` en `WebConfigContents`, su propiedad `TextMode` en `MultiLine`y sus propiedades `Width` y `Rows` en 95% y 15, respectivamente. Este control TextBox mostrará el contenido de `Web.config` nos permite ver rápidamente si el contenido está cifrado o no. Por supuesto, en una aplicación real nunca desearía mostrar el contenido de `Web.config`.

Debajo del cuadro de texto, agregue dos controles de botón denominados `EncryptConnStrings` y `DecryptConnStrings`. Establezca sus propiedades de texto para cifrar las cadenas de conexión y descifrar las cadenas de conexión.

En este momento, la pantalla debe tener un aspecto similar al de la figura 2.

[![agregar un cuadro de texto y dos controles Web de botón a la página](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Figura 2**: agregar un cuadro de texto y dos controles Web de botón a la página ([haga clic para ver la imagen de tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))

A continuación, es necesario escribir código que cargue y muestre el contenido de `Web.config` en el cuadro de texto `WebConfigContents` cuando se cargue la página por primera vez. Agregue el código siguiente a la clase de código subyacente de la página. Este código agrega un método denominado `DisplayWebConfig` y lo llama desde el controlador de eventos `Page_Load` cuando se `false``Page.IsPostBack`:

[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

El método `DisplayWebConfig` usa la [clase`File`](https://msdn.microsoft.com/library/system.io.file.aspx) para abrir el archivo `Web.config` de la aplicación, la [clase`StreamReader`](https://msdn.microsoft.com/library/system.io.streamreader.aspx) para leer su contenido en una cadena y la [clase`Path`](https://msdn.microsoft.com/library/system.io.path.aspx) para generar la ruta de acceso física al archivo `Web.config`. Estas tres clases se encuentran en el [espacio de nombres`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Por consiguiente, tendrá que agregar una `using` `System.IO` instrucción en la parte superior de la clase de código subyacente o, como alternativa, prefijar estos nombres de clase con `System.IO.`.

A continuación, es necesario agregar controladores de eventos para los dos controles de botón `Click` eventos y agregar el código necesario para cifrar y descifrar la sección `<connectionStrings>` mediante una clave de nivel de equipo con el proveedor DPAPI. En el diseñador, haga doble clic en cada uno de los botones para agregar un `Click` controlador de eventos en la clase de código subyacente y, a continuación, agregue el código siguiente:

[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

El código usado en los dos controladores de eventos es casi idéntico. Ambos empiezan por obtener información sobre el archivo de `Web.config` de la aplicación actual a través del método [`WebConfigurationManager` de clase](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [`OpenWebConfiguration`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Este método devuelve el archivo de configuración web para la ruta de acceso virtual especificada. A continuación, se tiene acceso a la sección `Web.config` File s `<connectionStrings>` a través del [método](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx) [`Configuration` Class](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s`GetSection(sectionName)`, que devuelve un objeto [`ConfigurationSection`](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) .

El objeto `ConfigurationSection` incluye una [propiedad`SectionInformation`](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) que proporciona información adicional y funcionalidad relacionada con la sección de configuración. Como se muestra en el código anterior, podemos determinar si la sección de configuración está cifrada mediante la comprobación de la propiedad `SectionInformation` propiedad `IsProtected`. Además, la sección se puede cifrar o descifrar a través de los métodos `ProtectSection(provider)` y `UnprotectSection` de la propiedad `SectionInformation`.

El método `ProtectSection(provider)` acepta como entrada una cadena que especifica el nombre del proveedor de configuración protegida que se va a utilizar para el cifrado. En el controlador de eventos `EncryptConnString` Button s pasamos DataProtectionConfigurationProvider al método `ProtectSection(provider)` para que se use el proveedor DPAPI. El método `UnprotectSection` puede determinar el proveedor que se usó para cifrar la sección de configuración y, por lo tanto, no requiere ningún parámetro de entrada.

Después de llamar al método `ProtectSection(provider)` o `UnprotectSection`, debe llamar al método `Configuration` Object s [`Save`](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) para conservar los cambios. Una vez que la información de configuración se ha cifrado o descifrado y se han guardado los cambios, llamamos `DisplayWebConfig` para cargar el contenido de `Web.config` actualizado en el control de cuadro de texto.

Una vez que haya escrito el código anterior, pruébelo; para ello, visite la página `EncryptingConfigSections.aspx` a través de un explorador. Debe ver inicialmente una página que muestra el contenido de `Web.config` con la sección `<connectionStrings>` que se muestra en texto sin formato (vea la figura 3).

[![agregar un cuadro de texto y dos controles Web de botón a la página](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Figura 3**: agregar un cuadro de texto y dos controles Web de botón a la página ([haga clic para ver la imagen de tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))

Ahora haga clic en el botón cifrar cadenas de conexión. Si la validación de solicitudes está habilitada, el marcado devuelto desde el cuadro de texto `WebConfigContents` producirá una `HttpRequestValidationException`, que muestra el mensaje, se detectó un valor de `Request.Form` potencialmente peligroso en el cliente. La validación de solicitudes, que está habilitada de forma predeterminada en ASP.NET 2,0, prohíbe los postbacks que incluyen código HTML descodificado y está diseñado para ayudar a evitar ataques de inyección de script. Esta comprobación se puede deshabilitar en el nivel de página o de aplicación. Para desactivarla en esta página, establezca el valor de `ValidateRequest` en `false` en la Directiva `@Page`. La Directiva de `@Page` se encuentra en la parte superior del marcado declarativo de la página.

[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Para obtener más información sobre la validación de solicitudes, su finalidad, cómo deshabilitarla en el nivel de página y de aplicación, así como sobre cómo codificar el marcado HTML, consulte [solicitud de validación-prevención de ataques de scripts](../../../../whitepapers/request-validation.md).

Después de deshabilitar la validación de solicitudes para la página, intente hacer clic de nuevo en el botón cifrar cadenas de conexión. En el postback, se tendrá acceso al archivo de configuración y su `<connectionStrings>` sección se cifrará mediante el proveedor de DPAPI. A continuación, el cuadro de texto se actualiza para mostrar el nuevo contenido de `Web.config`. Como se muestra en la figura 4, la información `<connectionStrings>` está ahora cifrada.

[![clic en el botón cifrar cadenas de conexión cifra la &lt;connectionString&gt;](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Figura 4**: al hacer clic en el botón cifrar cadenas de conexión, se cifra la sección `<connectionString>` ([haga clic para ver la imagen de tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))

A continuación se muestra la sección `<connectionStrings>` cifrada generada en el equipo, aunque se ha quitado parte del contenido del elemento `<CipherData>` por motivos de brevedad:

[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> El elemento `<connectionStrings>` especifica el proveedor que se usa para realizar el cifrado (`DataProtectionConfigurationProvider`). El método `UnprotectSection` usa esta información cuando se hace clic en el botón descifrar cadenas de conexión.

Cuando se tiene acceso a la información de la cadena de conexión desde `Web.config`, ya sea mediante el código que se escribe, desde un control SqlDataSource o desde el código generado automáticamente a partir de los TableAdapters de nuestros conjuntos de datos con tipo, se descifra automáticamente. En Resumen, no es necesario agregar ninguna lógica ni código adicional para descifrar la sección `<connectionString>` cifrada. Para demostrarlo, visite uno de los tutoriales anteriores en este momento, como el tutorial de pantalla simple de la sección informes básicos (`~/BasicReporting/SimpleDisplay.aspx`). Como se muestra en la figura 5, el tutorial funciona exactamente como cabría esperar, lo que indica que la página ASP.NET está descifrando automáticamente la información de la cadena de conexión cifrada.

[![el nivel de acceso a datos descifra automáticamente la información de la cadena de conexión](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Figura 5**: el nivel de acceso a datos descifra automáticamente la información de la cadena de conexión ([haga clic para ver la imagen de tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))

Para revertir la sección `<connectionStrings>` a su representación de texto sin formato, haga clic en el botón descifrar cadenas de conexión. En el postback, debería ver las cadenas de conexión en `Web.config` en texto sin formato. En este punto, la pantalla debe parecerse a la primera vez que visita esta página (vea la figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnet_regiisexe"></a>Paso 3: cifrado de las secciones de configuración con`aspnet_regiis.exe`

El .NET Framework incluye una variedad de herramientas de línea de comandos en la carpeta `$WINDOWS$\Microsoft.NET\Framework\version\`. En el tutorial [uso de dependencias de caché de SQL](../caching-data/using-sql-cache-dependencies-cs.md) , por ejemplo, analizamos el uso de la herramienta de línea de comandos `aspnet_regsql.exe` para agregar la infraestructura necesaria para las dependencias de caché de SQL. Otra herramienta de línea de comandos útil en esta carpeta es la [herramienta de registro de IIS de ASP.net (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Como su nombre implica, la herramienta de registro de IIS de ASP.NET se usa principalmente para registrar una aplicación ASP.NET 2,0 con un servidor Web de Microsoft para profesionales, IIS. Además de las características relacionadas con IIS, la herramienta de registro de IIS de ASP.NET también se puede usar para cifrar o descifrar secciones de configuración especificadas en `Web.config`.

La siguiente instrucción muestra la sintaxis general usada para cifrar una sección de configuración con la herramienta de línea de comandos `aspnet_regiis.exe`:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

la *sección* es la sección de configuración para cifrar (como connectionStrings), el *directorio físico\_* es la ruta de acceso física completa al directorio raíz de la aplicación web y el *proveedor* es el nombre del proveedor de configuración protegida que se va a usar (por ejemplo, DataProtectionConfigurationProvider). O bien, si la aplicación web está registrada en IIS, puede escribir la ruta de acceso virtual en lugar de la ruta de acceso física con la siguiente sintaxis:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

En el siguiente ejemplo de `aspnet_regiis.exe` se cifra la sección `<connectionStrings>` mediante el proveedor DPAPI con una clave de nivel de equipo:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

Del mismo modo, la herramienta de línea de comandos `aspnet_regiis.exe` se puede usar para descifrar las secciones de configuración. En lugar de usar el modificador `-pef`, use `-pdf` (o en lugar de `-pe`, use `-pd`). Además, tenga en cuenta que el nombre del proveedor no es necesario al descifrar.

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Puesto que estamos usando el proveedor DPAPI, que usa claves específicas del equipo, debe ejecutar `aspnet_regiis.exe` desde el mismo equipo desde el que se atienden las páginas Web. Por ejemplo, si ejecuta este programa de línea de comandos desde el equipo de desarrollo local y, a continuación, carga el archivo Web. config cifrado en el servidor de producción, el servidor de producción no podrá descifrar la información de la cadena de conexión desde que se cifró. usar claves específicas del equipo de desarrollo. El proveedor RSA no tiene esta limitación, ya que es posible exportar las claves RSA a otra máquina.

## <a name="understanding-database-authentication-options"></a>Descripción de las opciones de autenticación de base de datos

Antes de que cualquier aplicación pueda emitir `SELECT`, `INSERT`, `UPDATE`o `DELETE` consultas a una base de datos de Microsoft SQL Server, la base de datos debe identificar primero al solicitante. Este proceso se conoce como *autenticación* y SQL Server proporciona dos métodos de autenticación:

- **Autenticación de Windows** : el proceso en el que se ejecuta la aplicación se usa para comunicarse con la base de datos. Al ejecutar una aplicación de ASP.NET a través de Visual Studio 2005 s Servidor de desarrollo de ASP.NET, la aplicación ASP.NET asume la identidad del usuario que ha iniciado sesión actualmente. En el caso de las aplicaciones de ASP.NET en Microsoft Internet Information Server (IIS), las aplicaciones de ASP.NET normalmente suponen la identidad de `domainName``\MachineName` o `domainName``\NETWORK SERVICE`, aunque esto se puede personalizar.
- **Autenticación de SQL** : los valores de ID. de usuario y contraseña se proporcionan como credenciales para la autenticación. Con la autenticación de SQL, el ID. de usuario y la contraseña se proporcionan en la cadena de conexión.

La autenticación de Windows es preferible a la autenticación de SQL porque es más segura. Con la autenticación de Windows, la cadena de conexión es gratuita de un nombre de usuario y una contraseña, y si el servidor Web y el servidor de base de datos residen en dos equipos diferentes, las credenciales no se envían a través de la red en texto sin formato. Sin embargo, con la autenticación de SQL, las credenciales de autenticación están codificadas de forma rígida en la cadena de conexión y se transmiten desde el servidor Web al servidor de bases de datos en texto sin formato.

Estos tutoriales han usado la autenticación de Windows. Para saber qué modo de autenticación se está usando, inspeccione la cadena de conexión. La cadena de conexión de `Web.config` para nuestros tutoriales ha sido:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

La seguridad integrada = true y la falta de un nombre de usuario y contraseña indican que se está usando la autenticación de Windows. En algunas cadenas de conexión, se usa el término conexión de confianza = sí o Integrated Security = SSPI en lugar de Integrated Security = true, pero los tres indican el uso de la autenticación de Windows.

En el ejemplo siguiente se muestra una cadena de conexión que usa la autenticación de SQL. Anote las credenciales incrustadas en la cadena de conexión:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Imagine que un atacante puede ver el archivo de `Web.config` de la aplicación. Si usa la autenticación de SQL para conectarse a una base de datos a la que se puede tener acceso a través de Internet, el atacante puede utilizar esta cadena de conexión para conectarse a la base de datos a través de SQL Management Studio o desde páginas de ASP.NET en su propio sitio Web. Para ayudar a mitigar esta amenaza, cifre la información de la cadena de conexión en `Web.config` mediante el sistema de configuración protegida.

> [!NOTE]
> Para obtener más información sobre los diferentes tipos de autenticación disponibles en SQL Server, vea [Building secure ASP.Net Applications: autenticación, autorización y comunicación segura](https://msdn.microsoft.com/library/aa302392.aspx). Para obtener más ejemplos de cadenas de conexión que ilustran las diferencias entre la sintaxis de autenticación de Windows y SQL, consulte [connectionStrings.com](http://www.connectionstrings.com/).

## <a name="summary"></a>Resumen

De forma predeterminada, no se puede tener acceso a los archivos con una extensión `.config` en una aplicación ASP.NET a través de un explorador. Estos tipos de archivos no se devuelven porque pueden contener información confidencial, como cadenas de conexión de base de datos, nombres de usuario y contraseñas, etc. El sistema de configuración protegido en .NET 2,0 ayuda a proteger aún más la información confidencial permitiendo el cifrado de las secciones de configuración especificadas. Hay dos proveedores de configuración protegidos integrados: uno que usa el algoritmo RSA y otro que usa la API de protección de datos de Windows (DPAPI).

En este tutorial, hemos examinado cómo cifrar y descifrar los valores de configuración mediante el proveedor de DPAPI. Esto se puede realizar mediante programación, como vimos en el paso 2, así como a través de la herramienta de línea de comandos `aspnet_regiis.exe`, que se trató en el paso 3. Para obtener más información sobre el uso de claves de nivel de usuario o el uso del proveedor de RSA en su lugar, vea los recursos en la sección de lecturas adicionales.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Creación de una aplicación de ASP.NET segura: autenticación, autorización y comunicación segura](https://msdn.microsoft.com/library/aa302392.aspx)
- [Cifrar la información de configuración en aplicaciones ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Cifrado de los valores de `Web.config` en ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Cómo: cifrar secciones de configuración en ASP.NET 2,0 con DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Cómo: cifrar secciones de configuración en ASP.NET 2,0 mediante RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [La API de configuración en .NET 2,0](http://www.odetocode.com/Articles/418.aspx)
- [Protección de datos de Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y Randy Schmidt. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [Siguiente](debugging-stored-procedures-cs.md)
