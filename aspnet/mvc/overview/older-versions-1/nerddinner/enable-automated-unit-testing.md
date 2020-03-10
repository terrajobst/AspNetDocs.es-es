---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: Habilitar pruebas unitarias automatizadas | Microsoft Docs
author: microsoft
description: En el paso 12 se muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que comprueban la funcionalidad de NerdDinner y que nos dará la confianza para realizar cambios...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 09a7aa186605a6cce48ee94028425ded957c00d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435595"
---
# <a name="enable-automated-unit-testing"></a>Habilitar las pruebas unitarias automatizadas

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 12 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 12 se muestra cómo desarrollar un conjunto de pruebas unitarias automatizadas que comprueban la funcionalidad de NerdDinner y que nos dará la confianza de realizar cambios y mejoras en la aplicación en el futuro.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner paso 12: pruebas unitarias

Vamos a desarrollar un conjunto de pruebas unitarias automatizadas que comprueban la funcionalidad de NerdDinner y que nos dará la confianza de realizar cambios y mejoras en la aplicación en el futuro.

### <a name="why-unit-test"></a>¿Por qué prueba unitaria?

En la unidad en el trabajo de una mañana tiene un destello repentino de inspiración sobre una aplicación en la que está trabajando. Observa que hay un cambio que puede implementar, lo que hará que la aplicación sea mucho mejor. Podría tratarse de una refactorización que limpia el código, agrega una nueva característica o corrige un error.

La pregunta que le reparte cuando llega a su equipo es: "¿Qué es lo seguro que es hacer esta mejora?". ¿Qué ocurre si hacer el cambio tiene efectos secundarios o rompe algo? El cambio podría ser sencillo y solo tardará unos minutos en implementarse, pero ¿qué ocurre si tardan horas en probar manualmente todos los escenarios de la aplicación? ¿Qué ocurre si olvida cubrir un escenario y una aplicación interrumpida entra en producción? ¿Merece realmente esta mejora el esfuerzo?

Las pruebas unitarias automatizadas pueden proporcionar una red de seguridad que le permita mejorar continuamente sus aplicaciones y evitar tener miedo al código en el que está trabajando. Las pruebas automatizadas que comprueban la funcionalidad rápidamente le permiten codificar con confianza, y le permiten realizar mejoras que, de otro modo, podrían no sentirse cómodos. También ayudan a crear soluciones que son más fáciles de mantener y tienen una duración más larga, lo que conduce a una rentabilidad mucho mayor de la inversión.

El marco de MVC de ASP.NET hace que sea más fácil y natural realizar pruebas unitarias de la funcionalidad de la aplicación. También habilita un flujo de trabajo de desarrollo controlado por pruebas (TDD) que permite el desarrollo basado en pruebas primero.

### <a name="nerddinnertests-project"></a>Proyecto NerdDinner. tests

Cuando creamos la aplicación NerdDinner al principio de este tutorial, se le ha preguntado un cuadro de diálogo en el que se le pregunta si deseamos crear un proyecto de prueba unitaria para ir junto con el proyecto de aplicación:

![](enable-automated-unit-testing/_static/image1.png)

Mantuvimos el botón de radio "sí, crear un proyecto de prueba unitaria" seleccionado, lo que dio lugar a que se agregara un proyecto "NerdDinner. tests" a nuestra solución:

![](enable-automated-unit-testing/_static/image2.png)

El proyecto NerdDinner. tests hace referencia al ensamblado del proyecto de aplicación NerdDinner y nos permite agregar fácilmente las pruebas automatizadas que comprueban la funcionalidad de la aplicación.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Crear pruebas unitarias para nuestra clase de modelo cena

Vamos a agregar algunas pruebas al proyecto NerdDinner. tests que comprueban la clase cena que creamos cuando construimos nuestra capa de modelo.

Comenzaremos por crear una nueva carpeta en nuestro proyecto de prueba denominado "models", donde colocaremos las pruebas relacionadas con el modelo. A continuación, hacemos clic con el botón derecho en la carpeta y elegimos el comando de menú **agregar&gt;nueva prueba** . Se abrirá el cuadro de diálogo "Agregar nueva prueba".

Elegiremos crear una "prueba unitaria" y asignarle el nombre "DinnerTest.cs":

![](enable-automated-unit-testing/_static/image3.png)

