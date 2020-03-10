---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introducción a la programación web de ASP.NET mediante la sintaxisC#de Razor () | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se ofrece información general sobre la programación con ASP.NET Web Pages mediante el sintaxis Razor. ASP.NET es la tecnología de Microsoft para ejecutar Dynamic Web PA...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: c2f420bb7c2f7d2e31654c20fb9ec7497a30a9f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521209"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introducción a la programación web de ASP.NET mediante la sintaxisC#Razor ()

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se proporciona información general sobre la programación con ASP.NET Web Pages mediante el sintaxis Razor. ASP.NET es la tecnología de Microsoft para ejecutar páginas web dinámicas en servidores Web. Este artículo se centra en el C# uso del lenguaje de programación.
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

## <a name="the-top-8-programming-tips"></a>Las 8 mejores sugerencias de programación

En esta sección se enumeran algunas sugerencias que es absolutamente necesario saber a medida que empieza a escribir código de servidor de ASP.NET mediante el sintaxis Razor.

> [!NOTE]
> El sintaxis Razor se basa en el C# lenguaje de programación, y es el lenguaje que se usa con más frecuencia con ASP.NET Web pages. Sin embargo, el sintaxis Razor también admite el lenguaje de Visual Basic y todo lo que se ve también puede realizarse en Visual Basic. Para obtener más información, consulte el apéndice [Visual Basic lenguaje y la sintaxis](https://go.microsoft.com/fwlink/?LinkId=202908).

Puede encontrar más detalles sobre la mayoría de estas técnicas de programación más adelante en el artículo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Agregue código a una página mediante el carácter @.

El carácter `@` inicia las expresiones en línea, los bloques de instrucción única y los bloques de varias instrucciones:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Este es el aspecto de estas instrucciones cuando la página se ejecuta en un explorador:

![Razor: img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Codificación HTML**
> 
> Al mostrar el contenido en una página mediante el carácter `@`, como en los ejemplos anteriores, ASP.NET codifica el resultado en HTML. Esto reemplaza los caracteres HTML reservados (como `<` y `>` y `&`) con códigos que permiten mostrar los caracteres como caracteres en una página web en lugar de interpretarlos como etiquetas o entidades HTML. Sin la codificación HTML, la salida del código del servidor podría no mostrarse correctamente y podría exponer una página a los riesgos de seguridad.
> 
> Si el objetivo es generar el formato HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), vea la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.
> 
> Puede obtener más información sobre la codificación HTML en [trabajar con formularios](https://go.microsoft.com/fwlink/?LinkId=202892).

### <a name="2-you-enclose-code-blocks-in-braces"></a>2. incluir los bloques de código entre llaves

Un *bloque de código* incluye una o varias instrucciones de código y está entre llaves.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Resultado mostrado en un explorador:

![Razor: Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. dentro de un bloque, finaliza cada instrucción de código con un punto y coma

Dentro de un bloque de código, cada instrucción de código completo debe terminar con un punto y coma. Las expresiones insertadas no finalizan con un punto y coma.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Utilice variables para almacenar valores

Puede almacenar valores en una *variable*, como cadenas, números y fechas, etc. Cree una nueva variable mediante la palabra clave `var`. Los valores de las variables se pueden insertar directamente en una página mediante `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Resultado mostrado en un explorador:

![Razor: Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. incluir valores de cadena literales entre comillas dobles

Una *cadena* es una secuencia de caracteres que se trata como texto. Para especificar una cadena, debe encerrarla entre comillas dobles:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Si la cadena que desea mostrar contiene un carácter de barra diagonal inversa (`\`) o comillas dobles (`"`), use un *literal de cadena textual* que tenga como prefijo el operador `@`. (En C#, el carácter \ tiene un significado especial a menos que use un literal de cadena textual).

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Para insertar comillas dobles, use un literal de cadena textual y repita las comillas:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Este es el resultado de usar estos dos ejemplos en una página:

![Razor: Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Observe que el carácter `@` se usa para marcar los literales de cadena textual C# en y para marcar el código en las páginas de ASP.net.

### <a name="6-code-is-case-sensitive"></a>6. el código distingue mayúsculas de minúsculas

En C#, las palabras clave (como `var`, `true`y `if`) y los nombres de variable distinguen mayúsculas de minúsculas. Las siguientes líneas de código crean dos variables diferentes, `lastName` y `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Si declara una variable como `var lastName = "Smith";` e intenta hacer referencia a esa variable en la página como `@LastName`, obtendría el valor `"Jones"` en lugar de `"Smith"`.

> [!NOTE]
> En Visual Basic, las palabras clave y las variables *no* distinguen mayúsculas de minúsculas.

### <a name="7-much-of-your-coding-involves-objects"></a>7. gran parte de la codificación implica objetos

Un *objeto* representa un elemento que puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud Web, un mensaje de correo electrónico, un registro de cliente (fila de base de datos), etc. Los objetos tienen propiedades que describen sus características y que puede leer o cambiar &#8212; un objeto de cuadro de texto tiene una propiedad `Text` (entre otros), un objeto de solicitud tiene una propiedad `Url`, un mensaje de correo electrónico tiene una propiedad `From` y un objeto Customer tiene una propiedad `FirstName`. Los objetos también tienen métodos que son los &quot;verbos&quot; pueden realizar. Entre los ejemplos se incluye el método de `Save` de un objeto de archivo, el método de `Rotate` de un objeto de imagen y el método `Send` de un objeto de correo electrónico.

A menudo, trabajará con el objeto `Request`, que le proporciona información como los valores de los cuadros de texto (campos de formulario) en la página, el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc. En el ejemplo siguiente se muestra cómo obtener acceso a las propiedades del objeto `Request` y cómo llamar al método `MapPath` del objeto `Request`, que proporciona la ruta de acceso absoluta de la página en el servidor:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Resultado mostrado en un explorador:

![Razor: Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. puede escribir código que tome decisiones

Una característica clave de las páginas web dinámicas es que puede determinar qué hacer en función de las condiciones. La forma más común de hacerlo es con la instrucción `if` (y la instrucción `else` opcional).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

La instrucción `if(IsPost)` es una forma abreviada de escribir `if(IsPost == true)`. Junto con las instrucciones `if`, hay varias maneras de probar condiciones, repetir bloques de código, etc., que se describen más adelante en este artículo.

El resultado mostrado en un explorador (después de hacer clic en **submit**):

![Razor: Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>Métodos GET y POST de HTTP y la propiedad IsPost
> 
> El protocolo usado para páginas web (HTTP) admite un número muy limitado de métodos (verbos) que se usan para hacer solicitudes al servidor. Los dos más comunes son GET, que se usa para leer una página y POST, que se usa para enviar una página. En general, la primera vez que un usuario solicita una página, se solicita la página mediante GET. Si el usuario rellena un formulario y, a continuación, hace clic en un botón Enviar, el explorador realiza una solicitud POST al servidor.
> 
> En la programación web, a menudo resulta útil saber si se solicita una página como GET o como una entrada para que sepa cómo procesar la página. En ASP.NET Web Pages, puede usar la propiedad `IsPost` para ver si una solicitud es GET o POST. Si la solicitud es una publicación, la propiedad `IsPost` devolverá True y puede hacer cosas como leer los valores de los cuadros de texto de un formulario. Muchos ejemplos que verá muestran cómo procesar la página de forma diferente en función del valor de `IsPost`.

## <a name="a-simple-code-example"></a>Un ejemplo de código simple

En este procedimiento se muestra cómo crear una página que muestra las técnicas básicas de programación. En el ejemplo, creará una página que permite a los usuarios escribir dos números y, después, los agregará y mostrará el resultado.

1. En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers. cshtml*.
2. Copie el código y el marcado siguientes en la página, reemplazando todo lo que ya está en la página.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    A continuación se indican algunas cosas que debe tener en cuenta:

    - El carácter `@` inicia el primer bloque de código de la página y precede a la variable `totalMessage` que está incrustada cerca de la parte inferior de la página.
    - El bloque situado en la parte superior de la página se incluye entre llaves.
    - En el bloque de la parte superior, todas las líneas terminan con un punto y coma.
    - Las variables `total`, `num1`, `num2`y `totalMessage` almacenan varios números y una cadena.
    - El valor de cadena literal asignado a la variable `totalMessage` está entre comillas dobles.
    - Dado que el código distingue entre mayúsculas y minúsculas, cuando la variable de `totalMessage` se usa cerca de la parte inferior de la página, su nombre debe coincidir exactamente con la variable en la parte superior.
    - La expresión `num1.AsInt() + num2.AsInt()` muestra cómo trabajar con objetos y métodos. El método `AsInt` de cada variable convierte la cadena especificada por un usuario en un número (un entero) para que pueda realizar operaciones aritméticas en ella.
    - La etiqueta `<form>` incluye un atributo `method="post"`. Esto especifica que cuando el usuario haga clic en **Agregar**, la página se enviará al servidor mediante el método http post. Cuando se envía la página, la prueba `if(IsPost)` se evalúa como true y se ejecuta el código condicional, mostrando el resultado de sumar los números.
3. Guarde la página y ejecútela en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). Escriba dos números enteros y, a continuación, haga clic en el botón **Agregar** . 

    ![Razor: Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Conceptos básicos de la programación

En este artículo se proporciona información general sobre la programación web de ASP.NET. No es un examen exhaustivo, solo un paseo rápido por los conceptos de programación que usará con mayor frecuencia. Incluso así, cubre casi todo lo que necesita para empezar a trabajar con ASP.NET Web Pages.

Pero en primer lugar, un poco de conocimientos técnicos.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>La sintaxis de Razor, el código de servidor y ASP.NET

Sintaxis Razor es una sintaxis de programación sencilla para insertar código basado en servidor en una página web. En una página web que utiliza el sintaxis Razor, hay dos tipos de contenido: contenido del cliente y código del servidor. El contenido del cliente es el material que se usa en las páginas web: marcado HTML (elementos), información de estilo como CSS, quizás algún script de cliente como JavaScript y texto sin formato.

Sintaxis Razor le permite agregar código de servidor a este contenido de cliente. Si hay código de servidor en la página, el servidor ejecuta primero ese código, antes de enviar la página al explorador. Mediante la ejecución de en el servidor, el código puede realizar tareas que pueden ser mucho más complejas de utilizar solo el contenido del cliente, como el acceso a bases de datos basadas en el servidor. Lo más importante es que el código de servidor puede crear dinámicamente el contenido &#8212; del cliente y puede generar marcado HTML u otro contenido sobre la marcha y, a continuación, enviarlo al explorador junto con cualquier código HTML estático que pueda contener la página. Desde la perspectiva del explorador, el contenido de cliente generado por el código del servidor no es diferente de ningún otro contenido de cliente. Como ya ha visto, el código de servidor necesario es bastante sencillo.

Las páginas web de ASP.NET que incluyen la sintaxis Razor tienen una extensión de archivo especial ( *. cshtml* o *. vbhtml*). El servidor reconoce estas extensiones, ejecuta el código que está marcado con sintaxis Razor y, a continuación, envía la página al explorador.

### <a name="where-does-aspnet-fit-in"></a>¿Dónde encaja ASP.NET?

Sintaxis Razor se basa en una tecnología de Microsoft llamada ASP.NET, que a su vez se basa en el marco de Microsoft .NET. The.NET Framework es un marco de programación amplio y completo de Microsoft para desarrollar prácticamente cualquier tipo de aplicación de equipos. ASP.NET es la parte de la .NET Framework que está diseñada específicamente para la creación de aplicaciones Web. Los desarrolladores han usado ASP.NET para crear muchos de los sitios web de mayor y mayor tráfico del mundo. (Siempre que vea la extensión de nombre de archivo *. aspx* como parte de la dirección URL de un sitio, sabrá que el sitio se escribió con ASP.net).

El sintaxis Razor le proporciona toda la eficacia de ASP.NET, pero con una sintaxis simplificada que es más fácil de aprender si es un principiante y que le permite ser más productivo si es un experto. Aunque esta sintaxis es fácil de usar, su relación de familia con ASP.NET y el .NET Framework significa que, a medida que sus sitios web son más sofisticados, tiene la capacidad de los marcos de trabajo más grandes a su disposición.

![Razor: Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Clases e instancias**
> 
> El código de servidor ASP.NET usa objetos, que a su vez se basan en la idea de clases. La clase es la definición o la plantilla de un objeto. Por ejemplo, una aplicación podría contener una clase `Customer` que define las propiedades y los métodos que necesita cualquier objeto Customer.
> 
> Cuando la aplicación necesita trabajar con información de clientes real, crea una instancia de (o *crea instancias*) de un objeto de cliente. Cada cliente individual es una instancia independiente de la clase `Customer`. Cada instancia de admite las mismas propiedades y métodos, pero los valores de propiedad de cada instancia son normalmente diferentes, ya que cada objeto de cliente es único. En un objeto Customer, la propiedad `LastName` podría ser "Smith"; en otro objeto de cliente, la propiedad `LastName` podría ser "Jones".
> 
> Del mismo modo, cualquier página web individual del sitio es un objeto `Page` que es una instancia de la clase `Page`. Un botón de la página es un objeto `Button` que es una instancia de la clase `Button`, etc. Cada instancia tiene sus propias características, pero todas se basan en lo que se especifica en la definición de clase del objeto.

## <a name="basic-syntax"></a>Sintaxis básica

Antes vio un ejemplo básico de cómo crear una página ASP.NET Web Pages y cómo puede agregar código de servidor al marcado HTML. Aquí aprenderá los conceptos básicos de la escritura de código de servidor ASP.NET mediante &#8212; el sintaxis Razor es decir, las reglas del lenguaje de programación.

Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que Lee aquí le resultará familiar. Probablemente necesitará familiarizarse solo con el modo en que se agrega el código de servidor al marcado en los archivos *. cshtml* .

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Combinar texto, marcado y código en bloques de código

En los bloques de código de servidor, a menudo desea que se genere texto o marcado (o ambos) en la página. Si un bloque de código de servidor contiene texto que no es código y que, en su lugar, se debe representar como es, ASP.NET debe poder distinguir ese texto del código. Existen varias formas de hacerlo.

- Incluya el texto en un elemento HTML como `<p></p>` o `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    El elemento HTML puede incluir texto, elementos HTML adicionales y expresiones de código de servidor. Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), representa todo lo que incluye el elemento y su contenido tal como está en el explorador, resolviendo las expresiones de código de servidor a medida que van.
- Use el operador `@:` o el elemento `<text>`. El `@:` genera una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el elemento `<text>` incluye varias líneas para la salida. Estas opciones son útiles cuando no desea representar un elemento HTML como parte de la salida.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Si desea generar varias líneas de texto o etiquetas HTML no coincidentes, puede preceder cada línea con `@:`, o puede incluir la línea en un elemento `<text>`. Al igual que el operador de `@:`, las etiquetas de`<text>` se usan en ASP.NET para identificar el contenido de texto y nunca se representan en el resultado de la página.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    En el primer ejemplo se repite el ejemplo anterior, pero se usa un solo par de etiquetas `<text>` para incluir el texto que se va a representar. En el segundo ejemplo, las etiquetas `<text>` y `</text>` incluyen tres líneas, todas ellas tienen un texto no contenida y etiquetas HTML no coincidentes (`<br />`), junto con el código del servidor y las etiquetas HTML coincidentes. De nuevo, también podría preceder cada línea individualmente con el operador `@:`; ambos métodos funcionan.

    > [!NOTE]
    > Cuando se genera texto como se muestra en esta &#8212; sección mediante un elemento HTML, el operador `@:` o el elemento &#8212; `<text>` ASP.net no codifica en HTML el resultado. (Como se indicó anteriormente, ASP.NET codifica la salida de las expresiones de código de servidor y los bloques de código de servidor precedidos por `@`, excepto en los casos especiales que se indican en esta sección).

### <a name="whitespace"></a>Whitespace

Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Un salto de línea en una instrucción no tiene ningún efecto en la instrucción y puede ajustar las instrucciones para mejorar la legibilidad. Las siguientes instrucciones son iguales:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Sin embargo, no se puede ajustar una línea en medio de un literal de cadena. El ejemplo siguiente no funciona:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Para combinar una cadena larga que se ajusta a varias líneas como el código anterior, hay dos opciones. Puede usar el operador de concatenación (`+`), que verá más adelante en este artículo. También puede usar el carácter `@` para crear un literal de cadena textual, como vimos anteriormente en este artículo. Puede romper los literales de cadena textual entre las líneas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Comentarios de código (y marcado)

Los comentarios le permiten dejar notas por su cuenta o por otras personas. También le permiten deshabilitar (*comentar*) una sección de código o de marcado que no desea ejecutar, pero que desea mantener en la página por el momento.

Hay una sintaxis de comentario diferente para el código Razor y para el marcado HTML. Al igual que con todo el código Razor, los comentarios de Razor se procesan (y luego se quitan) en el servidor antes de que la página se envíe al explorador. Por lo tanto, la sintaxis de comentarios de Razor permite colocar comentarios en el código (o incluso en el marcado) que se pueden ver al editar el archivo, pero que los usuarios no ven, ni siquiera en el origen de la página.

En el caso de los comentarios de Razor ASP.NET, inicie el comentario con `@*` y termine con `*@`. El comentario puede estar en una o varias líneas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Este es un comentario dentro de un bloque de código:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Este es el mismo bloque de código, con la línea de código comentada para que no se ejecute:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Dentro de un bloque de código, como alternativa al uso de la sintaxis de comentarios de Razor, puede usar la sintaxis de comentarios del lenguaje de programación que está C#usando, por ejemplo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

En C#, los comentarios de una línea van precedidos de los caracteres `//` y los comentarios de varias líneas comienzan con `/*` y terminan con `*/`. (Al igual que con los C# comentarios de Razor, los comentarios no se representan en el explorador).

Para el marcado, como probablemente sepa, puede crear un Comentario HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Los comentarios HTML comienzan con `<!--` caracteres y terminan con `-->`. Puede usar comentarios HTML para rodear no solo texto, sino también cualquier código HTML que desee conservar en la página, pero que no quiera que se represente. Este comentario HTML ocultará todo el contenido de las etiquetas y el texto que contienen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

A diferencia de los comentarios de Razor, los comentarios HTML *se* representan en la página y el usuario puede verlos viendo el origen de la página.

Razor tiene limitaciones en los bloques anidados C#de. Para obtener más información [, C# consulte las variables con nombre y los bloques anidados generar código roto](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>variables

Una variable es un objeto con nombre que se utiliza para almacenar los datos. Puede asignar cualquier valor a las variables, pero el nombre debe comenzar por un carácter alfabético y no puede contener espacios en blanco ni caracteres reservados.

### <a name="variables-and-data-types"></a>Variables y tipos de datos

Una variable puede tener un tipo de datos específico, que indica qué tipo de datos se almacenan en la variable. Puede tener variables de cadena que almacenen valores de cadena (como &quot;Hello World&quot;), variables de entero que almacenan valores de número entero (como 3 o 79) y variables de fecha que almacenan valores de fecha en una variedad de formatos (como 4/12/2012 o marzo de 2009). Y hay muchos otros tipos de datos que puede usar.

Sin embargo, por lo general no es necesario especificar un tipo para una variable. La mayoría de las veces, ASP.NET puede averiguar el tipo en función de cómo se usan los datos de la variable. (En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).

Declare una variable mediante la palabra clave `var` (si no desea especificar un tipo) o con el nombre del tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

En el ejemplo siguiente se muestran algunos usos típicos de las variables en una página web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Si combina los ejemplos anteriores en una página, verá que se muestra en un explorador:

![Razor: Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Conversión y prueba de tipos de datos

Aunque ASP.NET normalmente puede determinar un tipo de datos de forma automática, a veces no es posible. Por lo tanto, es posible que necesite ayudar a ASP.NET a realizar una conversión explícita. Incluso si no tiene que convertir tipos, a veces resulta útil probar para ver el tipo de datos con el que puede trabajar.

El caso más común es que tenga que convertir una cadena en otro tipo, como un entero o una fecha. En el ejemplo siguiente se muestra un caso típico en el que debe convertir una cadena en un número.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Como regla, los datos proporcionados por el usuario se incluyen como cadenas. Incluso si ha solicitado que los usuarios escriban un número e incluso si han escrito un dígito, cuando se envía la entrada del usuario y lo lee en el código, los datos están en formato de cadena. Por lo tanto, debe convertir la cadena en un número. En el ejemplo, si intenta realizar operaciones aritméticas en los valores sin convertirlos, se producirá el siguiente error, porque ASP.NET no puede agregar dos cadenas:

*No se puede convertir implícitamente el tipo ' String ' a ' int '.*

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
    Convierte una cadena que representa un número entero (como "593") en un entero.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
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
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operadores

Un operador es una palabra clave o un carácter que indica a ASP.NET qué tipo de comando se debe realizar en una expresión. El C# lenguaje (y el sintaxis Razor que se basa en él) admite muchos operadores, pero solo tiene que reconocer algunos para comenzar. En la tabla siguiente se resumen los operadores más comunes.

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
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
    Operadores matemáticos usados en expresiones numéricas.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
    Asignación. Asigna el valor del lado derecho de una instrucción al objeto del lado izquierdo.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
    Igualdad. Devuelve `true` si los valores son iguales. (Observe la diferencia entre el operador `=` y el operador `==`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
    Desigualdad. Devuelve `true` si los valores no son iguales.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
    Menor que, mayor que, menor que o igual a, y mayor que o igual a.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
    Concatenación, que se usa para combinar cadenas. ASP.NET conoce la diferencia entre este operador y el operador de suma según el tipo de datos de la expresión.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
    Los operadores de incremento y decremento, que suman y restan 1 (respectivamente) de una variable.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
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
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
    Paréntesis. Se utiliza para agrupar expresiones y pasar parámetros a métodos.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
    Corchetes. Se utiliza para obtener acceso a los valores de matrices o colecciones.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
    Tampoco. Invierte un valor `true` en `false` y viceversa. Normalmente se utiliza como una forma abreviada de probar `false` (es decir, para no `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&&` `||`
    :::column-end:::
    :::column:::
    AND y OR lógicos, que se usan para vincular condiciones.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
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

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Referencia a la raíz virtual: el operador ~ y el método href

En un archivo *. cshtml* o *. vbhtml* , puede hacer referencia a la ruta de acceso de la raíz virtual mediante el operador `~`. Esto es muy útil porque puede mover páginas en un sitio y los vínculos que contienen a otras páginas no se romperán. También es útil en caso de que se mueva el sitio web a una ubicación diferente. A continuación se muestran algunos ejemplos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Si el sitio web está `http://myserver/myapp`, aquí se muestra cómo ASP.NET tratará estas rutas de acceso cuando se ejecute la página:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(En realidad, no verá estas rutas de acceso como los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso fuera).

Puede usar el operador `~` en el código del servidor (como arriba) y en el marcado, como se indica a continuación:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

En el marcado, se usa el operador `~` para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS. Cuando se ejecuta la página, ASP.NET busca en la página (código y marcado) y resuelve todas las referencias de `~` a la ruta de acceso adecuada.

## <a name="conditional-logic-and-loops"></a>Lógica y bucles condicionales

El código de servidor ASP.NET permite realizar tareas basadas en condiciones y escribir código que repite instrucciones un número específico de veces (es decir, código que ejecuta un bucle).

### <a name="testing-conditions"></a>Condiciones de prueba

Para probar una condición simple, use la instrucción `if`, que devuelve true o false en función de una prueba que especifique:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

La palabra clave `if` inicia un bloque. La prueba real (condición) está entre paréntesis y devuelve true o false. Las instrucciones que se ejecutan si la prueba es true se incluyen entre llaves. Una instrucción `if` puede incluir un bloque de `else` que especifica las instrucciones que se ejecutarán si la condición es falsa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Puede agregar varias condiciones mediante un bloque `else if`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

En este ejemplo, si la primera condición del bloque If no es true, se comprueba la condición `else if`. Si se cumple esta condición, se ejecutan las instrucciones del bloque `else if`. Si no se cumple ninguna de las condiciones, se ejecutan las instrucciones del bloque `else`. Puede agregar cualquier número de bloques else if y, a continuación, cerrar con un bloque `else` como el &quot;todo lo demás&quot; condición.

Para probar un gran número de condiciones, use un bloque `switch`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

El valor que se va a probar se encuentra entre paréntesis (en el ejemplo, la variable `weekday`). Cada prueba individual utiliza una instrucción `case` que termina con dos puntos (:). Si el valor de una instrucción `case` coincide con el valor de prueba, se ejecuta el código de ese bloque de casos. Cierre cada instrucción case con una instrucción `break`. (Si olvida incluir break en cada bloque `case`, el código de la siguiente instrucción `case` se ejecutará también). Un bloque `switch` suele tener una instrucción `default` como el último caso para un &quot;todo lo demás&quot; opción que se ejecuta si no se cumple ninguno de los otros casos.

El resultado de los dos últimos bloques condicionales mostrados en un explorador:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Código de bucle

A menudo es necesario ejecutar las mismas instrucciones varias veces. Para ello, puede crear un bucle. Por ejemplo, a menudo se ejecutan las mismas instrucciones para cada elemento en una colección de datos. Si sabe exactamente cuántas veces desea crear un bucle, puede usar un bucle `for`. Este tipo de bucle es especialmente útil para la acumulación o el recuento:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

El bucle comienza con la palabra clave `for`, seguido de tres instrucciones entre paréntesis, cada una termina con un punto y coma.

- Dentro de los paréntesis, la primera instrucción (`var i=10;`) crea un contador y lo inicializa en 10. No tiene que asignar un nombre al contador &#8212; `i` puede usar cualquier variable. Cuando se ejecuta el bucle `for`, el contador se incrementa automáticamente.
- La segunda instrucción (`i < 21;`) establece la condición de cuánto desea contar. En este caso, desea que pase a un máximo de 20 (es decir, continúe mientras el contador sea inferior a 21).
- La tercera instrucción (`i++`) utiliza un operador de incremento, que simplemente especifica que el contador debería tener 1 agregado cada vez que se ejecuta el bucle.

Dentro de las llaves está el código que se ejecutará para cada iteración del bucle. El marcado crea un nuevo párrafo (elemento`<p>`) cada vez y agrega una línea a la salida, mostrando el valor de `i` (el contador). Al ejecutar esta página, en el ejemplo se crean 11 líneas que muestran la salida, con el texto en cada línea que indica el número de elemento.

![Razor: Img11](introducing-razor-syntax-c/_static/image11.jpg)

Si está trabajando con una colección o una matriz, a menudo utiliza un bucle `foreach`. Una colección es un grupo de objetos similares y el bucle `foreach` le permite llevar a cabo una tarea en cada elemento de la colección. Este tipo de bucle es práctico para las colecciones, porque a diferencia de un bucle `for`, no tiene que incrementar el contador o establecer un límite. En su lugar, el código de bucle `foreach` simplemente continúa a través de la colección hasta que termina.

Por ejemplo, el código siguiente devuelve los elementos de la colección `Request.ServerVariables`, que es un objeto que contiene información sobre el servidor Web. Usa un bucle `foreac` h para mostrar el nombre de cada elemento mediante la creación de un nuevo elemento `<li>` en una lista con viñetas HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

La palabra clave `foreach` va seguida de paréntesis en el que se declara una variable que representa un único elemento de la colección (en el ejemplo, `var item`), seguido de la palabra clave `in`, seguido de la colección a la que se desea crear un bucle. En el cuerpo del bucle `foreach`, puede tener acceso al elemento actual mediante la variable que declaró anteriormente.

![Razor: Img12](introducing-razor-syntax-c/_static/image12.jpg)

Para crear un bucle de uso más general, use la instrucción `while`:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Un bucle `while` comienza con la palabra clave `while`, seguido de paréntesis en el que se especifica cuánto tiempo continúa el bucle (aquí, siempre y cuando `countNum` sea inferior a 50), después el bloque que se va a repetir. Los bucles suelen incrementar (agregar a) o disminuir (restar de) una variable o un objeto que se usa para el recuento. En el ejemplo, el operador `+=` agrega 1 a `countNum` cada vez que se ejecuta el bucle. (Para reducir una variable en un bucle que se recuenta, se usaría el operador de decremento `-=`).

## <a name="objects-and-collections"></a>Objetos y colecciones

Casi todo en un sitio web de ASP.NET es un objeto, incluida la propia página web. En esta sección se describen algunos objetos importantes con los que trabajará con frecuencia en el código.

### <a name="page-objects"></a>Objetos de página

El objeto más básico de ASP.NET es la página. Puede tener acceso directamente a las propiedades del objeto de página sin ningún objeto calificador. En el código siguiente se obtiene la ruta de acceso del archivo de la página mediante el `Request` objeto de la página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Para aclarar que está haciendo referencia a las propiedades y los métodos del objeto de página actual, puede usar opcionalmente la palabra clave `this` para representar el objeto de página en el código. Este es el ejemplo de código anterior, con `this` agregados para representar la página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Puede utilizar las propiedades del objeto `Page` para obtener una gran cantidad de información, como:

- `Request`Operador Como ya ha visto, se trata de una colección de información sobre la solicitud actual, incluido el tipo de explorador que realizó la solicitud, la dirección URL de la página, la identidad del usuario, etc.
- `Response`Operador Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando el código del servidor haya terminado de ejecutarse. Por ejemplo, puede usar esta propiedad para escribir información en la respuesta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de colección (matrices y diccionarios)

Una *colección* es un grupo de objetos del mismo tipo, como una colección de objetos `Customer` de una base de datos. ASP.NET contiene muchas colecciones integradas, como la `Request.Files` colección.

A menudo, trabajará con los datos de las colecciones. Dos tipos de colección comunes son la *matriz* y el *Diccionario*. Una matriz es útil cuando desea almacenar una colección de elementos similares pero no desea crear una variable independiente que contenga cada elemento:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Con las matrices, se declara un tipo de datos específico, como `string`, `int`o `DateTime`. Para indicar que la variable puede contener una matriz, agregue corchetes a la declaración (por ejemplo, `string[]` o `int[]`). Puede tener acceso a los elementos de una matriz mediante su posición (índice) o mediante la instrucción `foreach`. Los índices de matriz son de base &#8212; cero, es decir, el primer elemento está en la posición 0, el segundo elemento está en la posición 1, y así sucesivamente.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Puede determinar el número de elementos de una matriz obteniendo su `Length` propiedad. Para obtener la posición de un elemento específico en la matriz (para buscar en la matriz), use el método `Array.IndexOf`. También puede hacer cosas como invertir el contenido de una matriz (el método `Array.Reverse`) u ordenar el contenido (el método `Array.Sort`).

La salida del código de la matriz de cadenas que se muestra en un explorador:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Un diccionario es una colección de pares clave-valor, donde se proporciona la clave (o nombre) para establecer o recuperar el valor correspondiente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Para crear un diccionario, use la palabra clave `new` para indicar que está creando un nuevo objeto de diccionario. Puede asignar un diccionario a una variable mediante la palabra clave `var`. Los tipos de datos de los elementos del diccionario se indican mediante corchetes angulares (`< >`). Al final de la declaración, debe agregar un par de paréntesis, porque en realidad es un método que crea un nuevo diccionario.

Para agregar elementos al diccionario, puede llamar al método `Add` de la variable Dictionary (`myScores` en este caso) y, a continuación, especificar una clave y un valor. Como alternativa, puede usar corchetes para indicar la clave y realizar una asignación simple, como en el ejemplo siguiente:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Para obtener un valor del diccionario, especifique la clave entre corchetes:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Llamar a métodos con parámetros

Tal como se ha leído anteriormente en este artículo, los objetos con los que programa pueden tener métodos. Por ejemplo, un objeto de `Database` podría tener un método `Database.Connect`. Muchos métodos también tienen uno o más parámetros. Un *parámetro* es un valor que se pasa a un método para que el método pueda completar su tarea. Por ejemplo, examine una declaración para el método `Request.MapPath`, que toma tres parámetros:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(La línea se ha ajustado para que sea más legible. Recuerde que puede colocar los saltos de línea prácticamente en cualquier lugar excepto en las cadenas entre comillas).

Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada. Los tres parámetros del método son `virtualPath`, `baseVirtualDir`y `allowCrossAppMapping`. (Observe que en la declaración, los parámetros se enumeran con los tipos de datos de los datos que aceptarán). Cuando llame a este método, debe proporcionar valores para los tres parámetros.

El sintaxis Razor proporciona dos opciones para pasar parámetros a un método: *parámetros posicionales* y *parámetros con nombre*. Para llamar a un método mediante parámetros posicionales, se pasan los parámetros en un orden estricto que se especifica en la declaración del método. (Normalmente se conoce este orden leyendo la documentación del método). Debe seguir el orden y no puede omitir ninguno de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o `null` para un parámetro posicional para el que no tiene un valor.

En el ejemplo siguiente se da por supuesto que tiene una carpeta denominada *scripts* en el sitio Web. El código llama al método `Request.MapPath` y pasa los valores de los tres parámetros en el orden correcto. A continuación, se muestra la ruta de acceso asignada resultante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Cuando un método tiene muchos parámetros, puede mantener el código más legible mediante el uso de parámetros con nombre. Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de un signo de dos puntos (:) y, a continuación, el valor. La ventaja de los parámetros con nombre es que puede pasarlos en cualquier orden que desee. (Una desventaja es que la llamada al método no es tan compacta).

En el ejemplo siguiente se llama al mismo método que el anterior, pero se usan parámetros con nombre para proporcionar los valores:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Como puede ver, los parámetros se pasan en un orden diferente. Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverán el mismo valor.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Control de errores

### <a name="try-catch-statements"></a>Instrucciones try-catch

A menudo tendrá instrucciones en el código que podrían producir errores por motivos ajenos al control. Por ejemplo:

- Si el código intenta crear o tener acceso a un archivo, puede producirse todo tipo de errores. Es posible que el archivo que desea no exista, que esté bloqueado, que el código no tenga permisos, etc.
- Del mismo modo, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, que la conexión a la base de datos se haya quitado, que los datos que se van a guardar no sean válidos, etc.

En términos de programación, estas situaciones se denominan *excepciones*. Si el código encuentra una excepción, genera (lanza) un mensaje de error que, en el mejor de los usuarios, le molesta:

![Razor: Img14](introducing-razor-syntax-c/_static/image14.jpg)

En situaciones en las que el código pueda encontrar excepciones y para evitar los mensajes de error de este tipo, puede usar `try/catch` instrucciones. En la instrucción `try`, ejecute el código que está comprobando. En una o varias instrucciones de `catch`, puede buscar errores específicos (tipos específicos de excepciones) que puedan haberse producido. Puede incluir tantas instrucciones `catch` como necesite para buscar los errores que espera.

> [!NOTE]
> Se recomienda evitar el uso del método `Response.Redirect` en `try/catch` instrucciones, ya que puede producir una excepción en la página.

En el ejemplo siguiente se muestra una página que crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo. En el ejemplo se usa deliberadamente un nombre de archivo incorrecto para que se produzca una excepción. El código incluye `catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto y `DirectoryNotFoundException`, que se produce si ASP.NET no puede incluso encontrar la carpeta. (Puede quitar el comentario de una instrucción en el ejemplo para ver cómo se ejecuta cuando todo funciona correctamente).

Si el código no controla la excepción, verá una página de error como la captura de pantalla anterior. Sin embargo, la sección `try/catch` ayuda a evitar que el usuario vea estos tipos de errores.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

**Programar con Visual Basic**

[Apéndice: lenguaje de Visual Basic y sintaxis](https://go.microsoft.com/fwlink/?LinkId=202908)

**Documentación de referencia**

[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C#Módulo](https://msdn.microsoft.com/library/kx37x362.aspx)
