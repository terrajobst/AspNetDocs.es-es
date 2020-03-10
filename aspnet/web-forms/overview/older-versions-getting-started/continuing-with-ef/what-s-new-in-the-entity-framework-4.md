---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Novedades del Entity Framework 4,0 | Microsoft Docs
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 29d2bd61e8361b130e7b9415dad4fe1d5b847945
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78522151"
---
# <a name="whats-new-in-the-entity-framework-40"></a>Novedades de Entity Framework 4.0

por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales Entity Framework. Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales. Si tiene alguna pregunta sobre los tutoriales, puede publicarlos en el [Foro de Entity Framework de ASP.net](https://forums.asp.net/1227.aspx).

En el tutorial anterior vio algunos métodos para maximizar el rendimiento de una aplicación web que utiliza el Entity Framework. En este tutorial se revisan algunas de las nuevas características más importantes de la versión 4 del Entity Framework y se incluyen vínculos a recursos que proporcionan una introducción más completa a todas las características nuevas. Entre las características resaltadas en este tutorial se incluyen las siguientes:

- Asociaciones de clave externa.
- Ejecutar comandos SQL definidos por el usuario.
- Desarrollo del modelo primero.
- Compatibilidad con POCO.

Además, el tutorial presentará brevemente el *desarrollo de código primero*, una característica que estará disponible en la próxima versión de la Entity Framework.

Para iniciar el tutorial, inicie Visual Studio y abra la aplicación web contoso University con la que estaba trabajando en el tutorial anterior.

## <a name="foreign-key-associations"></a>Asociaciones de clave externa

La versión 3,5 de las propiedades de navegación incluidas en el Entity Framework, pero no incluye las propiedades de clave externa en el modelo de datos. Por ejemplo, las columnas `CourseID` y `StudentID` de la tabla `StudentGrade` se omitirán de la entidad `StudentGrade`.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

La razón de este enfoque era que, estrictamente hablando, las claves externas son un detalle de la implementación física y no pertenecen a un modelo de datos conceptual. Sin embargo, en la práctica, a menudo es más fácil trabajar con entidades en el código cuando se tiene acceso directo a las claves externas.

Para obtener un ejemplo de cómo las claves externas en el modelo de datos pueden simplificar el código, tenga en cuenta cómo habría tenido que codificar la página *DepartmentsAdd. aspx* sin ellos. En la entidad `Department`, la propiedad `Administrator` es una clave externa que corresponde a `PersonID` en la entidad `Person`. Con el fin de establecer la asociación entre un nuevo departamento y su administrador, lo único que debía hacer es establecer el valor de la propiedad `Administrator` en el controlador de eventos `ItemInserting` del control DataBound:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Sin las claves externas en el modelo de datos, se podría controlar el evento `Inserting` del control de origen de datos en lugar del evento `ItemInserting` del control enlazado a datos, para obtener una referencia a la propia entidad antes de que la entidad se agregue al conjunto de entidades. Cuando tenga esa referencia, establezca la asociación mediante código como el que se muestran en los ejemplos siguientes:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Como puede ver en la entrada de blog del equipo de Entity Framework [en asociaciones de clave externa](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), hay otros casos en los que la diferencia en la complejidad del código es mucho mayor. Para satisfacer las necesidades de aquellos que prefieren vivir con los detalles de implementación en el modelo de datos conceptual con el fin de que el código sea más sencillo, el Entity Framework ahora ofrece la opción de incluir claves externas en el modelo de datos.

En Entity Framework terminología, si incluye claves externas en el modelo de datos que usa asociaciones de *clave externa*, y si excluye claves externas, se usan *asociaciones independientes*.

## <a name="executing-user-defined-sql-commands"></a>Ejecutar comandos SQL definidos por el usuario

En las versiones anteriores del Entity Framework, no había ninguna manera fácil de crear sus propios comandos SQL sobre la marcha y ejecutarlos. El Entity Framework comandos SQL generados dinámicamente, o bien tenía que crear un procedimiento almacenado e importarlo como una función. La versión 4 agrega `ExecuteStoreQuery` y `ExecuteStoreCommand` métodos de la clase `ObjectContext` que facilitan la transferencia de cualquier consulta directamente a la base de datos.

Supongamos que los administradores de Contoso University quieren realizar cambios masivos en la base de datos sin tener que pasar por el proceso de creación de un procedimiento almacenado e importarlo en el modelo de datos. Su primera solicitud es para una página que les permite cambiar el número de créditos para todos los cursos de la base de datos. En la página web, desean poder escribir un número que se utilizará para multiplicar el valor de cada columna de `Credits` de `Course` fila.

Cree una nueva página que use la página maestra *site. Master* y asígnele el nombre *UpdateCredits. aspx*. A continuación, agregue el siguiente marcado al control `Content` denominado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Este marcado crea un control de `TextBox` en el que el usuario puede especificar el valor de multiplicador, un control de `Button` al que hacer clic para ejecutar el comando y un control de `Label` para indicar el número de filas afectadas.

Abra *UpdateCredits.aspx.CS*y agregue la siguiente instrucción de `using` y un controlador para el evento de `Click` del botón:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Este código ejecuta el comando de `Update` de SQL con el valor del cuadro de texto y usa la etiqueta para mostrar el número de filas afectadas. Antes de ejecutar la página, ejecute la página *Courses. aspx* para obtener una imagen "anterior" de algunos datos.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Ejecute *UpdateCredits. aspx*, escriba "10" como multiplicador y, a continuación, haga clic en **Ejecutar**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Vuelva a ejecutar la página *Courses. aspx* para ver los datos modificados.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Si desea volver a establecer el número de créditos en sus valores originales, en *UpdateCredits.aspx.CS* cambie `Credits * {0}` a `Credits / {0}` y vuelva a ejecutar la página, escribiendo 10 como divisor).

