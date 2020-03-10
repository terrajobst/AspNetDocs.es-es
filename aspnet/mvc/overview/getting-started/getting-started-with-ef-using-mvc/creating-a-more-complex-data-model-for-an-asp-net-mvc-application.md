---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: creación de un modelo de datos más complejo para una aplicación ASP.NET MVC'
author: tdykstra
description: En este tutorial agregará más entidades y relaciones, y personalizará el modelo de datos mediante la especificación de reglas de formato, validación y asignación de base de datos.
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 46f7f3c9-274f-4649-811d-92222a9b27e2
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 933354b09eb4674e6f523f8a65816410521f026f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471295"
---
# <a name="tutorial-create-a-more-complex-data-model-for-an-aspnet-mvc-app"></a>Tutorial: creación de un modelo de datos más complejo para una aplicación ASP.NET MVC

En los tutoriales anteriores ha trabajado con un modelo de datos simple que se compone de tres entidades. En este tutorial agregará más entidades y relaciones, y personalizará el modelo de datos mediante la especificación de reglas de formato, validación y asignación de base de datos. En este artículo se muestran dos maneras de personalizar el modelo de datos: agregando atributos a las clases de entidad y agregando código a la clase de contexto de base de datos.

Cuando haya terminado, las clases de entidad conformarán el modelo de datos completo que se muestra en la ilustración siguiente:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

En este tutorial va a:

> [!div class="checklist"]
> * Personalizar el modelo de datos
> * Actualizar la entidad Student
> * Crea la entidad Instructor
> * Crea la entidad OfficeAssignment
> * Modificación de la entidad Course
> * Crear la entidad Department
> * Modificar la entidad Enrollment
> * Agregar código al contexto de base de datos
> * Inicializa la base de datos con datos de prueba
> * Agregar una migración
> * Actualizar la base de datos

## <a name="prerequisites"></a>Requisitos previos

* [Migración e implementación de Code First](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="customize-the-data-model"></a>Personalizar el modelo de datos

En esta sección verá cómo personalizar el modelo de datos mediante el uso de atributos que especifican reglas de formato, validación y asignación de base de datos. En algunas de las secciones siguientes, creará el modelo de datos `School` completo agregando atributos a las clases que ya ha creado y creando nuevas clases para los tipos de entidad restantes en el modelo.

### <a name="the-datatype-attribute"></a>Atributo DataType

Para las fechas de inscripción de estudiantes, en todas las páginas web se muestra actualmente la hora junto con la fecha, aunque todo lo que le interesa para este campo es la fecha. Mediante los atributos de anotación de datos, puede realizar un cambio de código que fijará el formato de presentación en cada vista en la que se muestren los datos. Para ver un ejemplo de cómo hacerlo, deberá agregar un atributo a la propiedad `EnrollmentDate` en la clase `Student`.

En *Models\Student.CS*, agregue una instrucción `using` para el espacio de nombres `System.ComponentModel.DataAnnotations` y agregue los atributos `DataType` y `DisplayFormat` a la propiedad `EnrollmentDate`, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,12-13)]

El atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) se utiliza para especificar un tipo de datos más específico que el tipo intrínseco de la base de datos. En este caso solo se quiere realizar el seguimiento de la fecha, no de la fecha y la hora. La [enumeración DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) proporciona muchos tipos de datos, como *Date, Time, PhoneNumber, Currency, EmailAddress* , etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo, se puede crear un vínculo de `mailto:` para [DataType. EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)y se puede proporcionar un selector de fecha para [DataType. Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) en exploradores compatibles con [HTML5](http://html5.org/). Los atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) emiten atributos [de datos](http://ejohn.org/blog/html-5-data-attributes/) HTML 5 (pronunciados con *guiones de datos*) que los exploradores HTML 5 pueden comprender. Los atributos [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) no proporcionan ninguna validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De forma predeterminada, el campo de datos se muestra según los formatos predeterminados basados en el objeto [CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)del servidor.

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]

