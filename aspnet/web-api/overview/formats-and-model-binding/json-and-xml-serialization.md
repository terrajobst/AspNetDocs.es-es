---
uid: web-api/overview/formats-and-model-binding/json-and-xml-serialization
title: Serialización JSON y XML en ASP.NET Web API - ASP.NET 4.x
author: MikeWasson
description: Describe los formateadores JSON y XML en ASP.NET Web API para ASP.NET 4.x.
ms.author: riande
ms.date: 05/30/2012
ms.custom: seoapril2019
ms.assetid: 1cd7525d-de5e-4ab6-94f0-51480d3255d1
msc.legacyurl: /web-api/overview/formats-and-model-binding/json-and-xml-serialization
msc.type: authoredcontent
ms.openlocfilehash: 00fa07f00eabf7e6c883c5e9ceaf9a38a8f49605
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65126171"
---
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="20bae-103">Serialización JSON y XML en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="20bae-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="20bae-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="20bae-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="20bae-105">En este artículo se describe a los formateadores JSON y XML en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="20bae-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="20bae-106">En ASP.NET Web API, un *formateador de tipo de medio* es un objeto que puede:</span><span class="sxs-lookup"><span data-stu-id="20bae-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="20bae-107">Cuerpo del mensaje de los objetos de CLR de lectura de HTTP</span><span class="sxs-lookup"><span data-stu-id="20bae-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="20bae-108">Escribir objetos CLR en un cuerpo de mensaje HTTP</span><span class="sxs-lookup"><span data-stu-id="20bae-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="20bae-109">API Web proporciona a los formateadores de tipo de medio para JSON y XML.</span><span class="sxs-lookup"><span data-stu-id="20bae-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="20bae-110">El marco de trabajo inserta a estos formateadores en la canalización de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="20bae-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="20bae-111">Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="20bae-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="20bae-112">Contenido</span><span class="sxs-lookup"><span data-stu-id="20bae-112">Contents</span></span>

