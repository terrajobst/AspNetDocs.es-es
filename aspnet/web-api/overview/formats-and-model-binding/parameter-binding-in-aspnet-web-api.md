---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Enlace de parámetros en ASP.NET Web API ASP.NET 4. x
author: MikeWasson
description: Describe el modo en que la API Web enlaza los parámetros y cómo personalizar el proceso de enlace en ASP.NET 4. x.
ms.author: riande
ms.date: 07/11/2013
ms.custom: seoapril2019
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 464cb9b45dc0b62c4da38b7cf612934808854d32
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448897"
---
# <a name="parameter-binding-in-aspnet-web-api"></a>Enlace de parámetros en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

[!INCLUDE[](~/includes/coreWebAPI.md)]

En este artículo se describe cómo la API Web enlaza parámetros y cómo se puede personalizar el proceso de enlace. Cuando la API Web llama a un método en un controlador, debe establecer valores para los parámetros, un proceso llamado *enlace*.

De forma predeterminada, Web API usa las siguientes reglas para enlazar parámetros:

- Si el parámetro es un tipo "simple", la API Web intenta obtener el valor del URI. Los tipos simples incluyen los [tipos primitivos](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) de .net (**int**, **bool**, **Double**, etc.), más **TimeSpan**, **DateTime**, **GUID**, **decimal**y **String**, *además* de cualquier tipo con un convertidor de tipos que puede convertir de una cadena. (Más información sobre los convertidores de tipos más adelante).
- En el caso de los tipos complejos, la API Web intenta leer el valor del cuerpo del mensaje mediante un [formateador de tipo de medio](media-formatters.md).

Por ejemplo, a continuación se muestra un método de controlador de API Web típico:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

El parámetro *ID* es un &quot;tipo de&quot; simple, por lo que la API Web intenta obtener el valor del URI de solicitud. El parámetro *Item* es un tipo complejo, por lo que la API Web usa un formateador de tipo de medio para leer el valor del cuerpo de la solicitud.

Para obtener un valor del URI, la API Web busca en los datos de ruta y en la cadena de consulta de URI. Los datos de ruta se rellenan cuando el sistema de enrutamiento analiza el URI y lo hace coincidir con una ruta. Para obtener más información, consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md).

En el resto de este artículo, mostraré cómo puede personalizar el proceso de enlace de modelos. Sin embargo, para los tipos complejos, considere la posibilidad de usar formateadores de tipo de medio siempre que sea posible. Un principio clave de HTTP es que los recursos se envían en el cuerpo del mensaje, mediante la negociación de contenido para especificar la representación del recurso. Los formateadores de tipo de medio se diseñaron para este fin exactamente.

## <a name="using-fromuri"></a>Usar [FromUri]

Para forzar a la API Web a leer un tipo complejo del URI, agregue el atributo **[FromUri]** al parámetro. En el ejemplo siguiente se define un tipo de `GeoPoint`, junto con un método de controlador que obtiene el `GeoPoint` del URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

El cliente puede poner los valores de latitud y longitud en la cadena de consulta y la API Web los usará para construir un `GeoPoint`. Por ejemplo:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Usar [FromBody]

Para forzar a la API Web a leer un tipo simple del cuerpo de la solicitud, agregue el atributo **[FromBody]** al parámetro:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

En este ejemplo, Web API usará un formateador de tipo de medio para leer el valor del *nombre* del cuerpo de la solicitud. Este es un ejemplo de solicitud de cliente.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Cuando un parámetro tiene [FromBody], la API Web usa el encabezado Content-Type para seleccionar un formateador. En este ejemplo, el tipo de contenido es &quot;aplicación/&quot; JSON y el cuerpo de la solicitud es una cadena JSON sin formato (no un objeto JSON).

A lo sumo, se permite que un parámetro lea desde el cuerpo del mensaje. Por lo tanto, esto no funcionará:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

La razón de esta regla es que el cuerpo de la solicitud puede almacenarse en un flujo no almacenado en búfer que solo se puede leer una vez.

## <a name="type-converters"></a>Convertidores de tipos

Puede hacer que Web API trate una clase como un tipo simple (para que la API Web intente enlazarla desde el URI) mediante la creación de un **TypeConverter** y la conversión de una cadena.