Cuando hacemos clic en el botón "Aceptar", Visual Studio agregará (y abrirá) un archivo DinnerTest.cs al proyecto:

![](enable-automated-unit-testing/_static/image4.png)

La plantilla de prueba unitaria de Visual Studio predeterminada tiene un montón de código de placa de caldera que me ha encontrado un poco confuso. Vamos a limpiarlo para que solo contenga el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

El atributo [TestClass] de la clase DinnerTest anterior lo identifica como una clase que contendrá las pruebas, así como el código de inicialización y desmontaje de prueba opcional. Podemos definir pruebas en ella agregando métodos públicos que tengan un atributo [TestMethod] en ellos.

A continuación se muestran las dos primeras pruebas que vamos a agregar a nuestra clase cena. La primera prueba comprueba que nuestra cena no es válida si se crea una nueva cena sin que todas las propiedades se establezcan correctamente. La segunda prueba comprueba que nuestra cena es válida cuando una cena tiene todas las propiedades establecidas con valores válidos:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Observará que los nombres de las pruebas son muy explícitos (y ligeramente detallados). Estamos haciendo esto porque podríamos acabar creando cientos o miles de pruebas pequeñas, y queremos facilitar la determinación rápida de la intención y el comportamiento de cada uno de ellos (especialmente cuando se examina una lista de errores en un ejecutor de pruebas). Los nombres de las pruebas deben tener el nombre de la funcionalidad que están probando. Por encima, se usa un patrón de nomenclatura "Sustantivo\_debe\_verbo".

Vamos a estructurar las pruebas mediante el patrón de prueba "AAA", que significa "Arrange, Act, Assert":

- Organizar: configurar la unidad que se está probando
- Act: ejecute la unidad en pruebas y Capture los resultados
- Assert: comprueba el comportamiento

Cuando escribimos pruebas, queremos evitar que las pruebas individuales hagan demasiado. En su lugar, cada prueba debe comprobar un único concepto (lo que hará que sea mucho más fácil identificar la causa de los errores). Una buena directriz es probar y solo tener una única instrucción Assert para cada prueba. Si tiene más de una instrucción Assert en un método de prueba, asegúrese de que se usan para probar el mismo concepto. En casos de duda, realice otra prueba.

### <a name="running-tests"></a>Ejecutar pruebas

Visual Studio 2008 Professional (y ediciones posteriores) incluye un ejecutor de pruebas integrado que se puede usar para ejecutar proyectos de prueba unitaria de Visual Studio en el IDE. Podemos seleccionar el comando de menú **prueba-&gt;ejecutar-&gt;todas las pruebas en la solución** (o escribir Ctrl R, a) para ejecutar todas las pruebas unitarias. O, como alternativa, se puede colocar el cursor dentro de una clase de prueba o un método de prueba concretos y usar las **pruebas test-&gt;Run-&gt;en el comando de menú contextual actual** (o escribir Ctrl R, t) para ejecutar un subconjunto de las pruebas unitarias.

Vamos a colocar el cursor dentro de la clase DinnerTest y escribir "Ctrl R, T" para ejecutar las dos pruebas recién definidas. Cuando lo hagamos, aparecerá una ventana "Resultados de pruebas" en Visual Studio y veremos los resultados de la serie de pruebas que se muestran en ella:

![](enable-automated-unit-testing/_static/image5.png)

*Nota: de forma predeterminada, la ventana Resultados de pruebas de VS no muestra la columna Nombre de clase. Para agregar esto, haga clic con el botón derecho en la ventana de Resultados de pruebas y use el comando de menú Agregar o quitar columnas.*

Nuestras dos pruebas han tardado solo una fracción de segundo en ejecutarse y, como puede ver, ambos pasan. Ahora podemos continuar y aumentar su capacidad creando pruebas adicionales que comprueban validaciones de reglas específicas, así como abarcar los dos métodos auxiliares: IsUserHost () y IsUserRegistered (), que hemos agregado a la clase cena. Tener todas estas pruebas en su lugar para la clase cena hará que sea mucho más fácil y seguro agregar nuevas reglas de negocio y validaciones en el futuro. Podemos agregar la nueva lógica de regla a la cena y, en cuestión de segundos, comprobar que no ha interrumpido ninguna de nuestras funciones lógicas anteriores.

