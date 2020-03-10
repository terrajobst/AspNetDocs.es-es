---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 2: agregar una capa de lógica de negocios y pruebas unitarias | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 24344cc33d7c26d7c408db26c0530ef2c708a7d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439957"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 2: agregar una capa de lógica de negocios y pruebas unitarias

por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales Entity Framework 4,0. Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales. Si tiene alguna pregunta sobre los tutoriales, puede publicarlos en el [Foro de Entity Framework de ASP.net](https://forums.asp.net/1227.aspx).

En el tutorial anterior, ha creado una aplicación Web de n niveles con el Entity Framework y el control `ObjectDataSource`. En este tutorial se muestra cómo agregar lógica de negocios a la vez que se mantiene separada la capa de lógica empresarial (BLL) y la capa de acceso a datos (DAL), y se muestra cómo crear pruebas unitarias automatizadas para la capa BLL.

En este tutorial, realizará las siguientes tareas:

- Cree una interfaz de repositorio que declare los métodos de acceso a datos que necesita.
- Implemente la interfaz de repositorio en la clase de repositorio.
- Cree una clase de lógica empresarial que llame a la clase Repository para realizar funciones de acceso a datos.
- Conecte el control de `ObjectDataSource` a la clase de lógica de negocios en lugar de a la clase de repositorio.
- Cree un proyecto de prueba unitaria y una clase de repositorio que use colecciones en memoria para su almacén de datos.
- Cree una prueba unitaria para la lógica de negocios que desee agregar a la clase de lógica de negocios y, a continuación, ejecute la prueba y observe que se produce un error.
- Implemente la lógica de negocios en la clase de lógica de negocios y, a continuación, vuelva a ejecutar la prueba unitaria y vea el paso.

Trabajará con las páginas *departments. aspx* y *DepartmentsAdd. aspx* que creó en el tutorial anterior.

## <a name="creating-a-repository-interface"></a>Creación de una interfaz de repositorio

Comenzará creando la interfaz del repositorio.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

En la carpeta *Dal* , cree un nuevo archivo de clase, asígnele el nombre *ISchoolRepository.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

La interfaz define un método para cada uno de los métodos CRUD (crear, leer, actualizar, eliminar) que creó en la clase de repositorio.

En la clase `SchoolRepository` en *SchoolRepository.CS*, indique que esta clase implementa la interfaz `ISchoolRepository`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>Crear una clase de lógica de negocios

A continuación, creará la clase de lógica de negocios. Para ello, puede agregar lógica de negocios que se ejecutará en el control de `ObjectDataSource`, aunque no lo hará todavía. Por ahora, la nueva clase de lógica de negocios solo realizará las mismas operaciones CRUD que el repositorio.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

Cree una nueva carpeta y asígnele el nombre *BLL*. (En una aplicación real, el nivel de lógica empresarial se implementaría normalmente como una biblioteca de clases, un proyecto independiente, pero para simplificar este tutorial, las clases de BLL se conservarán en una carpeta de proyecto).

En la carpeta *BLL* , cree un nuevo archivo de clase, asígnele el nombre *SchoolBL.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

Este código crea los mismos métodos CRUD que vio anteriormente en la clase de repositorio, pero en lugar de tener acceso directamente a los métodos de Entity Framework, llama a los métodos de clase de repositorio.

La variable de clase que contiene una referencia a la clase de repositorio se define como un tipo de interfaz y el código que crea una instancia de la clase de repositorio se encuentra en dos constructores. El control `ObjectDataSource` usará el constructor sin parámetros. Crea una instancia de la clase `SchoolRepository` que creó anteriormente. El otro constructor permite cualquier código que cree instancias de la clase de lógica de negocios para pasar cualquier objeto que implemente la interfaz del repositorio.

Los métodos CRUD que llaman a la clase Repository y los dos constructores permiten usar la clase de lógica de negocios con cualquier almacén de datos de back-end que elija. No es necesario que la clase de lógica de negocios tenga en cuenta el modo en que la clase a la que llama conserva los datos. (A menudo se denomina *persistencia omisión*). Esto facilita las pruebas unitarias, ya que puede conectar la clase de lógica de negocios a una implementación de repositorio que use algo tan simple como las colecciones de `List` en memoria para almacenar los datos.

> [!NOTE]
> Técnicamente, los objetos de entidad siguen sin la persistencia, ya que se crean instancias de las clases que heredan de la clase `EntityObject` del Entity Framework. Para omisión de persistencia completa, puede usar *objetos CLR antiguos sin formato*, o *poco*, en lugar de objetos que heredan de la clase `EntityObject`. El uso de POCO está fuera del ámbito de este tutorial. Para obtener más información, vea [prueba y Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx) en el sitio web de MSDN).

Ahora puede conectar los controles `ObjectDataSource` a la clase de lógica empresarial en lugar de al repositorio y comprobar que todo funciona como antes.