El valor `ApplyFormatInEditMode` especifica que el formato especificado también debe aplicarse cuando el valor se muestra en un cuadro de texto para su edición. (Es posible que no desee que en algunos campos, por ejemplo, para los valores de moneda, no desee que el símbolo de moneda en el cuadro de texto se edite).

Puede usar el atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) por sí solo, pero normalmente es conveniente usar también el atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) . El atributo `DataType` transmite la *semántica* de los datos en lugar de cómo representarlos en una pantalla y ofrece las siguientes ventajas que no se obtienen con `DisplayFormat`:

- El explorador puede habilitar características de HTML5 (por ejemplo, para mostrar un control de calendario, el símbolo de divisa adecuado según la configuración regional, vínculos de correo electrónico, validación de entradas del lado cliente, etc.).
- De forma predeterminada, el explorador representará los datos con el formato correcto en función de la [configuración regional](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx).
- El atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) puede habilitar MVC para elegir la plantilla de campo correcta para representar los datos ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) usa la plantilla de cadena). Para obtener más información, consulte [las plantillas de ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)de Brad Wilson. (Aunque está escrito para MVC 2, este artículo se aplica a la versión actual de ASP.NET MVC).

Si utiliza el atributo `DataType` con un campo de fecha, tendrá que especificar el atributo `DisplayFormat` también para asegurarse de que el campo se representa correctamente en los exploradores Chrome. Para obtener más información, vea [este subproceso de stackoverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Para obtener más información sobre cómo controlar otros formatos de fecha en MVC, vaya a la [Introducción a MVC 5: examen de los métodos de edición y edición](../introduction/examining-the-edit-methods-and-edit-view.md) y búsqueda en la página de &quot;&quot;de internacionalización.

Vuelva a ejecutar la página de índice de estudiante y observe que las horas ya no se muestran para las fechas de inscripción. Lo mismo se aplicará a cualquier vista que use el modelo de `Student`.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>StringLengthAttribute

También puede especificar reglas de validación de datos y mensajes de error de validación mediante atributos. El [atributo StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) establece la longitud máxima en la base de datos y proporciona la validación del lado cliente y del lado servidor para ASP.NET MVC. En este atributo también se puede especificar la longitud mínima de la cadena, pero el valor mínimo no influye en el esquema de la base de datos.

Imagine que quiere asegurarse de que los usuarios no escriban más de 50 caracteres para un nombre. Para agregar esta limitación, agregue los atributos [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) a las propiedades `LastName` y `FirstMidName`, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

El atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) no impedirá que un usuario escriba espacios en blanco para un nombre. Puede usar el atributo [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) para aplicar restricciones a la entrada. Por ejemplo, el código siguiente requiere que el primer carácter esté en mayúsculas y que el resto de los caracteres sea alfabético:

`[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]`

