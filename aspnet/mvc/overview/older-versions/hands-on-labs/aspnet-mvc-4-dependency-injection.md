---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
title: Inyección de dependencia de MVC 4 de ASP.NET | Microsoft Docs
author: rick-anderson
description: 'Nota: en este laboratorio práctico se supone que tiene conocimientos básicos de los filtros de ASP.NET MVC y ASP.NET MVC 4. Si no ha usado los filtros de ASP.NET MVC 4 antes, Rec...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 84c7baca-1c54-4c44-8f52-4282122d6acb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 15c9d4dcb9e2c6b9f6adf54d65d15737b32cca3b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451825"
---
# <a name="aspnet-mvc-4-dependency-injection"></a>Inserción de dependencias de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico supone que tiene conocimientos básicos de los filtros de **ASP.NET MVC** y **ASP.NET MVC 4**. Si no ha usado los **filtros de ASP.NET MVC 4** antes, le recomendamos que consulte el laboratorio práctico de los **filtros de acción personalizada de MVC de ASP.net** .

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en [ASP.NET MVC 4 Dependency inserciones](https://github.com/Microsoft-Web/HOL-MVC4DependencyInjection).

En el paradigma de **programación orientada a objetos** , los objetos funcionan juntos en un modelo de colaboración en el que hay colaboradores y consumidores. Naturalmente, este modelo de comunicación genera dependencias entre objetos y componentes, lo que resulta difícil de administrar cuando aumenta la complejidad.

![Dependencias de clase y complejidad del modelo](aspnet-mvc-4-dependency-injection/_static/image1.png "Dependencias de clase y complejidad del modelo")

*Dependencias de clase y complejidad del modelo*

Probablemente ha oído hablar sobre el **patrón de fábrica** y la separación entre la interfaz y la implementación mediante servicios, donde los objetos de cliente suelen ser responsables de la ubicación del servicio.

El patrón de inserción de dependencias es una implementación concreta de la inversión de control. La **inversión de control (IOC)** significa que los objetos no crean otros objetos en los que se basan en su trabajo. En su lugar, obtienen los objetos que necesitan de un origen externo (por ejemplo, un archivo de configuración XML).

La **inserción de dependencias (di)** significa que esto se hace sin la intervención del objeto, normalmente por parte de un componente de marco que pasa los parámetros de constructor y las propiedades de conjunto.

<a id="The_Dependency_Injection_DI_Design_Pattern"></a>
### <a name="the-dependency-injection-di-design-pattern"></a>Modelo de diseño de inserción de dependencias (DI)

En un nivel alto, el objetivo de la inserción de dependencias es que una clase de cliente (por ejemplo, *el jugador*) necesita algo que satisfaga una interfaz (por ejemplo, *iClub*). No le importa cuál sea el tipo concreto (por ejemplo *, WoodClub, IronClub, WedgeClub* o *PutterClub*), sino que otra persona pueda controlarlo (por ejemplo, un buen *Caddy*). La resolución de dependencias en ASP.NET MVC puede permitirle registrar la lógica de dependencia en otra parte (por ejemplo, un contenedor o una *bolsa de tréboles*).

![Diagrama de inyección de dependencia](aspnet-mvc-4-dependency-injection/_static/image2.png "Ilustración de inyección de dependencia")

*Inserción de dependencias: analogía de golf*

Las ventajas de usar el patrón de inserción de dependencias y la inversión de control son las siguientes:

- Reduce el acoplamiento de clases
- Aumenta la reutilización de código
- Mejora el mantenimiento del código
- Mejora las pruebas de aplicaciones

> [!NOTE]
> La inserción de dependencias a veces se compara con el patrón de diseño de fábrica abstracta, pero existe una pequeña diferencia entre ambos enfoques. DI tiene un marco de trabajo en el que se resuelven las dependencias mediante una llamada a los generadores y los servicios registrados.

Ahora que comprende el patrón de inserción de dependencias, aprenderá a lo largo de este laboratorio cómo aplicarlo en ASP.NET MVC 4. Empezará a usar la inserción de dependencias en los **Controladores** para incluir un servicio de acceso a bases de datos. A continuación, aplicará la inserción de dependencias a las **vistas** para consumir un servicio y Mostrar información. Por último, extenderá el DI a ASP.NET MVC 4 filters e insertará un filtro de acción personalizado en la solución.

En este laboratorio práctico, aprenderá a:

- Integración de ASP.NET MVC 4 con Unity para la inserción de dependencias mediante paquetes NuGet
- Usar la inserción de dependencias dentro de un controlador ASP.NET MVC
- Usar la inserción de dependencias dentro de una vista de MVC de ASP.NET
- Usar la inserción de dependencias dentro de un filtro de acción de MVC de ASP.NET

> [!NOTE]
> En este laboratorio se usa el paquete de NuGet Unity. Mvc3 para la resolución de dependencias, pero es posible adaptar cualquier marco de inserción de dependencias para que funcione con ASP.NET MVC 4.

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

Este laboratorio práctico se compone de los siguientes ejercicios:

1. [Ejercicio 1: inyección de un controlador](#Exercise1)
2. [Ejercicio 2: inserción de una vista](#Exercise2)
3. [Ejercicio 3: inserción de filtros](#Exercise3)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Injecting_a_Controller"></a>
### <a name="exercise-1-injecting-a-controller"></a>Ejercicio 1: inyección de un controlador

En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en los controladores de ASP.NET MVC mediante la integración de Unity con un paquete de NuGet. Por ese motivo, incluirá servicios en los controladores de MvcMusicStore para separar la lógica del acceso a datos. Los servicios crearán una nueva dependencia en el constructor del controlador, que se resolverá mediante la inserción de dependencias con la ayuda de **Unity**.

Este enfoque le mostrará cómo generar aplicaciones menos acopladas, que son más flexibles y fáciles de mantener y probar. También aprenderá a integrar ASP.NET MVC con Unity.

<a id="About_StoreManager_Service"></a>
#### <a name="about-storemanager-service"></a>Acerca del servicio StoreManager

El almacén de música de MVC proporcionado en la solución de inicio ahora incluye un servicio que administra los datos del controlador de almacenamiento denominados **StoreService**. A continuación encontrará la implementación del servicio de tienda. Tenga en cuenta que todos los métodos devuelven entidades del modelo.

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample1.cs)]

**StoreController** de la solución Begin ahora consume **StoreService**. Todas las referencias de datos se han quitado de **StoreController**y ahora es posible modificar el proveedor de acceso a datos actual sin cambiar ningún método que consuma **StoreService**.

Encontrará más abajo que la implementación de **StoreController** tiene una dependencia con **StoreService** dentro del constructor de clase.

> [!NOTE]
> La dependencia introducida en este ejercicio está relacionada con la **inversión de control** (IOC).
> 
> El constructor de la clase **StoreController** recibe un parámetro de tipo **IStoreService** , que es esencial para realizar llamadas de servicio desde dentro de la clase. Sin embargo, **StoreController** no implementa el constructor predeterminado (sin parámetros) que cualquier controlador debe tener para trabajar con ASP.NET MVC.
> 
> Para resolver la dependencia, el controlador debe crearse mediante un generador abstracto (una clase que devuelve cualquier objeto del tipo especificado).

[!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample2.cs)]

> [!NOTE]
> Obtendrá un error cuando la clase intente crear el StoreController sin enviar el objeto de servicio, ya que no se ha declarado ningún constructor sin parámetros.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Running_the_Application"></a>
#### <a name="task-1---running-the-application"></a>Tarea 1: ejecución de la aplicación

En esta tarea, ejecutará la aplicación de inicio, que incluye el servicio en el controlador de almacenamiento que separa el acceso a los datos de la lógica de la aplicación.

Al ejecutar la aplicación, recibirá una excepción, ya que el servicio del controlador no se pasa como parámetro de forma predeterminada:

1. Abra la solución de **Inicio** que se encuentra en **Source\Ex01-injecting Controller\Begin**.

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Presione **Ctrl + F5** para ejecutar la aplicación sin depuración. Obtendrá el mensaje de error &quot;**no hay ningún constructor sin parámetros definido para este objeto**&quot;:

    ![Error al ejecutar la aplicación de inicio de ASP.NET MVC](aspnet-mvc-4-dependency-injection/_static/image3.png "Error al ejecutar la aplicación de inicio de ASP.NET MVC")

    *Error al ejecutar la aplicación de inicio de ASP.NET MVC*
3. Cierre el explorador.

En los pasos siguientes, trabajará en la solución Music Store para insertar la dependencia que este controlador necesita.

<a id="Ex1Task2"></a>

<a id="Task_2_-_Including_Unity_into_MvcMusicStore_Solution"></a>
#### <a name="task-2---including-unity-into-mvcmusicstore-solution"></a>Tarea 2: inclusión de Unity en la solución MvcMusicStore

En esta tarea, incluirá el paquete de NuGet **Unity. Mvc3** en la solución.

> [!NOTE]
> El paquete Unity. Mvc3 se diseñó para ASP.NET MVC 3, pero es totalmente compatible con ASP.NET MVC 4.
> 
> Unity es un contenedor de inserción de dependencias ligera y extensible con compatibilidad opcional para la intercepción de instancias y tipos. Es un contenedor de uso general para su uso en cualquier tipo de aplicación .NET. Proporciona todas las características comunes que se encuentran en los mecanismos de inserción de dependencias, entre las que se incluyen la creación de objetos, la abstracción de los requisitos mediante la especificación de dependencias en tiempo de ejecución y flexibilidad, al aplazar la configuración del componente en el contenedor.

1. Instale el paquete de NuGet de **Unity. Mvc3** en el proyecto **MvcMusicStore** . Para ello, abra la **consola del administrador de paquetes** en **Ver** | **otras ventanas**.
2. Ejecute el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-dependency-injection/samples/sample3.ps1)]

    ![Instalación del paquete de NuGet de Unity. Mvc3](aspnet-mvc-4-dependency-injection/_static/image4.png "Instalación del paquete de NuGet de Unity. Mvc3")

    *Instalación del paquete de NuGet de Unity. Mvc3*
