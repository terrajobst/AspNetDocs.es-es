---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio práctico: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API | Microsoft Docs'
author: rick-anderson
description: ASP.NET es un marco para compilar sitios web, aplicaciones y servicios mediante tecnologías especializadas como MVC, API Web y otras. Con la expansión ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505459"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratorio práctico: One ASP.NET: Integration ASP.NET Web Forms, MVC y Web API

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

> ASP.NET es un marco para compilar sitios web, aplicaciones y servicios mediante tecnologías especializadas como MVC, API Web y otras. Con el ASP.NET de expansión que ha detectado desde su creación y la necesidad de integrar estas tecnologías, se han realizado esfuerzos recientes en el trabajo hacia **un ASP.net**.
> 
> Visual Studio 2013 presenta un nuevo sistema de proyectos unificado que le permite compilar una aplicación y usar todas las tecnologías de ASP.NET en un proyecto. Esta característica elimina la necesidad de elegir una tecnología al principio de un proyecto y ceñirse a ella y, en su lugar, fomenta el uso de varios marcos de trabajo de ASP.NET dentro de un proyecto.
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Crear un sitio web basado en un tipo de proyecto de **ASP.net**
- Usar marcos de **ASP.net** diferentes como **MVC** y **Web API** en el mismo proyecto
- Identificar los componentes principales de una aplicación **ASP.net**
- Aproveche el marco de **scaffolding de ASP.net** para crear automáticamente controladores y vistas para realizar operaciones CRUD basadas en las clases de modelo
- Exponer el mismo conjunto de información en formatos de equipo y legibles con la herramienta adecuada para cada trabajo

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio práctico, es necesario lo siguiente:

- [Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/) o superior
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [Crear un nuevo proyecto de formularios Web Forms](#Exercise1)
2. [Crear un controlador de MVC mediante scaffolding](#Exercise2)
3. [Creación de un controlador de API Web mediante scaffolding](#Exercise3)

Tiempo estimado para completar este laboratorio: **60 minutos**

> [!NOTE]
> Al iniciar Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñada para coincidir con un estilo de desarrollo determinado y determina los diseños de ventana, el comportamiento del editor, los fragmentos de código de IntelliSense y las opciones del cuadro de diálogo. En los procedimientos de este laboratorio se describen las acciones necesarias para realizar una tarea determinada en Visual Studio cuando se usa la colección de **configuración de desarrollo general** . Si elige una colección de valores de configuración diferente para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Ejercicio 1: crear un nuevo proyecto de formularios Web Forms

En este ejercicio creará un nuevo sitio de formularios Web Forms en Visual Studio 2013 mediante la experiencia de proyecto Unificado de **ASP.net** , que le permitirá integrar fácilmente los componentes Web Forms, MVC y Web API en la misma aplicación. Después, explorará la solución generada e identificará sus partes y, por último, verá el sitio web en acción.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Tarea 1: creación de un sitio nuevo con una experiencia de ASP.NET

En esta tarea empezará a crear un nuevo sitio web en Visual Studio basado en el tipo de proyecto **ASP.net** . **Una ASP.net** unifica todas las tecnologías de ASP.net y ofrece la opción de combinarlas y coincidentes según sea necesario. A continuación, reconocerá los diferentes componentes de los formularios Web Forms, MVC y API Web que se encuentran en paralelo en la aplicación.

1. Abra **Visual Studio Express 2013 para web** y seleccione **archivo | Nuevo proyecto...** para iniciar una nueva solución.

    ![Crear un proyecto nuevo](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Crear un nuevo proyecto*
2. En el cuadro de diálogo **nuevo proyecto** , seleccione **aplicación Web ASP.net** en **el C# objeto visual |** En la pestaña web y asegúrese de que está seleccionado **.NET Framework 4,5** . Asigne al proyecto el nombre *MyHybridSite*, elija una **Ubicación** y haga clic en **Aceptar**.

    ![Nuevo proyecto de aplicación Web de ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Creación de un nuevo proyecto de aplicación Web de ASP.NET*
3. En el cuadro de diálogo **nuevo proyecto de ASP.net** , seleccione la plantilla de **formularios Web Forms** y seleccione las opciones **MVC** y **API Web** . Además, asegúrese de que la opción **autenticación** está establecida en **cuentas de usuario individuales**. Haga clic en **Aceptar** para continuar.

    ![Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de Web API y MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de Web API y MVC*
4. Ahora puede explorar la estructura de la solución generada.

    ![Explorar la solución generada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Explorar la solución generada*

    1. **Cuenta:** Esta carpeta contiene las páginas del formulario web que se van a registrar, iniciar sesión en y administrar las cuentas de usuario de la aplicación. Esta carpeta se agrega cuando se selecciona la opción de autenticación **cuentas de usuario individuales** durante la configuración de la plantilla de proyecto de formularios Web Forms.
    2. **Modelos:** Esta carpeta contendrá las clases que representan los datos de la aplicación.
    3. **Controladores** y **vistas**: estas carpetas son necesarias para los componentes **ASP.NET MVC** y **ASP.net web API** . Explorará las tecnologías MVC y API Web en los ejercicios siguientes.
    4. Los archivos **default. aspx**, **Contact. aspx** y **About. aspx** son páginas de formulario web predefinidas que se pueden utilizar como puntos de partida para compilar las páginas específicas de la aplicación. La lógica de programación de esos archivos reside en un archivo independiente denominado &quot;archivo de&quot; de código subyacente, que tiene una &quot;. aspx. VB&quot; o &quot;. aspx.cs&quot; extensión (según el idioma usado). La lógica de código subyacente se ejecuta en el servidor y genera dinámicamente la salida HTML de la página.
    5. Las páginas **site. Master** y **site. Mobile. Master** definen la apariencia y el comportamiento estándar de todas las páginas de la aplicación.
5. Haga doble clic en el archivo **default. aspx** para explorar el contenido de la página.

    ![Explorar la página default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Explorar la página default. aspx*

    > [!NOTE]
    > La Directiva de **Página** en la parte superior del archivo define los atributos de la página de formularios Web Forms. Por ejemplo, el atributo **MasterPageFile** especifica la ruta de acceso a la página maestra (en este caso, la página *site. Master* ) y el atributo **Inherits** define la clase de código subyacente de la página que se va a heredar. Esta clase se encuentra en el archivo determinado por el atributo **Codebehind** .
    > 
    > El control **asp: Content** contiene el contenido real de la página (texto, marcado y controles) y se asigna a un control **asp: ContentPlaceHolder** en la página maestra. En este caso, el contenido de la página se representará dentro del control *MainContent* definido en la página *site. Master* .
6. Expanda la carpeta de **Inicio del\_** de la aplicación y observe el archivo **WebApiConfig.CS** . Visual Studio incluye ese archivo en la solución generada porque incluyó la API Web al configurar el proyecto con una plantilla de ASP.NET.
7. Abra el archivo **WebApiConfig.CS** . En la clase *WebApiConfig* , encontrará la configuración asociada a la API Web, que asigna rutas http a **los controladores de API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Abra el archivo **RouteConfig.CS** . Dentro del método *RegisterRoutes* encontrará la configuración asociada a MVC, que asigna rutas http a **los controladores MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecución de la solución

En esta tarea, ejecutará la solución generada, explorará la aplicación y algunas de sus características, como la reescritura de direcciones URL y la autenticación integrada.

1. Para ejecutar la solución, presione **F5** o haga clic en el botón **Inicio** situado en la barra de herramientas. La Página principal de la aplicación debe abrirse en el explorador.

    ![Ejecutar la solución](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Compruebe que se invocan las páginas de formularios Web Forms. Para ello, anexe **/Contact.aspx** a la dirección URL en la barra de direcciones y presione **entrar**.

    ![Direcciones URL descriptivas](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Direcciones URL descriptivas*

    > [!NOTE]
    > Como puede ver, la dirección URL cambia a **/Contact**. A partir de **ASP.net 4**, las capacidades de enrutamiento de direcciones URL se agregaron a los formularios Web Forms, por lo que puede escribir direcciones url como *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* en lugar de *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* . Para obtener más información, consulte [enrutamiento de direcciones URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Ahora explorará el flujo de autenticación integrado en la aplicación. Para ello, haga clic en **registrar** en la esquina superior derecha de la página.

    ![Registro de un nuevo usuario](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registro de un nuevo usuario*
4. En la página **registrar** , escriba un **nombre de usuario** y una **contraseña**y, a continuación, haga clic en **registrar**.

    ![Página de registro](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Página de registro*
5. La aplicación registra la nueva cuenta y el usuario está autenticado.

    ![Usuario autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Usuario autenticado*
6. Vuelva a Visual Studio y presione **Mayús + F5** para detener la depuración.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Ejercicio 2: creación de un controlador de MVC mediante scaffolding

En este ejercicio se aprovechará el marco de scaffolding de ASP.NET que proporciona Visual Studio para crear un controlador ASP.NET MVC 5 con acciones y vistas de Razor para realizar operaciones CRUD, sin escribir una sola línea de código. El proceso de scaffolding usará Entity Framework Code First para generar el contexto de datos y el esquema de la base de datos en SQL Database.

**Acerca de Entity Framework Code First**

Entity Framework (EF) es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional.

El flujo de trabajo de modelado de Entity Framework Code First le permite usar sus propias clases de dominio para representar el modelo en el que se basa EF al realizar consultas, seguimiento de cambios y funciones de actualización. Con el flujo de trabajo de desarrollo de Code First, no es necesario iniciar la aplicación mediante la creación de una base de datos o la especificación de un esquema. En su lugar, puede escribir clases estándar de .NET que definan los objetos de modelo de dominio más adecuados para la aplicación y Entity Framework creará la base de datos automáticamente.

> [!NOTE]
> Puede obtener más información sobre Entity Framework [aquí](../../../entity-framework.md).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Tarea 1: crear un nuevo modelo

Ahora definirá una clase **Person** , que será el modelo usado por el proceso de scaffolding para crear el controlador MVC y las vistas. Comenzará creando una clase de modelo **Person** y las operaciones CRUD en el controlador se crearán automáticamente con las características de scaffolding.

1. Abra **Visual Studio Express 2013 para web** y la solución **MyHybridSite. sln** que se encuentra en la carpeta **source/Ex2-MvcScaffolding/Begin** . Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Models** del proyecto **MyHybridSite** y seleccione **Agregar | Clase..** ..

    ![Adición de la clase de modelo Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Adición de la clase de modelo Person*
3. En el cuadro de diálogo **Agregar nuevo elemento** , asigne al archivo el nombre *Person.CS* y haga clic en **Agregar**.

    ![Crear la clase de modelo Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Crear la clase de modelo Person*
4. Reemplace el contenido del archivo **Person.CS** por el código siguiente. Presione **Ctrl + S** para guardar los cambios.

    (Fragmento de código- *BringingTogetherOneAspNet-Ex2-PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto **MyHybridSite** y seleccione **compilar**o presione **Ctrl + Mayús + B** para compilar el proyecto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Tarea 2: creación de un controlador de MVC

Ahora que se ha creado el modelo **Person** , usará el scaffolding de ASP.NET MVC con Entity Framework para crear las acciones y las vistas del controlador CRUD para **Person**.

1. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Controllers** del proyecto **MyHybridSite** y seleccione **Agregar | Nuevo elemento con scaffolding.** ...

    ![Creación de un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Creación de un nuevo controlador con scaffolding*
2. En el cuadro de diálogo **Agregar scaffold** , seleccione **controlador de MVC 5 con vistas, usando Entity Framework** y, a continuación, haga clic en **Agregar.**

    ![Selección del controlador MVC 5 con vistas y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Selección del controlador MVC 5 con vistas y Entity Framework*
3. Establezca *MvcPersonController* como el **nombre del controlador**, seleccione la opción **usar acciones de controlador asincrónica** y seleccione **Person (MyHybridSite. Models)** como **clase de modelo**.

    ![Adición de un controlador de MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Adición de un controlador de MVC con scaffolding*
4. En **clase de contexto de datos**, haga clic en **nuevo contexto de datos.** ...

    ![Crear un nuevo contexto de datos](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Crear un nuevo contexto de datos*
5. En el cuadro de diálogo **nuevo contexto de datos** , asigne al nuevo contexto de datos el nombre *PersonContext* y haga clic en **Agregar**.

    ![Crear el nuevo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Crear el nuevo tipo PersonContext*
6. Haga clic en **Agregar** para crear el nuevo controlador para la **persona** con scaffolding. A continuación, Visual Studio generará las acciones del controlador, el contexto de datos person y las vistas de Razor.

    ![Después de crear el controlador de MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Después de crear el controlador de MVC con scaffolding*
7. Abra el archivo **MvcPersonController.CS** en la carpeta **Controllers** . Tenga en cuenta que los métodos de acción CRUD se han generado automáticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Al activar la casilla **usar acciones de controlador asincrónico** de las opciones de scaffolding en los pasos anteriores, Visual Studio genera métodos de acción asincrónicos para todas las acciones que implican el acceso al contexto de datos person. Se recomienda usar métodos de acción asincrónicos para las solicitudes de ejecución prolongada que no son de CPU para evitar que el servidor Web realice el trabajo mientras se procesa la solicitud.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecución de la solución

En esta tarea, volverá a ejecutar la solución para comprobar que las vistas para **Person** funcionan según lo previsto. Agregará una nueva persona para comprobar que se ha guardado correctamente en la base de datos.

1. Presione **F5** para ejecutar la solución.
2. Vaya a **/MvcPerson**. La vista con scaffolding que muestra la lista de personas debe aparecer.
3. Haga clic en **crear nuevo** para agregar una nueva persona.

    ![Navegar a las vistas de MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navegar a las vistas de MVC con scaffolding*
4. En la vista **crear** , proporcione un **nombre** y una **edad** para la persona y haga clic en **crear**.

    ![Agregar una nueva persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Agregar una nueva persona*
5. La nueva persona se agrega a la lista. En la lista de elementos, haga clic en **detalles** para mostrar la vista de detalles de la persona. A continuación, en la vista de **detalles** , haga clic en **volver a la lista** para volver a la vista de lista.

    ![Vista de detalles de la persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Vista de detalles de la persona*
6. Haga clic en el vínculo **eliminar** para eliminar la persona. En la vista **eliminar** , haga clic en **eliminar** para confirmar la operación.

    ![Eliminación de una persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Eliminación de una persona*
7. Vuelva a Visual Studio y presione **Mayús + F5** para detener la depuración.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Ejercicio 3: creación de un controlador de API Web mediante scaffolding

El marco de trabajo de la API Web forma parte de la pila de ASP.NET y se ha diseñado para facilitar la implementación de servicios HTTP, por lo general, el envío y la recepción de datos con formato JSON o XML a través de una API de RESTful.

En este ejercicio, usará el scaffolding de ASP.NET de nuevo para generar un controlador de API Web. Usará las mismas clases **Person** y **PersonContext** del ejercicio anterior para proporcionar los mismos datos de persona en formato JSON. Verá cómo puede exponer los mismos recursos de maneras diferentes dentro de la misma aplicación ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Tarea 1: creación de un controlador de API Web

En esta tarea, creará un nuevo **controlador de API Web** que expondrá los datos de persona en un formato consumible como JSON.

1. Si aún no está abierto, Abra **Visual Studio Express 2013 para web** y abra la solución **MyHybridSite. sln** que se encuentra en la carpeta **source/Ex3-WebAPI/Begin** . Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.

    > [!NOTE]
    > Si empieza con la solución Begin de ejercicio 3, presione **Ctrl + Mayús + B** para compilar la solución.
2. En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **Controllers** del proyecto **MyHybridSite** y seleccione **Agregar | Nuevo elemento con scaffolding.** ...

    ![Creación de un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Creación de un nuevo controlador con scaffolding*
3. En el cuadro de diálogo **Agregar scaffold** , seleccione **Web API** en el panel izquierdo y, a continuación, el **controlador de Web API 2 con acciones, con Entity Framework** en el panel central y, a continuación, haga clic en **Agregar.**

    ![Selección del controlador de Web API 2 con acciones y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selección del controlador de Web API 2 con acciones y Entity Framework")

    *Selección del controlador de Web API 2 con acciones y Entity Framework*
4. Establezca *ApiPersonController* como el **nombre del controlador**, seleccione la opción **usar acciones de controlador asincrónica** y seleccione **Person (MyHybridSite. Models)** y **PersonContext (MyHybridSite. Models)** como las clases de **contexto de datos** y **modelo** , respectivamente. A continuación, haga clic en **Agregar**.

    ![Adición de un controlador de API Web con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adición de un controlador de API Web con scaffolding")

    *Adición de un controlador de API Web con scaffolding*
5. A continuación, Visual Studio generará la clase **ApiPersonController** con las cuatro acciones CRUD para trabajar con los datos.

    ![Después de crear el controlador de API Web con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Después de crear el controlador de API Web con scaffolding")

    *Después de crear el controlador de API Web con scaffolding*
6. Abra el archivo **ApiPersonController.CS** e inspeccione el método de acción *GetPeople* . Este método consulta el campo dB de tipo **PersonContext** para obtener los datos de las personas.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Observe ahora el comentario sobre la definición del método. Proporciona el URI que expone esta acción que se usará en la siguiente tarea.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > De forma predeterminada, la API Web está configurada para capturar las consultas a la ruta de acceso */API* para evitar colisiones con controladores MVC. Si necesita cambiar esta configuración, consulte [enrutamiento en ASP.net web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecución de la solución

En esta tarea usará las **herramientas de desarrollo F12** de Internet Explorer para inspeccionar la respuesta completa desde el controlador de API Web. Verá cómo puede capturar el tráfico de red para obtener más información sobre los datos de la aplicación.

> [!NOTE]
> Asegúrese de que **Internet Explorer** está seleccionado en el botón **iniciar** situado en la barra de herramientas de Visual Studio.
> 
> ![Opción de Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> Las **herramientas de desarrollo F12** tienen un amplio conjunto de funcionalidades que no se describen en este laboratorio práctico. Si desea obtener más información al respecto, consulte [uso de las herramientas de desarrollo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).

1. Presione **F5** para ejecutar la solución.

    > [!NOTE]
    > Para seguir esta tarea correctamente, la aplicación debe tener datos. Si la base de datos está vacía, puede volver a la tarea 3 del ejercicio 2 y seguir los pasos para crear una nueva persona con las vistas de MVC.
2. En el explorador, presione **F12** para abrir el panel de **herramientas de desarrollo** . Presione **CTRL** + **4** o haga clic en el icono de **red** y, a continuación, haga clic en el botón de flecha verde para iniciar la captura del tráfico de red.

    ![Iniciando captura de red de API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Iniciando captura de red de API Web")

    *Iniciando captura de red de API Web*
3. Anexe **API/ApiPerson** a la dirección URL en la barra de direcciones del explorador. Ahora se inspeccionarán los detalles de la respuesta de **ApiPersonController**.

    ![Recuperación de datos de persona a través de Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Recuperación de datos de persona a través de Web API")

    *Recuperación de datos de persona a través de Web API*

    > [!NOTE]
    > Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado. Deje abierto el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de programadores.
4. Ahora se inspeccionará el cuerpo de la respuesta. Para ello, haga clic en la pestaña **detalles** y, a continuación, haga clic en **cuerpo de respuesta**. Puede comprobar que los datos descargados son una lista de objetos con el **identificador**de propiedades, el **nombre** y la **edad** que corresponden a la clase **Person** .

    ![Ver el cuerpo de respuesta de la API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Ver el cuerpo de respuesta de la API Web")

    *Ver el cuerpo de respuesta de la API Web*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Tarea 3: agregar páginas de ayuda de Web API

Al crear una API Web, resulta útil crear una página de ayuda para que otros desarrolladores sepan cómo llamar a la API. Puede crear y actualizar las páginas de documentación manualmente, pero es mejor generarlas automáticamente para evitar tener que realizar tareas de mantenimiento. En esta tarea usará un paquete de Nuget para generar automáticamente páginas de ayuda de Web API en la solución.

1. En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, haga clic en **consola del administrador de paquetes**.
2. En la ventana de la **consola del administrador de paquetes** , ejecute el siguiente comando:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > El paquete **Microsoft. Aspnet. WebApi. HelpPage** instala los ensamblados necesarios y agrega vistas de MVC para las páginas de ayuda en la carpeta **areas/HelpPage** .

    ![Área HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Área HelpPage")

    *Área HelpPage*
3. De forma predeterminada, las páginas de ayuda de tienen cadenas de marcador de posición para la documentación de. Puede usar los comentarios de documentación XML para crear la documentación. Para habilitar esta característica, abra el archivo **HelpPageConfig.CS** que se encuentra en la carpeta **areas/HelpPage/App\_Inicio** y quite la marca de comentario de la siguiente línea:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto **MyHybridSite**, seleccione **propiedades** y haga clic en la pestaña **compilar** .

    ![Pestaña compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Sección compilación")

    *Pestaña compilar*
5. En **salida**, seleccione **archivo de documentación XML**. En el cuadro de edición, escriba **App\_Data/XmlDocument. XML**.

    ![Sección de salida en la pestaña compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Sección de salida en la pestaña compilar")

    *Sección de salida en la pestaña compilar*
6. Presione **CTRL** + **S** para guardar los cambios.
7. Abra el archivo **ApiPersonController.CS** desde la carpeta **Controllers** .
8. Escriba una nueva línea entre la firma del método *GetPeople* y el comentario *//Get API/ApiPerson* y, a continuación, escriba tres barras diagonales.

    > [!NOTE]
    > Visual Studio inserta automáticamente los elementos XML que definen la documentación del método.
9. Agregue un texto de Resumen y el valor devuelto para el método *GetPeople* . Debería tener un aspecto similar al siguiente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Presione **F5** para ejecutar la solución.
11. Anexe **/Help** a la dirección URL en la barra de direcciones para ir a la página de ayuda.

    ![ASP.NET Web API página de ayuda](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API página de ayuda")

    *ASP.NET Web API página de ayuda*

    > [!NOTE]
    > El contenido principal de la página es una tabla de API, agrupadas por controlador. Las entradas de la tabla se generan dinámicamente, mediante la interfaz **IApiExplorer** . Si agrega o actualiza un controlador de API, la tabla se actualizará automáticamente la próxima vez que compile la aplicación.
    > 
    > La columna **API** muestra el método http y el URI relativo. La columna **Description** contiene información que se ha extraído de la documentación del método.
12. Tenga en cuenta que la descripción que agregó encima de la definición de método se muestra en la columna Descripción.

    ![Descripción del método de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Descripción del método de API")

    *Descripción del método de API*
13. Haga clic en uno de los métodos de la API para navegar a una página con información más detallada, incluidos los cuerpos de respuesta de ejemplo.

    ![Página información detallada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Página información detallada")

    *Página información detallada*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido a:

- Cree una nueva aplicación web con una experiencia de ASP.NET en Visual Studio 2013
- Integre varias tecnologías de ASP.NET en un único proyecto
- Generación de controladores y vistas de MVC a partir de las clases de modelo mediante scaffolding de ASP.NET
- Generar controladores de API Web, que usan características como la programación asincrónica y el acceso a datos a través de Entity Framework
- Generar automáticamente páginas de ayuda de la API Web para los controladores
