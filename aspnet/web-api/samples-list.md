---
uid: web-api/samples-list
title: 'Lista de ejemplos de la API Web: ASP.NET 4. x'
author: rick-anderson
description: Lista de ejemplos de ASP.NET Web API para ASP.NET 4. x
ms.author: riande
ms.date: 09/18/2012
ms.custom: seoapril2019
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 24368fc80e7fc3c840f1f1f9accf13fa15a06c1b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484267"
---
# <a name="web-api-samples-list"></a>Lista de ejemplos de Web API

## <a name="httpclient-samples"></a>Ejemplos de HttpClient

**Ejemplo de Bing Translate** | [origen de vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/BingTranslateSample)

Muestra cómo llamar al [servicio Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) mediante la clase **HttpClient** . La API del servicio Microsoft Translator requiere un token de OAuth, que la aplicación obtiene mediante el envío de una solicitud al servidor de tokens de Azure para cada solicitud al servicio de traductor. El resultado del servidor de token se introduce en la solicitud enviada al servicio de traducción. Antes de ejecutar este ejemplo, debe obtener una [clave de aplicación de Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) y rellenar la información en la clase de ejemplo AccessTokenMessageHandler.

**Ejemplo de Google Maps** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/GoogleMapsSample)

Usa **HttpClient** para descargar un mapa de Redmond, WA de [Google Maps API](https://developers.google.com/maps/), lo guarda como un archivo local y abre el visor de imágenes predeterminado.

**Ejemplo de cliente de Twitter** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/TwitterSample)

