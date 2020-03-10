---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Crear la capa de acceso a datos | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para nosotros...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: 0fcf050474a57be9ed53ec0783a6d6b7dde2bf4c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78438379"
---
# <a name="create-the-data-access-layer"></a>Crear la capa de acceso a datos

por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo deC#Wingtip Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar el libro electrónico (pdf)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4,5 y Microsoft Visual Studio Express 2013 para Web. Hay disponible un [proyecto C# de Visual Studio 2013 con código fuente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) para acompañar esta serie de tutoriales.

En este tutorial se describe cómo crear, obtener acceso y revisar datos de una base de datos de mediante los formularios Web Forms de ASP.NET y Entity Framework Code First. Este tutorial se basa en el tutorial anterior "creación del proyecto" y forma parte de la serie de tutoriales de Wingtip Toy Store. Cuando haya completado este tutorial, habrá creado un grupo de clases de acceso a datos que se encuentran en la carpeta *modelos* del proyecto.

## <a name="what-youll-learn"></a>Temas que se abordarán:

- Cómo crear los modelos de datos.
- Cómo inicializar y propagar la base de datos.
- Cómo actualizar y configurar la aplicación para admitir la base de datos.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estas son las características introducidas en el tutorial:

- Entity Framework Code First
- LocalDB
- Anotaciones de datos

## <a name="creating-the-data-models"></a>Crear los modelos de datos

