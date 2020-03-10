---
uid: web-pages/overview/data/5-working-with-data
title: Introducción al trabajo con una base de datos en sitios de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: En este capítulo se describe cómo obtener acceso a los datos de una base de datos y cómo mostrarlos mediante ASP.NET Web Pages.
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 45e988d037465e59ad352bb9444af2c69fd3cd70
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474043"
---
# <a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Introducción al trabajo con una base de datos en sitios de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se describe cómo usar las herramientas de Microsoft WebMatrix para crear una base de datos en un sitio web de ASP.NET Web Pages (Razor) y cómo crear páginas que le permitan Mostrar, agregar, editar y eliminar datos.
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear una base de datos.
> - Cómo conectarse a una base de datos.
> - Cómo mostrar los datos en una página web.
> - Cómo insertar, actualizar y eliminar registros de la base de datos.
> 
> Estas son las características introducidas en el artículo:
> 
> - Trabajar con una base de datos de Microsoft SQL Server Compact Edition.
> - Trabajar con consultas SQL.
> - La clase `Database`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial también funciona con WebMatrix 3. Puede usar ASP.NET Web Pages 3 y Visual Studio 2013 (o Visual Studio Express 2013 para web); sin embargo, la interfaz de usuario será diferente.

## <a name="introduction-to-databases"></a>Introducción a las bases de datos

Imagine una libreta de direcciones típica. Para cada entrada de la libreta de direcciones (es decir, para cada persona) tiene varios datos como el nombre, el apellido, la dirección, la dirección de correo electrónico y el número de teléfono.

Una forma habitual de imagen de datos como esta es como una tabla con filas y columnas. En términos de base de datos, a menudo se hace referencia a cada fila como un registro. Cada columna (a veces denominada campos) contiene un valor para cada tipo de datos: el nombre, el apellido, etc.

| **ID** | **FirstName** | **LastName** | **Dirección** | **Correo electrónico** | **Teléfono** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jim | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 Main St. Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Para la mayoría de las tablas de base de datos, la tabla debe tener una columna que contenga un identificador único, como un número de cliente, un número de cuenta, etc. Esto se conoce como la *clave principal*de la tabla y se utiliza para identificar cada fila de la tabla. En el ejemplo, la columna ID es la clave principal de la libreta de direcciones.

Con esta comprensión básica de las bases de datos, está listo para aprender a crear una base de datos simple y realizar operaciones como agregar, modificar y eliminar datos.

> [!TIP] 
> 
> **Bases de datos relacionales**
> 
> Puede almacenar datos de muchas maneras, como archivos de texto y hojas de cálculo. Sin embargo, para la mayoría de los usos empresariales, los datos se almacenan en una base de datos relacional.
> 
> Este artículo no profundiza en las bases de datos. Sin embargo, es posible que le resulte útil comprender un poco. En una base de datos relacional, la información se divide lógicamente en tablas independientes. Por ejemplo, una base de datos para una escuela podría contener tablas independientes para estudiantes y para ofertas de clases. El software de base de datos (como SQL Server) admite comandos eficaces que permiten establecer dinámicamente las relaciones entre las tablas. Por ejemplo, puede utilizar la base de datos relacional para establecer una relación lógica entre los alumnos y las clases con el fin de crear una programación. Almacenar datos en tablas independientes reduce la complejidad de la estructura de la tabla y reduce la necesidad de mantener los datos redundantes en las tablas.

## <a name="creating-a-database"></a>Crear una base de datos

En este procedimiento se muestra cómo crear una base de datos denominada SmallBakery mediante la herramienta de diseño de base de datos SQL Server Compact que se incluye en WebMatrix. Aunque puede crear una base de datos mediante código, es más habitual crear la base de datos y las tablas de base de datos mediante una herramienta de diseño como WebMatrix.

