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
# <a name="json-and-xml-serialization-in-aspnet-web-api"></a><span data-ttu-id="090f5-103">Serialización de JSON y XML en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="090f5-103">JSON and XML Serialization in ASP.NET Web API</span></span>

<span data-ttu-id="090f5-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="090f5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="090f5-105">En este artículo se describen los formateadores JSON y XML en ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="090f5-105">This article describes the JSON and XML formatters in ASP.NET Web API.</span></span>

<span data-ttu-id="090f5-106">En ASP.NET Web API, un *formateador de tipo de medio* es un objeto que puede:</span><span class="sxs-lookup"><span data-stu-id="090f5-106">In ASP.NET Web API, a *media-type formatter* is an object that can:</span></span>

- <span data-ttu-id="090f5-107">Leer objetos CLR de un cuerpo de mensaje HTTP</span><span class="sxs-lookup"><span data-stu-id="090f5-107">Read CLR objects from an HTTP message body</span></span>
- <span data-ttu-id="090f5-108">Escribir objetos CLR en el cuerpo de un mensaje HTTP</span><span class="sxs-lookup"><span data-stu-id="090f5-108">Write CLR objects into an HTTP message body</span></span>

<span data-ttu-id="090f5-109">Web API proporciona formateadores de tipo de medio para JSON y XML.</span><span class="sxs-lookup"><span data-stu-id="090f5-109">Web API provides media-type formatters for both JSON and XML.</span></span> <span data-ttu-id="090f5-110">El marco de trabajo inserta estos formateadores en la canalización de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="090f5-110">The framework inserts these formatters into the pipeline by default.</span></span> <span data-ttu-id="090f5-111">Los clientes pueden solicitar JSON o XML en el encabezado Accept de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="090f5-111">Clients can request either JSON or XML in the Accept header of the HTTP request.</span></span>

## <a name="contents"></a><span data-ttu-id="090f5-112">Contenido</span><span class="sxs-lookup"><span data-stu-id="090f5-112">Contents</span></span>

