---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Novedades de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 es un marco de trabajo para compilar aplicaciones web escalables basadas en estándares con patrones de diseño bien establecidos y la eficacia de ASP.NET y...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 4235f4fe666cdeb7d0821127a2b349f2ff30cd6e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433675"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Novedades de ASP.NET MVC 4

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 es un marco de trabajo para compilar aplicaciones web escalables basadas en estándares con patrones de diseño bien establecidos y la eficacia de ASP.NET y .NET Framework. Esta nueva y cuarta versión del marco se centra en facilitar el desarrollo de aplicaciones web móviles.

Para empezar, cuando se crea un nuevo proyecto de ASP.NET MVC 4, ahora hay una plantilla de proyecto de aplicación móvil que se puede usar para crear una aplicación independiente específicamente para dispositivos móviles. Además, ASP.NET MVC 4 se integra con jQuery Mobile a través de un paquete NuGet jQuery. Mobile. MVC. jQuery Mobile es un marco basado en HTML5 para desarrollar aplicaciones web que son compatibles con todas las plataformas de dispositivos móviles populares, como Windows Phone, iPhone, Android, etc. Sin embargo, si necesita especialización, ASP.NET MVC 4 también le permite atender diferentes vistas para diferentes dispositivos y proporcionar optimizaciones específicas del dispositivo.

En este laboratorio práctico, comenzará con la plantilla de proyecto de ASP.NET MVC 4 &quot;&quot; aplicación de Internet para crear una aplicación de la Galería fotográfica. Mejorará progresivamente la aplicación con las nuevas características de jQuery Mobile y ASP.NET MVC 4 para que sea compatible con diferentes dispositivos móviles y exploradores Web de escritorio. También obtendrá información sobre las nuevas recetas de código para la generación de código y cómo ASP.NET MVC 4 facilita la escritura de métodos de acción asincrónicos mediante la compatibilidad con la tarea&lt;ActionResult&gt; tipos de valor devuelto.