3. Una vez instalado el paquete **Unity. Mvc3** , explore los archivos y las carpetas que agrega automáticamente para simplificar la configuración de Unity.

    ![Paquete Unity. Mvc3 instalado](aspnet-mvc-4-dependency-injection/_static/image5.png "Paquete Unity. Mvc3 instalado")

    *Paquete Unity. Mvc3 instalado*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Registering_Unity_in_Globalasaxcs_Application_Start"></a>
#### <a name="task-3---registering-unity-in-globalasaxcs-application_start"></a>Tarea 3: registro de Unity en la aplicación Global.asax.cs Inicio\_

En esta tarea, actualizará la **aplicación\_método Start** ubicado en **global.asax.CS** para llamar al inicializador de arranque de Unity y, a continuación, actualizar el archivo de programa previo que registra el servicio y el controlador que usará para la inserción de dependencias.

1. Ahora, enlazará el programa previo, que es el archivo que inicializa el contenedor de Unity y la resolución de dependencias. Para ello, Abra **global.asax.CS** y agregue el siguiente código resaltado en el método de inicio de la **aplicación\_** .

    (Fragmento de código- *ASP.net de inserción de dependencias-EX01-Initialize Unity*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample4.cs)]
2. Abra el archivo **Bootstrapper.CS** .
3. Incluya los siguientes espacios de nombres: **MvcMusicStore. Services** y **MusicStore. Controllers**.

    (Fragmento de código- *ASP.net inserciones de dependencia Lab-EX01-Bootstrapper agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample5.cs)]
