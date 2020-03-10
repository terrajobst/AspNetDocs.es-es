---
uid: web-pages/overview/data/working-with-files
title: Trabajar con archivos en un sitio de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se explica cómo leer, escribir, anexar, eliminar y cargar archivos.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 684c47a8a8480dc040e5144144577c94c35d39e5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521887"
---
# <a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>Trabajar con archivos en un sitio de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica cómo leer, escribir, anexar, eliminar y cargar archivos en un sitio de ASP.NET Web Pages (Razor).
> 
> > [!NOTE]
> > Si desea cargar imágenes y manipularlas (por ejemplo, voltear o cambiar su tamaño), consulte [trabajar con imágenes en un sitio de ASP.NET Web pages](/aspnet/web-pages/overview/ui-layouts-and-themes/9-working-with-images).
> 
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un archivo de texto y escribir datos en él.
> - Cómo anexar datos a un archivo existente.
> - Cómo leer un archivo y mostrarlo desde él.
> - Cómo eliminar archivos de un sitio Web.
> - Cómo permitir a los usuarios cargar un archivo o varios archivos.
> 
> Estas son las características de programación de ASP.NET que se introdujeron en el artículo:
> 
> - El objeto `File`, que proporciona una manera de administrar archivos.
> - Aplicación auxiliar de `FileUpload`.
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

<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>Crear un archivo de texto y escribir datos en él

Además de usar una base de datos en el sitio web, puede trabajar con archivos. Por ejemplo, puede usar archivos de texto como una forma sencilla de almacenar datos para el sitio. (Un archivo de texto que se usa para almacenar datos a veces se denomina *archivo plano*). Los archivos de texto pueden estar en formatos diferentes, como *. txt*, *. XML*o *. csv* (valores delimitados por comas).

Si desea almacenar los datos en un archivo de texto, puede utilizar el método `File.WriteAllText` para especificar el archivo que se va a crear y los datos que se van a escribir en él. En este procedimiento, creará una página que contenga un formulario sencillo con tres `input` elementos (nombre, apellidos y dirección de correo electrónico) y un botón **Enviar** . Cuando el usuario envía el formulario, almacenará la entrada del usuario en un archivo de texto.

