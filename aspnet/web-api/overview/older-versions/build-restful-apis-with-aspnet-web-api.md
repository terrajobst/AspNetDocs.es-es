---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Cree API de RESTful con ASP.NET Web API ASP.NET 4. x
author: rick-anderson
description: 'Laboratorio práctico: Use Web API en ASP.NET 4. x para compilar una sencilla API de REST para una aplicación de Contact Manager.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504271"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a>Compilación de API de RESTful con ASP.NET Web API

por [equipo de grupos de web](https://twitter.com/webcamps)

> Laboratorio práctico: Use Web API en ASP.NET 4. x para compilar una sencilla API de REST para una aplicación de Contact Manager. También creará un cliente para usar la API.

En los últimos años, se ha vuelto claro que HTTP no es solo para proporcionar páginas HTML. También es una plataforma eficaz para la creación de API Web, con una serie de verbos (GET, POST, etc.) además de algunos conceptos simples, como los *URI* y los *encabezados*. ASP.NET Web API es un conjunto de componentes que simplifican la programación HTTP. Dado que se basa en el tiempo de ejecución de ASP.NET MVC, Web API controla automáticamente los detalles de transporte de bajo nivel de HTTP. Al mismo tiempo, la API Web expone de forma natural el modelo de programación HTTP. De hecho, un objetivo de Web API es *no* abstraer la realidad de http. Como resultado, la API Web es flexible y fácil de ampliar.  El estilo arquitectónico REST ha demostrado ser una manera eficaz de aprovechar HTTP, aunque ciertamente no es el único enfoque válido para HTTP. El administrador de contactos expondrá el RESTful para enumerar, agregar y quitar contactos, entre otros. 

Este laboratorio requiere un conocimiento básico de HTTP, REST y supone que tiene conocimientos prácticos básicos de HTML, JavaScript y jQuery.
> 
> > [!NOTE]
> > El sitio web de ASP.NET tiene un área dedicada al marco de ASP.NET Web API en [https://asp.net/web-api](https://asp.net/web-api). Este sitio seguirá proporcionando información de última hora, ejemplos y noticias relacionados con Web API, por lo que debe comprobarlo con frecuencia si desea profundizar más en el arte de crear API Web personalizadas disponibles para prácticamente cualquier dispositivo o marco de desarrollo.
> > 
> > ASP.NET Web API, similar a ASP.NET MVC 4, tiene una gran flexibilidad en cuanto a la separación del nivel de servicio de los controladores, lo que le permite usar fácilmente varios de los marcos de inserción de dependencias disponibles. Hay un buen ejemplo en MSDN que muestra cómo usar Ninject para la inserción de dependencias en un proyecto ASP.NET Web API que puede descargar desde [aquí](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Implementación de una API web RESTful
- Llamar a la API desde un cliente HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio práctico, es necesario lo siguiente:

- [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalar fragmentos de código**

Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.

Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice A: usar fragmentos de código](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye el siguiente ejercicio:

1. [Ejercicio 1: creación de una API Web de solo lectura](#Exercise1)
2. [Ejercicio 2: creación de una API Web de lectura/escritura](#Exercise2)
3. [Ejercicio 3: usar la API Web desde un cliente HTML](#Exercise3)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Ejercicio 1: creación de una API Web de solo lectura

En este ejercicio, implementará los métodos GET de solo lectura para el administrador de contactos.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Tarea 1: creación del proyecto de API

En esta tarea, usará las nuevas plantillas de proyecto Web de ASP.NET para crear una aplicación Web de API Web.

1. Ejecute **Visual Studio 2012 Express para web**. para ello, vaya a **inicio** y escriba **vs Express para web** Presione **entrar**.
2. En el menú **archivo** , seleccione **nuevo proyecto**. Seleccione el **Visual C# |** Tipo de proyecto web en la vista de árbol tipo de proyecto y, a continuación, seleccione el tipo de proyecto **aplicación web MVC 4 ASP.net** . Establezca el **nombre** del proyecto en *ContactManager* y el **nombre** de la solución en *Inicio*; a continuación, haga clic en **Aceptar**.

    ![Creación de un nuevo proyecto de aplicación Web ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creación de un nuevo proyecto de aplicación Web ASP.NET MVC 4,0")

    *Creación de un nuevo proyecto de aplicación Web ASP.NET MVC 4,0*
3. En el cuadro de diálogo tipo de proyecto de ASP.NET MVC 4, seleccione el tipo de proyecto **API Web** . Haga clic en **Aceptar**.

    ![Especificación del tipo de proyecto de API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Especificación del tipo de proyecto de API Web")

    *Especificación del tipo de proyecto de API Web*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Tarea 2: creación de los controladores de API de Contact Manager

En esta tarea, creará las clases de controlador en las que residirán los métodos de la API.

1. Elimine el archivo denominado **ValuesController.CS** dentro de la carpeta **Controllers** del proyecto.
2. Haga clic con el botón derecho en la carpeta **Controllers** del proyecto y seleccione **Agregar | Controlador** del menú contextual.

    ![Agregar un nuevo controlador al proyecto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Agregar un nuevo controlador al proyecto")

    *Agregar un nuevo controlador al proyecto*
3. En el cuadro de diálogo **Agregar controlador** que aparece, seleccione **controlador de API vacío** en el menú plantilla. Asigne a la clase de controlador el nombre **ContactController**. A continuación, haga clic en **Agregar.**

    ![Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web")

    *Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web*
4. Agregue el código siguiente a **ContactController**.

    (Fragmento de código: *API Web Lab-EX01-Get API Method*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Presione **F5** para depurar la aplicación. Debe aparecer la Página principal predeterminada de un proyecto de API Web.

    ![La Página principal predeterminada de una aplicación ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "La Página principal predeterminada de una aplicación ASP.NET Web API")

    *La Página principal predeterminada de una aplicación ASP.NET Web API*
6. En la ventana de Internet Explorer, presione la tecla **F12** para abrir la ventana **herramientas de desarrollo** . Haga clic en la pestaña **red** y, a continuación, haga clic en el botón **Iniciar captura** para empezar a capturar el tráfico de red en la ventana.

    ![Apertura de la pestaña red e inicio de la captura de red](build-restful-apis-with-aspnet-web-api/_static/image6.png "Apertura de la pestaña red e inicio de la captura de red")

    *Apertura de la pestaña red e inicio de la captura de red*
7. Anexe la dirección URL de la barra de direcciones del explorador con **/API/Contact** y presione Entrar. Los detalles de la transmisión aparecerán en la ventana captura de red. Tenga en cuenta que el tipo MIME de la respuesta es **Application/JSON**. Esto muestra cómo el formato de salida predeterminado es JSON.

    ![Visualización de la salida de la solicitud de API Web en la vista de red](build-restful-apis-with-aspnet-web-api/_static/image7.png "Visualización de la salida de la solicitud de API Web en la vista de red")

    *Visualización de la salida de la solicitud de API Web en la vista de red*

    > [!NOTE]
    > El comportamiento predeterminado de Internet Explorer 10 en este momento será preguntar si el usuario desea guardar o abrir el flujo resultante de la llamada a la API Web. La salida será un archivo de texto que contenga el resultado JSON de la llamada URL de la API Web. No cancele el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de desarrolladores.
8. Haga clic en el botón **ir a vista detallada** para ver más detalles sobre la respuesta de esta llamada de API.

    ![Cambiar a la vista detallada](build-restful-apis-with-aspnet-web-api/_static/image8.png "Cambiar a la vista de detalles")

    *Cambiar a la vista detallada*
9. Haga clic en la pestaña **cuerpo de respuesta** para ver el texto de respuesta JSON real.

    ![Visualización del texto de salida JSON en el monitor de red](build-restful-apis-with-aspnet-web-api/_static/image9.png "Visualización del texto de salida JSON en el monitor de red")

    *Visualización del texto de salida JSON en el monitor de red*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Tarea 3: crear los modelos de contacto y aumentar el controlador de contactos

En esta tarea, creará las clases de controlador en las que residirán los métodos de la API.

1. Haga clic con el botón derecho en la carpeta **modelos** y seleccione **Agregar | Clase...** en el menú contextual.

    ![Agregar un nuevo modelo a la aplicación Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Agregar un nuevo modelo a la aplicación Web")

    *Agregar un nuevo modelo a la aplicación Web*
2. En el cuadro de diálogo **Agregar nuevo elemento** , asigne al nuevo archivo el nombre **Contact.CS** y haga clic en **Agregar.**

    ![Crear el nuevo archivo de clase de contacto](build-restful-apis-with-aspnet-web-api/_static/image11.png "Crear el nuevo archivo de clase de contacto")

    *Crear el nuevo archivo de clase de contacto*
3. Agregue el siguiente código resaltado a la clase **Contact** .

    (Fragmento de código- *Web API Lab-EX01-Contact (clase*))

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. En la clase **ContactController** , seleccione la palabra **cadena** en la definición de método del método **Get** y escriba la palabra *Contact*. Una vez escrita la palabra, aparecerá un indicador al principio de la palabra **contacto**. Mantenga presionada la tecla **Ctrl** y presione la tecla punto (.) o haga clic en el icono con el mouse para abrir el cuadro de diálogo de asistencia en el editor de código y rellene automáticamente la directiva **using** para el espacio de nombres Models.

    ![Usar la asistencia de IntelliSense para las declaraciones de espacio de nombres](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Usar la asistencia de IntelliSense para las declaraciones de espacio de nombres*
5. Modifique el código para el método **Get** para que devuelva una matriz de instancias de modelo de contacto.

    (Fragmento de código: *API Web Lab-EX01: devolver una lista de contactos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Presione **F5** para depurar la aplicación web en el explorador. Para ver los cambios realizados en la salida de respuesta de la API, realice los pasos siguientes.

   1. Una vez que se abra el explorador, presione **F12** si las herramientas de desarrollo no están abiertas todavía.
   2. Haga clic en la pestaña **red** .
   3. Presione el botón **Iniciar captura** .
   4. Agregue el sufijo de dirección URL **/API/Contact** a la dirección URL en la barra de direcciones y presione la tecla **entrar** .
   5. Presione el botón **ir a vista detallada** .
   6. Seleccione la pestaña **cuerpo de respuesta** . Debería ver una cadena JSON que representa el formato serializado de una matriz de instancias de contacto.

      ![Salida serializada de JSON de una llamada de método de API Web compleja](build-restful-apis-with-aspnet-web-api/_static/image13.png "Salida serializada de JSON de una llamada de método de API Web compleja")

      *Salida serializada de JSON de una llamada de método de API Web compleja*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Tarea 4: extracción de la funcionalidad en un nivel de servicio

Esta tarea mostrará cómo extraer funcionalidad en un nivel de servicio para facilitar a los desarrolladores la separación de su funcionalidad de servicio del nivel de controlador, lo que permite la reutilización de los servicios que realmente realizan el trabajo.

1. Cree una nueva carpeta en la raíz de la solución y asígnele el nombre **servicios**. Para ello, haga clic con el botón derecho en proyecto **ContactManager** , seleccione **Agregar** | **nueva carpeta**, asígnele el nombre *servicios*.

    ![Creando carpeta de servicios](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creando carpeta de servicios")

    *Creando carpeta de servicios*
2. Haga clic con el botón derecho en la carpeta **servicios** y seleccione **Agregar | Clase...** en el menú contextual.

    ![Agregar una nueva clase a la carpeta servicios](build-restful-apis-with-aspnet-web-api/_static/image15.png "Agregar una nueva clase a la carpeta servicios")

    *Agregar una nueva clase a la carpeta servicios*
3. Cuando aparezca el cuadro de diálogo **Agregar nuevo elemento** , asigne a la nueva clase el nombre **ContactRepository** y haga clic en **Agregar**.

    ![Crear un archivo de clase que contenga el código para la capa de servicio del repositorio de contactos](build-restful-apis-with-aspnet-web-api/_static/image16.png "Crear un archivo de clase que contenga el código para la capa de servicio del repositorio de contactos")

    *Crear un archivo de clase que contenga el código para la capa de servicio del repositorio de contactos*
4. Agregue una directiva using al archivo **ContactRepository.CS** para incluir el espacio de nombres de los modelos.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Agregue el siguiente código resaltado al archivo **ContactRepository.CS** para implementar el método GetAllContacts.

    (Fragmento de código- *Web API Lab-EX01-repositorio del contacto*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Abra el archivo **ContactController.CS** si aún no está abierto.
7. Agregue la siguiente instrucción using a la sección de declaración de espacio de nombres del archivo.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Agregue el siguiente código resaltado a la clase **ContactController.CS** para agregar un campo privado que represente la instancia del repositorio, de modo que el resto de los miembros de clase pueda usar la implementación del servicio.

    (Fragmento de código- *Web API Lab-EX01-Contact Controller*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Cambie el método **Get** para que use el servicio de repositorio de contactos.

    (Fragmento de código: *API Web Lab-EX01: devolver una lista de contactos a través del repositorio*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Coloque un punto de interrupción en la definición del método **Get** de **ContactController**.

   ![Agregar puntos de interrupción al controlador de contacto](build-restful-apis-with-aspnet-web-api/_static/image17.png "Agregar puntos de interrupción al controlador de contacto")

   *Agregar puntos de interrupción al controlador de contacto*
11. Presione **F5** para ejecutar la aplicación.
12. Cuando se abra el explorador, presione **F12** para abrir las herramientas de desarrollo.
13. Haga clic en la pestaña **red** .
14. Haga clic en el botón **Iniciar captura** .
15. Anexe la dirección URL en la barra de direcciones con el sufijo **/API/Contact** y presione **entrar** para cargar el controlador de API.
16. Visual Studio 2012 debería interrumpir una vez que el método **Get** comience la ejecución.

   ![Interrumpir dentro del método get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Interrumpir dentro del método get")

   *Interrumpir dentro del método get*
17. Presione **F5** para continuar.
18. Vuelva a Internet Explorer si aún no está en el foco. Observe la ventana de captura de red.

    ![Vista de red en Internet Explorer que muestra los resultados de la llamada a la API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Vista de red en Internet Explorer que muestra los resultados de la llamada a la API Web")

    *Vista de red en Internet Explorer que muestra los resultados de la llamada a la API Web*
19. Haga clic en el botón **ir a vista detallada** .
20. Haga clic en la pestaña **cuerpo de respuesta** . tenga en cuenta la salida JSON de la llamada de API y cómo representa los dos contactos recuperados por el nivel de servicio.

    ![Visualización de la salida JSON desde la API Web en la ventana herramientas de desarrollo](build-restful-apis-with-aspnet-web-api/_static/image20.png "Visualización de la salida JSON desde la API Web en la ventana herramientas de desarrollo")

    *Visualización de la salida JSON desde la API Web en la ventana herramientas de desarrollo*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Ejercicio 2: creación de una API Web de lectura/escritura

En este ejercicio, implementará los métodos POST y PUT para el administrador de contactos con el fin de habilitarlo con características de edición de datos.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Tarea 1: apertura del proyecto de API Web

En esta tarea, preparará la mejora del proyecto de API Web creado en el ejercicio 1 para que pueda aceptar datos proporcionados por el usuario.

1. Ejecute **Visual Studio 2012 Express para web**. para ello, vaya a **inicio** y escriba **vs Express para web** Presione **entrar**.
2. Abra la carpeta **Begin** Solution ubicada en **source/Ex02-ReadWriteWebAPI/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. Abra el archivo **Services/ContactRepository. CS** .

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Tarea 2: agregar características de persistencia de datos a la implementación del repositorio de contactos

En esta tarea, ampliará la clase ContactRepository del proyecto de API Web creado en el ejercicio 1 para que pueda conservar y aceptar la entrada del usuario y las nuevas instancias de contacto.

1. Agregue la siguiente constante a la clase **ContactRepository** para representar el nombre del nombre de la clave del elemento de la caché del servidor Web más adelante en este ejercicio.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Agregue un constructor a **ContactRepository** que contenga el código siguiente.

    (Fragmento de código- *Web API Lab-Ex02-constructor de repositorio de contactos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modifique el código para el método **GetAllContacts** como se muestra a continuación.

    (Fragmento de código- *Web API Lab-Ex02-obtener todos los contactos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Este ejemplo se usa para fines de demostración y utilizará la memoria caché del servidor web como medio de almacenamiento, de modo que los valores estarán disponibles para varios clientes simultáneamente, en lugar de usar un mecanismo de almacenamiento de sesión o una duración de almacenamiento de solicitudes. Puede usar Entity Framework, el almacenamiento XML o cualquier otra variedad en lugar de la memoria caché del servidor Web.
4. Implemente un nuevo método denominado **SaveContact** en la clase **ContactRepository** para realizar el trabajo de guardar un contacto. El método **SaveContact** debe tomar un único parámetro **Contact** y devolver un valor booleano que indique si se ha realizado correctamente o no.

    (Fragmento de código- *Web API Lab-Ex02-implementando el método SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Ejercicio 3: usar la API Web desde un cliente HTML

En este ejercicio, creará un cliente HTML para llamar a la API Web. Este cliente facilitará el intercambio de datos con la API Web mediante JavaScript y mostrará los resultados en un explorador Web mediante el marcado HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Tarea 1: modificar la vista de índice para proporcionar una GUI para mostrar contactos

En esta tarea, modificará la vista de índice predeterminada de la aplicación web para admitir el requisito de mostrar la lista de contactos existentes en un explorador HTML.

1. Abra **Visual Studio 2012 Express para web** si aún no está abierto.
2. Abra la carpeta **Begin** Solution ubicada en **source/Ex03-ConsumingWebAPI/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. Abra el archivo **index. cshtml** que se encuentra en la carpeta **views/Home** .
4. Reemplace el código HTML dentro del elemento div con el **cuerpo** del identificador para que sea similar al código siguiente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Agregue el siguiente código JavaScript en la parte inferior del archivo para realizar la solicitud HTTP a la API Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Abra el archivo **ContactController.CS** si aún no está abierto.
7. Coloque un punto de interrupción en el método **Get** de la clase **ContactController** .

    ![Colocar un punto de interrupción en el método get del controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Colocar un punto de interrupción en el método get del controlador de API")

    *Colocar un punto de interrupción en el método get del controlador de API*
8. Presione **F5** para ejecutar el proyecto. El explorador cargará el documento HTML.

    > [!NOTE]
    > Asegúrese de que está explorando la dirección URL raíz de la aplicación.
9. Una vez que se carga la página y se ejecuta JavaScript, se alcanza el punto de interrupción y la ejecución del código se pausará en el controlador.

    ![Depurar en las llamadas a la API Web mediante VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Depurar en las llamadas a la API Web mediante VS Express para Web")

    *Depuración en la llamada de la API Web con Visual Studio 2012 Express for Web*
10. Quite el punto de interrupción y presione **F5** o el botón **continuar** de la barra de herramientas de depuración para continuar cargando la vista en el explorador. Una vez que se complete la llamada a la API Web, debería ver los contactos devueltos por la llamada a la API Web que se muestra como elementos de lista en el explorador.

    ![Resultados de la llamada API mostrada en el explorador como elementos de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "Resultados de la llamada API mostrada en el explorador como elementos de lista")

    *Resultados de la llamada API mostrada en el explorador como elementos de lista*
11. Detenga la depuración.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Tarea 2: modificación de la vista de índice para proporcionar una GUI para crear contactos

En esta tarea, seguirá modificando la vista de índice de la aplicación MVC. Se agregará un formulario a la página HTML que capturará los datos proporcionados por el usuario y los enviará a la API Web para crear un nuevo contacto, y se creará un nuevo método de controlador de API Web para recopilar la fecha de la GUI.

1. Abra el archivo **ContactController.CS** .
2. Agregue un nuevo método a la clase de controlador denominada **post** tal y como se muestra en el código siguiente.

    (Fragmento de código- *Web API Lab-Ex03-post (método*))

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Abra el archivo **index. cshtml** en Visual Studio si aún no está abierto.
4. Agregue el código HTML siguiente al archivo justo después de la lista sin ordenar que agregó en la tarea anterior.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. En el elemento script situado en la parte inferior del documento, agregue el siguiente código resaltado para controlar los eventos de clic de botón, que publicarán los datos en la API Web mediante una llamada HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. En **ContactController.CS**, coloque un punto de interrupción en el método **post** .
7. Presione **F5** para ejecutar la aplicación en el explorador.
8. Una vez que la página se cargue en el explorador, escriba un nuevo nombre y un identificador de contacto y haga clic en el botón **Guardar** .

    ![Documento HTML del cliente cargado en el explorador](build-restful-apis-with-aspnet-web-api/_static/image24.png "Documento HTML del cliente cargado en el explorador")

    *Documento HTML del cliente cargado en el explorador*
9. Cuando la ventana del depurador se interrumpa en el método **post** , eche un vistazo a las propiedades del parámetro **Contact** . Los valores deben coincidir con los datos que especificó en el formulario.

    ![El objeto de contacto que se envía a la API Web desde el cliente.](build-restful-apis-with-aspnet-web-api/_static/image25.png "El objeto de contacto que se envía a la API Web desde el cliente.")

    *El objeto de contacto que se envía a la API Web desde el cliente.*
10. Recorra el método en el depurador hasta que se haya creado la variable de **respuesta** . Al realizar la inspección en la ventana **variables locales** del depurador, verá que se han establecido todas las propiedades.

   ![La respuesta siguiente a la creación en el depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "La respuesta siguiente a la creación en el depurador")

   *La respuesta siguiente a la creación en el depurador*
11. Si presiona **F5** o hace clic en **continuar** en el depurador, la solicitud se completará. Una vez que vuelva al explorador, el nuevo contacto se ha agregado a la lista de contactos almacenados por la implementación de **ContactRepository** .

   ![El explorador refleja la creación correcta de la nueva instancia de contacto](build-restful-apis-with-aspnet-web-api/_static/image27.png "El explorador refleja la creación correcta de la nueva instancia de contacto")

   *El explorador refleja la creación correcta de la nueva instancia de contacto*

> [!NOTE]
> Además, puede implementar esta aplicación en Azure después del [Apéndice C: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixC).

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Este laboratorio le ha incorporado el nuevo marco de ASP.NET Web API y la implementación de las API Web de RESTful mediante el marco de trabajo. Desde aquí, puede crear un nuevo repositorio que facilite la persistencia de los datos mediante cualquier número de mecanismos y conecte el servicio en lugar del simple proporcionado como ejemplo en este laboratorio. Web API admite varias características adicionales, como la habilitación de la comunicación desde clientes no HTML escritos en cualquier lenguaje que admita HTTP y JSON o XML. También es posible hospedar una API Web fuera de una aplicación web típica, así como la capacidad de crear sus propios formatos de serialización.

El sitio web de ASP.NET tiene un área dedicada al marco de ASP.NET Web API en [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api). Este sitio seguirá proporcionando información de última hora, ejemplos y noticias relacionados con Web API, por lo que debe comprobarlo con frecuencia si desea profundizar más en el arte de crear API Web personalizadas disponibles para prácticamente cualquier dispositivo o marco de desarrollo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apéndice A: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Para agregar un fragmento de código mediante el tecladoC# (solo)

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

    ![Comience a escribir el nombre del fragmento de código.](build-restful-apis-with-aspnet-web-api/_static/image29.png "Comience a escribir el nombre del fragmento de código.")

    *Comience a escribir el nombre del fragmento de código.*

    ![Presione TAB para seleccionar el fragmento de código resaltado](build-restful-apis-with-aspnet-web-api/_static/image30.png "Presione TAB para seleccionar el fragmento de código resaltado")

    *Presione TAB para seleccionar el fragmento de código resaltado*

    ![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](build-restful-apis-with-aspnet-web-api/_static/image31.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

    *Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)

1. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.
2. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
3. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

    ![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](build-restful-apis-with-aspnet-web-api/_static/image32.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

    *Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

    ![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](build-restful-apis-with-aspnet-web-api/_static/image33.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

    *Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apéndice B: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express para Web icono*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice C: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde Azure portal y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarea 1: creación de un nuevo sitio web desde Azure portal

1. Vaya a [Azure portal de administración](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.

    > [!NOTE]
    > Con Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico. Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).

    ![Iniciar sesión en Windows Azure Portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Iniciar sesión en Windows Azure Portal")

    *Iniciar sesión en el portal*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](build-restful-apis-with-aspnet-web-api/_static/image40.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](build-restful-apis-with-aspnet-web-api/_static/image42.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](build-restful-apis-with-aspnet-web-api/_static/image43.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](build-restful-apis-with-aspnet-web-api/_static/image44.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en Azure.

    ![Descargar el perfil de publicación del sitio web](build-restful-apis-with-aspnet-web-api/_static/image45.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image46.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](build-restful-apis-with-aspnet-web-api/_static/image47.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](build-restful-apis-with-aspnet-web-api/_static/image48.png).

    ![Agregando la dirección IP del cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confirmar cambios*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](build-restful-apis-with-aspnet-web-api/_static/image56.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

    ![Aplicación publicada en Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplicación publicada en Windows Azure")

    *Aplicación publicada en Azure*
