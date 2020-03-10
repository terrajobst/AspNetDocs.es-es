---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Cree un punto de conexión de OData V4 con ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: El Open Data Protocol (OData) es un protocolo de acceso a datos para la Web. OData proporciona una manera uniforme de consultar y manipular conjuntos de datos a través de operaciones CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484501"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Cree un punto de conexión de OData V4 mediante ASP.NET Web API 

> El Open Data Protocol (OData) es un protocolo de acceso a datos para la Web. OData proporciona una manera uniforme de consultar y manipular conjuntos de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).
>
> ASP.NET Web API admite V3 y V4 del protocolo. Incluso puede tener un punto de conexión V4 que se ejecute en paralelo con un punto de conexión V3.
>
> En este tutorial se muestra cómo crear un punto de conexión de OData V4 que admita operaciones CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
> - API Web 5,2
> - OData v4
> - Visual Studio 2017 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - 4\.7.2 .NET
>
> ## <a name="tutorial-versions"></a>Versiones del tutorial
>
> Para la versión 3 de OData, consulte [creación de un punto de conexión de oData V3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

En Visual Studio, en el menú **archivo** , seleccione **nuevo** **proyecto**de &gt;.

Expanda **instalado** &gt;  **C# visual** &gt; **Web**y seleccione la plantilla **aplicación Web de ASP.net (.NET Framework)** . Asigne al proyecto el nombre &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Seleccione **Aceptar**.

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Seleccione la plantilla **Vacía**. En **Agregar carpetas y referencias principales para:** , seleccione **Web API**. Seleccione **Aceptar**.

## <a name="install-the-odata-packages"></a>Instalar los paquetes de OData

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**. En la ventana de la consola del administrador de paquetes, escriba:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Este comando instala los paquetes de NuGet de OData más recientes.

## <a name="add-a-model-class"></a>Incorporación de una clase de modelo

Un *modelo* es un objeto que representa una entidad de datos en la aplicación.

En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Models. En el menú contextual, seleccione **agregar** &gt; **clase**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Por Convención, las clases de modelo se colocan en la carpeta models, pero no es necesario seguir esta Convención en sus propios proyectos.

Asigne a la clase el nombre `Product`. En el archivo Product.cs, reemplace el código reutilizable por lo siguiente:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

La propiedad `Id` es la clave de entidad. Los clientes pueden consultar entidades por clave. Por ejemplo, para obtener el producto con el identificador 5, el URI es `/Products(5)`. La propiedad `Id` también será la clave principal en la base de datos back-end.

## <a name="enable-entity-framework"></a>Habilitar Entity Framework

En este tutorial, usaremos el Code First de Entity Framework (EF) para crear la base de datos back-end.

> [!NOTE]
> OData de Web API no requiere EF. Use cualquier capa de acceso a datos que pueda traducir entidades de base de datos en modelos.

En primer lugar, instale el paquete NuGet para EF. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**. En la ventana de la consola del administrador de paquetes, escriba:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Abra el archivo Web. config y agregue la sección siguiente dentro del elemento de **configuración** , después del elemento **configSections** .

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Esta configuración agrega una cadena de conexión para una base de datos de LocalDB. Esta base de datos se usará al ejecutar la aplicación localmente.

A continuación, agregue una clase denominada `ProductsContext` a la carpeta models:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

En el constructor, `"name=ProductsContext"` proporciona el nombre de la cadena de conexión.

## <a name="configure-the-odata-endpoint"></a>Configuración del punto de conexión de OData

Abra la aplicación de archivo\_Start/WebApiConfig. cs. Agregue las siguientes instrucciones **using** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Después, agregue el código siguiente al método **Register** :

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Este código hace dos cosas:

- Crea un Entity Data Model (EDM).
- Agrega una ruta.

Un EDM es un modelo abstracto de los datos. El EDM se usa para crear el documento de metadatos del servicio. La clase **ODataConventionModelBuilder** crea un EDM mediante las convenciones de nomenclatura predeterminadas. Este enfoque requiere el menos código. Si desea tener más control sobre el EDM, puede usar la clase **ODataModelBuilder** para crear el EDM agregando propiedades, claves y propiedades de navegación explícitamente.

Una *ruta* indica a la API Web cómo enrutar las solicitudes HTTP al punto de conexión. Para crear una ruta de OData V4, llame al método de extensión **MapODataServiceRoute** .

Si la aplicación tiene varios puntos de conexión de OData, cree una ruta independiente para cada uno. Asigne a cada ruta un nombre y un prefijo de ruta únicos.

## <a name="add-the-odata-controller"></a>Agregar el controlador OData

Un *controlador* es una clase que controla las solicitudes HTTP. Cree un controlador independiente para cada conjunto de entidades en el servicio OData. En este tutorial, creará un controlador para la entidad `Product`.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers y seleccione **Agregar** **clase**de &gt;. Asigne a la clase el nombre `ProductsController`.

> [!NOTE]
> La versión de este tutorial para OData V3 usa el scaffolding **Agregar controlador** . Actualmente, no hay ningún scaffolding para OData V4.

Reemplace el código reutilizable en ProductsController.cs con lo siguiente.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

El controlador utiliza la clase `ProductsContext` para tener acceso a la base de datos mediante EF. Tenga en cuenta que el controlador invalida el método **Dispose** para eliminar el **ProductsContext**.

Este es el punto de partida para el controlador. A continuación, agregaremos métodos para todas las operaciones CRUD.

## <a name="query-the-entity-set"></a>Consultar el conjunto de entidades

Agregue los métodos siguientes a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La versión sin parámetros del método `Get` devuelve toda la colección de productos. El método `Get` con un parámetro *clave* busca un producto por su clave (en este caso, la propiedad `Id`).

El atributo **[EnableQuery]** permite a los clientes modificar la consulta mediante opciones de consulta como $filter, $sort y $Page. Para obtener más información, consulte [compatibilidad con las opciones de consulta de oData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Agregar una entidad al conjunto de entidades

Para que los clientes puedan agregar un nuevo producto a la base de datos, agregue el método siguiente a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Actualización de una entidad

OData admite dos semánticas diferentes para actualizar una entidad, PATCH y PUT.

- PATCH realiza una actualización parcial. El cliente especifica solo las propiedades que se van a actualizar.
- PUT reemplaza toda la entidad.

El inconveniente de PUT es que el cliente debe enviar valores para todas las propiedades de la entidad, incluidos los valores que no cambian. La [especificación de oData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica que se prefiere la revisión.

En cualquier caso, este es el código de los métodos PATCH y PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

En el caso de la revisión, el controlador usa el tipo de **&gt;Delta&lt;t** para realizar el seguimiento de los cambios.

## <a name="delete-an-entity"></a>Eliminación de una entidad

Para permitir que los clientes eliminen un producto de la base de datos, agregue el método siguiente a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