Observe que el uso de un nombre de prueba descriptivo facilita la comprensión rápida de lo que está comprobando cada prueba. Recomiendo usar el comando **de menú herramientas-&gt;opciones** , abrir la pantalla herramientas de prueba-&gt;probar la configuración de ejecución y activar la casilla "hacer doble clic en un resultado de pruebas unitarias con errores o no concluyente" muestra el punto de error en la comprobación ". Esto le permitirá hacer doble clic en un error en la ventana Resultados de pruebas y pasar inmediatamente al error de aserción.

### <a name="creating-dinnerscontroller-unit-tests"></a>Crear pruebas unitarias de DinnersController

Ahora vamos a crear algunas pruebas unitarias que comprueben la funcionalidad de DinnersController. Comenzaremos haciendo clic con el botón derecho en la carpeta "Controllers" en nuestro proyecto de prueba y, a continuación, elegiremos el comando de menú **agregar&gt;nueva prueba** . Vamos a crear una "prueba unitaria" y le asignaremos el nombre "DinnersControllerTest.cs".

Vamos a crear dos métodos de prueba que comprueban el método de acción Details () en DinnersController. La primera comprobará que se devuelve una vista cuando se solicita una cena existente. La segunda comprobará que se devuelve una vista "NotFound" cuando se solicita una cena no existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

El código anterior compila Clean. Sin embargo, cuando se ejecutan las pruebas, se produce un error:

![](enable-automated-unit-testing/_static/image6.png)

Si observamos los mensajes de error, veremos que la razón por la que se produjo un error en las pruebas fue porque nuestra clase DinnersRepository no pudo conectarse a una base de datos. Nuestra aplicación NerdDinner usa una cadena de conexión a un archivo de SQL Server Express local que reside en el directorio \App\_data del proyecto de aplicación NerdDinner. Dado que nuestro proyecto NerdDinner. tests se compila y se ejecuta en un directorio diferente, el proyecto de aplicación, la ubicación de la ruta de acceso relativa de la cadena de conexión es incorrecta.

Para *solucionar este* problema, copie el archivo de base de datos de SQL Express en nuestro proyecto de prueba y, a continuación, agregue una cadena de conexión de prueba adecuada en el archivo app. config del proyecto de prueba. Esto obtendría las pruebas anteriores desbloqueadas y en ejecución.

Sin embargo, el código de las pruebas unitarias que usan una base de datos real aporta una serie de desafíos. De manera específica:

- Reduce considerablemente el tiempo de ejecución de las pruebas unitarias. Cuanto más tiempo se tarde en ejecutar las pruebas, menos probable es que se ejecuten con frecuencia. Lo ideal es que desee que las pruebas unitarias se ejecuten en segundos, y que sea algo que haga tan natural como compilar el proyecto.
- Complica la lógica de instalación y limpieza en las pruebas. Desea que cada prueba unitaria se aísle e independiente de otras (sin efectos secundarios o dependencias). Al trabajar con una base de datos real, tiene que tener en cuenta el estado y restablecerla entre las pruebas.

Echemos un vistazo a un patrón de diseño denominado "inserción de dependencias" que puede ayudarnos a solucionar estos problemas y evitar la necesidad de usar una base de datos real con nuestras pruebas.

### <a name="dependency-injection"></a>Inserción de dependencias

En la actualidad, DinnersController está estrechamente "acoplada" a la clase DinnerRepository. "Acoplamiento" hace referencia a una situación en la que una clase se basa explícitamente en otra clase para poder funcionar:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Dado que la clase DinnerRepository requiere acceso a una base de datos, la dependencia estrechamente acoplada que la clase DinnersController tiene en la DinnerRepository termina requiriendo tener una base de datos para que se puedan probar los métodos de acción DinnersController.

Podemos evitar esto empleando un modelo de diseño denominado "inserción de dependencias", que es un enfoque en el que las dependencias (como las clases de repositorio que proporcionan acceso a los datos) ya no se crean de forma implícita dentro de las clases que las utilizan. En su lugar, las dependencias se pueden pasar explícitamente a la clase que las utiliza mediante argumentos de constructor. Si las dependencias se definen mediante interfaces, tenemos la flexibilidad de pasar implementaciones de dependencias "falsas" para escenarios de prueba unitaria. Esto nos permite crear implementaciones de dependencia específicas de la prueba que realmente no requieren acceso a una base de datos.