En *departments. aspx* y *DepartmentsAdd. aspx*, cambie cada aparición de `TypeName="ContosoUniversity.DAL.SchoolRepository"` a `TypeName="ContosoUniversity.BLL.SchoolBL`". (Hay cuatro instancias en absoluto).

Ejecute las páginas *departments. aspx* y *DepartmentsAdd. aspx* para comprobar que siguen funcionando como antes.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>Crear un proyecto de prueba unitaria y una implementación de repositorio

Agregue un nuevo proyecto a la solución mediante la plantilla de **proyecto de prueba** y asígnele el nombre `ContosoUniversity.Tests`.

En el proyecto de prueba, agregue una referencia a `System.Data.Entity` y agregue una referencia de proyecto al proyecto `ContosoUniversity`.

Ahora puede crear la clase de repositorio que usará con las pruebas unitarias. El almacén de datos para este repositorio estará dentro de la clase.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

En el proyecto de prueba, cree un nuevo archivo de clase, asígnele el nombre *MockSchoolRepository.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

Esta clase de repositorio tiene los mismos métodos CRUD que el que tiene acceso directamente al Entity Framework, pero funcionan con `List` colecciones en memoria en lugar de con una base de datos. Esto hace que sea más fácil para una clase de prueba configurar y validar pruebas unitarias para la clase de lógica de negocios.

## <a name="creating-unit-tests"></a>Crear pruebas unitarias

La plantilla de proyecto de **prueba** creó una clase de prueba unitaria de código auxiliar y la siguiente tarea consiste en modificar esta clase agregando métodos de prueba unitaria para la lógica de negocios que se desea agregar a la clase de lógica de negocios.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

En Contoso University, cualquier instructor individual solo puede ser el administrador de un único departamento y debe agregar lógica de negocios para aplicar esta regla. Empezará agregando pruebas y ejecutando las pruebas para verlas erróneas. A continuación, agregará el código y volverá a ejecutar las pruebas, esperando verlos.

Abra el archivo *UnitTest1.CS* y agregue `using` instrucciones para las capas de lógica de negocios y de acceso a datos que ha creado en el proyecto ContosoUniversity:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

Reemplace el método `TestMethod1` por los métodos siguientes:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

El método `CreateSchoolBL` crea una instancia de la clase de repositorio que creó para el proyecto de prueba unitaria, que luego pasa a una nueva instancia de la clase de lógica de negocios. A continuación, el método utiliza la clase de lógica de negocios para insertar tres departamentos que se pueden usar en los métodos de prueba.

Los métodos de prueba comprueban que la clase de lógica de negocios produce una excepción si alguien intenta insertar un nuevo departamento con el mismo administrador que un departamento existente, o si alguien intenta actualizar el administrador de un departamento estableciéndolo en el identificador de una persona Quién ya es el administrador de otro departamento.

Todavía no ha creado la clase de excepción, por lo que este código no se compilará. Para que se compile, haga clic con el botón derecho en `DuplicateAdministratorException` y seleccione **generar**y, a continuación, **clase**.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

Esto crea una clase en el proyecto de prueba que se puede eliminar después de haber creado la clase de excepción en el proyecto principal. y implementan la lógica de negocios.

Ejecute el proyecto de prueba. Como se esperaba, se produce un error en las pruebas.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>Agregar lógica de negocios para hacer una prueba superada

A continuación, implementará la lógica de negocios que hace que sea imposible establecer como administrador de un departamento que ya es administrador de otro departamento. Producirá una excepción de la capa de lógica de negocios y la detectará en el nivel de presentación si un usuario edita un departamento y hace clic en **Actualizar** después de seleccionar a alguien que ya sea administrador. (También puede quitar a los instructores de la lista desplegable que ya son administradores antes de presentar la página, pero el propósito es trabajar con la capa de lógica de negocios).

Empiece por crear la clase de excepción que se iniciará cuando un usuario intente convertir un instructor en Administrador de más de un departamento. En el proyecto principal, cree un nuevo archivo de clase en la carpeta *BLL* , asígnele el nombre *DuplicateAdministratorException.CS*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

Ahora, elimine el archivo *DuplicateAdministratorException.CS* temporal que creó anteriormente en el proyecto de prueba para poder compilar.

En el proyecto principal, abra el archivo *SchoolBL.CS* y agregue el siguiente método que contiene la lógica de validación. (El código hace referencia a un método que creará más adelante).

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

Llamará a este método cuando inserte o actualice `Department` entidades con el fin de comprobar si otro departamento ya tiene el mismo administrador.

El código llama a un método para buscar en la base de datos una entidad `Department` que tenga el mismo valor de `Administrator` propiedad que la entidad que se va a insertar o actualizar. Si se encuentra uno, el código produce una excepción. No se requiere ninguna comprobación de validación si la entidad que se va a insertar o actualizar no tiene ningún valor `Administrator` y no se produce ninguna excepción si se llama al método durante una actualización y la entidad `Department` encontrada coincide con la entidad `Department` que se está actualizando.

