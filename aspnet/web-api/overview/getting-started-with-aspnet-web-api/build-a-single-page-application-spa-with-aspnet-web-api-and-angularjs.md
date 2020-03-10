---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Laboratorio práctico: compilar una aplicación de una sola página (SPA) con ASP.NET Web API y angular. js-ASP.NET 4. x'
author: rick-anderson
description: 'Código paso a paso: compilar una aplicación de una sola página (SPA) con ASP.NET Web API y angular. js para ASP.NET 4. x.'
ms.author: riande
ms.date: 09/30/2015
ms.custom: seoapril2019
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 86833a890da759e489dd11dc9afb128a9b7a75e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448765"
---
# <a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratorio práctico: compilar una aplicación de una sola página (SPA) con ASP.NET Web API y angular. js

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico muestra cómo compilar una aplicación de una sola página (SPA) con ASP.NET Web API y angular. js para ASP.NET 4. x.

En este laboratorio de la mano, aprovechará las ventajas de esas tecnologías para implementar un quiz práctico, un sitio web de curiosidades basado en el concepto de SPA. Primero implementará el nivel de servicio con ASP.NET Web API para exponer los puntos de conexión necesarios para recuperar las preguntas de prueba y almacenar las respuestas. Después, creará una interfaz de usuario enriquecida y con una gran capacidad de respuesta mediante los efectos de transformación AngularJS y CSS3.

En las aplicaciones web tradicionales, el cliente (explorador) inicia la comunicación con el servidor solicitando una página. A continuación, el servidor procesa la solicitud y envía el código HTML de la página al cliente. En interacciones posteriores con la página, por ejemplo, el usuario navega a un vínculo o envía un formulario con datos, se envía una nueva solicitud al servidor y el flujo se inicia de nuevo: el servidor procesa la solicitud y envía una nueva página al explorador en respuesta a la nueva solicitud de acción. Ed por el cliente.
> 
> En las aplicaciones de una sola página (Spa), toda la página se carga en el explorador después de la solicitud inicial, pero las interacciones posteriores tienen lugar a través de las solicitudes Ajax. Esto significa que el explorador solo tiene que actualizar la parte de la página que ha cambiado; no es necesario volver a cargar toda la página. El enfoque SPA reduce el tiempo que tarda la aplicación en responder a las acciones del usuario, lo que da lugar a una experiencia más fluida.
> 
> La arquitectura de un SPA implica ciertos desafíos que no están presentes en las aplicaciones web tradicionales. Sin embargo, las tecnologías emergentes como ASP.NET Web API, marcos de JavaScript como AngularJS y nuevas características de estilo proporcionadas por CSS3 facilitan el diseño y la compilación de spa.
> 
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Creación de un servicio ASP.NET Web API para enviar y recibir datos JSON
- Crear una interfaz de usuario con capacidad de respuesta con AngularJS
- Mejora de la experiencia de interfaz de usuario con transformaciones CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio práctico, es necesario lo siguiente:

- [Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/) o superior

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Con el fin de ejecutar los ejercicios de este laboratorio práctico, deberá configurar el entorno primero.

1. Abra el explorador de Windows y vaya a la carpeta de **origen** del laboratorio.
2. Haga clic con el botón derecho en **setup. cmd** y seleccione **Ejecutar como administrador** para iniciar el proceso de instalación que configurará el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha comprobado todas las dependencias de este laboratorio antes de ejecutar el programa de instalación.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usar los fragmentos de código

En todo el documento de laboratorio, se le indicará que inserte bloques de código. Para su comodidad, la mayor parte de este código se proporciona como fragmentos de Visual Studio Code, a los que puede acceder desde Visual Studio 2013 para evitar tener que agregarlo manualmente.

