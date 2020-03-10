---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: Generar un modelo con validaciones de reglas de negocios | Microsoft Docs
author: microsoft
description: En el paso 3 se muestra cómo crear un modelo que se puede usar para consultar y actualizar la base de datos para nuestra aplicación NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 6ebf1b71c089229ba9139ff7dc788b8978724046
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435583"
---
# <a name="build-a-model-with-business-rule-validations"></a>Crear un modelo con validaciones de regla de negocios

por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 3 de un [tutorial de aplicación "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito en el que se explica cómo crear una aplicación web pequeña, pero completa, con ASP.NET MVC 1.
> 
> En el paso 3 se muestra cómo crear un modelo que se puede usar para consultar y actualizar la base de datos para nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda seguir los tutoriales [de la introducción con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o MVC [Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner paso 3: compilar el modelo

En un marco modelo-vista-controlador, el término "modelo" hace referencia a los objetos que representan los datos de la aplicación, así como a la lógica de dominio correspondiente que integra la validación y las reglas de negocios. El modelo es de muchas maneras el "corazón" de una aplicación basada en MVC y, como veremos más adelante de forma fundamental el comportamiento del mismo.

El marco de MVC de ASP.NET admite el uso de cualquier tecnología de acceso a datos y los desarrolladores pueden elegir entre una variedad de opciones de datos de .NET enriquecidas para implementar sus modelos: LINQ to Entities, LINQ to SQL, NHibernate, LLBLGen Pro, Subsonic, WilsonORM o simplemente ADO sin formato. NET DataReader o datasets.

Para nuestra aplicación NerdDinner, vamos a usar LINQ to SQL para crear un modelo simple que se corresponda bastante cerca con el diseño de la base de datos y que agregue algunas reglas de negocios y lógica de validación personalizada. A continuación, implementaremos una clase de repositorio que ayuda a abstraer la implementación de la persistencia de datos del resto de la aplicación y nos permite realizar pruebas de forma sencilla.

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL es un ORM (asignador relacional de objetos) que se distribuye como parte de .NET 3,5.

LINQ to SQL proporciona una manera sencilla de asignar tablas de base de datos a clases de .NET con las que se puede codificar. Para nuestra aplicación NerdDinner, la usaremos para asignar las cenas y las tablas de RSVP de nuestra base de datos a las clases cena y RSVP. Las columnas de las tablas cenas y RSVP se corresponden con las propiedades de las clases cena y RSVP. Cada cena y objeto RSVP representará una fila independiente dentro de las cenas o las tablas RSVP en la base de datos.

LINQ to SQL nos permite evitar tener que crear manualmente instrucciones SQL para recuperar y actualizar objetos de cena y RSVP con datos de base de datos. En su lugar, definiremos las clases cena y RSVP, cómo se asignan a la base de datos, y las relaciones entre ellas. A continuación, LINQ to SQL se encargará de generar la lógica de ejecución de SQL adecuada que se usará en tiempo de ejecución cuando se interactúe y se usen.

Podemos usar la compatibilidad con el lenguaje LINQ en VB C# y escribir consultas expresivas que recuperen objetos de cena y RSVP de la base de datos. Esto minimiza la cantidad de código de datos que es necesario escribir y nos permite crear aplicaciones realmente limpias.

### <a name="adding-linq-to-sql-classes-to-our-project"></a>Agregar clases de LINQ to SQL a nuestro proyecto

Comenzaremos haciendo clic con el botón derecho en la carpeta "models" dentro de nuestro proyecto y seleccionamos el comando de menú **agregar&gt;nuevo elemento** :

![](build-a-model-with-business-rule-validations/_static/image1.png)

Se abrirá el cuadro de diálogo "Agregar nuevo elemento". Filtraremos por la categoría de "datos" y seleccionaremos la plantilla "LINQ to SQL classes" en ella:

![](build-a-model-with-business-rule-validations/_static/image2.png)

Asignaremos el nombre "NerdDinner" al elemento y hacemos clic en el botón "agregar". Visual Studio agregará un archivo NerdDinner. dbml en nuestro directorio \Models y, a continuación, abrirá el LINQ to SQL Object Relational Designer:

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>Crear clases de modelo de datos con LINQ to SQL

LINQ to SQL nos permite crear rápidamente clases de modelo de datos a partir de un esquema de base de datos existente. Para ello, se abrirá la base de datos NerdDinner en el Explorador de servidores y se seleccionarán las tablas en las que se desea modelar:

![](build-a-model-with-business-rule-validations/_static/image4.png)

A continuación, podemos arrastrar las tablas a la superficie del diseñador de LINQ to SQL. Cuando lo hagamos LINQ to SQL creará automáticamente clases de cenas y RSVP mediante el esquema de las tablas (con propiedades de clase que se asignan a las columnas de la tabla de base de datos):

![](build-a-model-with-business-rule-validations/_static/image5.png)

De forma predeterminada, el diseñador de LINQ to SQL "singular" automáticamente los nombres de tabla y columna cuando crea clases basadas en un esquema de base de datos. Por ejemplo: la tabla "cenas" del ejemplo anterior dio como resultado una clase "cena". Este nombre de clase ayuda a lograr que nuestros modelos sean coherentes con las convenciones de nomenclatura de .NET y, por lo general, esto se puede hacer que el diseñador lo solucione mejor (especialmente al agregar muchas tablas). Sin embargo, si no le gusta el nombre de una clase o propiedad generada por el diseñador, siempre puede invalidarlo y cambiarlo a cualquier nombre que desee. Puede hacerlo editando el nombre de la entidad o propiedad en línea dentro del diseñador o editándolo a través de la cuadrícula de propiedades.

De forma predeterminada, el diseñador de LINQ to SQL también inspecciona las relaciones de clave principal y clave externa de las tablas y, en función de ellas, crea automáticamente "asociaciones de relaciones" predeterminadas entre las distintas clases de modelo que crea. Por ejemplo, al arrastrar las tablas cenas y RSVP al diseñador LINQ to SQL, se deduce una asociación de relación uno a varios entre ambos en función del hecho de que la tabla RSVP tuviera una clave externa en la tabla de cenas (esto se indica mediante la flecha en la diseñador):

![](build-a-model-with-business-rule-validations/_static/image6.png)

La asociación anterior hará que LINQ to SQL agregue una propiedad "cena" fuertemente tipada a la clase RSVP que los desarrolladores pueden usar para tener acceso a la cena asociada a un RSVP determinado. También hará que la clase cena tenga una propiedad de colección "RSVP" que permita a los desarrolladores recuperar y actualizar objetos RSVP asociados a una cena determinada.

A continuación puede ver un ejemplo de IntelliSense en Visual Studio cuando creamos un nuevo objeto RSVP y lo agrego a una colección RSVP de cena. Observe cómo LINQ to SQL agregado automáticamente una colección de "RSVP" en el objeto cena:

![](build-a-model-with-business-rule-validations/_static/image7.png)

Al agregar el objeto RSVP a la colección RSVPs de la cena, estamos indicando LINQ to SQL para asociar una relación de clave externa entre la cena y la fila RSVP de nuestra base de datos:

![](build-a-model-with-business-rule-validations/_static/image8.png)

Si no le gusta cómo el diseñador ha modelado o denominado una asociación de tabla, puede invalidarlo. Basta con hacer clic en la flecha de asociación en el diseñador y obtener acceso a sus propiedades a través de la cuadrícula de propiedades para cambiarle el nombre, eliminarla o modificarla. En el caso de la aplicación NerdDinner, las reglas de asociación predeterminadas funcionan bien para las clases de modelo de datos que estamos creando y solo podemos usar el comportamiento predeterminado.

### <a name="nerddinnerdatacontext-class"></a>Clase NerdDinnerDataContext

Visual Studio creará automáticamente clases de .NET que representan los modelos y las relaciones de base de datos definidas mediante el diseñador de LINQ to SQL. También se genera una clase DataContext LINQ to SQL para cada archivo de diseñador de LINQ to SQL agregado a la solución. Como hemos denominado nuestro LINQ to SQL elemento de clase "NerdDinner", la clase DataContext creada se denominará "NerdDinnerDataContext". Esta clase NerdDinnerDataContext es el método principal para interactuar con la base de datos.

Nuestra clase NerdDinnerDataContext expone dos propiedades: "cenas" y "RSVP", que representan las dos tablas que modelamos en la base de datos. Podemos usar C# para escribir consultas LINQ en esas propiedades para consultar y recuperar objetos de cena y RSVP desde la base de datos.

En el código siguiente se muestra cómo crear una instancia de un objeto NerdDinnerDataContext y realizar una consulta LINQ en él para obtener una secuencia de cenas que se producen en el futuro. Visual Studio proporciona IntelliSense completo al escribir la consulta LINQ, y los objetos devueltos de él tienen un tipo seguro y también admiten IntelliSense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

Además de permitirnos consultar los objetos de cenas y RSVP, un NerdDinnerDataContext también realiza un seguimiento automático de los cambios que se realizan posteriormente en los objetos de la cena y RSVP que se recuperan a través de él. Podemos usar esta funcionalidad para guardar fácilmente los cambios de nuevo en la base de datos, sin tener que escribir ningún código de actualización de SQL explícito.

Por ejemplo, el código siguiente muestra cómo usar una consulta LINQ para recuperar un único objeto cena de la base de datos, actualizar dos de las propiedades cena y, a continuación, guardar los cambios de nuevo en la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

El objeto NerdDinnerDataContext del código anterior realizó un seguimiento automático de los cambios de propiedad realizados en el objeto cena recuperado de él. Cuando llamamos al método "SubmitChanges ()", se ejecutará una instrucción SQL "UPDATE" adecuada en la base de datos para volver a guardar los valores actualizados.

### <a name="creating-a-dinnerrepository-class"></a>Crear una clase DinnerRepository

En el caso de las aplicaciones pequeñas, a veces es adecuado que los controladores funcionen directamente en una LINQ to SQL clase DataContext e inserten consultas LINQ en los controladores. A pesar de que las aplicaciones son más grandes, este enfoque resulta complicado de mantener y probar. También puede llevarnos a duplicar las mismas consultas LINQ en varios lugares.

Un enfoque que puede hacer que las aplicaciones sean más fáciles de mantener y probar es usar un patrón de "repositorio". Una clase de repositorio ayuda a encapsular la lógica de consulta y persistencia de datos y abstrae los detalles de implementación de la persistencia de datos de la aplicación. Además de simplificar el código de la aplicación, el uso de un patrón de repositorio puede facilitar la modificación de las implementaciones de almacenamiento de datos en el futuro, y puede ayudar a facilitar las pruebas unitarias de una aplicación sin requerir una base de datos real.

Para nuestra aplicación NerdDinner, definiremos una clase DinnerRepository con la siguiente firma:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*Nota: más adelante en este capítulo, se extraerá una interfaz IDinnerRepository de esta clase y se habilitará la inserción de dependencias con ella en nuestros controladores. Sin embargo, para empezar, vamos a empezar de forma sencilla y solo trabajamos directamente con la clase DinnerRepository.*

Para implementar esta clase, haremos clic con el botón derecho en la carpeta "models" y elija el comando de menú **&gt;agregar nuevo elemento** . En el cuadro de diálogo "Agregar nuevo elemento", seleccionaremos la plantilla "clase" y denominaremos el archivo "DinnerRepository.cs":

![](build-a-model-with-business-rule-validations/_static/image10.png)

A continuación, podemos implementar nuestra clase DinnerRepository con el código siguiente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>Recuperar, actualizar, insertar y eliminar mediante la clase DinnerRepository

Ahora que hemos creado nuestra clase DinnerRepository, echemos un vistazo a algunos ejemplos de código que muestran las tareas comunes que podemos hacer con él:

#### <a name="querying-examples"></a>Ejemplos de consultas

El código siguiente recupera una cena única con el valor DinnerID:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

En el código siguiente se recuperan todos los próximos cenas y se recorren en bucle:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>Ejemplos de INSERT y Update

En el código siguiente se muestra cómo agregar dos nuevas cenas. Las adiciones y modificaciones del repositorio no se confirman en la base de datos hasta que se llama al método "Save ()" en ella. LINQ to SQL ajusta automáticamente todos los cambios en una transacción de base de datos, de modo que se produzcan todos los cambios o ninguno de ellos cuando se guarde el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

El código siguiente recupera un objeto cena existente y modifica dos propiedades. Los cambios se confirman de nuevo en la base de datos cuando se llama al método "Save ()" en nuestro repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

El código siguiente recupera una cena y luego le agrega un RSVP. Para ello, usa la colección RSVPs en el objeto cena que LINQ to SQL creado para nosotros (porque hay una relación de clave principal y clave externa entre los dos en la base de datos). Este cambio se conserva en la base de datos como una nueva fila de la tabla RSVP cuando se llama al método "Save ()" en el repositorio:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Ejemplo de DELETE

El código siguiente recupera un objeto cena existente y, a continuación, lo marca para que se elimine. Cuando se llama al método "Save ()" en el repositorio, se confirmará de nuevo la eliminación en la base de datos:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>Integración de la validación y la lógica de reglas de negocios con clases de modelo

La integración de la validación y la lógica de reglas de negocios es una parte fundamental de cualquier aplicación que funcione con datos.

#### <a name="schema-validation"></a>Validación de esquemas

Cuando se definen clases de modelo mediante el diseñador de LINQ to SQL, los tipos de datos de las propiedades de las clases del modelo de datos se corresponden con los tipos de datos de la tabla de base de datos. Por ejemplo: Si la columna "EventDate" de la tabla cenas es una "fecha y hora", la clase del modelo de datos creada por LINQ to SQL será de tipo "DateTime" (que es un tipo de datos .NET integrado). Esto significa que obtendrá errores de compilación si intenta asignar un entero o booleano a él desde el código, y generará un error automáticamente si intenta convertir implícitamente un tipo de cadena no válido en el tiempo de ejecución.

LINQ to SQL también controlará automáticamente los valores de SQL de escape cuando se utilicen cadenas, lo que le ayuda a protegerse frente a ataques de inyección de SQL cuando se usa.

#### <a name="validation-and-business-rule-logic"></a>Lógica de validación y regla de negocios

La validación de esquemas es útil como primer paso, pero rara vez es suficiente. La mayoría de los escenarios del mundo real requieren la capacidad de especificar una lógica de validación más enriquecida que puede abarcar varias propiedades, ejecutar código y, a menudo, tener conocimiento del estado de un modelo (por ejemplo: se crea/Updated/Deleted o dentro de un estado específico del dominio, como "archivado"). Hay una variedad de diferentes patrones y marcos que se pueden usar para definir y aplicar reglas de validación a las clases de modelo, y hay varios marcos de trabajo basados en .NET que se pueden usar para ayudarle con esto. Puede usar prácticamente cualquier parte de ellos en aplicaciones ASP.NET MVC.

Para los fines de nuestra aplicación NerdDinner, usaremos un patrón relativamente sencillo y directo en el que exponemos una propiedad IsValid y un método GetRuleViolations () en nuestro objeto de modelo cena. La propiedad IsValid devolverá True o false, en función de si las reglas de validación y de negocios son válidas. El método GetRuleViolations () devolverá una lista de cualquier error de regla.

Implementaremos IsValid y GetRuleViolations () para nuestro modelo de cena agregando una "clase parcial" a nuestro proyecto. Las clases parciales se pueden usar para agregar métodos, propiedades o eventos a las clases mantenidas por un diseñador de VS (como la clase cena generada por el diseñador de LINQ to SQL) y ayudar a evitar que la herramienta entre en el código. Para agregar una nueva clase parcial a nuestro proyecto, haga clic con el botón derecho en la carpeta \Models y, a continuación, seleccione el comando de menú "Agregar nuevo elemento". A continuación, podemos elegir la plantilla "clase" en el cuadro de diálogo "Agregar nuevo elemento" y asignarle el nombre Dinner.cs.

![](build-a-model-with-business-rule-validations/_static/image11.png)

Al hacer clic en el botón "agregar", se agregará un archivo Dinner.cs a nuestro proyecto y se abrirá en el IDE. A continuación, podemos implementar un marco básico de cumplimiento de reglas/validación mediante el código siguiente:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

Algunas notas sobre el código anterior:

- La clase cena está precedida por una palabra clave "Partial", lo que significa que el código contenido en él se combinará con la clase generada o mantenida por el diseñador de LINQ to SQL y compilada en una sola clase.
- La clase RuleViolation es una clase auxiliar que agregaremos al proyecto que nos permite proporcionar más detalles sobre una infracción de la regla.
- El método cena. GetRuleViolations () hace que se evalúen nuestras reglas de validación y negocio (las implementaremos en breve). A continuación, devuelve una secuencia de objetos RuleViolation que proporcionan más detalles sobre los errores de regla.
- La propiedad cena. IsValid proporciona una adecuada propiedad auxiliar que indica si el objeto cena tiene cualquier RuleViolations activo. Un desarrollador puede comprobarlo proactivamente mediante el objeto cena en cualquier momento (y no genera una excepción).
- El método parcial cena. OnValidate () es un enlace que LINQ to SQL proporciona y nos permite recibir notificaciones en cualquier momento en que el objeto cena esté a punto de persistir en la base de datos. La implementación de OnValidate () anterior garantiza que la cena no tiene ningún RuleViolations antes de guardarse. Si se encuentra en un estado no válido, genera una excepción, lo que hará que LINQ to SQL anule la transacción.

Este enfoque proporciona un marco de trabajo sencillo en el que se pueden integrar reglas de validación y negocios. Por ahora, vamos a agregar las siguientes reglas al método GetRuleViolations ():

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

Estamos usando la característica "yield return" de C# para devolver una secuencia de cualquier RuleViolations. Las seis primeras comprobaciones de reglas anteriores simplemente exigen que las propiedades de cadena de nuestra cena no puedan ser nulas ni estar vacías. La última regla es un poco más interesante y llama a un método auxiliar PhoneValidator. IsValidNumber () que se puede Agregar a nuestro proyecto para comprobar que el formato de número ContactPhone coincide con el país de la cena.

Podemos usar. Compatibilidad de la expresión regular de la red para implementar esta compatibilidad de validación de teléfono. A continuación se muestra una implementación de PhoneValidator sencilla que se puede Agregar a nuestro proyecto que nos permite agregar comprobaciones de patrón de Regex específicas del país:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>Control de infracciones de validación y lógica de negocios

Ahora que hemos agregado la validación anterior y el código de la regla de negocios, cada vez que se intenta crear o actualizar una cena, se evaluarán y aplicarán nuestras reglas lógicas de validación.

Los desarrolladores pueden escribir código como el siguiente para determinar de forma proactiva Si un objeto cena es válido y recuperar una lista de todas las infracciones en él sin generar ninguna excepción:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

Si se intenta guardar una cena en un estado no válido, se generará una excepción cuando se llame al método Save () en DinnerRepository. Esto se produce porque LINQ to SQL llama automáticamente al método parcial cena. OnValidate () antes de guardar los cambios de la cena, y hemos agregado el código a cena. OnValidate () para generar una excepción si hay alguna infracción de regla en la cena. Podemos detectar esta excepción y recuperar de forma reactiva una lista de las infracciones para corregir:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

Dado que nuestras reglas de validación y de negocios se implementan dentro de nuestra capa del modelo y no dentro de la capa de la interfaz de usuario, se aplicarán y se usarán en todos los escenarios de nuestra aplicación. Posteriormente, se pueden cambiar o agregar reglas de negocios y hacer que todo el código que funciona con nuestros objetos de cena las respete.

Tener la flexibilidad de cambiar las reglas de negocios en un solo lugar, sin que estos cambios se llenen a lo largo de la aplicación y la lógica de la interfaz de usuario, es un signo de una aplicación bien escrita y una ventaja que el marco de MVC le ayuda a fomentar.

### <a name="next-step"></a>Paso siguiente

Ahora tenemos un modelo que se puede usar para consultar y actualizar nuestra base de datos.

Ahora vamos a agregar algunos controladores y vistas al proyecto que se pueden usar para crear una experiencia de interfaz de usuario HTML en torno a él.

> [!div class="step-by-step"]
> [Anterior](create-a-database.md)
> [Siguiente](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
