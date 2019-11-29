---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Depurar procedimientos almacenadosC#() | Microsoft Docs
author: rick-anderson
description: Las ediciones Visual Studio Professional y Team System permiten establecer puntos de interrupción y depurar los procedimientos almacenados en SQL Server, lo que hace que se almacene la depuración...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 12a8500b107345b9cc9ab457016fdef09ca1bb9d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74604788"
---
# <a name="debugging-stored-procedures-c"></a>Depurar procedimientos almacenados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) o [Descargar PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Las ediciones Visual Studio Professional y Team System permiten establecer puntos de interrupción y depurar los procedimientos almacenados en SQL Server, lo que hace que la depuración de procedimientos almacenados sea tan sencilla como la depuración del código de la aplicación. En este tutorial se muestra la depuración directa de la base de datos y la depuración de aplicaciones de procedimientos almacenados.

## <a name="introduction"></a>Introducción

Visual Studio proporciona una experiencia de depuración enriquecida. Con unas cuantas pulsaciones de tecla o clics del mouse, es posible usar puntos de interrupción para detener la ejecución de un programa y examinar su estado y el flujo de control. Junto con la depuración de código de aplicación, Visual Studio ofrece compatibilidad para la depuración de procedimientos almacenados desde SQL Server. Al igual que los puntos de interrupción se pueden establecer dentro del código de una clase de código subyacente de ASP.NET o clase de capa de lógica de negocios, también se pueden colocar en procedimientos almacenados.

En este tutorial veremos cómo entrar en procedimientos almacenados desde el Explorador de servidores dentro de Visual Studio y cómo establecer puntos de interrupción que se alcanzan cuando se llama al procedimiento almacenado desde la aplicación ASP.NET en ejecución.

> [!NOTE]
> Desafortunadamente, los procedimientos almacenados solo se pueden recorrer y depurar a través de las versiones Professional y Team Systems de Visual Studio. Si usa Visual Web Developer o la versión estándar de Visual Studio, le agradecemos que lea los pasos necesarios para depurar los procedimientos almacenados, pero no podrá replicar estos pasos en la máquina.

## <a name="sql-server-debugging-concepts"></a>Conceptos de depuración SQL Server

Microsoft SQL Server 2005 se diseñó para proporcionar integración con [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), que es el tiempo de ejecución que usan todos los ensamblados .net. Por consiguiente, SQL Server 2005 admite objetos de base de datos administrados. Es decir, puede crear objetos de base de datos como procedimientos almacenados y funciones definidas por el usuario (UDF) como C# métodos en una clase. Esto permite que estos procedimientos almacenados y UDF utilicen la funcionalidad de la .NET Framework y de sus propias clases personalizadas. Por supuesto, SQL Server 2005 también proporciona compatibilidad con los objetos de base de datos T-SQL.

SQL Server 2005 ofrece compatibilidad para la depuración de T-SQL y objetos de base de datos administrados. Sin embargo, estos objetos solo se pueden depurar mediante las ediciones de Visual Studio 2005 Professional y Team Systems. En este tutorial se examinará la depuración de objetos de base de datos T-SQL. En el tutorial siguiente se examina la depuración de objetos de base de datos administrados.

