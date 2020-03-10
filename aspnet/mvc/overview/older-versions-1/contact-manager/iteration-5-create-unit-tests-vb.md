---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
title: '#5 de iteración: crear pruebas unitarias (VB) | Microsoft Docs'
author: microsoft
description: En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Simulamos nuestras clases de modelo de datos y compilamos pruebas unitarias para o...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: c6e5c036-2265-4fa7-a9eb-47f197bdc262
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-vb
msc.type: authoredcontent
ms.openlocfilehash: 4ce1c6224a7e9203ff62f136f4f3a43e4561a904
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78437839"
---
# <a name="iteration-5--create-unit-tests-vb"></a>#5 de iteración: crear pruebas unitarias (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-5-create-unit-tests-vb/_static/contactmanager_5_vb1.zip)

> En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Hemos simulado nuestras clases de modelo de datos y compilamos pruebas unitarias para nuestros controladores y la lógica de validación.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación ASP.NET MVC de administración de contactos (VB)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto: nombres, números de teléfono y direcciones de correo electrónico, para obtener una lista de personas.

Compilamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de varias iteraciones es permitirle comprender la razón de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y eliminar (CRUD).

- #2 de iteración: haga que la aplicación tenga un buen aspecto. En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos las direcciones de correo electrónico y los números de teléfono.

- #4 de iteración: hacer que la aplicación esté acoplada de forma flexible. En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y el patrón de inserción de dependencias.

- #5 de iteración: crear pruebas unitarias. En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Hemos simulado nuestras clases de modelo de datos y compilamos pruebas unitarias para nuestros controladores y la lógica de validación.

- #6 de iteración: Use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, se agregan grupos de contactos.

- #7 de iteración: agregar funcionalidad de Ajax. En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En la iteración anterior de la aplicación Contact Manager, hemos refactorizado la aplicación para que esté más acoplada. Dividimos la aplicación en distintas capas de controlador, servicio y repositorio. Cada capa interactúa con la capa que hay debajo a través de las interfaces.

Hemos refactorizado la aplicación para facilitar el mantenimiento y la modificación de la aplicación. Por ejemplo, si necesitamos usar una nueva tecnología de acceso a datos, podemos cambiar simplemente la capa del repositorio sin tocar el nivel de servicio o el controlador. Al hacer que Contact Manager esté débilmente acoplado, se hace que la aplicación sea más resistente a los cambios.

Pero, ¿qué ocurre cuando es necesario agregar una nueva característica a la aplicación Contact Manager? O bien, ¿qué ocurre cuando se corrige un error? Una verdad, pero bien comprobada, de escribir código es que, cada vez que toca código, crea el riesgo de introducir nuevos errores.

Por ejemplo, una vez al día, es posible que el administrador le pida que agregue una nueva característica al administrador de contactos. Desea agregar compatibilidad para los grupos de contactos. Quiere permitir que los usuarios organicen sus contactos en grupos como amigos, negocios, etc.

Para implementar esta nueva característica, deberá modificar las tres capas de la aplicación Contact Manager. Deberá agregar nuevas funcionalidades a los controladores, la capa de servicio y el repositorio. En cuanto empiece a modificar el código, correrá el riesgo de interrumpir las funciones que funcionaban antes.

La refactorización de nuestra aplicación en capas independientes, como hicimos en la iteración anterior, era una buena cosa. Era una buena cosa porque nos permite realizar cambios en las capas completas sin tocar el resto de la aplicación. Sin embargo, si desea que el código de una capa sea más fácil de mantener y modificar, debe crear pruebas unitarias para el código.

Use una prueba unitaria para probar una unidad de código individual. Estas unidades de código son más pequeñas que las capas de aplicación completas. Normalmente, se usa una prueba unitaria para comprobar si un método determinado del código se comporta de la manera esperada. Por ejemplo, debe crear una prueba unitaria para el método CreateContact () expuesto por la clase ContactManagerService.

