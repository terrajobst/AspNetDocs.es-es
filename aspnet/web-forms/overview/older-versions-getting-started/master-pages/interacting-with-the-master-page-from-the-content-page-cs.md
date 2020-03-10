---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: Interactuar con la página maestra desde la página de contenido (C#) | Microsoft Docs
author: rick-anderson
description: Examina cómo llamar a métodos, establecer propiedades, etc. de la página maestra desde el código de la página de contenido.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 5ef030d3bed117e98fdd090f7c63643354b47f76
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78456133"
---
# <a name="interacting-with-the-master-page-from-the-content-page-c"></a>Interactuar con la página maestra desde la página de contenido (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Examina cómo llamar a métodos, establecer propiedades, etc. de la página maestra desde el código de la página de contenido.

## <a name="introduction"></a>Introducción

En el transcurso de los cinco últimos tutoriales, hemos examinado cómo crear una página maestra, cómo definir regiones de contenido, cómo enlazar páginas de ASP.NET a una página maestra y cómo definir contenido específico de la página. Cuando un visitante solicita una página de contenido determinada, el marcado de contenido y páginas maestras se fusiona en tiempo de ejecución, lo que da lugar a la representación de una jerarquía de control unificada. Por lo tanto, ya hemos encontrado una manera en la que la página maestra y una de sus páginas de contenido pueden interactuar: la página de contenido escribe el marcado en transfuse en los controles ContentPlaceHolder de la página maestra.

Lo que hemos examinado todavía es cómo la página maestra y la página de contenido pueden interactuar mediante programación. Además de definir el marcado para los controles de ContentPlaceHolder de la página maestra, una página de contenido también puede asignar valores a las propiedades públicas de su página maestra e invocar sus métodos públicos. Del mismo modo, una página maestra puede interactuar con sus páginas de contenido. Aunque la interacción mediante programación entre una página maestra y una página de contenido es menos común que la interacción entre sus marcas declarativas, existen muchos escenarios en los que se necesita dicha interacción mediante programación.

En este tutorial se examina el modo en que una página de contenido puede interactuar mediante programación con su página maestra. en el siguiente tutorial, veremos cómo la página maestra puede interactuar de forma similar con sus páginas de contenido.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Ejemplos de interacción mediante programación entre una página de contenido y su página maestra

Cuando una región determinada de una página debe configurarse página por página, usamos un control ContentPlaceHolder. Pero, ¿qué ocurre con las situaciones en las que la mayoría de las páginas necesitan emitir una salida determinada, pero un número pequeño de páginas debe personalizarla para mostrar algo más? Un ejemplo de este tipo, que examinamos en el tutorial de [*varios ContentPlaceHolders y contenido predeterminado*](multiple-contentplaceholders-and-default-content-cs.md) , implica la visualización de una interfaz de inicio de sesión en cada página. Aunque la mayoría de las páginas deben incluir una interfaz de inicio de sesión, se debe suprimir para unas cuantas páginas, como: la página de inicio de sesión principal (`Login.aspx`); la página crear cuenta; y otras páginas a las que solo pueden acceder los usuarios autenticados. En el tutorial de [*varios ContentPlaceHolders y contenido predeterminado*](multiple-contentplaceholders-and-default-content-cs.md) se ha mostrado cómo definir el contenido predeterminado para un ContentPlaceHolder en la página maestra y, a continuación, cómo invalidarlo en las páginas en las que no se deseaba el contenido predeterminado.

Otra opción consiste en crear una propiedad o un método públicos dentro de la página maestra que indique si se va a mostrar u ocultar la interfaz de inicio de sesión. Por ejemplo, la página maestra podría incluir una propiedad pública denominada `ShowLoginUI` cuyo valor se usó para establecer la propiedad `Visible` del control de inicio de sesión en la página maestra. Las páginas de contenido en las que se debería suprimir la interfaz de usuario de inicio de sesión podrían establecer mediante programación la propiedad `ShowLoginUI` en `false`.

Quizás el ejemplo más común de interacción de contenido y página maestra se produce cuando es necesario actualizar los datos mostrados en la página maestra después de que haya transcurrido alguna acción en la página de contenido. Considere una página maestra que incluye un control GridView que muestra los cinco registros agregados más recientemente de una tabla de base de datos determinada, y que una de sus páginas de contenido incluye una interfaz para agregar nuevos registros a la misma tabla.

Cuando un usuario visita la página para agregar un nuevo registro, ve los cinco registros agregados más recientemente que se muestran en la página maestra. Después de rellenar los valores de las columnas del nuevo registro, envía el formulario. Suponiendo que el control GridView de la página maestra tiene su propiedad `EnableViewState` establecida en true (el valor predeterminado), su contenido se recarga desde el estado de vista y, por consiguiente, se muestran los cinco mismos registros aunque un registro más reciente se agregara a la base de datos. Esto puede confundir al usuario.

> [!NOTE]
> Aunque deshabilite el estado de vista de GridView para que se vuelva a enlazar a su origen de datos subyacente en cada postback, todavía no se mostrará el registro que se acaba de agregar porque los datos están enlazados a GridView anteriormente en el ciclo de vida de la página que cuando el nuevo registro se agrega a Datab ase.

Para solucionar este error de modo que el registro agregado se muestre en el control GridView de la página maestra en el postback, es necesario indicar a GridView que vuelva a enlazar a su origen de datos *una vez* agregado el nuevo registro a la base de datos. Esto requiere interacción entre el contenido y las páginas maestras porque la interfaz para agregar el nuevo registro (y sus controladores de eventos) está en la página de contenido, pero el control GridView que debe actualizarse está en la página maestra.

Dado que la actualización de la presentación de la página maestra desde un controlador de eventos en la página de contenido es una de las necesidades más comunes de contenido y de interacción con las páginas maestras, vamos a explorar este tema con más detalle. La descarga de este tutorial incluye una base de datos Microsoft SQL Server 2005 Express Edition denominada `NORTHWIND.MDF` en la carpeta `App_Data` del sitio Web. La base de datos Northwind almacena información de productos, empleados y ventas para una compañía ficticia, Northwind Traders.

En el paso 1 se explica cómo mostrar los cinco últimos productos agregados en GridView en la página maestra. En el paso 2 se crea una página de contenido para agregar nuevos productos. En el paso 3, veremos cómo crear propiedades y métodos públicos en la página maestra y el paso 4 muestra cómo interactuar mediante programación con estas propiedades y métodos desde la página de contenido.

> [!NOTE]
> En este tutorial no se profundiza en los detalles del trabajo con datos en ASP.NET. Los pasos para configurar la página maestra para Mostrar datos y la página de contenido para insertar datos están completos, pero Breezy. Para obtener información más detallada sobre cómo mostrar e insertar datos y usar los controles SqlDataSource y GridView, consulte los recursos en la sección lecturas adicionales al final de este tutorial.

## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Paso 1: mostrar los cinco productos agregados más recientemente en la página maestra

Abra la página maestra de `Site.master` y agregue una etiqueta y un control GridView al `<div>``leftContent`. Borre la propiedad `Text` de la etiqueta, establezca su propiedad `EnableViewState` en false y su propiedad `ID` en `GridMessage`; Establezca la propiedad `ID` de GridView en `RecentProducts`. A continuación, desde el diseñador, expanda la etiqueta inteligente de GridView y elija enlazarla a un nuevo origen de datos. Se inicia el Asistente para la configuración de orígenes de datos. Dado que la base de datos Northwind de la carpeta `App_Data` es una Microsoft SQL Server base de datos, elija crear una SqlDataSource seleccionando (vea la figura 1). Asigne al `RecentProductsDataSource`SqlDataSource el nombre.

[![enlazar GridView a un control SqlDataSource denominado RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Figura 01**: enlace de GridView a un control SqlDataSource denominado `RecentProductsDataSource` ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))

El siguiente paso nos pide que especifique la base de datos a la que se va a conectar. Elija el archivo de base de datos de `NORTHWIND.MDF` en la lista desplegable y haga clic en siguiente. Dado que esta es la primera vez que se usa esta base de datos, el asistente le ofrecerá la forma de almacenar la cadena de conexión en `Web.config`. Haga que almacene la cadena de conexión con el nombre `NorthwindConnectionString`.

[![conectarse a la base de datos Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Figura 02**: conexión a la base de datos Northwind ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))

El Asistente para configurar orígenes de datos proporciona dos medios por los que se puede especificar la consulta que se usa para recuperar datos:

- Mediante la especificación de una instrucción SQL personalizada o un procedimiento almacenado, o bien
- Al seleccionar una tabla o vista y, a continuación, especificar las columnas que se van a devolver

Dado que queremos que solo se devuelvan los cinco últimos productos agregados, es necesario especificar una instrucción SQL personalizada. Use la siguiente consulta SELECT:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

La palabra clave `TOP 5` solo devuelve los cinco primeros registros de la consulta. La clave principal de la tabla `Products`, `ProductID`, es una columna de `IDENTITY`, que garantiza que cada nuevo producto agregado a la tabla tendrá un valor mayor que el de la entrada anterior. Por lo tanto, la ordenación de los resultados por `ProductID` en orden descendente devuelve los productos a partir de los creados más recientemente.

[![devuelven los cinco últimos productos agregados](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Figura 03**: devolver los cinco últimos productos agregados ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))

Después de completar el asistente, Visual Studio genera dos BoundFields para que GridView muestre los campos `ProductName` y `UnitPrice` devueltos por la base de datos. En este momento, el marcado declarativo de la página maestra debe incluir un marcado similar al siguiente:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Como puede ver, el marcado contiene: el control Web Label (`GridMessage`); GridView `RecentProducts`, con dos BoundFields; y un control SqlDataSource que devuelve los cinco productos agregados más recientemente.

Con este GridView creado y su control SqlDataSource configurado, visite el sitio web a través de un explorador. Como se muestra en la figura 4, verá una cuadrícula en la esquina inferior izquierda que muestra los cinco productos agregados más recientemente.

[![GridView muestra los cinco últimos productos agregados](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Figura 04**: GridView muestra los cinco últimos productos agregados ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))

> [!NOTE]
> No dude en limpiar la apariencia de GridView. Algunas sugerencias incluyen dar formato al valor de `UnitPrice` mostrado como moneda y usar colores de fondo y fuentes para mejorar la apariencia de la cuadrícula.

## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Paso 2: creación de una página de contenido para agregar nuevos productos

La siguiente tarea consiste en crear una página de contenido desde la que un usuario puede Agregar un nuevo producto a la tabla `Products`. Agregue una nueva página de contenido a la carpeta `Admin` denominada `AddProduct.aspx`, asegurándose de enlazarla a la página maestra `Site.master`. En la figura 5 se muestra el Explorador de soluciones después de que esta página se haya agregado al sitio Web.

[![agregar una nueva página ASP.NET a la carpeta admin](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Figura 05**: agregar una nueva página ASP.net a la carpeta `Admin` ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))

Recuerde que en el tutorial para [*especificar el título, las etiquetas meta y otros encabezados HTML en la página maestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) se ha creado una clase de página base personalizada denominada `BasePage` que ha generado el título de la página si no se ha establecido explícitamente. Vaya a la clase de código subyacente de la página de `AddProduct.aspx` y haga que se derive de `BasePage` (en lugar de `System.Web.UI.Page`).

Por último, actualice el archivo de `Web.sitemap` para incluir una entrada para esta lección. Agregue el siguiente marcado debajo del `<siteMapNode>` para la lección problemas de nomenclatura de ID. de control:

[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Como se muestra en la figura 6, la adición de este elemento `<siteMapNode>` se refleja en la lista lecciones.

Vuelva a `AddProduct.aspx`. En el control de contenido del `MainContent` ContentPlaceHolder, agregue un control DetailsView y asígnele el nombre `NewProduct`. Enlace DetailsView a un nuevo control SqlDataSource denominado `NewProductDataSource`. Al igual que con SqlDataSource en el paso 1, configure el Asistente para que use la base de datos Northwind y elija especificar una instrucción SQL personalizada. Dado que DetailsView se usará para agregar elementos a la base de datos, es necesario especificar una instrucción `SELECT` y una instrucción `INSERT`. Use la siguiente consulta de `SELECT`:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

A continuación, en la pestaña insertar, agregue la siguiente instrucción `INSERT`:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Después de completar el asistente, vaya a la etiqueta inteligente de DetailsView y active la casilla "habilitar inserción". Esto agrega un CommandField a DetailsView con su propiedad `ShowInsertButton` establecida en true. Dado que este DetailsView se usará únicamente para insertar datos, establezca la propiedad `DefaultMode` de DetailsView en `Insert`.

Así de simple. Vamos a probar esta página. Visite `AddProduct.aspx` a través de un explorador, escriba un nombre y un precio (consulte la figura 6).

[![agregar un nuevo producto a la base de datos](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Figura 06**: agregar un nuevo producto a la base de datos ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))

Después de escribir el nombre y el precio del nuevo producto, haga clic en el botón Insertar. Esto hace que el formulario se postback. En el postback, se ejecuta la instrucción `INSERT` del control SqlDataSource; sus dos parámetros se rellenan con los valores especificados por el usuario en los dos controles TextBox de DetailsView. Desafortunadamente, no hay ningún comentario visual de que se haya producido una inserción. Estaría bien mostrar un mensaje, lo que confirma que se ha agregado un nuevo registro. Dejo esto como un ejercicio para el lector. Además, después de agregar un nuevo registro desde DetailsView, el control GridView en la página maestra sigue mostrando los mismos cinco registros que antes. no incluye el registro recién agregado. Examinaremos cómo solucionarlo en los próximos pasos.

> [!NOTE]
> Además de agregar alguna forma de comentarios visuales que la inserción ha realizado correctamente, recomiendo que también actualice la interfaz de inserción de DetailsView para incluir la validación. Actualmente, no hay ninguna validación. Si un usuario escribe un valor no válido para el campo `UnitPrice`, como "demasiado caro", se iniciará una excepción en el PostBack cuando el sistema intente convertir esa cadena en un decimal. Para obtener más información sobre la personalización de la interfaz de inserción, consulte el [tutorial *Personalización de la interfaz de modificación de datos* ](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) de mi [trabajo con la serie de tutoriales de datos](../../data-access/index.md).

## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Paso 3: crear propiedades y métodos públicos en la página maestra

En el paso 1 se ha agregado un control Web Label denominado `GridMessage` sobre GridView en la página maestra. Esta etiqueta está pensada para mostrar opcionalmente un mensaje. Por ejemplo, después de agregar un nuevo registro a la tabla `Products`, es posible que desee mostrar un mensaje que indique: "*ProductName* se ha agregado a la base de datos". En lugar de codificar de forma rígida el texto de esta etiqueta en la página maestra, es posible que desee que la página de contenido pueda personalizar el mensaje.

Dado que el control etiqueta se implementa como una variable miembro protegida dentro de la página maestra, no se puede tener acceso a ella directamente desde las páginas de contenido. Para trabajar con la etiqueta dentro de una página maestra desde la página de contenido (o, en ese caso, cualquier control Web en la página maestra), es necesario crear una propiedad pública en la página maestra que exponga el control Web o que sirva como proxy por el que se puede  acceso. Agregue la siguiente sintaxis a la clase de código subyacente de la página maestra para exponer la propiedad `Text` de la etiqueta:

[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Cuando se agrega un nuevo registro a la tabla de `Products` de una página de contenido, el `RecentProducts` GridView en la página maestra debe volver a enlazarse a su origen de datos subyacente. Para volver a enlazar el control GridView, llame a su método `DataBind`. Dado que el control GridView de la página maestra no es accesible mediante programación para las páginas de contenido, es necesario crear un método público en la página maestra que, cuando se llama, vuelve a enlazar los datos a GridView. Agregue el método siguiente a la clase de código subyacente de la página maestra:

[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

Con la propiedad `GridMessageText` y `RefreshRecentProductsGrid` método en su lugar, cualquier página de contenido puede establecer o leer mediante programación el valor de la propiedad `Text` de la etiqueta `GridMessage` o volver a enlazar los datos al `RecentProducts` GridView. En el paso 4 se examina el modo de obtener acceso a las propiedades y los métodos públicos de la página maestra desde una página de contenido.

> [!NOTE]
> No olvide marcar las propiedades y los métodos de la página maestra como `public`. Si no denota explícitamente estas propiedades y métodos como `public`, no se podrá acceder a ellos desde la página de contenido.

## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Paso 4: llamar a los miembros públicos de la página maestra desde una página de contenido

Ahora que la página maestra tiene las propiedades y los métodos públicos necesarios, estamos preparados para invocar estas propiedades y métodos desde la página de contenido de `AddProduct.aspx`. En concreto, es necesario establecer la propiedad `GridMessageText` de la página maestra y llamar a su método `RefreshRecentProductsGrid` una vez que el nuevo producto se ha agregado a la base de datos. Todos los controles Web de datos de ASP.NET activan los eventos inmediatamente antes y después de completar varias tareas, lo que facilita que los desarrolladores de páginas tomen algunas acciones de programación antes o después de la tarea. Por ejemplo, cuando el usuario final hace clic en el botón Insertar de DetailsView, en el postback, DetailsView genera su evento `ItemInserting` antes de comenzar el flujo de trabajo de inserción. A continuación, inserta el registro en la base de datos. A continuación, DetailsView genera su evento `ItemInserted`. Por lo tanto, para trabajar con la página maestra después de agregar el nuevo producto, cree un controlador de eventos para el evento `ItemInserted` de DetailsView.

Una página de contenido puede interactuar mediante programación con su página maestra de dos maneras:

- Usar la propiedad `Page.Master`, que devuelve una referencia con tipo flexible a la página maestra, o bien
- Especifique el tipo o la ruta de acceso de archivo de la página maestra de la página a través de una directiva de `@MasterType`; Esto agrega automáticamente una propiedad fuertemente tipada a la página denominada `Master`.

Vamos a examinar ambos enfoques.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usar la propiedad`Page.Master`de tipo flexible

Todas las páginas web de ASP.NET deben derivarse de la clase `Page`, que se encuentra en el espacio de nombres `System.Web.UI`. La clase `Page` incluye una [propiedad`Master`](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) que devuelve una referencia a la página maestra de la página. Si la página no tiene una página maestra `Master` devuelve `null`.

La propiedad `Master` devuelve un objeto de tipo [`MasterPage`](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (también ubicado en el espacio de nombres `System.Web.UI`) que es el tipo base del que se derivan todas las páginas maestras. Por lo tanto, para usar las propiedades o los métodos públicos definidos en la página maestra del sitio web, debemos convertir el objeto de `MasterPage` devuelto desde la propiedad `Master` al tipo adecuado. Como hemos denominado el archivo de la página maestra `Site.master`, la clase de código subyacente se denominaba `Site`. Por lo tanto, el código siguiente convierte la propiedad `Page.Master` en una instancia de la clase site.

[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Ahora que hemos convertido la propiedad de `Page.Master` con tipo flexible en el tipo de `Site`, podemos hacer referencia a las propiedades y los métodos específicos del sitio. Como se muestra en la figura 7, la propiedad pública `GridMessageText` aparece en la lista desplegable de IntelliSense.

[![IntelliSense muestra las propiedades y los métodos públicos de la página maestra](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Figura 07**: IntelliSense muestra las propiedades y los métodos públicos de la página maestra ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))

> [!NOTE]
> Si ha llamado al archivo de la página maestra `MasterPage.master` el nombre de la clase de código subyacente de la página maestra es `MasterPage`. Esto puede conducir a código ambiguo al convertir desde el tipo `System.Web.UI.MasterPage` a la clase `MasterPage`. En Resumen, debe calificar totalmente el tipo al que está convirtiendo, que puede ser un poco complicado al usar el modelo de proyecto de sitio Web. Mi sugerencia sería asegurarse de que cuando cree la página maestra le asigne un nombre que no sea `MasterPage.master` o, incluso mejor, cree una referencia fuertemente tipada a la página maestra.

### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Crear una referencia fuertemente tipada con la Directiva`@MasterType`

Si observa atentamente, puede ver que la clase de código subyacente de una página de ASP.NET es una clase parcial (observe la palabra clave `partial` en la definición de clase). Las clases parciales se introdujeron C# en y Visual Basic with.NET Framework 2,0 y, en pocas palabras, permiten que los miembros de una clase se definan en varios archivos. El archivo de clase de código subyacente `AddProduct.aspx.cs`, por ejemplo, contiene el código que el desarrollador de páginas crea. Además de nuestro código, el motor ASP.NET crea automáticamente un archivo de clase independiente con propiedades y controladores de eventos en que traducen el marcado declarativo en la jerarquía de clases de la página.

La generación de código automática que se produce siempre que se visita una página de ASP.NET prepara la forma de algunas posibilidades bastante interesantes y útiles. En el caso de las páginas maestras, si se indica al motor de ASP.NET qué página maestra está usando la página de contenido, se genera una propiedad fuertemente tipada `Master` para nosotros.

Use la [directiva`@MasterType`](https://msdn.microsoft.com/library/ms228274.aspx) para informar al motor de ASP.net del tipo de página maestra de la página de contenido. La Directiva de `@MasterType` puede aceptar el nombre de tipo de la página maestra o su ruta de acceso de archivo. Para especificar que la página `AddProduct.aspx` usa `Site.master` como su página maestra, agregue la siguiente directiva a la parte superior de `AddProduct.aspx`:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Esta directiva indica al motor de ASP.NET que agregue una referencia fuertemente tipada a la página maestra a través de una propiedad denominada `Master`. Con la Directiva de `@MasterType` en su lugar, podemos llamar a los métodos y propiedades públicos de la página maestra de `Site.master` directamente a través de la propiedad `Master` sin ninguna conversión.

> [!NOTE]
> Si omite la Directiva de `@MasterType`, la sintaxis `Page.Master` y `Master` devuelven lo mismo: un objeto débilmente tipado en la página maestra de la página. Si incluye la Directiva `@MasterType`, `Master` devuelve una referencia fuertemente tipada a la página maestra especificada. sin embargo, `Page.Master`todavía devuelve una referencia débilmente tipada. Para obtener una visión más detallada de por qué es el caso y cómo se construye la propiedad `Master` cuando se incluye la Directiva de `@MasterType`, consulte la entrada del blog de [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) [`@MasterType` en ASP.net 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).

### <a name="updating-the-master-page-after-adding-a-new-product"></a>Actualización de la página maestra después de agregar un nuevo producto

Ahora que sabemos cómo invocar las propiedades y los métodos públicos de una página maestra desde una página de contenido, estamos listos para actualizar la página de `AddProduct.aspx` de modo que la página maestra se actualice después de agregar un nuevo producto. Al principio del paso 4 se crea un controlador de eventos para el evento `ItemInserting` del control DetailsView, que se ejecuta inmediatamente después de que el nuevo producto se haya agregado a la base de datos. Agregue el código siguiente a ese controlador de eventos:

[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

En el código anterior se usa la propiedad `Page.Master` fuertemente tipada y la propiedad `Master` fuertemente tipada. Tenga en cuenta que la propiedad `GridMessageText` está establecida en "*ProductName* Added to Grid..." Se puede acceder a los valores del producto recién agregados a través de la colección de `e.Values`; como puede ver, se tiene acceso al valor `ProductName` agregado a través de `e.Values["ProductName"]`.

En la figura 8 se muestra la página de `AddProduct.aspx` inmediatamente después de que se ha agregado a la base de datos un nuevo producto: soda de Scott. Tenga en cuenta que el nombre de producto recién agregado se indica en la etiqueta de la página maestra y que GridView se ha actualizado para incluir el producto y su precio.

[![la etiqueta de la página maestra y GridView mostrar el producto recién agregado](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Figura 08**: la etiqueta de la página maestra y el control GridView muestran el producto recién agregado ([haga clic para ver la imagen de tamaño completo](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))

## <a name="summary"></a>Resumen

Idealmente, una página maestra y sus páginas de contenido son completamente independientes entre sí y no requieren ningún nivel de interacción. Mientras que las páginas maestras y las páginas de contenido deben diseñarse teniendo en cuenta ese objetivo, hay una serie de escenarios comunes en los que una página de contenido debe interactuar con su página maestra. Uno de los motivos más comunes se centra en la actualización de una parte determinada de la presentación de la página maestra en función de alguna acción que haya transcurrido en la página de contenido.

La buena noticia es que es relativamente sencillo hacer que una página de contenido interactúe mediante programación con su página maestra. Empiece por crear propiedades o métodos públicos en la página maestra que encapsulan la funcionalidad que debe invocarse en una página de contenido. A continuación, en la página contenido, acceda a las propiedades y los métodos de la página maestra a través de la propiedad `Page.Master` fuertemente tipada o utilice la Directiva `@MasterType` para crear una referencia fuertemente tipada a la página maestra.

En el tutorial siguiente, se examina cómo hacer que la página maestra interactúe mediante programación con una de sus páginas de contenido.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Obtener acceso a los datos y actualizarlos en ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Páginas maestras de ASP.NET: sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` en ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Pasar información entre el contenido y las páginas maestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabajar con datos en tutoriales de ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue Zack Jones. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](control-id-naming-in-content-pages-cs.md)
> [Siguiente](interacting-with-the-content-page-from-the-master-page-cs.md)
