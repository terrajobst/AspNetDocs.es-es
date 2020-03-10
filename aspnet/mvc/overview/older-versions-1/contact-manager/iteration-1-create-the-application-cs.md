---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: '#1 de iteración: cree laC#aplicación () | Microsoft Docs'
author: microsoft
description: 'En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y D...'
ms.author: riande
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: d3a940308f21a4f87bf80249bd465e8812794f68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470269"
---
# <a name="iteration-1--create-the-application-c"></a>#1 de iteración: cree laC#aplicación ()

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y eliminar (CRUD).

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

En esta primera iteración, se crea la aplicación básica. El objetivo es crear el administrador de contactos de la manera más rápida y sencilla posible. En iteraciones posteriores, mejoramos el diseño de la aplicación.

La aplicación Contact Manager es una aplicación básica controlada por base de datos. Puede usar la aplicación para crear nuevos contactos, editar contactos existentes y eliminar contactos.

En esta iteración, se completan los pasos siguientes:

1. Aplicación ASP.NET MVC
2. Crear una base de datos para almacenar nuestros contactos
3. Generar una clase de modelo para nuestra base de datos con Microsoft Entity Framework
4. Cree una acción de controlador y una vista que nos permita enumerar todos los contactos en la base de datos.
5. Crear acciones de controlador y una vista que nos permita crear un nuevo contacto en la base de datos
6. Crear acciones de controlador y una vista que nos permita editar un contacto existente en la base de datos
7. Crear acciones de controlador y una vista que nos permita eliminar un contacto existente en la base de datos

## <a name="software-prerequisites"></a>Requisitos previos de software

En las aplicaciones ASP.NET MVC, debe tener Visual Studio 2008 o Visual Web Developer 2008 instalado en el equipo (Visual Web Developer es una versión gratuita de Visual Studio que no incluye todas las características avanzadas de Visual Studio). Puede descargar la versión de prueba de Visual Studio 2008 o Visual Web Developer de la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> En el caso de las aplicaciones ASP.NET MVC con Visual Web Developer, debe tener instalado Visual Web Developer Service Pack 1. Sin Service Pack 1, no se pueden crear proyectos de aplicación Web.

Marco de MVC de ASP.NET. Puede descargar el marco de MVC de ASP.NET desde la siguiente dirección:

[https://www.asp.net/mvc](../../../index.md)

En este tutorial, usamos Microsoft Entity Framework para tener acceso a una base de datos. El Entity Framework se incluye con .NET Framework 3,5 Service Pack 1. Puede descargar esta Service Pack desde la siguiente ubicación:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;d isplaylang = en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Como alternativa a la realización de cada una de estas descargas, puede aprovechar el instalador de plataforma web (Web PI). Puede descargar el Web PI desde la siguiente dirección:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>Proyecto ASP.NET MVC

Proyecto de aplicación web MVC de ASP.NET. Inicie Visual Studio y seleccione el archivo de opciones de menú **, nuevo proyecto**. Aparecerá el cuadro de diálogo **nuevo proyecto** (vea la figura 1). Seleccione el tipo de proyecto **Web** y la plantilla de **aplicación web MVC de ASP.net** . Asigne al nuevo proyecto el nombre *ContactManager* y haga clic en el botón Aceptar.

Asegúrese de que tiene .NET Framework 3,5 seleccionado en la lista desplegable de la parte superior derecha del cuadro de diálogo **nuevo proyecto** . De lo contrario, la plantilla de aplicación web MVC de ASP.NET no aparecerá.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Figura 01**: el cuadro de diálogo nuevo proyecto ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image2.png))

Aplicación ASP.NET MVC, aparece el cuadro de diálogo **crear proyecto de prueba unitaria** . Puede usar este cuadro de diálogo para indicar que desea crear y agregar un proyecto de prueba unitaria a la solución al crear la aplicación ASP.NET MVC. Aunque no vamos a compilar pruebas unitarias en esta iteración, debe seleccionar la opción **sí, crear un proyecto de prueba unitaria** porque tenemos previsto agregar pruebas unitarias en una iteración posterior. Agregar un proyecto de prueba cuando se crea por primera vez un nuevo proyecto de ASP.NET MVC es mucho más fácil que agregar un proyecto de prueba después de que se haya creado el proyecto de ASP.NET MVC.

> [!NOTE] 
> 
> Dado que Visual Web Developer no admite proyectos de prueba, no se obtiene el cuadro de diálogo crear proyecto de prueba unitaria al usar Visual Web Developer.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Figura 02**: el cuadro de diálogo crear proyecto de prueba unitaria ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image4.png))

