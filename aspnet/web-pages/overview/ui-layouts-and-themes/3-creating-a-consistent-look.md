---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Creación de un diseño coherente en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Para que resulte más eficaz crear páginas web para el sitio, puede crear bloques de contenido reutilizables (como encabezados y pies de página) para el sitio web y c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 3f63ce68ae4c13970ac0df196167ace0b22b592c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509101"
---
# <a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Creación de un diseño coherente en sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo puede usar las páginas de diseño de un sitio web de ASP.NET Web Pages (Razor) para crear bloques de contenido reutilizables (como encabezados y pies de página) y para crear una apariencia coherente para todas las páginas del sitio.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear bloques de contenido reutilizables, como encabezados y pies de página.
> - Cómo crear una apariencia coherente para todas las páginas del sitio mediante un diseño.
> - Cómo pasar datos en tiempo de ejecución a una página de diseño.
> 
> Estas son las características de ASP.NET presentadas en el artículo:
> 
> - Bloques de contenido, que son archivos que contienen contenido con formato HTML que se va a insertar en varias páginas.
> - Páginas de diseño, que son páginas que contienen contenido con formato HTML que pueden compartir las páginas del sitio Web.
> - Los métodos `RenderPage`, `RenderBody`y `RenderSection`, que indican a ASP.NET dónde insertar los elementos de la página.
> - `PageData` Diccionario que permite compartir datos entre bloques de contenido y páginas de diseño.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

## <a name="about-layout-pages"></a>Acerca de las páginas de diseño

Muchos sitios web tienen contenido que se muestra en todas las páginas, como un encabezado y un pie de página, o un cuadro que indica a los usuarios que han iniciado sesión. ASP.NET permite crear un archivo independiente con un bloque de contenido que puede contener texto, marcado y código, como una página web normal. Después, puede insertar el bloque de contenido en otras páginas del sitio en el que desea que aparezca la información. De este modo, no tendrá que copiar y pegar el mismo contenido en todas las páginas. Crear contenido común como este también facilita la actualización del sitio. Si necesita cambiar el contenido, basta con actualizar un solo archivo y, a continuación, los cambios se reflejarán en cualquier lugar en el que se haya insertado el contenido.

En el diagrama siguiente se muestra cómo funcionan los bloques de contenido. Cuando un explorador solicita una página del servidor Web, ASP.NET inserta los bloques de contenido en el punto en el que se llama al método de `RenderPage` en la Página principal. La página finalizada (combinada) se envía al explorador.

![Diagrama conceptual en el que se muestra cómo el método RenderPage inserta una página a la que se hace referencia en la página actual.](3-creating-a-consistent-look/_static/image1.jpg)

En este procedimiento, creará una página que hace referencia a dos bloques de contenido (un encabezado y un pie de página) que se encuentran en archivos independientes. Puede utilizar estos mismos bloques de contenido en cualquier página de su sitio. Cuando haya terminado, recibirá una página similar a la siguiente:

![Captura de pantalla que muestra una página en el explorador que es el resultado de la ejecución de una página que incluye llamadas al método RenderPage.](3-creating-a-consistent-look/_static/image2.png)

1. En la carpeta raíz de su sitio web, cree un archivo denominado *index. cshtml*.
2. Reemplace el marcado existente por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. En la carpeta raíz, cree una carpeta denominada *Shared*.

    > [!NOTE]
    > Es habitual almacenar los archivos que se comparten entre las páginas web en una carpeta denominada *Shared*.