El atributo [MaxLength](https://msdn.microsoft.com/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) proporciona una funcionalidad similar al atributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) , pero no proporciona la validación del lado cliente.

Ejecute la aplicación y haga clic en la pestaña **Students** . Obtiene el siguiente error:

*El modelo de respaldo del contexto ' SchoolContext ' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar Migraciones de Code First para actualizar la base de datos ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

El modelo de base de datos ha cambiado de forma que requiere un cambio en el esquema de la base de datos y Entity Framework detectado. Usará las migraciones para actualizar el esquema sin perder ningún dato que haya agregado a la base de datos mediante la interfaz de usuario. Si ha cambiado los datos creados por el método `Seed`, se volverá a cambiar a su estado original debido al método [AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) que está usando en el método `Seed`. ([AddOrUpdate](https://msdn.microsoft.com/library/hh846520(v=vs.103).aspx) es equivalente a una operación "Upsert" de la terminología de bases de datos).

En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

El comando `add-migration` crea un archivo denominado *&lt;timeStamp&gt;\_MaxLengthOnNames.CS*. Este archivo contiene código en el método `Up` que actualizará la base de datos para que coincida con el modelo de datos actual. El comando `update-database` ejecutó ese código.

La marca de tiempo que se antepone al nombre del archivo de migración se usa en Entity Framework para ordenar las migraciones. Puede crear varias migraciones antes de ejecutar el comando `update-database` y, a continuación, todas las migraciones se aplican en el orden en que se crearon.

Ejecute la página **crear** y escriba un nombre con más de 50 caracteres. Al hacer clic en **crear**, la validación del lado cliente muestra un mensaje de error: *el campo LastName debe ser una cadena con una longitud máxima de 50.*

### <a name="the-column-attribute"></a>Atributo de columna

También puede usar atributos para controlar cómo se asignan las clases y propiedades a la base de datos. Imagine que hubiera usado el nombre `FirstMidName` para el nombre de campo por la posibilidad de que el campo contenga también un segundo nombre. Pero quiere que la columna de base de datos se denomine `FirstName`, ya que los usuarios que van a escribir consultas ad hoc en la base de datos están acostumbrados a ese nombre. Para realizar esta asignación, puede usar el atributo `Column`.

El atributo `Column` especifica que, cuando se cree la base de datos, la columna de la tabla `Student` que se asigna a la propiedad `FirstMidName` se denominará `FirstName`. En otras palabras, cuando el código hace referencia a `Student.FirstMidName`, los datos procederán o se actualizarán en la columna `FirstName` de la tabla `Student`. Si no se especifican nombres de columna, se les asigna el mismo nombre que el nombre de la propiedad.

En el archivo *Student.CS* , agregue una instrucción `using` para [System. ComponentModel. DataAnnotations. Schema](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.aspx) y agregue el atributo name de la columna a la propiedad `FirstMidName`, tal y como se muestra en el siguiente código resaltado:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

La adición del [atributo de columna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) cambia el modelo de respaldo de SchoolContext, por lo que no coincidirá con la base de datos. Escriba los siguientes comandos en el PMC para crear otra migración:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

En **Explorador de servidores**, abra el diseñador de tablas de *Student* haciendo doble clic en la tabla *Student* .

En la imagen siguiente se muestra el nombre de la columna original tal y como se encontraba antes de aplicar las dos primeras migraciones. Además del nombre de columna que cambia de `FirstMidName` a `FirstName`, las dos columnas de nombres han cambiado de `MAX` longitud a 50 caracteres.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

También puede realizar cambios en la asignación de la base de datos mediante la [API fluida](https://msdn.microsoft.com/data/jj591617), como verá más adelante en este tutorial.

> [!NOTE]
> Si intenta compilar antes de terminar de crear todas las clases de entidad en las secciones siguientes, es posible que se produzcan errores del compilador.

## <a name="update-student-entity"></a>Actualizar la entidad Student

En *Models\Student.CS*, reemplace el código que agregó anteriormente con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=11,13,15,18,22,25-32)]

### <a name="the-required-attribute"></a>El atributo required

El [atributo required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) hace que las propiedades de nombre sean campos obligatorios. El `Required attribute` no es necesario para los tipos de valor como DateTime, int, Double y Float. A los tipos de valor no se les puede asignar un valor null, por lo que se tratan de forma inherente como campos obligatorios. 

El atributo `Required` se debe usar con `MinimumLength` para que se aplique `MinimumLength`.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs?highlight=2)]

`MinimumLength` y `Required` permiten que el espacio en blanco satisfaga la validación. Utilice el atributo `RegularExpression` para el control total sobre la cadena.

### <a name="the-display-attribute"></a>El atributo display

El atributo `Display` especifica que el título de los cuadros de texto debe ser "First Name" (Nombre), "Last Name" (Apellidos), "Full Name" (Nombre completo) y "Enrollment Date" (Fecha de inscripción) en lugar del nombre de propiedad de cada instancia (que no tiene ningún espacio para dividir las palabras).

### <a name="the-fullname-calculated-property"></a>Propiedad calculada FullName

`FullName` es una propiedad calculada que devuelve un valor que se crea mediante la concatenación de otras dos propiedades. Por lo tanto, solo tiene un descriptor de acceso `get` y no se generará ninguna columna de `FullName` en la base de datos.