La aplicación ASP.NET MVC aparece en la ventana Explorador de soluciones de Visual Studio (vea la figura 3). Si no ve la ventana Explorador de soluciones, puede abrir esta ventana seleccionando la vista de la opción de menú **Explorador de soluciones**. Tenga en cuenta que la solución contiene dos proyectos: el proyecto de MVC de ASP.NET y el proyecto de prueba. El proyecto ASP.NET MVC se denomina ContactManager y el proyecto de prueba se llama ContactManager. tests.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Figura 03**: la ventana de explorador de soluciones ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image6.png))

## <a name="deleting-the-project-sample-files"></a>Eliminar los archivos de ejemplo del proyecto

La plantilla de proyecto ASP.NET MVC incluye archivos de ejemplo para controladores y vistas. Antes de crear una nueva aplicación ASP.NET MVC, debe eliminar estos archivos. Para eliminar archivos y carpetas en la ventana de Explorador de soluciones, haga clic con el botón secundario en un archivo o carpeta y seleccione la opción de menú **eliminar**.

Debe eliminar los siguientes archivos del proyecto ASP.NET MVC:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Además, debe eliminar el siguiente archivo del proyecto de prueba:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Crear la base de datos

La aplicación Contact Manager es una aplicación web controlada por la base de datos. Usamos una base de datos para almacenar la información de contacto.

El marco de MVC de ASP.NET con cualquier base de datos moderna, como bases de datos de Microsoft SQL Server, Oracle, MySQL e IBM DB2. En este tutorial, se utiliza una base de datos de Microsoft SQL Server. Al instalar Visual Studio, se le ofrece la opción de instalar Microsoft SQL Server Express, que es una versión gratuita de la base de datos de Microsoft SQL Server.

Cree una nueva base de datos haciendo clic con el botón derecho en la carpeta de\_de la aplicación en la ventana de Explorador de soluciones y seleccionando la opción de menú **Agregar, nuevo elemento**. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione la categoría de datos y la plantilla de base de **datos** **SQL Server** (consulte la figura 4). Asigne a la nueva base de datos el nombre ContactManagerDB. MDF y haga clic en el botón Aceptar.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Figura 04**: creación de una nueva base de datos de Microsoft SQL Server Express ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image8.png))

Después de crear la nueva base de datos, la base de datos aparece en la carpeta app\_data de la ventana de Explorador de soluciones. Haga doble clic en el archivo ContactManager. MDF para abrir la ventana Explorador de servidores y conectarse a la base de datos.

> [!NOTE] 
> 
> La ventana de Explorador de servidores se denomina ventana de Explorador de bases de datos en el caso de Microsoft Visual Web Developer.

Puede utilizar la ventana Explorador de servidores para crear nuevos objetos de base de datos, como tablas de base de datos, vistas, desencadenadores y procedimientos almacenados. Haga clic con el botón secundario en la carpeta tablas y seleccione la opción de menú **Agregar nueva tabla**. Aparecerá el Diseñador de tablas de base de datos (consulte la figura 5).

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Figura 05**: diseñador de tablas de la base de datos ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image10.png))

Necesitamos crear una tabla que contenga las columnas siguientes:

<a id="0.1_table01"></a>

| **Nombre de la columna** | **Tipo de datos** | **Permitir valores NULL** |
| --- | --- | --- |
| Id. | int | false |
| Nombre | nvarchar(50) | false |
| LastName | nvarchar(50) | false |
| Teléfono | nvarchar(50) | false |
| Correo electrónico | nvarchar(255) | false |

La primera columna, la columna ID, es especial. Debe marcar la columna ID como una columna de identidad y una columna de clave principal. Para indicar que una columna es una columna de identidad, expanda las propiedades de columna (mire en la parte inferior de la figura 6) y desplácese hacia abajo hasta la propiedad especificación de identidad. Establezca la propiedad **(identidad)** en el valor **sí**.

Para marcar una columna como una columna de clave principal, seleccione la columna y haga clic en el botón con el icono de una tecla. Una vez que se ha marcado una columna como columna de clave principal, aparece un icono de una clave junto a la columna (vea la figura 6).

Cuando termine de crear la tabla, haga clic en el botón Guardar (el botón con el icono de un disquete) para guardar la nueva tabla. Asigne a la nueva tabla el nombre *contactos*.