4. En la carpeta *compartida* , cree un archivo denominado *\_header. cshtml*.
5. Reemplace el contenido existente por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Observe que el nombre de archivo es *\_header. cshtml*, con un carácter de subrayado (\_) como prefijo. ASP.NET no enviará una página al explorador si su nombre comienza con un carácter de subrayado. Esto impide que los usuarios soliciten estas páginas (involuntariamente o de otro modo) directamente. Es aconsejable usar un carácter de subrayado para asignar nombres a las páginas que tengan bloques de contenido en ellas, ya que en realidad no desea que los usuarios &#8212; puedan solicitar estas páginas y que se inserten de forma estricta en otras páginas.
6. En la carpeta *compartida* , cree un archivo denominado *\_footer. cshtml* y reemplace el contenido por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. En la página *index. cshtml* , agregue dos llamadas al método `RenderPage`, como se muestra aquí:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Esto muestra cómo insertar un bloque de contenido en una página web. Llame al método `RenderPage` y pásele el nombre del archivo cuyo contenido desea insertar en ese momento. Aquí va a insertar el contenido de los archivos *\_header. cshtml* y *\_footer. cshtml* en el archivo *index. cshtml* .
8. Ejecute la página *index. cshtml* en un explorador. (En WebMatrix, en el área de trabajo **archivos** , haga clic con el botón derecho en el archivo y seleccione **iniciar en el explorador**).
9. En el explorador, vea el origen de la página. (Por ejemplo, en Internet Explorer, haga clic con el botón secundario en la página y, a continuación, haga clic en **Ver código fuente**).

    Esto le permite ver el marcado de la página web que se envía al explorador, que combina el marcado de la página de índice con los bloques de contenido. En el ejemplo siguiente se muestra el origen de la página que se representa para *index. cshtml*. Las llamadas a `RenderPage` que insertó en *index. cshtml* se han reemplazado por el contenido real de los archivos de encabezado y de pie de página.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Crear una apariencia coherente mediante páginas de diseño

Hasta ahora ha visto que es fácil incluir el mismo contenido en varias páginas. Un enfoque más estructurado para crear una apariencia coherente para un sitio es usar páginas de diseño. Una página de diseño define la estructura de una página web, pero no contiene contenido real. Después de crear una página de diseño, puede crear páginas web que contengan el contenido y vincularlas a la página de diseño. Cuando se muestran estas páginas, se les aplicará formato según la página de diseño. (En este sentido, una página de diseño actúa como un tipo de plantilla para el contenido que se define en otras páginas).

La página de diseño es igual que cualquier página HTML, salvo que contiene una llamada al método `RenderBody`. La posición del método `RenderBody` en la página de diseño determina dónde se incluirá la información de la página de contenido.

En el diagrama siguiente se muestra cómo se combinan las páginas de contenido y de diseño en tiempo de ejecución para generar la página web finalizada. El explorador solicita una página de contenido. La página contenido contiene código que especifica la página de diseño que se va a utilizar para la estructura de la página. En la página de diseño, el contenido se inserta en el punto en el que se llama al método de `RenderBody`. Los bloques de contenido también se pueden insertar en la página de diseño llamando al método `RenderPage`, como hizo en la sección anterior. Cuando se completa la página web, se envía al explorador.

