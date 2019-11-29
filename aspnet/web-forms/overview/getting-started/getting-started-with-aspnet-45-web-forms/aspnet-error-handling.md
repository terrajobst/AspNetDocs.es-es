---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Control de errores de ASP.NET | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 9514142ca50b33470a3f4c033e4f8e319a9ee09b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636469"
---
# <a name="aspnet-error-handling"></a>Control de errores de ASP.NET

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial, modificará la aplicación de ejemplo Wingtip Toys para incluir el control de errores y el registro de errores. El control de errores permitirá a la aplicación controlar correctamente los errores y mostrar los mensajes de error en consecuencia. El registro de errores le permitirá encontrar y corregir los errores que se hayan producido. Este tutorial se basa en el tutorial anterior "enrutamiento de direcciones URL" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo agregar control de errores global a la configuración de la aplicación.
- Cómo agregar control de errores en los niveles de aplicación, página y código.
- Cómo registrar errores para su posterior revisión.
- Cómo mostrar mensajes de error que no pongan en peligro la seguridad.
- Cómo implementar el registro de errores de módulos y controladores de registro de errores (ELMAH).

## <a name="overview"></a>Información general del

Las aplicaciones de ASP.NET deben ser capaces de controlar los errores que se producen durante la ejecución de una manera coherente. ASP.NET usa el Common Language Runtime (CLR), que proporciona una manera de notificar a las aplicaciones de errores de una manera uniforme. Cuando se produce un error, se produce una excepción. Una excepción es cualquier error, condición o comportamiento inesperado que encuentra una aplicación.

En .NET Framework, una excepción es un objeto que hereda de la clase `System.Exception`. Una excepción se inicia desde un área del código en la que ha producido un problema. La excepción se pasa a la pila de llamadas hasta un lugar donde la aplicación proporciona el código para controlar la excepción. Si la aplicación no controla la excepción, el explorador se verá obligado a mostrar los detalles del error.

Como práctica recomendada, controle los errores en en el nivel de código de `Try`/`Catch`/bloques de `Finally` en el código. Intente colocar estos bloques para que el usuario pueda corregir los problemas en el contexto en el que se producen. Si los bloques de control de errores están demasiado lejos de donde se produjo el error, resulta más difícil proporcionar a los usuarios la información necesaria para solucionar el problema.

### <a name="exception-class"></a>Clase Exception

La clase de excepción es la clase base de la que heredan las excepciones. La mayoría de los objetos de excepción son instancias de alguna clase derivada de la clase de excepción, como la clase `SystemException`, la clase `IndexOutOfRangeException` o la clase `ArgumentNullException`. La clase de excepción tiene propiedades, como la propiedad `StackTrace`, la propiedad `InnerException` y la propiedad `Message`, que proporcionan información específica sobre el error que se ha producido.

### <a name="exception-inheritance-hierarchy"></a>Jerarquía de herencia de excepciones

El motor en tiempo de ejecución tiene un conjunto base de excepciones que derivan de la clase `SystemException` que el tiempo de ejecución produce cuando se encuentra una excepción. La mayoría de las clases que heredan de la clase de excepción, como la clase `IndexOutOfRangeException` y la clase `ArgumentNullException`, no implementan miembros adicionales. Por lo tanto, la información más importante para una excepción puede encontrarse en la jerarquía de excepciones, el nombre de la excepción y la información contenida en la excepción.

### <a name="exception-handling-hierarchy"></a>Jerarquía de control de excepciones

En una aplicación de formularios Web Forms ASP.NET, las excepciones se pueden controlar en función de una jerarquía de control específica. Una excepción se puede controlar en los niveles siguientes:

- Nivel de aplicación
- Nivel de página
- Nivel de código

Cuando una aplicación controla excepciones, a menudo se puede recuperar y mostrar al usuario información adicional sobre la excepción que se hereda de la clase de excepción. Además del nivel de aplicación, página y código, también puede controlar excepciones en el nivel de módulo HTTP y mediante un controlador personalizado de IIS.

### <a name="application-level-error-handling"></a>Control de errores en el nivel de aplicación