- [<span data-ttu-id="090f5-113">Formateador de tipo de medio JSON</span><span class="sxs-lookup"><span data-stu-id="090f5-113">JSON Media-Type Formatter</span></span>](#json_media_type_formatter)

    - [<span data-ttu-id="090f5-114">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="090f5-114">Read-Only Properties</span></span>](#json_readonly)
    - [<span data-ttu-id="090f5-115">Días</span><span class="sxs-lookup"><span data-stu-id="090f5-115">Dates</span></span>](#json_dates)
    - [<span data-ttu-id="090f5-116">Sangría</span><span class="sxs-lookup"><span data-stu-id="090f5-116">Indenting</span></span>](#json_indenting)
    - [<span data-ttu-id="090f5-117">Grafía Camel</span><span class="sxs-lookup"><span data-stu-id="090f5-117">Camel Casing</span></span>](#json_camelcasing)
    - [<span data-ttu-id="090f5-118">Objetos anónimos y con establecimiento flexible de tipos</span><span class="sxs-lookup"><span data-stu-id="090f5-118">Anonymous and Weakly-Typed Objects</span></span>](#json_anon)
- [<span data-ttu-id="090f5-119">Formateador de tipo de medio XML</span><span class="sxs-lookup"><span data-stu-id="090f5-119">XML Media-Type Formatter</span></span>](#xml_media_type_formatter)

    - [<span data-ttu-id="090f5-120">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="090f5-120">Read-Only Properties</span></span>](#xml_readonly)
    - [<span data-ttu-id="090f5-121">Días</span><span class="sxs-lookup"><span data-stu-id="090f5-121">Dates</span></span>](#xml_dates)
    - [<span data-ttu-id="090f5-122">Sangría</span><span class="sxs-lookup"><span data-stu-id="090f5-122">Indenting</span></span>](#xml_indenting)
    - [<span data-ttu-id="090f5-123">Establecer serializadores XML por tipo</span><span class="sxs-lookup"><span data-stu-id="090f5-123">Setting Per-Type XML Serializers</span></span>](#xml_pertype)
- [<span data-ttu-id="090f5-124">Quitar el formateador JSON o XML</span><span class="sxs-lookup"><span data-stu-id="090f5-124">Removing the JSON or XML Formatter</span></span>](#removing_the_json_or_xml_formatter)
- [<span data-ttu-id="090f5-125">Controlar referencias circulares de objetos</span><span class="sxs-lookup"><span data-stu-id="090f5-125">Handling Circular Object References</span></span>](#handling_circular_object_references)
- [<span data-ttu-id="090f5-126">Probar la serialización de objetos</span><span class="sxs-lookup"><span data-stu-id="090f5-126">Testing Object Serialization</span></span>](#testing_object_serialization)

<a id="json_media_type_formatter"></a>
## <a name="json-media-type-formatter"></a><span data-ttu-id="090f5-127">Formateador de tipo de medio JSON</span><span class="sxs-lookup"><span data-stu-id="090f5-127">JSON Media-Type Formatter</span></span>

<span data-ttu-id="090f5-128">La clase **JsonMediaTypeFormatter** proporciona formato JSON.</span><span class="sxs-lookup"><span data-stu-id="090f5-128">JSON formatting is provided by the **JsonMediaTypeFormatter** class.</span></span> <span data-ttu-id="090f5-129">De forma predeterminada, **JsonMediaTypeFormatter** usa la biblioteca [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="090f5-129">By default, **JsonMediaTypeFormatter** uses the [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) library to perform serialization.</span></span> <span data-ttu-id="090f5-130">Json.NET es un proyecto de código abierto de terceros.</span><span class="sxs-lookup"><span data-stu-id="090f5-130">Json.NET is a third-party open source project.</span></span>

<span data-ttu-id="090f5-131">Si lo prefiere, puede configurar la clase **JsonMediaTypeFormatter** para usar **DataContractJsonSerializer** en lugar de JSON.net.</span><span class="sxs-lookup"><span data-stu-id="090f5-131">If you prefer, you can configure the **JsonMediaTypeFormatter** class to use the **DataContractJsonSerializer** instead of Json.NET.</span></span> <span data-ttu-id="090f5-132">Para ello, establezca la propiedad **UseDataContractJsonSerializer** en **true**:</span><span class="sxs-lookup"><span data-stu-id="090f5-132">To do so, set the **UseDataContractJsonSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample1.cs)]

### <a name="json-serialization"></a><span data-ttu-id="090f5-133">Serialización de JSON</span><span class="sxs-lookup"><span data-stu-id="090f5-133">JSON Serialization</span></span>

<span data-ttu-id="090f5-134">En esta sección se describen algunos comportamientos específicos del formateador JSON mediante el serializador [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) predeterminado.</span><span class="sxs-lookup"><span data-stu-id="090f5-134">This section describes some specific behaviors of the JSON formatter, using the default [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) serializer.</span></span> <span data-ttu-id="090f5-135">No pretende ser documentación completa de la biblioteca Json.NET; para obtener más información, vea la [documentación de JSON.net](http://james.newtonking.com/projects/json/help/).</span><span class="sxs-lookup"><span data-stu-id="090f5-135">This is not meant to be comprehensive documentation of the Json.NET library; for more information, see the [Json.NET Documentation](http://james.newtonking.com/projects/json/help/).</span></span>

#### <a name="what-gets-serialized"></a><span data-ttu-id="090f5-136">¿Qué se serializa?</span><span class="sxs-lookup"><span data-stu-id="090f5-136">What Gets Serialized?</span></span>

<span data-ttu-id="090f5-137">De forma predeterminada, todas las propiedades y campos públicos se incluyen en el JSON serializado.</span><span class="sxs-lookup"><span data-stu-id="090f5-137">By default, all public properties and fields are included in the serialized JSON.</span></span> <span data-ttu-id="090f5-138">Para omitir una propiedad o un campo, decorarlo con el atributo **JsonIgnore** .</span><span class="sxs-lookup"><span data-stu-id="090f5-138">To omit a property or field, decorate it with the **JsonIgnore** attribute.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample2.cs)]

<span data-ttu-id="090f5-139">Si prefiere un enfoque de&quot; de &quot;participar, decore la clase con el atributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="090f5-139">If you prefer an &quot;opt-in&quot; approach, decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="090f5-140">Si este atributo está presente, se omiten los miembros a menos que tengan **DataMember**.</span><span class="sxs-lookup"><span data-stu-id="090f5-140">If this attribute is present, members are ignored unless they have the **DataMember**.</span></span> <span data-ttu-id="090f5-141">También puede usar **DataMember** para serializar miembros privados.</span><span class="sxs-lookup"><span data-stu-id="090f5-141">You can also use **DataMember** to serialize private members.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample3.cs)]

<a id="json_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="090f5-142">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="090f5-142">Read-Only Properties</span></span>

<span data-ttu-id="090f5-143">De forma predeterminada, las propiedades de solo lectura se serializan.</span><span class="sxs-lookup"><span data-stu-id="090f5-143">Read-only properties are serialized by default.</span></span>

<a id="json_dates"></a>
### <a name="dates"></a><span data-ttu-id="090f5-144">Fechas</span><span class="sxs-lookup"><span data-stu-id="090f5-144">Dates</span></span>

<span data-ttu-id="090f5-145">De forma predeterminada, Json.NET escribe fechas en formato [ISO 8601](http://www.w3.org/TR/NOTE-datetime) .</span><span class="sxs-lookup"><span data-stu-id="090f5-145">By default, Json.NET writes dates in [ISO 8601](http://www.w3.org/TR/NOTE-datetime) format.</span></span> <span data-ttu-id="090f5-146">Las fechas en UTC (hora universal coordinada) se escriben con un sufijo "Z".</span><span class="sxs-lookup"><span data-stu-id="090f5-146">Dates in UTC (Coordinated Universal Time) are written with a "Z" suffix.</span></span> <span data-ttu-id="090f5-147">Las fechas en hora local incluyen un ajuste de zona horaria.</span><span class="sxs-lookup"><span data-stu-id="090f5-147">Dates in local time include a time-zone offset.</span></span> <span data-ttu-id="090f5-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="090f5-148">For example:</span></span>

[!code-console[Main](json-and-xml-serialization/samples/sample4.cmd)]

<span data-ttu-id="090f5-149">De forma predeterminada, Json.NET conserva la zona horaria.</span><span class="sxs-lookup"><span data-stu-id="090f5-149">By default, Json.NET preserves the time zone.</span></span> <span data-ttu-id="090f5-150">Puede invalidar esto estableciendo la propiedad DateTimeZoneHandling:</span><span class="sxs-lookup"><span data-stu-id="090f5-150">You can override this by setting the DateTimeZoneHandling property:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample5.cs)]

<span data-ttu-id="090f5-151">Si prefiere usar el formato de fecha (`"\/Date(ticks)\/"`) [JSON de Microsoft](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) en lugar de ISO 8601, establezca la propiedad **DateFormatHandling** en la configuración del serializador:</span><span class="sxs-lookup"><span data-stu-id="090f5-151">If you prefer to use [Microsoft JSON date format](https://msdn.microsoft.com/library/bb299886.aspx#intro_to_json_sidebarb) (`"\/Date(ticks)\/"`) instead of ISO 8601, set the **DateFormatHandling** property on the serializer settings:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample6.cs)]

<a id="json_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="090f5-152">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="090f5-152">Indenting</span></span>

<span data-ttu-id="090f5-153">Para escribir JSON con sangría, establezca la configuración de **formato** en **formato. sangría**:</span><span class="sxs-lookup"><span data-stu-id="090f5-153">To write indented JSON, set the **Formatting** setting to **Formatting.Indented**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample7.cs)]

<a id="json_camelcasing"></a>
### <a name="camel-casing"></a><span data-ttu-id="090f5-154">Grafía Camel</span><span class="sxs-lookup"><span data-stu-id="090f5-154">Camel Casing</span></span>

<span data-ttu-id="090f5-155">Para escribir nombres de propiedad JSON con grafía Camel, sin cambiar el modelo de datos, establezca **CamelCasePropertyNamesContractResolver** en el serializador:</span><span class="sxs-lookup"><span data-stu-id="090f5-155">To write JSON property names with camel casing, without changing your data model, set the **CamelCasePropertyNamesContractResolver** on the serializer:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample8.cs)]

<a id="json_anon"></a>
### <a name="anonymous-and-weakly-typed-objects"></a><span data-ttu-id="090f5-156">Objetos anónimos y con establecimiento flexible de tipos</span><span class="sxs-lookup"><span data-stu-id="090f5-156">Anonymous and Weakly-Typed Objects</span></span>

<span data-ttu-id="090f5-157">Un método de acción puede devolver un objeto anónimo y serializarlo en JSON.</span><span class="sxs-lookup"><span data-stu-id="090f5-157">An action method can return an anonymous object and serialize it to JSON.</span></span> <span data-ttu-id="090f5-158">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="090f5-158">For example:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample9.cs)]

<span data-ttu-id="090f5-159">El cuerpo del mensaje de respuesta contendrá el siguiente JSON:</span><span class="sxs-lookup"><span data-stu-id="090f5-159">The response message body will contain the following JSON:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample10.json)]

<span data-ttu-id="090f5-160">Si la API Web recibe objetos JSON con estructura imprecisa de los clientes, puede deserializar el cuerpo de la solicitud en un tipo **Newtonsoft. JSON. Linq. JObject** .</span><span class="sxs-lookup"><span data-stu-id="090f5-160">If your web API receives loosely structured JSON objects from clients, you can deserialize the request body to a **Newtonsoft.Json.Linq.JObject** type.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample11.cs)]