![Captura de pantalla que muestra una página en el explorador que es el resultado de la ejecución de una página que incluye llamadas al método RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

En el siguiente procedimiento se muestra cómo crear una página de diseño y vincular páginas de contenido a ella.

1. En la carpeta *compartida* de su sitio web, cree un archivo denominado *\_Layout1. cshtml*.
2. Reemplace el contenido existente por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Use el método `RenderPage` en una página de diseño para insertar bloques de contenido. Una página de diseño solo puede contener una llamada al método `RenderBody`.
3. En la carpeta *compartida* , cree un archivo denominado *\_Header2. cshtml* y reemplace el contenido existente por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. En la carpeta raíz, cree una nueva carpeta y asígnele el nombre *estilos*.
5. En la carpeta *styles* , cree un archivo denominado *site. CSS* y agregue las siguientes definiciones de estilo:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Estas definiciones de estilo solo están aquí para mostrar cómo se pueden usar las hojas de estilos con las páginas de diseño. Si lo desea, puede definir sus propios estilos para estos elementos.
6. En la carpeta raíz, cree un archivo denominado *Content1. cshtml* y reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Se trata de una página que utilizará una página de diseño. El bloque de código en la parte superior de la página indica qué página de diseño se va a usar para dar formato a este contenido.
7. Ejecute *Content1. cshtml* en un explorador. La página representada usa el formato y la hoja de estilos definidos en *\_Layout1. cshtml* y el texto (contenido) definido en *Content1. cshtml*.

    ![[imagen]](3-creating-a-consistent-look/_static/image4.png)

    Puede repetir el paso 6 para crear páginas de contenido adicionales que pueden compartir la misma página de diseño.

    > [!NOTE]
    > Puede configurar el sitio para que pueda usar automáticamente la misma página de diseño para todas las páginas de contenido de una carpeta. Para obtener más información, consulte [personalizar el comportamiento de todo el sitio para ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Diseñar páginas de diseño que tengan varias secciones de contenido

Una página de contenido puede tener varias secciones, lo que resulta útil si desea usar diseños que tengan varias áreas con contenido reemplazable. En la página contenido, asigne un nombre único a cada sección. (La sección predeterminada se deja sin nombre). En la página diseño, agregue un método `RenderBody` para especificar dónde debe aparecer la sección sin nombre (valor predeterminado). A continuación, agregue métodos de `RenderSection` independientes para representar secciones con nombre de forma individual.

En el diagrama siguiente se muestra cómo ASP.NET controla el contenido dividido en varias secciones. Cada sección con nombre se encuentra en un bloque de sección de la página de contenido. (Se denominan `Header` y `List` en el ejemplo). La sección inserta contenido de Framework en la página de diseño en el punto en el que se llama al método `RenderSection`. La sección sin nombre (valor predeterminado) se inserta en el punto en el que se llama al método `RenderBody`, como se vio anteriormente.

![Diagrama conceptual en el que se muestra cómo el método RenderSection inserta las secciones References en la página actual.](3-creating-a-consistent-look/_static/image5.jpg)

Este procedimiento muestra cómo crear una página de contenido que tiene varias secciones de contenido y cómo representarla mediante una página de diseño que admite varias secciones de contenido.

1. En la carpeta *compartida* , cree un archivo denominado *\_Layout2. cshtml*.
2. Reemplace el contenido existente por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    El método `RenderSection` se usa para representar las secciones de encabezado y lista.
3. En la carpeta raíz, cree un archivo denominado *Content2. cshtml* y reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Esta página de contenido contiene un bloque de código en la parte superior de la página. Cada sección con nombre se encuentra en un bloque de sección. El resto de la página contiene la sección de contenido predeterminado (sin nombre).
4. Ejecute *Content2. cshtml* en un explorador.

    ![Captura de pantalla que muestra una página en el explorador que es el resultado de la ejecución de una página que incluye llamadas al método RenderSection.](3-creating-a-consistent-look/_static/image6.png)

## <a name="making-content-sections-optional"></a>Convertir las secciones de contenido en opcionales

Normalmente, las secciones que se crean en una página de contenido tienen que coincidir con las que se definen en la página de diseño. Puede obtener errores si se produce alguna de las siguientes situaciones:

- La página contenido contiene una sección que no tiene ninguna sección correspondiente en la página de diseño.
- La página de diseño contiene una sección para la que no hay contenido.
- La página de diseño incluye llamadas a métodos que intentan representar la misma sección más de una vez.

Sin embargo, puede invalidar este comportamiento para una sección con nombre declarando la sección como opcional en la página de diseño. Esto le permite definir varias páginas de contenido que pueden compartir una página de diseño pero que pueden o no tener contenido para una sección específica.

1. Abra *Content2. cshtml* y quite la siguiente sección:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Guarde la página y ejecútela en un explorador. Aparece un mensaje de error, porque la página de contenido no proporciona contenido para una sección definida en la página de diseño, es decir, la sección de encabezado.

    ![Captura de pantalla que muestra el error que se produce si se ejecuta una página que llama al método RenderSection, pero no se proporciona la sección correspondiente.](3-creating-a-consistent-look/_static/image7.png)
3. En la carpeta *compartida* , abra la página *\_Layout2. cshtml* y reemplace esta línea:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    por el siguiente código:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Como alternativa, podría reemplazar la línea anterior del código por el siguiente bloque de código, lo que produce los mismos resultados:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Vuelva a ejecutar la página *Content2. cshtml* en un explorador. (Si sigue teniendo esta página abierta en el explorador, solo puede actualizarla). Esta vez, la página se muestra sin errores, aunque no tenga ningún encabezado.

## <a name="passing-data-to-layout-pages"></a>Pasar datos a páginas de diseño

Es posible que tenga datos definidos en la página de contenido a los que necesita hacer referencia en una página de diseño. Si es así, debe pasar los datos de la página de contenido a la página de diseño. Por ejemplo, puede que desee mostrar el estado de inicio de sesión de un usuario, o puede que desee mostrar u ocultar áreas de contenido en función de los datos proporcionados por el usuario.

Para pasar datos de una página de contenido a una página de diseño, puede colocar valores en la propiedad `PageData` de la página de contenido. La propiedad `PageData` es una colección de pares nombre-valor que contienen los datos que se van a pasar de una página a otra. En la página diseño, puede leer los valores de la propiedad `PageData`.

Este es otro diagrama. Esta muestra cómo ASP.NET puede usar la propiedad `PageData` para pasar valores de una página de contenido a la página de diseño. Cuando ASP.NET comienza a compilar la página web, crea la colección de `PageData`. En la página contenido, escriba el código para colocar los datos en la colección de `PageData`. También se puede tener acceso a los valores de la colección `PageData` en otras secciones de la página de contenido o de bloques de contenido adicionales.

![Diagrama conceptual que muestra cómo una página de contenido puede rellenar un diccionario de PageData y pasar esa información a la página de diseño.](3-creating-a-consistent-look/_static/image8.jpg)

En el procedimiento siguiente se muestra cómo pasar datos de una página de contenido a una página de diseño. Cuando se ejecuta la página, muestra un botón que permite al usuario ocultar o mostrar una lista definida en la página de diseño. Cuando los usuarios hacen clic en el botón, establece un valor true/false (booleano) en la propiedad `PageData`. La página de diseño lee ese valor y, si es false, oculta la lista. El valor también se utiliza en la página contenido para determinar si se debe mostrar el botón **ocultar lista** o el botón **Mostrar lista** .

![[imagen]](3-creating-a-consistent-look/_static/image9.jpg)

1. En la carpeta raíz, cree un archivo denominado *Content3. cshtml* y reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    El código almacena dos fragmentos de datos en la propiedad &#8212; `PageData` el título de la página web y true o false para especificar si se va a mostrar una lista.

    Tenga en cuenta que ASP.NET le permite colocar el marcado HTML en la página condicionalmente mediante un bloque de código. Por ejemplo, el bloque `if/else` del cuerpo de la página determina el formulario que se va a mostrar dependiendo de si `PageData["ShowList"]` está establecido en true.
2. En la carpeta *compartida* , cree un archivo denominado *\_Layout3. cshtml* y reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    La página de diseño incluye una expresión en el elemento `<title>` que obtiene el valor de título de la propiedad `PageData`. También utiliza el valor `ShowList` de la propiedad `PageData` para determinar si se va a mostrar el bloque de contenido de la lista.
3. En la carpeta *compartida* , cree un archivo denominado *\_List. cshtml* y reemplace el contenido existente por lo siguiente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Ejecute la página *Content3. cshtml* en un explorador. La página se muestra con la lista visible en el lado izquierdo de la página y un botón **ocultar lista** en la parte inferior.

    ![Captura de pantalla que muestra la página que incluye la lista y un botón que dice "ocultar lista".](3-creating-a-consistent-look/_static/image10.png)
5. Haga clic en **ocultar lista**. La lista desaparece y el botón cambia a **Mostrar lista**.

    ![Captura de pantalla que muestra la página que no incluye la lista y un botón que indica "Mostrar lista".](3-creating-a-consistent-look/_static/image11.png)
6. Haga clic en el botón **Mostrar lista** y se volverá a mostrar la lista.

## <a name="additional-resources"></a>Recursos adicionales

[Personalizar el comportamiento de todo el sitio para ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906)