En el código siguiente se muestra una `GeoPoint` clase que representa un punto geográfico, más un **TypeConverter** que convierte cadenas en instancias de `GeoPoint`. La clase `GeoPoint` está decorada con un atributo **[TypeConverter]** para especificar el convertidor de tipos. (Este ejemplo fue inspirado en la entrada de blog de Mike retrete [cómo enlazar a objetos personalizados en firmas de acción en MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)).

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Ahora, la API web tratará `GeoPoint` como un tipo simple, lo que significa que intentará enlazar parámetros de `GeoPoint` desde el URI. No es necesario incluir **[FromUri]** en el parámetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

El cliente puede invocar el método con un URI como este:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Enlazadores modelo

Una opción más flexible que un convertidor de tipos es crear un enlazador de modelos personalizado. Con un enlazador de modelos, tiene acceso a aspectos como la solicitud HTTP, la descripción de la acción y los valores sin formato de los datos de ruta.

Para crear un enlazador de modelos, implemente la interfaz **IModelBinder** . Esta interfaz define un método único, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Este es un enlazador de modelos para objetos de `GeoPoint`.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Un enlazador de modelos obtiene los valores de entrada sin formato de un *proveedor de valores*. Este diseño separa dos funciones distintas:

- El proveedor de valores toma la solicitud HTTP y rellena un diccionario de pares clave-valor.
- El enlazador de modelos usa este diccionario para rellenar el modelo.

El proveedor de valor predeterminado en Web API obtiene los valores de los datos de ruta y la cadena de consulta. Por ejemplo, si el URI es `http://localhost/api/values/1?location=48,-122`, el proveedor de valores crea los siguientes pares de clave-valor:

- ID = &quot;1&quot;
- Location = &quot;48.122&quot;

(Supongamos que la plantilla de ruta predeterminada, que es &quot;API/{Controller}/{ID}&quot;).

El nombre del parámetro que se va a enlazar se almacena en la propiedad **ModelBindingContext. modelname** . El enlazador de modelos busca una clave con este valor en el diccionario. Si el valor existe y se puede convertir en un `GeoPoint`, el enlazador de modelos asigna el valor enlazado a la propiedad **ModelBindingContext. Model** .

Observe que el enlazador de modelos no se limita a una conversión de tipo simple. En este ejemplo, el enlazador de modelos busca primero en una tabla de ubicaciones conocidas y, si se produce un error, usa la conversión de tipos.

**Establecer el enlazador de modelos**

Hay varias maneras de establecer un enlazador de modelos. En primer lugar, puede Agregar un atributo **[ModelBinder]** al parámetro.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

También puede Agregar un atributo **[ModelBinder]** al tipo. Web API usará el enlazador de modelos especificado para todos los parámetros de ese tipo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Por último, puede Agregar un proveedor de enlazador de modelos a **HttpConfiguration**. Un proveedor de enlazador de modelos es simplemente una clase de generador que crea un enlazador de modelos. Puede crear un proveedor derivando de la clase [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) . Sin embargo, si el enlazador de modelos controla un solo tipo, es más fácil usar el **SimpleModelBinderProvider**integrado, que está diseñado para este fin. El código siguiente muestra cómo hacerlo.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Con un proveedor de enlace de modelos, todavía necesita agregar el atributo **[ModelBinder]** al parámetro para indicar a la API Web que debe usar un enlazador de modelos y no un formateador de tipo de medio. Pero ahora no es necesario especificar el tipo de enlazador de modelos en el atributo:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Proveedores de valores

He mencionado que un enlazador de modelos obtiene valores de un proveedor de valores. Para escribir un proveedor de valores personalizado, implemente la interfaz **IValueProvider** . Este es un ejemplo que extrae los valores de las cookies de la solicitud:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

También debe crear un generador de proveedores de valores derivando de la clase **ValueProviderFactory** .

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Agregue el generador de proveedores de valores a **HttpConfiguration** como se indica a continuación.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

La API Web crea todos los proveedores de valores, por lo que cuando un enlazador de modelos llama a **ValueProvider. GetValue**, el enlazador de modelos recibe el valor del primer proveedor de valor que puede generarlo.

