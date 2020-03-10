---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Control de la simultaneidad con el Entity Framework 4,0 en una aplicación Web de ASP.NET 4 | Microsoft Docs
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 3df5f7d9c8fb22e1ea34fe16560bdb9a1309bb56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513505"
---
# <a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Control de la simultaneidad con el Entity Framework 4,0 en una aplicación Web de ASP.NET 4

por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales Entity Framework 4,0. Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales. Si tiene alguna pregunta sobre los tutoriales, puede publicarlos en el [Foro de Entity Framework de ASP.net](https://forums.asp.net/1227.aspx).

En el tutorial anterior, aprendió a ordenar y filtrar datos mediante el control `ObjectDataSource` y el Entity Framework. En este tutorial se muestran las opciones para administrar la simultaneidad en una aplicación Web ASP.NET que usa el Entity Framework. Creará una nueva página web dedicada a actualizar las asignaciones de oficina del instructor. Podrá controlar los problemas de simultaneidad en esa página y en la página departamentos que creó anteriormente.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Conflictos de simultaneidad

Un conflicto de simultaneidad se produce cuando un usuario edita un registro y otro usuario edita el mismo registro antes de que el primer cambio del usuario se escriba en la base de datos. Si no configura la Entity Framework para detectar estos conflictos, quienquiera que actualice la base de datos sobrescribe los cambios del otro usuario. En muchas aplicaciones, este riesgo es aceptable y no es necesario configurar la aplicación para controlar posibles conflictos de simultaneidad. (Si hay pocos usuarios, o pocas actualizaciones, o si no es realmente crítico si se sobrescriben algunos cambios, el costo de la programación para la simultaneidad puede superar la ventaja). Si no necesita preocuparse por los conflictos de simultaneidad, puede omitir este tutorial; los dos tutoriales restantes de la serie no dependen de nada que cree en este.

### <a name="pessimistic-concurrency-locking"></a>Simultaneidad pesimista (bloqueo)

Si la aplicación necesita evitar la pérdida accidental de datos en casos de simultaneidad, una manera de hacerlo es usar los bloqueos de base de datos. Esto se denomina *simultaneidad pesimista*. Por ejemplo, antes de leer una fila de una base de datos, solicita un bloqueo de solo lectura o para acceso de actualización. Si bloquea una fila para acceso de actualización, no se permite que ningún otro usuario bloquee la fila como solo lectura o para acceso de actualización, porque recibirían una copia de los datos que se están modificando. Si bloquea una fila para acceso de solo lectura, otras personas también pueden bloquearla para acceso de solo lectura pero no para actualización.

La administración de bloqueos tiene algunas desventajas. Puede ser bastante complicado de programar. Requiere importantes recursos de administración de bases de datos y puede provocar problemas de rendimiento a medida que aumenta el número de usuarios de una aplicación (es decir, no se escala bien). Por estos motivos, no todos los sistemas de administración de bases de datos admiten la simultaneidad pesimista. El Entity Framework no proporciona compatibilidad integrada para ello, y en este tutorial no se muestra cómo implementarlo.

### <a name="optimistic-concurrency"></a>Simultaneidad optimista

La alternativa a la simultaneidad pesimista es la *simultaneidad optimista*. La simultaneidad optimista implica permitir que se produzcan conflictos de simultaneidad y reaccionar correctamente si ocurren. Por ejemplo, Juan ejecuta la página *Department. aspx* , hace clic en el vínculo **Editar** del Departamento de historial y reduce la cantidad de **presupuesto** de $1.000.000,00 a $125.000,00. (Juan administra un departamento competidor y desea liberar dinero para su propio departamento).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Antes de que John haga clic en **Actualizar**, Julia ejecuta la misma página, hace clic en el vínculo **Editar** del Departamento de historial y, a continuación, cambia el campo de **fecha de inicio** de 1/10/2011 a 1/1/1999. (Julia administra el Departamento de historial y desea darle más responsabilidad).

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

Juan hace clic en **Actualizar** primero y, a continuación, Julia hace clic en **Actualizar**. El explorador de Julia ahora muestra el importe de la **cotización** como $1.000.000,00, pero es incorrecto porque el importe ha cambiado por John a $125.000,00.

Algunas de las acciones que puede realizar en este escenario son las siguientes:

- Puede realizar un seguimiento de la propiedad que ha modificado un usuario y actualizar solo las columnas correspondientes de la base de datos. En el escenario de ejemplo, no se perdería ningún dato porque los dos usuarios actualizaron diferentes propiedades. La próxima vez que un usuario examine el Departamento de historial, verá 1/1/1999 y $125.000,00. 

    Este es el comportamiento predeterminado en el Entity Framework y puede reducir considerablemente el número de conflictos que podrían provocar la pérdida de datos. Sin embargo, este comportamiento no evita la pérdida de datos si se realizan cambios competitivos en la misma propiedad de una entidad. Además, este comportamiento no siempre es posible; al asignar procedimientos almacenados a un tipo de entidad, todas las propiedades de una entidad se actualizan cuando se realizan cambios en la entidad en la base de datos.
- Puede dejar que el cambio de Julia sobrescriba el cambio de John. Una vez que Julia haga clic en **Actualizar**, la cantidad de **presupuesto** volverá a $1.000.000,00. Esto se denomina un escenario de *Prevalece el cliente* o *Prevalece el último*. (Los valores del cliente tienen prioridad sobre lo que hay en el almacén de datos).
- Puede impedir que el cambio de Julia se actualice en la base de datos. Normalmente, se muestra un mensaje de error, se muestra el estado actual de los datos y se le permite volver a escribir los cambios si todavía desea realizarlos. También puede automatizar el proceso si guarda su entrada y le proporciona la oportunidad de volver a aplicarla sin tener que volver a escribirla. Esto se denomina un escenario de *Prevalece el almacén*. (Los valores del almacén de datos tienen prioridad sobre los valores enviados por el cliente).

### <a name="detecting-concurrency-conflicts"></a>Detectar conflictos de simultaneidad

En el Entity Framework, puede resolver conflictos controlando `OptimisticConcurrencyException` excepciones que inicia la Entity Framework. Para saber cuándo se producen dichas excepciones, Entity Framework debe ser capaz de detectar conflictos. Por lo tanto, debe configurar correctamente la base de datos y el modelo de datos. Algunas opciones para habilitar la detección de conflictos son las siguientes:

- En la base de datos, incluya una columna de la tabla que se puede usar para determinar cuándo se ha cambiado una fila. Después, puede configurar el Entity Framework para incluir esa columna en la cláusula `Where` de los comandos SQL `Update` o `Delete`.

    Este es el propósito de la columna `Timestamp` en la tabla `OfficeAssignment`.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    El tipo de datos de la columna de `Timestamp` también se denomina `Timestamp`. Sin embargo, la columna no contiene realmente un valor de fecha u hora. En su lugar, el valor es un número secuencial que se incrementa cada vez que se actualiza la fila. En un comando `Update` o `Delete`, la cláusula `Where` incluye el valor de `Timestamp` original. Si otro usuario ha cambiado la fila que se está actualizando, el valor de `Timestamp` es diferente del valor original, por lo que la cláusula `Where` no devuelve ninguna fila para actualizar. Cuando el Entity Framework encuentra que no se ha actualizado ninguna fila con el comando actual o el `Update` de `Delete` (es decir, cuando el número de filas afectadas es cero), lo interpreta como un conflicto de simultaneidad.
- Configure el Entity Framework para incluir los valores originales de cada columna de la tabla en la cláusula `Where` de los comandos `Update` y `Delete`.

    Como en la primera opción, si alguna de las filas ha cambiado desde la primera vez que se leyó la fila, la cláusula `Where` no devolverá una fila para actualizar, lo que el Entity Framework interpreta como un conflicto de simultaneidad. Este método es tan eficaz como el uso de un campo de `Timestamp`, pero puede ser ineficaz. En el caso de las tablas de base de datos que tienen muchas columnas, puede dar lugar a cláusulas de `Where` muy grandes y, en una aplicación Web, puede requerir que mantenga grandes cantidades de estado. Mantener grandes cantidades de estado puede afectar al rendimiento de la aplicación porque requiere recursos del servidor (por ejemplo, el estado de la sesión) o debe incluirse en la propia página web (por ejemplo, el estado de vista).

En este tutorial, agregará control de errores para conflictos de simultaneidad optimista para una entidad que no tiene una propiedad de seguimiento (la entidad `Department`) y para una entidad que tiene una propiedad de seguimiento (la entidad `OfficeAssignment`).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Controlar la simultaneidad optimista sin una propiedad de seguimiento

Para implementar la simultaneidad optimista para la entidad `Department`, que no tiene una propiedad Tracking (`Timestamp`), realizará las tareas siguientes:

- Cambie el modelo de datos para habilitar el seguimiento de simultaneidad para entidades de `Department`.
- En la clase `SchoolRepository`, controle las excepciones de simultaneidad en el método `SaveChanges`.
- En la página *departments. aspx* , controle las excepciones de simultaneidad mostrando un mensaje a la advertencia del usuario de que los cambios que se han intentado no se han realizado correctamente. Después, el usuario puede ver los valores actuales y volver a intentar los cambios si todavía son necesarios.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Habilitar el seguimiento de simultaneidad en el modelo de datos

En Visual Studio, abra la aplicación web contoso University con la que estaba trabajando en el tutorial anterior de esta serie.

Abra *SchoolModel. edmx*y, en el diseñador de modelos de datos, haga clic con el botón secundario en la propiedad `Name` de la entidad `Department` y, a continuación, haga clic en **propiedades**. En la ventana **propiedades** , cambie la propiedad `ConcurrencyMode` a `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Haga lo mismo para las demás propiedades escalares que no sean de clave principal (`Budget`, `StartDate`y `Administrator`). (No se puede hacer esto para las propiedades de navegación). Esto especifica que, cada vez que el Entity Framework genera un comando SQL `Update` o `Delete` para actualizar la entidad `Department` en la base de datos, estas columnas (con valores originales) deben incluirse en la cláusula `Where`. Si no se encuentra ninguna fila cuando se ejecuta el comando `Update` o `Delete`, el Entity Framework producirá una excepción de simultaneidad optimista.

Guarde y cierre el modelo de datos.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Controlar las excepciones de simultaneidad en la capa DAL

Abra *SchoolRepository.CS* y agregue la siguiente instrucción `using` para el espacio de nombres `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Agregue el siguiente método `SaveChanges` nuevo, que controla las excepciones de simultaneidad optimista:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Si se produce un error de simultaneidad cuando se llama a este método, los valores de propiedad de la entidad en memoria se reemplazan por los valores que se encuentran actualmente en la base de datos. Se vuelve a producir la excepción de simultaneidad para que la página web pueda controlarla.

En los métodos `DeleteDepartment` y `UpdateDepartment`, reemplace la llamada existente a `context.SaveChanges()` por una llamada a `SaveChanges()` para invocar el nuevo método.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Controlar las excepciones de simultaneidad en el nivel de presentación

Abra *departments. aspx* y agregue un atributo `OnDeleted="DepartmentsObjectDataSource_Deleted"` al control `DepartmentsObjectDataSource`. La etiqueta de apertura del control ahora será similar a la del ejemplo siguiente.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

En el control `DepartmentsGridView`, especifique todas las columnas de la tabla en el atributo `DataKeyNames`, tal y como se muestra en el ejemplo siguiente. Tenga en cuenta que esto creará campos de estado de vista muy grandes, que es una de las razones por las que el uso de un campo de seguimiento suele ser la mejor manera de realizar un seguimiento de los conflictos de simultaneidad.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Abra *departments.aspx.CS* y agregue la siguiente instrucción `using` para el espacio de nombres `System.Data`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Agregue el siguiente método nuevo, al que se llamará desde los controladores de eventos `Updated` y `Deleted` del control de origen de datos para controlar las excepciones de simultaneidad:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Este código comprueba el tipo de excepción y, si se trata de una excepción de simultaneidad, el código crea dinámicamente un control `CustomValidator` que, a su vez, muestra un mensaje en el control `ValidationSummary`.

Llame al nuevo método desde el `Updated` controlador de eventos que agregó anteriormente. Además, cree un nuevo controlador de eventos `Deleted` que llame al mismo método (pero no haga nada más):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Prueba de la simultaneidad optimista en la página de departamentos

Ejecute la página *departments. aspx* .

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Haga clic en **Editar** en una fila y cambie el valor de la columna **presupuesto** . (Recuerde que solo puede editar los registros que ha creado para este tutorial, ya que los registros de la base de datos de `School` existentes contienen algunos datos no válidos. El registro del Departamento de economía es un seguro con el que experimentar.

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Abra una nueva ventana del explorador y vuelva a ejecutar la página (Copie la dirección URL del cuadro de dirección de la primera ventana del explorador en la segunda ventana del explorador).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Haga clic en **Editar** en la misma fila que editó anteriormente y cambie el valor de **presupuesto** a algo diferente.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

En la segunda ventana del explorador, haga clic en **Actualizar**. El importe del **presupuesto** se ha cambiado correctamente a este nuevo valor.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

En la primera ventana del explorador, haga clic en **Actualizar**. Se produce un error en la actualización. El importe del **presupuesto** se vuelve a mostrar con el valor establecido en la segunda ventana del explorador y se muestra un mensaje de error.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Controlar la simultaneidad optimista mediante una propiedad de seguimiento

Para controlar la simultaneidad optimista de una entidad que tiene una propiedad de seguimiento, realizará las tareas siguientes:

- Agregue procedimientos almacenados al modelo de datos para administrar entidades `OfficeAssignment`. (Las propiedades de seguimiento y los procedimientos almacenados no tienen que usarse juntos; se agrupan aquí para ilustrarlos).
- Agregue métodos CRUD a la capa DAL y el BLL para entidades `OfficeAssignment`, incluido el código para controlar las excepciones de simultaneidad optimista en la capa DAL.
- Cree una página web de asignaciones de Office.
- Pruebe la simultaneidad optimista en la nueva página web.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Agregar procedimientos almacenados OfficeAssignment al modelo de datos

Abra el archivo *SchoolModel. edmx* en el diseñador de modelos, haga clic con el botón secundario en la superficie de diseño y haga clic en **Actualizar modelo desde base de datos**. En la pestaña **Agregar** del cuadro de diálogo **Elija los objetos de base de datos** , expanda **procedimientos almacenados** y seleccione los tres `OfficeAssignment` procedimientos almacenados (vea la siguiente captura de pantalla) y, a continuación, haga clic en **Finalizar**. (Estos procedimientos almacenados ya estaban en la base de datos cuando lo descargó o creó mediante un script).

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Haga clic con el botón secundario en la entidad `OfficeAssignment` y seleccione **asignación de procedimiento almacenado**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Establezca las funciones de **inserción**, **actualización**y **eliminación** para usar sus procedimientos almacenados correspondientes. Para el parámetro `OrigTimestamp` de la función `Update`, establezca la **propiedad** en `Timestamp` y seleccione la opción **usar valor original** .

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Cuando el Entity Framework llama al procedimiento almacenado `UpdateOfficeAssignment`, pasará el valor original de la columna `Timestamp` en el parámetro `OrigTimestamp`. El procedimiento almacenado utiliza este parámetro en su cláusula `Where`:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

El procedimiento almacenado también selecciona el nuevo valor de la columna `Timestamp` después de la actualización para que el Entity Framework pueda mantener la entidad `OfficeAssignment` que está en la memoria sincronizada con la fila de base de datos correspondiente.

(Tenga en cuenta que el procedimiento almacenado para eliminar una asignación de Office no tiene un parámetro `OrigTimestamp`. Por este motivo, el Entity Framework no puede comprobar que una entidad no ha cambiado antes de eliminarla.)

Guarde y cierre el modelo de datos.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Agregar métodos OfficeAssignment a la capa DAL

Abra *ISchoolRepository.CS* y agregue los siguientes métodos CRUD para el conjunto de entidades `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Agregue los siguientes métodos nuevos a *SchoolRepository.CS*. En el método `UpdateOfficeAssignment`, se llama al método de `SaveChanges` local en lugar de a `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

En el proyecto de prueba, Abra *MockSchoolRepository.CS* y agregue la siguiente `OfficeAssignment` colección y métodos CRUD. (El repositorio ficticio debe implementar la interfaz del repositorio o la solución no se compilará).

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Agregar métodos OfficeAssignment a la capa BLL

En el proyecto principal, Abra *SchoolBL.CS* y agregue los siguientes métodos CRUD para el conjunto de entidades `OfficeAssignment`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Crear una página web de OfficeAssignments

Cree una nueva página web que use la página maestra *site. Master* y asígnele el nombre *OfficeAssignments. aspx*. Agregue el marcado siguiente al control `Content` denominado `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Observe que en el atributo `DataKeyNames`, el marcado especifica la propiedad `Timestamp` así como la clave de registro (`InstructorID`). Al especificar propiedades en el atributo `DataKeyNames`, el control los guarda en el estado de control (que es similar al estado de vista) para que los valores originales estén disponibles durante el procesamiento de los postback.

Si no guardó el valor de `Timestamp`, el Entity Framework no lo tendría para la cláusula `Where` del comando de SQL `Update`. En consecuencia, no se puede actualizar nada. Como resultado, el Entity Framework produciría una excepción de simultaneidad optimista cada vez que se actualiza una entidad de `OfficeAssignment`.

Abra *OfficeAssignments.aspx.CS* y agregue la siguiente instrucción `using` para la capa de acceso a datos:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Agregue el siguiente método de `Page_Init`, que habilita la funcionalidad de datos dinámicos. Agregue también el siguiente controlador para el evento de `Updated` del control `ObjectDataSource` para comprobar los errores de simultaneidad:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Prueba de la simultaneidad optimista en la página OfficeAssignments

Ejecute la página *OfficeAssignments. aspx* .

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Haga clic en **Editar** en una fila y cambie el valor en la columna **Ubicación** .

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Abra una nueva ventana del explorador y vuelva a ejecutar la página (Copie la dirección URL de la primera ventana del explorador en la segunda ventana del explorador).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Haga clic en **Editar** en la misma fila que editó anteriormente y cambie el valor de **Ubicación** a algo diferente.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

En la segunda ventana del explorador, haga clic en **Actualizar**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Cambie a la primera ventana del explorador y haga clic en **Actualizar**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Verá un mensaje de error y el valor de **Ubicación** se ha actualizado para mostrar el valor en el que lo ha cambiado en la segunda ventana del explorador.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Controlar la simultaneidad con el control EntityDataSource

El control `EntityDataSource` incluye lógica integrada que reconoce la configuración de simultaneidad en el modelo de datos y controla las operaciones de actualización y eliminación en consecuencia. Sin embargo, al igual que con todas las excepciones, debe controlar las excepciones de `OptimisticConcurrencyException` para proporcionar un mensaje de error descriptivo.

A continuación, configurará la página *Courses. aspx* (que usa un control de `EntityDataSource`) para permitir las operaciones de actualización y eliminación y para mostrar un mensaje de error si se produce un conflicto de simultaneidad. La entidad `Course` no tiene una columna de seguimiento de simultaneidad, por lo que usará el mismo método que hizo con la entidad `Department`: realice un seguimiento de los valores de todas las propiedades que no son de clave.

Abra el archivo *SchoolModel. edmx* . Para las propiedades que no son de clave de la entidad `Course` (`Title`, `Credits`y `DepartmentID`), establezca la propiedad **modo de simultaneidad** en `Fixed`. A continuación, guarde y cierre el modelo de datos.

Abra la página *Courses. aspx* y realice los cambios siguientes:

- En el control `CoursesEntityDataSource`, agregue los atributos `EnableUpdate="true"` y `EnableDelete="true"`. La etiqueta de apertura de ese control ahora es similar al ejemplo siguiente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- En el control `CoursesGridView`, cambie el valor del atributo `DataKeyNames` a `"CourseID,Title,Credits,DepartmentID"`. A continuación, agregue un elemento `CommandField` al elemento `Columns` que muestra los botones de **edición** y **eliminación** (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). El control `GridView` ahora es similar al ejemplo siguiente:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Ejecute la página y cree una situación de conflicto como hizo antes en la página departamentos. Ejecute la página en dos ventanas del explorador, haga clic en **Editar** en la misma línea en cada ventana y realice un cambio diferente en cada una de ellas. Haga clic en **Actualizar** en una ventana y, a continuación, haga clic en **Actualizar** en la otra ventana. Al hacer clic en **Actualizar** la segunda vez, verá la página de error que resulta de una excepción de simultaneidad no controlada.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Este error se controla de forma muy similar a como se controla en el control de `ObjectDataSource`. Abra la página *Courses. aspx* y, en el control `CoursesEntityDataSource`, especifique los controladores para los eventos `Deleted` y `Updated`. La etiqueta de apertura del control ahora es similar al ejemplo siguiente:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Antes del control `CoursesGridView`, agregue el siguiente control de `ValidationSummary`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

En *Courses.aspx.CS*, agregue una instrucción `using` para el espacio de nombres `System.Data`, agregue un método que compruebe las excepciones de simultaneidad y agregue Controladores para los controladores de `Updated` y `Deleted` del control `EntityDataSource`. El código tendrá un aspecto similar al siguiente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

La única diferencia entre este código y lo que hizo para el control `ObjectDataSource` es que, en este caso, la excepción de simultaneidad está en la propiedad `Exception` del objeto argumentos de evento en lugar de en la propiedad `InnerException` de esa excepción.

Ejecute la página y vuelva a crear un conflicto de simultaneidad. Esta vez aparece un mensaje de error:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Con esto finaliza la introducción para el control de los conflictos de simultaneidad. En el siguiente tutorial se proporcionan instrucciones sobre cómo mejorar el rendimiento en una aplicación web que utiliza el Entity Framework.

> [!div class="step-by-step"]
> [Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Siguiente](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
