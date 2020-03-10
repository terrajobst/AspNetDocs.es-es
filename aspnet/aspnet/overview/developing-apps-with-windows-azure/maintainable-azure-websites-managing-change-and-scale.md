---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Laboratorio práctico: sitios web de Azure que se mantienen: administración de cambios y escalado | Microsoft Docs'
author: rick-anderson
description: En este laboratorio, aprenderá cómo Microsoft Azure facilita la creación e implementación de sitios web en producción.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: c88bae40a8aa092037c0b359ee391acaf161cf10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78506377"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Laboratorio práctico: sitios web de Azure que se mantienen: administración de cambios y escalado

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

> Microsoft Azure facilita la creación e implementación de sitios web en producción. Pero no está listo cuando la aplicación está activa, ya está empezando. Deberá controlar los requisitos cambiantes, las actualizaciones de la base de datos, la escala y mucho más. Afortunadamente, Azure App Service le ha tratado, con una gran cantidad de características que le ayudarán a mantener sus sitios en funcionamiento sin problemas.
>
> Azure ofrece opciones de desarrollo, implementación y escalado seguras y flexibles para aplicaciones Web de cualquier tamaño. Aproveche sus herramientas existentes para crear e implementar aplicaciones sin las complicaciones de administrar infraestructura.
>
> Aprovisione una aplicación Web de producción en tan solo unos minutos mediante la implementación sencilla de contenido creado con su herramienta de desarrollo favorita. Puede implementar un sitio existente directamente desde el control de código fuente con compatibilidad con **git**, **GitHub**, **Bitbucket**, **TFS**e incluso **Dropbox**. Implemente directamente desde su IDE favorito o desde scripts mediante **PowerShell** en Windows o las herramientas de la **CLI** que se ejecutan en cualquier sistema operativo. Una vez implementado, mantenga sus sitios constantemente al día con compatibilidad para la implementación continua.
>
> Azure proporciona soluciones de almacenamiento en la nube, copia de seguridad y recuperación escalables y duraderas para los datos, grandes o pequeños. Al implementar aplicaciones en un entorno de producción, los servicios de almacenamiento, como tablas, blobs y bases de datos SQL, ayudan a escalar la aplicación en la nube.
>
> Con las bases de datos SQL, es importante mantener actualizada la base de datos productiva al implementar nuevas versiones de la aplicación. Gracias a **migraciones de Entity Framework Code First**, el desarrollo y la implementación de su modelo de datos se ha simplificado para actualizar sus entornos en minutos. Este laboratorio práctico le mostrará los distintos temas que puede encontrar al implementar la aplicación web en entornos de producción en Microsoft Azure.
>
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).
>
> Para obtener una cobertura más detallada de este tema, consulte la sección creación de aplicaciones en la [nube reales con el libro electrónico de Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).

<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Habilitación de migraciones de Entity Framework con un modelo existente
- Actualizar el modelo de objetos y la base de datos según corresponda con Entity Framework migraciones
- Implementación en Azure App Service con git
- Revertir a una implementación anterior mediante el portal de administración de Azure
- Uso de Azure Storage para escalar una aplicación Web
- Configuración del escalado automático para una aplicación web con Azure Portal de administración
- Crear y configurar un proyecto de prueba de carga en Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio práctico, es necesario lo siguiente:

- [Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/) o superior
- [SDK de Azure para .NET 2,2](https://www.microsoft.com/windowsazure/sdk/)
- [Sistema de control de versiones de GIT](http://git-scm.com/download)
- Una suscripción de Microsoft Azure

    - Regístrese para obtener una [evaluación gratuita](https://aka.ms/watk-freetrial)
    - Si es un Visual Studio Professional, Test Professional, Premium o Ultimate con MSDN o Plataformas de MSDN suscriptor, active el [beneficio de MSDN](https://aka.ms/watk-msdn) ahora para comenzar a desarrollar y probar en Azure
    - Los miembros de [BizSpark](https://aka.ms/watk-bizspark) reciben automáticamente las ventajas de Azure a través de sus suscripciones Visual Studio Ultimate con MSDN
    - Los miembros del programa [Microsoft Partner Network](https://aka.ms/watk-mpn) Cloud Essentials reciben créditos mensuales de Azure sin cargo alguno.

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

1. [Uso de migraciones de Entity Framework](#Exercise1)
2. [Implementación de una aplicación web en el almacenamiento provisional](#Exercise2)
3. [Realización de la reversión de la implementación en producción](#Exercise3)
4. [Escalado mediante Azure Storage](#Exercise4)
5. [Usar el escalado automático para Web Apps](#Exercise5) (opcional para Visual Studio 2013 Ultimate Edition)

Tiempo estimado para completar este laboratorio: **75 minutos**

> [!NOTE]
> Al iniciar Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñada para coincidir con un estilo de desarrollo determinado y determina los diseños de ventana, el comportamiento del editor, los fragmentos de código de IntelliSense y las opciones del cuadro de diálogo. En los procedimientos de este laboratorio se describen las acciones necesarias para realizar una tarea determinada en Visual Studio cuando se usa la colección de **configuración de desarrollo general** . Si elige una colección de valores de configuración diferente para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.

<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Ejercicio 1: uso de migraciones de Entity Framework

Al desarrollar una aplicación, el modelo de datos puede cambiar con el tiempo. Estos cambios podrían afectar al modelo existente en la base de datos (si está creando una nueva versión) y es importante mantener la base de datos actualizada para evitar errores.

Para simplificar el seguimiento de estos cambios en el modelo, **migraciones de Entity Framework Code First** detectar automáticamente los cambios que comparan el modelo con el esquema de la base de datos y genera código específico para actualizar la base de datos y crear nuevas *versiones* de la base de datos.

En este ejercicio se muestra cómo habilitar las **migraciones** para su aplicación y cómo puede detectar fácilmente y generar cambios para actualizar las bases de datos.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Tarea 1: habilitación de migraciones

En esta tarea, recorrerá los pasos necesarios para habilitar **migraciones de Entity Framework Code First** en la base de datos de **prueba** de la información y cambiar el modelo y comprender cómo se reflejan esos cambios en la base de datos.

1. Abra Visual Studio y abra el archivo de solución **GeekQuiz. sln** desde **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Compile la solución para descargar e instalar las dependencias del paquete **NuGet** . Para ello, haga clic con el botón secundario en la solución y haga clic en **compilar solución** o presione **Ctrl + Mayús + B**.
3. En el menú **herramientas** de Visual Studio, seleccione **Administrador de paquetes NuGet**y, a continuación, haga clic en **consola del administrador de paquetes**.
4. En la **consola del administrador de paquetes**, escriba el siguiente comando y, a continuación, presione **entrar**. Se creará una migración inicial basada en el modelo existente.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Habilitación de migraciones](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Habilitación de migraciones")

    *Habilitación de migraciones*

    > [!NOTE]
    > Este comando agrega una carpeta **Migrations** al proyecto de prueba que contiene un archivo denominado **Configuration.CS**. La clase de **configuración** permite configurar el comportamiento de las migraciones para el contexto.
5. Con las migraciones habilitadas, debe actualizar la clase de **configuración** para rellenar la base de datos con los datos iniciales que requiere la **prueba** . En **migraciones**, reemplace el archivo **Configuration.CS** por el que se encuentra en la carpeta **Source\Assets** de este laboratorio.

    > [!NOTE]
    > Puesto que las **migraciones** llamarán al método de **inicialización** con cada actualización de la base de datos, debe asegurarse de que los registros no estén duplicados en la base de datos. El método **AddOrUpdate** le ayudará a evitar los datos duplicados.
6. Para agregar una migración inicial, escriba el siguiente comando y, a continuación, presione **entrar**.

    > [!NOTE]
    > Asegúrese de que no hay ninguna base de datos denominada &quot;GeekQuizProd&quot; en la instancia de LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Agregar la migración de esquemas base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Agregar la migración de esquemas base")

    *Agregar la migración de esquemas base*

    > [!NOTE]
    > **Add-Migration** agregará scaffolding a la siguiente migración en función de los cambios realizados en el modelo desde la última vez que se creó la migración. En este caso, como es la primera migración del proyecto, agregará los scripts para crear todas las tablas definidas en la clase **TriviaContext** .
7. Ejecute el comando siguiente para ejecutar la migración a fin de actualizar la base de datos. Para este comando, especificará la marca **verbose** para ver las instrucciones SQL que se van a aplicar a la base de datos de destino.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Creando base de datos inicial](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creando base de datos inicial")

    *Creando base de datos inicial*

    > [!NOTE]
    > **Update-Database** aplicará las migraciones pendientes a la base de datos. En este caso, se creará la base de datos con la cadena de conexión definida en el archivo Web. config.
8. Vaya al menú **Ver** y Abra **Explorador de objetos de SQL Server**.

    ![Abrir en Explorador de objetos de SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Abrir en Explorador de objetos de SQL Server")

    *Abrir en Explorador de objetos de SQL Server*
9. En la ventana de **Explorador de objetos de SQL Server** , conéctese a la instancia de LocalDB haciendo clic con el botón secundario en el nodo **SQL Server** y seleccionando la opción **Agregar SQL Server..** ..

    ![Agregar una instancia de SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Agregar una instancia de SQL Server")

    *Agregar una instancia de SQL Server a Explorador de objetos de SQL Server*
10. Establezca el **nombre del servidor** en *(LocalDB) \v11.0* y deje la **autenticación de Windows** como modo de autenticación. Haga clic en **Conectar** para continuar.

    ![Conectar con LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Conectar con LocalDB")

    *Conectar con LocalDB*
11. Abra la base de datos **GeekQuizProd** y expanda el nodo **tablas** . Como puede ver, el comando **Update-Database** generó todas las tablas definidas en la clase **TriviaContext** . Busque el **dbo. Tabla TriviaQuestions** y abra el nodo columnas. En la tarea siguiente, agregará una nueva columna a esta tabla y actualizará la base de datos mediante **migraciones**.

    ![Columnas de preguntas de curiosidad](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Columnas de preguntas de curiosidad")

    *Columnas de preguntas de curiosidad*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Tarea 2: actualizar el esquema de la base de datos mediante migraciones

En esta tarea, usará **migraciones de Entity Framework Code First** para detectar un cambio en el modelo y generar el código necesario para actualizar la base de datos. Actualizará la entidad **TriviaQuestions** agregando una nueva propiedad a ella. A continuación, ejecutará los comandos para crear una nueva migración para incluir la nueva columna en la tabla.

1. En **Explorador de soluciones**, haga doble clic en el archivo **TriviaQuestion.CS** ubicado dentro de la carpeta **Models** .
2. Agregue una nueva propiedad denominada **Hint**, tal como se muestra en el siguiente fragmento de código.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. En la **consola del administrador de paquetes**, escriba el siguiente comando y, a continuación, presione **entrar**. Se creará una nueva migración que reflejará el cambio en nuestro modelo.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Agregar-migración](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Agregar-migración")

    *Agregar-migración*

    > [!NOTE]
    > Un archivo de migración se compone de dos métodos, **hacia arriba** y **hacia abajo**.
    >
    > - El método **up** se usará para especificar qué cambios debe aplicar la versión actual de la aplicación a la base de datos.
    > - La **baja** se usa para invertir los cambios que hemos agregado al método **up** .
    >
    > Cuando la migración de la base de datos actualiza la base de datos, se ejecutarán todas las migraciones en el orden de la marca de tiempo y solo las que no se hayan usado desde la última actualización (la tabla \_MigrationHistory realiza un seguimiento de las migraciones que se han aplicado). Se llamará al método **up** de todas las migraciones y se realizarán los cambios que se hayan especificado en la base de datos. Si decidimos volver a una migración anterior, se llamará al método **Down** para rehacer los cambios en un orden inverso.
4. En la **consola del administrador de paquetes**, escriba el siguiente comando y, a continuación, presione **entrar**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. La salida del comando **Update-Database** generó una instrucción **ALTER TABLE** de SQL para agregar una nueva columna a la tabla **TriviaQuestions** , tal y como se muestra en la imagen siguiente.

    ![Agregar instrucción SQL de columna generada](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Agregar instrucción SQL de columna generada")

    *Agregar instrucción SQL de columna generada*
6. En **Explorador de objetos de SQL Server**, actualice el **dbo. Tabla TriviaQuestions** y compruebe que se muestra la nueva columna **sugerencia** .

    ![Mostrar la nueva columna de sugerencia](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Mostrar la nueva columna de sugerencia")

    *Mostrar la nueva columna de sugerencia*
7. De nuevo en el editor de **TriviaQuestion.CS** , agregue una restricción **StringLength** a la propiedad *Hint* , tal como se muestra en el siguiente fragmento de código.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. En la **consola del administrador de paquetes**, escriba el siguiente comando y, a continuación, presione **entrar**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. En la **consola del administrador de paquetes**, escriba el siguiente comando y, a continuación, presione **entrar**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. La salida del comando **Update-Database** generó una instrucción **ALTER TABLE** de SQL para actualizar el tipo de columna *Hint* de la tabla **TriviaQuestions** , tal como se muestra en la imagen siguiente.

    ![Instrucción SQL Alter Column generada](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Instrucción SQL Alter Column generada")

    *Instrucción SQL Alter Column generada*
11. En **Explorador de objetos de SQL Server**, actualice el **dbo. Tabla TriviaQuestions** y compruebe que el tipo de columna **Hint** es **nvarchar (150)** .

    ![Mostrar la nueva restricción](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Mostrar la nueva restricción")

    *Mostrar la nueva restricción*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Ejercicio 2: implementación de una aplicación web en el almacenamiento provisional

**Web Apps en Azure App Service** le permite realizar publicaciones almacenadas provisionalmente. La publicación de ensayo crea una ranura de sitio de ensayo para cada sitio de producción predeterminado y le permite intercambiar estas ranuras sin tiempo de inactividad. Esto es realmente útil para validar los cambios antes de publicarlos en el sitio público, integrar incrementalmente el contenido del sitio y revertir si los cambios no funcionan según lo previsto.

En este ejercicio, implementará la aplicación de **prueba** integrada en el entorno de ensayo de la aplicación Web mediante el control de código fuente de Git. Para ello, creará la aplicación web y aprovisionará los componentes necesarios en el portal de administración, configurará un repositorio de **git** e incluirá el código fuente de la aplicación del equipo local en la ranura de ensayo. También actualizará la base de datos de producción con la **migraciones de Code First** que creó en el ejercicio anterior. A continuación, ejecutará la aplicación en este entorno de prueba para comprobar su funcionamiento. Una vez que esté satisfecho con el funcionamiento según sus expectativas, promoverá la aplicación a producción.

> [!NOTE]
> Para habilitar la publicación de ensayo, la aplicación Web debe estar en **modo estándar**. Tenga en cuenta que se aplicarán cargos adicionales si cambia la aplicación web al modo estándar. Para obtener más información sobre los precios, consulte [precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Tarea 1: creación de una aplicación web en Azure App Service

En esta tarea, creará una aplicación web en **Azure App Service** desde el portal de administración. También configurará un **SQL Database** para conservar los datos de la aplicación y configurar un repositorio de Git local para el control de código fuente.

1. Vaya al [portal de administración de Azure](https://manage.windowsazure.com) e inicie sesión con el cuenta de Microsoft asociado a su suscripción.

    ![Inicie sesión en el portal de administración de Azure.](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Inicie sesión en el portal de administración de Azure.*
2. Haga clic en **nuevo** en la barra de comandos en la parte inferior de la página.

    ![Creación de una nueva aplicación Web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creación de una nueva aplicación Web")

    *Creación de una nueva aplicación Web*
3. Haga clic en **proceso**, **sitio web** y, a continuación, en **creación personalizada**.

    ![Creación de una nueva aplicación Web mediante creación personalizada](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creación de una nueva aplicación Web mediante creación personalizada")

    *Creación de una nueva aplicación Web mediante creación personalizada*
4. En el cuadro de diálogo **nuevo sitio web: creación personalizada** , proporcione una **dirección URL** disponible (por ejemplo, un *quiz experto*), seleccione una ubicación en la lista desplegable **región** y seleccione **crear una nueva base de datos SQL** en la lista desplegable **base de datos** . Por último, active la casilla **publicar desde control de código fuente** y haga clic en **siguiente**.

    ![Personalización de la nueva aplicación Web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personalización de la nueva aplicación Web*
5. Especifique la siguiente información para la configuración de la base de datos:

   - En el cuadro de texto **nombre** , escriba un nombre de base de datos (por ejemplo, *geekquiz\_dB*).
   - En la lista **desplegable** servidor, seleccione **nuevo servidor de SQL Database**. Como alternativa, puede seleccionar un servidor existente.
   - En los cuadros **nombre de usuario de base** de datos y contraseña de **base** de datos, escriba el nombre de usuario y la contraseña de administrador para el servidor de SQL Database. Si selecciona un servidor que ya ha creado, se le pedirá la contraseña.

     ![Especificar la configuración de la base de datos](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Especificar la configuración de la base de datos*
6. Haga clic en **Siguiente** para continuar.
7. Seleccione **repositorio de Git local** para que lo use el control de código fuente y haga clic en **siguiente**.

    > [!NOTE]
    > Es posible que se le pidan las credenciales de implementación (un nombre de usuario y una contraseña).

    ![Creación del repositorio de Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Creación del repositorio de Git*
8. Espere hasta que se cree la nueva aplicación Web.

    > [!NOTE]
    > De forma predeterminada, Azure proporciona dominios en *azurewebsites.net* , pero también ofrece la posibilidad de establecer dominios personalizados mediante el portal de administración de Azure. Sin embargo, solo puede administrar dominios personalizados si usa determinados modos de Azure App Service.
    >
    > Azure App Service está disponible en las ediciones gratis, compartido, básico, estándar y Premium. En el modo gratuito y compartido, todas las aplicaciones web se ejecutan en un entorno de varios inquilinos y tienen cuotas de uso de la CPU, la memoria y la red. El número máximo de aplicaciones gratuitas puede variar con el plan. En el modo estándar, se eligen las aplicaciones que se ejecutan en máquinas virtuales dedicadas que corresponden a los recursos de proceso de Azure estándar. Puede encontrar la configuración del modo de aplicación web en el menú **escala** de la aplicación Web.
    >
    > ![Modos de Azure App Service](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Modos de Azure App Service")
    >
    > Si usa el modo **compartido** o **estándar** , podrá administrar dominios personalizados para su aplicación Web. para ello, vaya al menú de **configuración** de la aplicación y haga clic en **administrar dominios** en *nombres de dominio*.
    >
    > ![Administrar dominios](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Administrar dominios")
    >
    > ![Administrar dominios personalizados](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Administrar dominios personalizados")
9. Una vez creada la aplicación Web, haga clic en el vínculo de la columna **URL** para comprobar que la nueva aplicación web se está ejecutando.

    ![Exploración de la nueva aplicación Web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Exploración de la nueva aplicación Web*

    ![aplicación web en ejecución](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *aplicación web en ejecución*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Tarea 2: creación del SQL Database de producción

En esta tarea, usará el **migraciones de Entity Framework Code First** para crear la base de datos de destino de la instancia de **Azure SQL Database** que ha creado en la tarea anterior.

1. En el Portal de administración, vaya a la aplicación web que creó en la tarea anterior y vaya a su **Panel**.
2. En la página **Panel** , haga clic en el vínculo **Ver cadenas de conexión** en la sección vista **rápida** .

    ![Ver cadenas de conexión](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Ver cadenas de conexión")

    *Ver cadenas de conexión*
3. Copie el valor de la **cadena de conexión** y cierre el cuadro de diálogo.

    ![Cadena de conexión en Azure Portal de administración](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Cadena de conexión en Azure Portal de administración")

    *Cadena de conexión en Azure Portal de administración*
4. Haga clic en **bases de datos SQL** para ver la lista de bases de datos SQL en Azure.

    ![Menú de SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "Menú de SQL Database")

    *Menú de SQL Database*
5. Busque la base de datos que ha creado en la tarea anterior y haga clic en el servidor.

    ![Servidor de SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "un servidor de SQL Database")

    *Servidor de SQL Database*
6. En la página **Inicio rápido** del servidor, haga clic en **configurar**.

    ![Menú configurar](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Menú configurar")

    *Menú configurar*
7. En la sección **direcciones IP permitidas** , haga clic en **Agregar al vínculo direcciones IP permitidas** para permitir que la dirección IP se conecte al servidor de SQL Database.

    ![Direcciones IP permitidas](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Direcciones IP permitidas")

    *Direcciones IP permitidas*
8. Haga clic en **Guardar** en la parte inferior de la página para completar el paso.
9. Vuelva a Visual Studio.
10. En la **consola del administrador de paquetes**, ejecute el siguiente comando reemplazando el marcador de posición *[su-conexión-cadena]* con la cadena de conexión que copió de Azure.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Actualizar la base de datos que tiene como destino Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Actualizar la base de datos que tiene como destino Windows Azure SQL Database")

    *Actualizar destinatarios de base de datos Azure SQL Database*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Tarea 3: implementación de una prueba integrada en el almacenamiento provisional con git

En esta tarea, habilitará la publicación de ensayo en la aplicación Web. A continuación, usará git para publicar la aplicación de prueba de la que se trata directamente desde su equipo local en el entorno de ensayo de la aplicación Web.

1. Vuelva al portal y haga clic en el nombre de la aplicación web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de aplicaciones Web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Abrir las páginas de administración de aplicaciones Web*
2. Navegue hasta la página **escalar** . En la sección **General** , seleccione **estándar** para la configuración y haga clic en **Guardar** en la barra de comandos.

    > [!NOTE]
    > Para ejecutar todas las aplicaciones web en la región y la suscripción actuales en el modo **estándar** , deje seleccionada la casilla **seleccionar todo** en la configuración **elegir sitios** . De lo contrario, desactive la casilla **seleccionar todo** .

    ![Actualización de la aplicación web al modo estándar](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Actualización de la aplicación web al modo estándar")

    *Actualización de la aplicación web al modo estándar*
3. Haga clic en **sí** para confirmar los cambios.

    ![Confirmar el cambio en el modo estándar](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuar con el cambio del modo de la aplicación Web")

    *Confirmar el cambio en el modo estándar*
4. Vaya a la página **Panel** y haga clic en **Habilitar publicación almacenada provisionalmente** en la sección **vista rápida** .

    ![Habilitar la publicación de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Habilitar la publicación de ensayo")

    *Habilitar la publicación de ensayo*
5. Haga clic en **sí** para habilitar la publicación de ensayo.

    ![Confirmación de la publicación de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Hacer clic en sí para habilitar la publicación de ensayo")

    *Confirmación de la publicación de ensayo*
6. En la lista de aplicaciones Web, expanda la marca situada a la izquierda del nombre de la aplicación web para mostrar la ranura del sitio de ensayo. Tiene el nombre de la aplicación web seguido de ***(ensayo)***. Haga clic en el sitio de ensayo para ir a la página de administración.

    ![Navegar a la aplicación Web de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navegar a la aplicación Web de ensayo")

    *Navegar a la aplicación de ensayo*
7. Tenga en cuenta que la página de administración es similar a cualquier otra página de administración de la aplicación Web. Vaya a la página **implementaciones** y copie el valor de **dirección URL de Git** . La usará más adelante en este ejercicio.

    ![Copiar el valor de la dirección URL de Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copiar el valor de la dirección URL de Git*
8. Abra una nueva consola de **git Bash** y ejecute los siguientes comandos. Actualice el marcador de posición *[Your-Application-path]* con la ruta de acceso a la solución **GeekQuiz** , que se encuentra en la carpeta **Source\Ex1-DeployingWebSiteToStaging\Begin** de este laboratorio.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicialización de Git y primera confirmación](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicialización de Git y primera confirmación*
9. Ejecute el siguiente comando para enviar la aplicación web al repositorio de **git** remoto. Reemplace el marcador de posición por la dirección URL que obtuvo en el portal de administración. Se le pedirá la contraseña de implementación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Insertar en Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Inserción en Azure*

    > [!NOTE]
    > Al implementar contenido en el host FTP o en el repositorio de GIT de una aplicación Web, debe autenticarse con las **credenciales de implementación** que creó desde las páginas de administración de **Inicio rápido** o **Panel** de la aplicación Web. Si no conoce las credenciales de implementación, puede restablecerlas fácilmente mediante el portal de administración. Abra la página **Panel** de la aplicación web y haga clic en el vínculo **restablecer las credenciales de implementación** . Proporcione una nueva contraseña y haga clic en **Aceptar**. Las credenciales de implementación son válidas para su uso con todas las aplicaciones web asociadas a su suscripción.
10. Para comprobar que la aplicación web se ha insertado correctamente en Azure, vuelva al portal de administración y haga clic en **sitios web**.
11. Busque la aplicación web y expanda la entrada para mostrar la ranura de sitio de ensayo. Haga clic en su **nombre** para ir a la página de administración.
12. Haga clic en **implementaciones** para ver el **historial de implementaciones**. Compruebe que haya una **implementación activa** con su *&quot;&quot;de confirmación inicial* .

    ![Implementación activa](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Implementación activa*
13. Por último, haga clic en **examinar** en la barra de comandos para ir a la aplicación Web.

    ![Examinar aplicación Web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Examinar aplicación Web*
14. Si la aplicación se implementa correctamente, verá la página de inicio de sesión de quiz.

    > [!NOTE]
    > La dirección URL de la aplicación implementada contiene el nombre de la aplicación web seguido *de-staging*.

    ![Aplicación que se ejecuta en el entorno de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplicación que se ejecuta en el entorno de ensayo*
15. Si desea explorar la aplicación, haga clic en **registrar** para registrar un nuevo usuario. Para completar los detalles de la cuenta, escriba un nombre de usuario, una dirección de correo electrónico y una contraseña. A continuación, la aplicación muestra la primera pregunta de la prueba. Responda a algunas preguntas para asegurarse de que funciona según lo previsto.

    ![Aplicación lista para usarse](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplicación lista para usarse*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Tarea 4: promoción de la aplicación web a producción

Ahora que ha comprobado que la aplicación web funciona correctamente en el entorno de ensayo, está listo para promoverla a producción. En esta tarea, intercambiará la ranura del sitio de ensayo con la del sitio de producción.

1. Vuelva al portal de administración y seleccione la ranura del sitio de ensayo. Haga clic en **intercambiar** en la barra de comandos.

    ![Intercambio a producción](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Intercambio a producción*
2. Haga clic en **sí** en el cuadro de diálogo de confirmación para continuar con la operación de intercambio. Azure intercambiará inmediatamente el contenido del sitio de producción con el contenido del sitio de ensayo.

    > [!NOTE]
    > Algunos valores de configuración de la versión almacenada provisionalmente se copiarán automáticamente a la versión de producción (por ejemplo, invalidaciones de cadenas de conexión, asignaciones de controlador, etc.), pero no cambiarán otras opciones (por ejemplo, extremos DNS, enlaces SSL, etc.).

    ![Confirmar la operación de intercambio](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Confirmar la operación de intercambio*
3. Una vez completado el intercambio, seleccione la ranura de producción y haga clic en **examinar** en la barra de comandos para abrir el sitio de producción. Observe la dirección URL en la barra de direcciones.

    > [!NOTE]
    > Es posible que tenga que actualizar el explorador para borrar la memoria caché. En Internet Explorer, puede hacer esto presionando **Ctrl + R**.

    ![Aplicación web que se ejecuta en el entorno de producción](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. En la consola de **GitBash** , actualice la dirección URL remota del repositorio de Git local para que tenga como destino la ranura de producción. Para ello, ejecute el siguiente comando reemplazando los marcadores de posición por el nombre de usuario de implementación y el nombre de la aplicación Web.

    > [!NOTE]
    > En los siguientes ejercicios, se enviarán cambios al sitio de producción en lugar de almacenarse provisionalmente solo por la simplicidad del laboratorio. En un escenario real, se recomienda comprobar los cambios en el entorno de ensayo antes de promoverlos a producción.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Ejercicio 3: realización de la reversión de la implementación en producción

Hay escenarios en los que no tiene una ranura de ensayo para realizar el intercambio activo entre el almacenamiento provisional y la producción, por ejemplo, si está trabajando con el modo **gratuito** o **compartido** . En estos escenarios, debe probar la aplicación en un entorno de prueba, ya sea de forma local o en un sitio remoto, antes de la implementación en producción. Sin embargo, es posible que se produzca un problema que no se detectó durante la fase de prueba en el sitio de producción. En este caso, es importante tener un mecanismo para cambiar fácilmente a una versión anterior y más estable de la aplicación lo más rápido posible.

En **Azure App Service**, la implementación continua desde el control de código fuente hace posible gracias a la acción **volver a implementar** disponible en el portal de administración. Azure realiza un seguimiento de las implementaciones asociadas a las confirmaciones insertadas en el repositorio y proporciona una opción para volver a implementar la aplicación con cualquiera de las implementaciones anteriores, en cualquier momento.

En este ejercicio, realizará un cambio en el código de la aplicación de **prueba** de la que inserta intencionadamente un *error*. Implementará la aplicación en producción para ver el error y, a continuación, aprovechará la característica volver a implementar para volver al estado anterior.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Tarea 1: actualización de la aplicación de prueba fanático

En esta tarea, refactorizará una pequeña parte del código de la clase **TriviaController** para extraer parte de la lógica que recupera la opción de quiz seleccionada de la base de datos en un nuevo método.

1. Cambie a la instancia de Visual Studio con la solución **GeekQuiz** del ejercicio anterior.
2. En **Explorador de soluciones**, abra el archivo **TriviaController.CS** dentro de la carpeta **Controllers** .
3. Busque el método **StoreAsync** y seleccione el código resaltado en la ilustración siguiente.

    ![Selección del código](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Selección del código*
4. Haga clic con el botón derecho en el código seleccionado, expanda el menú **refactorizar** y seleccione **Extraer método..** ..

    ![Extraer el código como un nuevo método](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Seleccionar extraer método*
5. En el cuadro de diálogo **Extraer método** , asigne al nuevo método el nombre *MatchesOption* y haga clic en **Aceptar**.

    ![Especificar el nombre del método](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Especificar el nombre del método extraído*
6. A continuación, el código seleccionado se extrae en el método **MatchesOption** . El código resultante se muestra en el siguiente fragmento de código.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Presione **Ctrl + S** para guardar los cambios.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Tarea 2: volver a implementar la aplicación de prueba fanático

Ahora realizará la instalación de los cambios realizados en la tarea anterior en el repositorio, lo que desencadenará una nueva implementación en el entorno de producción. A continuación, se solucionar un problema con las **herramientas de desarrollo F12** que proporciona Internet Explorer y, a continuación, se realiza una reversión a la implementación anterior desde el portal de administración de Azure.

1. Abra una nueva consola de **git Bash** para implementar la aplicación actualizada en Azure App Service.
2. Ejecute los siguientes comandos para enviar los cambios a Azure. Actualice el marcador de posición *[Your-Application-path]* con la ruta de acceso a la solución **GeekQuiz** . Se le pedirá la contraseña de implementación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Inserción de código refactorizado en Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Inserción de código refactorizado en Azure*
3. Abra Internet Explorer y vaya a la aplicación web (por ejemplo, `http://<your-web-site>.azurewebsites.net`). Inicie sesión con las credenciales creadas anteriormente.
4. Presione **F12** para iniciar las herramientas de desarrollo, seleccione la pestaña **red** y haga clic en el botón **reproducir** para iniciar la grabación.

    ![Inicio de la grabación de red](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Inicio de la grabación de red")

    *Inicio de la grabación de red*
5. Seleccione cualquier opción de la prueba. Verá que no sucede nada.
6. En la ventana de **F12** , la entrada correspondiente a la solicitud post http muestra un resultado http **500** .

    ![Error HTTP 500](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *Error HTTP 500*
7. Seleccione la pestaña **consola** . Se registra un error con los detalles de la causa.

    ![Error registrado](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Error registrado*
8. Busque la parte de detalles del error. Obviamente, este error se debe a que la refactorización de código se ha confirmado en los pasos anteriores.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`Operador
9. No cierre el explorador.
10. En una nueva instancia del explorador, vaya al [portal de administración de Azure](https://manage.windowsazure.com) e inicie sesión con el cuenta de Microsoft asociado a su suscripción.
11. Seleccione **sitios** web y haga clic en la aplicación web que creó en el ejercicio 2.
12. Vaya a la página **implementaciones** . Observe que todas las confirmaciones realizadas se muestran en el historial de implementaciones.

    ![Lista de implementaciones existentes](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Lista de implementaciones existentes*
13. Seleccione la confirmación anterior y haga clic en **volver a implementar** en la barra de comandos.

    ![Volver a implementar la confirmación anterior](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Volver a implementar la confirmación anterior*
14. Cuando se le solicite confirmar, haga clic en **Sí**.

    ![Confirmar la reimplementación](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Una vez finalizada la implementación, vuelva a la instancia del explorador con la aplicación web y presione **Ctrl + F5**.
16. Haga clic en cualquiera de las opciones. Ahora se realizará la animación de volteo y se mostrará el resultado (*correcto o incorrecto*).
17. Opta Cambie a la consola de **git Bash** y ejecute los siguientes comandos para revertir a la confirmación anterior.

    > [!NOTE]
    > Estos comandos crean una nueva confirmación que deshace todos los cambios del repositorio de Git que se realizaron en la confirmación incorrecta. Después, Azure volverá a implementar la aplicación con la nueva confirmación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Ejercicio 4: escalado mediante Azure Storage

Los **blobs** son la forma más sencilla de almacenar grandes cantidades de texto no estructurado o datos binarios como vídeo, audio e imágenes. Mover el contenido estático de la aplicación al almacenamiento ayuda a escalar la aplicación mediante el servicio de imágenes o documentos directamente en el explorador.

En este ejercicio, moverá el contenido estático de la aplicación a un contenedor de blobs. Después, configurará la aplicación para agregar una **regla de reescritura de URL de ASP.net** en el **archivo Web. config** para redirigir el contenido al contenedor de blobs.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Tarea 1: creación de una cuenta de Azure Storage

En esta tarea, aprenderá a crear una nueva cuenta de almacenamiento mediante el portal de administración.

1. Vaya al [portal de administración de Azure](https://manage.windowsazure.com) e inicie sesión con el cuenta de Microsoft asociado a su suscripción.
2. Seleccione **nuevo | Data Services | Almacenamiento | Creación rápida** para empezar a crear una nueva cuenta de almacenamiento. Escriba un nombre único para la cuenta y seleccione una **región** de la lista. Haga clic en **crear cuenta de almacenamiento** para continuar.

    ![Creación de una nueva cuenta de almacenamiento](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creación de una nueva cuenta de almacenamiento")

    *Creación de una nueva cuenta de almacenamiento*
3. En la sección **almacenamiento** , espere a que el estado de la nueva cuenta de almacenamiento cambie a *en línea* para continuar con el siguiente paso.

    ![Cuenta de almacenamiento creada](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Cuenta de almacenamiento creada")

    *Cuenta de almacenamiento creada*
4. Haga clic en el nombre de la cuenta de almacenamiento y, a continuación, haga clic en el vínculo **Panel** en la parte superior de la página. En la página **Panel** se proporciona información sobre el estado de la cuenta y los puntos de conexión de servicio que se pueden usar en las aplicaciones.

    ![Mostrar el panel de la cuenta de almacenamiento](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Mostrar el panel de la cuenta de almacenamiento")

    *Mostrar el panel de la cuenta de almacenamiento*
5. Haga clic en el botón **administrar claves de acceso** en la barra de navegación.

    ![Botón administrar claves de acceso](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Botón administrar claves de acceso")

    *Botón administrar claves de acceso*
6. En el cuadro de diálogo **administrar claves de acceso** , copie el nombre de la **cuenta de almacenamiento** y la **clave de acceso principal** , ya que los necesitará en el ejercicio siguiente. A continuación, cierre el cuadro de diálogo.

    ![Cuadro de diálogo administrar clave de acceso](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Cuadro de diálogo administrar clave de acceso")

    *Cuadro de diálogo administrar clave de acceso*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Tarea 2: cargar un recurso en Azure Blob Storage

En esta tarea, usará la ventana de Explorador de servidores de Visual Studio para conectarse a su cuenta de almacenamiento. Después creará un contenedor de blobs y cargará un archivo con el logotipo de la prueba de experto en el contenedor.

1. Cambie a la instancia de Visual Studio con la solución **GeekQuiz** del ejercicio anterior.
2. En la barra de menús, seleccione **Ver** y, a continuación, haga clic en **Explorador de servidores**.
3. En **Explorador de servidores**, haga clic con el botón secundario en el nodo **Azure** y seleccione **conectar a Azure..** .. Inicie sesión con el cuenta de Microsoft asociado a su suscripción.

    ![Conectarse a Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Conexión a Azure*
4. Expanda el nodo **Azure** , haga clic con el botón derecho en **almacenamiento** y seleccione **asociar almacenamiento externo..** ..
5. En el cuadro de diálogo **Agregar nueva cuenta de almacenamiento** , escriba el **nombre de cuenta** y la **clave de cuenta** que obtuvo en la tarea anterior y haga clic en **Aceptar**.

    ![Cuadro de diálogo Agregar nueva cuenta de almacenamiento](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Cuadro de diálogo Agregar nueva cuenta de almacenamiento*
6. La cuenta de almacenamiento debe aparecer en el nodo **almacenamiento** . Expanda la cuenta de almacenamiento, haga clic con el botón derecho en **blobs** y seleccione **crear contenedor de blobs.** ...

    ![Crear contenedor de blobs](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Crear contenedor de blobs")

    *Crear contenedor de blobs*
7. En el cuadro de diálogo **crear contenedor de blobs** , escriba un nombre para el contenedor de blobs y haga clic en **Aceptar**.

    ![Cuadro de diálogo crear contenedor de blobs](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Cuadro de diálogo crear contenedor de blobs")

    *Cuadro de diálogo crear contenedor de blobs*
8. El nuevo contenedor de blobs debe agregarse al nodo **blobs** . Cambie los permisos de acceso en el contenedor para que el contenedor sea público. Para ello, haga clic con el botón secundario en el contenedor **images** y seleccione **propiedades**.

    ![propiedades del contenedor de imágenes](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "propiedades del contenedor de imágenes")

    *Propiedades del contenedor de imágenes*
9. En la ventana **propiedades** , establezca el **acceso de lectura público** en el **contenedor**.

    ![Cambiar la propiedad de acceso de lectura público](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Cambiar la propiedad de acceso de lectura público")

    *Cambiar la propiedad de acceso de lectura público*
10. Cuando se le pregunte si está seguro de que desea cambiar la propiedad de acceso público, haga clic en **sí**.

    ![Microsoft Visual Studio ADVERTENCIA](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio ADVERTENCIA")

    *Microsoft Visual Studio ADVERTENCIA*
11. En **Explorador de servidores**, haga clic con el botón derecho en el contenedor de blob **images** y seleccione **Ver contenedor de blobs**.

    ![Ver contenedor de blobs](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Ver contenedor de blobs")

    *Ver contenedor de blobs*
12. El contenedor de imágenes debe abrirse en una nueva ventana y se debe mostrar una leyenda sin entradas. Haga clic en el icono de **carga** para cargar un archivo en el contenedor de blobs.

    ![Contenedor de imágenes sin entradas](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Contenedor de imágenes sin entradas")

    *Contenedor de imágenes sin entradas*
13. En el cuadro de diálogo **cargar BLOB** , navegue a la carpeta **activos** del laboratorio. Seleccione el archivo **logo-Big. png** y haga clic en **abrir**.
14. Espere hasta que se cargue el archivo. Una vez finalizada la carga, el archivo debe aparecer en el contenedor images. Haga clic con el botón derecho en la entrada archivo y seleccione **copiar dirección URL**.

    ![Copiar dirección URL del BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copiar dirección URL de archivo de BLOB")

    *Copiar dirección URL del BLOB*
15. Abra Internet Explorer y pegue la dirección URL. La siguiente imagen debe mostrarse en el explorador.

    ![imagen de logo-Big. png desde Blob Storage de Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "imagen de logo-Big. png desde el almacenamiento")

    *imagen de logo-Big. png desde Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Tarea 3: actualización de la solución para consumir contenido estático desde Azure Blob Storage

En esta tarea, configurará la solución **GeekQuiz** para consumir la imagen que se ha cargado en Azure BLOB Storage (en lugar de la imagen que se encuentra en la aplicación web) agregando una regla de reescritura de dirección URL de ASP.net en el archivo **Web. config** .

1. En Visual Studio, abra el archivo **Web. config** en el proyecto **GeekQuiz** y busque el elemento **&lt;System. WebServer&gt;** .
2. Agregue el código siguiente para agregar una regla de reescritura de URL, actualizando el marcador de posición con el nombre de la cuenta de almacenamiento.

    (Fragmento de código- *WebSitesInProduction-Ex4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > La reescritura de direcciones URL es el proceso de interceptar una solicitud Web de entrada y redirigir la solicitud a otro recurso. Las reglas de reescritura de direcciones URL indican al motor de reescritura Cuándo es necesario redirigir una solicitud y dónde deben redirigirse. Una regla de reescritura se compone de dos cadenas: el patrón que se va a buscar en la dirección URL solicitada (normalmente, mediante expresiones regulares) y la cadena con la que se va a reemplazar el modelo, si se encuentra. Para obtener más información, consulte [reescritura de direcciones URL en ASP.net](https://msdn.microsoft.com/library/ms972974.aspx).
3. Presione **Ctrl + S** para guardar los cambios.
4. Abra una nueva consola de **git Bash** para implementar la aplicación actualizada en Azure App Service.
5. Ejecute los siguientes comandos para enviar los cambios a Azure. Actualice el marcador de posición *[Your-Application-path]* con la ruta de acceso a la solución **GeekQuiz** . Se le pedirá la contraseña de implementación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Implementación de la actualización en Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Implementación de la actualización en Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Tarea 4: comprobación

En esta tarea usará **Internet Explorer** para examinar la aplicación de **prueba** de la ayuda del usuario y comprobará que la regla de reescritura de direcciones URL para las imágenes funciona y se le redirigirá a la imagen hospedada en **Azure BLOB Storage**.

1. Abra Internet Explorer y vaya a la aplicación web (por ejemplo, `http://<your-web-site>.azurewebsites.net`). Inicie sesión con las credenciales creadas anteriormente.

    ![Visualización de la aplicación Web de quiz con la imagen](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Visualización de la aplicación Web de quiz con la imagen")

    *Visualización de la aplicación Web de quiz con la imagen*
2. Presione **F12** para iniciar las herramientas de desarrollo, seleccione la pestaña **red** e inicie la grabación.

    ![Inicio de la grabación de red](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Inicio de la grabación de red")

    *Inicio de la grabación de red*
3. Presione **Ctrl + F5** para actualizar la Página Web.
4. Una vez finalizada la carga de la página, debería ver una solicitud HTTP para la dirección URL de **/IMG/logo-Big.png** con un resultado http **301** (redireccionamiento) y otra solicitud de `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` dirección URL con un resultado http **200** .

    ![Comprobando la redirección de la dirección URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Mostrar la redirección en herramientas de desarrollo")

    *Comprobando la redirección de la dirección URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Ejercicio 5: usar el escalado automático para Web Apps

> [!NOTE]
> Este ejercicio es opcional, ya que requiere compatibilidad con la carga web &amp; pruebas de rendimiento que solo está disponible para **Visual Studio 2013 Ultimate Edition**. Para obtener más información sobre características de Visual Studio 2013 específicas, compare versiones [aquí](https://www.microsoft.com/visualstudio/eng/products/compare).

**Azure App Service Web Apps** proporciona la característica de escalado automático para las aplicaciones web que se ejecutan en **modo estándar**. El escalado automático permite que Azure escale automáticamente el recuento de instancias de la aplicación web en función de la carga. Cuando el escalado automático está habilitado, Azure comprueba la CPU de la aplicación web una vez cada cinco minutos y agrega las instancias según sea necesario en ese momento. Si el uso de CPU es bajo, Azure quitará las instancias una vez cada dos horas para asegurarse de que el rendimiento de la aplicación web no esté degradado.

En este ejercicio, recorrerá los pasos necesarios para configurar la característica de **escalado automático** para la aplicación Web de **quiz** . Comprobará esta característica ejecutando una prueba de carga de Visual Studio para generar suficiente carga de CPU en la aplicación para desencadenar una actualización de la instancia.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Tarea 1: configuración del escalado automático en función de la métrica de la CPU

En esta tarea usará el portal de administración de Azure para habilitar la característica de escalado automático para la aplicación web que creó en el ejercicio 2.

1. En el [portal de administración de Azure](https://manage.windowsazure.com/), seleccione **websites** y haga clic en la aplicación web que creó en el ejercicio 2.
2. Navegue hasta la página **escalar** . En la sección **capacidad** , seleccione **CPU** para la configuración de **escalado por métrica** .

    > [!NOTE]
    > Al escalar por CPU, Azure ajusta dinámicamente el número de instancias que usa la aplicación si cambia el uso de la CPU.

    ![Selección de escalado por CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selección de la métrica de CPU para el escalado automático")

    *Selección de escalado por CPU*
3. Cambie la configuración de la **CPU de destino** a **20**-**40** por ciento.

    > [!NOTE]
    > Este intervalo representa el uso medio de la CPU para la aplicación Web. Azure agregará o quitará instancias para mantener la aplicación web dentro de este intervalo. El número mínimo y máximo de instancias usadas para el escalado se especifican en la configuración de **recuento de instancias** . Azure no superará nunca este límite.
    >
    > Los valores de **CPU de destino** predeterminados se modifican solo para los fines de este laboratorio. Al configurar el intervalo de CPU con valores pequeños, aumenta las posibilidades de desencadenar el escalado automático cuando se coloca una carga moderada en la aplicación.

    ![Cambio de la CPU de destino para que esté entre el 20 y el 40 por ciento](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Cambio de la CPU de destino para que esté entre el 20 y el 40 por ciento")

    *Cambio de la CPU de destino para que esté entre el 20 y el 40 por ciento*
4. Haga clic en **Guardar** en la barra de comandos para guardar los cambios.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Tarea 2: pruebas de carga con Visual Studio

Ahora que se ha configurado el **escalado automático** , creará un **proyecto de prueba de carga y rendimiento web** en Visual Studio para generar alguna carga de CPU en la aplicación Web.

1. Abra **Visual Studio Ultimate 2013** y seleccione **archivo | Nuevo | Proyecto...** para iniciar una nueva solución.

    ![Crear un nuevo proyecto](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Crear un nuevo proyecto")

    *Crear un nuevo proyecto*
2. En el cuadro de diálogo **nuevo proyecto** , seleccione **proyecto de prueba de carga y rendimiento web** en el **objeto C# visual | Pestaña prueba** . Asegúrese de que se ha seleccionado **.NET Framework 4,5** , asigne al proyecto el nombre *WebAndLoadTestProject*, elija una **Ubicación** y haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de prueba de carga y Web](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Crear un nuevo proyecto de prueba de carga y Web")

    *Crear un nuevo proyecto de prueba de carga y Web*
3. En **WebTest1. WebTest** , haga clic con el botón secundario en el nodo **WebTest1** y haga clic en **Agregar solicitud**.

    ![Agregar una solicitud a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Agregar una solicitud a WebTest1")

    *Agregar una solicitud a WebTest1*
4. En la ventana **propiedades** del nuevo nodo solicitud, actualice la propiedad **dirección URL** para que apunte a la dirección URL de la aplicación web (por ejemplo, *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Cambiar la propiedad de dirección URL](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Cambiar la propiedad de dirección URL")

    *Cambiar la propiedad de dirección URL*
5. En la ventana **WebTest1. WebTest** , haga clic con el botón secundario en **WebTest1** y haga clic en **Agregar bucle..** ..

    ![Agregar un bucle a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Agregar un bucle a WebTest1")

    *Agregar un bucle a WebTest1*
6. En el cuadro de diálogo **Agregar regla condicional y elementos al bucle** , seleccione la regla **de bucle for** y modifique las siguientes propiedades.

   1. **Valor de terminación:** 1000
   2. **Nombre del parámetro de contexto:** Apunta
   3. **Valor de incremento:** 1

      ![Seleccionar la regla de bucle for y actualizar las propiedades](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Seleccionar la regla de bucle for y actualizar las propiedades")

      *Seleccionar la regla de bucle for y actualizar las propiedades*
7. En la sección **elementos en bucle** , seleccione la solicitud que creó anteriormente para que sea el primer y el último elemento del bucle. Haga clic en **Aceptar** para continuar.

    ![Seleccionar el primer y el último elemento del bucle](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Seleccionar el primer y el último elemento del bucle")

    *Seleccionar el primer y el último elemento del bucle*
8. En **Explorador de soluciones**, haga clic con el botón secundario en el proyecto **WebAndLoadTestProject** , expanda el menú **Agregar** y seleccione **prueba de carga..** ..

    ![Agregar una prueba de carga al proyecto WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Agregar una prueba de carga al proyecto WebAndLoadTestProject")

    *Agregar una prueba de carga al proyecto WebAndLoadTestProject*
9. En el cuadro de diálogo **nuevo Asistente para prueba de carga** , haga clic en **siguiente**.

    ![Nuevo Asistente para prueba de carga](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Nuevo Asistente para prueba de carga")

    *Nuevo Asistente para prueba de carga*
10. En la página **escenario** , seleccione no **usar tiempos de reflexión** y haga clic en **siguiente**.

    ![Seleccionar no usar tiempos de reflexión](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Seleccionar no usar tiempos de reflexión")

    *Seleccionar no usar tiempos de reflexión*
11. En la página **modelo de carga** , asegúrese de que la opción **carga constante** está seleccionada. Cambie la configuración de **recuento de usuarios** a **250** usuarios y haga clic en **siguiente**.

    ![Cambiar el número de usuarios a 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Cambiar el número de usuarios a 250")

    *Cambiar el número de usuarios a 250*
12. En la página **modelo de combinación de pruebas** , seleccione **basado en el orden de pruebas secuencial** y haga clic en **siguiente**.

    ![Seleccionar el modelo de combinación de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Seleccionar el modelo de combinación de pruebas")

    *Seleccionar el modelo de combinación de pruebas*
13. En la página **modelo de combinación de pruebas** , haga clic en **Agregar...** para agregar una prueba a la combinación.

    ![Agregar una prueba a la combinación de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Agregar una prueba a la combinación de pruebas")

    *Agregar una prueba a la combinación de pruebas*
14. En el cuadro de diálogo **Agregar pruebas** , haga doble clic en **WebTest1** para agregar la prueba a la lista de **pruebas seleccionadas** . Haga clic en **Aceptar** para continuar.

    ![Agregar la prueba WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Agregar la prueba WebTest1")

    *Agregar la prueba WebTest1*
15. En la página **combinación de pruebas** , haga clic en **siguiente**.

    ![Completar la página de combinación de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completar la página de combinación de pruebas")

    *Completar la página de combinación de pruebas*
16. En la página **combinación de redes** , haga clic en **siguiente**.

    ![Hacer clic en siguiente en la página combinación de redes](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Hacer clic en siguiente en la página combinación de redes")

    *Hacer clic en siguiente en la página combinación de redes*
17. En la página **combinación de exploradores** , seleccione **Internet Explorer 10,0** como tipo de explorador y haga clic en **siguiente**.

    ![Seleccionar el tipo de explorador](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Seleccionar el tipo de explorador")

    *Seleccionar el tipo de explorador*
18. En la página **conjuntos de contadores** , haga clic en **siguiente**.

    ![Hacer clic en siguiente en la página conjuntos de contadores](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Hacer clic en siguiente en la página conjuntos de contadores")

    *Hacer clic en siguiente en la página conjuntos de contadores*
19. En la página **configuración de ejecución** , establezca la duración de la prueba de **carga** en **5 minutos** y haga clic en **Finalizar**.

    ![Establecer la duración de la prueba de carga en 5 minutos](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Establecer la duración de la prueba de carga en 5 minutos")

    *Establecer la duración de la prueba de carga en 5 minutos*
20. En **Explorador de soluciones**, haga doble clic en el archivo **local. Settings** para explorar la configuración de pruebas. De forma predeterminada, Visual Studio usa el equipo local para ejecutar las pruebas.

    > [!NOTE]
    > Como alternativa, puede configurar el proyecto de prueba para ejecutar las pruebas de carga en la nube mediante **Azure Test Plans**. Azure Test Plans proporciona un servicio de pruebas de carga basado en la nube que simula una carga más realista, evitando las restricciones del entorno local, como la capacidad de la CPU, la memoria disponible y el ancho de banda de red. Para obtener más información sobre el uso de Azure Test Plans para ejecutar pruebas de carga, vea [escenarios de pruebas de carga](/azure/devops/test/load-test/overview?view=vsts).

    ![Configuración de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Tarea 3: comprobación de escalado automático

Ahora ejecutará la prueba de carga que creó en la tarea anterior y verá cómo se comporta la aplicación web en carga.

1. En **Explorador de soluciones**, haga doble clic en **LoadTest1. LoadTest** para abrir la prueba de carga.

    ![Abrir LoadTest1. LoadTest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Abrir LoadTest1. LoadTest")

    *Abrir LoadTest1. LoadTest*
2. En la ventana **LoadTest1. LoadTest** , haga clic en el primer botón del cuadro de herramientas para ejecutar la prueba de carga.

    ![Ejecutar la prueba de carga](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Ejecutar la prueba de carga")

    *Ejecutar la prueba de carga*
3. Espere hasta que se complete la prueba de carga.

    > [!NOTE]
    > La prueba de carga simula varios usuarios que envían solicitudes a la aplicación web simultáneamente. Cuando la prueba se está ejecutando, puede supervisar los contadores disponibles para detectar los errores, las advertencias u otra información relacionada con la ejecución de la prueba de carga.

    ![Prueba de carga en ejecución](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Esperando a que se complete la prueba de carga")

    *Prueba de carga en ejecución*
4. Una vez completada la prueba, vuelva al portal de administración y navegue hasta la página **escala** de la aplicación Web. En la sección **Capacity (capacidad** ), debería ver en el gráfico que se ha implementado automáticamente una nueva instancia.

    ![Nueva instancia implementada automáticamente](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nueva instancia implementada automáticamente*

    > [!NOTE]
    > Los cambios pueden tardar varios minutos en aparecer en el gráfico (presione **Ctrl + F5** periódicamente para actualizar la página). Si no ve ningún cambio, puede intentar lo siguiente:
    >
    > - Aumentar la duración de la prueba de carga (por ejemplo, **10 minutos**)
    > - Reducir los valores máximo y mínimo del intervalo de **CPU de destino** en la configuración de escalado automático de la aplicación Web
    > - Ejecute la prueba de carga en la nube con **Azure Test Plans**. Puede encontrar más información [aquí](/azure/devops/test/load-test/index?view=vsts)

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido a configurar e implementar la aplicación en aplicaciones Web de producción en Azure. Inició la detección y la actualización de las bases de datos mediante **migraciones de Entity Framework Code First**y, después, continuó implementando nuevas versiones del sitio con **git** y realizando reversiones a la versión estable más reciente de su sitio. Además, ha aprendido a escalar la aplicación mediante el almacenamiento para trasladar el contenido estático a un contenedor de blobs.