1. Inicie WebMatrix y, en la página Inicio rápido, haga clic en **sitio desde plantilla**.
2. Seleccione **sitio vacío**y, en el cuadro **nombre del sitio** , escriba "SmallBakery" y, a continuación, haga clic en **Aceptar**. El sitio se crea y se muestra en WebMatrix.
3. En el panel izquierdo, haga clic en el área de trabajo **bases de datos** .
4. En la cinta de opciones, haga clic en **nueva base de datos**. Se crea una base de datos vacía con el mismo nombre que el sitio.
5. En el panel izquierdo, expanda el nodo **SmallBakery. sdf** y, a continuación, haga clic en **tablas**.
6. En la cinta de opciones, haga clic en **nueva tabla**. WebMatrix abre el diseñador de tablas.

    ![[imagen]](5-working-with-data/_static/image1.jpg)
7. Haga clic en la columna **nombre** y escriba &quot;ID&quot;.
8. En la columna **tipo de datos** , seleccione **int**.
9. Establezca la opción **¿es la clave principal?** y **es Identify?** en **sí**.

    Como sugiere el nombre, **is PRIMARY KEY** indica a la base de datos que será la clave principal de la tabla. **Is Identity** indica a la base de datos que cree automáticamente un número de identificación para cada nuevo registro y lo asigne al siguiente número secuencial (empezando por 1).