Las pruebas unitarias para una aplicación funcionan como una red de seguridad. Siempre que modifique el código en una aplicación, puede ejecutar un conjunto de pruebas unitarias para comprobar si la modificación interrumpe la funcionalidad existente. Las pruebas unitarias hacen que el código sea seguro para modificarse. Las pruebas unitarias hacen que todo el código de la aplicación sea más resistente a los cambios.

En esta iteración, se agregan pruebas unitarias a nuestra aplicación de Contact Manager. De este modo, en la siguiente iteración, podemos agregar grupos de contactos a nuestra aplicación sin preocuparse por interrumpir la funcionalidad existente.

> [!NOTE] 
> 
> Hay una variedad de marcos de pruebas unitarias, entre los que se incluyen NUnit, xUnit.net y MbUnit. En este tutorial, usamos el marco de pruebas unitarias incluido con Visual Studio. Sin embargo, podría usar fácilmente uno de estos marcos de trabajo alternativos.

## <a name="what-gets-tested"></a>Qué se prueba

En el mundo perfecto, todo el código estará incluido en las pruebas unitarias. En el mundo perfecto, tendría la red de seguridad perfecta. Puede modificar cualquier línea de código de la aplicación y conocer al instante ejecutando las pruebas unitarias, independientemente de si el cambio ha infringido la funcionalidad existente.

Sin embargo, no vivimos en un mundo perfecto. En la práctica, al escribir pruebas unitarias, se concentra en escribir pruebas para su lógica de negocios (por ejemplo, la lógica de validación). En concreto, *no* se escriben pruebas unitarias para la lógica de acceso a datos ni la lógica de la vista.

Para que sea útil, las pruebas unitarias deben ejecutarse muy rápidamente. Puede acumular fácilmente cientos (o incluso miles) de pruebas unitarias para una aplicación. Si las pruebas unitarias tardan mucho tiempo en ejecutarse, evitará ejecutarlas. En otras palabras, las pruebas unitarias de larga ejecución no son útiles para la codificación diaria.

Por esta razón, normalmente no se escriben pruebas unitarias para el código que interactúa con una base de datos. La ejecución de cientos de pruebas unitarias en una base de datos activa sería demasiado lenta. En su lugar, simulará la base de datos y escribirá código que interactúe con la base de datos ficticia (se describe el simulacro de una base de datos a continuación).

Por una razón similar, normalmente no se escriben pruebas unitarias para las vistas. Para probar una vista, debe poner en marcha un servidor Web. Dado que la rotación de un servidor web es un proceso relativamente lento, no se recomienda crear pruebas unitarias para las vistas.

Si la vista contiene lógica complicada, considere la posibilidad de mover la lógica a métodos auxiliares. Puede escribir pruebas unitarias para métodos auxiliares que se ejecuten sin necesidad de ejecutar un servidor Web.

> [!NOTE] 
> 
> Al escribir las pruebas para la lógica de acceso a datos o la lógica de vista, no es una buena idea al escribir pruebas unitarias, estas pruebas pueden ser muy valiosas al compilar pruebas funcionales o de integración.

> [!NOTE] 
> 
> ASP.NET MVC es el motor de vista de formularios Web Forms. Aunque el motor de vista de formularios Web Forms depende de un servidor Web, puede que otros motores de vista no estén.

## <a name="using-a-mock-object-framework"></a>Usar un marco de objeto ficticio

Al compilar pruebas unitarias, casi siempre es necesario sacar provecho de un marco de objeto ficticio. Un marco de objeto ficticio le permite crear simulacros y códigos auxiliares para las clases de la aplicación.

Por ejemplo, puede usar un marco de objetos ficticios para generar una versión ficticia de la clase de repositorio. De este modo, puede usar la clase de repositorio ficticio en lugar de la clase de repositorio real en las pruebas unitarias. El uso del repositorio ficticio le permite evitar la ejecución de código de base de datos al ejecutar una prueba unitaria.

