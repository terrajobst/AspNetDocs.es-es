---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de acción personalizada de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC proporciona filtros de acción para ejecutar la lógica de filtrado antes o después de llamar a un método de acción. Los filtros de acción son atributos personalizados...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78468181"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a>Filtros de acción personalizada de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

ASP.NET MVC proporciona filtros de acción para ejecutar la lógica de filtrado antes o después de llamar a un método de acción. Los filtros de acción son atributos personalizados que proporcionan medios declarativos para agregar el comportamiento de acciones previas y posteriores a los métodos de acción del controlador.

En este laboratorio práctico, creará un atributo de filtro de acción personalizado en la solución MvcMusicStore para detectar las solicitudes del controlador y registrar la actividad de un sitio en una tabla de base de datos. Podrá agregar el filtro de registro mediante la inyección a cualquier controlador o acción. Por último, verá la vista de registro que muestra la lista de visitantes.

Este laboratorio práctico supone que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que consulte el laboratorio práctico de **conceptos básicos de ASP.NET MVC 4** .

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en [filtros de acción personalizada de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Crear un atributo de filtro de acción personalizado para extender las capacidades de filtrado
- Aplicar un atributo de filtro personalizado mediante inyección a un nivel específico
- Registrar un filtro de acción personalizado globalmente

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los siguientes elementos para completar este laboratorio:

- [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalar fragmentos de código**

Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.

Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice C: usar fragmentos de código](#AppendixC)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico se compone de los siguientes ejercicios:

1. [Ejercicio 1: registro de acciones](#Exercise1)
2. [Ejercicio 2: administración de varios filtros de acción](#Exercise2)

Tiempo estimado para completar este laboratorio: **30 minutos**.

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a>Ejercicio 1: registro de acciones

En este ejercicio, obtendrá información sobre cómo crear un filtro de registro de acciones personalizado mediante el uso de proveedores de filtros de ASP.NET MVC 4. Para ello, aplicará un filtro de registro al sitio MusicStore que registrará todas las actividades en los controladores seleccionados.

El filtro extenderá **ActionFilterAttributeClass** e invalidará el método **OnActionExecuting** para que detecte cada solicitud y, a continuación, realice las acciones de registro. La clase ASP.NET MVC **ActionExecutingContext** proporcionará la información de contexto sobre las solicitudes HTTP, la ejecución de métodos, resultados y parámetros **.**

> [!NOTE]
> ASP.NET MVC 4 también tiene proveedores de filtros predeterminados que puede usar sin crear un filtro personalizado. ASP.NET MVC 4 proporciona los siguientes tipos de filtros:
> 
> - Filtro de **autorización** , que toma decisiones de seguridad sobre si se debe ejecutar un método de acción, como la realización de la autenticación o la validación de las propiedades de la solicitud.
> - Filtro de **acción** , que incluye la ejecución del método de acción. Este filtro puede realizar un procesamiento adicional, como proporcionar datos adicionales al método de acción, inspeccionar el valor devuelto o cancelar la ejecución del método de acción.
> - Filtro de **resultados** , que ajusta la ejecución del objeto ActionResult. Este filtro puede realizar un procesamiento adicional del resultado, como la modificación de la respuesta HTTP.
> - Filtro de **excepciones** , que se ejecuta si se produce una excepción no controlada en alguna parte del método de acción, empezando por los filtros de autorización y finalizando con la ejecución del resultado. Los filtros de excepciones se pueden usar para tareas como registrar o mostrar una página de error.
> 
> Para obtener más información acerca de los proveedores de filtros, visite este vínculo de MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a>Acerca de la característica de registro de aplicaciones de almacén de música MVC

Esta solución de almacén de música tiene una nueva tabla de modelo de datos para el registro del sitio, **ActionLog**, con los siguientes campos: nombre del controlador que recibió una solicitud, denominada acción, dirección IP del cliente y marca de tiempo.

![Modelo de datos. Tabla ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de datos. Tabla ActionLog.")

*Modelo de datos: tabla ActionLog*

La solución proporciona una vista de MVC de ASP.NET para el registro de acciones que se puede encontrar en **MvcMusicStores/views/ActionLog**:

![Vista del registro de acciones](aspnet-mvc-4-custom-action-filters/_static/image2.png "Vista del registro de acciones")

*Vista del registro de acciones*

Con esta estructura determinada, todo el trabajo se centrará en la solicitud del controlador de interrupción y en la realización del registro mediante el filtrado personalizado.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a>Tarea 1: creación de un filtro personalizado para detectar la solicitud de un controlador

En esta tarea creará una clase de atributo de filtro personalizado que contendrá la lógica de registro. Para ello, extenderá ASP.NET MVC **ActionFilterAttribute** Class e implementará la interfaz **IActionFilter**.

> [!NOTE]
> **ActionFilterAttribute** es la clase base para todos los filtros de atributos. Proporciona los métodos siguientes para ejecutar una lógica específica después y antes de la ejecución de la acción del controlador:
> 
> - **OnActionExecuting**(ActionExecutingContext filterContext): justo antes de llamar al método de acción.
> - **OnActionExecuted**(ActionExecutedContext filterContext): después de llamar al método de acción y antes de que se ejecute el resultado (antes de que se represente la vista).
> - **OnResultExecuting**(ResultExecutingContext filterContext): justo antes de que se ejecute el resultado (antes de que se represente la vista).
> - **OnResultExecuted**(ResultExecutedContext filterContext): después de que se ejecute el resultado (después de que se represente la vista).
> 
> Al invalidar cualquiera de estos métodos en una clase derivada, puede ejecutar su propio código de filtrado.

1. Abra la solución de **Inicio** que se encuentra en la carpeta **\Source\Ex01-LoggingActions\Begin** .

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Agregue una nueva C# clase a la carpeta **Filters** y asígnele el nombre *CustomActionFilter.CS*. Esta carpeta almacenará todos los filtros personalizados.
3. Abra **CustomActionFilter.CS** y agregue una referencia a los espacios de nombres **System. Web. Mvc** y **MvcMusicStore. Models** :

    (Fragmento de código- *ASP.NET MVC 4 Custom Action filters-EX1-CustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. Herede la clase **CustomActionFilter** de **ActionFilterAttribute** y, a continuación, haga que la clase **CustomActionFilter** implemente la interfaz **IActionFilter** .

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. Haga que la clase **CustomActionFilter** invalide el método **OnActionExecuting** y agregue la lógica necesaria para registrar la ejecución del filtro. Para ello, agregue el siguiente código resaltado dentro de la clase **CustomActionFilter** .

    (Fragmento de código- *ASP.NET MVC 4 Custom Action filters-EX1-LoggingActions*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > El método **OnActionExecuting** usa **Entity Framework** para agregar un nuevo registro ActionLog. Crea y rellena una nueva instancia de la entidad con la información de contexto de **filterContext**.
    > 
    > Puede obtener más información acerca de la clase **ControllerContext** en [MSDN](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a>Tarea 2: inyección de un interceptor de código en la clase de controlador de almacén

En esta tarea, agregará el filtro personalizado mediante su inserción en todas las clases de controlador y acciones de controlador que se registrarán. Para este ejercicio, la clase de controlador de almacenamiento tendrá un registro.

El método **OnActionExecuting** de **ActionLogFilterAttribute** filtro personalizado se ejecuta cuando se llama a un elemento insertado.

También es posible interceptar un método de controlador específico.

1. Abra **StoreController** en **MvcMusicStore\Controllers** y agregue una referencia al espacio de nombres **Filters** :

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. Inserte el filtro personalizado **CustomActionFilter** en la clase **StoreController** agregando el atributo **[CustomActionFilter]** antes de la declaración de clase.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > Cuando se inserta un filtro en una clase de controlador, también se insertan todas sus acciones. Si desea aplicar el filtro solo para un conjunto de acciones, tendría que insertar **[CustomActionFilter]** en cada uno de ellos:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecución de la aplicación

En esta tarea, probará que el filtro de registro funciona. Iniciará la aplicación y visitará la tienda y, a continuación, comprobará las actividades registradas.

1. Presione **F5** para ejecutar la aplicación.
2. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro:

    ![Estado del seguimiento de registro antes de la actividad de la página](aspnet-mvc-4-custom-action-filters/_static/image3.png "Estado del seguimiento de registro antes de la actividad de la página")

    *Estado del seguimiento de registro antes de la actividad de la página*

   > [!NOTE]
   > De forma predeterminada, siempre mostrará un elemento que se genera al recuperar los géneros existentes para el menú.
   > 
   > Por motivos de simplicidad, limpiaremos la tabla **ActionLog** cada vez que se ejecute la aplicación, de modo que solo se mostrarán los registros de la comprobación de cada tarea determinada.
   > 
   > Es posible que tenga que quitar el siguiente código de la **sesión\_método Start** (en la clase **global. asax** ) para guardar un registro histórico para todas las acciones ejecutadas en el controlador de almacenamiento.
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.
4. Vaya a **/ActionLog** y, si el registro está vacío, presione **F5** para actualizar la página. Compruebe que se ha realizado el seguimiento de las visitas:

    ![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image4.png "Registro de acciones con actividad registrada")

    *Registro de acciones con actividad registrada*

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a>Ejercicio 2: administración de varios filtros de acción

En este ejercicio, agregará un segundo filtro de acción personalizada a la clase StoreController y definirá el orden específico en el que se ejecutarán ambos filtros. A continuación, actualizará el código para registrar el filtro globalmente.

Hay diferentes opciones que hay que tener en cuenta al definir el orden de ejecución de los filtros. Por ejemplo, la propiedad order y el ámbito filters:

Puede definir un **ámbito** para cada uno de los filtros; por ejemplo, puede establecer el ámbito de todos los filtros de acción para que se ejecuten en el **ámbito del controlador**y todos los filtros de autorización para ejecutarse en el **ámbito global**. Los ámbitos tienen un orden de ejecución definido.

Además, cada filtro de acción tiene una propiedad Order que se usa para determinar el orden de ejecución en el ámbito del filtro.

Para obtener más información sobre el orden de ejecución de los filtros de acción personalizados, visite este artículo de MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a>Tarea 1: crear un filtro de acción personalizado nuevo

En esta tarea, creará un nuevo filtro de acción personalizada para insertarlo en la clase StoreController y aprenderá a administrar el orden de ejecución de los filtros.

1. Abra la solución de **Inicio** que se encuentra en la carpeta **\Source\Ex02-ManagingMultipleActionFilters\Begin** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

    1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
    2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
    3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

        > [!NOTE]
        > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
        > 
        > Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Agregue una nueva C# clase a la carpeta **Filters** y asígnele el nombre *MyNewCustomActionFilter.CS*
3. Abra **MyNewCustomActionFilter.CS** y agregue una referencia a **System. Web. Mvc** y el espacio de nombres **MvcMusicStore. Models** :

    (Fragmento de código- *ASP.NET MVC 4 Custom Action filters-Ex2-MyNewCustomActionFilterNamespaces*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. Reemplace la declaración de clase predeterminada por el código siguiente.

    (Fragmento de código- *ASP.NET MVC 4 Custom Action filters-Ex2-MyNewCustomActionFilterClass*)

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > Este filtro de acción personalizada es casi el mismo que el que creó en el ejercicio anterior. La principal diferencia es que tiene la *&quot;registrada por&quot;* atributo actualizado con el nombre de esta nueva clase para identificar qué filtro registró el registro.

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a>Tarea 2: insertar un nuevo interceptor de código en la clase StoreController

En esta tarea, agregará un nuevo filtro personalizado a la clase StoreController y ejecutará la solución para comprobar el modo en que ambos filtros funcionan juntos.

1. Abra la clase **StoreController** que se encuentra en **MvcMusicStore\Controllers** e inserte el nuevo filtro personalizado **MyNewCustomActionFilter** en **StoreController** clase como se muestra en el código siguiente.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. Ahora, ejecute la aplicación para ver cómo funcionan estos dos filtros de acción personalizados. Para ello, presione **F5** y espere hasta que se inicie la aplicación.
3. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.

    ![Estado del seguimiento de registro antes de la actividad de la página](aspnet-mvc-4-custom-action-filters/_static/image5.png "Estado del seguimiento de registro antes de la actividad de la página")

    *Estado del seguimiento de registro antes de la actividad de la página*
4. Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.
5. Compruebe esta hora; se realizó un seguimiento de las visitas dos veces: una vez para cada uno de los filtros de acción personalizada que agregó en la clase **StorageController** .

    ![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image6.png "Registro de acciones con actividad registrada")

    *Registro de acciones con actividad registrada*
6. Cierre el explorador.

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a>Tarea 3: administrar el orden de los filtros

En esta tarea, aprenderá a administrar el orden de ejecución de los filtros mediante la propiedad order.

1. Abra la clase **StoreController** que se encuentra en **MvcMusicStore\Controllers** y especifique la propiedad **Order** en ambos filtros, como se muestra a continuación.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. Ahora, Compruebe cómo se ejecutan los filtros según el valor de su propiedad order. Observará que el filtro con el menor valor de orden (**CustomActionFilter**) es el primero que se ejecuta. Presione **F5** y espere hasta que se inicie la aplicación.
3. Vaya a **/ActionLog** para ver el estado inicial de la vista de registro.

    ![Estado del seguimiento de registro antes de la actividad de la página](aspnet-mvc-4-custom-action-filters/_static/image7.png "Estado del seguimiento de registro antes de la actividad de la página")

    *Estado del seguimiento de registro antes de la actividad de la página*
4. Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.
5. Compruebe que, en este momento, se realizó un seguimiento de las visitas por los filtros "valor de orden: registros de **CustomActionFilter** " en primer lugar.

    ![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image8.png "Registro de acciones con actividad registrada")

    *Registro de acciones con actividad registrada*
6. Ahora, actualizará el valor de orden de los filtros y comprobará cómo cambia el orden de registro. En la clase **StoreController** , actualice el valor de orden de los filtros, como se muestra a continuación.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. Vuelva a ejecutar la aplicación presionando **F5**.
8. Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.
9. Compruebe que esta vez, los registros creados por el filtro **MyNewCustomActionFilter** aparecen en primer lugar.

    ![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image9.png "Registro de acciones con actividad registrada")

    *Registro de acciones con actividad registrada*

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a>Tarea 4: registro de filtros globalmente

En esta tarea, actualizará la solución para registrar el nuevo filtro (**MyNewCustomActionFilter**) como filtro global. Al hacerlo, se desencadenarán todas las acciones realizadas en la aplicación y no solo en las StoreController de la tarea anterior.

1. En la clase **StoreController** , quite el atributo **[MyNewCustomActionFilter]** y la propiedad order de **[CustomActionFilter]** . Debería tener este aspecto:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. Abra el archivo **global. asax** y busque el método de **Inicio de\_** de la aplicación. Tenga en cuenta que cada vez que se inicia la aplicación, se registran los filtros globales mediante una llamada al método **RegisterGlobalFilters** dentro de la clase **FilterConfig** .

    ![Registrar filtros globales en global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrar filtros globales en global. asax")

    *Registrar filtros globales en global. asax*
3. Abra el archivo **FilterConfig.CS** en la carpeta de inicio de la **aplicación\_** .
4. Agregar una referencia a mediante System. Web. Mvc; usar MvcMusicStore. filters; System.IO.

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. Actualice el método **RegisterGlobalFilters** agregando el filtro personalizado. Para ello, agregue el código resaltado:

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. Ejecute la aplicación presionando **F5**.
7. Haga clic en uno de los **géneros** del menú y realice algunas acciones allí, como examinar un álbum disponible.
8. Compruebe que ahora **[MyNewCustomActionFilter]** se está insertando en HomeController y ActionLogController.

    ![Registro de acciones con actividad registrada](aspnet-mvc-4-custom-action-filters/_static/image11.png "Registro de acciones con actividad registrada")

    *Registro de acciones con actividad global registrada*

> [!NOTE]
> Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido a extender un filtro de acción para ejecutar acciones personalizadas. También ha aprendido a insertar cualquier filtro en los controladores de páginas. Se usaron los siguientes conceptos:

- Cómo crear filtros de acción personalizados con la clase ActionFilterAttribute MVC ASP.NET
- Inserción de filtros en controladores ASP.NET MVC
- Cómo administrar el orden de los filtros mediante la propiedad Order
- Cómo registrar filtros globalmente

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    *VS Express para Web icono*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio web desde el portal de Windows Azure

1. Vaya a la [portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.

    > [!NOTE]
    > Con Windows Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico. Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).

    ![Iniciar sesión en Windows Azure Portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Iniciar sesión en Windows Azure Portal")

    *Iniciar sesión en Windows Azure Portal de administración*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](aspnet-mvc-4-custom-action-filters/_static/image18.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](aspnet-mvc-4-custom-action-filters/_static/image20.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](aspnet-mvc-4-custom-action-filters/_static/image21.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](aspnet-mvc-4-custom-action-filters/_static/image22.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.

    ![Descargar el perfil de publicación del sitio web](aspnet-mvc-4-custom-action-filters/_static/image23.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image24.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](aspnet-mvc-4-custom-action-filters/_static/image25.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](aspnet-mvc-4-custom-action-filters/_static/image26.png).

    ![Agregando la dirección IP del cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configuración de la cadena de conexión de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](aspnet-mvc-4-custom-action-filters/_static/image34.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-custom-action-filters/_static/image37.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-custom-action-filters/_static/image38.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-custom-action-filters/_static/image39.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-custom-action-filters/_static/image40.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-custom-action-filters/_static/image41.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-custom-action-filters/_static/image42.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
