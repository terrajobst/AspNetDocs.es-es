---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creación de un singleton en OData V4 con Web API 2,2 | Microsoft Docs
author: rick-anderson
description: En este tema se muestra cómo definir un singleton en un punto de conexión de OData en Web API 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504505"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Creación de un singleton en OData V4 mediante Web API 2,2

por Zoe Luo

> Tradicionalmente, solo se podía tener acceso a una entidad si estaba encapsulada dentro de un conjunto de entidades. Sin embargo, OData V4 proporciona dos opciones adicionales, singleton y contención, en las que se admite WebAPI 2,2.

En este artículo se muestra cómo definir un singleton en un punto de conexión de OData en Web API 2,2. Para obtener información sobre qué es un singleton y cómo puede beneficiarse de su uso, consulte [uso de un singleton para definir la entidad especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Para crear un punto de conexión de OData V4 en Web API, consulte [crear un punto de conexión de oData V4 mediante ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md). 

Vamos a crear un singleton en el proyecto de API Web con el siguiente modelo de datos:

![Modelo de datos](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un singleton denominado `Umbrella` se definirá en función del tipo `Company`y un conjunto de entidades denominado `Employees` se definirá en función del tipo `Employee`.

La solución usada en este tutorial se puede descargar desde [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definir el modelo de datos

1. Defina los tipos de CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generar el modelo EDM basado en los tipos de CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    Aquí, `builder.Singleton<Company>("Umbrella")` indica al generador de modelos que cree un singleton denominado `Umbrella` en el modelo EDM.

    Los metadatos generados tendrán un aspecto similar al siguiente:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    A partir de los metadatos, podemos ver que la propiedad de navegación `Company` del conjunto de entidades `Employees` está enlazada a la `Umbrella`singleton. `ODataConventionModelBuilder`realiza automáticamente el enlace, ya que solo `Umbrella` tiene el tipo de `Company`. Si hay alguna ambigüedad en el modelo, puede usar `HasSingletonBinding` para enlazar explícitamente una propiedad de navegación a un singleton. `HasSingletonBinding` tiene el mismo efecto que usar el atributo `Singleton` en la definición de tipo de CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definir el controlador singleton

Al igual que el controlador EntitySet, el controlador singleton hereda de `ODataController`y el nombre del controlador singleton debe ser `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Para controlar diferentes tipos de solicitudes, es necesario que las acciones estén predefinidas en el controlador. El **enrutamiento de atributos** está habilitado de forma predeterminada en WebApi 2,2. Por ejemplo, para definir una acción para controlar las consultas `Revenue` desde `Company` mediante el enrutamiento de atributos, use lo siguiente:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Si no está dispuesto a definir atributos para cada acción, defina las acciones siguiendo las [convenciones de enrutamiento de oData](../odata-routing-conventions.md). Dado que no se necesita una clave para consultar un singleton, las acciones definidas en el controlador singleton son ligeramente diferentes de las acciones definidas en el controlador EntitySet.

Como referencia, a continuación se enumeran las firmas de método para cada definición de acción en el controlador singleton.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Básicamente, esto es todo lo que necesita hacer en el lado del servicio. El [proyecto de ejemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene todo el código de la solución y el cliente de oData que muestra cómo utilizar el singleton. El cliente se crea siguiendo los pasos descritos en [creación de una aplicación cliente de oData V4](create-an-odata-v4-client-app.md).

. 

*Gracias a Leo hu en el contenido original de este artículo.*