Para obtener más información sobre cómo ejecutar consultas que se definen en el código, vea [Cómo: ejecutar directamente comandos en el origen de datos](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Desarrollo del modelo primero

En estos tutoriales, se creó primero la base de datos y, a continuación, se generó el modelo de datos en función de la estructura de la base de datos. En el Entity Framework 4 puede empezar con el modelo de datos en su lugar y generar la base de datos en función de la estructura del modelo de datos. Si está creando una aplicación para la que aún no existe la base de datos, el enfoque del modelo primero le permite crear entidades y relaciones que tengan sentido conceptualmente para la aplicación, sin preocuparse por los detalles de la implementación física. . (Sin embargo, esto solo es aplicable a través de las fases iniciales de desarrollo. Al final, la base de datos se creará y tendrá datos de producción en ella y la recreación del modelo dejará de ser práctica. en ese momento, volverá al enfoque de la base de datos primero).

En esta sección del tutorial, creará un modelo de datos simple y generará la base de datos a partir de él.

En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *Dal* y seleccione **Agregar nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , en **plantillas instaladas** , seleccione **datos** y, a continuación, seleccione la plantilla **ADO.NET Entity Data Model** . Asigne al nuevo archivo el nombre *AlumniAssociationModel. edmx* y haga clic en **Agregar**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Se iniciará el Asistente para Entity Data Model. En el paso **elegir contenido del modelo** , seleccione **modelo vacío** y, a continuación, haga clic en **Finalizar**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

El **Diseñador de Entity Data Model** se abre con una superficie de diseño en blanco. Arrastre un elemento de **entidad** desde el **cuadro de herramientas** hasta la superficie de diseño.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Cambie el nombre de la entidad de `Entity1` a `Alumnus`, cambie el nombre de la propiedad `Id` a `AlumnusId`y agregue una nueva propiedad escalar denominada `Name`. Para agregar nuevas propiedades, puede presionar entrar después de cambiar el nombre de la columna `Id`, o bien hacer clic con el botón secundario en la entidad y seleccionar **Agregar propiedad escalar**. El tipo predeterminado para las nuevas propiedades es `String`, que es adecuado para esta sencilla demostración, pero, por supuesto, puede cambiar cosas como tipo de datos en la ventana **propiedades** .

Cree otra entidad de la misma manera y asígnele el nombre `Donation`. Cambie la propiedad `Id` a `DonationId` y agregue una propiedad escalar denominada `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Para agregar una asociación entre estas dos entidades, haga clic con el botón secundario en la entidad `Alumnus`, seleccione **Agregar**y, a continuación, seleccione **Asociación**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Los valores predeterminados del cuadro de diálogo **Agregar Asociación** son lo que desea (uno a varios, incluir propiedades de navegación, incluir claves externas), por lo que solo tiene que hacer clic en **Aceptar**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

El diseñador agrega una línea de asociación y una propiedad de clave externa.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Ahora está listo para crear la base de datos. Haga clic con el botón secundario en la superficie de diseño y seleccione **generar base de datos a partir del modelo**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Esto inicia el Asistente para generar base de datos. (Si ve advertencias que indican que las entidades no están asignadas, puede omitirlas por el momento).

En el paso **elegir la conexión de datos** , haga clic en **nueva conexión**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

En el cuadro de diálogo **propiedades de conexión** , seleccione la instancia de SQL Server Express local y asigne el nombre `AlumniAssociation`a la base de datos.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Haga clic en **sí** cuando se le pregunte si desea crear la base de datos. Cuando vuelva a aparecer el paso **elegir la conexión de datos** , haga clic en **siguiente**.

En el paso **Resumen y configuración** , haga clic en **Finalizar**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

Se crea un archivo *. SQL* con los comandos del lenguaje de definición de datos (DDL), pero aún no se han ejecutado los comandos.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Use una herramienta como **SQL Server Management Studio** para ejecutar el script y crear las tablas, como podría haber hecho cuando creó la base de datos `School` para [el primer tutorial de la serie de tutoriales de introducción](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (A menos que haya descargado la base de datos).

Ahora puede usar el modelo de datos `AlumniAssociation` en las páginas web de la misma forma que ha estado usando el modelo de `School`. Para probar esto, agregue algunos datos a las tablas y cree una página web que muestre los datos.

Con **Explorador de servidores**, agregue las siguientes filas a las tablas `Alumnus` y `Donation`.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Cree una nueva página web denominada *ex. aspx* que use la página maestra *site. Master* . Agregue el marcado siguiente al control `Content` denominado `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Este marcado crea controles `GridView` anidados, el externo para mostrar los nombres de los alumnos y el interno para mostrar las fechas y los importes de la donación.

Abra *Alumni.aspx.CS*. Agregue una instrucción `using` para la capa de acceso a datos y un controlador para el evento `RowDataBound` del control de `GridView` externo:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Este código enlaza el control `GridView` interno mediante la propiedad de navegación `Donations` de la entidad `Alumnus` de la fila actual.

Ejecute la página.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Nota: esta página se incluye en el proyecto descargable, pero para que funcione, debe crear la base de datos en la instancia local de SQL Server Express; la base de datos no se incluye como un archivo *. MDF* en la carpeta *App\_Data* ).

