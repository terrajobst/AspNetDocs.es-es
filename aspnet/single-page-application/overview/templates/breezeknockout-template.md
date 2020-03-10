---
uid: single-page-application/overview/templates/breezeknockout-template
title: Plantilla de pan/Knockout | Microsoft Docs
author: madskristensen
description: Plantilla de aplicación de una sola página
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 5bb9ee8f758a25afa6baf3ccbaf7d5864754c7df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449749"
---
# <a name="breezeknockout-template"></a>Plantilla de Breeze/Knockout

por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla MVC de forma más sencilla se escribió con una campana
> 
> [Descargar la plantilla de MVC de muy sencilla/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)

Ha oído hablar de "aplicación de una sola página" (SPA) y se ha preguntado cuál es. Aunque podría leer sobre él, en su lugar puede experimentarlo por sí mismo. Pero, ¿quién tiene tiempo para descargar un ejemplo? Si tiene Visual Studio, tendrá un ejemplo de SPA en funcionamiento en menos de 60 segundos con la plantilla de ASP.NET MVC 4 "aplicación de una sola página".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>¿Cuál es la plantilla de SPA multiestable?

La mayoría de las plantillas de proyecto generan un esqueleto de la aplicación. Para ello, agregue el código y, finalmente, entregue una aplicación en funcionamiento. La plantilla de SPA/extractor es diferente. Genera una aplicación de ejemplo para que se pueda estudiar. Muestra un diseño de aplicaciones SPA y muchas de las técnicas para crear un SPA.

La plantilla de más o de cobertura es una variación de la [plantilla de KNOCKOUTJS Spa](../introduction/knockoutjs-template.md) incluida en la actualización ASP.net and Web Tools 2012,2. La plantilla de SPA de uso sencillo genera una aplicación con la misma experiencia del usuario, pero tiene una implementación diferente, con la administración de datos.

La plantilla KnockoutJS SPA realiza solicitudes de servicio con AJAX de jQuery sin procesar, que es adecuado para una aplicación sencilla. Pero las aplicaciones más sofisticadas tienen requisitos de administración de datos más exigentes. Por ejemplo, la mayoría de las aplicaciones:

- Consultar y volver a consultar el servidor durante una sesión de usuario extendida.
- Agregar filtros de consulta, ordenación y paginación.
- Comparta los mismos datos en varias pantallas.
- Acumular los cambios en muchos objetos y guardarlos como una sola transacción.
- Validar cambios en el cliente para que el usuario pueda corregir los errores antes de confirmar los cambios en la base de datos.

La biblioteca de BreezeJS controla estas tareas, lo que le permite desarrollar la lógica de la aplicación y la experiencia del usuario más importantes.

[**Es una**](http://www.breezejs.com/?utm_source=ms-spa) biblioteca de código abierto para compilar aplicaciones de datos enriquecidas en JavaScript y HTML, los tipos de aplicaciones que históricamente se han entregado como aplicaciones de escritorio independientes.

La plantilla de gran o cobertura le ayuda a tomar ese primer paso crucial hacia una infraestructura de administración de datos más sólida. Genera una aplicación todo de ejemplo que es idéntica a la plantilla KnockoutJS SPA. En el interior, reemplaza el nivel de datos AJAX con rapidez, por lo que puede comparar los dos enfoques en paralelo. Por supuesto, apenas toca el potencial de una aplicación sencilla. Pero verá cómo funciona de forma más sencilla y lo poco necesario para realizar esa transición.

Comencemos.

## <a name="create-a-breezeknockout-template-project"></a>Creación de un proyecto de plantilla de Knockout

Descargue e instale la plantilla haciendo clic en el botón Descargar anterior. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Es posible que tenga que reiniciar Visual Studio.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Proporcione un nombre al proyecto y haga clic en **Aceptar**.

En el Asistente para **nuevo proyecto** , seleccione el extracto de la **cobertura de spa**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar, o presione F5 para ejecutarla con la depuración.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

La lógica de validación se realiza de forma sencilla en el cliente. Los atributos de validación en las clases del modelo de servidor se propagan al cliente y se ejecutan automáticamente antes de que el cliente se comunique con el servidor.

Revise el tráfico de red. Observe que no había llamadas al servidor cuando se detectó un error con rapidez. Cada cambio válido dio como resultado una solicitud POST a "/api/Todo/SaveChanges". Combina con rapidez los cambios y los envía como una única solicitud al método `SaveChanges` del controlador de API Web. Es diferente de la plantilla de KnockoutJS SPA, que hace que las solicitudes PUT, POST y DELETE de cada elemento se realicen individualmente.

## <a name="peek-inside"></a>Inspeccionar dentro

Esta aplicación tiene un lado cliente y un servidor. La pila del lado cliente consta de un poco de HTML y una combinación de módulos de aplicación de JavaScript (en la carpeta "app") más bibliotecas de JavaScript de terceros (en la carpeta "scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Si ha investigado la plantilla de KnockoutJS SPA, debería resultar muy familiar. Céntrese en los cuadros azules. La arquitectura de la interfaz de usuario es Model-View-ViewModel (MVVM), en la que los widgets HTML de la vista se separan correctamente del código de presentación auxiliar en el modelo de vista. Un sistema de enlace de datos (en este caso) coordina la vista y el modelo de vista para que cada uno de ellos pueda realizar su trabajo sin un conocimiento profundo de la otra.

El modelo encapsula los datos de todo. Las entidades del modelo se construyen con rapidez con propiedades observables de Knockout, por lo que se pueden enlazar directamente a widgets en la vista. El modelo de vista pide al contexto de datos que adquiera y guarde las entidades del modelo. El contexto de datos delega la mayor parte del trabajo en pan.

La pila del lado servidor consta de algunos códigos de desarrollador y tres bibliotecas .NET principales: API Web, Entity Framework y Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

La arquitectura básica es la misma que la plantilla KnockoutJS SPA. Sin embargo, la implementación es mucho más sencilla: DTO se eliminaron y la mayoría de los detalles de Entity Framework se han delegado a Breeze.NET.

## <a name="next-steps"></a>Pasos siguientes

Le recomendamos que explore el código, guiado por la [extensa discusión](http://www.breezejs.com/spa-template?utm_source=ms-spa) de las pilas de cliente y de servidor en el sitio web de gran rapidez.

Podría probar la reproducción con una consulta de lado cliente sencilla; Agregue algunos filtros y ordenaciones. Puede agregar más propiedades del modelo y más entidades para mejorar el desarrollo de SPA de un extremo a otro. Cuando esté seguro del diseño, puede anular las características de todo y reemplazarlas por las suyas propias.

Pronto estará listo para el próximo paso: agregar pantallas del lado cliente y navegar entre ellas. Dejará esta plantilla de SPA detrás y convertirá en una pila de SPA más completa, como el [paño activo de John Papa](https://github.com/johnpapa/HotTowel#readme "Paño de acceso frecuente"), que agrega Durandal a la mezcla de la mano y la de la mano.