[Entity Framework](https://msdn.microsoft.com/data/aa937723) es un marco de trabajo de asignación relacional de objetos (ORM). Le permite trabajar con datos relacionales como objetos, lo que elimina la mayor parte del código de acceso a datos que normalmente necesita escribir. Con Entity Framework, puede emitir consultas mediante [LINQ](https://msdn.microsoft.com/library/bb397926.aspx)y, a continuación, recuperar y manipular los datos como objetos fuertemente tipados. LINQ proporciona patrones para consultar y actualizar los datos. El uso de Entity Framework permite centrarse en la creación del resto de la aplicación, en lugar de centrarse en los aspectos básicos del acceso a datos. Más adelante en esta serie de tutoriales, le mostraremos cómo usar los datos para rellenar las consultas de navegación y de producto.

Entity Framework admite un paradigma de desarrollo denominado *code First*. Code First permite definir modelos de datos mediante clases. Una clase es una construcción que permite crear sus propios tipos personalizados mediante la agrupación de variables de otros tipos, métodos y eventos. Puede asignar clases a una base de datos existente o usarlas para generar una base de datos. En este tutorial, creará los modelos de datos escribiendo clases de modelo de datos. A continuación, permitirá que Entity Framework crear la base de datos sobre la marcha desde estas nuevas clases.

Comenzará creando las clases de entidad que definen los modelos de datos de la aplicación de formularios Web Forms. A continuación, creará una clase de contexto que administra las clases de entidad y proporciona acceso de datos a la base de datos. También creará una clase de inicializador que usará para rellenar la base de datos.

### <a name="entity-framework-and-references"></a>Entity Framework y referencias

De forma predeterminada, Entity Framework se incluye al crear una nueva **aplicación Web de ASP.net** mediante la plantilla de **formularios Web Forms** . Entity Framework se puede instalar, desinstalar y actualizar como un paquete NuGet.

Este paquete de NuGet incluye los siguientes ensamblados en **tiempo de ejecución** dentro del proyecto:

- EntityFramework. dll: todo el código en tiempo de ejecución común usado por Entity Framework
- EntityFramework. SqlServer. dll: el proveedor de Microsoft SQL Server para Entity Framework

### <a name="entity-classes"></a>Clases de entidad

Las clases que se crean para definir el esquema de los datos se denominan clases de entidad. Si no está familiarizado con el diseño de bases de datos, piense en las clases de entidad como definiciones de tabla de una base de datos. Cada propiedad de la clase especifica una columna en la tabla de la base de datos. Estas clases proporcionan una interfaz ligera y relacional de objetos entre el código orientado a objetos y la estructura de la tabla relacional de la base de datos.

En este tutorial, empezará por agregar clases de entidad simples que representan los esquemas de productos y categorías. La clase Products contendrá definiciones para cada producto. El nombre de cada uno de los miembros de la clase Product será `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`y `Category`. La clase de categoría contendrá definiciones para cada categoría a la que puede pertenecer un producto, como automóvil, barco o plano. El nombre de cada uno de los miembros de la clase Category será `CategoryID`, `CategoryName`, `Description`y `Products`. Cada producto pertenecerá a una de las categorías. Estas clases de entidad se agregarán a la carpeta *modelos* existentes del proyecto.

1. En **Explorador de soluciones**, haga clic con el botón secundario en la carpeta *modelos* y seleccione **Agregar** -&gt; **nuevo elemento**. 

    ![Crear el menú capa de acceso a datos-nuevo elemento](create_the_data_access_layer/_static/image1.png)

   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En **Visual C#**  , en el panel **instalado** de la izquierda, seleccione **código**. 

    ![Crear el menú capa de acceso a datos-nuevo elemento](create_the_data_access_layer/_static/image2.png)
3. Seleccione **clase** en el panel central y asigne a esta nueva clase el nombre *product.CS*.
4. Haga clic en **Agregar**.  
   El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado por el siguiente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Cree otra clase repitiendo los pasos 1 a 4; no obstante, asigne a la nueva clase el nombre *Category.CS* y reemplace el código predeterminado por el código siguiente:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Como se mencionó anteriormente, la clase `Category` representa el tipo de producto que la aplicación está diseñada para vender (como <a id="a"></a>&quot;automóviles&quot;, &quot;barcos&quot;, &quot;cohetes&quot;, etc.) y la clase `Product` representa los productos individuales (juguetes) en la base de datos. Cada instancia de un objeto de `Product` se corresponderá con una fila de una tabla de base de datos relacional, y cada propiedad de la clase Product se asignará a una columna de la tabla de base de datos relacional. Más adelante en este tutorial, revisará los datos del producto incluidos en la base de datos.

### <a name="data-annotations"></a>Anotaciones de datos

Es posible que haya observado que determinados miembros de las clases tienen atributos que especifican detalles sobre el miembro, como `[ScaffoldColumn(false)]`. Se trata de *anotaciones de datos*. Los atributos de anotación de datos pueden describir cómo validar los datos proporcionados por el usuario para ese miembro, especificar su formato y especificar cómo se modelan cuando se crea la base de datos.

### <a name="context-class"></a>Context (Clase)

Para empezar a usar las clases para el acceso a datos, debe definir una clase de contexto. Como se mencionó anteriormente, la clase de contexto administra las clases de entidad (como la clase `Product` y la clase `Category`) y proporciona acceso a los datos a la base de datos.

Este procedimiento agrega una nueva C# clase de contexto a la carpeta *Models* .

1. Haga clic con el botón secundario en la carpeta *modelos* y seleccione **Agregar** -&gt; **nuevo elemento**.   
   Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione **clase** en el panel central, asígnele el nombre *ProductContext.CS* y haga clic en **Agregar**.
3. Reemplace el código predeterminado contenido en la clase por el código siguiente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Este código agrega el espacio de nombres `System.Data.Entity` de forma que tenga acceso a toda la funcionalidad básica de Entity Framework, que incluye la capacidad de consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados.

La clase `ProductContext` representa Entity Framework contexto de la base de datos del producto, que controla la captura, el almacenamiento y la actualización de instancias de clase `Product` en la base de datos. La clase `ProductContext` deriva de la clase base `DbContext` proporcionada por Entity Framework.

### <a name="initializer-class"></a>Initializer (clase)

Tendrá que ejecutar una lógica personalizada para inicializar la base de datos la primera vez que se use el contexto. Esto permitirá que los datos de inicialización se agreguen a la base de datos para que pueda mostrar inmediatamente productos y categorías.

Este procedimiento agrega una nueva C# clase de inicializador a la carpeta *Models* .

1. Cree otro `Class` en la carpeta *Models* y asígnele el nombre *ProductDatabaseInitializer.CS*.
2. Reemplace el código predeterminado contenido en la clase por el código siguiente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Como puede ver en el código anterior, cuando se crea e inicializa la base de datos, la propiedad `Seed` se invalida y se establece. Cuando se establece la propiedad `Seed`, los valores de las categorías y los productos se utilizan para rellenar la base de datos. Si intenta actualizar los datos de inicialización modificando el código anterior una vez creada la base de datos, no verá ninguna actualización al ejecutar la aplicación Web. La razón es que el código anterior usa una implementación de la clase `DropCreateDatabaseIfModelChanges` para reconocer si el modelo (esquema) ha cambiado antes de restablecer los datos de inicialización. Si no se realiza ningún cambio en las clases de entidad `Category` y `Product`, la base de datos no se reinicializará con los datos de inicialización.

> [!NOTE] 
> 
> Si desea que la base de datos se vuelva a crear cada vez que ejecutó la aplicación, podría usar la clase `DropCreateDatabaseAlways` en lugar de la clase `DropCreateDatabaseIfModelChanges`. Sin embargo, para esta serie de tutoriales, utilice la clase `DropCreateDatabaseIfModelChanges`.

En este punto de este tutorial, tendrá una carpeta *Models* con cuatro clases nuevas y una clase predeterminada:

![Crear la carpeta de modelos capa de acceso a datos](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configurar la aplicación para que use el modelo de datos

Ahora que ha creado las clases que representan los datos, debe configurar la aplicación para utilizar las clases. En el archivo *global. asax* , se agrega código que inicializa el modelo. En el archivo *Web. config* , agregue información que indique a la aplicación qué base de datos usará para almacenar los datos representados por las nuevas clases de datos. El archivo *global. asax* se puede usar para controlar eventos o métodos de la aplicación. El archivo *Web. config* permite controlar la configuración de la aplicación Web ASP.net.

#### <a name="updating-the-globalasax-file"></a>Actualizar el archivo global. asax

Para inicializar los modelos de datos cuando se inicia la aplicación, actualizará el controlador de `Application_Start` en el archivo *global.asax.CS* .

> [!NOTE] 
> 
> En Explorador de soluciones, puede seleccionar el archivo *global. asax* o el archivo *global.asax.CS* para editar el archivo *global.asax.CS* .

1. Agregue el siguiente código resaltado en amarillo al método `Application_Start` del archivo *global.asax.CS* .   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> El explorador debe ser compatible con HTML5 para ver el código resaltado en amarillo al ver esta serie de tutoriales en un explorador.

Como se muestra en el código anterior, cuando se inicia la aplicación, la aplicación especifica el inicializador que se ejecutará durante la primera vez que se tenga acceso a los datos. Los dos espacios de nombres adicionales son necesarios para tener acceso al objeto `Database` y al objeto `ProductDatabaseInitializer`.

 Modificar el archivo Web. config 

Aunque Entity Framework Code First generará una base de datos automáticamente en una ubicación predeterminada cuando la base de datos se rellena con datos de inicialización, al agregar su propia información de conexión a la aplicación, podrá controlar la ubicación de la base de datos. Esta conexión de base de datos se especifica mediante una cadena de conexión en el archivo *Web. config* de la aplicación en la raíz del proyecto. Al agregar una nueva cadena de conexión, puede dirigir la ubicación de la base de datos (*wingtiptoys. MDF*) que se va a compilar en el directorio de datos de la aplicación (*datos de\_* de la aplicación), en lugar de su ubicación predeterminada. La realización de este cambio le permitirá buscar e inspeccionar el archivo de base de datos más adelante en este tutorial.

1. En **Explorador de soluciones**, busque y abra el archivo *Web. config* .
2. Agregue la cadena de conexión resaltada en amarillo a la sección `<connectionStrings>` del archivo *Web. config* como se indica a continuación:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Cuando la aplicación se ejecuta por primera vez, generará la base de datos en la ubicación especificada por la cadena de conexión. Pero antes de ejecutar la aplicación, vamos a compilarla primero.

## <a name="building-the-application"></a>Compilar la aplicación

Para asegurarse de que todas las clases y cambios realizados en la aplicación web funcionan, debe compilar la aplicación.

1. En el menú **depurar** , seleccione **compilar WingtipToys**.  
 Se muestra la ventana **salida** y, si todo ha ido bien, verá un mensaje *correcto* .  

    ![Crear la capa de acceso a datos: ventanas de salida](create_the_data_access_layer/_static/image4.png)

Si se encuentra con un error, vuelva a comprobar los pasos anteriores. La información de la ventana **salida** indicará qué archivo tiene un problema y dónde se requiere un cambio en el archivo. Esta información le permitirá determinar qué parte de los pasos anteriores debe revisarse y corregirse en el proyecto.

## <a name="summary"></a>Resumen

En este tutorial de la serie que ha creado el modelo de datos, así como, ha agregado el código que se utilizará para inicializar y propagar la base de datos. También ha configurado la aplicación para usar los modelos de datos cuando se ejecuta la aplicación.

En el siguiente tutorial, actualizará la interfaz de usuario, agregará navegación y recuperará datos de la base de datos. Esto hará que la base de datos se cree automáticamente en función de las clases de entidad creadas en este tutorial.

## <a name="additional-resources"></a>Recursos adicionales

[Información general de Entity Framework](https://msdn.microsoft.com/library/bb399567.aspx)   
[Guía para principiantes sobre el Entity Framework de ADO.NET](https://msdn.microsoft.com/data/ee712907)   
[Desarrollo de Code First con Entity Framework](http://www.msteched.com/2010/Europe/DEV212) (vídeo)   
[API fluida de relaciones Code First](https://msdn.microsoft.com/data/hh134698)   
[Anotaciones de datos de Code First](https://msdn.microsoft.com/data/gg193958)  
[Mejoras de productividad para el Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

> [!div class="step-by-step"]
> [Anterior](create-the-project.md)
> [Siguiente](ui_and_navigation.md)
