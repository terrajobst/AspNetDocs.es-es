---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creación de un punto de conexión de OData V3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: El Open Data Protocol (OData) es un protocolo de acceso a datos para la Web. OData proporciona una manera uniforme de estructurar los datos, consultar los datos y manipular los datos...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: e68a454398f109dfd089be9c9a44d3fe662acc2f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600433"
---
# <a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Creación de un punto de conexión de OData V3 con Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> El [Open Data Protocol](http://www.odata.org/) (OData) es un protocolo de acceso a datos para la Web. OData proporciona una manera uniforme de estructurar los datos, consultar los datos y manipular el conjunto de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar). OData admite los formatos AtomPub (XML) y JSON. OData también define una manera de exponer metadatos sobre los datos. Los clientes pueden usar los metadatos para detectar la información de tipo y las relaciones del conjunto de datos.
>
> ASP.NET Web API facilita la creación de un punto de conexión de OData para un conjunto de datos. Puede controlar exactamente las operaciones de OData que admite el extremo. Puede hospedar varios puntos de conexión de OData, junto con puntos de conexión que no son de OData. Tiene control total sobre el modelo de datos, la lógica de negocios de back-end y el nivel de datos.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - API Web 2
> - Versión 3 de OData
> - Entity Framework 6
> - [Proxy de depuración web de Fiddler (opcional)](http://www.fiddler2.com)
>
> Se ha agregado compatibilidad con la API Web de OData en [ASP.net and Web Tools actualización 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Sin embargo, en este tutorial se usa la técnica scaffolding que se agregó en Visual Studio 2013.

En este tutorial, creará un punto de conexión de OData sencillo que los clientes pueden consultar. También creará un C# cliente para el punto de conexión. Después de completar este tutorial, el siguiente conjunto de tutoriales muestra cómo agregar más funcionalidad, incluidas las relaciones de entidad, las acciones y el $expand/$select.

- [Crear el proyecto de Visual Studio](#create-project)
- [Agregar un modelo de entidad](#add-model)
- [Agregar un controlador OData](#add-controller)
- [Agregar el EDM y la ruta](#edm)
- [Inicialización de la base de datos (opcional)](#seed-db)
- [Exploración del punto de conexión de OData](#explore)
- [Formatos de serialización de OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

En este tutorial, creará un punto de conexión de OData que admita las operaciones CRUD básicas. El punto de conexión expondrá un único recurso, una lista de productos. En los tutoriales posteriores se agregarán más características.

Inicie Visual Studio y seleccione **nuevo proyecto** en la página de inicio. O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda C# el nodo visual. En **Visual C#** , seleccione **Web**. Seleccione **la plantilla aplicación Web ASP.net** .

![](creating-an-odata-endpoint/_static/image1.png)

En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **vacía** . En &quot;agregar carpetas y referencias principales para...&quot;, Compruebe la **API Web**. Haga clic en **Aceptar**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Agregar un modelo de entidad

Un *modelo* es un objeto que representa los datos de la aplicación. En este tutorial, se necesita un modelo que represente un producto. El modelo corresponde a nuestro tipo de entidad OData.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos. En el menú contextual, seleccione **Agregar** y, a continuación, seleccione **clase**.

![](creating-an-odata-endpoint/_static/image3.png)

En el cuadro de diálogo **Agregar nuevo** elemento, asigne a la clase el nombre &quot;&quot;de producto.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Por Convención, las clases de modelo se colocan en la carpeta models. No tiene que seguir esta Convención en sus propios proyectos, pero la usaremos para este tutorial.

En el archivo Product.cs, agregue la siguiente definición de clase:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

La propiedad ID será la clave de entidad. Los clientes pueden consultar los productos por identificador. Este campo también sería la clave principal en la base de datos back-end.

Compile el proyecto ahora. En el paso siguiente, usaremos algunos scaffolding de Visual Studio que usa la reflexión para buscar el tipo de producto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Agregar un controlador OData

Un *controlador* es una clase que controla las solicitudes HTTP. Debe definir un controlador independiente para cada conjunto de entidades en el servicio OData. En este tutorial, vamos a crear un único controlador.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers. Seleccione **Agregar** y, a continuación, seleccione **controlador**.

![](creating-an-odata-endpoint/_static/image5.png)

En el cuadro de diálogo **Agregar scaffold** , seleccione &quot;controlador OData de Web API 2 con acciones, con Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre "ProductsController". Active la casilla &quot;usar las acciones de controlador Async&quot;. En la lista desplegable **modelo** , seleccione la clase product.

![](creating-an-odata-endpoint/_static/image7.png)

Haga clic en el botón **nuevo contexto de datos...** . Deje el nombre predeterminado para el tipo de contexto de datos y haga clic en **Agregar**.

![](creating-an-odata-endpoint/_static/image8.png)

Haga clic en agregar en el cuadro de diálogo Agregar controlador para agregar el controlador.

![](creating-an-odata-endpoint/_static/image9.png)

Nota: Si recibe un mensaje de error que indica &quot;se produjo un error al obtener el tipo...&quot;, asegúrese de que ha compilado el proyecto de Visual Studio después de haber agregado la clase product. El scaffolding usa la reflexión para buscar la clase.

![](creating-an-odata-endpoint/_static/image10.png)

El scaffolding agrega dos archivos de código al proyecto:

- Products.cs define el controlador de API Web que implementa el punto de conexión de OData.
- ProductServiceContext.cs proporciona métodos para consultar la base de datos subyacente mediante Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Agregar el EDM y la ruta

En Explorador de soluciones, expanda la carpeta app\_Start y abra el archivo denominado WebApiConfig.cs. Esta clase contiene el código de configuración de la API Web. Reemplace este código por lo siguiente:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Este código hace dos cosas:

- Crea un Entity Data Model (EDM) para el extremo de OData.
- Agrega una ruta para el extremo.

Un EDM es un modelo abstracto de los datos. El EDM se usa para crear el documento de metadatos y definir los URI para el servicio. **ODataConventionModelBuilder** crea un EDM mediante un conjunto de convenciones de nomenclatura predeterminadas EDM. Este enfoque requiere el menos código. Si desea tener más control sobre el EDM, puede usar la clase **ODataModelBuilder** para crear el EDM agregando propiedades, claves y propiedades de navegación explícitamente.

El método **EntitySet** agrega un conjunto de entidades al EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

La cadena "Products" define el nombre del conjunto de entidades. El nombre del controlador debe coincidir con el nombre del conjunto de entidades. En este tutorial, el conjunto de entidades se denomina "Products" y el controlador se denomina `ProductsController`. Si ha llamado al conjunto de entidades "ProductSet", deberá asignar un nombre al controlador `ProductSetController`. Tenga en cuenta que un punto de conexión puede tener varios conjuntos de entidades. Llame a **EntitySet&lt;t&gt;** para cada conjunto de entidades y, a continuación, defina un controlador correspondiente.

El método **MapODataRoute** agrega una ruta para el punto de conexión de oData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

El primer parámetro es un nombre descriptivo para la ruta. Los clientes de su servicio no ven este nombre. El segundo parámetro es el prefijo URI del extremo. Dado este código, el URI del conjunto de entidades Products es http://<em>hostname</em>/OData/Products. La aplicación puede tener más de un punto de conexión de OData. Para cada punto de conexión, llame a <strong>MapODataRoute</strong> y proporcione un nombre de ruta único y un prefijo de identificador URI único.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Inicialización de la base de datos (opcional)

En este paso, usará Entity Framework para inicializar la base de datos con algunos datos de prueba. Este paso es opcional, pero le permite probar el punto de conexión de OData de inmediato.

En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Esto agrega una carpeta denominada Migrations y un archivo de código denominado Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Abra este archivo y agregue el código siguiente al método `Configuration.Seed`.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Estos comandos generan código que crea la base de datos y, a continuación, ejecutan ese código.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Exploración del punto de conexión de OData

En esta sección, usaremos el [proxy de depuración web de Fiddler](http://www.fiddler2.com) para enviar solicitudes al punto de conexión y examinar los mensajes de respuesta. Esto le ayudará a comprender las capacidades de un punto de conexión de OData.

En Visual Studio, presione F5 para iniciar la depuración. De forma predeterminada, Visual Studio abre el explorador para `http://localhost:*port*`, donde *Port* es el número de puerto configurado en la configuración del proyecto.

Puede cambiar el número de puerto en la configuración del proyecto. En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **propiedades**. En la ventana Propiedades, seleccione **Web**. Escriba el número de puerto en **dirección URL del proyecto**.

### <a name="service-document"></a>Documento de servicio

El *documento de servicio* contiene una lista de los conjuntos de entidades para el punto de conexión de oData. Para obtener el documento de servicio, envíe una solicitud GET al URI raíz del servicio.

Con Fiddler, escriba el siguiente URI en la pestaña **compositor** : `http://localhost:port/odata/`, donde *Port* es el número de puerto.

![](creating-an-odata-endpoint/_static/image13.png)

Haga clic en el botón **Ejecutar** . Fiddler envía una solicitud HTTP GET a la aplicación. Debería ver la respuesta en la lista de sesiones Web. Si todo funciona, el código de estado será 200.

![](creating-an-odata-endpoint/_static/image14.png)

Haga doble clic en la respuesta en la lista sesiones Web para ver los detalles del mensaje de respuesta en la pestaña inspectores.

![](creating-an-odata-endpoint/_static/image15.png)

El mensaje de respuesta HTTP sin formato debe ser similar al siguiente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

De forma predeterminada, la API Web devuelve el documento de servicio en formato AtomPub. Para solicitar JSON, agregue el siguiente encabezado a la solicitud HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Ahora la respuesta HTTP contiene una carga JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Documento de metadatos de servicio

El *documento de metadatos del servicio* describe el modelo de datos del servicio, utilizando un lenguaje XML denominado lenguaje de definición de esquemas conceptuales (CSDL). El documento de metadatos muestra la estructura de los datos en el servicio y se puede usar para generar el código de cliente.

Para obtener el documento de metadatos, envíe una solicitud GET a `http://localhost:port/odata/$metadata`. Estos son los metadatos del punto de conexión que se muestra en este tutorial.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Conjunto de entidades

Para obtener el conjunto de entidades Products, envíe una solicitud GET a `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entity

Para obtener un producto individual, envíe una solicitud GET a `http://localhost:port/odata/Products(1)`, donde "1" es el identificador del producto.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formatos de serialización de OData

OData admite varios formatos de serialización:

- Atom pub (XML)
- JSON "Light" (introducido en OData V3)
- JSON "detallado" (OData V2)

De forma predeterminada, Web API usa el formato "Light" de AtomPubJSON.

Para obtener el formato AtomPub, establezca el encabezado Accept en "Application/Atom + XML". A continuación se muestra un cuerpo de respuesta de ejemplo:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Puede ver una desventaja obvia del formato Atom: es mucho más detallado que la luz JSON. Sin embargo, si tiene un cliente que entienda AtomPub, el cliente podría preferir ese formato a través de JSON.

Esta es la versión JSON Light de la misma entidad:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

El formato JSON Light se presentó en la versión 3 del protocolo OData. Para mantener la compatibilidad con versiones anteriores, un cliente puede solicitar el formato JSON anterior "detallado". Para solicitar JSON detallado, establezca el encabezado Accept en `application/json;odata=verbose`. Esta es la versión detallada:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Este formato transmite más metadatos en el cuerpo de la respuesta, que puede Agregar una sobrecarga considerable sobre una sesión completa. Además, agrega un nivel de direccionamiento indirecto ajustando el objeto en una propiedad denominada "d".

## <a name="next-steps"></a>Pasos siguientes

- [Agregar relaciones de entidad](working-with-entity-relations.md)
- [Agregar acciones de OData](odata-actions.md)
- [Llamar al servicio de OData desde un cliente .NET](calling-an-odata-service-from-a-net-client.md)