Para ver esto en acción, vamos a implementar la inserción de dependencias con nuestro DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Extracción de una interfaz IDinnerRepository

El primer paso es crear una nueva interfaz IDinnerRepository que encapsula el contrato del repositorio que nuestros controladores requieren para recuperar y actualizar las cenas.

Para definir este contrato de interfaz manualmente, haga clic con el botón derecho en la carpeta \Models y, a continuación, elija el comando de menú **&gt;agregar nuevo elemento** y cree una nueva interfaz denominada IDinnerRepository.cs.

Como alternativa, podemos usar las herramientas de refactorización integradas en Visual Studio Professional (y ediciones posteriores) para extraer y crear automáticamente una interfaz para nosotros de nuestra clase DinnerRepository existente. Para extraer esta interfaz con VS, simplemente coloque el cursor en el editor de texto en la clase DinnerRepository y, a continuación, haga clic con el botón derecho y elija el comando de menú **refactorizar-&gt;extraer interfaz** :

![](enable-automated-unit-testing/_static/image7.png)

Se abrirá el cuadro de diálogo "Extraer interfaz" y nos pedirá el nombre de la interfaz que se va a crear. De forma predeterminada, se IDinnerRepository y se seleccionan automáticamente todos los métodos públicos de la clase DinnerRepository existente para agregarlos a la interfaz:

![](enable-automated-unit-testing/_static/image8.png)

Cuando hacemos clic en el botón "Aceptar", Visual Studio agregará una nueva interfaz IDinnerRepository a nuestra aplicación:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

Y nuestra clase DinnerRepository existente se actualizará para que implemente la interfaz:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Actualización de DinnersController para admitir la inserción de constructores

Ahora actualizaremos la clase DinnersController para usar la nueva interfaz.

Actualmente, DinnersController está codificado de forma rígida, de modo que su campo "dinnerRepository" siempre es una clase DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Lo cambiaremos para que el campo "dinnerRepository" sea de tipo IDinnerRepository en lugar de DinnerRepository. A continuación, vamos a agregar dos constructores DinnersController públicos. Uno de los constructores permite pasar un IDinnerRepository como argumento. El otro es un constructor predeterminado que usa nuestra implementación de DinnerRepository existente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Dado que ASP.NET MVC crea clases de controlador de forma predeterminada mediante constructores predeterminados, nuestro DinnersController en tiempo de ejecución seguirá usando la clase DinnerRepository para realizar el acceso a los datos.

Ahora podemos actualizar nuestras pruebas unitarias para pasar una implementación de repositorio de cenas "falsa" mediante el constructor de parámetros. Este repositorio de cenas "falso" no necesitará acceso a una base de datos real y, en su lugar, utilizará datos de ejemplo en memoria.

#### <a name="creating-the-fakedinnerrepository-class"></a>Crear la clase FakeDinnerRepository

Vamos a crear una clase FakeDinnerRepository.

Comenzaremos por crear un directorio "falsificaciones" dentro del proyecto NerdDinner. tests y, a continuación, agregaremos una nueva clase FakeDinnerRepository a él (haga clic con el botón derecho en la carpeta y elija **Agregar-&gt;nueva clase**):

![](enable-automated-unit-testing/_static/image9.png)

Actualizaremos el código para que la clase FakeDinnerRepository implemente la interfaz IDinnerRepository. A continuación, podemos hacer clic con el botón derecho en él y elegir el comando de menú contextual "implementar interfaz IDinnerRepository":

![](enable-automated-unit-testing/_static/image10.png)

Esto hará que Visual Studio agregue automáticamente todos los miembros de la interfaz IDinnerRepository a nuestra clase FakeDinnerRepository con implementaciones de "código auxiliar" predeterminadas:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

A continuación, podemos actualizar la implementación de FakeDinnerRepository para que funcione fuera de una lista en memoria&lt;cena&gt; colección que se le ha pasado como argumento de constructor:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Ahora tenemos una implementación de IDinnerRepository falsa que no necesita una base de datos y que, en su lugar, puede trabajar con una lista en memoria de objetos de cenas.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>Usar FakeDinnerRepository con pruebas unitarias

Vamos a volver a las pruebas unitarias de DinnersController que dieron error antes porque la base de datos no estaba disponible. Podemos actualizar los métodos de prueba para usar un FakeDinnerRepository rellenado con datos de cenas en memoria de ejemplo para DinnersController mediante el código siguiente:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