Puede controlar los errores predeterminados en el nivel de aplicación modificando la configuración de la aplicación o agregando un controlador de `Application_Error` en el archivo *global. asax* de la aplicación.

Puede controlar los errores predeterminados y los errores HTTP agregando una `customErrors` sección al archivo *Web. config* . La sección `customErrors` permite especificar una página predeterminada a la que se redirigirán los usuarios cuando se produzca un error. También permite especificar páginas individuales para errores de código de estado específicos.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Desafortunadamente, cuando se usa la configuración para redirigir al usuario a una página diferente, no se tienen los detalles del error que se ha producido.

Sin embargo, puede interceptar los errores que se produzcan en cualquier parte de la aplicación agregando código al controlador de `Application_Error` en el archivo *global. asax* .

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Control de eventos de error de nivel de página

Un controlador de nivel de página devuelve al usuario a la página donde se produjo el error, pero como no se mantienen las instancias de controles, ya no habrá ningún elemento en la página. Para proporcionar los detalles del error al usuario de la aplicación, debe escribir específicamente los detalles del error en la página.

Normalmente, se utiliza un controlador de errores de nivel de página para registrar errores no controlados o para llevar al usuario a una página que puede mostrar información útil.

En este ejemplo de código se muestra un controlador para el evento de error en una página web de ASP.NET. Este controlador detecta todas las excepciones que todavía no se han controlado dentro de `try`bloques de `catch` /de la página.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Después de controlar un error, debe borrarlo llamando al método `ClearError` del objeto de servidor (clase`HttpServerUtility`); de lo contrario, verá un error que se ha producido previamente.

### <a name="code-level-error-handling"></a>Control de errores en el nivel de código

La instrucción try-catch consta de un bloque try seguido de una o más cláusulas Catch, que especifican Controladores para diferentes excepciones. Cuando se produce una excepción, el Common Language Runtime (CLR) busca la instrucción catch que controla esta excepción. Si el método que se ejecuta actualmente no contiene un bloque catch, CLR examina el método que llamó al método actual, y así sucesivamente, hacia arriba en la pila de llamadas. Si no se encuentra ningún bloque catch, CLR muestra al usuario un mensaje de excepción no controlada y detiene la ejecución del programa.

En el ejemplo de código siguiente se muestra una manera común de usar `try`/`catch`/`finally` para controlar los errores.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

En el código anterior, el bloque try contiene el código que debe protegerse contra una posible excepción. El bloque se ejecuta hasta que se produce una excepción o hasta que el bloque se completa correctamente. Si se produce una excepción `FileNotFoundException` o una excepción `IOException`, la ejecución se transfiere a una página diferente. A continuación, se ejecuta el código contenido en el bloque Finally, tanto si se ha producido un error como si no.

## <a name="adding-error-logging-support"></a>Agregar compatibilidad de registro de errores

Antes de agregar el control de errores a la aplicación de ejemplo Wingtip Toys, agregará una `ExceptionUtility` clase a la carpeta *lógica* para agregar compatibilidad con el registro de errores. Al hacerlo, cada vez que la aplicación controla un error, los detalles del error se agregan al archivo de registro de errores.

1. Haga clic con el botón secundario en la carpeta *lógica* y, a continuación, seleccione **Agregar** -&gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el grupo plantillas de **código** de **Visual C#**  -&gt; a la izquierda. A continuación, seleccione **clase**en la lista central y asígnele el nombre **ExceptionUtility.CS**.
3. Haga clic en **Agregar**. Se muestra el nuevo archivo de clase.
4. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Cuando se produce una excepción, la excepción se puede escribir en un archivo de registro de excepciones llamando al método `LogException`. Este método toma dos parámetros, el objeto de excepción y una cadena que contiene los detalles sobre el origen de la excepción. El registro de excepciones se escribe en el archivo *ErrorLog. txt* de la carpeta *App\_Data* .

### <a name="adding-an-error-page"></a>Agregar una página de error