Para obtener más información sobre el uso de la característica modelo primero del Entity Framework, vea [modelo-First en el Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Compatibilidad con POCO

Cuando se utiliza la metodología de diseño controlado por dominios, se diseñan clases de datos que representan los datos y el comportamiento pertinente para el dominio empresarial. Estas clases deben ser independientes de cualquier tecnología específica utilizada para almacenar (conservar) los datos; en otras palabras, deben ser *ignorados*en la persistencia. La persistencia omisión también puede facilitar la prueba unitaria, ya que el proyecto de prueba unitaria puede usar cualquier tecnología de persistencia que sea más conveniente para las pruebas. Las versiones anteriores de la Entity Framework ofrecían compatibilidad limitada para la persistencia omisión porque las clases de entidad tenían que heredar de la clase `EntityObject` y, por tanto, incluían una gran cantidad de funcionalidad específica de Entity Framework.

En el Entity Framework 4 se presenta la capacidad de usar clases de entidad que no se heredan de la clase `EntityObject` y, por tanto, se ignora la persistencia. En el contexto de la Entity Framework, las clases como esta suelen denominarse *objetos CLR antiguos sin formato* (POCO o poco). Puede escribir clases POCO manualmente o puede generarlas automáticamente en función de un modelo de datos existente mediante las plantillas del kit de herramientas de transformación de plantillas de texto (T4) proporcionadas por el Entity Framework.

Para obtener más información sobre el uso de POCO en el Entity Framework, vea los siguientes recursos:

- [Trabajar con entidades poco](https://msdn.microsoft.com/library/dd456853.aspx). Se trata de un documento de MSDN que es una visión general de POCO, con vínculos a otros documentos que contienen información más detallada.
- [Tutorial: plantilla poco para el Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) Se trata de una entrada de blog del equipo de desarrollo de Entity Framework, con vínculos a otras entradas de blog sobre POCO.

## <a name="code-first-development"></a>Desarrollo de código primero

La compatibilidad con POCO en el Entity Framework 4 todavía requiere la creación de un modelo de datos y la vinculación de las clases de entidad al modelo de datos. La siguiente versión del Entity Framework incluirá una característica denominada *desarrollo de código primero*. Esta característica permite usar el Entity Framework con sus propias clases POCO, sin necesidad de usar el diseñador de modelos de datos o un archivo XML de modelo de datos. (Por lo tanto, esta opción también se denomina *solo código*; *code-First* y *code-Only* hacen referencia a la misma característica Entity Framework).

Para obtener más información sobre cómo usar el enfoque de primer código para el desarrollo, vea los siguientes recursos:

- [Desarrollo de código primero con Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Se trata de una entrada de blog de Scott Guthrie que presenta el desarrollo de código primero.
- [Blog del equipo de desarrollo de Entity Framework: publica CodeOnly etiquetadas](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog del equipo de desarrollo de Entity Framework: Entradas etiquetadas Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Tutorial de la tienda de música de MVC, parte 4: modelos y acceso a datos](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Introducción con MVC 3, parte 4: Entity Framework desarrollo de código primero](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Además, se prevé que se publique un nuevo tutorial de código MVC en primer lugar que cree una aplicación similar a la aplicación contoso University en el muelle de 2011 en [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Más información

Con esto se completa la información general sobre las novedades del Entity Framework y se continúa con la serie de tutoriales Entity Framework. Para obtener más información acerca de las nuevas características del Entity Framework 4 que no se describen aquí, consulte los siguientes recursos:

- [Novedades de ADO.net](https://msdn.microsoft.com/library/ex6y04yf.aspx) Tema de MSDN sobre las nuevas características de la versión 4 de la Entity Framework.
- [Presentación de la versión de Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) La entrada de blog del equipo de desarrollo de Entity Framework acerca de las nuevas características de la versión 4.

> [!div class="step-by-step"]
> [Anterior](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