<span data-ttu-id="090f5-161">Sin embargo, suele ser mejor utilizar objetos de datos fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="090f5-161">However, it is usually better to use strongly typed data objects.</span></span> <span data-ttu-id="090f5-162">No es necesario analizar los datos por sí mismo y se obtienen las ventajas de la validación del modelo.</span><span class="sxs-lookup"><span data-stu-id="090f5-162">Then you don't need to parse the data yourself, and you get the benefits of model validation.</span></span>

<span data-ttu-id="090f5-163">El serializador XML no admite tipos anónimos o instancias de **JObject** .</span><span class="sxs-lookup"><span data-stu-id="090f5-163">The XML serializer does not support anonymous types or **JObject** instances.</span></span> <span data-ttu-id="090f5-164">Si usa estas características para los datos JSON, debe quitar el formateador XML de la canalización, como se describe más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="090f5-164">If you use these features for your JSON data, you should remove the XML formatter from the pipeline, as described later in this article.</span></span>

<a id="xml_media_type_formatter"></a>
## <a name="xml-media-type-formatter"></a><span data-ttu-id="090f5-165">Formateador de tipo de medio XML</span><span class="sxs-lookup"><span data-stu-id="090f5-165">XML Media-Type Formatter</span></span>

<span data-ttu-id="090f5-166">La clase **XmlMediaTypeFormatter** proporciona el formato XML.</span><span class="sxs-lookup"><span data-stu-id="090f5-166">XML formatting is provided by the **XmlMediaTypeFormatter** class.</span></span> <span data-ttu-id="090f5-167">De forma predeterminada, **XmlMediaTypeFormatter** usa la clase **DataContractSerializer** para realizar la serialización.</span><span class="sxs-lookup"><span data-stu-id="090f5-167">By default, **XmlMediaTypeFormatter** uses the **DataContractSerializer** class to perform serialization.</span></span>

