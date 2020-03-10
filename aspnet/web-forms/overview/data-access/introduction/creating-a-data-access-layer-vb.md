---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Crear una capa de acceso a datos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial comenzaremos desde el principio y creamos la capa de acceso a datos (DAL), mediante conjuntos de datos con tipo, para tener acceso a la información de una base de datos.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 51c9255f80f83a68cf26decf318347752498491a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78489493"
---
# <a name="creating-a-data-access-layer-vb"></a>Crear una capa de acceso a datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) de la aplicación de ejemplo o [descarga de PDF](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> En este tutorial comenzaremos desde el principio y creamos la capa de acceso a datos (DAL), mediante conjuntos de datos con tipo, para tener acceso a la información de una base de datos.

## <a name="introduction"></a>Introducción

Como desarrolladores web, nuestra vidas gira en torno al trabajo con datos. Creamos bases de datos para almacenar los datos, código para recuperarlos y modificarlos, y páginas web para recopilarlos y resumirlos. Este es el primer tutorial de una serie larga que explorará las técnicas para implementar estos patrones comunes en ASP.NET 2,0. Comenzaremos por crear una [arquitectura de software](http://en.wikipedia.org/wiki/Software_architecture) compuesta por una capa de acceso a datos (dal) mediante conjuntos de datos con tipo, una capa de lógica de negocios (BLL) que aplica reglas de negocios personalizadas y un nivel de presentación compuesto por páginas de ASP.net que comparten un diseño de página común. Una vez que se ha creado este terreno de back-end, pasaremos a los informes, donde se muestra cómo mostrar, resumir, recopilar y validar los datos de una aplicación Web. Estos tutoriales están orientados a ser concisos y proporcionan instrucciones paso a paso con muchas capturas de pantalla para guiarle en el proceso visualmente. Cada tutorial está disponible en C# y Visual Basic versiones e incluye una descarga del código completo usado. (Este primer tutorial es bastante largo, pero el resto se presenta en fragmentos mucho más digestibles).

Para estos tutoriales, usaremos una versión Microsoft SQL Server 2005 Express Edition de la base de datos Northwind colocada en el directorio `App_Data`. Además del archivo de base de datos, la carpeta `App_Data` contiene también los scripts SQL para crear la base de datos, en caso de que desee usar una versión de base de datos diferente. Estos scripts también se pueden [descargar directamente de Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), si así lo prefiere. Si usa otra versión de SQL Server de la base de datos Northwind, deberá actualizar el valor de `NORTHWNDConnectionString` en el archivo `Web.config` de la aplicación. La aplicación web se compiló con Visual Studio 2005 Professional Edition como un proyecto de sitio web basado en sistema de archivos. Sin embargo, todos los tutoriales funcionarán igual de bien con la versión gratuita de Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

En este tutorial comenzaremos desde el principio y creamos la capa de acceso a datos (DAL), seguida de la creación de la [capa de lógica de negocios (BLL)](creating-a-business-logic-layer-vb.md) en el segundo tutorial y de cómo trabajar con el [diseño y la navegación de páginas](master-pages-and-site-navigation-vb.md) en el tercero. Los tutoriales después del tercero se basarán en la base establecida en los tres primeros. Tenemos mucho que cubrir en este primer tutorial, así que inicie Visual Studio y comencemos.

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Paso 1: crear un proyecto web y conectarse a la base de datos

Antes de poder crear la capa de acceso a datos (DAL), primero es necesario crear un sitio web y configurar nuestra base de datos. Empiece por crear un nuevo sitio Web ASP.NET basado en el sistema de archivos. Para ello, vaya al menú Archivo y elija nuevo sitio web, mostrando el cuadro de diálogo nuevo sitio Web. Elija la plantilla sitio Web ASP.NET, establezca la lista desplegable ubicación en sistema de archivos, elija una carpeta para colocar el sitio web y establezca el idioma en Visual Basic.

[![crear un nuevo sitio web basado en el sistema de archivos](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Figura 1**: crear un nuevo sitio web basado en el sistema de archivos ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image3.png))

Se creará un nuevo sitio web con una `Default.aspx` página ASP.NET, una carpeta `App_Data` y un archivo `Web.config`.

Una vez creado el sitio web, el siguiente paso es agregar una referencia a la base de datos en la Explorador de servidores de Visual Studio. Al agregar una base de datos al Explorador de servidores puede agregar tablas, procedimientos almacenados, vistas, etc. desde Visual Studio. También puede ver los datos de la tabla o crear sus propias consultas de forma manual o gráfica mediante el Generador de consultas. Además, cuando se crean los conjuntos de datos con tipo para la capa DAL, será necesario señalar Visual Studio en la base de datos desde la que se deben construir los conjuntos de datos con tipo. Aunque podemos proporcionar esta información de conexión en ese momento en el tiempo, Visual Studio rellena automáticamente una lista desplegable de las bases de datos ya registradas en el Explorador de servidores.

Los pasos para agregar la base de datos Northwind al Explorador de servidores dependen de si desea usar la base de datos de SQL Server 2005 Express Edition en la carpeta `App_Data` o si tiene una configuración de servidor de base de datos Microsoft SQL Server 2000 o 2005 que desea utilizar en su lugar.

## <a name="using-a-database-in-theapp_datafolder"></a>Usar una base de datos en la carpeta`App_Data`

Si no tiene un servidor de base de datos SQL Server 2000 o 2005 al que conectarse o simplemente desea evitar tener que agregar la base de datos a un servidor de base de datos, puede usar la versión SQL Server 2005 Express Edition de la base de datos Northwind que se encuentra en la carpeta `App_Data` del sitio web descargado (`NORTHWND.MDF`).

Una base de datos colocada en la carpeta `App_Data` se agrega automáticamente a la Explorador de servidores. Suponiendo que SQL Server 2005 Express Edition instalado en el equipo, debería ver un nodo denominado archivo NORTHWND. MDF en el Explorador de servidores, que puede expandir y explorar sus tablas, vistas, procedimientos almacenados, etc. (vea la figura 2).

La carpeta `App_Data` también puede contener archivos de `.mdb` de Microsoft Access, que, como sus homólogos SQL Server, se agregan automáticamente al Explorador de servidores. Si no desea usar ninguna de las opciones de SQL Server, siempre puede [descargar una versión de Microsoft Access del archivo de base de datos Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) y colocarla en el directorio `App_Data`. Sin embargo, tenga en cuenta que las bases de datos de Access no tienen tantas características como SQL Server y no están diseñadas para usarse en escenarios de sitios Web. Además, un par de los 35 tutoriales utilizará ciertas características de nivel de base de datos que no son compatibles con el acceso.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Conexión a la base de datos en un servidor de base de datos Microsoft SQL Server 2000 o 2005

Como alternativa, puede conectarse a una base de datos Northwind instalada en un servidor de base de datos. Si el servidor de base de datos todavía no tiene instalada la base de datos Northwind, primero debe agregarla al servidor de base de datos ejecutando el script de instalación incluido en la descarga de este tutorial o [descargando la versión SQL Server 2000 de Northwind y el script de instalación](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) directamente desde el sitio web de Microsoft.

Una vez instalada la base de datos, vaya al Explorador de servidores en Visual Studio, haga clic con el botón derecho en el nodo conexiones de datos y elija Agregar conexión. Si no ve el Explorador de servidores vaya a la vista/Explorador de servidores, o presione Ctrl + Alt + S. Se abrirá el cuadro de diálogo Agregar conexión, donde puede especificar el servidor al que se va a conectar, la información de autenticación y el nombre de la base de datos. Una vez que haya configurado correctamente la información de conexión de base de datos y haya hecho clic en el botón Aceptar, la base de datos se agregará como un nodo debajo del nodo conexiones de datos. Puede expandir el nodo de base de datos para explorar sus tablas, vistas, procedimientos almacenados, etc.

![Agregar una conexión a la base de datos Northwind del servidor de bases de datos](creating-a-data-access-layer-vb/_static/image4.png)

**Figura 2**: agregar una conexión a la base de datos Northwind del servidor de bases de datos

## <a name="step-2-creating-the-data-access-layer"></a>Paso 2: creación de la capa de acceso a datos

Al trabajar con datos, una opción consiste en incrustar la lógica específica de los datos directamente en el nivel de presentación (en una aplicación Web, las páginas de ASP.NET componen el nivel de presentación). Esto puede adoptar la forma de escribir código ADO.NET en la parte del código de la página ASP.NET o usar el control SqlDataSource de la parte de marcado. En cualquier caso, este enfoque acopla estrechamente la lógica de acceso a datos con la capa de presentación. Sin embargo, el enfoque recomendado es separar la lógica de acceso a datos de la capa de presentación. Esta capa independiente se conoce como la capa de acceso a datos, DAL para abreviar y se implementa normalmente como un proyecto de biblioteca de clases independiente. Las ventajas de esta arquitectura en capas están bien documentadas (consulte la sección "Lecturas adicionales" al final de este tutorial para obtener información sobre estas ventajas) y es el enfoque que se va a seguir en esta serie.

Todo el código específico del origen de datos subyacente, como la creación de una conexión a la base de datos, la emisión de `SELECT`, `INSERT`, `UPDATE`y `DELETE` comandos, etc. debe estar ubicado en la capa DAL. El nivel de presentación no debe contener ninguna referencia a ese código de acceso a datos, sino que debe realizar llamadas a la capa DAL para todas las solicitudes de datos. Los niveles de acceso a datos suelen contener métodos para tener acceso a los datos de la base de datos subyacente. La base de datos Northwind, por ejemplo, tiene `Products` y `Categories` tablas que registran los productos de venta y las categorías a las que pertenecen. En nuestro DAL, tendremos métodos como:

- `GetCategories(),` que devolverá información sobre todas las categorías
- `GetProducts()`, que devolverá información sobre todos los productos
- `GetProductsByCategoryID(categoryID)`, que devolverá todos los productos que pertenecen a una categoría especificada
- `GetProductByProductID(productID)`, que devolverá información sobre un producto determinado.

Estos métodos, cuando se invocan, se conectan a la base de datos, emiten la consulta adecuada y devuelven los resultados. La forma en que se devuelven estos resultados es importante. Estos métodos podrían devolver simplemente un conjunto de datos o DataReader rellenado por la consulta de base de datos, pero idealmente estos resultados se deben devolver mediante *objetos fuertemente tipados*. Un objeto fuertemente tipado es aquél cuyo esquema se define de forma rígida en tiempo de compilación, mientras que el contrario, un objeto débilmente tipado, es aquél cuyo esquema no se conoce hasta el tiempo de ejecución.

Por ejemplo, DataReader y el conjunto de datos (de forma predeterminada) son objetos débilmente tipados, ya que su esquema está definido por las columnas devueltas por la consulta de base de datos utilizada para rellenarlos. Para tener acceso a una columna determinada desde un objeto DataTable con tipo flexible, es necesario usar una sintaxis como: `DataTable.Rows(index)("columnName")`. La escritura floja de la DataTable en este ejemplo se exhibe por el hecho de que necesitamos tener acceso al nombre de la columna mediante una cadena o un índice ordinal. Por otro lado, una DataTable fuertemente tipada tendrá cada una de sus columnas implementadas como propiedades, lo que dará lugar a un código similar al siguiente: `DataTable.Rows(index).columnName`.

Para devolver objetos fuertemente tipados, los desarrolladores pueden crear sus propios objetos comerciales personalizados o utilizar conjuntos de DataSet con tipo. El desarrollador implementa un objeto comercial como una clase cuyas propiedades suelen reflejar las columnas de la tabla de base de datos subyacente que representa el objeto comercial. Un conjunto de datos con tipo es una clase que Visual Studio genera automáticamente en función de un esquema de base de datos y cuyos miembros están fuertemente tipados según este esquema. El propio conjunto de tipos se compone de clases que extienden las clases DataSet, DataTable y DataRow de ADO.NET. Además de las tablas de datos fuertemente tipadas, los conjuntos de datos con tipo también incluyen TableAdapters, que son clases con métodos para rellenar las DataTables del conjunto de datos y propagar las modificaciones de las tablas de datos de nuevo a la base de datos.

> [!NOTE]
> Para obtener más información sobre las ventajas y desventajas del uso de conjuntos de datos con tipo frente a objetos empresariales personalizados, consulte [diseño de componentes de capa de datos y transferencia de datos a través de niveles](https://msdn.microsoft.com/library/ms978496.aspx).

Usaremos conjuntos de valores fuertemente tipados para la arquitectura de estos tutoriales. En la figura 3 se muestra el flujo de trabajo entre los distintos niveles de una aplicación que usa conjuntos de valores con tipo.

[![todo el código de acceso a datos se relega a la capa DAL](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Figura 3**: el código de acceso a todos los datos se relega a la capa Dal ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image7.png))

## <a name="creating-a-typed-dataset-and-table-adapter"></a>Crear un conjunto de DataSet con tipo y un adaptador de tabla

Para empezar a crear nuestro DAL, empezamos agregando un conjunto de un conjunto de tipos a nuestro proyecto. Para ello, haga clic con el botón secundario en el nodo del proyecto en el Explorador de soluciones y elija Agregar un nuevo elemento. Seleccione la opción conjunto de los conjuntos de opciones en la lista de plantillas y asígnele el nombre `Northwind.xsd`.

[![elegir agregar un nuevo conjunto de nuevos al proyecto](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Figura 4**: elegir agregar un nuevo conjunto de información al proyecto ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image10.png))

Después de hacer clic en agregar, cuando se le pida que agregue el conjunto de los `App_Code` de la carpeta, elija sí. A continuación, se mostrará el diseñador del conjunto de objetos con tipo y se iniciará el Asistente para configuración de TableAdapter, lo que le permitirá agregar su primer TableAdapter al conjunto de tipos.

Un conjunto de datos con tipo actúa como una colección fuertemente tipada de datos; se compone de instancias de DataTable fuertemente tipadas, cada una de las cuales, a su vez, se compone de instancias de DataRow fuertemente tipadas. Vamos a crear un DataTable fuertemente tipado para cada una de las tablas de base de datos subyacentes con las que necesitamos trabajar en esta serie de tutoriales. Comencemos con la creación de un objeto DataTable para la tabla `Products`.

Tenga en cuenta que las tablas de datos fuertemente tipadas no incluyen información sobre cómo obtener acceso a los datos de la tabla de base de datos subyacente. Para recuperar los datos con el fin de rellenar la DataTable, usamos una clase TableAdapter, que funciona como la capa de acceso a datos. En el caso de la `Products` DataTable, el TableAdapter contendrá los métodos `GetProducts()`, `GetProductByCategoryID(categoryID)`, etc. que llamaremos desde el nivel de presentación. El rol de DataTable es actuar como los objetos fuertemente tipados que se usan para pasar datos entre las capas.

El Asistente para configuración de TableAdapter comienza solicitando seleccionar la base de datos con la que trabajar. La lista desplegable muestra las bases de datos en el Explorador de servidores. Si no agregó la base de datos Northwind al Explorador de servidores, puede hacer clic en el botón nueva conexión en este momento para hacerlo.

[![elija la base de datos Northwind en la lista desplegable](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Figura 5**: elija la base de datos Northwind en la lista desplegable ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image13.png))

Después de seleccionar la base de datos y hacer clic en siguiente, se le preguntará si desea guardar la cadena de conexión en el archivo de `Web.config`. Al guardar la cadena de conexión, evitará que esté codificada de forma rígida en las clases de TableAdapter, lo que simplifica las cosas si la información de la cadena de conexión cambia en el futuro. Si opta por guardar la cadena de conexión en el archivo de configuración, se coloca en la sección `<connectionStrings>`, que se puede [cifrar opcionalmente](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) para mejorar la seguridad o modificarse más adelante a través de la nueva página de propiedades de ASP.net 2,0 dentro de la herramienta de administración GUI de IIS, que es más idónea para los administradores.

[![guardar la cadena de conexión en Web. config](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Figura 6**: Guarde la cadena de conexión en `Web.config` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image16.png))

A continuación, es necesario definir el esquema para el primer objeto DataTable fuertemente tipado y proporcionar el primer método que el TableAdapter debe usar al rellenar el conjunto de DataSet fuertemente tipado. Estos dos pasos se realizan simultáneamente mediante la creación de una consulta que devuelve las columnas de la tabla que queremos reflejar en el DataTable. Al final del asistente, asignaremos un nombre de método a esta consulta. Una vez que se ha realizado, este método se puede invocar desde el nivel de presentación. El método ejecutará la consulta definida y rellenará una DataTable fuertemente tipada.

Para empezar a definir la consulta SQL, primero debemos indicar cómo deseamos que el TableAdapter emita la consulta. Podemos usar una instrucción SQL ad hoc, crear un nuevo procedimiento almacenado o usar un procedimiento almacenado existente. Para estos tutoriales, usaremos instrucciones SQL ad hoc. Consulte el artículo de [Brian Noyes](http://briannoyes.net/), [creación de una capa de acceso a datos con el diseñador de DataSet de Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) para obtener un ejemplo del uso de procedimientos almacenados.

[![consultar los datos mediante una instrucción SQL ad hoc](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Figura 7**: consulta de los datos mediante una instrucción SQL ad hoc ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image19.png))

En este momento, podemos escribir la consulta SQL a mano. Al crear el primer método en el TableAdapter, normalmente desea que la consulta devuelva las columnas que deben expresarse en el DataTable correspondiente. Para ello, se crea una consulta que devuelve todas las columnas y todas las filas de la tabla `Products`:

[![escriba la consulta SQL en el cuadro de texto](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Figura 8**: escriba la consulta SQL en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image22.png))

Como alternativa, use el Generador de consultas y construya gráficamente la consulta, como se muestra en la figura 9.

[![crear la consulta gráficamente, a través del editor de consultas](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Ilustración 9**: crear la consulta gráficamente mediante el editor de consultas ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image25.png))

Después de crear la consulta, pero antes de pasar a la siguiente pantalla, haga clic en el botón Opciones avanzadas. En los proyectos de sitio web, "generar instrucciones INSERT, Update y DELETE" es la única opción avanzada seleccionada de forma predeterminada. Si ejecuta este asistente desde una biblioteca de clases o un proyecto de Windows, la opción "usar simultaneidad optimista" también se seleccionará. Deje la opción "usar simultaneidad optimista" desactivada por ahora. Examinaremos la simultaneidad optimista en futuros tutoriales.

[![seleccionar solo la opción generar instrucciones INSERT, Update y DELETE](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Figura 10**: seleccione solo la opción generar instrucciones INSERT, Update y Delete ([haga clic para ver la imagen a tamaño completo](creating-a-data-access-layer-vb/_static/image28.png))

Después de comprobar las opciones avanzadas, haga clic en siguiente para pasar a la última pantalla. Aquí se le pedirá que seleccione los métodos que se van a agregar al TableAdapter. Hay dos patrones para rellenar los datos:

- **Rellenar un DataTable** con este enfoque se crea un método que toma un DataTable como parámetro y lo rellena basándose en los resultados de la consulta. Por ejemplo, la clase ADO.NET DataAdapter implementa este patrón con su método `Fill()`.
- **Devolver un DataTable** con este enfoque el método crea y rellena la DataTable automáticamente y la devuelve como el valor devuelto de los métodos.

Puede hacer que el TableAdapter implemente uno de estos patrones o ambos. También puede cambiar el nombre de los métodos que se proporcionan aquí. Vamos a dejar ambas casillas activadas, aunque solo usaremos el último patrón en estos tutoriales. Además, vamos a cambiar el nombre del método de `GetData` en su lugar genérico a `GetProducts`.

Si está activada, la casilla final "GenerateDBDirectMethods" crea los métodos `Insert()`, `Update()`y `Delete()` para el TableAdapter. Si deja esta opción desactivada, todas las actualizaciones deberán realizarse a través del método `Update()` único del TableAdapter, que toma el DataSet con tipo, un objeto DataTable, una sola DataRow o una matriz de DataRows. (Si ha desactivado la opción "generar instrucciones INSERT, Update y DELETE" de las propiedades avanzadas de la figura 9, la configuración de esta casilla no tendrá ningún efecto. Deje esta casilla activada.

[![cambiar el nombre del método de GetData a GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Figura 11**: cambie el nombre del método de `GetData` a `GetProducts` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image31.png))

Para completar el asistente, haga clic en finalizar. Una vez que se cierre el asistente, se devolverá al diseñador de DataSet, que muestra el DataTable que acabamos de crear. Puede ver la lista de columnas en el `Products` DataTable (`ProductID`, `ProductName`, etc.), así como los métodos del `ProductsTableAdapter` (`Fill()` y `GetProducts()`).

[![los productos DataTable y ProductsTableAdapter se han agregado al conjunto de DataSet con tipo](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Figura 12**: el `Products` DataTable y el `ProductsTableAdapter` se han agregado al conjunto de tipos ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image34.png))

En este punto, tenemos un conjunto de DataSet con tipo con una sola DataTable (`Northwind.Products`) y una clase DataAdapter fuertemente tipada (`NorthwindTableAdapters.ProductsTableAdapter`) con un método `GetProducts()`. Estos objetos se pueden usar para tener acceso a una lista de todos los productos desde código como:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Este código no nos requirió escribir un bit de código específico de acceso a datos. No tuvimos que crear instancias de ninguna clase ADO.NET, por lo que no tenemos que hacer referencia a ninguna cadena de conexión, consulta SQL o procedimiento almacenado. En su lugar, el TableAdapter proporciona el código de acceso a datos de bajo nivel para nosotros.

Cada objeto que se usa en este ejemplo también es fuertemente tipado, lo que permite que Visual Studio proporcione IntelliSense y la comprobación de tipos en tiempo de compilación. Y lo mejor de todos los objetos DataTable devueltos por TableAdapter se pueden enlazar a controles Web de datos de ASP.NET, como GridView, DetailsView, DropDownList, CheckBoxList y otros muchos. En el ejemplo siguiente se muestra cómo enlazar la DataTable devuelta por el método `GetProducts()` a GridView en simplemente una exploraciones de tres líneas de código dentro del controlador de eventos `Page_Load`.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]

[![la lista de productos se muestra en un control GridView](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Figura 13**: la lista de productos se muestra en un control GridView ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image37.png))

Aunque en este ejemplo se requiere que se escriban tres líneas de código en el controlador de eventos `Page_Load` de la página ASP.NET, en futuros tutoriales veremos cómo usar ObjectDataSource para recuperar los datos de la capa DAL mediante declaración. Con ObjectDataSource no tendremos que escribir ningún código, sino que también tendrá compatibilidad con la paginación y la ordenación.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Paso 3: agregar métodos parametrizados a la capa de acceso a datos

En este momento, nuestra `ProductsTableAdapter` clase tiene un método, `GetProducts()`, que devuelve todos los productos de la base de datos. Aunque ser capaz de trabajar con todos los productos es definitivamente útil, hay ocasiones en las que queremos recuperar información acerca de un producto específico o de todos los productos que pertenecen a una categoría determinada. Para agregar esta funcionalidad a nuestra capa de acceso a datos, podemos agregar métodos parametrizados al TableAdapter.

Vamos a agregar el método `GetProductsByCategoryID(categoryID)`. Para agregar un nuevo método a la capa DAL, vuelva al diseñador de DataSet, haga clic con el botón derecho en la sección `ProductsTableAdapter` y elija Agregar consulta.

![Haga clic con el botón derecho en el TableAdapter y elija Agregar consulta.](creating-a-data-access-layer-vb/_static/image38.png)

**Figura 14**: haga clic con el botón derecho en el TableAdapter y elija Agregar consulta.

En primer lugar, se le preguntará si desea tener acceso a la base de datos mediante una instrucción SQL ad hoc o un procedimiento almacenado nuevo o existente. Vamos a elegir usar una instrucción SQL ad hoc de nuevo. A continuación, se le preguntará qué tipo de consulta SQL nos gustaría usar. Dado que queremos devolver todos los productos que pertenecen a una categoría especificada, queremos escribir una instrucción `SELECT` que devuelva filas.

[![optar por crear una instrucción SELECT que devuelve filas](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Figura 15**: elija crear una instrucción `SELECT` que devuelva filas ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image41.png))

El siguiente paso consiste en definir la consulta SQL que se usa para tener acceso a los datos. Puesto que queremos devolver solo los productos que pertenecen a una categoría determinada, utilizo la misma instrucción `SELECT` de `GetProducts()`, pero agregue la siguiente cláusula `WHERE`: `WHERE CategoryID = @CategoryID`. El parámetro `@CategoryID` indica al asistente de TableAdapter que el método que se va a crear requerirá un parámetro de entrada del tipo correspondiente (es decir, un entero que acepta valores NULL).

[![especificar una consulta para devolver solo los productos de una categoría especificada](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Figura 16**: escriba una consulta para devolver solo los productos de la categoría especificada ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image44.png))

En el paso final, podemos elegir los patrones de acceso a datos que se van a usar, así como personalizar los nombres de los métodos generados. Para el patrón Fill, vamos a cambiar el nombre a `FillByCategoryID` y para el patrón de devolución de DataTable devuelto (los métodos de `GetX`), vamos a usar `GetProductsByCategoryID`.

[![elegir los nombres de los métodos de TableAdapter](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Figura 17**: elegir los nombres de los métodos de TableAdapter ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image47.png))

Después de completar el asistente, el diseñador de DataSet incluye los nuevos métodos de TableAdapter.

![Ahora se pueden consultar los productos por categoría.](creating-a-data-access-layer-vb/_static/image48.png)

**Figura 18**: ahora se pueden consultar los productos por categoría.

Dedique un momento a agregar un método de `GetProductByProductID(productID)` con la misma técnica.

Estas consultas con parámetros se pueden probar directamente desde el diseñador de DataSet. Haga clic con el botón derecho en el método del TableAdapter y elija vista previa de los datos. A continuación, escriba los valores que se usarán para los parámetros y haga clic en vista previa.

[![se muestran los productos que pertenecen a la categoría bebidas](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Figura 19**: se muestran los productos que pertenecen a la categoría bebidas ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image51.png))

Con el método `GetProductsByCategoryID(categoryID)` en la capa DAL, ahora podemos crear una página ASP.NET que muestra solo los productos de una categoría especificada. En el ejemplo siguiente se muestran todos los productos que se encuentran en la categoría Beverages, que tienen un `CategoryID` de 1.

Beverages.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]

[![se muestran los productos de la categoría bebidas](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Figura 20**: se muestran los productos de la categoría bebidas ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image54.png))

## <a name="step-4-inserting-updating-and-deleting-data"></a>Paso 4: insertar, actualizar y eliminar datos

Hay dos patrones que se suelen usar para insertar, actualizar y eliminar datos. El primer patrón, que llamaré modelo directo de base de datos, implica la creación de métodos que, cuando se invocan, emiten un comando `INSERT`, `UPDATE`o `DELETE` a la base de datos que funciona en un único registro de base de datos. Estos métodos normalmente se pasan en una serie de valores escalares (enteros, cadenas, valores booleanos, fechas y horas, etc.) que corresponden a los valores que se van a insertar, actualizar o eliminar. Por ejemplo, con este patrón para la tabla `Products` el método Delete tomaría en un parámetro entero, indicando el `ProductID` del registro que se va a eliminar, mientras que el método Insert tomaría una cadena para el `ProductName`, un decimal para la `UnitPrice`, un entero para la `UnitsOnStock`, etc.

[![cada solicitud INSERT, Update y Delete se envía a la base de datos inmediatamente](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Figura 21**: cada solicitud INSERT, Update y Delete se envía a la base de datos inmediatamente ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image57.png))

El otro patrón, al que haré referencia como el patrón de actualización por lotes, es actualizar todo un conjunto de filas, DataTable o colección de DataRows en una llamada al método. Con este patrón, un desarrollador elimina, inserta y modifica las filas de la fila de un DataTable y, a continuación, pasa esas filas o DataTable a un método Update. A continuación, este método enumera las filas de datos que se han pasado, determina si se han modificado, agregado o eliminado (a través del valor de la [propiedad RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) de DataRow) y emite la solicitud de base de datos adecuada para cada registro.

[![todos los cambios se sincronizan con la base de datos cuando se invoca el método de actualización](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Figura 22**: todos los cambios se sincronizan con la base de datos cuando se invoca el método de actualización ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image60.png))

El TableAdapter usa el patrón de actualización por lotes de forma predeterminada, pero también admite el patrón de DB Direct. Dado que se ha seleccionado la opción "generar instrucciones INSERT, Update y DELETE" desde las propiedades avanzadas al crear nuestro TableAdapter, el `ProductsTableAdapter` contiene un método `Update()`, que implementa el patrón de actualización por lotes. En concreto, el TableAdapter contiene un método `Update()` que se puede pasar al conjunto de información con tipo, a una DataTable fuertemente tipada o a una o varias filas de objetos. Si deja activada la casilla "GenerateDBDirectMethods" al crear primero el TableAdapter, el patrón de DB Direct también se implementará a través de los métodos `Insert()`, `Update()`y `Delete()`.

Ambos patrones de modificación de datos usan las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` del TableAdapter para emitir sus comandos `INSERT`, `UPDATE`y `DELETE` en la base de datos. Puede inspeccionar y modificar las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` haciendo clic en el TableAdapter en el diseñador de DataSet y, a continuación, vaya al ventana Propiedades. (Asegúrese de que ha seleccionado el TableAdapter y de que el objeto `ProductsTableAdapter` es el seleccionado en la lista desplegable del ventana Propiedades).

[![TableAdapter tiene propiedades InsertCommand, UpdateCommand y DeleteCommand](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Figura 23**: el TableAdapter tiene `InsertCommand`, `UpdateCommand`y `DeleteCommand` propiedades ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image63.png))

Para examinar o modificar cualquiera de estas propiedades de comando de base de datos, haga clic en la subpropiedad `CommandText`, que abrirá el Generador de consultas.

[![configurar las instrucciones INSERT, UPDATE y DELETE en el Generador de consultas](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Figura 24**: Configure las instrucciones `INSERT`, `UPDATE`y `DELETE` en el generador de consultas ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image66.png))

En el ejemplo de código siguiente se muestra cómo usar el patrón de actualización por lotes para duplicar el precio de todos los productos que no se han suspendido y que tienen 25 unidades de existencias o menos.

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

En el código siguiente se muestra cómo usar el patrón de base de datos Direct para eliminar mediante programación un determinado producto, actualizar uno y, a continuación, agregar uno nuevo:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Crear métodos INSERT, Update y DELETE personalizados

Los métodos `Insert()`, `Update()`y `Delete()` creados por el método de DB Direct pueden ser un poco complicados, especialmente en el caso de las tablas con muchas columnas. En el ejemplo de código anterior, sin la ayuda de IntelliSense, no es especialmente evidente qué `Products` columna de tabla se asigna a cada parámetro de entrada a los métodos `Update()` y `Insert()`. En ocasiones, es posible que solo quiera actualizar una o dos columnas, o que desee un método de `Insert()` personalizado que, quizás, devuelva el valor del campo `IDENTITY` (incremento automático) del registro que se acaba de insertar.

Para crear este tipo de método personalizado, vuelva al diseñador de DataSet. Haga clic con el botón secundario en el TableAdapter y elija Agregar consulta, y vuelva al asistente de TableAdapter. En la segunda pantalla se puede indicar el tipo de consulta que se va a crear. Vamos a crear un método que agrega un nuevo producto y, a continuación, devuelve el valor del `ProductID`del registro recién agregado. Por lo tanto, opte por crear una consulta de `INSERT`.

[![crear un método para agregar una nueva fila a la tabla Products](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Figura 25**: creación de un método para agregar una nueva fila a la tabla de `Products` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image69.png))

En la pantalla siguiente aparece el `CommandText` del `InsertCommand`. Aumente esta consulta agregando `SELECT SCOPE_IDENTITY()` al final de la consulta, que devolverá el último valor de identidad insertado en una columna de `IDENTITY` en el mismo ámbito. (Consulte la [documentación técnica](https://msdn.microsoft.com/library/ms190315.aspx) para obtener más información acerca de `SCOPE_IDENTITY()` y el motivo por el que probablemente quiera [usar el ámbito\_Identity () en lugar de @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx)). Asegúrese de finalizar la instrucción `INSERT` con un punto y coma antes de agregar la instrucción `SELECT`.

[![aumentar la consulta para devolver el valor SCOPE_IDENTITY ()](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Figura 26**: aumentar la consulta para que devuelva el valor `SCOPE_IDENTITY()` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image72.png))

Por último, asigne al nuevo método el nombre `InsertProduct`.

[![establecer el nombre del nuevo método en InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Figura 27**: establezca el nombre del nuevo método en `InsertProduct` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image75.png))

Cuando vuelva al diseñador de DataSet verá que el `ProductsTableAdapter` contiene un nuevo método, `InsertProduct`. Si este nuevo método no tiene un parámetro para cada columna de la tabla de `Products`, lo más probable es que haya olvidado finalizar la instrucción de `INSERT` con un punto y coma. Configure el método `InsertProduct` y asegúrese de que tiene un punto y coma delimitado por las instrucciones `INSERT` y `SELECT`.

De forma predeterminada, los métodos Insert emiten métodos que no son de consulta, lo que significa que devuelven el número de filas afectadas. Sin embargo, queremos que el método `InsertProduct` devuelva el valor devuelto por la consulta, no el número de filas afectadas. Para ello, ajuste la propiedad `ExecuteMode` del método `InsertProduct` en `Scalar`.

[![cambiar la propiedad ExecuteMode a escalar](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Figura 28**: cambie la propiedad `ExecuteMode` a `Scalar` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image78.png))

En el código siguiente se muestra este nuevo método de `InsertProduct` en acción:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Paso 5: completar la capa de acceso a datos

Tenga en cuenta que la clase `ProductsTableAdapters` devuelve los valores `CategoryID` y `SupplierID` de la tabla `Products`, pero no incluye la columna `CategoryName` de la tabla `Categories` o la columna `CompanyName` de la tabla `Suppliers`, aunque es probable que se muestren las columnas que se van a mostrar al mostrar la información del producto. Podemos aumentar el método inicial del TableAdapter, `GetProducts()`, para incluir los valores de las columnas `CategoryName` y `CompanyName`, que actualizarán la DataTable fuertemente tipada para incluir también estas nuevas columnas.

Sin embargo, esto puede presentar un problema, ya que los métodos del TableAdapter para insertar, actualizar y eliminar datos se basan en este método inicial. Afortunadamente, los métodos generados automáticamente para insertar, actualizar y eliminar no se ven afectados por las subconsultas en la cláusula `SELECT`. Al tener cuidado de agregar nuestras consultas a `Categories` y `Suppliers` como subconsultas, en lugar de `JOIN` s, evitaremos tener que volver a trabajar con esos métodos para modificar los datos. Haga clic con el botón derecho en el método `GetProducts()` del `ProductsTableAdapter` y elija configurar. A continuación, ajuste la cláusula `SELECT` para que tenga el aspecto siguiente:

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]

[![actualizar la instrucción SELECT para el método GetProducts ()](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Figura 29**: actualización de la instrucción `SELECT` para el método `GetProducts()` ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image81.png))

Después de actualizar el método de `GetProducts()` para utilizar esta nueva consulta, el objeto DataTable incluirá dos nuevas columnas: `CategoryName` y `SupplierName`.

![La DataTable Products tiene dos columnas nuevas](creating-a-data-access-layer-vb/_static/image82.png)

**Figura 30**: el `Products` DataTable tiene dos columnas nuevas

Dedique un momento a actualizar también la cláusula `SELECT` en el método `GetProductsByCategoryID(categoryID)`.

Si actualiza la `SELECT` de `GetProducts()` mediante la sintaxis de `JOIN`, el diseñador de DataSet no podrá generar automáticamente los métodos para insertar, actualizar y eliminar datos de la base de datos mediante el patrón de DB Direct. En su lugar, tendrá que crearlos manualmente de forma similar al método `InsertProduct` anteriormente en este tutorial. Además, tendrá que proporcionar manualmente los valores de las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` si desea utilizar el patrón de actualización por lotes.

## <a name="adding-the-remaining-tableadapters"></a>Agregar los TableAdapters restantes

Hasta ahora, solo hemos examinado el trabajo con un solo TableAdapter para una tabla de base de datos única. Sin embargo, la base de datos Northwind contiene varias tablas relacionadas con las que se debe trabajar en nuestra aplicación Web. Un DataSet con tipo puede contener varias tablas de tipos de objeto. Por lo tanto, para completar el DAL, necesitamos agregar DataTables para las otras tablas que vamos a usar en estos tutoriales. Para agregar un nuevo TableAdapter a un conjunto de DataSet con tipo, abra el diseñador de DataSet, haga clic con el botón derecho en el diseñador y elija Agregar/TableAdapter. Esto creará un nuevo DataTable y TableAdapter y le guiará a través del asistente que hemos examinado anteriormente en este tutorial.

Dedique unos minutos a crear los siguientes TableAdapters y métodos con las siguientes consultas. Tenga en cuenta que las consultas del `ProductsTableAdapter` incluyen las subconsultas para captar los nombres de categoría y proveedor de cada producto. Además, si ha estado siguiendo, ya ha agregado los métodos `GetProducts()` y `GetProductsByCategoryID(categoryID)` de la clase `ProductsTableAdapter`.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]

[![el diseñador de DataSet una vez agregados los cuatro TableAdapters](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Figura 31**: el diseñador de DataSet después de haber agregado los cuatro TableAdapters ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image85.png))

## <a name="adding-custom-code-to-the-dal"></a>Agregar código personalizado a la capa DAL

Los TableAdapters y DataTables que se agregan al conjunto de los tipos se expresan como un archivo de definición de esquemas XML (`Northwind.xsd`). Para ver esta información de esquema, haga clic con el botón derecho en el archivo `Northwind.xsd` en el Explorador de soluciones y elija Ver código.

[![el archivo de definición de esquemas XML (XSD) para el DataSet con tipo de Northwind](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Figura 32**: el archivo de definición de esquemas XML (XSD) para el conjunto de información con tipo Northwind ([haga clic para ver la imagen de tamaño completo](creating-a-data-access-layer-vb/_static/image88.png))

Esta información de esquema se traduce C# en o Visual Basic código en tiempo de diseño cuando se compila o en tiempo de ejecución (si es necesario), momento en el que se puede recorrer paso a paso el depurador. Para ver este código generado automáticamente, vaya a la Vista de clases y explore en profundidad hasta las clases TableAdapter o DataSet con tipo. Si no ve el Vista de clases en la pantalla, vaya al menú Ver y selecciónelo desde allí, o presione Ctrl + Mayús + C. En el Vista de clases puede ver las propiedades, los métodos y los eventos de las clases DataSet y TableAdapter con tipo. Para ver el código de un método determinado, haga doble clic en el nombre del método en el Vista de clases o haga clic con el botón derecho en él y elija ir a definición.

![Inspeccione el código generado automáticamente seleccionando ir a definición en el Vista de clases](creating-a-data-access-layer-vb/_static/image89.png)

**Figura 33**: Inspeccione el código generado automáticamente seleccionando ir a definición en el vista de clases

Aunque el código generado automáticamente puede ser un gran ahorro de tiempo, el código suele ser muy genérico y debe personalizarse para satisfacer las necesidades únicas de una aplicación. Sin embargo, el riesgo de extender el código generado automáticamente es que la herramienta que generó el código puede decidir que es el momento de "regenerar" y sobrescribir las personalizaciones. Con el nuevo concepto de clase parcial de .NET 2.0, es fácil dividir una clase en varios archivos. Esto nos permite agregar nuestros propios métodos, propiedades y eventos a las clases generadas automáticamente sin tener que preocuparse por la sobrescritura de Visual Studio de las personalizaciones.

Para mostrar cómo personalizar la capa DAL, vamos a agregar un método `GetProducts()` a la clase `SuppliersRow`. La clase `SuppliersRow` representa un único registro de la tabla `Suppliers`; cada proveedor puede ser un proveedor de cero a muchos productos, por lo que `GetProducts()` devolverá los productos del proveedor especificado. Para ello, cree un nuevo archivo de clase en la carpeta `App_Code` denominada `SuppliersRow.vb` y agregue el código siguiente:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Esta clase parcial indica al compilador que cuando se crea la clase `Northwind.SuppliersRow` para incluir el `GetProducts()` método que se acaba de definir. Si compila el proyecto y, a continuación, vuelve al Vista de clases verá `GetProducts()` aparece ahora como un método de `Northwind.SuppliersRow`.

![El método GetProducts () ahora forma parte de la clase Northwind. SuppliersRow](creating-a-data-access-layer-vb/_static/image90.png)

**Figura 34**: el método `GetProducts()` forma ahora parte de la clase `Northwind.SuppliersRow`

El método `GetProducts()` ahora se puede usar para enumerar el conjunto de productos para un proveedor determinado, como se muestra en el código siguiente:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Estos datos también se pueden mostrar en cualquiera de las páginas ASP. Controles Web de datos de la red. En la página siguiente se usa un control GridView con dos campos:

- BoundField que muestra el nombre de cada proveedor y
- TemplateField que contiene un control BulletedList que está enlazado a los resultados devueltos por el método `GetProducts()` para cada proveedor.

Examinaremos cómo mostrar estos informes maestros y detallados en futuros tutoriales. Por ahora, este ejemplo está diseñado para ilustrar el uso del método personalizado agregado a la clase `Northwind.SuppliersRow`.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]

[![el nombre de la empresa del proveedor aparece en la columna izquierda, sus productos a la derecha](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Figura 35**: el nombre de la empresa del proveedor aparece en la columna izquierda, sus productos a la derecha ([haga clic para ver la imagen a tamaño completo](creating-a-data-access-layer-vb/_static/image93.png))

## <a name="summary"></a>Resumen

Al crear una aplicación Web, la creación de la capa DAL debe ser uno de los primeros pasos que se produzca antes de empezar a crear el nivel de presentación. Con Visual Studio, la creación de una capa DAL basada en conjuntos de objetos con tipo es una tarea que se puede realizar en 10-15 minutos sin escribir una línea de código. Los tutoriales de avance se basarán en esta capa DAL. En el [siguiente tutorial](creating-a-business-logic-layer-vb.md) , vamos a definir un número de reglas de negocios y veremos cómo implementarlas en una capa de lógica de negocios independiente.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Compilar una capa DAL mediante TableAdapters fuertemente tipados y DataTables en VS 2005 y ASP.NET 2,0](https://weblogs.asp.net/scottgu/435498)
- [Diseñar componentes de capa de datos y pasar datos a través de niveles](https://msdn.microsoft.com/library/ms978496.aspx)
- [Crear una capa de acceso a datos con el diseñador de DataSet de Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Cifrar la información de configuración en aplicaciones ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Introducción a TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Trabajar con un conjunto de DataSet con tipo](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Usar el acceso a datos fuertemente tipado en Visual Studio 2005 y ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Cómo extender métodos de TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recuperar datos escalares de un procedimiento almacenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Vídeo de aprendizaje sobre temas incluidos en este tutorial

- [Niveles de acceso a datos en aplicaciones de ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Cómo enlazar manualmente un conjunto de objeto a un control DataGrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Cómo trabajar con conjuntos de información y filtros desde una aplicación ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial son Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez y Carlos Santos. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-site-navigation-cs.md)
> [Siguiente](creating-a-business-logic-layer-vb.md)
