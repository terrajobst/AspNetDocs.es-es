---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Compatibilidad con acciones de OData en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 'En OData, las acciones son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones incluyen: implementar...'
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: ae8b23f0868f992cb2bbbf14ee3f7ac848501515
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448177"
---
# <a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Compatibilidad de las acciones de OData en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> En OData, *las acciones* son una manera de agregar comportamientos del lado servidor que no se definen fácilmente como operaciones CRUD en entidades. Algunos usos de las acciones son:
> 
> - Implementación de transacciones complejas.
> - Manipular varias entidades a la vez.
> - Permitir actualizaciones solo en determinadas propiedades de una entidad.
> - Enviar información al servidor que no está definida en una entidad.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - API Web 2
> - Versión 3 de OData
> - Entity Framework 6

## <a name="example-rating-a-product"></a>Ejemplo: clasificación de un producto

En este ejemplo, queremos que los usuarios puedan valorar los productos y, a continuación, exponer el promedio de las clasificaciones de cada producto. En la base de datos, se almacenará una lista de clasificaciones con clave en los productos.

Este es el modelo que se puede usar para representar las clasificaciones en Entity Framework:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Pero no queremos que los clientes PUBLIQUEn un objeto `ProductRating` en una colección "ratings". De forma intuitiva, la clasificación está asociada a la colección de productos y el cliente solo debe publicar el valor de clasificación.

Por lo tanto, en lugar de usar las operaciones CRUD normales, se define una acción que un cliente puede invocar en un producto. En la terminología de OData, la acción se *enlaza* a las entidades product.

>Las acciones tienen efectos secundarios en el servidor. Por esta razón, se invocan mediante solicitudes HTTP POST. Las acciones pueden tener parámetros y tipos devueltos, que se describen en los metadatos del servicio. El cliente envía los parámetros en el cuerpo de la solicitud y el servidor envía el valor devuelto en el cuerpo de la respuesta. Para invocar la acción "Rate Product", el cliente envía un POST a un URI como el siguiente:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Los datos de la solicitud POST son simplemente la clasificación del producto:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Declare la acción en el Entity Data Model

En la configuración de la API Web, agregue la acción a Entity Data Model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Este código define "RateProduct" como una acción que se puede realizar en las entidades del producto. También declara que la acción toma un parámetro **int** denominado "rating" y devuelve un valor **int** .

## <a name="add-the-action-to-the-controller"></a>Agregar la acción al controlador

La acción "RateProduct" se enlaza a las entidades product. Para implementar la acción, agregue un método denominado `RateProduct` al controlador Products:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Observe que el nombre de método coincide con el nombre de la acción en el EDM. El método tiene dos parámetros:

- *key*: la clave del producto que se va a evaluar.
- *parámetros*: un diccionario de valores de parámetro de acción.

Si utiliza las convenciones de enrutamiento predeterminadas, el parámetro de clave se debe denominar "Key". También es importante incluir el atributo **[FromOdataUri]** , como se muestra. Este atributo indica a la API Web que use las reglas de sintaxis de OData cuando analiza la clave del URI de solicitud.

Use el Diccionario de *parámetros* para obtener los parámetros de acción:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Si el cliente envía los parámetros de acción en el formato correcto, el valor de **ModelState. IsValid** es true. En ese caso, puede usar el diccionario **ODataActionParameters** para obtener los valores de los parámetros. En este ejemplo, la acción de `RateProduct` toma un parámetro único denominado "rating".

## <a name="action-metadata"></a>Metadatos de acción

Para ver los metadatos del servicio, envíe una solicitud GET a/OData/$metadata. Esta es la parte de los metadatos que declara la acción `RateProduct`:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

El elemento **FunctionImport** declara la acción. La mayoría de los campos se explican por sí solos, pero hay dos que merece la pena mencionar:

- **IsBindable** significa que la acción se puede invocar en la entidad de destino, al menos parte del tiempo.
- **IsAlwaysBindable** significa que la acción siempre se puede invocar en la entidad de destino.

La diferencia es que algunas acciones siempre están disponibles para los clientes, pero otras acciones pueden depender del estado de la entidad. Por ejemplo, supongamos que define una acción de "compra". Solo puede comprar un elemento que esté en existencias. Si el elemento está agotado, un cliente no puede invocar esa acción.

Al definir el EDM, el método de **acción** crea una acción que se puede enlazar siempre:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Hablaré sobre acciones no siempre enlazables (también denominadas acciones *transitorias* ) más adelante en este tema.

## <a name="invoking-the-action"></a>Invocar la acción

Ahora veamos cómo un cliente invocaría esta acción. Supongamos que el cliente desea proporcionar una clasificación de 2 al producto con el identificador 4. Este es un mensaje de solicitud de ejemplo, con el formato JSON para el cuerpo de la solicitud:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Este es el mensaje de respuesta:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Enlazar una acción a un conjunto de entidades

En el ejemplo anterior, la acción se enlaza a una sola entidad: el cliente califica un único producto. También puede enlazar una acción a una colección de entidades. Solo tiene que realizar los cambios siguientes:

En el EDM, agregue la acción a la propiedad de **colección** de la entidad.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

En el método de controlador, omita el parámetro *key* .

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Ahora el cliente invoca la acción en el conjunto de entidades Products:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Acciones con parámetros de colección

Las acciones pueden tener parámetros que toman una colección de valores. En el EDM, use **CollectionParameter&lt;t&gt;** para declarar el parámetro.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Esto declara un parámetro denominado "ratings" que toma una colección de valores **int** . En el método de controlador, todavía se obtiene el valor del parámetro del objeto **ODataActionParameters** , pero ahora el valor es un valor **int&lt;int&gt;** :

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Acciones transitorias

En el ejemplo "RateProduct", los usuarios siempre pueden clasificar un producto, por lo que la acción siempre está disponible. Pero algunas acciones dependen del estado de la entidad. Por ejemplo, en un servicio de alquiler de vídeo, la acción "desproteger" no siempre está disponible. (Depende de si hay una copia de ese vídeo disponible). Este tipo de acción se denomina acción *transitoria* .

En los metadatos del servicio, una acción transitoria tiene **IsAlwaysBindable** igual a false. Ese es realmente el valor predeterminado, por lo que los metadatos tendrán el siguiente aspecto:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Este es el motivo por el que esto es importante: Si una acción es transitoria, el servidor debe indicar al cliente si la acción está disponible. Para ello, incluye un vínculo a la acción en la entidad. Este es un ejemplo de una entidad de película:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

La propiedad "#CheckOut" contiene un vínculo a la acción de desprotección. Si la acción no está disponible, el servidor omite el vínculo.

Para declarar una acción transitoria en el EDM, llame al método **TransientAction** :

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Además, debe proporcionar una función que devuelva un vínculo de acción para una entidad determinada. Establezca esta función llamando a **HasActionLink**. Puede escribir la función como una expresión lambda:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Si la acción está disponible, la expresión lambda devuelve un vínculo a la acción. El serializador de OData incluye este vínculo cuando serializa la entidad. Cuando la acción no está disponible, la función devuelve `null`.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de acciones de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
