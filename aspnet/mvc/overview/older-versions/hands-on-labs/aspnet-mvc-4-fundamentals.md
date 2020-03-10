---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Aspectos básicos de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratorio práctico se basa en la tienda de música de MVC (controlador de vista de modelo), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484573"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Conceptos básicos de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico se basa en la tienda de música de MVC (controlador de vista de modelo), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio. En todo el laboratorio aprenderá la simplicidad, pero podrá usar estas tecnologías juntas. Comenzará con una aplicación sencilla y la compilará hasta que tenga una aplicación Web de ASP.NET MVC 4 totalmente funcional.

Este laboratorio funciona con ASP.NET MVC 4.

Si desea explorar la versión ASP.NET MVC 3 de la aplicación tutorial, puede encontrarla en [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Este laboratorio práctico supone que el desarrollador tiene experiencia en tecnologías de desarrollo web, como HTML y JavaScript.

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>La aplicación Music Store

La aplicación Web de Music Store que se creará en este laboratorio consta de tres partes principales: compras, retirada y administración. Los visitantes podrán explorar álbumes por género, agregar álbumes a su carro, revisar su selección y, por último, proceder a la retirada para iniciar sesión y completar el pedido. Además, los administradores del almacén podrán administrar los álbumes disponibles, así como sus propiedades principales.

![Pantallas de la tienda de música](aspnet-mvc-4-fundamentals/_static/image1.png "Pantallas de la tienda de música")

*Pantallas de la tienda de música*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>ASP.NET MVC 4 Essentials

La aplicación Music Store se generará con el **controlador de vista de modelo (MVC)** , un patrón arquitectónico que separa una aplicación en tres componentes principales:

- **Modelos**: los objetos de modelo son las partes de la aplicación que implementan la lógica del dominio. A menudo, los objetos de modelo también recuperan y almacenan el estado del modelo en una base de datos.
- **Vistas:** Las vistas son los componentes que muestran la interfaz de usuario (UI) de la aplicación. Normalmente, esta interfaz de usuario se crea a partir de los datos de modelo. Un ejemplo sería la vista de edición de álbumes que muestra cuadros de texto y una lista desplegable basada en el estado actual de un objeto Album.
- **Controladores:** Los controladores son los componentes que controlan la interacción con el usuario, manipulan el modelo y, en última instancia, seleccionan una vista para representar la interfaz de usuario. En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.

El patrón MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica de negocios y lógica de la interfaz de usuario), a la vez que proporciona un acoplamiento flexible entre estos elementos. Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que le permite centrarse en un aspecto de la implementación a la vez. Además, el modelo de MVC facilita la prueba de las aplicaciones, fomentando también el uso de desarrollo controlado por pruebas (TDD) para crear aplicaciones.

El marco de **MVC de ASP.net** proporciona una alternativa al patrón de formularios Web Forms de ASP.net para crear aplicaciones web basadas en mvc de ASP.net. El marco de **MVC de ASP.net** es un marco de presentación ligero y muy comprobable que, al igual que con las aplicaciones basadas en formularios Web, se integra con las características de ASP.net existentes, como las páginas maestras y la autenticación basada en pertenencia, de modo que se obtiene toda la eficacia de .NET Framework principal. Esto resulta útil si ya está familiarizado con los formularios Web Forms de ASP.NET, ya que todas las bibliotecas que ya usa también están disponibles en ASP.NET MVC 4.

Además, el acoplamiento flexible entre los tres componentes principales de una aplicación MVC también promueve el desarrollo paralelo. Por ejemplo, un desarrollador puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y un tercer desarrollador puede centrarse en la lógica de negocios del modelo.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Creación de una aplicación de ASP.NET MVC desde cero basada en el tutorial de la aplicación Music Store
- Agregar controladores para controlar las direcciones URL en la Página principal del sitio y para examinar su funcionalidad principal
- Agregar una vista para personalizar el contenido que se muestra junto con su estilo
- Agregar clases de modelo para contener y administrar la lógica de datos y dominios
- Usar modelo de modelo de vista para pasar información de las acciones del controlador a las plantillas de vista
- Exploración de la nueva plantilla de ASP.NET MVC 4 para aplicaciones de Internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los siguientes elementos para completar este laboratorio:

- [Visual Studio 2012 Express para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (lea [el Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalar fragmentos de código**

Para mayor comodidad, gran parte del código que va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar el archivo **.\Source\Setup\CodeSnippets.VSI** de ejecución de fragmentos de código.

Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice C: usar fragmentos de código](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico se compone de los siguientes ejercicios:

1. [Ejercicio 1: creación de un proyecto de aplicación web MVC de MusicStore ASP.NET](#Exercise1)
2. [Ejercicio 2: creación de un controlador](#Exercise2)
3. [Ejercicio 3: pasar parámetros a un controlador](#Exercise3)
4. [Ejercicio 4: crear una vista](#Exercise4)
5. [Ejercicio 5: crear un modelo de vista](#Exercise5)
6. [Ejercicio 6: uso de parámetros en la vista](#Exercise6)
7. [Ejercicio 7: una nueva plantilla de ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Ejercicio 1: creación de un proyecto de aplicación web MVC de MusicStore ASP.NET

En este ejercicio, obtendrá información sobre cómo crear una aplicación ASP.NET MVC en Visual Studio 2012 Express para Web, así como en su organización de carpetas principal. Además, aprenderá a agregar un nuevo controlador y a mostrar una cadena simple en la Página principal de la aplicación.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tarea 1: creación del proyecto de aplicación web MVC de ASP.NET

1. En esta tarea, creará un proyecto de aplicación MVC de ASP.NET vacío mediante la plantilla de Visual Studio MVC. Inicie **vs Express para web**.
2. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
3. En el cuadro de diálogo **nuevo proyecto** , seleccione el tipo de proyecto **aplicación web MVC 4 ASP.net** , que se encuentra en **Visual C#,** lista de plantillas **Web** .
4. Cambie el **nombre** a *MvcMusicStore*.
5. Establezca la ubicación de la solución dentro de una nueva carpeta de **Inicio** en la carpeta de origen de este ejercicio, por ejemplo **[Your-Hol-path] \Source\Ex01-CreatingMusicStoreProject\Begin**. Haga clic en **Aceptar**.

    ![Cuadro de diálogo Crear nuevo proyecto](aspnet-mvc-4-fundamentals/_static/image2.png "Cuadro de diálogo Crear nuevo proyecto")

    *Cuadro de diálogo Crear nuevo proyecto*
6. En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla **básica** y asegúrese de que el **motor de vista** seleccionado sea **Razor**. Haga clic en **Aceptar**.

    ![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4")

    *Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tarea 2: explorar la estructura de la solución

El marco de MVC de ASP.NET incluye una plantilla de proyecto de Visual Studio que le ayuda a crear aplicaciones Web compatibles con el patrón MVC. Esta plantilla crea una nueva aplicación web MVC ASP.NET con las carpetas, plantillas de elementos y entradas de archivo de configuración necesarias.

En esta tarea, examinará la estructura de la solución para comprender los elementos implicados y sus relaciones. Las siguientes carpetas se incluyen en toda la aplicación ASP.NET MVC porque el marco ASP.NET MVC usa de forma predeterminada una Convención de &quot;sobre el enfoque de configuración&quot; y realiza algunas suposiciones predeterminadas en función de las convenciones de nomenclatura de carpetas.

1. Una vez creado el proyecto, revise la estructura de carpetas que se ha creado en el Explorador de soluciones del lado derecho:

    ![ASP.NET estructura de carpetas de MVC en Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET estructura de carpetas de MVC en Explorador de soluciones")

    *ASP.NET estructura de carpetas de MVC en Explorador de soluciones*

   1. **Controladores**. Esta carpeta contendrá las clases de controlador. En una aplicación basada en MVC, los controladores son responsables de administrar la interacción con el usuario final, manipular el modelo y, en última instancia, elegir una vista para representar la interfaz de usuario.

       > [!NOTE]
       > El marco de MVC requiere que los nombres de todos los controladores terminen con &quot;controlador&quot;(por ejemplo, HomeController, LoginController o ProductController).
   2. **Modelos**. Esta carpeta se proporciona para las clases que representan el modelo de aplicación de la aplicación web MVC. Normalmente incluye código que define los objetos y la lógica para interactuar con el almacén de datos. Normalmente, los objetos del modelo real estarán en bibliotecas de clases independientes. Sin embargo, cuando se crea una nueva aplicación, se pueden incluir clases y, a continuación, moverlas a bibliotecas de clases independientes en un punto posterior del ciclo de desarrollo.
   3. **Vistas**. Esta carpeta es la ubicación recomendada para las vistas, los componentes responsables de mostrar la interfaz de usuario de la aplicación. Las vistas usan archivos. aspx,. ascx,. cshtml y. Master, además de otros archivos relacionados con las vistas de representación. La carpeta views contiene una carpeta para cada controlador. la carpeta se denomina con el prefijo del nombre del controlador. Por ejemplo, si tiene un controlador denominado **HomeController**, la carpeta views contendrá una carpeta denominada Home. De forma predeterminada, cuando el marco de ASP.NET MVC carga una vista, busca un archivo. aspx con el nombre de vista solicitado en la carpeta Views\controllerName (**views [controllerName] [Action]. aspx**) o (**views [ControllerName] [Action]. cshtml**) para las vistas de Razor.

      > [!NOTE]
      > Además de las carpetas enumeradas anteriormente, una aplicación web MVC utiliza el archivo **global. asax** para establecer los valores predeterminados de enrutamiento de URL global y usa el archivo **Web. config** para configurar la aplicación.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tarea 3: agregar un HomeController

En las aplicaciones de ASP.NET que no usan el marco MVC, la interacción con el usuario se organiza en torno a las páginas y en torno a la generación y control de eventos de esas páginas. Por el contrario, la interacción del usuario con aplicaciones ASP.NET MVC se organiza en torno a los controladores y sus métodos de acción.

Por otro lado, el marco de MVC de ASP.NET asigna las direcciones URL a las clases a las que se hace referencia como controladores. Los controladores procesan las solicitudes entrantes, controlan las interacciones y los datos proporcionados por el usuario, ejecutan la lógica de aplicación adecuada y determinan la respuesta que se devuelve al cliente (muestran HTML, descargan un archivo, redirigen a una dirección URL diferente, etc.). En el caso de mostrar HTML, una clase de controlador normalmente llama a un componente de vista independiente para generar el marcado HTML para la solicitud. En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.

En esta tarea, agregará una clase de controlador que controlará las direcciones URL de la Página principal del sitio de la tienda de música.

1. Haga clic con el botón derecho en la carpeta **Controllers** dentro del explorador de soluciones, seleccione **Agregar** y, a continuación, comando del **controlador** :

    ![Agregar un comando de controlador](aspnet-mvc-4-fundamentals/_static/image5.png "Agregar un comando de controlador")

    *Agregar controlador (comando)*
2. Aparece el cuadro de diálogo **Agregar controlador** . Asigne al controlador el nombre *HomeController* y presione **Agregar**.

    ![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Cuadro de diálogo Agregar controlador")

    *Cuadro de diálogo Agregar controlador*
3. El archivo **HomeController.CS** se crea en la carpeta **Controllers** . Para que el **HomeController** devuelva una cadena en su acción de índice, reemplace el método de **Índice** por el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX1 HomeController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, probará la aplicación en un explorador Web.

1. Presione **F5** para ejecutar la aplicación. El proyecto se compila y se inicia el servidor Web de IIS local. El servidor Web de IIS local abrirá automáticamente un explorador Web que apunte a la dirección URL del servidor Web.

    ![Aplicación que se ejecuta en un explorador Web](aspnet-mvc-4-fundamentals/_static/image7.png "Aplicación que se ejecuta en un explorador Web")

    *Aplicación que se ejecuta en un explorador Web*

    > [!NOTE]
    > El servidor Web de IIS local ejecutará el sitio web en un número de puerto libre aleatorio. En la ilustración anterior, el sitio se ejecuta en `http://localhost:50103/`, por lo que usa el puerto 50103. El número de puerto puede variar.
2. Cierre el explorador.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Ejercicio 2: creación de un controlador

En este ejercicio, obtendrá información sobre cómo actualizar el controlador para implementar una funcionalidad sencilla de la aplicación Music Store. Ese controlador definirá métodos de acción para controlar cada una de las siguientes solicitudes específicas:

- Página de lista de los géneros musicales en la tienda de música
- Página de exploración en la que se muestran todos los álbumes de música de un género determinado.
- Una página de detalles que muestra información sobre un álbum de música específico

Para el ámbito de este ejercicio, esas acciones simplemente devolverán una cadena en este momento.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tarea 1: agregar una nueva clase StoreController

En esta tarea, agregará un nuevo controlador.

1. Si aún no está abierto, inicie **VS Express para Web 2012**.
2. En el menú **archivo** , elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex02-CreatingAController\Begin**, seleccione **Begin. sln** y haga clic en **abrir**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. Agregue el nuevo controlador. Para ello, haga clic con el botón secundario en la carpeta **Controllers** dentro del explorador de soluciones, seleccione **Agregar** y, a continuación, el comando **controlador** . Cambie el **nombre del controlador** a *StoreController*y haga clic en **Agregar**.

    ![Cuadro de diálogo Agregar controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Cuadro de diálogo Agregar controlador")

    *Cuadro de diálogo Agregar controlador*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tarea 2: modificación de las acciones de StoreController

En esta tarea, modificará los métodos de controlador que se denominan **acciones**. Las acciones son responsables de administrar las solicitudes de URL y de determinar el contenido que se debe devolver al explorador o al usuario que invocó la dirección URL.

1. La clase **StoreController** ya tiene un método de **Índice** . La usará más adelante en este laboratorio para implementar la página en la que se enumeran todos los géneros de la tienda de música. Por ahora, solo tiene que reemplazar el método de **Índice** por el código siguiente, que devuelve una cadena &quot;Hello de Store. index ()&quot;:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex2 StoreController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Agregue los métodos **Browse** y **Details** . Para ello, agregue el código siguiente al **StoreController**:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecución de la aplicación

En esta tarea, probará la aplicación en un explorador Web.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página **principal** . Cambie la dirección URL para comprobar la implementación de cada acción.

    1. **/Store**. Verá **&quot;Hola del&quot;Store. index ()** .
    2. **/Store/Browse**. Verá **&quot;Hola de Store. browse ()&quot;** .
    3. **/Store/details**. Verá **&quot;Hola en Store. Details ()&quot;** .

        ![Examinar StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Examinar StoreBrowse")

        *Examinar/Store/Browse*
3. Cierre el explorador.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Ejercicio 3: pasar parámetros a un controlador

Hasta ahora, se han devuelto cadenas de constantes de los controladores. En este ejercicio aprenderá a pasar parámetros a un controlador mediante la dirección URL y la cadena de texto, y, a continuación, hará que las acciones del método respondan con texto al explorador.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tarea 1: agregar el parámetro Genre a StoreController

En esta tarea, usará la **cadena** de consulta para enviar parámetros al método de acción de **exploración** en el **StoreController**.

1. Si aún no está abierto, inicie **vs Express para web**.
2. En el menú **archivo** , elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex03-PassingParametersToAController\Begin**, seleccione **Begin. sln** y haga clic en **abrir**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. Abra la clase **StoreController** . Para ello, en el **Explorador de soluciones**, expanda la carpeta **Controllers** y haga doble clic en **StoreController.CS**.
4. Cambie el método de **exploración** y agregue un parámetro de cadena para solicitar un género específico. ASP.NET MVC pasará automáticamente los parámetros QueryString o post de formulario denominados **Genre** a este método de acción cuando se invoque. Para ello, reemplace el método **Browse** por el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> Está usando el método de la utilidad **HttpUtility. HtmlEncode** para evitar que los usuarios inserten JavaScript en la vista con un vínculo como **/Store/Browse? Genre =&lt;script&gt;Window. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .
> 
> Para obtener más información, visite [este artículo de MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecución de la aplicación

En esta tarea, probará la aplicación en un explorador Web y usará el parámetro **Genre** .

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página **principal** . Cambie la dirección URL a */Store/Browse? Genre = disco* para comprobar que la acción recibe el parámetro Genre.

    ![Exploración de StoreBrowseGenre = disco](aspnet-mvc-4-fundamentals/_static/image10.png "Exploración de StoreBrowseGenre = disco")

    *¿Examinar/Store/Browse? Género = disco*
3. Cierre el explorador.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tarea 3: agregar un parámetro de identificador incrustado en la dirección URL

En esta tarea, usará la **dirección URL** para pasar un parámetro de **identificador** al método de acción **Details** de **StoreController**. La Convención de enrutamiento predeterminada de MVC de ASP.NET consiste en tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado **ID**. Si el método de acción tiene un parámetro denominado ID, ASP.NET MVC pasará automáticamente el segmento de dirección URL como parámetro. En el almacén de direcciones URL **/detalles/5**, el **identificador** se interpretará como **5**.

1. Cambie el método de **detalles** de **StoreController**, agregando un parámetro **int** denominado **ID**. Para ello, reemplace el método **Details** con el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, probará la aplicación en un explorador Web y usará el parámetro **ID** .

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página **principal** . Cambie la dirección URL a */Store/details/5* para comprobar que la acción recibe el parámetro id.

    ![Examinar StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Examinar StoreDetails5")

    *Examinar/Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Ejercicio 4: crear una vista

Hasta ahora se han devuelto cadenas desde las acciones del controlador. Aunque es una manera útil de comprender cómo funcionan los controladores, no es cómo se compilan las aplicaciones Web reales. Las vistas son componentes que proporcionan un mejor enfoque para volver a generar HTML en el explorador con el uso de archivos de plantilla.

En este ejercicio aprenderá a agregar una página maestra de diseño para configurar una plantilla para contenido HTML común, una hoja de estilos para mejorar la apariencia y el funcionamiento del sitio y, por último, una plantilla de vista para habilitar HomeController para que devuelva HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a>Tarea 1: modificación del archivo \_layout. cshtml

El archivo **~/Views/Shared/\_layout. cshtml** permite configurar una plantilla para el HTML común que se usará en todo el sitio Web. En esta tarea, agregará una página maestra de diseño con un encabezado común con vínculos a la Página principal y al área de almacenamiento.

1. Si aún no está abierto, inicie **vs Express para web**.
2. En el menú **archivo** , elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex04-CreatingAView\Begin**, seleccione **Begin. sln** y haga clic en **abrir**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. El archivo <strong>\_layout. cshtml</strong> contiene el diseño del contenedor HTML para todas las páginas del sitio. Incluye el&lt;elemento de <strong>&gt;HTML</strong> para la respuesta HTML, así como los elementos de <strong>&gt;de encabezado&lt;</strong> y <strong>&lt;cuerpo</strong> .&gt; <strong>@RenderBody()</strong> en el cuerpo HTML identifican las regiones que las plantillas de vistas podrán rellenar con contenido dinámico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Agregue un encabezado común con vínculos a la Página principal y al área de almacenamiento en todas las páginas del sitio. Para ello, agregue el siguiente código debajo de &lt;instrucción Body&gt;.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Incluya un elemento div para representar la sección de cuerpo de cada página. Reemplace <strong>@RenderBody()</strong> por el siguiente código resaltadoC#: ()

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > ¿Sabía que? Visual Studio 2012 tiene fragmentos de código que facilitan la tarea de agregar código de uso común en HTML, archivos de código, etc. Para probarlo, escriba **&lt;div&gt;** y presione la **tecla TAB** dos veces para insertar una etiqueta **div** completa.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tarea 2: agregar una hoja de estilos CSS

La plantilla de proyecto vacía incluye un archivo CSS muy simplificado que solo incluye estilos que se usan para mostrar los mensajes de validación y formularios básicos. Usará CSS y imágenes adicionales (potencialmente proporcionadas por un diseñador) para mejorar la apariencia y el funcionamiento del sitio.

En esta tarea, agregará una hoja de estilos CSS para definir los estilos del sitio.

1. El archivo CSS y las imágenes se incluyen en la carpeta **Source\Assets\Content** de este laboratorio. Para agregarlas a la aplicación, arrastre su contenido desde una ventana del **Explorador de Windows** hasta el **Explorador de soluciones** en Visual Studio Express para Web, como se muestra a continuación:

    ![Arrastrar contenido de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "Arrastrar contenido de estilo")

    *Arrastrar contenido de estilo*
2. Aparecerá un cuadro de diálogo de advertencia en el que se le pide confirmación para reemplazar el archivo **site. CSS** y algunas imágenes existentes. Active **aplicar a todos los elementos** y haga clic en **sí**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tarea 3: agregar una plantilla de vista

En esta tarea, agregará una plantilla de vista para generar la respuesta HTML que utilizará la página maestra de diseño y la CSS agregada en este ejercicio.

1. Para usar una plantilla de vista al examinar la Página principal del sitio, primero debe indicar que, en lugar de devolver una cadena, el método de **Índice de HomeController** devolverá una **vista**. Abra la clase **HomeController** y cambie su método de **Índice** para que devuelva un **ActionResult**y haga que devuelva la **vista ()** .

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex4 HomeController index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Ahora, debe agregar una plantilla de vista adecuada. Para ello, **haga clic con el botón derecho en** el método de acción de **Índice** y seleccione **Agregar vista**. Se abrirá el cuadro de diálogo **Agregar vista** .

    ![Agregar una vista desde el método index](aspnet-mvc-4-fundamentals/_static/image13.png "Agregar una vista desde el método index")

    *Agregar una vista desde el método index*
3. Aparecerá el cuadro de diálogo **Agregar vista** para generar un archivo de plantilla de vista. De forma predeterminada, este cuadro de diálogo rellena previamente el nombre de la plantilla de vista para que coincida con el método de acción que la usará. Dado que usó el menú contextual **Agregar vista** en el método de acción de **Índice** dentro del HomeController, el cuadro de diálogo **Agregar vista** tiene el índice como nombre de vista predeterminado. Haga clic en **Agregar**.

    ![Cuadro de diálogo Agregar vista](aspnet-mvc-4-fundamentals/_static/image14.png "Cuadro de diálogo Agregar vista")

    *Cuadro de diálogo Agregar vista*
4. Visual Studio genera una plantilla de vista **index. cshtml** dentro de la carpeta **Views\Home** y, a continuación, la abre.

    ![Vista de índice de inicio creada](aspnet-mvc-4-fundamentals/_static/image15.png "Vista de índice de inicio creada")

    *Vista de índice de inicio creada*

    > [!NOTE]
    > el nombre y la ubicación del archivo **index. cshtml** es relevante y sigue las convenciones de nomenclatura predeterminadas de ASP.NET MVC.
    > 
    > La carpeta \Views\**Home** coincide con el nombre del controlador (**Home** Controller). El nombre de la plantilla de vista (**Índice**) coincide con el método de acción del controlador que mostrará la vista.
    > 
    > De este modo, ASP.NET MVC evita tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se usa esta Convención de nomenclatura para devolver una vista.
5. La plantilla de vista generada se basa en la plantilla **\_layout. cshtml** que se ha definido anteriormente. Actualice la propiedad ViewBag. title a **Home**y cambie el contenido principal a **esta página principal**, como se muestra en el código siguiente:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Seleccione el proyecto **MvcMusicStore** en el explorador de soluciones y presione **F5** para ejecutar la aplicación.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tarea 4: comprobación

Para comprobar que ha realizado correctamente todos los pasos del ejercicio anterior, realice lo siguiente:

Con la aplicación abierta en un explorador, debe tener en cuenta lo siguiente:

1. El método de acción de índice de HomeController encontró y mostraba la plantilla de vista **\Views\Home\Index.cshtml** , aunque el código llamó a la **vista de devolución ()** , porque la plantilla de vista siguió la Convención de nomenclatura estándar.
2. En la Página principal se muestra el mensaje de bienvenida definido en la plantilla de vista **\Views\Home\Index.cshtml** .
3. La Página principal usa la plantilla **\_layout. cshtml** y, por tanto, el mensaje de bienvenida está incluido en el diseño HTML del sitio estándar.

    ![Vista de índice de inicio con el LayoutPage y estilo definidos](aspnet-mvc-4-fundamentals/_static/image16.png "Vista de índice de inicio con el LayoutPage y estilo definidos")

    *Vista de índice de inicio con el LayoutPage y estilo definidos*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Ejercicio 5: crear un modelo de vista

Hasta ahora, hizo que las vistas mostraran código HTML codificado, pero, para crear aplicaciones web dinámicas, la plantilla de vista debe recibir información del controlador. Una técnica común que se usa para ese propósito es el patrón **ViewModel** , que permite a un controlador empaquetar toda la información necesaria para generar la respuesta HTML adecuada.

En este ejercicio, obtendrá información sobre cómo crear una clase ViewModel y agregar las propiedades necesarias: el número de géneros en el almacén y una lista de esos géneros. También actualizará StoreController para usar el ViewModel creado y, por último, creará una nueva plantilla de vista que mostrará las propiedades mencionadas en la página.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tarea 1: creación de una clase ViewModel

En esta tarea, creará una clase ViewModel que implementará el escenario de anuncio de género de tienda.

1. Si aún no está abierto, inicie **vs Express para web**.
2. En el menú **archivo** , elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex05-CreatingAViewModel\Begin**, seleccione **Begin. sln** y haga clic en **abrir**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. Cree una carpeta **ViewModels** para contener el ViewModel. Para ello, haga clic con el botón derecho en el proyecto **MvcMusicStore** de nivel superior, seleccione **Agregar** y, a continuación, **nueva carpeta**.

    ![Agregar una nueva carpeta](aspnet-mvc-4-fundamentals/_static/image17.png "Agregar una nueva carpeta")

    *Agregar una nueva carpeta*
4. Asigne a la carpeta el nombre *ViewModels*.

    ![Carpeta ViewModels en Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image18.png "Carpeta ViewModels en Explorador de soluciones")

    *Carpeta ViewModels en Explorador de soluciones*
5. Cree una clase **ViewModel** . Para ello, haga clic con el botón derecho en la carpeta **ViewModels** que ha creado recientemente, seleccione **Agregar** y, a continuación, **nuevo elemento**. En **código**, elija el elemento de **clase** y asigne al archivo el nombre *StoreIndexViewModel.CS*y, a continuación, haga clic en **Agregar**.

    ![Agregar una nueva clase](aspnet-mvc-4-fundamentals/_static/image19.png "Agregar una nueva clase")

    *Agregar una nueva clase*

    ![Crear la clase StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Crear la clase StoreIndexViewModel")

    *Crear la clase StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tarea 2: agregar propiedades a la clase ViewModel

Hay dos parámetros que se pasarán desde StoreController a la plantilla de vista para generar la respuesta HTML esperada: el número de géneros en el almacén y una lista de esos géneros.

En esta tarea, agregará esas dos propiedades a la clase **StoreIndexViewModel** : **NumberOfGenres** (un entero) y **géneros** (una lista de cadenas).

1. Agregue las propiedades **NumberOfGenres** y **Genre** a la clase **StoreIndexViewModel** . Para ello, agregue las dos líneas siguientes a la definición de clase:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel Properties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> La notación **{Get; Set;}** hace uso de C#la característica de propiedades implementadas automáticamente de. Proporciona las ventajas de una propiedad sin necesidad de declarar un campo de respaldo.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tarea 3: actualización de StoreController para usar StoreIndexViewModel

La clase **StoreIndexViewModel** encapsula la información necesaria para pasar del método **index** de **StoreController**a una plantilla de vista con el fin de generar una respuesta.

En esta tarea, actualizará el **StoreController** para usar **StoreIndexViewModel**.

1. Abra la clase **StoreController** .

    ![Apertura de la clase StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Apertura de la clase StoreController")

    *Apertura de la clase StoreController*
2. Para usar la clase **StoreIndexViewModel** de **StoreController**, agregue el siguiente espacio de nombres en la parte superior del código **StoreController** :

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel con ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Cambie el método de acción de **Índice** de **StoreController**para que cree y rellene un objeto **StoreIndexViewModel** y, a continuación, lo pase a una plantilla de vista para generar una respuesta HTML con él.

    > [!NOTE]
    > En Lab &quot;los modelos y el acceso a datos de ASP.NET MVC&quot; escribirá código que recupera la lista de géneros de almacén de una base de datos. En el código siguiente, creará una **lista** de géneros de datos ficticios que rellenará el **StoreIndexViewModel**.
    > 
    > Después de crear y configurar el objeto **StoreIndexViewModel** , se pasará como argumento al método **View** . Esto indica que la plantilla de vista utilizará ese objeto para generar una respuesta HTML.
4. Reemplace el método de **Índice** por el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreController index Method*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Si no está familiarizado con C#, puede suponer que el uso de **var** significa que la variable **viewModel** está enlazada en tiempo de ejecución. Esto no es correcto: el C# compilador usa la inferencia de tipos basada en lo que se asigna a la variable para determinar que **viewModel** es de tipo **StoreIndexViewModel**. Además, al compilar la variable de **viewModel** local como un tipo **StoreIndexViewModel** , obtiene la comprobación en tiempo de compilación y la compatibilidad con el editor de código de Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tarea 4: creación de una plantilla de vista que usa StoreIndexViewModel

En esta tarea, creará una plantilla de vista que usará un objeto StoreIndexViewModel pasado desde el controlador para mostrar una lista de géneros.

1. Antes de crear la nueva plantilla de vista, vamos a compilar el proyecto para que el **cuadro de diálogo Agregar vista** Conozca la clase **StoreIndexViewModel** . Compile el proyecto; para ello, seleccione el elemento de menú **compilar** y, a continuación, **compile MvcMusicStore**.

    ![Compilar el proyecto](aspnet-mvc-4-fundamentals/_static/image22.png "Compilar el proyecto")

    *Compilar el proyecto*
2. Cree una nueva plantilla de vista. Para ello, haga clic con el botón derecho en el método de **Índice** y seleccione **Agregar vista**.

    ![Agregar una vista](aspnet-mvc-4-fundamentals/_static/image23.png "Agregar una vista")

    *Agregar una vista*
3. Dado que el **cuadro de diálogo Agregar vista** se invocó desde **StoreController**, agregará la plantilla de vista de forma predeterminada en un archivo **\Views\Store\Index.cshtml** . Active la casilla **crear una vista fuertemente tipada** y, a continuación, seleccione **StoreIndexViewModel** como **clase de modelo**. Además, asegúrese de que el motor de vista seleccionado sea **Razor**. Haga clic en **Agregar**.

    ![Cuadro de diálogo Agregar vista](aspnet-mvc-4-fundamentals/_static/image24.png "Cuadro de diálogo Agregar vista")

    *Cuadro de diálogo Agregar vista*

    El archivo de plantilla de vista **\Views\Store\Index.cshtml** se crea y se abre. En función de la información que se proporciona al cuadro de diálogo **Agregar vista** en el último paso, la plantilla de vista esperará una instancia de **StoreIndexViewModel** como los datos que se van a usar para generar una respuesta HTML. Observará que la plantilla hereda un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tarea 5: actualización de la plantilla de vista

En esta tarea, actualizará la plantilla de vista creada en la última tarea para recuperar el número de géneros y sus nombres dentro de la página.

> [!NOTE]
> Usará @ Syntax (a menudo conocido como &quot;códigos de código&quot;) para ejecutar código dentro de la plantilla de vista.

1. En el archivo **index. cshtml** , dentro de la carpeta **Store** , reemplace su código por lo siguiente:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Recorra en iteración la lista género de **StoreIndexViewModel** y cree una lista HTML **&lt;UL&gt;** mediante un bucle **foreach** .
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Presione **F5** para ejecutar la aplicación y examinar **/Store**. Verá la lista de géneros pasados en el objeto **StoreIndexViewModel** de **StoreController** a la plantilla de vista.

    ![Vista que muestra una lista de géneros](aspnet-mvc-4-fundamentals/_static/image26.png "Vista que muestra una lista de géneros")

    *Vista que muestra una lista de géneros*
4. Cierre el explorador.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Ejercicio 6: uso de parámetros en la vista

En el ejercicio 3 aprendió a pasar parámetros al controlador. En este ejercicio, obtendrá información sobre cómo usar esos parámetros en la plantilla de vista. Para ello, se introducirá primero en las clases de modelo que le ayudarán a administrar los datos y la lógica del dominio. Además, aprenderá a crear vínculos a páginas dentro de la aplicación ASP.NET MVC sin preocuparse por cosas como la codificación de rutas de acceso URL.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tarea 1: agregar clases de modelo

A diferencia de ViewModels, que se crean solo para pasar información del controlador a la vista, las clases de modelo se compilan para contener y administrar la lógica de los datos y del dominio. En esta tarea, agregará dos clases de modelo para representar estos conceptos: **Genre** y **album**.

1. Si aún no está abierto, inicie **vs Express para web**
2. En el menú **archivo** , elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex06-UsingParametersInView\Begin**, seleccione **Begin. sln** y haga clic en **abrir**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
3. Agregue una clase de modelo **Genre** . Para ello, haga clic con el botón secundario en la carpeta **modelos** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, la opción **nuevo elemento** . En **código**, elija el elemento de **clase** y asigne al archivo el nombre *Genre.CS*y, a continuación, haga clic en **Agregar**.

    ![Agregar una clase](aspnet-mvc-4-fundamentals/_static/image27.png "Agregar una clase")

    *Agregar un nuevo elemento*

    ![Agregar clase de modelo de género](aspnet-mvc-4-fundamentals/_static/image28.png "Agregar clase de modelo de género")

    *Agregar clase de modelo de género*
4. Agregue una propiedad **Name** a la clase Genre. Para ello, agregue el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 Genre*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. Siguiendo el mismo procedimiento que antes, agregue una clase **album** . Para ello, haga clic con el botón secundario en la carpeta **modelos** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, la opción **nuevo elemento** . En **código**, elija el elemento de **clase** y asigne al archivo el nombre *Album.CS*y, a continuación, haga clic en **Agregar**.
6. Agregue dos propiedades a la clase Album: **género** y **título**. Para ello, agregue el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 album*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tarea 2: adición de un StoreBrowseViewModel

Se usará una **StoreBrowseViewModel** en esta tarea para mostrar los álbumes que coinciden con un género seleccionado. En esta tarea, creará esta clase y, a continuación, agregará dos propiedades para controlar el **género** y la lista de su **álbum**.

1. Agregue una clase **StoreBrowseViewModel** . Para ello, haga clic con el botón secundario en la carpeta **ViewModels** en el **Explorador de soluciones**, seleccione **Agregar** y, a continuación, la opción **nuevo elemento** . En **código**, elija el elemento de **clase** y asigne al archivo el nombre *StoreBrowseViewModel.CS*y, a continuación, haga clic en **Agregar**.
2. Agregue una referencia a los modelos de la clase **StoreBrowseViewModel** . Para ello, agregue lo siguiente mediante el espacio de nombres:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Agregue dos propiedades a la clase **StoreBrowseViewModel** : **género** y **álbumes**. Para ello, agregue el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> ¿Qué es la **lista&lt;álbum&gt;** ?: esta definición usa el tipo de **&gt;de lista&lt;t** , donde **t** restringe el tipo al que pertenecen los elementos de esta **lista** , en este caso **album** (o cualquiera de sus descendientes).
> 
> Esta capacidad de diseñar clases y métodos que aplazan la especificación de uno o más tipos hasta que se declara la clase o el método y el código de cliente crea una instancia C# del mismo, una característica del lenguaje denominada **genéricos**.
> 
> **List&lt;t&gt;** es el equivalente genérico del tipo **ArrayList** y está disponible en el espacio de nombres **System. Collections. Generic** . Una de las ventajas de usar **genéricos** es que, puesto que se especifica el tipo, no es necesario que se ocupe de las operaciones de comprobación de tipos, como convertir los elementos en un **álbum** como haría con una **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tarea 3: uso del nuevo ViewModel en el StoreController

En esta tarea, modificará los métodos de acción **Browse** y **Details** de **StoreController**para usar el nuevo **StoreBrowseViewModel**.

1. Agregue una referencia a la carpeta models en la clase **StoreController** . Para ello, expanda la carpeta **Controllers** en el **Explorador de soluciones** y abra la clase **StoreController** . A continuación, agregue el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Reemplace el método de acción **Browse** para usar la clase **StoreViewBrowseController** . Creará un género y dos nuevos objetos álbumes con datos ficticios (en el siguiente laboratorio práctico usará datos reales de una base de datos). Para ello, reemplace el método **Browse** por el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Reemplace el método de acción **Details** para usar la clase **StoreViewBrowseController** . Creará un nuevo objeto de **álbum** que se va a devolver a la **vista**. Para ello, reemplace el método **Details** con el código siguiente:

    (Fragmento de código- *ASP.NET MVC 4 Fundamentals-EX6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tarea 4: agregar una plantilla de vista de exploración

En esta tarea, agregará una vista **examinar** para mostrar los álbumes encontrados para un género específico.

1. Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el cuadro de diálogo **Agregar vista** Conozca la clase **ViewModel** que se va a usar. Compile el proyecto; para ello, seleccione el elemento de menú **compilar** y, a continuación, **compile MvcMusicStore**.
2. Agregue una vista **examinar** . Para ello, haga clic con el botón secundario en el método de acción **Browse** de **StoreController** y haga clic en **Agregar vista**.
3. En el cuadro de diálogo **Agregar vista** , compruebe que el nombre de la vista es **examinar**. Active la casilla **crear una vista fuertemente tipada** y seleccione **StoreBrowseViewModel** en la lista desplegable **clase de modelo** . Deje los demás campos con su valor predeterminado. A continuación, haga clic en **Agregar**.

    ![Agregar una vista examinar](aspnet-mvc-4-fundamentals/_static/image29.png "Agregar una vista examinar")

    *Agregar una vista examinar*
4. Modifique el objeto **Browse. cshtml** para mostrar la información del género, teniendo acceso al objeto **StoreBrowseViewModel** que se pasa a la plantilla de vista. Para ello, reemplace el contenido por lo siguiente: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, probará que el método **Browse** recupera álbumes de la acción **Browse** Method.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/Store/Browse? Genre = disco** para comprobar que la acción devuelve dos álbumes.

    ![Examinar álbumes en disco de almacén](aspnet-mvc-4-fundamentals/_static/image30.png "Examinar álbumes en disco de almacén")

    *Examinar álbumes en disco de almacén*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tarea 6: Mostrar información acerca de un álbum específico

En esta tarea, implementará la vista de **almacén y detalles** para mostrar información sobre un álbum específico. En este laboratorio práctico, todo lo que se muestra sobre el álbum ya está incluido en la plantilla de **vista** . Por lo tanto, en lugar de crear una clase **StoreDetailsViewModel** , utilizará la plantilla **StoreBrowseViewModel** actual pasándole el álbum.

1. Cierre el explorador si es necesario para volver a la ventana de Visual Studio. Agregue una nueva vista de **detalles** para el método de acción de **detalles** de **StoreController**. Para ello, haga clic con el botón secundario en el método **Details** de la clase **StoreController** y haga clic en **Agregar vista**.
2. En el cuadro de diálogo **Agregar vista** , compruebe que el nombre de la **vista** sea **detalles**. Active la casilla **crear una vista fuertemente tipada** y seleccione **álbum** en la lista desplegable **clase de modelo** . Deje los demás campos con su valor predeterminado. A continuación, haga clic en **Agregar**. Se creará y se abrirá un archivo **\Views\Store\Details.cshtml** .

    ![Agregar una vista de detalles](aspnet-mvc-4-fundamentals/_static/image31.png "Agregar una vista de detalles")

    *Agregar una vista de detalles*
3. Modifique el archivo **details. cshtml** para mostrar la información del álbum, teniendo acceso al objeto de **álbum** que se pasa a la plantilla de vista. Para ello, reemplace el contenido por lo siguiente: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarea 7: ejecución de la aplicación

En esta tarea, probará que la vista de **detalles** recupera la información del álbum del método de **acción detalles** .

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página **principal** . Cambie la dirección URL a **/Store/details/5** para comprobar la información del álbum.

    ![Detalles de la exploración de álbumes](aspnet-mvc-4-fundamentals/_static/image32.png "Detalles de la exploración de álbumes")

    *Examinar los detalles del álbum*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tarea 8: agregar vínculos entre páginas

En esta tarea, agregará un vínculo en la vista de almacén para tener un vínculo en cada nombre de género a la dirección URL de **/Store/Browse** adecuada. De este modo, al hacer clic en un género, por **ejemplo,** se desplazará a **/Store/Browse? Genre =** dirección URL de disco.

1. Cierre el explorador si es necesario para volver a la ventana de Visual Studio. Actualice la página de **Índice** para agregar un vínculo a la página de **exploración** . Para ello, en el **Explorador de soluciones** expanda la carpeta **views** , la carpeta **Store** y haga doble clic en la página **index. cshtml** .
2. Agregue un vínculo a la vista examinar que indica el género seleccionado. Para ello, reemplace el siguiente código resaltado dentro de las etiquetas **&lt;li&gt;** : (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > otro enfoque sería vincular directamente a la página, con un código similar al siguiente:
   > 
   > &lt;a href =&quot;/Store/Browse? Genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Aunque este enfoque funciona, depende de una cadena codificada. Si posteriormente cambia el nombre del controlador, deberá cambiar esta instrucción manualmente. Una alternativa mejor es usar un método **auxiliar HTML** . ASP.NET MVC incluye un método auxiliar HTML que está disponible para estas tareas. El método auxiliar **HTML. ActionLink ()** facilita la compilación de HTML **&lt;un&gt;** vínculos, asegurándose de que las rutas de acceso de las direcciones URL están codificadas correctamente como dirección URL.
   > 
   > HTML. ActionLink tiene varias sobrecargas. En este ejercicio usará una que toma tres parámetros:
   > 
   > 1. Texto del vínculo, que mostrará el nombre del género
   > 2. Nombre de acción del controlador (**examinar**)
   > 3. Valores de parámetro de la ruta, que especifican el nombre (**género**) y el valor (**nombre de género**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tarea 9: ejecución de la aplicación

En esta tarea, probará que cada género se muestra con un vínculo a la dirección URL de **/Store/Browse** adecuada.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/Store** para comprobar que cada género se vincula a la dirección URL de **/Store/Browse** adecuada.

    ![Examinar géneros con vínculos a la página examinar](aspnet-mvc-4-fundamentals/_static/image33.png "Examinar géneros con vínculos a la página examinar")

    *Examinar géneros con vínculos a la página examinar*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tarea 10: usar la colección ViewModel dinámica para pasar valores

En esta tarea, aprenderá un método sencillo y eficaz para pasar valores entre el controlador y la vista sin realizar ningún cambio en el modelo. ASP.NET MVC 4 proporciona la colección &quot;ViewModel&quot;, que se puede asignar a cualquier valor dinámico y tener acceso también dentro de los controladores y las vistas.

Ahora usará la colección dinámica ViewBag para pasar una lista de &quot;**géneros destacan**&quot; desde el controlador a la vista. La vista de índice de almacén tendrá acceso a **ViewModel** y mostrará la información.

1. Cierre el explorador si es necesario para volver a la ventana de Visual Studio. Abra **StoreController.CS** y modifique el método **index** para crear una lista de géneros destacan en la colección ViewModel:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > También puede usar la sintaxis **ViewBag [&quot;destacan&quot;]** para tener acceso a las propiedades.
2. El icono de estrella **&quot;&quot;destacan. png** se incluye en la carpeta **Source\Assets\Images** de este laboratorio. Para agregarlo a la aplicación, arrastre su contenido desde una ventana del **Explorador de Windows** hasta el **Explorador de soluciones** en Visual Web Developer Express, como se muestra a continuación:

    ![Agregar una imagen de estrella a la solución](aspnet-mvc-4-fundamentals/_static/image34.png "Agregar una imagen de estrella a la solución")

    *Agregar una imagen de estrella a la solución*
3. Abra la vista **Store/index. cshtml** y modifique el contenido. Leerá la propiedad &quot;destacan&quot; de la colección **ViewBag** y le preguntará si el nombre de género actual está en la lista. En ese caso, mostrará un icono de estrella directamente al vínculo género.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tarea 11: ejecución de la aplicación

En esta tarea, probará que el género destacan muestra un icono de estrella.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página **principal** . Cambie la dirección URL a **/Store** para comprobar que los géneros destacados tienen la etiqueta de respeto:

    ![Examinar géneros con elementos destacan](aspnet-mvc-4-fundamentals/_static/image35.png "Examinar géneros con elementos destacan")

    *Examinar géneros con elementos destacan*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Ejercicio 7: una nueva plantilla de ASP.NET MVC 4

En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4 y examinará las características más relevantes de la nueva plantilla.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tarea 1: exploración de la plantilla de aplicación de Internet de ASP.NET MVC 4

1. Si aún no está abierto, inicie **vs Express para web**
2. Seleccione el **archivo | Nuevo |** Comando de menú proyecto. En el cuadro de diálogo **nuevo proyecto** , seleccione el **Visual C#| Plantilla Web** en el árbol del panel izquierdo y elija la **aplicación web MVC 4 ASP.net**. **Asigne** al proyecto el nombre *MusicStore* y actualice el nombre de la **solución** a *Inicio*, a continuación, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.

    ![Creación de un nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Creación de un nuevo proyecto de ASP.NET MVC 4")

    *Creación de un nuevo proyecto de ASP.NET MVC 4*
3. En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla de proyecto **aplicación de Internet** y haga clic en **Aceptar**. Tenga en cuenta que puede seleccionar Razor o ASPX como motor de vista.

    ![Creación de una nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Creación de una nueva aplicación de Internet de ASP.NET MVC 4")

    *Creación de una nueva aplicación de Internet de ASP.NET MVC 4*

    > [!NOTE]
    > Sintaxis Razor se ha introducido en ASP.NET MVC 3. Su objetivo es minimizar el número de caracteres y pulsaciones de teclas necesarios en un archivo, lo que permite un flujo de trabajo de codificación rápida y fluida. Razor aprovecha los conocimientos C#existentes del lenguaje/VB (u otros) y proporciona una sintaxis de marcado de plantilla que permite un flujo de trabajo de construcción de HTML impresionante.
4. Presione **F5** para ejecutar la solución y ver la plantilla renovada. Puede consultar las siguientes características:

    1. **Plantillas de estilo moderno**

        Las plantillas se han renovado, lo que proporciona más estilos de aspecto moderno.

        ![Plantillas con estilo de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image38.png "Plantillas con estilo de ASP.NET MVC 4")

        *Plantillas con estilo de ASP.NET MVC 4*
    2. **Representación adaptable**

        Desproteja el cambio de tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al nuevo tamaño de la ventana. Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.

        ![Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador](aspnet-mvc-4-fundamentals/_static/image39.png "Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador")

        *Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador*
5. Cierre el explorador para detener el depurador y volver a Visual Studio.
6. Ahora puede explorar la solución y consultar algunas de las nuevas características introducidas por ASP.NET MVC 4 en la plantilla de proyecto.

    ![ASP.NET MVC4-Internet-Application-Project-Template](aspnet-mvc-4-fundamentals/_static/image40.png "La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")

    *La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*

   1. **Marcado HTML5**

       Examine las vistas de plantilla para averiguar el nuevo marcado del tema; por ejemplo, abra la vista **About. cshtml** dentro de la carpeta **principal** .

       ![Nueva plantilla, con Razor y marcado HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Nueva plantilla, con Razor y marcado HTML5")

       *Nueva plantilla, con Razor y marcado HTML5*
   2. **Bibliotecas de JavaScript incluidas**

      1. **jQuery**: jQuery simplifica el recorrido de documentos HTML, el control de eventos, la animación y las interacciones de Ajax.
      2. **jQuery UI**: esta biblioteca proporciona abstracciones para la interacción y animación de bajo nivel, efectos avanzados y widgets temáticos, que se basan en la biblioteca de JavaScript de jQuery.

         > [!NOTE]
         > Puede obtener información sobre jQuery y jQuery UI en [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).
      3. **KnockoutJS**: la plantilla predeterminada de ASP.NET MVC 4 ahora incluye **KnockoutJS**, un marco de trabajo de JavaScript MVVM que le permite crear aplicaciones web enriquecidas y con una gran capacidad de respuesta mediante JavaScript y HTML. Al igual que en ASP.NET MVC 3, las bibliotecas de interfaz de usuario de jQuery y jQuery también se incluyen en ASP.NET MVC 4.

          > [!NOTE]
          > Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).
      4. **Modernizr**: esta biblioteca se ejecuta automáticamente, lo que hace que el sitio sea compatible con los exploradores más antiguos cuando se usan las tecnologías HTML5 y CSS3.

          > [!NOTE]
          > Puede obtener más información acerca de la biblioteca Modernizr en este vínculo: [http://www.modernizr.com/](http://www.modernizr.com/).
   3. **SimpleMembership incluido en la solución**

       SimpleMembership se ha diseñado como un sustituto para el rol de ASP.NET anterior y el sistema de proveedor de pertenencia. Tiene muchas características nuevas que facilitan a los desarrolladores la protección de las páginas web de una manera más flexible.

       La plantilla de Internet ya ha configurado algunas cosas para integrar SimpleMembership, por ejemplo, AccountController está preparado para usar OAuthWebSecurity (para el registro de la cuenta OAuth, el inicio de sesión, la administración, etc.) y la seguridad Web.

       ![SimpleMembership incluido en la solución](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluido en la solución")

       *SimpleMembership incluido en la solución*

       > [!NOTE]
       > Obtenga más información sobre [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) en MSDN.

> [!NOTE]
> Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido los aspectos básicos de ASP.NET MVC:

- Los elementos básicos de una aplicación MVC y cómo interactúan
- Cómo crear una aplicación ASP.NET MVC
- Cómo agregar y configurar Controladores para controlar los parámetros que se pasan a través de la dirección URL y QueryString
- Cómo agregar una página maestra de diseño para configurar una plantilla para contenido HTML común, una hoja de estilos para mejorar la apariencia y el funcionamiento y una plantilla de vista para mostrar contenido HTML
- Cómo usar el patrón ViewModel para pasar propiedades a la plantilla de vista para mostrar información dinámica
- Cómo usar los parámetros pasados a los controladores en la plantilla de vista
- Cómo agregar vínculos a páginas dentro de la aplicación ASP.NET MVC
- Cómo agregar y usar propiedades dinámicas en una vista
- Mejoras en las plantillas de proyecto de ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](aspnet-mvc-4-fundamentals/_static/image47.png)

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

    ![Iniciar sesión en Windows Azure Portal](aspnet-mvc-4-fundamentals/_static/image48.png "Iniciar sesión en Windows Azure Portal")

    *Iniciar sesión en Windows Azure Portal de administración*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](aspnet-mvc-4-fundamentals/_static/image49.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](aspnet-mvc-4-fundamentals/_static/image50.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](aspnet-mvc-4-fundamentals/_static/image51.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](aspnet-mvc-4-fundamentals/_static/image52.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](aspnet-mvc-4-fundamentals/_static/image53.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.

    ![Descargar el perfil de publicación del sitio web](aspnet-mvc-4-fundamentals/_static/image54.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](aspnet-mvc-4-fundamentals/_static/image55.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](aspnet-mvc-4-fundamentals/_static/image56.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](aspnet-mvc-4-fundamentals/_static/image57.png).

    ![Agregando la dirección IP del cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-fundamentals/_static/image60.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-fundamentals/_static/image61.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](aspnet-mvc-4-fundamentals/_static/image62.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](aspnet-mvc-4-fundamentals/_static/image63.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](aspnet-mvc-4-fundamentals/_static/image64.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](aspnet-mvc-4-fundamentals/_static/image65.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

    ![Aplicación publicada en Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Aplicación publicada en Windows Azure")

    *Aplicación publicada en Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-fundamentals/_static/image69.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-fundamentals/_static/image70.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-fundamentals/_static/image71.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-fundamentals/_static/image72.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-fundamentals/_static/image73.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-fundamentals/_static/image74.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
