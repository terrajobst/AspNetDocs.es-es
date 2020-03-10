---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serialización de JSON y XML en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Describe los formateadores JSON y XML en ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449131"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a>Serialización de JSON y XML en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

En este artículo se describen los formateadores JSON y XML en ASP.NET Web API.

En ASP.NET Web API, un *formateador de tipo de medio* es un objeto que puede:

- Leer objetos CLR de un cuerpo de mensaje HTTP
- Escribir objetos CLR en el cuerpo de un mensaje HTTP

Web API proporciona formateadores de tipo de medio para JSON y XML. El marco de trabajo inserta estos formateadores en la canalización de forma predeterminada. Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.

## <a name="contents"></a>Contenido

- [Formateador de tipo de medio JSON](#json_media_type_formatter)

    - [Propiedades de solo lectura](#json_readonly)
    - [Días](#json_dates)
    - [Sangría](#json_indenting)
    - [Grafía Camel](#json_camelcasing)
    - [Objetos anónimos y con establecimiento flexible de tipos](#json_anon)
- [Formateador de tipo de medio XML](#xml_media_type_formatter)

    - [Propiedades de solo lectura](#xml_readonly)
    - [Días](#xml_dates)
    - [Sangría](#xml_indenting)
    - [Establecer serializadores XML por tipo](#xml_pertype)
- [Quitar el formateador JSON o XML](#removing_the_json_or_xml_formatter)
- [Controlar referencias circulares de objetos](#handling_circular_object_references)
- [Probar la serialización de objetos](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a>Formateador de tipo de medio JSON

La clase **JsonMediaTypeFormatter** proporciona formato JSON. De forma predeterminada, **JsonMediaTypeFormatter** usa la biblioteca [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) para realizar la serialización. Json.NET es un proyecto de código abierto de terceros.

Si lo prefiere, puede configurar la clase **JsonMediaTypeFormatter** para usar **DataContractJsonSerializer** en lugar de JSON.net. Para ello, establezca la propiedad **UseDataContractJsonSerializer** en **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a>Serialización de JSON

En esta sección se describen algunos comportamientos específicos del formateador JSON mediante el serializador [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) predeterminado. No pretende ser documentación completa de la biblioteca Json.NET; para obtener más información, vea la [documentación de JSON.net](http://james.newtonking.com/projects/json/help/).

#### <a name="what-gets-serialized"></a>¿Qué se serializa?

De forma predeterminada, todas las propiedades y campos públicos se incluyen en el JSON serializado. Para omitir una propiedad o un campo, decorarlo con el atributo **JsonIgnore** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

Si prefiere un enfoque de&quot; de &quot;participar, decore la clase con el atributo **DataContract** . Si este atributo está presente, se omiten los miembros a menos que tengan **DataMember**. También puede usar **DataMember** para serializar miembros privados.

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

De forma predeterminada, las propiedades de solo lectura se serializan.

<a id="json_dates"></a>
### <a name="dates"></a>Fechas

De forma predeterminada, Json.NET escribe fechas en formato [ISO 8601](http://www.w3.org/TR/NOTE-datetime) . Las fechas en UTC (hora universal coordinada) se escriben con un sufijo "Z". Las fechas en hora local incluyen un ajuste de zona horaria. Por ejemplo:

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

De forma predeterminada, Json.NET conserva la zona horaria. Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

Si prefiere usar el formato de fecha (`"\/Date(ticks)\/"`) [JSON de Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) en lugar de ISO 8601, establezca la propiedad **DateFormatHandling** en la configuración del serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a>Aplicar sangría

Para escribir JSON con sangría, establezca la configuración de **formato** en **formato. sangría**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a>Grafía Camel

Para escribir nombres de propiedad JSON con grafía Camel, sin cambiar el modelo de datos, establezca **CamelCasePropertyNamesContractResolver** en el serializador:

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a>Objetos anónimos y con establecimiento flexible de tipos

Un método de acción puede devolver un objeto anónimo y serializarlo en JSON. Por ejemplo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

El cuerpo del mensaje de respuesta contendrá el siguiente JSON:

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

Si la API Web recibe objetos JSON con estructura imprecisa de los clientes, puede deserializar el cuerpo de la solicitud en un tipo **Newtonsoft. JSON. Linq. JObject** .

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

Sin embargo, suele ser mejor utilizar objetos de datos fuertemente tipados. No es necesario analizar los datos por sí mismo y se obtienen las ventajas de la validación del modelo.

El serializador XML no admite tipos anónimos o instancias de **JObject** . Si usa estas características para los datos JSON, debe quitar el formateador XML de la canalización, como se describe más adelante en este artículo.

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a>Formateador de tipo de medio XML

La clase **XmlMediaTypeFormatter** proporciona el formato XML. De forma predeterminada, **XmlMediaTypeFormatter** usa la clase **DataContractSerializer** para realizar la serialización.

Si lo prefiere, puede configurar **XmlMediaTypeFormatter** para usar **XmlSerializer** en lugar de **DataContractSerializer**. Para ello, establezca la propiedad **UseXmlSerializer** en **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

La clase **XmlSerializer** admite un conjunto más estrecho de tipos que **DataContractSerializer**, pero proporciona más control sobre el XML resultante. Considere la posibilidad de usar **XmlSerializer** si necesita hacer coincidir un esquema XML existente.

### <a name="xml-serialization"></a>Serialización XML

En esta sección se describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.

De forma predeterminada, DataContractSerializer se comporta de la siguiente manera:

- Se serializan todas las propiedades y campos públicos de lectura/escritura. Para omitir una propiedad o un campo, decorarlo con el atributo **IgnoreDataMember** .
- Los miembros privados y protegidos no se serializan.
- Las propiedades de solo lectura no se serializan. (Sin embargo, se serializa el contenido de una propiedad de colección de solo lectura).
- Los nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.
- Se utiliza un espacio de nombres XML predeterminado.

Si necesita más control sobre la serialización, puede decorar la clase con el atributo **DataContract** . Cuando este atributo está presente, la clase se serializa como se indica a continuación:

- &quot;participar&quot; enfoque: las propiedades y los campos no se serializan de forma predeterminada. Para serializar una propiedad o un campo, decorarlo con el atributo **DataMember** .
- Para serializar un miembro privado o protegido, decorarlo con el atributo **DataMember** .
- Las propiedades de solo lectura no se serializan.
- Para cambiar el modo en que el nombre de clase aparece en el XML, establezca el parámetro *Name* en el atributo **DataContract** .
- Para cambiar el modo en que aparece un nombre de miembro en el XML, establezca el parámetro *Name* en el atributo **DataMember** .
- Para cambiar el espacio de nombres XML, establezca el parámetro *namespace* en la clase **DataContract** .

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a>Propiedades de solo lectura

Las propiedades de solo lectura no se serializan. Si una propiedad de solo lectura tiene un campo privado de respaldo, puede marcar el campo privado con el atributo **DataMember** . Este enfoque requiere el atributo **DataContract** en la clase.

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a>Fechas

Las fechas se escriben en formato ISO 8601. Por ejemplo, &quot;2012-05-23T20:21:37.9116538 Z&quot;.

<a id="xml_indenting"></a>
### <a name="indenting"></a>Aplicar sangría

Para escribir XML con sangría, establezca la propiedad **indent** en **true**:

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a>Establecer serializadores XML por tipo

Puede establecer diferentes serializadores XML para diferentes tipos CLR. Por ejemplo, podría tener un objeto de datos determinado que requiera **XmlSerializer** para la compatibilidad con versiones anteriores. Puede usar **XmlSerializer** para este objeto y seguir usando **DataContractSerializer** para otros tipos.

Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

Puede especificar un **XmlSerializer** o cualquier objeto que se derive de **XmlObjectSerializer**.

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a>Quitar el formateador JSON o XML

Puede quitar el formateador JSON o el formateador XML de la lista de formateadores, si no desea utilizarlos. Las principales razones para ello son:

- Para restringir las respuestas de la API Web a un tipo de medio determinado. Por ejemplo, puede decidir admitir solo las respuestas JSON y quitar el formateador XML.
- Es para reemplazar el formateador predeterminado por un formateador personalizado. Por ejemplo, podría reemplazar el formateador JSON por su propia implementación personalizada de un formateador JSON.

En el código siguiente se muestra cómo quitar los formateadores predeterminados. Llame a este desde el método de **Inicio de\_** de la aplicación, definido en global. asax.

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a>Controlar referencias circulares de objetos

De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores. Si dos propiedades hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces. Se trata de un problema determinado si el gráfico de objetos contiene ciclos, ya que el serializador producirá una excepción cuando detecte un bucle en el gráfico.

Tenga en cuenta los siguientes modelos de objetos y controladores.

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

Al invocar esta acción, el formateador producirá una excepción, lo que se traduce en una respuesta de código de estado 500 (error interno del servidor) al cliente.

Para conservar las referencias de objeto en JSON, agregue el siguiente código al método de **Inicio de\_** de la aplicación en el archivo global. asax:

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

Ahora, la acción del controlador devolverá JSON que tiene el siguiente aspecto:

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

Observe que el serializador agrega una &quot;$id propiedad&quot; a ambos objetos. Además, detecta que la propiedad Employee. Department crea un bucle, de modo que reemplaza el valor por una referencia de objeto: {&quot;$ref&quot;:&quot;1&quot;}.

> [!NOTE]
> Las referencias de objeto no son estándar en JSON. Antes de usar esta característica, tenga en cuenta si los clientes podrán analizar los resultados. Podría ser más fácil quitar ciclos del gráfico. Por ejemplo, en este ejemplo no se necesita realmente el vínculo del empleado al Departamento.

Para conservar las referencias de objeto en XML, tiene dos opciones. La opción más sencilla es agregar `[DataContract(IsReference=true)]` a la clase de modelo. El parámetro *IsReference* habilita las referencias a objetos. Recuerde que **DataContract** hace que se opte por la serialización, por lo que también tendrá que agregar atributos **DataMember** a las propiedades:

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

Ahora, el formateador generará código XML similar al siguiente:

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

Si desea evitar atributos en la clase del modelo, hay otra opción: crear una nueva instancia de **DataContractSerializer** específica del tipo y establecer *preserveObjectReferences* en **true** en el constructor. Después, establezca esta instancia como un serializador por tipo en el formateador de tipo de medio XML. En el código siguiente se muestra cómo hacerlo:

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a>Probar la serialización de objetos

Al diseñar la API Web, resulta útil probar cómo se serializan los objetos de datos. Puede hacerlo sin crear un controlador o invocar una acción de controlador.

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
