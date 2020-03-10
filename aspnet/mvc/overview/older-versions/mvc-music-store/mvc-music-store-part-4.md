---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: acceso a los modelos y a los datos | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 4 cubre los modelos y el acceso a los datos.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 402be340f1ea3344675e7b859cea8c5130cfc8ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451021"
---
# <a name="part-4-models-and-data-access"></a>Parte 4: modelos y acceso a datos

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.
> 
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 4 cubre los modelos y el acceso a los datos.

Hasta ahora, acabamos de pasar "datos ficticios" de nuestros controladores a nuestras plantillas de vista. Ahora estamos preparados para enlazar una base de datos real. En este tutorial, vamos a tratar cómo usar SQL Server Compact Edition (a menudo denominado SQL CE) como nuestro motor de base de datos. SQL CE es una base de datos gratuita, incrustada y basada en archivos que no requiere ninguna instalación o configuración, lo que hace que sea realmente conveniente para el desarrollo local.

## <a name="database-access-with-entity-framework-code-first"></a>Acceso a bases de datos con Entity Framework primero código

Usaremos la compatibilidad con Entity Framework (EF) que se incluye en los proyectos de ASP.NET MVC 3 para consultar y actualizar la base de datos. EF es una API de datos de asignación relacional de objetos (ORM) flexible que permite a los desarrolladores consultar y actualizar los datos almacenados en una base de datos de forma orientada a objetos.

Entity Framework versión 4 admite un paradigma de desarrollo denominado primero código. Code-First permite crear un objeto de modelo escribiendo clases simples (también conocidas como POCO a partir de objetos CLR "antiguos sin formato") y puede incluso crear la base de datos sobre la marcha desde las clases.

### <a name="changes-to-our-model-classes"></a>Cambios en las clases de modelo

En este tutorial se aprovechará la característica de creación de bases de datos en Entity Framework. Sin embargo, antes de hacerlo, vamos a realizar algunos cambios menores en las clases del modelo para agregar algunas cosas que usaremos más adelante.

#### <a name="adding-the-artist-model-classes"></a>Adición de las clases del modelo de intérprete

Nuestros álbumes se asociarán con artistas, por lo que agregaremos una clase de modelo simple para describir un artista. Agregue una nueva clase a la carpeta models denominada Artist.cs con el código que se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Actualización de las clases del modelo

Actualice la clase album como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

A continuación, realice las siguientes actualizaciones en la clase Genre.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-app_data-folder"></a>Adición de la carpeta de datos de\_de la aplicación

Agregaremos una aplicación\_directorio de datos a nuestro proyecto para que contenga los archivos de base de datos de SQL Server Express. Los datos de\_de la aplicación son un directorio especial en ASP.NET que ya tiene los permisos de acceso de seguridad correctos para el acceso a la base de datos. En el menú proyecto, seleccione Agregar carpeta ASP.NET y, a continuación,\_datos de la aplicación.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Crear una cadena de conexión en el archivo Web. config

Agregaremos algunas líneas al archivo de configuración del sitio web para que Entity Framework sepa cómo conectarse a nuestra base de datos. Haga doble clic en el archivo Web. config que se encuentra en la raíz del proyecto.

![](mvc-music-store-part-4/_static/image2.png)

Desplácese hasta la parte inferior de este archivo y agregue una sección &lt;connectionStrings&gt; directamente encima de la última línea, como se muestra a continuación.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Agregar una clase de contexto

Haga clic con el botón secundario en la carpeta modelos y agregue una nueva clase denominada MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Esta clase representará el Entity Framework contexto de base de datos y controlará nuestras operaciones de creación, lectura, actualización y eliminación. A continuación se muestra el código de esta clase.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Es decir, no hay ninguna otra configuración, interfaces especiales, etc. Mediante la extensión de la clase base DbContext, nuestra clase MusicStoreEntities es capaz de controlar nuestras operaciones de base de datos. Ahora que hemos enlazado, vamos a agregar algunas propiedades más a las clases del modelo para aprovechar parte de la información adicional de nuestra base de datos.

### <a name="adding-our-store-catalog-data"></a>Agregar los datos del catálogo de tiendas

Se aprovechará una característica de Entity Framework que agrega datos de "inicialización" a una base de datos recién creada. Esto rellenará previamente el catálogo de tiendas con una lista de géneros, artistas y álbumes. La descarga de MvcMusicStore-Assets. zip, que incluía los archivos de diseño del sitio usados anteriormente en este tutorial, tiene un archivo de clase con estos datos de inicialización, ubicado en una carpeta denominada Code.