En la aplicación de ejemplo Wingtip Toys, se usará una página para mostrar los errores. La página de error está diseñada para mostrar un mensaje de error seguro a los usuarios del sitio. Sin embargo, si el usuario es un desarrollador que realiza una solicitud HTTP que se atiende localmente en el equipo en el que reside el código, se mostrarán más detalles sobre el error en la página de error.

1. Haga clic con el botón derecho en el nombre del proyecto (**Wingtip Toys**) en **Explorador de soluciones** y seleccione **Agregar** -&gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el grupo plantillas **Web** de **Visual C#**  -&gt; a la izquierda. En la lista central, seleccione **Web Forms con página maestra**y asígnele el nombre **ErrorPage. aspx**.
3. Haga clic en **Agregar**.
4. Seleccione el archivo *site. Master* como la página maestra y, a continuación, elija **Aceptar**.
5. Reemplace el marcado existente por lo siguiente:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Reemplace el código existente del código subyacente (*ErrorPage.aspx.CS*) para que aparezca de la manera siguiente:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Cuando se muestra la página de error, se ejecuta el controlador de eventos `Page_Load`. En el controlador de `Page_Load`, se determina la ubicación donde se controló el error por primera vez. Después, el último error que se produjo se determina mediante una llamada al método `GetLastError` del objeto de servidor. Si la excepción ya no existe, se crea una excepción genérica. A continuación, si la solicitud HTTP se realizó localmente, se muestran todos los detalles del error. En este caso, solo el equipo local que ejecuta la aplicación web verá estos detalles del error. Una vez que se muestra la información del error, el error se agrega al archivo de registro y el error se borra del servidor.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Mostrar mensajes de error no controlados para la aplicación

Al agregar una sección `customErrors` al archivo *Web. config* , puede controlar rápidamente los errores sencillos que se producen en la aplicación. También puede especificar cómo controlar los errores en función de su valor de código de estado, como 404-archivo no encontrado.

#### <a name="update-the-configuration"></a>Actualización de la configuración

Actualice la configuración agregando una `customErrors` sección al archivo *Web. config* .

1. En **Explorador de soluciones**, busque y abra el archivo *Web. config* en la raíz de la aplicación de ejemplo Wingtip Toys.
2. Agregue la sección `customErrors` al archivo *Web. config* dentro del nodo `<system.web>` como se indica a continuación:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Guarde el archivo *Web. config* .

En la sección `customErrors` se especifica el modo, que se establece en "ON". También especifica el `defaultRedirect`, que indica a la aplicación a qué página se debe navegar cuando se produce un error. Además, se ha agregado un elemento error específico que especifica cómo controlar un error 404 cuando no se encuentra una página. Más adelante en este tutorial, agregará un control de errores adicional que capturará los detalles de un error en el nivel de aplicación.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver las rutas actualizadas.

1. Presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 El explorador se abre y muestra la página *default. aspx* .
2. Escriba la siguiente dirección URL en el explorador (Asegúrese de usar el número **de** puerto):  
    `https://localhost:44300/NoPage.aspx`
3. Revise el de *ErrorPage. aspx* mostrado en el explorador. 

    ![Control de errores de ASP.NET: error de página no encontrada](aspnet-error-handling/_static/image1.png)

Cuando solicite la página *nopage. aspx* , que no existe, la página de error mostrará el mensaje de error simple y la información detallada del error si hay más detalles disponibles. Sin embargo, si el usuario solicitó una página no existente desde una ubicación remota, la página de error solo mostraría el mensaje de error en rojo.

### <a name="including-an-exception-for-testing-purposes"></a>Incluir una excepción con fines de prueba

Para comprobar cómo funcionará la aplicación cuando se produce un error, puede crear deliberadamente condiciones de error en ASP.NET. En la aplicación de ejemplo Wingtip Toys, iniciará una excepción de prueba cuando se cargue la página predeterminada para ver lo que ocurre.

1. Abra el código subyacente de la página *default. aspx* en Visual Studio.   
   Se mostrará la página de código subyacente de *default.aspx.CS* .
2. En el controlador de `Page_Load`, agregue el código para que el controlador aparezca como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Es posible crear distintos tipos de excepciones. En el código anterior, está creando un `InvalidOperationException` cuando se carga la página *default. aspx* .