Visual Studio no incluye un marco de objeto ficticio. Sin embargo, hay varios marcos de objetos ficticios comerciales y de código abierto disponibles para .NET Framework:

1. MOQ: este marco de trabajo está disponible en la licencia BSD de código abierto. Puede descargar MOQ desde [https://code.google.com/p/moq/](https://code.google.com/p/moq/).
2. Rhino Mocks: este marco de trabajo está disponible en la licencia BSD de código abierto. Puede descargar Rhino Mocks de [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).
3. Typemock Isolator: se trata de un marco comercial. Puede descargar una versión de prueba de [http://www.typemock.com/](http://www.typemock.com/).

En este tutorial, decidí usar MOQ. Sin embargo, puede usar fácilmente los simulacros de Rhino o Typemock Isolator para crear los objetos ficticios para la aplicación Contact Manager.

Antes de poder usar MOQ, debe completar los pasos siguientes:

1. .
2. Antes de descomprimir la descarga, asegúrese de hacer clic con el botón secundario en el archivo y haga clic en el botón **desbloquear** (consulte la figura 1).
3. Descomprima la descarga.
4. Agregue una referencia al ensamblado MOQ en el proyecto de prueba; para ello, seleccione la opción de menú **proyecto, agregar referencia** para abrir el cuadro de diálogo **Agregar referencia** . En la pestaña examinar, vaya a la carpeta donde descomprimió MOQ y seleccione el ensamblado MOQ. dll. Haga clic en el botón **Aceptar** (vea la figura 2).

[![desbloquear MOQ](iteration-5-create-unit-tests-vb/_static/image1.jpg)](iteration-5-create-unit-tests-vb/_static/image1.png)

**Figura 01**: desbloqueo de MOQ ([haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-vb/_static/image2.png))

[![referencias después de agregar MOQ](iteration-5-create-unit-tests-vb/_static/image2.jpg)](iteration-5-create-unit-tests-vb/_static/image3.png)

**Figura 02**: referencias después de agregar MOQ ([haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-vb/_static/image4.png))

## <a name="creating-unit-tests-for-the-service-layer"></a>Crear pruebas unitarias para el nivel de servicio

Comencemos por crear un conjunto de pruebas unitarias para la capa de servicio de la aplicación de Contact Manager. Usaremos estas pruebas para comprobar la lógica de validación.

Cree una nueva carpeta denominada models en el proyecto ContactManager. tests. A continuación, haga clic con el botón derecho en la carpeta modelos y seleccione **Agregar, nueva prueba**. Aparece el cuadro de diálogo **Agregar nueva prueba** que se muestra en la figura 3. Seleccione la plantilla de **prueba unitaria** y asigne a la nueva prueba el nombre ContactManagerServiceTest. VB. Haga clic en el botón **Aceptar** para agregar la nueva prueba al proyecto de prueba.

> [!NOTE] 
> 
> En general, desea que la estructura de carpetas del proyecto de prueba coincida con la estructura de carpetas del proyecto de ASP.NET MVC. Por ejemplo, las pruebas de controlador se colocan en una carpeta de controladores, las pruebas de modelo en una carpeta de modelos, etc.

[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-vb/_static/image3.jpg)](iteration-5-create-unit-tests-vb/_static/image5.png)

**Figura 03**: Models\ContactManagerServiceTest.CS ([haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-vb/_static/image6.png))

Inicialmente, queremos probar el método CreateContact () expuesto por la clase ContactManagerService. Vamos a crear las cinco pruebas siguientes:

- CreateContact (): comprueba que CreateContact () devuelve el valor true cuando se pasa un contacto válido al método.
- CreateContactRequiredFirstName (): comprueba que se agrega un mensaje de error al estado del modelo cuando un contacto con un nombre que falta se pasa al método CreateContact ().
- CreateContactRequiredLastName (): comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con un apellido que falta al método CreateContact ().
- CreateContactInvalidPhone (): comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con un número de teléfono no válido al método CreateContact ().
- CreateContactInvalidEmail (): comprueba que se agrega un mensaje de error al estado del modelo cuando se pasa un contacto con una dirección de correo electrónico no válida al método CreateContact ().

La primera prueba comprueba que un contacto válido no genera un error de validación. Las pruebas restantes comprueban cada una de las reglas de validación.

El código de estas pruebas está incluido en la lista 1.

**Lista 1-Models\ContactManagerServiceTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample1.vb)]

