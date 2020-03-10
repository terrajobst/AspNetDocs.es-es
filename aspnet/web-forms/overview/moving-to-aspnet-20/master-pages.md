---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Páginas maestras | Microsoft Docs
author: microsoft
description: Uno de los componentes clave de un sitio web correcto es una apariencia coherente. En ASP.NET 1. x, los desarrolladores usaban controles de usuario para replicar un Elem de página común...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 36f2caf7c2c9bcafd22c8f6681c1d6b19fe5078a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457585"
---
# <a name="master-pages"></a>Páginas maestras

por [Microsoft](https://github.com/microsoft)

> Uno de los componentes clave de un sitio web correcto es una apariencia coherente. En ASP.NET 1. x, los desarrolladores usaban controles de usuario para replicar los elementos de página comunes en una aplicación Web. Aunque es ciertamente una solución factible, el uso de controles de usuario tiene algunas desventajas. Por ejemplo, un cambio en la posición de un control de usuario requiere un cambio en varias páginas en un sitio. Los controles de usuario tampoco se representan en Vista de diseño después de insertarse en una página.

Uno de los componentes clave de un sitio web correcto es una apariencia coherente. En ASP.NET 1. x, los desarrolladores usaban controles de usuario para replicar los elementos de página comunes en una aplicación Web. Aunque es ciertamente una solución factible, el uso de controles de usuario tiene algunas desventajas. Por ejemplo, un cambio en la posición de un control de usuario requiere un cambio en varias páginas en un sitio. Los controles de usuario tampoco se representan en Vista de diseño después de insertarse en una página.

ASP.NET 2,0 introduce páginas maestras como una forma de mantener una apariencia coherente y, como pronto verá, las páginas maestras representan una mejora significativa en el método de control de usuario.

## <a name="why-master-pages"></a>¿Por qué las páginas maestras?

Es posible que se pregunte por qué se necesitaban páginas maestras en ASP.NET 2,0. Después de todo, los desarrolladores de sitios web ya usan controles de usuario en ASP.NET 1. x para compartir áreas de contenido entre las páginas. En realidad, hay varias razones por las que los controles de usuario son una solución menos que óptima para crear un diseño común.

Los controles de usuario no definen realmente el diseño de página. En su lugar, definen el diseño y la funcionalidad de una parte de una página. La diferencia entre estos dos es importante porque hace que la administración de una solución de control de usuario sea mucho más difícil. Por ejemplo, si desea cambiar la posición de un control de usuario en la página, debe editar la página real en la que aparece el control de usuario. Si solo tiene unas pocas páginas, pero en sitios grandes, se convierte rápidamente en una pesadilla de administración de sitios.

Otro inconveniente de usar controles de usuario para definir un diseño común se basa en la arquitectura de ASP.NET. Si se cambia cualquier miembro público de un control de usuario, es necesario volver a compilar todas las páginas que usan el control de usuario. A su vez, ASP.NET volverá a compilar las páginas cuando se tenga acceso por primera vez. Esto, una vez más, genera una arquitectura no escalable y un problema de administración del sitio para sitios de mayor tamaño.

Estos dos problemas (y muchos más) se dirigen bien a las páginas maestras en ASP.NET 2,0.

## <a name="how-master-pages-work"></a>Cómo funcionan las páginas maestras

Una página maestra es análoga a una plantilla para otras páginas. Los elementos de página que se deben compartir entre otras páginas (es decir, menús, bordes, etc.) se agregan a la página maestra. Cuando se agregan nuevas páginas al sitio, se pueden asociar a una página maestra. Una página que está asociada a una página maestra se denomina **Página de contenido**. De forma predeterminada, una página de contenido toma la apariencia de la página maestra. Sin embargo, cuando se crea una página maestra, puede definir partes de la página que la página de contenido puede reemplazar por su propio contenido. Estas partes se definen mediante un nuevo control introducido en ASP.NET 2,0; el control **ContentPlaceHolder** .

Una página maestra puede contener cualquier número de controles ContentPlaceHolder (o ninguno). En la página contenido, el contenido de los controles ContentPlaceHolder aparece dentro de los controles de **contenido** , otro nuevo control en ASP.net 2,0. De forma predeterminada, los controles de contenido de las páginas de contenido están vacíos para que pueda proporcionar su propio contenido. Si desea usar el contenido de la página maestra dentro de los controles de contenido, puede hacerlo tal y como verá más adelante en este módulo. El control de contenido se asigna al control ContentPlaceHolder a través del atributo ContentPlaceHolderID del control de contenido. El código siguiente asigna un control de contenido a un control ContentPlaceHolder denominado mainBody en una página maestra.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> A menudo, se oye que los usuarios describen las páginas maestras como una clase base para otras páginas. Que realmente no es true. La relación entre las páginas maestras y las páginas de contenido no es una de herencia.

La **figura 1** muestra una página maestra y una página de contenido asociada tal como aparecen en Visual Studio 2005. Puede ver el control ContentPlaceHolder en la página maestra y el control de contenido correspondiente en la página de contenido. Observe que el contenido de las páginas maestras que está fuera del ContentPlaceHolder es visible pero atenuado en la página de contenido. La página de contenido solo puede suplantar el contenido dentro de ContentPlaceHolder. El resto del contenido que procede de la página maestra es inmutable.

![Una página maestra y su página de contenido asociada](master-pages/_static/image1.jpg)

**Figura 1**: página maestra y su página de contenido asociada

## <a name="creating-a-master-page"></a>Crear una página maestra

Para crear una nueva página maestra:

1. Abra Visual Studio 2005 y cree un nuevo sitio Web.
2. Haga clic en archivo, nuevo, archivo.
3. Elija archivo maestro en el cuadro de diálogo Agregar nuevo elemento, como se muestra en la **figura 2**.
4. Haga clic en Agregar.

![Crear una nueva página maestra](master-pages/_static/image2.jpg)

**Figura 2**: creación de una nueva página maestra

Tenga en cuenta que la extensión de archivo de una página maestra es *. Master*. Esta es una de las formas en que una página maestra difiere de una página normal. La otra diferencia principal es que, en lugar de una directiva de @Page, la página maestra contiene una directiva @Master. Cambie a la vista Código fuente de la página maestra que acaba de crear y revise el código.

De forma predeterminada, una nueva página maestra tendrá un control ContentPlaceHolder. En la mayoría de los casos, tiene más sentido crear primero los elementos de página comunes y, a continuación, insertar los controles de ContentPlaceHolder en los que se desea obtener el contenido personalizado. En esos casos, los desarrolladores querrán eliminar el control ContentPlaceHolder predeterminado e insertar otros nuevos mientras se desarrolla la página. Los controles ContentPlaceHolder no se pueden cambiar de tamaño a pesar de que se muestren los controladores de tamaño. El control ContentPlaceHolder dimensiona automáticamente en función del contenido que contiene con una excepción; Si coloca un control ContentPlaceHolder dentro de un elemento de bloque, como una celda de tabla, se ajustará su tamaño según el tamaño del elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratorio 1 trabajar con páginas maestras

En este laboratorio, creará una nueva página maestra y definirá tres controles de ContentPlaceHolder. Después, creará una nueva página de contenido y reemplazará el contenido de al menos uno de los controles ContentPlaceHolder.

1. Cree una página maestra e inserte controles ContentPlaceHolder. 

    1. Cree una nueva página maestra como se describió anteriormente.
    2. Elimine el control ContentPlaceHolder predeterminado.
    3. Seleccione el control ContentPlaceHolder; para ello, haga clic en el borde superior sombreado del control y, a continuación, elimínelo presionando la tecla Supr del teclado.
    4. Inserte una nueva tabla con el *encabezado y* la plantilla de lado, tal como se muestra en la figura 3. Cambie el ancho y el alto al 90% para que toda la tabla esté visible en el diseñador.

![](master-pages/_static/image3.jpg)

**Ilustración 3**

1. Coloque el cursor en cada celda de la tabla y establezca la propiedad *valign* en *Top*.
2. En el cuadro de herramientas, inserte un control ContentPlaceHolder en la celda superior de la tabla (la celda de encabezado).
3. Al insertar este control ContentPlaceHolder, observará que el alto de fila ocupará casi toda la página como se muestra en la figura 4. No le preocupe en este momento.

![El espacio vacío está en la misma celda que el ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: el espacio vacío está en la misma celda que el ContentPlaceHolder

1. Coloque un control ContentPlaceHolder en las otras dos celdas. Una vez insertados los demás controles ContentPlaceHolder, el tamaño de las celdas de la tabla debe ser el que cabría esperar. La página debe parecerse ahora a la página que se muestra en la **figura 5**.

![Maestro con todos los controles ContentPlaceHolder. Observe que el alto de celda de la celda de encabezado es ahora lo que debe ser](master-pages/_static/image2.gif)

**Figura 5**: el patrón con todos los controles ContentPlaceHolder. Observe que el alto de celda de la celda de encabezado es ahora lo que debe ser

1. Escriba el texto que prefiera en cada uno de los tres controles de ContentPlaceHolder.
2. Guarde la página maestra como exercise1. Master.
3. Cree un nuevo formulario web y asócielo a la página maestra exercise1. Master.
4. Seleccione Archivo, nuevo, archivo en Visual Studio 2005.
5. Seleccione **Web Forms** en el cuadro de diálogo Agregar nuevo elemento.
6. Asegúrese de que la casilla seleccionar página maestra está activada como se muestra en la figura 6.

![Agregar una nueva página de contenido](master-pages/_static/image3.gif)

**Ilustración 6**: agregar una nueva página de contenido

1. Haga clic en Agregar.
2. Seleccione exercise1. Master en el cuadro de diálogo Seleccionar una página maestra como se muestra en la figura 7.
3. Haga clic en Aceptar para agregar la nueva página de contenido.

La nueva página de contenido aparece en Visual Studio con un control de contenido para cada control ContentPlaceHolder en la página maestra. De forma predeterminada, los controles de contenido están vacíos para que pueda agregar su propio contenido. Si desea que use el contenido del control ContentPlaceHolder en la página maestra, simplemente haga clic en el símbolo de etiqueta inteligente (la flecha pequeña de color negro en la esquina superior derecha del control) y elija de *forma predeterminada el contenido* de los patrones de la etiqueta inteligente, tal como se muestra en la **figura 8**. Al hacerlo, el elemento de menú cambia para *crear contenido personalizado*. Al hacer clic en él en ese punto, se quita el contenido de la página maestra, lo que le permite definir el contenido personalizado para ese control de contenido determinado.

![Establecer un control de contenido de forma predeterminada en el contenido de las páginas maestras](master-pages/_static/image4.gif)

**Figura 7**: configuración de un control de contenido de forma predeterminada en el contenido de las páginas maestras

## <a name="connecting-master-page-and-content-pages"></a>Conexión de página maestra y páginas de contenido

La asociación entre una página maestra y una página de contenido puede configurarse de una de estas cuatro maneras diferentes:

- El atributo <strong>MasterPageFile</strong> de la Directiva @Page
- Establecimiento de la propiedad **Page. MasterPageFile** en el código.
- En el **&lt;páginas&gt;** elemento del archivo de configuración de aplicaciones (Web. config en la carpeta raíz de la aplicación)
- El **&lt;páginas&gt;** elemento de un archivo de configuración de subcarpetas (Web. config en una subcarpeta)

## <a name="masterpagefile-attribute"></a>MasterPageFile (atributo)

El atributo MasterPageFile facilita la aplicación de una página maestra a una página de ASP.NET determinada. También es el método que se usa para aplicar la página maestra al activar la casilla **seleccionar página maestra** como hizo en el ejercicio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Configuración de Page. MasterPageFile en el código

Al establecer la propiedad MasterPageFile en el código, puede aplicar una página maestra determinada a su contenido en tiempo de ejecución. Esto resulta útil en casos en los que es posible que deba aplicar una página maestra específica basada en un rol de usuario o en otros criterios. La propiedad MasterPageFile debe establecerse en el método PreInit. Si se establece después del método PreInit, se producirá una excepción InvalidOperationException. La página en la que se establece esta propiedad también debe tener un control de contenido como control de nivel superior de la página. De lo contrario, se producirá una HttpException cuando se establezca la propiedad MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Usar el elemento&gt; de páginas &lt;

Puede configurar una página maestra para las páginas estableciendo el atributo masterPageFile en el elemento &lt;páginas&gt; del archivo Web. config. Al usar este método, tenga en cuenta que los archivos Web. config inferiores en la estructura de la aplicación pueden invalidar esta configuración. Cualquier atributo MasterPageFile establecido en una directiva @Page también invalidará esta configuración. El uso del elemento&gt; de páginas &lt;facilita la creación de una página maestra *maestra* que se puede invalidar si es necesario en archivos o carpetas concretos.

## <a name="properties-in-master-pages"></a>Propiedades en páginas maestras

Una página maestra puede exponer propiedades simplemente haciendo que esas propiedades sean públicas en la página maestra. Por ejemplo, el código siguiente define una propiedad denominada SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Para tener acceso a la propiedad SomeProperty desde la página de contenido, debe usar la propiedad maestra de la siguiente manera:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Anidar páginas maestras

Las páginas maestras son la solución perfecta para garantizar una apariencia común en una aplicación Web de gran tamaño. Sin embargo, no es raro que algunas partes de un sitio de gran tamaño compartan una interfaz común mientras otras partes comparten una interfaz diferente. Para abordar esa necesidad, varias páginas maestras son la solución perfecta. Sin embargo, eso todavía no soluciona el hecho de que una aplicación grande puede tener determinados componentes (como un menú, por ejemplo) que se comparten entre todas las páginas y otros componentes que se comparten solo entre determinadas secciones del sitio. Para ese tipo de situación, las páginas maestras anidadas rellenan las necesidades. Como ha visto, una página maestra normal consta de una página maestra y una página de contenido. En una situación de página maestra anidada, hay dos páginas maestras; un maestro primario y otro secundario. La página maestra secundaria es también una página de contenido y su principal es la página maestra primaria.

Este es el código de una página maestra típica:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

En un escenario maestro anidado, este sería el maestro primario. Otra página maestra usaría esta página como su página maestra y ese código tendría el aspecto siguiente:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Tenga en cuenta que, en este escenario, el maestro secundario también es una página de contenido del maestro primario. Todo el contenido del maestro secundario aparece dentro de un control de contenido que obtiene su contenido del control ContentPlaceHolder del elemento primario.

> [!NOTE]
> La compatibilidad con el diseñador no está disponible para las páginas maestras anidadas. Al desarrollar mediante patrones anidados, deberá usar la vista Código fuente.

En este vídeo se muestra un tutorial sobre el uso de páginas maestras anidadas.

![](master-pages/_static/image1.png)

[Abrir vídeo de pantalla completa](master-pages/_static/nested1.wmv)

![Seleccionar una página maestra](master-pages/_static/image4.jpg)

**Figura 8**: selección de una página maestra