#### <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación para ver cómo la aplicación controla la excepción.

1. Presione **Ctrl + F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 La aplicación produce la excepción InvalidOperationException. 

    > [!NOTE] 
    > 
    > Debe presionar **Ctrl + F5** para mostrar la página sin entrar en el código para ver el origen del error en Visual Studio.
2. Revise el de *ErrorPage. aspx* mostrado en el explorador. 

    ![Control de errores de ASP.NET: Página error](aspnet-error-handling/_static/image2.png)

Como puede ver en los detalles del error, la excepción se ha capturado en la sección `customError` en el archivo *Web. config* .

### <a name="adding-application-level-error-handling"></a>Agregar el control de errores en el nivel de aplicación

En lugar de interceptar la excepción mediante la sección `customErrors` en el archivo *Web. config* , donde se obtiene poca información sobre la excepción, se puede interceptar el error en el nivel de aplicación y recuperar los detalles del error.

1. En **Explorador de soluciones**, busque y abra el archivo *global.asax.CS* .
2. Agregue un controlador de **errores de\_de aplicaciones** para que aparezca de la manera siguiente:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Cuando se produce un error en la aplicación, se llama al controlador de `Application_Error`. En este controlador, se recupera y revisa la última excepción. Si no se controló la excepción y la excepción contiene detalles de la excepción interna (es decir, `InnerException` no es null), la aplicación transfiere la ejecución a la página de error donde se muestran los detalles de la excepción.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación para ver los detalles de error adicionales que se proporcionan mediante el control de la excepción en el nivel de aplicación.

