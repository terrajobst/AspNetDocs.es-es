---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: Mostrar datos en un gráfico con ASP.NET Web Pages (Razor) | Microsoft Docs
author: microsoft
description: En este capítulo se explica cómo mostrar los datos en un gráfico. En los capítulos anteriores, aprendió a Mostrar datos manualmente y en una cuadrícula. En este capítulo se explica...
ms.author: riande
ms.date: 05/22/2012
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 6dad67d4e3d38d57a761c567d937d714a3184ea9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78509137"
---
# <a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>Mostrar datos en un gráfico con ASP.NET Web Pages (Razor)

por [Microsoft](https://github.com/microsoft)

> En este artículo se explica cómo usar un gráfico para Mostrar datos en un sitio web de ASP.NET Web Pages (Razor) mediante la aplicación auxiliar de `Chart`.
> 
> **Lo que aprenderá**:
> 
> - Cómo mostrar los datos en un gráfico.
> - Cómo aplicar estilos a los gráficos mediante temas integrados.
> - Cómo guardar gráficos y cómo almacenarlos en memoria caché para mejorar el rendimiento.
> 
> Estas son las características de programación de ASP.NET que se introdujeron en el artículo:
> 
> - Aplicación auxiliar de `Chart`.
> 
> > [!NOTE]
> > La información de este artículo se aplica a ASP.NET Web Pages 1,0 y páginas web 2.

<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Aplicación auxiliar de gráficos

Si desea mostrar los datos en formato gráfico, puede utilizar la aplicación auxiliar `Chart`. La aplicación auxiliar de `Chart` puede representar una imagen que muestra los datos en diversos tipos de gráficos. Admite muchas opciones de formato y etiquetado. La aplicación auxiliar de `Chart` puede representar más de 30 tipos de gráficos, incluidos todos los tipos de gráficos con los que puede estar familiarizado desde Microsoft Excel u &#8212; otros gráficos de áreas de herramientas, gráficos de barras, gráficos de columnas, gráficos de líneas y gráficos circulares, junto con gráficos más especializados como gráficos de cotizaciones.

| **Gráfico de áreas** ![Descripción: imagen del tipo de gráfico de áreas](7-displaying-data-in-a-chart/_static/image1.jpg) | **Gráfico de barras** ![Descripción: imagen del tipo de gráfico de barras](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Gráfico de columnas** ![Descripción: imagen del tipo de gráfico de columnas](7-displaying-data-in-a-chart/_static/image3.jpg) | **Gráfico de líneas** ![Descripción: imagen del tipo de gráfico de líneas](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Gráfico circular** ![Descripción: imagen del tipo de gráfico circular](7-displaying-data-in-a-chart/_static/image5.jpg) | **Gráfico de cotizaciones** ![Descripción: imagen del tipo de gráfico de cotizaciones](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Elemento de gráfico

Los gráficos muestran datos y elementos adicionales como leyendas, ejes, series, etc. En la imagen siguiente se muestran muchos de los elementos de gráfico que se pueden personalizar al utilizar la aplicación auxiliar `Chart`. En este artículo se muestra cómo establecer algunos (no todos) de estos elementos.

![Descripción: imagen que muestra los elementos del gráfico](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Crear un gráfico a partir de datos

Los datos que se muestran en un gráfico pueden ser de una matriz, de los resultados devueltos de una base de datos o de los datos que se encuentra en un archivo XML.

### <a name="using-an-array"></a>Usar una matriz

Como se explica en [Introducción a la programación de ASP.NET Web pages mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890), una matriz permite almacenar una colección de elementos similares en una sola variable. Puede usar matrices para contener los datos que desea incluir en el gráfico.

Este procedimiento muestra cómo se puede crear un gráfico a partir de los datos de las matrices, utilizando el tipo de gráfico predeterminado. También muestra cómo mostrar el gráfico dentro de la página.

1. Cree un nuevo archivo llamado *ChartArrayBasic. cshtml*.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    En primer lugar, el código crea un nuevo gráfico y establece su ancho y alto. El título del gráfico se especifica mediante el método `AddTitle`. Para agregar datos, use el método `AddSeries`. En este ejemplo, se usan los parámetros `name`, `xValue`y `yValues` del método `AddSeries`. El parámetro `name` se muestra en la leyenda del gráfico. El parámetro `xValue` contiene una matriz de datos que se muestra a lo largo del eje horizontal del gráfico. El parámetro `yValues` contiene una matriz de datos que se usa para trazar los puntos verticales del gráfico.

    En realidad, el método `Write` representa el gráfico. En este caso, como no especificó un tipo de gráfico, el ayudante de `Chart` representa su gráfico predeterminado, que es un gráfico de columnas.
3. Ejecute la página en el explorador. El explorador muestra el gráfico. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Usar una consulta de base de datos para los datos de gráfico

Si la información que desea incluir en el gráfico se encuentra en una base de datos, puede ejecutar una consulta de base de datos y, a continuación, utilizar los datos de los resultados para crear el gráfico. En este procedimiento se muestra cómo leer y mostrar los datos de la base de datos creada en el artículo [Introducción al trabajo con una base de datos en ASP.NET Web pages sitios](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Agregue una carpeta de *\_de datos* de la aplicación a la raíz del sitio web si la carpeta no existe todavía.
2. En la carpeta *App\_Data* , agregue el archivo de base de datos denominado *SmallBakery. sdf* que se describe en [Introducción al trabajo con una base de datos en sitios de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Cree un nuevo archivo llamado *ChartDataQuery. cshtml*.
4. Reemplace el contenido existente por lo siguiente:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    El código abre primero la base de datos SmallBakery y la asigna a una variable denominada `db`. Esta variable representa un objeto `Database` que se puede utilizar para leer y escribir en la base de datos. A continuación, el código ejecuta una consulta SQL para obtener el nombre y el precio de cada producto. El código crea un nuevo gráfico y le pasa la consulta de base de datos llamando al método `DataBindTable` del gráfico. Este método toma dos parámetros: el parámetro `dataSource` es para los datos de la consulta y el parámetro `xField` permite establecer qué columna de datos se utiliza para el eje x del gráfico.

    Como alternativa al uso del método `DataBindTable`, puede utilizar el método `AddSeries` de la aplicación auxiliar `Chart`. El método `AddSeries` permite establecer los parámetros `xValue` y `yValues`. Por ejemplo, en lugar de usar el método `DataBindTable` como este:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Puede usar el método `AddSeries` como el siguiente:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Ambos representan los mismos resultados. El método `AddSeries` es más flexible, ya que puede especificar el tipo de gráfico y los datos más explícitamente, pero el método `DataBindTable` es más fácil de usar si no necesita la flexibilidad adicional.
5. Ejecute la página en un explorador. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>Usar datos XML

La tercera opción para los gráficos es usar un archivo XML como los datos para el gráfico. Esto requiere que el archivo XML también tenga un archivo de esquema (archivo *. xsd* ) que describa la estructura XML. En este procedimiento se muestra cómo leer datos de un archivo XML.

1. En la carpeta *App\_Data* , cree un nuevo archivo XML denominado *Data. XML*.
2. Reemplace el XML existente por lo siguiente, que contiene algunos datos XML acerca de los empleados de una compañía ficticia. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. En la carpeta *App\_Data* , cree un nuevo archivo XML denominado *Data. xsd*. (Tenga en cuenta que la extensión es *. xsd*).
4. Reemplace el XML existente por lo siguiente: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. En la raíz del sitio web, cree un nuevo archivo denominado *ChartDataXML. cshtml*.
6. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    En primer lugar, el código crea un objeto `DataSet`. Este objeto se usa para administrar los datos que se leen desde el archivo XML y los organiza en función de la información del archivo de esquema. (Observe que la parte superior del código incluye la instrucción `using SystemData`. Esto es necesario para poder trabajar con el objeto de `DataSet`. Para obtener más información, vea [&quot;usar instrucciones de&quot; y nombres completos](#SB_UsingStatements) más adelante en este artículo).

    A continuación, el código crea un objeto de `DataView` basado en el conjunto de DataSet. La vista de datos proporciona un objeto al &#8212; que se puede enlazar el gráfico; es decir, lectura y trazado. El gráfico se enlaza a los datos mediante el método `AddSeries`, como se vio antes al representar los datos de la matriz, excepto en que esta vez los parámetros `xValue` y `yValues` se establecen en el objeto `DataView`.

    En este ejemplo también se muestra cómo especificar un tipo de gráfico determinado. Cuando los datos se agregan en el método `AddSeries`, el parámetro `chartType` también se establece para mostrar un gráfico circular.
7. Ejecute la página en un explorador. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>Instrucciones "Using" y nombres completos
> 
> El .NET Framework que ASP.NET Web Pages con sintaxis Razor se basa en se compone de muchos miles de componentes (clases). Para que sea manejable trabajar con todas estas clases, se organizan en *espacios de nombres*, que son similares a las bibliotecas. Por ejemplo, el espacio de nombres `System.Web` contiene clases que admiten la comunicación entre exploradores y servidores, el espacio de nombres `System.Xml` contiene clases que se usan para crear y leer archivos XML, y el espacio de nombres `System.Data` contiene clases que permiten trabajar con datos.
> 
> Para tener acceso a cualquier clase determinada en el .NET Framework, el código necesita saber no solo el nombre de clase, sino también el espacio de nombres en el que se encuentra la clase. Por ejemplo, para usar la aplicación auxiliar `Chart`, el código necesita encontrar la clase `System.Web.Helpers.Chart`, que combina el espacio de nombres (`System.Web.Helpers`) con el nombre de clase (`Chart`). Esto se conoce como el nombre &#8212; *completo* de la clase de su ubicación completa e inequívoca dentro de la inmensaidad del .NET Framework. En el código, esto sería similar al siguiente:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Sin embargo, es engorroso (y propenso a errores) tener que usar estos nombres completos y largos cada vez que desea hacer referencia a una clase o aplicación auxiliar. Por lo tanto, para facilitar el uso de nombres de clase, puede *importar* los espacios de nombres que le interesan, que suele ser un puñado de entre los muchos espacios de nombres del .NET Framework. Si ha importado un espacio de nombres, puede usar solo un nombre de clase (`Chart`) en lugar del nombre completo (`System.Web.Helpers.Chart`). Cuando el código se ejecuta y encuentra un nombre de clase, puede buscar en solo los espacios de nombres que ha importado para encontrar esa clase.
> 
> Cuando se usa ASP.NET Web Pages con sintaxis Razor para crear páginas web, normalmente se usa el mismo conjunto de clases cada vez, incluidas la clase `WebPage`, las diversas aplicaciones auxiliares, etc. Para ahorrarle el trabajo de importar los espacios de nombres relevantes cada vez que cree un sitio web, ASP.NET se configura de modo que importe automáticamente un conjunto de espacios de nombres principales para cada sitio Web. Esta es la razón por la que no ha tenido que tratar con los espacios de nombres ni importar hasta ahora; todas las clases con las que ha trabajado se encuentran en espacios de nombres que ya se han importado.
> 
> Sin embargo, a veces es necesario trabajar con una clase que no está en un espacio de nombres que se importa automáticamente. En ese caso, puede usar el nombre completo de la clase, o bien puede importar manualmente el espacio de nombres que contiene la clase. Para importar un espacio de nombres, use la instrucción `using` (`import` en Visual Basic), como vio en un ejemplo anterior de este artículo.
> 
> Por ejemplo, la clase `DataSet` está en el espacio de nombres `System.Data`. El espacio de nombres `System.Data` no está disponible automáticamente para las páginas de Razor ASP.NET. Por lo tanto, para trabajar con la clase `DataSet` utilizando su nombre completo, puede usar código como el siguiente:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Si tiene que usar la clase `DataSet` repetidamente, puede importar un espacio de nombres como este y, a continuación, usar solo el nombre de clase en el código:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Puede Agregar `using` instrucciones para cualquier otro espacio de nombres de .NET Framework al que desee hacer referencia. Sin embargo, como se indicó, no tendrá que hacer esto a menudo, porque la mayoría de las clases con las que trabajará se encuentran en espacios de nombres que se importan automáticamente mediante ASP.NET para su uso en las páginas *. cshtml* y *. vbhtml* .

<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Mostrar gráficos dentro de una página web

En los ejemplos que se han visto hasta ahora, se crea un gráfico y, a continuación, el gráfico se representa directamente en el explorador como un gráfico. Sin embargo, en muchos casos, desea mostrar un gráfico como parte de una página, no solo en el explorador. Para ello, es necesario un proceso de dos pasos. El primer paso es crear una página que genere el gráfico, tal como ya se ha visto.

El segundo paso es mostrar la imagen resultante en otra página. Para mostrar la imagen, use un elemento de `<img>` HTML, de la misma manera que mostraría cualquier imagen. Sin embargo, en lugar de hacer referencia a un archivo *. jpg* o *. png* , el elemento `<img>` hace referencia al archivo *. cshtml* que contiene la aplicación auxiliar de `Chart` que crea el gráfico. Cuando se ejecuta la página de visualización, el elemento `<img>` obtiene la salida de la aplicación auxiliar de `Chart` y representa el gráfico.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Cree un archivo denominado *ShowChart. cshtml*.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    El código utiliza el elemento `<img>` para mostrar el gráfico que creó anteriormente en el archivo *ChartArrayBasic. cshtml* .
3. Ejecute la página web en un explorador. El archivo *ShowChart. cshtml* muestra la imagen del gráfico en función del código contenido en el archivo *ChartArrayBasic. cshtml* .

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Aplicar estilos a un gráfico

La aplicación auxiliar de `Chart` admite un gran número de opciones que permiten personalizar la apariencia del gráfico. Puede establecer colores, fuentes, bordes, etc. Una manera fácil de personalizar la apariencia de un gráfico es usar un *tema*. Los temas son colecciones de información que especifican cómo presentar un gráfico utilizando fuentes, colores, etiquetas, paletas, bordes y efectos. (Tenga en cuenta que el estilo de un gráfico no indica el tipo de gráfico).

En la tabla siguiente se enumeran los temas integrados.

| Tema | Description |
| --- | --- |
| `Vanilla` | Muestra las columnas rojas en un fondo blanco. |
| `Blue` | Muestra las columnas azules en un fondo de degradado azul. |
| `Green` | Muestra las columnas azules en un fondo de degradado verde. |
| `Yellow` | Muestra las columnas naranja en un fondo de degradado amarillo. |
| `Vanilla3D` | Muestra las columnas 3D en rojo en un fondo blanco. |

Puede especificar el tema que se va a utilizar al crear un nuevo gráfico.

1. Cree un nuevo archivo llamado *ChartStyleGreen. cshtml*.
2. Reemplace el contenido existente en la página por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Este código es el mismo que el ejemplo anterior que usa la base de datos para los datos, pero agrega el parámetro `theme` cuando crea el objeto `Chart`. A continuación se muestra el código modificado:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Ejecute la página en un explorador. Verá los mismos datos que antes, pero el gráfico tiene un aspecto más pulido: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Guardar un gráfico

Cuando se usa la aplicación auxiliar `Chart` como se ha visto hasta ahora en este artículo, el ayudante vuelve a crear el gráfico desde el principio cada vez que se invoca. Si es necesario, el código del gráfico también vuelve a consultar la base de datos o vuelve a leer el archivo XML para obtener los datos. En algunos casos, puede tratarse de una operación compleja, por ejemplo, si la base de datos que está consultando es grande o si el archivo XML contiene una gran cantidad de datos. Incluso si el gráfico no implica muchos datos, el proceso de creación dinámica de una imagen ocupa recursos del servidor y, si muchas personas solicitan la página o las páginas que muestran el gráfico, puede haber un impacto en el rendimiento del sitio Web.

Para ayudar a reducir el impacto potencial en el rendimiento de la creación de un gráfico, puede crear un gráfico la primera vez que lo necesite y, después, guardarlo. Cuando vuelva a ser necesario el gráfico, en lugar de volver a generarlo, puede capturar la versión guardada y representarla.

Puede guardar un gráfico de las siguientes maneras:

- Almacene en caché el gráfico en la memoria del equipo (en el servidor).
- Guarde el gráfico como un archivo de imagen.
- Guarde el gráfico como un archivo XML. Esta opción le permite modificar el gráfico antes de guardarlo.

### <a name="caching-a-chart"></a>Almacenar en caché un gráfico

Después de crear un gráfico, puede almacenarlo en caché. Almacenar en caché un gráfico significa que no es necesario volver a crearlo si debe volver a mostrarse. Al guardar un gráfico en la memoria caché, se le asigna una clave que debe ser única para ese gráfico.

Es posible que se quiten los gráficos guardados en la memoria caché si el servidor tiene poca memoria. Además, la memoria caché se borra si la aplicación se reinicia por cualquier motivo. Por lo tanto, la manera estándar de trabajar con un gráfico en caché es comprobar siempre si está disponible en la memoria caché y, si no es así, crearlo o volver a crearlo.

1. En la raíz de su sitio web, cree un archivo denominado *ShowCachedChart. cshtml*.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    La etiqueta `<img>` incluye un atributo `src` que apunta al archivo *ChartSaveToCache. cshtml* y pasa una clave a la página como una cadena de consulta. La clave contiene el valor &quot;myChartKey&quot;. El archivo *ChartSaveToCache. cshtml* contiene el ayudante `Chart` que crea el gráfico. Creará esta página en un momento.

    Al final de la página hay un vínculo a una página denominada *ClearCache. cshtml*. Esta es una página que también creará en breve. Solo necesita *ClearCache. cshtml* para probar el almacenamiento en caché en este ejemplo, no es un vínculo o página que normalmente incluiría al trabajar con gráficos en caché.
3. En la raíz de su sitio web, cree un nuevo archivo denominado *ChartSaveToCache. cshtml*.
4. Reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    En primer lugar, el código comprueba si se ha pasado algo como valor de clave en la cadena de consulta. Si es así, el código intenta leer un gráfico de la memoria caché llamando al método `GetFromCache` y pasándole la clave. Si no hay nada en la memoria caché bajo esa clave (que se produciría la primera vez que se solicita el gráfico), el código crea el gráfico como de costumbre. Una vez finalizado el gráfico, el código lo guarda en la memoria caché mediante una llamada a `SaveToCache`. Ese método requiere una clave (para que el gráfico se pueda solicitar más adelante) y la cantidad de tiempo que el gráfico se debe guardar en la memoria caché. (El tiempo exacto en el que se podía almacenar en caché un gráfico dependería de la frecuencia con la que los datos que representa podrían cambiar). El método `SaveToCache` también requiere un parámetro &#8212; `slidingExpiration` si se establece en true, se restablece el contador de tiempo de espera cada vez que se tiene acceso al gráfico. En este caso, en efecto significa que la entrada de caché del gráfico expira 2 minutos después de la última vez que un usuario tuvo acceso al gráfico. (La alternativa a la expiración variable es la expiración absoluta, lo que significa que la entrada de caché expiraría exactamente 2 minutos después de colocarse en la memoria caché, independientemente de la frecuencia con la que se había tenido acceso a ella).

    Por último, el código utiliza el método `WriteFromCache` para capturar y representar el gráfico de la memoria caché. Tenga en cuenta que este método está fuera del bloque `if` que comprueba la memoria caché, ya que obtendrá el gráfico de la memoria caché independientemente de si el gráfico se ha podido iniciar o debe generarse y guardarse en la memoria caché.

    Observe que en el ejemplo, el método `AddTitle` incluye una marca de tiempo. (Agrega la fecha y hora &#8212; actuales `DateTime.Now` &#8212; al título).
5. Cree una nueva página denominada *ClearCache. cshtml* y reemplace su contenido por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    En esta página se usa la aplicación auxiliar de `WebCache` para quitar el gráfico almacenado en caché en *ChartSaveToCache. cshtml*. Como se indicó anteriormente, normalmente no tiene que tener una página como esta. Solo se crea aquí para que sea más fácil probar el almacenamiento en caché.
6. Ejecute la Página Web de *ShowCachedChart. cshtml* en un explorador. La página muestra la imagen del gráfico en función del código contenido en el archivo *ChartSaveToCache. cshtml* . Tome nota de lo que la marca de tiempo indica en el título del gráfico. 

    ![Descripción: imagen del gráfico básico con marca de tiempo en el título del gráfico](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Cierre el explorador.
8. Vuelva a ejecutar *ShowCachedChart. cshtml* . Observe que la marca de tiempo es igual que antes, lo que indica que el gráfico no se regeneró, sino que se leyó en su lugar desde la memoria caché.
9. En *ShowCachedChart. cshtml*, haga clic en el vínculo **Borrar caché** . Esto le lleva a *ClearCache. cshtml*, que informa de que se ha borrado la memoria caché.
10. Haga clic en el vínculo **volver a ShowCachedChart. cshtml** o vuelva a ejecutar *ShowCachedChart. cshtml* desde WebMatrix. Tenga en cuenta que esta vez la marca de tiempo ha cambiado porque se ha borrado la memoria caché. Por lo tanto, el código tenía que volver a generar el gráfico y colocarlo de nuevo en la memoria caché.

### <a name="saving-a-chart-as-an-image-file"></a>Guardar un gráfico como un archivo de imagen

También puede guardar un gráfico como un archivo de imagen (por ejemplo, como un archivo *. jpg* ) en el servidor. Después, puede usar el archivo de imagen como lo haría con cualquier imagen. La ventaja es que el archivo se almacena en lugar de guardarse en una caché temporal. Puede guardar una nueva imagen de gráfico en momentos diferentes (por ejemplo, cada hora) y, a continuación, mantener un registro permanente de los cambios que se producen a lo largo del tiempo. Tenga en cuenta que debe asegurarse de que la aplicación web tiene permiso para guardar un archivo en la carpeta del servidor en el que desea colocar el archivo de imagen.

1. En la raíz de su sitio web, cree una carpeta denominada *\_ChartFiles* si aún no existe.
2. En la raíz de su sitio web, cree un nuevo archivo denominado *ChartSave. cshtml*.
3. Reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    El código comprueba primero para ver si existe el archivo *. jpg* llamando al método `File.Exists`. Si el archivo no existe, el código crea un nuevo `Chart` a partir de una matriz. Esta vez, el código llama al método `Save` y pasa el parámetro `path` para especificar la ruta de acceso del archivo y el nombre del archivo donde se guardará el gráfico. En el cuerpo de la página, un elemento `<img>` usa la ruta de acceso para que apunte al archivo *. jpg* que se va a mostrar.
4. Ejecute el archivo *ChartSave. cshtml* .
5. Vuelva a WebMatrix. Observe que se ha guardado un archivo de imagen denominado *chart01. jpg* en la carpeta *\_ChartFiles*

### <a name="saving-a-chart-as-an-xml-file"></a>Guardar un gráfico como un archivo XML

Por último, puede guardar un gráfico como un archivo XML en el servidor. Una ventaja de usar este método para almacenar en caché el gráfico o guardar el gráfico en un archivo es que puede modificar el código XML antes de mostrar el gráfico si lo desea. La aplicación debe tener permisos de lectura y escritura para la carpeta en el servidor donde desea colocar el archivo de imagen.

1. En la raíz de su sitio web, cree un nuevo archivo denominado *ChartSaveXml. cshtml*.
2. Reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Este código es similar al código que vio anteriormente para almacenar un gráfico en la memoria caché, con la excepción de que usa un archivo XML. El código comprueba primero para ver si el archivo XML existe llamando al método `File.Exists`. Si el archivo existe, el código crea un nuevo objeto `Chart` y pasa el nombre del archivo como parámetro `themePath`. Esto crea el gráfico en función de lo que haya en el archivo XML. Si el archivo XML todavía no existe, el código crea un gráfico como normal y, a continuación, llama a `SaveXml` para guardarlo. El gráfico se representa utilizando el método `Write`, como se ha visto antes.

    Como con la página que mostró el almacenamiento en caché, este código incluye una marca de tiempo en el título del gráfico.
3. Cree una nueva página denominada *ChartDisplayXMLChart. cshtml* y agréguele el siguiente marcado: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Ejecute la página *ChartDisplayXMLChart. cshtml* . Se muestra el gráfico. Tome nota de la marca de tiempo en el título del gráfico.
5. Cierre el explorador.
6. En WebMatrix, haga clic con el botón secundario en la carpeta *\_ChartFiles* , haga clic en **Actualizar**y, a continuación, abra la carpeta. El archivo *XMLChart. XML* de esta carpeta fue creado por la aplicación auxiliar de `Chart`. 

    ![Descripción: la carpeta _ChartFiles que muestra el archivo XMLChart. XML creado por la aplicación auxiliar de gráfico.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Vuelva a ejecutar la página *ChartDisplayXMLChart. cshtml* . El gráfico muestra la misma marca de tiempo que la primera vez que ejecutó la página. Esto se debe a que el gráfico se genera a partir del XML que guardó anteriormente.
8. En WebMatrix, abra la carpeta *\_ChartFiles* y elimine el archivo *XMLChart. XML* .
9. Ejecute la página *ChartDisplayXMLChart. cshtml* una vez más. Esta vez, la marca de tiempo se actualiza porque el `Chart` ayudante tenía que volver a crear el archivo XML. Si lo desea, Compruebe la carpeta *\_ChartFiles* y observe que el archivo XML vuelve a ser.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Introducción al trabajo con una base de datos en ASP.NET Web Pages sitios](https://go.microsoft.com/fwlink/?LinkId=202893)
- [Usar el almacenamiento en caché en sitios de ASP.NET Web Pages para mejorar el rendimiento](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Chart (clase](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) ) (referencia de la API de ASP.NET Web pages en MSDN)