Después de terminar de crear la tabla de la base de datos Contacts, debe agregar algunos registros a la tabla. Haga clic con el botón secundario en la tabla contactos de la ventana de Explorador de servidores y seleccione la opción de menú **Mostrar datos de tabla**. Escriba uno o más contactos en la cuadrícula que aparece.

## <a name="creating-the-data-model"></a>Crear el modelo de datos

La aplicación ASP.NET MVC consta de modelos, vistas y controladores. Comenzaremos por crear una clase de modelo que represente la tabla Contacts que hemos creado en la sección anterior.

En este tutorial, usamos Microsoft Entity Framework para generar automáticamente una clase de modelo a partir de la base de datos.

> [!NOTE] 
> 
> El marco de MVC de ASP.NET no está asociado a Microsoft Entity Framework de ninguna manera. Puede usar ASP.NET MVC con tecnologías de acceso a bases de datos alternativas, como NHibernate, LINQ to SQL o ADO.NET.

Siga estos pasos para crear las clases del modelo de datos:

1. Haga clic con el botón derecho en la carpeta modelos de la ventana de Explorador de soluciones y seleccione **Agregar, nuevo elemento**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento** (vea la figura 6).
2. Seleccione la categoría de **datos** y la plantilla de **Entity Data Model de ADO.net** . Asigne al modelo de datos el nombre *ContactManagerModel. edmx* y haga clic en el botón **Agregar** . Aparece el Asistente para Entity Data Model (vea la figura 7).
3. En el paso **elegir contenido del modelo** , seleccione **generar desde la base de datos** (consulte la figura 7).
4. En el paso **elegir la conexión de datos** , seleccione la base de datos ContactManagerDB. MDF y escriba el nombre *ContactManagerDBEntities* para la configuración de la conexión de entidad (vea la figura 8).
5. En el paso **Elija los objetos de base de datos** , active la casilla tables (ver las tablas) (consulte la figura 9). El modelo de datos incluirá todas las tablas contenidas en la base de datos (solo hay una, la tabla Contacts). Escriba los *modelos*de espacio de nombres. Haga clic en el botón Finalizar para completar el asistente.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Figura 06**: cuadro de diálogo Agregar nuevo elemento ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image12.png))

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Figura 07**: elegir contenido del modelo ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image14.png))

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Figura 08**: elegir la conexión de datos ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image16.png))

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Figura 09**: elegir los objetos de base de datos ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image18.png))

Después de completar el Asistente para Entity Data Model, aparece el diseñador de Entity Data Model. El diseñador muestra una clase que corresponde a cada tabla que se está modelando. Debería ver una clase denominada Contacts.

El Asistente para Entity Data Model genera nombres de clase basados en nombres de tabla de base de datos. Casi siempre es necesario cambiar el nombre de la clase generada por el asistente. Haga clic con el botón secundario en la clase contactos en el diseñador y seleccione la opción de menú **cambiar nombre**. Cambie el nombre de la clase de contactos (plural) a contacto (singular). Después de cambiar el nombre de la clase, la clase debe aparecer como la figura 10.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Figura 10**: la clase Contact ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image20.png))

En este punto, hemos creado nuestro modelo de base de datos. Podemos usar la clase Contact para representar un registro de contacto determinado en nuestra base de datos.

## <a name="creating-the-home-controller"></a>Crear el controlador de inicio

El siguiente paso es crear nuestro controlador de inicio. El controlador Home es el controlador predeterminado que se invoca en una aplicación ASP.NET MVC.

Cree la clase de controlador de inicio haciendo clic con el botón derecho en la carpeta Controllers de la ventana de Explorador de soluciones y seleccionando la opción de menú **Agregar, controlador** (vea la figura 11). Observe la casilla **Agregar métodos de acción para los escenarios de creación, actualización y detalles**. Asegúrese de que esta casilla está activada antes de hacer clic en el botón **Agregar** .

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Figura 11**: adición del controlador de inicio ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image22.png))

Al crear el controlador Home, obtendrá la clase en la lista 1.

**Lista 1-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Enumerar los contactos

Para mostrar los registros en la tabla de la base de datos Contacts, es necesario crear una acción index () y una vista de índice.

El controlador Home ya contiene una acción index (). Tenemos que modificar este método para que se parezca a la lista 2.

**Lista 2-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Tenga en cuenta que la clase de controlador de inicio de la lista 2 contiene un campo privado denominado \_entidades. El campo \_entidades representa las entidades del modelo de datos. Usamos el campo \_entidades para comunicarse con la base de datos.

