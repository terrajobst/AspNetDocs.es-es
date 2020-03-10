---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
title: Implementación del repositorio y la unidad de patrones de trabajo en una aplicación ASP.NET MVC (9 de 10) | Microsoft Docs
author: tdykstra
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 44761193-04ba-4990-9f90-145d3c10a716
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 18de9b125ee5d10795b9ce1a366918dadf4fc4e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434383"
---
# <a name="implementing-the-repository-and-unit-of-work-patterns-in-an-aspnet-mvc-application-9-of-10"></a>Implementación del repositorio y la unidad de patrones de trabajo en una aplicación ASP.NET MVC (9 de 10)

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto completado](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empezar aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema que no puede resolver, [Descargue el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general, puede encontrar la solución al problema comparando el código con el código completado. Para algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)

En el tutorial anterior usaba la herencia para reducir el código redundante en las clases de entidad `Student` y `Instructor`. En este tutorial, verá algunas maneras de usar el repositorio y los patrones de unidad de trabajo para las operaciones CRUD. Como en el tutorial anterior, en este caso cambiará la forma en que el código funciona con las páginas que ya ha creado en lugar de crear nuevas páginas.

## <a name="the-repository-and-unit-of-work-patterns"></a>Los patrones de repositorio y unidad de trabajo

Los patrones de repositorio y unidad de trabajo están diseñados para crear una capa de abstracción entre la capa de acceso a datos y la capa de lógica de negocios de una aplicación. Implementar estos patrones puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar la realización de pruebas unitarias automatizadas o el desarrollo controlado por pruebas (TDD).

En este tutorial, va a implementar una clase de repositorio para cada tipo de entidad. Para el tipo de entidad `Student`, creará una interfaz de repositorio y una clase de repositorio. Al crear una instancia del repositorio en el controlador, usará la interfaz para que el controlador acepte una referencia a cualquier objeto que implemente la interfaz del repositorio. Cuando el controlador se ejecuta en un servidor Web, recibe un repositorio que funciona con el Entity Framework. Cuando el controlador se ejecuta en una clase de prueba unitaria, recibe un repositorio que funciona con datos almacenados de una forma que se puede manipular fácilmente para las pruebas, como una colección en memoria.

Más adelante en el tutorial, usará varios repositorios y una clase de unidad de trabajo para los tipos de entidad `Course` y `Department` en el controlador de `Course`. La clase unidad de trabajo coordina el trabajo de varios repositorios mediante la creación de una clase de contexto de base de datos única compartida por todos ellos. Si desea poder realizar pruebas unitarias automatizadas, debe crear y usar interfaces para estas clases de la misma manera que lo hizo para el repositorio de `Student`. Sin embargo, para simplificar el tutorial, creará y usará estas clases sin interfaces.

En la ilustración siguiente se muestra una manera de conceptualizar las relaciones entre el controlador y las clases de contexto en comparación con no usar el modelo de repositorio o unidad de trabajo en absoluto.

![Repository_pattern_diagram](https://asp.net/media/2578149/Windows-Live-Writer_8c4963ba1fa3_CE3B_Repository_pattern_diagram_1df790d3-bdf2-4c11-9098-946ddd9cd884.png)

No creará pruebas unitarias en esta serie de tutoriales. Para ver una introducción a TDD con una aplicación MVC que usa el patrón de repositorio, consulte [Tutorial: uso de TDD con ASP.NET MVC](https://msdn.microsoft.com/library/ff847525.aspx). Para obtener más información sobre el patrón de repositorio, consulte los siguientes recursos:

- [Patrón de repositorio](https://msdn.microsoft.com/library/ff649690.aspx) en MSDN.
- [Usar los patrones de repositorio y unidad de trabajo con Entity Framework 4,0](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) en el blog del equipo de Entity Framework.
- En [Agile Entity Framework 4 series de repositorios](http://thedatafarm.com/blog/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/) de publicaciones en el blog de Julia Lerman.
- [Creación de la cuenta en una mirada aplicación HTML5/jQuery en el](https://weblogs.asp.net/dwahlin/archive/2011/08/15/building-the-account-at-a-glance-html5-jquery-application.aspx) blog de dan Wahlin.

> [!NOTE]
> Hay muchas maneras de implementar el repositorio y los patrones de unidad de trabajo. Puede usar clases de repositorio con o sin una clase de unidad de trabajo. Puede implementar un único repositorio para todos los tipos de entidad o uno para cada tipo. Si implementa una para cada tipo, puede usar clases independientes, una clase base genérica y clases derivadas, o una clase base abstracta y clases derivadas. Puede incluir la lógica de negocios en el repositorio o restringirla a la lógica de acceso a datos. También puede crear una capa de abstracción en la clase de contexto de base de datos mediante el uso de interfaces de [IDbSet](https://msdn.microsoft.com/library/gg679233(v=vs.103).aspx) en lugar de tipos de [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) para los conjuntos de entidades. El método para implementar una capa de abstracción que se muestra en este tutorial es una opción que se debe tener en cuenta, no una recomendación para todos los escenarios y entornos.

## <a name="creating-the-student-repository-class"></a>Crear la clase de repositorio Student

En la carpeta *Dal* , cree un archivo de clase denominado *IStudentRepository.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código declara un conjunto típico de métodos CRUD, incluidos dos métodos de lectura: uno que devuelve todas `Student` entidades y otro que encuentra una sola entidad de `Student` por identificador.

En la carpeta *Dal* , cree un archivo de clase denominado *StudentRepository.CS* File. Reemplace el código existente por el código siguiente, que implementa la interfaz `IStudentRepository`:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample2.cs)]

El contexto de la base de datos se define en una variable de clase y el constructor espera que el objeto que realiza la llamada pase en una instancia del contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample3.cs)]

Puede crear una instancia de un nuevo contexto en el repositorio, pero si usa varios repositorios en un controlador, cada uno de ellos terminará con un contexto independiente. Más adelante usará varios repositorios en el controlador de `Course` y verá cómo una clase de unidad de trabajo puede garantizar que todos los repositorios usen el mismo contexto.

El repositorio implementa [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx) y desecha el contexto de la base de datos tal como se vio anteriormente en el controlador, y sus métodos CRUD realizan llamadas al contexto de la base de datos de la misma manera que vio anteriormente.

## <a name="change-the-student-controller-to-use-the-repository"></a>Cambiar el controlador de estudiante para usar el repositorio

En *StudentController.CS*, reemplace el código que se encuentra actualmente en la clase por el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=13-18,44,75,77,102-103,120,137-138,159,172-174,186)]

Ahora, el controlador declara una variable de clase para un objeto que implementa la interfaz `IStudentRepository` en lugar de la clase de contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample5.cs)]

