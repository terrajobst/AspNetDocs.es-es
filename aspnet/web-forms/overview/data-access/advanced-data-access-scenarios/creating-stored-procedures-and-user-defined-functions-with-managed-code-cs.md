---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Crear procedimientos almacenados y funciones definidas por el usuario con código administradoC#() | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 se integra con .NET Common Language Runtime para que los desarrolladores puedan crear objetos de base de datos a través de código administrado. Este tutorial...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: c6aec9ca70fe3ab568b3d17fea6bfd56671edc03
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74605404"
---
# <a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Crear procedimientos almacenados y funciones definidas por el usuario con código administrado (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) o [Descargar PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 se integra con .NET Common Language Runtime para que los desarrolladores puedan crear objetos de base de datos a través de código administrado. En este tutorial se muestra cómo crear procedimientos almacenados administrados y funciones administradas definidas por el usuario C# con la Visual Basic o el código. También veremos cómo estas ediciones de Visual Studio le permiten depurar estos objetos de base de datos administrados.

## <a name="introduction"></a>Introducción

Las bases de datos como Microsoft s SQL Server 2005 usan [Transact-lenguaje de consulta estructurado (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) para insertar, modificar y recuperar datos. La mayoría de los sistemas de base de datos incluyen construcciones para agrupar una serie de instrucciones SQL que se pueden ejecutar como una sola unidad reutilizable. Los procedimientos almacenados son un ejemplo. Otro es una *función definida por el usuario*(UDF), una construcción que examinaremos con más detalle en el paso 9.

En esencia, SQL está diseñado para trabajar con conjuntos de datos. Las instrucciones `SELECT`, `UPDATE`y `DELETE` se aplican de forma inherente a todos los registros de la tabla correspondiente y solo están limitadas por sus cláusulas `WHERE`. Sin embargo, hay muchas características de lenguaje diseñadas para trabajar con un registro cada vez y para manipular datos escalares. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) permiten que un conjunto de registros se recorran de uno en uno. Las funciones de manipulación de cadenas como `LEFT`, `CHARINDEX`y `PATINDEX` funcionan con datos escalares. SQL también incluye instrucciones de flujo de control como `IF` y `WHILE`.

Antes de Microsoft SQL Server 2005, los procedimientos almacenados y las UDF solo se podían definir como una colección de instrucciones T-SQL. Sin embargo, SQL Server 2005 se diseñó para proporcionar integración con [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), que es el tiempo de ejecución que usan todos los ensamblados .net. Por consiguiente, los procedimientos almacenados y las UDF en una base de datos SQL Server 2005 se pueden crear mediante código administrado. Es decir, puede crear un procedimiento almacenado o una UDF como método en una C# clase. Esto permite que estos procedimientos almacenados y UDF utilicen la funcionalidad de la .NET Framework y de sus propias clases personalizadas.

En este tutorial, examinaremos cómo crear procedimientos almacenados administrados y funciones definidas por el usuario, y cómo integrarlos en nuestra base de datos Northwind. Vamos a empezar a trabajar.

> [!NOTE]
> Los objetos de base de datos administrados ofrecen algunas ventajas con respecto a sus homólogos SQL. La riqueza y la familiaridad del lenguaje y la capacidad de reutilizar el código y la lógica existentes son las principales ventajas. Pero es probable que los objetos de base de datos administrados sean menos eficientes cuando se trabaja con conjuntos de datos que no implican mucha lógica de procedimiento. Para obtener una explicación más detallada sobre las ventajas del uso de código administrado en lugar de T-SQL, consulte las [ventajas de usar código administrado para crear objetos de base de datos](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).

## <a name="step-1-moving-the-northwind-database-out-ofapp_data"></a>Paso 1: mover la base de datos Northwind fuera de`App_Data`

En este momento, todos nuestros tutoriales han usado un archivo de base de datos de Microsoft SQL Server 2005 Express Edition en la carpeta de `App_Data` de aplicaciones Web. Colocar la base de datos en `App_Data` simplificar la distribución y ejecución de estos tutoriales, ya que todos los archivos se encuentran en un directorio y no se requiere ningún paso de configuración adicional para probar el tutorial.

Sin embargo, para este tutorial, se mueve la base de datos Northwind fuera de `App_Data` y se registra explícitamente en la instancia de la base de datos SQL Server 2005 Express Edition. Aunque se pueden realizar los pasos de este tutorial con la base de datos de la carpeta `App_Data`, es mucho más sencillo realizar una serie de pasos registrando explícitamente la base de datos con la instancia de base de datos SQL Server 2005 Express Edition.