Llame al método New desde los métodos `Insert` y `Update`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

En *ISchoolRepository.CS*, agregue la siguiente declaración para el nuevo método de acceso a datos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

En *SchoolRepository.CS*, agregue la siguiente instrucción `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

En *SchoolRepository.CS*, agregue el siguiente método de acceso a datos nuevo:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

Este código recupera `Department` entidades que tienen un administrador especificado. Solo se debe encontrar un departamento (si existe). Sin embargo, dado que no hay ninguna restricción integrada en la base de datos, el tipo de valor devuelto es una colección en caso de que se encuentren varios departamentos.

De forma predeterminada, cuando el contexto del objeto recupera entidades de la base de datos, realiza un seguimiento de ellas en su administrador de estado de objetos. El parámetro `MergeOption.NoTracking` especifica que este seguimiento no se realizará para esta consulta. Esto es necesario porque la consulta podría devolver la entidad exacta que está intentando actualizar y, después, no podría adjuntar esa entidad. Por ejemplo, Si edita el Departamento de historial en la página *departments. aspx* y deja el administrador sin cambios, esta consulta devolverá el Departamento de historial. Si no se establece `NoTracking`, el contexto del objeto ya tendrá la entidad del Departamento de historial en su administrador de estado de objetos. Después, al adjuntar la entidad del Departamento de historial que se vuelve a crear desde el estado de vista, el contexto del objeto produciría una excepción que indica `"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`.

(Como alternativa a especificar `MergeOption.NoTracking`, puede crear un nuevo contexto de objeto solo para esta consulta. Dado que el nuevo contexto del objeto tendría su propio administrador de estado de objeto, no habría ningún conflicto al llamar al método `Attach`. El nuevo contexto del objeto compartiría los metadatos y la conexión de la base de datos con el contexto del objeto original, por lo que la reducción del rendimiento de este enfoque alternativo sería mínima. Sin embargo, el enfoque que se muestra aquí presenta la opción `NoTracking`, que le resultará útil en otros contextos. La opción `NoTracking` se describe más adelante en un tutorial posterior de esta serie).

En el proyecto de prueba, agregue el nuevo método de acceso a datos a *MockSchoolRepository.CS*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

Este código usa LINQ para realizar la misma selección de datos para la que LINQ to Entities el repositorio del proyecto `ContosoUniversity`.

Vuelva a ejecutar el proyecto de prueba. Esta vez las pruebas son correctas.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>Control de excepciones ObjectDataSource

En el proyecto de `ContosoUniversity`, ejecute la página *departments. aspx* e intente cambiar el administrador de un departamento a alguien que ya sea administrador de otro departamento. (Recuerde que solo puede editar los departamentos que agregó durante este tutorial, porque la base de datos se carga previamente con datos no válidos). Obtiene la siguiente página de error del servidor:

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

No quiere que los usuarios vean este tipo de página de error, por lo que debe agregar código de control de errores. Abra *departments. aspx* y especifique un controlador para el evento `OnUpdated` de la `DepartmentsObjectDataSource`. La etiqueta de apertura de `ObjectDataSource` ahora es similar al ejemplo siguiente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

En *departments.aspx.CS*, agregue la siguiente instrucción `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

Agregue el siguiente controlador para el evento `Updated`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

Si el control `ObjectDataSource` detecta una excepción al intentar realizar la actualización, pasa la excepción en el argumento de evento (`e`) a este controlador. El código del controlador comprueba si la excepción es la excepción de administrador duplicada. Si es así, el código crea un control de validación que contiene un mensaje de error para que se muestre el control `ValidationSummary`.

Ejecute la página e intente crear otra vez al administrador de dos departamentos. Esta vez, el control `ValidationSummary` muestra un mensaje de error.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

Realice cambios similares en la página *DepartmentsAdd. aspx* . En *DepartmentsAdd. aspx*, especifique un controlador para el evento `OnInserted` de la `DepartmentsObjectDataSource`. El marcado resultante será similar al ejemplo siguiente.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

En *DepartmentsAdd.aspx.CS*, agregue la misma instrucción `using`:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

Agregue el siguiente controlador de eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

Ahora puede probar la página *DepartmentsAdd.aspx.CS* para comprobar que también controla correctamente los intentos de hacer que una persona sea el administrador de más de un departamento.

Esto completa la introducción a la implementación del modelo de repositorio para usar el control de `ObjectDataSource` con el Entity Framework. Para obtener más información sobre el patrón de repositorio y la capacidad de prueba, vea las notas del producto de MSDN [testity y Entity Framework 4,0](https://msdn.microsoft.com/library/ff714955.aspx).

En el siguiente tutorial, verá cómo agregar la funcionalidad de ordenación y filtrado a la aplicación.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [Siguiente](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
