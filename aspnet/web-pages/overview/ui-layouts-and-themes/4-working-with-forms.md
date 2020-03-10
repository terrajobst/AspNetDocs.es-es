---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Trabajar con formularios HTML en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Un formulario es una sección de un documento HTML donde se colocan controles de entrada de usuario, como cuadros de texto, casillas, botones de radio y listas desplegables. Use Forms...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: c7d4802063c8610a246afe67bd15eea429f7304a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519607"
---
# <a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Trabajar con formularios HTML en sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo procesar un formulario HTML (con cuadros de texto y botones) cuando se trabaja en un sitio web de ASP.NET Web Pages (Razor).
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un formulario HTML.
> - Cómo leer los datos proporcionados por el usuario desde el formulario.
> - Cómo validar los datos proporcionados por el usuario.
> - Cómo restaurar valores de formulario después de enviar la página.
> 
> Estos son los conceptos de programación de ASP.NET que se incluyen en el artículo:
> 
> - Objeto `Request`.
> - Validación de entrada.
> - Codificación HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

## <a name="creating-a-simple-html-form"></a>Crear un formulario HTML sencillo

1. Cree un nuevo sitio Web.
2. En la carpeta raíz, cree una página web denominada *Form. cshtml* y escriba el marcado siguiente:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Inicie la página en el explorador. (En WebMatrix, en el área de trabajo **archivos** , haga clic con el botón derecho en el archivo y seleccione **iniciar en el explorador**). Se muestra un formulario sencillo con tres campos de entrada y un botón de **envío** .

    ![Captura de pantalla de un formulario con tres cuadros de texto.](4-working-with-forms/_static/image1.png)

    En este punto, si hace clic en el botón **Enviar** , no sucede nada. Para que el formulario sea útil, debe agregar código que se ejecutará en el servidor.

## <a name="reading-user-input-from-the-form"></a>Leer la entrada del usuario del formulario

Para procesar el formulario, agregue el código que lee los valores de campo enviados y realiza alguna acción con ellos. En este procedimiento se muestra cómo leer los campos y mostrar la entrada del usuario en la página. En una aplicación de producción, por lo general, se realizan cosas más interesantes con los datos proporcionados por el usuario. Lo hará en el artículo sobre cómo trabajar con bases de datos).

1. En la parte superior del archivo *Form. cshtml* , escriba el código siguiente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Cuando el usuario solicita la página por primera vez, solo se muestra el formulario vacío. El usuario (que será) rellena el formulario y, a continuación, hace clic en **Enviar**. Esto envía (envía) la entrada del usuario al servidor. De forma predeterminada, la solicitud se dirige a la misma página (es decir, *Form. cshtml*).

    Cuando se envía la página esta vez, los valores especificados se muestran justo encima del formulario:

    ![Captura de pantalla que muestra los valores que ha especificado en la página.](4-working-with-forms/_static/image2.png)

    Examine el código de la página. En primer lugar, use el método `IsPost` para determinar si la página se &#8212; está publicando, es decir, si un usuario hizo clic en el botón **Enviar** . Si se trata de una publicación, `IsPost` devuelve true. Esta es la manera estándar de ASP.NET Web Pages para determinar si está trabajando con una solicitud inicial (una solicitud GET) o un postback (una solicitud POST). (Para obtener más información sobre GET y POST, vea la barra lateral "HTTP GET y POST y la propiedad IsPost" en [Introduction to ASP.NET Web pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost)).

    A continuación, obtiene los valores que el usuario ha rellenado del objeto `Request.Form` y los coloca en las variables para más adelante. El objeto `Request.Form` contiene todos los valores que se enviaron con la página, cada uno identificado por una clave. La clave es equivalente al atributo `name` del campo de formulario que desea leer. Por ejemplo, para leer el campo `companyname` (cuadro de texto), utilice `Request.Form["companyname"]`.

    Los valores de formulario se almacenan en el objeto `Request.Form` como cadenas. Por lo tanto, cuando tenga que trabajar con un valor como un número o una fecha o algún otro tipo, tendrá que convertirlo de una cadena a ese tipo. En el ejemplo, el método `AsInt` del `Request.Form` se utiliza para convertir el valor del campo Employees (que contiene un recuento de empleados) en un entero.