4. Reemplace el contenido del método **BuildUnityContainer** por el código siguiente que registra el controlador de almacén y el servicio de almacén.

    (Fragmento de código- *ASP.net de inserción de dependencias-EX01-registrar controlador y servicio de almacén*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample6.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, ejecutará la aplicación para comprobar que ahora se puede cargar después de incluir Unity.

1. Presione **F5** para ejecutar la aplicación, la aplicación debe cargarse ahora sin mostrar ningún mensaje de error.

    ![Ejecución de la aplicación con la inserción de dependencias](aspnet-mvc-4-dependency-injection/_static/image6.png "Ejecución de la aplicación con la inserción de dependencias")

    *Ejecución de la aplicación con la inserción de dependencias*
2. Vaya a **/Store**. Esto invocará a **StoreController**, que ahora se crea con **Unity**.

    ![Almacén de música de MVC](aspnet-mvc-4-dependency-injection/_static/image7.png "Almacén de música de MVC")

    *Almacén de música de MVC*
3. Cierre el explorador.

En los siguientes ejercicios aprenderá a ampliar el ámbito de inserción de dependencias para usarlo dentro de las vistas y los filtros de acción de ASP.NET MVC.

<a id="Exercise2"></a>

<a id="Exercise_2_Injecting_a_View"></a>
### <a name="exercise-2-injecting-a-view"></a>Ejercicio 2: inserción de una vista

En este ejercicio, obtendrá información sobre cómo usar la inserción de dependencias en una vista con las nuevas características de ASP.NET MVC 4 para la integración de Unity. Para ello, llamará a un servicio personalizado dentro de la vista examinar del almacén, que mostrará un mensaje y una imagen a continuación.

A continuación, integrará el proyecto con Unity y creará una resolución de dependencia personalizada para insertar las dependencias.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Creating_a_View_that_Consumes_a_Service"></a>
#### <a name="task-1---creating-a-view-that-consumes-a-service"></a>Tarea 1: creación de una vista que consume un servicio

En esta tarea, creará una vista que realiza una llamada de servicio para generar una nueva dependencia. El servicio está en un servicio de mensajería simple incluido en esta solución.

1. Abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex02-injecting View\Begin** De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluya las clases **MessageService.CS** y **IMessageService.CS** que se encuentran en la carpeta **source \Assets** en **/Services**. Para ello, haga clic con el botón derecho en la carpeta **servicios** y seleccione **Agregar elemento existente**. Vaya a la ubicación de los archivos e inclúyalo.

    ![Adición del servicio de mensajes y la interfaz de servicio](aspnet-mvc-4-dependency-injection/_static/image8.png "Adición del servicio de mensajes y la interfaz de servicio")

    *Adición del servicio de mensajes y la interfaz de servicio*

    > [!NOTE]
    > La interfaz **IMessageService** define dos propiedades implementadas por la clase **MessageService** . Estas propiedades-**Message** y **ImageUrl**: almacenan el mensaje y la dirección URL de la imagen que se va a mostrar.
3. Cree la carpeta **/pages** en la carpeta raíz del proyecto y, a continuación, agregue la clase existente **MyBasePage.CS** desde **Source\Assets**. La página base de la que se va a heredar tiene la estructura siguiente.

    ![Carpeta de páginas](aspnet-mvc-4-dependency-injection/_static/image9.png "Carpeta Pages")

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample7.cs)]
4. Abra la vista **examinar. cshtml** de la carpeta **/views/Store** y haga que se herede de **MyBasePage.CS**.

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample8.cshtml)]
5. En la vista **examinar** , agregue una llamada a **MessageService** para mostrar una imagen y un mensaje recuperado por el servicio.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-dependency-injection/samples/sample9.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Including_a_Custom_Dependency_Resolver_and_a_Custom_View_Page_Activator"></a>
#### <a name="task-2---including-a-custom-dependency-resolver-and-a-custom-view-page-activator"></a>Tarea 2: incluir un solucionador de dependencias personalizado y un activador de página de vista personalizada

