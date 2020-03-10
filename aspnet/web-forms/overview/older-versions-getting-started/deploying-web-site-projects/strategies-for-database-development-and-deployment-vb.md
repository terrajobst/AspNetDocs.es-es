---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Estrategias para el desarrollo y la implementación de bases de datos (VB) | Microsoft Docs
author: rick-anderson
description: Al implementar una aplicación controlada por datos por primera vez, puede copiar ciegamente la base de datos del entorno de desarrollo en el entorno de producción. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ea4ade32fb05b9e69647ece7d1a4c400fe9fb21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439771"
---
# <a name="strategies-for-database-development-and-deployment-vb"></a>Estrategias del desarrollo e implementación de bases de datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Al implementar una aplicación controlada por datos por primera vez, puede copiar ciegamente la base de datos del entorno de desarrollo en el entorno de producción. Sin embargo, realizar una copia oculta en las implementaciones posteriores sobrescribirá todos los datos introducidos en la base de datos de producción. En su lugar, la implementación de una base de datos implica la aplicación de los cambios realizados en la base de datos de desarrollo desde la última implementación en la base de datos de producción. En este tutorial se examinan estos desafíos y se ofrecen varias estrategias para ayudar con la crónica y la aplicación de los cambios realizados en la base de datos desde la última implementación.

## <a name="introduction"></a>Introducción

Como se explicó en los tutoriales anteriores, la implementación de una aplicación de ASP.NET conlleva copiar el contenido pertinente del entorno de desarrollo en el entorno de producción. La implementación no es un evento que se realiza una sola vez, sino que es algo que se produce cada vez que se publica una nueva versión del software o cuando se identifican y solucionan errores o problemas de seguridad. Al copiar páginas ASP.NET, imágenes, archivos JavaScript y otros archivos de este tipo en el entorno de producción, no es necesario preocuparse por la forma en que se han modificado estos archivos desde la última implementación. Puede copiar ciegamente el archivo en producción, sobrescribiendo el contenido existente. Desafortunadamente, esta simplicidad no se extiende a la implementación de la base de datos.

Al implementar una aplicación controlada por datos por primera vez, puede copiar ciegamente la base de datos del entorno de desarrollo en el entorno de producción. Sin embargo, realizar una copia oculta en las implementaciones posteriores sobrescribirá todos los datos introducidos en la base de datos de producción. En su lugar, la implementación de una base de datos implica la aplicación de los *cambios* realizados en la base de datos de desarrollo desde la última implementación en la base de datos de producción. En este tutorial se examinan estos desafíos y se ofrecen varias estrategias para ayudar con la crónica y la aplicación de los cambios realizados en la base de datos desde la última implementación.

## <a name="the-challenges-of-deploying-a-database"></a>Desafíos de la implementación de una base de datos

Antes de que una aplicación controlada por datos se haya implementado por primera vez, solo hay una base de datos, es decir, la base de datos en el entorno de desarrollo, por lo que al implementar una aplicación controlada por datos por primera vez, puede copiar ciegamente la base de datos en el entorno de desarrollo para el entorno de producción. Pero una vez implementada la aplicación, hay dos copias de la base de datos: una en desarrollo y otra en producción.

Entre las implementaciones, las bases de datos de desarrollo y producción pueden dejar de estar sincronizadas. Aunque el esquema de la base de datos de producción permanece inalterado, el esquema de la base de datos de desarrollo puede cambiar a medida que se agregan nuevas características. Puede Agregar o quitar columnas, tablas, vistas o procedimientos almacenados. También puede haber datos importantes que se agreguen a la base de datos de desarrollo. Muchas aplicaciones controladas por datos incluyen tablas de búsqueda rellenadas con datos específicos de la aplicación codificados de forma rígida que no son editables por el usuario. Por ejemplo, un sitio web de subasta puede tener una lista desplegable con opciones que describen la condición del elemento que se va a suponer: nuevo, como nuevo, bueno y equitativo. En lugar de codificar estas opciones de forma rígida directamente en la lista desplegable, suele ser mejor colocarlas en una tabla de base de datos. Si, durante el desarrollo, se agrega a la tabla una nueva condición denominada mala, cuando se implementa la aplicación, es necesario agregar este mismo registro a la tabla de búsqueda de la base de datos de producción.

