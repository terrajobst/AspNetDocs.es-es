---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Información general sobre la autenticación deC#formularios () | Microsoft Docs
author: rick-anderson
description: Crear rutas personalizadas
ms.author: riande
ms.date: 01/14/2008
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 009c3f84e00d648ede4a15e530ceac2d23e01eec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620752"
---
# <a name="an-overview-of-forms-authentication-c"></a>Información general de la autenticación deC#formularios ()

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> En este tutorial, cambiaremos de mero debate a implementación; en concreto, veremos la implementación de la autenticación de formularios. La aplicación web que comenzamos a crear en este tutorial se seguirá compilando en los tutoriales siguientes, ya que pasamos de la autenticación de formularios simples a la pertenencia y los roles.
> 
> Consulte este vídeo para obtener más información sobre este tema: [uso de la autenticación de formularios básica en ASP.net](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Introducción

En el [tutorial anterior](security-basics-and-asp-net-support-cs.md) se describen las distintas opciones de autenticación, autorización y cuenta de usuario proporcionadas por ASP.net. En este tutorial, cambiaremos de mero debate a implementación; en concreto, veremos la implementación de la autenticación de formularios. La aplicación web que comenzamos a crear en este tutorial se seguirá compilando en los tutoriales siguientes, ya que pasamos de la autenticación de formularios simples a la pertenencia y los roles.

Este tutorial comienza con una visión detallada del flujo de trabajo de autenticación de formularios, un tema que hemos tratado en el tutorial anterior. A continuación, se creará un sitio web de ASP.NET a través del cual se mostrarán los conceptos de autenticación de formularios. A continuación, configuraremos el sitio para usar la autenticación de formularios, crearemos una página de inicio de sesión simple y veremos cómo determinar, en el código, si un usuario está autenticado y, en ese caso, el nombre de usuario con el que inició sesión.

Entender el flujo de trabajo de autenticación de formularios, habilitarlo en una aplicación web y crear las páginas de inicio y cierre de sesión son todos los pasos fundamentales para crear una aplicación de ASP.NET que admita cuentas de usuario y autentique a los usuarios a través de una página web. Debido a esto, y dado que estos tutoriales se basan unos en otros, le recomendamos que siga este tutorial en su totalidad antes de pasar al siguiente aunque ya haya tenido experiencia en configurar la autenticación de formularios en proyectos anteriores.

## <a name="understanding-the-forms-authentication-workflow"></a>Descripción del flujo de trabajo de autenticación de formularios

Cuando el tiempo de ejecución de ASP.NET procesa una solicitud de un recurso ASP.NET, como una página ASP.NET o un servicio Web ASP.NET, la solicitud genera una serie de eventos durante su ciclo de vida. Hay eventos generados al principio y al final de la solicitud, los que se producen cuando la solicitud se autentica y se autoriza, se genera un evento en el caso de una excepción no controlada, etc. Para ver una lista completa de los eventos, consulte los [eventos del objeto HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

Los *módulos http* son clases administradas cuyo código se ejecuta como respuesta a un evento determinado en el ciclo de vida de la solicitud. ASP.NET se suministra con varios módulos HTTP que realizan tareas esenciales en segundo plano. Dos módulos HTTP integrados que son especialmente relevantes para nuestro debate son:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** : autentica al usuario mediante la inspección del vale de autenticación de formularios, que normalmente se incluye en la colección de cookies del usuario. Si no hay ningún vale de autenticación de formularios, el usuario es anónimo.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** : determina si el usuario actual está autorizado o no para tener acceso a la dirección URL solicitada. Este módulo determina la autoridad mediante la consulta de las reglas de autorización especificadas en los archivos de configuración de la aplicación. ASP.NET también incluye el [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) que determina la autoridad mediante la consulta de las ACL de los archivos solicitados.

El `FormsAuthenticationModule` intenta autenticar al usuario antes de que se ejecute el `UrlAuthorizationModule` (y `FileAuthorizationModule`). Si el usuario que realiza la solicitud no tiene autorización para obtener acceso al recurso solicitado, el módulo de autorización finaliza la solicitud y devuelve un estado [no autorizado HTTP 401](http://www.checkupdown.com/status/E401.html) . En escenarios de autenticación de Windows, se devuelve el Estado HTTP 401 al explorador. Este código de estado hace que el explorador solicite sus credenciales al usuario a través de un cuadro de diálogo modal. Sin embargo, con la autenticación de formularios, el estado no autorizado HTTP 401 nunca se envía al explorador porque FormsAuthenticationModule detecta este estado y lo modifica para redirigir al usuario a la página de inicio de sesión en su lugar (a través de un estado de [redirección HTTP 302](http://www.checkupdown.com/status/E302.html) ).

La responsabilidad de la página de inicio de sesión es determinar si las credenciales del usuario son válidas y, en tal caso, crear un vale de autenticación de formularios y redirigir al usuario a la página que estaba intentando visitar. El vale de autenticación se incluye en las solicitudes posteriores a las páginas del sitio web, que el `FormsAuthenticationModule` usa para identificar al usuario.

![Flujo de trabajo de autenticación de formularios](an-overview-of-forms-authentication-cs/_static/image1.png)

**Figura 1**: flujo de trabajo de autenticación de formularios

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Recordar el vale de autenticación a través de visitas de página

Después del inicio de sesión, el vale de autenticación de formularios debe enviarse de vuelta al servidor Web en cada solicitud para que el usuario siga conectado mientras examina el sitio. Esto se logra normalmente colocando el vale de autenticación en la colección de cookies del usuario. Las [cookies](http://en.wikipedia.org/wiki/HTTP_cookie) son pequeños archivos de texto que residen en el equipo del usuario y se transmiten en los encabezados HTTP en cada solicitud al sitio web que creó la cookie. Por lo tanto, una vez que el vale de autenticación de formularios se ha creado y almacenado en las cookies del explorador, cada visita posterior a ese sitio envía el vale de autenticación junto con la solicitud, lo que identifica al usuario.

Un aspecto de las cookies es su expiración, que es la fecha y la hora en que el explorador descarta la cookie. Cuando expira la cookie de autenticación de formularios, el usuario ya no se puede autenticar y, por tanto, se vuelve anónimo. Cuando un usuario visita desde un terminal público, lo más probable es que quiera que expire su vale de autenticación cuando cierre el explorador. Sin embargo, al visitar desde casa, es posible que el mismo usuario desee que el vale de autenticación se recuerde en los reinicios del explorador para que no tengan que volver a iniciar sesión cada vez que visiten el sitio. Esta decisión suele realizarla el usuario en forma de una casilla "Recordarme" en la página de inicio de sesión. En el paso 3, examinaremos cómo implementar una casilla Recordarme en la página de inicio de sesión. En el siguiente tutorial se trata la configuración de tiempo de espera del vale de autenticación en detalle.

> [!NOTE]
> Es posible que el agente de usuario que se usa para iniciar sesión en el sitio web no admita cookies. En tal caso, ASP.NET puede usar vales de autenticación de formularios sin cookies. En este modo, el vale de autenticación se codifica en la dirección URL. Veremos Cuándo se usan vales de autenticación sin cookies y cómo se crean y administran en el siguiente tutorial.

### <a name="the-scope-of-forms-authentication"></a>El ámbito de autenticación de formularios

El `FormsAuthenticationModule` es código administrado que forma parte del tiempo de ejecución de ASP.NET. Antes de la versión 7 del servidor Web de [Internet Information Services (IIS)](https://www.iis.net/) de Microsoft, había una barrera distinta entre la CANALización http de IIS y la canalización del tiempo de ejecución de ASP.net. En Resumen, en IIS 6 y versiones anteriores, el `FormsAuthenticationModule` solo se ejecuta cuando se delega una solicitud desde IIS al tiempo de ejecución de ASP.NET. De forma predeterminada, IIS procesa contenido estático en sí mismo, como páginas HTML y archivos de imagen y CSS, y solo entrega las solicitudes al tiempo de ejecución de ASP.NET cuando se solicita una página con una extensión. aspx,. asmx o. ashx.

Sin embargo, IIS 7 permite canalizaciones integradas de IIS y ASP.NET. Con algunas opciones de configuración, puede configurar IIS 7 para que invoque la FormsAuthenticationModule para *todas* las solicitudes. Además, con IIS 7 puede definir reglas de autorización de URL para archivos de cualquier tipo. Para obtener más información, vea [cambios entre la seguridad de IIS6 y IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [la seguridad de la plataforma web y la](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements) [autorización de dirección URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Larga historia breve, en las versiones anteriores a IIS 7, solo puede utilizar la autenticación de formularios para proteger los recursos administrados por el tiempo de ejecución de ASP.NET. Del mismo modo, las reglas de autorización de URL solo se aplican a los recursos administrados por el tiempo de ejecución de ASP.NET. Pero con IIS 7 es posible integrar FormsAuthenticationModule y UrlAuthorizationModule en la canalización HTTP de IIS, con lo que se amplía esta funcionalidad a todas las solicitudes.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Paso 1: creación de un sitio web de ASP.NET para esta serie de tutoriales

Para llegar a la mayor audiencia posible, el sitio web de ASP.NET que se va a crear a lo largo de esta serie se creará con la versión gratuita de Microsoft de Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Implementaremos el `SqlMembershipProvider` almacén de usuario en una base de datos de [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) . Si usa Visual Studio 2005 o una edición diferente de Visual Studio 2008 o SQL Server, no se preocupe: los pasos serán casi idénticos y se señalarán las diferencias no triviales.

> [!NOTE]
> La aplicación Web de demostración utilizada en cada tutorial está disponible como descarga. Esta aplicación descargable se creó con Visual Web Developer 2008 destinado a la versión 3,5 de .NET Framework. Dado que la aplicación está destinada a .NET 3,5, su archivo Web. config incluye elementos de configuración adicionales, específicos de 3,5. Larga historia breve, si todavía tiene que instalar .NET 3,5 en el equipo, la aplicación web descargable no funcionará sin quitar primero el marcado específico de 3,5 de Web. config.

Antes de poder configurar la autenticación mediante formularios, primero necesitamos un sitio web de ASP.NET. Empiece por crear un nuevo sitio web de ASP.NET basado en el sistema de archivos. Para ello, inicie Visual Web Developer y, a continuación, vaya al menú Archivo y elija nuevo sitio web, mostrando el cuadro de diálogo nuevo sitio Web. Elija la plantilla de sitio web de ASP.NET, establezca la lista desplegable ubicación en sistema de archivos, elija una carpeta para colocar el sitio web y establezca el idioma C#en. Se creará un nuevo sitio web con una página default. aspx ASP.NET, una aplicación\_carpeta de datos y un archivo Web. config.

> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios web y proyectos de aplicación Web. Los proyectos de sitio web no tienen un archivo de proyecto, mientras que los proyectos de aplicación web imitan la arquitectura de proyecto en Visual Studio .NET 2002/2003: incluyen un archivo de proyecto y compilan el código fuente del proyecto en un único ensamblado, que se coloca en la carpeta/bin. Visual Studio 2005 inicialmente solo admitía proyectos de sitio web, aunque el modelo de proyecto de aplicación web se representó con Service Pack 1; Visual Studio 2008 ofrece ambos modelos de proyecto. Sin embargo, las ediciones 2005 y 2008 de Visual Web Developer, solo admiten proyectos de sitio Web. Usaré el modelo de proyecto de sitio Web. Si usa una edición que no es Express y desea usar el modelo de [proyecto de aplicación web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) en su lugar, no dude en hacerlo, pero tenga en cuenta que puede haber algunas discrepancias entre lo que ve en la pantalla y los pasos que debe realizar en comparación con las capturas de pantalla que se muestran y las instrucciones proporcionadas en estos tutoriales.

[![crear un nuevo sitio web basado en el sistema de archivos](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Figura 2**: crear un nuevo sitio web basado en el sistema de archivos ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image4.png))

### <a name="adding-a-master-page"></a>Agregar una página maestra

A continuación, agregue una nueva página maestra al sitio en el directorio raíz denominado site. Master. [Las páginas maestras](https://msdn.microsoft.com/library/wtxbf3hh.aspx) permiten a un desarrollador de páginas definir una plantilla para todo el sitio que se puede aplicar a las páginas de ASP.net. La principal ventaja de las páginas maestras es que la apariencia general del sitio se puede definir en una sola ubicación, por lo que resulta fácil actualizar o retocar el diseño del sitio.

[![agregar una página maestra denominada site. Master al sitio web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Figura 3**: agregar una página maestra denominada site. Master al sitio web ([haga clic para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image7.png))

Defina el diseño de página para todo el sitio aquí en la página maestra. Puede usar el Vista de diseño y agregar el diseño o los controles Web que necesite, o puede Agregar manualmente el marcado de forma manual en la vista Código fuente. He estructurado el diseño de la página maestra para imitar el diseño usado en la serie de tutoriales mi *[trabajo con datos en ASP.NET 2,0](../../data-access/index.md)* (consulte la figura 4). La página maestra utiliza [hojas de estilos en cascada](http://www.w3schools.com/css/default.asp) para colocar y estilos con la configuración de CSS definida en el archivo Style. CSS (que se incluye en la descarga asociada de este tutorial). Aunque no se puede determinar a partir del marcado que se muestra a continuación, las reglas de CSS se definen de forma que la navegación &lt;el contenido de la&gt;div está totalmente posicionada para que aparezca a la izquierda y tenga un ancho fijo de 200 píxeles.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Una página maestra define el diseño de página estático y las regiones que pueden editar las páginas de ASP.NET que usan la página maestra. Estas regiones modificables de contenido se indican mediante el control `ContentPlaceHolder`, que se puede observar en el contenido &lt;&gt;div. Nuestra página maestra tiene un único `ContentPlaceHolder` (MainContent), pero la página maestra puede tener varios ContentPlaceHolders.

Con el marcado especificado anteriormente, al cambiar a la Vista de diseño se muestra el diseño de la página maestra. Cualquier página de ASP.NET que use esta página maestra tendrá este diseño uniforme, con la capacidad de especificar el marcado para la región de `MainContent`.

[![la página maestra, cuando se ve a través de la vista de diseño](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Figura 4**: la página maestra, cuando se ve a través de la vista de diseño ([haga clic para ver la imagen a tamaño completo](an-overview-of-forms-authentication-cs/_static/image10.png))

### <a name="creating-content-pages"></a>Crear páginas de contenido

En este punto tenemos una página default. aspx en nuestro sitio web, pero no usa la página maestra que acabamos de crear. Aunque es posible manipular el marcado declarativo de una página web para usar una página maestra, si la página no contiene ningún contenido, es más fácil eliminar la página y volver a agregarla al proyecto, especificando la página maestra que se va a usar. Por lo tanto, empiece por eliminar default. aspx del proyecto.

A continuación, haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y elija Agregar un nuevo formulario web denominado Default. aspx. Esta vez, active la casilla "seleccionar página maestra" y elija la página maestra site. Master de la lista.

[![agregar una nueva página default. aspx seleccionando una página maestra](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Figura 5**: agregar una nueva página default. aspx seleccionando una página maestra ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image13.png))

![Usar la página maestra site. Master](an-overview-of-forms-authentication-cs/_static/image14.png)

**Figura 6**: uso de la página maestra de site. Master

> [!NOTE]
> Si usa el modelo de proyecto de aplicación Web, el cuadro de diálogo Agregar nuevo elemento no incluye una casilla "seleccionar página maestra". En su lugar, debe agregar un elemento de tipo "formulario de contenido web". Después de elegir la opción "formulario de contenido web" y hacer clic en agregar, Visual Studio mostrará el mismo cuadro de diálogo Seleccionar un patrón que se muestra en la figura 6.

El nuevo marcado declarativo de la página default. aspx incluye solo una directiva de @Page que especifica la ruta de acceso al archivo de la página maestra y un control de contenido para el MainContent ContentPlaceHolder de la página maestra.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Por ahora, deje vacío default. aspx. Volveremos a él más adelante en este tutorial para agregar contenido.

> [!NOTE]
> Nuestra página maestra incluye una sección para un menú u otra interfaz de navegación. Se creará una interfaz de este tipo en un tutorial futuro.

## <a name="step-2-enabling-forms-authentication"></a>Paso 2: habilitar la autenticación de formularios

Con el sitio web de ASP.NET creado, la siguiente tarea consiste en habilitar la autenticación de formularios. La configuración de autenticación de la aplicación se especifica mediante el [elemento`<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx) en Web. config. El elemento `<authentication>` contiene un solo atributo denominado Mode que especifica el modelo de autenticación utilizado por la aplicación. Este atributo puede tener uno de los cuatro valores siguientes:

- **Windows** : como se describe en el tutorial anterior, cuando una aplicación usa la autenticación de Windows, es responsabilidad del servidor Web autenticar al visitante, lo que normalmente se realiza a través de la autenticación básica, implícita o integrada de Windows.
- **Formularios**: los usuarios se autentican a través de un formulario en una página web.
- **Passport**: los usuarios se autentican mediante Passport Network de Microsoft.
- **Ninguno**: no se utiliza ningún modelo de autenticación; todos los visitantes son anónimos.

De forma predeterminada, las aplicaciones de ASP.NET usan la autenticación de Windows. Para cambiar el tipo de autenticación a la autenticación de formularios, es necesario modificar el atributo de modo del elemento `<authentication>` a Forms.

Si el proyecto aún no contiene un archivo Web. config, agregue uno ahora; para ello, haga clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones, elija Agregar nuevo elemento y, a continuación, agregue un archivo de configuración Web.

[![si el proyecto aún no incluye Web. config, agréguelo ahora](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Figura 7**: Si el proyecto aún no incluye Web. config, agréguelo ahora ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image17.png))

A continuación, busque el elemento `<authentication>` y actualícelo para utilizar la autenticación de formularios. Después de este cambio, el marcado del archivo Web. config debe ser similar al siguiente:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Como Web. config es un archivo XML, es importante el uso de mayúsculas y minúsculas. Asegúrese de establecer el atributo modo en Forms, con una "F" mayúscula. Si usa otra grafía, como "formularios", recibirá un error de configuración al visitar el sitio a través de un explorador.

Opcionalmente, el elemento `<authentication>` puede incluir un elemento secundario `<forms>` que contenga la configuración específica de la autenticación de formularios. Por ahora, vamos a usar la configuración de autenticación de formularios predeterminada. Exploraremos el elemento secundario `<forms>` con más detalle en el siguiente tutorial.

## <a name="step-3-building-the-login-page"></a>Paso 3: compilar la página de inicio de sesión

Con el fin de admitir la autenticación de formularios, nuestro sitio web necesita una página de inicio de sesión. Como se describe en la sección "Descripción del flujo de trabajo de autenticación de formularios", el `FormsAuthenticationModule` redirigirá automáticamente al usuario a la página de inicio de sesión si intenta obtener acceso a una página que no esté autorizado a ver. También hay controles Web ASP.NET que mostrarán un vínculo a la página de inicio de sesión para los usuarios anónimos. Esto nos lleva la pregunta "¿Cuál es la dirección URL de la página de inicio de sesión?"

De forma predeterminada, el sistema de autenticación de formularios espera que la página de inicio de sesión se denomine login. aspx y se coloque en el directorio raíz de la aplicación Web. Si desea usar una dirección URL de la página de inicio de sesión diferente, puede especificarla en Web. config. Veremos cómo hacerlo en el tutorial siguiente.

La página de inicio de sesión tiene tres responsabilidades:

1. Proporcione una interfaz que permita al visitante escribir sus credenciales.
2. Determine si las credenciales enviadas son válidas.
3. "Inicie sesión" al usuario mediante la creación del vale de autenticación de formularios.

### <a name="creating-the-login-pages-user-interface"></a>Crear la interfaz de usuario de la página de inicio de sesión

Comencemos con la primera tarea. Agregue una nueva página ASP.NET al directorio raíz del sitio denominado login. aspx y asócielo a la página maestra site. Master.

[![agregar una nueva página ASP.NET denominada login. aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Figura 8**: agregue una nueva página de ASP.net denominada login. aspx ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image20.png))

La interfaz de página de inicio de sesión típica consta de dos cuadros de texto: uno para el nombre del usuario, uno para su contraseña y un botón para enviar el formulario. Los sitios web a menudo incluyen una casilla "Recordarme" que, si está activada, conserva el vale de autenticación resultante en los reinicios del explorador.

Agregue dos cuadros de texto a login. aspx y establezca sus propiedades de `ID` en el nombre de usuario y la contraseña, respectivamente. Establezca también la propiedad `TextMode` de la contraseña en password. Después, agregue un control CheckBox, estableciendo su `ID` propiedad en RememberMe y su propiedad `Text` en "Recuérdame". Después, agregue un botón denominado LoginButton cuya propiedad `Text` esté establecida en "login". Y, por último, agregue un control Web Label y establezca su propiedad `ID` en InvalidCredentialsMessage, su propiedad `Text` en "su nombre de usuario o contraseña no son válidos. Vuelva a intentarlo. ", su propiedad `ForeColor` en rojo y su propiedad `Visible` en false.

En este punto, la pantalla debe ser similar a la captura de pantalla de la ilustración 9 y la sintaxis declarativa de la página debe ser similar a la siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]

[![la página de inicio de sesión contiene dos cuadros de texto, una casilla, un botón y una etiqueta](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Figura 9**: la página de inicio de sesión contiene dos cuadros de texto, una casilla, un botón y una etiqueta ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image23.png))

Por último, cree un controlador de eventos para el evento click de LoginButton. En el diseñador, simplemente haga doble clic en el control de botón para crear este controlador de eventos.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Determinar si las credenciales proporcionadas son válidas

Ahora tenemos que implementar la tarea 2 en el controlador de eventos Click del botón, para determinar si las credenciales proporcionadas son válidas. Para ello, debe haber un almacén de usuario que contenga todas las credenciales de los usuarios para que podamos determinar si las credenciales proporcionadas coinciden con las credenciales conocidas.

Antes de ASP.NET 2,0, los desarrolladores eran responsables de implementar sus propios almacenes de usuarios y escribir el código para validar las credenciales proporcionadas en el almacén. La mayoría de los desarrolladores implementarían el almacén de usuario en una base de datos, creando una tabla denominada usuarios con columnas como nombre de usuario, contraseña, correo electrónico, LastLoginDate, etc. Esta tabla, a continuación, tendría un registro por cada cuenta de usuario. Comprobar las credenciales proporcionadas por un usuario implicaría consultar la base de datos en busca de un nombre de usuario coincidente y asegurarse de que la contraseña de la base de datos correspondía a la contraseña proporcionada.

Con ASP.NET 2,0, los desarrolladores deben usar uno de los proveedores de pertenencia para administrar el almacén de usuario. En esta serie de tutoriales, usaremos SqlMembershipProvider, que usa una base de datos SQL Server para el almacén del usuario. Al usar SqlMembershipProvider, es necesario implementar un esquema de base de datos específico que incluya las tablas, vistas y procedimientos almacenados que espera el proveedor. Examinaremos cómo implementar este esquema en el tutorial ***crear el esquema de pertenencia en SQL Server*** . Con el proveedor de pertenencia, la validación de las credenciales del usuario es tan sencilla como llamar al [método ValidateUser (*nombre*de usuario, *contraseña*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx)de la [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx), que devuelve un valor booleano que indica si la validez de la combinación de *nombre de usuario* y *contraseña* . Al ver que todavía no hemos implementado el almacén de usuario de SqlMembershipProvider, no podemos usar el método ValidateUser de la clase de pertenencia en este momento.

En lugar de dedicar tiempo a crear nuestra propia tabla de base de datos de usuarios personalizados (que sería obsoleta una vez que hemos implementado el SqlMembershipProvider), vamos a codificar de forma rígida las credenciales válidas dentro de la propia página de inicio de sesión. En el controlador de eventos Click del LoginButton, agregue el código siguiente:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Como puede ver, hay tres cuentas de usuario válidas: Scott, Jisun y Sam, y las tres tienen la misma contraseña ("contraseña"). El código recorre en bucle las matrices de usuarios y contraseñas que buscan un nombre de usuario y una contraseña válidos. Si el nombre de usuario y la contraseña son válidos, es necesario iniciar sesión en el usuario y, después, redirigirlos a la página correspondiente. Si las credenciales no son válidas, se muestra la etiqueta InvalidCredentialsMessage.

Cuando un usuario escribe credenciales válidas, he mencionado que se le redirigirá a la "página correspondiente". ¿Cuál es la página adecuada? Recuerde que cuando un usuario visita una página que no está autorizado a ver, el FormsAuthenticationModule los redirige automáticamente a la página de inicio de sesión. Al hacerlo, incluye la dirección URL solicitada en QueryString a través del parámetro ReturnUrl. Es decir, si un usuario intentó visitar ProtectedPage. aspx y no estaba autorizado para hacerlo, el objeto FormsAuthenticationModule los redirigirá a:

¿Login. aspx? ReturnUrl = ProtectedPage. aspx

Tras iniciar sesión correctamente, el usuario debe redirigirse de nuevo a ProtectedPage. aspx. Como alternativa, los usuarios pueden visitar la página de inicio de sesión en su propio Volition. En ese caso, después de iniciar sesión en el usuario, deben enviarse a la página default. aspx de la carpeta raíz.

### <a name="logging-in-the-user"></a>Inicio de sesión del usuario

Suponiendo que las credenciales proporcionadas son válidas, es necesario crear un vale de autenticación de formularios, con lo que se inicia la sesión del usuario en el sitio. La [clase FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) del [espacio de nombres System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) proporciona métodos coordenados para iniciar y cerrar la sesión de los usuarios mediante el sistema de autenticación de formularios. Aunque hay varios métodos en la clase FormsAuthentication, los tres que nos interesan en este momento son:

- [GetAuthCookie (*nombre de usuario*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) : crea un vale de autenticación de formularios para el nombre de *usuario*proporcionado. A continuación, este método crea y devuelve un objeto HttpCookie que contiene el contenido del vale de autenticación. Si *persistCookie* es true, se crea una cookie persistente.
- [SetAuthCookie (*nombre de usuario*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) : llama al método GetAuthCookie (*username*, *persistCookie*) para generar la cookie de autenticación de formularios. A continuación, este método agrega la cookie devuelta por GetAuthCookie a la colección de cookies (suponiendo que se use la autenticación de formularios basada en cookies; de lo contrario, este método llama a una clase interna que controla la lógica del vale sin cookies).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) : este método llama a SetAuthCookie (*username*, *persistCookie*) y, a continuación, redirige al usuario a la página adecuada.

GetAuthCookie es útil cuando es necesario modificar el vale de autenticación antes de escribir la cookie en la colección de cookies. SetAuthCookie es útil si desea crear el vale de autenticación de formularios y agregarlo a la colección de cookies, pero no desea redirigir al usuario a la página correspondiente. Quizás desee mantenerlos en la página de inicio de sesión o enviarlos a alguna página alternativa.

Dado que deseamos iniciar sesión en el usuario y redirigirlo a la página adecuada, vamos a usar RedirectFromLoginPage. Actualice el controlador de eventos Click del LoginButton y reemplace las dos líneas de TODO comentadas por la siguiente línea de código:

FormsAuthentication. RedirectFromLoginPage (UserName. Text, RememberMe. Checked);

Al crear el vale de autenticación de formularios, usamos la propiedad text del cuadro de texto del nombre de usuario para el parámetro de *nombre de usuario* del vale de autenticación de formularios y el estado de activación de la casilla RememberMe para el parámetro *persistCookie* .

Para probar la página de inicio de sesión, visítela en un explorador. Empiece por escribir credenciales no válidas, como un nombre de usuario "no" y una contraseña "incorrecta". Al hacer clic en el botón de inicio de sesión, se producirá un postback y se mostrará la etiqueta InvalidCredentialsMessage.

[![se muestra la etiqueta InvalidCredentialsMessage al escribir credenciales no válidas](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Figura 10**: se muestra la etiqueta InvalidCredentialsMessage al escribir credenciales no válidas ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image26.png))

A continuación, escriba las credenciales válidas y haga clic en el botón Inicio de sesión. Esta vez, cuando se produce el postback, se crea un vale de autenticación de formularios y se le redirige automáticamente a default. aspx. En este punto, ha iniciado sesión en el sitio web, aunque no hay ninguna indicación visual para indicar que está conectado actualmente. En el paso 4, veremos cómo determinar mediante programación si un usuario ha iniciado sesión o no, así como cómo identificar al usuario que visita la página.

En el paso 5 se examinan técnicas para cerrar la sesión de un usuario del sitio Web.

### <a name="securing-the-login-page"></a>Protección de la página de inicio de sesión

Cuando el usuario escribe sus credenciales y envía el formulario de la página de inicio de sesión, las credenciales, incluida su contraseña, se transmiten por Internet al servidor Web en *texto sin formato*. Esto significa que cualquier pirata informático que examine el tráfico de red puede ver el nombre de usuario y la contraseña. Para evitar esto, es esencial cifrar el tráfico de red mediante el uso de [capas de sockets seguros (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Esto garantizará que las credenciales (así como el marcado HTML de la página completa) se cifren desde el momento en que salen del explorador hasta que las reciba el servidor Web.

A menos que el sitio web contenga información confidencial, solo tendrá que usar SSL en la página de inicio de sesión y en otras páginas en las que la contraseña del usuario se envíe de otro modo a través de la conexión en texto sin formato. No es necesario preocuparse por la protección del vale de autenticación de formularios, ya que, de forma predeterminada, está cifrado y firmado digitalmente (para evitar alteraciones). En el siguiente tutorial se muestra una explicación más detallada sobre la seguridad de vales de autenticación de formularios.

> [!NOTE]
> Muchos sitios web financieros y médicos están configurados para usar SSL en *todas* las páginas a las que pueden acceder los usuarios autenticados. Si va a compilar este tipo de sitio web, puede configurar el sistema de autenticación de formularios para que el vale de autenticación de formularios solo se transmita a través de una conexión segura. Veremos las distintas opciones de configuración de la autenticación de formularios en el tutorial siguiente, *[configuración de la autenticación de formularios y temas avanzados](forms-authentication-configuration-and-advanced-topics-cs.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Paso 4: detectar visitantes autenticados y determinar su identidad

En este momento se ha habilitado la autenticación de formularios y se ha creado una página de inicio de sesión rudimentaria, pero aún se ha examinado cómo se puede determinar si un usuario está autenticado o es anónimo. En algunos escenarios, es posible que desee Mostrar datos o información diferentes en función de si un usuario autenticado o anónimo está visitando la página. Además, a menudo es necesario conocer la identidad del usuario autenticado.

Vamos a aumentar la página default. aspx existente para ilustrar estas técnicas. En default. aspx, agregue dos controles de panel, uno denominado AuthenticatedMessagePanel y otro denominado AnonymousMessagePanel. Agregue un control etiqueta denominado WelcomeBackMessage en el primer panel. En el segundo panel, agregue un control de hipervínculo, establezca su propiedad texto en "iniciar sesión" y su propiedad NavigateUrl en "~/login.aspx". En este momento, el marcado declarativo de default. aspx debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Como es probable que ya haya adivinado ahora, la idea es mostrar solo el AuthenticatedMessagePanel a los visitantes autenticados y simplemente AnonymousMessagePanel a los visitantes anónimos. Para ello, es necesario establecer las propiedades visibles de estos paneles en función de si el usuario ha iniciado sesión o no.

La [propiedad request. IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) devuelve un valor booleano que indica si la solicitud se ha autenticado. Escriba el código siguiente en la página\_cargar el código del controlador de eventos:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Con este código en su lugar, visite default. aspx a través de un explorador. Suponiendo que todavía tiene que iniciar sesión, verá un vínculo a la página de inicio de sesión (consulte la figura 11). Haga clic en este vínculo e inicie sesión en el sitio. Como vimos en el paso 3, después de escribir sus credenciales, se le devolverá a default. aspx, pero esta vez la página mostrará el mensaje (consulte la figura 12).

![Al visitar de forma anónima, se muestra un vínculo de inicio de sesión.](an-overview-of-forms-authentication-cs/_static/image27.png)

**Figura 11**: al visitar de forma anónima, se muestra un vínculo de inicio de sesión

![A los usuarios autenticados se les muestra el](an-overview-of-forms-authentication-cs/_static/image28.png)

**Figura 12**: los usuarios autenticados se muestran en la página de bienvenida. Mensaje

Podemos determinar la identidad del usuario que ha iniciado sesión actualmente a través de la [propiedad de usuario](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx)del [objeto HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx). El objeto HttpContext representa información sobre la solicitud actual y es el inicio de estos objetos ASP.NET comunes como respuesta, solicitud y sesión, entre otros. La propiedad User representa el contexto de seguridad de la solicitud HTTP actual e implementa la [interfaz IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

La propiedad de usuario se establece mediante FormsAuthenticationModule. En concreto, cuando FormsAuthenticationModule encuentra un vale de autenticación de formularios en la solicitud entrante, crea un nuevo objeto GenericPrincipal y lo asigna a la propiedad de usuario.

Los objetos de entidad de seguridad (como GenericPrincipal) proporcionan información sobre la identidad del usuario y los roles a los que pertenecen. La interfaz IPrincipal define dos miembros:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) : un método que devuelve un valor booleano que indica si la entidad de seguridad pertenece al rol especificado.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) : una propiedad que devuelve un objeto que implementa la [interfaz IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). La interfaz IIdentity define tres propiedades: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)y [Name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Podemos determinar el nombre del visitante actual mediante el código siguiente:

String currentUsersName = User.Identity.Name;

Al utilizar la autenticación de formularios, se crea un [objeto FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) para la propiedad de identidad de GenericPrincipal. La clase FormsIdentity siempre devuelve la cadena "Forms" para su propiedad AuthenticationType y true para su propiedad IsAuthenticated. La propiedad Name devuelve el nombre de usuario especificado al crear el vale de autenticación de formularios. Además de estas tres propiedades, FormsIdentity incluye el acceso al vale de autenticación subyacente a través de su [propiedad de vale](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). La propiedad ticket devuelve un objeto de tipo [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), que tiene propiedades como expire, IsPersistent, IssueDate, Name, etc.

Lo más importante es que desecharlo aquí es que el parámetro de *nombre de usuario* especificado en los métodos FormsAuthentication. GetAuthCookie (*username*, *PersistCookie*), FormsAuthentication. SetAuthCookie (*username*, *persistCookie*) y FormsAuthentication. RedirectFromLoginPage (*username*, *persistCookie*) es el mismo valor devuelto por user.Identity.Name. Además, el vale de autenticación creado por estos métodos está disponible mediante la conversión de User. Identity a un objeto FormsIdentity y, a continuación, el acceso a la propiedad ticket:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Vamos a proporcionar un mensaje más personalizado en default. aspx. Actualice la página\_controlador de eventos de carga de modo que se asigne a la propiedad de texto de la etiqueta WelcomeBackMessage la cadena "Welcome Back, *username*!"

WelcomeBackMessage. Text = "Welcome Back," + User.Identity.Name + "!";

En la figura 13 se muestra el efecto de esta modificación (al iniciar sesión como usuario Scott).

![El mensaje de bienvenida incluye el nombre del usuario que ha iniciado sesión actualmente](an-overview-of-forms-authentication-cs/_static/image29.png)

**Figura 13**: el mensaje de bienvenida incluye el nombre del usuario que ha iniciado sesión actualmente

### <a name="using-the-loginview-and-loginname-controls"></a>Usar los controles LoginView y LoginName

Mostrar contenido diferente a usuarios autenticados y anónimos es un requisito común; por lo tanto, se muestra el nombre del usuario que ha iniciado sesión actualmente. Por ese motivo, ASP.NET incluye dos controles Web que proporcionan la misma funcionalidad que se muestra en la figura 13, pero sin necesidad de escribir una sola línea de código.

El [control LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) es un control Web basado en una plantilla que facilita la visualización de datos diferentes para usuarios autenticados y anónimos. LoginView incluye dos plantillas predefinidas:

- AnonymousTemplate: cualquier marcado agregado a esta plantilla solo se muestra a los visitantes anónimos.
- LoggedInTemplate: el marcado de esta plantilla solo se muestra a los usuarios autenticados.

Vamos a agregar el control LoginView a la página maestra del sitio, site. Master. Sin embargo, en lugar de agregar únicamente el control LoginView, vamos a agregar un nuevo control ContentPlaceHolder y, a continuación, colocar el control LoginView dentro de ese nuevo ContentPlaceHolder. La justificación de esta decisión se hará patente en breve.

> [!NOTE]
> Además de las funciones AnonymousTemplate y LoggedInTemplate, el control LoginView puede incluir plantillas específicas del rol. Las plantillas específicas de roles muestran el marcado solo a los usuarios que pertenecen a un rol especificado. Examinaremos las características basadas en roles del control LoginView en un tutorial futuro.

Empiece agregando un ContentPlaceHolder denominado LoginContent en la página maestra dentro del elemento de navegación &lt;div&gt;. Simplemente puede arrastrar un control ContentPlaceHolder desde el cuadro de herramientas a la vista de código fuente, colocando el marcado resultante justo encima del menú "TODO: se irá aquí...". negrita.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Después, agregue un control LoginView dentro de LoginContent ContentPlaceHolder. El contenido colocado en los controles ContentPlaceHolder de la página maestra se considera *contenido predeterminado* para el control ContentPlaceHolder. Es decir, las páginas de ASP.NET que usan esta página maestra pueden especificar su propio contenido para cada ContentPlaceHolder o usar el contenido predeterminado de la página maestra.

LoginView y otros controles relacionados con el inicio de sesión se encuentran en la pestaña Inicio de sesión del cuadro de herramientas.

![Control LoginView en el cuadro de herramientas](an-overview-of-forms-authentication-cs/_static/image30.png)

**Figura 14**: el control LoginView en el cuadro de herramientas

A continuación, agregue dos &lt;elementos br/&gt; inmediatamente después del control LoginView, pero aún dentro de ContentPlaceHolder. Llegados a este punto, el marcado del elemento de&gt; div de &lt;de navegación debe ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Las plantillas de LoginView se pueden definir desde el diseñador o el marcado declarativo. En el diseñador de Visual Studio, expanda la etiqueta inteligente de LoginView, que enumera las plantillas configuradas en una lista desplegable. Escriba el texto "Hello, extraño" en AnonymousTemplate; después, agregue un control de hipervínculo y establezca sus propiedades Text y NavigateUrl en "iniciar sesión" y "~/login.aspx", respectivamente.

Después de configurar la versión AnonymousTemplate, cambie a LoggedInTemplate y escriba el texto "Bienvenido atrás". A continuación, arrastre un control LoginName desde el cuadro de herramientas hasta el LoggedInTemplate, colocándolo inmediatamente después del texto "Bienvenido de nuevo". El [control LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), como su nombre implica, muestra el nombre del usuario que ha iniciado sesión actualmente. Internamente, el control LoginName simplemente genera la propiedad User.Identity.Name

Después de hacer estas adiciones a las plantillas de LoginView, el marcado debería ser similar al siguiente:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Con esta adición a la página maestra site. Master, cada página del sitio web mostrará un mensaje diferente en función de si el usuario está autenticado. En la figura 15 se muestra la página default. aspx cuando el usuario visita un explorador Jisun. El mensaje "Welcome up, Jisun" se repite dos veces: una vez en la sección de navegación de la página maestra de la izquierda (a través del control LoginView recién agregado) y una vez en el área de contenido default. aspx (a través de los controles de panel y la lógica de programación).

![El control LoginView muestra](an-overview-of-forms-authentication-cs/_static/image31.png)

**Figura 15**: el control LoginView muestra "Welcome Back, Jisun".

Dado que se ha agregado LoginView a la página maestra, puede aparecer en todas las páginas del sitio. Sin embargo, puede haber páginas web en las que no quiera mostrar este mensaje. Una de estas páginas es la página de inicio de sesión, ya que un vínculo a la página de inicio de sesión parece estar fuera de la ubicación. Como colocamos el control LoginView en un ContentPlaceHolder en la página maestra, podemos invalidar este marcado predeterminado en nuestra página de contenido. Abra login. aspx y vaya al diseñador. Dado que no hemos definido explícitamente un control de contenido en login. aspx para el LoginContent ContentPlaceHolder en la página maestra, la página de inicio de sesión mostrará el marcado predeterminado de la página maestra para este ContentPlaceHolder. Puede verlo a través del diseñador: el LoginContent ContentPlaceHolder muestra el marcado predeterminado (el control LoginView).

[![la página de inicio de sesión muestra el contenido predeterminado para el LoginContent ContentPlaceHolder de la página maestra](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Figura 16**: la página de inicio de sesión muestra el contenido predeterminado para el LoginContent ContentPlaceHolder de la página maestra ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image34.png))

Para invalidar el marcado predeterminado para el LoginContent ContentPlaceHolder, simplemente haga clic con el botón derecho en la región en el diseñador y elija la opción crear contenido personalizado en el menú contextual. (Cuando se usa Visual Studio 2008, ContentPlaceHolder incluye una etiqueta inteligente que, cuando se selecciona, ofrece la misma opción). Esto agrega un nuevo control de contenido al marcado de la página y, por tanto, nos permite definir el contenido personalizado para esta página. Puede Agregar un mensaje personalizado aquí, como "iniciar sesión...", pero deje este campo en blanco.

> [!NOTE]
> En Visual Studio 2005, la creación de contenido personalizado crea un control de contenido vacío en la página ASP.NET. Sin embargo, en Visual Studio 2008, la creación de contenido personalizado copia el contenido predeterminado de la página maestra en el control de contenido que se acaba de crear. Si usa Visual Studio 2008, después de crear el nuevo control de contenido, asegúrese de borrar el contenido copiado de la página maestra.

En la figura 17 se muestra la página login. aspx cuando se visita desde un explorador después de efectuar este cambio. Tenga en cuenta que no hay ningún mensaje "Hola, extraño" o "Bienvenido atrás, *nombre de usuario*" en el panel de navegación izquierdo &lt;div&gt; como cuando se visita default. aspx.

[![la página de inicio de sesión oculta el marcado predeterminado de LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Figura 17**: la página de inicio de sesión oculta el marcado de LoginContent de ContentPlaceHolder predeterminado ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image37.png))

## <a name="step-5-logging-out"></a>Paso 5: cerrar la sesión

En el paso 3, hemos examinado la creación de una página de inicio de sesión para iniciar una sesión de usuario en el sitio, pero todavía hemos visto cómo cerrar la sesión de un usuario. Además de los métodos para registrar un usuario en, la clase FormsAuthentication también proporciona un [método SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). El método SignOut simplemente destruye el vale de autenticación de formularios, lo que registra al usuario fuera del sitio.

La oferta de un vínculo de cierre de sesión es una característica común que ASP.NET incluye un control diseñado específicamente para cerrar la sesión de un usuario. El [control LoginStatus](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) muestra un LinkButton "login" o un LinkButton "Logout", según el estado de autenticación del usuario. Un LinkButton de "Inicio de sesión" se representa para los usuarios anónimos, mientras que un LinkButton "Logout" se muestra a los usuarios autenticados. El texto de los LinkButtons de inicio y cierre de sesión se puede configurar mediante las propiedades LoginText y LogoutText de LoginStatus.

Al hacer clic en el LinkButton de inicio de sesión, se produce un postback, desde el que se emite una redirección a la página de inicio de sesión. Al hacer clic en el control LinkButton "Logout", el control LoginStatus invoca el método FormsAuthentication. acfirme y, a continuación, redirige al usuario a una página. La página a la que se redirige el usuario con la sesión iniciada depende de la propiedad LogoutAction, que se puede asignar a uno de los tres valores siguientes:

- Actualizar: el valor predeterminado; redirige al usuario a la página que acaba de visitar. Si la página que acaba de visitar no permite usuarios anónimos, FormsAuthenticationModule redirigirá automáticamente al usuario a la página de inicio de sesión.

Puede tener curiosidad sobre por qué se realiza aquí una redirección. Si el usuario desea permanecer en la misma página, ¿por qué es necesario el redireccionamiento explícito? La razón es que, cuando se hace clic en el LinkButton "Logoff", el usuario sigue teniendo el vale de autenticación de formularios en la colección de cookies. Por consiguiente, la solicitud de postback es una solicitud autenticada. El control LoginStatus llama al método SignOut, pero esto sucede después de que FormsAuthenticationModule haya autenticado al usuario. Por lo tanto, una redirección explícita hace que el explorador vuelva a solicitar la página. En el momento en que el explorador vuelve a solicitar la página, se ha quitado el vale de autenticación de formularios y, por tanto, la solicitud entrante es anónima.

- Redirect: el usuario se redirige a la dirección URL especificada por la propiedad LogoutPageUrl de LoginStatus.
- RedirectToLoginPage: se redirige al usuario a la página de inicio de sesión.

Vamos a agregar un control LoginStatus a la página maestra y configurarlo para que use la opción de redireccionamiento para enviar al usuario a una página que muestre un mensaje que confirma que se ha cerrado la sesión. Empiece por crear una página en el directorio raíz denominado logout. aspx. No olvide asociar esta página con la página maestra site. Master. A continuación, escriba un mensaje en el marcado de la página explicando al usuario que se ha cerrado la sesión.

A continuación, vuelva a la página maestra site. Master y agregue un control LoginStatus bajo el LoginView en el LoginContent ContentPlaceHolder. Establezca la propiedad LogoutAction del control LoginStatus en redirect y su propiedad LogoutPageUrl en "~/Logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Dado que LoginStatus está fuera del control LoginView, aparecerá para los usuarios anónimos y autenticados, pero eso es correcto porque el LoginStatus mostrará correctamente un control LinkButton de inicio de sesión o de cierre de sesión. Con la adición del control LoginStatus, el hipervínculo "iniciar sesión" en la AnonymousTemplate es superfluo, por lo que debe quitarlo.

La figura 18 muestra default. aspx cuando Jisun visita. Tenga en cuenta que la columna de la izquierda muestra el mensaje "Welcome up, Jisun" junto con un vínculo para cerrar la sesión. Al hacer clic en el LinkButton de cierre de sesión, se produce un postback, se firma Jisun fuera del sistema y, a continuación, se redirige a logout. aspx. Como se muestra en la figura 19, en el momento en que Jisun alcanza logout. aspx, ya se ha cerrado la sesión y, por tanto, es anónimo. Por lo tanto, la columna de la izquierda muestra el texto "Welcome, extraño" y un vínculo a la página de inicio de sesión.

[![default. aspx muestra](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Figura 18**: default. aspx muestra "Welcome up, Jisun" junto con un LinkButton "Logout" ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image40.png))

[![logout. aspx muestra](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Figura 19**: logout. aspx muestra "Welcome, extraño" junto con un LinkButton de "login" ([haga clic para ver la imagen de tamaño completo](an-overview-of-forms-authentication-cs/_static/image43.png))

> [!NOTE]
> Recomiendo personalizar la página logout. aspx para ocultar el LoginContent ContentPlaceHolder de la página maestra (como hicimos con login. aspx en el paso 4). La razón es que el LinkButton de "Inicio de sesión" representado por el control LoginStatus (el que se encuentra debajo de "Hello, extraño") envía al usuario a la página de inicio de sesión pasando la dirección URL actual en el parámetro QueryString de ReturnUrl. En Resumen, si un usuario que ha cerrado la sesión hace clic en este "Inicio de sesión" de LoginStatus y, a continuación, inicia sesión, se le redirigirá de nuevo a logout. aspx, lo que podría confundir fácilmente al usuario.

## <a name="summary"></a>Resumen

En este tutorial, comenzamos con un examen del flujo de trabajo de autenticación de formularios y, a continuación, se activaba la implementación de la autenticación de formularios en una aplicación ASP.NET. La autenticación mediante formularios se basa en FormsAuthenticationModule, que tiene dos responsabilidades: identificar a los usuarios en función de su vale de autenticación de formularios y redirigir a los usuarios no autorizados a la página de inicio de sesión.

La clase FormsAuthentication del .NET Framework incluye métodos para crear, inspeccionar y quitar vales de autenticación de formularios. La propiedad request. IsAuthenticated y el objeto de usuario proporcionan compatibilidad adicional mediante programación para determinar si una solicitud está autenticada e información sobre la identidad del usuario. También hay controles Web LoginView, LoginStatus y LoginName, que proporcionan a los desarrolladores una manera rápida y sin código para realizar muchas tareas comunes relacionadas con el inicio de sesión. Examinaremos estos y otros controles Web relacionados con el inicio de sesión con más detalle en futuros tutoriales.

En este tutorial se ha proporcionado información general sobre la autenticación de formularios. No se examinaron las opciones de configuración ordenadas, se examina cómo funcionan los vales de autenticación de formularios sin cookies o se explora cómo ASP.NET protege el contenido del vale de autenticación de formularios. Hablaremos de estos temas, entre otros, en el [siguiente tutorial](forms-authentication-configuration-and-advanced-topics-cs.md).

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Cambios entre la seguridad de IIS6 y IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Controles de inicio de sesión ASP.NET](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Seguridad, pertenencia y administración de roles de Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [El elemento `<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx)
- [El elemento `<forms>` de `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vídeo de aprendizaje sobre temas incluidos en este tutorial

- [Usar la autenticación de formularios básicos en ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue revisado por muchos revisores útiles. Los revisores responsables de este tutorial incluyen Alicja Maziarz, John Suru y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](security-basics-and-asp-net-support-cs.md)
> [Siguiente](forms-authentication-configuration-and-advanced-topics-cs.md)