El método index () devuelve una vista que representa todos los contactos de la tabla de la base de datos Contacts. Expresión \_entidades. ContactSet. ToList () devuelve la lista de contactos como una lista genérica.

Ahora que hemos creado el controlador de índice, necesitamos crear la vista de índice. Antes de crear la vista de índice, compile la aplicación seleccionando la opción de menú **generar, compilar solución**. Siempre debe compilar el proyecto antes de agregar una vista para que la lista de clases de modelo se muestre en el cuadro de diálogo **Agregar vista** .

Para crear la vista de índice, haga clic con el botón derecho en el método index () y seleccione la opción de menú **Agregar vista** (vea la figura 12). Al seleccionar esta opción de menú, se abre el cuadro de diálogo **Agregar vista** (vea la figura 13).

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Figura 12**: adición de la vista de índice ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image24.png))

En el cuadro de diálogo **Agregar vista** , active la casilla **crear una vista fuertemente tipada**. Seleccione la clase de datos de vista ContactManager. Models. contact y la lista ver contenido. Al seleccionar estas opciones se genera una vista que muestra una lista de los registros de contactos.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Figura 13**: cuadro de diálogo Agregar vista ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image26.png))

Al hacer clic en el botón **Agregar** , se genera la vista de índice de la lista 3. Observe la Directiva &lt;% @ Page%&gt; que aparece en la parte superior del archivo. La vista de índice hereda de la clase ViewPage&lt;IEnumerable&lt;ContactManager. Models. contact&gt;&gt;. En otras palabras, la clase de modelo de la vista representa una lista de entidades contact.

El cuerpo de la vista de índice contiene un bucle foreach que recorre en iteración cada uno de los contactos representados por la clase de modelo. El valor de cada propiedad de la clase Contact se muestra en una tabla HTML.

**Lista 3-Views\Home\Index.aspx (sin modificar)**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Es necesario realizar una modificación en la vista de índice. Dado que no vamos a crear una vista de detalles, podemos quitar el vínculo de detalles. Busque y quite el siguiente código de la vista de índice:

{ID = elemento. ID})%&gt;

Después de modificar la vista de índice, puede ejecutar la aplicación Contact Manager. Seleccione la opción de menú Depurar, iniciar depuración o simplemente presione F5. La primera vez que ejecute la aplicación, obtendrá el cuadro de diálogo en la figura 14. Seleccione la opción **modificar el archivo Web. config para habilitar la depuración** y haga clic en el botón Aceptar.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Figura 14**: habilitación de la depuración ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image28.png))

La vista de índice se devuelve de forma predeterminada. Esta vista muestra todos los datos de la tabla de la base de datos Contacts (vea la figura 15).

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Figura 15**: la vista de índice ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image30.png))

Observe que la vista de índice incluye un vínculo con la etiqueta crear nuevo en la parte inferior de la vista. En la sección siguiente, aprenderá a crear nuevos contactos.

## <a name="creating-new-contacts"></a>Crear nuevos contactos

Para permitir que los usuarios creen nuevos contactos, es necesario agregar dos acciones Create () al controlador Home. Necesitamos crear una acción Create () que devuelva un formulario HTML para crear un nuevo contacto. Necesitamos crear una segunda acción Create () que realice la inserción de la base de datos real del nuevo contacto.

Los nuevos métodos Create () que necesitamos agregar al controlador Home están incluidos en la lista 4.

**Lista 4-Controllers\HomeController.cs (con métodos de creación)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

Se puede invocar el primer método Create () con un HTTP GET, mientras que el segundo método Create () solo se puede invocar mediante HTTP POST. En otras palabras, solo se puede invocar el segundo método Create () al publicar un formulario HTML. El primer método Create () simplemente devuelve una vista que contiene el formulario HTML para crear un nuevo contacto. El segundo método Create () es mucho más interesante: agrega el nuevo contacto a la base de datos.

Observe que el segundo método Create () se ha modificado para aceptar una instancia de la clase contact. Los valores de formulario enviados desde el formulario HTML se enlazan automáticamente a esta clase de contacto mediante el marco de MVC de ASP.NET. Cada campo de formulario del formulario de creación HTML se asigna a una propiedad del parámetro contact.

Observe que el parámetro Contact está decorado con un atributo [BIND]. El atributo [BIND] se usa para excluir la propiedad de identificador de contacto del enlace. Dado que la propiedad ID representa una propiedad Identity, no queremos establecer la propiedad ID.

