---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Enrutamiento de direcciones URL | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 66b727b69ca4f9a3d35b67f492f9a554146e09ef
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74590709"
---
# <a name="url-routing"></a>Enrutamiento de direcciones URL

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial, modificará la aplicación de ejemplo Wingtip Toys para admitir el enrutamiento de direcciones URL. El enrutamiento permite a la aplicación web usar direcciones URL que son fáciles de recordar y que son más compatibles con los motores de búsqueda. Este tutorial se basa en el tutorial anterior "pertenencia y administración" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo registrar rutas para una aplicación de formularios Web Forms ASP.NET.
- Cómo agregar rutas a una página web.
- Cómo seleccionar datos de una base de datos para admitir rutas.

## <a name="aspnet-routing-overview"></a>Introducción al enrutamiento de ASP.NET

El enrutamiento de direcciones URL permite configurar una aplicación para que acepte direcciones URL de solicitud que no se asignan a archivos físicos. Una dirección URL de solicitud es simplemente la dirección URL que un usuario escribe en el explorador para buscar una página en el sitio Web. El enrutamiento se usa para definir direcciones URL semánticamente significativas para los usuarios y que pueden ayudar con la optimización del motor de búsqueda (SEO).

De forma predeterminada, la plantilla de formularios Web Forms incluye [direcciones URL descriptivas de ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Gran parte del trabajo de enrutamiento básico se implementará mediante el uso de *direcciones URL descriptivas*. Sin embargo, en este tutorial agregará capacidades de enrutamiento personalizadas.

Antes de personalizar el enrutamiento de direcciones URL, la aplicación de ejemplo Wingtip Toys puede vincularse a un producto mediante la siguiente dirección URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Mediante la personalización del enrutamiento de direcciones URL, la aplicación de ejemplo Wingtip Toys se vinculará al mismo producto mediante una dirección URL más fácil de leer:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Rutas

Una ruta es un patrón de dirección URL que se asigna a un controlador. El controlador puede ser un archivo físico, como un archivo. aspx, en una aplicación de formularios Web Forms. Un controlador también puede ser una clase que procesa la solicitud. Para definir una ruta, cree una instancia de la clase Route especificando el patrón de la dirección URL, el controlador y, opcionalmente, un nombre para la ruta.

Agregue la ruta a la aplicación agregando el objeto `Route` a la propiedad `Routes` estática de la clase `RouteTable`. La propiedad Routes es una `RouteCollection` objeto que almacena todas las rutas para la aplicación.

### <a name="url-patterns"></a>Patrones de dirección URL

Un patrón de dirección URL puede contener valores literales y marcadores de posición de variables (denominados parámetros de dirección URL). Los literales y los marcadores de posición se encuentran en segmentos de la dirección URL que están delimitados por el carácter de barra diagonal (`/`).

Cuando se realiza una solicitud a la aplicación Web, la dirección URL se analiza en segmentos y marcadores de posición, y los valores de las variables se proporcionan al controlador de solicitudes. Este proceso es similar a la forma en que se analizan los datos de una cadena de consulta y se pasan al controlador de solicitudes. En ambos casos, la información de variable se incluye en la dirección URL y se pasa al controlador en forma de pares clave-valor. En el caso de las cadenas de consulta, tanto las claves como los valores se encuentran en la dirección URL. En el caso de las rutas, las claves son los nombres de marcador de posición definidos en el patrón de la dirección URL y solo los valores se encuentran en la dirección URL.

En un patrón de dirección URL, los marcadores de posición se definen entre llaves (`{` y `}`). Puede definir más de un marcador de posición en un segmento, pero los marcadores de posición deben estar separados por un valor literal. Por ejemplo, `{language}-{country}/{action}` es un patrón de ruta válido. Sin embargo, `{language}{country}/{action}` no es un patrón válido, ya que no hay ningún valor literal o delimitador entre los marcadores de posición. Por lo tanto, el enrutamiento no puede determinar dónde se debe separar el valor del marcador de posición de idioma del valor del marcador de posición Country.

### <a name="mapping-and-registering-routes"></a>Asignación y registro de rutas

Para poder incluir rutas a páginas de la aplicación de ejemplo Wingtip Toys, debe registrar las rutas cuando se inicie la aplicación. Para registrar las rutas, modificará el controlador de eventos `Application_Start`.

1. En **Explorador de soluciones**de Visual Studio, busque y abra el archivo *global.asax.CS* .
2. Agregue el código resaltado en amarillo al archivo *global.asax.CS* como se indica a continuación:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Cuando se inicia la aplicación de ejemplo Wingtip Toys, llama al controlador de eventos `Application_Start`. Al final de este controlador de eventos, se llama al método `RegisterCustomRoutes`. El método `RegisterCustomRoutes` agrega cada ruta llamando al método `MapPageRoute` del objeto `RouteCollection`. Las rutas se definen mediante un nombre de ruta, una dirección URL de ruta y una dirección URL física.

El primer parámetro ("`ProductsByCategoryRoute`") es el nombre de la ruta. Se usa para llamar a la ruta cuando es necesario. El segundo parámetro ("`Category/{categoryName}`") define la dirección URL de reemplazo descriptivo que puede ser dinámica en función del código. Esta ruta se usa cuando se rellena un control de datos con vínculos que se generan en función de los datos. Una ruta se muestra de la siguiente manera:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

El segundo parámetro de la ruta incluye un valor dinámico especificado por llaves (`{ }`). En este caso, el `categoryName` es una variable que se utilizará para determinar la ruta de acceso de enrutamiento adecuada.

> [!NOTE] 
> 
> **Optional**
> 
> Puede que le resulte más fácil administrar el código moviendo el método `RegisterCustomRoutes` a una clase independiente. En la carpeta *lógica* , cree una clase de `RouteActions` independiente. Mueva el método de `RegisterCustomRoutes` anterior del archivo *global.asax.CS* a la nueva clase de `RoutesActions`. Use la clase `RoleActions` y el método `createAdmin` como ejemplo de cómo llamar al método `RegisterCustomRoutes` desde el archivo *global.asax.CS* .

También puede haber observado el `RegisterRoutes` llamada al método con el `RouteConfig` objeto al principio del controlador de eventos `Application_Start`. Esta llamada se realiza para implementar el enrutamiento predeterminado. Se incluyó como código predeterminado al crear la aplicación mediante la plantilla de formularios Web Forms de Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recuperación y uso de datos de ruta

Como se mencionó anteriormente, se pueden definir rutas. El código que ha agregado al controlador de eventos `Application_Start` en el archivo *global.asax.CS* carga las rutas que se puede definir.

### <a name="setting-routes"></a>Establecer rutas

Las rutas requieren que se agregue código adicional. En este tutorial, utilizará el enlace de modelos para recuperar un `RouteValueDictionary` objeto que se usa al generar las rutas utilizando los datos de un control de datos. El objeto `RouteValueDictionary` contendrá una lista de nombres de productos que pertenecen a una categoría específica de productos. Se crea un vínculo para cada producto en función de los datos y la ruta.

#### <a name="enable-routes-for-categories-and-products"></a>Habilitar rutas para categorías y productos

A continuación, actualizará la aplicación para que use el `ProductsByCategoryRoute` para determinar la ruta correcta que se va a incluir para cada vínculo de categoría de producto. También actualizará la página *productlist. aspx* para incluir un vínculo enrutado para cada producto. Los vínculos se mostrarán tal y como estaban antes del cambio; sin embargo, los vínculos usarán ahora el enrutamiento de direcciones URL.

1. En **Explorador de soluciones**, abra la página *site. Master* si aún no está abierta.
2. Actualice el control **ListView** denominado "`categoryList`" con los cambios resaltados en amarillo, de modo que el marcado tenga el siguiente aspecto:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. En **Explorador de soluciones**, abra la página *productlist. aspx* .
4. Actualice el elemento `ItemTemplate` de la página *productlist. aspx* con las actualizaciones resaltadas en amarillo, de modo que el marcado tenga el siguiente aspecto:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Abra el código subyacente de *productlist.aspx.CS* y agregue el siguiente espacio de nombres como se resalta en amarillo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Reemplace el método `GetProducts` del código subyacente (*productlist.aspx.CS*) por el código siguiente:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Agregar código para los detalles del producto

Ahora, actualice el código subyacente (*productdetails.aspx.CS*) de la página *productdetails. aspx* para usar los datos de ruta. Tenga en cuenta que el nuevo método `GetProduct` también acepta un valor de cadena de consulta para el caso en el que el usuario tenga un vínculo marcado que use la dirección URL no enrutada anterior no descriptiva.

1. Reemplace el método `GetProduct` del código subyacente (*productdetails.aspx.CS*) por el código siguiente:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver las rutas actualizadas.

1. Presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 El explorador se abre y muestra la página *default. aspx* .
2. Haga clic en el vínculo **productos** en la parte superior de la página.  
 Todos los productos se muestran en la página *productlist. aspx* . Se muestra la siguiente dirección URL (con el número de puerto) para el explorador:  
    `https://localhost:44300/ProductList`
3. A continuación, haga clic en el vínculo de la categoría **Cars** cerca de la parte superior de la página.  
 En la página *productlist. aspx* solo se muestran los automóviles. Se muestra la siguiente dirección URL (con el número de puerto) para el explorador:  
    `https://localhost:44300/Category/Cars`
4. Haga clic en el vínculo que contiene el nombre del primer automóvil incluido en la página ("**automóvil convertible**") para mostrar los detalles del producto.  
 Se muestra la siguiente dirección URL (con el número de puerto) para el explorador:  
    `https://localhost:44300/Product/Convertible%20Car`
5. A continuación, escriba la siguiente dirección URL no enrutada (con el número de puerto) en el explorador:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 El código sigue reconociendo una dirección URL que incluye una cadena de consulta, para el caso en el que un usuario tiene un vínculo marcado.

## <a name="summary"></a>Resumen

En este tutorial, ha agregado rutas para categorías y productos. Ha aprendido cómo se pueden integrar las rutas con controles de datos que usan el enlace de modelos. En el siguiente tutorial, implementará el control de errores global.

## <a name="additional-resources"></a>Recursos adicionales

[Direcciones URL descriptivas de ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Implementar una aplicación de formularios Web Forms de ASP.NET segura con pertenencia, OAuth y SQL Database para Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](membership-and-administration.md)
> [Siguiente](aspnet-error-handling.md)
