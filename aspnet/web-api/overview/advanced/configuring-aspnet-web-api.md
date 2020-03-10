---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: Configuración de ASP.NET Web API 2-ASP.NET 4. x
author: MikeWasson
description: 'Configure ASP.NET Web API 2 para ASP.NET 4. x: Configure las opciones, el hospedaje de ASP.NET 4. x, el autohospedado OWIN, los servicios globales y la configuración previa del controlador.'
ms.author: riande
ms.date: 03/31/2014
ms.custom: seoapril2019
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4f76728fa5e4602e35e1b7cb2d41b2245093cad8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449347"
---
# <a name="configuring-aspnet-web-api-2"></a>Configuración de ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describe cómo configurar ASP.NET Web API.

- [Opciones de configuración](#settings)
- [Configuración de Web API con hospedaje de ASP.NET](#webhost)
- [Configuración de la API Web con autohospedaje OWIN](#selfhost)
- [Servicios globales de la API Web](#services)
- [Configuración por controlador](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Valores de configuración

Las opciones de configuración de la API Web se definen en la clase [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) .

| Miembro | Description |
| --- | --- |
| **DependencyResolver** | Habilita la inserción de dependencias para los controladores. Vea [uso de la resolución de dependencias de API Web](dependency-injection.md). |
| **Filtros** | Filtros de acción. |
| **Formateadores** | [Formateadores de tipo de medio](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Especifica si el servidor debe incluir los detalles del error, como los mensajes de excepción y los seguimientos de la pila, en los mensajes de respuesta HTTP. Vea [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializador** | Función que realiza la inicialización final de **HttpConfiguration**. |
| **MessageHandlers** | [Controladores de mensajes http](http-message-handlers.md). |
| **ParameterBindingRules** | Una colección de reglas para enlazar parámetros en las acciones del controlador. |
| **Propiedades** | Contenedor de propiedades genérico. |
| **Rutas** | Colección de rutas. Consulte [enrutamiento en ASP.net web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Servicios** | Colección de servicios. Consulte [servicios](#services). |

## <a name="prerequisites"></a>Requisitos previos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configuración de Web API con hospedaje de ASP.NET

En una aplicación de ASP.NET, configure la API Web llamando a [GlobalConfiguration. Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) en el método de **Inicio de\_** de la aplicación. El método **Configure** toma un delegado con un parámetro único de tipo **HttpConfiguration**. Realice toda la configuración dentro del delegado.

A continuación se muestra un ejemplo de uso de un delegado anónimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

En Visual Studio 2017, la plantilla de proyecto "aplicación Web ASP.NET configura automáticamente el código de configuración, si selecciona" API Web "en el cuadro de diálogo **nuevo proyecto ASP.net** .

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

La plantilla de proyecto crea un archivo denominado WebApiConfig.cs dentro de la aplicación\_carpeta de inicio. Este archivo de código define el delegado en el que se debe colocar el código de configuración de la API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

La plantilla de proyecto también agrega el código que llama al delegado desde la **aplicación\_Inicio**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configuración de la API Web con autohospedaje OWIN

Si está autohospedado con OWIN, cree una nueva instancia de **HttpConfiguration** . Realice cualquier configuración en esta instancia y, a continuación, pase la instancia al método de extensión **Owin. UseWebApi** .

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

En el tutorial [uso de OWIN para Autohospedar ASP.net web API 2 se](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) muestran los pasos completos.

<a id="services"></a>
## <a name="global-web-api-services"></a>Servicios globales de la API Web

La colección **HttpConfiguration. Services** contiene un conjunto de servicios globales que la API Web usa para realizar varias tareas, como la selección de controlador y la negociación de contenido.

> [!NOTE]
> La colección de **servicios** no es un mecanismo de uso general para la detección de servicios o la inserción de dependencias. Solo almacena los tipos de servicio que conoce el marco de la API Web.

La colección de **servicios** se inicializa con un conjunto predeterminado de servicios y puede proporcionar sus propias implementaciones personalizadas. Algunos servicios admiten varias instancias, mientras que otras solo pueden tener una instancia. (Sin embargo, también puede proporcionar servicios en el nivel de controlador; consulte [configuración por controlador](#percontrollerconfig).

Servicios de instancia única

| dirigido | Description |
| --- | --- |
| **IActionValueBinder** | Obtiene un enlace para un parámetro. |
| **IApiExplorer** | Obtiene las descripciones de las API expuestas por la aplicación. Consulte [creación de una página de ayuda para una API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtiene una lista de los ensamblados de la aplicación. Consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valida un modelo que se lee desde el cuerpo de la solicitud mediante un formateador de tipo de medio. |
| **IContentNegotiator** | Realiza la negociación de contenido. |
| **IDocumentationProvider** | Proporciona documentación para las API de. El valor predeterminado es **null**. Consulte [creación de una página de ayuda para una API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica si el host debe almacenar en búfer los cuerpos de la entidad de mensaje HTTP. |
| **IHttpActionInvoker** | Invoca una acción de controlador. Consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Selecciona una acción de controlador. Consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Activa un controlador. Consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Selecciona un controlador. Consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Proporciona una lista de los tipos de controlador de API Web en la aplicación. Consulte [enrutamiento y selección de acciones](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializa el marco de seguimiento. Consulte [seguimiento en ASP.net web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Proporciona un escritor de seguimiento. El valor predeterminado es un escritor de seguimiento "no operativo". Consulte [seguimiento en ASP.net web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Proporciona una memoria caché de validadores de modelo. |

Servicios de varias instancias

|                 dirigido                 |                                                                                                              Description                                                                                                               |
|-----------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <strong>IFilterProvider</strong>     |                                                                                           Devuelve una lista de filtros para una acción de controlador.                                                                                           |
|  <strong>ModelBinderProvider</strong>   |                                                                                                Devuelve un enlazador de modelos para un tipo determinado.                                                                                                |
| <strong>ModelMetadataProvider</strong>  |                                                                                                     Proporciona metadatos para un modelo.                                                                                                     |
| <strong>ModelValidatorProvider</strong> |                                                                                                   Proporciona un validador para un modelo.                                                                                                    |
|  <strong>ValueProviderFactory</strong>  | Crea un proveedor de valores. Para obtener más información, consulte la entrada de blog de Mike retrete [creación de un proveedor de valores personalizado en WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |

Para agregar una implementación personalizada a un servicio de varias instancias, llame a **Add** o **Insert** en la colección de **servicios** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Para reemplazar un servicio de una sola instancia con una implementación personalizada, llame a **Replace** en la colección de **servicios** :

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuración por controlador

Puede invalidar la siguiente configuración en cada controlador:

- Formateadores de tipo de medio
- Reglas de enlace de parámetros
- Servicios

Para ello, defina un atributo personalizado que implemente la interfaz **IControllerConfiguration** . A continuación, aplique el atributo al controlador.

En el ejemplo siguiente se reemplazan los formateadores de tipo de medio predeterminados por un formateador personalizado.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

El método **IControllerConfiguration. Initialize** toma dos parámetros:

- Un objeto **HttpControllerSettings**
- Un objeto **HttpControllerDescriptor**

**HttpControllerDescriptor** contiene una descripción del controlador, que puede examinar con fines informativos (por ejemplo, para distinguir entre dos controladores).

Use el objeto **HttpControllerSettings** para configurar el controlador. Este objeto contiene el subconjunto de parámetros de configuración que se pueden invalidar en cada controlador. Cualquier configuración que no cambie de forma predeterminada al objeto **HttpConfiguration** global.
