---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Mostrar elementos de datos y detalles | Microsoft Docs
author: Erikre
description: En esta serie de tutoriales se muestran los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,7 y Microsoft Visual Studio 2017
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 130c9ffd29df612dac5bb954830a2eb9b738aaf0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520813"
---
# <a name="display-data-items-and-details"></a>Mostrar elementos de datos y detalles

por [Erik Reitan](https://github.com/Erikre)

> Esta serie de tutoriales le enseña los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,7 y Microsoft Visual Studio 2017.

En este tutorial, aprenderá a mostrar los elementos de datos y los detalles de los elementos de datos con los formularios Web Forms de ASP.NET y Entity Framework Code First. Este tutorial se basa en el tutorial anterior "interfaz de usuario y navegación" como parte de la serie de tutoriales de Wingtip Toy Store. Después de completar este tutorial, verá productos en la página *ProductsList. aspx* y los detalles de un producto en la página *productdetails. aspx* .

## <a name="youll-learn-how-to"></a>Aprenderá a:

- Agregar un control de datos para mostrar los productos de la base de datos
- Conectar un control de datos a los datos seleccionados
- Agregar un control de datos para mostrar los detalles del producto de la base de datos
- Recuperar un valor de la cadena de consulta y usar ese valor para limitar los datos que se recuperan de la base de datos

### <a name="features-introduced-in-this-tutorial"></a>Características introducidas en este tutorial:

- Enlace de modelos
- Proveedores de valores

## <a name="add-a-data-control"></a>Agregar un control de datos

Puede usar algunas opciones diferentes para enlazar datos a un control de servidor. Entre las más comunes se incluyen:

* Agregar un control de origen de datos
* Agregar código manualmente
* Usar el enlace de modelos

### <a name="use-a-data-source-control-to-bind-data"></a>Usar un control de origen de datos para enlazar datos

Agregar un control de origen de datos le permite vincular el control de origen de datos al control que muestra los datos. Con este enfoque, puede, en lugar de hacerlo mediante programación, conectar controles del lado servidor a los orígenes de datos.

### <a name="code-by-hand-to-bind-data"></a>Código a mano para enlazar datos

La codificación a mano implica:

1. Leer un valor
2. Comprobando si es null
3. Convertirlo en un tipo adecuado
4. Comprobando la conversión correcta
5. Usar el valor de la consulta 

Este enfoque le permite tener control total sobre la lógica de acceso a datos.

### <a name="use-model-binding-to-bind-data"></a>Usar el enlace de modelos para enlazar datos

El enlace de modelos permite enlazar los resultados con mucho menos código y ofrece la posibilidad de reutilizar la funcionalidad en toda la aplicación. Simplifica el trabajo con la lógica de acceso a datos centrada en el código y, al mismo tiempo, proporciona un marco de enlace de datos enriquecido.

## <a name="display-products"></a>Mostrar productos

En este tutorial, usará el enlace de modelos para enlazar datos. Para configurar un control de datos para que use el enlace de modelos para seleccionar datos, establezca la propiedad `SelectMethod` del control en un nombre de método en el código de la página. El control de datos llama al método en el momento adecuado del ciclo de vida de la página y enlaza automáticamente los datos devueltos. No es necesario llamar explícitamente al método `DataBind`.

1. En **Explorador de soluciones**, Abra *productlist. aspx*.
2. Reemplace el marcado existente por este marcado:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Este código usa un control **ListView** denominado `productList` para mostrar los productos.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Con las plantillas y los estilos, se define el modo en que el control **ListView** muestra los datos. Es útil para los datos de cualquier estructura de repetición. Aunque en este ejemplo de **ListView** solo se muestran los datos de la base de datos, también puede, sin código, permitir a los usuarios editar, insertar y eliminar datos, así como ordenar y paginar los datos.

Al establecer la propiedad `ItemType` en el control **ListView** , la expresión de enlace de datos `Item` está disponible y el control se vuelve fuertemente tipado. Como se mencionó en el tutorial anterior, puede seleccionar detalles del objeto de elemento con IntelliSense, como especificar el `ProductName`:

![Mostrar elementos de datos y detalles: IntelliSense](display_data_items_and_details/_static/image1.png)

También está utilizando el enlace de modelos para especificar un valor `SelectMethod`. Este valor (`GetProducts`) corresponde al método que agregará al código subyacente para mostrar los productos en el paso siguiente.

### <a name="add-code-to-display-products"></a>Agregar código para mostrar productos

En este paso, agregará código para rellenar el control **ListView** con datos de productos de la base de datos. El código admite la visualización de todos los productos y productos de categoría individuales.

1. En **Explorador de soluciones**, haga clic con el botón secundario en *productlist. aspx* y seleccione **Ver código**.
2. Reemplace el código existente en el archivo *productlist.aspx.CS* por este:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Este código muestra el método `GetProducts` al que hace referencia la propiedad `ItemType` del control **ListView** en la página *productlist. aspx* . Para limitar los resultados a una categoría de base de datos específica, el código establece el valor `categoryId` del valor de la cadena de consulta que se pasa a la página *productlist. aspx* cuando se navega a la página *productlist. aspx* . La clase `QueryStringAttribute` del espacio de nombres `System.Web.ModelBinding` se utiliza para recuperar el valor de la variable de cadena de consulta `id`. Esto indica al enlace de modelos que intente enlazar un valor de la cadena de consulta con el parámetro `categoryId` en tiempo de ejecución.

Cuando se pasa una categoría válida como una cadena de consulta a la página, los resultados de la consulta se limitan a los productos de la base de datos que coinciden con el valor de `categoryId`. Por ejemplo, si la dirección URL de la página *ProductsList. aspx* es esta:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

En la página solo se muestran los productos en los que el `categoryId` es igual a `1`.

Se muestran todos los productos si no se incluye ninguna cadena de consulta cuando se llama a la página *productlist. aspx* .

Los orígenes de los valores de estos métodos se conocen como *proveedores de valores* (como *QueryString*), y los atributos de parámetro que indican qué proveedor de valor se va a usar se conocen como *atributos de proveedor de valor* (como `id`). ASP.NET incluye proveedores de valores y atributos correspondientes para todos los orígenes típicos de datos proporcionados por el usuario en una aplicación de formularios Web Forms, como la cadena de consulta, las cookies, los valores de formulario, los controles, el estado de la vista, el estado de la sesión y las propiedades del perfil. También puede escribir proveedores de valores personalizados.

### <a name="run-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación ahora para ver todos los productos o los productos de una categoría.

1. Presione **F5** mientras está en Visual Studio para ejecutar la aplicación.  
   El explorador se abre y muestra la página *default. aspx* .

2. Seleccione **automóviles** en el menú de navegación categoría de producto.  
   La página *productlist. aspx* muestra solo los productos de categoría **Cars** . Más adelante en este tutorial, se mostrarán los detalles del producto.  

    ![Mostrar elementos de datos y detalles: automóviles](display_data_items_and_details/_static/image2.png)

3. Seleccione **productos** en el menú de navegación de la parte superior.  
   Una vez más, se muestra la página *productlist. aspx* , pero esta vez muestra toda la lista de productos.   

    ![Mostrar elementos de datos y detalles: productos](display_data_items_and_details/_static/image3.png)

4. Cierre el explorador y vuelva a Visual Studio.

### <a name="add-a-data-control-to-display-product-details"></a>Agregar un control de datos para mostrar los detalles del producto

A continuación, modificará el marcado en la página *productdetails. aspx* que agregó en el tutorial anterior para mostrar información específica del producto.

1. En **Explorador de soluciones**, Abra *productdetails. aspx*.

2. Reemplace el marcado existente por este marcado:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Este código usa un control **FormView** para mostrar detalles concretos del producto. Este marcado usa métodos como los métodos que se usan para Mostrar datos en la página *productlist. aspx* . El control **FormView** se utiliza para mostrar un único registro a la vez desde un origen de datos. Cuando se usa el control **FormView** , se crean plantillas para mostrar y editar valores enlazados a datos. Estas plantillas contienen controles, expresiones de enlace y formatos que definen la apariencia y la funcionalidad del formulario.

La conexión del marcado anterior a la base de datos requiere código adicional.

1. En **Explorador de soluciones**, haga clic con el botón secundario en *productdetails. aspx* y, a continuación, haga clic en **Ver código**.  
   Se muestra el archivo *productdetails.aspx.CS* .

2. Reemplace el código existente con este código:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Este código comprueba un valor de cadena de consulta "`productID`". Si se encuentra un valor de cadena de consulta válido, se muestra el producto correspondiente. Si no se encuentra la cadena de consulta o su valor no es válido, no se muestra ningún producto.

### <a name="run-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver un producto individual que se muestra según el ID. de producto.

1. Presione **F5** mientras está en Visual Studio para ejecutar la aplicación.  
   El explorador se abre y muestra la página *default. aspx* .

2. Seleccione **barcos** en el menú de navegación categoría.  
   Se muestra la página *productlist. aspx* .

3. Seleccione el **barco de papel** en la lista de productos.
   Se muestra la página *productdetails. aspx* .

    ![Mostrar elementos de datos y detalles: productos](display_data_items_and_details/_static/image4.png)
    
4. Cierre el explorador.

## <a name="additional-resources"></a>Recursos adicionales

[Recuperar y Mostrar datos con el enlace de modelos y formularios Web Forms](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha agregado marcado y código para mostrar los productos y los detalles del producto. Ha aprendido acerca de los controles de datos fuertemente tipados, el enlace de modelos y los proveedores de valores. En el siguiente tutorial, agregará un carro de la compra a la aplicación de ejemplo Wingtip Toys. 

> [!div class="step-by-step"]
> [Anterior](ui_and_navigation.md)
> [Siguiente](shopping-cart.md)