En la tarea anterior, insertó una nueva dependencia dentro de una vista para realizar una llamada de servicio dentro de ella. Ahora, resolverá esa dependencia implementando las interfaces de inyección de dependencia de ASP.NET MVC **IViewPageActivator** y **objeto idependencyresolver**. Incluirá en la solución una implementación de **objeto idependencyresolver** que se ocupará de la recuperación del servicio mediante Unity. A continuación, incluirá otra implementación personalizada de la interfaz **IViewPageActivator** que resolverá la creación de las vistas.

> [!NOTE]
> Dado que ASP.NET MVC 3, la implementación de la inserción de dependencias ha simplificado las interfaces para registrar los servicios. **Objeto idependencyresolver** y **IViewPageActivator** forman parte de las características de ASP.NET MVC 3 para la inserción de dependencias.
> 
> **-** La interfaz objeto idependencyresolver reemplaza el IMvcServiceLocator anterior. Los implementadores de objeto idependencyresolver deben devolver una instancia del servicio o una colección de servicios.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample10.cs)]
> 
> **-La interfaz IViewPageActivator** proporciona un control más preciso sobre cómo se crean instancias de las páginas de vista a través de la inserción de dependencias. Las clases que implementan la interfaz **IViewPageActivator** pueden crear instancias de vista mediante la información de contexto.
> 
> 
> [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample11.cs)]

