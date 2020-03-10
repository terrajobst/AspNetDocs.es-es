---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Llamar a un servicio de OData desde un clienteC#.net () | Microsoft Docs
author: MikeWasson
description: En este tutorial se muestra cómo llamar a un servicio de C# OData desde una aplicación cliente. Versiones de software usadas en el tutorial Visual Studio 2013 (funciona con objetos visuales...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498169"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a>Llamar a un servicio OData desde un cliente .NET (C#)

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> En este tutorial se muestra cómo llamar a un servicio de C# OData desde una aplicación cliente.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funciona con Visual Studio 2012)
> - [Biblioteca cliente de Servicios de datos de WCF](https://msdn.microsoft.com/library/cc668772.aspx)
> - API Web 2. (El servicio OData de ejemplo se crea con Web API 2, pero la aplicación cliente no depende de la API Web).

En este tutorial, veremos cómo crear una aplicación cliente que llama a un servicio de OData. El servicio OData expone las siguientes entidades:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

En los artículos siguientes se describe cómo implementar el servicio OData en Web API. (Sin embargo, no es necesario que los lea para comprender este tutorial).

- [Creación de un punto de conexión de OData en Web API 2](creating-an-odata-endpoint.md)
- [Relaciones de entidad de OData en Web API 2](working-with-entity-relations.md)
- [Acciones de OData en Web API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generar el proxy del servicio

El primer paso es generar un proxy de servicio. El proxy de servicio es una clase .NET que define los métodos para tener acceso al servicio OData. El proxy convierte las llamadas al método en solicitudes HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Abra el proyecto de servicio OData en Visual Studio. Presione CTRL + F5 para ejecutar el servicio localmente en IIS Express. Tenga en cuenta la dirección local, incluido el número de puerto que Visual Studio asigna. Necesitará esta dirección al crear el proxy.

Después, abra otra instancia de Visual Studio y cree un proyecto de aplicación de consola. La aplicación de consola será nuestra aplicación cliente de OData. (También puede Agregar el proyecto a la misma solución que el servicio).

> [!NOTE]
> Los pasos restantes hacen referencia al proyecto de consola.

En Explorador de soluciones, haga clic con el botón secundario en **referencias** y seleccione **Agregar referencia de servicio**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

En el cuadro de diálogo **Agregar referencia de servicio** , escriba la dirección del servicio OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

donde *Port* es el número de puerto.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

En **espacio de nombres**, escriba "ProductService". Esta opción define el espacio de nombres de la clase de proxy.

Haga clic en **Ir**. Visual Studio lee el documento de metadatos de OData para detectar las entidades en el servicio.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Haga clic en **Aceptar** para agregar la clase de proxy a su proyecto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Crear una instancia de la clase de proxy de servicio

Dentro de su método de `Main`, cree una nueva instancia de la clase de proxy, como se indica a continuación:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

De nuevo, use el número de Puerto Real en el que se ejecuta el servicio. Al implementar el servicio, usará el URI del servicio en directo. No es necesario actualizar el proxy.

En el código siguiente se agrega un controlador de eventos que imprime los URI de solicitud en la ventana de la consola. Este paso no es necesario, pero es interesante ver los URI de cada consulta.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Consultar el servicio

El código siguiente obtiene la lista de productos del servicio OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Tenga en cuenta que no es necesario escribir código para enviar la solicitud HTTP o analizar la respuesta. La clase de proxy hace esto automáticamente al enumerar la `Container.Products` colección en el bucle **foreach** .

Al ejecutar la aplicación, el resultado debe ser similar al siguiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Para obtener una entidad por identificador, utilice una cláusula `where`.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

En el resto de este tema, no mostraré toda la función de `Main`, solo el código necesario para llamar al servicio.

## <a name="apply-query-options"></a>Aplicar opciones de consulta

OData define [las opciones de consulta](../supporting-odata-query-options.md) que se pueden usar para filtrar, ordenar, datos de página, etc. En el proxy del servicio, puede aplicar estas opciones mediante varias expresiones LINQ.

En esta sección, mostraré breves ejemplos. Para obtener más información, vea el tema [consideraciones sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) en MSDN.

### <a name="filtering-filter"></a>Filtrado ($filter)

Para filtrar, use una cláusula `where`. En el ejemplo siguiente se filtra por categoría de producto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Este código corresponde a la siguiente consulta de OData.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Observe que el proxy convierte la cláusula `where` en una expresión `$filter` de OData.

### <a name="sorting-orderby"></a>Ordenación ($orderby)

Para ordenar, use una cláusula `orderby`. En el ejemplo siguiente se ordena por precio, de mayor a menor.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Esta es la solicitud de OData correspondiente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paginación del lado cliente ($skip y $top)

En el caso de los conjuntos de entidades de gran tamaño, es posible que el cliente desee limitar el número de resultados. Por ejemplo, un cliente podría mostrar 10 entradas a la vez. Esto se denomina *paginación del lado cliente*. (También hay [paginación del lado servidor](../supporting-odata-query-options.md#server-paging), donde el servidor limita el número de resultados). Para realizar la paginación del lado cliente, use los métodos **SKIP** y **Take** de Linq. En el ejemplo siguiente se omiten los primeros 40 resultados y se toman los 10 siguientes.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Select ($select) y Expand ($expand)

Para incluir entidades relacionadas, use el método `DataServiceQuery<t>.Expand`. Por ejemplo, para incluir el `Supplier` para cada `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Para cambiar la forma de la respuesta, use la cláusula LINQ **Select** . En el ejemplo siguiente se obtiene solo el nombre de cada producto, sin ninguna otra propiedad.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Esta es la solicitud de OData correspondiente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Una cláusula SELECT puede incluir entidades relacionadas. En ese caso, no llame a **Expand**; en este caso, el proxy incluye automáticamente la expansión. En el ejemplo siguiente se obtiene el nombre y el proveedor de cada producto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Esta es la solicitud de OData correspondiente. Tenga en cuenta que incluye la opción **$Expand** .

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Para obtener más información sobre $select y $expand, consulte [uso de $Select, $Expand y $Value en Web API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Agregar una nueva entidad

Para agregar una nueva entidad a un conjunto de entidades, llame a `AddToEntitySet`, donde *EntitySet* es el nombre del conjunto de entidades. Por ejemplo, `AddToProducts` agrega un nuevo `Product` al conjunto de entidades `Products`. Al generar el proxy, WCF Data Services crea automáticamente estos métodos **AddTo** fuertemente tipados.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Para agregar un vínculo entre dos entidades, use los métodos **AddLink** y **SetLink** . El código siguiente agrega un nuevo proveedor y un nuevo producto y, a continuación, crea vínculos entre ellos.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Use **AddLink** cuando la propiedad de navegación sea una colección. En este ejemplo, vamos a agregar un producto a la colección de `Products` del proveedor.

Use **SetLink** cuando la propiedad de navegación sea una sola entidad. En este ejemplo, estamos estableciendo la propiedad `Supplier` en el producto.

## <a name="update--patch"></a>Actualización/revisión

Para actualizar una entidad, llame al método **UpdateObject** .

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

La actualización se realiza cuando se llama a **SaveChanges**. De forma predeterminada, WCF envía una solicitud HTTP MERGE. La opción **PatchOnUpdate** indica a WCF que envíe una revisión http en su lugar.

> [!NOTE]
> ¿Por qué PATCH y MERGE? La especificación HTTP 1,1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) no definió ningún método http con la semántica "actualización parcial". Para admitir las actualizaciones parciales, la especificación de OData definía el método MERGE. En 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) definía el método patch para actualizaciones parciales. Puede leer parte del historial de esta entrada de [blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) en el blog de WCF Data Services. En la actualidad, la revisión es preferible a la combinación. El controlador OData creado por el scaffolding de API Web admite ambos métodos.

Si desea reemplazar toda la entidad (semántica de PUT), especifique la opción **ReplaceOnUpdate** . Esto hace que WCF envíe una solicitud HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Eliminar una entidad

Para eliminar una entidad, llame a **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Invocar una acción de OData

En OData, [las acciones](odata-actions.md) son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades.

Aunque el documento de metadatos de OData describe las acciones, la clase de proxy no crea ningún método fuertemente tipado para ellos. Todavía puede invocar una acción de OData mediante el método **Execute** genérico. Sin embargo, tendrá que conocer los tipos de datos de los parámetros y el valor devuelto.

Por ejemplo, la acción `RateProduct` toma el parámetro denominado "rating" de tipo `Int32` y devuelve una `double`. En el código siguiente se muestra cómo invocar esta acción.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Para obtener más información, consulte[llamar a operaciones y acciones de servicio](https://msdn.microsoft.com/library/hh230677.aspx).

Una opción consiste en extender la clase de **contenedor** para proporcionar un método fuertemente tipado que invoca la acción:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
