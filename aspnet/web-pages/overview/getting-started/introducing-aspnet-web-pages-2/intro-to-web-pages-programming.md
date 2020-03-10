---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Introducción a los conceptos básicos de la programación de ASP.NET Web Pages | Microsoft Docs
author: Rick-Anderson
description: 'En este tutorial se ofrece información general sobre cómo programar en ASP.NET Web Pages con sintaxis Razor. Lo que aprenderá: la sintaxis básica "Razor" que se usa para PR...'
ms.author: riande
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 474de7671ac2931e5ba9ff635d77385403644521
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509983"
---
# <a name="introducing-aspnet-web-pages---programming-basics"></a>Introducción a los conceptos básicos de la programación de ASP.NET Web Pages

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tutorial se ofrece información general sobre cómo programar en ASP.NET Web Pages con sintaxis Razor.
> 
> Temas que se abordarán:
> 
> - La sintaxis básica "Razor" que se usa para programar en ASP.NET Web Pages.
> - Algunos aspectos C#básicos, que es el lenguaje de programación que se va a usar.
> - Algunos conceptos fundamentales de programación para las páginas Web.
> - Cómo instalar paquetes (componentes que contienen código creado previamente) para usarlos con el sitio.
> - Cómo usar las *aplicaciones auxiliares* para realizar tareas de programación comunes.
>   
> 
> Características y tecnologías descritas:
> 
> - NuGet y el administrador de paquetes.
> - Aplicación auxiliar de `Gravatar`.

Este tutorial es principalmente un ejercicio en el que se presenta la sintaxis de programación que se usará para ASP.NET Web Pages. Conocerá *Sintaxis Razor* y el código escrito en el C# lenguaje de programación. En el tutorial anterior, tiene una visión de esta sintaxis; en este tutorial explicaremos la sintaxis más.

Prometemos que este tutorial implica la mayor parte de la programación que verá en un solo tutorial, y que es el único tutorial que *solo* trata sobre programación. En los tutoriales restantes de este conjunto, creará realmente páginas que hacen cosas interesantes.

También obtendrá información sobre las *aplicaciones auxiliares*. Una aplicación auxiliar es un componente (un fragmento de código empaquetado) que se puede Agregar a una página. El ayudante realiza un trabajo que, de lo contrario, podría ser tedioso o complejo de hacer a mano.

## <a name="creating-a-page-to-play-with-razor"></a>Crear una página para reproducirla con Razor

En esta sección, reproducirá un poco con Razor para que pueda hacerse una idea de la sintaxis básica.

