---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Uso de imágenes en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se muestra cómo agregar, mostrar y manipular imágenes (cambiar el tamaño, voltear y agregar marcas de agua) en el sitio Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 53514b3c314fc182a43c82974ffcfa8158a636a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512881"
---
# <a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Trabajar con imágenes en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se muestra cómo agregar, mostrar y manipular imágenes (cambiar el tamaño, voltear y agregar marcas de agua) en un sitio web de ASP.NET Web Pages (Razor).
> 
> Temas que se abordarán:
> 
> - Cómo agregar una imagen a una página dinámicamente.
> - Cómo permitir que los usuarios carguen una imagen.
> - Cómo cambiar el tamaño de una imagen.
> - Cómo voltear o girar una imagen.
> - Cómo agregar una marca de agua a una imagen.
> - Cómo usar una imagen como marca de agua.
> 
> Estas son las características de programación de ASP.NET que se introdujeron en el artículo:
> 
> - Aplicación auxiliar de `WebImage`.
> - El objeto `Path`, que proporciona métodos que permiten manipular nombres de archivo y ruta de acceso.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con WebMatrix 3.

<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Agregar una imagen a una página web dinámicamente

Puede Agregar imágenes al sitio web y a páginas individuales mientras está desarrollando el sitio Web. También puede permitir que los usuarios carguen imágenes, lo que puede resultar útil para tareas como permitirles agregar una foto de perfil.

Si ya hay una imagen disponible en el sitio y solo desea mostrarla en una página, use un elemento de `<img>` HTML como este:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

Sin embargo, a veces es necesario poder mostrar imágenes de forma dinámica, &#8212; es decir, no se sabe qué imagen se debe mostrar hasta que la página se está ejecutando.

En el procedimiento de esta sección se muestra cómo mostrar una imagen sobre la marcha en la que los usuarios especifican el nombre del archivo de imagen en una lista de nombres de imagen. Seleccionan el nombre de la imagen en una lista desplegable y, cuando envían la página, se muestra la imagen que seleccionó.

![impresión](9-working-with-images/_static/image1.jpg "ch9images-1. jpg")

1. En WebMatrix, cree un nuevo sitio Web.
2. Agregue una nueva página denominada *DynamicImage. cshtml*.
3. En la carpeta raíz del sitio web, agregue una nueva carpeta y asígnele el nombre *imágenes*.
4. Agregue cuatro imágenes a la carpeta *images* que acaba de crear. (Las imágenes que tenga resultan útiles, pero deben ajustarse a una página). Cambie el nombre de las imágenes *Photo1. jpg*, *Photo2. jpg*, *Photo3. jpg*y *Photo4. jpg*. (No usará *Photo4. jpg* en este procedimiento, pero lo usará más adelante en el artículo).
5. Compruebe que las cuatro imágenes no estén marcadas como de solo lectura.
6. Reemplace el contenido existente en la página por lo siguiente:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    El cuerpo de la página tiene una lista desplegable (un elemento `<select>`) denominada `photoChoice`. La lista tiene tres opciones y el atributo `value` de cada opción de lista tiene el nombre de una de las imágenes que se colocan en la carpeta *imágenes* . En esencia, la lista permite al usuario seleccionar un nombre descriptivo como &quot;foto 1&quot;y, a continuación, pasa el nombre del archivo *. jpg* cuando se envía la página.

    En el código, puede obtener la selección del usuario (es decir, el nombre del archivo de imagen) de la lista leyendo `Request["photoChoice"]`. En primer lugar, verá si hay una selección en absoluto. Si existe, se crea una ruta de acceso para la imagen que consta del nombre de la carpeta para las imágenes y el nombre del archivo de imagen del usuario. (Si intentó crear una ruta de acceso pero no había nada en `Request["photoChoice"]`, obtendría un error). Esto da como resultado una ruta de acceso relativa como esta:

    *images/Photo1. jpg*

    La ruta de acceso se almacena en la variable denominada `imagePath` que necesitará más adelante en la página.

    En el cuerpo, hay también un elemento `<img>` que se usa para mostrar la imagen que el usuario ha seleccionado. El atributo `src` no se establece en un nombre de archivo o una dirección URL, como haría para mostrar un elemento estático. En su lugar, se establece en `@imagePath`, lo que significa que obtiene su valor de la ruta de acceso establecida en el código.

    La primera vez que se ejecuta la página, sin embargo, no hay ninguna imagen que mostrar, porque el usuario no ha seleccionado nada. Normalmente, esto significaría que el atributo `src`ría estar vacío y la imagen se mostraría como una&quot; roja &quot;x (o cualquier otra cosa que se represente en el explorador cuando no pueda encontrar una imagen). Para evitarlo, coloque el elemento `<img>` en un bloque de `if` que prueba para ver si la variable `imagePath` tiene cualquier elemento. Si el usuario realizó una selección, `imagePath` contiene la ruta de acceso. Si el usuario no ha seleccionado una imagen o si es la primera vez que se muestra la página, el elemento `<img>` no se representa incluso.
