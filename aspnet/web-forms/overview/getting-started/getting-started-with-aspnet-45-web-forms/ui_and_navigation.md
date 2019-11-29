---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
title: Interfaz de usuario y navegación | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 5c76891d-e515-4885-b576-76bd2c494efe
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/ui_and_navigation
msc.type: authoredcontent
ms.openlocfilehash: ac1dcaf1ba911fdcaeb3845c6836ec771733d93e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636812"
---
# <a name="ui-and-navigation"></a>Interfaz de usuario y navegación

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial, modificará la interfaz de usuario de la aplicación web predeterminada para admitir características de la aplicación de front-end de Wingtip Toys Store. Además, agregará una navegación simple y enlazada a datos. Este tutorial se basa en el tutorial anterior "creación de la capa de acceso a datos" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo cambiar la interfaz de usuario para admitir características de la aplicación de front-end de Wingtip Toys Store.
- Cómo configurar un elemento HTML5 para incluir la navegación de páginas.
- Cómo crear un control controlado por datos para navegar a datos de productos específicos.
- Cómo mostrar los datos de una base de datos creada mediante Entity Framework Code First.

Los formularios Web Forms de ASP.NET permiten crear contenido dinámico para la aplicación Web. Cada página web de ASP.NET se crea de forma similar a una página web HTML estática (una página que no incluye el procesamiento basado en servidor), pero ASP.NET página web incluye elementos adicionales que ASP.NET reconoce y procesa para generar HTML cuando se ejecuta la página.

Con una página HTML estática (archivo *. htm* o *. html* ), el servidor cumple una solicitud de `Web` leyendo el archivo y enviándolo tal cual al explorador. Por el contrario, cuando alguien solicita una página web de ASP.NET (archivo *. aspx* ), la página se ejecuta como un programa en el servidor Web. Mientras la página se está ejecutando, puede realizar cualquier tarea que requiera el sitio web, como calcular valores, leer o escribir información de base de datos, o llamar a otros programas. Como resultado, la página genera dinámicamente el marcado (como elementos en HTML) y envía esta salida dinámica al explorador.

## <a name="modifying-the-ui"></a>Modificar la interfaz de usuario

Continuará con esta serie de tutoriales modificando la página *default. aspx* . Modificará la interfaz de usuario que ya se ha establecido en la plantilla predeterminada utilizada para crear la aplicación. El tipo de modificaciones que hará es típico al crear cualquier aplicación de formularios Web Forms. Para ello, cambie el título, reemplace el contenido y quite el contenido predeterminado innecesario.

1. Abra o cambie a la página *default. aspx* .
2. Si la página aparece en la vista **diseño** , cambie a vista **código fuente** .
3. En la parte superior de la página de la Directiva de `@Page`, cambie el atributo de `Title` a "Welcome", como se muestra en amarillo a continuación. 

    [!code-aspx[Main](ui_and_navigation/samples/sample1.aspx?highlight=1)]
4. También en la página *default. aspx* , reemplace todo el contenido predeterminado incluido en la etiqueta de `<asp:Content>` para que el marcado aparezca como se indica a continuación. 

    [!code-aspx[Main](ui_and_navigation/samples/sample2.aspx)]
5. Guarde la página *default. aspx* seleccionando **Guardar default. aspx** en el menú **archivo** .

   La página *default. aspx* resultante aparecerá de la siguiente manera: 

[!code-aspx[Main](ui_and_navigation/samples/sample3.aspx)]

En el ejemplo, se ha establecido el atributo `Title` de la Directiva `@Page`. Cuando el HTML se muestra en un explorador, el código de servidor `<%: Page.Title %>` se resuelve en el contenido incluido en el atributo de `Title`.

En la página de ejemplo se incluyen los elementos básicos que constituyen una página web de ASP.NET. La página contiene texto estático como puede tener en una página HTML, junto con elementos específicos de ASP.NET. El contenido incluido en la página *default. aspx* se integrará con el contenido de la página maestra, que se explicará más adelante en este tutorial.