## <a name="create-instructor-entity"></a>Crea la entidad Instructor

Cree *Models\Instructor.CS*y reemplace el código de plantilla por el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs)]

Tenga en cuenta que varias propiedades son las mismas en las entidades `Student` y `Instructor`. En el tutorial [Implementación de la herencia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) más adelante en esta serie, deberá refactorizar este código para eliminar la redundancia.

Puede colocar varios atributos en una línea, por lo que también podría escribir la clase instructor de la siguiente manera:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Propiedades de navegación Courses y OfficeAssignment

`Courses` y `OfficeAssignment` son propiedades de navegación. Como se explicó anteriormente, normalmente se definen como [virtuales](https://msdn.microsoft.com/library/9fkccyh4(v=vs.110).aspx) para que puedan beneficiarse de una característica Entity Framework denominada [carga diferida](https://msdn.microsoft.com/magazine/hh205756.aspx). Además, si una propiedad de navegación puede contener varias entidades, su tipo debe implementar la interfaz [ICollection&lt;t&gt;](https://msdn.microsoft.com/library/92t2ye13.aspx) . Por ejemplo, [IList&lt;t&gt;](https://msdn.microsoft.com/library/5y536ey6.aspx) califica pero no [IEnumerable&lt;t&gt;](https://msdn.microsoft.com/library/9eekhta0.aspx) porque `IEnumerable<T>` no implementa [Add](https://msdn.microsoft.com/library/63ywd54z.aspx).

Un instructor puede enseñar cualquier número de cursos, por lo que `Courses` se define como una colección de entidades `Course`.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

Nuestro estado de las reglas de negocios un instructor solo puede tener como máximo una oficina, por lo que `OfficeAssignment` se define como una única entidad de `OfficeAssignment` (que se puede `null` si no se asigna ninguna oficina).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-officeassignment-entity"></a>Crea la entidad OfficeAssignment

Cree *Models\OfficeAssignment.CS* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compile el proyecto, que guarda los cambios y comprueba que no ha realizado ningún error de copia y pegado que el compilador pueda detectar.

### <a name="the-key-attribute"></a>El atributo clave

Existe una relación de uno a cero o uno entre las entidades `Instructor` y `OfficeAssignment`. Una asignación de oficina solo existe en relación con el instructor al que está asignada y, por tanto, su clave principal también es su clave externa para la entidad `Instructor`. Pero el Entity Framework no puede reconocer automáticamente `InstructorID` como la clave principal de esta entidad porque su nombre no sigue la Convención de nomenclatura `ID` o *classname* `ID`. Por tanto, se usa el atributo `Key` para identificarla como la clave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

También puede usar el atributo `Key` si la entidad tiene su propia clave principal, pero desea asignar un nombre a la propiedad algo diferente de `classnameID` o `ID`. De forma predeterminada, EF trata la clave como no generada por la base de datos porque la columna es para una relación de identificación.

### <a name="the-foreignkey-attribute"></a>El atributo ForeignKey

Cuando hay una relación de uno a cero o uno o una relación de uno a uno entre dos entidades (por ejemplo, entre `OfficeAssignment` y `Instructor`), EF no puede averiguar qué extremo de la relación es la entidad de seguridad y qué extremo es dependiente. Las relaciones uno a uno tienen una propiedad de navegación de referencia en cada clase para la otra clase. El [atributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) se puede aplicar a la clase dependiente para establecer la relación. Si omite el [atributo ForeignKey](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), obtendrá el siguiente error al intentar crear la migración:

*No se puede determinar el extremo principal de una asociación entre los tipos ' ContosoUniversity. Models. OfficeAssignment ' y ' ContosoUniversity. Models. instructor '. El extremo principal de esta asociación se debe configurar explícitamente mediante la API fluida de la relación o las anotaciones de datos.*

Más adelante en el tutorial, verá cómo configurar esta relación con la API fluida.

### <a name="the-instructor-navigation-property"></a>Propiedad de navegación del instructor

La entidad `Instructor` tiene una propiedad de navegación `OfficeAssignment` que acepta valores NULL (porque un instructor podría no tener una asignación de Office) y la entidad `OfficeAssignment` tiene una propiedad de navegación `Instructor` que no acepta valores NULL (porque una asignación de la oficina no puede existir sin un instructor--`InstructorID` no admite valores NULL). Cuando una entidad `Instructor` tiene una entidad `OfficeAssignment` relacionada, cada entidad tendrá una referencia a la otra en su propiedad de navegación.

Podría colocar un atributo `[Required]` en la propiedad de navegación del instructor para especificar que debe haber un instructor relacionado, pero no tiene que hacerlo porque la clave externa InstructorID (que también es la clave de esta tabla) no admite valores NULL.

## <a name="modify-the-course-entity"></a>Modificación de la entidad Course

En *Models\Course.CS*, reemplace el código que agregó anteriormente con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

La entidad Course tiene una propiedad de clave externa `DepartmentID` que señala a la entidad `Department` relacionada y tiene una propiedad de navegación `Department`. Entity Framework no requiere que agregue una propiedad de clave externa al modelo de datos cuando tenga una propiedad de navegación para una entidad relacionada. EF crea automáticamente claves externas en la base de datos dondequiera que se necesiten. Pero tener la clave externa en el modelo de datos puede hacer que las actualizaciones sean más sencillas y eficaces. Por ejemplo, al capturar una entidad Course para editarla, la entidad `Department` es NULL si no la carga, por lo que al actualizar la entidad Course, tendría que capturar primero la entidad `Department`. Cuando la propiedad de clave externa `DepartmentID` está incluida en el modelo de datos, no es necesario capturar la entidad `Department` antes de actualizar.

### <a name="the-databasegenerated-attribute"></a>El atributo DatabaseGenerated

El [atributo DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con el parámetro [None](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) en la propiedad `CourseID` especifica que los valores de clave principal son proporcionados por el usuario en lugar de ser generados por la base de datos.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

De forma predeterminada, el Entity Framework supone que la base de datos genera los valores de clave principal. Es lo que le interesa en la mayoría de los escenarios. Sin embargo, para entidades `Course`, usará un número de curso especificado por el usuario, como una serie 1000 para un departamento, una serie 2000 para otro departamento, etc.

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de clave externa y de navegación

Las propiedades de clave externa y las propiedades de navegación de la entidad `Course` reflejan las siguientes relaciones:

- Un curso se asigna a un departamento, por lo que hay una clave externa `DepartmentID` y una propiedad de navegación `Department` por las razones mencionadas anteriormente.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un curso puede tener cualquier número de alumnos inscritos en él, por lo que la propiedad de navegación `Enrollments` es una colección:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un curso puede ser impartido por varios instructores, por lo que la propiedad de navegación `Instructors` es una colección:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="create-the-department-entity"></a>Crear la entidad Department

Cree *Models\Department.CS* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>Atributo de columna

Antes usaba el [atributo de columna](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) para cambiar la asignación de nombres de columna. En el código de la entidad `Department`, se usa el atributo `Column` para cambiar la asignación de tipos de datos SQL de forma que la columna se defina con el tipo de SQL Server [Money](https://msdn.microsoft.com/library/ms179882.aspx) en la base de datos:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Normalmente, la asignación de columnas no es necesaria, porque el Entity Framework suele elegir el tipo de datos SQL Server adecuado en función del tipo CLR que defina para la propiedad. El tipo CLR `decimal` se asigna a un tipo `decimal` de SQL Server. Pero en este caso sabe que la columna va a contener las cantidades de moneda y el tipo de datos [Money](https://msdn.microsoft.com/library/ms179882.aspx) es más adecuado para eso. Para obtener más información sobre los tipos de datos de CLR y cómo coinciden con los tipos de datos SQL Server, vea [SqlClient para Entity Framework](https://msdn.microsoft.com/library/bb896344.aspx).

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de clave externa y de navegación

Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:

- Un departamento puede tener o no un administrador, y un administrador es siempre un instructor. Por lo tanto, la propiedad `InstructorID` se incluye como la clave externa para la entidad `Instructor` y se agrega un signo de interrogación después de la designación de tipo `int` para marcar la propiedad como que acepta valores NULL. La propiedad de navegación se denomina `Administrator` pero contiene una entidad `Instructor`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un departamento puede tener muchos cursos, por lo que hay una propiedad de navegación `Courses`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

  > [!NOTE]
  > Por convención, Entity Framework permite la eliminación en cascada para las claves externas que no aceptan valores NULL y para las relaciones de varios a varios. Esto puede dar lugar a reglas de eliminación en cascada circulares, lo que producirá una excepción al intentar agregar una migración. Por ejemplo, si no definió la propiedad `Department.InstructorID` como que acepta valores NULL, obtendría el siguiente mensaje de excepción: "la relación referencial producirá una referencia cíclica no permitida". Si las reglas de negocios requeridas `InstructorID` propiedad para que no admita valores NULL, tendría que usar la siguiente instrucción de API fluida para deshabilitar la eliminación en cascada en la relación:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]

## <a name="modify-the-enrollment-entity"></a>Modificar la entidad Enrollment

 En *Models\Enrollment.CS*, reemplace el código que agregó anteriormente con el código siguiente.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=1,15)]

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de clave externa y de navegación

Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:

- Un registro de inscripción es para un solo curso, por lo que hay una propiedad de clave externa `CourseID` y una propiedad de navegación `Course`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs)]
- Un registro de inscripción es para un solo estudiante, por lo que hay una propiedad de clave externa `StudentID` y una propiedad de navegación `Student`:

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]

### <a name="many-to-many-relationships"></a>Relaciones Varios a Varios

Hay una relación de varios a varios entre las entidades `Student` y `Course`, y la entidad `Enrollment` funciona como una tabla de combinación de varios a varios *con carga* en la base de datos. Esto significa que la tabla `Enrollment` contiene datos adicionales además de las claves externas de las tablas combinadas (en este caso, una clave principal y una propiedad `Grade`).

En la ilustración siguiente se muestra el aspecto de estas relaciones en un diagrama de entidades. (Este diagrama se generó con las [herramientas avanzadas de Entity Framework](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); la creación del diagrama no forma parte del tutorial, simplemente se usa aquí como ilustración).

![Estudiantes-Course_many a many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, que indica una relación de uno a varios.

Si la tabla `Enrollment` no incluía información de grado, solo tendría que contener las dos claves externas `CourseID` y `StudentID`. En ese caso, se correspondería con una tabla de combinación de varios a varios *sin carga* (o una *Tabla combinada pura*) en la base de datos, y no tendría que crear una clase de modelo para ella. Las entidades `Instructor` y `Course` tienen ese tipo de relación de varios a varios y, como puede ver, no hay ninguna clase de entidad entre ellas:

![Many_relationship de Course_many de instructor](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

No obstante, se requiere una tabla de combinación en la base de datos, tal y como se muestra en el siguiente diagrama de base de datos:

![Many_relationship_tables de Course_many de instructor](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

El Entity Framework crea automáticamente la tabla de `CourseInstructor` y la Lee y actualiza indirectamente mediante la lectura y actualización de las propiedades de navegación `Instructor.Courses` y `Course.Instructors`.

## <a name="entity-relationship-diagram"></a>Diagrama de relaciones de entidades

En la siguiente ilustración se muestra el diagrama creado por Entity Framework Power Tools para el modelo School completado.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

Además de las líneas de relación de varios a varios (\* a \*) y las líneas de relación uno a varios (1 a \*), aquí puede ver la línea de relación de uno a cero o uno (1 a 0.. 1) entre las entidades `Instructor` y `OfficeAssignment` y la línea de relación de cero o uno a varios (0.. 1 para \*) entre las entidades instructor y Department.

## <a name="add-code-to-database-context"></a>Agregar código al contexto de base de datos

A continuación, agregará las nuevas entidades a la clase `SchoolContext` y personalizará parte de la asignación mediante llamadas a la [API fluida](https://msdn.microsoft.com/data/jj591617) . La API es "fluida" porque a menudo se usa mediante la cadena de una serie de llamadas de método en una sola instrucción, como en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

En este tutorial, usará la API fluida solo para la asignación de bases de datos que no puede realizar con atributos. Pero también se puede usar la API fluida para especificar casi todas las reglas de formato, validación y asignación que se pueden realizar mediante el uso de atributos. Algunos atributos como `MinimumLength` no se pueden aplicar con la API fluida. Como se mencionó anteriormente, `MinimumLength` no cambia el esquema, solo aplica una regla de validación del lado servidor y cliente.

Algunos desarrolladores prefieren usar la API fluida exclusivamente para mantener "limpias" las clases de entidad. Si quiere, puede mezclar atributos y la API fluida, y hay algunas personalizaciones que solo se pueden realizar mediante la API fluida, pero en general el procedimiento recomendado es elegir uno de estos dos enfoques y usarlo de forma constante siempre que sea posible.

Para agregar las nuevas entidades al modelo de datos y realizar la asignación de base de datos que no se ha realizado mediante atributos, reemplace el código de *DAL\SchoolContext.CS* por el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nueva instrucción en el método [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) configura la tabla de combinación de varios a varios:

- En el caso de la relación de varios a varios entre las entidades `Instructor` y `Course`, el código especifica los nombres de tabla y de columna para la tabla de combinación. Code First puede configurar la relación de varios a varios sin este código, pero si no lo llama, obtendrá nombres predeterminados como `InstructorInstructorID` para la columna `InstructorID`.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

El código siguiente proporciona un ejemplo de cómo podría haber usado la API fluida en lugar de atributos para especificar la relación entre las entidades `Instructor` y `OfficeAssignment`:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Para obtener información sobre qué instrucciones de "API fluida" se realizan en segundo plano, vea la entrada de blog sobre la [API fluida](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) .

## <a name="seed-database-with-test-data"></a>Inicializa la base de datos con datos de prueba

Reemplace el código del archivo *Migrations\Configuration.CS* por el código siguiente para proporcionar los datos de inicialización para las nuevas entidades que ha creado.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Como vimos en el primer tutorial, la mayor parte de este código simplemente actualiza o crea nuevos objetos de entidad y carga los datos de ejemplo en las propiedades según sea necesario para las pruebas. Sin embargo, observe que la entidad `Course`, que tiene una relación de varios a varios con la entidad `Instructor`, se controla:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Cuando se crea un objeto de `Course`, se inicializa la propiedad de navegación `Instructors` como una colección vacía mediante el `Instructors = new List<Instructor>()`de código. Esto hace posible agregar `Instructor` entidades relacionadas con este `Course` mediante el método `Instructors.Add`. Si no ha creado una lista vacía, no podrá agregar estas relaciones, porque la propiedad `Instructors` sería NULL y no tendría un método `Add`. También puede Agregar la inicialización de la lista al constructor.

## <a name="add-a-migration"></a>Agregar una migración

Desde la PMC, escriba el comando `add-migration` (no haga el comando de `update-database` todavía):

`add-Migration ComplexDataModel`

Si ahora intentara ejecutar el comando `update-database` (no lo haga todavía), obtendría el error siguiente:

*La instrucción ALTER TABLE está en conflicto con la restricción FOREIGN KEY "FK\_DBO. Course\_DBO. Department\_DepartmentID ". El conflicto se produjo en la base de datos "ContosoUniversity", tabla "dbo. Department ", columna ' DepartmentID '.*

A veces, al ejecutar migraciones con datos existentes, debe insertar datos de código auxiliar en la base de datos para satisfacer las restricciones de clave externa y eso es lo que tiene que hacer ahora. El código generado en el método ComplexDataModel `Up` agrega una clave externa `DepartmentID` que no acepta valores NULL a la tabla `Course`. Dado que ya hay filas en la tabla `Course` cuando se ejecuta el código, se producirá un error en la operación de `AddColumn` porque SQL Server no sabe qué valor se debe colocar en la columna que no puede ser null. Por lo tanto, debe cambiar el código para asignar a la nueva columna un valor predeterminado y crear un departamento de código auxiliar denominado "Temp" para que actúe como el Departamento predeterminado. Como resultado, las filas `Course` existentes se relacionarán con el Departamento "Temp" después de que se ejecute el método `Up`. Puede relacionarlas con los departamentos correctos en el método `Seed`.

Edite la marca de tiempo de &lt; *&gt;\_archivo ComplexDataModel.CS* , convirtió en comentario la línea de código que agrega la columna DepartmentID a la tabla Course y agregue el siguiente código resaltado (también se resalta la línea comentada):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

Cuando se ejecute el método `Seed`, insertará filas en la tabla `Department` y se relacionarán las filas `Course` existentes con las filas nuevas `Department`. Si no ha agregado ningún curso en la interfaz de usuario, ya no necesitará el Departamento "Temp" o el valor predeterminado en la columna `Course.DepartmentID`. Para permitir la posibilidad de que alguien pueda haber agregado cursos mediante la aplicación, también debería actualizar el código del método `Seed` para asegurarse de que todas las filas `Course` (no solo las insertadas por ejecuciones anteriores del método `Seed`) tienen valores `DepartmentID` válidos antes de quitar el valor predeterminado de la columna y eliminar el Departamento "Temp".

## <a name="update-the-database"></a>Actualizar la base de datos

Una vez que haya terminado de editar la marca de tiempo de &lt; *&gt;\_archivo ComplexDataModel.CS* , escriba el comando `update-database` en el PMC para ejecutar la migración.

[!code-powershell[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.ps1)]

> [!NOTE]
> Es posible obtener otros errores al migrar datos y realizar cambios en el esquema. Si se producen errores de migración que no se pueden resolver, puede cambiar el nombre de la base de datos en la cadena de conexión o eliminar la base de datos. El enfoque más sencillo es cambiar el nombre de la base de datos en el archivo *Web. config* . En el ejemplo siguiente se muestra el nombre cambiado a CU\_test:
>
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample36.xml?highlight=1)]
>
> Con una base de datos nueva, no hay ningún dato para migrar y es mucho más probable que el comando `update-database` se complete sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, vea [quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).
>
> Si se produce un error, otra cosa que puede intentar es volver a inicializar la base de datos escribiendo el siguiente comando en la PMC:
>
> `update-database -TargetMigration:0`

Abra la base de datos en **Explorador de servidores** como hizo anteriormente y expanda el nodo **tablas** para ver que se han creado todas las tablas. (Si todavía tiene **Explorador de servidores** abierto desde la hora anterior, haga clic en el botón **Actualizar** ).

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image16.png)

No ha creado una clase de modelo para la tabla `CourseInstructor`. Como se explicó anteriormente, se trata de una tabla combinada para la relación de varios a varios entre las entidades `Instructor` y `Course`.

Haga clic con el botón secundario en la tabla `CourseInstructor` y seleccione **Mostrar datos de tabla** para comprobar que contiene datos como resultado de las entidades `Instructor` que agregó a la propiedad de navegación `Course.Instructors`.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image17.png)

## <a name="get-the-code"></a>Obtención del código

[Descargar proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en el [acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Personalización del modelo de datos
> * Entidad Student actualizada
> * Creado la entidad Instructor
> * Creado la entidad OfficeAssignment
> * Modificación de la entidad Course
> * Se creó la entidad Department.
> * Se modificó la entidad de inscripción
> * Código agregado al contexto de base de datos
> * Inicializado la base de datos con datos de prueba
> * Agregado una migración
> * Actualizado la base de datos

Avance al siguiente artículo para aprender a leer y Mostrar datos relacionados que el Entity Framework carga en las propiedades de navegación.

> [!div class="nextstepaction"]
> [Lectura de datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