7. Guarde el archivo y ejecute la página en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla).
8. Seleccione una imagen en la lista desplegable y, a continuación, haga clic en **imagen de ejemplo**. Asegúrese de que ve imágenes diferentes para distintas opciones.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Carga de una imagen

En el ejemplo anterior se mostró cómo mostrar una imagen dinámicamente, pero solo funcionaba con imágenes que ya estaban en el sitio Web. Este procedimiento muestra cómo permitir que los usuarios carguen una imagen, que se muestra en la página. En ASP.NET, puede manipular imágenes sobre la marcha con la aplicación auxiliar de `WebImage`, que tiene métodos que permiten crear, manipular y guardar imágenes. La aplicación auxiliar de `WebImage` admite todos los tipos de archivo de imagen web comunes, incluidos *. jpg*, *. png*y *. bmp*. En este artículo, usará imágenes *. jpg* , pero puede usar cualquiera de los tipos de imagen.

![impresión](9-working-with-images/_static/image2.jpg "ch9images-2. jpg")

1. Agregue una nueva página y asígnele el nombre *UploadImage. cshtml*.
2. Reemplace el contenido existente en la página por lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    El cuerpo del texto tiene un elemento `<input type="file">`, que permite a los usuarios seleccionar un archivo para cargarlo. Cuando hacen clic en **Enviar**, el archivo que ha seleccionado se envía junto con el formulario.

    Para obtener la imagen cargada, use la aplicación auxiliar `WebImage`, que tiene todo tipo de métodos útiles para trabajar con imágenes. En concreto, se usa `WebImage.GetImageFromRequest` para obtener la imagen cargada (si existe) y almacenarla en una variable denominada `photo`.

    Una gran parte del trabajo en este ejemplo implica obtener y establecer nombres de archivo y ruta de acceso. El problema es que desea obtener el nombre (y solo el nombre) de la imagen que ha cargado el usuario y, a continuación, crear una nueva ruta de acceso para la ubicación en la que va a almacenar la imagen. Dado que es posible que los usuarios carguen varias imágenes que tengan el mismo nombre, utilice un poco de código adicional para crear nombres únicos y asegurarse de que los usuarios no sobrescribirán las imágenes existentes.

    Si una imagen se ha cargado realmente (la prueba `if (photo != null)`), obtendrá el nombre de la imagen de la propiedad `FileName` de la imagen. Cuando el usuario carga la imagen, `FileName` contiene el nombre original del usuario, que incluye la ruta de acceso del equipo del usuario. Podría tener este aspecto:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    No desea toda la información de la ruta de &#8212; acceso, aunque solo desea el nombre de archivo real (*SamplePhoto1. jpg*). Puede quitar solo el archivo de una ruta de acceso mediante el método `Path.GetFileName`, de la siguiente manera:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Después, puede crear un nuevo nombre de archivo único agregando un GUID al nombre original. (Para obtener más información acerca de los GUID, consulte Acerca de los [GUID](#SB_AboutGUIDs) más adelante en este artículo). A continuación, se crea una ruta de acceso completa que se puede usar para guardar la imagen. La ruta de acceso de guardado se compone del nuevo nombre de archivo, la carpeta (imágenes) y la ubicación del sitio web actual.

    > [!NOTE]
    > Para que el código guarde los archivos en la carpeta *images* , la aplicación necesita permisos de lectura y escritura para esa carpeta. En el equipo de desarrollo, esto no suele ser un problema. Sin embargo, al publicar el sitio en el servidor Web de un proveedor de hospedaje, puede que tenga que establecer esos permisos explícitamente. Si ejecuta este código en el servidor de un proveedor de hospedaje y obtiene errores, consulte con el proveedor de hospedaje para averiguar cómo establecer esos permisos.

    Por último, se pasa la ruta de acceso de guardado al método `Save` de la aplicación auxiliar de `WebImage`. Esto almacena la imagen cargada con el nuevo nombre. El método Save tiene el siguiente aspecto: `photo.Save(@"~\" + imagePath)`. La ruta de acceso completa se anexa a `@"~\"`, que es la ubicación del sitio web actual. (Para obtener información sobre el operador `~`, consulte [Introduction to ASP.net web Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths)).

    Como en el ejemplo anterior, el cuerpo de la página contiene un elemento `<img>` para mostrar la imagen. Si se ha establecido `imagePath`, se representará el elemento `<img>` y su atributo `src` se establecerá en el valor `imagePath`.
