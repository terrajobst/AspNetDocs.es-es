---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework scaffolding y migraciones | Microsoft Docs
author: rick-anderson
description: Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y validación&quot; laboratorio práctico, debe tener en cuenta...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 2b26224390af70e19ca0593abe93a6867140f8ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484645"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Scaffolding y migraciones de Entity Framework de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y validación&quot; laboratorio práctico, debe tener en cuenta que muchas de las lógicas para crear, actualizar, enumerar y quitar cualquier entidad de datos se repiten entre la aplicación. No se debe mencionar que, si el modelo tiene varias clases que manipular, es probable que se dedique un tiempo considerable a escribir los métodos de acción POST y GET para cada operación de entidad, así como a cada una de las vistas.

En este laboratorio aprenderá a usar el scaffolding de ASP.NET MVC 4 para generar automáticamente la línea base del CRUD de la aplicación (creación, lectura, actualización y eliminación). A partir de una clase de modelo simple y, sin escribir una sola línea de código, creará un controlador que contendrá todas las operaciones CRUD, así como todas las vistas necesarias. Después de compilar y ejecutar la solución simple, tendrá la base de datos de la aplicación generada, junto con la lógica y las vistas de MVC para la manipulación de datos.

Además, aprenderá lo fácil que es usar las migraciones de Entity Framework para realizar actualizaciones de modelos en toda la aplicación. Entity Framework migraciones le permitirán modificar la base de datos una vez que el modelo haya cambiado con pasos sencillos. Teniendo en cuenta todos estos elementos, podrá compilar y mantener las aplicaciones Web de forma más eficaz, aprovechando las características más recientes de ASP.NET MVC 4.

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en [ASP.NET MVC 4 Entity Framework scaffolding y migraciones](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Use el scaffolding de ASP.NET para las operaciones CRUD en los controladores.
- Cambiar el modelo de base de datos mediante migraciones de Entity Framework.

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

El siguiente ejercicio constituye este laboratorio práctico:

1. [Uso de la técnica scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework](#Exercise1)

> [!NOTE]
> Este ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar el ejercicio. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en el ejercicio.

Tiempo estimado para completar este laboratorio: **30 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Ejercicio 1: uso de la técnica scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework

La técnica scaffolding de ASP.NET MVC proporciona una forma rápida de generar las operaciones CRUD de forma estandarizada, creando la lógica necesaria que permite a la aplicación interactuar con el nivel de base de datos.

En este ejercicio, obtendrá información sobre cómo usar la técnica de scaffolding de ASP.NET MVC 4 con Code First para crear los métodos CRUD. A continuación, obtendrá información sobre cómo actualizar el modelo aplicando los cambios en la base de datos mediante migraciones de Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Tarea 1: creación de un nuevo proyecto de ASP.NET MVC 4 con scaffolding

1. Si aún no está abierto, inicie **Visual Studio 2012**.
2. Seleccionar **archivo | Nuevo proyecto**. En el cuadro de diálogo nuevo proyecto, en el **objeto visual C# |** En la sección Web, seleccione **aplicación web MVC 4 ASP.net**. Asigne al proyecto el nombre **MVC4andEFMigrations** y establezca la ubicación en la carpeta **Source\Ex1-UsingMVC4ScaffoldingEFMigrations** de este laboratorio. Establezca el **nombre** de la solución en **Inicio** y asegúrese de que la opción **Crear directorio para la solución** está activada. Haga clic en **Aceptar**.

    ![Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4")

    *Cuadro de diálogo nuevo proyecto de ASP.NET MVC 4*
3. En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla **aplicación de Internet** y asegúrese de que **Razor** es el **motor de vista**seleccionado. Haga clic en **Aceptar** para crear el proyecto.

    ![Nueva aplicación de Internet ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "Nueva aplicación de Internet ASP.NET MVC 4")

    *Nueva aplicación de Internet ASP.NET MVC 4*
4. En el Explorador de soluciones, haga clic con el botón derecho en **modelos** y seleccione **Agregar | Clase** para crear una clase simple person (poco). Asígnele el nombre **Person** y haga clic en **Aceptar**.
5. Abra la clase Person e inserte las siguientes propiedades.

    (Fragmentos de código- *ASP.NET MVC 4 y migraciones Entity Framework-propiedades de la persona EX1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Haga clic en **generar | Compile la solución** para guardar los cambios y compilar el proyecto.

    ![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "Compilación de la aplicación")

    *Compilación de la aplicación*
7. En el Explorador de soluciones, haga clic con el botón derecho en la carpeta Controllers y seleccione **Agregar | Controlador**.
8. Asigne al controlador el nombre *PersonController* y complete las **Opciones de scaffolding** con los valores siguientes.

   1. En la lista desplegable **plantilla** , seleccione el **controlador de MVC con acciones y vistas de lectura/escritura con Entity Framework** opción.
   2. En la lista desplegable **clase de modelo** , seleccione la clase **Person** .
   3. En la lista **clase de contexto de datos** , seleccione **&lt;nuevo contexto de datos...&gt;** . Elija cualquier nombre y haga clic en **Aceptar**.
   4. En la lista desplegable **vistas** , asegúrese de que **Razor** está seleccionado.

      ![Adición del controlador person con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "Adición del controlador person con scaffolding")

      *Adición del controlador person con scaffolding*
9. Haga clic en **Agregar** para crear el nuevo controlador para la persona con scaffolding. Ahora ha generado las acciones del controlador, así como las vistas.

    ![Después de crear el controlador person con scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "Después de crear el controlador person con scaffolding")

    *Después de crear el controlador person con scaffolding*
10. Abra la clase **PersonController** . Tenga en cuenta que los métodos de acción CRUD completos se han generado automáticamente.

   ![Dentro del controlador Person](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "Dentro del controlador Person")

   *Dentro del controlador Person*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tarea 2: ejecución de la aplicación

En este momento, la base de datos aún no se ha creado. En esta tarea, ejecutará la aplicación por primera vez y probará las operaciones CRUD. La base de datos se creará sobre la marcha con Code First.

1. Presione **F5** para ejecutar la aplicación.
2. En el explorador, agregue **/Person** a la dirección URL para abrir la página person.

    ![Primera ejecución de la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "Primera ejecución de la aplicación")

    *Aplicación: primera ejecución*
3. Ahora explorará las páginas person y probará las operaciones CRUD.

    1. Haga clic en **crear nuevo** para agregar una nueva persona. Escriba un nombre y un apellido y haga clic en **crear**.

        ![Agregar una nueva persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "Agregar una nueva persona")

        *Agregar una nueva persona*
    2. En la lista de personas, puede eliminar, editar o agregar elementos.

        ![lista de personas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de personas")

        *Lista de personas*
    3. Haga clic en **detalles** para abrir los detalles de la persona.

        ![Detalles de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "Detalles de la persona")

        *Detalles de la persona*
4. Cierre el explorador y vuelva a Visual Studio. Tenga en cuenta que ha creado el CRUD completo para la entidad Person en toda la aplicación: desde el modelo hasta las vistas, sin tener que escribir una sola línea de código.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tarea 3: actualización de la base de datos mediante migraciones de Entity Framework

En esta tarea, actualizará la base de datos mediante migraciones de Entity Framework. Descubrirá lo fácil que es cambiar el modelo y reflejar los cambios en las bases de datos mediante la característica de migraciones de Entity Framework.

1. Abra la consola del administrador de paquetes. Seleccione **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.
2. En la Consola del Administrador de paquetes, escriba el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Habilitación de migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "Habilitación de migraciones")

    *Habilitación de migraciones*

    El comando enable-Migration crea la carpeta **Migrations** , que contiene un script para inicializar la base de datos.

    ![Carpeta migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "Carpeta migraciones")

    *Carpeta migraciones*
3. Abra el archivo **Configuration.CS** en la carpeta Migrations. Busque el constructor de clase y cambie el valor de **AutomaticMigrationsEnabled** a *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Abra la clase person y agregue un atributo para el segundo nombre de la persona. Con este nuevo atributo, está cambiando el modelo.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Seleccionar **compilación | Compilar solución** en el menú para compilar la aplicación.

    ![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "Compilación de la aplicación")

    *Compilar la aplicación*
6. En la Consola del Administrador de paquetes, escriba el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Este comando buscará cambios en los objetos de datos y, a continuación, agregará los comandos necesarios para modificar la base de datos según corresponda.

    ![Agregar un segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "Agregar un segundo nombre")

    *Agregar un segundo nombre*
7. Opta Puede ejecutar el siguiente comando para generar un script SQL con la actualización diferencial. Esto le permitirá actualizar la base de datos manualmente (en este caso, no es necesario) o aplicar los cambios en otras bases de datos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generar un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "Generación de un script SQL")

    *Generar un script SQL*

    ![Actualización de scripts SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "Actualización de scripts SQL")

    *Actualización de scripts SQL*
8. En la consola del administrador de paquetes, escriba el comando siguiente para actualizar la base de datos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Actualizar la base de datos](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "Actualizar la base de datos")

    *Actualizar la base de datos*

    Esto agregará la columna **MiddleName** de la tabla **People** para que coincida con la definición actual de la clase **Person** .
9. Una vez actualizada la base de datos, haga clic con el botón derecho en la carpeta Controller y seleccione **Agregar | Controlador** para volver a agregar el controlador de persona (complete con los mismos valores). Esto actualizará los métodos y vistas existentes agregando el nuevo atributo.

    ![Agregar una actualización de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "Agregar una actualización de controlador")

    *Actualización del controlador*
10. Haga clic en **Agregar**. A continuación, seleccione los valores **sobrescribir PersonController.CS** y **sobrescribir vistas asociadas** y haga clic en **Aceptar**.

   ![Agregar una sobrescritura de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Actualización del controlador*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4: ejecución de la aplicación

1. Presione **F5** para ejecutar la aplicación.
2. Abra **/Person**. Tenga en cuenta que se han conservado los datos, mientras que se ha agregado la columna Middle Name.

    ![Segundo nombre agregado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "Segundo nombre agregado")

    *Segundo nombre agregado*
3. Si hace clic en **Editar**, podrá agregar un segundo nombre a la persona actual.

    ![Edición de segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "Edición de segundo nombre")

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido pasos sencillos para crear operaciones CRUD con scaffolding de ASP.NET MVC 4 mediante cualquier clase de modelo. Después, ha aprendido a realizar una actualización de un extremo a otro en la aplicación: desde la base de datos hasta las vistas, mediante migraciones de Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express para Web icono*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)*** dimensional. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*