10. Haga clic en la fila siguiente. El editor inicia una nueva definición de columna.
11. En el valor de nombre, escriba &quot;nombre&quot;.
12. En **tipo de datos**, elija &quot;nvarchar&quot; y establezca la longitud en 50. La parte *var* de `nvarchar` indica a la base de datos que los datos de esta columna serán una cadena cuyo tamaño puede variar de un registro a otro. (El prefijo *n* representa el *nacional*, lo que indica que el campo puede contener datos de caracteres que representan &#8212; cualquier alfabeto o sistema de escritura; es decir, que el campo contiene datos Unicode).
13. Establezca la opción **permitir valores NULL** en **no**. Esto exigirá que la columna *nombre* no se deje en blanco.
14. Con este mismo proceso, cree una columna denominada *Description*. Establezca **tipo de datos** en "nvarchar" y 50 para la longitud y establezca **permitir valores NULL** en false.
15. Cree una columna denominada *Price*. Establezca **tipo de datos en "Money"** y establezca **permitir valores NULL** en false.
16. En el cuadro de la parte superior, asigne a la tabla el nombre &quot;&quot;del producto.

    Cuando haya terminado, la definición tendrá el siguiente aspecto:

    ![[imagen]](5-working-with-data/_static/image2.png)
17. Presione CTRL + S para guardar la tabla.

## <a name="adding-data-to-the-database"></a>Agregar datos a la base de datos

Ahora puede agregar algunos datos de ejemplo a la base de datos con la que trabajará más adelante en el artículo.

1. En el panel izquierdo, expanda el nodo **SmallBakery. sdf** y, a continuación, haga clic en **tablas**.
2. Haga clic con el botón secundario en la tabla Product y, a continuación, haga clic en **datos**.
3. En el panel de edición, escriba los registros siguientes:

    | **Name** | **Descripción** | **Tarifas** |
    | --- | --- | --- |
    | Pan | Se ha incorporado cada día. | 2.99 |
    | Shortcake de fresa | Realizado con fresas orgánicas de nuestro jardín. | 9.99 |
    | Gráfico circular de Apple | En segundo lugar, solo en el gráfico circular de su mamá. | 12.99 |
    | Pecan circular | Si desea pecans, esto es así. | 10.99 |
    | Círculo de limón | Se realiza con los mejores limones del mundo. | 11.99 |
    | Madalenas | Sus hijos y el niño que le encantarán. | 7.99 |

    Recuerde que no tiene que escribir nada en la columna *ID* . Al crear la columna *ID* , se establece su propiedad **is Identity** en true, lo que hace que se rellene automáticamente.

    Cuando haya terminado de escribir los datos, el diseñador de tablas tendrá el siguiente aspecto:

    ![[imagen]](5-working-with-data/_static/image3.jpg)
4. Cierre la pestaña que contiene los datos de la base de datos.

## <a name="displaying-data-from-a-database"></a>Mostrar datos de una base de datos

Una vez que tenga una base de datos con datos, puede mostrar los datos en una página web de ASP.NET. Para seleccionar las filas de la tabla que se van a mostrar, se usa una instrucción SQL, que es un comando que se pasa a la base de datos.

1. En el panel izquierdo, haga clic en el área de trabajo **archivos** .
2. En la raíz del sitio web, cree una nueva página CSHTML denominada *ListProducts. CSHTML*.
3. Reemplace el marcado existente por lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    En el primer bloque de código, abra el archivo *SmallBakery. sdf* (base de datos) que creó anteriormente. El método `Database.Open` supone que el archivo *. sdf* está en la carpeta de *datos* de la aplicación del sitio web\_. (Tenga en cuenta que no es necesario especificar la extensión &#8212; *. sdf* de hecho; si lo hace, no funcionará el método `Open`).

    > [!NOTE]
    > La carpeta *App\_Data* es una carpeta especial de ASP.net que se usa para almacenar archivos de datos. Para obtener más información, vea [conectarse a una base de datos](#SB_ConnectingToADatabase) más adelante en este artículo.

    A continuación, realice una solicitud para consultar la base de datos mediante la siguiente instrucción de SQL `Select`:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    En la instrucción, `Product` identifica la tabla que se va a consultar. El carácter `*` especifica que la consulta debe devolver todas las columnas de la tabla. (También puede enumerar columnas individualmente, separadas por comas, si desea ver solo algunas de las columnas). La cláusula `Order By` indica cómo se deben ordenar los datos &#8212; en este caso por la columna *Name* . Esto significa que los datos se ordenan alfabéticamente según el valor de la columna *nombre* de cada fila.

    En el cuerpo de la página, el marcado crea una tabla HTML que se utilizará para mostrar los datos. Dentro del elemento `<tbody>`, se usa un bucle de `foreach` para obtener individualmente cada fila de datos devuelta por la consulta. Para cada fila de datos, se crea una fila de tabla HTML (elemento`<tr>`). A continuación, cree las celdas de la tabla HTML (elementos`<td>`) para cada columna. Cada vez que se recorre el bucle, la siguiente fila disponible de la base de datos se encuentra en la `row` variable (se configura en la instrucción `foreach`). Para obtener una columna individual de la fila, puede usar `row.Name` o `row.Description` o cualquier nombre de la columna que desee.
4. Ejecute la página en un explorador. (Asegúrese de que la página esté seleccionada en el área de trabajo **archivos** antes de ejecutarla). En la página se muestra una lista similar a la siguiente:

    ![[imagen]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Lenguaje de consulta estructurado (SQL)**
> 
> SQL es un lenguaje que se usa en la mayoría de las bases de datos relacionales para administrar datos en una base de datos. Incluye comandos que permiten recuperar datos y actualizarlos, y que permiten crear, modificar y administrar tablas de bases de datos. SQL es diferente de un lenguaje de programación (como el que está usando en WebMatrix) porque con SQL, la idea es que indique a la base de datos lo que desea, y es el trabajo de la base de datos para averiguar cómo obtener los datos o realizar la tarea. Estos son algunos ejemplos de algunos comandos SQL y lo que hacen:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> De esta forma se capturan las columnas de *identificador*, *nombre*y *precio* de los registros de la tabla *Product* si el valor del *precio* es superior a 10 y se devuelven los resultados en orden alfabético en función de los valores de la columna *nombre* . Este comando devolverá un conjunto de resultados que contiene los registros que cumplen los criterios, o un conjunto vacío si no coinciden los registros.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Esto inserta un nuevo registro en la tabla *Product* , estableciendo la columna *Name* en &quot;croissant&quot;, la columna *Description* para &quot;una alegría&quot;y el precio en 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Este comando elimina los registros de la tabla *Product* cuya columna fecha de expiración sea anterior al 1 de enero de 2008. (Esto supone que la tabla *Product* tiene una columna de este tipo, por supuesto). La fecha se escribe aquí en formato MM/DD/AAAA, pero debe escribirse en el formato que se usa para la configuración regional.
> 
> Los comandos `Insert Into` y `Delete` no devuelven conjuntos de resultados. En su lugar, devuelven un número que indica cuántos registros se vieron afectados por el comando.
> 
> En algunas de estas operaciones (como la inserción y eliminación de registros), el proceso que solicita la operación debe tener los permisos adecuados en la base de datos. Por este motivo, en el caso de las bases de datos de producción, a menudo tiene que proporcionar un nombre de usuario y una contraseña al conectarse a la base de datos.
> 
> Hay docenas de comandos SQL, pero todos siguen un patrón similar al siguiente. Puede usar comandos SQL para crear tablas de base de datos, contar el número de registros de una tabla, calcular precios y realizar muchas más operaciones.

## <a name="inserting-data-in-a-database"></a>Insertar datos en una base de datos

En esta sección se muestra cómo crear una página que permite a los usuarios agregar un nuevo producto a la tabla de la base de datos de *productos* . Después de insertar un nuevo registro de producto, la página muestra la tabla actualizada mediante la página *ListProducts. cshtml* que creó en la sección anterior.

La página incluye validación para asegurarse de que los datos especificados por el usuario son válidos para la base de datos. Por ejemplo, el código de la página se asegura de que se ha especificado un valor para todas las columnas necesarias.

1. En el sitio web, cree un nuevo archivo CSHTML llamado *InsertProducts. CSHTML*.
2. Reemplace el marcado existente por lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    El cuerpo de la página contiene un formulario HTML con tres cuadros de texto que permiten a los usuarios escribir un nombre, una descripción y un precio. Cuando los usuarios hacen clic en el botón **Insertar** , el código de la parte superior de la página abre una conexión a la base de datos *SmallBakery. sdf* . A continuación, obtiene los valores que el usuario ha enviado mediante el `Request` objeto y asigna esos valores a las variables locales.

    Para validar que el usuario ha especificado un valor para cada columna necesaria, registre cada `<input>` elemento que desea validar:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    La aplicación auxiliar de `Validation` comprueba que hay un valor en cada uno de los campos que ha registrado. Puede comprobar si todos los campos pasaron la validación comprobando `Validation.IsValid()`, que normalmente se realizan antes de procesar la información que se obtiene del usuario:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    (El operador `&&` significa y: esta prueba es *si se trata de un envío de formulario y todos los campos han superado la validación*).

    Si todas las columnas validadas (ninguna está vacía), continúe y cree una instrucción SQL para insertar los datos y, a continuación, ejecútelo como se muestra a continuación:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    En el caso de los valores que se van a insertar, se incluyen los marcadores de posición de los parámetros (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Como medida de seguridad, pase siempre los valores a una instrucción SQL con parámetros, como se ve en el ejemplo anterior. Esto le ofrece la oportunidad de validar los datos del usuario, además de ayudarle a protegerse contra los intentos de enviar comandos malintencionados a la base de datos (a veces denominados ataques por inyección de SQL).

    Para ejecutar la consulta, use esta instrucción, pasándole las variables que contienen los valores que se van a sustituir para los marcadores de posición:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Una vez que se ha ejecutado la instrucción `Insert Into`, se envía al usuario a la página que muestra los productos mediante esta línea:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Si la validación no se realizó correctamente, se omite la inserción. En su lugar, tiene una aplicación auxiliar en la página que puede mostrar los mensajes de error acumulados (si existen):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Observe que el bloque de estilo del marcado incluye una definición de clase CSS denominada `.validation-summary-errors`. Es el nombre de la clase CSS que se utiliza de forma predeterminada para el elemento `<div>` que contiene los errores de validación. En este caso, la clase CSS especifica que los errores de Resumen de validación se muestran en rojo y en negrita, pero puede definir la clase `.validation-summary-errors` para mostrar el formato que desee.

### <a name="testing-the-insert-page"></a>Probar la página de inserción

1. Vea la página en un explorador. La página muestra un formulario similar al que se muestra en la siguiente ilustración.

    ![[imagen]](5-working-with-data/_static/image5.jpg)
2. Escriba valores para todas las columnas, pero asegúrese de dejar la columna *Price* en blanco.
3. Haga clic en **Insertar**. La página muestra un mensaje de error, tal y como se muestra en la siguiente ilustración. (No se crea ningún registro nuevo).

    ![[imagen]](5-working-with-data/_static/image6.jpg)
4. Rellene el formulario completamente y, a continuación, haga clic en **Insertar**. Esta vez, se muestra la página *ListProducts. cshtml* y se muestra el nuevo registro.

## <a name="updating-data-in-a-database"></a>Actualizar datos en una base de datos

Una vez especificados los datos en una tabla, es posible que tenga que actualizarlos. En este procedimiento se muestra cómo crear dos páginas que son similares a las que creó anteriormente para la inserción de datos. La primera página muestra productos y permite a los usuarios seleccionar uno para cambiar. La segunda página permite que los usuarios realicen realmente las modificaciones y las guarden.

> [!NOTE] 
> 
> **Importante** En un sitio web de producción, normalmente se restringe quién tiene permiso para realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y sobre cómo autorizar a los usuarios para que realicen tareas en el sitio, consulte [adición de seguridad y pertenencia a un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202904).

1. En el sitio web, cree un nuevo archivo CSHTML llamado *EditProducts. CSHTML*.
2. Reemplace el marcado existente en el archivo por lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    La única diferencia entre esta página y la página *ListProducts. cshtml* anterior es que la tabla HTML de esta página incluye una columna adicional que muestra un vínculo de **edición** . Al hacer clic en este vínculo, se le remite a la página *UpdateProducts. cshtml* (que creará a continuación), donde puede editar el registro seleccionado.

    Examine el código que crea el vínculo de **edición** :

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Esto crea un elemento de `<a>` HTML cuyo atributo de `href` se establece dinámicamente. El atributo `href` especifica la página que se va a mostrar cuando el usuario haga clic en el vínculo. También pasa el valor `Id` de la fila actual al vínculo. Cuando se ejecuta la página, el origen de la página podría contener vínculos como estos:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Observe que el atributo `href` está establecido en `UpdateProducts/n`, donde *n* es un número de producto. Cuando un usuario hace clic en uno de estos vínculos, la dirección URL resultante tendrá un aspecto similar al siguiente:

    `http://localhost:18816/UpdateProducts/6`

    En otras palabras, el número de producto que se va a editar se pasará en la dirección URL.
3. Vea la página en un explorador. La página muestra los datos en un formato similar al siguiente:

    ![[imagen]](5-working-with-data/_static/image7.jpg)

    A continuación, creará la página que permite a los usuarios actualizar realmente los datos. La página de actualización incluye validación para validar los datos especificados por el usuario. Por ejemplo, el código de la página se asegura de que se ha especificado un valor para todas las columnas necesarias.
4. En el sitio web, cree un nuevo archivo CSHTML llamado *UpdateProducts. CSHTML*.
5. Reemplace el marcado existente en el archivo por lo siguiente.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    El cuerpo de la página contiene un formulario HTML en el que se muestra un producto y donde los usuarios pueden modificarlo. Para ver el producto que se va a mostrar, use esta instrucción SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    Se seleccionará el producto cuyo identificador coincida con el valor que se pasa en el parámetro `@0`. (Dado que el *identificador* es la clave principal y, por tanto, debe ser único, solo se puede seleccionar un registro de producto de esta manera). Para obtener el valor de identificador que se va a pasar a esta instrucción `Select`, puede leer el valor que se pasa a la página como parte de la dirección URL con la siguiente sintaxis:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Para obtener realmente el registro del producto, use el método `QuerySingle`, que devolverá solo un registro:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    La única fila se devuelve a la variable `row`. Puede obtener datos de cada columna y asignarlos a variables locales de la siguiente manera:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    En el marcado del formulario, estos valores se muestran automáticamente en cuadros de texto individuales mediante el uso de código incrustado como el siguiente:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Esa parte del código muestra el registro del producto que se va a actualizar. Una vez que se muestra el registro, el usuario puede editar columnas individuales.

    Cuando el usuario envía el formulario haciendo clic en el botón **Actualizar** , se ejecuta el código del bloque `if(IsPost)`. Esto obtiene los valores del usuario del objeto `Request`, almacena los valores en las variables y comprueba que se ha rellenado cada columna. Si la validación es correcta, el código crea la siguiente instrucción UPDATE de SQL:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    En una instrucción de `Update` de SQL, se especifica cada columna que se va a actualizar y el valor en el que se va a establecer. En este código, los valores se especifican mediante los marcadores de posición de parámetro `@0`, `@1`, `@2`, etc. (Como se indicó anteriormente, para la seguridad, siempre debe pasar valores a una instrucción SQL mediante parámetros).

    Cuando se llama al método `db.Execute`, se pasan las variables que contienen los valores en el orden que corresponde a los parámetros de la instrucción SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Una vez ejecutada la instrucción `Update`, se llama al método siguiente para redirigir al usuario de nuevo a la página de edición:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    El efecto es que el usuario ve una lista actualizada de los datos en la base de datos y puede editar otro producto.
6. Guarde la página.
7. Ejecute la página *EditProducts. cshtml* (no la página actualizar) y, a continuación, haga clic en **Editar** para seleccionar un producto para editarlo. Se muestra la página *UpdateProducts. cshtml* , en la que se muestra el registro que ha seleccionado.

    ![[imagen]](5-working-with-data/_static/image8.jpg)
8. Realice un cambio y haga clic en **Actualizar**. La lista de productos se muestra de nuevo con los datos actualizados.

## <a name="deleting-data-in-a-database"></a>Eliminar datos en una base de datos

En esta sección se muestra cómo permitir que los usuarios eliminen un producto de la tabla de la base de datos *Product* . El ejemplo consta de dos páginas. En la primera página, los usuarios seleccionan un registro para eliminar. El registro que se va a eliminar se muestra en una segunda página que les permite confirmar que desea eliminar el registro.

> [!NOTE] 
> 
> **Importante** En un sitio web de producción, normalmente se restringe quién tiene permiso para realizar cambios en los datos. Para obtener información acerca de cómo configurar la pertenencia y acerca de cómo autorizar a los usuarios para que realicen tareas en el sitio, consulte [adición de seguridad y pertenencia a un sitio de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=202904).

1. En el sitio web, cree un nuevo archivo CSHTML llamado *ListProductsForDelete. CSHTML*.
2. Reemplace el marcado existente por lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Esta página es similar a la página *EditProducts. cshtml* anterior. Sin embargo, en lugar de mostrar un vínculo de **edición** para cada producto, muestra un vínculo de **eliminación** . El vínculo de **eliminación** se crea con el siguiente código incrustado en el marcado:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Esto crea una dirección URL que tiene este aspecto cuando los usuarios hacen clic en el vínculo:

    `http://<server>/DeleteProduct/4`

    La dirección URL llama a una página denominada *DeleteProduct. cshtml* (que creará a continuación) y le pasa el identificador del producto que se va a eliminar (aquí, 4).
3. Guarde el archivo, pero déjelo abierto.
4. Cree otro archivo CHTML llamado *DeleteProduct. cshtml*. Reemplace el contenido existente por lo siguiente:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    *ListProductsForDelete. cshtml* llama a esta página y permite a los usuarios confirmar que desean eliminar un producto. Para enumerar el producto que se va a eliminar, obtenga el identificador del producto que se va a eliminar de la dirección URL mediante el código siguiente:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    A continuación, la página pide al usuario que haga clic en un botón para eliminar realmente el registro. Esta es una medida de seguridad importante: al realizar operaciones confidenciales en el sitio web como actualizar o eliminar datos, estas operaciones se deben realizar siempre mediante una operación POST, no una operación GET. Si el sitio está configurado para que se pueda realizar una operación de eliminación mediante una operación GET, cualquiera puede pasar una dirección URL como `http://<server>/DeleteProduct/4` y eliminar todo lo que desee de la base de datos. Agregando la confirmación y codificando la página para que la eliminación pueda realizarse solo mediante una publicación, agregue una medida de seguridad a su sitio.

    La operación de eliminación real se realiza mediante el código siguiente, que primero confirma que se trata de una operación post y que el identificador no está vacío:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    El código ejecuta una instrucción SQL que elimina el registro especificado y, a continuación, redirige al usuario a la página de lista.
5. Ejecute *ListProductsForDelete. cshtml* en un explorador.

    ![[imagen]](5-working-with-data/_static/image9.jpg)
6. Haga clic en el vínculo **eliminar** de uno de los productos. Se muestra la página *DeleteProduct. cshtml* para confirmar que desea eliminar ese registro.
7. Haga clic en el botón **Eliminar** . Se elimina el registro del producto y se actualiza la página con una lista de productos actualizada.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Conectar a una base de datos
> 
> Puede conectarse a una base de datos de dos maneras. La primera es usar el método `Database.Open` y especificar el nombre del archivo de base de datos (menos la extensión *. sdf* ):
> 
> `var db = Database.Open("SmallBakery");`
> 
> El método `Open` supone que. el archivo *SDF* está en la carpeta de\_de la *aplicación* del sitio Web. Esta carpeta está diseñada específicamente para contener datos. Por ejemplo, tiene los permisos adecuados para permitir que el sitio web Lea y escriba datos, y como medida de seguridad, WebMatrix no permite el acceso a los archivos de esta carpeta.
> 
> La segunda manera es usar una cadena de conexión. Una cadena de conexión contiene información sobre cómo conectarse a una base de datos. Esto puede incluir una ruta de acceso de archivo o puede incluir el nombre de una SQL Server base de datos en un servidor local o remoto, junto con un nombre de usuario y una contraseña para conectarse a ese servidor. (Si mantiene los datos en una versión administrada centralmente de SQL Server, como en el sitio de un proveedor de hospedaje, siempre se usa una cadena de conexión para especificar la información de conexión a la base de datos).
> 
> En WebMatrix, las cadenas de conexión suelen almacenarse en un archivo XML denominado *Web. config*. Como implica el nombre, puede usar un archivo *Web. config* en la raíz de su sitio web para almacenar la información de configuración del sitio, incluidas las cadenas de conexión que el sitio pueda necesitar. Un ejemplo de una cadena de conexión en un archivo *Web. config* podría ser similar al siguiente:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> En el ejemplo, la cadena de conexión apunta a una base de datos en una instancia de SQL Server que se ejecuta en un servidor en algún lugar (en lugar de un archivo *. sdf* local). Deberá sustituir los nombres adecuados por `myServer` y `myDatabase`, y especificar SQL Server valores de inicio de sesión para `username` y `password`. (Los valores de nombre de usuario y contraseña no son necesariamente los mismos que las credenciales de Windows o como los valores que el proveedor de hospedaje le ha dado para iniciar sesión en sus servidores. Consulte con el administrador los valores exactos que necesita).
> 
> El método `Database.Open` es flexible, porque le permite pasar el nombre de un archivo *. sdf* de base de datos o el nombre de una cadena de conexión que se almacena en el archivo *Web. config* . En el ejemplo siguiente se muestra cómo conectarse a la base de datos mediante la cadena de conexión que se muestra en el ejemplo anterior:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Como se indicó, el método `Database.Open` le permite pasar un nombre de base de datos o una cadena de conexión y descubrirá cuál usar. Esto resulta muy útil al implementar (publicar) el sitio Web. Puede usar un archivo *. sdf* en la carpeta *App\_Data* al desarrollar y probar el sitio. Después, cuando mueva el sitio a un servidor de producción, puede usar una cadena de conexión en el archivo *Web. config* que tenga el mismo nombre que el archivo *. sdf* , pero que apunte a la base de &#8212; datos del proveedor de hospedaje sin tener que cambiar el código.
> 
> Por último, si desea trabajar directamente con una cadena de conexión, puede llamar al método `Database.OpenConnectionString` y pasar la cadena de conexión real en lugar de simplemente el nombre de una en el archivo *Web. config* . Esto puede resultar útil en situaciones en las que, por algún motivo, no se tiene acceso a la cadena de conexión (o a los valores que contiene, como el nombre de archivo *. sdf* ) hasta que se ejecuta la página. Sin embargo, para la mayoría de los escenarios, puede usar `Database.Open` tal y como se describe en este artículo.

## <a name="additional-resources"></a>Recursos adicionales

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Conexión a una base de datos SQL Server o MySQL en WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Validar la entrada del usuario en los sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
