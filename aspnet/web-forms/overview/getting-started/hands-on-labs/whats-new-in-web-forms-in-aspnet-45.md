---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Novedades de los formularios Web Forms en ASP.NET 4,5 | Microsoft Docs
author: rick-anderson
description: La nueva versión de los formularios Web Forms de ASP.NET presenta una serie de mejoras centradas en mejorar la experiencia del usuario al trabajar con datos. En versiones anteriores de...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 301af8ed877b58624e419c04f605c41f27dbdd0c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78421927"
---
# <a name="whats-new-in-web-forms-in-aspnet-45"></a>Novedades de los formularios Web Forms en ASP.NET 4.5

por [equipo de grupos de web](https://twitter.com/webcamps)

> La nueva versión de los formularios Web Forms de ASP.NET presenta una serie de mejoras centradas en mejorar la experiencia del usuario al trabajar con datos.
> 
> En versiones anteriores de formularios Web Forms, cuando se usaba el enlace de datos para emitir el valor de un miembro de objeto, se usaban las expresiones de enlace de datos BIND () o eval (). En la nueva versión de ASP.NET, es posible declarar el tipo de datos a los que se va a enlazar un control mediante una nueva propiedad ItemType. El establecimiento de esta propiedad le permitirá usar una variable fuertemente tipada para obtener todas las ventajas de la experiencia de desarrollo de Visual Studio, como IntelliSense, la navegación por miembros y la comprobación en tiempo de compilación.
> 
> Con los controles enlazados a datos, ahora puede especificar sus propios métodos personalizados para seleccionar, actualizar, eliminar e insertar datos, simplificando la interacción entre los controles de página y la lógica de la aplicación. Además, se han agregado funciones de enlace de modelos a ASP.NET, lo que significa que puede asignar datos de la página directamente a los parámetros de tipo de método.
> 
> La validación de los datos proporcionados por el usuario también debe ser más fácil con la versión más reciente de formularios Web Forms. Ahora puede anotar las clases de modelo con atributos de validación del espacio de nombres **System. ComponentModel. DataAnnotations** y solicitar que todos los controles de sitio validen los datos proporcionados por el usuario con esa información. La validación del lado cliente en formularios Web Forms ahora está integrada con jQuery, lo que proporciona un código de cliente más limpio y características de JavaScript discretas.
> 
> En el área de validación de solicitudes, se han realizado mejoras para que sea más fácil desactivar selectivamente la validación de solicitudes para partes específicas de las aplicaciones o leer datos de solicitud invalidados.
> 
> Se han realizado algunas mejoras en los controles de servidor de formularios Web Forms para aprovechar las nuevas características de HTML5:
> 
> - La propiedad TextMode del control TextBox se ha actualizado para admitir los nuevos tipos de entrada HTML5 como email, DateTime, etc.
> - El control FileUpload admite ahora varias cargas de archivos desde exploradores compatibles con esta característica de HTML5.
> - Los controles de validador ahora admiten la validación de elementos de entrada de HTML5.
> - Los nuevos elementos HTML5 que tienen atributos que representan una dirección URL ahora admiten runat =&quot;Server&quot;. Como resultado, puede usar las convenciones de ASP.NET en rutas de dirección URL, como el operador ~ para representar la raíz de la aplicación (por ejemplo, &lt;vídeo runat =&quot;Server&quot; src =&quot;~/myVideo.wmv&quot;&gt;&lt;/video&gt;).
> - El control UpdatePanel se ha corregido para admitir la publicación de los campos de entrada de HTML5.
> 
> En el portal oficial de ASP.NET puede encontrar más ejemplos de las nuevas características de ASP.NET WebForms 4,5: [What's New in ASP.NET 4,5 and Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Usar expresiones de enlace de datos fuertemente tipadas
- Usar nuevas características de enlace de modelos en formularios Web Forms
- Usar proveedores de valores para asignar datos de página a métodos de código subyacente
- Usar anotaciones de datos para la validación de entradas de usuario
- Aprovechar la validación discreta del lado cliente con jQuery en formularios Web Forms
- Implementar la validación de solicitudes granulares
- Implementar el procesamiento asincrónico de páginas en formularios Web Forms

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los siguientes elementos para completar este laboratorio:

- [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalar fragmentos de código**

Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.

Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice C: usar fragmentos de código](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Ejercicio 1: enlace de modelos en formularios Web Forms de ASP.NET](#Exercise1)
2. [Ejercicio 2: validación de datos](#Exercise2)
3. [Ejercicio 3: procesamiento de páginas asincrónicas en formularios Web Forms ASP.NET](#Exercise3)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Ejercicio 1: enlace de modelos en formularios Web Forms de ASP.NET

La nueva versión de los formularios Web Forms de ASP.NET presenta una serie de mejoras centradas en mejorar la experiencia al trabajar con datos. En este ejercicio, obtendrá información sobre los controles de datos fuertemente tipados y el enlace de modelos.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tarea 1: uso de enlaces de datos fuertemente tipados

En esta tarea, detectará los nuevos enlaces fuertemente tipados disponibles en ASP.NET 4,5.

1. Abra la carpeta **Begin** Solution ubicada en **source/EX1-ModelBinding/Begin/** .

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la página **customers. aspx** . Coloque una lista no numerada en el control principal e incluya un control Repeater dentro de para mostrar cada cliente. Establezca el nombre del repetidor en **customersRepeater** como se muestra en el código siguiente.

    En versiones anteriores de formularios Web Forms, al usar el enlace de datos para emitir el valor de un miembro en un objeto al que se enlaza a datos, se usaría una expresión de enlace de datos, junto con una llamada al método eval, pasando el nombre del miembro como una cadena.

    En tiempo de ejecución, estas llamadas a eval usarán la reflexión en el objeto enlazado actualmente para leer el valor del miembro con el nombre especificado y mostrar el resultado en el código HTML. Este enfoque hace que sea muy fácil enlazar datos con datos arbitrarios y no formados.

    Desafortunadamente, pierde muchas de las excelentes características de experiencia en tiempo de desarrollo de Visual Studio, como IntelliSense para nombres de miembro, compatibilidad con la navegación (como ir a definición) y comprobación en tiempo de compilación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Abra el archivo **customers.aspx.CS** .
4. Agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. En la **página\_** método de carga, agregue código para rellenar el repetidor con la lista de clientes.

    (Fragmento de código: *Web Forms Lab-EX01-BIND customers Data Source*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La solución usa EntityFramework junto con CodeFirst para crear y obtener acceso a la base de datos. En el código siguiente, customersRepeater se enlaza a una consulta materializada que devuelve todos los clientes de la base de datos.
6. Presione **F5** para ejecutar la solución y vaya a la página **clientes** para ver el repetidor en acción. Como la solución usa CodeFirst, la base de datos se creará y rellenará en la instancia local de SQL Express cuando se ejecute la aplicación.

    ![Enumerar los clientes con un repetidor](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "Enumerar los clientes con un repetidor")

    *Enumerar los clientes con un repetidor*

    > [!NOTE]
    > En Visual Studio 2012, IIS Express es el servidor de desarrollo web predeterminado.
7. Cierre el explorador y vuelva a Visual Studio.
8. Ahora Reemplace la implementación para utilizar enlaces fuertemente tipados. Abra la página **customers. aspx** y use el nuevo atributo **itemType** en el repetidor para establecer el tipo de **cliente** como el tipo de enlace.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La propiedad ItemType permite declarar el tipo de datos al que se va a enlazar el control y permite usar el enlace fuertemente tipado dentro del control enlazado a datos.
9. Reemplace el contenido ItemTemplate por el código siguiente.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Uno de los inconvenientes de los enfoques anteriores es que las llamadas a eval () y BIND () se enlazan en tiempo de ejecución, lo que significa que se pasan cadenas para representar los nombres de propiedad. Esto significa que no obtendrá IntelliSense para los nombres de miembro, la compatibilidad con la navegación de código (como ir a definición) ni la compatibilidad con la comprobación en tiempo de compilación.

    Al establecer la propiedad ItemType, se generan dos nuevas variables con tipo en el ámbito de las expresiones de enlace de datos: **Item** y **BindItem**. Estas variables fuertemente tipadas se pueden usar en las expresiones de enlace de datos y se obtienen todas las ventajas de la experiencia de desarrollo de Visual Studio.

    El &quot; **:** &quot; usados en la expresión codificará automáticamente la salida para evitar problemas de seguridad (por ejemplo, ataques de scripting entre sitios). Esta notación estaba disponible desde .NET 4 para la escritura de respuesta, pero ahora también está disponible en expresiones de enlace de datos.

    > [!NOTE]
    > El miembro del elemento funciona para el enlace unidireccional. Si desea realizar un enlace bidireccional, use el miembro **BindItem** .

    ![Compatibilidad de IntelliSense en un enlace fuertemente tipado](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "Compatibilidad de IntelliSense en un enlace fuertemente tipado")

    *Compatibilidad de IntelliSense en un enlace fuertemente tipado*
10. Presione **F5** para ejecutar la solución y vaya a la página clientes para asegurarse de que los cambios funcionan según lo previsto.

    ![Enumeración de los detalles del cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "Enumeración de los detalles del cliente")

    *Enumeración de los detalles del cliente*
11. Cierre el explorador y vuelva a Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tarea 2: Introducción al enlace de modelos en formularios Web Forms

En versiones anteriores de formularios Web Forms de ASP.NET, cuando deseaba realizar un enlace de datos bidireccional, recuperando y actualizando datos, necesitaba usar un objeto de origen de datos. Podría ser un origen de datos de objeto, un origen de datos SQL, un origen de datos LINQ, etc. Sin embargo, si su escenario requiere código personalizado para controlar los datos, debe usar el origen de datos del objeto y esto supuso algunas desventajas. Por ejemplo, si necesita evitar tipos complejos y necesita controlar excepciones al ejecutar la lógica de validación.

En la nueva versión de ASP.NET Web Forms, los controles enlazados a datos admiten el enlace de modelos. Esto significa que puede especificar métodos Select, Update, INSERT y DELETE directamente en el control enlazado a datos para llamar a la lógica desde el archivo de código subyacente o desde otra clase.

Para obtener información sobre esto, usará un control GridView para mostrar las categorías de producto mediante el nuevo atributo **SelectMethod** . Este atributo le permite especificar un método para recuperar los datos de GridView.

1. Abra la página **Products. aspx** e incluya **GridView**. Configure GridView como se muestra a continuación para usar enlaces fuertemente tipados y habilitar la ordenación y la paginación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Use el nuevo atributo **SelectMethod** para configurar GridView de modo que llame a un método **GetCategories** y seleccione los datos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Abra el archivo de código subyacente **Products.aspx.CS** y agregue las siguientes instrucciones using.

    (Fragmento de código- *Web Forms Lab-EX01-namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Agregue un miembro privado en la clase **Products** y asigne una nueva instancia de **ProductsContext**. Esta propiedad almacenará el Entity Framework contexto de datos que le permite conectarse a la base de datos.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Cree un método **GetCategories** para recuperar la lista de categorías mediante Linq. La consulta incluirá la propiedad **Products** para que GridView pueda mostrar la cantidad de productos de cada categoría. Observe que el método devuelve un objeto IQueryable sin formato que representa la consulta que se va a ejecutar más adelante en el ciclo de vida de la página.

    (Fragmento de código: *Web Forms Lab-EX01-GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > En versiones anteriores de formularios Web Forms de ASP.NET, la habilitación de la ordenación y la paginación usando su propia lógica del repositorio dentro de un contexto de origen de datos de objeto, necesaria para escribir su propio código personalizado y recibir todos los parámetros necesarios. Ahora, dado que los métodos de enlace de datos pueden devolver IQueryable y esto representa una consulta que todavía se ejecuta, ASP.NET puede encargarse de modificar la consulta para agregar los parámetros de ordenación y paginación adecuados.
6. Presione **F5** para iniciar la depuración del sitio y vaya a la página productos. Debería ver que GridView se rellena con las categorías devueltas por el método GetCategories.

    ![Rellenar un control GridView mediante el enlace de modelos](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "Rellenar un control GridView mediante el enlace de modelos")

    *Rellenar un control GridView mediante el enlace de modelos*
7. Presione **mayús**+**F5** detener depuración.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tarea 3: proveedores de valores en el enlace de modelos

El enlace de modelos no solo permite especificar métodos personalizados para trabajar directamente con los datos en el control enlazado a datos, sino que también permite asignar datos de la página a parámetros de estos métodos. En el parámetro de método, puede utilizar atributos de proveedor de valor para especificar el origen de datos del valor. Por ejemplo:

- Controles en la página
- Valores de cadena de consulta
- Visualización de datos
- Estado de sesión
- Cookies
- Datos de formulario publicados
- Estado de vista
- También se admiten proveedores de valores personalizados.

Si ha usado ASP.NET MVC 4, observará que la compatibilidad con el enlace de modelos es similar. En realidad, estas características se han tomado de ASP.NET MVC y se han pasado al ensamblado **System. Web** para poder usarlas también en formularios Web Forms.

En esta tarea, actualizará GridView para filtrar los resultados por la cantidad de productos de cada categoría, recibiendo el parámetro de filtro con el enlace de modelos.

1. Vuelva a la página **Products. aspx** .
2. En la parte superior de GridView, agregue una **etiqueta** y un **cuadro combinado** para seleccionar el número de productos de cada categoría, tal como se muestra a continuación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Agregue un **EmptyDataTemplate** a GridView para mostrar un mensaje cuando no haya ninguna categoría con el número de productos seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Abra el código subyacente de **Products.aspx.CS** y agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modifique el método **GetCategories** para recibir un argumento **minProductsCount** entero y filtre los resultados devueltos. Para ello, reemplace el método por el código siguiente.

    (Fragmento de código- *Web Forms Lab-EX01-GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    El nuevo atributo **[control]** del argumento **minProductsCount** permitirá a ASP.net saber que su valor se debe rellenar con un control de la página. ASP.NET buscará cualquier control que coincida con el nombre del argumento (minProductsCount) y realizará la asignación y conversión necesarias para rellenar el parámetro con el valor del control.

    Como alternativa, el atributo proporciona un constructor sobrecargado que le permite especificar el control desde dónde obtener el valor.

    > [!NOTE]
    > Un objetivo de las características de enlace de datos es reducir la cantidad de código que debe escribirse para la interacción de la página. Además del proveedor de valores [control], puede usar otros proveedores de enlace de modelos en los parámetros de método. Algunos de ellos se muestran en la introducción de la tarea.
6. Presione **F5** para iniciar la depuración del sitio y vaya a la página productos. Seleccione un número de productos en la lista desplegable y observe cómo se actualiza GridView ahora.

    ![Filtrar GridView con un valor de lista desplegable](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "Filtrar GridView con un valor de lista desplegable")

    *Filtrar GridView con un valor de lista desplegable*
7. Detenga la depuración.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tarea 4: uso del enlace de modelos para filtrar

En esta tarea, agregará un segundo GridView secundario para mostrar los productos de la categoría seleccionada.

1. Abra la página **Products. aspx** y actualice la categoría GridView para generar automáticamente el botón seleccionar.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Agregue un segundo **GridView** denominado **productsGrid** en la parte inferior. Establezca **itemType** en **WebFormsLab. Model. Product**, **DataKeyNames** en **ProductID** y **SelectMethod** en **GetProducts**. Establezca **AutoGenerateColumns** en **false** y agregue las columnas de ProductID, ProductName, Description y UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Abra el archivo de código subyacente de **Products.aspx.CS** . Implemente el método **GetProducts** para recibir el identificador de categoría de la categoría GridView y filtre los productos. El enlace de modelos establecerá el valor del parámetro mediante la fila seleccionada en el **categoriesGrid**. Dado que el nombre del argumento y el nombre del control no coinciden, debe especificar el nombre del control en el proveedor de valores del control.

    (Fragmento de código: *Web Forms Lab-EX01-GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Este enfoque facilita la prueba unitaria de estos métodos. En un contexto de prueba unitaria, en el que los formularios Web Forms no se están ejecutando, el atributo [control] no realizará ninguna acción específica.
4. Abra la página **Products. aspx** y busque los productos GridView. Actualice el GridView de los productos para mostrar un vínculo para editar el producto seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Abra el código subyacente de la página **productdetails. aspx** y reemplace el método **SelectProduct** por el código siguiente.

    (Fragmento de código: *método de Web Forms Lab-EX01-SelectProduct*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Observe que el atributo **[QueryString]** se usa para rellenar el parámetro de método de un parámetro productId en la cadena de consulta.
6. Presione **F5** para iniciar la depuración del sitio y vaya a la página productos. Seleccione una categoría de las categorías GridView y observe que se ha actualizado la GridView de los productos.

    ![Mostrar los productos de la categoría seleccionada](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "Mostrar los productos de la categoría seleccionada")

    *Mostrar los productos de la categoría seleccionada*
7. Haga clic en el vínculo **Ver** de un producto para abrir la página productdetails. aspx.

    Tenga en cuenta que la página recupera el producto con SelectMethod mediante el parámetro productId de la cadena de consulta.

    ![Ver los detalles del producto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "Ver los detalles del producto")

    *Ver los detalles del producto*

    > [!NOTE]
    > La capacidad de escribir una descripción de HTML se implementará en el ejercicio siguiente.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tarea 5: usar el enlace de modelos para las operaciones de actualización

En la tarea anterior, ha utilizado el enlace de modelos principalmente para seleccionar datos. en esta tarea aprenderá a usar el enlace de modelos en las operaciones de actualización.

Actualizará las categorías GridView para permitir que el usuario actualice las categorías.

1. Abra la página **Products. aspx** y actualice la categoría GridView para generar automáticamente el botón Editar y use el atributo **UpdateMethod** nuevo para especificar un método **UpdateCategory** para actualizar el elemento seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    El atributo DataKeyNames de GridView define que son los miembros que identifican de forma única el objeto enlazado a un modelo y, por lo tanto, son los parámetros que el método Update debe recibir al menos.
2. Abra el archivo de código subyacente **Products.aspx.CS** e implemente el método **UpdateCategory** . El método debe recibir el identificador de categoría para cargar la categoría actual, rellenar los valores de GridView y, a continuación, actualizar la categoría.

    (Fragmento de código: *Web Forms Lab-EX01-UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    El nuevo método **TryUpdateModel** de la clase Page es responsable de rellenar el objeto de modelo usando los valores de los controles de la página. En este caso, reemplazará los valores actualizados de la fila de GridView actual que se está editando en el objeto de **categoría** .

    > [!NOTE]
    > En el siguiente ejercicio se explica el uso de ModelState. IsValid para validar los datos introducidos por el usuario al editar el objeto.
3. Ejecute el sitio y vaya a la página productos. Edite una categoría. Escriba un nuevo nombre y, a continuación, haga clic en **Actualizar** para conservar los cambios.

    ![Editar categorías](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "Editar categorías")

    *Editar categorías*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Ejercicio 2: validación de datos

En este ejercicio, obtendrá información sobre las nuevas características de validación de datos de ASP.NET 4,5. Comprobará las nuevas características de validación discretas en formularios Web Forms. Usará anotaciones de datos en las clases del modelo de aplicación para la validación de la entrada del usuario y, por último, aprenderá a activar o desactivar la validación de solicitudes en controles individuales de una página.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tarea 1: validación discreta

Los formularios con datos complejos, incluidos los validadores, tienden a generar demasiado código JavaScript en la página, que puede representar aproximadamente el 60% del código. Con la validación discreta habilitada, el código HTML tendrá un aspecto más limpio y tidier.

En esta sección, habilitará la validación discreta en ASP.NET para comparar el código HTML generado por ambas configuraciones.

1. Abra **Visual Studio 2012** y abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex2-Validation\Begin** de este laboratorio. Como alternativa, puede seguir trabajando en la solución existente del ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, en el Explorador de soluciones, haga clic en el proyecto **WebFormsLab** **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Presione **F5** para iniciar la aplicación Web. Vaya a la página clientes y haga clic en el vínculo **Agregar un nuevo cliente** .
3. Haga clic con el botón derecho en la página del explorador y seleccione la opción **Ver código fuente** para abrir el código HTML generado por la aplicación.

    ![Mostrar el código HTML de la página](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "Mostrar el código HTML de la página")

    *Mostrar el código HTML de la página*
4. Desplácese por el código fuente de la página y observe que ASP.NET ha insertado código JavaScript y validadores de datos en la página para realizar las validaciones y mostrar la lista de errores.

    ![Código JavaScript de validación en la página CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "Código JavaScript de validación en la página CustomerDetails")

    *Código JavaScript de validación en la página CustomerDetails*
5. Cierre el explorador y vuelva a Visual Studio.
6. Ahora se habilitará la validación discreta. Abra **Web. config** y busque la clave **ValidationSettings: UnobtrusiveValidationMode** en la sección **appSettings** **.** Establezca el valor de clave en **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > También puede establecer esta propiedad en la página &quot; **\_evento Load**&quot; en caso de que desee habilitar la validación discreta solo para algunas páginas.
7. Abra **CustomerDetails. aspx** y presione **F5** para iniciar la aplicación Web.
8. Presione la tecla F12 para abrir las herramientas de desarrollo de IE. Una vez abiertas las herramientas de desarrollo, seleccione la pestaña script. Seleccione **CustomerDetails. aspx** en el menú y tenga en cuenta que los scripts necesarios para ejecutar jQuery en la página se han cargado en el explorador desde el sitio local.

    ![Carga de archivos de jQuery JavaScript directamente desde el servidor IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "Carga de archivos de jQuery JavaScript directamente desde el servidor IIS local")

    *Carga de archivos de jQuery JavaScript directamente desde el servidor IIS local*
9. Cierre el explorador para volver a Visual Studio. Vuelva a abrir el archivo **site. Master** y busque el **ScriptManager**. Agregue la propiedad Attribute **EnableCdn** con el valor **true**. Esto obligará a que jQuery se cargue desde la dirección URL en línea, no desde la dirección URL del sitio local.
10. Abra **CustomerDetails. aspx** en Visual Studio. Presione la tecla F5 para ejecutar el sitio. Una vez que se abra Internet Explorer, presione la tecla F12 para abrir las herramientas de desarrollo. Seleccione la pestaña **script** y, a continuación, eche un vistazo a la lista desplegable. Tenga en cuenta que los archivos de JavaScript de jQuery ya no se cargan desde el sitio local, sino desde la red CDN de jQuery en línea.

    ![Carga de archivos de jQuery JavaScript desde la red CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "Carga de archivos de jQuery JavaScript desde la red CDN")

    *Carga de archivos de jQuery JavaScript desde la red CDN*
11. Vuelva a abrir el código fuente de la página HTML mediante la opción Ver origen en el explorador. Tenga en cuenta que, al habilitar la validación discreta, ASP.NET reemplaza el código JavaScript insertado con los atributos de \*de datos.

    ![Código de validación discreta](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "Código de validación discreta")

    *Código de validación discreta*

    > [!NOTE]
    > En este ejemplo, vio cómo un resumen de validación con anotaciones de datos se simplificó solo en algunas líneas HTML y JavaScript. Anteriormente, sin una validación discreta, más controles de validación que agregue, mayor será el crecimiento del código de validación de JavaScript.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tarea 2: validar el modelo con anotaciones de datos

ASP.NET 4,5 presenta la validación de anotaciones de datos para formularios Web Forms. En lugar de tener un control de validación en cada entrada, ahora puede definir restricciones en las clases de modelo y usarlas en toda la aplicación Web. En esta sección, aprenderá a usar anotaciones de datos para validar un formulario de cliente nuevo/editar.

1. Abra la página **CustomerDetail. aspx** . Tenga en cuenta que el nombre del cliente y el segundo nombre de las secciones **EditItemTemplate** e **InsertItemTemplate** se validan con los controles RequiredFieldValidator. Cada validador está asociado a una condición determinada, por lo que debe incluir tantos validadores como condiciones comprobar.
2. Agregue anotaciones de datos para validar la clase del modelo de cliente. Abra la clase **Customer.CS** en la carpeta **Model** y represente cada propiedad mediante *atributos de anotación* de datos.

    (Fragmento de código: *anotaciones de datos Lab-Ex02-Data*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET Framework 4,5 ha extendido la colección de anotaciones de datos existente. Estas son algunas de las anotaciones de datos que puede usar: [CreditCard], [Phone], [EmailAddress], [Range], [Compare], [URL], [FileExtensions], [required], [key], [RegularExpression].
    > 
    > Algunos ejemplos de uso:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > También puede definir sus propios mensajes de error dentro de cada atributo.
3. Abra **CustomerDetails. aspx** y quite todos los RequiredFieldValidators de los campos nombre y apellidos de en las secciones EditItemTemplate e InsertItemTemplate del control FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Una ventaja de usar anotaciones de datos es que la lógica de validación no está duplicada en las páginas de la aplicación. Se define una vez en el modelo y se usa en todas las páginas de la aplicación que manipulan los datos.
4. Abra el código subyacente de **CustomerDetails. aspx** y busque el método SaveCustomer. Se llama a este método al insertar un nuevo cliente y recibe el parámetro Customer de los valores del control FormView. Cuando se produce la asignación entre los controles de página y el objeto de parámetro, ASP.NET ejecutará la validación del modelo en todos los atributos de anotación de datos y rellenará el Diccionario ModelState con los errores encontrados, si los hay.

    ModelState. IsValid solo devolverá True si todos los campos del modelo son válidos después de realizar la validación.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Agregue un control **ValidationSummary** al final de la página CustomerDetails para mostrar la lista de errores del modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    **ShowModelStateErrors** es una nueva propiedad en el control ValidationSummary que cuando se establece en **true**, el control mostrará los errores del diccionario ModelState. Estos errores proceden de la validación de anotaciones de datos.
6. Presione **F5** para ejecutar la aplicación Web. Rellene el formulario con algunos valores erróneos y haga clic en **Guardar** para ejecutar la validación. Observe el Resumen de errores en la parte inferior.

    ![Validación con anotaciones de datos](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "Validación con anotaciones de datos")

    *Validación con anotaciones de datos*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tarea 3: control de errores de base de datos personalizada con ModelState

En la versión anterior de los formularios Web Forms, el control de errores de base de datos como una cadena demasiado larga o una infracción de clave única podría implicar la generación de excepciones en el código del repositorio y la posterior administración de las excepciones en el código subyacente para mostrar un error. Se requiere una gran cantidad de código para hacer algo relativamente sencillo.

En los formularios Web Forms 4,5, el objeto ModelState se puede usar para mostrar los errores en la página, ya sea desde el modelo o desde la base de datos, de una manera coherente.

En esta tarea, agregará código para controlar correctamente las excepciones de la base de datos y mostrar el mensaje adecuado al usuario mediante el objeto ModelState.

1. Mientras la aplicación todavía se está ejecutando, intente actualizar el nombre de una categoría con un valor duplicado.

    ![Actualizar una categoría con un nombre duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "Actualizar una categoría con un nombre duplicado")

    *Actualizar una categoría con un nombre duplicado*

    Observe que se produce una excepción debido a la &quot;restricción UNIQUE&quot; de la columna **CategoryName** .

    ![Excepción para nombres de categoría duplicados](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "Excepción para nombres de categoría duplicados")

    *Excepción para nombres de categoría duplicados*
2. Detenga la depuración. En el archivo de código subyacente **Products.aspx.CS** , actualice el método **UpdateCategory** para controlar las excepciones producidas por la base de BD. Llamada al método SaveChanges () y agrega un error al objeto **ModelState** .

    El nuevo método **TryUpdateModel** actualiza el objeto de categoría recuperado de la base de datos con los datos de formulario proporcionados por el usuario.

    (Fragmento de código- *Web Forms Lab-Ex02-UpdateCategory controlar errores*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, tendría que identificar la causa de la excepción dbupdateexception y comprobar si la causa raíz es la infracción de una restricción de clave única.
3. Abra **Products. aspx** y agregue un control **ValidationSummary** debajo de la categoría GridView para mostrar la lista de errores del modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Ejecute el sitio y vaya a la página productos. Intente actualizar el nombre de una categoría con un valor duplicado.

    Observe que se controló la excepción y que el mensaje de error aparece en el control **ValidationSummary** .

    ![Error de categoría duplicada](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "Error de categoría duplicada")

    *Error de categoría duplicada*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tarea 4: validación de solicitudes en ASP.NET Web Forms 4,5

La característica de validación de solicitudes de ASP.NET proporciona un cierto nivel de protección predeterminada contra ataques de scripting entre sitios (XSS). En versiones anteriores de ASP.NET, la validación de solicitudes estaba habilitada de forma predeterminada y solo se podía deshabilitar para toda la página. Con la nueva versión de formularios Web Forms de ASP.NET, ahora puede deshabilitar la validación de solicitudes para un solo control, realizar la validación de solicitudes diferidas o acceder a los datos de solicitudes no validadas (tenga cuidado si lo hace).

1. Presione **Ctrl + F5** para iniciar el sitio sin depurar y vaya a la página productos. Seleccione una categoría y, a continuación, haga clic en el vínculo **Editar** en cualquiera de los productos.
2. Escriba una descripción que contenga contenido potencialmente peligroso, por ejemplo, incluyendo etiquetas HTML. Observe la excepción que se produce debido a la validación de la solicitud.

    ![Edición de un producto con contenido potencialmente peligroso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "Edición de un producto con contenido potencialmente peligroso")

    *Edición de un producto con contenido potencialmente peligroso*

    ![Excepción producida debido a la validación de la solicitud](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "Excepción producida debido a la validación de la solicitud")

    *Excepción producida debido a la validación de la solicitud*
3. Cierre la página y, en Visual Studio, presione **Mayús + F5** para detener la depuración.
4. Abra la página **productdetails. aspx** y busque el cuadro de texto **Descripción** .
5. Agregue la nueva propiedad **ValidateRequestMode** al cuadro de texto y establezca su valor en **Disabled**.

    El nuevo atributo **ValidateRequestMode** permite deshabilitar la validación de solicitudes de forma granular en cada control. Esto resulta útil cuando se desea usar una entrada que puede recibir código HTML, pero se desea mantener la validación en funcionamiento para el resto de la página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Presione **F5** para ejecutar la aplicación Web. Vuelva a abrir la página Editar producto y complete una descripción del producto, incluidas las etiquetas HTML. Tenga en cuenta que ahora puede agregar contenido HTML a la descripción.

    ![Validación de solicitud deshabilitada para la descripción del producto](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "Validación de solicitud deshabilitada para la descripción del producto")

    *Validación de solicitud deshabilitada para la descripción del producto*

    > [!NOTE]
    > En una aplicación de producción, debe corregir el código HTML escrito por el usuario para asegurarse de que solo se escriban etiquetas HTML seguras (por ejemplo, no hay ninguna &lt;script&gt; etiquetas). Para ello, puede usar la [biblioteca de protección Web de Microsoft](https://www.nuget.org/packages/AntiXSS).
7. Vuelva a editar el producto. Escriba código HTML en el campo Nombre y haga clic en **Guardar**. Tenga en cuenta que la validación de solicitudes solo está deshabilitada para el campo Descripción y el resto de los campos todavía se han validado con respecto al contenido potencialmente peligroso.

    ![Validación de solicitudes habilitada en el resto de los campos](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "Validación de solicitudes habilitada en el resto de los campos")

    *Validación de solicitudes habilitada en el resto de los campos*

    Los formularios Web Forms de ASP.NET 4,5 incluyen un nuevo modo de validación de solicitudes para realizar la validación de la solicitud de forma diferida. Con el modo de validación de solicitud establecido en **4,5**, si un fragmento de código tiene acceso a *request. Form [&quot;Key&quot;]* , la validación de la solicitud de ASP.net 4.5 solo desencadenará la validación de solicitudes para ese elemento específico en la colección de formularios.

    Además, ASP.NET 4,5 ahora incluye rutinas de codificación básicas de la biblioteca AntiXSS de Microsoft v 4.0. El nuevo tipo *AntiXssEncoder* que se encuentra en el nuevo espacio de nombres **System. Web. Security. AntiXss** implementa las rutinas de codificación AntiXSS. Con el parámetro **encoderType** configurado para usar *AntiXssEncoder*, toda la codificación de salida dentro de ASP.net utiliza automáticamente las nuevas rutinas de codificación.
8. La validación de solicitudes de ASP.NET 4,5 también admite el acceso no validado a los datos de solicitud. ASP.NET 4,5 agrega una nueva propiedad de colección al objeto **HttpRequest** denominado no **validado**. Cuando navega a **HttpRequest. unvalidad** , tiene acceso a todos los elementos comunes de datos de solicitud, como formularios, las querystrings, cookies, direcciones URL, etc.

    ![Objeto request. unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Objeto request. unvalidated")

    *Objeto request. unvalidated*

    > [!NOTE]
    > **Use la propiedad HttpRequest. unvalidated con precaución.** Asegúrese de realizar cuidadosamente la validación personalizada en los datos de la solicitud sin procesar para asegurarse de que el texto peligroso no sea de ida y vuelta y represente a los clientes que no sean de la sospecha.

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Ejercicio 3: procesamiento de páginas asincrónicas en formularios Web Forms ASP.NET

En este ejercicio, se presentarán las nuevas características de procesamiento de páginas asincrónicas en formularios Web Forms de ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Tarea 1: actualización de la página de detalles del producto para cargar y mostrar imágenes

En esta tarea, actualizará la página de detalles del producto para permitir al usuario especificar una dirección URL de imagen para el producto y mostrarla en la vista de solo lectura. Va a crear una copia local de la imagen especificada descargándolos sincrónicamente. En la tarea siguiente, actualizará esta implementación para que funcione de forma asincrónica.

1. Abra **Visual Studio 2012** y cargue la solución de **Inicio** ubicada en **Source\Ex3-Async\Begin** desde la carpeta de este laboratorio. Como alternativa, puede seguir trabajando en la solución existente desde los ejercicios anteriores.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, en el Explorador de soluciones, haga clic en el proyecto **WebFormsLab** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra el origen de la página **productdetails. aspx** y agregue un campo en el ItemTemplate de FormView para mostrar la imagen del producto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Agregue un campo para especificar la dirección URL de la imagen en el EditTemplate de FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Abra el archivo de código subyacente **productdetails.aspx.CS** y agregue las siguientes directivas de espacio de nombres.

    (Fragmento de código- *Web Forms Lab-Ex03-namespaces*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Cree un método **UpdateProductImage** para almacenar imágenes remotas en la carpeta de **imágenes** locales y actualice la entidad Product con el nuevo valor de ubicación de la imagen.

    (Fragmento de código: *Web Forms Lab-Ex03-UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Actualice el método **UpdateProduct** para llamar al método **UpdateProductImage** .

    (Fragmento de código- *Web Forms Lab-Ex03-UpdateProductImage Call*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Ejecute la aplicación e intente cargar una imagen para un producto. Por ejemplo, puede usar la siguiente dirección URL de imagen de Office Clip Arts: [ [http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Configuración de una imagen para un producto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "Configuración de una imagen para un producto")

    *Configuración de una imagen para un producto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tarea 2: agregar el procesamiento asincrónico a la página de detalles del producto

En esta tarea, actualizará la página de detalles del producto para que funcione de forma asincrónica. Mejorará una tarea de ejecución prolongada: el proceso de descarga de la imagen mediante el procesamiento de páginas asincrónicas de ASP.NET 4,5.

Los métodos asincrónicos de las aplicaciones web se pueden usar para optimizar la manera en que se usan los grupos de subprocesos de ASP.NET. En ASP.NET hay un número limitado de subprocesos en el grupo de subprocesos para las solicitudes de asistencia, por lo tanto, cuando todos los subprocesos están ocupados, ASP.NET comienza a rechazar nuevas solicitudes, envía mensajes de error de la aplicación y hace que el sitio no esté disponible.

Las operaciones que consumen mucho tiempo en el sitio web son buenos candidatos para la programación asincrónica porque ocupan el subproceso asignado durante mucho tiempo. Esto incluye solicitudes de larga ejecución, páginas con muchos elementos y páginas diferentes que requieren operaciones sin conexión, como consultar una base de datos o tener acceso a un servidor Web externo. La ventaja es que si utiliza métodos asincrónicos para estas operaciones, mientras se procesa la página, el subproceso se libera y se devuelve al grupo de subprocesos y se puede usar para asistir a una nueva solicitud de página. Esto significa que la página comenzará el procesamiento en un subproceso del grupo de subprocesos y puede completar el procesamiento en otro diferente, una vez que se complete el procesamiento asincrónico.

1. Abra la página **productdetails. aspx** . Agregue el atributo **Async** en el elemento **Page** y establézcalo en **true**. Este atributo indica a ASP.NET que implemente la interfaz IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Agregue una etiqueta en la parte inferior de la página para mostrar los detalles de los subprocesos que ejecutan la página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Abra **productdetails.aspx.CS** y agregue las siguientes directivas de espacio de nombres.

    (Fragmento de código: *formularios Web Forms Lab-Ex03-namespaces 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modifique el método **UpdateProductImage** para descargar la imagen con una tarea asincrónica. Reemplazará el método de **WebClient** **DownloadFile** con el método **DownloadFileTaskAsync** e incluirá la palabra clave **Await** .

    (Fragmento de código- *Web Forms Lab-Ex03-UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    RegisterAsyncTask registra una nueva tarea asincrónica de la página que se va a ejecutar en un subproceso diferente. Recibe una expresión lambda con la tarea (t) que se va a ejecutar. La palabra clave **Await** en el método **DownloadFileTaskAsync** convierte el resto del método en una devolución de llamada que se invoca de forma asincrónica después de que se haya completado el método **DownloadFileTaskAsync** . ASP.NET reanudará la ejecución del método manteniendo automáticamente todos los valores originales de la solicitud HTTP. El nuevo modelo de programación asincrónica en .NET 4,5 le permite escribir código asincrónico que se parece mucho a un código sincrónico y dejar que el compilador controle las complicaciones de las funciones de devolución de llamada o el código de continuación.
    > [!NOTE]
    > RegisterAsyncTask y PageAsyncTask ya estaban disponibles desde .NET 2,0. La palabra clave Await es nueva en el modelo de programación asincrónica de .NET 4,5 y se puede usar junto con los nuevos métodos TaskAsync del objeto WebClient de .NET.
5. Agregue código para mostrar los subprocesos en los que se inició y finalizó la ejecución del código. Para ello, actualice el método **UpdateProductImage** con el código siguiente.

    (Fragmento de código- *Web Forms Lab-Ex03-show threads*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Abra el archivo **Web. config** del sitio Web. Agregue la siguiente variable appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Presione **F5** para ejecutar la aplicación y cargar una imagen para el producto. Observe que el ID. de subprocesos donde el código se ha iniciado y finalizado puede ser diferente. Esto se debe a que las tareas asincrónicas se ejecutan en un subproceso independiente del grupo de subprocesos ASP.NET. Cuando la tarea se completa, ASP.NET vuelve a colocar la tarea en la cola y asigna cualquiera de los subprocesos disponibles.

    ![Descargar una imagen de forma asincrónica](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "Descargar una imagen de forma asincrónica")

    *Descargar una imagen de forma asincrónica*

> [!NOTE]
> Además, puede implementar esta aplicación en Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, se han abordado y demostrado los siguientes conceptos:

- Usar expresiones de enlace de datos fuertemente tipadas
- Usar nuevas características de enlace de modelos en formularios Web Forms
- Usar proveedores de valores para asignar datos de página a métodos de código subyacente
- Usar anotaciones de datos para la validación de entradas de usuario
- Aprovechar la validación discreta del lado cliente con jQuery en formularios Web Forms
- Implementar la validación de solicitudes granulares
- Implementar el procesamiento asincrónico de páginas en formularios Web Forms

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express para Web icono*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde Azure portal y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarea 1: creación de un nuevo sitio web desde Azure portal

1. Vaya a [Azure portal de administración](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.

    > [!NOTE]
    > Con Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico. Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).

    ![Iniciar sesión en Windows Azure Portal](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "Iniciar sesión en Windows Azure Portal")

    *Inicio de sesión en el Portal*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en Azure.

    ![Descargar el perfil de publicación del sitio web](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](whats-new-in-web-forms-in-aspnet-45/_static/image39.png).

    ![Agregando la dirección IP del cliente](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configuración de la cadena de conexión de destino](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