Inicie WebMatrix si aún no se está ejecutando. Usará el sitio web que creó en el tutorial anterior ([Introducción con páginas web](https://go.microsoft.com/fwlink/?LinkId=251578)). Para volver a abrirlo, haga clic en **mis sitios** y elija **WebPageMovies**:

![Pantalla Inicio de WebMatrix que muestra las opciones del sitio abierto y mis sitios resaltados](intro-to-web-pages-programming/_static/image1.png)

Seleccione el área de trabajo **archivos** .

En la cinta de opciones, haga clic en **nuevo** para crear una página. Seleccione **CSHTML** y asigne a la nueva página el nombre *TestRazor. CSHTML*.

Haga clic en **Aceptar**.

Copie lo siguiente en el archivo, reemplazando completamente lo que ya existe.

> [!NOTE]
> Al copiar código o marcado de los ejemplos en una página, la sangría y la alineación podrían no ser las mismas que en el tutorial. Sin embargo, la sangría y la alineación no afectan al modo en que se ejecuta el código.

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Examinar la página de ejemplo

La mayor parte de lo que se ve es HTML normal. Sin embargo, en la parte superior, este bloque de código:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Tenga en cuenta lo siguiente sobre este bloque de código:

- El carácter @ indica a ASP.NET que lo que sigue es código Razor, no HTML. ASP.NET tratará todo lo que haya después del carácter @ como código hasta que vuelva a ejecutarse en HTML. (En este caso, eso es el &lt;. DOCTYPE&gt; elemento.
- Las llaves ({y}) encierran un bloque de código Razor si el código tiene más de una línea. Las llaves indican a ASP.NET dónde se inicia y finaliza el código para ese bloque.
- Los caracteres//marcan un comentario, es decir, una parte del código que no se ejecutará.
- Cada instrucción tiene que terminar con un punto y coma (;). (Sin embargo, no comentarios).
- Puede almacenar valores en *variables*, que crea (*declare*) con la palabra clave var. Cuando se crea una variable, se le asigna un nombre, que puede incluir letras, números y caracteres de subrayado (\_). Los nombres de variable no pueden comenzar con un número y no pueden usar el nombre de una palabra clave de programación (como var).
- Las cadenas de caracteres (como "ASP.NET" y "páginas web") se delimitan entre comillas. (Deben ser comillas dobles). Los números no están entre comillas.
- No importa espacios en blanco fuera de las comillas. Los saltos de línea no importan principalmente; la excepción es que no se puede dividir una cadena entre comillas entre las líneas. La sangría y la alineación no importan.

Algo que no es obvio en este ejemplo es que todo el código distingue mayúsculas de minúsculas. Esto significa que la variable TheSum es una variable diferente de las variables que se pueden denominar theSum o TheSum. Del mismo modo, var es una palabra clave, pero var no lo es.

### <a name="objects-and-properties-and-methods"></a>Objetos y propiedades y métodos

Después hay la expresión DateTime. Now. En términos simples, DateTime es un *objeto*. Un objeto es un elemento que se puede programar con (una página, un cuadro de texto, un archivo, una imagen, una solicitud Web, un mensaje de correo electrónico, un registro de cliente, etc.). Los objetos tienen una o más *propiedades* que describen sus características. Un objeto de cuadro de texto tiene una propiedad de texto (entre otros), un objeto de solicitud tiene una propiedad de dirección URL (y otros), un mensaje de correo electrónico tiene una propiedad From y una propiedad to, y así sucesivamente. Los objetos también tienen *métodos* que son los "verbos" que pueden realizar. Trabajará con objetos mucho.

Como puede ver en el ejemplo, DateTime es un objeto que le permite programar fechas y horas. Tiene una propiedad denominada Now que devuelve la fecha y hora actuales.

### <a name="using-code-to-render-markup-in-the-page"></a>Usar código para representar el marcado en la página

En el cuerpo de la página, observe lo siguiente:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

De nuevo, el carácter @ indica a ASP.NET que lo que sigue es código, no HTML. En el marcado puede Agregar @ seguido de una expresión de código y ASP.NET representará el valor de esa expresión justo en ese punto. En el ejemplo, @a representará cualquier valor de la variable denominada a, @product representa lo que hay en la variable denominada product, etc.

Sin embargo, no está limitado a las variables. En algunas instancias aquí, el carácter @ precede a una expresión:

- @ (a\*b) representa el producto de lo que se encuentra en las variables a y b. (El operador \* significa multiplicación).
- @ (Technology + "" + Product) representa los valores de la tecnología de variables y el producto después de concatenarlos y agregar un espacio entre ellos. El operador (+) para concatenar cadenas es igual que el operador para sumar números. Normalmente, ASP.NET puede indicar si está trabajando con números o con cadenas y hace lo correcto con el operador +.
- @Request.Url representa la propiedad URL del objeto de solicitud. El objeto de solicitud contiene información sobre la solicitud actual desde el explorador y, por supuesto, la propiedad URL contiene la dirección URL de esa solicitud actual.

El ejemplo también está diseñado para mostrarle que puede trabajar de maneras diferentes. Puede realizar cálculos en el bloque de código de la parte superior, colocar los resultados en una variable y, a continuación, representar la variable en el marcado. O bien, puede realizar cálculos en una expresión directamente en el marcado. El método que use dependerá de lo que esté haciendo y, en cierta medida, según sus preferencias.

### <a name="seeing-the-code-in-action"></a>Ver el código en acción

Haga clic con el botón secundario en el nombre del archivo y, a continuación, elija **iniciar en el explorador**. Verá la página en el explorador con todos los valores y las expresiones que se resuelven en la página.

![Página ' TestRazor ' en ejecución en el explorador](intro-to-web-pages-programming/_static/image2.png)

Examine el origen en el explorador.

![Origen de la página "probar Razor" en el explorador](intro-to-web-pages-programming/_static/image3.png)

Tal como se espera de su experiencia en el tutorial anterior, no hay ningún código Razor en la página. Lo único que ve son los valores de presentación reales. Al ejecutar una página, realmente está realizando una solicitud al servidor Web integrado en WebMatrix. Cuando se recibe la solicitud, ASP.NET resuelve todos los valores y las expresiones y representa sus valores en la página. A continuación, envía la página al explorador.

> [!TIP] 
> 
> **Razor yC#**
> 
> Hasta ahora hemos mencionado que está trabajando con sintaxis Razor. Eso es cierto, pero no es la historia completa. Se llama *C#* al lenguaje de programación real que está usando. C#fue creado por Microsoft hace una década y se ha convertido en uno de los principales lenguajes de programación para crear aplicaciones de Windows. Todas las reglas que ha visto sobre cómo asignar un nombre a una variable y cómo crear instrucciones, etc., son realmente todas las C# reglas del lenguaje.
> 
> Razor hace referencia más específicamente al conjunto reducido de convenciones sobre cómo insertar este código en una página. Por ejemplo, la Convención de usar @ para marcar el código en la página y usar @ {} para insertar un bloque de código es el aspecto de Razor de una página. Las aplicaciones auxiliares también se consideran parte de Razor. Sintaxis Razor se usa en más lugares que en ASP.NET Web Pages. (Por ejemplo, también se usa en las vistas de MVC de ASP.NET).
> 
> Lo mencionamos porque, si busca información sobre la programación ASP.NET Web Pages, encontrará una gran cantidad de referencias a Razor. Sin embargo, muchas de esas referencias no se aplican a lo que está haciendo y, por tanto, pueden resultar confusos. Y, de hecho, muchas de las preguntas de programación van a ser sobre cómo trabajar con C# ASP.net o trabajar con ellas. Por lo tanto, si busca específicamente información sobre Razor, puede que no encuentre las respuestas que necesita.

## <a name="adding-some-conditional-logic"></a>Agregar una lógica condicional

Una de las excelentes características sobre el uso de código en una página es que puede cambiar lo que sucede en función de las distintas condiciones. En esta parte del tutorial, reproducirá algunas maneras de cambiar lo que se muestra en la página.

El ejemplo será sencillo y ligeramente inventado para que podamos concentrarnos en la lógica condicional. La página que creará hará lo siguiente:

- Muestre texto diferente en la página dependiendo de si es la primera vez que se muestra la página o si ha hecho clic en un botón para enviar la página. Que será la primera prueba condicional.
- Mostrar el mensaje solo si se pasa un valor determinado en la cadena de consulta de la dirección URL (http://...? show = true). Que será la segunda prueba condicional.

En WebMatrix, cree una página y asígnele el nombre *TestRazorPart2. cshtml*. (En la cinta de opciones, haga clic en **nuevo**, elija **CSHTML**, asigne un nombre al archivo y, a continuación, haga clic en **Aceptar**).

Reemplace el contenido de esa página por lo siguiente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

El bloque de código de la parte superior Inicializa una variable denominada Message con texto. En el cuerpo de la página, el contenido de la variable de mensaje se muestra dentro de un elemento &lt;p&gt;. El marcado también contiene un elemento &lt;&gt; de entrada para crear un botón de **envío** .

Ejecute la página para ver cómo funciona ahora. Por ahora, es básicamente una página estática, aunque haga clic en el botón **Enviar** .

Vuelva a WebMatrix. Dentro del bloque de código, agregue el siguiente código resaltado *después* de la línea que inicializa el mensaje:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>El bloque if {}

Lo que acaba de agregar era una condición if. En el código, la condición If tiene una estructura como la siguiente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

La condición que se va a probar se encuentra entre paréntesis. Debe ser un valor o una expresión que devuelva true o false. Si la condición es true, ASP.NET ejecuta la instrucción o instrucciones que se encuentran dentro de las llaves. ( *Son parte de la lógica* *if-then* ). Si la condición es falsa, se omite el bloque de código.

Estos son algunos ejemplos de condiciones que puede probar en una instrucción If:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Puede probar variables con valores o con expresiones mediante un *operador lógico* o un *operador de comparación*: igual a (= =), mayor que (&gt;), menor que (&lt;), mayor o igual que (&gt;=) y menor o igual que (&lt;=). El operador! = significa que no es igual a (por ejemplo, si (a! = 0) significa que *no es igual a 0*.

> [!NOTE]
> Asegúrese de que observa que el operador de comparación para es igual a (= =) no es igual que =. El operador = solo se usa para asignar valores (var a = 2). Si combina estos operadores, recibirá un error o obtendrá algunos resultados más extraños.

Para probar si algo es true, la sintaxis completa es si (hace = = true). Pero también puede usar el método abreviado si (hace). Si no hay ningún operador de comparación, ASP.NET supone que está probando la verdadera.

El operador ! por sí solo significa un operador lógico NOT. Por ejemplo, la condición if (! IsPost) significa *si IsPost no es true*.

Puede combinar condiciones mediante un operador lógico AND (&amp;&amp;) o Logical OR (| | Operator). Por ejemplo, la última de las condiciones if de los ejemplos anteriores significa *si FileProcessingIsDone no está establecido en true y displayMessage está establecido en false*.

### <a name="the-else-block"></a>El bloque else

Una de las cosas finales sobre los bloques if: un bloque if puede ir seguido de un bloque else. Un bloque else es útil si tiene que ejecutar código diferente cuando la condición es falsa. A continuación se muestra un sencillo ejemplo:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

En los tutoriales posteriores de esta serie verá algunos ejemplos en los que el uso de un bloque else es útil.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Comprobar si la solicitud es un envío (post)

Hay más, pero vamos a volver al ejemplo, que tiene la condición if (IsPost) {...}. IsPost es realmente una propiedad de la página actual. La primera vez que se solicita la página, IsPost devuelve false. Sin embargo, si hace clic en un botón o envía la página; es decir, la publica, IsPost devuelve true. Por lo tanto, IsPost le permite determinar si está tratando con un envío de formulario. (En términos de verbos HTTP, si la solicitud es una operación GET, IsPost devuelve false. Si la solicitud es una operación POST, IsPost devuelve true). En un tutorial posterior, trabajará con los formularios de entrada, donde esta prueba resulta especialmente útil.

Ejecute la página. Dado que esta es la primera vez que solicita la página, verá "esta es la primera vez que solicitó la página". Esa cadena es el valor al que ha inicializado la variable de mensaje. Hay una prueba if (IsPost), pero que devuelve false en ese momento, por lo que el código dentro del bloque If no se ejecuta.

Haga clic en el botón **Enviar** . La página se vuelve a solicitar. Como antes, la variable de mensaje se establece en "esta es la primera vez...". Pero esta vez, la prueba if (IsPost) devuelve true, por lo que el código incluido en el bloque if se ejecuta. El código cambia el valor de la variable de mensaje a un valor diferente, que es lo que se representa en el marcado.

Ahora, agregue una condición if en el marcado. Debajo del elemento &lt;p&gt; que contiene el botón **Enviar** , agregue el siguiente marcado:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Va a agregar código dentro del marcado, por lo que tiene que empezar con @. Después hay una prueba IF similar a la que agregó anteriormente en el bloque de código. Dentro de las llaves, sin embargo, está agregando HTML ordinario, es decir, es ordinaria hasta que llega a @DateTime.Now. Este es otro poco de código Razor, por lo que debe agregar @ delante de él.

El punto aquí es que puede agregar condiciones if en el bloque de código en la parte superior y en el marcado. Si usa una condición if en el cuerpo de la página, las líneas dentro del bloque pueden ser de marcado o de código. En ese caso, y como es cierto siempre que mezcle el marcado y el código, debe usar @ para que quede claro a ASP.NET donde se encuentra el código.

Ejecute la página y haga clic en **Enviar**. Esta vez no solo ve un mensaje diferente cuando se envía ("ahora se ha enviado..."), pero aparece un mensaje nuevo que muestra la fecha y la hora.

![Página ' probar Razor 2 ' que se ejecuta en el explorador con la marca de tiempo que se muestra después del envío](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Probar el valor de una cadena de consulta

Una prueba más. Esta vez, agregará un bloque if que prueba un valor denominado show que podría pasarse en la cadena de consulta. (Similar a esto: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) Cambiará la página para que el mensaje que se muestra ("esta es la primera vez...", etc.) solo se muestra si el valor de show es true.

En la parte inferior (pero dentro) el bloque de código situado en la parte superior de la página, agregue lo siguiente:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

El bloque de código completo ahora tiene un aspecto similar al del ejemplo siguiente. (Recuerde que al copiar el código en la página, la sangría podría ser diferente. Pero esto no afecta al modo en que se ejecuta el código).

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

El nuevo código del bloque Inicializa una variable denominada showMessage en false. A continuación, realiza una prueba IF para buscar un valor en la cadena de consulta. La primera vez que se solicita la página, tiene una dirección URL como esta:

`http://localhost:43097/TestRazorPart2.cshtml`

El código determina si la dirección URL contiene una variable denominada show en la cadena de consulta, como esta versión de la dirección URL:

`http://localhost:43097/TestRazorPart2.cshtml`? show = true

La propia prueba examina la propiedad QueryString del objeto de solicitud. Si la cadena de consulta contiene un elemento denominado show y dicho elemento se establece en true, el bloque if se ejecuta y establece la variable showMessage en true.

Aquí hay un truco, como puede ver. Como se indica en el nombre, la cadena de consulta es una cadena. Sin embargo, solo puede probar true y false si el valor que está probando es un valor booleano (true/false). Antes de poder probar el valor de la variable show en la cadena de consulta, tiene que convertirlo en un valor booleano. Eso es lo que hace el método asbool: toma una cadena como entrada y la convierte en un valor booleano. Claramente, si la cadena es "true", el método asbool convierte ese valor en true. Si el valor de la cadena es cualquier otro, asbool devuelve false.

> [!TIP] 
> 
> **Tipos de datos y métodos as ()**
> 
> Solo hemos dicho que cuando cree una variable, use la palabra clave var. Sin embargo, eso no es todo. Para manipular los valores, para agregar números, concatenar cadenas o comparar fechas, o para probar true/false, C# tiene que trabajar con una representación interna adecuada del valor. C#*normalmente* , puede averiguar cuál debe ser la representación (es decir, el *tipo* de datos) en función de lo que está haciendo con los valores. Sin embargo, ahora y no es posible hacerlo. Si no es así, tiene que ayudarle indicando de C# forma explícita cómo debe representar los datos. El método asbool lo hace, lo que C# indica que un valor de cadena de "true" o "false" se debe tratar como un valor booleano. Existen métodos similares para representar cadenas como otros tipos también, como AsInt (tratar como entero), asdatetime (tratar como fecha y hora), asfloat (tratar como número de punto flotante), etc. Cuando se usan como métodos como (), si C# no puede representar el valor de cadena como se ha solicitado, verá un error.

En el marcado de la página, quite o comente este elemento (aquí se muestra comentado):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Justo donde haya quitado o comentado el texto, agregue lo siguiente:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

La prueba IF indica que si la variable showMessage es true, representa un elemento &lt;p&gt; con el valor de la variable Message.

### <a name="summary-of-your-conditional-logic"></a>Resumen de la lógica condicional

En caso de que no esté totalmente seguro de lo que acaba de hacer, aquí se muestra un resumen.

- La variable de mensaje se inicializa en una cadena predeterminada ("esta es la primera vez...").
- Si la solicitud de página es el resultado de un envío (post), el valor del mensaje cambia a "Now has enviado..."
- La variable showMessage se inicializa en false.
- Si la cadena de consulta contiene? show = true, la variable showMessage está establecida en true.
- En el marcado, si showMessage es true, se representa un elemento &lt;p&gt; que muestra el valor de Message. (Si showMessage es false, no se representa nada en ese punto en el marcado).
- En el marcado, si la solicitud es una publicación, se representa un elemento &lt;p&gt; que muestra la fecha y la hora.

Ejecute la página. No hay ningún mensaje, porque showMessage es false, por lo que en el marcado la prueba if (showMessage) devuelve false.

Haga clic en **Enviar**. Verá la fecha y la hora, pero sin ningún mensaje.

En el explorador, vaya al cuadro de dirección URL y agregue lo siguiente al final de la dirección URL:? show = true y, a continuación, presione Entrar.

![Página "probar Razor 2" del explorador que muestra la cadena de consulta](intro-to-web-pages-programming/_static/image5.png)

La página se muestra de nuevo. (Como ha cambiado la dirección URL, se trata de una nueva solicitud, no de un envío). Haga clic en **Enviar** de nuevo. El mensaje se muestra de nuevo, como la fecha y la hora.

![Página ' probar Razor 2 ' después del envío cuando hay una cadena de consulta](intro-to-web-pages-programming/_static/image6.png)

En la dirección URL, cambie? show = true a? show = false y presione Entrar. Vuelva a enviar la página. La página vuelve a empezar, sin mensaje.

Como se indicó anteriormente, la lógica de este ejemplo es un poco inventado. Sin embargo, si va a aparecer en muchas de sus páginas y toma uno o varios de los formularios que ha visto aquí.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Instalación de una aplicación auxiliar (Mostrar una imagen de Gravatar)

Algunas tareas que los usuarios suelen querer realizar en páginas Web requieren mucho código o requieren conocimientos adicionales. Ejemplos: mostrar un gráfico para datos; colocar un botón "like" de Facebook en una página; enviar correo electrónico desde su sitio web; recortar o cambiar el tamaño de las imágenes; usar PayPal para su sitio. Para facilitar la creación de estos tipos de cosas, ASP.NET Web Pages le permite usar *aplicaciones auxiliares*. Las aplicaciones auxiliares son componentes que se instalan para un sitio y que permiten realizar tareas típicas mediante el uso de unas pocas líneas de código Razor.

ASP.NET Web Pages tiene algunas aplicaciones auxiliares integradas. Sin embargo, hay muchas aplicaciones auxiliares disponibles en paquetes (complementos) que se proporcionan mediante el administrador de paquetes NuGet. NuGet le permite seleccionar un paquete para instalarlo y, a continuación, se ocupa de todos los detalles de la instalación.

En esta parte del tutorial, instalará una aplicación auxiliar que le permite mostrar una imagen de Gravatar ("Avatar Globally reconocid"). Aprenderá dos cosas. Uno es cómo buscar e instalar una aplicación auxiliar. También aprenderá cómo una aplicación auxiliar facilita hacer algo que, de otro modo, tendría que hacer con una gran cantidad de código que tendría que escribir.

Puede registrar su propia Gravatar en el sitio web de Gravatar en [http://www.gravatar.com/](http://www.gravatar.com/), pero no es esencial crear una cuenta de Gravatar para realizar esta parte del tutorial.

En WebMatrix, haga clic en el botón **NuGet** .

![Cuadro de diálogo Galería de NuGet en WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Se inicia el administrador de paquetes NuGet y se muestran los paquetes disponibles. (No todos los paquetes son aplicaciones auxiliares; algunos agregan funcionalidad a WebMatrix, algunas son plantillas adicionales, etc.). Puede obtener un mensaje de error sobre la incompatibilidad de versiones. Para pasar por alto este mensaje de error, haga clic en **Aceptar** y continúe con este tutorial.

![Cuadro de diálogo Galería de NuGet en WebMatrix](intro-to-web-pages-programming/_static/image8.png)

En el cuadro de búsqueda, escriba "aplicaciones auxiliares de asp.net". NuGet muestra los paquetes que coinciden con los términos de búsqueda.

![Galería de NuGet en WebMatrix que muestra paquetes](intro-to-web-pages-programming/_static/image9.png)

La biblioteca web helpers de ASP.NET contiene código para simplificar muchas tareas comunes, incluido el uso de imágenes de Gravatar. Seleccione el paquete de la **biblioteca de aplicaciones auxiliares Web ASP.net** y, a continuación, haga clic en **instalar** para iniciar el instalador. Seleccione **sí** cuando se le pregunte si desea instalar el paquete y acepte los términos para completar la instalación.

Ya está. NuGet descarga e instala todo, incluidos los componentes adicionales que podrían ser necesarios (*dependencias*).

Si, por alguna razón, tiene que desinstalar una aplicación auxiliar, el proceso es muy similar. Haga clic en el botón **NuGet** , haga clic en la pestaña **instalado** y seleccione el paquete que desea desinstalar.

## <a name="using-a-helper-in-a-page"></a>Usar una aplicación auxiliar en una página

Ahora usará el ayudante que acaba de instalar. El proceso para agregar una aplicación auxiliar a una página es similar para la mayoría de las aplicaciones auxiliares.

En WebMatrix, cree una página y asígnele el nombre *GravatarTest. cshml*. (Va a crear una página especial para probar el ayudante, pero puede usar las aplicaciones auxiliares en cualquier página del sitio).

Dentro del elemento &lt;cuerpo&gt;, agregue un elemento &lt;div&gt;. En el elemento &lt;div&gt;, escriba lo siguiente:

@GravatarOperador

El carácter @ es el mismo carácter que ha utilizado para marcar el código Razor. **Gravatar** es el objeto auxiliar con el que está trabajando.

En cuanto escribe el punto (.), WebMatrix muestra una lista de *los métodos* (funciones) que la aplicación auxiliar Gravatar pone a disposición:

![Lista desplegable de IntelliSense de la aplicación auxiliar Gravatar](intro-to-web-pages-programming/_static/image10.png)

Esta característica se conoce como *IntelliSense*. Le ayuda a codificar proporcionando opciones adecuadas para el contexto. IntelliSense funciona con código HTML, CSS, ASP.NET, JavaScript y otros lenguajes que se admiten en WebMatrix. Es otra característica que facilita el desarrollo de páginas web en WebMatrix.

Presione G en el teclado y verá que IntelliSense encuentra el método GetHtml. Presione TAB. IntelliSense inserta el método seleccionado (GetHtml). Escriba un paréntesis de apertura y observe que el paréntesis de cierre se agrega automáticamente. Escriba la dirección de correo electrónico entre comillas entre los dos paréntesis. Si tiene una cuenta de Gravatar, se devolverá la imagen de perfil. Si no tiene una cuenta de Gravatar, se devuelve una imagen predeterminada. Cuando haya terminado, la línea tendrá el siguiente aspecto:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Ahora vea la página en un explorador. Se muestra la imagen o la imagen predeterminada, en función de si tiene una cuenta de Gravatar.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![imagen predeterminada](intro-to-web-pages-programming/_static/image12.png)

Para hacerse una idea de lo que está haciendo el ayudante, vea el origen de la página en el explorador. Junto con el código HTML que tenía en la página, verá un elemento de imagen que incluye un identificador. Este es el código que el ayudante representa en la página en el lugar donde se ha @Gravatar.GetHtml. El ayudante tomó la información proporcionada y generó el código que se comunica directamente con Gravatar para recuperar la imagen correcta de la cuenta proporcionada.

El método GetHtml también le permite personalizar la imagen proporcionando otros parámetros. En el código siguiente se muestra cómo solicitar una imagen con un ancho y un alto de 40 píxeles, y se usa una imagen predeterminada especificada denominada **Wavatar** si la cuenta especificada no existe.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Este código genera algo parecido al resultado siguiente (la imagen predeterminada variará de forma aleatoria).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Próxima

Para seguir este tutorial, teníamos que centrarnos solo en unos pocos aspectos básicos. Naturalmente, hay *mucho* más para Razor y C#. Aprenderá más a medida que avanza por estos tutoriales. Si está interesado en obtener más información sobre los aspectos de programación de Razor C# y ahora puede leer una introducción más exhaustiva aquí: [Introducción a la programación web de ASP.net mediante la sintaxis de Razor](https://go.microsoft.com/fwlink/?LinkID=202890).

En el siguiente tutorial se explica cómo trabajar con una base de datos de. En ese tutorial, comenzará a crear la aplicación de ejemplo que le permite enumerar sus películas favoritas.

## <a name="complete-listing-for-testrazor-page"></a>Lista completa de la página de TestRazor

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Lista completa de la página de TestRazorPart2

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Lista completa de la página de GravatarTest

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación web de ASP.NET mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Aplicación auxiliar de Twitter](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Siguiente](displaying-data.md)