1. Cree la carpeta/**factorías** en la carpeta raíz del proyecto.
2. Incluya **CustomViewPageActivator.CS** en la solución de **/sources/assets/** en la carpeta **factorías** . Para ello, haga clic con el botón derecho en la carpeta **/factories** , seleccione **Agregar | Elemento existente** y, a continuación, seleccione **CustomViewPageActivator.CS**. Esta clase implementa la interfaz **IViewPageActivator** para contener el contenedor de Unity.

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample12.cs)]

    > [!NOTE]
    > **CustomViewPageActivator** es responsable de administrar la creación de una vista mediante el uso de un contenedor de Unity.
3. Incluya el archivo **UnityDependencyResolver.CS** de **/sources/assets** en la carpeta **/factories** Para ello, haga clic con el botón derecho en la carpeta **/factories** , seleccione **Agregar | Elemento existente** y, a continuación, seleccione el archivo **UnityDependencyResolver.CS** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample13.cs)]

    > [!NOTE]
    > La clase **UnityDependencyResolver** es un DependencyResolver personalizado para Unity. Cuando no se puede encontrar un servicio dentro del contenedor de Unity, el solucionador base es invocated.

En la tarea siguiente, ambas implementaciones se registrarán para permitir que el modelo Conozca la ubicación de los servicios y las vistas.

<a id="Ex2Task3"></a>

<a id="Task_3_-_Registering_for_Dependency_Injection_within_Unity_container"></a>
#### <a name="task-3---registering-for-dependency-injection-within-unity-container"></a>Tarea 3: registro de la inserción de dependencias en el contenedor de Unity

En esta tarea, colocará todos los elementos anteriores juntos para que la inserción de dependencias funcione.

Hasta ahora la solución tiene los siguientes elementos:

- Una vista de **exploración** que hereda de **MyBaseClass** y consume **MessageService**.
- Clase intermedia-**MyBaseClass**: que tiene la inserción de dependencias declarada para la interfaz de servicio.
- Un servicio- **MessageService** -y su interfaz **IMessageService**.
- Una resolución de dependencia personalizada para Unity- **UnityDependencyResolver** , que se ocupa de la recuperación del servicio.
- Una página de vista Activator- **CustomViewPageActivator** : que crea la página.

Para insertar la vista **examinar** , ahora registrará la resolución de dependencia personalizada en el contenedor de Unity.

1. Abra el archivo **Bootstrapper.CS** .
2. Registre una instancia de **MessageService** en el contenedor de Unity para inicializar el servicio:

    (Fragmento de código- *ASP.net de inserción de dependencias-Ex02-registrar servicio de mensajes*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample14.cs)]
