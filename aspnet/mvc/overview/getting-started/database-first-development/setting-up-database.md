---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: 'Tutorial: Introducción a EF Database First con MVC 5'
description: En este tutorial se muestra cómo empezar con una base de datos existente y crear rápidamente una aplicación web que permite a los usuarios interactuar con los datos.
author: Rick-Anderson
ms.author: riande
ms.date: 01/15/2019
ms.topic: tutorial
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: a760767839a834a9c7e9fe358a3fd806a833261f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471463"
---
# <a name="tutorial-get-started-with-ef-database-first-using-mvc-5"></a>Tutorial: Introducción a EF Database First con MVC 5

Con el scaffolding de MVC, Entity Framework y ASP.NET, puede crear una aplicación web que proporcione una interfaz a una base de datos existente. En esta serie de tutoriales se muestra cómo generar automáticamente código que permite a los usuarios mostrar, editar, crear y eliminar datos que residen en una tabla de base de datos. El código generado corresponde a las columnas de la tabla de base de datos. En la última parte de la serie, aprenderá a agregar anotaciones de datos al modelo de datos para especificar los requisitos de validación y el formato de presentación. Cuando haya terminado, puede avanzar a un artículo de Azure para aprender a implementar una aplicación .NET y SQL Database para Azure App Service.

En este tutorial se muestra cómo empezar con una base de datos existente y crear rápidamente una aplicación web que permite a los usuarios interactuar con los datos. Usa el Entity Framework 6 y MVC 5 para compilar la aplicación Web. La característica de scaffolding de ASP.NET permite generar automáticamente código para mostrar, actualizar, crear y eliminar datos. Con las herramientas de publicación de Visual Studio, puede implementar fácilmente el sitio y la base de datos en Azure.

Esta parte de la serie se centra en crear una base de datos y rellenarla con datos.

Esta serie se ha escrito con las contribuciones de Tom Dykstra y Rick Anderson. Se mejoró en función de los comentarios de los usuarios en la sección de comentarios.

En este tutorial va a:

> [!div class="checklist"]
> * Configurar la base de datos

## <a name="prerequisites"></a>Requisitos previos

[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="set-up-the-database"></a>Configurar la base de datos

Para imitar el entorno de tener una base de datos existente, primero debe crear una base de datos con algunos datos rellenados previamente y, a continuación, crear la aplicación web que se conecta a la base de datos.

Este tutorial se desarrolló con LocalDB con Visual Studio 2017. Puede usar un servidor de base de datos existente en lugar de LocalDB, pero en función de la versión de Visual Studio y del tipo de base de datos, es posible que no se admitan todas las herramientas de datos de Visual Studio. Si las herramientas no están disponibles para la base de datos, puede que necesite realizar algunos de los pasos específicos de la base de datos en el conjunto de administración de la base de datos.

Si tiene un problema con las herramientas de base de datos en su versión de Visual Studio, asegúrese de que ha instalado la versión más reciente de las herramientas de base de datos. Para obtener información sobre la actualización o la instalación de las herramientas de base de datos, vea [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Inicie Visual Studio y cree un **proyecto de base de datos de SQL Server**. Asigne al proyecto el nombre **ContosoUniversityData**.

![crear proyecto de base de datos](setting-up-database/_static/image1.png)

Ahora tiene un proyecto de base de datos vacío. Para asegurarse de que puede implementar esta base de datos en Azure, establezca Azure SQL Database como plataforma de destino para el proyecto. La configuración de la plataforma de destino no implementa realmente la base de datos. solo significa que el proyecto de base de datos comprobará que el diseño de la base de datos es compatible con la plataforma de destino. Para establecer la plataforma de destino, abra las **propiedades** del proyecto y seleccione **Microsoft Azure SQL Database** para la plataforma de destino.

Puede crear las tablas necesarias para este tutorial agregando scripts SQL que definen las tablas. Haga clic con el botón derecho en el proyecto y agregue un nuevo elemento. Seleccione **tablas y vistas** > **tabla** y asígnele el nombre *Student*.

En el archivo de tabla, reemplace el comando T-SQL con el código siguiente para crear la tabla.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Tenga en cuenta que la ventana de diseño se sincroniza automáticamente con el código. Puede trabajar con el código o con el diseñador.

![Mostrar código y diseño](setting-up-database/_static/image5.png)

Agregue otra tabla. Esta vez, asígnele el nombre Course y use el siguiente comando de T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

Y, repita una vez más para crear una tabla denominada Enrollment.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Puede rellenar la base de datos con datos a través de un script que se ejecuta después de implementar la base de datos. Agregue un script posterior a la implementación al proyecto. Haga clic con el botón derecho en el proyecto y agregue un nuevo elemento. Seleccione scripts de **usuario** > **script posterior a la implementación**. Puede usar el nombre predeterminado.

Agregue el siguiente código de T-SQL al script posterior a la implementación. Este script simplemente agrega datos a la base de datos cuando no se encuentra ningún registro coincidente. No sobrescribe ni elimina ningún dato que haya escrito en la base de datos.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Es importante tener en cuenta que el script posterior a la implementación se ejecuta cada vez que se implementa el proyecto de base de datos. Por lo tanto, debe tener en cuenta los requisitos al escribir este script. En algunos casos, puede que desee comenzar de nuevo desde un conjunto de datos conocido cada vez que se implemente el proyecto. En otros casos, es posible que no desee modificar los datos existentes de ningún modo. En función de sus requisitos, puede decidir si necesita un script posterior a la implementación o lo que necesita incluir en el script. Para obtener más información sobre cómo rellenar la base de datos con un script posterior a la implementación, vea [incluir datos en un proyecto de base de datos de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Ahora tiene 4 archivos de script de SQL, pero no tiene ninguna tabla real. Está listo para implementar el proyecto de base de datos en LocalDB. En Visual Studio, haga clic en el botón Inicio (o F5) para compilar e implementar el proyecto de base de datos. Compruebe la pestaña **salida** para comprobar que la compilación y la implementación se realizaron correctamente.

Para ver que se ha creado la nueva base de datos, Abra **Explorador de objetos de SQL Server** y busque el nombre del proyecto en el servidor de base de datos local correcto (en este caso **, LocalDB) \ProjectsV13**).

Para ver que las tablas se rellenan con datos, haga clic con el botón secundario en una tabla y seleccione **ver datos**.

![Mostrar datos de tabla](setting-up-database/_static/image9.png)

Se muestra una vista editable de los datos de la tabla. Por ejemplo, si selecciona **tablas** > **dbo. Course** > **ver datos**, verá una tabla con tres columnas (**curso**, **título**y **créditos**) y cuatro filas.

## <a name="additional-resources"></a>Recursos adicionales

Para obtener un ejemplo introductorio del desarrollo de Code First, consulte [Introducción con ASP.NET MVC 5](../introduction/getting-started.md). Para obtener un ejemplo más avanzado, vea [creación de un modelo de datos de Entity Framework para una aplicación ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Para obtener instrucciones sobre cómo seleccionar qué enfoque de Entity Framework usar, consulte [métodos de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Configurar la base de datos

Avance al siguiente tutorial para aprender a crear la aplicación web y los modelos de datos.
> [!div class="nextstepaction"]
> [Crear la aplicación web y los modelos de datos](creating-the-web-application.md)