Y ahora, cuando ejecutamos estas pruebas, ambos pasan:

![](enable-automated-unit-testing/_static/image11.png)

Lo mejor de todo es que solo tardan una fracción de un segundo en ejecutarse y no requieren lógica complicada de instalación y limpieza. Ahora podemos realizar pruebas unitarias de todo el código del método de acción DinnersController (incluido el listado, la paginación, los detalles, la creación, la actualización y la eliminación) sin necesidad de conectarse a una base de datos real.

| **Tema secundario: marcos de inserción de dependencias** |
| --- |
| La realización manual de la inserción de dependencias (como hemos superado) funciona bien, pero se hace más difícil de mantener a medida que aumenta el número de dependencias y componentes de una aplicación. Existen varios marcos de inserción de dependencias para .NET que pueden ayudar a proporcionar aún más flexibilidad de administración de dependencias. Estos marcos de trabajo, a veces denominados "inversion of control" (IoC), proporcionan mecanismos que permiten un nivel adicional de compatibilidad de configuración para especificar y pasar dependencias a objetos en tiempo de ejecución (la mayoría de las veces se usa la inserción de constructores. ). Algunos de los marcos de ejemplo de inserción de dependencias de OSS más populares en .NET son: AutoFac, Ninject, Spring.NET, StructureMap y Windsor. ASP.NET MVC expone API de extensibilidad que permiten a los desarrolladores participar en la resolución y la creación de instancias de controladores, y lo que permite que los marcos de inserción de dependencias y de IoC estén perfectamente integrados dentro de este proceso. El uso de un marco de DI/IOC también nos permite quitar el constructor predeterminado de nuestro DinnersController, que quitaría por completo el acoplamiento entre él y el DinnerRepository. No vamos a usar un marco de inserción de dependencias/IOC con nuestra aplicación NerdDinner. Pero es algo que podríamos tener en cuenta en el futuro si la base de código y las funcionalidades de NerdDinner han crecido. |

### <a name="creating-edit-action-unit-tests"></a>Crear pruebas unitarias de acción de edición

Ahora vamos a crear algunas pruebas unitarias que comprueben la funcionalidad de edición de DinnersController. Comenzaremos probando la versión HTTP-GET de nuestra acción de edición:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Vamos a crear una prueba que comprueba que una vista respaldada por un objeto DinnerFormViewModel se representará cuando se solicite una cena válida:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

Sin embargo, cuando se ejecuta la prueba, veremos que se produce un error porque se produce una excepción de referencia nula cuando el método Edit tiene acceso a la propiedad User.Identity.Name para realizar la comprobación cena. IsHostedBy ().

El objeto de usuario de la clase base del controlador encapsula los detalles sobre el usuario que ha iniciado sesión y lo rellena ASP.NET MVC cuando crea el controlador en tiempo de ejecución. Dado que estamos probando el DinnersController fuera de un entorno de servidor Web, no se establece el objeto de usuario (por lo tanto, la excepción de referencia nula).

### <a name="mocking-the-useridentityname-property"></a>Simular la propiedad User.Identity.Name

Los marcos ficticios facilitan la realización de pruebas, ya que permiten crear dinámicamente versiones falsas de objetos dependientes que admiten nuestras pruebas. Por ejemplo, podemos utilizar un marco ficticio en nuestra prueba de acción de edición para crear dinámicamente un objeto de usuario que nuestro DinnersController puede usar para buscar un nombre de usuario simulado. Esto evitará que se produzca una referencia nula cuando se ejecute la prueba.