3. Agregue una referencia al espacio de nombres **MvcMusicStore. factorías** .

    (Fragmento de código- *ASP.net de inserción de dependencias Lab-Ex02-factorías espacio de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample15.cs)]
4. Registre **CustomViewPageActivator** como un activador de página de vista en el contenedor de Unity:

    (Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex02-Register CustomViewPageActivator*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample16.cs)]
5. Reemplace la resolución de dependencia predeterminada de ASP.NET MVC 4 por una instancia de **UnityDependencyResolver**. Para ello, reemplace el contenido del método **Initialize** por el código siguiente:

    (Fragmento de código- *ASP.net de inserción de dependencias laboratorio-Ex02-solucionador de dependencias de actualización*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample17.cs)]

    > [!NOTE]
    > ASP.NET MVC proporciona una clase de solucionador de dependencias predeterminada. Para trabajar con resoluciones de dependencia personalizadas como la que hemos creado para Unity, este solucionador debe reemplazarse.

<a id="Ex2Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, ejecutará la aplicación para comprobar que el explorador de la tienda consume el servicio y muestra la imagen y el mensaje recuperado:

1. Presione **F5** para ejecutar la aplicación.
2. Haga clic en **Rock** en el menú géneros y vea cómo se insertó **MessageService** en la vista y cargó el mensaje de bienvenida y la imagen. En este ejemplo, vamos a entrar en &quot;**Rock**&quot;:

    ![Almacén de música de MVC: ver inyección](aspnet-mvc-4-dependency-injection/_static/image10.png "Almacén de música de MVC: ver inyección")

    *Almacén de música de MVC: ver inyección*
3. Cierre el explorador.

<a id="Exercise3"></a>

<a id="Exercise_3_Injecting_Action_Filters"></a>
### <a name="exercise-3-injecting-action-filters"></a>Ejercicio 3: inserción de filtros de acción

En los **filtros de acción personalizada** del laboratorio práctico anterior, ha trabajado con la personalización de filtros y la inserción. En este ejercicio, obtendrá información sobre cómo insertar filtros con la inserción de dependencias mediante el contenedor Unity. Para ello, agregará a la solución Music Store un filtro de acción personalizado que realizará el seguimiento de la actividad del sitio.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Including_the_Tracking_Filter_in_the_Solution"></a>
#### <a name="task-1---including-the-tracking-filter-in-the-solution"></a>Tarea 1: incluir el filtro de seguimiento en la solución

En esta tarea, incluirá en la tienda de música un filtro de acción personalizado para realizar un seguimiento de los eventos. Como los conceptos de filtros de acción personalizados ya se tratan en el laboratorio anterior &quot;filtros de acción personalizados&quot;, solo se incluye la clase de filtro de la carpeta activos de este laboratorio y, a continuación, se crea un proveedor de filtro para Unity:

1. Abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex03-inyectando acción Filter\Begin** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
      > 
      > Para obtener más información, consulte este artículo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).
2. Incluya el archivo **TraceActionFilter.CS** de **/sources/assets** en la carpeta **/Filters**

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample18.cs)]

    > [!NOTE]
    > Este filtro de acción personalizada realiza el seguimiento de ASP.NET. Puede comprobar &quot;los filtros de acciones locales y dinámicas de ASP.NET MVC 4&quot; laboratorio para obtener más referencia.
3. Agregue la clase vacía **FilterProvider.CS** al proyecto en la carpeta **/Filters.**
4. Agregue los espacios de nombres **System. Web. Mvc** y **Microsoft. Practices. Unity** en **FilterProvider.CS**.

    (Fragmento de código- *ASP.net de inserción de dependencias-Ex03-filtro agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample19.cs)]
5. Haga que la clase herede de la interfaz **IFilterProvider** .

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample20.cs)]
6. Agregue una propiedad **IUnityContainer** en la clase **FilterProvider** y, a continuación, cree un constructor de clase para asignar el contenedor.

    (Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex03-Filter Provider constructor*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample21.cs)]

    > [!NOTE]
    > El constructor de clase de proveedor de filtros no está creando un **nuevo** objeto dentro de. El contenedor se pasa como parámetro y Unity resuelve la dependencia.