Dentro de la carpeta Code/Models, busque el archivo SampleData.cs y colóquelo en la carpeta models en nuestro proyecto, como se muestra a continuación.

![](mvc-music-store-part-4/_static/image4.png)

Ahora debemos agregar una línea de código para indicar Entity Framework sobre esa clase SampleData. Haga doble clic en el archivo global. asax en la raíz del proyecto para abrirlo y agregue la siguiente línea al principio del método de inicio de la aplicación\_.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

En este momento, hemos completado el trabajo necesario para configurar Entity Framework para nuestro proyecto.

## <a name="querying-the-database"></a>Consultar la base de datos

Ahora vamos a actualizar nuestro StoreController de modo que, en lugar de usar "datos ficticios", llame a nuestra base de datos para consultar toda su información. Comenzaremos declarando un campo en el **StoreController** para que contenga una instancia de la clase MusicStoreEntities, denominada storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Actualizar el índice de almacén para consultar la base de datos

El Entity Framework mantiene la clase MusicStoreEntities y expone una propiedad de colección para cada tabla de nuestra base de datos. Vamos a actualizar la acción de índice de StoreController para recuperar todos los géneros de nuestra base de datos. Anteriormente lo hicimos mediante la codificación de datos de cadena. Ahora podemos usar la Entity Framework colección generes context:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

No es necesario realizar ningún cambio en nuestra plantilla de vista, ya que seguimos devolviendo el mismo StoreIndexViewModel que hemos devuelto antes de que solo se devuelvan datos activos de nuestra base de datos.

Al ejecutar el proyecto de nuevo y visitar la dirección URL "/Store", ahora veremos una lista de todos los géneros de nuestra base de datos:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Actualización de la exploración y los detalles del almacén para usar datos activos

Con el método de acción/Store/Browse? Genre = *[some-Genre]* , buscamos un género por nombre. Solo se espera un resultado, ya que nunca tenemos dos entradas para el mismo nombre de género y, por tanto, podemos usar. Extensión Single () en LINQ para consultar el objeto de género adecuado como este (no lo escriba todavía):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

El método único toma una expresión lambda como parámetro, que especifica que se desea un objeto de género único, de modo que su nombre coincida con el valor que hemos definido. En el caso anterior, vamos a cargar un objeto de género único con un valor de nombre que coincide con disco.

Sacaremos provecho de una característica Entity Framework que nos permite indicar otras entidades relacionadas que queremos cargar también cuando se recupera el objeto Genre. Esta característica se denomina modelado de resultados de consultas y nos permite reducir el número de veces que necesitamos tener acceso a la base de datos para recuperar toda la información que necesitamos. Queremos realizar una captura previa de los álbumes para el género que recuperemos, por lo que actualizaremos la consulta para incluirla en los géneros. incluya ("álbumes") para indicar que también queremos álbumes relacionados. Esto es más eficaz, ya que recuperará los datos del género y del álbum en una única solicitud de base de datos.

Con las explicaciones fuera del camino, este es el aspecto de nuestra acción de controlador de exploración actualizada:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Ahora podemos actualizar la vista de exploración del almacén para mostrar los álbumes que están disponibles en cada género. Abra la plantilla de vista (que se encuentra en/Views/Store/Browse.cshtml) y agregue una lista con viñetas de álbumes como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

La ejecución de la aplicación y la navegación a/Store/Browse? Genre = jazz muestra que los resultados se están extrayendo de la base de datos y muestran todos los álbumes del género seleccionado.

![](mvc-music-store-part-4/_static/image2.jpg)

Realizaremos el mismo cambio en la dirección URL de/Store/Details/[id] y reemplazaremos los datos ficticios por una consulta de base de datos que cargue un álbum cuyo identificador coincida con el valor del parámetro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

La ejecución de la aplicación y el desplazamiento a/Store/Details/1 muestra que ahora se extraen los resultados de la base de datos.

![](mvc-music-store-part-4/_static/image5.png)

Ahora que la página de detalles de la tienda está configurada para mostrar un álbum por el identificador del álbum, vamos a actualizar la vista **examinar** para vincularla a la vista detalles. Usaremos HTML. ActionLink, exactamente como hicimos para vincular el índice de almacén al de la tienda al final de la sección anterior. A continuación se muestra el código fuente completo de la vista examinar.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Ahora podemos examinar desde nuestra página de la tienda hasta una página de género, en la que se enumeran los álbumes disponibles y haciendo clic en un álbum en el que se pueden ver los detalles de ese álbum.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-3.md)
> [Siguiente](mvc-music-store-part-5.md)