Hay muchos marcos de trabajo ficticios de .NET que se pueden usar con ASP.NET MVC (puede ver una lista de ellos aquí: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Para probar la aplicación NerdDinner, usaremos un marco ficticio de código abierto denominado "MOQ", que se puede descargar de forma gratuita desde [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

Una vez descargado, agregaremos una referencia en el proyecto NerdDinner. tests al ensamblado MOQ. dll:

![](enable-automated-unit-testing/_static/image12.png)

Después, agregaremos un método auxiliar "CreateDinnersControllerAs (username)" a nuestra clase de prueba que toma un nombre de usuario como parámetro y que, a continuación, "simula" la propiedad User.Identity.Name en la instancia de DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Por encima, se usa MOQ para crear un objeto ficticio que falsifica un objeto ControllerContext (que es lo que ASP.NET MVC pasa a las clases de controlador para exponer objetos en tiempo de ejecución como usuario, solicitud, respuesta y sesión). Estamos llamando al método "SetupGet" en el simulacro para indicar que la propiedad HttpContext.User.Identity.Name de ControllerContext debe devolver la cadena de nombre de usuario pasada al método auxiliar.

Se puede simular cualquier número de métodos y propiedades de ControllerContext. Para ilustrar esto, también he agregado una llamada a SetupGet () para la propiedad request. IsAuthenticated (que realmente no es necesaria para las pruebas siguientes, pero que ayuda a ilustrar cómo se pueden simular las propiedades de la solicitud). Cuando se hace, se asigna una instancia del simulacro ControllerContext a la DinnersController que el método auxiliar devuelve.

Ahora podemos escribir pruebas unitarias que utilicen este método auxiliar para probar escenarios de edición que impliquen a distintos usuarios:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

Y ahora cuando se ejecutan las pruebas que superan:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>Prueba de escenarios de UpdateModel ()

Hemos creado pruebas que cubren la versión HTTP-GET de la acción de edición. Vamos a crear ahora algunas pruebas que comprueban la versión HTTP-POST de la acción de edición:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

El nuevo escenario interesante de pruebas para nosotros es compatible con este método de acción es el uso del método auxiliar UpdateModel () en la clase base del controlador. Usamos este método auxiliar para enlazar valores de envío de formulario a nuestra instancia de objeto cena.

A continuación se muestran dos pruebas que muestran cómo se pueden proporcionar valores publicados de formulario para que los use el método auxiliar UpdateModel (). Haremos esto creando y rellenando un objeto FormCollection y, a continuación, asígnelo a la propiedad "ValueProvider" en el controlador.

La primera prueba comprueba que, si se guarda correctamente, el explorador se redirige a la acción de detalles. La segunda prueba comprueba que, cuando se publica una entrada no válida, la acción vuelve a mostrar la vista de edición con un mensaje de error.

[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Realización de pruebas

Hemos tratado los conceptos básicos implicados en las clases de controlador de pruebas unitarias. Podemos usar estas técnicas para crear fácilmente cientos de pruebas simples que comprueban el comportamiento de la aplicación.

Dado que nuestras pruebas de modelo y controlador no requieren una base de datos real, son muy rápidas y fáciles de ejecutar. Podremos ejecutar cientos de pruebas automatizadas en solo unos segundos y obtener comentarios de inmediato sobre si un cambio que hemos realizado ha roto algo. Esto le ayudará a proporcionarnos la confianza de mejorar, refactorizar y refinar la aplicación continuamente.

Hemos tratado las pruebas como el último tema de este capítulo, pero no porque las pruebas son algo que debería hacer al final de un proceso de desarrollo. Por el contrario, debe escribir pruebas automatizadas lo antes posible en el proceso de desarrollo. Esto le permite obtener comentarios inmediatos a medida que desarrolla, le ayuda a pensar de forma exhaustiva en los escenarios de casos de uso de su aplicación y le guía para diseñar la aplicación con capas limpias y acoplamiento en mente.

En un capítulo posterior del libro se tratará el desarrollo controlado por pruebas (TDD) y cómo usarlo con ASP.NET MVC. TDD es una práctica de codificación iterativa en la que se escriben primero las pruebas que satisfará el código resultante. Con TDD, cada característica se inicia creando una prueba que comprueba la funcionalidad que va a implementar. Escribir la prueba unitaria primero ayuda a asegurarse de que comprende claramente la característica y cómo debe funcionar. Solo después de escribir la prueba (y haber comprobado que se produce un error), se implementa la funcionalidad real que comprueba la prueba. Dado que ya ha dedicado tiempo a pensar en el caso de uso de cómo debe funcionar la característica, tendrá una mejor comprensión de los requisitos y la mejor manera de implementarlos. Cuando haya terminado la implementación, puede volver a ejecutar la prueba y obtener comentarios inmediatos sobre si la característica funciona correctamente. Veremos más en el capítulo 10.

### <a name="next-step"></a>Paso siguiente

Algunos comentarios finales de resumen.

> [!div class="step-by-step"]
> [Anterior](use-ajax-to-implement-mapping-scenarios.md)
> [Siguiente](nerddinner-wrap-up.md)