2. Inicie la página en el explorador, rellene los campos de formulario y haga clic en **Enviar**. En la página se muestran los valores especificados.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codificación HTML para la apariencia y la seguridad
> 
> HTML tiene usos especiales para caracteres como `<`, `>`y `&`. Si estos caracteres especiales aparecen donde no se esperan, pueden estropear la apariencia y la funcionalidad de la Página Web. Por ejemplo, el explorador interpreta el carácter `<` (a menos que vaya seguido de un espacio) como el principio de un elemento HTML, como `<b>` o `<input ...>`. Si el explorador no reconoce el elemento, simplemente descarta la cadena que comienza con `<` hasta que llega a algo que reconoce de nuevo. Obviamente, esto puede dar lugar a una representación extraña en la página.
> 
> La codificación HTML reemplaza estos caracteres reservados por un código que los exploradores interpretan como el símbolo correcto. Por ejemplo, el carácter `<` se reemplaza por `&lt;` y el carácter `>` se reemplaza por `&gt;`. El explorador representa estas cadenas de reemplazo como los caracteres que desea ver.
> 
> Es aconsejable usar la codificación HTML siempre que se muestren cadenas (entrada) que haya recibido de un usuario. Si no lo hace, un usuario puede intentar obtener la página web para ejecutar un script malintencionado o hacer algo más que ponga en peligro la seguridad del sitio o que no sea lo que desea. (Esto es especialmente importante si toma datos proporcionados por el usuario, los almacena en un lugar y &#8212; después los muestra posteriormente, como un Comentario de blog, una revisión de usuario o algo parecido).
> 
> Para ayudar a evitar estos problemas, ASP.NET Web Pages codifica automáticamente en HTML todo el contenido de texto que se genera desde el código. Por ejemplo, al mostrar el contenido de una variable o una expresión mediante código como `@MyVar`, ASP.NET Web Pages codifica automáticamente el resultado.

## <a name="validating-user-input"></a>Validar los datos introducidos por el usuario

Los usuarios cometen errores. Pídales que rellenen un campo y que se olviden o que le pidan que escriban el número de empleados y escriban un nombre en su lugar. Para asegurarse de que un formulario se ha rellenado correctamente antes de procesarlo, valide la entrada del usuario.

Este procedimiento muestra cómo validar los tres campos de formulario para asegurarse de que el usuario no los dejó en blanco. También se comprueba que el valor de recuento de empleados es un número. Si hay errores, se mostrará un mensaje de error que indica al usuario qué valores no superaron la validación.

1. En el archivo *Form. cshtml* , reemplace el primer bloque de código por el código siguiente: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Para validar la entrada del usuario, use la aplicación auxiliar de `Validation`. Para registrar los campos obligatorios, llame a `Validation.RequireField`. Para registrar otros tipos de validación, se llama a `Validation.Add` y se especifica el campo que se va a validar y el tipo de validación que se va a realizar.

    Cuando se ejecuta la página, ASP.NET realiza toda la validación. Puede comprobar los resultados llamando a `Validation.IsValid`, que devuelve true si todo lo pasó y false si algún campo no ha superado la validación. Normalmente, se llama a `Validation.IsValid` antes de realizar cualquier procesamiento de los datos proporcionados por el usuario.
2. Actualice el elemento `<body>` agregando tres llamadas al método `Html.ValidationMessage`, de la siguiente manera:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Para mostrar los mensajes de error de validación, puede llamar a HTML.`ValidationMessage` y pásele el nombre del campo para el que desea el mensaje.
3. Ejecute la página. Deje los campos en blanco y haga clic en **Enviar**. Verá mensajes de error.

    ![Captura de pantalla que muestra mensajes de error que se muestran si la entrada del usuario no pasa la validación.](4-working-with-forms/_static/image3.jpg)
4. Agregue una cadena (por ejemplo, "ABC") al campo **recuento de empleados** y haga clic en **Enviar** de nuevo. Esta vez verá un error que indica que la cadena no está en el formato correcto, es decir, un entero.

    ![Captura de pantalla que muestra mensajes de error que se muestran si los usuarios escriben una cadena para el campo empleados.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web Pages proporciona más opciones para validar la entrada del usuario, incluida la capacidad de realizar automáticamente la validación mediante el script de cliente, de modo que los usuarios obtengan comentarios inmediatos en el explorador. Vea la sección [recursos adicionales](#Additional_Resources) más adelante para obtener más información.

## <a name="restoring-form-values-after-postbacks"></a>Restaurar valores de formulario después de postback

Al probar la página en la sección anterior, es posible que haya observado que, si se produjo un error de validación, todo lo que ha escrito (no solo los datos no válidos) ha desaparecido y tenía que volver a escribir los valores para todos los campos. Esto ilustra un punto importante: cuando se envía una página, se procesa y, a continuación, se vuelve a presentar la página, la página se vuelve a crear desde cero. Como ha visto, esto significa que los valores que estaban en la página cuando se enviaron se pierden.

Sin embargo, puede solucionarlo fácilmente. Tiene acceso a los valores que se enviaron (en el objeto `Request.Form`, por lo que puede volver a rellenar los valores en los campos de formulario cuando se represente la página.

1. En el archivo *Form. cshtml* , reemplace los `value` atributos de los elementos `<input>` mediante el atributo `value`: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    El atributo `value` de los elementos de la `<input>` se ha establecido para leer dinámicamente el valor del campo fuera del objeto `Request.Form`. La primera vez que se solicita la página, los valores del objeto `Request.Form` están vacíos. Esto es correcto, ya que el formulario está en blanco.
2. Inicie la página en el explorador, rellene los campos de formulario o déjelos en blanco y haga clic en **submit (enviar**). Se muestra una página que muestra los valores enviados.

    ![formularios-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [1.001 formas de obtener información de los usuarios Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Usar formularios y procesar los datos proporcionados por el usuario](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validar la entrada del usuario en los sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Usar autocompletar en formularios HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