Idealmente, la implementación de la base de datos implicaría copiar la base de datos de desarrollo a producción. Pero tenga en cuenta que después de haber implementado la aplicación y reanudado el desarrollo, la base de datos de producción se rellena con datos reales de usuarios reales. Por lo tanto, si simplemente desea copiar la base de datos de desarrollo a producción en la siguiente implementación, se sobrescribirá la base de datos de producción y se perderán los datos existentes. El resultado neto es que la implementación de la base de datos se reduce a la aplicación de los cambios realizados en la base de datos de desarrollo desde la última implementación.

Dado que la implementación de una base de datos implica la aplicación de los cambios en el esquema y, posiblemente, los datos desde la última implementación, se debe mantener un historial de cambios (o determinarse en tiempo de implementación) para que esos cambios se puedan aplicar en producción. Existen varias técnicas para administrar y aplicar cambios en el modelo de datos.

### <a name="defining-the-baseline"></a>Definir la línea base

Para mantener los cambios en la base de datos de la aplicación, debe tener un estado de inicio, una línea de base a la que se aplican los cambios. En un extremo, el estado inicial podría ser una base de datos vacía sin tablas, vistas o procedimientos almacenados. Este tipo de línea de base da como resultado un registro de cambios grande porque debe incluir la creación de todas las tablas, vistas y procedimientos almacenados de la base de datos junto con los cambios realizados después de la implementación inicial. En el otro extremo del espectro, podría establecer la línea base como la versión de la base de datos que se implementa inicialmente en el entorno de producción. Esta opción da como resultado un registro de cambios mucho menor, ya que solo incluye los cambios realizados en la base de datos después de la primera implementación. Este es el método preferido. Y, por supuesto, puede elegir un enfoque más intermedio, definiendo la línea de base como algún punto entre la creación inicial de la base de datos y la primera vez que se implemente la base de datos.

Una vez que haya elegido una línea de base, considere la posibilidad de generar un script SQL que se pueda ejecutar para volver a crear la versión de línea de base. Este tipo de script permite volver a crear rápidamente la versión de línea base de la base de datos. Esta funcionalidad es especialmente útil en proyectos más grandes, en los que puede haber varios desarrolladores que trabajan en el proyecto o en entornos adicionales, como pruebas o almacenamiento provisional, y cada uno de ellos necesita su propia copia de la base de datos.

Hay una gran variedad de herramientas a su disposición para generar un script SQL de la versión de línea de base. En SQL Server Management Studio (SSMS) puede hacer clic con el botón derecho en la base de datos, ir al submenú tareas y elegir la opción generar scripts. Esto inicia el Asistente para scripts, que puede indicar a que genere un archivo que contiene los comandos SQL para crear los objetos de base de datos. Otra opción es el Asistente para publicar bases de datos, que puede generar los comandos SQL para no solo crear el esquema de la base de datos, sino también los datos de las tablas de base de datos. El Asistente para la publicación de bases de datos se examinó en detalle en el tutorial *implementación de una base de datos* . Independientemente de la herramienta que use, al final debe tener un archivo de script que pueda usar para volver a crear la versión de línea base de la base de datos, en caso de que sea necesario.

## <a name="documenting-the-database-changes-in-prose"></a>Documentar los cambios de la base de datos en Prose

La manera más sencilla de mantener un registro de cambios en el modelo de datos durante la fase de desarrollo es registrar los cambios en Prose. Por ejemplo, si durante el desarrollo de una aplicación ya implementada agrega una nueva columna a la tabla `Employees`, quita una columna de la tabla `Orders` y agrega una nueva tabla (`ProductCategories`), se mantendrá un archivo de texto o un documento de Microsoft Word con el historial siguiente:

<a id="0.8_table01"></a>

