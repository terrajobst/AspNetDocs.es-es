---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introducción a la programación web de ASP.NET mediante la sintaxis de Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: En este apéndice se ofrece información general sobre la programación con páginas Web ASP.NET en Visual Basic, mediante el sintaxis Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422665"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introducción a la programación web de ASP.NET mediante la sintaxis Razor (Visual Basic)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se proporciona información general sobre la programación con ASP.NET Web Pages mediante el sintaxis Razor y Visual Basic. ASP.NET es la tecnología de Microsoft para ejecutar páginas web dinámicas en servidores Web.
> 
> **Lo que aprenderá**:
> 
> - Las 8 mejores sugerencias de programación para empezar a programar ASP.NET Web Pages mediante sintaxis Razor.
> - Conceptos de programación básicos que necesitará.
> - Qué es el código del servidor de ASP.NET y el sintaxis Razor.
>   
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.

La mayoría de los ejemplos de uso de C#ASP.NET Web Pages con sintaxis Razor usar. Pero el sintaxis Razor también admite Visual Basic. Para programar una página web de ASP.NET en Visual Basic, cree una página web con una extensión de nombre de archivo *. vbhtml* y, a continuación, agregue Visual Basic código. En este artículo se proporciona información general sobre cómo trabajar con el lenguaje de Visual Basic y la sintaxis para crear páginas web de ASP.NET.