El constructor predeterminado (sin parámetros) crea una nueva instancia de contexto y un constructor opcional permite que el llamador pase en una instancia de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample6.cs)]

(Si estuviera usando la *inserción de dependencias*, o di, no necesitaría el constructor predeterminado, ya que el software de di debería asegurarse de que siempre se proporcione el objeto de repositorio correcto).

En los métodos CRUD, ahora se llama al repositorio en lugar del contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample7.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample8.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample9.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample10.cs)]

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample11.cs)]

Y el método `Dispose` ahora desecha el repositorio en lugar del contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample12.cs)]

Ejecute el sitio y haga clic en la pestaña **Students** .

![Students_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image1.png)

La página tiene el mismo aspecto y funcionamiento que antes de cambiar el código para usar el repositorio, y las demás páginas de estudiante también funcionan igual. Sin embargo, hay una diferencia importante en la forma en que el método `Index` del controlador realiza el filtrado y la ordenación. La versión original de este método contiene el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample13.cs?highlight=1)]

El método `Index` actualizado contiene el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=1)]

Solo el código resaltado ha cambiado.

En la versión original del código, `students` se escribe como un objeto `IQueryable`. La consulta no se envía a la base de datos hasta que se convierte en una colección mediante un método como `ToList`, que no se produce hasta que la vista de índice tiene acceso al modelo Student. El método `Where` del código original anterior se convierte en una cláusula `WHERE` de la consulta SQL que se envía a la base de datos. Esto significa que, a su vez, la base de datos devuelve solo las entidades seleccionadas. Sin embargo, como resultado de cambiar `context.Students` a `studentRepository.GetStudents()`, la variable `students` después de esta instrucción es una colección de `IEnumerable` que incluye todos los alumnos de la base de datos. El resultado final de aplicar el método `Where` es el mismo, pero ahora el trabajo se realiza en la memoria en el servidor Web y no en la base de datos. En el caso de las consultas que devuelven grandes volúmenes de datos, esto puede ser ineficaz.

