---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controles de servidor | Microsoft Docs
author: microsoft
description: ASP.NET 2,0 mejora los controles de servidor de muchas maneras. En este módulo, trataremos algunos de los cambios arquitectónicos en la forma en que ASP.NET 2,0 y Visual Studio 200...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: c02a633013f061c09141d4f98871848c011a799e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521095"
---
# <a name="server-controls"></a>Controles de servidor

por [Microsoft](https://github.com/microsoft)

> ASP.NET 2,0 mejora los controles de servidor de muchas maneras. En este módulo, trataremos algunos de los cambios arquitectónicos en la forma en que ASP.NET 2,0 y Visual Studio 2005 tratan con los controles de servidor.

ASP.NET 2,0 mejora los controles de servidor de muchas maneras. En este módulo, trataremos algunos de los cambios arquitectónicos en la forma en que ASP.NET 2,0 y Visual Studio 2005 tratan con los controles de servidor.

## <a name="view-state"></a>Estado de vista

El cambio principal en el estado de vista en ASP.NET 2,0 es una reducción drástica del tamaño. Considere una página que solo contiene un control de calendario. Este es el estado de vista en ASP.NET 1,1.

[!code-css[Main](server-controls/samples/sample1.css)]

Ahora se muestra el estado de vista en una página idéntica en ASP.NET 2,0.

[!code-css[Main](server-controls/samples/sample2.css)]

Eso es un cambio bastante significativo y considerando que el estado de vista se lleva a cabo en la conexión, este cambio puede ofrecer a los desarrolladores un aumento significativo del rendimiento. La reducción del tamaño del estado de vista se debe en gran medida a la manera en que se trata internamente. Recuerde que el estado de vista es una cadena codificada en Base64. Para comprender mejor el cambio en el estado de vista en ASP.NET 2,0, echemos un vistazo a los valores descodificados de los ejemplos anteriores.

Este es el estado de vista 1,1 descodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Esto puede parecer un poco como galimatías, pero aquí hay un patrón. En ASP.NET 1. x, usamos caracteres individuales para identificar tipos de datos y valores delimitados mediante el &lt;caracteres &gt;. La "t" en el ejemplo de estado de vista anterior representa un triple. El triple contiene un par de matrices (la "l" representa una ArrayList). Uno de estos matrices contiene un Int32 ("i") con un valor de 1 y otro triple. El triple contiene un par de matrices, etc. Lo importante es recordar que se usan triples que contienen pares, se identifican los tipos de datos a través de una letra y se usan los caracteres &lt; y &gt; como delimitadores.

En ASP.NET 2,0, el estado de vista descodificado es un poco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Debería observar un gran cambio en la apariencia del estado de la vista descodificada. Este cambio tiene varios fundamentos arquitectónicos. El estado de vista de ASP.NET 1. x usó LosFormatter para serializar los datos. En 2,0, usamos la nueva clase ObjectStateFormatter. Esta clase se diseñó específicamente para ayudar en la serialización y deserialización del estado de vista y el estado de control. (El estado del control se tratará en la sección siguiente). Hay muchas ventajas que se obtienen al cambiar el método por el que tienen lugar la serialización y la deserialización. Uno de los más drásticos es el hecho de que, a diferencia de LosFormatter, que usa TextWriter, ObjectStateFormatter usa BinaryWriter. Esto permite a ASP.NET 2,0 almacenar el estado de vista de una serie de bytes en lugar de cadenas. Tome como ejemplo un entero. En ASP.NET 1,1, un entero necesitaba 4 bytes de estado de vista. En ASP.NET 2,0, el mismo entero solo requiere 1 byte. Se han realizado otras mejoras para reducir la cantidad de estado de vista que se almacena. Los valores DateTime, por ejemplo, se almacenan ahora mediante un TickCount en lugar de una cadena.

Como si todo eso fuera suficiente, se ha prestado especial atención al hecho de que uno de los mayores consumidores del estado de vista de 1. x era DataGrid y controles similares. Un importante inconveniente de los controles como el elemento DataGrid, donde se preocupa el estado de vista, es que a menudo contiene grandes cantidades de información repetida. En ASP.NET 1. x, la información repetida simplemente se almacenó una y otra vez, lo que da lugar a un estado de vista redimensionado. En ASP.NET 2,0, usamos la nueva clase IndexedString para almacenar estos datos. Si se repite una cadena, simplemente almacenamos el token de IndexedString y el índice en una tabla de ejecución de objetos IndexedString.

## <a name="control-state"></a>Estado del control

Uno de los principales controles que tenían los desarrolladores con el estado de vista era el tamaño que agregó a la carga de HTTP. Como se mencionó anteriormente, uno de los mayores consumidores del estado de vista es el control DataGrid. Para evitar las enormes cantidades de estado de vista generadas por un control DataGrid, muchos desarrolladores simplemente deshabilitaron el estado de vista de ese control. Desafortunadamente, esa solución no siempre era una buena. El estado de vista de ASP.NET 1. x contiene no solo los datos necesarios para la funcionalidad correcta del control. También contiene información sobre el estado de la interfaz de usuario del control. Esto significa que si desea permitir la paginación en un control DataGrid, debe habilitar el estado de vista aunque no necesite toda la información de la interfaz de usuario que contiene el estado de vista. Se trata de un escenario de todo o nada.

En ASP.NET 2,0, el estado de control resuelve el problema de la misma a través de la introducción del estado del control. El estado del control contiene los datos que son absolutamente necesarios para la funcionalidad adecuada de un control. A diferencia del estado de vista, no se puede deshabilitar el estado del control. Por lo tanto, es importante que los datos que se almacenan en el estado de control se controlen cuidadosamente.

> [!NOTE]
> El estado del control se conserva junto con el estado de vista en el campo \_\_VIEWSTATE Hidden Form.

Este vídeo es un tutorial sobre el estado de vista y el estado de control.

![](server-controls/_static/image1.png)

[Abrir vídeo de pantalla completa](server-controls/_static/state1.wmv)

Para que un control de servidor Lea y escriba en el estado de control, debe seguir tres pasos.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Paso 1: llamar al método RegisterRequiresControlState

El método RegisterRequiresControlState informa a ASP.NET de que un control debe conservar el estado del control. Toma un argumento de tipo control que es el control que se está registrando.

Es importante tener en cuenta que el registro no se conserva de la solicitud. Por consiguiente, se debe llamar a este método en cada solicitud si un control va a conservar el estado de control. Se recomienda llamar al método en OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Paso 2: invalidar SaveControlState

El método SaveControlState guarda los cambios de estado de control de un control desde la última devolución. Devuelve un objeto que representa el estado del control.

## <a name="step-3-override-loadcontrolstate"></a>Paso 3: invalidar LoadControlState

El método LoadControlState carga el estado guardado en un control. El método toma un argumento de tipo Object que contiene el estado guardado del control.

## <a name="full-xhtml-compliance"></a>Compatibilidad completa con XHTML

Cualquier desarrollador web conoce la importancia de los estándares en las aplicaciones Web. Con el fin de mantener un entorno de desarrollo basado en estándares, ASP.NET 2,0 es totalmente compatible con XHTML. Por lo tanto, todas las etiquetas se representan de acuerdo con los estándares XHTML en exploradores que admiten HTML 4,0 o superior.

La definición DOCTYPE en ASP.NET 1,1 fue la siguiente:

[!code-html[Main](server-controls/samples/sample6.html)]

En ASP.NET 2,0, la definición DOCTYPE predeterminada es la siguiente:

[!code-html[Main](server-controls/samples/sample7.html)]

Si lo desea, puede modificar la compatibilidad predeterminada de XHTML a través del nodo xhtmlConformance en el archivo de configuración. Por ejemplo, el siguiente nodo del archivo Web. config cambiará la compatibilidad con XHTML a XHTML 1,0 STRICT:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si lo desea, también puede configurar ASP.NET para usar la configuración heredada que se usa en ASP.NET 1. x como se indica a continuación:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Representación adaptable mediante adaptadores

En ASP.NET 1. x, el archivo de configuración contenía una &lt;browserCaps&gt; sección que rellenó un objeto HttpBrowserCapabilities. Este objeto permitió a un desarrollador determinar qué dispositivo está realizando una solicitud determinada y representar el código adecuadamente. En ASP.NET 2,0, el modelo ha mejorado y ahora usa la nueva clase ControlAdapter. La clase ControlAdapter invalida los eventos del ciclo de vida del control y controla la representación de los controles en función de las capacidades del agente de usuario. Las capacidades de un agente de usuario específico se definen mediante un archivo de definición del explorador (un archivo con una extensión de archivo. browser) almacenado en el c:\Windows\Microsoft.NET\Framework\v2.0.\*\*\*\*carpeta \CONFIG\Browsers

> [!NOTE]
> La clase ControlAdapter es una clase abstracta.

De forma similar a la sección &lt;browserCaps&gt; de 1. x, el archivo de definición del explorador usa una expresión regular para analizar la cadena de agente de usuario con el fin de identificar el explorador que lo solicita. En él se definen las capacidades específicas de ese agente de usuario. ControlAdapter representa el control a través del método Render. Por consiguiente, si invalida el método Render, no debe llamar a render en la clase base. Esto puede hacer que la representación se realice dos veces, una para el adaptador y otra para el propio control.

## <a name="developing-a-custom-adapter"></a>Desarrollo de un adaptador personalizado

Puede desarrollar su propio adaptador personalizado heredando de ControlAdapter. Además, puede heredar de la clase abstracta PageAdapter en los casos en los que se necesita un adaptador para una página. La asignación de controles al adaptador personalizado se realiza a través del elemento &lt;controlAdapters&gt; en el archivo de definición del explorador. Por ejemplo, el siguiente código XML de un archivo de definición del explorador asigna el control de menú a la clase MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Con este modelo, es bastante fácil para un desarrollador de controles tener como destino un determinado dispositivo o explorador. También es bastante sencillo para un desarrollador tener un control completo sobre cómo se representan las páginas en cada dispositivo.

## <a name="per-device-rendering"></a>Representación por dispositivo

Las propiedades de control de servidor de ASP.NET 2,0 se pueden especificar por dispositivo mediante un prefijo específico del explorador. Por ejemplo, el código siguiente cambiará el texto de una etiqueta en función del dispositivo que se use para examinar la página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Cuando la página que contiene esta etiqueta se examina desde Internet Explorer, la etiqueta mostrará el texto que indica que se está explorando desde Internet Explorer. Cuando la página se examina desde Firefox, la etiqueta mostrará el texto "está explorando desde Firefox". Cuando la página se examine desde cualquier otro dispositivo, se mostrará el error "se está explorando desde un dispositivo desconocido". Cualquier propiedad se puede especificar con esta sintaxis especial.

## <a name="setting-focus"></a>Establecer el foco

Los desarrolladores de ASP.NET 1. x frecuentes preguntan cómo establecer el foco inicial en un control determinado. Por ejemplo, en una página de inicio de sesión, es útil que el cuadro de texto ID. de usuario obtenga el foco cuando se cargue la página por primera vez. En ASP.NET 1. x, para ello es necesario escribir algún script del lado cliente. Aunque este tipo de script es una tarea trivial, ya no es necesario en ASP.NET 2,0 gracias al método SetFocus. El método SetFocus toma un argumento que indica el control que debe recibir el foco. Este argumento puede ser el identificador de cliente del control como una cadena o el nombre del control de servidor como un objeto de control. Por ejemplo, para establecer el foco inicial en un control de cuadro de texto denominado txtUserID al cargar la página por primera vez, agregue el siguiente código a la página\_carga:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--o bien

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2,0 usa el controlador WebResource. axd (descrito anteriormente) para representar una función del lado cliente que establece el foco. El nombre de la función del lado cliente es WebForm\_autofocus como se muestra aquí:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, puede utilizar el método Focus para un control para establecer el foco inicial en ese control. El método Focus deriva de la clase control y está disponible para todos los controles ASP.NET 2,0. También es posible establecer el foco en un control determinado cuando se produce un error de validación. Esto se tratará en un módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Nuevos controles de servidor en ASP.NET 2,0

A continuación se muestran nuevos controles de servidor en ASP.NET 2,0. Veremos más detalladamente algunos de ellos en módulos posteriores.

## <a name="imagemap-control"></a>ImageMap (control)

El control ImageMap permite agregar zonas activas a una imagen que puede iniciar una devolución o desplazarse a una dirección URL. Hay tres tipos de zonas activas disponibles: CircleHotSpot, RectangleHotSpot y PolygonHotSpot. Las zonas activas se agregan a través de un editor de colección en Visual Studio o mediante programación en código. No hay ninguna interfaz de usuario disponible para dibujar zonas activas en una imagen. Las coordenadas y el tamaño o el radio de la zona activa se deben especificar mediante declaración. Tampoco hay ninguna representación visual de una zona activa en el diseñador. Si una zona activa está configurada para navegar a una dirección URL, la dirección URL se especifica mediante la propiedad NavigateUrl de la zona activa. En el caso de una zona activa posterior, la propiedad PostBackValue permite pasar una cadena en la devolución que se puede recuperar en el código del servidor.

![Editor de colección de HotSpot en Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: editor de colección de hotspot en Visual Studio

## <a name="bulletedlist-control"></a>BulletedList (control)

El control BulletedList es una lista con viñetas que puede estar enlazada fácilmente a los datos. La lista se puede ordenar (numerar) o desordenar a través de la propiedad BulletStyle. Cada elemento de la lista se representa mediante un objeto ListItem.

![Control BulletedList en Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: control BulletedList en Visual Studio

## <a name="hiddenfield-control"></a>HiddenField (control)

El control HiddenField agrega un campo de formulario oculto a la página, cuyo valor está disponible en el código del servidor. Normalmente se espera que el valor de un campo de formulario oculto permanezca sin cambios entre devoluciones de datos. Sin embargo, es posible que un usuario malintencionado cambie el valor antes de la devolución. Si esto ocurre, el control HiddenField generará el evento ValueChanged. Si tiene información confidencial en el control HiddenField y desea asegurarse de que permanece sin cambios, debe controlar el evento ValueChanged en el código.

## <a name="fileupload-control"></a>FileUpload (control)

El control FileUpload de ASP.NET 2,0 hace posible la carga de archivos en un servidor Web a través de una página de ASP.NET. Este control es muy similar a la clase ASP.NET 1. x HtmlInputFile con algunas excepciones. En ASP.NET 1. x, se recomienda comprobar si la propiedad PostedFile es null para determinar si tenía un buen archivo. El control FileUpload de ASP.NET 2,0 agrega una nueva propiedad HasFile que puede usar para el mismo propósito y es un poco más eficaz.

La propiedad PostedFile todavía está disponible para el acceso a un objeto HttpPostedFile, pero parte de la funcionalidad de HttpPostedFile está ahora disponible de forma intrínseca con el control FileUpload. Por ejemplo, para guardar un archivo cargado en ASP.NET 1. x, debe llamar al método SaveAs en el objeto HttpPostedFile. Con el control FileUpload en ASP.NET 2,0, llamaría al método SaveAs en el propio control FileUpload.

Otro cambio importante en el comportamiento de 2,0 (y probablemente el cambio más significativo) es que ya no es necesario cargar un archivo cargado completo en la memoria antes de guardarlo. En 1. x, cualquier archivo que se haya cargado se guarda completamente en la memoria antes de escribirse en el disco. Esta arquitectura evita la carga de archivos grandes.

En ASP.NET 2,0, el atributo requestLengthDiskThreshold del elemento httpRuntime permite configurar el número de kilobytes que se conservan en un búfer en la memoria antes de escribirlos en el disco.

**Importante**: la documentación de MSDN (y la documentación en otro lugar) especifica que este valor se encuentra en bytes (no en kilobytes) y que el valor predeterminado es 256. El valor se especifica realmente en kilobytes y el valor predeterminado es 80. Si tiene un valor predeterminado de 80K, garantizamos que el búfer no termina en el montón de objetos grandes.

## <a name="wizard-control"></a>Control Wizard

Es bastante habitual encontrar a los desarrolladores de ASP.NET que intentan recopilar información en una serie de "páginas" mediante el uso de paneles o mediante la transferencia de páginas a páginas. Con mayor frecuencia que no, el esfuerzo es frustrante y lleva mucho tiempo. El nuevo control de asistente resuelve los problemas al permitir pasos lineales y no lineales en una interfaz de asistente con la que los usuarios están familiarizados. El control de asistente presenta los formularios de entrada en una serie de pasos. Cada paso es de un tipo concreto especificado por la propiedad StepType del control. Los tipos de pasos disponibles son los siguientes:

| **Tipo de paso** | **Explicación** |
| --- | --- |
| Auto | El asistente determina automáticamente el tipo de paso en función de su posición dentro de la jerarquía de pasos. |
| Iniciar | Primer paso, que se usa a menudo para presentar una instrucción introductoria. |
| Paso | Un paso normal. |
| Finalizar | El paso final, que normalmente se usa para presentar un botón para finalizar el asistente. |
| Completado | Presenta un mensaje que comunica si se ha realizado correctamente o no. |

> [!NOTE]
> El control del asistente realiza un seguimiento de su estado mediante el estado del control ASP.NET. Por lo tanto, la propiedad EnableViewState se puede establecer en false sin perjuicio.

Este vídeo es un tutorial del control del asistente.

![](server-controls/_static/image2.png)

[Abrir vídeo de pantalla completa](server-controls/_static/wizard1.wmv)

## <a name="localize-control"></a>Localize (control)

El control Localize es similar a un control literal. Sin embargo, el control Localize tiene una propiedad de **modo** que controla cómo se representa el marcado que se agrega a él. La propiedad Mode admite los siguientes valores:

| **Modo** | **Explicación** |
| --- | --- |
| Transformación | El marcado se transforma según el protocolo del explorador que realiza la solicitud. |
| Acceso directo | El marcado se representa tal cual. |
| Codificación | El marcado que se agrega al control se codifica mediante HtmlEncode. |

## <a name="multiview-and-view-controls"></a>Controles de vista y multivista

El control MultiView actúa como contenedor de los controles de vista, y el control de vista actúa como un contenedor (como un control de panel) para otros controles. Cada vista de un control MultiView se representa mediante un control de vista único. El primer control de vista de la vista MultiView es View 0, el segundo es View 1, etc. Puede cambiar las vistas especificando el ActiveViewIndex del control MultiView.

## <a name="substitution-control"></a>Control de sustitución

El control de sustitución se utiliza junto con el almacenamiento en caché de ASP.NET. En los casos en los que desea aprovechar las ventajas del almacenamiento en caché, pero tiene partes de una página que se deben actualizar en cada solicitud (es decir, partes de una página que están exentas del almacenamiento en caché), el componente de sustitución proporciona una solución excelente. En realidad, el control no representa ningún resultado por sí mismo. En su lugar, se enlaza a un método en el código del lado servidor. Cuando se solicita la página, se llama al método y se representa el marcado devuelto en lugar del control de sustitución.

El método al que está enlazado el control de sustitución se especifica mediante la propiedad **MethodName** . Ese método debe cumplir los siguientes criterios:

- Debe ser un método estático (compartido en VB).
- Acepta un parámetro de tipo HttpContext.
- Devuelve una cadena que representa el marcado que debe reemplazar el control en la página.

El control de sustitución no tiene la capacidad de modificar ningún otro control de la página, pero sí tiene acceso al HttpContext actual a través de su parámetro.

## <a name="gridview-control"></a>GridView (control)

El control GridView es el sustituto del control DataGrid. Este control se tratará con más detalle en un módulo posterior.

## <a name="detailsview-control"></a>DetailsView (control)

El control DetailsView permite mostrar un único registro de un origen de datos y editarlo o eliminarlo. Se trata con más detalle en un módulo posterior.

## <a name="formview-control"></a>FormView (control)

El control FormView se usa para mostrar un único registro de un origen de orígenes en una interfaz configurable. Se trata con más detalle en un módulo posterior.

## <a name="accessdatasource-control"></a>AccessDataSource (control)

El control AccessDataSource se utiliza para enlazar datos a una base de datos de Access. Se trata con más detalle en un módulo posterior.

## <a name="objectdatasource-control"></a>ObjectDataSource (control)

El control ObjectDataSource se utiliza para admitir una arquitectura de tres niveles, de modo que los controles se pueden enlazar a datos a un objeto comercial de nivel intermedio en lugar de a un modelo de dos niveles, donde los controles se enlazan directamente al origen de datos. Se tratará con más detalle en un módulo posterior.

## <a name="xmldatasource-control"></a>XmlDataSource (control)

El control XmlDataSource se utiliza para enlazar datos a un origen de datos XML. Se trata con más detalle en un módulo posterior.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource (control)

El control SiteMapDataSource proporciona enlace de datos para los controles de navegación del sitio basados en un mapa del sitio. Se tratará con más detalle en un módulo posterior.

## <a name="sitemappath-control"></a>SiteMapPath (control)

El control SiteMapPath muestra una serie de vínculos de navegación a los que se suele hacer referencia como rutas de navegación. Se trata con más detalle en un módulo posterior.

## <a name="menu-control"></a>Control Menu

El control de menú muestra los menús dinámicos mediante DHTML. Se trata con más detalle en un módulo posterior.

## <a name="treeview-control"></a>TreeView (Control)

El control TreeView se usa para mostrar una vista jerárquica de los datos. Se trata con más detalle en un módulo posterior.

## <a name="login-control"></a>Control de inicio de sesión

El control de inicio de sesión proporciona un mecanismo para iniciar sesión en un sitio Web. Se trata con más detalle en un módulo posterior.

## <a name="loginview-control"></a>Control LoginView

El control LoginView permite mostrar diferentes plantillas en función del estado de inicio de sesión de un usuario. Se trata con más detalle en un módulo posterior.

## <a name="passwordrecovery-control"></a>Control PasswordRecovery

El control PasswordRecovery se usa para recuperar las contraseñas olvidadas por los usuarios de una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="loginstatus"></a>LoginStatus

El control LoginStatus muestra el estado de inicio de sesión de un usuario. Se trata con más detalle en un módulo posterior.

## <a name="loginname"></a>LoginName

El control LoginName muestra el nombre de usuario de un usuario después de haber iniciado sesión en una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard es un asistente configurable que ofrece a los usuarios la posibilidad de crear una cuenta de pertenencia a ASP.NET para su uso en una aplicación de ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="changepassword"></a>ChangePassword

El control ChangePassword permite a los usuarios cambiar su contraseña para una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="various-webparts"></a>Varios elementos Web

ASP.NET 2,0 se distribuye con varios elementos web. Estos se tratarán en detalle en un módulo posterior.