3. Ejecute la página en un explorador.
4. Cargue una imagen y asegúrese de que se muestra en la página.
5. En el sitio, abra la carpeta *images* . Verá que se ha agregado un nuevo archivo cuyo nombre de archivo tiene un aspecto similar al siguiente: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_fotophoto. png*

    Esta es la imagen que ha cargado con un GUID con el prefijo del nombre. (Su propio archivo tendrá un GUID diferente y, probablemente, se asignará un nombre distinto al de *foto. png*).

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Acerca de los GUID
> 
> Un GUID (identificador único global) es un identificador que normalmente se representa en un formato similar al siguiente: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Los números y letras (de A-F) se diferencian en cada GUID, pero siguen el patrón de uso de grupos de 8-4-4-4-12 caracteres. (Técnicamente, un GUID es un número de 16 bytes/128 bits). Cuando necesite un GUID, puede llamar a código especializado que genere un GUID automáticamente. La idea subyacente a los GUID es que entre el tamaño enorme del número (3,4 x 10<sup>38</sup>) y el algoritmo para generarlo, se garantiza prácticamente que el número resultante es uno de un tipo. Por lo tanto, los GUID son una buena manera de generar nombres para las cosas cuando se debe garantizar que no se utilizará el mismo nombre dos veces. Por supuesto, el inconveniente es que los GUID no son especialmente descriptivos, por lo que se suelen usar cuando el nombre se usa solo en el código.

<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Cambiar el tamaño de una imagen

Si el sitio web acepta imágenes de un usuario, es posible que desee cambiar el tamaño de las imágenes antes de mostrarlas o guardarlas. Puede usar de nuevo la aplicación auxiliar `WebImage` para ello.

En este procedimiento se muestra cómo cambiar el tamaño de una imagen cargada para crear una miniatura y, a continuación, guardar la imagen en miniatura y la imagen original en el sitio Web. La miniatura de la página se muestra y se usa un hipervínculo para redirigir a los usuarios a la imagen de tamaño completo.

![impresión](9-working-with-images/_static/image3.jpg "ch9images-3. jpg")

1. Agregue una nueva página denominada *thumbnail. cshtml*.
2. En la carpeta *images* , cree una subcarpeta denominada *Thumbs*.
3. Reemplace el contenido existente en la página por lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Este código es similar al código del ejemplo anterior. La diferencia es que este código guarda la imagen dos veces, una vez por lo general y una vez después de crear una copia en miniatura de la imagen. En primer lugar, se obtiene la imagen cargada y se guarda en la carpeta *images* . A continuación, cree una nueva ruta de acceso para la imagen en miniatura. Para crear realmente la miniatura, llame al método `Resize` del ayudante `WebImage` para crear una imagen de 60 píxeles por 60 píxeles. En el ejemplo se muestra cómo se conserva la relación de aspecto y cómo se puede evitar que la imagen se amplíe (en caso de que el nuevo tamaño haga que la imagen sea más grande). La imagen cuyo tamaño se ha cambiado se guarda en la subcarpeta *Thumbs* .

    Al final del marcado, se usa el mismo elemento `<img>` con el atributo de `src` dinámico que se ha visto en los ejemplos anteriores para mostrar condicionalmente la imagen. En este caso, se muestra la miniatura. También se usa un elemento `<a>` para crear un hipervínculo a la versión grande de la imagen. Al igual que con el atributo `src` del elemento `<img>`, se establece dinámicamente el atributo `href` del elemento `<a>` en el `imagePath`. Para asegurarse de que la ruta de acceso puede funcionar como dirección URL, pase `imagePath` al método `Html.AttributeEncode`, que convierte los caracteres reservados en la ruta de acceso en caracteres que son correctos en una dirección URL.
4. Ejecute la página en un explorador.
5. Cargue una foto y compruebe que se muestra la miniatura.
6. Haga clic en la miniatura para ver la imagen a tamaño completo.
7. En *imágenes* e *imágenes/miniaturas*, tenga en cuenta que se han agregado nuevos archivos.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Rotación y volteo de una imagen

La aplicación auxiliar de `WebImage` también le permite voltear y girar imágenes. Este procedimiento muestra cómo obtener una imagen del servidor, voltear la imagen hacia abajo (verticalmente), guardarla y, a continuación, Mostrar la imagen volteada en la página. En este ejemplo, está usando simplemente un archivo que ya existe en el servidor (*Photo2. jpg*). En una aplicación real, probablemente podría voltear una imagen cuyo nombre obtiene dinámicamente, como hizo en ejemplos anteriores.

