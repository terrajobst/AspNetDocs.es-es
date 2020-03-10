---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: '#6 de iteración: Use el desarrollo controladoC#por pruebas () | Microsoft Docs'
author: microsoft
description: En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración,...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: aee0ff9d8d7f17e8a00dab12467bd3a3457fbe18
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78487129"
---
# <a name="iteration-6--use-test-driven-development-c"></a>#6 de iteración: usar el desarrollo controladoC#por pruebas ()

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, se agregan grupos de contactos.

## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Creación de una aplicación ASP.NET MVC de administraciónC#de contactos ()

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

En la iteración anterior de la aplicación Contact Manager, creamos pruebas unitarias para proporcionar una red de seguridad para el código. El motivo por el que se crean las pruebas unitarias era hacer que el código fuera más resistente a los cambios. Una vez realizadas las pruebas unitarias, podemos realizar cualquier cambio en nuestro código e saber inmediatamente si hemos interrumpido la funcionalidad existente.

En esta iteración, usamos pruebas unitarias para un propósito totalmente diferente. En esta iteración, usamos pruebas unitarias como parte de una filosofía del diseño de aplicaciones denominada *desarrollo controlado por pruebas*. Cuando se practica el desarrollo controlado por pruebas, se escriben primero las pruebas y, a continuación, se escribe código en las pruebas.

Más concretamente, cuando se practica el desarrollo controlado por pruebas, hay tres pasos que se completan al crear el código (rojo, verde o refactorizar):

1. Escribir una prueba unitaria que produzca un error (rojo)
2. Escribir código que pase la prueba unitaria (verde)
3. Refactorizar el código (refactorizar)

En primer lugar, escriba la prueba unitaria. La prueba unitaria debe expresar su intención de cómo espera que se comporte el código. La primera vez que se crea la prueba unitaria, se debe producir un error en la prueba unitaria. La prueba debe producir un error porque aún no ha escrito ningún código de aplicación que cumpla la prueba.

A continuación, escriba un solo código suficiente para que la prueba unitaria se pase. El objetivo es escribir el código en Laziest, sloppiest y la manera más rápida posible. No debe desperdiciar tiempo pensando en la arquitectura de la aplicación. En su lugar, debe centrarse en escribir la cantidad mínima de código necesaria para satisfacer la intención expresada por la prueba unitaria.

Por último, después de haber escrito suficiente código, puede depurar y tener en cuenta la arquitectura global de la aplicación. En este paso, se vuelve a escribir (refactorizar) el código mediante el uso de los patrones de diseño de software, como el patrón de repositorio, para que el código sea más fácil de mantener. Puede sin miedo reescribir el código en este paso porque el código está incluido en las pruebas unitarias.

Hay muchas ventajas derivadas de la práctica del desarrollo controlado por pruebas. En primer lugar, el desarrollo controlado por pruebas obliga a centrarse en el código que realmente se necesita escribir. Dado que se centra constantemente en escribir suficiente código para pasar una prueba determinada, no se le impide atravesar las malas y escribir grandes cantidades de código que nunca usará.

En segundo lugar, una metodología de diseño "probar primero" obliga a escribir código desde la perspectiva de cómo se utilizará el código. En otras palabras, al practicar el desarrollo controlado por pruebas, está escribiendo constantemente las pruebas desde la perspectiva del usuario. Por lo tanto, el desarrollo controlado por pruebas puede dar lugar a API más claras y comprensibles.

Por último, el desarrollo controlado por pruebas obliga a escribir pruebas unitarias como parte del proceso normal de escritura de una aplicación. A medida que los enfoques de fecha límite del proyecto, la prueba es normalmente lo primero que sale de la ventana. Por otro lado, al practicar el desarrollo controlado por pruebas, es más probable que se uso realmente virtuoso sobre la escritura de pruebas unitarias, ya que el desarrollo controlado por pruebas hace que las pruebas unitarias sean esenciales para el proceso de creación de una aplicación.

> [!NOTE] 
> 
> Para obtener más información sobre el desarrollo controlado por pruebas, recomiendo que lea el libro de Michael plumas que **funciona de forma eficaz con el código heredado**.

En esta iteración, se agrega una nueva característica a nuestra aplicación Contact Manager. Agregamos compatibilidad con grupos de contactos. Puede usar grupos de contactos para organizar los contactos en categorías como empresa y grupos de confianza.