> [!TIP]
> 
> **IQueryable frente a IEnumerable**
> 
> Después de implementar el repositorio como se muestra aquí, incluso si escribe algo en el cuadro de **búsqueda** , la consulta enviada a SQL Server devuelve todas las filas de estudiante porque no incluye los criterios de búsqueda:
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image2.png)
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample15.sql)]
> 
> Esta consulta devuelve todos los datos de estudiante porque el repositorio ejecutó la consulta sin conocer los criterios de búsqueda. El proceso de ordenación, la aplicación de criterios de búsqueda y la selección de un subconjunto de los datos para la paginación (mostrando solo 3 filas en este caso) se realiza en la memoria más adelante cuando se llama al método `ToPagedList` en la colección `IEnumerable`.
> 
> En la versión anterior del código (antes de implementar el repositorio), la consulta no se envía a la base de datos hasta que se aplican los criterios de búsqueda, cuando se llama a `ToPagedList` en el objeto `IQueryable`.
> 
> ![](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image3.png)
> 
> Cuando se llama a ToPagedList en un objeto `IQueryable`, la consulta enviada a SQL Server especifica la cadena de búsqueda y, como resultado, solo se devuelven las filas que cumplen los criterios de búsqueda y no es necesario realizar ningún filtrado en la memoria.
> 
> [!code-sql[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample16.sql)]
> 
> (En el siguiente tutorial se explica cómo examinar las consultas enviadas a SQL Server).

En la siguiente sección se muestra cómo implementar métodos de repositorio que le permiten especificar que este trabajo debe realizarse en la base de datos.

Ahora ha creado una capa de abstracción entre el controlador y el contexto de la base de datos Entity Framework. Si va a realizar pruebas unitarias automatizadas con esta aplicación, puede crear una clase de repositorio alternativa en un proyecto de prueba unitaria que implementa `IStudentRepository` *.* En lugar de llamar al contexto para leer y escribir datos, esta clase de repositorio ficticio podría manipular colecciones en memoria para probar las funciones del controlador.

## <a name="implement-a-generic-repository-and-a-unit-of-work-class"></a>Implementar un repositorio genérico y una clase de unidad de trabajo

La creación de una clase de repositorio para cada tipo de entidad podría dar lugar a una gran cantidad de código redundante y podría dar lugar a actualizaciones parciales. Por ejemplo, supongamos que tiene que actualizar dos tipos de entidad diferentes como parte de la misma transacción. Si cada una de ellas utiliza una instancia de contexto de base de datos independiente, una puede tener éxito y la otra podría producir un error. Una manera de minimizar el código redundante es usar un repositorio genérico y una manera de asegurarse de que todos los repositorios usen el mismo contexto de base de datos (y, por tanto, coordinar todas las actualizaciones) es usar una clase de unidad de trabajo.

En esta sección del tutorial, creará una clase `GenericRepository` y una clase `UnitOfWork` y las usará en el controlador de `Course` para tener acceso a los conjuntos de entidades `Department` y `Course`. Tal y como se explicó anteriormente, para mantener esta parte del tutorial en simple, no se crean interfaces para estas clases. Pero si va a utilizarlos para facilitar el TDD, normalmente los implementaría con interfaces de la misma forma que lo hacía `Student` repositorio.

### <a name="create-a-generic-repository"></a>Crear un repositorio genérico

En la carpeta *Dal* , cree *GenericRepository.CS* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample17.cs)]

Las variables de clase se declaran para el contexto de la base de datos y para el conjunto de entidades para el que se crea una instancia del repositorio:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample18.cs)]

El constructor acepta una instancia de contexto de base de datos e inicializa la variable de conjunto de entidades:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample19.cs)]

El método `Get` usa expresiones lambda para permitir que el código de llamada especifique una condición de filtro y una columna para ordenar los resultados por, y un parámetro de cadena permite al llamador proporcionar una lista delimitada por comas de propiedades de navegación para la carga diligente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample20.cs)]

El código `Expression<Func<TEntity, bool>> filter` significa que el autor de la llamada proporcionará una expresión lambda basada en el tipo de `TEntity` y esta expresión devolverá un valor booleano. Por ejemplo, si se crea una instancia del repositorio para el tipo de entidad `Student`, el código del método que realiza la llamada podría especificar `student => student.LastName == "Smith`&quot; para el parámetro `filter`.

El código `Func<IQueryable<TEntity>, IOrderedQueryable<TEntity>> orderBy` también significa que el autor de la llamada proporcionará una expresión lambda. Pero en este caso, la entrada a la expresión es un objeto `IQueryable` para el tipo de `TEntity`. La expresión devolverá una versión ordenada de ese objeto `IQueryable`. Por ejemplo, si se crea una instancia del repositorio para el tipo de entidad `Student`, el código del método que realiza la llamada podría especificar `q => q.OrderBy(s => s.LastName)` para el parámetro `orderBy`.

El código del método `Get` crea un objeto `IQueryable` y, a continuación, aplica la expresión de filtro si hay alguno:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample21.cs)]

A continuación, aplica las expresiones de carga diligente después de analizar la lista delimitada por comas:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample22.cs)]

Finalmente, aplica la expresión `orderBy` si hay una y devuelve los resultados; en caso contrario, devuelve los resultados de la consulta no ordenada:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample23.cs)]

