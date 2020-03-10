---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'Aplicación de una sola página: plantilla KnockoutJS | Microsoft Docs'
author: MikeWasson
description: Plantilla de cobertura
ms.author: riande
ms.date: 01/30/2013
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 3a551db1caa9636eb7f2e04c287d3ef371263584
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467323"
---
# <a name="single-page-application-knockoutjs-template"></a>Aplicación de una sola página: plantilla KnockoutJS

por [Mike Wasson](https://github.com/MikeWasson)

> La plantilla de MVC de cobertura es parte de ASP.NET and Web Tools 2012,2
> 
> [Descargar ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

La actualización ASP.NET and Web Tools 2012,2 incluye una plantilla de aplicación de una sola página (SPA) para ASP.NET MVC 4. Esta plantilla está diseñada para empezar a crear rápidamente aplicaciones Web interactivas del lado cliente.

"Aplicación de una sola página" (SPA) es el término general para una aplicación web que carga una sola página HTML y, a continuación, actualiza la página dinámicamente, en lugar de cargar nuevas páginas. Después de la carga inicial de la página, el SPA se comunica con el servidor a través de solicitudes AJAX.

![](knockoutjs-template/_static/image1.png)

AJAX no es nuevo, pero hoy en día hay marcos de JavaScript que facilitan la compilación y el mantenimiento de una aplicación de SPA sofisticada. Además, HTML 5 y CSS3 facilitan la creación de interfaces de IU enriquecidas.

Para empezar, la plantilla de SPA crea una aplicación de ejemplo "lista de tareas pendientes". En este tutorial, realizaremos una visita guiada por la plantilla. En primer lugar, veremos la aplicación de lista de tareas pendientes y luego examinamos los elementos tecnológicos que lo hacen.

## <a name="create-a-new-spa-template-project"></a>Crear un nuevo proyecto de plantilla de SPA

Requisitos:

- Visual Studio 2012 o Visual Studio Express 2012 para Web
- ASP.NET Web Tools 2012,2 Update. Puede instalar la actualización [aquí](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2).

Inicie Visual Studio y seleccione **nuevo proyecto** en la página de inicio. O bien, en el menú **archivo** , seleccione **nuevo** y luego **proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Proporcione un nombre al proyecto y haga clic en **Aceptar**.

![](knockoutjs-template/_static/image2.png)

En el Asistente para **nuevo proyecto** , seleccione **aplicación de una sola página**.

![](knockoutjs-template/_static/image3.png)

Presione F5 para compilar y ejecutar la aplicación. Cuando la aplicación se ejecuta por primera vez, se muestra una pantalla de inicio de sesión.

![](knockoutjs-template/_static/image4.png)

Haga clic en el vínculo &quot;registrarse&quot; y crear un nuevo usuario.

![](knockoutjs-template/_static/image5.png)

Después de iniciar sesión, la aplicación crea una lista de tareas predeterminada con dos elementos. Puede hacer clic en "Agregar lista de tareas" para agregar una nueva lista.

![](knockoutjs-template/_static/image6.png)

Cambiar el nombre de la lista, agregar elementos a la lista y desactivarla. También puede eliminar elementos o eliminar una lista completa. Los cambios se conservan automáticamente en una base de datos del servidor (en realidad LocalDB, ya que se está ejecutando la aplicación localmente).

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>Arquitectura de la plantilla de SPA

Este diagrama muestra los principales bloques de creación de la aplicación.

![](knockoutjs-template/_static/image8.png)

En el lado del servidor, ASP.NET MVC sirve al código HTML y también controla la autenticación basada en formularios.

ASP.NET Web API controla todas las solicitudes relacionadas con ToDoLists y ToDoItems, como obtener, crear, actualizar y eliminar. El cliente intercambia datos con la API Web en formato JSON.

Entity Framework (EF) es la capa O/RM. Media entre el mundo orientado a objetos de ASP.NET y la base de datos subyacente. La base de datos utiliza LocalDB, pero puede cambiarla en el archivo Web. config. Normalmente, se usaría LocalDB para el desarrollo local y, a continuación, se implementaría en una base de datos SQL en el servidor mediante la migración de EF Code-First.

En el lado del cliente, la biblioteca Knockout. js controla las actualizaciones de página de las solicitudes AJAX. El knockout usa el enlace de datos para sincronizar la página con los datos más recientes. De este modo, no tiene que escribir ningún código que recorra los datos JSON y actualice el DOM. En su lugar, se colocan atributos declarativos en el código HTML que indican cómo presentar los datos.

Una gran ventaja de esta arquitectura es que separa el nivel de presentación de la lógica de la aplicación. Puede crear la parte de la API Web sin saber nada sobre el aspecto que tendrá la Página Web. En el lado cliente, se crea un "modelo de vista" para representar los datos y el modelo de vista utiliza el knockout para enlazar con el código HTML. Esto le permite cambiar fácilmente el código HTML sin cambiar el modelo de vista. (Veremos un poco más adelante).

## <a name="models"></a>Modelos

En el proyecto de Visual Studio, la carpeta modelos contiene los modelos que se usan en el lado servidor. (También hay modelos en el lado cliente; los vamos a obtener).

![](knockoutjs-template/_static/image9.png)

**TodoItem, TodoList**

Estos son los modelos de base de datos para Entity Framework Code First. Tenga en cuenta que estos modelos tienen propiedades que apuntan entre sí. `ToDoList` contiene una colección de ToDoItems y cada `ToDoItem` tiene una referencia a su ToDoList primario. Estas propiedades se denominan propiedades de navegación y representan la relación de uno a varios con una lista de tareas pendientes y sus elementos pendientes.

La clase `ToDoItem` también utiliza el atributo **[ForeignKey]** para especificar que `ToDoListId` es una clave externa en la tabla `ToDoList`. Esto indica a EF que agregue una restricción de clave externa a la base de datos.

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto, TodoListDto**

Estas clases definen los datos que se enviarán al cliente. "DTO" significa "objeto de transferencia de datos". El DTO define cómo se serializarán las entidades en JSON. En general, hay varias razones para usar DTO:

- Para controlar qué propiedades se serializan. DTO puede contener un subconjunto de las propiedades del modelo de dominio. Podría hacerlo por razones de seguridad (para ocultar información confidencial) o simplemente para reducir la cantidad de datos que envía.
- Para cambiar la forma de los datos, por ejemplo, para aplanar una estructura de datos más compleja.
- Para mantener la lógica de negocios fuera del DTO (separación de preocupaciones).
- Si por algún motivo no se pueden serializar los modelos de dominio. Por ejemplo, las referencias circulares pueden causar problemas al serializar un objeto. existen maneras de controlar este problema en Web API (consulte [control de referencias circulares de objetos](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); pero usar un DTO simplemente evita el problema por completo.

En la plantilla SPA, dto contiene los mismos datos que los modelos de dominio. Sin embargo, siguen siendo útiles porque evitan referencias circulares de las propiedades de navegación y muestran el patrón DTO general.

**AccountModels.cs**

Este archivo contiene modelos para la pertenencia al sitio. La clase `UserProfile` define el esquema de los perfiles de usuario en la base de la pertenencia. (En este caso, la única información es el identificador de usuario y el nombre de usuario). Las otras clases de modelo de este archivo se utilizan para crear el registro de usuario y los formularios de inicio de sesión.

## <a name="entity-framework"></a>Entity Framework

La plantilla SPA usa EF Code First. En el desarrollo de Code First, primero se definen los modelos en el código y, a continuación, EF usa el modelo para crear la base de datos. También puede usar EF con una base de datos existente ([Database First](https://msdn.microsoft.com/data/jj206878.aspx)).

La clase `TodoItemContext` de la carpeta models se deriva de **DbContext**. Esta clase proporciona la "adherencia" entre los modelos y EF. El `TodoItemContext` contiene una colección de `ToDoItem` y una colección de `TodoList`. Para consultar la base de datos, basta con escribir una consulta LINQ en estas colecciones. Por ejemplo, aquí se muestra cómo puede seleccionar todas las listas de tareas pendientes para el usuario "Alice":

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

También puede agregar nuevos elementos a la colección, actualizar elementos o eliminar elementos de la colección y conservar los cambios en la base de datos.

## <a name="aspnet-web-api-controllers"></a>Controladores de ASP.NET Web API

En ASP.NET Web API, los controladores son objetos que controlan las solicitudes HTTP. Como se mencionó, la plantilla de SPA usa la API Web para habilitar las operaciones CRUD en instancias de `ToDoList` y `ToDoItem`. Los controladores se encuentran en la carpeta Controllers de la solución.

![](knockoutjs-template/_static/image10.png)

- `TodoController`: controla las solicitudes HTTP para los elementos de tareas pendientes
- `TodoListController`: controla las solicitudes HTTP para las listas de tareas pendientes.

Estos nombres son significativos, ya que la API Web coincide con la ruta de acceso del URI al nombre del controlador. (Para obtener información sobre cómo la API Web enruta las solicitudes HTTP a los controladores, consulte [enrutamiento en ASP.net web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)).

Echemos un vistazo a la clase `ToDoListController`. Contiene un solo miembro de datos:

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

El `TodoItemContext` se usa para comunicarse con EF, como se describió anteriormente. Los métodos del controlador implementan las operaciones CRUD. La API Web asigna solicitudes HTTP desde el cliente a métodos de controlador, como se indica a continuación:

| Solicitud HTTP | Método de controlador | Description |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | Obtiene una colección de listas de tareas pendientes. |
| OBTENER el*identificador* de/API/todo/ | `GetTodoList` | Obtiene una lista de tareas pendientes por identificador. |
| PUT/API/todo/*ID* | `PutTodoList` | Actualiza una lista de tareas pendientes. |
| POST /api/todo | `PostTodoList` | Crea una nueva lista de tareas pendientes. |
| ELIMINAR el*ID.* de/API/todo/ | `DeleteTodoList` | Elimina una lista de tareas pendientes. |

Observe que los URI de algunas operaciones contienen marcadores de posición para el valor de identificador. Por ejemplo, para eliminar una lista de para con un identificador de 42, el URI se `/api/todo/42`.

Para obtener más información sobre el uso de la API Web para las operaciones CRUD, consulte [creación de una API Web que admita operaciones CRUD](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md). El código de este controlador es bastante sencillo. Estos son algunos puntos interesantes:

- El método `GetTodoLists` usa una consulta LINQ para filtrar los resultados por el identificador del usuario que ha iniciado sesión. De este modo, un usuario solo verá los datos que le pertenecen. Además, observe que se usa una instrucción SELECT para convertir las instancias de `ToDoList` en instancias de `TodoListDto`.
- Los métodos PUT y POST comprueban el estado del modelo antes de modificar la base de datos. Si **ModelState. IsValid** es false, estos métodos devuelven http 400, solicitud incorrecta. Obtenga más información sobre la validación de modelos en Web API en la [validación del modelo](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md).
- La clase de controlador también se decora con el atributo **[Authorize]** . Este atributo comprueba si se ha autenticado la solicitud HTTP. Si la solicitud no está autenticada, el cliente recibe HTTP 401, no autorizado. Obtenga más información sobre la autenticación en la [autenticación y la autorización en ASP.net web API](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md).

La clase `TodoController` es muy similar a `TodoListController`. La diferencia más importante es que no define ningún método GET, ya que el cliente obtendrá los elementos de tareas pendientes junto con cada lista de tareas pendientes.

## <a name="mvc-controllers-and-views"></a>Controladores y vistas de MVC

Los controladores de MVC también se encuentran en la carpeta Controllers de la solución. `HomeController` representa el HTML principal de la aplicación. La vista del controlador Home se define en views/home/index. cshtml. La vista Inicio representa contenido diferente en función de si el usuario ha iniciado sesión:

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

Cuando los usuarios inician sesión, ven la interfaz de usuario principal. De lo contrario, verán el panel de inicio de sesión. Tenga en cuenta que esta representación condicional se produce en el lado servidor. No intente ocultar contenido confidencial en el lado&#8212;del cliente. todo lo que envíe en una respuesta HTTP es visible para alguien que esté viendo los mensajes HTTP sin formato.

## <a name="client-side-javascript-and-knockoutjs"></a>JavaScript del lado cliente y Knockout. js

Ahora vamos a pasar del lado servidor de la aplicación al cliente. La plantilla SPA usa una combinación de jQuery y Knockout. js para crear una interfaz de usuario fluida e interactiva. Knockout. js es una biblioteca de JavaScript que facilita el enlace de HTML a los datos. Knockout. js usa un patrón denominado "Model-View-ViewModel".

- El modelo son los datos de dominio (listas de tareas pendientes y elementos de la lista de tareas).
- La vista es el documento HTML.
- El modelo de vista es un objeto de JavaScript que contiene los datos del modelo. El modelo de vista es una abstracción de código de la interfaz de usuario. No tiene ningún conocimiento de la representación en HTML. En su lugar, representa características abstractas de la vista, como "una lista de elementos pendientes".

La vista está enlazada a datos con el modelo de vista. Las actualizaciones del modelo de vista se reflejan automáticamente en la vista. Los enlaces también funcionan con la otra dirección. Los eventos del DOM (como los clics) están enlazados a datos en las funciones del modelo de vista, que desencadenan llamadas AJAX.

La plantilla SPA organiza el código JavaScript del lado cliente en tres capas:

- todo. DataContext. js: envía solicitudes AJAX.
- todo. Model. js: define los modelos.
- todo. ViewModel. js: define el modelo de vista.

![](knockoutjs-template/_static/image11.png)

Estos archivos de script se encuentran en la carpeta scripts/aplicación de la solución.

![](knockoutjs-template/_static/image12.png)

**todo. DataContext** controla todas las llamadas Ajax a los controladores de la API Web. (Las llamadas AJAX para el inicio de sesión se definen en otro lugar, en ajaxLogin. js).

**todo. Model. js** define los modelos del lado cliente (explorador) de las listas de tareas pendientes. Hay dos clases de modelo: todoItem y todoList.

Muchas de las propiedades de las clases de modelo son del tipo "KO. observable". Los Observabless son la forma en que el Knockout es su magia. En la [documentación](http://knockoutjs.com/documentation/introduction.html)de la cobertura: un objeto observable es un "objeto de JavaScript que puede notificar los cambios a los suscriptores". Cuando el valor de un objeto observable cambia, el knockout actualiza cualquier elemento HTML que esté enlazado a esos observables. Por ejemplo, todoItem tiene observables para las propiedades title y hace:

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

También puede suscribirse a un objeto observable en el código. Por ejemplo, la clase todoItem se suscribe a los cambios en las propiedades "hace" y "title":

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**Modelo de vista**

El modelo de vista se define en todo. ViewModel. js. El modelo de vista es el punto central en el que la aplicación enlaza los elementos de la página HTML con los datos del dominio. En la plantilla SPA, el modelo de vista contiene una matriz observable de todoLists. En el siguiente código del modelo de vista se indica la cobertura para aplicar los enlaces:

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML y enlace de datos

El HTML principal de la página se define en views/home/index. cshtml. Dado que estamos usando el enlace de datos, el código HTML es solo una plantilla para lo que realmente se representa. El knockout usa enlaces *declarativos* . Los elementos de página se enlazan a los datos agregando un atributo "enlace de datos" al elemento. Este es un ejemplo muy sencillo, tomado de la documentación de la cobertura:

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

En este ejemplo, el knockout actualiza el contenido de la **&lt;intervalo&gt;** elemento con el valor de `myItems.count()`. Cada vez que este valor cambia, el knockout actualiza el documento.

El knockout proporciona varios tipos de enlace diferentes. Estos son algunos de los enlaces que se usan en la plantilla de SPA:

- **foreach**: permite recorrer en iteración un bucle y aplicar el mismo marcado a cada elemento de la lista. Se usa para representar las listas de tareas pendientes y los elementos pendientes. Dentro de la **instrucción foreach**, los enlaces se aplican a los elementos de la lista.
- **visible**: se usa para alternar la visibilidad. Oculte el marcado cuando una colección esté vacía o haga que el mensaje de error esté visible.
- **valor**: se usa para rellenar los valores de formulario.
- **click**: enlaza un evento click a una función en el modelo de vista.

## <a name="anti-csrf-protection"></a>Protección contra CSRF

La falsificación de solicitudes entre sitios (CSRF) es un ataque en el que un sitio malintencionado envía una solicitud a un sitio vulnerable en el que el usuario ha iniciado sesión actualmente. Para ayudar a evitar ataques CSRF, ASP.NET MVC usa *tokens antifalsificación*, también denominados tokens de comprobación de solicitudes. La idea es que el servidor coloca un token generado aleatoriamente en una página web. Cuando el cliente envía datos al servidor, debe incluir este valor en el mensaje de solicitud.

Los tokens antifalsificación funcionan porque la página malintencionada no puede leer los tokens del usuario debido a las directivas del mismo origen. (Las directivas del mismo origen impiden que los documentos hospedados en dos sitios diferentes accedan al contenido de los demás).

ASP.NET MVC proporciona compatibilidad integrada para tokens antifalsificación, a través de la clase [antifalsificación](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx) y el atributo [[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx) . Actualmente, esta funcionalidad no está integrada en la API Web. Sin embargo, la plantilla de SPA incluye una implementación personalizada para la API Web. Este código se define en la clase `ValidateHttpAntiForgeryTokenAttribute`, que se encuentra en la carpeta filtros de la solución. Para obtener más información acerca de anti-CSRF en Web API, consulte [prevención de ataques de falsificación de solicitud entre sitios (CSRF)](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="conclusion"></a>Conclusión

La plantilla de SPA está diseñada para ayudarle a empezar a escribir rápidamente aplicaciones web modernas e interactivas. Usa la biblioteca Knockout. js para separar la presentación (marcado HTML) de los datos y la lógica de la aplicación. Pero el knockout no es la única biblioteca de JavaScript que puede usar para crear un SPA. Si desea explorar otras opciones, eche un vistazo a las [plantillas de spa creadas por la comunidad](../templates/index.md).