7. En la clase **FilterProvider** , implemente el método **GetFilters** desde la interfaz **IFilterProvider** .

    (Fragmento de código- *ASP.net de inserción de dependencias-Ex03-proveedor de filtros GetFilters*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample22.cs)]

<a id="Ex3Task2"></a>

<a id="Task_2_-_Registering_and_Enabling_the_Filter"></a>
#### <a name="task-2---registering-and-enabling-the-filter"></a>Tarea 2: registro y habilitación del filtro

En esta tarea, habilitará el seguimiento del sitio. Para ello, registrará el filtro en el método **Bootstrapper.CS BuildUnityContainer** para iniciar el seguimiento:

1. Abra el **archivo Web. config** que se encuentra en la raíz del proyecto y habilite seguimiento de seguimiento en el grupo System. Web.

    [!code-xml[Main](aspnet-mvc-4-dependency-injection/samples/sample23.xml)]
2. Abra **Bootstrapper.CS** en la raíz del proyecto.
3. Agregue una referencia al espacio de nombres **MvcMusicStore. filters** .

    (Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex03-Bootstrapper agregar espacios de nombres*)

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample24.cs)]
4. Seleccione el método **BuildUnityContainer** y registre el filtro en el contenedor de Unity. Tendrá que registrar el proveedor de filtro, así como el filtro de acción.

    (Fragmento de código- *ASP.net inserciones de dependencia Lab-Ex03-Register FilterProvider y actionfilter (* )

    [!code-csharp[Main](aspnet-mvc-4-dependency-injection/samples/sample25.cs)]

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecución de la aplicación

En esta tarea, ejecutará la aplicación y comprobará que el filtro de acción personalizada está realizando el seguimiento de la actividad:

1. Presione **F5** para ejecutar la aplicación.
2. Haga clic en **Rock** en el menú géneros. Si lo desea, puede ir a más géneros.

    ![Tienda de música](aspnet-mvc-4-dependency-injection/_static/image11.png "Tienda de música")

    *Tienda de música*
3. Vaya a **/Trace.axd** para ver la página seguimiento de la aplicación y, a continuación, haga clic en **Ver detalles**.

    ![Registro de seguimiento de la aplicación](aspnet-mvc-4-dependency-injection/_static/image12.png "Registro de seguimiento de la aplicación")

    *Registro de seguimiento de la aplicación*

    ![Seguimiento de la aplicación-detalles de la solicitud](aspnet-mvc-4-dependency-injection/_static/image13.png "Seguimiento de la aplicación-detalles de la solicitud")

    *Seguimiento de la aplicación-detalles de la solicitud*
4. Cierre el explorador.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido a usar la inserción de dependencias en ASP.NET MVC 4 mediante la integración de Unity con un paquete de NuGet. Para lograrlo, ha usado la inserción de dependencias en los controladores, las vistas y los filtros de acción.

Se trataron los siguientes conceptos:

- Características de inserción de dependencias de MVC 4 de ASP.NET
- Integración de Unity mediante el paquete NuGet de Unity. Mvc3
- Inserción de dependencias en controladores
- Inserción de dependencias en vistas
- Inserción de dependencias de filtros de acción

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](aspnet-mvc-4-dependency-injection/_static/image14.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](aspnet-mvc-4-dependency-injection/_static/image15.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-dependency-injection/_static/image16.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](aspnet-mvc-4-dependency-injection/_static/image17.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](aspnet-mvc-4-dependency-injection/_static/image18.png)

    *VS Express para Web icono*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-dependency-injection/_static/image19.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-dependency-injection/_static/image20.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-dependency-injection/_static/image21.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-dependency-injection/_static/image22.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-dependency-injection/_static/image23.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-dependency-injection/_static/image24.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