<span data-ttu-id="090f5-168">Si lo prefiere, puede configurar **XmlMediaTypeFormatter** para usar **XmlSerializer** en lugar de **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="090f5-168">If you prefer, you can configure the **XmlMediaTypeFormatter** to use the **XmlSerializer** instead of the **DataContractSerializer**.</span></span> <span data-ttu-id="090f5-169">Para ello, establezca la propiedad **UseXmlSerializer** en **true**:</span><span class="sxs-lookup"><span data-stu-id="090f5-169">To do so, set the **UseXmlSerializer** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample12.cs)]

<span data-ttu-id="090f5-170">La clase **XmlSerializer** admite un conjunto más estrecho de tipos que **DataContractSerializer**, pero proporciona más control sobre el XML resultante.</span><span class="sxs-lookup"><span data-stu-id="090f5-170">The **XmlSerializer** class supports a narrower set of types than **DataContractSerializer**, but gives more control over the resulting XML.</span></span> <span data-ttu-id="090f5-171">Considere la posibilidad de usar **XmlSerializer** si necesita hacer coincidir un esquema XML existente.</span><span class="sxs-lookup"><span data-stu-id="090f5-171">Consider using **XmlSerializer** if you need to match an existing XML schema.</span></span>

### <a name="xml-serialization"></a><span data-ttu-id="090f5-172">Serialización XML</span><span class="sxs-lookup"><span data-stu-id="090f5-172">XML Serialization</span></span>

<span data-ttu-id="090f5-173">En esta sección se describen algunos comportamientos específicos del formateador XML, utilizando el valor predeterminado **DataContractSerializer**.</span><span class="sxs-lookup"><span data-stu-id="090f5-173">This section describes some specific behaviors of the XML formatter, using the default **DataContractSerializer**.</span></span>