La descarga de este tutorial tiene los dos archivos de base de datos: `NORTHWND.MDF` y `NORTHWND_log.LDF`-colocados en una carpeta denominada `DataFiles`. Si va a seguir con su propia implementación de los tutoriales, cierre Visual Studio y mueva los archivos de `NORTHWND.MDF` y `NORTHWND_log.LDF` de la carpeta websites `App_Data` a una carpeta fuera del sitio Web. Una vez que los archivos de base de datos se han migrado a otra carpeta, es necesario registrar la base de datos Northwind con la instancia de base de datos SQL Server 2005 Express Edition. Esto se puede hacer desde SQL Server Management Studio. Si tiene instalada una edición no Express de SQL Server 2005 en el equipo, es probable que ya tenga Management Studio instalado. Si solo tiene SQL Server 2005 Express Edition en el equipo, dedique un momento a descargar e instalar [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Inicie SQL Server Management Studio. Como se muestra en la figura 1, Management Studio comienza preguntando con qué servidor se va a conectar. Escriba localhost\SQLExpress para el nombre del servidor, elija autenticación de Windows en la lista desplegable autenticación y haga clic en conectar.

![Conectarse a la instancia de base de datos adecuada](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Figura 1**: conexión a la instancia de base de datos adecuada

Una vez conectado, la ventana de Explorador de objetos mostrará información acerca de la instancia de la base de datos de SQL Server 2005 Express Edition, incluidas las bases de datos, la información de seguridad, las opciones de administración, etc.

Necesitamos adjuntar la base de datos Northwind en la `DataFiles` carpeta (o dondequiera que la haya colocado) en la instancia de base de datos de SQL Server 2005 Express Edition. Haga clic con el botón derecho en la carpeta bases de datos y elija la opción adjuntar en el menú contextual. Se abrirá el cuadro de diálogo Adjuntar bases de datos. Haga clic en el botón Agregar, explore en profundidad hasta el archivo de `NORTHWND.MDF` correspondiente y haga clic en Aceptar. En este momento, la pantalla debe tener un aspecto similar al de la figura 2.

[![conectarse a la instancia de base de datos adecuada](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Figura 2**: Conéctese a la instancia de base de datos adecuada ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))

> [!NOTE]
> Al conectarse a la instancia de SQL Server 2005 Express Edition mediante Management Studio el cuadro de diálogo Adjuntar bases de datos no permite explorar en profundidad los directorios de Perfil de usuario, como mis documentos. Por lo tanto, asegúrese de colocar los archivos `NORTHWND.MDF` y `NORTHWND_log.LDF` en un directorio de perfil que no sea de usuario.

Haga clic en el botón Aceptar para adjuntar la base de datos. El cuadro de diálogo Adjuntar bases de datos se cerrará y el Explorador de objetos debería mostrar ahora la base de datos asociada. Lo más probable es que la base de datos Northwind tenga un nombre como `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Cambie el nombre de la base de datos a Northwind; para ello, haga clic con el botón derecho en la base de datos y elija cambiar nombre.

![Cambiar el nombre de la base de datos a Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Figura 3**: cambiar el nombre de la base de datos a Northwind

## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Paso 2: creación de una nueva solución y SQL Server proyecto en Visual Studio

Para crear procedimientos almacenados administrados o UDF en SQL Server 2005 se escribirá el procedimiento almacenado y la lógica C# de UDF como código en una clase. Una vez que se ha escrito el código, deberá compilar esta clase en un ensamblado (un archivo `.dll`), registrar el ensamblado con la SQL Server base de datos y, a continuación, crear un procedimiento almacenado o un objeto UDF en la base de datos que apunte al método correspondiente del ensamblado. Estos pasos se pueden realizar de forma manual. Podemos crear el código en cualquier editor de texto, compilarlo desde la línea de comandos C# mediante el compilador ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), registrarlo con la base de datos mediante el comando [`CREATE ASSEMBLY`](https://msdn.microsoft.com/library/ms189524.aspx) o desde Management Studio y agregar el procedimiento almacenado o el objeto UDF a través de medios similares. Afortunadamente, las versiones Professional y Team Systems de Visual Studio incluyen un tipo de proyecto SQL Server que automatiza estas tareas. En este tutorial se le guiará por el uso del tipo de proyecto SQL Server para crear un procedimiento almacenado administrado y una UDF.

> [!NOTE]
> Si usa Visual Web Developer o la edición Standard de Visual Studio, tendrá que usar el enfoque manual en su lugar. En el paso 13 se proporcionan instrucciones detalladas para realizar estos pasos manualmente. Recomiendo que lea los pasos del 2 al 12 antes de leer el paso 13, ya que estos pasos incluyen instrucciones de configuración de SQL Server importantes que se deben aplicar independientemente de la versión de Visual Studio que esté usando.

Abra Visual Studio. En el menú Archivo, elija nuevo proyecto para mostrar el cuadro de diálogo nuevo proyecto (vea la figura 4). Explore en profundidad hasta el tipo de proyecto de base de datos y, a continuación, desde las plantillas que aparecen a la derecha, elija crear un nuevo proyecto de SQL Server. He elegido nombrar este proyecto `ManagedDatabaseConstructs` y colocarlo en una solución denominada `Tutorial75`.

[![crear un nuevo proyecto de SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Figura 4**: crear un nuevo proyecto de SQL Server ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))

Haga clic en el botón Aceptar del cuadro de diálogo nuevo proyecto para crear la solución y SQL Server proyecto.

Un proyecto SQL Server está asociado a una base de datos determinada. Por lo tanto, después de crear el nuevo proyecto de SQL Server, se le pedirá inmediatamente que especifique esta información. En la figura 5 se muestra el cuadro de diálogo nueva referencia de base de datos que se ha rellenado para que apunte a la base de datos Northwind registrada en el SQL Server 2005 Express Edition instancia de base de datos en el paso 1.

![Asociar el proyecto de SQL Server a la base de datos Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Figura 5**: asociar el proyecto de SQL Server a la base de datos Northwind

Para depurar los procedimientos almacenados administrados y las UDF que se crearán en este proyecto, es necesario habilitar la compatibilidad con la depuración de SQL/CLR para la conexión. Cada vez que se asocia un proyecto de SQL Server a una nueva base de datos (como hicimos en la figura 5), Visual Studio nos pregunta si queremos habilitar la depuración de SQL/CLR en la conexión (consulte la figura 6). Haz clic en Sí.

![Habilitar la depuración de SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Figura 6**: habilitación de la depuración de SQL/CLR

En este momento, el nuevo proyecto de SQL Server se ha agregado a la solución. Contiene una carpeta denominada `Test Scripts` con un archivo denominado `Test.sql`, que se usa para depurar los objetos de base de datos administrados creados en el proyecto. Veremos la depuración en el paso 12.

Ahora podemos agregar nuevos procedimientos almacenados administrados y UDF a este proyecto, pero antes de que podamos incluir primero la aplicación web existente en la solución. En el menú Archivo, seleccione la opción Agregar y elija sitio web existente. Vaya a la carpeta del sitio web correspondiente y haga clic en Aceptar. Como se muestra en la figura 7, se actualizará la solución para incluir dos proyectos: el sitio web y el `ManagedDatabaseConstructs` SQL Server proyecto.

![El Explorador de soluciones ahora incluye dos proyectos](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Figura 7**: el explorador de soluciones ahora incluye dos proyectos

El valor `NORTHWNDConnectionString` en `Web.config` hace referencia actualmente al archivo `NORTHWND.MDF` en la carpeta `App_Data`. Dado que se ha quitado esta base de datos de `App_Data` y se ha registrado explícitamente en la instancia de SQL Server 2005 Express Edition Database, es necesario actualizar el valor de `NORTHWNDConnectionString`. Abra el archivo `Web.config` en el sitio web y cambie el valor `NORTHWNDConnectionString` para que la cadena de conexión Lea: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Después de este cambio, la sección `<connectionStrings>` en `Web.config` debe ser similar a la siguiente:

[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Como se explicó en el [tutorial anterior](debugging-stored-procedures-cs.md), al depurar un objeto SQL Server desde una aplicación cliente, como un sitio web de ASP.net, es necesario deshabilitar la agrupación de conexiones. La cadena de conexión mostrada anteriormente deshabilita la agrupación de conexiones (`Pooling=false`). Si no tiene previsto depurar los procedimientos almacenados administrados y los UDF desde el sitio web de ASP.NET, habilite la agrupación de conexiones.

## <a name="step-3-creating-a-managed-stored-procedure"></a>Paso 3: crear un procedimiento almacenado administrado

Para agregar un procedimiento almacenado administrado a la base de datos Northwind, primero es necesario crear el procedimiento almacenado como un método en el proyecto SQL Server. En el Explorador de soluciones, haga clic con el botón derecho en el nombre del proyecto `ManagedDatabaseConstructs` y elija Agregar un nuevo elemento. Se mostrará el cuadro de diálogo Agregar nuevo elemento, en el que se muestran los tipos de objetos de base de datos administrados que se pueden agregar al proyecto. Como se muestra en la figura 8, esto incluye procedimientos almacenados y funciones definidas por el usuario, entre otros.

Permita que s comience por agregar un procedimiento almacenado que simplemente devuelva todos los productos que se han suspendido. Asigne al nuevo archivo de procedimiento almacenado el nombre `GetDiscontinuedProducts.cs`.

[![agregar un nuevo procedimiento almacenado denominado GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Figura 8**: agregar un nuevo procedimiento almacenado denominado `GetDiscontinuedProducts.cs` ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))

Se creará un nuevo C# archivo de clase con el siguiente contenido:

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Tenga en cuenta que el procedimiento almacenado se implementa como un método `static` dentro de un archivo de clase `partial` denominado `StoredProcedures`. Además, el método `GetDiscontinuedProducts` se decora con el `SqlProcedure attribute`, que marca el método como un procedimiento almacenado.

En el código siguiente se crea un objeto `SqlCommand` y se establece su `CommandText` en una consulta `SELECT` que devuelve todas las columnas de la tabla `Products` para los productos cuyo campo `Discontinued` es igual a 1. Después, ejecuta el comando y devuelve los resultados a la aplicación cliente. Agregue este código al método `GetDiscontinuedProducts`.

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Todos los objetos de base de datos administrados tienen acceso a un [objeto`SqlContext`](https://msdn.microsoft.com/library/ms131108.aspx) que representa el contexto del llamador. El `SqlContext` proporciona acceso a un [objeto`SqlPipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) a través de su [propiedad`Pipe`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Este objeto `SqlPipe` se utiliza para transbordador información entre la base de datos SQL Server y la aplicación que realiza la llamada. Como su nombre implica, el [método`ExecuteAndSend`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) ejecuta un objeto `SqlCommand` pasado y envía los resultados de vuelta a la aplicación cliente.

> [!NOTE]
> Los objetos de base de datos administrados son más adecuados para los procedimientos almacenados y las UDF que usan la lógica de procedimientos en lugar de la lógica basada en conjuntos. La lógica de procedimientos implica trabajar con conjuntos de datos fila por fila o trabajar con datos escalares. Sin embargo, el método `GetDiscontinuedProducts` que se acaba de crear no implica ninguna lógica de procedimiento. Por lo tanto, idealmente se implementaría como un procedimiento almacenado de T-SQL. Se implementa como un procedimiento almacenado administrado para mostrar los pasos necesarios para crear e implementar procedimientos almacenados administrados.

## <a name="step-4-deploying-the-managed-stored-procedure"></a>Paso 4: implementar el procedimiento almacenado administrado

Con este código completo, estamos listos para implementarlo en la base de datos Northwind. La implementación de un proyecto de SQL Server compila el código en un ensamblado, registra el ensamblado con la base de datos y crea los objetos correspondientes en la base de datos, vinculándolos a los métodos adecuados en el ensamblado. El conjunto exacto de tareas que realiza la opción de implementación se escribe con más precisión en el paso 13. Haga clic con el botón derecho en el nombre del proyecto `ManagedDatabaseConstructs` en el Explorador de soluciones y elija la opción implementar. Sin embargo, se produce el siguiente error en la implementación: Sintaxis incorrecta cerca de ' EXTERNAL '. Es posible que tenga que establecer el nivel de compatibilidad de la base de datos actual en un valor superior para habilitar esta característica. Vea la ayuda del procedimiento almacenado `sp_dbcmptlevel`.

Este mensaje de error se produce al intentar registrar el ensamblado con la base de datos Northwind. Para registrar un ensamblado con una base de datos SQL Server 2005, el nivel de compatibilidad de la base de datos debe establecerse en 90. De forma predeterminada, las nuevas bases de datos de SQL Server 2005 tienen un nivel de compatibilidad de 90. Sin embargo, las bases de datos creadas con Microsoft SQL Server 2000 tienen un nivel de compatibilidad predeterminado de 80. Dado que la base de datos Northwind era inicialmente una Microsoft SQL Server base de datos 2000, su nivel de compatibilidad está establecido actualmente en 80 y, por tanto, debe aumentarse a 90 para poder registrar objetos de base de datos administrados.

Para actualizar el nivel de compatibilidad de la base de datos, abra una nueva ventana de consulta en Management Studio y escriba:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Haga clic en el icono ejecutar de la barra de herramientas para ejecutar la consulta anterior.

[![actualizar el nivel de compatibilidad de la base de datos Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Ilustración 9**: actualizar el nivel de compatibilidad de la base de datos Northwind ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))

Después de actualizar el nivel de compatibilidad, vuelva a implementar el proyecto SQL Server. Esta vez, la implementación debe completarse sin errores.

Vuelva a SQL Server Management Studio, haga clic con el botón derecho en la base de datos Northwind en el Explorador de objetos y elija actualizar. A continuación, explore en profundidad la carpeta programación y, a continuación, expanda la carpeta ensamblados. Como se muestra en la figura 10, la base de datos Northwind ahora incluye el ensamblado generado por el proyecto `ManagedDatabaseConstructs`.

![El ensamblado ManagedDatabaseConstructs ya está registrado con la base de datos Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Figura 10**: el ensamblado de `ManagedDatabaseConstructs` ahora se ha registrado con la base de datos Northwind

También expanda la carpeta procedimientos almacenados. Allí verá un procedimiento almacenado denominado `GetDiscontinuedProducts`. Este procedimiento almacenado lo creó el proceso de implementación y apunta al método `GetDiscontinuedProducts` del ensamblado `ManagedDatabaseConstructs`. Cuando se ejecuta el `GetDiscontinuedProducts` procedimiento almacenado, éste, a su vez, ejecuta el método `GetDiscontinuedProducts`. Dado que se trata de un procedimiento almacenado administrado, no se puede editar a través de Management Studio (por lo tanto, el icono de candado situado junto al nombre del procedimiento almacenado).

![El procedimiento almacenado GetDiscontinuedProducts se muestra en la carpeta procedimientos almacenados.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Figura 11**: el `GetDiscontinuedProducts` procedimiento almacenado se muestra en la carpeta procedimientos almacenados.

Todavía hay un obstáculo más que debemos solucionar antes de poder llamar al procedimiento almacenado administrado: la base de datos está configurada para evitar la ejecución de código administrado. Para comprobarlo, abra una nueva ventana de consulta y ejecute el `GetDiscontinuedProducts` procedimiento almacenado. Recibirá el mensaje de error siguiente: la ejecución del código de usuario en el .NET Framework está deshabilitada. Habilitar la opción de configuración CLR habilitado.

Para examinar la información de configuración de la base de datos Northwind, escriba y ejecute el comando `exec sp_configure` en la ventana de consulta. Esto muestra que el valor clr enabled está establecido actualmente en 0.

[![la opción clr enabled está establecida actualmente en 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Figura 12**: el valor clr enabled está establecido actualmente en 0 ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))

Tenga en cuenta que cada valor de configuración de la figura 12 tiene cuatro valores enumerados: los valores mínimo y máximo, y los valores de configuración y ejecución. Para actualizar el valor de configuración de la opción clr enabled, ejecute el siguiente comando:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Si vuelve a ejecutar el `exec sp_configure` verá que en la instrucción anterior se ha actualizado el valor de configuración s de configuración de CLR habilitado a 1, pero que el valor de ejecución sigue establecido en 0. Para que este cambio de configuración surta efecto, necesitamos ejecutar el [comando`RECONFIGURE`](https://msdn.microsoft.com/library/ms176069.aspx), que establecerá el valor de ejecución en el valor de configuración actual. Simplemente escriba `RECONFIGURE` en la ventana de consulta y haga clic en el icono ejecutar de la barra de herramientas. Si ejecuta `exec sp_configure` ahora debería ver el valor 1 para los valores de configuración y ejecución de configuración de CLR habilitados.

Una vez completada la configuración de CLR habilitada, estamos preparados para ejecutar el procedimiento almacenado `GetDiscontinuedProducts` administrado. En la ventana de consulta, escriba y ejecute el comando `exec` `GetDiscontinuedProducts`. Al invocar el procedimiento almacenado, se ejecuta el código administrado correspondiente en el método `GetDiscontinuedProducts`. Este código emite una consulta `SELECT` para devolver todos los productos que no se han suspendido y devuelve estos datos a la aplicación que realiza la llamada, que es SQL Server Management Studio en esta instancia. Management Studio recibe estos resultados y los muestra en la ventana resultados.

[![el procedimiento almacenado GetDiscontinuedProducts devuelve todos los productos que no se han suspendido](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Figura 13**: el `GetDiscontinuedProducts` procedimiento almacenado devuelve todos los productos que no se han suspendido ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))

## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Paso 5: crear procedimientos almacenados administrados que acepten parámetros de entrada

Muchas de las consultas y procedimientos almacenados que hemos creado en estos tutoriales han usado *parámetros*. Por ejemplo, en el tutorial [crear nuevos procedimientos almacenados para el conjunto de datos con tipo de TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) , hemos creado un procedimiento almacenado denominado `GetProductsByCategoryID` que aceptó un parámetro de entrada denominado `@CategoryID`. A continuación, el procedimiento almacenado devuelve todos los productos cuyo campo `CategoryID` coincidió con el valor del parámetro `@CategoryID` proporcionado.

Para crear un procedimiento almacenado administrado que acepte parámetros de entrada, solo tiene que especificar esos parámetros en la definición del método. Para ilustrar esto, se debe agregar otro procedimiento almacenado administrado al proyecto de `ManagedDatabaseConstructs` denominado `GetProductsWithPriceLessThan`. Este procedimiento almacenado administrado aceptará un parámetro de entrada que especifique un precio y devolverá todos los productos cuyo campo de `UnitPrice` sea menor que el valor del parámetro s.

Para agregar un nuevo procedimiento almacenado al proyecto, haga clic con el botón secundario en el nombre del proyecto `ManagedDatabaseConstructs` y elija Agregar un nuevo procedimiento almacenado. Asigne al archivo el nombre `GetProductsWithPriceLessThan.cs`. Como vimos en el paso 3, se creará un nuevo C# archivo de clase con un método denominado `GetProductsWithPriceLessThan` colocado dentro de la clase `partial` `StoredProcedures`.

Actualice la definición del método `GetProductsWithPriceLessThan` para que acepte un parámetro de entrada [`SqlMoney`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) denominado `price` y escriba el código para ejecutar y devolver los resultados de la consulta:

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

La definición y el código del método `GetProductsWithPriceLessThan` es muy similar a la definición y el código del método `GetDiscontinuedProducts` creado en el paso 3. Las únicas diferencias son que el método `GetProductsWithPriceLessThan` acepta como parámetro de entrada (`price`), la consulta `SqlCommand` s incluye un parámetro (`@MaxPrice`) y se agrega un parámetro a la colección `SqlCommand` `Parameters` s y se le asigna el valor de la variable `price`.

Después de agregar este código, vuelva a implementar el proyecto SQL Server. A continuación, vuelva a SQL Server Management Studio y actualice la carpeta procedimientos almacenados. Debería ver una entrada nueva `GetProductsWithPriceLessThan`. En una ventana de consulta, escriba y ejecute el comando `exec GetProductsWithPriceLessThan 25`, que enumerará todos los productos inferiores a $25, como se muestra en la figura 14.

[se muestran ![productos en $25](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Figura 14**: se muestran los productos en $25 ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))

## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Paso 6: llamar al procedimiento almacenado administrado desde el nivel de acceso a datos

En este punto se han agregado los procedimientos almacenados administrados `GetDiscontinuedProducts` y `GetProductsWithPriceLessThan` al proyecto `ManagedDatabaseConstructs` y se han registrado con la base de datos Northwind SQL Server. También se invocan estos procedimientos almacenados administrados desde SQL Server Management Studio (vea la figura s 13 y 14). Sin embargo, para que nuestra aplicación ASP.NET use estos procedimientos almacenados administrados, es necesario agregarlos al acceso a datos y a las capas de lógica de negocios de la arquitectura. En este paso, se agregarán dos nuevos métodos a la `ProductsTableAdapter` en el `NorthwindWithSprocs` conjunto de DataSet con tipo, que se creó inicialmente en el tutorial [crear nuevos procedimientos almacenados para el objeto DataSet](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . En el paso 7, agregaremos los métodos correspondientes a la capa BLL.

Abra el `NorthwindWithSprocs` conjunto de elementos con tipo en Visual Studio y empiece agregando un nuevo método al `ProductsTableAdapter` denominado `GetDiscontinuedProducts`. Para agregar un nuevo método a un TableAdapter, haga clic con el botón derecho en el nombre de TableAdapter en el diseñador y elija la opción Agregar consulta en el menú contextual.

> [!NOTE]
> Dado que se ha migrado la base de datos Northwind desde la carpeta `App_Data` a la instancia de SQL Server 2005 Express Edition Database, es imperativo que la cadena de conexión correspondiente en Web. config se actualice para reflejar este cambio. En el paso 2 se describe cómo actualizar el valor de `NORTHWNDConnectionString` en `Web.config`. Si olvidó realizar esta actualización, verá el mensaje de error no se pudo agregar la consulta. No se puede encontrar la conexión `NORTHWNDConnectionString` para el `Web.config` de objetos en un cuadro de diálogo al intentar agregar un nuevo método al TableAdapter. Para resolver este error, haga clic en aceptar y, a continuación, vaya a `Web.config` y actualice el valor de `NORTHWNDConnectionString` como se describe en el paso 2. A continuación, intente volver a agregar el método al TableAdapter. Esta vez debería funcionar sin errores.

Al agregar un nuevo método, se inicia el Asistente para configuración de consultas de TableAdapter, que hemos usado muchas veces en los tutoriales anteriores. El primer paso nos pide que especifique cómo el TableAdapter debe tener acceso a la base de datos: a través de una instrucción SQL ad hoc o a través de un procedimiento almacenado nuevo o existente. Dado que ya hemos creado y registrado el `GetDiscontinuedProducts` procedimiento almacenado administrado con la base de datos, elija la opción usar procedimiento almacenado existente y haga clic en siguiente.

[![elegir la opción usar procedimiento almacenado existente](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Figura 15**: elija la opción usar procedimiento almacenado existente ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))

En la pantalla siguiente se le pide el procedimiento almacenado que invocará el método. Elija el `GetDiscontinuedProducts` procedimiento almacenado administrado en la lista desplegable y haga clic en siguiente.

[![seleccionar el procedimiento almacenado administrado GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Figura 16**: seleccione el `GetDiscontinuedProducts` procedimiento almacenado administrado ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))

A continuación, se le pedirá que especifique si el procedimiento almacenado devuelve filas, un solo valor o nada. Como `GetDiscontinuedProducts` devuelve el conjunto de filas del producto no incluidas, elija la primera opción (datos tabulares) y haga clic en siguiente.

[![seleccionar la opción de datos tabulares](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Figura 17**: seleccione la opción datos tabulares ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))

La pantalla final del asistente nos permite especificar los patrones de acceso a datos usados y los nombres de los métodos resultantes. Deje ambas casillas activadas y asigne un nombre a los métodos `FillByDiscontinued` y `GetDiscontinuedProducts`. Haga clic en Finalizar para completar el asistente.

[![nombre de los métodos FillByDiscontinued y GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Figura 18**: asigne un nombre a los métodos `FillByDiscontinued` y `GetDiscontinuedProducts` ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))

Repita estos pasos para crear métodos denominados `FillByPriceLessThan` y `GetProductsWithPriceLessThan` en el `ProductsTableAdapter` para el `GetProductsWithPriceLessThan` procedimiento almacenado administrado.

En la figura 19 se muestra una captura de pantalla del diseñador de DataSet después de agregar los métodos al `ProductsTableAdapter` para los procedimientos almacenados administrados `GetDiscontinuedProducts` y `GetProductsWithPriceLessThan`.

[![ProductsTableAdapter incluye los nuevos métodos agregados en este paso](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Figura 19**: el `ProductsTableAdapter` incluye los nuevos métodos agregados en este paso ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))

## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Paso 7: agregar los métodos correspondientes a la capa de lógica de negocios

Ahora que hemos actualizado la capa de acceso a datos para incluir métodos para llamar a los procedimientos almacenados administrados agregados en los pasos 4 y 5, es necesario agregar los métodos correspondientes a la capa de lógica de negocios. Agregue los dos métodos siguientes a la clase `ProductsBLLWithSprocs`:

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Ambos métodos simplemente llaman al método DAL correspondiente y devuelven la instancia de `ProductsDataTable`. El `DataObjectMethodAttribute` marcado situado encima de cada método hace que estos métodos se incluyan en la lista desplegable de la pestaña seleccionar del Asistente para configurar origen de datos de ObjectDataSource s.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Paso 8: invocar los procedimientos almacenados administrados desde el nivel de presentación

Con las capas de lógica de negocios y de acceso a datos ampliadas para incluir la compatibilidad con la llamada a los procedimientos almacenados administrados `GetDiscontinuedProducts` y `GetProductsWithPriceLessThan`, ahora podemos mostrar estos resultados de los procedimientos almacenados a través de una página de ASP.NET.

Abra la página `ManagedFunctionsAndSprocs.aspx` en la carpeta `AdvancedDAL` y, en el cuadro de herramientas, arrastre un control GridView hasta el diseñador. Establezca la propiedad GridView s `ID` en `DiscontinuedProducts` y, desde su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `DiscontinuedProductsDataSource`. Configure ObjectDataSource para que extraiga sus datos del método `ProductsBLLWithSprocs` Class s `GetDiscontinuedProducts`.

[![configurar ObjectDataSource para usar la clase ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Figura 20**: configuración de ObjectDataSource para usar la clase `ProductsBLLWithSprocs` ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))

[![elegir el método GetDiscontinuedProducts en la lista desplegable de la pestaña seleccionar](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Figura 21**: elija el método de `GetDiscontinuedProducts` de la lista desplegable de la pestaña seleccionar ([haga clic para ver la imagen a tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))

Puesto que esta cuadrícula se usará para mostrar la información del producto, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguno) y, a continuación, haga clic en finalizar.

Al finalizar el asistente, Visual Studio agregará automáticamente un CheckBoxField para cada campo de datos en el `ProductsDataTable`. Dedique un momento a quitar todos estos campos, excepto `ProductName` y `Discontinued`, momento en el que el marcado declarativo de GridView y ObjectDataSource s debe ser similar al siguiente:

[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Tómese un momento para ver esta página a través de un explorador. Cuando se visita la página, ObjectDataSource llama al método `ProductsBLLWithSprocs` de clase `GetDiscontinuedProducts`. Como vimos en el paso 7, este método llama al método de `GetDiscontinuedProducts` DAL s `ProductsDataTable` Class s, que invoca el procedimiento almacenado `GetDiscontinuedProducts`. Este procedimiento almacenado es un procedimiento almacenado administrado y ejecuta el código creado en el paso 3, que devuelve los productos que no se han suspendido.

Los resultados devueltos por el procedimiento almacenado administrado se empaquetan en un `ProductsDataTable` por el DAL y después se devuelven a la capa BLL, que, a continuación, los devuelve al nivel de presentación donde están enlazados a GridView y se muestran. Como se esperaba, la cuadrícula muestra los productos que han dejado de estar disponibles.

[![se muestran los productos descontinuados](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Figura 22**: se enumeran los productos descontinuados ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))

Para practicar más, agregue un cuadro de texto y otro GridView a la página. Haga que este GridView muestre los productos que son menores que la cantidad especificada en el cuadro de texto llamando al método `ProductsBLLWithSprocs` Class s `GetProductsWithPriceLessThan`.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Paso 9: crear y llamar a UDF T-SQL

Las funciones definidas por el usuario, o UDF, son objetos de base de datos que imitan estrechamente la semántica de las funciones de los lenguajes de programación. Al igual que una C#función de, las UDF pueden incluir un número variable de parámetros de entrada y devolver un valor de un tipo determinado. Una UDF puede devolver datos escalares (una cadena, un entero, etc.) o datos tabulares. Vamos a echar un vistazo rápido a ambos tipos de UDF, empezando por una UDF que devuelve un tipo de datos escalar.

La siguiente UDF calcula el valor estimado del inventario para un producto determinado. Para ello, toma tres parámetros de entrada: los valores de `UnitPrice`, `UnitsInStock`y `Discontinued` de un producto determinado, y devuelve un valor de tipo `money`. Calcula el valor estimado del inventario multiplicando el `UnitPrice` por el `UnitsInStock`. En el caso de los elementos no continuos, este valor es la mitad.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Una vez que se ha agregado esta UDF a la base de datos, se puede encontrar en Management Studio expandiendo la carpeta programación, las funciones y las funciones de valor escalar. Se puede usar en una consulta de `SELECT` como la siguiente:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

He agregado la `udf_ComputeInventoryValue` UDF a la base de datos Northwind; En la Figura 23 se muestra el resultado de la consulta `SELECT` anterior cuando se ve a través de Management Studio. Tenga en cuenta también que la UDF aparece en la carpeta funciones de valores escalares en el Explorador de objetos.

[![se muestra cada uno de los valores de inventario de cada producto](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Figura 23**: se enumeran todos los valores de inventario de cada producto ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))

Las UDF también pueden devolver datos tabulares. Por ejemplo, podemos crear una UDF que devuelva productos que pertenezcan a una categoría determinada:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

La UDF `udf_GetProductsByCategoryID` acepta un parámetro de entrada `@CategoryID` y devuelve los resultados de la consulta `SELECT` especificada. Una vez creada, se puede hacer referencia a esta UDF en la cláusula `FROM` (o `JOIN`) de una consulta de `SELECT`. En el ejemplo siguiente se devolverían los valores `ProductID`, `ProductName`y `CategoryID` de cada una de las bebidas.

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

He agregado la `udf_GetProductsByCategoryID` UDF a la base de datos Northwind; En la Figura 24 se muestra el resultado de la consulta `SELECT` anterior cuando se ve a través de Management Studio. Las UDF que devuelven datos tabulares se pueden encontrar en la carpeta de funciones de valores de tabla de Explorador de objetos s.

[![se enumeran los ProductID, ProductName y CategoryID para cada bebida](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Figura 24**: el `ProductID`, el `ProductName`y el `CategoryID` se muestran para cada bebida ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))

> [!NOTE]
> Para obtener más información sobre la creación y el uso de UDF, consulte [Introducción a las funciones definidas por el usuario](http://www.sqlteam.com/item.asp?ItemID=1955). Consulte también [las ventajas y desventajas de las funciones definidas por el usuario](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).

## <a name="step-10-creating-a-managed-udf"></a>Paso 10: crear una UDF administrada

Las UDF `udf_ComputeInventoryValue` y `udf_GetProductsByCategoryID` creadas en los ejemplos anteriores son objetos de base de datos T-SQL. SQL Server 2005 también admite UDF administradas, que se pueden agregar al proyecto de `ManagedDatabaseConstructs` al igual que los procedimientos almacenados administrados de los pasos 3 y 5. En este paso, vamos a implementar el `udf_ComputeInventoryValue` UDF en código administrado.

Para agregar una UDF administrada al proyecto de `ManagedDatabaseConstructs`, haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones y elija Agregar un nuevo elemento. Seleccione la plantilla definida por el usuario en el cuadro de diálogo Agregar nuevo elemento y asigne al nuevo archivo UDF el nombre `udf_ComputeInventoryValue_Managed.cs`.

[![agregar una nueva UDF administrada al proyecto ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Figura 25**: agregar una nueva UDF administrada al proyecto de `ManagedDatabaseConstructs` ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))

La plantilla de función definida por el usuario crea una clase de `partial` denominada `UserDefinedFunctions` con un método cuyo nombre es el mismo que el nombre del archivo de clase (`udf_ComputeInventoryValue_Managed`, en esta instancia). Este método se decora mediante el [atributo`SqlFunction`](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), que marca el método como un UDF administrado.

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

Actualmente, el método `udf_ComputeInventoryValue` devuelve un [objeto`SqlString`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) y no acepta ningún parámetro de entrada. Necesitamos actualizar la definición del método para que acepte tres parámetros de entrada: `UnitPrice`, `UnitsInStock`y `Discontinued`-y devuelva un objeto `SqlMoney`. La lógica para calcular el valor de inventario es idéntica a la del UDF de T-SQL `udf_ComputeInventoryValue`.

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Tenga en cuenta que los parámetros de entrada del método UDF son de sus tipos SQL correspondientes: `SqlMoney` para el campo `UnitPrice`, [`SqlInt16`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) para `UnitsInStock`y [`SqlBoolean`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) para `Discontinued`. Estos tipos de datos reflejan los tipos definidos en la tabla `Products`: la `UnitPrice` columna es de tipo `money`, la `UnitsInStock` columna de tipo `smallint`y la columna `Discontinued` de tipo `bit`.

El código comienza creando una instancia de `SqlMoney` denominada `inventoryValue` a la que se le asigna un valor de 0. La tabla `Products` permite valores de `NULL` de base de datos en las columnas `UnitsInPrice` y `UnitsInStock`. Por lo tanto, debemos comprobar primero si estos valores contienen `NULL` s, que hacemos a través de la propiedad `SqlMoney` Object s [`IsNull`](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Si tanto `UnitPrice` como `UnitsInStock` contienen valores que no son de`NULL`, se calcula el `inventoryValue` para que sea el producto de los dos. A continuación, si `Discontinued` es true, se reducirá el valor.

> [!NOTE]
> El objeto `SqlMoney` solo permite multiplicar dos instancias de `SqlMoney`. No permite multiplicar una instancia de `SqlMoney` por un número de punto flotante literal. Por lo tanto, para la mitad de `inventoryValue` se multiplica por una nueva instancia de `SqlMoney` que tiene el valor 0,5.

## <a name="step-11-deploying-the-managed-udf"></a>Paso 11: implementación de la UDF administrada

Ahora que se ha creado la UDF administrada, estamos listos para implementarla en la base de datos Northwind. Como vimos en el paso 4, los objetos administrados de un proyecto SQL Server se implementan haciendo clic con el botón derecho en el nombre del proyecto en el Explorador de soluciones y eligiendo la opción implementar en el menú contextual.

Una vez que haya implementado el proyecto, vuelva a SQL Server Management Studio y actualice la carpeta funciones escalares. Ahora debería ver dos entradas:

- `dbo.udf_ComputeInventoryValue`: la UDF de T-SQL creada en el paso 9 y
- `dbo.udf ComputeInventoryValue_Managed`: la UDF administrada creada en el paso 10 que se acaba de implementar.

Para probar esta UDF administrada, ejecute la siguiente consulta desde Management Studio:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

Este comando usa el UDF de `udf ComputeInventoryValue_Managed` administrado en lugar de T-SQL `udf_ComputeInventoryValue` UDF, pero el resultado es el mismo. Consulte la Figura 23 para ver una captura de pantalla de la salida de UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Paso 12: depurar los objetos de base de datos administrados

En el tutorial [depuración de procedimientos almacenados](debugging-stored-procedures-cs.md) se describen las tres opciones para depurar SQL Server a través de Visual Studio: depuración directa de la base de datos, depuración de aplicaciones y depuración desde un proyecto de SQL Server. Los objetos de base de datos administrados no se pueden depurar mediante la depuración directa de la base de datos, pero se pueden depurar desde una aplicación cliente y directamente desde el proyecto SQL Server. Sin embargo, para que la depuración funcione, la base de datos SQL Server 2005 debe permitir la depuración de SQL/CLR. Recuerde que la primera vez que creamos el proyecto `ManagedDatabaseConstructs` Visual Studio nos preguntó si deseamos habilitar la depuración de SQL/CLR (consulte la figura 6 en el paso 2). Para modificar esta configuración, haga clic con el botón derecho en la base de datos desde la ventana Explorador de servidores.

![Asegurarse de que la base de datos permita la depuración de SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Figura 26**: Asegúrese de que la base de datos permita la depuración de SQL/CLR

Imagine que desea depurar el `GetProductsWithPriceLessThan` procedimiento almacenado administrado. Comenzaremos estableciendo un punto de interrupción en el código del método `GetProductsWithPriceLessThan`.

[![establecer un punto de interrupción en el método GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Figura 27**: establecimiento de un punto de interrupción en el método `GetProductsWithPriceLessThan` ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))

Vamos a examinar primero la depuración de los objetos de base de datos administrados desde el proyecto SQL Server. Puesto que nuestra solución incluye dos proyectos: el `ManagedDatabaseConstructs` SQL Server proyecto junto con nuestro sitio web, para depurar desde el proyecto de SQL Server, necesitamos indicar a Visual Studio que inicie el `ManagedDatabaseConstructs` SQL Server proyecto cuando iniciemos la depuración. Haga clic con el botón derecho en el proyecto `ManagedDatabaseConstructs` en Explorador de soluciones y elija la opción establecer como proyecto de inicio en el menú contextual.

Cuando el proyecto de `ManagedDatabaseConstructs` se inicia desde el depurador, ejecuta las instrucciones SQL en el archivo `Test.sql`, que se encuentra en la carpeta `Test Scripts`. Por ejemplo, para probar el `GetProductsWithPriceLessThan` procedimiento almacenado administrado, reemplace el contenido del archivo de `Test.sql` existente por la siguiente instrucción, que invoca el `GetProductsWithPriceLessThan` procedimiento almacenado administrado pasando el valor `@CategoryID` de 14,95:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Una vez que haya escrito el script anterior en `Test.sql`, inicie la depuración. para ello, vaya al menú Depurar, elija iniciar depuración o presione F5 o el icono de reproducción verde en la barra de herramientas. Esto compilará los proyectos dentro de la solución, implementará los objetos de base de datos administrados en la base de datos Northwind y, a continuación, ejecutará el script `Test.sql`. En este momento, se alcanzará el punto de interrupción y se puede recorrer el método `GetProductsWithPriceLessThan`, examinar los valores de los parámetros de entrada, etc.

[![se alcanzó el punto de interrupción en el método GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Figura 28**: se ha alcanzado el punto de interrupción en el método `GetProductsWithPriceLessThan` ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))

Para que un objeto de SQL Database se depure a través de una aplicación cliente, es imperativo que la base de datos esté configurada para admitir la depuración de aplicaciones. Haga clic con el botón secundario en la base de datos en Explorador de servidores y asegúrese de que la opción depuración de la aplicación está activada. Además, es necesario configurar la aplicación ASP.NET para que se integre con el depurador de SQL y para deshabilitar la agrupación de conexiones. Estos pasos se trataron en detalle en el paso 2 del tutorial [depuración de procedimientos almacenados](debugging-stored-procedures-cs.md) .

Una vez que haya configurado la aplicación y la base de datos de ASP.NET, establezca el sitio web de ASP.NET como proyecto de inicio e inicie la depuración. Si visita una página que llama a uno de los objetos administrados que tiene un punto de interrupción, la aplicación se detendrá y el control se desactivará en el depurador, donde podrá recorrer el código como se muestra en la figura 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Paso 13: compilar e implementar manualmente objetos de base de datos administrados

SQL Server proyectos facilitan la creación, compilación e implementación de objetos de base de datos administrados. Desafortunadamente, los proyectos de SQL Server solo están disponibles en las ediciones Professional y Team Systems de Visual Studio. Si usa Visual Web Developer o la edición Standard de Visual Studio y desea usar objetos de base de datos administrados, tendrá que crearlos e implementarlos manualmente. Esto implica cuatro pasos:

1. Cree un archivo que contenga el código fuente para el objeto de base de datos administrado.
2. Compile el objeto en un ensamblado,
3. Registre el ensamblado con la base de datos SQL Server 2005 y
4. Cree un objeto de base de datos en SQL Server que apunte al método adecuado en el ensamblado.

Para ilustrar estas tareas, cree un nuevo procedimiento almacenado administrado que devuelva los productos cuya `UnitPrice` sea mayor que un valor especificado. Cree un nuevo archivo en el equipo denominado `GetProductsWithPriceGreaterThan.cs` y escriba el siguiente código en el archivo (puede usar Visual Studio, el Bloc de notas o cualquier editor de texto para hacerlo):

[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Este código es casi idéntico al del `GetProductsWithPriceLessThan` método creado en el paso 5. Las únicas diferencias son los nombres de método, la cláusula `WHERE` y el nombre de parámetro usado en la consulta. De nuevo en el método `GetProductsWithPriceLessThan`, la cláusula `WHERE` lee: `WHERE UnitPrice < @MaxPrice`. Aquí, en `GetProductsWithPriceGreaterThan`, usamos: `WHERE UnitPrice > @MinPrice`.

Ahora tenemos que compilar esta clase en un ensamblado. Desde la línea de comandos, navegue hasta el directorio donde guardó el archivo de `GetProductsWithPriceGreaterThan.cs` y use C# el compilador (`csc.exe`) para compilar el archivo de clase en un ensamblado:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Si la carpeta que contiene `csc.exe` no está en el `PATH`del sistema, tendrá que hacer referencia completa a su ruta de acceso, `%WINDOWS%\Microsoft.NET\Framework\version\`, como:

[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]

[![compilar GetProductsWithPriceGreaterThan.cs en un ensamblado](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Figura 29**: compilar `GetProductsWithPriceGreaterThan.cs` en un ensamblado ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))

La marca `/t` especifica que el C# archivo de clase se debe compilar en un archivo DLL (en lugar de un ejecutable). La marca `/out` especifica el nombre del ensamblado resultante.

> [!NOTE]
> En lugar de compilar el archivo de clase `GetProductsWithPriceGreaterThan.cs` desde la línea de comandos, también podría usar [ C# visual Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) o crear un proyecto de biblioteca de clases independiente en Visual Studio Standard Edition. S Ren Jacob Lauritsen ha proporcionado un proyecto de Visual C# Express Edition con código para el `GetProductsWithPriceGreaterThan` procedimiento almacenado y los dos procedimientos almacenados administrados y la UDF creados en los pasos 3, 5 y 10. El proyecto s Ren s también incluye los comandos T-SQL necesarios para agregar los objetos de base de datos correspondientes.

Con el código compilado en un ensamblado, estamos listos para registrar el ensamblado en la base de datos SQL Server 2005. Esto puede realizarse a través de T-SQL, mediante el `CREATE ASSEMBLY`de comandos o a través de SQL Server Management Studio. Deje que se Centre en el uso de Management Studio.

En Management Studio, expanda la carpeta programación en la base de datos Northwind. Una de sus subcarpetas es Assemblies. Para agregar manualmente un nuevo ensamblado a la base de datos, haga clic con el botón derecho en la carpeta ensamblados y elija nuevo ensamblado en el menú contextual. Esto muestra el cuadro de diálogo nuevo ensamblado (vea la figura 30). Haga clic en el botón examinar, seleccione el ensamblado `ManuallyCreatedDBObjects.dll` que acaba de compilar y, a continuación, haga clic en Aceptar para agregar el ensamblado a la base de datos. No debe ver el ensamblado `ManuallyCreatedDBObjects.dll` en el Explorador de objetos.

[![agregar el ensamblado ManuallyCreatedDBObjects. dll a la base de datos](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Figura 30**: agregar el ensamblado de `ManuallyCreatedDBObjects.dll` a la base de datos ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))

![El archivo ManuallyCreatedDBObjects. dll aparece en el Explorador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Figura 31**: el `ManuallyCreatedDBObjects.dll` aparece en el explorador de objetos

Aunque hemos agregado el ensamblado a la base de datos Northwind, todavía hemos asociado un procedimiento almacenado con el método `GetProductsWithPriceGreaterThan` del ensamblado. Para ello, abra una nueva ventana de consulta y ejecute el siguiente script:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Esto crea un nuevo procedimiento almacenado en la base de datos Northwind denominada `GetProductsWithPriceGreaterThan` y lo asocia con el método administrado `GetProductsWithPriceGreaterThan` (que se encuentra en la clase `StoredProcedures`, que se encuentra en el ensamblado `ManuallyCreatedDBObjects`).

Después de ejecutar el script anterior, actualice la carpeta procedimientos almacenados en el Explorador de objetos. Debería ver un nuevo procedimiento almacenado, `GetProductsWithPriceGreaterThan`, que tiene un icono de candado junto a él. Para probar este procedimiento almacenado, escriba y ejecute el siguiente script en la ventana de consulta:

[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Como se muestra en la Figura 32, el comando anterior muestra información de los productos con un `UnitPrice` superior a $24,95.

[![ManuallyCreatedDBObjects. dll aparece en la Explorador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Figura 32**: la `ManuallyCreatedDBObjects.dll` se muestra en el explorador de objetos ([haga clic para ver la imagen de tamaño completo](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))

## <a name="summary"></a>Resumen

Microsoft SQL Server 2005 proporciona la integración con Common Language Runtime (CLR), que permite crear objetos de base de datos mediante código administrado. Anteriormente, estos objetos de base de datos solo se podían crear con T-SQL, pero ahora podemos crear estos objetos mediante lenguajes C#de programación de .net como. En este tutorial se han creado dos procedimientos almacenados administrados y una función administrada definida por el usuario.

Visual Studio s SQL Server tipo de proyecto facilita la creación, compilación e implementación de objetos de base de datos administrados. Además, ofrece una amplia compatibilidad con la depuración. Sin embargo, SQL Server tipos de proyecto solo están disponibles en las ediciones Professional y Team Systems de Visual Studio. Para aquellos que usen Visual Web Developer o la edición Standard de Visual Studio, los pasos de creación, compilación e implementación deben realizarse manualmente, como vimos en el paso 13.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Ventajas y desventajas de las funciones definidas por el usuario](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Crear SQL Server objetos 2005 en código administrado](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Crear desencadenadores mediante código administrado en SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Cómo: crear y ejecutar un procedimiento almacenado de SQL Server CLR](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Cómo: crear y ejecutar una función CLR SQL Server definida por el usuario](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Cómo: editar el script de `Test.sql` para ejecutar objetos de SQL](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Introducción a las funciones definidas por el usuario](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Código administrado y SQL Server 2005 (vídeo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Referencia de Transact-SQL](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Tutorial: crear un procedimiento almacenado en código administrado](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era S Ren Jacob Lauritsen. Además de revisar este artículo, S Ren también creó el proyecto de Visual C# Express Edition incluido en este artículo descargar para compilar manualmente los objetos de base de datos administrados. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](debugging-stored-procedures-cs.md)
> [Siguiente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