1. Presione **Ctrl + F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 La aplicación produce el `InvalidOperationException`.
2. Revise el de *ErrorPage. aspx* mostrado en el explorador. 

    ![Control de errores de ASP.NET: error de nivel de aplicación](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Agregar control de errores en el nivel de página

Puede Agregar un control de errores en el nivel de página a una página, ya sea mediante la adición de un atributo `ErrorPage` a la Directiva `@Page` de la página, o bien agregando un controlador de eventos `Page_Error` al código subyacente de una página. En esta sección, agregará un controlador de eventos `Page_Error` que transferirá la ejecución a la página *ErrorPage. aspx* .

1. En **Explorador de soluciones**, busque y abra el archivo *default.aspx.CS* .
2. Agregue un controlador de `Page_Error` para que el código subyacente aparezca de la manera siguiente:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Cuando se produce un error en la página, se llama al controlador de eventos `Page_Error`. En este controlador, se recupera y revisa la última excepción. Si se produce un `InvalidOperationException`, el controlador de eventos `Page_Error` transfiere la ejecución a la página de error donde se muestran los detalles de la excepción.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Ahora puede ejecutar la aplicación para ver las rutas actualizadas.

1. Presione **Ctrl + F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 La aplicación produce el `InvalidOperationException`.
2. Revise el de *ErrorPage. aspx* mostrado en el explorador. 

    ![Control de errores de ASP.NET: error de nivel de página](aspnet-error-handling/_static/image4.png)
3. Cierre la ventana del explorador.

### <a name="removing-the-exception-used-for-testing"></a>Quitar la excepción usada para las pruebas

Para permitir que la aplicación de ejemplo Wingtip Toys funcione sin iniciar la excepción que agregó anteriormente en este tutorial, quite la excepción.

1. Abra el código subyacente de la página *default. aspx* .
2. En el controlador de `Page_Load`, quite el código que produce la excepción para que el controlador aparezca de la manera siguiente:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Agregar registro de errores de nivel de código

Como se mencionó anteriormente en este tutorial, puede Agregar instrucciones try/catch para intentar ejecutar una sección de código y controlar el primer error que se produce. En este ejemplo, solo escribirá los detalles del error en el archivo de registro de errores para que se pueda revisar el error más adelante.

1. En **Explorador de soluciones**, en la carpeta *lógica* , busque y abra el archivo *PayPalFunctions.CS* .
2. Actualice el método de `HttpCall` para que el código aparezca como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

El código anterior llama al método `LogException` incluido en la clase `ExceptionUtility`. Anteriormente en este tutorial, ha agregado el archivo de clase *ExceptionUtility.CS* a la carpeta *lógica* . El método `LogException` toma dos parámetros. El primer parámetro es el objeto de excepción. El segundo parámetro es una cadena que se usa para reconocer el origen del error.

### <a name="inspecting-the-error-logging-information"></a>Inspeccionar la información de registro de errores

Como se mencionó anteriormente, puede usar el registro de errores para determinar qué errores de la aplicación deben corregirse primero. Por supuesto, solo se registrarán los errores que se hayan capturado y escrito en el registro de errores.

1. En **Explorador de soluciones**, busque y abra el archivo *ErrorLog. txt* en la carpeta *App\_Data* .   
 Es posible que tenga que seleccionar la opción "**Mostrar todos los archivos**" o la opción "**Actualizar**" en la parte superior de **Explorador de soluciones** para ver el archivo *ErrorLog. txt* .
2. Revise el registro de errores que se muestra en Visual Studio: 

    ![Control de errores de ASP.NET: ErrorLog. txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Mensajes de error seguros

Es **importante tener en cuenta** que cuando la aplicación muestra mensajes de error, no debe proporcionar información que un usuario malintencionado pueda encontrar útil para atacar la aplicación. Por ejemplo, si la aplicación intenta escribir incorrectamente en una base de datos, no debería mostrar un mensaje de error que incluya el nombre de usuario que está usando. Por esta razón, se muestra al usuario un mensaje de error genérico en rojo. Todos los detalles de error adicionales solo se muestran al desarrollador en el equipo local.

## <a name="using-elmah"></a>Usar ELMAH

ELMAH (módulos de registro de errores y controladores) es una característica de registro de errores que se conecta a la aplicación ASP.NET como un paquete NuGet. ELMAH proporciona las siguientes funcionalidades:

- Registro de excepciones no controladas.
- Una página web para ver el registro completo de las excepciones no controladas que se han codificado.
- Una página web para ver los detalles completos de cada excepción registrada.
- Una notificación por correo electrónico de cada error en el momento en que se produce.
- Una fuente RSS de los últimos 15 errores del registro.

Para poder trabajar con el ELMAH, debe instalarlo. Esto es fácil con el instalador de paquetes *NuGet* . Como se mencionó anteriormente en esta serie de tutoriales, NuGet es una extensión de Visual Studio que facilita la instalación y actualización de bibliotecas y herramientas de código abierto en Visual Studio.

1. En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes Nuget** > **administrar paquetes Nuget para la solución**. 

    ![Control de errores de ASP.NET: administración de paquetes NuGet para la solución](aspnet-error-handling/_static/image6.png)
2. En Visual Studio se muestra el cuadro de diálogo **administrar paquetes NuGet** .
3. En el cuadro de diálogo **administrar paquetes NuGet** , expanda en **línea** a la izquierda y, a continuación, seleccione **Nuget.org**. A continuación, busque e instale el paquete **ELMAH** en la lista de paquetes disponibles en línea. 

    ![Control de errores de ASP.NET: paquete de NuGet de ELMA](aspnet-error-handling/_static/image7.png)
4. Deberá tener una conexión a Internet para descargar el paquete.
5. En el cuadro de diálogo **seleccionar proyectos** , asegúrese de que está seleccionada la selección **WingtipToys** y, a continuación, haga clic en **Aceptar**. 

    ![Control de errores de ASP.NET: cuadro de diálogo Seleccionar proyectos](aspnet-error-handling/_static/image8.png)
6. Haga clic en **cerrar** en el cuadro de diálogo **administrar paquetes NuGet** si es necesario.
7. Si Visual Studio solicita que vuelva a cargar los archivos abiertos, seleccione "**sí a todo**".
8. El paquete ELMAH agrega entradas para sí misma en el archivo *Web. config* en la raíz del proyecto. Si Visual Studio le pregunta si desea recargar el archivo *Web. config* modificado, haga clic en **sí**.

ELMAH ya está listo para almacenar los errores no controlados que se producen.

### <a name="viewing-the-elmah-log"></a>Visualización del registro ELMAH

Ver el registro ELMAH es fácil, pero primero creará una excepción no controlada que se registrará en el registro ELMAH.

1. Presione **Ctrl + F5** para ejecutar la aplicación de ejemplo Wingtip Toys.
2. Para escribir una excepción no controlada en el registro ELMAH, navegue en el explorador hasta la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/NoPage.aspx` se mostrará la página de error.
3. Para mostrar el registro ELMAH, navegue por el explorador hasta la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/elmah.axd`

    ![Control de errores de ASP.NET: registro de errores de ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a controlar los errores en el nivel de aplicación, en el nivel de página y en el nivel de código. También ha aprendido a registrar errores administrados y no controlados para su posterior revisión. Ha agregado la utilidad ELMAH para proporcionar el registro de excepciones y la notificación a la aplicación mediante NuGet. Además, ha aprendido la importancia de los mensajes de error seguros.

## <a name="tutorial-series-conclusion"></a>Conclusión de la serie de tutoriales

Gracias por lo siguiente. Espero que este conjunto de tutoriales le haya ayudado a familiarizarse con los formularios Web Forms de ASP.NET. Si necesita más información sobre las características de formularios Web Forms disponibles en ASP.NET 4,5 y Visual Studio 2013, consulte [ASP.net and Web Tools para obtener Visual Studio 2013 notas de la versión](../../../../visual-studio/overview/2013/release-notes.md). Además, asegúrese de echar un vistazo al tutorial mencionado en la sección **pasos siguientes** y Defintely probar la [evaluación gratuita de Azure](https://azure.microsoft.com/pricing/free-trial/).

![Agradecimiento-Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre la implementación de la aplicación web en Microsoft Azure, vea [implementación de una aplicación de formularios Web Forms de ASP.net segura con pertenencia, OAuth y SQL Database en un sitio web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Evaluación gratuita

[Evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)  
 La publicación de su sitio web en Microsoft Azure le permitirá ahorrar tiempo, mantenimiento y gastos. Es un proceso rápido de implementación de la aplicación web en Azure. Cuando necesite mantener y supervisar la aplicación Web, Azure ofrece una variedad de herramientas y servicios. Administre datos, tráfico, identidad, copias de seguridad, mensajería, medios y rendimiento en Azure. Y todo esto se proporciona en un enfoque muy rentable.

## <a name="additional-resources"></a>Recursos adicionales

[Registrar detalles de error con la supervisión de estado de ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Agradecimientos

Me gustaría agradecer a las siguientes personas que realizaran importantes contribuciones al contenido de esta serie de tutoriales:

- [Alberto poblacion, MVP &amp; MCT, España](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex THISSEN, Países Bajos](http://blog.alexthissen.nl/) (twitter: [@alexthissen](http://twitter.com/alexthissen))
- [Andre Tournier, EE. UU.](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Eslovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasil](http://msmvps.com/blogs/bsonnino) (twitter: [@bsonnino](http://twitter.com/bsonnino))
- [Carlos dos Santos, Brasil](http://www.carloscds.net/)
- [Dave Campbell, EE. UU.](http://www.wynapse.com/) (twitter: [@windowsdevnews](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Michael sharpes, EE. UU.](http://www.930solutions.com/) (twitter: [@mrsharps](http://twitter.com/mrsharps))
- Mike Pope
- [Mitchel vendedores, EE. UU.](http://www.mitchelsellers.com/) (twitter: [@MitchelSellers](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contribuciones a la comunidad

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Ejemplo de código relacionado con Visual Studio 2012 en MSDN: [navegación Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Ejemplo de código relacionado con Visual Studio 2012 en MSDN: [serie de tutoriales de formularios Web Forms de ASP.NET 4,5 en Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo-colaborador de la audiencia técnica de Microsoft (Twitter: @driazevedo)  
  Visual Studio 2012 Translation: [iniciando com asp.net web forms 4,5-parte 1-Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [Anterior](url-routing.md)