<span data-ttu-id="090f5-174">De forma predeterminada, DataContractSerializer se comporta de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="090f5-174">By default, the DataContractSerializer behaves as follows:</span></span>

- <span data-ttu-id="090f5-175">Se serializan todas las propiedades y campos públicos de lectura/escritura.</span><span class="sxs-lookup"><span data-stu-id="090f5-175">All public read/write properties and fields are serialized.</span></span> <span data-ttu-id="090f5-176">Para omitir una propiedad o un campo, decorarlo con el atributo **IgnoreDataMember** .</span><span class="sxs-lookup"><span data-stu-id="090f5-176">To omit a property or field, decorate it with the **IgnoreDataMember** attribute.</span></span>
- <span data-ttu-id="090f5-177">Los miembros privados y protegidos no se serializan.</span><span class="sxs-lookup"><span data-stu-id="090f5-177">Private and protected members are not serialized.</span></span>
- <span data-ttu-id="090f5-178">Las propiedades de solo lectura no se serializan.</span><span class="sxs-lookup"><span data-stu-id="090f5-178">Read-only properties are not serialized.</span></span> <span data-ttu-id="090f5-179">(Sin embargo, se serializa el contenido de una propiedad de colección de solo lectura).</span><span class="sxs-lookup"><span data-stu-id="090f5-179">(However, the contents of a read-only collection property are serialized.)</span></span>
- <span data-ttu-id="090f5-180">Los nombres de clase y miembro se escriben en el XML exactamente como aparecen en la declaración de clase.</span><span class="sxs-lookup"><span data-stu-id="090f5-180">Class and member names are written in the XML exactly as they appear in the class declaration.</span></span>
- <span data-ttu-id="090f5-181">Se utiliza un espacio de nombres XML predeterminado.</span><span class="sxs-lookup"><span data-stu-id="090f5-181">A default XML namespace is used.</span></span>

<span data-ttu-id="090f5-182">Si necesita más control sobre la serialización, puede decorar la clase con el atributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="090f5-182">If you need more control over the serialization, you can decorate the class with the **DataContract** attribute.</span></span> <span data-ttu-id="090f5-183">Cuando este atributo está presente, la clase se serializa como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="090f5-183">When this attribute is present, the class is serialized as follows:</span></span>

- <span data-ttu-id="090f5-184">&quot;participar&quot; enfoque: las propiedades y los campos no se serializan de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="090f5-184">&quot;Opt in&quot; approach: Properties and fields are not serialized by default.</span></span> <span data-ttu-id="090f5-185">Para serializar una propiedad o un campo, decorarlo con el atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="090f5-185">To serialize a property or field, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="090f5-186">Para serializar un miembro privado o protegido, decorarlo con el atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="090f5-186">To serialize a private or protected member, decorate it with the **DataMember** attribute.</span></span>
- <span data-ttu-id="090f5-187">Las propiedades de solo lectura no se serializan.</span><span class="sxs-lookup"><span data-stu-id="090f5-187">Read-only properties are not serialized.</span></span>
- <span data-ttu-id="090f5-188">Para cambiar el modo en que el nombre de clase aparece en el XML, establezca el parámetro *Name* en el atributo **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="090f5-188">To change how the class name appears in the XML, set the *Name* parameter in the **DataContract** attribute.</span></span>
- <span data-ttu-id="090f5-189">Para cambiar el modo en que aparece un nombre de miembro en el XML, establezca el parámetro *Name* en el atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="090f5-189">To change how a member name appears in the XML, set the *Name* parameter in the **DataMember** attribute.</span></span>
- <span data-ttu-id="090f5-190">Para cambiar el espacio de nombres XML, establezca el parámetro *namespace* en la clase **DataContract** .</span><span class="sxs-lookup"><span data-stu-id="090f5-190">To change the XML namespace, set the *Namespace* parameter in the **DataContract** class.</span></span>

<a id="xml_readonly"></a>
### <a name="read-only-properties"></a><span data-ttu-id="090f5-191">Propiedades de solo lectura</span><span class="sxs-lookup"><span data-stu-id="090f5-191">Read-Only Properties</span></span>