> [!NOTE]
> Cada ejercicio va acompañado de una solución de inicio que se encuentra en la carpeta **Inicio** del ejercicio que le permite seguir cada ejercicio con independencia de los demás. Tenga en cuenta que los fragmentos de código que se agregan durante un ejercicio no se encuentran en estas soluciones de inicio y puede que no funcionen hasta que haya completado el ejercicio. Dentro del código fuente de un ejercicio, también encontrará una carpeta **final** que contiene una solución de Visual Studio con el código que es el resultado de completar los pasos del ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional a medida que trabaja en este laboratorio práctico.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Creación de una API Web](#Exercise1)
2. [Creación de una interfaz SPA](#Exercise2)

Tiempo estimado para completar este laboratorio: **60 minutos**

> [!NOTE]
> Al iniciar Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñada para coincidir con un estilo de desarrollo determinado y determina los diseños de ventana, el comportamiento del editor, los fragmentos de código de IntelliSense y las opciones del cuadro de diálogo. En los procedimientos de este laboratorio se describen las acciones necesarias para realizar una tarea determinada en Visual Studio cuando se usa la colección de **configuración de desarrollo general** . Si elige una colección de valores de configuración diferente para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Ejercicio 1: creación de una API Web

Una de las partes principales de un SPA es el nivel de servicio. Es responsable de procesar las llamadas Ajax enviadas por la interfaz de usuario y devolver los datos en respuesta a esa llamada. Los datos recuperados deben presentarse en un formato legible para el equipo para que el cliente pueda analizarlos y usarlos.

El marco de trabajo de la API Web forma parte de la pila de ASP.NET y está diseñado para facilitar la implementación de servicios HTTP, por lo general, el envío y la recepción de datos con formato JSON o XML a través de una API de RESTful. En este ejercicio creará el sitio web para hospedar la aplicación de prueba de información y, a continuación, implementará el servicio back-end para exponer y conservar los datos de la prueba mediante ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tarea 1: creación del proyecto inicial para una prueba de experto

En esta tarea empezará a crear un nuevo proyecto de ASP.NET MVC con compatibilidad para ASP.NET Web API basado en el tipo de proyecto **ASP.net** que viene con Visual Studio. **Una ASP.net** unifica todas las tecnologías de ASP.net y ofrece la opción de combinarlas y coincidentes según sea necesario. A continuación, agregará las clases de modelo del Entity Framework y el inicializador de base de datos para insertar las preguntas de la prueba.

1. Abra **Visual Studio Express 2013 para web** y seleccione **archivo | Nuevo proyecto...** para iniciar una nueva solución.

    ![Crear un nuevo proyecto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "Crear un proyecto nuevo")

    *Crear un nuevo proyecto*
2. En el cuadro de diálogo **nuevo proyecto** , seleccione **aplicación Web ASP.net** en **el C# objeto visual | Pestaña web** . Asegúrese de que **.NET Framework 4,5** está seleccionado, asígnele el nombre *GeekQuiz*, elija una **Ubicación** y haga clic en **Aceptar**.

    ![Creación de un nuevo proyecto de aplicación Web de ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "Creación de un nuevo proyecto de aplicación Web de ASP.NET")

    *Creación de un nuevo proyecto de aplicación Web de ASP.NET*
3. En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione la plantilla **MVC** y seleccione la opción **API Web** . Además, asegúrese de que la opción **autenticación** está establecida en **cuentas de usuario individuales**. Haga clic en **Aceptar** para continuar.

    ![Crear un nuevo proyecto con la plantilla MVC, incluidos los componentes de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Crear un nuevo proyecto con la plantilla MVC, incluidos los componentes de la API Web*
4. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Models** del proyecto **GeekQuiz** y seleccione **Agregar | Elemento existente..** ..

    ![Agregar un elemento existente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "Agregar un elemento existente")

    *Agregar un elemento existente*
5. En el cuadro de diálogo **Agregar elemento existente** , vaya a la carpeta **source/assets/Models** y seleccione todos los archivos. Haga clic en **Agregar**.

    ![Agregar los recursos del modelo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "Agregar los recursos del modelo")

    *Agregar los recursos del modelo*

    > [!NOTE]
    > Agregando estos archivos, agregará el modelo de datos, el contexto de la base de datos del Entity Framework y el inicializador de base de datos para la aplicación de prueba de información.
    > 
    > **Entity Framework (EF)** es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional. Puede obtener más información sobre Entity Framework [aquí](../../../entity-framework.md).
    > 
    > La siguiente es una descripción de las clases que acaba de agregar:
    > 
    > - **TriviaOption:** representa una única opción asociada a una pregunta de prueba.
    > - **TriviaQuestion:** representa una pregunta de quiz y expone las opciones asociadas a través de la propiedad **Options** .
    > - **TriviaAnswer:** representa la opción seleccionada por el usuario en respuesta a una pregunta de prueba.
    > - **TriviaContext:** representa el contexto de base de datos del Entity Framework de la aplicación de prueba de la que se trata. Esta clase deriva de **DContext** y expone propiedades **DbSet** que representan colecciones de las entidades descritas anteriormente.
    > - **TriviaDatabaseInitializer:** la implementación del inicializador Entity Framework para la clase **TriviaContext** que hereda de **CreateDatabaseIfNotExists**. El comportamiento predeterminado de esta clase es crear la base de datos solo si no existe, insertando las entidades especificadas en el método de **inicialización** .
6. Abra el archivo **global.asax.CS** y agregue la siguiente instrucción using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Agregue el código siguiente al principio de la **aplicación\_método Start** para establecer **TriviaDatabaseInitializer** como el inicializador de base de datos.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modifique el controlador **Home** para restringir el acceso a los usuarios autenticados. Para ello, abra el archivo **HomeController.CS** en la carpeta **Controllers** y agregue el atributo **Authorize** a la definición de clase **HomeController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > El filtro de **autorización** comprueba para ver si el usuario está autenticado. Si el usuario no está autenticado, devuelve el código de Estado HTTP 401 (no autorizado) sin invocar la acción. Puede aplicar el filtro globalmente, en el nivel de controlador o en el nivel de acciones individuales.
9. Ahora personalizará el diseño de las páginas web y la personalización de marca. Para ello, abra el archivo **\_layout. cshtml** dentro de las **vistas | Carpeta compartida** y actualice el contenido de la **&lt;título&gt;** elemento reemplazando la *aplicación ASP.net* por una prueba de uso *experto*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. En el mismo archivo, actualice la barra de navegación; para ello, quite los vínculos *About* y *Contact* y cambie el nombre del vínculo *Home* a *Replay*. Además, cambie el nombre del vínculo de nombre de la *aplicación* a una *prueba de experto*. El código HTML de la barra de navegación debería ser similar al código siguiente.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Actualice el pie de página de la página de diseño sustituyendo la *aplicación ASP.net* por una *prueba de experto*. Para ello, reemplace el contenido del elemento **&lt;footer&gt;** por el siguiente código resaltado.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tarea 2: creación de la API Web de TriviaController

En la tarea anterior, creó la estructura inicial de la aplicación Web de la prueba integrada. Ahora creará un servicio de API web simple que interactúe con el modelo de datos de quiz y exponga las siguientes acciones:

- **Obtener/API/Trivia**: recupera la siguiente pregunta de la lista de cuestionarios a la que debe responder el usuario autenticado.
- **Post/API/Trivia**: almacena la respuesta de la prueba especificada por el usuario autenticado.

Usará las herramientas de scaffolding de ASP.NET que proporciona Visual Studio para crear la línea de base para la clase de controlador de API Web.

1. Abra el archivo **WebApiConfig.CS** dentro de la **aplicación\_** carpeta de inicio. Este archivo define la configuración del servicio de API Web, como el modo en que las rutas se asignan a las acciones del controlador de API Web.
2. Agregue la siguiente instrucción using al principio del archivo.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Agregue el siguiente código resaltado al método **Register** para configurar globalmente el formateador para los datos JSON recuperados por los métodos de acción de la API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver** convierte automáticamente los nombres de propiedad en mayúsculas y minúsculas *Camel* , que es la Convención General para los nombres de propiedad en JavaScript.
4. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Controllers** del proyecto **GeekQuiz** y seleccione **Agregar | Nuevo elemento con scaffolding.** ...

    ![Crear un nuevo elemento con scaffolding](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "Crear un nuevo elemento con scaffolding")

    *Crear un nuevo elemento con scaffolding*
5. En el cuadro de diálogo **Agregar scaffold** , asegúrese de que el nodo **común** esté seleccionado en el panel izquierdo. A continuación, seleccione la plantilla **API Web 2-Empty** en el panel central y haga clic en **Agregar**.

    ![Selección de la plantilla vacía del controlador de Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Selección de la plantilla vacía del controlador de Web API 2")

    *Selección de la plantilla vacía del controlador de Web API 2*

    > [!NOTE]
    > **ASP.net scaffolding** es un marco de generación de código para aplicaciones Web de ASP.net. Visual Studio 2013 incluye generadores de código preinstalados para proyectos de MVC y API Web. Debe utilizar la técnica scaffolding en el proyecto si desea agregar rápidamente código que interactúe con los modelos de datos con el fin de reducir la cantidad de tiempo necesario para desarrollar operaciones de datos estándar.
    > 
    > El proceso de scaffolding también garantiza que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si comienza con un proyecto de ASP.NET vacío y, a continuación, usa la técnica scaffolding para agregar un controlador de API Web, los paquetes de NuGet y las referencias de API Web necesarios se agregan automáticamente al proyecto.
6. En el cuadro de diálogo **Agregar controlador** , escriba *TriviaController* en el cuadro de texto **nombre del controlador** y haga clic en **Agregar**.

    ![Adición del controlador de curiosidad](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "Adición del controlador de curiosidad")

    *Adición del controlador de curiosidad*
7. El archivo **TriviaController.CS** se agrega a la carpeta **Controllers** del proyecto **GeekQuiz** , que contiene una clase **TriviaController** vacía. Agregue las siguientes instrucciones using al principio del archivo.

    (Fragmento de código- *AspNetWebApiSpa-EX1-TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Agregue el código siguiente al principio de la clase **TriviaController** para definir, inicializar y desechar la instancia de **TriviaContext** en el controlador.

    (Fragmento de código- *AspNetWebApiSpa-EX1-TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > El método **Dispose** de **TriviaController** invoca el método **Dispose** de la instancia de **TriviaContext** , que garantiza que todos los recursos utilizados por el objeto de contexto se liberan cuando la instancia de **TriviaContext** se desecha o se recolecta como elemento no utilizado. Esto incluye el cierre de todas las conexiones de base de datos abiertas por Entity Framework.
9. Agregue el siguiente método auxiliar al final de la clase **TriviaController** . Este método recupera la siguiente pregunta de prueba de la base de datos a la que debe responder el usuario especificado.

    (Fragmento de código- *AspNetWebApiSpa-EX1-TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Agregue el siguiente método de acción **Get** a la clase **TriviaController** . Este método de acción llama al método auxiliar **NextQuestionAsync** definido en el paso anterior para recuperar la siguiente pregunta del usuario autenticado.

    (Fragmento de código- *AspNetWebApiSpa-EX1-TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Agregue el siguiente método auxiliar al final de la clase **TriviaController** . Este método almacena la respuesta especificada en la base de datos y devuelve un valor booleano que indica si la respuesta es correcta o no.

    (Fragmento de código- *AspNetWebApiSpa-EX1-TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Agregue el siguiente método de acción **post** a la clase **TriviaController** . Este método de acción asocia la respuesta al usuario autenticado y llama al método auxiliar **StoreAsync** . A continuación, envía una respuesta con el valor booleano devuelto por el método auxiliar.

    (Fragmento de código- *AspNetWebApiSpa-EX1-TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modifique el controlador de API Web para restringir el acceso a los usuarios autenticados agregando el atributo **Authorize** a la definición de clase **TriviaController** .

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecución de la solución

En esta tarea, comprobará que el servicio de API Web que creó en la tarea anterior funciona según lo previsto. Usará el **herramientas de desarrollo F12** de Internet Explorer para capturar el tráfico de red e inspeccionar la respuesta completa del servicio de API Web.

> [!NOTE]
> Asegúrese de que **Internet Explorer** está seleccionado en el botón **iniciar** situado en la barra de herramientas de Visual Studio.
> 
> ![Opción de Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)

1. Presione **F5** para ejecutar la solución. La página de **Inicio de sesión** debe aparecer en el explorador.

    > [!NOTE]
    > Cuando se inicia la aplicación, se desencadena la ruta MVC predeterminada, que se asigna de forma predeterminada a la acción de **Índice** de la clase **HomeController** . Puesto que **HomeController** está restringido a los usuarios autenticados (Recuerde que representaba esa clase con el atributo **Authorize** en el ejercicio 1) y que todavía no hay ningún usuario autenticado, la aplicación redirige la solicitud original a la página de inicio de sesión.

    ![Ejecutar la solución](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "Ejecutar la solución")

    *Ejecutar la solución*
2. Haga clic en **registrar** para crear un nuevo usuario.

    ![Registro de un nuevo usuario](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "Registro de un nuevo usuario")

    *Registro de un nuevo usuario*
3. En la página **registrar** , escriba un **nombre de usuario** y una **contraseña**y, a continuación, haga clic en **registrar**.

    ![Página de registro](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "Página de registro")

    *Página de registro*
4. La aplicación registra la nueva cuenta y el usuario se autentica y redirige de nuevo a la Página principal.

    ![El usuario está autenticado](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "Usuario autenticado")

    *El usuario está autenticado*
5. En el explorador, presione **F12** para abrir el panel de **herramientas de desarrollo** . Presione **Ctrl + 4** o haga clic en el icono de **red** y, a continuación, haga clic en el botón de flecha verde para iniciar la captura del tráfico de red.

    ![Iniciando captura de red de API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "Iniciando captura de red de API Web")

    *Iniciando captura de red de API Web*
6. Anexe **API/curiosidad** a la dirección URL en la barra de direcciones del explorador. Ahora se inspeccionarán los detalles de la respuesta del método **Get** Action en **TriviaController**.

    ![Recuperación de los datos de la siguiente pregunta a través de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Recuperación de los datos de la siguiente pregunta a través de la API Web")

    *Recuperación de los datos de la siguiente pregunta a través de la API Web*

    > [!NOTE]
    > Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado. Deje abierto el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de programadores.
7. Ahora se inspeccionará el cuerpo de la respuesta. Para ello, haga clic en la pestaña **detalles** y, a continuación, haga clic en **cuerpo de respuesta**. Puede comprobar que los datos descargados son un objeto con las **Opciones** de propiedades (que es una lista de objetos **TriviaOption** ), el **identificador** y el **título** que corresponden a la clase **TriviaQuestion** .

    ![Visualización del cuerpo de respuesta de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Visualización del cuerpo de respuesta de la API Web")

    *Ver el cuerpo de respuesta de la API Web*
8. Vuelva a Visual Studio y presione **Mayús + F5** para detener la depuración.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Ejercicio 2: creación de la interfaz SPA

En este ejercicio, creará primero la parte de front-end web de una prueba de integración, centrándose en la interacción de la aplicación de una sola página mediante **AngularJS**. A continuación, mejorará la experiencia del usuario con CSS3 para realizar animaciones enriquecidas y proporcionará un efecto visual del cambio de contexto al pasar de una pregunta a la siguiente.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tarea 1: creación de la interfaz SPA con AngularJS

En esta tarea utilizará **AngularJS** para implementar el lado cliente de la aplicación de prueba de la práctica. **AngularJS** es un marco de JavaScript de código abierto que aumenta las aplicaciones basadas en explorador con *la funcionalidad del controlador de vista de modelos* (MVC), lo que facilita el desarrollo y las pruebas.

Comenzará instalando AngularJS desde la consola del administrador de paquetes de Visual Studio. A continuación, creará el controlador para proporcionar el comportamiento de la aplicación de quiz y la vista para presentar las preguntas y respuestas de la prueba mediante el motor de plantillas de AngularJS.

> [!NOTE]
> Para obtener más información sobre AngularJS, consulte [[http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).

1. Abra **Visual Studio Express 2013 para web** y abra la solución **GeekQuiz. sln** que se encuentra en la carpeta **source/Ex2-CreatingASPAInterface/Begin** . Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. Abra la **consola del administrador de paquetes** desde **herramientas** > **Administrador de paquetes NuGet**. Escriba el siguiente comando para instalar el paquete de NuGet **AngularJS. Core** .

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **scripts** del proyecto **GeekQuiz** y seleccione **Agregar | Nueva carpeta**. Asigne un nombre a la **aplicación** de carpeta y presione **entrar**.
4. Haga clic con el botón derecho en la carpeta de la **aplicación** que acaba de crear y seleccione **Agregar | Archivo JavaScript**.

    ![Crear un nuevo archivo de JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Crear un nuevo archivo de JavaScript*
5. En el cuadro de diálogo **especificar nombre para el elemento** , escriba *quiz-Controller* en el cuadro de texto **nombre de elemento** y haga clic en **Aceptar**.

    ![Asignar un nombre al nuevo archivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Asignar un nombre al nuevo archivo JavaScript*
6. En el archivo **quiz-Controller. js** , agregue el código siguiente para declarar e inicializar el controlador de AngularJS **QuizCtrl** .

    (Fragmento de código- *AspNetWebApiSpa-Ex2-AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La función constructora del controlador **QuizCtrl** espera un parámetro insertable denominado **$Scope**. El estado inicial del ámbito se debe configurar en la función constructora adjuntando propiedades al objeto **$Scope** . Las propiedades contienen el **modelo de vista**y será accesible para la plantilla cuando se registre el controlador.
    > 
    > El controlador **QuizCtrl** se define dentro de un módulo denominado **QuizApp**. Los módulos son unidades de trabajo que permiten dividir la aplicación en componentes independientes. Las principales ventajas del uso de módulos es que el código es más fácil de entender y facilita las pruebas unitarias, la reusabilidad y el mantenimiento.
7. Ahora agregará comportamiento al ámbito para reaccionar a los eventos desencadenados desde la vista. Agregue el código siguiente al final del controlador **QuizCtrl** para definir la función **nextQuestion** en el objeto **$Scope** .

    (Fragmento de código- *AspNetWebApiSpa-Ex2-AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Esta función recupera la siguiente pregunta de la API Web de **curiosidad** creada en el ejercicio anterior y adjunta los datos de la pregunta al objeto **$Scope** .
8. Inserte el código siguiente al final del controlador **QuizCtrl** para definir la función **sendAnswer** en el objeto **$Scope** .

    (Fragmento de código- *AspNetWebApiSpa-Ex2-AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Esta función envía la respuesta seleccionada por el usuario a la API Web de **curiosidad** y almacena el resultado, es decir, si la respuesta es correcta o no, en el objeto de **$Scope** .
    > 
    > Las funciones **nextQuestion** y **sendAnswer** de arriba usan el objeto AngularJS **$http** para abstraer la comunicación con la API Web mediante el objeto de JavaScript de XMLHttpRequest desde el explorador. AngularJS admite otro servicio que ofrece un nivel más alto de abstracción para realizar operaciones CRUD en un recurso a través de las API de RESTful. El objeto AngularJS **$Resource** tiene métodos de acción que proporcionan comportamientos de alto nivel sin necesidad de interactuar con el objeto **$http** . Considere la posibilidad de usar el objeto de **$Resource** en escenarios que requieran el modelo CRUD (información sobre el uso de, vea la [documentación de $Resource](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. El siguiente paso consiste en crear la plantilla de AngularJS que define la vista de la prueba. Para ello, abra el archivo **index. cshtml** dentro de las **vistas |** Y reemplace el contenido por el código siguiente.

    (Fragmento de código- *AspNetWebApiSpa-Ex2-GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > La plantilla de AngularJS es una especificación declarativa que usa información del modelo y el controlador para transformar el marcado estático en la vista dinámica que el usuario ve en el explorador. A continuación se muestran ejemplos de elementos de AngularJS y atributos de elemento que se pueden usar en una plantilla:
    > 
    > - La directiva **ng-App** indica a AngularJS el elemento DOM que representa el elemento raíz de la aplicación.
    > - La directiva **ng-Controller** asocia un controlador al Dom en el punto en el que se declara la Directiva.
    > - La notación de llaves **{{}}** denota los enlaces a las propiedades de ámbito definidas en el controlador.
    > - La directiva **ng-click** se usa para invocar las funciones definidas en el ámbito en respuesta a los clics del usuario.
10. Abra el archivo **site. CSS** dentro de la carpeta de **contenido** y agregue los siguientes estilos resaltados al final del archivo para proporcionar una apariencia y funcionamiento para la vista de cuestionario.

    (Fragmento de código- *AspNetWebApiSpa-Ex2-GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecución de la solución

En esta tarea, ejecutará la solución con la nueva interfaz de usuario que creó con AngularJS para responder a algunas de las preguntas de la prueba.

1. Presione **F5** para ejecutar la solución.
2. Registre una nueva cuenta de usuario. Para ello, siga los pasos de registro descritos en el ejercicio 1, tarea 3.

    > [!NOTE]
    > Si usa la solución del ejercicio anterior, puede iniciar sesión con la cuenta de usuario que creó anteriormente.
3. Debe aparecer la página **principal** , que muestra la primera pregunta de la prueba. Para responder a la pregunta, haga clic en una de las opciones. Esto desencadenará la función **sendAnswer** definida anteriormente, que envía la opción seleccionada a la API Web de **curiosidad** .

    ![Responder a una pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "Responder a una pregunta")

    *Responder a una pregunta*
4. Después de hacer clic en uno de los botones, debe aparecer la respuesta. Haga clic en **pregunta siguiente** para mostrar la siguiente pregunta. Esto desencadenará la función **nextQuestion** definida en el controlador.

    ![Solicitud de la siguiente pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "Solicitud de la siguiente pregunta")

    *Solicitud de la siguiente pregunta*
5. Debería aparecer la siguiente pregunta. Siga respondiendo a las preguntas tantas veces como desee. Después de completar todas las preguntas, debe volver a la primera pregunta.

    ![Otra pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "Otra pregunta")

    *Siguiente pregunta*
6. Vuelva a Visual Studio y presione **Mayús + F5** para detener la depuración.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tarea 3: crear una animación Flip con CSS3

En esta tarea usará las propiedades CSS3 para realizar animaciones enriquecidas agregando un efecto de volteo cuando se responda una pregunta y cuando se recupere la siguiente pregunta.

1. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Content** del proyecto **GeekQuiz** y seleccione **Agregar | Elemento existente..** ..

    ![Agregar un elemento existente a la carpeta de contenido](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "Agregar un elemento existente a la carpeta de contenido")

    *Agregar un elemento existente a la carpeta de contenido*
2. En el cuadro de diálogo **Agregar elemento existente** , vaya a la carpeta **source/assets** y seleccione **flip. CSS**. Haga clic en **Agregar**.

    ![Agregar el archivo flip. CSS desde Assets](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "Agregar el archivo flip. CSS desde Assets")

    *Agregar el archivo flip. CSS desde Assets*
3. Abra el archivo **flip. CSS** que acaba de agregar e inspeccione su contenido.
4. Busque el comentario de **transformación de volteo** . Los estilos que aparecen debajo del comentario usan las transformaciones de **perspectiva** y **giro** de CSS para generar un efecto de&quot; de la tarjeta de &quot;.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Busque el **Panel ocultar detrás durante el comentario de voltear** . El estilo que se encuentra debajo del comentario oculta la parte trasera de las caras cuando está orientada fuera del visor estableciendo la propiedad CSS de visibilidad de la **superficie** en *oculta*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Abra el archivo **BundleConfig.CS** dentro de la **aplicación\_** carpeta de inicio y agregue la referencia al archivo **flip. CSS** en el grupo de estilo **&quot;~/Content/CSS&quot;**

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Presione **F5** para ejecutar la solución e iniciar sesión con sus credenciales.
8. Para responder a una pregunta, haga clic en una de las opciones. Observe el efecto de volteo al realizar la transición entre vistas.

    ![Responder a una pregunta con el efecto de volteo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "Responder a una pregunta con el efecto de volteo")

    *Responder a una pregunta con el efecto de volteo*
9. Haga clic en **pregunta siguiente** para recuperar la siguiente pregunta. El efecto de volteo debe volver a aparecer.

    ![Recuperar la siguiente pregunta con el efecto de volteo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "Recuperar la siguiente pregunta con el efecto de volteo")

    *Recuperar la siguiente pregunta con el efecto de volteo*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido a:

- Creación de un controlador de ASP.NET Web API con scaffolding de ASP.NET
- Implementar una acción de obtención de API Web para recuperar la siguiente pregunta de prueba
- Implementar una acción post de API Web para almacenar las respuestas de las pruebas
- Instalación de AngularJS desde la consola del administrador de paquetes de Visual Studio
- Implementar controladores y plantillas de AngularJS
- Usar transiciones CSS3 para realizar efectos de animación