En el cuerpo del método Create (), el Entity Framework se utiliza para insertar el nuevo contacto en la base de datos. El nuevo contacto se agrega al conjunto de contactos existente y se llama al método SaveChanges () para volver a enviar los cambios a la base de datos subyacente.

Puede generar un formulario HTML para crear nuevos contactos haciendo clic con el botón secundario en cualquiera de los dos métodos Create () y seleccionando la opción de menú **Agregar vista** (vea la figura 16).

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Figura 16**: adición de la vista de creación ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image32.png))

En el cuadro de diálogo **Agregar vista** , seleccione la clase **ContactManager. Models. contact** y la opción **crear** para ver contenido (vea la figura 17). Al hacer clic en el botón **Agregar** , se genera automáticamente una vista de creación.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Figura 17**: visualización de una página de explosión ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image34.png))

La vista crear contiene campos de formulario para cada una de las propiedades de la clase contact. El código de la vista Create se incluye en la lista 5.

**Lista 5-Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Después de modificar los métodos Create () y agregar la vista de creación, puede ejecutar la aplicación Contact Manager y crear nuevos contactos. Haga clic en el vínculo **crear nuevo** que aparece en la vista de índice para navegar a la vista crear. Debería ver la vista en la figura 18.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Figura 18**: creación de vista ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image36.png))

## <a name="editing-contacts"></a>Editar contactos

Agregar la funcionalidad para editar un registro de contacto es muy similar a agregar la funcionalidad para crear nuevos registros de contacto. En primer lugar, es necesario agregar dos nuevos métodos de edición a la clase de controlador Home. Estos nuevos métodos Edit () se incluyen en la lista 6.

**Enumerar 6-Controllers\HomeController.cs (con métodos de edición)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

El primer método Edit () se invoca mediante una operación GET de HTTP. Se pasa un parámetro de identificador a este método que representa el identificador del registro de contacto que se está editando. El Entity Framework se utiliza para recuperar un contacto que coincida con el identificador. Se devuelve una vista que contiene un formulario HTML para editar un registro.

El segundo método Edit () realiza la actualización real en la base de datos. Este método acepta una instancia de la clase Contact como parámetro. El marco de MVC de ASP.NET enlaza los campos de formulario del formulario de edición a esta clase automáticamente. Tenga en cuenta que no incluye el atributo [BIND] al editar un contacto (necesitamos el valor de la propiedad ID).

El Entity Framework se utiliza para guardar el contacto modificado en la base de datos. Primero se debe recuperar el contacto original de la base de datos. A continuación, se llama al método Entity Framework ApplyPropertyChanges () para registrar los cambios en el contacto. Por último, se llama al método Entity Framework SaveChanges () para conservar los cambios en la base de datos subyacente.

Para generar la vista que contiene el formulario de edición, haga clic con el botón secundario en el método Edit () y seleccione la opción de menú Agregar vista. En el cuadro de diálogo Agregar vista, seleccione la clase **ContactManager. Models. contact** y el contenido de la vista de **edición** (vea la figura 19).

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Figura 19**: adición de una vista de edición ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image38.png))

Al hacer clic en el botón Agregar, se genera automáticamente una nueva vista de edición. El formulario HTML que se genera contiene campos que corresponden a cada una de las propiedades de la clase Contact (vea la lista 7).

**Listado 7-Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Eliminar contactos

Si desea eliminar contactos, debe agregar dos acciones Delete () a la clase de controlador Home. La primera acción Delete () muestra un formulario de confirmación de eliminación. La segunda acción Delete () realiza la eliminación real.

> [!NOTE] 
> 
> Más adelante, en la iteración #7, se modificará el administrador de contactos para que admita una eliminación de AJAX de un paso.

Los dos nuevos métodos Delete () se incluyen en la lista 8.

**Enumerar 8-Controllers\HomeController.cs (métodos de eliminación)**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

El primer método Delete () devuelve un formulario de confirmación para eliminar un registro de contacto de la base de datos (vea Figure20). El segundo método Delete () realiza la operación de eliminación real en la base de datos. Una vez recuperado el contacto original de la base de datos, se llama a los métodos Entity Framework DeleteObject () y SaveChanges () para realizar la eliminación de la base de datos.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Figura 20**: vista de confirmación de eliminación ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image40.png))

Es necesario modificar la vista de índice para que contenga un vínculo para eliminar los registros de contacto (vea la figura 21). Debe agregar el código siguiente a la misma celda de la tabla que contiene el vínculo de edición:

HTML. ActionLink ({ID = Item. ID})%&gt;

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Figura 21**: vista de índice con el vínculo Editar ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image42.png))

