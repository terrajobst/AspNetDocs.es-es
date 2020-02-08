---
uid: single-page-application/overview/templates/backbonejs-template
title: Plantilla de red troncal | Microsoft Docs
author: madskristensen
description: Plantilla SPA de backbone. js
ms.author: riande
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 7297db7d5b35a53b40f9d9162960e529a167bd12
ms.sourcegitcommit: e365196c75ce93cd8967412b1cfdc27121816110
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074896"
---
# <a name="backbone-template"></a>Plantilla de Backbone

por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de la red troncal SPA se escribió con Kazi Manzur Rashid
> 
> [Descargar la plantilla de la red troncal. js SPA](https://go.microsoft.com/fwlink/?LinkId=293631)

La plantilla de la red troncal. js SPA está diseñada para empezar a crear rápidamente aplicaciones Web interactivas del lado cliente con [backbone. js.](http://backbonejs.org/)

La plantilla proporciona un esqueleto inicial para desarrollar una aplicación backbone. js en ASP.NET MVC. De forma integrada, proporciona funcionalidad básica de inicio de sesión de usuario, como registro de usuario, Inicio de sesión, restablecimiento de contraseña y confirmación de usuario con plantillas de correo electrónico básicas.

Requisitos:

- [ASP.NET and Web Tools actualización 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Creación de un proyecto de plantilla de red troncal

Descargue e instale la plantilla haciendo clic en el botón Descargar anterior. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Es posible que tenga que reiniciar Visual Studio.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Proporcione un nombre al proyecto y haga clic en **Aceptar**.

![](backbonejs-template/_static/image1.png)

En el Asistente para **nuevo proyecto** , seleccione el proyecto backbone. js Spa.

![](backbonejs-template/_static/image2.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar, o presione F5 para ejecutarla con la depuración.

![](backbonejs-template/_static/image3.png)

Al hacer clic en "mi cuenta", se abre la página de inicio de sesión:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Tutorial: código de cliente

Comencemos con el lado del cliente. Los scripts de la aplicación cliente se encuentran en la carpeta ~/scripts/Application La aplicación se escribe en [TypeScript](http://www.typescriptlang.org/) (archivos. TS) que se compilan en JavaScript (archivos. js).

**Aplicación**

`Application` se define en Application. TS. Este objeto inicializa la aplicación y actúa como espacio de nombres raíz. Mantiene la información de configuración y estado que se comparte entre la aplicación, por ejemplo, si el usuario ha iniciado sesión.

El método `application.start` crea las vistas modales y asocia los controladores de eventos para los eventos de nivel de aplicación, como el inicio de sesión de usuario. A continuación, crea el enrutador predeterminado y comprueba si se ha especificado alguna dirección URL del lado cliente. Si no es así, redirige a la dirección URL predeterminada (#!/).

**Eventos**

Los eventos siempre son importantes al desarrollar componentes de acoplamiento flexible. Las aplicaciones suelen realizar varias operaciones en respuesta a una acción del usuario. La red troncal proporciona eventos integrados con componentes como el modelo, la colección y la vista. En lugar de crear interdependencias entre estos componentes, la plantilla usa un modelo "pub/sub": el objeto `events`, que se define en events. TS, actúa como centro de eventos para publicar y suscribirse a eventos de aplicación. El objeto `events` es un singleton. En el código siguiente se muestra cómo suscribirse a un evento y, a continuación, desencadenar el evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Componente**

En backbone. js, un enrutador proporciona métodos para enrutar páginas del lado cliente y conectarlas a acciones y eventos. La plantilla define un solo enrutador, en enrutador. TS. El enrutador crea las vistas activables y mantiene el estado al cambiar de vista. (Las vistas interactivable se describen en la sección siguiente). Inicialmente, el proyecto tiene dos vistas ficticias, Home y about. También tiene una vista NotFound, que se muestra si no se conoce la ruta.

**Vistas**

Las vistas se definen en ~/scripts/Application/views. Hay dos tipos de vistas: vistas interactivable y vistas de cuadro de diálogo modales. El enrutador invoca las vistas que se pueden ejecutar. Cuando se muestra una vista interactivable, todas las demás vistas que se puede activar se vuelven inactivas. Para crear una vista interactivable, extienda la vista con el `Activable` objeto:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

La extensión con `Activable` agrega dos nuevos métodos a la vista, `activate` y `deactivate`. El enrutador llama a estos métodos para activar y desactivar la vista.

Las vistas modales se implementan como cuadros de diálogo modales de [Twitter bootstrap](https://twitter.github.com/bootstrap/) . Las vistas `Membership` y `Profile` son vistas modales. Cualquier evento de aplicación puede invocar las vistas de modelo. Por ejemplo, en la vista `Navigation`, al hacer clic en el vínculo "mi cuenta", se muestra la vista `Membership` o la vista `Profile`, en función de si el usuario ha iniciado sesión. El `Navigation` asocia los controladores de eventos click a los elementos secundarios que tienen el atributo `data-command`. Este es el marcado HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Este es el código de Navigation. TS para enlazar los eventos:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelos**

Los modelos se definen en ~/scripts/Application/Models. Todos los modelos tienen tres cosas básicas: atributos predeterminados, reglas de validación y un punto final del servidor. Este es un ejemplo típico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Complementos de**

La carpeta ~/scripts/Application/lib contiene algunos complementos de jQuery útiles. El archivo form. ts define un complemento para trabajar con datos del formulario. A menudo es necesario serializar o deserializar los datos del formulario y mostrar los errores de validación del modelo. El complemento Form. ts tiene métodos como `serializeFields`, `deserializeFields`y `showFieldErrors`. En el ejemplo siguiente se muestra cómo serializar un formulario en un modelo.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

El complemento flashbar. ts proporciona varios tipos de mensajes de comentarios al usuario. Los métodos son `$.showSuccessbar`, `$.showErrorbar` y `$.showInfobar`. En segundo plano, usa alertas de arranque de Twitter para mostrar mensajes perfectamente animados.

El complemento confirm. ts reemplaza al cuadro de diálogo de confirmación del explorador, aunque la API es algo diferente:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Tutorial: código de servidor

Ahora veamos el lado del servidor.

**Controladores**

En una aplicación de una sola página, el servidor solo desempeña un rol pequeño en la interfaz de usuario. Normalmente, el servidor representa la página inicial y, a continuación, envía y recibe datos JSON.

La plantilla tiene dos controladores MVC: `HomeController` representa la página inicial y `SupportsController` se usa para confirmar nuevas cuentas de usuario y restablecer contraseñas. Todos los demás controladores de la plantilla son ASP.NET Web API controladores, que envían y reciben datos JSON. De forma predeterminada, los controladores usan la nueva clase de `WebSecurity` para realizar tareas relacionadas con el usuario. Sin embargo, también tienen constructores opcionales que permiten pasar los delegados para estas tareas. Esto facilita la realización de pruebas y permite reemplazar `WebSecurity` por otra cosa, mediante el uso de un contenedor de IoC. Este es un ejemplo:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Vistas

Las vistas están diseñadas para ser modulares: cada sección de una página tiene su propia vista dedicada. En una aplicación de una sola página, es habitual incluir vistas que no tengan ningún controlador correspondiente. Puede incluir una vista mediante una llamada a `@Html.Partial('myView')`, pero esto resulta tedioso. Para facilitar esta tarea, la plantilla define un método auxiliar, `IncludeClientViews`, que representa todas las vistas en una carpeta especificada:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Si no se especifica el nombre de la carpeta, el nombre de carpeta predeterminado es "ClientViews". Si la vista de cliente también usa vistas parciales, asigne un nombre a la vista parcial con un carácter de subrayado (por ejemplo, `_SignUp`). El método `IncludeClientViews` excluye las vistas cuyo nombre comienza por un carácter de subrayado. Para incluir una vista parcial en la vista de cliente, llame a `Html.ClientView('SignUp')` en lugar de `Html.Partial('_SignUp')`.

**Enviar correo electrónico**

Para enviar correo electrónico, la plantilla usa el correo [postal](http://aboutcode.net/postal). Sin embargo, el correo postal se abstrae del resto del código con la interfaz `IMailer`, por lo que se puede reemplazar fácilmente por otra implementación. Las plantillas de correo electrónico se encuentran en la carpeta views/emails. La dirección de correo electrónico del remitente se especifica en el archivo Web. config, en la clave `sender.email` de la sección **appSettings** . Además, cuando `debug="true"` en Web. config, la aplicación no requiere la confirmación por correo electrónico del usuario para acelerar el desarrollo.

## <a name="github"></a>GitHub

También puede encontrar la plantilla de red troncal. js SPA en [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
