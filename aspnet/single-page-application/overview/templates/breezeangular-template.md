---
uid: single-page-application/overview/templates/breezeangular-template
title: Plantilla MULTIPAN/angular | Microsoft Docs
author: madskristensen
description: Plantilla de aplicación de una sola página de un solo ángulo
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: 3e4e63d385a56d51d3d08696782b43d6228f6201
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467191"
---
# <a name="breezeangular-template"></a>Plantilla de Breeze/Angular

por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC multiángulo se escribió con una campana
> 
> [Descargar la plantilla de MVC multisencilla/angular](https://go.microsoft.com/fwlink/?LinkId=286437)

[AngularJS](http://angularjs.org) es una biblioteca de código abierto de Google para compilar aplicaciones de una sola página (Spa). Ofrece el enlace de datos, la inserción de dependencias y la administración de la pantalla. Combínelo con [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), otra biblioteca de código abierto para la administración de datos y modelado de datos, y tiene los ingredientes esenciales para una excelente aplicación cliente HTML/JavaScript.

La plantilla de SPA/angular es una variación de la [plantilla de KNOCKOUTJS Spa](../introduction/knockoutjs-template.md) incluida en la actualización ASP.net and Web Tools 2012,2. Si tiene Visual Studio, tendrá un ejemplo de SPA en funcionamiento en menos de 60 segundos.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

A la vez, la aplicación tiene un aspecto similar al de la plantilla KnockoutJS SPA. Pero es bastante diferente en el capó. La plantilla KnockoutJS usa la cobertura para el enlace de datos y el AJAX sin formato para el acceso a datos. La plantilla multiángulo/angular usa angular para el enlace de datos y la rapidez para el acceso a los datos. Estas bibliotecas habilitan funcionalidades adicionales, como la navegación de páginas y el historial.

Esta es la página acerca de la aplicación:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

En esta página se muestra un registro de eventos en ejecución durante la sesión de usuario actual, incluidos:

- Buscapersona. Tenga en cuenta la creación del controlador de todo en #2 y #7.
- Consultas remotas (#3) y consultas de caché local (#7).
- Guardar las entidades nuevas (#5, #6) y modificadas (#4).
- Cambios validados en el cliente (#9), para que el usuario pueda corregir los errores antes de confirmar los cambios en la base de datos.

Hay más información para explorar en esta plantilla, entre las que se incluyen:

- Carga dinámica de plantillas de vista HTML.
- Enlace de datos personalizado a través de directivas de angular.
- La modularidad y la inserción de dependencias.
- Filtros de consulta, ordenaciones, paginación, proyecciones e inclusión de entidades relacionadas.
- Uso compartido de datos en varias pantallas.
- Guardar varios cambios como una sola transacción.
- Las reglas de validación se propagan automáticamente desde el servidor al cliente de JavaScript.

Comencemos.

## <a name="create-a-breezeangular-template-project"></a>Creación de un proyecto de plantilla de un solo ángulo

Descargue e instale la plantilla haciendo clic en el botón Descargar anterior. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Es posible que tenga que reiniciar Visual Studio.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Proporcione un nombre al proyecto y haga clic en **Aceptar**.

En el Asistente para **nuevo proyecto** , seleccione **multiangular Spa**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar, o presione F5 para ejecutarla con la depuración.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Cuando la aplicación se ejecuta por primera vez, se muestra una pantalla de inicio de sesión. Haga clic en el vínculo "suscribirse" y se abrirá una nueva página en la vista, donde puede especificar un nombre de usuario y una contraseña. (Las páginas de inicio de sesión y registro se compilan con ASP.NET MVC). Cuando se envía el formulario de registro, el servidor genera un TodoList con dos elementos para su cuenta. A continuación, los presenta en una nota amarilla.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Ahora está en el interior de SPA. Todo lo que ve y experimenta mientras manipula todos los clientes se representa y administra en el cliente con la ayuda de la cobertura y la rapidez. Explorar la aplicación como un usuario... pero con el ojo de un desarrollador. Use las herramientas de desarrollo del explorador para capturar el tráfico de red. (En Internet Explorer: Presione F12, seleccione la pestaña **red** y haga clic en **Iniciar captura**). Ahora pruebe lo siguiente:

- Agregue un nuevo elemento de la lista de tareas.
- Haga clic en la etiqueta y edite el título del elemento todo.
- Active una casilla para marcar el elemento como listo. Observe que el cuadro de texto está deshabilitado, por lo que el título ya no es editable.
- Haga clic en la "x" a la derecha de la etiqueta. El elemento desaparece y se elimina de la base de datos.
- Elija otro elemento y borre su título. Obtendrá un error de validación de que el título es obligatorio. Después de una breve pausa, se restaura el título anterior.
- Escriba un título largo de demasiado. Obtendrá un error de validación diferente que el título es demasiado largo.
- Haga clic en el botón "Agregar lista de tareas". Aparecerá una lista nueva a la izquierda de la lista anterior.
- Juegue con el título TodoList, desencadenando las validaciones necesarias y de longitud.
- Haga clic en el cuadro de texto título para borrar el mensaje de error.
- Haga clic en la "x" en el círculo de la esquina superior derecha para eliminar el TodoList y sus todos.
- Haga clic en el vínculo "acerca de" de la esquina superior derecha para ver un registro de estas actividades.

La lógica de validación se realiza de forma sencilla en el cliente. Los atributos de validación en las clases del modelo de servidor se propagan al cliente y se ejecutan automáticamente antes de que el cliente se comunique con el servidor.

Revise el tráfico de red. Observe que no había llamadas al servidor cuando se detectó un error con rapidez. Cada cambio válido dio como resultado una solicitud POST a "/api/Todo/SaveChanges". Combina con rapidez los cambios y los envía como una única solicitud al método `SaveChanges` del controlador de API Web. Es diferente de la plantilla de KnockoutJS SPA, que hace que las solicitudes PUT, POST y DELETE de cada elemento se realicen individualmente.

Además, observe que no hay tráfico de red cuando se cambia entre las páginas TodoList y about. Esto se debe a que la consulta se ha restringido a la memoria caché más local.

## <a name="peek-inside"></a>Inspeccionar dentro

Esta aplicación tiene un lado cliente y un servidor. La pila del lado cliente consta de un poco de HTML y una combinación de módulos de aplicación de JavaScript (en la carpeta "app") más bibliotecas de JavaScript de terceros (en la carpeta "scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

La arquitectura de la interfaz de usuario separa los widgets HTML de las vistas del código de presentación auxiliar en los controladores. El sistema de enlace de datos angular coordina las vistas y los controladores para que cada uno de ellos pueda realizar su trabajo sin un conocimiento profundo de los demás.

El controlador pide al contexto de datos que adquiera y guarde las entidades del modelo. El contexto de datos delega la mayor parte del trabajo en pan, que construye objetos del modelo de seguimiento propio a partir de los resultados de la consulta JSON.

La pila del lado servidor consta de algunos códigos de desarrollador y tres bibliotecas .NET principales: API Web, Entity Framework y Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

La arquitectura básica es la misma que la plantilla KnockoutJS SPA. Sin embargo, la implementación es mucho más sencilla: DTO se eliminaron y la mayoría de los detalles de Entity Framework se han delegado a Breeze.NET.

## <a name="next-steps"></a>Pasos siguientes

Le recomendamos que explore el código, guiado por la [extensa discusión](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) de las pilas de cliente y de servidor en el sitio web de gran rapidez.

Podría probar la reproducción con una consulta de lado cliente sencilla; Agregue algunos filtros y ordenaciones. Puede agregar más propiedades del modelo y más entidades para mejorar el desarrollo de SPA de un extremo a otro. Cuando esté seguro del diseño, puede anular las características de todo y reemplazarlas por las suyas propias.

¡Que disfrute programando!