<span data-ttu-id="090f5-192">Las propiedades de solo lectura no se serializan.</span><span class="sxs-lookup"><span data-stu-id="090f5-192">Read-only properties are not serialized.</span></span> <span data-ttu-id="090f5-193">Si una propiedad de solo lectura tiene un campo privado de respaldo, puede marcar el campo privado con el atributo **DataMember** .</span><span class="sxs-lookup"><span data-stu-id="090f5-193">If a read-only property has a backing private field, you can mark the private field with the **DataMember** attribute.</span></span> <span data-ttu-id="090f5-194">Este enfoque requiere el atributo **DataContract** en la clase.</span><span class="sxs-lookup"><span data-stu-id="090f5-194">This approach requires the **DataContract** attribute on the class.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample13.cs)]

<a id="xml_dates"></a>
### <a name="dates"></a><span data-ttu-id="090f5-195">Fechas</span><span class="sxs-lookup"><span data-stu-id="090f5-195">Dates</span></span>

<span data-ttu-id="090f5-196">Las fechas se escriben en formato ISO 8601.</span><span class="sxs-lookup"><span data-stu-id="090f5-196">Dates are written in ISO 8601 format.</span></span> <span data-ttu-id="090f5-197">Por ejemplo, &quot;2012-05-23T20:21:37.9116538 Z&quot;.</span><span class="sxs-lookup"><span data-stu-id="090f5-197">For example, &quot;2012-05-23T20:21:37.9116538Z&quot;.</span></span>

<a id="xml_indenting"></a>
### <a name="indenting"></a><span data-ttu-id="090f5-198">Aplicar sangría</span><span class="sxs-lookup"><span data-stu-id="090f5-198">Indenting</span></span>

<span data-ttu-id="090f5-199">Para escribir XML con sangría, establezca la propiedad **indent** en **true**:</span><span class="sxs-lookup"><span data-stu-id="090f5-199">To write indented XML, set the **Indent** property to **true**:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample14.cs)]

<a id="xml_pertype"></a>
## <a name="setting-per-type-xml-serializers"></a><span data-ttu-id="090f5-200">Establecer serializadores XML por tipo</span><span class="sxs-lookup"><span data-stu-id="090f5-200">Setting Per-Type XML Serializers</span></span>

<span data-ttu-id="090f5-201">Puede establecer diferentes serializadores XML para diferentes tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="090f5-201">You can set different XML serializers for different CLR types.</span></span> <span data-ttu-id="090f5-202">Por ejemplo, podría tener un objeto de datos determinado que requiera **XmlSerializer** para la compatibilidad con versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="090f5-202">For example, you might have a particular data object that requires **XmlSerializer** for backward compatibility.</span></span> <span data-ttu-id="090f5-203">Puede usar **XmlSerializer** para este objeto y seguir usando **DataContractSerializer** para otros tipos.</span><span class="sxs-lookup"><span data-stu-id="090f5-203">You can use **XmlSerializer** for this object and continue to use **DataContractSerializer** for other types.</span></span>

<span data-ttu-id="090f5-204">Para establecer un serializador XML para un tipo determinado, llame a **SetSerializer**.</span><span class="sxs-lookup"><span data-stu-id="090f5-204">To set an XML serializer for a particular type, call **SetSerializer**.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample15.cs)]

<span data-ttu-id="090f5-205">Puede especificar un **XmlSerializer** o cualquier objeto que se derive de **XmlObjectSerializer**.</span><span class="sxs-lookup"><span data-stu-id="090f5-205">You can specify an **XmlSerializer** or any object that derives from **XmlObjectSerializer**.</span></span>

<a id="removing_the_json_or_xml_formatter"></a>
## <a name="removing-the-json-or-xml-formatter"></a><span data-ttu-id="090f5-206">Quitar el formateador JSON o XML</span><span class="sxs-lookup"><span data-stu-id="090f5-206">Removing the JSON or XML Formatter</span></span>

<span data-ttu-id="090f5-207">Puede quitar el formateador JSON o el formateador XML de la lista de formateadores, si no desea utilizarlos.</span><span class="sxs-lookup"><span data-stu-id="090f5-207">You can remove the JSON formatter or the XML formatter from the list of formatters, if you do not want to use them.</span></span> <span data-ttu-id="090f5-208">Las principales razones para ello son:</span><span class="sxs-lookup"><span data-stu-id="090f5-208">The main reasons to do this are:</span></span>