![impresión](9-working-with-images/_static/image4.jpg "ch9images-4. jpg")

1. Agregue una nueva página denominada *FlipImage. cshtml*.
2. Reemplace el contenido existente en la página por lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    El código usa la aplicación auxiliar de `WebImage` para obtener una imagen del servidor. Puede crear la ruta de acceso a la imagen con la misma técnica que usó en ejemplos anteriores para guardar imágenes y pasar esa ruta de acceso al crear una imagen mediante `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Si se encuentra una imagen, cree una nueva ruta de acceso y nombre de archivo, como hizo en ejemplos anteriores. Para voltear la imagen, llame al método `FlipVertical` y, a continuación, vuelva a guardar la imagen.

    La imagen se muestra de nuevo en la página mediante el elemento `<img>` con el atributo `src` establecido en `imagePath`.
3. Ejecute la página en un explorador. La imagen de *Photo2. jpg* se muestra al revés.
4. Actualice la página o vuelva a solicitar la página para ver que la imagen se voltea hacia la derecha hacia arriba.

Para girar una imagen, se usa el mismo código, salvo que, en lugar de llamar al `FlipVertical` o `FlipHorizontal`, se llama a `RotateLeft` o `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Agregar una marca de agua a una imagen

Al agregar imágenes al sitio web, puede que desee agregar una marca de agua a la imagen antes de guardarla o mostrarla en una página. Las personas suelen usar marcas de agua para agregar información de copyright a una imagen o para anunciar el nombre de su empresa.

![impresión](9-working-with-images/_static/image5.jpg "ch9images-5. jpg")

1. Agregue una nueva página denominada *marca de agua. cshtml*.
2. Reemplace el contenido existente en la página por lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Este código es como el código de la página *FlipImage. cshtml* anterior (aunque esta vez usa el archivo *Photo3. jpg* ). Para agregar la marca de agua, llame al método `AddTextWatermark` del ayudante `WebImage` antes de guardar la imagen. En la llamada a `AddTextWatermark`, pase el texto &quot;la marca de agua&quot;, establezca el color de la fuente en amarillo y establezca la familia de fuentes en Arial. (Aunque no se muestra aquí, la aplicación auxiliar de `WebImage` también le permite especificar la opacidad, la familia de fuentes y el tamaño de fuente, y la posición del texto de marca de agua). Al guardar la imagen, no debe ser de solo lectura.

    Como ha visto antes, la imagen se muestra en la página mediante el elemento `<img>` con el atributo src establecido en `@imagePath`.
3. Ejecute la página en un explorador. Observe el texto "mi marca de agua" en la esquina inferior derecha de la imagen.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Usar una imagen como una marca de agua

En lugar de utilizar texto para una marca de agua, puede usar otra imagen. A veces, las personas usan imágenes como el logotipo de la empresa como una marca de agua, o bien usan una imagen de marca de agua en lugar de texto para la información de copyright.

![impresión](9-working-with-images/_static/image6.jpg "ch9images-6. jpg")

1. Agregue una nueva página denominada *ImageWatermark. cshtml*.
2. Agregue una imagen a la carpeta *images* que puede usar como logotipo y cambie el nombre de la imagen *MyCompanyLogo. jpg*. Esta imagen debe ser una imagen que puede ver claramente cuando se establece en 80 píxeles de ancho y 20 píxeles de alto.
3. Reemplace el contenido existente en la página por lo siguiente: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Esta es otra variación del código de los ejemplos anteriores. En este caso, se llama a `AddImageWatermark` para agregar la imagen de marca de agua a la imagen de destino (*Photo3. jpg*) antes de guardar la imagen. Cuando se llama a `AddImageWatermark`, se establece su ancho en 80 píxeles y el alto en 20 píxeles. La imagen *MyCompanyLogo. jpg* se alinea horizontalmente en el centro y verticalmente en la parte inferior de la imagen de destino. La opacidad se establece en 100% y el relleno se establece en 10 píxeles. Si la imagen de marca de agua es mayor que la imagen de destino, no ocurrirá nada. Si la imagen de marca de agua es mayor que la imagen de destino y establece el relleno de la marca de agua de la imagen en cero, se omite la marca de agua.

    Como antes, se muestra la imagen mediante el elemento `<img>` y un atributo de `src` dinámico.
4. Ejecute la página en un explorador. Observe que la imagen de marca de agua aparece en la parte inferior de la imagen principal.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Trabajar con archivos en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202896)

[Introducción a la programación de ASP.NET Web Pages mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