1. Cree una nueva carpeta denominada *App\_Data*, si aún no existe.
2. En la raíz de su sitio web, cree un nuevo archivo llamado *userdata. cshtml*.
3. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    El marcado HTML crea el formulario con los tres cuadros de texto. En el código, use la propiedad `IsPost` para determinar si se ha enviado la página antes de comenzar el procesamiento.

    La primera tarea consiste en obtener la entrada del usuario y asignarla a las variables. A continuación, el código concatena los valores de las variables independientes en una cadena delimitada por comas, que se almacena en una variable diferente. Tenga en cuenta que el separador de comas es una cadena contenida entre comillas (","), porque está insertando literalmente una coma en la cadena grande que está creando. Al final de los datos que se concatenan juntos, se agrega `Environment.NewLine`. Esto agrega un salto de línea (un carácter de nueva línea). Lo que va a crear con esta concatenación es una cadena similar a la siguiente:

    [!code-css[Main](working-with-files/samples/sample2.css)]

    (Con un salto de línea invisible al final).

    A continuación, cree una variable (`dataFile`) que contenga la ubicación y el nombre del archivo en el que almacenar los datos. La configuración de la ubicación requiere un tratamiento especial. En sitios web, se recomienda hacer referencia al código en rutas de acceso absolutas como *C:\Folder\File.txt* para los archivos del servidor Web. Si se mueve un sitio web, una ruta de acceso absoluta será incorrecta. Además, para un sitio hospedado (en lugar de en su propio equipo), normalmente no es necesario saber cuál es la ruta de acceso correcta al escribir el código.

    Pero a veces (como ahora, para escribir un archivo) necesita una ruta de acceso completa. La solución consiste en usar el método `MapPath` del objeto `Server`. Esto devuelve la ruta de acceso completa a su sitio Web. Para obtener la ruta de acceso de la raíz del sitio web, puede utilizar el operador de `~` (para que REPRES la raíz virtual del sitio) a `MapPath`. (También puede pasarle un nombre de subcarpeta, como *~/App\_Data/* , para obtener la ruta de acceso de esa subcarpeta). Después, puede concatenar información adicional en cualquier objeto que devuelva el método para crear una ruta de acceso completa. En este ejemplo, se agrega un nombre de archivo. (Puede obtener más información sobre cómo trabajar con rutas de acceso de archivos y carpetas en [Introducción a la programación de ASP.NET Web pages mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)).

    El archivo se guarda en la carpeta de datos de la\_de la *aplicación* . Esta carpeta es una carpeta especial de ASP.NET que se usa para almacenar archivos de datos, tal y como se describe en [Introducción al trabajo con una base de datos en ASP.NET Web pages sitios](https://go.microsoft.com/fwlink/?LinkId=195209).

    El método `WriteAllText` del objeto `File` escribe los datos en el archivo. Este método toma dos parámetros: el nombre (con la ruta de acceso) del archivo en el que se va a escribir y los datos reales que se van a escribir. Observe que el nombre del primer parámetro tiene un carácter `@` como prefijo. Esto indica a ASP.NET que está proporcionando un literal de cadena textual y que los caracteres como "/" no deben interpretarse de manera especial. (Para obtener más información, vea [Introducción a la programación web de ASP.net mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)).

    > [!NOTE]
    > Para que el código guarde los archivos en la carpeta de datos de la\_de la *aplicación* , la aplicación necesita permisos de lectura y escritura para esa carpeta. En el equipo de desarrollo, esto no suele ser un problema. Sin embargo, al publicar el sitio en el servidor Web de un proveedor de hospedaje, puede que tenga que establecer esos permisos explícitamente. Si ejecuta este código en el servidor de un proveedor de hospedaje y obtiene errores, consulte con el proveedor de hospedaje para averiguar cómo establecer esos permisos.

- Ejecute la página en un explorador. 

    ![](working-with-files/_static/image1.jpg)
- Escriba los valores en los campos y, a continuación, haga clic en **Enviar**.
- Cierre el explorador.
- Vuelva al proyecto y actualice la vista.
- Abra el archivo *Data. txt* . Los datos que envió en el formulario están en el archivo. 

    ![[imagen]](working-with-files/_static/image2.jpg)
- Cierre el archivo *Data. txt* .

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>Anexar datos a un archivo existente

En el ejemplo anterior, usó `WriteAllText` para crear un archivo de texto que tiene solo un fragmento de datos. Si llama de nuevo al método y lo pasa al mismo nombre de archivo, el archivo existente se sobrescribe por completo. Sin embargo, después de crear un archivo, a menudo desea agregar nuevos datos al final del archivo. Puede hacerlo mediante el método `AppendAllText` del objeto `File`.

1. En el sitio web, realice una copia del archivo *userdata. cshtml* y asígnele el nombre *UserDataMultiple. cshtml*.
2. Reemplace el bloque de código antes de la etiqueta de apertura `<!DOCTYPE html>` por el siguiente bloque de código: 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    Este código tiene un cambio en el ejemplo anterior. En lugar de utilizar `WriteAllText`, utiliza `the AppendAllText` método. Los métodos son similares, salvo que `AppendAllText` agrega los datos al final del archivo. Como con `WriteAllText`, `AppendAllText` crea el archivo si aún no existe.
3. Ejecute la página en un explorador.
4. Escriba los valores de los campos y, a continuación, haga clic en **Enviar**.
5. Agregue más datos y envíe el formulario de nuevo.
6. Vuelva al proyecto, haga clic con el botón secundario en la carpeta del proyecto y, a continuación, haga clic en **Actualizar**.
7. Abra el archivo *Data. txt* . Ahora contiene los nuevos datos que acaba de escribir. 

    ![[imagen]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>Leer y Mostrar datos de un archivo

Incluso si no necesita escribir datos en un archivo de texto, es probable que a veces necesite leer los datos de uno de ellos. Para ello, puede volver a usar el objeto `File`. Puede usar el objeto `File` para leer cada línea individualmente (separadas por saltos de línea) o para leer el elemento individual, independientemente de cómo estén separados.

En este procedimiento se muestra cómo leer y mostrar los datos creados en el ejemplo anterior.

1. En la raíz de su sitio web, cree un nuevo archivo denominado *DisplayData. cshtml*.
2. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    El código comienza leyendo el archivo que creó en el ejemplo anterior en una variable denominada `userData`, mediante esta llamada al método:

    [!code-css[Main](working-with-files/samples/sample5.css)]

    El código para hacerlo está dentro de una instrucción `if`. Cuando desea leer un archivo, es una buena idea usar el método `File.Exists` para determinar primero si el archivo está disponible. El código también comprueba si el archivo está vacío.

    El cuerpo de la página contiene dos bucles `foreach`, uno anidado dentro de otro. El bucle `foreach` externo obtiene una línea a la vez del archivo de datos. En este caso, las líneas se definen mediante saltos de línea en &#8212; el archivo, es decir, cada elemento de datos se encuentra en su propia línea. El bucle exterior crea un nuevo elemento (`<li>` elemento) dentro de una lista ordenada (elemento`<ol>`).

    El bucle interno divide cada línea de datos en elementos (campos) mediante una coma como delimitador. (Según el ejemplo anterior, esto significa que cada línea contiene tres campos &#8212; , el nombre, el apellido y la dirección de correo electrónico, separados por una coma). El bucle interno también crea una lista de `<ul>` y muestra un elemento de lista para cada campo de la línea de datos.

    El código muestra cómo utilizar dos tipos de datos, una matriz y el tipo de datos `char`. La matriz es necesaria porque el método `File.ReadAllLines` devuelve datos como una matriz. El tipo de datos `char` es necesario porque el método `Split` devuelve un `array` en el que cada elemento es de tipo `char`. (Para obtener información sobre las matrices, vea [Introducción a la programación web de ASP.net mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)).
3. Ejecute la página en un explorador. Se muestran los datos especificados para los ejemplos anteriores. 

    ![[imagen]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **Mostrar datos de un archivo delimitado por comas de Microsoft Excel**
> 
> Puede usar Microsoft Excel para guardar los datos contenidos en una hoja de cálculo como un archivo delimitado por comas (archivo *. csv* ). Cuando lo haga, el archivo se guardará en texto sin formato, no en formato de Excel. Cada fila de la hoja de cálculo está separada por un salto de línea en el archivo de texto y cada elemento de datos está separado por una coma. Puede usar el código que se muestra en el ejemplo anterior para leer un archivo delimitado por comas de Excel simplemente cambiando el nombre del archivo de datos en el código.

<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>Eliminar archivos

Para eliminar archivos del sitio web, puede usar el método `File.Delete`. Este procedimiento muestra cómo permitir que los usuarios eliminen una imagen (archivo *. jpg* ) de una carpeta de *imágenes* si conocen el nombre del archivo.

> [!NOTE] 
> 
> **Importante** En un sitio web de producción, normalmente se restringe quién tiene permiso para realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y sobre cómo autorizar a los usuarios para que realicen tareas en el sitio, consulte [adición de seguridad y pertenencia a un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202904).

1. En el sitio web, cree una subcarpeta denominada *images*.
2. Copie uno o varios archivos *. jpg* en la carpeta *images* .
3. En la raíz del sitio web, cree un nuevo archivo denominado *FileDelete. cshtml*.
4. Reemplace el contenido existente por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    Esta página contiene un formulario en el que los usuarios pueden escribir el nombre de un archivo de imagen. No escriben la extensión de nombre de archivo *. jpg.* al restringir el nombre de archivo de este modo, se evita que los usuarios eliminen archivos arbitrarios en el sitio.

    El código lee el nombre de archivo que el usuario ha escrito y, a continuación, construye una ruta de acceso completa. Para crear la ruta de acceso, el código utiliza la ruta de acceso del sitio web actual (tal como la devuelve el método `Server.MapPath`), el nombre de la carpeta *images* , el nombre que el usuario ha proporcionado y ". jpg" como una cadena literal.

    Para eliminar el archivo, el código llama al método `File.Delete`, pasándole la ruta de acceso completa que acaba de crear. Al final del marcado, el código muestra un mensaje de confirmación que indica que el archivo se ha eliminado.
5. Ejecute la página en un explorador. 

    ![[imagen]](working-with-files/_static/image5.jpg)
6. Escriba el nombre del archivo que desea eliminar y, a continuación, haga clic en **Enviar**. Si se eliminó el archivo, el nombre del archivo se muestra en la parte inferior de la página.

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>Permitir a los usuarios cargar un archivo

La aplicación auxiliar de `FileUpload` permite a los usuarios cargar archivos en el sitio Web. En el procedimiento siguiente se muestra cómo permitir a los usuarios cargar un único archivo.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo agregó anteriormente.
2. En la carpeta *App\_Data* , cree una nueva carpeta y asígnele el nombre *UploadedFiles*.
3. En la raíz, cree un nuevo archivo denominado *FileUpload. cshtml*.
4. Reemplace el contenido existente en la página por lo siguiente: 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    La parte del cuerpo de la página usa la aplicación auxiliar de `FileUpload` para crear el cuadro de carga y los botones con los que probablemente esté familiarizado:

    ![[imagen]](working-with-files/_static/image6.jpg)

    Las propiedades que establezca para la aplicación auxiliar de `FileUpload` especifican que desea que se cargue un solo cuadro para el archivo y que desea que el botón Enviar lea **cargar**. (Agregará más cuadros más adelante en el artículo).

    Cuando el usuario hace clic en **cargar**, el código que se encuentra en la parte superior de la página obtiene el archivo y lo guarda. El objeto `Request` que normalmente se usa para obtener los valores de los campos de formulario también tiene una matriz de `Files` que contiene el archivo (o archivos) que se ha cargado. Puede obtener archivos individuales de posiciones específicas en la matriz &#8212; , por ejemplo, para obtener el primer archivo cargado, obtener `Request.Files[0]`, para obtener el segundo archivo, obtener `Request.Files[1]`, etc. (Recuerde que en la programación, el recuento normalmente comienza en cero).

    Al capturar un archivo cargado, lo coloca en una variable (aquí, `uploadedFile`) para que pueda manipularlo. Para determinar el nombre del archivo cargado, solo tiene que obtener su propiedad `FileName`. Sin embargo, cuando el usuario carga un archivo, `FileName` contiene el nombre original del usuario, que incluye la ruta de acceso completa. Podría tener este aspecto:

    *C:\Users\Public\Sample.txt*

    Sin embargo, no desea toda la información de la ruta de acceso, ya que es la ruta de acceso en el equipo del usuario, no en el servidor. Simplemente desea el nombre de archivo real (*sample. txt*). Puede quitar solo el archivo de una ruta de acceso mediante el método `Path.GetFileName`, de la siguiente manera:

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    El objeto `Path` es una utilidad que tiene una serie de métodos como este que puede usar para quitar trazados, combinar trazados, etc.

    Una vez que haya obtenido el nombre del archivo cargado, puede crear una nueva ruta de acceso en la que desea almacenar el archivo cargado en el sitio Web. En este caso, se combinan `Server.MapPath`, los nombres de carpeta (*App\_Data/UploadedFiles*) y el nombre de archivo recién eliminado para crear una nueva ruta de acceso. Después, puede llamar al método `SaveAs` del archivo cargado para guardar realmente el archivo.
5. Ejecute la página en un explorador. 

    ![[imagen]](working-with-files/_static/image7.jpg)
6. Haga clic en **examinar** y seleccione un archivo para cargarlo. 

    ![[imagen]](working-with-files/_static/image8.jpg)

    El cuadro de texto situado junto al botón **examinar** contendrá la ruta de acceso y la ubicación del archivo.

    ![[imagen]](working-with-files/_static/image9.jpg)
7. Haga clic en **Cargar**.
8. En el sitio web, haga clic con el botón secundario en la carpeta de proyecto y haga clic en **Actualizar**.
9. Abra la carpeta *UploadedFiles* . El archivo que ha cargado está en la carpeta. 

    ![[imagen]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>Permitir a los usuarios cargar varios archivos

En el ejemplo anterior, permite a los usuarios cargar un archivo. Sin embargo, puede usar la aplicación auxiliar `FileUpload` para cargar más de un archivo a la vez. Esto resulta útil para escenarios como la carga de fotografías, donde la carga de un archivo a la vez es tediosa. (Puede leer acerca de la carga de fotografías [al trabajar con imágenes en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202897)). Este ejemplo muestra cómo permitir que los usuarios carguen dos a la vez, aunque puede usar la misma técnica para cargar más que eso.

1. Agregue la biblioteca de aplicaciones auxiliares Web de ASP.NET a su sitio web, tal como se describe en [instalación de aplicaciones auxiliares en un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.
2. Cree una nueva página denominada *FileUploadMultiple. cshtml*.
3. Reemplace el contenido existente en la página por lo siguiente:  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    En este ejemplo, la aplicación auxiliar de `FileUpload` en el cuerpo de la página está configurada para permitir que los usuarios carguen dos archivos de forma predeterminada. Como `allowMoreFilesToBeAdded` está establecido en `true`, el ayudante representa un vínculo que permite al usuario agregar más cuadros de carga:

    ![[imagen]](working-with-files/_static/image11.jpg)

    Para procesar los archivos que el usuario carga, el código utiliza la misma técnica básica que usó en el ejemplo &#8212; anterior obtener un archivo de `Request.Files` y, a continuación, guardarlo. (Incluidas las diversas cosas que debe hacer para obtener el nombre de archivo y la ruta de acceso correctos). La innovación es que es posible que el usuario esté cargando varios archivos y no conozca muchos. Para averiguarlo, puede obtener `Request.Files.Count`.

    Con este número a mano, puede recorrer `Request.Files`, capturar cada archivo a su vez y guardarlo. Si desea crear un bucle de un número conocido de veces a través de una colección, puede usar un bucle `for`, como el siguiente:

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    La variable `i` es simplemente un contador temporal que va de cero a cualquier límite superior establecido. En este caso, el límite superior es el número de archivos. Pero dado que el contador comienza en cero, como es típico para el recuento de escenarios en ASP.NET, el límite superior es realmente uno menos que el número de archivos. (Si se cargan tres archivos, el recuento es de cero a 2).

    La variable `uploadedCount` suma todos los archivos que se han cargado y guardado correctamente. Este código tiene en cuenta la posibilidad de que un archivo esperado no pueda cargarse.
4. Ejecute la página en un explorador. El explorador muestra la página y sus dos cuadros de carga.
5. Seleccione dos archivos para cargar.
6. Haga clic en **Agregar otro archivo**. La página muestra un nuevo cuadro de carga. 

    ![[imagen]](working-with-files/_static/image12.jpg)
7. Haga clic en **Cargar**.
8. En el sitio web, haga clic con el botón secundario en la carpeta de proyecto y haga clic en **Actualizar**.
9. Abra la carpeta *UploadedFiles* para ver los archivos cargados correctamente.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Trabajar con imágenes en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202897)

[Exportación a un archivo CSV](https://msdn.microsoft.com/library/ms155919.aspx)
