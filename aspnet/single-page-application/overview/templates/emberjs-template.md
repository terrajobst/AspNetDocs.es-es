---
uid: single-page-application/overview/templates/emberjs-template
title: Plantilla EmberJS | Microsoft Docs
author: xqiu
description: Plantilla de EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467161"
---
# <a name="emberjs-template"></a>Plantilla de EmberJS

por [Xinyang Qiu](https://github.com/xqiu)

> La plantilla EmberJS MVC está escrita por Nathan Totten, Thiago Santos y Xinyang Qiu.
> 
> [Descarga de la plantilla EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)

La plantilla EmberJS SPA está diseñada para empezar a crear rápidamente aplicaciones Web interactivas del lado cliente con EmberJS.

"Aplicación de una sola página" (SPA) es el término general para una aplicación web que carga una sola página HTML y, a continuación, actualiza la página dinámicamente, en lugar de cargar nuevas páginas. Después de la carga inicial de la página, el SPA se comunica con el servidor a través de solicitudes AJAX.

![](emberjs-template/_static/image1.png)

AJAX no es nuevo, pero hoy en día hay marcos de JavaScript que facilitan la compilación y el mantenimiento de una aplicación de SPA sofisticada. Además, HTML 5 y CSS3 facilitan la creación de interfaces de IU enriquecidas.

La plantilla EmberJS SPA usa la biblioteca de JavaScript para [Ember](http://emberjs.com/) para controlar las actualizaciones de páginas de las solicitudes Ajax. Ember. js usa el enlace de datos para sincronizar la página con los datos más recientes. De este modo, no tiene que escribir ningún código que recorra los datos JSON y actualice el DOM. En su lugar, se colocan atributos declarativos en el código HTML que indican a Ember. js cómo presentar los datos.

En el lado del servidor, la plantilla EmberJS es casi idéntica a la [plantilla KNOCKOUTJS Spa](../introduction/knockoutjs-template.md). Usa ASP.NET MVC para servir documentos HTML y ASP.NET Web API para controlar las solicitudes AJAX desde el cliente. Para obtener más información sobre esos aspectos de la plantilla, consulte la documentación de la [plantilla KnockoutJS](../introduction/knockoutjs-template.md) . Este tema se centra en las diferencias entre la plantilla de cobertura y la plantilla EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Creación de un proyecto de plantilla de EmberJS SPA

Descargue e instale la plantilla haciendo clic en el botón Descargar anterior. Es posible que tenga que reiniciar Visual Studio.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Proporcione un nombre al proyecto y haga clic en **Aceptar**.

![](emberjs-template/_static/image2.png)

En el Asistente para **nuevo proyecto** , seleccione **proyecto de Ember. js Spa**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Información general de la plantilla de EmberJS SPA

La plantilla EmberJS usa una combinación de jQuery, Ember. js y Manillars. js para crear una interfaz de usuario fluida e interactiva.

Ember. js es una biblioteca de JavaScript que usa un patrón MVC del lado cliente.

- Una *plantilla*, que se escribe en el lenguaje de plantillas del manillar, describe la interfaz de usuario de la aplicación. En el modo de lanzamiento, el [compilador del manillar](https://github.com/Myslik/csharp-ember-handlebars) se utiliza para agrupar y compilar la plantilla del manillar.
- Un *modelo* almacena los datos de aplicación que obtiene del servidor (listas de tareas pendientes y elementos de la lista de tareas).
- Un *controlador* almacena el estado de la aplicación. Los controladores suelen presentar los datos del modelo a las plantillas correspondientes.
- Una *vista* traduce los eventos primitivos de la aplicación y los pasa al controlador.
- Un *enrutador* administra el estado de la aplicación, manteniendo las direcciones URL y las plantillas sincronizadas.

Además, la biblioteca de datos Ember se puede usar para sincronizar objetos JSON (obtenidos del servidor a través de una API de RESTful) y los modelos de cliente.

La plantilla EmberJS SPA organiza los scripts en ocho capas:

- WebAPI\_Adapter. js, WebAPI\_serializador. js: extiende la biblioteca de datos Ember para trabajar con ASP.NET Web API.
- Scripts/helpers. js: define las nuevas aplicaciones auxiliares de Ember manillar.
- Scripts/app. js: crea la aplicación y configura el adaptador y el serializador.
- Scripts/App/Models/\*. js: define los modelos.
- Scripts/App/views/\*. js: define las vistas.
- Scripts/App/Controllers/\*. js: define los controladores.
- Scripts/aplicación/rutas, scripts/App/router. js: define las rutas.
- Templates/\*. HBS: define las plantillas del manillar.

Echemos un vistazo a algunos de estos scripts con más detalle.

## <a name="models"></a>Modelos

Los modelos se definen en la carpeta scripts/aplicación/modelos. Hay dos archivos de modelo: todoItem. js y todoList. js.

**todo. Model. js** define los modelos del lado cliente (explorador) de las listas de tareas pendientes. Hay dos clases de modelo: todoItem y todoList. En Ember, los modelos son subclases de DS. Modela. Un modelo puede tener propiedades con atributos:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Los modelos pueden definir relaciones con otros modelos:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Los modelos pueden tener propiedades calculadas que se enlazan a otras propiedades:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Los modelos pueden tener funciones de observador, que se invocan cuando cambia una propiedad observada:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Vistas

Las vistas se definen en la carpeta scripts/aplicación/vistas. Una vista traduce los eventos de la interfaz de usuario de la aplicación. Un controlador de eventos puede devolver la llamada a las funciones de controlador o simplemente llamar directamente al contexto de datos.

Por ejemplo, el código siguiente procede de views/TodoItemEditView. js. Define el control de eventos para un campo de texto de entrada.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controlador

Los controladores se definen en la carpeta scripts/aplicación/controladores. Para representar un único modelo, extienda `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un controlador también puede representar una colección de modelos extendiendo `Ember.ArrayController`. Por ejemplo, TodoListController representa una matriz de objetos `todoList`. El controlador ordena por el identificador de todoList, en orden descendente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

El controlador define una función denominada `addTodoList`, que crea un nuevo todoList y lo agrega a la matriz. Para ver cómo se llama a esta función, abra el archivo de plantilla denominado todoListTemplate. html en la carpeta de plantillas. El código de plantilla siguiente enlaza un botón a la función `addTodoList`:

[!code-html[Main](emberjs-template/samples/sample8.html)]

El controlador también contiene una propiedad `error`, que contiene un mensaje de error. Este es el código de plantilla para mostrar el mensaje de error (también en todoListTemplate. html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Rutas

Enrutador. js define las rutas y la plantilla predeterminada que se va a mostrar, configura el estado de la aplicación y coincide con las direcciones URL de las rutas:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute. js carga los datos para TodoListRoute invalidando la función setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember usa convenciones de nomenclatura para hacer coincidir las direcciones URL, los nombres de ruta, los controladores y las plantillas. Para obtener más información, consulte [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) en la documentación de EmberJS.

## <a name="templates"></a>Plantillas

La carpeta Templates contiene cuatro plantillas:

- Application. HBS: plantilla predeterminada que se representa cuando se inicia la aplicación.
- about. HBS: plantilla para la ruta "/About".
- index. HBS: la plantilla para la ruta de la raíz "/".
- todoList. HBS: plantilla para la ruta "/todo".
- \_barra de navegación. HBS: la plantilla define el menú de navegación.

La plantilla de aplicación actúa como una página maestra. Contiene un encabezado, un pie de página y un "{{Outlet}}" para insertar otras plantillas en función de la ruta. Para obtener más información acerca de las plantillas de aplicación en Ember, consulte [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

La plantilla "/todoList" contiene dos expresiones de bucle. El bucle exterior es `{{#each controller}}`y el bucle interior se `{{#each todos}}`. En el código siguiente se muestra una vista `Ember.Checkbox` integrada, una `App.TodoItemEditView`personalizada y un vínculo con una acción `deleteTodo`.

[!code-html[Main](emberjs-template/samples/sample12.html)]

La clase `HtmlHelperExtensions`, que se define en Controllers/HtmlHelperExtensions. CS, define una función auxiliar para almacenar en memoria caché e insertar archivos de plantilla cuando **Debug** está establecido en **true** en el archivo Web. config. Esta función se llama desde el archivo de vista de ASP.NET MVC definido en views/Home/app. cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Se llama sin argumentos, la función representa todos los archivos de plantilla en la carpeta de plantillas. También puede especificar una subcarpeta o un archivo de plantilla específico.

Cuando **Debug** es **false** en Web. config, la aplicación incluye el elemento de agrupación "~/bundles/templates". Este elemento de paquete se agrega en BundleConfig.cs, mediante la biblioteca del compilador del manillar:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
