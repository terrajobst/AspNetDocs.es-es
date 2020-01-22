---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: Compatibilidad con BSON en ASP.NET Web API 2,1-ASP.NET 4. x
author: MikeWasson
description: muestra cómo usar BSON en un controlador de API Web (servidor) y en una aplicación cliente de .NET para ASP.NET 4. x.
ms.author: riande
ms.date: 01/20/2014
ms.custom: seoapril2019
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: ccbc0372120301b1cd8d4cdc86bd9fba9404d8ae
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519340"
---
# <a name="bson-support-in-aspnet-web-api-21"></a>Compatibilidad con BSON en ASP.NET Web API 2,1

por [Mike Wasson](https://github.com/MikeWasson)

En este tema se muestra cómo usar BSON en el controlador de API Web (lado servidor) y en una aplicación cliente .NET. Web API 2,1 presenta compatibilidad con BSON. 

## <a name="what-is-bson"></a>¿Qué es BSON?

[Bson](http://bsonspec.org/) es un formato de serialización binaria. "BSON" representa "JSON binario", pero BSON y JSON se serializan de forma muy diferente. BSON es "JSON-like", porque los objetos se representan como pares de nombre y valor, similares a JSON. A diferencia de JSON, los tipos de datos numéricos se almacenan como bytes, no como cadenas

BSON se diseñó para ser ligero, fácil de examinar y rápido para codificar y descodificar.

- BSON tiene un tamaño comparable en JSON. En función de los datos, una carga BSON puede ser menor o mayor que una carga JSON. Para serializar datos binarios, como un archivo de imagen, BSON es menor que JSON, ya que los datos binarios no están codificados en Base64.
- Los documentos BSON son fáciles de examinar porque los elementos tienen el prefijo de un campo de longitud, por lo que un analizador puede omitir elementos sin descodificarlos.
- La codificación y la descodificación son eficaces, ya que los tipos de datos numéricos se almacenan como números, no como cadenas.

Los clientes nativos, como las aplicaciones cliente de .NET, pueden beneficiarse del uso de BSON en lugar de formatos basados en texto como JSON o XML. En el caso de los clientes del explorador, probablemente querrá usar JSON, ya que JavaScript puede convertir directamente la carga de JSON.

Afortunadamente, Web API usa la [negociación de contenido](content-negotiation.md), por lo que la API puede admitir ambos formatos y permitir que el cliente elija.

## <a name="enabling-bson-on-the-server"></a>Habilitación de BSON en el servidor

En la configuración de la API Web, agregue **BsonMediaTypeFormatter** a la colección de formateadores.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Ahora, si el cliente solicita "Application/bson", la API Web usará el formateador BSON.

Para asociar BSON a otros tipos de medios, agréguelos a la colección SupportedMediaTypes. El código siguiente agrega "Application/vnd. contoso" a los tipos de medios admitidos:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Sesión HTTP de ejemplo

En este ejemplo, usaremos la siguiente clase de modelo junto con un controlador de API web simple:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Un cliente puede enviar la siguiente solicitud HTTP:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Esta es la respuesta:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Aquí he reemplazado los datos binarios por &quot;.&quot; caracteres. En la siguiente captura de pantalla de Fiddler se muestran los valores hexadecimales sin formato.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>Uso de BSON con HttpClient

Las aplicaciones de clientes de .NET pueden usar el formateador BSON con **HttpClient**. Para obtener más información sobre **HttpClient**, consulte [llamada a una API Web desde un cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).

El código siguiente envía una solicitud GET que acepta BSON y, a continuación, deserializa la carga BSON en la respuesta.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

Para solicitar BSON desde el servidor, establezca el encabezado Accept en "Application/bson":

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Para deserializar el cuerpo de la respuesta, use **BsonMediaTypeFormatter**. Este formateador no está en la colección de formateadores predeterminados, por lo que tiene que especificarlo al leer el cuerpo de la respuesta:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

En el ejemplo siguiente se muestra cómo enviar una solicitud POST que contiene BSON.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Gran parte de este código es igual que el ejemplo anterior. Pero en el método **PostAsync** , especifique **BsonMediaTypeFormatter** como el formateador:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Serializar tipos primitivos de nivel superior

Cada documento de BSON es una lista de pares clave-valor. La especificación BSON no define una sintaxis para serializar un solo valor sin formato, como un entero o una cadena.

Para evitar esta limitación, el **BsonMediaTypeFormatter** trata los tipos primitivos como un caso especial. Antes de la serialización, convierte el valor en un par clave-valor con la clave "Value". Por ejemplo, supongamos que el controlador de API devuelve un entero:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Antes de la serialización, el formateador BSON lo convierte al siguiente par clave-valor:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Al deserializar, el formateador vuelve a convertir los datos al valor original. Sin embargo, los clientes que usen un analizador de BSON diferente deberán controlar este caso, si la API Web devuelve valores sin formato. En general, debe considerar la posibilidad de devolver datos estructurados, en lugar de valores sin formato.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de BSON de API Web](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BSONSample/)

[Formateadores multimedia](media-formatters.md)