> [!NOTE]
> Las plantillas de sitio Web predeterminadas para Microsoft WebMatrix (**panadería**, **Galería fotográfica**y **sitio de inicio**, etc C# .) están disponibles en y Visual Basic versiones. Puede instalar las plantillas de Visual Basic como paquetes NuGet. Las plantillas de sitio web se instalan en la carpeta raíz de su sitio en una carpeta denominada *plantillas de Microsoft*.

## <a name="the-top-8-programming-tips"></a>Las 8 mejores sugerencias de programación

En esta sección se enumeran algunas sugerencias que es absolutamente necesario saber a medida que empieza a escribir código de servidor de ASP.NET mediante el sintaxis Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Agregue código a una página mediante el carácter @.

El carácter `@` inicia las expresiones insertadas, los bloques de una sola instrucción y los bloques de varias instrucciones:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Resultado mostrado en un explorador:

![Razor: img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **Codificación HTML**
> 
> Al mostrar el contenido en una página mediante el carácter `@`, como en los ejemplos anteriores, ASP.NET codifica el resultado en HTML. Esto reemplaza los caracteres HTML reservados (como `<` y `>` y `&`) con códigos que permiten mostrar los caracteres como caracteres en una página web en lugar de interpretarlos como etiquetas o entidades HTML. Sin la codificación HTML, la salida del código del servidor podría no mostrarse correctamente y podría exponer una página a los riesgos de seguridad.
> 
> Si el objetivo es generar el formato HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), vea la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.
> 
> Puede obtener más información sobre la codificación HTML en [trabajar con formularios HTML en ASP.NET Web pages sitios](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. delimite los bloques de código con código... Código final

Un bloque de código incluye una o varias instrucciones de código y se encuentra entre las palabras clave `Code` y `End Code`. Coloque la palabra clave de `Code` de apertura inmediatamente después &#8212; del carácter de `@` no puede haber espacio en blanco entre ellas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Resultado mostrado en un explorador:

![Razor: Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. dentro de un bloque, finaliza cada instrucción de código con un salto de línea.

En un bloque de código Visual Basic, cada instrucción finaliza con un salto de línea. (Más adelante en el artículo verá una forma de ajustar una instrucción de código larga en varias líneas si es necesario).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Utilice variables para almacenar valores

Puede almacenar valores en una *variable*, como cadenas, números y fechas, etc. Cree una nueva variable mediante la palabra clave `Dim`. Los valores de las variables se pueden insertar directamente en una página mediante `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Resultado mostrado en un explorador:

![Razor: Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. incluir valores de cadena literales entre comillas dobles

Una *cadena* es una secuencia de caracteres que se trata como texto. Para especificar una cadena, debe encerrarla entre comillas dobles:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Para insertar comillas dobles en un valor de cadena, inserte dos caracteres de comillas dobles. Si desea que el carácter de comillas dobles aparezca una vez en el resultado de la página, escríbalo como `""` dentro de la cadena entrecomillada y, si desea que aparezca dos veces, escríbalo como `""""` dentro de la cadena entrecomillada.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Resultado mostrado en un explorador:

![Razor: Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic código no distingue entre mayúsculas y minúsculas

El lenguaje Visual Basic no distingue mayúsculas de minúsculas. En cualquier caso, se pueden escribir palabras clave de programación (como `Dim`, `If`y `True`) y nombres de variables (como `myString`o `subTotal`).

Las siguientes líneas de código asignan un valor a la variable `lastname` con un nombre en minúsculas y, a continuación, envían el valor de la variable a la página con un nombre en mayúsculas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Resultado mostrado en un explorador:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. gran parte de la codificación implica trabajar con objetos

Un objeto representa un elemento que puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud Web, un mensaje de correo electrónico, un registro de cliente (fila de base de datos), etc. Los objetos tienen propiedades que describen sus &#8212; características. un objeto de cuadro de texto tiene una propiedad `Text`, un objeto de solicitud tiene una propiedad `Url`, un mensaje de correo electrónico tiene una propiedad `From` y un objeto Customer tiene una propiedad `FirstName`. Los objetos también tienen métodos que son los &quot;verbos&quot; pueden realizar. Entre los ejemplos se incluye el método de `Save` de un objeto de archivo, el método de `Rotate` de un objeto de imagen y el método `Send` de un objeto de correo electrónico.

A menudo, trabajará con el objeto `Request`, que le proporciona información como los valores de los campos de formulario en la página (cuadros de texto, etc.), el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc. En este ejemplo se muestra cómo obtener acceso a las propiedades del objeto `Request` y cómo llamar al método `MapPath` del objeto `Request`, que proporciona la ruta de acceso absoluta de la página en el servidor:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Resultado mostrado en un explorador:

![Razor: Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. puede escribir código que tome decisiones

Una característica clave de las páginas web dinámicas es que puede determinar qué hacer en función de las condiciones. La forma más común de hacerlo es con la instrucción `If` (y la instrucción `Else` opcional).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

La instrucción `If IsPost` es una forma abreviada de escribir `If IsPost = True`. Junto con las instrucciones `If`, hay varias maneras de probar condiciones, repetir bloques de código, etc., que se describen más adelante en este artículo.

El resultado mostrado en un explorador (después de hacer clic en **submit**):

![Razor: Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **Métodos GET y POST de HTTP y la propiedad IsPost**
> 
> El protocolo usado para páginas web (HTTP) admite un número muy limitado de métodos (&quot;verbos&quot;) que se usan para hacer solicitudes al servidor. Los dos más comunes son GET, que se usa para leer una página y POST, que se usa para enviar una página. En general, la primera vez que un usuario solicita una página, se solicita la página mediante GET. Si el usuario rellena un formulario y, a continuación, hace clic en **Enviar**, el explorador realiza una solicitud post al servidor.
> 
> En la programación web, a menudo resulta útil saber si se solicita una página como GET o como una entrada para que sepa cómo procesar la página. En ASP.NET Web Pages, puede usar la propiedad `IsPost` para ver si una solicitud es GET o POST. Si la solicitud es una publicación, la propiedad `IsPost` devolverá True y puede hacer cosas como leer los valores de los cuadros de texto de un formulario. Muchos ejemplos que verá muestran cómo procesar la página de forma diferente en función del valor de `IsPost`.

## <a name="a-simple-code-example"></a>Un ejemplo de código simple

En este procedimiento se muestra cómo crear una página que muestra las técnicas básicas de programación. En el ejemplo, creará una página que permite a los usuarios escribir dos números y, después, los agregará y mostrará el resultado.

1. En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers. vbhtml*.
2. Copie el código y el marcado siguientes en la página, reemplazando todo lo que ya está en la página.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    A continuación se indican algunas cosas que debe tener en cuenta:

    - El carácter `@` inicia el primer bloque de código de la página y precede a la variable `totalMessage` incrustada cerca de la parte inferior.
    - El bloque situado en la parte superior de la página se incluye en `Code...End Code`.
    - Las variables `total`, `num1`, `num2`y `totalMessage` almacenan varios números y una cadena.
    - El valor de cadena literal asignado a la variable `totalMessage` está entre comillas dobles.
    - Dado que Visual Basic código no distingue entre mayúsculas y minúsculas, cuando la variable de `totalMessage` se usa cerca de la parte inferior de la página, su nombre solo debe coincidir con la ortografía de la declaración de variable en la parte superior de la página. No importa el uso de mayúsculas y minúsculas.
    - La expresión `num1.AsInt()` + `num2.AsInt()` muestra cómo trabajar con objetos y métodos. El método `AsInt` de cada variable convierte la cadena especificada por un usuario en un número entero (un entero) que se puede Agregar.
    - La etiqueta `<form>` incluye un atributo `method="post"`. Esto especifica que cuando el usuario haga clic en **Agregar**, la página se enviará al servidor mediante el método http post. Cuando se envía la página, el código `If IsPost` se evalúa como true y se ejecuta el código condicional, lo que muestra el resultado de sumar los números.
3. Guarde la página y ejecútela en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). Escriba dos números enteros y, a continuación, haga clic en el botón **Agregar** .

    ![Razor: Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Lenguaje Visual Basic y sintaxis

Antes vio un ejemplo básico de cómo crear una página web de ASP.NET y cómo agregar código de servidor al marcado HTML. Aquí aprenderá los conceptos básicos del uso de Visual Basic para escribir código del servidor de ASP.NET mediante &#8212; el sintaxis Razor es decir, las reglas del lenguaje de programación.

Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que Lee aquí le resultará familiar. Probablemente necesitará familiarizarse solo con cómo se agrega el código WebMatrix al marcado en los archivos *. vbhtml* .

### <a id="BM_CombiningTextMarkupAndCode"></a>Combinar texto, marcado y código en bloques de código

En los bloques de código de servidor, a menudo querrá Mostrar texto y marcado en la página. Si un bloque de código de servidor contiene texto que no es código y que, en su lugar, se debe representar como es, ASP.NET debe poder distinguir ese texto del código. Existen varias formas de hacerlo.

- Incluya el texto en un elemento de bloque HTML, como `<p></p>` o `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    El elemento HTML puede incluir texto, elementos HTML adicionales y expresiones de código de servidor. Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), representa todo el elemento y su contenido tal como está en el explorador (y resuelve las expresiones de código de servidor).

- Use el operador `@:` o el elemento `<text>`. El `@:` genera una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el elemento `<text>` incluye varias líneas para la salida. Estas opciones son útiles cuando no desea representar un elemento HTML como parte de la salida.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    En el ejemplo siguiente se repite el ejemplo anterior, pero se usa un solo par de etiquetas `<text>` para incluir el texto que se va a representar.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    En el ejemplo siguiente, las etiquetas `<text>` y `</text>` incluyen tres líneas, todas ellas tienen un texto no contenida y etiquetas HTML no coincidentes (`<br />`), junto con el código del servidor y las etiquetas HTML coincidentes. De nuevo, también podría preceder cada línea individualmente con el operador `@:`; ambos métodos funcionan.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Cuando se genera texto como se muestra en esta &#8212; sección mediante un elemento HTML, el operador `@:` o el elemento &#8212; `<text>` ASP.net no codifica en HTML el resultado. (Como se indicó anteriormente, ASP.NET codifica la salida de las expresiones de código de servidor y los bloques de código de servidor precedidos por `@`, excepto en los casos especiales que se indican en esta sección).

### <a name="whitespace"></a>Whitespace

Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Dividir instrucciones largas en varias líneas

Puede dividir una instrucción de código larga en varias líneas mediante el carácter de subrayado `_` (que en Visual Basic se denomina el *carácter de continuación*) después de cada línea de código. Para dividir una instrucción en la línea siguiente, al final de la línea agregue un espacio y, a continuación, el carácter de continuación. Continúe con la instrucción en la línea siguiente. Puede incluir instrucciones en tantas líneas como necesite para mejorar la legibilidad. Las siguientes instrucciones son iguales:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Sin embargo, no se puede ajustar una línea en medio de un literal de cadena. El ejemplo siguiente no funciona:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Para combinar una cadena larga que se ajusta a varias líneas como el código anterior, deberá usar el *operador de concatenación* (`&`), que verá más adelante en este artículo.

### <a name="code-comments"></a>Comentarios de código

Los comentarios le permiten dejar notas por su cuenta o por otras personas. Sintaxis Razor comentarios llevan el prefijo `@*` y terminan con `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Dentro de los bloques de código puede utilizar los comentarios de sintaxis Razor, o bien usar el carácter de comentario de Visual Basic ordinario, que es una comilla simple (`'`) con prefijo en cada línea.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>variables

Una variable es un objeto con nombre que se utiliza para almacenar los datos. Puede asignar cualquier valor a las variables, pero el nombre debe comenzar por un carácter alfabético y no puede contener espacios en blanco ni caracteres reservados. En Visual Basic, como vimos anteriormente, no importa el caso de las letras de un nombre de variable.

### <a name="variables-and-data-types"></a>Variables y tipos de datos

Una variable puede tener un tipo de datos específico, que indica qué tipo de datos se almacenan en la variable. Puede tener variables de cadena que almacenen valores de cadena (como &quot;Hello World&quot;), variables de entero que almacenan valores de número entero (como 3 o 79) y variables de fecha que almacenan valores de fecha en una variedad de formatos (como 4/12/2012 o marzo de 2009). Y hay muchos otros tipos de datos que puede usar.

Sin embargo, no es necesario especificar un tipo para una variable. En la mayoría de los casos, ASP.NET puede averiguar el tipo en función de cómo se usan los datos de la variable. (En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).

Para declarar una variable sin especificar un tipo, utilice `Dim` más el nombre de la variable (por ejemplo, `Dim myVar`). Para declarar una variable con un tipo, utilice `Dim` más el nombre de la variable, seguido de `As` y del nombre de tipo (por ejemplo, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

En el ejemplo siguiente se muestran algunas expresiones en línea que usan las variables en una página web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Resultado mostrado en un explorador:

![Razor: Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversión y prueba de tipos de datos

Aunque ASP.NET normalmente puede determinar un tipo de datos de forma automática, a veces no es posible. Por lo tanto, es posible que necesite ayudar a ASP.NET a realizar una conversión explícita. Incluso si no tiene que convertir tipos, a veces resulta útil probar para ver el tipo de datos con el que puede trabajar.

El caso más común es que tenga que convertir una cadena en otro tipo, como un entero o una fecha. En el ejemplo siguiente se muestra un caso típico en el que debe convertir una cadena en un número.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Como regla, los datos proporcionados por el usuario se incluyen como cadenas. Incluso si ha solicitado al usuario que escriba un número e incluso si ha escrito un dígito, cuando se envía la entrada del usuario y lo lee en el código, los datos están en formato de cadena. Por lo tanto, debe convertir la cadena en un número. En el ejemplo, si intenta realizar operaciones aritméticas en los valores sin convertirlos, se producirá el siguiente error, porque ASP.NET no puede agregar dos cadenas:

`Cannot implicitly convert type 'string' to 'int'.`

Para convertir los valores en enteros, llame al método `AsInt`. Si la conversión se realiza correctamente, puede Agregar los números.

En la tabla siguiente se enumeran algunos métodos de conversión y prueba comunes para las variables.

:::row:::
    :::column:::
        <strong>Método</strong>
    :::column-end:::
    :::column:::
        <strong>Descripción</strong>
    :::column-end:::
    :::column:::
        <strong>Ejemplo</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Convierte una cadena que representa un número entero (como &quot;593&quot;) en un entero.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Convierte una cadena como &quot;true&quot; o &quot;falso&quot; a un tipo booleano.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Convierte una cadena que tiene un valor decimal como &quot;1,3&quot; o &quot;7,439&quot; a un número de punto flotante.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Convierte una cadena que tiene un valor decimal como &quot;1,3&quot; o &quot;7,439&quot; un número decimal. (En ASP.NET, un número decimal es más preciso que un número de punto flotante).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Convierte una cadena que representa un valor de fecha y hora en el tipo de `DateTime` ASP.NET.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Convierte cualquier otro tipo de datos en una cadena.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operadores

Un operador es una palabra clave o un carácter que indica a ASP.NET qué tipo de comando se debe realizar en una expresión. Visual Basic admite muchos operadores, pero solo necesita reconocer algunos para empezar a desarrollar páginas web de ASP.NET. En la tabla siguiente se resumen los operadores más comunes.

:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Descripción</strong>
    :::column-end:::
    :::column:::
        <strong>Ejemplos</strong>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        Operadores matemáticos usados en expresiones numéricas.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Asignación e igualdad. Dependiendo del contexto, asigna el valor en el lado derecho de una instrucción al objeto en el lado izquierdo o comprueba si los valores son iguales.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Desigualdad. Devuelve `True` si los valores no son iguales.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Menor que, mayor que, menor o igual que y mayor o igual que.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenación, que se usa para combinar cadenas.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        Los operadores de incremento y decremento, que suman y restan 1 (respectivamente) de una variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Semitono. Se usa para distinguir objetos y sus propiedades y métodos.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Paréntesis. Se utiliza para agrupar expresiones, para pasar parámetros a métodos y para obtener acceso a miembros de matrices y colecciones.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Tampoco. Invierte un valor true a false y viceversa. Normalmente se utiliza como una forma abreviada de probar `False` (es decir, para no `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        AND y OR lógicos, que se usan para vincular condiciones.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a>Trabajar con rutas de acceso de archivos y carpetas en el código

A menudo, trabajará con rutas de acceso de archivos y carpetas en el código. A continuación se muestra un ejemplo de una estructura de carpetas físicas para un sitio Web tal y como podría aparecer en el equipo de desarrollo:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Estos son algunos detalles esenciales sobre las direcciones URL y las rutas de acceso:

- Una dirección URL comienza con un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).
- Una dirección URL corresponde a una ruta de acceso física en un equipo host. Por ejemplo, `http://myserver` podría corresponder a la carpeta *C:\websites\mywebsite* del servidor.
- Una ruta de acceso virtual es una forma abreviada de representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa. Incluye la parte de una dirección URL que sigue al nombre de dominio o de servidor. Al usar las rutas de acceso virtuales, puede trasladar el código a otro dominio o servidor sin tener que actualizar las rutas de acceso.

Este es un ejemplo para ayudarle a comprender las diferencias:

| Dirección URL completa | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nombre de servidor | *mycompanyserver* |
| Ruta de acceso virtual | */humanresources/CompanyPolicy.htm* |
| Ruta de acceso física | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

La raíz virtual es/, al igual que la raíz de la unidad C: \. (Las rutas de acceso de carpeta virtual siempre usan barras diagonales). La ruta de acceso virtual de una carpeta no tiene que tener el mismo nombre que la carpeta física; puede ser un alias. (En servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta).

Cuando se trabaja con archivos y carpetas en el código, a veces es necesario hacer referencia a la ruta de acceso física y, en ocasiones, a una ruta de acceso virtual, en función de los objetos con los que esté trabajando. ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el método `Server.MapPath` y el operador `~` y el método `Href`.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversión de rutas de acceso virtuales a físicas: el método Server. MapPath

El método `Server.MapPath` convierte una ruta de acceso virtual (como */default.cshtml*) en una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Utilice este método siempre que necesite una ruta de acceso física completa. Un ejemplo típico es al leer o escribir un archivo de texto o de imagen en el servidor Web.

Normalmente, no se conoce la ruta de acceso física absoluta del sitio en el servidor de un sitio de hospedaje, por lo que este método puede convertir la ruta de acceso que se conoce (la ruta de acceso virtual) en la ruta de acceso correspondiente en el servidor. Pasa la ruta de acceso virtual a un archivo o una carpeta al método y devuelve la ruta de acceso física:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Referencia a la raíz virtual: el operador ~ y el método href

En un archivo *. cshtml* o *. vbhtml* , puede hacer referencia a la ruta de acceso de la raíz virtual mediante el operador `~`. Esto es muy útil porque puede mover páginas en un sitio y los vínculos que contienen a otras páginas no se romperán. También es útil en caso de que se mueva el sitio web a una ubicación diferente. A continuación se muestran algunos ejemplos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Si el sitio web está `http://myserver/myapp`, aquí se muestra cómo ASP.NET tratará estas rutas de acceso cuando se ejecute la página:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(En realidad, no verá estas rutas de acceso como los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso fuera).

Puede usar el operador `~` en el código del servidor (como arriba) y en el marcado, como se indica a continuación:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

En el marcado, se usa el operador `~` para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS. Cuando se ejecuta la página, ASP.NET busca en la página (código y marcado) y resuelve todas las referencias de `~` a la ruta de acceso adecuada.

## <a name="conditional-logic-and-loops"></a>Lógica y bucles condicionales

El código de servidor ASP.NET permite realizar tareas basadas en condiciones y escribir código que repite instrucciones un número específico de veces, es decir, código que ejecuta un bucle).

### <a name="testing-conditions"></a>Condiciones de prueba

Para probar una condición simple, use la instrucción `If...Then`, que devuelve `True` o `False` en función de una prueba que especifique:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

La palabra clave `If` inicia un bloque. La prueba real (condición) sigue a la palabra clave `If` y devuelve true o false. La instrucción `If` finaliza con `Then`. Las instrucciones que se ejecutarán si la prueba es true se incluyen `If` y `End If`. Una instrucción `If` puede incluir un bloque de `Else` que especifica las instrucciones que se ejecutarán si la condición es falsa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Si una instrucción `If` inicia un bloque de código, no tiene que utilizar las instrucciones de `Code...End Code` normales para incluir los bloques. Simplemente puede Agregar `@` al bloque y funcionará. Este enfoque funciona con `If` así como con otras palabras clave de programación de Visual Basic que van seguidas de bloques de código, incluidos `For`, `For Each`, `Do While`, etc.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Puede agregar varias condiciones mediante uno o varios bloques de `ElseIf`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

En este ejemplo, si la primera condición del bloque `If` no es true, se comprueba la condición de `ElseIf`. Si se cumple esta condición, se ejecutan las instrucciones del bloque `ElseIf`. Si no se cumple ninguna de las condiciones, se ejecutan las instrucciones del bloque `Else`. Puede agregar cualquier número de bloques de `ElseIf` y, a continuación, cerrar con un bloque de `Else` como &quot;todo lo demás&quot; condición.

Para probar un gran número de condiciones, use un bloque `Select Case`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

El valor que se va a probar se encuentra entre paréntesis (en el ejemplo, la variable Weekday). Cada prueba individual utiliza una instrucción `Case` que muestra un valor. Si el valor de una instrucción `Case` coincide con el valor de prueba, se ejecuta el código de ese bloque de `Case`.

El resultado de los dos últimos bloques condicionales mostrados en un explorador:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Código de bucle

A menudo es necesario ejecutar las mismas instrucciones varias veces. Para ello, puede crear un bucle. Por ejemplo, a menudo se ejecutan las mismas instrucciones para cada elemento en una colección de datos. Si sabe exactamente cuántas veces desea crear un bucle, puede usar un bucle `For`. Este tipo de bucle es especialmente útil para la acumulación o el recuento:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

El bucle comienza con la palabra clave `For`, seguido de tres elementos:

- Inmediatamente después de la instrucción `For`, se declara una variable de contador (no es necesario usar `Dim`) y, a continuación, se indica el intervalo, como en `i = 10 to 20`. Esto significa que la variable `i` comenzará a contar en 10 y continuará hasta que alcance 20 (inclusive).
- Entre las instrucciones `For` y `Next` se encuentra el contenido del bloque. Puede contener una o varias instrucciones de código que se ejecutan con cada bucle.
- La instrucción `Next i` finaliza el bucle. Incrementa el contador e inicia la siguiente iteración del bucle.

La línea de código entre las líneas `For` y `Next` contiene el código que se ejecuta para cada iteración del bucle. El marcado crea un nuevo párrafo (elemento`<p>`) cada vez y agrega una línea a la salida, mostrando el valor de i (el contador). Al ejecutar esta página, en el ejemplo se crean 11 líneas que muestran la salida, con el texto en cada línea que indica el número de elemento.

![Razor: Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Si está trabajando con una colección o una matriz, a menudo utiliza un bucle `For Each`. Una colección es un grupo de objetos similares y el bucle `For Each` le permite llevar a cabo una tarea en cada elemento de la colección. Este tipo de bucle es práctico para las colecciones, porque a diferencia de un bucle `For`, no tiene que incrementar el contador o establecer un límite. En su lugar, el código de bucle `For Each` simplemente continúa a través de la colección hasta que termina.

En este ejemplo se devuelven los elementos de la colección `Request.ServerVariables` (que contiene información sobre el servidor Web). Usa un bucle `For Each` para mostrar el nombre de cada elemento mediante la creación de un nuevo elemento `<li>` en una lista con viñetas HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

La palabra clave `For Each` va seguida de una variable que representa un único elemento de la colección (en el ejemplo, `myItem`), seguido de la palabra clave `In`, seguida de la colección a la que desea recorrer el bucle. En el cuerpo del bucle `For Each`, puede tener acceso al elemento actual mediante la variable que declaró anteriormente.

![Razor: Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Para crear un bucle de uso más general, use la instrucción `Do While`:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Este bucle comienza con la palabra clave `Do While`, seguida de una condición, seguida del bloque que se va a repetir. Los bucles suelen incrementar (agregar a) o disminuir (restar de) una variable o un objeto que se usa para el recuento. En el ejemplo, el operador `+=` agrega 1 al valor de una variable cada vez que se ejecuta el bucle. (Para reducir una variable en un bucle que se repite, se usaría el operador de decremento `-=`).

## <a name="objects-and-collections"></a>Objetos y colecciones

Casi todo en un sitio web de ASP.NET es un objeto, incluida la propia página web. En esta sección se describen algunos objetos importantes con los que trabajará con frecuencia en el código.

### <a name="page-objects"></a>Objetos de página

El objeto más básico de ASP.NET es la página. Puede tener acceso directamente a las propiedades del objeto de página sin ningún objeto calificador. En el código siguiente se obtiene la ruta de acceso del archivo de la página mediante el `Request` objeto de la página:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Puede utilizar las propiedades del objeto `Page` para obtener una gran cantidad de información, como:

- `Request`Operador Como ya ha visto, se trata de una colección de información sobre la solicitud actual, incluido el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc.
- `Response`Operador Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando el código del servidor haya terminado de ejecutarse. Por ejemplo, puede usar esta propiedad para escribir información en la respuesta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de colección (matrices y diccionarios)

Una colección es un grupo de objetos del mismo tipo, como una colección de objetos `Customer` de una base de datos. ASP.NET contiene muchas colecciones integradas, como la `Request.Files` colección.

A menudo, trabajará con los datos de las colecciones. Dos tipos de colección comunes son la *matriz* y el *Diccionario*. Una matriz es útil cuando desea almacenar una colección de elementos similares pero no desea crear una variable independiente que contenga cada elemento:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Con las matrices, se declara un tipo de datos específico, como `String`, `Integer`o `DateTime`. Para indicar que la variable puede contener una matriz, agregue paréntesis al nombre de la variable en la declaración (como `Dim myVar() As String`). Puede tener acceso a los elementos de una matriz mediante su posición (índice) o mediante la instrucción `For Each`. Los índices de matriz son de base &#8212; cero, es decir, el primer elemento está en la posición 0, el segundo elemento está en la posición 1, y así sucesivamente.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Puede determinar el número de elementos de una matriz obteniendo su `Length` propiedad. Para obtener la posición de un elemento específico en la matriz (es decir, para buscar en la matriz), use el método `Array.IndexOf`. También puede hacer cosas como invertir el contenido de una matriz (el método `Array.Reverse`) u ordenar el contenido (el método `Array.Sort`).

La salida del código de la matriz de cadenas que se muestra en un explorador:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Un diccionario es una colección de pares clave-valor, donde se proporciona la clave (o nombre) para establecer o recuperar el valor correspondiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Para crear un diccionario, use la palabra clave `New` para indicar que está creando un nuevo objeto `Dictionary`. Puede asignar un diccionario a una variable mediante la palabra clave `Dim`. Se indican los tipos de datos de los elementos del diccionario mediante paréntesis (`( )`). Al final de la declaración, debe agregar otro par de paréntesis, porque en realidad es un método que crea un nuevo diccionario.

Para agregar elementos al diccionario, puede llamar al método `Add` de la variable Dictionary (`myScores` en este caso) y, a continuación, especificar una clave y un valor. Como alternativa, puede usar paréntesis para indicar la clave y realizar una asignación simple, como en el ejemplo siguiente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Para obtener un valor del diccionario, especifique la clave entre paréntesis:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Llamar a métodos con parámetros

Como vimos anteriormente en este artículo, los objetos con los que programa tienen métodos. Por ejemplo, un objeto de `Database` podría tener un método `Database.Connect`. Muchos métodos también tienen uno o más parámetros. Un *parámetro* es un valor que se pasa a un método para que el método pueda completar su tarea. Por ejemplo, examine una declaración para el método `Request.MapPath`, que toma tres parámetros:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada. Los tres parámetros del método son `virtualPath`, `baseVirtualDir`y `allowCrossAppMapping`. (Observe que en la declaración, los parámetros se enumeran con los tipos de datos de los datos que aceptarán). Cuando llame a este método, debe proporcionar valores para los tres parámetros.

Cuando se usa Visual Basic con el sintaxis Razor, tiene dos opciones para pasar parámetros a un método: *parámetros posicionales* o *parámetros con nombre*. Para llamar a un método mediante parámetros posicionales, se pasan los parámetros en un orden estricto que se especifica en la declaración del método. (Normalmente se conoce este orden leyendo la documentación del método). Debe seguir el orden y no puede omitir ninguno de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o null para un parámetro posicional para el que no tiene un valor.

En el ejemplo siguiente se da por supuesto que tiene una carpeta denominada *scripts* en el sitio Web. El código llama al método `Request.MapPath` y pasa los valores de los tres parámetros en el orden correcto. A continuación, se muestra la ruta de acceso asignada resultante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Cuando hay muchos parámetros para un método, puede mantener el código más limpio y más legible mediante el uso de parámetros con nombre. Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de `:=` y, a continuación, proporcione el valor. Una ventaja de los parámetros con nombre es que puede agregarlos en cualquier orden que desee. (Una desventaja es que la llamada al método no es tan compacta).

En el ejemplo siguiente se llama al mismo método que el anterior, pero se usan parámetros con nombre para proporcionar los valores:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Como puede ver, los parámetros se pasan en un orden diferente. Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverán el mismo valor.

## <a name="handling-errors"></a>Control de errores

### <a name="try-catch-statements"></a>Instrucciones try-catch

A menudo tendrá instrucciones en el código que podrían producir errores por motivos ajenos al control. Por ejemplo:

- Si el código intenta abrir, crear, leer o escribir un archivo, puede producirse todo tipo de errores. Es posible que el archivo que desea no exista, que esté bloqueado, que el código no tenga permisos, etc.
- Del mismo modo, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, que la conexión a la base de datos se haya quitado, que los datos que se van a guardar no sean válidos, etc.

En términos de programación, estas situaciones se denominan *excepciones*. Si el código encuentra una excepción, genera (lanza) un mensaje de error que, en el mejor de los usuarios, es molesto.

![Razor: Img14](introducing-razor-syntax-vb/_static/image14.jpg)

En situaciones en las que el código pueda encontrar excepciones y para evitar los mensajes de error de este tipo, puede usar `Try/Catch` instrucciones. En la instrucción `Try`, ejecute el código que está comprobando. En una o varias instrucciones de `Catch`, puede buscar errores específicos (tipos específicos de excepciones) que puedan haberse producido. Puede incluir tantas instrucciones `Catch` como necesite para buscar los errores que espera.

> [!NOTE]
> Se recomienda evitar el uso del método `Response.Redirect` en `Try/Catch` instrucciones, ya que puede producir una excepción en la página.

En el ejemplo siguiente se muestra una página que crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo. En el ejemplo se usa deliberadamente un nombre de archivo incorrecto para que se produzca una excepción. El código incluye `Catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto y `DirectoryNotFoundException`, que se produce si ASP.NET no puede incluso encontrar la carpeta. (Puede quitar el comentario de una instrucción en el ejemplo para ver cómo se ejecuta cuando todo funciona correctamente).

Si el código no controla la excepción, verá una página de error como la captura de pantalla anterior. Sin embargo, la sección `Try/Catch` ayuda a evitar que el usuario vea estos tipos de errores.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Recursos adicionales

### <a name="reference-documentation"></a>Documentación de referencia

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic (lenguaje)](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