Al llamar al método `Get`, puede realizar el filtrado y la ordenación en la colección `IEnumerable` devuelta por el método en lugar de proporcionar parámetros para estas funciones. Pero el trabajo de ordenación y filtrado se haría entonces en la memoria del servidor Web. Mediante el uso de estos parámetros, se asegura de que el trabajo se realiza en la base de datos en lugar de en el servidor Web. Una alternativa es crear clases derivadas para tipos de entidad específicos y agregar métodos `Get` especializados, como `GetStudentsInNameOrder` o `GetStudentsByName`. Sin embargo, en una aplicación compleja, esto puede dar lugar a un gran número de clases derivadas y métodos especializados, lo que podría ser más trabajo de mantener.

El código de los métodos `GetByID`, `Insert`y `Update` es similar a lo que vio en el repositorio no genérico. (No se proporciona un parámetro de carga diligente en la firma `GetByID`, porque no se puede realizar la carga diligente con el método `Find`).

Se proporcionan dos sobrecargas para el método `Delete`:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample24.cs)]

Uno de ellos permite pasar solo el identificador de la entidad que se va a eliminar y otro toma una instancia de la entidad. Como vio en el tutorial de [control de simultaneidad](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) , para el control de simultaneidad necesita un método `Delete` que toma una instancia de la entidad que incluye el valor original de una propiedad de seguimiento.

Este repositorio genérico controlará los requisitos de CRUD típicos. Cuando un tipo de entidad determinado tiene requisitos especiales, como filtrado o ordenación más complejos, puede crear una clase derivada que tenga métodos adicionales para ese tipo.

## <a name="creating-the-unit-of-work-class"></a>Crear la clase de unidad de trabajo

La clase unidad de trabajo sirve para un propósito: para asegurarse de que cuando se usan varios repositorios, comparten un único contexto de base de datos. De este modo, cuando se complete una unidad de trabajo, puede llamar al método `SaveChanges` en esa instancia del contexto y estar seguro de que todos los cambios relacionados se coordinarán. Todo lo que necesita la clase es un método `Save` y una propiedad para cada repositorio. Cada propiedad del repositorio devuelve una instancia de repositorio de la que se han creado instancias con la misma instancia de contexto de base de datos que las demás instancias de repositorio.

En la carpeta *Dal* , cree un archivo de clase denominado *UnitOfWork.CS* y reemplace el código de plantilla por el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample25.cs)]

El código crea variables de clase para el contexto de la base de datos y para cada repositorio. En el caso de la variable `context`, se crea una instancia de un nuevo contexto:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample26.cs)]

Cada propiedad del repositorio comprueba si el repositorio ya existe. De lo contrario, crea una instancia del repositorio, pasando la instancia de contexto. Como resultado, todos los repositorios comparten la misma instancia de contexto.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample27.cs)]

El método `Save` llama a `SaveChanges` en el contexto de la base de datos.

Al igual que cualquier clase que crea instancias de un contexto de base de datos en una variable de clase, la clase `UnitOfWork` implementa `IDisposable` y desecha el contexto.

### <a name="changing-the-course-controller-to-use-the-unitofwork-class-and-repositories"></a>Cambiar el controlador del curso para usar la clase y los repositorios de UnitOfWork

Reemplace el código que tiene actualmente en *CourseController.CS* por el código siguiente:

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample28.cs?highlight=15,20,22,31,54-55,70,85-86,101-102,122-124,130)]

Este código agrega una variable de clase para la clase `UnitOfWork`. (Si utilizaba las interfaces aquí, no inicializarías la variable aquí; en su lugar, implementaría un patrón de dos constructores como hizo para el repositorio de `Student`).

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample29.cs)]

En el resto de la clase, todas las referencias al contexto de la base de datos se reemplazan por referencias al repositorio adecuado, utilizando `UnitOfWork` propiedades para tener acceso al repositorio. El método `Dispose` desecha la instancia de `UnitOfWork`.

[!code-csharp[Main](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/samples/sample30.cs)]

Ejecute el sitio y haga clic en la pestaña **cursos** .

![Courses_Index_page](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application/_static/image4.png)

La página busca y funciona igual que antes de los cambios, y las demás páginas del curso también funcionan igual.

## <a name="summary"></a>Resumen

Ahora ha implementado los patrones de repositorio y de unidad de trabajo. Ha usado expresiones lambda como parámetros de método en el repositorio genérico. Para obtener más información sobre cómo usar estas expresiones con un objeto de `IQueryable`, consulte [interfaz IQueryable (t) (System. Linq)](https://msdn.microsoft.com/library/bb351562.aspx) en MSDN Library. En el siguiente tutorial aprenderá a administrar algunos escenarios avanzados.

Los vínculos a otros recursos de Entity Framework pueden encontrarse en la [asignación de contenido de ASP.net Data Access](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