> [!NOTE]
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en las [versiones Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). El proyecto específico de este laboratorio está disponible en [What's New in Web Forms in ASP.NET 4,5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Aproveche las mejoras realizadas en las plantillas de proyecto de ASP.NET MVC, incluida la nueva plantilla de proyecto de aplicación móvil.
- Usar el atributo de ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles
- Usar jQuery Mobile para mejoras progresivas y para compilar una interfaz de usuario Web optimizada para Touch
- Crear vistas específicas para dispositivos móviles
- Usar el componente View-Switcher para alternar entre las vistas de escritorio y móviles de la aplicación
- Crear controladores asincrónicos con la compatibilidad de tareas

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los siguientes elementos para completar este laboratorio:

- [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).
- [ASP.NET MVC 4](../../../mvc4.md) (incluido en la instalación de Microsoft Visual Studio 2012)
- Windows Phone emulador (incluido en el [SDK de Windows Phone 7.1.1](https://www.microsoft.com/download/details.aspx?id=29233))
- Opcional: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) con la extensión **Electric ciruela iPhone Simulator** (solo para el ejercicio 3 que se usa para examinar la aplicación web con un simulador de iPhone)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

En todo el documento de laboratorio, se le indicará que inserte bloques de código. Para su comodidad, la mayor parte del código se proporciona como fragmentos de Visual Studio Code, que puede usar en Visual Studio para evitar tener que agregarlo manualmente.

Para instalar los fragmentos de código:

1. Abra una ventana del explorador de Windows y vaya a la carpeta **Source\Setup** del laboratorio.
2. Haga doble clic en el archivo **setup. cmd** de esta carpeta para instalar los fragmentos de código de Visual Studio.

Si no está familiarizado con los fragmentos de Visual Studio Code y desea obtener información sobre cómo usarlos, puede consultar el apéndice de este documento &quot;[Apéndice A: usar fragmentos de código](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Nuevas plantillas de proyecto de ASP.NET MVC 4](#Exercise1)
2. [Creación de la aplicación Web de la Galería fotográfica](#Exercise2)
3. [Incorporación de compatibilidad para dispositivos móviles](#Exercise3)
4. [Usar controladores asincrónicos](#Exercise4)

> [!NOTE]
> Cada ejercicio va acompañado de una carpeta **final** que contiene la solución resultante que debe obtener después de completar los ejercicios. Puede usar esta solución como guía si necesita ayuda adicional para trabajar en los ejercicios.

Tiempo estimado para completar este laboratorio: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Ejercicio 1: nuevas plantillas de proyecto de ASP.NET MVC 4

En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4. Además de la plantilla de aplicación de Internet, que ya está presente en MVC 3, esta versión incluye ahora una plantilla independiente para las aplicaciones móviles. En primer lugar, observará algunas características relevantes de cada una de las plantillas. A continuación, trabajará en la representación correcta de la página en las distintas plataformas mediante el enfoque correcto.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Tarea 1: exploración de la plantilla de aplicación de Internet

1. Abra **Visual Studio**.
2. Seleccione el **archivo | Nuevo |** Comando de menú proyecto. En el cuadro de diálogo **nuevo proyecto** , seleccione el **Visual C# | Plantilla Web** en el árbol del panel izquierdo y elija **aplicación Web de ASP.NET MVC 4.** Asigne un nombre a la **Galería**fotode proyecto, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**.

    > [!NOTE]
    > Más adelante podrá personalizar la solución ASP.NET MVC 4 de la Galería Fotogalería que está creando ahora.

    ![Crear un nuevo proyecto](whats-new-in-aspnet-mvc-4/_static/image1.png "Crear un nuevo proyecto")

    *Crear un nuevo proyecto*
3. En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla de proyecto **aplicación de Internet** y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como motor de vista.

    ![Creación de una nueva aplicación de Internet de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image2.png "Creación de una nueva aplicación de Internet de ASP.NET MVC 4")

    *Creación de una nueva aplicación de Internet de ASP.NET MVC 4*

    > [!NOTE]
    > Sintaxis Razor se ha introducido en ASP.NET MVC 3. Su objetivo es minimizar el número de caracteres y pulsaciones de teclas necesarios en un archivo, lo que permite un flujo de trabajo de codificación rápida y fluida. Razor aprovecha los conocimientos C# de lenguaje existente o VB (u otros) y proporciona una sintaxis de marcado de plantilla que permite un flujo de trabajo de construcción de HTML impresionante.
4. Presione **F5** para ejecutar la solución y ver las plantillas renovadas. Puede consultar las siguientes características:

    **Plantillas de estilo moderno**

    Las plantillas se han renovado, lo que proporciona más estilos de aspecto moderno.

    ![Plantillas con estilo de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image3.png "Plantillas con estilo de MVC 4")

    *Plantillas con estilo de ASP.NET MVC 4*

    ![Página nuevo contacto](whats-new-in-aspnet-mvc-4/_static/image4.png "Página nuevo contacto")

    *Página nuevo contacto*

    **Representación adaptable**

    Desproteja el cambio de tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente al nuevo tamaño de la ventana. Estas plantillas usan la técnica de representación adaptable para representar correctamente en plataformas de escritorio y móviles sin ninguna personalización.

    ![Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador](whats-new-in-aspnet-mvc-4/_static/image5.png "Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador")

    *Plantilla de proyecto de ASP.NET MVC 4 en diferentes tamaños de explorador*

    **Interfaz de usuario más enriquecida con JavaScript**

    Otra mejora de las plantillas de proyecto predeterminadas es el uso de JavaScript para proporcionar un JavaScript más interactivo. Los vínculos de inicio de sesión y registro usados en la plantilla ejemplifican cómo usar las validaciones de jQuery para validar los campos de entrada desde el lado cliente.

    ![Validación de jQuery](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *Validación de jQuery*

    > [!NOTE]
    > Observe las dos secciones de inicio de sesión, en la primera sección puede iniciar sesión con una cuenta registrada en el sitio y en la segunda sección, también puede iniciar sesión con otro servicio de autenticación como Google (deshabilitado de forma predeterminada).
5. Cierre el explorador para detener el depurador y volver a Visual Studio.
6. Abra el archivo **AuthConfig.CS** que se encuentra en la carpeta de inicio de la **aplicación\_** .
7. Quite el comentario de la última línea para registrar Google Client para la autenticación de *OAuth* .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Tenga en cuenta que puede habilitar fácilmente la autenticación mediante un servicio OpenID o OAuth como Facebook, Twitter, Microsoft, etc.
8. Presione **F5** para ejecutar la solución y navegar a la página de inicio de sesión.
9. Seleccione el servicio **Google** para iniciar sesión.

    ![Selección del servicio de inicio de sesión](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Selección del servicio de inicio de sesión*
10. Inicie sesión con su cuenta de Google.
11. Permita que el sitio (localhost) recupere información de la cuenta de Google.
12. Por último, tendrá que registrarse en el sitio para asociar la cuenta de Google.

   ![Asociar su cuenta de Google](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Asociación de la cuenta de Google*
13. Cierre el explorador para detener el depurador y volver a Visual Studio.
14. Ahora Explore la solución para desproteger otras características nuevas introducidas por ASP.NET MVC 4 en la plantilla de proyecto.

   ![La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4](whats-new-in-aspnet-mvc-4/_static/image9.png "La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")

   *La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*

    - **Marcado HTML 5**

       Examinar vistas de plantilla para averiguar el nuevo marcado de tema.

       ![Nueva plantilla con el marcado Razor y HTML5 about. cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "Nueva plantilla con el marcado Razor y HTML5 about. cshtml.")

       *Nueva plantilla, con el marcado de Razor y HTML5 (about. cshtml).*
    - **Bibliotecas de JavaScript actualizadas**

       La plantilla predeterminada de ASP.NET MVC 4 ahora incluye KnockoutJS, un marco de JavaScript MVVM que le permite crear aplicaciones web enriquecidas y con una gran capacidad de respuesta mediante JavaScript y HTML. Al igual que en MVC3, las bibliotecas de interfaz de usuario de jQuery y jQuery también se incluyen en ASP.NET MVC 4.

     > [!NOTE]
     > Puede obtener más información acerca de la biblioteca de KnockOutJS en este vínculo: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/). Además, puede obtener información sobre jQuery y jQuery UI en [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Tarea 2: exploración de la plantilla de aplicación móvil

ASP.NET MVC 4 facilita el desarrollo de sitios web para exploradores móviles y de Tablet PC. Esta plantilla tiene la misma estructura de aplicación que la plantilla de aplicación de Internet (Observe que el código del controlador es prácticamente idéntico), pero su estilo se modificó para representarse correctamente en dispositivos móviles basados en Touch.

1. Seleccione el **archivo | Nuevo |** Comando de menú proyecto. En el cuadro de diálogo **nuevo proyecto** , seleccione el **Visual C# | Plantilla Web** en el árbol del panel izquierdo y elija la **aplicación web MVC 4 ASP.net.** Asigne al proyecto el nombre **Galería Fotogalería. móvil**, seleccione una ubicación (o deje el valor predeterminado), seleccione &quot;agregar a&quot; de soluciones y haga clic en **Aceptar**.
2. En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla de proyecto **aplicación móvil** y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como motor de vista.

    ![Creación de una nueva aplicación de ASP.NET MVC 4 Mobile](whats-new-in-aspnet-mvc-4/_static/image11.png "Creación de una nueva aplicación de ASP.NET MVC 4 Mobile")

    *Creación de una nueva aplicación de ASP.NET MVC 4 Mobile*
3. Ahora puede explorar la solución y consultar algunas de las nuevas características introducidas por la plantilla de la solución ASP.NET MVC 4 para móviles:

    - **Biblioteca de jQuery Mobile**

        La plantilla de proyecto de aplicación móvil incluye jQuery Mobile Library, que es una biblioteca de código abierto para la compatibilidad con exploradores móviles. jQuery Mobile aplica la mejora progresiva a los exploradores móviles que admiten CSS y JavaScript. La mejora progresiva permite que todos los exploradores muestren el contenido básico de una página web, mientras que solo permite que los exploradores más eficaces muestren el contenido enriquecido. Los archivos JavaScript y CSS, incluidos en el estilo de jQuery Mobile, ayudan a los exploradores móviles a ajustar el contenido en la pantalla sin hacer ningún cambio en el marcado de la página.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *Biblioteca de jQuery Mobile incluida en la plantilla*
    - **Marcado basado en HTML5**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Plantilla de aplicación móvil con marcado HTML5, (login. cshtml e index. cshtml)*
4. Presione **F5** para ejecutar la solución.
5. Abra el **emulador de Windows Phone 7**.
6. En la pantalla Inicio del teléfono, abra Internet Explorer. Compruebe la dirección URL en la que se inició la aplicación de escritorio y vaya a esa dirección URL desde el teléfono (por ejemplo, `http://localhost:[PortNumber]/`).
7. Ahora puede entrar en la página de inicio de sesión o consultar la página acerca de. Tenga en cuenta que el estilo del sitio web se basa en la nueva aplicación metro para móviles. La plantilla de proyecto ASP.NET MVC 4 se muestra correctamente en los dispositivos móviles, asegurándose de que todos los elementos de la página están visibles y habilitados. Tenga en cuenta que los vínculos del encabezado son lo suficientemente grandes como para hacer clic o tocar.

    ![Páginas de plantillas de proyecto en un dispositivo móvil](whats-new-in-aspnet-mvc-4/_static/image14.png "Páginas de plantillas de proyecto en un dispositivo móvil")

    *Páginas de plantillas de proyecto en un dispositivo móvil*
8. La nueva plantilla también utiliza la **etiqueta meta**de la ventanilla. La mayoría de los exploradores móviles definen un ancho para una ventana de explorador virtual o &quot;ventanilla&quot;, que es mayor que el ancho real del dispositivo móvil. Esto permite que los exploradores móviles muestren toda la página web dentro de la pantalla virtual. La **etiqueta meta** de la ventanilla permite a los desarrolladores web establecer el ancho, el alto y la escala del área del explorador en los dispositivos móviles **.** La plantilla ASP.NET MVC 4 para aplicaciones móviles establece la ventanilla en el ancho del dispositivo (&quot;width = Device-width&quot;) en la plantilla de diseño (*Views\Shared\_layout. cshtml*), de modo que todas las páginas tendrán su ventanilla establecida en el ancho de la pantalla del dispositivo. Tenga en cuenta que la etiqueta meta de la ventanilla no cambiará la vista predeterminada del explorador.
9. Abra **\_layout. cshtml**, que se encuentra en las **vistas | Carpeta compartida** y comente la etiqueta meta de la ventanilla. Ejecute la aplicación, si aún no está abierta, y consulte las diferencias.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![El sitio después de comentar la etiqueta meta de la ventanilla](whats-new-in-aspnet-mvc-4/_static/image15.png "El sitio después de comentar la etiqueta meta de la ventanilla")

*El sitio después de comentar la etiqueta meta de la ventanilla*
10. En Visual Studio, presione **mayús** + **F5** para detener la depuración de la aplicación.
11. Quite la marca de comentario de la etiqueta meta de la ventanilla.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Tarea 3: uso de la representación adaptable

En esta tarea, aprenderá otro método para representar correctamente una página web en dispositivos móviles y exploradores Web al mismo tiempo sin ninguna personalización. Ya ha usado la etiqueta meta de ventanilla con un propósito similar. Ahora habrá otro método eficaz: *representación adaptable*.

La representación adaptable es una técnica que utiliza **consultas de medios CSS3** para personalizar el estilo aplicado a una página. Las consultas de medios definen condiciones dentro de una hoja de estilos y agrupan los estilos CSS en una condición determinada. Solo cuando la condición es true, el estilo se aplica a los objetos declarados.

La flexibilidad que proporciona la técnica de representación adaptable habilita cualquier personalización para mostrar el sitio en diferentes dispositivos. Puede definir tantos estilos como desee en una sola hoja de estilos sin escribir código lógico para elegir el estilo. Por lo tanto, es una forma muy cómoda de adaptar los estilos de página, ya que reduce la cantidad de código duplicado y lógica para fines de representación. Por otro lado, el consumo de ancho de banda aumentaría, ya que el tamaño de los archivos CSS podía crecer de forma marginal.

Mediante el uso de la técnica de representación adaptable, el sitio se **mostrará correctamente, independientemente del explorador.** Sin embargo, debe considerar si la carga adicional de ancho de banda es un problema.

> [!NOTE]
> El formato básico de una consulta de medios es: @media \[ámbito: ALL | dispositivo de mano | imprimir | proyección |\] de pantalla ([propiedad: valor] y... [propiedad: valor])

Ejemplos de consultas de medios: &gt; **@media todos y (Max-width: 1000px) y (min-width: 700px) {}:** para todas las resoluciones entre 700px y 1000px.

> **@media pantalla y (min-width: 400px) y (Max-width: 700px) {...}:** Solo para pantallas. La resolución debe estar entre 400 y 700px.
> 
> **@media handheld y (min-width: 20em), screen y (min-width: 20em) {...}:** Para equipos portátiles (móviles y dispositivos) y pantallas. El ancho mínimo debe ser mayor que 20em.
> 
> Puede encontrar más información sobre esto en el [sitio de W3C](http://www.w3.org/TR/css3-mediaqueries/).

Ahora aprenderá cómo funciona la representación adaptable y mejorará la legibilidad de la plantilla de sitio web predeterminada de ASP.NET MVC 4.

1. Abra la solución **Fotogallery. sln** que ha creado en la tarea 1 y seleccione el proyecto de la **Galería Fotogallery** . Presione **F5** para ejecutar la solución.
2. Cambie el tamaño del ancho del explorador y establezca las ventanas en la mitad o en menos de un cuarto de su tamaño original. Observe lo que sucede con los elementos del encabezado: algunos elementos no aparecerán en el área visible del encabezado.
3. Abra el archivo **site. CSS** desde el explorador de soluciones de Visual Studio, situado en la carpeta de proyecto de **contenido** . Presione **Ctrl + F** para abrir búsqueda integrada de Visual Studio y escriba `@media` para buscar la **consulta multimedia de CSS**.

    La condición de consulta de medios definida en esta plantilla funciona de esta manera: cuando el tamaño de la ventana del explorador es inferior a **850 PX**, las reglas de CSS que se aplican son las definidas dentro de este bloque multimedia.

    ![Buscar la consulta de medios](whats-new-in-aspnet-mvc-4/_static/image16.png "Buscar la consulta de medios")

    *Buscar la consulta de medios*
4. Reemplace el valor del atributo Max-width establecido en 850 PX por **10px**, para deshabilitar la representación adaptable y presione **Ctrl + S** para guardar los cambios. Vuelva al explorador y presione **Ctrl + F5** para actualizar la página con los cambios que ha realizado. Observe las diferencias en ambas páginas al ajustar el ancho de la ventana.

    ![A la izquierda, la página está aplicando el estilo de @media, a la derecha, se omite el estilo](whats-new-in-aspnet-mvc-4/_static/image17.png "A la izquierda, la página está aplicando el estilo de @media, a la derecha, se omite el estilo")

    *A la izquierda, la página está aplicando el estilo de @media, a la derecha, se omite el estilo*

    Ahora, veamos lo que sucede en los dispositivos móviles:

    ![A la izquierda, la página está aplicando el estilo de @media, a la derecha, se omite el estilo](whats-new-in-aspnet-mvc-4/_static/image18.png "A la izquierda, la página está aplicando el estilo de @media, a la derecha, se omite el estilo")

    *A la izquierda, la página está aplicando el estilo de @media, a la derecha, se omite el estilo*

    Aunque observará que los cambios cuando la página se represente en un explorador Web no son muy significativos, cuando se usa un dispositivo móvil, las diferencias son más obvias. En el lado izquierdo de la imagen, podemos ver que el estilo personalizado mejoró la legibilidad.

    La representación adaptable se puede usar en muchos más escenarios, lo que facilita la aplicación de un estilo condicional a un sitio web y la resolución de problemas de estilo comunes con un enfoque sencillo.

    La etiqueta meta de la ventanilla y las consultas de elementos multimedia de CSS no son específicas de ASP.NET MVC 4, por lo que puede aprovechar estas características en cualquier aplicación Web.
5. En Visual Studio, presione **mayús** + **F5** para detener la depuración de la aplicación.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Ejercicio 2: crear la aplicación Web de la Galería fotográfica

En este ejercicio, trabajará en una aplicación de la Galería fotográfica para mostrar fotografías. Empezará con la plantilla de proyecto ASP.NET MVC 4 y, a continuación, agregará una característica para recuperar fotografías de un servicio y mostrarlas en la Página principal.

En el ejercicio siguiente, actualizará esta solución para mejorar la forma en que se muestra en los dispositivos móviles.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Tarea 1: creación de un servicio fotográfico ficticio

En esta tarea, creará un simulacro de Photo Service para recuperar el contenido que se mostrará en la galería. Para ello, agregará un nuevo controlador que simplemente devolverá un archivo JSON con los datos de cada foto.

1. Abra **Visual Studio** si aún no está abierto.
2. Seleccione el **archivo | Nuevo |** Comando de menú proyecto. En el cuadro de diálogo **nuevo proyecto** , seleccione el **Visual C# | Plantilla Web** en el árbol del panel izquierdo y elija **aplicación Web de ASP.NET MVC 4.** Asigne un nombre a la **Galería**fotode proyecto, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar**. Como alternativa, puede seguir trabajando desde su solución de **aplicación de Internet** de ASP.NET MVC 4 existente desde el **ejercicio 1** y omitir el paso siguiente.
3. En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione la plantilla de proyecto **aplicación de Internet** y haga clic en **Aceptar**. Asegúrese de que ha seleccionado Razor como motor de vistas.
4. En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **App\_Data** del proyecto y seleccione **Agregar | Elemento existente**. Vaya a la carpeta **Source\Assets\App\_Data** de este laboratorio y agregue el archivo **photos. JSON** .
5. Cree un nuevo controlador con el nombre **fotocontroller**. Para ello, haga clic con el botón derecho en la carpeta **Controllers** , vaya a **Agregar** y seleccione **controlador.** Complete el nombre del controlador, deje la plantilla de **controlador de MVC vacía** y haga clic en **Agregar**.

    ![Adición de la fotocontroladora](whats-new-in-aspnet-mvc-4/_static/image19.png "Adición de la fotocontroladora")

    *Adición de la fotocontroladora*
6. Reemplace el método **index** por la siguiente acción de la **Galería** y devuelva el contenido del archivo JSON que ha agregado recientemente al proyecto.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex02-Gallery Action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Presione **F5** para ejecutar la solución y, a continuación, vaya a la siguiente dirección URL para probar el servicio fotográfico ficticio: `http://localhost:[port]/photo/gallery` (el valor [puerto] depende del puerto actual en el que se inició la aplicación). La solicitud a esta dirección URL debe recuperar el contenido del archivo **photos. JSON** .

    ![Probar el servicio fotográfico ficticio](whats-new-in-aspnet-mvc-4/_static/image20.png "Probar el servicio fotográfico ficticio")

    *Probar el servicio fotográfico ficticio*

En una implementación real, podría usar [ASP.net web API](../../../../web-api/index.md) para implementar el servicio de la Galería fotográfica de. ASP.NET Web API es un marco que facilita la creación de servicios HTTP disponibles para una amplia variedad de clientes, entre los que se incluyen exploradores y dispositivos móviles. ASP.NET Web API es una plataforma ideal para compilar aplicaciones de RESTful en .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Tarea 2: Mostrar la Galería fotográfica

En esta tarea, actualizará la Página principal para mostrar la Galería fotográfica mediante el servicio ficticio creado en la primera tarea de este ejercicio. Agregará los archivos de modelo y actualizará las vistas de la galería.

1. En Visual Studio, presione **mayús** + **F5** para detener la depuración de la aplicación.
2. Cree la clase **Photo** en la carpeta **Models** . Para ello, haga clic con el botón derecho en la carpeta **modelos** , seleccione **Agregar** y haga clic en **clase**. Después, establezca el nombre en **Photo.CS** y haga clic en **Agregar**.
3. Agregue los siguientes miembros a la clase **Photo** .

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex02-Photo Model*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Abra el archivo **HomeController.cs** desde la carpeta **Controladores**.
5. Agregue las siguientes instrucciones de uso.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex02-HomeController Using*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Actualice la acción de **Índice** para usar **HttpClient** para recuperar los datos de la galería y, a continuación, use **JavaScriptSerializer** para deserializarlo en el modelo de vista.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex02-index Action*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Abra el archivo **index. cshtml** que se encuentra en la carpeta **Views\Home** y reemplace todo el contenido por el código siguiente.

    Este código recorre en bucle todas las fotografías recuperadas del servicio y las muestra en una lista sin ordenar.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex02-Photo List*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta **contenido** del proyecto y seleccione **Agregar | Elemento existente**. Vaya a la carpeta **Source\Assets\Content** de este laboratorio y agregue el archivo **site. CSS** . Tendrá que confirmar su sustitución. Si tiene abierto el archivo **site. CSS** , tendrá que confirmar la recarga del archivo también.
9. Abra el explorador de archivos y copie toda la carpeta **fotos** que se encuentra en la carpeta **Source\Assets** de este laboratorio en la carpeta raíz del proyecto en explorador de soluciones.
10. Ejecute la aplicación. Ahora debería ver la Página principal que muestra las fotos en la galería.

    ![Galería fotográfica de](whats-new-in-aspnet-mvc-4/_static/image21.png "Galería fotográfica")

    *Galería fotográfica de*
11. En Visual Studio, presione **mayús** + **F5** para detener la depuración de la aplicación.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Ejercicio 3: agregar compatibilidad con dispositivos móviles

Una de las actualizaciones clave en ASP.NET MVC 4 es la compatibilidad con el desarrollo móvil. En este ejercicio, explorará las nuevas características de ASP.NET MVC 4 para las aplicaciones móviles mediante la extensión de la solución de Fotogalería que ha creado en el ejercicio anterior.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Tarea 1: instalación de jQuery Mobile en una aplicación ASP.NET MVC 4

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex3-MobileSupport/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la **consola del administrador de paquetes** haciendo clic en la opción de menú **herramientas** > **Administrador de paquetes NuGet** > **consola del administrador de paquetes** .

    ![Apertura de la consola del administrador de paquetes NuGet](whats-new-in-aspnet-mvc-4/_static/image22.png "Apertura de la consola del administrador de paquetes NuGet")

    *Apertura de la consola del administrador de paquetes NuGet*
3. En la consola del administrador de paquetes, ejecute el siguiente comando para instalar el paquete **jQuery. Mobile. Mvc** .

    jQuery Mobile es una biblioteca de código abierto para compilar una interfaz de usuario Web optimizada para Touch. El paquete de NuGet jQuery. Mobile. MVC incluye aplicaciones auxiliares para usar jQuery Mobile con una aplicación ASP.NET MVC 4.

    > [!NOTE]
    > Al ejecutar el comando siguiente, descargará la biblioteca jQuery. Mobile. MVC desde Nuget.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Este comando instala jQuery Mobile y algunos archivos auxiliares, entre los que se incluyen los siguientes:

    - **Views/Shared/\_layout. Mobile. cshtml**: es un diseño basado en móviles de jQuery optimizado para una pantalla más pequeña. Cuando el sitio web recibe una solicitud de un explorador móvil, reemplazará el diseño original (\_layout. cshtml) por este.
    - Un componente de modificador de vista: consta de la vista parcial views **/Shared/\_ViewSwitcher. cshtml** y el controlador **ViewSwitcherController.CS** . Este componente mostrará un vínculo en los exploradores móviles para permitir que los usuarios cambien a la versión de escritorio de la página.  
        ![Proyecto de Galería fotográfica con soporte móvil](whats-new-in-aspnet-mvc-4/_static/image23.png "Proyecto de Galería fotográfica con soporte móvil")

        *Proyecto de Galería fotográfica con soporte móvil*
4. Registre los paquetes móviles. Para ello, abra el archivo **global.asax.CS** y agregue la siguiente línea.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex03-registrar paquetes móviles*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Ejecute la aplicación mediante un explorador Web de escritorio.
6. Abra el **emulador de Windows Phone 7,** que se encuentra en el **menú Inicio | Todos los programas | Windows Phone SDK 7,1 | Windows Phone emulador.**
7. En la pantalla Inicio del teléfono, abra Internet Explorer. Compruebe la dirección URL en la que se inició la aplicación y vaya a esa dirección URL con el explorador del teléfono (por ejemplo, `http://localhost:[PortNumber]/`).

    Observará que la aplicación tendrá un aspecto diferente en el emulador de Windows Phone, ya que jQuery. Mobile. MVC ha creado nuevos recursos en el proyecto que muestran las vistas optimizadas para dispositivos móviles.

    Observe el mensaje que se encuentra en la parte superior del teléfono y que muestra el vínculo que cambia a la vista de escritorio. Además, el diseño **\_layout. Mobile. cshtml** creado por el paquete que ha instalado incluye un diseño diferente en la aplicación.

    > [!NOTE]
    > Hasta ahora, no hay ningún vínculo para volver a la vista móvil. Se incluirá en versiones posteriores.

    ![Vista móvil de la Página principal de la Galería fotográfica](whats-new-in-aspnet-mvc-4/_static/image24.png "Vista móvil de la Página principal de la Galería fotográfica")

    *Vista móvil de la Página principal de la Galería fotográfica*
8. En Visual Studio, presione **mayús** + **F5** para detener la depuración de la aplicación.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Tarea 2: creación de vistas móviles

En esta tarea, creará una versión móvil de la vista de índice con el contenido adaptado para una mejor apariencia en los dispositivos móviles.

1. Copie la vista **Views\Home\Index.cshtml** y péguela para crear una copia, cambie el nombre del nuevo archivo a **index. Mobile. cshtml**.
2. Abra la nueva vista **index. Mobile. cshtml** y reemplace la etiqueta &lt;UL&gt; existente por este código. Al hacerlo, actualizará la etiqueta &lt;UL&gt; con anotaciones de datos móviles de jQuery para usar los temas móviles de jQuery.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Tenga en cuenta que:
    > 
    > - El atributo **de rol de datos** establecido en **ListView** representará la lista con los estilos ListView.
    > 
    > - El atributo **Data-bajorrelieve** establecido en true mostrará la lista con borde redondeado y margen.
    > 
    > - El atributo **Data-Filter** establecido en **true** generará un cuadro de búsqueda.
    > 
    > Puede obtener más información acerca de las convenciones móviles de jQuery en la documentación del proyecto: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Presione **Ctrl + S** para guardar los cambios.
4. Cambie al **emulador de Windows Phone** y actualice el sitio. Observe la nueva apariencia de la lista Galería, así como el nuevo cuadro de búsqueda que se encuentra en la parte superior. A continuación, escriba una palabra en el cuadro de búsqueda (por ejemplo, **Tulips**) para iniciar una búsqueda en la Galería fotográfica.

    ![Galería con estilo ListView con filtrado](whats-new-in-aspnet-mvc-4/_static/image25.png "Galería con estilo ListView con filtrado")

    *Galería con estilo ListView con filtrado*

    En Resumen, ha utilizado la receta ver el dispositivo de movilización para crear una copia de la vista de índice con el &quot;sufijo de&quot; móvil. Este sufijo indica a ASP.NET MVC 4 que cada solicitud generada desde un dispositivo móvil usará esta copia del índice. Además, ha actualizado la versión móvil de la vista de índice para usar jQuery Mobile para mejorar la apariencia del sitio en los dispositivos móviles.
5. Vuelva a Visual Studio y Abra **site. Mobile. CSS** , que se encuentra en la carpeta de **contenido** .
6. Corrija el posicionamiento del título de la fotografía para que se muestre en el lado derecho de la imagen. Para ello, agregue el código siguiente al archivo **site. Mobile. CSS** .

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Presione **Ctrl + S** para guardar los cambios.
8. Vuelva al **emulador de Windows Phone** y actualice el sitio. Observe que el título de la fotografía está ahora colocado correctamente.

    ![Título situado en el lado derecho de la imagen](whats-new-in-aspnet-mvc-4/_static/image26.png "Título situado en el lado derecho de la imagen")

    *Título situado en el lado derecho de la imagen*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Tarea 3: temas móviles de jQuery

Cada diseño y widget de jQuery Mobile está diseñado en torno a un nuevo marco de CSS orientado a objetos que permite aplicar un tema de diseño visual unificado completo a sitios y aplicaciones.

el tema predeterminado de jQuery Mobile incluye 5 muestras con letras (a, b, c, d, e) para una referencia rápida.

En esta tarea, actualizará el diseño móvil para usar un tema diferente del predeterminado.

1. Vuelva a Visual Studio.
2. Abra el archivo **\_layout. Mobile. cshtml** que se encuentra en **Views\Shared**.
3. Busque el elemento div con el rol de datos establecido en &quot;página&quot; y actualice el **tema de datos** a &quot;**e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Presione **Ctrl + S** para guardar los cambios.
5. Actualice el sitio en el **emulador de Windows Phone** y observe la nueva combinación de colores.

    ![Diseño móvil con una combinación de colores diferente](whats-new-in-aspnet-mvc-4/_static/image27.png "Diseño móvil con una combinación de colores diferente")

    *Diseño móvil con una combinación de colores diferente*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Tarea 4: usar el componente de modificador de vista y el explorador reemplazar características

Una Convención para las páginas web optimizadas para móviles es agregar un vínculo cuyo texto es similar a la vista de escritorio o al modo de sitio completo que permite a los usuarios cambiar a una versión de escritorio de la página. El paquete jQuery. Mobile. MVC incluye un componente de **selector de vista** de ejemplo para este propósito usado en la vista **\_layout. Mobile. cshtml** .

![Vínculo para cambiar a la vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image28.png "Vínculo para cambiar a la vista de escritorio")

*Vínculo para cambiar a la vista de escritorio*

El modificador de vista utiliza una nueva característica denominada **invalidación del explorador**. Esta característica permite a la aplicación tratar las solicitudes como si provinieran de un explorador diferente (agente de usuario) del que procede realmente.

En esta tarea, explorará la implementación de ejemplo de un modificador de vista agregado por jQuery. Mobile. MVC y las nuevas características de invalidación del explorador en ASP.NET MVC 4.

1. Vuelva a Visual Studio.
2. Abra la vista **\_layout. Mobile. cshtml** que se encuentra en la carpeta **Views\Shared** y observe el componente de modificador de vista al que se hace referencia como una vista parcial.

    ![Diseño móvil mediante el componente de modificador de vista](whats-new-in-aspnet-mvc-4/_static/image29.png "Diseño móvil mediante el componente de modificador de vista")

    *Diseño móvil mediante el componente de modificador de vista*
3. Abra la vista parcial de **\_ViewSwitcher. cshtml** .

    La vista parcial usa el nuevo método **ViewContext. HttpContext. GetOverriddenBrowser ()** para determinar el origen de la solicitud Web y mostrar el vínculo correspondiente para cambiar a las vistas de escritorio o móviles.

    El método **GetOverriddenBrowser** devuelve una instancia de **HttpBrowserCapabilitiesBase** que corresponde al agente de usuario establecido actualmente para la solicitud (real o invalidada). Puede usar este valor para obtener propiedades como **IsMobileDevice**.

    ![Vista parcial de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "Vista parcial de ViewSwitcher")

    *Vista parcial de ViewSwitcher*
4. Abra la clase **ViewSwitcherController.CS** que se encuentra en la carpeta **Controllers** . Compruebe que el vínculo del componente ViewSwitcher llama a la acción del SwitchView y observe los nuevos métodos HttpContext.

    - El método **HttpContext. ClearOverriddenBrowser ()** quita cualquier agente de usuario invalidado para la solicitud actual.
    - El método **HttpContext. SetOverriddenBrowser ()** invalida el valor del agente de usuario real de la solicitud mediante el agente de usuario especificado.  
        ![Controlador de ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image31.png "Controlador de ViewSwitcher")  
*Controlador de ViewSwitcher*

        El reemplazo del explorador es una característica principal de ASP.NET MVC 4, que también está disponible incluso si no se instala el paquete jQuery. Mobile. MVC. Sin embargo, esta característica solo afecta a la vista, el diseño y la vista parcial, y no afecta a ninguna de las características que dependen del objeto request. Browser.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Tarea 5: agregar el modificador de vista en la vista de escritorio

En esta tarea, actualizará el diseño del escritorio para incluir el modificador de vista. Esto permitirá que los usuarios móviles vuelvan a la vista móvil al examinar la vista de escritorio.

1. Actualice el sitio en el **emulador de Windows Phone**.
2. Haga clic en el vínculo **vista del escritorio** situado en la parte superior de la galería. Observe que no hay ningún modificador de vista en la vista de escritorio para que pueda volver a la vista móvil.
3. Vuelva a Visual Studio y abra la vista **\_layout. cshtml** .
4. Busque la sección de inicio de sesión e inserte una llamada para representar la vista parcial **\_ViewSwitcher** debajo de la vista parcial **\_LogOnPartial** . A continuación, presione **Ctrl + S** para guardar los cambios.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Presione **Ctrl + S** para guardar los cambios.
6. Actualice la página en el emulador de Windows Phone y haga doble clic en la pantalla para acercarla. Observe que la Página principal muestra ahora el vínculo de **vista móvil** que cambia de la vista de móvil a la de escritorio.

    ![Selector de vistas representado en la vista de escritorio](whats-new-in-aspnet-mvc-4/_static/image32.png "Selector de vistas representado en la vista de escritorio")

    *Selector de vistas representado en la vista de escritorio*
7. Cambie a la vista móvil de nuevo y vaya a la página **acerca de** (http://localhost[puerto]/home/About). Tenga en cuenta que, incluso si no ha creado una vista about. Mobile. cshtml, se muestra la página about con el diseño móvil (\_layout. Mobile. cshtml).

    ![Página acerca de](whats-new-in-aspnet-mvc-4/_static/image33.png "Página About")

    *Página acerca de*
8. Por último, abra el sitio en un explorador Web de escritorio. Tenga en cuenta que ninguna de las actualizaciones anteriores ha afectado a la vista de escritorio.

    ![Vista de escritorio de la Galería Fotogallery](whats-new-in-aspnet-mvc-4/_static/image34.png "Vista de escritorio de la Galería Fotogallery")

    *Vista de escritorio de la Galería Fotogallery*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Tarea 6: crear nuevos modos de presentación

La nueva característica modos de presentación permite a una aplicación seleccionar vistas según el explorador que está generando la solicitud. Por ejemplo, si un explorador de escritorio solicita la Página principal, la aplicación devolverá la plantilla **Views\Home\Index.cshtml** . Después, si un explorador móvil solicita la Página principal, la aplicación devolverá la plantilla **Views\Home\Index.Mobile.cshtml** .

En esta tarea, creará un diseño personalizado para dispositivos iPhone y tendrá que simular solicitudes de dispositivos iPhone. Para ello, puede usar un emulador o simulador de iPhone (como el [simulador móvil eléctrico](http://www.electricplum.com/)) o un explorador con complementos que modifiquen el agente de usuario. Para obtener instrucciones sobre cómo establecer la cadena de agente de usuario en un explorador Safari para emular un iPhone, consulte [Cómo dejar que Safari simule que es así en el](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) blog de David Alison.

**Tenga en cuenta que esta tarea es opcional y puede continuar a lo largo del laboratorio sin ejecutarla.**

1. En Visual Studio, presione **mayús** + **F5** para detener la depuración de la aplicación.
2. Abra **global.asax.CS** y agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Agregue el siguiente código resaltado en el método de inicio de\_de la aplicación.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex03-iPhone DisplayMode*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Ha registrado un nuevo **DefaultDisplayMode llamado &quot;iPhone&quot;** , dentro de la lista estática **DisplayModeProvider. Instance. modes** , que se comparará con cada solicitud entrante. Si la solicitud entrante contiene la cadena &quot;iPhone&quot;, ASP.NET MVC buscará las vistas cuyo nombre contenga el sufijo &quot;iPhone&quot;. El parámetro 0 indica cómo es específico el nuevo modo. por ejemplo, esta vista es más específica que la regla general &quot;. Mobile&quot; que coincide con las solicitudes de dispositivos móviles.

Después de que se ejecute este código, cuando un explorador de iPhone genere una solicitud, la aplicación usará el diseño **Views\Shared\\_Layout. iPhone. cshtml** que creará en los pasos siguientes.

> [!NOTE]
> Esta forma de probar la solicitud de iPhone se ha simplificado con fines de demostración y podría no funcionar según lo esperado para cada cadena de agente de usuario de iPhone (por ejemplo, test distingue mayúsculas de minúsculas).

4. Cree una copia del archivo **\_layout. Mobile. cshtml** en la carpeta **Views\Shared** y cambie el nombre de la copia a &quot; **\_Layout. iPhone. cshtml**&quot;.
5. Abra **\_layout. iPhone. cshtml** que creó en el paso anterior.
6. Busque el elemento div con el atributo de rol de datos establecido en la **Página** y cambie el atributo **Data-Theme** por &quot;**un**&quot;.

[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Ahora tiene 3 diseños en la aplicación ASP.NET MVC 4:

1. **\_layout. cshtml**: diseño predeterminado usado para los exploradores de escritorio.
2. **\_layout. Mobile. cshtml**: diseño predeterminado usado para dispositivos móviles.
3. **\_layout. iPhone. cshtml**: diseño específico para dispositivos iPhone, con una combinación de colores diferente para diferenciar de \_layout. Mobile. cshtml.
7. Presione **F5** para ejecutar la aplicación y examinar el sitio en el **emulador de Windows Phone**.
8. Abra un **simulador de iPhone** (consulte el [Apéndice C](#AppendixC) para obtener instrucciones sobre cómo instalar y configurar un simulador de iPhone) y vaya también al sitio. Tenga en cuenta que cada teléfono usa la plantilla específica.

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Usar vistas diferentes para cada dispositivo móvil*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Ejercicio 4: uso de controladores asincrónicos

Microsoft .NET Framework 4,5 incorpora nuevas características de lenguaje C# en y Visual Basic para proporcionar una nueva base para asincronía en la programación de .net. Esta nueva base hace que la programación asincrónica sea similar a la programación sincrónica, y de la misma forma que la programación sincrónica. Ahora puede escribir métodos de acción asincrónicos en ASP.NET MVC 4 mediante la clase **AsyncController** . Los métodos de acción asincrónicos se pueden utilizar para solicitudes de ejecución prolongada no relacionadas con la CPU. De este modo, el servidor web no se bloquea y se puede realizar trabajo en él mientras se procesa la solicitud. La clase AsyncController se usa normalmente para llamadas a servicios Web de ejecución prolongada.

En este ejercicio se explican los conceptos básicos del funcionamiento asincrónico en ASP.NET MVC 4. Si desea profundizar más, puede consultar el artículo siguiente: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Tarea 1: implementar un controlador asincrónico

1. Abra la carpeta **Begin** Solution ubicada en **source/Ex4-Async/Begin/** . De lo contrario, puede seguir usando la solución **final** obtenida al completar el ejercicio anterior.

   1. Si ha abierto la solución de **Inicio** proporcionada, tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. Abra la clase **HomeController.CS** desde la carpeta **Controllers** .
3. Agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Actualice la clase **HomeController** para que herede de **AsyncController**. Los controladores que se derivan de AsyncController permiten que ASP.NET Procese las solicitudes asincrónicas, y todavía pueden atender métodos de acción sincrónicos.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Agregue la palabra clave **Async** al método **index** y haga que devuelva la **tarea Type&lt;ActionResult&gt;** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > La palabra clave **Async** es una de las nuevas palabras clave que proporciona el .NET Framework 4,5. indica al compilador que este método contiene código asincrónico. Un objeto de **tarea** representa una operación asincrónica que puede completarse en algún momento en el futuro.
6. Reemplace el **cliente. Llamada a GetAsync ()** con la versión completa de Async mediante la palabra clave Await, como se muestra a continuación.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex04-GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > En la versión anterior, usaba la propiedad **result** del objeto **Task** para bloquear el subproceso hasta que se devuelva el resultado (versión de sincronización).
    > 
    > Al agregar la palabra clave **Await** , se indica al compilador que espere de forma asincrónica a la tarea devuelta por la llamada al método. Esto significa que el resto del código se ejecutará como una devolución de llamada solo después de que se complete el método Await. Otro aspecto que se debe tener en cuenta es que no es necesario cambiar el bloque try-catch para realizar este trabajo: las excepciones que se producen en segundo plano o en primer plano se seguirán detectando sin ningún trabajo adicional mediante el uso de un controlador proporcionado por el marco.
7. Cambie el código para continuar con la implementación asincrónica reemplazando las líneas por el nuevo código, tal como se muestra a continuación.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex04-ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Ejecute la aplicación. Observará que no hay cambios importantes, pero el código no bloqueará un subproceso del grupo de subprocesos con un mejor uso de los recursos del servidor y la mejora del rendimiento.

    > [!NOTE]
    > Puede obtener más información sobre las nuevas características de programación asincrónica en el laboratorio &quot;**programación asincrónica en .net 4,5 con C# y Visual Basic**&quot; incluido en el kit de aprendizaje de Visual Studio.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Tarea 2: control de los tiempos de espera con tokens de cancelación

Los métodos de acción asincrónicos que devuelven instancias de tarea también pueden admitir tiempos de espera. En esta tarea, actualizará el código del método de índice para controlar un escenario de tiempo de espera mediante un token de cancelación.

1. Vuelva a Visual Studio y presione **Mayús + F5** para detener la depuración.
2. Agregue la siguiente instrucción using al archivo **HomeController.CS** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Actualice la acción de índice para recibir un argumento **CancellationToken** .

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Actualice la llamada a **GetAsync** para pasar el token de cancelación.

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex04-SendAsync con CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Decora el método de *Índice* con un atributo **AsyncTimeout** establecido en 500 milisegundos y un atributo **handleError** configurado para controlar **TaskCanceledException** redirigiendo a una vista **TimedOut** .

    (Fragmento de código- *ASP.NET MVC 4 Lab-Ex04-Attributes*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Abra la clase de **Fotocontroladora** y actualice el método de la **Galería** para retrasar la ejecución 1000 milisegundos (1 segundo) para simular una tarea de ejecución prolongada.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Abra el archivo **Web. config** y habilite los errores personalizados agregando el elemento siguiente.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Cree una nueva vista en **Views\Shared** denominada **TimedOut** y use el diseño predeterminado. En el Explorador de soluciones, haga clic con el botón secundario en la carpeta **Views\Shared** y seleccione **Agregar | Ver**.

    ![Usar vistas diferentes para cada dispositivo móvil](whats-new-in-aspnet-mvc-4/_static/image36.png "Usar vistas diferentes para cada dispositivo móvil")

    *Usar vistas diferentes para cada dispositivo móvil*
9. Actualice el contenido de la vista **TimedOut** tal como se muestra a continuación.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Ejecute la aplicación y vaya a la dirección URL raíz. Como ha agregado un **subproceso. Sleep** de 1000 milisegundos, obtendrá un error de tiempo de espera, generado por el atributo **AsyncTimeout** y filtrado por el atributo **handleError** .

    ![Excepción de tiempo de espera controlada](whats-new-in-aspnet-mvc-4/_static/image37.png "Excepción de tiempo de espera controlada")

    *Excepción de tiempo de espera controlada*

> [!NOTE]
> Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice D: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixD).

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha observado algunas de las nuevas características residentes en ASP.NET MVC 4. Se han analizado los siguientes conceptos:

- Aproveche las mejoras realizadas en las plantillas de proyecto de ASP.NET MVC, incluida la nueva plantilla de proyecto de aplicación móvil.
- Usar el atributo de ventanilla de HTML5 y las consultas de medios CSS para mejorar la presentación en dispositivos móviles
- Usar jQuery Mobile para mejoras progresivas y para compilar una interfaz de usuario Web optimizada para Touch
- Crear vistas específicas para dispositivos móviles
- Usar el componente View-Switcher para alternar entre las vistas de escritorio y móviles de la aplicación
- Crear controladores asincrónicos con la compatibilidad de tareas

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apéndice A: usar fragmentos de código

Con los fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo se pueden usar, como se muestra en la ilustración siguiente.

![Usar fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-aspnet-mvc-4/_static/image38.png "Usar fragmentos de código de Visual Studio para insertar código en el proyecto")

*Usar fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el tecladoC# (solo)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Ver como IntelliSense muestra los nombres de los fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se seleccione el nombre del fragmento de código completo).
5. Presione la tecla TAB dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento de código.](whats-new-in-aspnet-mvc-4/_static/image39.png "Comience a escribir el nombre del fragmento de código.")

*Comience a escribir el nombre del fragmento de código.*

![Presione TAB para seleccionar el fragmento de código resaltado](whats-new-in-aspnet-mvc-4/_static/image40.png "Presione TAB para seleccionar el fragmento de código resaltado")

*Presione TAB para seleccionar el fragmento de código resaltado*

![Presione la tecla TAB de nuevo y el fragmento de código se expandirá](whats-new-in-aspnet-mvc-4/_static/image41.png "Presione la tecla TAB de nuevo y el fragmento de código se expandirá")

*Presione la tecla TAB de nuevo y el fragmento de código se expandirá*

***Para agregar un fragmento de código mediante el mouseC#(, Visual Basic y XML)***

1. Haga clic con el botón secundario en el lugar donde desea insertar el fragmento de código.
2. Seleccione **Insertar fragmento** de código seguido de **mis fragmentos de código**.
3. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.](whats-new-in-aspnet-mvc-4/_static/image42.png "Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.")

*Haga clic con el botón derecho en el lugar donde desea insertar el fragmento de código y seleccione Insertar fragmento de código.*

![Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él](whats-new-in-aspnet-mvc-4/_static/image43.png "Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él")

*Seleccione el fragmento de código relevante de la lista; para ello, haga clic en él*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apéndice B: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;*Visual Studio Express 2012 para web con el SDK de Windows Azure*&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *VS Express para Web icono*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Apéndice C: instalación de WebMatrix 2 y el simulador de iPhone

Para ejecutar el sitio en un dispositivo iPhone simulado, puede usar la extensión WebMatrix &quot;simulador móvil eléctrico para la&quot;de iPhone. Además, puede configurar la misma extensión para ejecutar el simulador desde Visual Studio 2012.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Tarea 1: instalación de WebMatrix 2

1. Vaya a [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;*WebMatrix 2*&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalación de WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Instalación de WebMatrix 2")

    *Instalación de WebMatrix 2*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](whats-new-in-aspnet-mvc-4/_static/image50.png "Aceptación de los términos de licencia")

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-mvc-4/_static/image51.png "Progreso de la instalación")

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](whats-new-in-aspnet-mvc-4/_static/image52.png "Instalación completada")

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Tarea 2: instalación de la extensión del simulador de iPhone

1. Ejecute **WebMatrix** y abra cualquier sitio web existente o cree uno nuevo.
2. Haga clic en el botón **Ejecutar** de la cinta de opciones **Inicio** y seleccione **Agregar nuevo**.

    ![Agregando nueva extensión WebMatrix](whats-new-in-aspnet-mvc-4/_static/image53.png "Agregando nueva extensión WebMatrix")

    *Agregando nueva extensión WebMatrix*
3. Seleccione **simulador de iPhone** y haga clic en **instalar**.

    ![Examinar extensiones WebMatrix](whats-new-in-aspnet-mvc-4/_static/image54.png "Examinar extensiones WebMatrix")

    *Examinar extensiones WebMatrix*
4. En los detalles del paquete, haga clic en **instalar** para continuar con la instalación de la extensión.

    ![extensión del simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image55.png "extensión del simulador de iPhone")

    *extensión del simulador de iPhone*
5. Lea y acepte el CLUF de la extensión.

    ![CLUF de la extensión WebMatrix](whats-new-in-aspnet-mvc-4/_static/image56.png "CLUF de la extensión WebMatrix")

    *CLUF de la extensión WebMatrix*
6. Ahora, puede ejecutar el sitio web desde WebMatrix mediante la opción simulador de iPhone.

    ![Ejecutar con iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Ejecutar con iPhone")

    *Ejecutar con iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Tarea 3: configuración de Visual Studio 2012 para ejecutar el simulador de iPhone

1. Abra **Visual Studio 2012** y abra cualquier sitio web o cree un nuevo proyecto.
2. Haga clic en la flecha abajo en el botón ejecutar y seleccione **examinar con**.

    ![Examinar con](whats-new-in-aspnet-mvc-4/_static/image58.png "Explorar con")

    *Examinar con*
3. En el cuadro de diálogo &quot;examinar con&quot;, haga clic en **Agregar**.
4. En el cuadro de diálogo &quot;agregar&quot; de programa, use los valores siguientes:

   - **Programa**: C:\Users\*{CurrentUser} * \AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(actualice la ruta de acceso en consecuencia)*
   - **Argumentos**: &quot;1&quot;
   - **Nombre descriptivo**: simulador de iPhone

     ![Agregar programa](whats-new-in-aspnet-mvc-4/_static/image59.png "Agregar programa")

     *Agregar programa para buscar*
5. Haga clic en **Aceptar** y cierre los cuadros de diálogo.
6. Ahora puede ejecutar sus aplicaciones web en el simulador de iPhone desde Visual Studio 2012.

    ![Examinar con el simulador de iPhone](whats-new-in-aspnet-mvc-4/_static/image60.png "Examinar con el simulador de iPhone")

    *Examinar con el simulador de iPhone*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice D: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Windows Azure.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio web desde el portal de Windows Azure

1. Vaya a la [portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.

    > [!NOTE]
    > Con Windows Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico. Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).

    ![Iniciar sesión en Windows Azure Portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Iniciar sesión en Windows Azure Portal")

    *Iniciar sesión en Windows Azure Portal de administración*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](whats-new-in-aspnet-mvc-4/_static/image62.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](whats-new-in-aspnet-mvc-4/_static/image63.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](whats-new-in-aspnet-mvc-4/_static/image64.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](whats-new-in-aspnet-mvc-4/_static/image65.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](whats-new-in-aspnet-mvc-4/_static/image66.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.

    ![Descargar el perfil de publicación del sitio web](whats-new-in-aspnet-mvc-4/_static/image67.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image68.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](whats-new-in-aspnet-mvc-4/_static/image69.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** , y haga clic en el botón ![agregar-Client-IP-dirección-aceptar-botón](whats-new-in-aspnet-mvc-4/_static/image70.png).

    ![Agregando la dirección IP del cliente](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Confirmar cambios*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-aspnet-mvc-4/_static/image73.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-aspnet-mvc-4/_static/image74.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](whats-new-in-aspnet-mvc-4/_static/image75.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](whats-new-in-aspnet-mvc-4/_static/image76.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](whats-new-in-aspnet-mvc-4/_static/image78.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](whats-new-in-aspnet-mvc-4/_static/image80.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

    ![Aplicación publicada en Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Aplicación publicada en Windows Azure")

    *Aplicación publicada en Windows Azure*