- <span data-ttu-id="090f5-209">Para restringir las respuestas de la API Web a un tipo de medio determinado.</span><span class="sxs-lookup"><span data-stu-id="090f5-209">To restrict your web API responses to a particular media type.</span></span> <span data-ttu-id="090f5-210">Por ejemplo, puede decidir admitir solo las respuestas JSON y quitar el formateador XML.</span><span class="sxs-lookup"><span data-stu-id="090f5-210">For example, you might decide to support only JSON responses, and remove the XML formatter.</span></span>
- <span data-ttu-id="090f5-211">Es para reemplazar el formateador predeterminado por un formateador personalizado.</span><span class="sxs-lookup"><span data-stu-id="090f5-211">To replace the default formatter with a custom formatter.</span></span> <span data-ttu-id="090f5-212">Por ejemplo, podría reemplazar el formateador JSON por su propia implementación personalizada de un formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="090f5-212">For example, you could replace the JSON formatter with your own custom implementation of a JSON formatter.</span></span>

<span data-ttu-id="090f5-213">En el código siguiente se muestra cómo quitar los formateadores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="090f5-213">The following code shows how to remove the default formatters.</span></span> <span data-ttu-id="090f5-214">Llame a este desde el método de **Inicio de\_** de la aplicación, definido en global. asax.</span><span class="sxs-lookup"><span data-stu-id="090f5-214">Call this from your **Application\_Start** method, defined in Global.asax.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample16.cs)]

<a id="handling_circular_object_references"></a>
## <a name="handling-circular-object-references"></a><span data-ttu-id="090f5-215">Controlar referencias circulares de objetos</span><span class="sxs-lookup"><span data-stu-id="090f5-215">Handling Circular Object References</span></span>

<span data-ttu-id="090f5-216">De forma predeterminada, los formateadores JSON y XML escriben todos los objetos como valores.</span><span class="sxs-lookup"><span data-stu-id="090f5-216">By default, the JSON and XML formatters write all objects as values.</span></span> <span data-ttu-id="090f5-217">Si dos propiedades hacen referencia al mismo objeto, o si el mismo objeto aparece dos veces en una colección, el formateador serializará el objeto dos veces.</span><span class="sxs-lookup"><span data-stu-id="090f5-217">If two properties refer to the same object, or if the same object appears twice in a collection, the formatter will serialize the object twice.</span></span> <span data-ttu-id="090f5-218">Se trata de un problema determinado si el gráfico de objetos contiene ciclos, ya que el serializador producirá una excepción cuando detecte un bucle en el gráfico.</span><span class="sxs-lookup"><span data-stu-id="090f5-218">This is a particular problem if your object graph contains cycles, because the serializer will throw an exception when it detects a loop in the graph.</span></span>

<span data-ttu-id="090f5-219">Tenga en cuenta los siguientes modelos de objetos y controladores.</span><span class="sxs-lookup"><span data-stu-id="090f5-219">Consider the following object models and controller.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample17.cs)]

<span data-ttu-id="090f5-220">Al invocar esta acción, el formateador producirá una excepción, lo que se traduce en una respuesta de código de estado 500 (error interno del servidor) al cliente.</span><span class="sxs-lookup"><span data-stu-id="090f5-220">Invoking this action will cause the formatter to thrown an exception, which translates to a status code 500 (Internal Server Error) response to the client.</span></span>

<span data-ttu-id="090f5-221">Para conservar las referencias de objeto en JSON, agregue el siguiente código al método de **Inicio de\_** de la aplicación en el archivo global. asax:</span><span class="sxs-lookup"><span data-stu-id="090f5-221">To preserve object references in JSON, add the following code to **Application\_Start** method in the Global.asax file:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample18.cs)]

<span data-ttu-id="090f5-222">Ahora, la acción del controlador devolverá JSON que tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="090f5-222">Now the controller action will return JSON that looks like this:</span></span>

[!code-json[Main](json-and-xml-serialization/samples/sample19.json)]

<span data-ttu-id="090f5-223">Observe que el serializador agrega una &quot;$id propiedad&quot; a ambos objetos.</span><span class="sxs-lookup"><span data-stu-id="090f5-223">Notice that the serializer adds an &quot;$id&quot; property to both objects.</span></span> <span data-ttu-id="090f5-224">Además, detecta que la propiedad Employee. Department crea un bucle, de modo que reemplaza el valor por una referencia de objeto: {&quot;$ref&quot;:&quot;1&quot;}.</span><span class="sxs-lookup"><span data-stu-id="090f5-224">Also, it detects that the Employee.Department property creates a loop, so it replaces the value with an object reference: {&quot;$ref&quot;:&quot;1&quot;}.</span></span>

