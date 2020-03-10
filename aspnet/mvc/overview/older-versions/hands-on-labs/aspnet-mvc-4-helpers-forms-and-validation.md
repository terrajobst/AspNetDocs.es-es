---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Aplicaciones auxiliares, formularios y validación de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: En los modelos de ASP.NET MVC 4 y el laboratorio práctico de acceso a datos, ha estado cargando y mostrando datos de la base de datos. En este laboratorio práctico, agregará a...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433795"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a>Validación, formularios y asistentes de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

En los **modelos de ASP.NET MVC 4 y** el laboratorio práctico de acceso a datos, ha estado cargando y mostrando datos de la base de datos. En este laboratorio práctico, agregará a la aplicación **Music Store** la capacidad de editar los datos.

Teniendo en cuenta ese objetivo, primero creará el controlador que admitirá las acciones de creación, lectura, actualización y eliminación (CRUD) de los álbumes. Se generará una plantilla de vista de índice que aprovecha la característica de scaffolding de ASP.NET MVC para mostrar las propiedades de los álbumes en una tabla HTML. Para mejorar la vista, agregará una aplicación auxiliar HTML personalizada que truncará las descripciones largas.

Después, agregará las vistas Editar y crear que le permitirá modificar los álbumes en la base de datos, con la ayuda de elementos de formulario como listas desplegables.

Por último, permitirá que los usuarios eliminen un álbum y también les impedirá que escriban datos incorrectos mediante la validación de su entrada.

Este laboratorio práctico supone que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que consulte el laboratorio práctico de **conceptos básicos de ASP.NET MVC** .

