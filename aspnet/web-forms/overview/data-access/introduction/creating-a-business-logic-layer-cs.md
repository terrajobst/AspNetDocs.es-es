---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Crear una capa de lógica deC#negocios () | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo centralizar las reglas de negocios en una capa de lógica de negocios (BLL) que actúa como intermediario para el intercambio de datos entre t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: df96f3e7422a0537bf1b003a33fe8d71a671ac33
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586193"
---
# <a name="creating-a-business-logic-layer-c"></a>Crear una capa de lógica empresarial (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) de la aplicación de ejemplo o [descarga de PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> En este tutorial, veremos cómo centralizar las reglas de negocios en una capa de lógica de negocios (BLL) que actúa como intermediario para el intercambio de datos entre el nivel de presentación y la capa DAL.

## <a name="introduction"></a>Introducción

La capa de acceso a datos (DAL) creada en el [primer tutorial](creating-a-data-access-layer-cs.md) separa sin problemas la lógica de acceso a datos de la lógica de presentación. Sin embargo, aunque la capa DAL separa sin problemas los detalles de acceso a los datos del nivel de presentación, no aplica las reglas de negocios que se puedan aplicar. Por ejemplo, para nuestra aplicación, es posible que deseemos no permitir que se modifiquen los campos `CategoryID` o `SupplierID` de la tabla de `Products` cuando el campo de `Discontinued` esté establecido en 1, o bien podríamos aplicar reglas de altaidad, lo que prohíbe situaciones en las que un empleado está administrado por alguien contratado tras ellos. Otro escenario común es la autorización. quizás solo los usuarios de un rol determinado pueden eliminar productos o cambiar el valor de `UnitPrice`.

En este tutorial, veremos cómo centralizar estas reglas de negocios en una capa de lógica de negocios (BLL) que actúa como intermediario para el intercambio de datos entre el nivel de presentación y la capa DAL. En una aplicación real, el BLL debe implementarse como un proyecto de biblioteca de clases independiente. sin embargo, para estos tutoriales, implementaremos BLL como una serie de clases en la carpeta `App_Code` para simplificar la estructura del proyecto. En la figura 1 se muestran las relaciones arquitectónicas entre el nivel de presentación, BLL y DAL.

![El BLL separa el nivel de presentación de la capa de acceso a datos e impone reglas de negocios.](creating-a-business-logic-layer-cs/_static/image1.png)

**Figura 1**: el BLL separa el nivel de presentación de la capa de acceso a datos e impone reglas de negocios

## <a name="step-1-creating-the-bll-classes"></a>Paso 1: creación de las clases de BLL

Nuestro BLL se compone de cuatro clases, una para cada TableAdapter en la capa DAL; cada una de estas clases BLL tendrá métodos para recuperar, insertar, actualizar y eliminar desde el TableAdapter respectivo en la capa DAL, aplicando las reglas de negocios apropiadas.

Para separar más claramente las clases relacionadas con DAL y BLL, vamos a crear dos subcarpetas en la carpeta `App_Code`, `DAL` y `BLL`. Simplemente haga clic con el botón derecho en la carpeta `App_Code` del Explorador de soluciones y elija nueva carpeta. Después de crear estas dos carpetas, mueva el conjunto de tipos creado en el primer tutorial a la subcarpeta `DAL`.

A continuación, cree los cuatro archivos de clase BLL en la subcarpeta `BLL`. Para ello, haga clic con el botón derecho en la subcarpeta `BLL`, elija Agregar un nuevo elemento y seleccione la plantilla clase. Asigne a las cuatro clases el nombre `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`y `EmployeesBLL`.