> [!NOTE]
> <span data-ttu-id="090f5-225">Las referencias de objeto no son estándar en JSON.</span><span class="sxs-lookup"><span data-stu-id="090f5-225">Object references are not standard in JSON.</span></span> <span data-ttu-id="090f5-226">Antes de usar esta característica, tenga en cuenta si los clientes podrán analizar los resultados.</span><span class="sxs-lookup"><span data-stu-id="090f5-226">Before using this feature, consider whether your clients will be able to parse the results.</span></span> <span data-ttu-id="090f5-227">Podría ser más fácil quitar ciclos del gráfico.</span><span class="sxs-lookup"><span data-stu-id="090f5-227">It might be better simply to remove cycles from the graph.</span></span> <span data-ttu-id="090f5-228">Por ejemplo, en este ejemplo no se necesita realmente el vínculo del empleado al Departamento.</span><span class="sxs-lookup"><span data-stu-id="090f5-228">For example, the link from Employee back to Department is not really needed in this example.</span></span>

<span data-ttu-id="090f5-229">Para conservar las referencias de objeto en XML, tiene dos opciones.</span><span class="sxs-lookup"><span data-stu-id="090f5-229">To preserve object references in XML, you have two options.</span></span> <span data-ttu-id="090f5-230">La opción más sencilla es agregar `[DataContract(IsReference=true)]` a la clase de modelo.</span><span class="sxs-lookup"><span data-stu-id="090f5-230">The simpler option is to add `[DataContract(IsReference=true)]` to your model class.</span></span> <span data-ttu-id="090f5-231">El parámetro *IsReference* habilita las referencias a objetos.</span><span class="sxs-lookup"><span data-stu-id="090f5-231">The *IsReference* parameter enables object references.</span></span> <span data-ttu-id="090f5-232">Recuerde que **DataContract** hace que se opte por la serialización, por lo que también tendrá que agregar atributos **DataMember** a las propiedades:</span><span class="sxs-lookup"><span data-stu-id="090f5-232">Remember that **DataContract** makes serialization opt-in, so you will also need to add **DataMember** attributes to the properties:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample20.cs)]

<span data-ttu-id="090f5-233">Ahora, el formateador generará código XML similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="090f5-233">Now the formatter will produce XML similar to following:</span></span>

[!code-xml[Main](json-and-xml-serialization/samples/sample21.xml)]

<span data-ttu-id="090f5-234">Si desea evitar atributos en la clase del modelo, hay otra opción: crear una nueva instancia de **DataContractSerializer** específica del tipo y establecer *preserveObjectReferences* en **true** en el constructor.</span><span class="sxs-lookup"><span data-stu-id="090f5-234">If you want to avoid attributes on your model class, there is another option: Create a new type-specific **DataContractSerializer** instance and set *preserveObjectReferences* to **true** in the constructor.</span></span> <span data-ttu-id="090f5-235">Después, establezca esta instancia como un serializador por tipo en el formateador de tipo de medio XML.</span><span class="sxs-lookup"><span data-stu-id="090f5-235">Then set this instance as a per-type serializer on the XML media-type formatter.</span></span> <span data-ttu-id="090f5-236">En el código siguiente se muestra cómo hacerlo:</span><span class="sxs-lookup"><span data-stu-id="090f5-236">The following code show how to do this:</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample22.cs?highlight=3)]

<a id="testing_object_serialization"></a>
## <a name="testing-object-serialization"></a><span data-ttu-id="090f5-237">Probar la serialización de objetos</span><span class="sxs-lookup"><span data-stu-id="090f5-237">Testing Object Serialization</span></span>

<span data-ttu-id="090f5-238">Al diseñar la API Web, resulta útil probar cómo se serializan los objetos de datos.</span><span class="sxs-lookup"><span data-stu-id="090f5-238">As you design your web API, it is useful to test how your data objects will be serialized.</span></span> <span data-ttu-id="090f5-239">Puede hacerlo sin crear un controlador o invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="090f5-239">You can do this without creating a controller or invoking a controller action.</span></span>

[!code-csharp[Main](json-and-xml-serialization/samples/sample23.cs)]