Este laboratorio le guía a través de las mejoras y las nuevas características descritas anteriormente mediante la aplicación de cambios menores en una aplicación Web de ejemplo que se proporciona en la carpeta de origen.

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en las [aplicaciones auxiliares, formularios y validación de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Creación de un controlador para admitir operaciones CRUD
- Generar una vista de índice para mostrar las propiedades de la entidad en una tabla HTML
- Agregar una aplicación auxiliar HTML personalizada
- Crear y personalizar una vista de edición
- Diferenciar entre los métodos de acción que reaccionan a las llamadas HTTP-GET o HTTP-POST
- Adición y personalización de una vista de creación
- Controlar la eliminación de una entidad
- Validación de entradas de usuario

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

Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice B: usar fragmentos de código](#AppendixB)&quot;.

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Los ejercicios siguientes constituyen este laboratorio práctico:

1. [Creación del controlador del administrador de la tienda y su vista de índice](#Exercise1)
2. [Agregar una aplicación auxiliar HTML](#Exercise2)
3. [Crear la vista de edición](#Exercise3)
4. [Agregar una vista de creación](#Exercise4)
5. [Controlar la eliminación](#Exercise5)
6. [Agregar una validación](#Exercise6)
7. [Usar jQuery discreto en el lado cliente](#Exercise7)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **60 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a>Ejercicio 1: creación del controlador del administrador de la tienda y su vista de índice

En este ejercicio, obtendrá información sobre cómo crear un controlador nuevo para admitir operaciones CRUD, personalizar su método de acción de índice para que devuelva una lista de álbumes de la base de datos y, por último, generar una plantilla de vista de índice que aproveche el scaffolding de ASP.NET MVC. característica para mostrar las propiedades de los álbumes en una tabla HTML.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a>Tarea 1: creación de StoreManagerController

En esta tarea, creará un nuevo controlador denominado **StoreManagerController** para admitir las operaciones CRUD.

1. Abra la carpeta **Begin** Solution ubicada en **source/EX1-CreatingTheStoreManagerController/Begin/** .

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Agregue un nuevo controlador. Para ello, haga clic con el botón secundario en la carpeta **Controllers** dentro del explorador de soluciones, seleccione **Agregar** y, a continuación, el comando **controlador** . Cambie el **nombre** del controlador a **StoreManagerController** y asegúrese de que está seleccionada la opción **controlador de MVC con acciones de lectura/escritura vacías** . Haga clic en **Agregar**.

    ![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Cuadro de diálogo Agregar controlador")

    *Cuadro de diálogo Agregar controlador*

    Se genera una nueva clase de controlador. Dado que ha indicado agregar acciones para la lectura y escritura, los métodos de código auxiliar para estas, las acciones CRUD comunes se crean con los comentarios TODO rellenados, lo que solicita incluir la lógica específica de la aplicación.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a>Tarea 2: personalización del índice de StoreManager

En esta tarea, personalizará el método de acción de índice StoreManager para que devuelva una vista con la lista de álbumes de la base de datos.

1. En la clase StoreManagerController, agregue las siguientes directivas *using* .

    (Fragmento de código-ASP.NET de las *aplicaciones auxiliares y formularios y validación-EX1 con MvcMusicStore*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. Agregue un campo a **StoreManagerController** para que contenga una instancia de **MusicStoreEntities.**

    (Fragmento de código- *ASP.NET MVC 4 helpers y formularios y validación-EX1 MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. Implemente la acción de índice StoreManagerController para devolver una vista con la lista de álbumes.

    La lógica de acción del controlador será muy similar a la acción de índice de StoreController escrita anteriormente. Use LINQ para recuperar todos los álbumes, incluida la información del género y el artista para su presentación.

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-EX1 StoreManagerController index*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a>Tarea 3: crear la vista de índice

En esta tarea, creará la plantilla de vista de índice para mostrar la lista de álbumes devueltos por el controlador **StoreManager** .

1. Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el **cuadro de diálogo Agregar vista** Conozca la clase **album** que se va a usar. Seleccionar **compilación | Cree MvcMusicStore** para compilar el proyecto.
2. Haga clic con el botón derecho en el método de acción de **Índice** y seleccione **Agregar vista**. Se abrirá el cuadro de diálogo **Agregar vista** .

    ![Agregar vista](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Agregar vista")

    *Agregar una vista desde el método index*
3. En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista es **Índice**. Seleccione la opción **crear una vista fuertemente tipada** y seleccione **álbum (MvcMusicStore. Models)** en la lista desplegable **clase de modelo** . Seleccione **lista** en el menú desplegable **plantilla scaffolding** . Deje el **motor de vista** en **Razor** y los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.

    ![Agregar una vista de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Agregar una vista de índice")

    *Agregar una vista de índice*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a>Tarea 4: personalizar el scaffolding de la vista de índice

En esta tarea, ajustará la plantilla de vista simple creada con la característica de scaffolding de ASP.NET MVC para que muestre los campos que desee.

> [!NOTE]
> La compatibilidad con **scaffolding** en ASP.NET MVC genera una plantilla de vista simple que muestra todos los campos del modelo de álbum. La **técnica scaffolding** proporciona una forma rápida de empezar a trabajar en una vista fuertemente tipada: en lugar de tener que escribir la plantilla de vista manualmente, la técnica de scaffolding genera rápidamente una plantilla predeterminada y, a continuación, puede modificar el código generado.

1. Revise el código creado. La lista de campos generada formará parte de la siguiente tabla HTML que usa la **técnica scaffolding** para mostrar los datos tabulares.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. Reemplace la **&lt;tabla&gt;** código por el código siguiente para mostrar solo los campos **género**, **intérprete**, **título del álbum**y **precio** . Esto elimina las columnas **AlbumId** y **dirección URL** de carátula de álbum. Además, cambia las columnas GenreId y ArtistId para mostrar las propiedades de la clase vinculada de **Artist.Name** y **Genre.Name**, y quita el vínculo **Details** .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. Cambie las descripciones siguientes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, comprobará que la plantilla de vista de **Índice** **StoreManager** muestra una lista de álbumes según el diseño de los pasos anteriores.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager** para comprobar que se muestra una lista de álbumes, mostrando su **título**, **intérprete** y **género**.

    ![Examinar la lista de álbumes](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Examinar la lista de álbumes")

    *Examinar la lista de álbumes*

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a>Ejercicio 2: agregar una aplicación auxiliar HTML

La página de índice de StoreManager tiene un posible problema: las propiedades de título y nombre de intérprete pueden ser lo suficientemente largas para salir del formato de tabla. En este ejercicio aprenderá a agregar una aplicación auxiliar HTML personalizada para truncar el texto.

En la ilustración siguiente, puede ver cómo se modifica el formato debido a la longitud del texto cuando se usa un tamaño pequeño del explorador.

![Examinar la lista de álbumes con texto no truncado](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Examinar la lista de álbumes con texto no truncado")

*Examinar la lista de álbumes con texto no truncado*

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a>Tarea 1: extensión de la aplicación auxiliar HTML

En esta tarea, agregará un nuevo método que se **truncará** en el objeto **HTML** expuesto en las vistas de MVC de ASP.net. Para ello, implementará un método de **extensión** en la clase integrada **System. Web. Mvc. HtmlHelper** proporcionada por ASP.NET MVC.

> [!NOTE]
> Para obtener más información acerca de **los métodos de extensión**, visite este artículo de MSDN. [https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex2-AddingAnHTMLHelper/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la vista de índice de StoreManager. Para ello, en el Explorador de soluciones expanda la carpeta **views** , **StoreManager** y abra el archivo **index. cshtml** .
3. Agregue el siguiente código debajo de la directiva <strong>@model</strong> para definir el método de aplicación auxiliar <strong>TRUNCATE</strong> .

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a>Tarea 2: truncamiento de texto en la página

En esta tarea, usará el método **TRUNCATE** para truncar el texto de la plantilla de vista.

1. Abra la vista de índice de StoreManager. Para ello, en el Explorador de soluciones expanda la carpeta **views** , **StoreManager** y abra el archivo **index. cshtml** .
2. Reemplace las líneas que muestran el **nombre del intérprete** y el **título**del álbum. Para ello, reemplace las líneas siguientes.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecución de la aplicación

En esta tarea, comprobará que la plantilla de vista de **Índice** **StoreManager** trunca el título del álbum y el nombre del intérprete.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager** para comprobar que se truncan los textos largos en el **título** y la columna de **intérprete** .

    ![Nombres de los títulos y artistas truncados](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nombres de los títulos y artistas truncados")

    *Nombres de intérpretes y títulos truncados*

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a>Ejercicio 3: crear la vista de edición

En este ejercicio, obtendrá información sobre cómo crear un formulario para permitir que los administradores de la tienda editen un álbum. Examinarán la dirección URL de **/StoreManager/Edit/ID** (**identificador** que es el identificador único del álbum que se va a editar), con lo que se realiza una llamada HTTP-GET al servidor.

El método de acción de edición del controlador recuperará el álbum adecuado de la base de datos, creará un objeto **StoreManagerViewModel** para encapsularlo (junto con una lista de artistas y géneros) y, a continuación, lo pasará a una plantilla de vista para volver a presentar la página HTML al usuario. Esta página contendrá un **formulario&lt;&gt;** elemento con cuadros de texto y listas desplegables para editar las propiedades del álbum.

Una vez que el usuario actualiza los valores del formulario de álbum y hace clic en el botón **Guardar** , los cambios se envían a través de una llamada http-post a **/StoreManager/Edit/ID**. Aunque la dirección URL sigue siendo la misma que en la última llamada, ASP.NET MVC identifica que esta vez es HTTP-POST y, por tanto, ejecuta un método de acción de edición diferente (uno decorado con **[HttpPost]** ).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a>Tarea 1: implementar el método de acción de edición HTTP-GET

En esta tarea, implementará la versión HTTP-GET del método de acción Edit para recuperar el álbum adecuado de la base de datos, así como una lista de todos los géneros y artistas. Empaquetará estos datos en el objeto **StoreManagerViewModel** definido en el último paso, que se pasará después a una plantilla de vista para representar la respuesta con.

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex3-CreatingTheEditView/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la clase **StoreManagerController** . Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.
3. Reemplace el método **de acción de edición HTTP-GET** con el siguiente código para recuperar el **álbum** adecuado, así como las listas **géneros** y **intérpretes** .

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex3 STOREMANAGERCONTROLLER HTTP-GET Edit Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > Está usando **System. Web. Mvc** **SelectList** para artistas y géneros en lugar de la lista **System. Collections. Generic** .
    > 
    > **SelectList** es una forma más limpia de rellenar las listas desplegables HTML y administrar elementos como la selección actual. La creación de instancias de estos objetos ViewModel y versiones posteriores en la acción del controlador hará que el limpiador del escenario del formulario de edición se limpie.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a>Tarea 2: creación de la vista de edición

En esta tarea, creará una plantilla de vista de edición que mostrará posteriormente las propiedades del álbum.

1. Cree la vista de edición. Para ello, haga clic con el botón derecho en el método **Editar** acción y seleccione **Agregar vista**.
2. En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista es **Editar**. Active la casilla **crear una vista fuertemente tipada** y seleccione **álbum (MvcMusicStore. Models)** en la lista desplegable **Ver clase de datos** . Seleccione **Editar** en la lista desplegable **plantilla scaffolding** . Deje los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.

    ![Agregar una vista de edición](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Agregar una vista de edición")

    *Agregar una vista de edición*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecución de la aplicación

En esta tarea, probará que en la página **Editar** vista de StoreManager se muestran los valores de las propiedades del álbum pasado como parámetro.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager/Edit/1** para comprobar que se muestran los valores de las propiedades del álbum que se ha pasado.

    ![Examinar la vista de edición del álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Examinar la vista de edición del álbum")

    *Examinar la vista de edición del álbum*

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a>Tarea 4: implementar listas desplegables en la plantilla del editor de álbumes

En esta tarea, agregará listas desplegables a la plantilla de vista creada en la última tarea, de modo que el usuario pueda seleccionar en una lista de artistas y géneros.

1. Reemplace todo el código de fieldset del **álbum** por lo siguiente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > Se ha agregado una aplicación auxiliar **HTML. DropDownList** para representar las listas desplegables para elegir artistas y géneros. Los parámetros que se pasan a **HTML. DropDownList** son:
    > 
    > 1. Nombre del campo de formulario ( **&quot;ArtistId&quot;** ).
    > 2. **SelectList** de los valores de la lista desplegable.

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, probará que la página **Editar** vista de **StoreManager** muestra las listas desplegables en lugar de los campos de texto ID. de intérprete y género.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager/Edit/1** para comprobar que muestra las listas desplegables en lugar de los campos de texto de ID. de intérprete y género.

    ![Examinar la vista de edición del álbum con listas desplegables](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Examinar la vista de edición del álbum con listas desplegables")

    *Examinar la vista de edición del álbum, esta vez con listas desplegables*

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a>Tarea 6: implementar el método de acción de edición HTTP-POST

Ahora que la vista de edición se muestra como se esperaba, debe implementar el método de acción de edición HTTP-POST para guardar los cambios realizados en el álbum.

1. Cierre el explorador si es necesario para volver a la ventana de Visual Studio. Abra **StoreManagerController** desde la carpeta **Controllers** .
2. Reemplace el código del método de acción **de edición http-post** con lo siguiente (tenga en cuenta que el método que se debe reemplazar es una versión sobrecargada que recibe dos parámetros):

    (Fragmento de código-ASP.NET de las *aplicaciones auxiliares y formularios y validación-Ex3 STOREMANAGERCONTROLLER http-post*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > Este método se ejecutará cuando el usuario haga clic en el botón **Guardar** de la vista y realice una post http de los valores del formulario en el servidor para que los conserve en la base de datos. El elemento Decorator **[HttpPost]** indica que el método se debe usar para esos escenarios http-post. El método toma un objeto **album** . ASP.NET MVC creará automáticamente el objeto de álbum a partir del formulario de &lt;publicado&gt; valores.
    > 
    > El método realizará estos pasos:
    > 
    > 1. Si el modelo es válido:
    > 
    >     1. Actualice la entrada del álbum en el contexto para marcarla como un objeto modificado.
    >     2. Guarde los cambios y redirija a la vista de índice.
    > 2. Si el modelo no es válido, rellenará ViewBag con **GenreId** y **ArtistId**y, a continuación, devolverá la vista con el objeto album recibido para permitir que el usuario realice las actualizaciones necesarias.

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarea 7: ejecución de la aplicación

En esta tarea, probará que la página de vista de **edición de StoreManager** guarda realmente los datos de álbumes actualizados en la base de datos.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager/Edit/1**. Cambie el título del álbum a **cargar** y haga clic en **Guardar**. Compruebe que el título del álbum ha cambiado realmente en la lista de álbumes.

    ![Actualizar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Actualizar un álbum")

    *Actualizar un álbum*

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a>Ejercicio 4: agregar una vista de creación

Ahora que **StoreManagerController** admite la capacidad de **edición** , en este ejercicio aprenderá a agregar una plantilla de creación de vistas para permitir que los administradores de la tienda agreguen nuevos álbumes a la aplicación.

Como hizo con la funcionalidad de edición, implementará el escenario de creación con dos métodos independientes dentro de la clase **StoreManagerController** :

1. Un método de acción mostrará un formulario vacío cuando los administradores de almacenes visiten primero la dirección URL de **/StoreManager/Create** .
2. Un segundo método de acción controlará el escenario en el que el administrador de la tienda hace clic en el botón **Guardar** del formulario y envía los valores de nuevo a la dirección URL de **/StoreManager/Create** como http-post.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a>Tarea 1: implementar el método de acción HTTP-GET Create

En esta tarea, implementará la versión HTTP-GET del método Create Action para recuperar una lista de todos los géneros y artistas, empaquetará estos datos en un objeto **StoreManagerViewModel** , que se pasará a continuación a una plantilla de vista.

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex4-AddingACreateView/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la clase **StoreManagerController** . Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.
3. Reemplace el código del método **Create** Action por lo siguiente:

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex4 STOREMANAGERCONTROLLER HTTP-GET Create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a>Tarea 2: adición de la vista de creación

En esta tarea, agregará la plantilla crear vista que mostrará un nuevo formulario de álbum (vacío).

1. Haga clic con el botón derecho en el método **Create** Action y seleccione **Agregar vista**. Se abrirá el cuadro de diálogo Agregar vista.
2. En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista sea **crear**. Seleccione la opción **crear una vista fuertemente tipada** y seleccione **album (MvcMusicStore. Models)** en la lista desplegable **clase de modelo** y **cree** a partir de la lista desplegable **plantilla scaffolding** . Deje los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.

    ![Agregar una vista de creación](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "Adding-a-Create-View. png")

    *Agregar la vista de creación*
3. Actualice los campos **GenreId** y **ArtistId** para utilizar una lista desplegable, como se muestra a continuación:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecución de la aplicación

En esta tarea, comprobará que la página **crear** vista de StoreManager muestra un formulario de álbum vacío.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager/Create**. Compruebe que se muestra un formulario vacío para rellenar las nuevas propiedades del álbum.

    ![Crear vista con un formulario vacío](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Crear vista con un formulario vacío")

    *Crear vista con un formulario vacío*

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a>Tarea 4: implementar el método de acción HTTP-POST Create

En esta tarea, implementará la versión HTTP-POST del método de acción Create que se invocará cuando un usuario haga clic en el botón **Guardar** . El método debe guardar el nuevo álbum en la base de datos.

1. Cierre el explorador si es necesario para volver a la ventana de Visual Studio. Abra la clase **StoreManagerController** . Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.
2. Reemplace **http-post crear** código de método de acción con lo siguiente:

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex4 STOREMANAGERCONTROLLER http-post Create Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > La acción Create es muy similar al método de acción de edición anterior, pero en lugar de establecer el objeto como modificado, se agrega al contexto.

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, comprobará que la página **crear** vista de StoreManager le permite crear un nuevo álbum y, a continuación, redirige a la vista de índice StoreManager.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager/Create**. Rellene todos los campos de formulario con datos para un nuevo álbum, como el de la ilustración siguiente:

    ![Crear un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Crear un álbum")

    *Crear un álbum*
3. Compruebe que se le redirige a la vista de índice StoreManager que incluye el nuevo álbum que acaba de crear.

    ![Nuevo álbum creado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Nuevo álbum creado")

    *Nuevo álbum creado*

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a>Ejercicio 5: control de la eliminación

La capacidad de eliminar álbumes todavía no está implementada. Este es el ejercicio. Al igual que antes, se implementará el escenario de eliminación con dos métodos independientes dentro de la clase **StoreManagerController** :

1. Un método de acción mostrará un formulario de confirmación
2. Un segundo método de acción controlará el envío del formulario

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a>Tarea 1: implementar el método de acción de eliminación HTTP-GET

En esta tarea, implementará la versión HTTP-GET del método de acción delete para recuperar la información del álbum.

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex5-HandlingDeletion/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la clase **StoreManagerController** . Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.
3. La acción eliminar controlador es exactamente la misma que la acción anterior del controlador de detalles de la tienda: consulta el objeto de **álbum** desde la base de datos con el **identificador** proporcionado en la dirección URL y devuelve la **vista**adecuada. Para ello, reemplace el código del método de acción HTTP-GET **Delete** por lo siguiente:

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex5 Handling Deleting HTTP-GET Delete Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. Haga clic con el botón derecho en el método de acción de **eliminación** y seleccione **Agregar vista**. Se abrirá el cuadro de diálogo Agregar vista.
5. En el cuadro de diálogo Agregar vista, compruebe que el nombre de la vista es **Delete**. Seleccione la opción **crear una vista fuertemente tipada** y seleccione **álbum (MvcMusicStore. Models)** en la lista desplegable **clase de modelo** . Seleccione **eliminar** en la lista desplegable **plantilla scaffolding** . Deje los demás campos con su valor predeterminado y, a continuación, haga clic en **Agregar**.

    ![Agregar una vista de eliminación](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Agregar una vista de eliminación")

    *Agregar una vista de eliminación*
6. La plantilla de eliminación muestra todos los campos del modelo. Solo mostrará el título del álbum. Para ello, reemplace el contenido de la vista por el código siguiente:

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecución de la aplicación

En esta tarea, comprobará que la página de vista de **eliminación** de **StoreManager** muestra un formulario de eliminación de confirmación.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager**. Seleccione el álbum que desea eliminar haciendo clic en **eliminar** y compruebe que la nueva vista se ha cargado.

    ![Eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Eliminar un álbum")

    *Eliminar un álbum*

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a>Tarea 3: implementar el método de acción de eliminación HTTP-POST

En esta tarea, implementará la versión HTTP-POST del método de acción Delete que se invocará cuando un usuario haga clic en el botón **eliminar** . El método debe eliminar el álbum en la base de datos.

1. Cierre el explorador si es necesario para volver a la ventana de Visual Studio. Abra la clase **StoreManagerController** . Para ello, expanda la carpeta **Controllers** y haga doble clic en **StoreManagerController.CS**.
2. Reemplace el código del método de acción **de eliminación http-post** por lo siguiente:

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-Ex5 Handling Deleting http-post Delete Action*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, probará que la página de vista de **eliminación de StoreManager** le permite eliminar un álbum y, a continuación, redirige a la vista de índice de StoreManager.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager**. Seleccione el álbum que desea eliminar haciendo clic en **eliminar.** Para confirmar la eliminación, haga clic en el botón **eliminar** :

    ![Eliminar un álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Eliminar un álbum")

    *Eliminar un álbum*
3. Compruebe que el álbum se ha eliminado porque no aparece en la página de **Índice** .

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a>Ejercicio 6: agregar validación

Actualmente, los formularios de creación y edición que tiene en su lugar no realizan ningún tipo de validación. Si el usuario deja un campo obligatorio en blanco o escribe letras en el campo Precio, el primer error que obtendrá será de la base de datos.

Puede Agregar la validación a la aplicación agregando anotaciones de datos a la clase de modelo. Las anotaciones de datos permiten describir las reglas que desea aplicar a las propiedades del modelo y ASP.NET MVC se encargará de aplicar y mostrar el mensaje adecuado a los usuarios.

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a>Tarea 1: agregar anotaciones de datos

En esta tarea, agregará anotaciones de datos al modelo de álbum que hará que la página crear y editar muestre los mensajes de validación cuando corresponda.

En el caso de una clase de modelo simple, agregar una anotación de datos se controla simplemente agregando una instrucción **using** para **System. ComponentModel. DataAnnotations**y colocando un atributo **[required]** en las propiedades adecuadas. En el ejemplo siguiente, la propiedad de **nombre** se convierte en un campo obligatorio en la vista.

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

Este es un poco más complejo en casos como esta aplicación donde se genera el Entity Data Model. Si agregó anotaciones de datos directamente a las clases del modelo, se sobrescribirán si actualiza el modelo desde la base de datos. En su lugar, puede hacer uso de las clases parciales de metadatos que existirán para contener las anotaciones y se asocian a las clases de modelo mediante el atributo **[MetadataType]** .

1. Abra la carpeta **Begin** Solution ubicada en **source/EX6-AddingValidation/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra **Album.CS** desde la carpeta **Models** .
3. Reemplace el contenido de **Album.CS** por el código resaltado para que tenga el aspecto siguiente:

    > [!NOTE]
    > La línea **[DisplayFormat (ConvertEmptyStringToNull = false)]** indica que las cadenas vacías del modelo no se convertirán en NULL cuando se actualice el campo de datos en el origen de datos. Esta configuración evitará una excepción cuando el Entity Framework asigna valores NULL al modelo antes de que la anotación de datos valide los campos.

    (Fragmento de código- *ASP.NET MVC 4 helpers and Forms and Validation-EX6 album Metadata clase parcial*)

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > Esta clase parcial de **álbum** tiene un atributo **MetadataType** que apunta a la clase **AlbumMetaData** para las anotaciones de datos. Estos son algunos de los atributos de anotación de datos que se usan para anotar el modelo de álbum:
    > 
    > - Requerido: indica que la propiedad es un campo obligatorio.
    > - DisplayName: define el texto que se va a usar en los campos de formulario y en los mensajes de validación.
    > - DisplayFormat: especifica cómo se muestran los campos de datos y cómo se les da formato.
    > - StringLength: define una longitud máxima para un campo de cadena.
    > - Range: proporciona un valor máximo y mínimo para un campo numérico.
    > - ScaffoldColumn: permite ocultar campos de los formularios del editor

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecución de la aplicación

En esta tarea, probará que los campos crear y editar páginas validan, con los nombres para mostrar elegidos en la última tarea.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/StoreManager/Create**. Compruebe que los nombres para mostrar coinciden con los de la clase parcial (como la **dirección URL** de carátula de álbum en lugar de **AlbumArtUrl**)
3. Haga clic en **crear**sin rellenar el formulario. Compruebe que obtiene los mensajes de validación correspondientes.

    ![Campos validados en la página crear](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Campos validados en la página crear")

    *Campos validados en la página crear*
4. Puede comprobar que ocurre lo mismo con la página **Editar** . Cambie la dirección URL a **/StoreManager/Edit/1** y compruebe que los nombres para mostrar coinciden con los de la clase parcial (como la **dirección URL** de la carátula de álbum en lugar de **AlbumArtUrl**). Vacíe los campos **título** y **precio** y haga clic en **Guardar**. Compruebe que obtiene los mensajes de validación correspondientes.

    ![Campos validados en la página Editar](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    *Campos validados en la página Editar*

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a>Ejercicio 7: usar jQuery discreto en el lado cliente

En este ejercicio, obtendrá información sobre cómo habilitar la validación de jQuery discreta de MVC 4 en el lado cliente.

> [!NOTE]
> La jQuery discreta usa JavaScript de prefijo de AJAX de datos para invocar métodos de acción en el servidor en lugar de emitir indiscretamente scripts de cliente insertados.

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a>Tarea 1: ejecutar la aplicación antes de habilitar jQuery discreto

En esta tarea, ejecutará la aplicación antes de incluir jQuery para comparar ambos modelos de validación.

1. Abra la carpeta **Begin** Solution ubicada en **source/EX7-UnobtrusivejQueryValidation/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Presione **F5** para ejecutar la aplicación.
3. El proyecto se inicia en la Página principal. Vaya a **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para comprobar que obtiene los mensajes de validación:

    ![Validación de cliente deshabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Validación de cliente deshabilitada")

    *Validación de cliente deshabilitada*
4. En el explorador, abra el código fuente HTML:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a>Tarea 2: habilitar la validación de cliente discreta

En esta tarea, habilitará la **validación de cliente discreta** de jQuery desde el archivo **Web. config** , que está establecido de forma predeterminada en false en todos los proyectos nuevos de ASP.NET MVC 4. Además, agregará las referencias de script necesarias para hacer que el trabajo de validación de clientes discretos de jQuery no funcione.

1. Abra el archivo **Web. config** en la raíz del proyecto y asegúrese de que los valores de las claves **ClientValidationEnabled** y **UnobtrusiveJavaScriptEnabled** están establecidos en **true**.

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > También puede habilitar la validación del cliente por código en Global.asax.cs para obtener los mismos resultados:
    > 
    > **HtmlHelper. ClientValidationEnabled = true;**
    > 
    > Además, puede asignar el atributo ClientValidationEnabled en cualquier controlador para que tenga un comportamiento personalizado.
2. Abra **Create. cshtml** en **Views\StoreManager**.
3. Asegúrese de que se hace referencia a los siguientes archivos de script, **jQuery. Validate** y **jQuery. Validate. discreta**, en la vista a través del conjunto de &quot; **~/bundles/jqueryval**&quot;.

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > Todas estas bibliotecas de jQuery se incluyen en los nuevos proyectos de MVC 4. Puede encontrar más bibliotecas en la carpeta **/scripts** del proyecto.
    > 
    > Para que estas bibliotecas de validación funcionen, debe agregar una referencia a la biblioteca de marcos de jQuery. Puesto que esta referencia ya se ha agregado en el archivo **\_layout. cshtml** , no es necesario agregarla en esta vista concreta.

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a>Tarea 3: ejecución de la aplicación con la validación de jQuery discreta

En esta tarea, comprobará que la plantilla de creación de vista de **StoreManager** realiza la validación del lado cliente mediante bibliotecas de jQuery cuando el usuario crea un nuevo álbum.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Vaya a **/StoreManager/Create** y haga clic en **crear** sin rellenar el formulario para comprobar que obtiene los mensajes de validación:

    ![Validación de cliente con jQuery habilitado](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validación de cliente con jQuery habilitado")

    *Validación de cliente con jQuery habilitado*
3. En el explorador, abra el código fuente para crear vista:

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > Para cada regla de validación de cliente, jQuery discreto agrega un atributo con Data-Val-*rulename*=&quot;&quot;de *mensaje* . A continuación se muestra una lista de etiquetas que no molestan las inserciones de jQuery en el campo de entrada HTML para realizar la validación del cliente:
   > 
   > - Datos: Val
   > - Data-Val-número
   > - Data-Val-Range
   > - Data-Val-Range-min/Data-Val-Range-Max
   > - Data-Val-required
   > - Data-Val-longitud
   > - Data-Val-Length-Max/Data-Val-length-min
   > 
   > Todos los valores de datos se rellenan con la anotación del modelo de **datos**. Después, toda la lógica que funciona en el lado servidor se puede ejecutar en el lado cliente. Por ejemplo, el atributo Price tiene la siguiente anotación de datos en el modelo:
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > Después de usar jQuery discreto, el código generado es:
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido a permitir que los usuarios cambien los datos almacenados en la base de datos por el uso de lo siguiente:

- Acciones de controlador como index, Create, Edit, Delete
- Característica de scaffolding de ASP.NET MVC para mostrar propiedades en una tabla HTML
- Aplicaciones auxiliares HTML personalizadas para mejorar la experiencia del usuario
- Métodos de acción que reaccionan a llamadas HTTP-GET o HTTP-POST
- Una plantilla de editor compartida para plantillas de vista similares, como crear y editar
- Elementos de formulario como listas desplegables
- Anotaciones de datos para la validación del modelo
- Validación del lado cliente con la biblioteca discreta de jQuery

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    *VS Express para Web icono*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