Dado que usamos la clase Contact en la lista 1, es necesario agregar una referencia a Microsoft Entity Framework en nuestro proyecto de prueba. Agregue una referencia al ensamblado System. Data. Entity.

La lista 1 contiene un método denominado Initialize () que se decora con el atributo [TestInitialize]. Se llama a este método automáticamente antes de que se ejecute cada una de las pruebas unitarias (se llama 5 veces a la derecha antes de cada una de las pruebas unitarias). El método Initialize () crea un repositorio ficticio con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample2.vb)]

Esta línea de código usa el marco MOQ para generar un repositorio ficticio a partir de la interfaz IContactManagerRepository. El repositorio ficticio se usa en lugar del EntityContactManagerRepository real para evitar el acceso a la base de datos cuando se ejecuta cada prueba unitaria. El repositorio ficticio implementa los métodos de la interfaz IContactManagerRepository, pero los métodos realmente no hacen nada.

> [!NOTE] 
> 
> Al usar el marco de trabajo de MOQ, hay una diferencia entre \_mockRepository \_y el objeto mockRepository. El primero hace referencia a la clase ficticia (of IContactManagerRepository) que contiene métodos para especificar cómo se comportará el repositorio ficticio. Esto último hace referencia al repositorio ficticio real que implementa la interfaz IContactManagerRepository.

El repositorio ficticio se usa en el método Initialize () al crear una instancia de la clase ContactManagerService. Todas las pruebas unitarias individuales usan esta instancia de la clase ContactManagerService.

La lista 1 contiene cinco métodos que corresponden a cada una de las pruebas unitarias. Cada uno de estos métodos se decora con el atributo [TestMethod]. Al ejecutar las pruebas unitarias, se llama a cualquier método que tenga este atributo. En otras palabras, cualquier método que esté decorado con el atributo [TestMethod] es una prueba unitaria.

La primera prueba unitaria, denominada CreateContact (), comprueba que la llamada a CreateContact () devuelve el valor true cuando se pasa al método una instancia válida de la clase contact. La prueba crea una instancia de la clase Contact, llama al método CreateContact () y comprueba que CreateContact () devuelve el valor true.

Las pruebas restantes comprueban que cuando se llama al método CreateContact () con un contacto no válido, el método devuelve false y se agrega el mensaje de error de validación esperado al estado del modelo. Por ejemplo, la prueba CreateContactRequiredFirstName () crea una instancia de la clase contact con una cadena vacía para su propiedad FirstName. A continuación, se llama al método CreateContact () con el contacto no válido. Por último, la prueba comprueba que CreateContact () devuelve false y que el estado del modelo contiene el mensaje de error de validación esperado "el nombre es obligatorio".

Puede ejecutar las pruebas unitarias de la lista 1 seleccionando la opción de menú **probar, ejecutar, todas las pruebas de la solución (Ctrl + R, A)** . Los resultados de las pruebas se muestran en la ventana de Resultados de pruebas (vea la ilustración 4).

[![Resultados de pruebas](iteration-5-create-unit-tests-vb/_static/image4.jpg)](iteration-5-create-unit-tests-vb/_static/image7.png)

**Figura 04**: resultados de pruebas ([haga clic para ver la imagen de tamaño completo](iteration-5-create-unit-tests-vb/_static/image8.png))