La [Introducción a la depuración de T-SQL y CLR en SQL Server](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrada de blog de 2005 del [equipo de integración de clr de SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) destaca las tres maneras de depurar objetos de SQL Server 2005 desde Visual Studio:

- **Depuración directa de la base de datos (DDD)** : desde explorador de servidores podemos entrar en cualquier objeto de base de datos T-SQL, como procedimientos almacenados y UDF. Examinaremos el DDD en el paso 1.
- **Depuración de aplicaciones** : podemos establecer puntos de interrupción en un objeto de base de datos y, a continuación, ejecutar la aplicación ASP.net. Cuando se ejecute el objeto de base de datos, se alcanzará el punto de interrupción y se activará el control al depurador. Tenga en cuenta que con la depuración de aplicaciones no se puede entrar en un objeto de base de datos desde el código de la aplicación. Se deben establecer explícitamente puntos de interrupción en esos procedimientos almacenados o UDF en los que se desea que el depurador se detenga. La depuración de aplicaciones se examina a partir del paso 2.
- La **depuración desde un SQL Server proyecto** Visual Studio Professional y las ediciones de Team Systems incluyen un tipo de proyecto SQL Server que se usa normalmente para crear objetos de base de datos administrados. Examinaremos el uso de proyectos de SQL Server y depurarán su contenido en el siguiente tutorial.

Visual Studio puede depurar procedimientos almacenados en instancias de SQL Server locales y remotas. Una instancia de SQL Server local es aquella que se instala en el mismo equipo que Visual Studio. Si el SQL Server base de datos que está usando no se encuentra en el equipo de desarrollo, se considera una instancia remota. Para estos tutoriales, se han usado instancias locales de SQL Server. La depuración de procedimientos almacenados en una instancia remota de SQL Server requiere más pasos de configuración que al depurar procedimientos almacenados en una instancia local.

Si usa una instancia de SQL Server local, puede comenzar con el paso 1 y trabajar en este tutorial hasta el final. Sin embargo, si usa una instancia de SQL Server remota, primero deberá asegurarse de que al depurar está registrado en el equipo de desarrollo con una cuenta de usuario de Windows que tiene un inicio de sesión SQL Server en la instancia remota. Además, tanto este inicio de sesión de base de datos como el inicio de sesión de base de datos usado para conectarse a la base de datos desde la aplicación ASP.NET en ejecución deben ser miembros del rol `sysadmin`. Vea la sección depurar objetos de T-SQL Database en instancias remotas al final de este tutorial para obtener más información sobre la configuración de Visual Studio y SQL Server para depurar una instancia remota.

Por último, comprenda que la compatibilidad con la depuración de objetos de base de datos T-SQL no es tan enriquecida como la compatibilidad con la depuración para aplicaciones .NET. Por ejemplo, las condiciones del punto de interrupción y los filtros no se admiten, solo hay disponible un subconjunto de las ventanas de depuración, no puede usar editar y continuar, la ventana inmediato se representa como inútil, etc. Para obtener más información [, vea limitaciones de los comandos y las características del depurador](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) .

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Paso 1: ejecutar paso a paso un procedimiento almacenado directamente

Visual Studio facilita la depuración directa de un objeto de base de datos. Echemos un vistazo a cómo usar la característica de depuración directa de la base de datos (DDD) para entrar en el `Products_SelectByCategoryID` procedimiento almacenado en la base de datos Northwind. Como su nombre implica, `Products_SelectByCategoryID` devuelve información del producto de una categoría determinada; se creó en el tutorial [uso de procedimientos almacenados existentes para el conjunto de objetos de tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . Para empezar, vaya a la Explorador de servidores y expanda el nodo de base de datos Northwind. Después, explore en profundidad la carpeta procedimientos almacenados, haga clic con el botón secundario en el `Products_SelectByCategoryID` procedimiento almacenado y elija la opción ir a procedimiento almacenado del menú contextual. Se iniciará el depurador.

Dado que el `Products_SelectByCategoryID` procedimiento almacenado espera un parámetro de entrada `@CategoryID`, se le pedirá que proporcione este valor. Escriba 1, que devolverá información sobre las bebidas.

![Use el valor 1 para el parámetro @CategoryID](debugging-stored-procedures-cs/_static/image1.png)

**Figura 1**: uso del valor 1 para el parámetro `@CategoryID`

Después de proporcionar el valor para el parámetro `@CategoryID`, se ejecuta el procedimiento almacenado. Sin embargo, en lugar de ejecutar hasta completarse, el depurador detiene la ejecución en la primera instrucción. Observe la flecha amarilla en el margen, que indica la ubicación actual en el procedimiento almacenado. Puede ver y modificar los valores de los parámetros mediante el ventana Inspección o desplazando el puntero sobre el nombre del parámetro en el procedimiento almacenado.

[![el depurador se ha detenido en la primera instrucción del procedimiento almacenado](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Figura 2**: el depurador se ha detenido en la primera instrucción del procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](debugging-stored-procedures-cs/_static/image4.png))

Para recorrer paso a paso el procedimiento almacenado una instrucción cada vez, haga clic en el botón paso a paso por procedimientos en la barra de herramientas o presione la tecla F10. El `Products_SelectByCategoryID` procedimiento almacenado contiene una sola instrucción de `SELECT`, por lo que al presionar F10 se devolverá paso a paso por procedimientos la instrucción única y finalizará la ejecución del procedimiento almacenado. Una vez completado el procedimiento almacenado, el resultado aparecerá en la ventana de salida y el depurador finalizará.

> [!NOTE]
> La depuración de T-SQL se produce en el nivel de instrucción; no se puede entrar en una instrucción `SELECT`.

## <a name="step-2-configuring-the-website-for-application-debugging"></a>Paso 2: configurar el sitio web para la depuración de aplicaciones

Aunque la depuración de un procedimiento almacenado directamente desde el Explorador de servidores es útil, en muchos escenarios estamos más interesados en depurar el procedimiento almacenado cuando se llama desde nuestra aplicación ASP.NET. Podemos agregar puntos de interrupción a un procedimiento almacenado desde dentro de Visual Studio y, a continuación, iniciar la depuración de la aplicación ASP.NET. Cuando se invoca un procedimiento almacenado con puntos de interrupción desde la aplicación, la ejecución se detendrá en el punto de interrupción y podremos ver y cambiar los valores de los parámetros de los procedimientos almacenados y recorrer sus instrucciones, tal como hicimos en el paso 1.

Antes de poder empezar a depurar los procedimientos almacenados a los que se llama desde la aplicación, es necesario indicar a la aplicación Web de ASP.NET que se integre con el depurador de SQL Server. Para empezar, haga clic con el botón derecho en el nombre del sitio web en el Explorador de soluciones (`ASPNET_Data_Tutorial_74_CS`). Elija la opción páginas de propiedades en el menú contextual, seleccione el elemento opciones de inicio de la izquierda y active la casilla SQL Server en la sección depuradores (vea la figura 3).

[![Active la casilla SQL Server en las páginas de propiedades de la aplicación](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Figura 3**: Active la casilla SQL Server en las páginas de propiedades de la aplicación ([haga clic para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image7.png))

Además, es necesario actualizar la cadena de conexión de base de datos utilizada por la aplicación para que la agrupación de conexiones esté deshabilitada. Cuando se cierra una conexión a una base de datos, el objeto de `SqlConnection` correspondiente se coloca en un grupo de conexiones disponibles. Al establecer una conexión con una base de datos, se puede recuperar un objeto de conexión disponible desde este grupo en lugar de tener que crear y establecer una nueva conexión. Esta agrupación de objetos de conexión es una mejora del rendimiento y está habilitada de forma predeterminada. Sin embargo, al depurar, queremos desactivar la agrupación de conexiones porque la infraestructura de depuración no se restablece correctamente cuando se trabaja con una conexión tomada del grupo.

Para deshabilitar la agrupación de conexiones, actualice el `NORTHWNDConnectionString` en `Web.config` para que incluya la configuración `Pooling=false`.

[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Una vez que haya terminado de depurar SQL Server a través de la aplicación ASP.NET, asegúrese de restablecer la agrupación de conexiones quitando el valor `Pooling` de la cadena de conexión (o estableciéndolo en `Pooling=true`).

En este momento, la aplicación ASP.NET se ha configurado para permitir que Visual Studio depure SQL Server objetos de base de datos cuando se invoca a través de la aplicación Web. Lo único que queda ahora es agregar un punto de interrupción a un procedimiento almacenado e iniciar la depuración.

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Paso 3: agregar un punto de interrupción y depuración

Abra el `Products_SelectByCategoryID` procedimiento almacenado y establezca un punto de interrupción al principio de la instrucción de `SELECT` haciendo clic en el margen en el lugar adecuado o colocando el cursor al principio de la instrucción `SELECT` y presionando F9. Como se muestra en la figura 4, el punto de interrupción aparece como un círculo rojo en el margen.

[![establecer un punto de interrupción en el procedimiento almacenado de Products_SelectByCategoryID](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Figura 4**: establecimiento de un punto de interrupción en el `Products_SelectByCategoryID` procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](debugging-stored-procedures-cs/_static/image10.png))

Para que un objeto de SQL Database se depure a través de una aplicación cliente, es imperativo que la base de datos esté configurada para admitir la depuración de aplicaciones. La primera vez que se establece un punto de interrupción, este valor se debe activar automáticamente, pero es prudente hacer doble comprobación. Haga clic con el botón secundario en el nodo `NORTHWND.MDF` en el Explorador de servidores. El menú contextual debe incluir un elemento de menú activado denominado depuración de aplicaciones.

![Asegúrese de que la opción de depuración de la aplicación está habilitada](debugging-stored-procedures-cs/_static/image11.png)

**Figura 5**: comprobación de que la opción de depuración de la aplicación está habilitada

Con el punto de interrupción establecido y la opción de depuración de la aplicación habilitada, estamos preparados para depurar el procedimiento almacenado cuando se llama desde la aplicación ASP.NET. Inicie el depurador; para ello, vaya al menú Depurar, elija iniciar depuración, presione F5 o haga clic en el icono de reproducción verde en la barra de herramientas. Se iniciará el depurador y se iniciará el sitio Web.

El procedimiento almacenado de `Products_SelectByCategoryID` se ha creado en el tutorial [uso de procedimientos almacenados existentes para el conjunto de objetos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) del mismo tipo. Su página web correspondiente (`~/AdvancedDAL/ExistingSprocs.aspx`) contiene un control GridView que muestra los resultados devueltos por este procedimiento almacenado. Visite esta página a través del explorador. Al llegar a la página, se alcanzará el punto de interrupción en el `Products_SelectByCategoryID` procedimiento almacenado y se devolverá el control a Visual Studio. Al igual que en el paso 1, puede recorrer paso a paso las instrucciones del procedimiento almacenado y ver y modificar los valores de los parámetros.

[![la página ExistingSprocs. aspx muestra inicialmente las bebidas](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Figura 6**: la página `ExistingSprocs.aspx` muestra inicialmente las bebidas ([haga clic para ver la imagen de tamaño completo](debugging-stored-procedures-cs/_static/image14.png))

[![se ha alcanzado el punto de interrupción del procedimiento almacenado](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Figura 7**: se ha alcanzado el punto de interrupción del procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](debugging-stored-procedures-cs/_static/image17.png))

Como se muestra en la ventana Inspección de la figura 7, el valor del parámetro `@CategoryID` es 1. Esto se debe a que la página `ExistingSprocs.aspx` muestra inicialmente los productos de la categoría Beverages, que tiene un valor `CategoryID` de 1. Elija una categoría diferente de la lista desplegable. Si lo hace, se producirá un postback y se volverá a ejecutar el `Products_SelectByCategoryID` procedimiento almacenado. El punto de interrupción se vuelve a visitar, pero esta vez el valor de `@CategoryID` parámetro s refleja el elemento de lista desplegable seleccionado s `CategoryID`.

[![elegir una categoría diferente de la lista desplegable](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Figura 8**: elija una categoría diferente de la lista desplegable ([haga clic para ver la imagen de tamaño completo](debugging-stored-procedures-cs/_static/image20.png))

[![el parámetro @CategoryID refleja la categoría seleccionada en la página web](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Figura 9**: el parámetro `@CategoryID` refleja la categoría seleccionada en la página web ([haga clic para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image23.png))

> [!NOTE]
> Si no se alcanza el punto de interrupción en el `Products_SelectByCategoryID` procedimiento almacenado al visitar la página de `ExistingSprocs.aspx`, asegúrese de que se ha activado la casilla de verificación SQL Server en la sección depuradores de la página de propiedades de la aplicación ASP.NET, que se ha deshabilitado la agrupación de conexiones y que la opción de depuración de aplicaciones de la base de datos está habilitada. Si sigue teniendo problemas, reinicie Visual Studio y vuelva a intentarlo.

## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Depurar objetos de SQL Database T en instancias remotas

La depuración de objetos de base de datos a través de Visual Studio es bastante sencilla cuando la instancia de base de datos de SQL Server está en el mismo equipo que Visual Studio. Sin embargo, si SQL Server y Visual Studio residen en equipos diferentes, es necesario realizar algunas configuraciones cuidadosas para que todo funcione correctamente. Hay dos tareas principales a las que nos enfrentamos:

- Asegúrese de que el inicio de sesión utilizado para conectarse a la base de datos a través de ADO.NET pertenece al rol `sysadmin`.
- Asegúrese de que la cuenta de usuario de Windows usada por Visual Studio en el equipo de desarrollo sea una cuenta de inicio de sesión SQL Server válida que pertenezca al rol de `sysadmin`.

El primer paso es relativamente sencillo. En primer lugar, identifique la cuenta de usuario utilizada para conectarse a la base de datos desde la aplicación ASP.NET y, a continuación, desde SQL Server Management Studio, agregue esa cuenta de inicio de sesión al rol de `sysadmin`.

La segunda tarea requiere que la cuenta de usuario de Windows que se usa para depurar la aplicación sea un inicio de sesión válido en la base de datos remota. Sin embargo, lo más probable es que la cuenta de Windows con la que inició sesión en la estación de trabajo no sea un inicio de sesión válido en SQL Server. En lugar de agregar su cuenta de inicio de sesión determinada a SQL Server, una opción mejor sería designar una cuenta de usuario de Windows como cuenta de depuración SQL Server. Después, para depurar los objetos de base de datos de una instancia de SQL Server remota, ejecutaría Visual Studio con las credenciales de la cuenta de inicio de sesión de Windows.

Un ejemplo le ayudará a aclarar las cosas. Imagine que hay una cuenta de Windows denominada `SQLDebug` dentro del dominio de Windows. Esta cuenta deberá agregarse a la instancia de SQL Server remota como un inicio de sesión válido y como miembro del rol de `sysadmin`. Después, para depurar la instancia de SQL Server remota desde Visual Studio, es necesario ejecutar Visual Studio como el usuario `SQLDebug`. Esto puede realizarse cerrando la sesión de la estación de trabajo, volviendo a iniciar sesión como `SQLDebug`y, a continuación, iniciando Visual Studio, pero un enfoque más sencillo sería iniciar sesión en la estación de trabajo con nuestras propias credenciales y, a continuación, usar `runas.exe` para iniciar Visual Studio como el usuario `SQLDebug`. `runas.exe` permite que una aplicación determinada se ejecute bajo el modo de crear una cuenta de usuario diferente. Para iniciar Visual Studio como `SQLDebug`, puede escribir la siguiente instrucción en la línea de comandos:

[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Para obtener una explicación más detallada sobre este proceso, vea [William R. Vaughn](http://betav.com/BLOG/billva/) s *autoestopista s Guide to Visual Studio and SQL Server, séptima Edition y* [How to: set SQL Server permissions for Debugging](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Si el equipo de desarrollo ejecuta Windows XP Service Pack 2, tendrá que configurar el Firewall de conexión a Internet para permitir la depuración remota. En el artículo sobre [Cómo habilitar la depuración de SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) se indica que esto implica dos pasos: (a) en el equipo host de Visual Studio, debe agregar `Devenv.exe` a la lista de excepciones y abrir el puerto TCP 135; y (b) en el equipo remoto (SQL), debe abrir el puerto TCP 135 y agregar `sqlservr.exe` a la lista de excepciones. Si la Directiva de dominio requiere que la comunicación de red se realice a través de IPSec, debe abrir los puertos UDP 4500 y UDP 500.

## <a name="summary"></a>Resumen

Además de proporcionar compatibilidad con la depuración para el código de aplicación .NET, Visual Studio también proporciona una variedad de opciones de depuración para SQL Server 2005. En este tutorial se han examinado dos de estas opciones: depuración directa de la base de datos y depuración de aplicaciones. Para depurar directamente un objeto de base de datos de T-SQL, busque el objeto a través de la Explorador de servidores haga clic con el botón derecho en él y elija paso a paso por instrucciones. Esto inicia el depurador y se detiene en la primera instrucción del objeto de base de datos, momento en el que puede recorrer las instrucciones Object s y ver y modificar los valores de los parámetros. En el paso 1 usamos este enfoque para entrar en el `Products_SelectByCategoryID` procedimiento almacenado.

La depuración de aplicaciones permite establecer puntos de interrupción directamente dentro de los objetos de base de datos. Cuando se invoca un objeto de base de datos con puntos de interrupción desde una aplicación cliente (por ejemplo, una aplicación Web ASP.NET), el programa se detiene a medida que el depurador toma el control. La depuración de aplicaciones es útil porque muestra más claramente qué acción de la aplicación hace que se invoque un objeto de base de datos determinado. Sin embargo, requiere un poco más configuración e instalación que la depuración directa de la base de datos.

Los objetos de base de datos también se pueden depurar a través de proyectos de SQL Server. Veremos el uso de proyectos de SQL Server y cómo usarlos para crear y depurar objetos de base de datos administrados en el siguiente tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](protecting-connection-strings-and-other-configuration-information-cs.md)
> [Siguiente](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
