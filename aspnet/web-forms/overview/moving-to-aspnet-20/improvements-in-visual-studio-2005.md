---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Mejoras en Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 proporciona a los desarrolladores de aplicaciones web una larga lista de mejoras y mejoras en los proyectos Web.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 64215d556ded0850537a13856fe69b094116ebca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464653"
---
# <a name="improvements-in-visual-studio-2005"></a>Mejoras en Visual Studio 2005

por [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 proporciona a los desarrolladores de aplicaciones web una larga lista de mejoras y mejoras en los proyectos Web.

Visual Studio 2005 proporciona a los desarrolladores de aplicaciones web una larga lista de mejoras y mejoras en los proyectos Web. Tan eficazmente como Visual Studio .NET 2002 y 2003 son muchas quejas en la forma en que se trataban los proyectos Web. Visual Studio 2005 agrega un número significativo de características nuevas para abordar estas quejas. Para aquellos que prefieren la manera en que Visual Studio .NET 2003 controla la compilación de aplicaciones Web, vea [proyectos de aplicación web](https://go.microsoft.com/fwlink/?LinkId=57870).

En este módulo, se tratarán mejor las mejoras en la creación, administración y desarrollo de proyectos Web. En un módulo posterior, se tratarán mejor las mejoras en la creación de proyectos web y su implementación.

## <a name="frontpage-server-extensions"></a>Extensiones de servidor de FrontPage

Visual Studio .NET 2002 y 2003 son necesarios Extensiones de servidor de FrontPage en el cuadro para crear o compilar proyectos Web. Los desarrolladores pueden elegir entre dos modos de acceso diferentes (Extensiones de servidor de FrontPage o el modo de acceso a archivos), ambos se usan Extensiones de servidor de FrontPage para realizar tareas como establecer la raíz de la aplicación en IIS, etc.

Visual Studio 2005 quita la dependencia de Extensiones de servidor de FrontPage para los proyectos locales. Visual Studio 2005 ahora accede a la metabase de IIS directamente en lugar de usar el Extensiones de servidor de FrontPage. Visual Studio 2005 también agrega compatibilidad con FTP que permite el acceso a proyectos remotos sin necesidad de Extensiones de servidor de FrontPage.

Para aquellos desarrolladores que deseen usar Extensiones de servidor de FrontPage en sus proyectos, la opción sigue estando disponible. Sin embargo, en función de los comentarios sólidos de la comunidad de desarrolladores de ASP.NET, no es un requisito.

> [!NOTE]
> Los Extensiones de servidor de FrontPage siguen siendo necesarios para la creación remota de proyectos, la apertura, etc.

## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Visual Studio 2005 se suministra con un nuevo servidor Web denominado Servidor de desarrollo de ASP.NET. (Este servidor Web se conocía anteriormente como Cassini).

Hay varias ventajas de la Servidor de desarrollo de ASP.NET.

- Ahora es posible que los usuarios que no son administradores puedan desarrollar y depurar en un servidor Web.
- El Servidor de desarrollo de ASP.NET asigna dinámicamente directorios virtuales a cualquier ubicación del sistema de archivos, lo que permite ubicaciones de proyecto flexibles.
- Los usuarios de Windows XP Professional que ya utilizan IIS podrán crear nuevas aplicaciones web que no afectarán a la estructura de archivos o carpetas de su sitio web predeterminado en IIS.

No se requiere ninguna configuración especial para aprovechar el Servidor de desarrollo de ASP.NET. Cuando se depura o se examina un proyecto web hospedado en el sistema de archivos, Visual Studio 2005 iniciará automáticamente una instancia del Servidor de desarrollo de ASP.NET en un puerto aleatorio para atender la solicitud.

En el Servidor de desarrollo de ASP.NET más adelante en este módulo se tratará más información.

## <a name="improved-file-management"></a>Administración de archivos mejorada

En Visual Studio 2002 y 2003, un archivo de proyecto (. vbproj para VB.NET y. csproj C#para) almacena información en todos los archivos de la aplicación Web. La visualización de Explorador de soluciones se basa en la información de archivo del archivo de proyecto. Por este motivo, el Explorador de soluciones a menudo mostraría información inexacta en los casos en los que se usaban editores externos. Visual Studio 2002 y 2003 con frecuencia sobrescribirán los cambios de archivo o no mostrarán la versión más reciente de los archivos.

Visual Studio 2005 se aparta del archivo de proyecto. En su lugar, lee la información de archivos y carpetas directamente desde el disco, lo que da lugar a una presentación precisa de los archivos del proyecto. Dado que la carpeta referencias de Visual Studio 2002 y 2003 no representa una carpeta real de la aplicación Web, Visual Studio 2005 también quita la carpeta referencias de Explorador de soluciones. Para tener acceso a las referencias del proyecto en Visual Studio 2005, debe usar las páginas de propiedades del proyecto.

## <a name="creating-web-projects"></a>Crear proyectos Web

Los desarrolladores web tienen muchas opciones nuevas disponibles para la creación de proyectos en Visual Studio 2005. Ahora, los sitios web se pueden crear en cualquier parte del sistema de archivos y, a continuación, se pueden depurar o examinar mediante el nuevo Servidor de desarrollo de ASP.NET. Los desarrolladores también pueden crear sitios web nuevos mediante FTP.

Haga clic aquí para ver un tutorial en vídeo sobre la creación de proyectos web en Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image1.png)

[Abrir vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)

### <a name="file-system-projects"></a>Proyectos del sistema de archivos

Como vio en el tutorial en vídeo, puede elegir crear sitios web en el sistema de archivos en el equipo local o en una ubicación remota a través de un recurso compartido de archivos. Los sitios web que se crean en el sistema de archivos se examinan y depuran mediante el Servidor de desarrollo de ASP.NET.

> [!NOTE]
> El Servidor de desarrollo de ASP.NET puede causar confusión a los clientes. Si se crea un proyecto web en el sistema de archivos en la estructura de directorios de IISs (es decir, c:/inetpub/wwwroot), el sitio web se seguirá explorando a través del Servidor de desarrollo de ASP.NET cuando se inicie desde dentro de Visual Studio 2005. Por lo tanto, cualquier configuración de IIS (es decir, los métodos de autenticación) no es aplicable.

El proyecto Web predeterminado también quita gran parte de la sobrecarga de solo incluye una página default. aspx, un archivo default.cs y una carpeta de aplicación/_Data. El archivo Web. config y las carpetas especiales (es decir, App/_code) se agregan a medida que se necesitan. El proyecto web solo incluye los archivos y las carpetas que necesite.

### <a name="http-projects"></a>Proyectos HTTP

Los proyectos HTTP pueden ser proyectos que se crean en un sitio web de IIS local o en un sitio Web remoto. La ubicación predeterminada del proyecto es `http://localhost`. Si hace clic en el botón examinar, hay dos opciones HTTP: local IIS y sitio remoto. La diferencia principal entre estas dos opciones es el método en el que se muestra la información del sitio web en el cuadro de diálogo Elegir ubicación y en cómo se copian los archivos en el servidor Web.

La opción IIS local lee la información del sitio de la metabase en el equipo local y los archivos se copian con el sistema de archivos. La opción sitio remoto utiliza el Extensiones de servidor de FrontPage y la información y los archivos del sitio se copian mediante HTTP y Extensiones de servidor de FrontPage llamadas RPC.

> [!NOTE]
> El archivo vs # # #/_tmp. htm y Get/_aspx/_ver. aspx ya no se usan para determinar la información de versión.

La opción predeterminada de HTTP es IIS local. Esta opción lee la metabase de IIS para determinar qué sitios están disponibles y la ubicación en la que se va a crear el contenido. Puede seleccionar una carpeta o un directorio virtual diferente seleccionándolos en la vista de árbol. También puede crear un nuevo directorio virtual, marcar carpetas como aplicaciones, así como eliminar directorios virtuales existentes desde este cuadro de diálogo.

![Cuadro de diálogo Elegir ubicación](improvements-in-visual-studio-2005/_static/image1.gif)

**Figura 1**: cuadro de diálogo Elegir ubicación

A diferencia de las versiones anteriores de Visual Studio, si activa la casilla **usar capa de sockets seguros** y el certificado SSL no coincide con la dirección URL que está explorando, aparecerá un cuadro de diálogo de alerta de seguridad en el que se le preguntará si desea continuar. Con Visual Studio .NET 2003, si el certificado no era una coincidencia, se produciría un error en la creación del proyecto.

![Alerta de seguridad sobre el certificado SSL](improvements-in-visual-studio-2005/_static/image2.gif)

**Figura 2**: alerta de seguridad sobre el certificado SSL

### <a name="note-on-host-headers"></a>Nota sobre encabezados de host

Si va a crear una aplicación web en un sitio enlazado a una dirección IP específica, deberá asegurarse de que se ha configurado un encabezado de host. De lo contrario, Visual Studio creará el sitio en `http://localhost`, pero la dirección IP no se resolverá correctamente cuando el sitio se examine o se depure desde dentro del IDE.

Si selecciona la opción sitio remoto, el cuadro de diálogo cambia para permitirle especificar la dirección URL de destino para el nuevo sitio Web. Esta dirección URL debe encontrarse en un servidor que tenga el Extensiones de servidor de FrontPage habilitado. Si desea trabajar con el servidor Web local mediante el Extensiones de servidor de FrontPage, puede usar la opción sitio remoto y especificar una dirección URL local.

![Crear un sitio web en un servidor remoto](improvements-in-visual-studio-2005/_static/image1.jpg)

**Figura 3**: creación de un sitio web en un servidor remoto

Al crear una aplicación en un sitio remoto a través de SSL, si el certificado SSL no coincide, el cuadro de diálogo de confirmación es ligeramente diferente del cuadro de diálogo que se muestra cuando se usa la opción IIS local.

![Alerta de seguridad del sitio remoto](improvements-in-visual-studio-2005/_static/image3.gif)

**Figura 4**: alerta de seguridad del sitio remoto

<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 presenta la opción para crear sitios web a través de FTP. Cuando se usa esta opción, el IDE crea los archivos localmente en la carpeta Temp de los usuarios y, a continuación, usa FTP para trasladar los archivos a la ubicación FTP.

> [!NOTE]
> La ubicación de la carpeta Temp es c:/Documents and Settings/&lt;usuario&gt;/local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nombre de aplicación&gt;

Al usar la opción FTP, aparecerá un cuadro de diálogo Elegir ubicación. Escriba la información de conexión de FTP necesaria en este cuadro de diálogo, como se muestra a continuación.

![Cuadro de diálogo Elegir ubicación para FTP](improvements-in-visual-studio-2005/_static/image2.jpg)

**Figura 5**: cuadro de diálogo Elegir ubicación para FTP

## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratorio: instalación del sitio FTP y creación de un proyecto

Los pasos siguientes configuran el sitio FTP para que un usuario tenga una ubicación que solo pueda cargar a través de FTP.

### <a name="install-the-ftp-service"></a>Instalar el servicio FTP

1. Abra agregar o quitar programas y seleccione Agregar o quitar componentes de Windows.
2. Seleccione Internet Information Services (servidor de aplicaciones en Windows 2003) y haga clic en **detalles**.
3. Compruebe **File Transfer Protocol servicio (FTP)** y haga clic en **Aceptar**.
4. Haga clic en **siguiente** para instalar el servicio FTP.

### <a name="create-a-new-folder-for-content"></a>Crear una nueva carpeta para el contenido

1. En el explorador de Windows, cree una nueva carpeta denominada **user1** dentro de c:/inetpub/wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Configurar carpetas y permisos en las carpetas.

1. Abra el complemento Internet Information Services desde herramientas administrativas. Ahora tendrá una carpeta de sitios FTP en el nodo nombre de equipo.
2. Expanda **sitios FTP**.
3. Haga clic con el botón secundario en el **sitio FTP predeterminado**, seleccione **nuevo**, **directorio virtual**y, a continuación, haga clic en **siguiente**.
4. Escriba **user1** como nombre del directorio virtual y haga clic en **siguiente**.
5. Escriba **c:/inetpub/wwwroot/user1** para la ruta de acceso y haga clic en **siguiente**.
6. Haga clic en **siguiente** y en **Finalizar** para completar el asistente.
7. Haga clic con el botón secundario en el directorio virtual **user1** en sitio FTP predeterminado y seleccione **propiedades**.
8. Active la casilla **escribir** y haga clic en **Aceptar** para cerrar el cuadro de diálogo.
9. Haga clic con el botón secundario en **sitio FTP predeterminado** y seleccione **propiedades**.
10. En la pestaña **cuentas de seguridad** , desactive **permitir conexiones anónimas**.
11. Haga clic en **sí** en el cuadro de diálogo que le pregunta si desea continuar.
12. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
13. Expanda el **sitio web predeterminado** en el nodo **sitios web** .
14. Haga clic con el botón secundario en el directorio **user1** y seleccione **propiedades** .
15. En la sección configuración de la **aplicación** , haga clic en **crear** para marcar la carpeta como una aplicación.
16. Haga clic en **Aceptar** para cerrar el cuadro de diálogo.
17. Cierre el complemento Internet Information Services.

### <a name="create-web-project"></a>Crear proyecto web

1. Abra Visual Studio 2005.
2. En el menú **archivo** , seleccione **nuevo sitio web**.
3. En el menú desplegable **Ubicación** , seleccione **FTP**.
4. Haga clic en **Examinar**.
5. Escriba **localhost** en el cuadro de texto **servidor** .
6. Escriba **user1** en el cuadro de texto directorio.
7. Haga clic en **Abrir**. La ubicación FTP se escribirá en el cuadro de diálogo nuevo sitio Web.
8. Haga clic en **Aceptar**.
9. Desactive el **Inicio de sesión anónimo** en el cuadro de diálogo Inicio de sesión FTP, escriba sus credenciales y haga clic en **Aceptar**.
10. ¿Cuál es la dirección URL del proyecto? (La dirección URL del proyecto se mostrará en Explorador de soluciones).
11. En el menú **compilar** , seleccione **compilar sitio web** o **compilar solución**.
12. Haga clic con el botón derecho en default. aspx en Explorador de soluciones y seleccione **ver en el explorador**.
13. En el cuadro de diálogo se requiere la dirección URL del sitio web, escriba `http://localhost/user1` de la dirección URL y haga clic en **Aceptar**.

> [!NOTE]
> Si recibe un error que indica una imposibilidad de cargar el tipo/_Default, asegúrese de que está ejecutando ASP.NET 2,0 en el sitio web y no en una versión anterior. Puede hacerlo desde la pestaña ASP.NET de Internet Information Services.

## <a name="opening-web-projects"></a>Abrir proyectos Web

La apertura de proyectos web es similar a la creación de proyectos. En las secciones siguientes se indican áreas que se van a supervisar mientras se trabaja en el IDE. También se explica cómo trabajar con proyectos Web mediante HTTP y FTP.

Para abrir un proyecto Web, seleccione abrir sitio web en el menú archivo. Se le solicitará el mismo cuadro de diálogo Elegir ubicación que se ha descrito anteriormente y tendrá las mismas cuatro opciones disponibles: sistema de archivos, local IIS, FTP y sitio remoto.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Sistema de archivos

Como se indicó anteriormente en este módulo, Visual Studio ya no usa un archivo de proyecto. Por lo tanto, si decide abrir un sitio web desde el sistema de archivos, tiene la opción de elegir cualquier carpeta que desee, aunque la carpeta que elija no se haya creado como proyecto web inicialmente en Visual Studio. Por ejemplo, puede optar por abrir la carpeta Mis documentos como un sitio web y Visual Studio se abrirá con suerte y mostrará los archivos como se muestra a continuación.

![Mis documentos abiertos como un sitio web](improvements-in-visual-studio-2005/_static/image3.jpg)

**Figura 6**: *Mis documentos* abiertos como un sitio web

Dado que Visual Studio solo crea archivos y carpetas adicionales cuando sea necesario, no se agregan archivos ni carpetas adicionales a la ubicación que se abre. Un efecto secundario de esta arquitectura es que impide anidar sitios web en el sistema de archivos. Por ejemplo, considere la siguiente estructura de directorios.

Proyecto web en C:/mi sitio web

Otro proyecto web en C:/mi sitio web/anidado

Al abrir el sitio web en c:/mi sitio web, la carpeta anidada aparecerá como una subcarpeta de esa aplicación.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Al abrir sitios web a través de HTTP, la configuración se lee desde la metabase de IIS (IIS local) o mediante Extensiones de servidor de FrontPage (sitio remoto). Si hay aplicaciones web anidadas, estas se muestran junto con un icono que las identifica como una aplicación. Si está familiarizado con el trabajo con aplicaciones web en FrontPage, el comportamiento de Visual Studio 2005 es similar.

Aunque Visual Studio mostrará un icono para las aplicaciones que están anidadas debajo de la aplicación que está abierta actualmente en el IDE, no le permitirá expandirlas para ver su contenido. Sin embargo, puede hacer doble clic en ellos para abrirlos. Cuando lo haga, aparecerá un cuadro de diálogo que le pide que abra la aplicación web (y reemplace la solución abierta actualmente) o que agregue la aplicación web a la solución actual.

![Al hacer doble clic en un icono de aplicación anidada, se muestra este cuadro de diálogo](improvements-in-visual-studio-2005/_static/image4.jpg)

**Figura 7**: hacer doble clic en un icono de aplicación anidada presenta este cuadro de diálogo

<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>Sitio FTP

Al abrir un sitio a través de FTP, todos los archivos se copian localmente en la carpeta Temp. La ruta de acceso completa de la ubicación de almacenamiento local se muestra en el panel de propiedades del proyecto y se crea con el siguiente formato.

C:/Documents and Settings/&lt;User&gt;/local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;nombre de aplicación&gt;

Al usar FTP, Visual Studio deberá especificar la dirección URL base del proyecto para que pueda examinarlo como se muestra a continuación. Si no especifica una dirección URL base, Visual Studio le pedirá la primera vez que intente examinar una página en el sitio Web.

![Especificar una dirección URL base para sitios FTP](improvements-in-visual-studio-2005/_static/image5.jpg)

**Figura 8**: especificación de una dirección URL base para sitios FTP

## <a name="improvements-in-compilation"></a>Mejoras en la compilación

Trabajar con aplicaciones web en Visual Studio 2005 es notablemente más rápido que las versiones anteriores. Esto se debe a que no hay una pequeña parte de los cambios en la arquitectura de compilación.

En Visual Studio 2002 y 2003, las aplicaciones web se compilaron en un ensamblado principal que reside en la carpeta/bin. En Visual Studio 2005, se ha agregado una carpeta app/_Code. Las clases y otros códigos que no son de interfaz de usuario se agregan a la carpeta app/_Code. Cuando Visual Studio compila el proyecto, todos los archivos de la carpeta app/_Code se compilan en un único archivo app/_Code. dll. El resultado de este cambio es que las compilaciones posteriores son mucho más rápidas que en las versiones anteriores.

> [!NOTE]
> La utilidad de línea de comandos de MSBuild también se puede utilizar para compilar aplicaciones Web de ASP.NET. Esa herramienta se tratará en el módulo 9.

Otra mejora de la compilación es la opción nueva página compilar del menú compilar. Esta característica permite a los desarrolladores recompilar solo la página actual (junto con, por supuesto, y dependencias) para que los cambios se puedan compilar más rápidamente. Dado C# que no ofrece compilación en segundo plano para la actualización de IntelliSense, etc., se beneficiará enormemente de esta característica, ya que permitirá que IntelliSense se actualice rápidamente con solo volver a generar una sola página.

Las propiedades de compilación de un proyecto permiten configurar el tipo de compilación que se produce antes de que se ejecute la página de inicio. Los desarrolladores pueden elegir compilar solo la página actual para que Visual Studio pueda iniciar la depuración de aplicaciones más rápidamente después de que cambie el código.

![Acción de inicio de la página de compilación](improvements-in-visual-studio-2005/_static/image6.jpg)

**Figura 9**: acción de inicio de la página de compilación

Otra mejora importante para Visual Studio y la arquitectura de ASP.NET se encuentra en el área de editar y continuar. En Visual Studio 2005, los desarrolladores pueden comenzar a depurar un proyecto y realizar cambios en el código en el proyecto sin desasociar el depurador. De hecho, puede empezar a depurar un proyecto, agregar una nueva clase, agregar código a esa clase, agregar código a la página que cree una nueva instancia de esa clase y ejecutar un método de la clase, todo ello sin desasociar el depurador. Ejecutar el nuevo código es literalmente tan sencillo como actualizar el explorador.

Haga clic aquí para ver un tutorial en vídeo de la característica editar y continuar en Visual Studio 2005.

![](improvements-in-visual-studio-2005/_static/image2.png)

[Abrir vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)

La sólida funcionalidad de edición y continuación de ASP.NET 2,0 y Visual Studio 2005 se debe a un cambio arquitectónico en las aplicaciones de ASP.NET. En ASP.NET 1. x, las aplicaciones creadas en Visual Studio 2002/2003 se compilaron en un ensamblado principal que estaba almacenado en la carpeta/bin. Todas las clases, páginas, etc. de la aplicación se compilaron en esa DLL. Después, en tiempo de ejecución, ASP.NET compilaría todos los controles, el marcado y el código ASP.NET dentro de las páginas y los copiaría en la carpeta temporal ASP.NET.

En Visual Studio 2005 con ASP.NET 2,0, los dos modelos de compilación que se describen anteriormente (uno para Visual Studio y otro para ASP.NET en tiempo de ejecución) se han combinado en un modelo de compilación común. Esto significa que ahora todos los problemas de compilación se detectan durante la fase de desarrollo en lugar de en tiempo de ejecución. También permite el diseñador y la compatibilidad con IntelliSense para características como controles de usuario y páginas maestras.

Haga clic aquí para ver un tutorial en vídeo sobre la compatibilidad del diseñador con los controles de usuario.

![](improvements-in-visual-studio-2005/_static/image3.png)

[Abrir vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)

> [!NOTE]
> Cuando se quita un control de usuario de una página, la Directiva de @Register permanece en el marcado y debe quitarse manualmente para evitar errores de analizador si el control de usuario se elimina del sitio Web.

Otra mejora en el modelo de compilación de Visual Studio es la característica publicar sitio Web. Dado que la característica de publicación precompila el sitio web, los desarrolladores pueden disfrutar del mayor rendimiento que no tener que compilar nada a petición. También precompila todo el código fuente de la carpeta app/_Code en un archivo DLL para que no se tenga que implementar ningún código fuente.

![Cuadro de diálogo Publicar sitio web](improvements-in-visual-studio-2005/_static/image7.jpg)

**Figura 10**: el cuadro de diálogo Publicar sitio web

> [!NOTE]
> La utilidad ASPNET/_compile. exe también se puede usar para compilar previamente una aplicación Web ASP.NET. Esa herramienta se tratará en el módulo 9.

Al publicar un sitio web, los archivos precompilados se almacenan en la carpeta Temporary ASP.NET files, tal y como se muestra a continuación. Los archivos con una extensión de archivo *. Compiled* son archivos XML que definen las dependencias de archivos dll concretos. Los controles de usuario o WebForm se compilan en archivos dll aleatorios que comienzan por *App/_Web/_* .

Si deja activada la casilla *permitir que este sitio precompilado sea actualizable* , el marcado dentro de los formularios Web Forms y los controles de usuario no se compilará previamente en un archivo DLL que le permita realizar cambios después de la implementación. Si prefiere bloquear el marcado para que no se permitan los cambios en el contenido implementado, desactive esta casilla.

La casilla *usar nombres fijos y ensamblados de una sola página* permite deshabilitar la compilación por lotes para que cada página se compile en un ensamblado con nombre fijo. Dejar este cuadro desactivado le permite aprovechar la compilación por lotes.

La casilla *Habilitar nombres seguros en ensamblados precompilados* le permite asignar un nombre seguro a los ensamblados precompilados.

> [!NOTE]
> En ASP.NET 1. x, los ensamblados con nombre seguro tenían que instalarse en la caché de ensamblados global (GAC). En ASP.NET 2,0, no es necesario instalar ensamblados con nombre seguro en la GAC.

![Archivos compilados previamente de ASP.NET Applications](improvements-in-visual-studio-2005/_static/image8.jpg)

**Figura 11**: archivos compilados previamente de ASP.Net Applications

> [!NOTE]
> En la aplicación anterior, no había ningún archivo Web. config. Si lo hubiera, se habría llamado *PrecompiledApp. config* después del proceso publicar sitio Web.

## <a name="improvements-in-deployment"></a>Mejoras en la implementación

Al igual que con Visual Studio 2002 y 2003, Visual Studio 2005 ofrece una característica de copia de proyecto. Sin embargo, la característica se ha recuperado de la carne en Visual Studio 2005 y ahora se denomina copiar sitio Web.

El cuadro de diálogo Copiar sitio web se divide en un marco izquierdo y un marco derecho. El marco izquierdo se denomina sitio web de origen y el marco derecho se denomina sitio Web remoto. Una cosa que puede confundir a algunos desarrolladores es que el sitio que se muestra en el marco derecho no es necesariamente un sitio remoto. Podría ser un sitio en el sistema de archivos local o en la instancia local de IIS. Además, el sitio que se muestra en el marco izquierdo no es necesariamente el sitio web de origen porque el cuadro de diálogo permite publicar desde el sitio Web remoto *en* el sitio web de origen.

Si va a copiar un proyecto en un sitio Web remoto, dicho sitio debe tener instalado el Extensiones de servidor de FrontPage. Si no es así, tendrá que conectarse mediante FTP. Por otro lado, si va a copiar un proyecto en la instancia local de IIS, Extensiones de servidor de FrontPage no son necesarios.

> [!NOTE]
> Si intenta crear un nuevo sitio web en la instancia local de IIS y se instalan las extensiones de servidor de FrontPage 2002, recibirá un mensaje de error que le indicará que no se admite la creación de sitios web en un servidor de SharePoint. En ese caso, tiene la opción de instalar las extensiones de servidor de FrontPage 2000 o quitar el Extensiones de servidor de FrontPage.

Haga clic aquí para ver un tutorial en vídeo de la característica Copiar sitio Web.

![](improvements-in-visual-studio-2005/_static/image4.png)

[Abrir vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/copysite1.wmv)

## <a name="improvements-in-debugging"></a>Mejoras en la depuración

En Visual Studio 2005 hay cuatro mejoras importantes en la depuración.

- La depuración local como no administrador es posible fuera de la caja.
- El atributo Debug del elemento Compilation ahora es false de forma predeterminada.
- La instalación y configuración de la depuración remota es más sencilla que antes.
- Ahora puede depurar un sitio web abierto a través de una ubicación FTP.

## <a name="debugging-as-a-non-administrator"></a>Depuración como no administrador

La adición de la Servidor de desarrollo de ASP.NET permite a los usuarios que no son administradores depurar fácilmente aplicaciones de ASP.NET directamente. Cuando se depura una aplicación de ASP.NET que se ejecuta en el sistema de archivos local, Visual Studio inicia el Servidor de desarrollo de ASP.NET en el contexto del usuario que ha iniciado sesión. Dicho usuario puede depurar la aplicación sin ninguna configuración adicional.

## <a name="debug-is-false-by-default"></a>Debug es false de forma predeterminada

En ASP.NET 1. x, el atributo *Debug* del elemento *Compilation* del archivo Web. config se estableció en *true* de forma predeterminada. Siempre se recomienda que los desarrolladores establezcan este atributo en *false* antes de implementar una aplicación en producción, pero como la mayoría de los desarrolladores no comprenden totalmente las consecuencias de dejar el atributo de depuración establecido en true, simplemente lo dejan tal cual.

El problema más grave de tener el atributo Debug establecido en true es que deshabilita el modelo de compilación por lotes de ASP. extranets. Por lo tanto, cada página se compila en un archivo DLL independiente. Si una aplicación web se compone de miles de páginas (no escuchadas por ningún medio), eso significa que la aplicación creará varios miles de archivos dll pequeños. Aunque estos archivos dll tienen un tamaño reducido, no se cargan en ninguna ubicación determinada en la memoria. Por lo tanto, provocan la fragmentación en la memoria del sistema y pueden contribuir a las apariciones de OutOfMemoryException.

En ASP.NET 2,0, el atributo Debug está establecido en false de forma predeterminada. Como ya ha podido ver, cuando un desarrollador depure una aplicación de ASP.NET en Visual Studio 2005, se le pedirá que agregue un archivo Web. config con la depuración habilitada. Al hacerlo, se incurre en los mismos inconvenientes que se encontraban en ASP.NET 1. x, pero ahora se advierte claramente al desarrollador de que el atributo se debe restablecer en false antes de mover la aplicación a producción.

## <a name="remote-debugging-setup-and-configuration"></a>Instalación y configuración de la depuración remota

En Visual Studio 2002/2003, la depuración remota se basó en el administrador de depuración de máquinas (MDM. exe) y el proceso vs7jit. exe. Debido a esto, la solución de problemas de depuración remota a menudo era una caja negra para los clientes y a menudo no era mucho mejor para PSS.

Visual Studio 2005 quita la dependencia de los procesos MDM. exe y vs7jit. exe. En su lugar, ahora usa el servicio de supervisión de depuración remota (msvsmon. exe).

El requisito de depuración en Visual Studio 2005 de forma remota es bastante sencillo. Debe ejecutar msvsmon. exe en el servidor remoto antes de la depuración. Puede instalar el monitor de depuración remota desde el CD de Visual Studio o simplemente ejecutar msvsmon. exe desde un recurso compartido sin necesidad de instalar nada en el servidor Web.

Al ejecutar msvsmon. exe, es probable que se queja de los puertos que están bloqueados para la depuración remota. Afortunadamente, puede desbloquear fácilmente los puertos desde el interior del cuadro de diálogo de advertencia, como se muestra a continuación.

![Notificación de que Firewall de Windows está bloqueando la depuración remota](improvements-in-visual-studio-2005/_static/image9.jpg)

**Figura 12**: notificación de que Firewall de Windows está bloqueando la depuración remota

Una vez que haya desbloqueado los puertos necesarios para la depuración, verá el Monitor de depuración remota tal y como se muestra a continuación. Desde esta interfaz, puede supervisar las conexiones y cambiar los permisos de depuración fácilmente.

![El Monitor de depuración remota](improvements-in-visual-studio-2005/_static/image10.jpg)

**Figura 13**: el monitor de depuración remota

También es posible depurar de forma remota una aplicación web abierta a través de FTP. Los pasos son los mismos que los descritos anteriormente. Sin embargo, tendrá que especificar una dirección URL base para examinar el proyecto FTP tal y como se describe anteriormente en este módulo.

## <a name="lab-2"></a>Laboratorio 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Depuración remota con Visual Studio 2005

Este laboratorio le guiará a través de la depuración remota con Visual Studio 2005.

Haga clic aquí para ver un tutorial en vídeo de este laboratorio.

![](improvements-in-visual-studio-2005/_static/image5.png)

[Abrir vídeo de pantalla completa](improvements-in-visual-studio-2005/_static/remdebug1.wmv)

Este laboratorio requiere tener dos máquinas, una que ejecute Visual Studio 2005 y la otra que ejecute IIS 5 o posterior.

1. Abra Visual Studio 2005 y cree un nuevo sitio web en el servidor remoto.

> [!NOTE]
> Puede crear el sitio web en una instancia remota de IIS o a través de FTP.

1. En el servidor Web remoto, busque msvsmon. exe en el equipo de desarrollo con una ruta de acceso UNC y ejecútelo.  
 La ubicación predeterminada de msvsmon. exe es//Server/c $/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.
2. Si se le pide que desbloquee los puertos para la depuración remota, hágalo.
3. En el equipo de desarrollo, abra el código subyacente de default. aspx y establezca un punto de interrupción en el método Page/_Load.
4. Inicie la depuración desde el equipo de desarrollo.

Debería alcanzar el punto de interrupción de la manera esperada.

## <a name="aspnet-development-server"></a>servidor de desarrollo de ASP.NET

Como ya hemos comentado, Visual Studio 2005 se suministra con un servidor Web denominado Servidor de desarrollo de ASP.NET. (El Servidor de desarrollo de ASP.NET a veces se conoce como Cassini). Este servidor web es un método cómodo para examinar y depurar aplicaciones web que se ejecutan en el sistema de archivos.

El Servidor de desarrollo de ASP.NET es un servidor Web restringido. No permite conexiones remotas, no permite solicitudes de ningún usuario que no sea el usuario que inició el servidor Web. Tampoco tiene la capacidad de servir páginas ASP. Solo se sirven recursos ASP.NET y recursos HTML (incluidas imágenes, archivos CSS, etc.).

El Servidor de desarrollo de ASP.NET se puede iniciar a través de la línea de comandos ejecutando el archivo WebDev. WebServer. exe ubicado en c:/Windows/Microsoft. NET/Framework/v 2.0./ */* / */* /*. En el siguiente cuadro de diálogo se muestran los parámetros que están disponibles.

![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Figura 14**

> [!NOTE]
> El Servidor de desarrollo de ASP.NET no se admite cuando se inicia explícitamente a través de la línea de comandos.