### <a name="page-directive"></a>@Page (Directiva)

Los formularios Web Forms ASP.NET normalmente contienen directivas que le permiten especificar propiedades de página e información de configuración para la página. ASP.NET usa las directivas como instrucciones para procesar la página, pero no se representarán como parte del marcado que se envía al explorador.

La Directiva de uso más común es la Directiva de `@Page`, que permite especificar muchas opciones de configuración para la página, entre las que se incluyen las siguientes:

1. El lenguaje de programación de servidor para el código de la página C#, como.
2. Si la página es una página con código de servidor directamente en la página, que se denomina página de un solo archivo, o si se trata de una página con código en un archivo de clase independiente, lo que se denomina página de código subyacente.
3. Si la página tiene una página maestra asociada y, por tanto, debe tratarse como una página de contenido.
4. Opciones de depuración y seguimiento.

Si no incluye una directiva de `@Page` en la página, o si la Directiva no incluye una configuración específica, se heredará un valor del archivo de configuración *Web. config* o del archivo de configuración *Machine. config* . El archivo *Machine. config* proporciona opciones de configuración adicionales a todas las aplicaciones que se ejecutan en un equipo.

> [!NOTE] 
> 
> El *archivo Machine. config* también proporciona detalles acerca de todas las posibles opciones de configuración.

### <a name="web-server-controls"></a>Controles de servidor Web

En la mayoría de las aplicaciones de formularios Web Forms ASP.NET, agregará controles que permiten al usuario interactuar con la página, como botones, cuadros de texto, listas, etc. Estos controles de servidor web son similares a los botones y elementos de entrada de HTML. Sin embargo, se procesan en el servidor, lo que le permite usar el código del servidor para establecer sus propiedades. Estos controles también generan eventos que se pueden controlar en el código del servidor.

Los controles de servidor usan una sintaxis especial que ASP.NET reconoce cuando se ejecuta la página. El nombre de etiqueta de los controles de servidor ASP.NET comienza con un prefijo `asp:`. Esto permite a ASP.NET reconocer y procesar estos controles de servidor. El prefijo puede ser diferente si el control no forma parte de la .NET Framework. Además del prefijo `asp:`, los controles de servidor ASP.NET también incluyen el atributo `runat="server"` y un `ID` que puede usar para hacer referencia al control en el código del servidor.

Cuando se ejecuta la página, ASP.NET identifica los controles de servidor y ejecuta el código que está asociado a esos controles. Muchos controles representan código HTML u otro marcado en la página cuando se muestra en un explorador.

### <a name="server-code"></a>Código de servidor

La mayoría de las aplicaciones de formularios Web Forms ASP.NET incluyen código que se ejecuta en el servidor cuando se procesa la página. Como se mencionó anteriormente, el código de servidor se puede usar para realizar diversas acciones, como agregar datos a un control ListView. ASP.NET admite muchos idiomas para ejecutarse en el servidor, C#incluidos, Visual Basic, J# y otros.

ASP.NET admite dos modelos para escribir código de servidor para una página web. En el modelo de un solo archivo, el código de la página se encuentra en un elemento de script en el que la etiqueta de apertura incluye el `runat="server"` atributo. Como alternativa, puede crear el código de la página en un archivo de clase independiente, al que se hace referencia como modelo de código subyacente. En este caso, la página de formularios Web Forms de ASP.NET generalmente no contiene ningún código de servidor. En su lugar, la Directiva `@Page` incluye información que vincula la página *. aspx* con su archivo de código subyacente asociado.

El atributo `CodeBehind` incluido en la Directiva `@Page` especifica el nombre del archivo de clase independiente y el atributo `Inherits` especifica el nombre de la clase dentro del archivo de código subyacente que corresponde a la página.