A continuación, es necesario crear la vista de confirmación de eliminación. Haga clic con el botón derecho en el método Delete () de la clase Home Controller y seleccione la opción de menú Agregar vista. Aparecerá el cuadro de diálogo Agregar vista (vea la figura 22).

A diferencia de lo que sucede en el caso de las vistas lista, crear y editar, el cuadro de diálogo Agregar vista no contiene una opción para crear una vista de eliminación. En su lugar, seleccione la clase de datos **ContactManager. Models. contact** y el contenido de la vista **vacía** . Al seleccionar la opción de contenido de vista vacía, nos obligaremos a crear la vista.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Figura 22**: adición de la vista de confirmación de eliminación ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image44.png))

El contenido de la vista de eliminación está incluido en la lista 9. Esta vista contiene un formulario que confirma si se debe eliminar o no un contacto determinado (vea la figura 21).

**Lista 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Cambiar el nombre del controlador predeterminado

Podría molestarle de que el nombre de nuestra clase de controlador para trabajar con contactos se denomina clase HomeController. ¿No se debe asignar el nombre del controlador a ContactController?

Este problema es bastante fácil de corregir. En primer lugar, es necesario refactorizar el nombre del controlador Home. Abra la clase HomeController en el editor de Visual Studio Code, haga clic con el botón secundario en el nombre de la clase y seleccione la opción de menú **refactorizar, cambiar nombre**. Al seleccionar esta opción de menú, se abre el cuadro de diálogo Cambiar nombre.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Figura 23**: refactorización de un nombre de controlador ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image46.png))

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Figura 24**: uso del cuadro de diálogo Cambiar nombre ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image48.png))

Si cambia el nombre de la clase del controlador, Visual Studio también actualizará el nombre de la carpeta en la carpeta views. Visual Studio cambiará el nombre de la carpeta \Views\Home a la carpeta \Views\Contact

Después de realizar este cambio, la aplicación ya no tendrá un controlador de inicio. Al ejecutar la aplicación, aparecerá la página de error en la figura 25.

[![el cuadro de diálogo nuevo proyecto](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Figura 25**: ningún controlador predeterminado ([haga clic para ver la imagen de tamaño completo](iteration-1-create-the-application-cs/_static/image50.png))

Necesitamos actualizar la ruta predeterminada en el archivo global. asax para usar el controlador de contactos en lugar del controlador Home. Abra el archivo global. asax y modifique el controlador predeterminado que usa la ruta predeterminada (vea el listado 10).

**Listado 10: Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Después de realizar estos cambios, el administrador de contactos se ejecutará correctamente. Ahora, usará la clase de controlador de contactos como controlador predeterminado.

## <a name="summary"></a>Resumen

En esta primera iteración, creamos una aplicación básica de Contact Manager de la manera más rápida posible. Aprovechamos Visual Studio para generar automáticamente el código inicial de nuestros controladores y vistas. También se ha aprovechado la Entity Framework para generar automáticamente las clases del modelo de base de datos.

Actualmente, se pueden enumerar, crear, editar y eliminar registros de contacto con la aplicación Contact Manager. En otras palabras, se pueden realizar todas las operaciones de base de datos básicas que requiere una aplicación web controlada por la base de datos.

Desafortunadamente, nuestra aplicación tiene algunos problemas. En primer lugar, y me dude en admitir esto, la aplicación Contact Manager no es la aplicación más atractiva. Necesita cierto trabajo de diseño. En la siguiente iteración, veremos cómo podemos cambiar la página maestra de vista predeterminada y la hoja de estilos en cascada para mejorar la apariencia de la aplicación.

En segundo lugar, no hemos implementado ninguna validación de formulario. Por ejemplo, no hay nada que le impida enviar el formulario crear contacto sin especificar valores para ninguno de los campos del formulario. Además, puede especificar números de teléfono y direcciones de correo electrónico no válidos. Empezamos a abordar el problema de la validación del formulario en la iteración #3.

Por último, y lo más importante, la iteración actual de la aplicación Contact Manager no se puede modificar ni mantener con facilidad. Por ejemplo, la lógica de acceso a la base de datos se incorpora directamente en las acciones del controlador. Esto significa que no podemos modificar nuestro código de acceso a datos sin modificar los controladores. En iteraciones posteriores, se exploran los patrones de diseño de software que se pueden implementar para que el administrador de contactos sea más resistente a los cambios.

> [!div class="step-by-step"]
> [Siguiente](iteration-2-make-the-application-look-nice-cs.md)