Como alternativa, puede establecer el generador de proveedores de valores en el nivel de parámetro mediante el atributo **ValueProvider** , como se indica a continuación:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Esto indica a la API Web que use el enlace de modelos con el generador de proveedores de valores especificado y que no use ninguno de los demás proveedores de valores registrados.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Los enlazadores de modelos son una instancia específica de un mecanismo más general. Si observa el atributo **[ModelBinder]** , verá que se deriva de la clase abstracta **ParameterBindingAttribute** . Esta clase define un método único, **GetBinding**, que devuelve un objeto **HttpParameterBinding** :

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Un **HttpParameterBinding** es responsable de enlazar un parámetro a un valor. En el caso de **[ModelBinder]** , el atributo devuelve una implementación de **HttpParameterBinding** que usa un **IModelBinder** para realizar el enlace real. También puede implementar su propio **HttpParameterBinding**.

Por ejemplo, supongamos que desea obtener ETags de los encabezados `if-match` y `if-none-match` de la solicitud. Comenzaremos definiendo una clase para representar ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

También definiremos una enumeración para indicar si se va a obtener el valor ETag del encabezado `if-match` o del encabezado `if-none-match`.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

A continuación se muestra un **HttpParameterBinding** que obtiene el ETag del encabezado deseado y lo enlaza a un parámetro de tipo ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

El método **ExecuteBindingAsync** realiza el enlace. Dentro de este método, agregue el valor del parámetro enlazado al diccionario **ActionArgument** en **HttpActionContext**.

> [!NOTE]
> Si el método **ExecuteBindingAsync** lee el cuerpo del mensaje de solicitud, invalide la propiedad **WillReadBody** para que devuelva true. El cuerpo de la solicitud puede ser un flujo no almacenado en búfer que solo se puede leer una vez, por lo que la API Web exige una regla que, como máximo, un enlace puede leer el cuerpo del mensaje.

Para aplicar un **HttpParameterBinding**personalizado, puede definir un atributo que se derive de **ParameterBindingAttribute**. Por `ETagParameterBinding`, definiremos dos atributos, uno para `if-match` encabezados y otro para los encabezados de `if-none-match`. Ambos derivan de una clase base abstracta.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

A continuación se muestra un método de controlador que utiliza el atributo `[IfNoneMatch]`.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Además de **ParameterBindingAttribute**, hay otro enlace para agregar un **HttpParameterBinding**personalizado. En el objeto **HttpConfiguration** , la propiedad **ParameterBindingRules** es una colección de funciones anónimas de tipo (**HttpParameterDescriptor** -&gt; **HttpParameterBinding**). Por ejemplo, puede Agregar una regla que cualquier parámetro ETag en un método GET use `ETagParameterBinding` con `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

La función debe devolver `null` para los parámetros en los que el enlace no es aplicable.

## <a name="iactionvaluebinder"></a>IActionValueBinder

El proceso de enlace de parámetros completo se controla mediante un servicio conectable, **IActionValueBinder**. La implementación predeterminada de **IActionValueBinder** hace lo siguiente:

1. Busque un **ParameterBindingAttribute** en el parámetro. Esto incluye los atributos personalizados **[FromBody]** , **[FromUri]** y **[ModelBinder]** .
2. De lo contrario, busque en **HttpConfiguration. ParameterBindingRules** una función que devuelva un **HttpParameterBinding**que no sea NULL.
3. De lo contrario, use las reglas predeterminadas descritas anteriormente. 

    - Si el tipo de parámetro es "simple" o tiene un convertidor de tipos, enlace desde el URI. Esto es equivalente a poner el atributo **[FromUri]** en el parámetro.
    - De lo contrario, intente leer el parámetro desde el cuerpo del mensaje. Esto es equivalente a poner **[FromBody]** en el parámetro.

Si lo desea, puede reemplazar todo el servicio **IActionValueBinder** por una implementación personalizada.

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de enlace de parámetros personalizados](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/CustomParameterBinding)

Mike retrete escribió una buena serie de entradas de blog sobre el enlace de parámetros de la API Web:

- [Cómo la API Web realiza el enlace de parámetros](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Enlace de parámetros de estilo de MVC para Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Cómo enlazar a objetos personalizados en firmas de acción en MVC/API Web](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Creación de un proveedor de valores personalizado en Web API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Enlace de parámetros de la API Web en el capó](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