## <a name="creating-unit-tests-for-controllers"></a>Crear pruebas unitarias para controladores

La aplicación ASP.NET MVC controla el flujo de interacción del usuario. Al probar un controlador, desea probar si el controlador devuelve el resultado de la acción correcta y los datos de la vista. También podría querer probar si un controlador interactúa con las clases de modelo de la manera esperada.

Por ejemplo, la lista 2 contiene dos pruebas unitarias para el método Create () del controlador de contactos. La primera prueba unitaria comprueba que, cuando se pasa un contacto válido al método Create (), el método Create () redirige a la acción de índice. En otras palabras, cuando se pasa un contacto válido, el método Create () debe devolver un RedirectToRouteResult que representa la acción de índice.

No queremos probar el nivel de servicio de ContactManager cuando se prueba la capa del controlador. Por lo tanto, simulamos el nivel de servicio con el código siguiente en el método Initialize:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample3.vb)]

En la prueba unitaria CreateValidContact (), simulamos el comportamiento de la llamada al método CreateContact () de nivel de servicio con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample4.vb)]

Esta línea de código hace que el servicio ContactManager ficticio devuelva el valor true cuando se llama a su método CreateContact (). Al simular el nivel de servicio, podemos probar el comportamiento de nuestro controlador sin necesidad de ejecutar ningún código en el nivel de servicio.

La segunda prueba unitaria comprueba que la acción Create () devuelve la vista de creación cuando se pasa un contacto no válido al método. Hacemos que el método CreateContact () del nivel de servicio devuelva el valor false con la siguiente línea de código:

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample5.vb)]

Si el método Create () se comporta como se espera, debe devolver la vista Create cuando el nivel de servicio devuelva el valor false. De este modo, el controlador puede mostrar los mensajes de error de validación en la vista de creación y el usuario tiene la oportunidad de corregir las propiedades de contacto no válidas.

Si tiene previsto compilar pruebas unitarias para los controladores, debe devolver nombres de vista explícitos de las acciones del controlador. Por ejemplo, no devuelva una vista similar a la siguiente:

Vista devuelta ()

En su lugar, devuelva la vista de la siguiente manera:

Devolver vista ("crear")

Si no es explícito al devolver una vista, la propiedad ViewResult. ViewName devuelve una cadena vacía.

**Lista 2-Controllers\ContactControllerTest.vb**

[!code-vb[Main](iteration-5-create-unit-tests-vb/samples/sample6.vb)]

## <a name="summary"></a>Resumen

En esta iteración, creamos pruebas unitarias para nuestra aplicación de Contact Manager. Podemos ejecutar estas pruebas unitarias en cualquier momento para comprobar que la aplicación todavía se comporta de la manera esperada. Las pruebas unitarias actúan como una red de seguridad para nuestra aplicación, lo que nos permite modificar de forma segura nuestra aplicación en el futuro.

Hemos creado dos conjuntos de pruebas unitarias. En primer lugar, hemos probado la lógica de validación mediante la creación de pruebas unitarias para la capa de servicio. A continuación, probamos nuestra lógica de control de flujo mediante la creación de pruebas unitarias para la capa de controlador. Al probar el nivel de servicio, hemos aislado nuestras pruebas para nuestro nivel de servicio de nuestra capa de repositorio mediante la simulación de la capa del repositorio. Al probar la capa del controlador, hemos aislado nuestras pruebas para la capa del controlador mediante la simulación del nivel de servicio.

En la siguiente iteración, se modifica la aplicación Contact Manager para que admita grupos de contactos. Agregaremos esta nueva funcionalidad a nuestra aplicación mediante un proceso de diseño de software denominado desarrollo controlado por pruebas.

> [!div class="step-by-step"]
> [Anterior](iteration-4-make-the-application-loosely-coupled-vb.md)
> [Siguiente](iteration-6-use-test-driven-development-vb.md)