Muestra cómo escribir un cliente de Twitter sencillo mediante **HttpClient**. En el ejemplo se usa un **HttpMessageHandler** para insertar información de autenticación de OAuth en el **HttpRequestMessage**saliente. El resultado de Twitter se lee mediante JSON.NET. Antes de ejecutar este ejemplo, debe obtener una [clave de aplicación de Twitter](https://dev.twitter.com/)y rellenar la información de la clase de ejemplo OAuthMessageHandler.

**Ejemplo de banco mundial** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [vs 2010 origen](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net40) | [vs 2012 origen](https://github.com/aspnet/samples/blob/master/samples/aspnet/HttpClient/WorldBankSample/Net45)

Muestra cómo recuperar datos del sitio de datos del Banco Mundial mediante JSON.NET para analizar el resultado.

## <a name="web-api-samples"></a>Ejemplos de API Web

**Introducción con ASP.NET Web API** | [origen de vs 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Muestra cómo crear una API Web básica que admita solicitudes HTTP GET. Contiene el código fuente del tutorial de [su primera ASP.net web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.net web API escenarios de JavaScript: comentarios** | [origen de vs 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Muestra cómo usar ASP.NET Web API para compilar API Web que admitan clientes del explorador y se pueden llamar fácilmente mediante jQuery.

Origen de **contactos** | [vs 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

En este ejemplo se usa ASP.NET Web API para compilar una sencilla aplicación de administrador de contactos. La aplicación se compone de una API Web de Contact Manager usada por una aplicación de ASP.NET MVC y una aplicación Windows Phone para mostrar y administrar una lista de contactos.

**Ejemplo de procesamiento por lotes** | [Descripción detallada](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [origen de vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/BatchSample)

Muestra cómo implementar el procesamiento por lotes HTTP en ASP.NET. El procesamiento por lotes consiste en colocar varias solicitudes HTTP en un solo cuerpo de entidad MIME de varias partes, que después se envía al servidor como HTTP POST. Las solicitudes se procesan individualmente y las respuestas se colocan en otro cuerpo de entidad MIME de varias partes, que se devuelve al cliente.

**Ejemplo de controlador de contenido** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) |  | de [origen de vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net40) [frente al origen 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/ContentControllerSample/Net45)

Muestra cómo leer y escribir entidades de solicitud y respuesta de forma asincrónica mediante secuencias. El controlador de ejemplo tiene dos acciones: una acción PUT que lee el cuerpo de la entidad de solicitud de forma asincrónica y la almacena en un archivo local, y una acción GET que devuelve el contenido del archivo local.

**Ejemplo de solucionador de ensamblados personalizado** | [origen de vs 2012](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomAssemblyResolverSample)

Muestra cómo modificar ASP.NET Web API para admitir la detección de controladores de un ensamblado de biblioteca cargado dinámicamente. En el ejemplo se implementa una **IAssembliesResolver** personalizada que llama a la implementación predeterminada y, a continuación, se agrega el ensamblado de biblioteca a los resultados predeterminados.

**Ejemplo de formateador de tipo de medio personalizado** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [origen de vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomMediaTypeFormatterSample)

Muestra cómo crear un formateador de tipo multimedia personalizado mediante la clase base **BufferedMediaTypeFormatter** . Esta clase base está pensada para formateadores que usan principalmente operaciones sincrónicas de lectura y escritura. Además de mostrar el formateador de tipo de medio, el ejemplo muestra cómo enlazarlo al registrarlo como parte del **HttpConfiguration** de la aplicación. Tenga en cuenta que también es posible utilizar directamente la clase base **elemento mediatypeformatter** para los formateadores que usan principalmente operaciones asincrónicas de lectura y escritura.

**Ejemplo de enlace de parámetros personalizados** | [Descripción detallada](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [origen de vs 2010](https://github.com/aspnet/samples/blob/master/samples/aspnet/WebApi/CustomParameterBinding)

Muestra cómo personalizar el proceso de enlace de parámetros, que es el proceso que determina cómo se enlaza la información de una solicitud a los parámetros de acción. En este ejemplo, el controlador Home tiene cuatro acciones:

1. BindPrincipal muestra cómo enlazar un parámetro IPrincipal desde una entidad de seguridad genérica personalizada, no desde un mensaje HTTP GET.
2. BindCustomComplexTypeFromUriOrBody muestra cómo enlazar un parámetro de tipo complejo, que puede proviene del cuerpo del mensaje o del URI de solicitud de un mensaje HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty muestra cómo enlazar un parámetro de tipo complejo con una propiedad con el nombre cambiado que procede del URI de solicitud de un mensaje HTTP POST.
4. PostMultipleParametersFromBody muestra cómo enlazar varios parámetros del cuerpo de un mensaje POST;

**Ejemplo de carga de archivos** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/FileUploadSample)

Muestra cómo cargar archivos en un **ApiController** mediante la carga de archivos de varias partes MIME y cómo configurar notificaciones de progreso con **HttpClient** mediante **ProgressNotificationHandler**. El controlador lee el contenido de una carga de archivos HTML de forma asincrónica y escribe una o varias partes del cuerpo en un archivo local. La respuesta contiene información sobre el archivo o archivos cargados.

**Ejemplo de carga de archivos en el almacén de blobs de Azure** | [Descripción detallada](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/AzureBlobsFileUploadSample)

Este ejemplo es similar al ejemplo de carga de archivos, pero en lugar de guardar los archivos cargados en el disco local, carga de forma asincrónica los archivos en el [almacén de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) mediante el [SDK de Windows Azure para .net](https://www.windowsazure.com/develop/net/). También proporciona un mecanismo para enumerar los blobs que se encuentran actualmente en un [contenedor Azure BLOB Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Puede probar el ejemplo que se ejecuta en **Azure Storage emulador** que se incluye con el SDK de Azure. Si tiene una [cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), puede ejecutar también en el servicio de almacenamiento real.

**Ejemplo de canalización del controlador de mensajes Http** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [origen de vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/HttpMessageHandlerPipelineSample)

Muestra cómo conectar instancias de **HttpMessageHandler** en el cliente (**HttpClient**) y en el servidor (ASP.net web API). En el ejemplo, se usa el mismo controlador tanto en el cliente como en el servidor. Aunque es poco frecuente que el mismo controlador se ejecutara en ambos lugares, el modelo de objetos es el mismo en el lado cliente y en el servidor.

**Ejemplo de carga de JSON** | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/JsonUploadSample)

Muestra cómo cargar y descargar JSON desde y hacia un **ApiController**. En el ejemplo se usa una **ApiController** mínima y se accede a ella mediante **HttpClient**.

**Ejemplo de Mashup** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MashupSample)

Muestra cómo obtener acceso a varios sitios remotos de forma asincrónica desde una acción de **ApiController** . Cada vez que se alcanza la acción, las solicitudes se realizan de forma asincrónica, de modo que no se bloquea ningún subproceso.

**Ejemplo de seguimiento de memoria** | [Descripción detallada](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [origen de vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MemoryTracingSample)

En este proyecto de ejemplo se crea un paquete de Nuget que instalará un escritor de seguimiento en memoria personalizado en ASP.NET Web API aplicaciones.

**Ejemplo de MongoDB** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/MongoSample)

Muestra cómo usar MongoDB como almacén persistente para un **ApiController**, mediante un patrón de repositorio.

**Ejemplo de procesador de cuerpo de respuesta** | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ResponseEntityProcessorSample)

Muestra cómo copiar una entidad de respuesta (es decir, un cuerpo de respuesta HTTP) en un archivo local antes de que se transmita al cliente y realizar un procesamiento adicional en ese archivo de forma asincrónica. En el ejemplo se implementa un **HttpMessageHandler** que contiene la entidad de respuesta con una que ambos se escriben en la salida como normal y en un archivo local.

**Ejemplo de carga de XDocument** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [origen de vs 2012](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/UploadXDocumentSample)

Muestra cómo cargar un XDocument a un **ApiController** con **PushStreamContent** y **HttpClient**.

**Ejemplo de validación** | [origen de vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/ValidationSample)

Muestra cómo puede usar los atributos de validación en los modelos de ASP.NET WebAPI para validar el contenido de la solicitud HTTP. Muestra cómo marcar las propiedades según sea necesario, cómo usar los atributos de validación personalizados y definidos por el marco para anotar el modelo y cómo devolver respuestas de error para Estados de modelo no válidos.

**Ejemplo de formulario Web** | [Descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [origen de vs 2010](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/WebFormSample)

Muestra un ApiController agregado a un proyecto de formularios Web Forms.

**[Ejemplo de RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs es una sencilla aplicación de seguimiento de errores que muestra cómo usar ASP.NET Web API y la nueva biblioteca de cliente HTTP para crear un sistema controlado por Hypermedia. El ejemplo incluye implementaciones de cliente y de servidor, utilizando ASP.NET Web API. El servidor utiliza un formateador Razor personalizado para generar representaciones de recursos. El ejemplo también proporciona un servidor node. js para ilustrar las ventajas que provienen del uso de un diseño de Hypermedia para desacoplar clientes y servidores.