![Agregue cuatro clases nuevas a la carpeta App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figura 2**: agregar cuatro clases nuevas a la carpeta `App_Code`

A continuación, vamos a agregar métodos a cada una de las clases para ajustar simplemente los métodos definidos para los TableAdapters del primer tutorial. Por ahora, estos métodos solo llamarán directamente a la capa DAL; volveremos más adelante para agregar cualquier lógica de negocios necesaria.

> [!NOTE]
> Si usa Visual Studio Standard Edition o una versión posterior (es decir, *no* está usando Visual Web Developer), puede diseñar sus clases de forma visual mediante el [Diseñador de clases](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Consulte el [blog de diseñador de clases](https://blogs.msdn.com/classdesigner/default.aspx) para obtener más información sobre esta nueva característica de Visual Studio.

En el caso de la clase `ProductsBLL` es necesario agregar un total de siete métodos:

- `GetProducts()` devuelve todos los productos
- `GetProductByProductID(productID)` devuelve el producto con el ID. de producto especificado
- `GetProductsByCategoryID(categoryID)` devuelve todos los productos de la categoría especificada
- `GetProductsBySupplier(supplierID)` devuelve todos los productos del proveedor especificado
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` inserta un nuevo producto en la base de datos con los valores pasados; Devuelve el valor `ProductID` del registro recién insertado.
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` actualiza un producto existente en la base de datos con los valores pasados; Devuelve `true` si se actualizó exactamente una fila; de lo contrario, `false`
- `DeleteProduct(productID)` elimina el producto especificado de la base de datos

ProductsBLL.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Los métodos que simplemente devuelven datos `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`y `GetProductBySuppliersID` son bastante sencillos, ya que simplemente llaman a la capa DAL. Aunque en algunos escenarios puede haber reglas de negocios que deban implementarse en este nivel (por ejemplo, reglas de autorización basadas en el usuario que ha iniciado sesión actualmente o en el rol al que pertenece el usuario), simplemente dejaremos estos métodos tal cual. Para estos métodos, el BLL sirve simplemente como proxy a través del cual el nivel de presentación tiene acceso a los datos subyacentes desde la capa de acceso a datos.

Los métodos `AddProduct` y `UpdateProduct` toman como parámetros los valores de los distintos campos de producto y agregan un nuevo producto o actualizan uno existente, respectivamente. Dado que muchas de las columnas de `Product` tabla pueden aceptar `NULL` valores (`CategoryID`, `SupplierID`y `UnitPrice`, por nombrar algunos), los parámetros de entrada de `AddProduct` y `UpdateProduct` que se asignan a esas columnas utilizan [tipos que aceptan valores NULL](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Los tipos que aceptan valores NULL son nuevos en .NET 2,0 y proporcionan una técnica para indicar si un tipo de valor debe, en su lugar, ser `null`. En C# , puede marcar un tipo de valor como un tipo que acepta valores NULL agregando `?` después del tipo (como `int? x;`). Consulte la sección [tipos que aceptan valores NULL](https://msdn.microsoft.com/library/1t3y8s4s.aspx) en la [ C# guía de programación](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) para obtener más información.

Los tres métodos devuelven un valor booleano que indica si se insertó, actualizó o eliminó una fila, ya que es posible que la operación no dé como resultado una fila afectada. Por ejemplo, si el desarrollador de páginas llama `DeleteProduct` pasando un `ProductID` para un producto que no existe, la instrucción `DELETE` emitida a la base de datos no tendrá ningún efecto y, por tanto, el método `DeleteProduct` devolverá `false`.

Tenga en cuenta que al agregar un nuevo producto o actualizar uno existente, se toman los valores de campo del producto nuevo o modificado como una lista de escalares en lugar de aceptar una instancia de `ProductsRow`. Se eligió este método porque la clase `ProductsRow` deriva de la clase `DataRow` ADO.NET, que no tiene un constructor sin parámetros predeterminado. Para crear una nueva instancia de `ProductsRow`, primero debemos crear una instancia de `ProductsDataTable` y, después, invocar su método `NewProductRow()` (que hacemos en `AddProduct`). Este defecto reparte su cabeza cuando se selecciona la inserción y la actualización de productos mediante ObjectDataSource. En Resumen, ObjectDataSource intentará crear una instancia de los parámetros de entrada. Si el método BLL espera una instancia de `ProductsRow`, ObjectDataSource intentará crear una, pero se producirá un error debido a la falta de un constructor sin parámetros predeterminado. Para obtener más información sobre este problema, consulte los dos siguientes blogs de foros de ASP.NET: [actualización de ObjectDataSource con conjuntos de datos fuertemente tipados](https://forums.asp.net/1098630/ShowPost.aspx)y [problemas con ObjectDataSource y conjunto de datos fuertemente tipado](https://forums.asp.net/1048212/ShowPost.aspx).

Después, tanto en `AddProduct` como en `UpdateProduct`, el código crea una instancia de `ProductsRow` y la rellena con los valores que se acaban de pasar. Cuando se asignan valores a las columnas DataColumn de una DataRow, se pueden realizar varias comprobaciones de validación de nivel de campo. Por consiguiente, volver a colocar manualmente los valores pasados en una DataRow ayuda a garantizar la validez de los datos que se pasan al método BLL. Desafortunadamente, las clases DataRow fuertemente tipadas generadas por Visual Studio no usan tipos que aceptan valores NULL. En su lugar, para indicar que una DataColumn determinada en una DataRow debe corresponder a un valor de base de datos `NULL`, se debe usar el método `SetColumnNameNull()`.

En `UpdateProduct` primero cargaremos en el producto para actualizar con `GetProductByProductID(productID)`. Aunque esto puede parecer un viaje innecesario a la base de datos, este viaje adicional le merecerá la pena en futuros tutoriales que exploren la simultaneidad optimista. La simultaneidad optimista es una técnica para garantizar que dos usuarios que trabajan simultáneamente en los mismos datos no sobrescriban accidentalmente los cambios de los demás. La captación del registro completo también facilita la creación de métodos de actualización en el BLL que solo modifican un subconjunto de las columnas de DataRow. Cuando Exploremos la clase `SuppliersBLL`, veremos un ejemplo de este tipo.

Por último, tenga en cuenta que la clase `ProductsBLL` tiene aplicado el [atributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) (la sintaxis de `[System.ComponentModel.DataObject]` justo delante de la instrucción Class cerca de la parte superior del archivo) y los métodos tienen [atributos DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). El atributo `DataObject` marca la clase como un objeto adecuado para enlazar a un [control ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), mientras que la `DataObjectMethodAttribute` indica el propósito del método. Como veremos en futuros tutoriales, el ObjectDataSource de ASP.NET 2.0 facilita el acceso mediante declaración a los datos de una clase. Para ayudar a filtrar la lista de posibles clases a las que se va a enlazar en el Asistente de ObjectDataSource, de forma predeterminada solo las clases marcadas como `DataObjects` se muestran en la lista desplegable del asistente. La clase `ProductsBLL` funcionará igual que sin estos atributos, pero agregarlos facilita el trabajo en el Asistente de ObjectDataSource.

## <a name="adding-the-other-classes"></a>Agregar las otras clases

Una vez completada la clase `ProductsBLL`, todavía es necesario agregar las clases para trabajar con categorías, proveedores y empleados. Dedique un momento a crear las siguientes clases y métodos con los conceptos del ejemplo anterior:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

El método que merece la pena mencionar es el método `UpdateSupplierAddress` de la clase `SuppliersBLL`. Este método proporciona una interfaz para actualizar solo la información de dirección del proveedor. Internamente, este método lee el objeto de `SupplierDataRow` para la `supplierID` especificada (mediante `GetSupplierBySupplierID`), establece sus propiedades relacionadas con la dirección y, a continuación, llama al método `Update` del `SupplierDataTable`. A continuación se muestra el método `UpdateSupplierAddress`:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Consulte la descarga de este artículo para obtener la implementación completa de las clases de BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Paso 2: obtener acceso a los conjuntos de tipos de DataSet con tipo a través de las clases de BLL

En el primer tutorial, vimos ejemplos de cómo trabajar directamente con el conjunto de elementos con tipo mediante programación, pero con la adición de las clases de BLL, el nivel de presentación debería funcionar en su lugar con la capa BLL. En el ejemplo `AllProducts.aspx` del primer tutorial, el `ProductsTableAdapter` se usó para enlazar la lista de productos a un control GridView, tal y como se muestra en el código siguiente:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Para usar las nuevas clases de BLL, todo lo que se debe cambiar es la primera línea de código simplemente reemplace el objeto `ProductsTableAdapter` por un objeto `ProductBLL`:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

También se puede tener acceso a las clases BLL mediante declaración (como puede ser el conjunto de DataSet con tipo) mediante ObjectDataSource. En los siguientes tutoriales se explicará con más detalle ObjectDataSource.

[![la lista de productos se muestra en un control GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figura 3**: la lista de productos se muestra en un control GridView ([haga clic para ver la imagen de tamaño completo](creating-a-business-logic-layer-cs/_static/image5.png))

## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Paso 3: agregar la validación de nivel de campo a las clases DataRow

La validación de nivel de campo es una comprobación que pertenece a los valores de propiedad de los objetos comerciales al insertar o actualizar. Algunas reglas de validación de nivel de campo para productos incluyen:

- El campo `ProductName` debe tener una longitud de 40 caracteres como máximo.
- El campo `QuantityPerUnit` debe tener una longitud de 20 caracteres o menos.
- Los campos `ProductID`, `ProductName`y `Discontinued` son obligatorios, pero todos los demás campos son opcionales.
- Los campos `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`y `ReorderLevel` deben ser mayores o iguales que cero

Estas reglas se pueden expresar en el nivel de base de datos. Los tipos de datos de las columnas de la tabla `Products` (`nvarchar(40)` y `nvarchar(20)`, respectivamente) capturan el límite de caracteres de los campos `ProductName` y `QuantityPerUnit`. Si los campos son obligatorios y son opcionales, expresados por si la columna de la tabla de base de datos permite `NULL` s. Existen cuatro [restricciones check](https://msdn.microsoft.com/library/ms188258.aspx) que garantizan que solo los valores mayores o iguales que cero pueden convertirlos en las columnas `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`o `ReorderLevel`.

Además de aplicar estas reglas a la base de datos, también deben aplicarse en el nivel de conjunto de datos. De hecho, la longitud del campo y si un valor es obligatorio u opcional ya se han capturado para cada conjunto de columnas DataColumn de cada DataTable. Para ver la validación de nivel de campo existente proporcionada automáticamente, vaya al diseñador de DataSet, seleccione un campo de una de las tablas de campos y, a continuación, vaya al ventana Propiedades. Como se muestra en la figura 4, el `QuantityPerUnit` DataColumn del `ProductsDataTable` tiene una longitud máxima de 20 caracteres y permite valores `NULL`. Si se intenta establecer la propiedad `QuantityPerUnit` del `ProductsDataRow`en un valor de cadena de más de 20 caracteres, se producirá una `ArgumentException`.

[![DataColumn proporciona una validación básica en el nivel de campo](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figura 4**: la columna DataColumn proporciona una validación de nivel de campo básica ([haga clic para ver la imagen de tamaño completo](creating-a-business-logic-layer-cs/_static/image8.png))

Desafortunadamente, no podemos especificar comprobaciones de límites, como el valor de `UnitPrice` debe ser mayor o igual que cero, a través de la ventana Propiedades. Con el fin de proporcionar este tipo de validación de nivel de campo, es necesario crear un controlador de eventos para el evento [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) de DataTable. Como se mencionó en el [tutorial anterior](creating-a-data-access-layer-cs.md), los objetos DataSet, DataTable y DataRow creados por el DataSet con tipo se pueden extender mediante el uso de clases parciales. Con esta técnica se puede crear un controlador de eventos `ColumnChanging` para la clase `ProductsDataTable`. Empiece por crear una clase en la carpeta `App_Code` denominada `ProductsDataTable.ColumnChanging.cs`.

[![agregar una nueva clase a la carpeta App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figura 5**: agregar una nueva clase a la carpeta `App_Code` ([haga clic para ver la imagen de tamaño completo](creating-a-business-logic-layer-cs/_static/image11.png))

A continuación, cree un controlador de eventos para el evento `ColumnChanging` que garantice que los valores de las columnas `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`y `ReorderLevel` (si no `NULL`) son mayores o iguales que cero. Si alguna de estas columnas está fuera del intervalo, inicie una `ArgumentException`.

ProductsDataTable.ColumnChanging.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Paso 4: agregar reglas de negocios personalizadas a las clases de BLL

Además de la validación de nivel de campo, puede haber reglas de negocios personalizadas de alto nivel que impliquen diferentes entidades o conceptos que no se pueden expresar en el nivel de una sola columna, como:

- Si un producto está descontinuado, no se puede actualizar su `UnitPrice`
- El país de residencia de un empleado debe ser el mismo que el país de residencia de su jefe
- No se puede suspender un producto si es el único producto proporcionado por el proveedor

Las clases BLL deben contener comprobaciones para garantizar el cumplimiento de las reglas de negocios de la aplicación. Estas comprobaciones se pueden agregar directamente a los métodos a los que se aplican.

Imagine que nuestras reglas de negocios indican que un producto no se pudo marcar como no incluida si era el único producto de un proveedor determinado. Es decir, si el producto *X* era el único producto adquirido del proveedor *y*, no pudimos marcar *X* como no incluida; sin embargo, si el proveedor *y* proporcionara los tres productos, *a*, *B*y *C*, podríamos marcar cualquiera de ellos como descontinuado. Una regla de negocios impar, pero las reglas de negocios y el sentido común no siempre están alineadas.

Para aplicar esta regla de negocios en el método de `UpdateProducts`, empezamos comprobando si `Discontinued` se estableció en `true` y, en caso afirmativo, llamaremos `GetProductsBySupplierID` para determinar cuántos productos compramos en el proveedor de este producto. Si solo se adquiere un producto de este proveedor, se lanza un `ApplicationException`.

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Responder a los errores de validación en el nivel de presentación

Cuando se llama a la capa BLL desde el nivel de presentación, se puede decidir si se va a intentar controlar cualquier excepción que se pueda producir o permitir que se propague hasta ASP.NET (lo que provocará el evento de `Error` del `HttpApplication`). Para controlar una excepción al trabajar con la capa BLL mediante programación, podemos usar una [instrucción try... catch](https://msdn.microsoft.com/library/0yd65esw.aspx) , como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Como veremos en futuros tutoriales, el control de las excepciones que se propagan desde la capa BLL cuando se usa un control Web de datos para insertar, actualizar o eliminar datos se puede controlar directamente en un controlador de eventos en lugar de tener que ajustar el código en los bloques de `try...catch`.

## <a name="summary"></a>Resumen

Una aplicación bien diseñada se diseña en capas distintas, cada una de las cuales encapsula un rol determinado. En el primer tutorial de este artículo, se crea una capa de acceso a datos mediante conjuntos de datos con tipo. en este tutorial, creamos una capa de lógica de negocios como una serie de clases en la carpeta de `App_Code` de la aplicación que llama a la capa DAL. La capa BLL implementa la lógica de nivel de campo y de nivel empresarial para nuestra aplicación. Además de crear un BLL independiente, como hicimos en este tutorial, otra opción consiste en extender los métodos de los TableAdapters mediante el uso de clases parciales. Sin embargo, el uso de esta técnica no nos permite invalidar los métodos existentes ni separar la capa DAL y la capa BLL como el enfoque que hemos tomado en este artículo.

Una vez completada la capa DAL y BLL, estamos preparados para empezar en nuestro nivel de presentación. En el [siguiente tutorial](master-pages-and-site-navigation-cs.md) , se realizará un breve resumen de los temas de acceso a datos y se definirá un diseño de página coherente para su uso en los tutoriales.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Liz Shulok, Dennis Patterson, Carlos Santos y Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-data-access-layer-cs.md)
> [Siguiente](master-pages-and-site-navigation-cs.md)
