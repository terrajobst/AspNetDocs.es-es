---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: ASP.NET los modelos y el acceso a los datos de MVC 4 | Microsoft Docs
author: rick-anderson
description: 'Nota: en este laboratorio práctico se supone que tiene conocimientos básicos de ASP.NET MVC. Si no ha usado ASP.NET MVC antes, le recomendamos que pase a ASP.NET MVC 4...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 90635b617930d0a9c126795f4c8790d542e33dc9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451471"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Acceso a datos y modelos de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico supone que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que consulte el laboratorio práctico de **conceptos básicos de ASP.NET MVC 4** .

Este laboratorio le guía a través de las mejoras y las nuevas características descritas anteriormente mediante la aplicación de cambios menores en una aplicación Web de ejemplo que se proporciona en la carpeta de origen.

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en los [modelos y el acceso a datos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

En el laboratorio práctico de **conceptos básicos de ASP.NET MVC** , ha estado pasando datos codificados de forma rígida desde los controladores a las plantillas de vista. Sin embargo, para compilar una aplicación web real, puede que desee usar una base de datos real.

Este laboratorio práctico le mostrará cómo usar un motor de base de datos para almacenar y recuperar los datos necesarios para la aplicación Music Store. Para ello, comenzará con una base de datos existente y creará la Entity Data Model a partir de ella. A lo largo de este laboratorio, cumplirá el enfoque **Database First** , así como el enfoque **code First** .

Sin embargo, también puede usar el enfoque de **Model First** , crear el mismo modelo con las herramientas de y, a continuación, generar la base de datos a partir de él.

![Database First frente a Model First](aspnet-mvc-4-models-and-data-access/_static/image1.png "Database First frente a Model First")

*Database First frente a Model First*

Después de generar el modelo, realizará los ajustes adecuados en el StoreController para proporcionar las vistas de almacén con los datos tomados de la base de datos, en lugar de usar datos codificados de forma rígida. No será necesario realizar ningún cambio en las plantillas de vista porque el StoreController devolverá el mismo ViewModels a las plantillas de vista, aunque esta vez los datos procederán de la base de datos.

**El enfoque Code First**

El enfoque Code First nos permite definir el modelo a partir del código sin generar clases que generalmente están acopladas al marco.

En Code First, los objetos de modelo se definen con POCO, &quot;objetos CLR antiguos sin formato&quot;. Los POCO son clases sin formato simples que no tienen herencia y no implementan interfaces. Podemos generar automáticamente la base de datos a partir de ellas, o bien usar una base de datos existente y generar la asignación de clase a partir del código.

Las ventajas de usar este enfoque es que el modelo sigue siendo independiente del marco de persistencia (en este caso, Entity Framework), ya que las clases POCO no se acoplan con el marco de asignación.

> [!NOTE]
> Este laboratorio se basa en ASP.NET MVC 4 y en una versión de la aplicación de ejemplo de Music Store personalizada y minimizada para ajustarse solo a las características que se muestran en este laboratorio práctico.
> 
> Si desea explorar toda la aplicación de tutorial de **Music Store** , puede encontrarla en [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

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

1. [Ejercicio 1: agregar una base de datos](#Exercise1)
2. [Ejercicio 2: crear una base de datos con Code First](#Exercise2)
3. [Ejercicio 3: consultar la base de datos con parámetros](#Exercise3)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **35 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Ejercicio 1: agregar una base de datos

En este ejercicio, obtendrá información sobre cómo agregar una base de datos con las tablas de la aplicación MusicStore a la solución con el fin de consumir sus datos. Una vez que la base de datos se genera con el modelo y se agrega a la solución, modificará la clase StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados de forma rígida.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Tarea 1: agregar una base de datos

En esta tarea, agregará una base de datos ya creada con las tablas principales de la aplicación MusicStore a la solución.

1. Abra la carpeta **Begin** Solution ubicada en **source/EX1-AddingADatabaseDBFirst/Begin/** .

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Agregue el archivo de base de datos **MvcMusicStore** . En este laboratorio práctico, usará una base de datos ya creada denominada **MvcMusicStore. MDF**. Para ello, haga clic con el botón derecho en **App\_** carpeta de datos, seleccione **Agregar** y, a continuación, haga clic en **elemento existente**. Vaya a **\Source\Assets** y seleccione el archivo **MvcMusicStore. MDF** .

    ![Agregar un elemento existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "Agregar un elemento existente")

    *Agregar un elemento existente*

    ![Archivo de base de datos MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image3.png "Archivo de base de datos MvcMusicStore. MDF")

    *Archivo de base de datos MvcMusicStore. MDF*

    La base de datos se ha agregado al proyecto. Incluso cuando la base de datos se encuentra dentro de la solución, puede consultarla y actualizarla tal y como se hospedaba en un servidor de base de datos diferente.

    ![Base de datos MvcMusicStore en Explorador de soluciones](aspnet-mvc-4-models-and-data-access/_static/image4.png "Base de datos MvcMusicStore en Explorador de soluciones")

    *Base de datos MvcMusicStore en Explorador de soluciones*
3. Compruebe la conexión a la base de datos. Para ello, haga doble clic en **MvcMusicStore. MDF** para establecer una conexión.

    ![Conexión a MvcMusicStore. MDF](aspnet-mvc-4-models-and-data-access/_static/image5.png "Conexión a MvcMusicStore. MDF")

    *Conexión a MvcMusicStore. MDF*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Tarea 2: crear un modelo de datos

En esta tarea, creará un modelo de datos para interactuar con la base de datos agregada en la tarea anterior.

1. Cree un modelo de datos que represente la base de datos. Para ello, en Explorador de soluciones haga clic con el botón secundario en la carpeta **modelos** , seleccione **Agregar** y, a continuación, haga clic en **nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la plantilla de **datos** y, a continuación, el elemento **Entity Data Model ADO.net** . Cambie el nombre del modelo de datos a **StoreDB. edmx** y haga clic en **Agregar**.

    ![Agregar el StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "Agregar el StoreDB ADO.NET Entity Data Model")

    *Agregar el StoreDB ADO.NET Entity Data Model*
2. Aparecerá el **Asistente para Entity Data Model** . Este asistente le guiará a través de la creación de la capa de modelo. Dado que el modelo debe crearse según la base de datos existente agregada recientemente, seleccione **generar desde la base de datos** y haga clic en **siguiente**.

    ![Elección del contenido del modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "Elección del contenido del modelo")

    *Elección del contenido del modelo*
3. Dado que va a generar un modelo a partir de una base de datos, deberá especificar la conexión que se va a usar. Haga clic en **nueva conexión**.
4. Seleccione **Microsoft SQL Server archivo de base de datos** y haga clic en **continuar**.

    ![Elegir origen de datos](aspnet-mvc-4-models-and-data-access/_static/image8.png "Elegir origen de datos")

    *Cuadro de diálogo elegir origen de datos*
5. Haga clic en **examinar** y seleccione la base de datos **MvcMusicStore. MDF** que se encuentra en la carpeta **App\_Data** y haga clic en **Aceptar**.

    ![Propiedades de conexión](aspnet-mvc-4-models-and-data-access/_static/image9.png "Propiedades de la conexión")

    *Propiedades de conexión*
6. La clase generada debe tener el mismo nombre que la cadena de conexión de entidad, así que cambie su nombre a **MusicStoreEntities** y haga clic en **siguiente**.

    ![Elegir la conexión de datos](aspnet-mvc-4-models-and-data-access/_static/image10.png "Elegir la conexión de datos")

    *Elegir la conexión de datos*
7. Elija los objetos de base de datos que desea usar. Dado que el modelo de entidad usará solo las tablas de la base de datos, seleccione la opción **tablas** y asegúrese de que las opciones **incluir columnas de clave externa en el modelo** y poner en **plural o en singular los nombres de objeto generados** también están seleccionadas. Cambie el espacio de nombres del modelo a **MvcMusicStore. Model** y haga clic en **Finalizar**.

    ![Elección de los objetos de base de datos](aspnet-mvc-4-models-and-data-access/_static/image11.png "Elección de los objetos de base de datos")

    *Elección de los objetos de base de datos*

    > [!NOTE]
    > Si se muestra un cuadro de diálogo de advertencia de seguridad, haga clic en **Aceptar** para ejecutar la plantilla y generar las clases para las entidades del modelo.
8. Aparecerá un diagrama de entidades para la base de datos, mientras que se creará una clase independiente que asigna cada tabla a la base de datos. Por ejemplo, la tabla de **álbumes** se representará mediante una clase **album** , donde cada columna de la tabla se asignará a una propiedad de clase. Esto le permitirá consultar y trabajar con objetos que representan las filas de la base de datos.

    ![Diagrama de entidades](aspnet-mvc-4-models-and-data-access/_static/image12.png "Diagrama de entidades")

    *Diagrama de entidades*

    > [!NOTE]
    > Las plantillas T4 (. TT) ejecutan código para generar las clases de entidades y sobrescribirán las clases existentes con el mismo nombre. En este ejemplo, las clases &quot;álbum&quot;, &quot;género&quot; y &quot;artista&quot; se han sobrescrito con el código generado.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Tarea 3: compilar la aplicación

En esta tarea, comprobará que, aunque la generación del modelo haya quitado las clases de modelo **album**, **género** y **artista** , el proyecto se compilará correctamente con las nuevas clases de modelo de datos.

1. Compile el proyecto; para ello, seleccione el elemento de menú **compilar** y, a continuación, **compile MvcMusicStore**.

    ![Compilar el proyecto](aspnet-mvc-4-models-and-data-access/_static/image13.png "Compilar el proyecto")

    *Compilar el proyecto*
2. El proyecto se compila correctamente. ¿Por qué sigue funcionando? Funciona porque las tablas de base de datos tienen campos que incluyen las propiedades que estaba usando en el **álbum** de clases quitadas y el **género**.

    ![Compilaciones correctas](aspnet-mvc-4-models-and-data-access/_static/image14.png "Compilaciones correctas")

    *Compilaciones correctas*
3. Aunque el diseñador muestra las entidades en un formato de diagrama, son realmente C# clases. Expanda el nodo **StoreDB. edmx** en el explorador de soluciones y, a continuación, **StoreDB.TT**, verá las nuevas entidades generadas.

    ![Archivos generados](aspnet-mvc-4-models-and-data-access/_static/image15.png "Archivos generados")

    *Archivos generados*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarea 4: consultar la base de datos

En esta tarea, actualizará la clase StoreController para que, en lugar de usar datos codificados, consulte la base de datos para recuperar la información.

1. Abra **Controllers\StoreController.CS** y agregue el siguiente campo a la clase para que contenga una instancia de la clase **MusicStoreEntities** , denominada **storeDB**:

    (Fragmentos de código- *modelos y acceso a datos-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. La clase **MusicStoreEntities** expone una propiedad de colección para cada tabla en la base de datos. Actualice el método de acción de **exploración** para recuperar un género con todos los **álbumes**.

    (Fragmentos de código- *modelos y acceso a datos; examen del almacén EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Está usando una funcionalidad de .NET llamada **LINQ** (Language-Integrated Query) para escribir expresiones de consulta fuertemente tipadas en estas colecciones, que ejecutan código en la base de datos y devuelven objetos que se pueden programar.
    > 
    > Para obtener más información acerca de LINQ, visite el [sitio de MSDN](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Actualice el método de acción de **Índice** para recuperar todos los géneros.

    (Fragmento de código- *modelos y acceso a datos-índice del almacén EX1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Actualice el método de acción de **Índice** para recuperar todos los géneros y transformar la colección en una lista.

    (Fragmentos de código- *modelos y acceso a datos-EX1 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, comprobará que la página de índice de la tienda mostrará ahora los géneros almacenados en la base de datos en lugar de los codificados. No es necesario cambiar la plantilla de vista porque el **StoreController** devuelve las mismas entidades que antes, pero esta vez los datos proceden de la base de datos.

1. Vuelva a compilar la solución y presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Compruebe que el menú de **géneros** ya no es una lista codificada y que los datos se recuperan directamente de la base de datos.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Examinar géneros de la base de datos*
3. Ahora, vaya a cualquier género y compruebe que los álbumes se han rellenado desde la base de datos.

    ![Examinar álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image17.png "Examinar álbumes de la base de datos")

    *Examinar álbumes de la base de datos*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Ejercicio 2: crear una base de datos con Code First

En este ejercicio, obtendrá información sobre cómo usar el enfoque Code First para crear una base de datos con las tablas de la aplicación MusicStore y cómo obtener acceso a sus datos.

Una vez generado el modelo, modificará el StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados de forma rígida.

> [!NOTE]
> Si ha completado el ejercicio 1 y ya ha trabajado con el enfoque Database First, ahora aprenderá a obtener los mismos resultados con un proceso diferente. Las tareas comunes con el ejercicio 1 se han marcado para facilitar la lectura. Si no ha completado el ejercicio 1 pero le gustaría aprender el enfoque de Code First, puede empezar a partir de este ejercicio y obtener una cobertura completa del tema.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Tarea 1: rellenar datos de ejemplo

En esta tarea, rellenará la base de datos con datos de ejemplo cuando se cree inicialmente con Code-First.

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex2-CreatingADatabaseCodeFirst/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Agregue el archivo **SampleData.CS** a la carpeta **Models** . Para ello, haga clic con el botón secundario en la carpeta **Models** , seleccione **Agregar** y, a continuación, haga clic en **elemento existente**. Vaya a **\Source\Assets** y seleccione el archivo **SampleData.CS** .

    ![Código de rellenado de datos de ejemplo](aspnet-mvc-4-models-and-data-access/_static/image18.png "Código de rellenado de datos de ejemplo")

    *Código de rellenado de datos de ejemplo*
3. Abra el archivo **global.asax.CS** y agregue las siguientes instrucciones *using* .

    (Fragmentos de código- *modelos y acceso a datos-Ex2 variables de asax globales mediante*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. En el método **Start () de Application\_,** agregue la siguiente línea para establecer el inicializador de base de datos.

    (Fragmentos de código- *modelos y acceso a datos-Ex2 global asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Tarea 2: configurar la conexión a la base de datos

Ahora que ya ha agregado una base de datos a nuestro proyecto, escribirá en el archivo **Web. config** la cadena de conexión.

1. Agregue una cadena de conexión en **Web. config**. Para ello, abra el **archivo Web. config** en la raíz del proyecto y reemplace la cadena de conexión denominada DefaultConnection por esta línea en la sección **&lt;connectionStrings&gt;** :

    ![Ubicación del archivo Web. config](aspnet-mvc-4-models-and-data-access/_static/image19.png "Ubicación del archivo Web. config")

    *Ubicación del archivo Web. config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Tarea 3: trabajar con el modelo

Ahora que ya ha configurado la conexión a la base de datos, vinculará el modelo con las tablas de base de datos. En esta tarea, creará una clase que se vinculará a la base de datos con Code First. Recuerde que hay una clase de modelo POCO existente que debe modificarse.

> [!NOTE]
> Si ha completado el ejercicio 1, observará que este paso lo realizó un asistente. Al realizar Code First, creará manualmente las clases que se vincularán a las entidades de datos.

1. Abra la carpeta del proyecto de clase POCO modelo **género** desde **modelos** e incluya un identificador. Use una propiedad int con el nombre **GenreId**.

    (Fragmentos de código- *modelos y acceso a datos-Ex2 Code First género*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Para trabajar con convenciones de Code First, el género de la clase debe tener una propiedad de clave principal que se detectará automáticamente.
    > 
    > Puede leer más información sobre las convenciones de Code First en este [artículo de MSDN](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Ahora, abra la carpeta de proyecto de clase POCO modelo **album** de **modelos** e incluya las claves externas, cree propiedades con los nombres **GenreId** y **ArtistId**. Esta clase ya tiene **GenreId** para la clave principal.

    (Fragmento de código- *modelos y acceso a datos-Ex2 Code First album*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Abra el **intérprete** de clase poco modelo y incluya la propiedad **ArtistId** .

    (Fragmentos de código- *modelos y acceso a datos-Ex2 Code First artista*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Haga clic con el botón derecho en la carpeta del proyecto **modelos** y seleccione **Agregar | Clase**. Asigne al archivo el nombre **MusicStoreEntities.CS**. A continuación, haga clic en **Agregar.**

    ![Agregar una clase](aspnet-mvc-4-models-and-data-access/_static/image20.png "Agregar una clase")

    *Agregar un nuevo elemento*

    ![Agregar una clase2](aspnet-mvc-4-models-and-data-access/_static/image21.png "Agregar una clase2")

    *Agregar una clase*
5. Abra la clase que acaba de crear, **MusicStoreEntities.CS**e incluya los espacios de nombres **System. Data. Entity** y **System. Data. Entity. Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Reemplace la declaración de clase para extender la clase **DbContext** : declare un método público **DBSet** e invalide **OnModelCreating** . Después de este paso, obtendrá una clase de dominio que vinculará el modelo con el Entity Framework. Para ello, reemplace el código de clase por lo siguiente:

    (Fragmentos de código- *modelos y acceso a datos-Ex2 Code First MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Con Entity Framework **DbContext** y **DBSet** , podrá consultar el género de clase poco. Al extender el método **OnModelCreating** , se especifica en el **código** cómo se asignará el género a una tabla de base de datos. Puede encontrar más información sobre DBContext y DBSet en este artículo de MSDN: [Link](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarea 4: consultar la base de datos

En esta tarea, actualizará la clase StoreController para que, en lugar de usar datos codificados, la recupere de la base de datos.

> [!NOTE]
> Esta tarea es común con el ejercicio 1.
> 
> Si ha completado el ejercicio 1, tendrá en cuenta que estos pasos son los mismos en ambos enfoques (primero la base de datos o el código primero). Son diferentes en la forma en que se vinculan los datos al modelo, pero el acceso a las entidades de datos todavía es transparente desde el controlador.

1. Abra **Controllers\StoreController.CS** y agregue el siguiente campo a la clase para que contenga una instancia de la clase **MusicStoreEntities** , denominada **storeDB**:

    (Fragmentos de código- *modelos y acceso a datos-EX1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. La clase **MusicStoreEntities** expone una propiedad de colección para cada tabla en la base de datos. Actualice el método de acción de **exploración** para recuperar un género con todos los **álbumes**.

    (Fragmentos de código- *modelos y acceso a datos-Ex2 Store Browse*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Está usando una funcionalidad de .NET llamada **LINQ** (Language-Integrated Query) para escribir expresiones de consulta fuertemente tipadas en estas colecciones, que ejecutan código en la base de datos y devuelven objetos que se pueden programar.
    > 
    > Para obtener más información acerca de LINQ, visite el [sitio de MSDN](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Actualice el método de acción de **Índice** para recuperar todos los géneros.

    (Fragmento de código- *modelos y acceso a datos-índice de almacén Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Actualice el método de acción de **Índice** para recuperar todos los géneros y transformar la colección en una lista.

    (Fragmentos de código- *modelos y acceso a datos-Ex2 Store GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, comprobará que la página de índice de la tienda mostrará ahora los géneros almacenados en la base de datos en lugar de los codificados. No es necesario cambiar la plantilla de vista porque el **StoreController** devuelve el mismo **StoreIndexViewModel** que antes, pero esta vez los datos proceden de la base de datos.

1. Vuelva a compilar la solución y presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Compruebe que el menú de **géneros** ya no es una lista codificada y que los datos se recuperan directamente de la base de datos.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Examinar géneros de la base de datos*
3. Ahora, vaya a cualquier género y compruebe que los álbumes se han rellenado desde la base de datos.

    ![Examinar álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image23.png "Examinar álbumes de la base de datos")

    *Examinar álbumes de la base de datos*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Ejercicio 3: consultar la base de datos con parámetros

En este ejercicio, obtendrá información sobre cómo consultar la base de datos mediante parámetros y cómo usar el modelado de resultados de consultas, una característica que reduce el número de accesos de base de datos a la recuperación de datos de una manera más eficaz.

> [!NOTE]
> Para obtener más información sobre el modelado de resultados de consultas, visite el siguiente [artículo de MSDN](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Tarea 1: modificación de StoreController para recuperar álbumes de la base de datos

En esta tarea, cambiará la clase **StoreController** para tener acceso a la base de datos con el fin de recuperar álbumes de un género específico.

1. Abra la solución de **Inicio** que se encuentra en la carpeta **Source\Ex3-QueryingTheDatabaseWithParametersCodeFirst\Begin** si quiere usar el enfoque de primer código o la carpeta **Source\Ex3-QueryingTheDatabaseWithParametersDBFirst\Begin** si desea usar el enfoque de primera base de datos. De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la clase **StoreController** para cambiar el método de acción de **exploración** . Para ello, en el **Explorador de soluciones**, expanda la carpeta **Controllers** y haga doble clic en **StoreController.CS**.
3. Cambie el método de acción de **exploración** para recuperar los álbumes de un género específico. Para ello, reemplace el código siguiente:

    (Fragmentos de código- *modelos y acceso a datos-Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Para rellenar una colección de la entidad, debe utilizar el método **include** para especificar que desea recuperar también los álbumes. Puede usar el. Extensión **Single ()** en LINQ porque, en este caso, solo se espera un género para un álbum. El método **Single ()** toma una expresión lambda como parámetro, que en este caso especifica un objeto de género único, de modo que su nombre coincide con el valor definido.
> 
> Aprovechará una característica que le permitirá indicar otras entidades relacionadas que desea cargar también cuando se recupere el objeto Genre. Esta característica se denomina **modelado de resultados de consultas**y permite reducir el número de veces que es necesario tener acceso a la base de datos para recuperar información. En este escenario, querrá capturar previamente los álbumes del género que recupere.
> 
> La consulta incluye **géneros. incluya (&quot;álbumes&quot;)** para indicar que desea también álbumes relacionados. Esto producirá una aplicación más eficaz, ya que recuperará los datos de género y de álbum en una única solicitud de base de datos.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecución de la aplicación

En esta tarea, ejecutará la aplicación y recuperará álbumes de un género específico de la base de datos.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/Store/Browse? Genre = pop** para comprobar que los resultados se recuperan de la base de datos.

    ![Examinar por género](aspnet-mvc-4-models-and-data-access/_static/image24.png "Examinar por género")

    *Examinar/Store/Browse? género = pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Tarea 3: acceso a álbumes por identificador

En esta tarea, repetirá el procedimiento anterior para obtener álbumes por su identificador.

1. Cierre el explorador si es necesario para volver a Visual Studio. Abra la clase **StoreController** para cambiar el método de acción **Details** . Para ello, en el **Explorador de soluciones**, expanda la carpeta **Controllers** y haga doble clic en **StoreController.CS**.
2. Cambie el método de acción de **detalles** para recuperar los detalles de los álbumes según su **identificador**. Para ello, reemplace el código siguiente:

    (Fragmentos de código- *modelos y acceso a datos-Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, ejecutará la aplicación en un explorador Web y obtendrá los detalles del álbum por su identificador.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la Página principal. Cambie la dirección URL a **/Store/details/51** o examine los géneros y seleccione un álbum para comprobar que los resultados se recuperan de la base de datos.

    ![Detalles de la exploración](aspnet-mvc-4-models-and-data-access/_static/image25.png "Detalles de la exploración")

    *Examinar/Store/Details/51*

> [!NOTE]
> Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido los aspectos básicos de los modelos y el acceso a los datos de ASP.NET MVC, mediante el enfoque de **Database First** , así como el enfoque **code First** :

- Cómo agregar una base de datos a la solución para consumir sus datos
- Cómo actualizar controladores para proporcionar plantillas de vista con los datos tomados de la base de datos en lugar de codificarlos de forma rígida
- Cómo consultar la base de datos mediante parámetros
- Cómo usar la forma de los resultados de la consulta, una característica que reduce el número de accesos a bases de datos, la recuperación de datos de una manera más eficaz
- Cómo usar los enfoques de Database First y Code First en Microsoft Entity Framework para vincular la base de datos con el modelo

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](aspnet-mvc-4-models-and-data-access/_static/image30.png)

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

    ![Iniciar sesión en Windows Azure Portal](aspnet-mvc-4-models-and-data-access/_static/image31.png "Iniciar sesión en Windows Azure Portal")

    *Iniciar sesión en Windows Azure Portal de administración*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](aspnet-mvc-4-models-and-data-access/_static/image32.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](aspnet-mvc-4-models-and-data-access/_static/image34.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](aspnet-mvc-4-models-and-data-access/_static/image35.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](aspnet-mvc-4-models-and-data-access/_static/image36.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.

    ![Descargar el perfil de publicación del sitio web](aspnet-mvc-4-models-and-data-access/_static/image37.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image38.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](aspnet-mvc-4-models-and-data-access/_static/image39.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](aspnet-mvc-4-models-and-data-access/_static/image40.png).

    ![Agregando la dirección IP del cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-models-and-data-access/_static/image43.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image44.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](aspnet-mvc-4-models-and-data-access/_static/image45.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configuración de la cadena de conexión de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](aspnet-mvc-4-models-and-data-access/_static/image48.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](aspnet-mvc-4-models-and-data-access/_static/image49.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](aspnet-mvc-4-models-and-data-access/_static/image50.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-models-and-data-access/_static/image51.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-models-and-data-access/_static/image52.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-models-and-data-access/_static/image53.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-models-and-data-access/_static/image54.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-models-and-data-access/_static/image55.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-models-and-data-access/_static/image56.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