### <a name="updating-the-master-page"></a>Actualización de la página maestra

En los formularios Web Forms de ASP.NET, las páginas maestras le permiten crear un diseño coherente para las páginas de la aplicación. Una sola página maestra define la apariencia y el comportamiento estándar que desea para todas las páginas (o un grupo de páginas) en la aplicación. Después, puede crear páginas de contenido individuales que contengan el contenido que desea mostrar, tal como se explicó anteriormente. Cuando los usuarios solicitan las páginas de contenido, ASP.NET las combina con la página maestra para producir una salida que combine el diseño de la página maestra con el contenido de la página de contenido.

El nuevo sitio necesita un único logotipo para mostrar en todas las páginas. Para agregar este logotipo, puede modificar el código HTML de la página maestra.

1. En **Explorador de soluciones**, busque y abra la página **site. Master** .
2. Si la página está en la vista **diseño** , cambie a la vista **código fuente** .
3. Actualice la página maestra **modificando o agregando** el marcado resaltado en amarillo: 

    [!code-aspx[Main](ui_and_navigation/samples/sample4.aspx?highlight=9,49,76-81,87)]

Este código HTML mostrará la imagen denominada *logo. jpg* de la carpeta *images* de la aplicación Web, que se agregará más adelante. Cuando se muestra en un explorador una página que utiliza la página maestra, se mostrará el logotipo. Si un usuario hace clic en el logotipo, el usuario volverá a la página *default. aspx* . La etiqueta delimitadora HTML `<a>` incluye el control de servidor de imágenes y permite incluir la imagen como parte del vínculo. El atributo `href` de la etiqueta delimitadora especifica el "`~/`" raíz del sitio web como ubicación del vínculo. De forma predeterminada, se muestra la página *default. aspx* cuando el usuario navega a la raíz del sitio Web. El control de servidor `<asp:Image>` de **imagen** incluye propiedades de suma, como `BorderStyle`, que se representan como HTML cuando se muestran en un explorador.

### <a name="master-pages"></a>Páginas maestras

Una página maestra es un archivo ASP.NET con la extensión. Master (por ejemplo, *site. Master*) con un diseño predefinido que puede incluir texto estático, elementos HTML y controles de servidor. La página maestra se identifica mediante una directiva de `@Master` especial que reemplaza a la Directiva de `@Page` que se utiliza para las páginas *. aspx* normales.

Además de la Directiva de `@Master`, la página maestra también contiene todos los elementos HTML de nivel superior de una página, como `html`, `head`y `form`. Por ejemplo, en la página maestra agregada anteriormente, use una `table` HTML para el diseño, un elemento `img` para el logotipo de la compañía, texto estático y controles de servidor para administrar la pertenencia común a su sitio. Puede usar cualquier elemento HTML y cualquier elemento ASP.NET como parte de la página maestra.

Además del texto estático y los controles que aparecerán en todas las páginas, la página maestra también incluye uno o más controles **ContentPlaceHolder** . Estos controles de marcador de posición definen regiones en las que aparecerá contenido reemplazable. A su vez, el contenido reemplazable se define en las páginas de contenido, como *default. aspx*, mediante el control de servidor de **contenido** .

#### <a name="adding-image-files"></a>Agregar archivos de imagen

La imagen de logotipo a la que se hace referencia anteriormente, junto con todas las imágenes de producto, debe agregarse a la aplicación web para que se puedan ver cuando el proyecto se muestre en un explorador.

#### <a name="download-from-msdn-samples-site"></a>Descargar desde el sitio de ejemplos de MSDN:

[Introducción con formularios Web Forms de ASP.NET 4,5 y Visual Studio 2013-Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

La descarga incluye los recursos de la carpeta *WingtipToys-assets* que se usan para crear la aplicación de ejemplo.

1. Si todavía no lo ha hecho, descargue los archivos de ejemplo comprimidos mediante el vínculo anterior del sitio de ejemplos de MSDN.
2. Una vez descargado, abra el archivo. zip y copie el contenido en una carpeta local de la máquina.
3. Busque y abra la carpeta *WingtipToys-assets* .
4. Arrastrando y colocando, copie la carpeta de *catálogos* de la carpeta local a la raíz del proyecto de aplicación web en el **Explorador de soluciones** de Visual Studio. 

    ![Interfaz de usuario y navegación: copiar archivos](ui_and_navigation/_static/image1.png)
5. A continuación, cree una nueva carpeta denominada *images* haciendo clic con el botón derecho en el proyecto **WingtipToys** en **Explorador de soluciones** y seleccionando **Agregar** -&gt; **nueva carpeta**.
6. Copie el *archivo logo. jpg* de la carpeta *WingtipToys-assets* del **Explorador de archivos** en la carpeta *images* del proyecto de aplicación web en **Explorador de soluciones** de Visual Studio.
7. Haga clic en la opción **Mostrar todos los archivos** de la parte superior de **Explorador de soluciones** para actualizar la lista de archivos si no ve los nuevos archivos.  
  
    **Explorador de soluciones** muestra ahora los archivos de proyecto actualizados. 

    ![Interfaz de usuario y navegación: Explorador de soluciones](ui_and_navigation/_static/image2.png)

### <a name="adding-pages"></a>Agregar páginas

Antes de agregar la navegación a la aplicación Web, primero agregará dos nuevas páginas a las que navegará. Más adelante en esta serie de tutoriales, se mostrarán los productos y los detalles del producto en estas páginas nuevas.

1. En **Explorador de soluciones**, haga clic con el botón secundario en **WingtipToys**, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.   
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el grupo plantillas **Web** de **Visual C#**  -&gt; a la izquierda. A continuación, seleccione **Web Forms con la página maestra** en la lista central y asígnele el nombre *productlist. aspx*. 

    ![Interfaz de usuario y navegación-cuadro de diálogo Agregar nuevo elemento](ui_and_navigation/_static/image3.png)
3. Seleccione **site. Master** para adjuntar la página maestra a la página *. aspx* recién creada. 

    ![Interfaz de usuario y navegación: seleccionar página maestra](ui_and_navigation/_static/image4.png)
4. Agregue una página adicional denominada *productdetails. aspx* siguiendo estos mismos pasos.

### <a name="updating-bootstrap"></a>Actualizando bootstrap

Las plantillas de proyecto de Visual Studio 2013 usan [bootstrap](http://getbootstrap.com/), un diseño y un marco de trabajo de creación de proyectos creados por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa que los diseños pueden adaptarse dinámicamente a distintos tamaños de ventana del explorador. También puede usar la característica de las funciones de bootstrap para aplicar fácilmente un cambio en la apariencia y el funcionamiento de la aplicación. De forma predeterminada, la plantilla de aplicación Web ASP.NET en Visual Studio 2013 incluye bootstrap como un paquete NuGet.

En este tutorial, cambiará la apariencia y el funcionamiento de la aplicación Wingtip Toys reemplazando los archivos CSS de bootstrap.

1. En **Explorador de soluciones**, abra la carpeta de *contenido* .
2. Haga clic con el botón secundario en el archivo *bootstrap. CSS* y cambie su nombre a *bootstrap-original. CSS*.
3. Cambie el nombre de *bootstrap. min. CSS* a *bootstrap-original. min. CSS*.
4. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *contenido* y seleccione **Abrir carpeta en el explorador de archivos**.  
   Se mostrará el explorador de archivos. Guardará los archivos CSS de bootstrap descargados en esta ubicación.
5. En el explorador, vaya a [https://bootswatch.com/3/](https://bootswatch.com/3/).
6. Desplácese por la ventana del explorador hasta que vea el tema Cerulean. 

    ![Interfaz de usuario y navegación: tema Cerulean](ui_and_navigation/_static/image5.png)
7. Descargue los archivos *bootstrap. CSS* y *bootstrap. min. CSS* en la carpeta de *contenido* . Use la ruta de acceso a la carpeta de contenido que se muestra en la ventana del **Explorador de archivos** que abrió anteriormente.
8. En la parte superior de **Explorador de soluciones**de **Visual Studio** , seleccione la opción **Mostrar todos los archivos** para mostrar los nuevos archivos en la carpeta de contenido. 

    ![Interfaz de usuario y navegación: Explorador de soluciones](ui_and_navigation/_static/image6.png)

   Verá los dos nuevos archivos CSS en la carpeta de **contenido** , pero Observe que el icono situado junto a cada nombre de archivo está atenuado. Esto significa que el archivo aún no se ha agregado al proyecto.
9. Haga clic con el botón derecho en los archivos *bootstrap. CSS* y *bootstrap. min. CSS* y seleccione **incluir en el proyecto**.   
   Al ejecutar la aplicación Wingtip Toys más adelante en este tutorial, se mostrará la nueva interfaz de usuario.

> [!NOTE] 
> 
> La plantilla de aplicación Web ASP.NET usa el archivo *bundle. config* en la raíz del proyecto para almacenar la ruta de acceso de los archivos CSS de bootstrap.

### <a name="modifying-the-default-navigation"></a>Modificar la navegación predeterminada

La navegación predeterminada para cada página de la aplicación se puede modificar cambiando el elemento de lista de navegación desordenado que se encuentra en la página *site. Master* .

1. En **Explorador de soluciones**, busque y abra la página *site. Master* .
2. Agregue el vínculo de navegación adicional resaltado en amarillo a la lista sin ordenar que se muestra a continuación:   

    [!code-html[Main](ui_and_navigation/samples/sample5.html?highlight=5)]

Como puede ver en el código HTML anterior, modificó cada elemento de línea `<li>` que contiene una etiqueta delimitadora `<a>` con un vínculo `href` atributo. Cada `href` apunta a una página de la aplicación Web. En el explorador, cuando un usuario hace clic en uno de estos vínculos (como **productos**), navegará a la página contenida en el `href` (por ejemplo, **productlist. aspx**). Al final de este tutorial, ejecutará la aplicación.

> [!NOTE] 
> 
> El carácter de tilde (`~`) se usa para especificar que la ruta de acceso de `href` comienza en la raíz del proyecto.

### <a name="adding-a-data-control-to-display-navigation-data"></a>Agregar un control de datos para mostrar los datos de navegación

A continuación, agregará un control para mostrar todas las categorías de la base de datos. Cada categoría actuará como un vínculo a la página *productlist. aspx* . Cuando un usuario hace clic en un vínculo de categoría en el explorador, navegará a la página productos y verá solo los productos asociados a la categoría seleccionada.

Usará un control **ListView** para mostrar todas las categorías contenidas en la base de datos. Para agregar un control **ListView** a la página maestra:

1. En la página *site. Master* , agregue el siguiente elemento `<div>` resaltado **después** del elemento `<div>` que contiene el `id="TitleContent"` que agregó anteriormente:  

    [!code-aspx[Main](ui_and_navigation/samples/sample6.aspx?highlight=7-21)]

Este código mostrará todas las categorías de la base de datos. El control **ListView** muestra cada nombre de categoría como texto de vínculo e incluye un vínculo a la página *productlist. aspx* con un valor de cadena de consulta que contiene el `ID` de la categoría. Al establecer la propiedad `ItemType` en el control **ListView** , la expresión de enlace de datos `Item` está disponible dentro del nodo `ItemTemplate` y el control se vuelve fuertemente tipado. Puede seleccionar detalles del objeto `Item` mediante IntelliSense, como especificar el `CategoryName`. Este código se encuentra dentro del contenedor `<%#: %>` que marca una expresión de enlace de datos. Agregando (:) al final del prefijo `<%#`, el resultado de la expresión de enlace de datos se codifica en HTML. Cuando el resultado está codificado en HTML, la aplicación está mejor protegida contra ataques por inyección de scripts entre sitios (XSS) y inyección de HTML.

> [!NOTE] 
> 
> **Tip**
> 
> Al agregar código escribiendo durante el desarrollo, puede estar seguro de que se encuentra un miembro válido de un objeto porque los controles de datos fuertemente tipados muestran los miembros disponibles basados en IntelliSense. IntelliSense ofrece opciones de código adecuadas para el contexto a medida que escribe código, como propiedades, métodos y objetos.

En el paso siguiente, implementará el método `GetCategories` para recuperar los datos.

### <a name="linking-the-data-control-to-the-database"></a>Vincular el control de datos a la base de datos

Antes de poder Mostrar datos en el control de datos, debe vincular el control de datos a la base de datos. Para crear el vínculo, puede modificar el código subyacente del archivo *site.Master.CS* .

1. En **Explorador de soluciones**, haga clic con el botón secundario en la página *site. Master* y, a continuación, haga clic en **Ver código**. El archivo *site.Master.CS* se abre en el editor.
2. Cerca del principio del archivo *site.Master.CS* , agregue dos espacios de nombres adicionales para que todos los espacios de nombres incluidos aparezcan de la siguiente manera:  

    [!code-csharp[Main](ui_and_navigation/samples/sample7.cs?highlight=8-9)]
3. Agregue el método de `GetCategories` resaltado después del controlador de eventos de `Page_Load` como se indica a continuación:  

    [!code-csharp[Main](ui_and_navigation/samples/sample8.cs?highlight=6-11)]

El código anterior se ejecuta cuando cualquier página que utiliza la página maestra se carga en el explorador. El control `ListView` (denominado "categoryList") que agregó anteriormente en este tutorial usa el enlace de modelos para seleccionar los datos. En el marcado del control `ListView`, establezca la propiedad `SelectMethod` del control en el método `GetCategories`, mostrado anteriormente. El control `ListView` llama al método `GetCategories` en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos. Obtendrá más información sobre el enlace de datos en el siguiente tutorial.

### <a name="running-the-application-and-creating-the-database"></a>Ejecutar la aplicación y crear la base de datos

Anteriormente en esta serie de tutoriales, ha creado una clase de inicializador (denominada "ProductDatabaseInitializer") y ha especificado esta clase en el archivo *global.asax.CS* . La Entity Framework generará la base de datos cuando la aplicación se ejecute por primera vez, ya que el método `Application_Start` incluido en el archivo *global.asax.CS* llamará a la clase de inicializador. La clase de inicializador usará las clases de modelo (`Category` y `Product`) que agregó anteriormente en esta serie de tutoriales para crear la base de datos.

1. En **Explorador de soluciones**, haga clic con el botón secundario en la página *default. aspx* y seleccione **establecer como página de inicio**.
2. En Visual Studio, presione **F5**.   
 Se tardará un poco en configurar todo durante la primera ejecución.   
    ![interfaz de usuario y ventanas del explorador de navegación](ui_and_navigation/_static/image7.png)  
 Al ejecutar la aplicación, se compilará la aplicación y se creará la base de datos denominada *wingtiptoys. MDF* en la carpeta *App\_Data* . En el explorador, verá un menú de navegación categoría. Este menú se generó mediante la recuperación de las categorías de la base de datos. En el siguiente tutorial, se implementará la navegación.
3. Cierre el explorador para detener la aplicación en ejecución.

### <a name="reviewing-the-database"></a>Revisar la base de datos

Abra el archivo *Web. config* y mire la sección cadena de conexión. Puede ver que el valor `AttachDbFilename` de la cadena de conexión apunta al `DataDirectory` del proyecto de aplicación Web. El valor `|DataDirectory|` es un valor reservado que representa la carpeta *App\_Data* del proyecto. Esta carpeta es donde se encuentra la base de datos que se creó a partir de las clases de entidad.

[!code-xml[Main](ui_and_navigation/samples/sample9.xml)]

> [!NOTE] 
> 
> Si la carpeta de datos de la\_de la *aplicación* no está visible o si la carpeta está vacía, seleccione el icono de **actualización** y, después, el icono **Mostrar todos los archivos** en la parte superior de la ventana de **Explorador de soluciones** . Es posible que sea necesario expandir el ancho de las ventanas de **Explorador de soluciones** para mostrar todos los iconos disponibles.

Ahora puede inspeccionar los datos contenidos en el archivo de base de datos *wingtiptoys. MDF* mediante la ventana **Explorador de servidores** .

1. Expanda la carpeta *App\_Data* . Si la carpeta de datos de la\_de la *aplicación* no está visible, consulte la nota anterior.
2. Si el archivo de base de datos *wingtiptoys. MDF* no está visible, seleccione el icono de **actualización** y, después, el icono **Mostrar todos los archivos** en la parte superior de la ventana de **Explorador de soluciones** .
3. Haga clic con el botón secundario en el archivo de base de datos *wingtiptoys. MDF* y seleccione **abrir**.  
    Se muestra **Explorador de servidores** . 

    ![Interfaz de usuario y navegación: Explorador de servidores](ui_and_navigation/_static/image8.png)
4. Expanda la carpeta *tablas* .
5. Haga clic con el botón secundario en la tabla **Products**y seleccione **Mostrar datos de tabla**.  
 Se muestra la tabla **productos** . 

    ![Interfaz de usuario y navegación: tabla de productos](ui_and_navigation/_static/image9.png)
6. Esta vista le permite ver y modificar los datos de la tabla **productos** a mano.
7. Cierre la ventana de la tabla **productos** .
8. En el **Explorador de servidores**, vuelva a hacer clic con el botón secundario en la tabla **Products** y seleccione **abrir definición de tabla**.  
 Se muestra el diseño de datos de la tabla **productos** . 

    ![Interfaz de usuario y navegación: diseño de productos](ui_and_navigation/_static/image10.png)
9. En la pestaña **T-SQL** , verá la instrucción DDL de SQL que se usó para crear la tabla. También puede usar la interfaz de usuario en la pestaña **diseño** para modificar el esquema.
10. En el **Explorador de servidores**, haga clic con el botón derecho en base de datos **WingtipToys** y seleccione **cerrar conexión**.   
 Al desasociar la base de datos de Visual Studio, el esquema de la base de datos se podrá modificar más adelante en esta serie de tutoriales.
11. Vuelva a **Explorador de soluciones**seleccionando la pestaña **Explorador de soluciones** en la parte inferior de la ventana de **Explorador de servidores** .

## <a name="summary"></a>Resumen

En este tutorial de la serie, ha agregado algunas interfaces de usuario, gráficos, páginas y navegación básicos. Además, ejecutó la aplicación Web, que creó la base de datos a partir de las clases de datos que agregó en el tutorial anterior. También ha visto el contenido de la tabla *Products* de la base de datos mediante la visualización de la base de datos directamente. En el siguiente tutorial, se mostrarán los elementos de datos y los detalles de la base de datos.

## <a name="additional-resources"></a>Recursos adicionales

[Introducción a la programación ASP.NET Web Pages](https://msdn.microsoft.com/library/ms178125.aspx)   
[Información general sobre los controles de servidor Web ASP.NET](https://msdn.microsoft.com/library/zsyt68f1.aspx)   
[Tutorial de CSS](http://www.w3schools.com/css/default.asp)

> [!div class="step-by-step"]
> [Anterior](create_the_data_access_layer.md)
> [Siguiente](display_data_items_and_details.md)