Agregaremos esta nueva funcionalidad a nuestra aplicación siguiendo un proceso de desarrollo controlado por pruebas. Escribiremos primero nuestras pruebas unitarias y escribiremos todo el código en estas pruebas.

## <a name="what-gets-tested"></a>Qué se prueba

Como se explicó en la iteración anterior, normalmente no se escriben pruebas unitarias para lógica de acceso a datos o lógica de vista. No se escriben pruebas unitarias para la lógica de acceso a datos porque el acceso a una base de datos es una operación relativamente lenta. No se escriben pruebas unitarias para la lógica de vista porque el acceso a una vista requiere la rotación de un servidor Web que es una operación relativamente lenta. No debe escribir una prueba unitaria a menos que la prueba se pueda ejecutar una y otra vez muy rápido.

Dado que el desarrollo controlado por pruebas está controlado por pruebas unitarias, nos centramos inicialmente en el controlador y la lógica de negocios. Evitamos tocar la base de datos o las vistas. No se modificará la base de datos ni se crearán las vistas hasta el final de este tutorial. Comenzamos con lo que se puede probar.

## <a name="creating-user-stories"></a>Creación de casos de usuario

Al practicar el desarrollo controlado por pruebas, siempre se empieza por escribir una prueba. Esto genera inmediatamente la pregunta: ¿cómo decide qué prueba escribir primero? Para responder a esta pregunta, debe escribir un conjunto de [**casos de usuario**](http://en.wikipedia.org/wiki/User_stories).

Un caso de usuario es una descripción breve (normalmente una oración) de un requisito de software. Debe ser una descripción no técnica de un requisito escrito desde la perspectiva del usuario.

Este es el conjunto de casos de usuario que describen las características requeridas por la nueva funcionalidad de grupo de contactos:

1. El usuario puede ver una lista de grupos de contactos.
2. El usuario puede crear un nuevo grupo de contactos.
3. El usuario puede eliminar un grupo de contactos existente.
4. El usuario puede seleccionar un grupo de contactos al crear un nuevo contacto.
5. El usuario puede seleccionar un grupo de contactos al editar un contacto existente.
6. En la vista de índice se muestra una lista de grupos de contactos.
7. Cuando un usuario hace clic en un grupo de contactos, se muestra una lista de contactos coincidentes.

Tenga en cuenta que la lista de casos de usuario es totalmente comprensible para un cliente. No hay ninguna mención de los detalles de la implementación técnica.

Aunque en el proceso de compilación de la aplicación, el conjunto de casos de usuario puede volverse más refinado. Podría dividir un caso de usuario en varios casos (requisitos). Por ejemplo, podría decidir que la creación de un nuevo grupo de contactos debe implicar la validación. El envío de un grupo de contactos sin nombre debe devolver un error de validación.

Después de crear una lista de casos de usuario, está listo para escribir su primera prueba unitaria. Comenzaremos por crear una prueba unitaria para ver la lista de grupos de contactos.

## <a name="listing-contact-groups"></a>Enumerar grupos de contactos

Nuestro primer caso de usuario es que un usuario debe ser capaz de ver una lista de grupos de contactos. Necesitamos expresar este caso con una prueba.

Cree una nueva prueba unitaria haciendo clic con el botón derecho en la carpeta Controllers del proyecto ContactManager. tests, seleccionando **Agregar, nueva prueba**y seleccionando la plantilla de **prueba unitaria** (consulte la figura 1). Asigne a la nueva prueba unitaria el nombre GroupControllerTest.cs y haga clic en el botón **Aceptar** .

[![agregar la prueba unitaria GroupControllerTest](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**Figura 01**: adición de la prueba unitaria GroupControllerTest ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image2.png))

La primera prueba unitaria está incluida en la lista 1. Esta prueba comprueba que el método index () del controlador de grupo devuelve un conjunto de grupos. La prueba comprueba que se devuelve una colección de grupos en los datos de la vista.

**Lista 1-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

La primera vez que escriba el código en la lista 1 de Visual Studio, obtendrá una gran cantidad de líneas onduladas rojas. No hemos creado las clases GroupController o Group.

En este momento, podemos compilar la aplicación para que se pueda ejecutar la primera prueba unitaria. Eso es bueno. Esto cuenta como una prueba con errores. Por lo tanto, ahora tenemos permiso para empezar a escribir el código de la aplicación. Necesitamos escribir código suficiente para ejecutar la prueba.

La clase de controlador de grupo de la lista 2 contiene el mínimo de código necesario para pasar la prueba unitaria. La acción index () devuelve una lista de grupos codificada estáticamente (la clase Group se define en la lista 3).

**Lista 2-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**Lista 3: Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

Después de agregar las clases GroupController y Group a nuestro proyecto, nuestra primera prueba unitaria se completa correctamente (consulte la figura 2). Hemos realizado el mínimo trabajo necesario para pasar la prueba. Es el momento de celebrarlo.

[![correcto.](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**Figura 02**: correcto. ([Haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image4.png))

## <a name="creating-contact-groups"></a>Crear grupos de contactos

Ahora podemos pasar al segundo caso de usuario. Es necesario poder crear nuevos grupos de contactos. Necesitamos expresar esta intención con una prueba.

La prueba de la lista 4 comprueba que la llamada al método Create () con un nuevo grupo agrega el grupo a la lista de grupos devueltos por el método index (). En otras palabras, si creo un nuevo grupo, debería poder volver a obtener el nuevo grupo desde la lista de grupos devueltos por el método index ().

**Lista 4: Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

La prueba de la lista 4 llama al método Create () del controlador de grupo con un nuevo grupo de contactos. A continuación, la prueba comprueba que la llamada al método Group Controller index () devuelve el nuevo grupo en los datos de la vista.

El controlador de grupo modificado de la lista 5 contiene el mínimo de cambios necesarios para pasar la nueva prueba.

**Lista 5-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>El controlador de grupo de la lista 5 tiene una nueva acción Create (). Esta acción agrega un grupo a una colección de grupos. Observe que la acción index () se ha modificado para devolver el contenido de la colección de grupos.

Una vez más, hemos realizado la cantidad mínima de trabajo necesaria para pasar la prueba unitaria. Después de realizar estos cambios en el controlador de grupo, todas nuestras pruebas unitarias pasan.

## <a name="adding-validation"></a>Adición de validación

Este requisito no se indicó explícitamente en el caso de usuario. Sin embargo, es razonable requerir que un grupo tenga un nombre. De lo contrario, la organización de contactos en grupos no sería muy útil.

La lista 6 contiene una nueva prueba que expresa esta intención. Esta prueba comprueba que al intentar crear un grupo sin proporcionar un nombre se genera un mensaje de error de validación en el estado del modelo.

**Listado 6-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

Para satisfacer esta prueba, es necesario agregar una propiedad Name a nuestra clase Group (consulte la lista 7). Además, es necesario agregar una pequeña parte de la lógica de validación a nuestra acción de creación () del controlador de grupo (consulte la lista 8).

**Listado 7-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**Lista 8-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

Tenga en cuenta que la acción de creación () del controlador de grupo ahora contiene la validación y la lógica de base de datos. Actualmente, la base de datos utilizada por el controlador de grupo no está formada por nada más que una colección en memoria.

## <a name="time-to-refactor"></a>Tiempo para refactorizar

El tercer paso en rojo/verde/refactorizar es la parte del refactorización. Llegados a este punto, necesitamos devolverlo de nuestro código y considerar cómo podemos refactorizar la aplicación para mejorar su diseño. La fase de refactorización es la fase en la que pensamos en la mejor manera de implementar los principios y patrones de diseño de software.

Tenemos la posibilidad de modificar el código de cualquier manera que elijamos para mejorar el diseño del código. Tenemos una red de seguridad de pruebas unitarias que nos impiden interrumpir la funcionalidad existente.

En este momento, el controlador de grupo es un desastre desde la perspectiva del buen diseño de software. El controlador de grupo contiene un desenredado de la validación y el código de acceso a datos. Para evitar infringir el principio de responsabilidad única, es necesario separar estas preocupaciones en clases diferentes.

Nuestra clase de controlador de grupo refactorizada se incluye en la lista 9. El controlador se ha modificado para usar el nivel de servicio ContactManager. Este es el mismo nivel de servicio que usamos con el controlador de contactos.

La lista 10 contiene los nuevos métodos agregados a la capa de servicio de ContactManager para admitir la validación, la enumeración y la creación de grupos. La interfaz IContactManagerService se actualizó para incluir los nuevos métodos.

La lista 11 contiene una nueva clase FakeContactManagerRepository que implementa la interfaz IContactManagerRepository. A diferencia de la clase EntityContactManagerRepository que también implementa la interfaz IContactManagerRepository, nuestra nueva clase FakeContactManagerRepository no se comunica con la base de datos. La clase FakeContactManagerRepository usa una colección en memoria como proxy para la base de datos. Usaremos esta clase en nuestras pruebas unitarias como una capa de repositorio falsa.

**Lista 9-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**Listado 10: Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**Listado 11-Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

La modificación de la interfaz IContactManagerRepository requiere el uso de para implementar los métodos CreateGroup () y ListGroups () en la clase EntityContactManagerRepository. La forma más rápida y Laziest de hacerlo es agregar métodos de código auxiliar que tengan el aspecto siguiente:   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]

Por último, estos cambios en el diseño de nuestra aplicación requieren realizar algunas modificaciones en nuestras pruebas unitarias. Ahora es necesario usar FakeContactManagerRepository al realizar las pruebas unitarias. La clase GroupControllerTest actualizada está incluida en la lista 12.

**Listado 12-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

Después de realizar todos estos cambios, una vez más, todas nuestras pruebas unitarias pasan. Hemos completado todo el ciclo de rojo, verde y refactorización. Hemos implementado los dos primeros casos de usuario. Ahora tenemos compatibilidad con pruebas unitarias para los requisitos expresados en los casos de usuario. Implementar el resto de los casos de usuario implica repetir el mismo ciclo de rojo, verde y refactorización.

## <a name="modifying-our-database"></a>Modificar nuestra base de datos

Desafortunadamente, aunque hemos satisfecho todos los requisitos expresados por nuestras pruebas unitarias, nuestro trabajo no se realiza. Todavía necesitamos modificar nuestra base de datos.

Necesitamos crear una nueva tabla de base de datos de grupo. Siga estos pasos:

1. En la ventana de Explorador de servidores, haga clic con el botón secundario en la carpeta tablas y seleccione la opción de menú **Agregar nueva tabla**.
2. Escriba las dos columnas que se describen a continuación en el Diseñador de tablas.
3. Marque la columna ID como una clave principal y una columna de identidad.
4. Guarde la nueva tabla con los grupos de nombres haciendo clic en el icono del disquete.

<a id="0.11_table01"></a>

| **Nombre de la columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Id. | int | False |
| Name | nvarchar(50) | False |

A continuación, es necesario eliminar todos los datos de la tabla contactos (de lo contrario, no se podrá crear una relación entre las tablas contactos y grupos). Siga estos pasos:

1. Haga clic con el botón secundario en la tabla contactos y seleccione la opción de menú **Mostrar datos**de la tabla.
2. Elimine todas las filas.

A continuación, es necesario definir una relación entre la tabla de base de datos de grupos y la tabla de base de datos de contactos existente. Siga estos pasos:

1. Haga doble clic en la tabla contactos de la ventana Explorador de servidores para abrir el Diseñador de tablas.
2. Agregue una nueva columna de tipo entero a la tabla Contacts denominada GroupId.
3. Haga clic en el botón relación para abrir el cuadro de diálogo relaciones de clave externa (vea la figura 3).
4. Haga clic en el botón Agregar.
5. Haga clic en el botón de puntos suspensivos que aparece junto al botón especificación de tabla y columnas.
6. En el cuadro de diálogo tablas y columnas, seleccione grupos como tabla de clave principal e identificador como columna de clave principal. Seleccione contactos como tabla de clave externa y GroupId como columna de clave externa (consulte la figura 4). Haga clic en el botón Aceptar.
7. En **especificación de inserción y actualización**, seleccione el valor **en cascada** para **eliminar regla**.
8. Haga clic en el botón Cerrar para cerrar el cuadro de diálogo relaciones de clave externa.
9. Haga clic en el botón Guardar para guardar los cambios en la tabla contactos.

[![crear una relación de tabla de base de datos](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**Figura 03**: creación de una relación de tabla de base de datos ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image6.png))

[![especificar relaciones de tabla](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**Figura 04**: especificación de relaciones de tabla ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image8.png))

### <a name="updating-our-data-model"></a>Actualización del modelo de datos

A continuación, es necesario actualizar nuestro modelo de datos para representar la nueva tabla de base de datos. Siga estos pasos:

1. Haga doble clic en el archivo ContactManagerModel. edmx en la carpeta models para abrir el diseñador de entidades.
2. Haga clic con el botón secundario en la superficie del diseñador y seleccione la opción **de menú Actualizar modelo desde base de datos**.
3. En el Asistente para actualización, seleccione la tabla grupos y haga clic en el botón Finalizar (vea la figura 5).
4. Haga clic con el botón secundario en la entidad grupos y seleccione la opción de menú **cambiar nombre**. Cambie el nombre de la entidad *Groups* a *Group* (singular).
5. Haga clic con el botón secundario en la propiedad de navegación grupos que aparece en la parte inferior de la entidad contacto. Cambie el nombre de la propiedad de navegación *Groups* a *Group* (singular).

[![actualizar un modelo de Entity Framework desde la base de datos](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**Figura 05**: actualización de un modelo de Entity Framework desde la base de datos ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image10.png))

Después de completar estos pasos, el modelo de datos representará las tablas contactos y grupos. Entity Designer debería mostrar ambas entidades (vea la ilustración 6).

[![Entity Designer mostrando el grupo y el contacto](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**Figura 06**: diseñador de entidades que muestra el grupo y el contacto ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image12.png))

### <a name="creating-our-repository-classes"></a>Creación de las clases de repositorio

A continuación, es necesario implementar nuestra clase de repositorio. En el transcurso de esta iteración, agregamos varios métodos nuevos a la interfaz IContactManagerRepository mientras escribía código para satisfacer nuestras pruebas unitarias. La versión final de la interfaz IContactManagerRepository se incluye en la lista 14.

**Listado 14-Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

No hemos implementado realmente ninguno de los métodos relacionados con el trabajo con grupos de contactos. Actualmente, la clase EntityContactManagerRepository tiene métodos de código auxiliar para cada uno de los métodos de grupo Contact enumerados en la interfaz IContactManagerRepository. Por ejemplo, el método ListGroups () tiene actualmente el siguiente aspecto:

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

Los métodos de código auxiliar nos han permitido compilar la aplicación y pasar las pruebas unitarias. Sin embargo, ahora es el momento de implementar estos métodos. La versión final de la clase EntityContactManagerRepository se encuentra en la lista 13.

**Lista 13-Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>Crear las vistas

Aplicación ASP.NET MVC cuando se usa el motor de vista ASP.NET predeterminado. Por lo tanto, no cree vistas en respuesta a una prueba unitaria determinada. Sin embargo, dado que una aplicación sería inútil sin vistas, podemos completar esta iteración sin crear y modificar las vistas contenidas en la aplicación Contact Manager.

Necesitamos crear las siguientes vistas nuevas para administrar grupos de contactos (consulte la figura 7):

- Views\Group\Index.aspx: muestra la lista de grupos de contactos
- Views\Group\Delete.aspx: muestra el formulario de confirmación para eliminar un grupo de contactos.

[![la vista de índice de grupo](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**Figura 07**: vista de índice de grupo ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image14.png))

Es necesario modificar las siguientes vistas existentes para que incluyan grupos de contactos:

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

Puede ver las vistas modificadas examinando la aplicación de Visual Studio que acompaña a este tutorial. Por ejemplo, la figura 8 muestra la vista de índice de contactos.

[![la vista de índice de contactos](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**Figura 08**: vista de índice de contactos ([haga clic para ver la imagen de tamaño completo](iteration-6-use-test-driven-development-cs/_static/image16.png))

## <a name="summary"></a>Resumen

En esta iteración, se ha agregado una nueva funcionalidad a nuestra aplicación de administrador de contactos siguiendo una metodología de diseño de aplicaciones de desarrollo controlado por pruebas. Comenzamos por crear un conjunto de casos de usuario. Hemos creado un conjunto de pruebas unitarias que corresponde a los requisitos expresados por los casos de usuario. Por último, escribimos solo código suficiente para satisfacer los requisitos expresados por las pruebas unitarias.

Una vez finalizada la escritura de código suficiente para satisfacer los requisitos expresados por las pruebas unitarias, hemos actualizado nuestra base de datos y las vistas. Hemos agregado una nueva tabla grupos a nuestra base de datos y hemos actualizado nuestro Entity Framework modelo de datos. También creamos y modificamos un conjunto de vistas.

En la siguiente iteración (la iteración final), se vuelve a escribir la aplicación para aprovechar las ventajas de Ajax. Al aprovechar las ventajas de Ajax, mejoraremos la capacidad de respuesta y el rendimiento de la aplicación Contact Manager.

> [!div class="step-by-step"]
> [Anterior](iteration-5-create-unit-tests-cs.md)
> [Siguiente](iteration-7-add-ajax-functionality-cs.md)