- [<span data-ttu-id="20bae-113">Formateador de tipo de medio JSON</span><span class="sxs-lookup"><span data-stu-id="20bae-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="20bae-114">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="20bae-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="20bae-115">Fechas</span><span class="sxs-lookup"><span data-stu-id="20bae-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="20bae-116">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="20bae-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="20bae-117">Uso de mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="20bae-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="20bae-118">Objetos anónimos y débilmente tipada</span><span class="sxs-lookup"><span data-stu-id="20bae-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="20bae-119">Formateador de tipo de medio XML</span><span class="sxs-lookup"><span data-stu-id="20bae-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="20bae-120">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="20bae-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="20bae-121">Fechas</span><span class="sxs-lookup"><span data-stu-id="20bae-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="20bae-122">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="20bae-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="20bae-123">Serializadores XML de configuración por tipo</span><span class="sxs-lookup"><span data-stu-id="20bae-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="20bae-124">Quitar el JSON o un formateador XML</span><span class="sxs-lookup"><span data-stu-id="20bae-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="20bae-125">Control de referencias circulares a objetos</span><span class="sxs-lookup"><span data-stu-id="20bae-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="20bae-126">Serialización de objetos de prueba</span><span class="sxs-lookup"><span data-stu-id="20bae-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="20bae-127">Formateador de tipo de medio JSON</span><span class="sxs-lookup"><span data-stu-id="20bae-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="20bae-128">Formato JSON proporcionado por el **JsonMediaTypeFormatter** clase.</span><span class="sxs-lookup"><span data-stu-id="20bae-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="20bae-129">De forma predeterminada, **JsonMediaTypeFormatter** usa el [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) biblioteca para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="20bae-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="20bae-130">Json.NET es un proyecto de código abierto de terceros.</span><span class="sxs-lookup"><span data-stu-id="20bae-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="20bae-131">Si lo prefiere, puede configurar el **JsonMediaTypeFormatter** clase se debe utilizar el **DataContractJsonSerializer** en lugar de Json.NET.</span><span class="sxs-lookup"><span data-stu-id="20bae-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="20bae-132">Para ello, establezca el **UseDataContractJsonSerializer** propiedad **true**:</span><span class="sxs-lookup"><span data-stu-id="20bae-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="20bae-133">Serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="20bae-133">JSON Serialization</span></span>

<span data-ttu-id="20bae-134">Esta sección describen algunos comportamientos específicos del formateador JSON, con el valor predeterminado [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializador.</span><span class="sxs-lookup"><span data-stu-id="20bae-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="20bae-135">Esto no pretende ser una documentación completa de la biblioteca Json.NET; Para obtener más información, consulte el [documentación Json.NET](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="20bae-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="20bae-136">¿Lo que se serializa?</span><span class="sxs-lookup"><span data-stu-id="20bae-136">What Gets Serialized?</span></span>

<span data-ttu-id="20bae-137">De forma predeterminada, todas las propiedades públicas y campos se incluyen en el JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="20bae-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="20bae-138">Para omitir un campo o propiedad, decórela con el **JsonIgnore** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="20bae-139">Si prefiere un &quot;participación&quot; enfocar, decore la clase con el **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="20bae-140">Si este atributo está presente, se omiten los miembros a menos que tengan la **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="20bae-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="20bae-141">También puede usar **DataMember** para serializar los miembros privados.</span><span class="sxs-lookup"><span data-stu-id="20bae-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="20bae-142">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="20bae-142">Read-Only Properties</span></span>

<span data-ttu-id="20bae-143">Propiedades de solo lectura se serializan de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="20bae-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="20bae-144">Fechas</span><span class="sxs-lookup"><span data-stu-id="20bae-144">Dates</span></span>

<span data-ttu-id="20bae-145">De forma predeterminada, Json.NET escribe las fechas [ISO 8601](http://www.w3.org/TR/NOTE-datetime) formato.</span><span class="sxs-lookup"><span data-stu-id="20bae-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="20bae-146">Las fechas en formato UTC (Coordinated Universal Time) se escriben con un sufijo "Z".</span><span class="sxs-lookup"><span data-stu-id="20bae-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="20bae-147">Las fechas en la hora local incluyen un desplazamiento de zona horaria.</span><span class="sxs-lookup"><span data-stu-id="20bae-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="20bae-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="20bae-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="20bae-149">De forma predeterminada, Json.NET conserva la zona horaria.</span><span class="sxs-lookup"><span data-stu-id="20bae-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="20bae-150">Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="20bae-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="20bae-151">Si prefiere usar [formato de fecha JSON Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) en lugar de ISO 8601, establezca el **DateFormatHandling** propiedad en la configuración del serializador:</span><span class="sxs-lookup"><span data-stu-id="20bae-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="20bae-152">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="20bae-152">Indenting</span></span>

<span data-ttu-id="20bae-153">Para escribir JSON con sangría, establezca el **formato** si se establece en **Formatting.Indented**:</span><span class="sxs-lookup"><span data-stu-id="20bae-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="20bae-154">Uso de mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="20bae-154">Camel Casing</span></span>

<span data-ttu-id="20bae-155">Para escribir los nombres de propiedad JSON con el uso de mayúsculas y minúsculas, sin cambiar el modelo de datos, establezca el **CamelCasePropertyNamesContractResolver** en el serializador:</span><span class="sxs-lookup"><span data-stu-id="20bae-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="20bae-156">Objetos anónimos y débilmente tipada</span><span class="sxs-lookup"><span data-stu-id="20bae-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="20bae-157">Un método de acción puede devolver un objeto anónimo y serializarlo en JSON.</span><span class="sxs-lookup"><span data-stu-id="20bae-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="20bae-158">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="20bae-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="20bae-159">El cuerpo del mensaje de respuesta contendrá el JSON siguiente:</span><span class="sxs-lookup"><span data-stu-id="20bae-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="20bae-160">Si su web API recibe imprecisa estructurados objetos JSON de los clientes, puede deserializar el cuerpo de solicitud a un **Newtonsoft.Json.Linq.JObject** tipo.</span><span class="sxs-lookup"><span data-stu-id="20bae-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="20bae-161">Sin embargo, es mejor usar objetos de datos fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="20bae-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="20bae-162">A continuación, no es necesario analizar los datos usted mismo y obtener las ventajas de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="20bae-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="20bae-163">El serializador XML no admite tipos anónimos o **JObject** instancias.</span><span class="sxs-lookup"><span data-stu-id="20bae-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="20bae-164">Si utiliza estas características para los datos JSON, debe quitar al formateador XML de la canalización, como se describe más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="20bae-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="20bae-165">Formateador de tipo de medio XML</span><span class="sxs-lookup"><span data-stu-id="20bae-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="20bae-166">Aplicación de formato XML proporcionada por el **XmlMediaTypeFormatter** clase.</span><span class="sxs-lookup"><span data-stu-id="20bae-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="20bae-167">De forma predeterminada, **XmlMediaTypeFormatter** usa el **DataContractSerializer** clase para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="20bae-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="20bae-168">Si lo prefiere, puede configurar el **XmlMediaTypeFormatter** para usar el **XmlSerializer** en lugar de la **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20bae-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="20bae-169">Para ello, establezca el **/usexmlserializer** propiedad **true**:</span><span class="sxs-lookup"><span data-stu-id="20bae-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="20bae-170">El **XmlSerializer** clase admite un conjunto más estrecho de tipos que **DataContractSerializer**, pero proporciona mayor control sobre el XML resultante.</span><span class="sxs-lookup"><span data-stu-id="20bae-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="20bae-171">Considere el uso de **XmlSerializer** si tiene que coincidir con un esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="20bae-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="20bae-172">Serialización XML</span><span class="sxs-lookup"><span data-stu-id="20bae-172">XML Serialization</span></span>

<span data-ttu-id="20bae-173">Esta sección describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20bae-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="20bae-174">De forma predeterminada, DataContractSerializer se comporta como sigue:</span><span class="sxs-lookup"><span data-stu-id="20bae-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="20bae-175">Se serializan todos los campos y propiedades públicas de lectura/escritura.</span><span class="sxs-lookup"><span data-stu-id="20bae-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="20bae-176">Para omitir un campo o propiedad, decórela con el **IgnoreDataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="20bae-177">No se serializan los miembros privados y protegidos.</span><span class="sxs-lookup"><span data-stu-id="20bae-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="20bae-178">No se serializan las propiedades de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="20bae-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="20bae-179">(No obstante, se serializa el contenido de una propiedad de colección de solo lectura).</span><span class="sxs-lookup"><span data-stu-id="20bae-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="20bae-180">Nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="20bae-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="20bae-181">Se utiliza el espacio de nombres XML predeterminado.</span><span class="sxs-lookup"><span data-stu-id="20bae-181">A default XML namespace is used.</span></span>

<span data-ttu-id="20bae-182">Si necesita más control sobre la serialización, puede decorar la clase con el **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="20bae-183">Cuando este atributo está presente, la clase se serializa como sigue:</span><span class="sxs-lookup"><span data-stu-id="20bae-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="20bae-184">&quot;Participar en&quot; enfoque: No se serializan las propiedades y campos de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="20bae-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="20bae-185">Para serializar una propiedad o campo, decórela con el **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="20bae-186">Para serializar un miembro privado o protegido, decórela con el **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="20bae-187">No se serializan las propiedades de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="20bae-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="20bae-188">Para cambiar cómo aparece el nombre de clase en el archivo XML, establezca el *nombre* parámetro en el **DataContract** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="20bae-189">Para cambiar la apariencia de un nombre de miembro en el archivo XML, establezca el *nombre* parámetro en el **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="20bae-190">Para cambiar el espacio de nombres XML, establezca el *Namespace* parámetro en el **DataContract** clase.</span><span class="sxs-lookup"><span data-stu-id="20bae-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="20bae-191">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="20bae-191">Read-Only Properties</span></span>

<span data-ttu-id="20bae-192">No se serializan las propiedades de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="20bae-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="20bae-193">Si una propiedad de solo lectura tiene un campo de respaldo privado, puede marcar el campo privado con la **DataMember** atributo.</span><span class="sxs-lookup"><span data-stu-id="20bae-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="20bae-194">Este enfoque requiere el **DataContract** atributo de la clase.</span><span class="sxs-lookup"><span data-stu-id="20bae-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="20bae-195">Fechas</span><span class="sxs-lookup"><span data-stu-id="20bae-195">Dates</span></span>

<span data-ttu-id="20bae-196">Las fechas se escriben en formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="20bae-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="20bae-197">Por ejemplo, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="20bae-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="20bae-198">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="20bae-198">Indenting</span></span>

<span data-ttu-id="20bae-199">Para escribir XML con sangría, establezca el **sangría** propiedad **true**:</span><span class="sxs-lookup"><span data-stu-id="20bae-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="20bae-200">Serializadores XML de configuración por tipo</span><span class="sxs-lookup"><span data-stu-id="20bae-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="20bae-201">Puede establecer los serializadores XML diferentes para distintos tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="20bae-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="20bae-202">Por ejemplo, podría tener un objeto de datos concreto requiere **XmlSerializer** para compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="20bae-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="20bae-203">Puede usar **XmlSerializer** para este objeto y seguir usando **DataContractSerializer** para otros tipos.</span><span class="sxs-lookup"><span data-stu-id="20bae-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="20bae-204">Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20bae-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="20bae-205">Puede especificar un **XmlSerializer** o cualquier objeto que se deriva de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="20bae-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="20bae-206">Quitar el JSON o un formateador XML</span><span class="sxs-lookup"><span data-stu-id="20bae-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="20bae-207">Puede quitar el formateador JSON o el formateador XML en la lista de formateadores, si no desea usarlos.</span><span class="sxs-lookup"><span data-stu-id="20bae-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="20bae-208">Las razones principales para hacerlo son:</span><span class="sxs-lookup"><span data-stu-id="20bae-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="20bae-209">Para restringir las respuestas de API web a un tipo de medio determinado.</span><span class="sxs-lookup"><span data-stu-id="20bae-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="20bae-210">Por ejemplo, podría decidir admitir sólo las respuestas JSON y quitar al formateador XML.</span><span class="sxs-lookup"><span data-stu-id="20bae-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="20bae-211">Para reemplazar al formateador predeterminado con un formateador personalizado.</span><span class="sxs-lookup"><span data-stu-id="20bae-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="20bae-212">Por ejemplo, podría reemplazar al formateador JSON con su propia implementación personalizada de un formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="20bae-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="20bae-213">El código siguiente muestra cómo quitar a los formateadores de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="20bae-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="20bae-214">Llamar a este método desde su **aplicación\_iniciar** método, definido en Global.asax.</span><span class="sxs-lookup"><span data-stu-id="20bae-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="20bae-215">Control de referencias circulares a objetos</span><span class="sxs-lookup"><span data-stu-id="20bae-215">Handling Circular Object References</span></span>

<span data-ttu-id="20bae-216">De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="20bae-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="20bae-217">Si dos propiedades que hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces.</span><span class="sxs-lookup"><span data-stu-id="20bae-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="20bae-218">Esto es un problema determinado si el gráfico de objetos contiene ciclos, porque el serializador producirá una excepción cuando detecta un bucle en el gráfico.</span><span class="sxs-lookup"><span data-stu-id="20bae-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="20bae-219">Tenga en cuenta los siguientes modelos de objetos y el controlador.</span><span class="sxs-lookup"><span data-stu-id="20bae-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="20bae-220">Invocar esta acción hará que al formateador que se produce una excepción, que se traduce en un estado código 500 (Error interno del servidor) de respuesta al cliente.</span><span class="sxs-lookup"><span data-stu-id="20bae-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="20bae-221">Para conservar las referencias de objeto en JSON, agregue el código siguiente al **aplicación\_iniciar** método en el archivo Global.asax:</span><span class="sxs-lookup"><span data-stu-id="20bae-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="20bae-222">Ahora, la acción de controlador devolverá JSON que tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="20bae-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="20bae-223">Tenga en cuenta que el serializador agrega un &quot;$id&quot; propiedad a ambos objetos.</span><span class="sxs-lookup"><span data-stu-id="20bae-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="20bae-224">Además, detecta que la propiedad Employee.Department crea un bucle, por lo que reemplaza el valor con una referencia de objeto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="20bae-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="20bae-225">Las referencias a objetos no son estándar en JSON.</span><span class="sxs-lookup"><span data-stu-id="20bae-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="20bae-226">Antes de usar esta característica, considere si los clientes podrán analizar los resultados.</span><span class="sxs-lookup"><span data-stu-id="20bae-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="20bae-227">Podría ser preferible simplemente quitar ciclos del gráfico.</span><span class="sxs-lookup"><span data-stu-id="20bae-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="20bae-228">Por ejemplo, el vínculo desde el empleado al departamento no es necesario realmente en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="20bae-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="20bae-229">Para conservar las referencias de objeto en XML, tiene dos opciones.</span><span class="sxs-lookup"><span data-stu-id="20bae-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="20bae-230">La opción más sencilla consiste en agregar `[DataContract(IsReference=true)]` a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="20bae-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="20bae-231">El *IsReference* parámetro permite referencias a objetos.</span><span class="sxs-lookup"><span data-stu-id="20bae-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="20bae-232">Recuerde que **DataContract** hace serialización participar en, por lo que también deberá agregar **DataMember** atributos a las propiedades:</span><span class="sxs-lookup"><span data-stu-id="20bae-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="20bae-233">Ahora el formateador generará XML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="20bae-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="20bae-234">Si desea evitar atributos en la clase de modelo, hay otra opción: Crear un nuevo específicos del tipo **DataContractSerializer** de instancia y establecer *preserveObjectReferences* a **true** en el constructor.</span><span class="sxs-lookup"><span data-stu-id="20bae-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="20bae-235">A continuación, establezca esta instancia como un serializador por tipo en el formateador de tipo de medio XML.</span><span class="sxs-lookup"><span data-stu-id="20bae-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="20bae-236">El código siguiente se muestra cómo hacerlo:</span><span class="sxs-lookup"><span data-stu-id="20bae-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="20bae-237">Serialización de objetos de prueba</span><span class="sxs-lookup"><span data-stu-id="20bae-237">Testing Object Serialization</span></span>

<span data-ttu-id="20bae-238">Al diseñar la API web, es útil probar cómo se va a serializar los objetos de datos.</span><span class="sxs-lookup"><span data-stu-id="20bae-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="20bae-239">Puede hacerlo sin crear un controlador o invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="20bae-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