| **Fecha de cambio** | **Cambiar detalles** |
| --- | --- |
| 2009-02-03: | `DepartmentID` de columna agregada (`int`, NOT NULL) a la tabla de `Employees`. Se ha agregado una restricción FOREIGN KEY de `Departments.DepartmentID` a `Employees.DepartmentID`. |
| 2009-02-05: | Se han quitado los `TotalWeight` de columna de la tabla `Orders`. Datos ya capturados en registros de `OrderDetails` asociados. |
| 2009-02-12: | Creó la tabla `ProductCategories`. Hay tres columnas: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`) y `Active` (`bit`, `NOT NULL`). Se ha agregado una restricción PRIMARY KEY a `ProductCategoryID`y un valor predeterminado de 1 para `Active`. |

Este enfoque tiene varias desventajas. En el caso de los iniciadores, no hay ninguna esperanza para la automatización. Cada vez que estos cambios deben aplicarse a una base de datos, por ejemplo, cuando se implementa la aplicación, un desarrollador debe implementar manualmente cada cambio, de uno en uno. Además, si necesita reconstruir una versión determinada de la base de datos desde la línea base mediante el registro de cambios, al hacerlo se tardará más y más tiempo a medida que aumente el tamaño del registro. Otro inconveniente de este método es que la claridad y el nivel de detalle de cada entrada del registro de cambios se deja a la persona que registra el cambio. En un equipo con varios desarrolladores, algunos pueden crear entradas más detalladas, más legibles o más precisas que otras. Además, es posible que se produzcan errores tipográficos y otros errores de entrada de datos relacionados con el usuario.

La principal ventaja de documentar los cambios de la base de datos en Prose es simplicidad. No necesita estar familiarizado con la sintaxis de SQL para crear y modificar objetos de base de datos. En su lugar, puede registrar los cambios en Prose e implementarlos a través de la interfaz gráfica de usuario de SQL Server Management Studio s.

El mantenimiento del registro de cambios en Prose es, es decir, no es muy sofisticado y no funciona bien con ciertos proyectos, como aquellos que tienen un ámbito grande, tienen cambios frecuentes en el modelo de datos o implican a varios desarrolladores. Pero he descubierto que este enfoque funciona bien en proyectos pequeños de un solo hombre que solo tienen cambios ocasionales en el modelo de datos y donde el desarrollador de solo no tiene un fondo seguro en la sintaxis de SQL para crear y modificar objetos de base de datos.

> [!NOTE]
> Aunque la información del registro de cambios es, técnicamente, solo necesaria hasta el momento de la implementación, recomiendo mantener un historial de cambios. Pero en lugar de mantener un único archivo de registro de cambios en constante crecimiento, considere la posibilidad de tener un archivo de registro de cambios diferente para cada versión de la base de datos. Normalmente, querrá crear una versión de la base de datos cada vez que se implemente. Al mantener un registro de registros de cambios, puede empezar desde la línea de base y volver a crear cualquier versión de la base de datos mediante la ejecución de los scripts del registro de cambios a partir de la versión 1 y continuar hasta que llegue a la versión que necesita volver a crear.

## <a name="recording-the-sql-change-statements"></a>Grabar las instrucciones de cambio de SQL

La inconveniente principal de mantener el registro de cambios en Prose es la falta de automatización. Idealmente, implementar los cambios de base de datos en la base de datos de producción en tiempo de implementación sería tan sencillo como hacer clic en un botón para ejecutar un script en lugar de tener que realizar manualmente una lista de instrucciones. Tal automatización es posible manteniendo un registro de cambios que contenga los comandos SQL usados para modificar el modelo de datos.

La sintaxis SQL incluye una serie de instrucciones para crear y modificar diversos objetos de base de datos. Por ejemplo, la [*instrucción CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), cuando se ejecuta, crea una nueva tabla con las columnas y restricciones especificadas. La [*instrucción ALTER TABLE*](https://msdn.microsoft.com/library/ms190273.aspx) modifica una tabla existente, agregando, quitando o modificando sus columnas o restricciones. También hay instrucciones para crear, modificar y quitar índices, vistas, funciones definidas por el usuario, procedimientos almacenados, desencadenadores y otros objetos de base de datos.

Al volver a nuestro ejemplo anterior, imagen que, durante el desarrollo de una aplicación ya implementada, se agrega una nueva columna a la tabla `Employees`, se quita una columna de la tabla `Orders` y se agrega una nueva tabla (`ProductCategories`). Tales acciones darían lugar a un archivo de registro de cambios con los siguientes comandos SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

La inserción de estos cambios en la base de datos de producción en tiempo de implementación es una operación de un solo clic: Abra SQL Server Management Studio, conéctese a la base de datos de producción, abra una nueva ventana de consulta, pegue el contenido del registro de cambios y haga clic en ejecutar para ejecutar el script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Usar una herramienta de comparación para sincronizar los modelos de datos

Documentar los cambios de la base de datos en Prose es fácil, pero la implementación de los cambios requiere que un desarrollador realice cada cambio en la base de datos de producción de uno en uno; la documentación de los comandos SQL de cambio hace que la implementación de esos cambios en la base de datos de producción sea tan sencilla y rápida como hacer clic en un botón, pero requiere aprender y dominar las instrucciones SQL y la sintaxis para crear y modificar objetos de base de datos. Las herramientas de comparación de bases de datos sacan el máximo partido de ambos enfoques y descartan el peor.

Una herramienta de comparación de bases de datos compara el esquema o los datos de dos bases de datos y muestra un informe de resumen que muestra cómo difieren las bases de datos. A continuación, con solo hacer clic en un botón, puede generar los comandos SQL para sincronizar uno o varios objetos de base de datos. En pocas palabras, puede usar una herramienta de comparación de bases de datos para comparar las bases de datos de desarrollo y de producción en tiempo de implementación, generando un archivo que contiene los comandos SQL que, cuando se ejecutan, aplicarán los cambios en el esquema s de la base de datos de producción para que refleja el esquema s de la base de datos de desarrollo.

Hay varias herramientas de comparación de bases de datos de terceros que ofrecen muchos proveedores diferentes. Un ejemplo de este tipo es [*comparación de SQL*](http://www.red-gate.com/products/SQL_Compare/), mediante el [*software de puerta roja*](http://www.red-gate.com/). Vamos a recorrer el proceso de uso de SQL Compare para comparar y sincronizar los esquemas de las bases de datos de desarrollo y producción.

> [!NOTE]
> En el momento de redactar este documento, la versión actual de SQL Compare era 7,1, con la edición Standard con el costo de $395. Puede seguir la descarga de una versión de prueba gratuita de 14 días.

Cuando se inicia la comparación de SQL, se abre el cuadro de diálogo proyectos de comparación, que muestra los proyectos de comparación de SQL guardados. Cree un nuevo proyecto. Esto inicia el Asistente para configuración de proyectos, que solicita información sobre las bases de datos que se van a comparar (consulte la figura 1). Escriba la información de las bases de datos del entorno de desarrollo y de producción.

[![comparar las bases de datos de desarrollo y producción](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Figura 1**: comparación de las bases de datos de desarrollo y de producción ([haga clic para ver la imagen de tamaño completo](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))

> [!NOTE]
> Si la base de datos del entorno de desarrollo es un archivo de base de datos de SQL Express Edition en la carpeta `App_Data` del sitio web, deberá registrar la base de datos en el servidor de base de datos de SQL Server Express para poder seleccionarla en el cuadro de diálogo que aparece en la figura 1. La forma más fácil de hacerlo es abrir SQL Server Management Studio (SSMS), conectarse al servidor de base de datos de SQL Server Express y adjuntar la base de datos. Si no tiene instalado SSMS en el equipo, puede descargar e instalar la [*versión gratuita SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).

Además de seleccionar las bases de datos que se van a comparar, también puede especificar diversos valores de comparación en la pestaña Opciones. Una opción que puede querer activar es "omitir restricciones y nombres de índice". Recuerde que en el tutorial anterior hemos agregado los objetos de base de datos de servicios de aplicación a las bases de datos de desarrollo y producción. Si ha usado la herramienta de `aspnet_regsql.exe` para crear estos objetos en la base de datos de producción, verá que los nombres de las restricciones PRIMARY KEY y Unique se diferencian entre las bases de datos de desarrollo y de producción. Por lo tanto, SQL Compare marcará todas las tablas de servicios de aplicación como diferentes. Puede dejar desactivadas las opciones "omitir restricciones y nombres de índice" y sincronizar los nombres de restricción, o indicar a SQL Compare que omita estas diferencias.

Después de seleccionar las bases de datos que se van a comparar (y revisar las opciones de comparación), haga clic en el botón Comparar ahora para comenzar la comparación. En los próximos segundos, SQL Compare examina los esquemas de las dos bases de datos y genera un informe de cómo se diferencian. He realizado intencionadamente algunas modificaciones en la base de datos de desarrollo para mostrar cómo se indican esas discrepancias en la interfaz de comparación de SQL. Como se muestra en la figura 2, se ha agregado una columna de `BirthDate` a la tabla de `Authors`, se ha quitado la columna de `ISBN` de la tabla de `Books` y se ha agregado una nueva tabla, `Ratings`, que permite a los usuarios visitar el sitio los libros revisados.

> [!NOTE]
> Los cambios en el modelo de datos realizados en este tutorial se han realizado para ilustrar el uso de una herramienta de comparación de bases de datos. Estos cambios no se encuentran en la base de datos en futuros tutoriales.

[![comparación de SQL enumera las diferencias entre las bases de datos de desarrollo y de producción](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Figura 2**: comparación de SQL enumera las diferencias entre las bases de datos de desarrollo y de producción ([haga clic para ver la imagen de tamaño completo](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))

Comparación de SQL divide los objetos de base de datos en grupos y muestra rápidamente los objetos que existen en ambas bases de datos, pero son diferentes, qué objetos existen en una base de datos pero no en la otra, y qué objetos son idénticos. Como puede ver, hay dos objetos que existen en ambas bases de datos, pero son diferentes: el `Authors` tabla, que tenía una columna agregada y la tabla `Books`, que tenía una quitada. Hay un objeto que solo existe en la base de datos de desarrollo, es decir, la tabla de `Ratings` recién creada. Y hay 117 objetos que son idénticos en ambas bases de datos.

Al seleccionar un objeto de base de datos se muestra la ventana diferencias de SQL, que muestra cómo difieren estos objetos. La ventana diferencias de SQL, que se muestra en la parte inferior de la figura 2, resalta que la tabla de `Authors` de la base de datos de desarrollo tiene la columna `BirthDate`, que no se encuentra en la tabla `Authors` en la base de datos de producción.

Después de revisar las diferencias y seleccionar los objetos que desea sincronizar, el paso siguiente consiste en generar los comandos SQL necesarios para actualizar el esquema de la base de datos de producción para que coincida con la base de datos de desarrollo. Esto se consigue a través del Asistente para sincronización. El Asistente para sincronización confirma qué objetos se van a sincronizar y resume el plan de acción (vea la figura 3). Puede sincronizar las bases de datos inmediatamente o generar un script con los comandos SQL que se pueden ejecutar en el momento de su ocio.

[![usar el Asistente para sincronización para sincronizar los esquemas de las bases de datos](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Figura 3**: usar el Asistente para sincronización para sincronizar los esquemas de las bases de datos ([haga clic para ver la imagen de tamaño completo](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))

Herramientas de comparación de bases de datos como red Gate software s SQL Compare hacen que la aplicación de los cambios en el esquema de la base de datos de desarrollo a la base de datos de producción sea tan fácil como hacer clic.

> [!NOTE]
> Comparación de SQL compara y sincroniza dos *esquemas*de bases de datos. Desafortunadamente, no se comparan y sincronizan los datos de las dos tablas de bases de datos. Red Gate software ofrece un producto denominado [*SQL Data Compare*](http://www.red-gate.com/products/SQL_Data_Compare/) que compara y sincroniza los datos entre dos bases de datos, pero es un producto independiente de comparación de SQL y cuesta otro $395.

## <a name="taking-the-application-offline-during-deployment"></a>Desconectar la aplicación durante la implementación

Como se ve en estos tutoriales, la implementación es un proceso que implica varios pasos: copiar páginas de ASP.NET, páginas maestras, archivos CSS, archivos JavaScript, imágenes y otro contenido necesario del entorno de desarrollo al de producción. entorno copia de la información de configuración específica del entorno de producción, si es necesario. y aplicar los cambios al modelo de datos desde la última implementación. En función del número de archivos y de la complejidad de los cambios de la base de datos, estos pasos pueden tardar desde unos segundos hasta varios minutos en completarse. Durante este período, la aplicación web está en flujo y los usuarios que visitan el sitio pueden experimentar errores o un comportamiento inesperado.

Al implementar un sitio web, es mejor poner la aplicación web "sin conexión" hasta que se haya completado la implementación. Poner la aplicación sin conexión (y realizar una copia de seguridad una vez finalizado el proceso de implementación) es tan fácil como cargar un archivo y, a continuación, eliminarlo. A partir de ASP.NET 2,0, la mera presencia de un archivo denominado `app_offline.htm` en el directorio raíz de la aplicación hace que todo el sitio web esté "sin conexión". Cualquier solicitud a una página de ASP.NET en ese sitio se responde automáticamente con el contenido del archivo de `app_offline.htm`. Una vez que se quita el archivo, la aplicación vuelve a estar en línea.

Dejar sin conexión una aplicación durante la implementación, es tan sencillo como cargar un archivo `app_offline.htm` en el directorio raíz del entorno de producción antes de comenzar el proceso de implementación y, a continuación, eliminarlo (o cambiarle el nombre a otra cosa) una vez completada la implementación. Para obtener más información sobre esta técnica, consulte el artículo de John Peterson s, desconectar una [*aplicación de ASP.net*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Resumen

El principal desafío en la implementación de una aplicación controlada por datos se centra en la implementación de la base de datos. Dado que hay dos versiones de la base de datos: una en el entorno de desarrollo y otra en el entorno de producción, estos dos esquemas de bases de datos pueden dejar de estar sincronizados a medida que se agregan nuevas características en el desarrollo. Lo que es más, dado que la base de datos de producción se rellena con datos reales de usuarios reales, no se puede sobrescribir la base de datos de producción con la base de datos de desarrollo modificada como se puede hacer al implementar los archivos que componen la aplicación (las páginas de ASP.NET, archivos de imagen, etc.). En su lugar, la implementación de una base de datos implica implementar el conjunto preciso de cambios realizados en la base de datos de desarrollo en la base de datos de producción desde la última implementación.

En este tutorial se han examinado tres técnicas para mantener y aplicar un registro de cambios en la base de datos. El enfoque más sencillo consiste en registrar los cambios en Prose. Aunque esta táctica hace que la implementación de estos cambios en la base de datos de producción sea un proceso manual, no es necesario conocer los comandos SQL para crear y modificar objetos de base de datos. Un enfoque más sofisticado y otro que es mucho más palatable en proyectos o proyectos más grandes con varios desarrolladores es registrar los cambios como una serie de comandos SQL. Esto aumenta considerablemente la implementación de estos cambios en la base de datos de destino. Lo mejor de ambos enfoques se puede lograr mediante una herramienta de comparación de bases de datos, como red Gate software s SQL Compare.

Este tutorial concluye nuestro interés en la implementación de una aplicación controlada por datos. El siguiente conjunto de tutoriales examina cómo responder a errores en el entorno de producción. Veremos cómo mostrar una página de error amistosa en lugar de la pantalla amarilla de muerte. Y veremos cómo registrar los detalles del error y cómo avisarle cuando se produzcan estos errores.

¡ Feliz programación!

> [!div class="step-by-step"]
> [Anterior](configuring-a-website-that-uses-application-services-vb.md)
> [Siguiente](displaying-a-custom-error-page-vb.md)
