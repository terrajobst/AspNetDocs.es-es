---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Blob Storage no estructurados (creación de aplicaciones en la nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: La creación de aplicaciones reales en la nube con el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 2afd4b5cf640eb97080de7e5280409f5e5347731
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583623"
---
# <a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Blob Storage no estructurados (creación de aplicaciones en la nube reales con Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de corrección de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar el libro electrónico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creación de aplicaciones reales en la nube con** el libro electrónico de Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas que pueden ayudarle a desarrollar correctamente aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

En el capítulo anterior, analizamos los esquemas de partición y explicamos cómo la aplicación de corrección de ti almacena imágenes en el servicio de Azure Storage Blob y otros datos de tareas en Azure SQL Database. En este capítulo profundizaremos en el Blob service y mostraremos cómo se implementa en el código del proyecto de corrección de ti.

## <a name="what-is-blob-storage"></a>¿Qué es BLOB Storage?

El servicio Azure Storage Blob proporciona una manera de almacenar archivos en la nube. El Blob service tiene varias ventajas sobre el almacenamiento de archivos en un sistema de archivos de red local:

- Es muy escalable. Una sola cuenta de almacenamiento puede almacenar [cientos de terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), y puede tener varias cuentas de almacenamiento. Algunos de los mayores clientes de Azure almacenan cientos de petabytes. Microsoft SkyDrive usa el almacenamiento de blobs.
- Es duradero. Se realiza automáticamente una copia de seguridad de todos los archivos almacenados en el Blob service.
- Proporciona alta disponibilidad. El [acuerdo de nivel de servicio para el almacenamiento](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promete 99,9% o 99,99% de tiempo de actividad, dependiendo de la opción de redundancia geográfica que elija.
- Se trata de una característica de plataforma como servicio (PaaS) de Azure, lo que significa que solo se almacenan y recuperan archivos, solo se paga por la cantidad real de almacenamiento que se usa y Azure se encarga de configurar y administrar todas las máquinas virtuales y las unidades de disco necesarias para el servicio.
- Puede tener acceso a la Blob service mediante una API de REST o mediante una API de lenguaje de programación. Los SDK están disponibles para .NET, Java, Ruby y otros.
- Al almacenar un archivo en el Blob service, puede hacer que esté disponible públicamente a través de Internet.
- Puede proteger los archivos en el Blob service para que solo los usuarios autorizados puedan acceder a ellos, o bien puede proporcionar tokens de acceso temporales que los pone a disposición de un solo usuario durante un período de tiempo limitado.

Siempre que esté compilando una aplicación para Azure y desee almacenar una gran cantidad de datos en un entorno local, en archivos, como imágenes, vídeos, PDF, hojas de cálculo, etc., tenga en cuenta el Blob service.

## <a name="creating-a-storage-account"></a>Creación de una cuenta de almacenamiento

Para empezar a trabajar con el Blob service cree una cuenta de almacenamiento en Azure. En el portal, haga clic en **nuevo** -- **Data Services** -- **almacenamiento** -- **creación rápida**y, a continuación, escriba una dirección URL y una ubicación de centro de datos. La ubicación del centro de datos debe ser la misma que la aplicación Web.

![Crear una cuenta de almacenamiento](unstructured-blob-storage/_static/image1.png)

Elija la región primaria en la que desea almacenar el contenido y, si elige la opción [de replicación geográfica](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) , Azure crea réplicas de todos los datos en un centro de datos diferente en otra región del país. Por ejemplo, si elige el centro de datos del oeste de EE. UU., al almacenar un archivo irá al centro de datos del oeste de EE. UU., pero en Azure en segundo plano también lo copiará en uno de los otros centros de datos de Estados Unidos. Si se produce un desastre en una región del país, los datos siguen siendo seguros.

Azure no replicará los datos a través de los límites geopolíticos: Si la ubicación principal está en EE. UU., los archivos solo se replican en otra región de los EE. UU. Si la ubicación principal es Australia, los archivos solo se replican en otro centro de datos en Australia.

Por supuesto, también puede crear una cuenta de almacenamiento ejecutando comandos desde un script, como vimos anteriormente. A continuación se muestra un comando de Windows PowerShell para crear una cuenta de almacenamiento:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Una vez que tenga una cuenta de almacenamiento, puede iniciar inmediatamente el almacenamiento de archivos en el Blob service.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Uso de BLOB Storage en la aplicación Fix it

La aplicación Fix it le permite cargar fotos.

![Crear una tarea corregir ti](unstructured-blob-storage/_static/image2.png)

Al hacer clic en **crear el FixIt**, la aplicación carga el archivo de imagen especificado y lo almacena en el BLOB Service.

### <a name="set-up-the-blob-container"></a>Configuración del contenedor de blobs

Para almacenar un archivo en el Blob service necesita un *contenedor* en el que almacenarlo. Un contenedor de Blob service se corresponde con una carpeta del sistema de archivos. Los scripts de creación de entorno que hemos revisado en el [capítulo automatizar todo](automate-everything.md) crean la cuenta de almacenamiento, pero no crean un contenedor. Por lo tanto, el propósito del método `CreateAndConfigure` de la clase `PhotoService` es crear un contenedor si aún no existe. Se llama a este método desde el método `Application_Start` en *global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

El nombre y la clave de acceso de la cuenta de almacenamiento se almacenan en la `appSettings` colección del archivo *Web. config* y el código del método `StorageUtils.StorageAccount` usa esos valores para crear una cadena de conexión y establecer una conexión:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

A continuación, el método `CreateAndConfigureAsync` crea un objeto que representa el Blob service y un objeto que representa un contenedor (carpeta) llamado "images" en el Blob service:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Si no existe un contenedor con el nombre "images", que será true la primera vez que ejecute la aplicación en una nueva cuenta de almacenamiento, el código creará el contenedor y establecerá los permisos para que sea público. (De forma predeterminada, los nuevos contenedores de blobs son privados y solo son accesibles para los usuarios que tienen permiso de acceso a la cuenta de almacenamiento).

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Almacenar la foto cargada en el almacenamiento de blobs

Para cargar y guardar el archivo de imagen, la aplicación usa una interfaz de `IPhotoService` y una implementación de la interfaz en la clase `PhotoService`. El archivo *PhotoService.CS* contiene todo el código de la aplicación Fix it que se comunica con el BLOB Service.

Cuando el usuario hace clic en **crear el FixIt**, se llama al siguiente método de controlador de MVC. En este código, `photoService` hace referencia a una instancia de la clase `PhotoService` y `fixittask` hace referencia a una instancia de la clase de entidad `FixItTask` que almacena los datos de una nueva tarea.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

El método `UploadPhotoAsync` de la clase `PhotoService` almacena el archivo cargado en el Blob service y devuelve una dirección URL que apunta al nuevo BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Como en el método `CreateAndConfigure`, el código se conecta a la cuenta de almacenamiento y crea un objeto que representa el contenedor de blobs "images", salvo que aquí se supone que el contenedor ya existe.

A continuación, crea un identificador único para la imagen que se va a cargar, mediante la concatenación de un nuevo valor GUID con la extensión de archivo:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

A continuación, el código usa el objeto contenedor de blobs y el nuevo identificador único para crear un objeto de BLOB, establece un atributo en ese objeto que indica qué tipo de archivo es y, a continuación, usa el objeto de BLOB para almacenar el archivo en el almacenamiento de blobs.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Por último, obtiene una dirección URL que hace referencia al BLOB. Esta dirección URL se almacenará en la base de datos y se puede usar en las páginas web de corrección de TI para mostrar la imagen cargada.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Esta dirección URL se almacena en la base de datos como una de las columnas de la tabla FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Solo con la dirección URL de la base de datos, y las imágenes del almacenamiento de blobs, la aplicación Fix it mantiene la base de datos pequeña, escalable y económica, mientras que las imágenes se almacenan donde el almacenamiento es barato y capaz de administrar terabytes o petabytes. Una cuenta de almacenamiento puede almacenar cientos de terabytes de fotografías de correcciones de ti y solo paga por lo que usa. Por lo tanto, puede empezar con un pequeño pago de 9 céntimos por el primer Gigabyte y agregar más imágenes para Penn por gigabyte adicional.

### <a name="display-the-uploaded-file"></a>Mostrar el archivo cargado

La aplicación Fix it muestra el archivo de imagen cargado cuando muestra detalles de una tarea.

![Corregir detalles de la tarea de TI con foto](unstructured-blob-storage/_static/image3.png)

Para mostrar la imagen, toda la vista de MVC tiene que incluir el valor de `PhotoUrl` en el HTML que se envía al explorador. El servidor Web y la base de datos no usan ciclos para mostrar la imagen, sino que solo atienden unos pocos bytes a la dirección URL de la imagen. En el siguiente código Razor, `Model` hace referencia a una instancia de la clase de entidad `FixItTask`.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Si observa el código HTML de la página que se muestra, verá la dirección URL que apunta directamente a la imagen en el almacenamiento de blobs, algo parecido a esto:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Resumen

Ha visto cómo la aplicación de corrección de ti almacena imágenes en el Blob service y solo las direcciones URL de imagen en la base de datos SQL. El uso de la Blob service mantiene la base de datos SQL mucho más pequeña de lo que sería, hace posible escalar verticalmente a un número casi ilimitado de tareas y se puede realizar sin escribir mucho código.

Puede tener cientos de terabytes en una cuenta de almacenamiento y el costo de almacenamiento es mucho más económico que SQL Database almacenamiento, empezando por unos 3 céntimos por Gigabyte al mes más un gasto de transacciones pequeño. Y tenga en cuenta que no está pagando por la capacidad máxima, sino solo por la cantidad que almacena realmente, por lo que la aplicación está lista para escalarse, pero no está pagando por toda esa capacidad adicional.

En el [siguiente capítulo](design-to-survive-failures.md) hablaremos acerca de la importancia de crear una aplicación en la nube capaz de controlar los errores correctamente.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

- [Una introducción a Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike madera.
- [Cómo usar el servicio de Azure BLOB Storage en .net](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentación oficial sobre el sitio de MicrosoftAzure.com. Una breve introducción al almacenamiento de blobs seguido de ejemplos de código que muestran cómo conectarse al almacenamiento de blobs, crear contenedores, cargar y descargar blobs, etc.
- [Failsafe: creación de Cloud Services escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Series de vídeos de nueve partes de Ulrich Homann, Marc Mercuri y Mark SIMM. Presenta conceptos de alto nivel y principios arquitectónicos de una manera muy accesible e interesante, con historias tomadas de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Para obtener una explicación de Azure Storage servicio y los blobs, consulte el episodio 5 a partir de 35:13.
- [Patrones y prácticas de Microsoft: Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte patrón de clave valet.

> [!div class="step-by-step"]
> [Anterior](data-partitioning-strategies.md)
> [Siguiente](design-to-survive-failures.md)
