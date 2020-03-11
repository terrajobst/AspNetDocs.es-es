---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Descripción de los modelos, las vistas yC#los controladores () | Microsoft Docs
author: StephenWalther
description: ¿Está confundido con respecto a los modelos, las vistas y los controladores? En este tutorial, Stephen Walther le presenta las distintas partes de una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 57dc82d02d38adc2514aa2c02c6f156ed0fb88a6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78486091"
---
# <a name="understanding-models-views-and-controllers-c"></a>Descripción de los modelos, vistas y controladores (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> ¿Está confundido con respecto a los modelos, las vistas y los controladores? En este tutorial, Stephen Walther le presenta las distintas partes de una aplicación ASP.NET MVC.

En este tutorial se proporciona información general de alto nivel sobre los modelos, las vistas y los controladores de ASP.NET MVC. En otras palabras, se explican los M ', V ' y C ' en ASP.NET MVC.

Después de leer este tutorial, debe comprender cómo funcionan conjuntamente las distintas partes de una aplicación ASP.NET MVC. También debe comprender cómo la arquitectura de una aplicación ASP.NET MVC difiere de una aplicación de formularios Web Forms ASP.NET o de una aplicación de Active Server páginas.

## <a name="the-sample-aspnet-mvc-application"></a>La aplicación ASP.NET MVC de ejemplo

La plantilla predeterminada de Visual Studio para crear aplicaciones web MVC de ASP.NET incluye una aplicación de ejemplo muy sencilla que se puede usar para comprender las diferentes partes de una aplicación ASP.NET MVC. Aprovechamos esta sencilla aplicación en este tutorial.

Para crear una nueva aplicación ASP.NET MVC con la plantilla MVC, inicie Visual Studio 2008 y seleccione el archivo de opciones de menú, nuevo proyecto (vea la figura 1). En el cuadro de diálogo nuevo proyecto, seleccione su lenguaje de programación favorito en tipos de C#proyecto (Visual Basic o) y seleccione **aplicación Web MVC de ASP.net** en plantillas. Haga clic en el botón Aceptar.

[![cuadro de diálogo nuevo proyecto](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Figura 01**: cuadro de diálogo nuevo proyecto ([haga clic para ver la imagen de tamaño completo](understanding-models-views-and-controllers-cs/_static/image2.png))

Al crear una nueva aplicación ASP.NET MVC, aparece el cuadro de diálogo **crear proyecto de prueba unitaria** (consulte la figura 2). Este cuadro de diálogo le permite crear un proyecto independiente en la solución para probar la aplicación ASP.NET MVC. Seleccione la opción no **, no crear un proyecto de prueba unitaria** y haga clic en el botón **Aceptar** .

[![cuadro de diálogo crear prueba unitaria](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Figura 02**: cuadro de diálogo crear prueba unitaria ([haga clic para ver la imagen de tamaño completo](understanding-models-views-and-controllers-cs/_static/image4.png))

Después de crear la nueva aplicación ASP.NET MVC. Verá varias carpetas y archivos en la ventana Explorador de soluciones. En concreto, verá tres carpetas denominadas modelos, vistas y controladores. Como puede imaginar de los nombres de carpeta, estas carpetas contienen los archivos para implementar modelos, vistas y controladores.

Si expande la carpeta Controllers, debería ver un archivo denominado AccountController.cs y un archivo denominado HomeController.cs. Si expande la carpeta vistas, debería ver tres subcarpetas denominadas cuenta, Inicio y compartido. Si expande la carpeta Inicio, verá dos archivos adicionales denominados about. aspx e index. aspx (vea la figura 3). Estos archivos componen la aplicación de ejemplo que se incluye con la plantilla ASP.NET MVC predeterminada.

[![la ventana de Explorador de soluciones](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Figura 03**: la ventana de explorador de soluciones ([haga clic para ver la imagen de tamaño completo](understanding-models-views-and-controllers-cs/_static/image6.png))

Puede ejecutar la aplicación de ejemplo seleccionando la opción de menú **depurar, iniciar depuración**. Como alternativa, puede presionar la tecla F5.

Cuando se ejecuta por primera vez una aplicación de ASP.NET, aparece el cuadro de diálogo de la figura 4 que recomienda habilitar el modo de depuración. Haga clic en el botón Aceptar y se ejecutará la aplicación.

[cuadro de diálogo ![depuración no habilitada](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Figura 04**: cuadro de diálogo depuración no habilitada ([haga clic para ver la imagen de tamaño completo](understanding-models-views-and-controllers-cs/_static/image8.png))

Al ejecutar una aplicación ASP.NET MVC, Visual Studio inicia la aplicación en el explorador Web. La aplicación de ejemplo solo consta de dos páginas: la página de índice y la página about. Cuando la aplicación se inicia por primera vez, aparece la página de índice (vea la figura 5). Para ir a la página acerca de, haga clic en el vínculo del menú situado en la parte superior derecha de la aplicación.

[![la página de índice](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Figura 05**: Página de índice ([haga clic para ver la imagen de tamaño completo](understanding-models-views-and-controllers-cs/_static/image11.png))

Observe las direcciones URL en la barra de direcciones del explorador. Por ejemplo, al hacer clic en el vínculo del menú acerca de, la dirección URL de la barra de direcciones del explorador cambia a **/home/About**.

Si cierra la ventana del explorador y vuelve a Visual Studio, no podrá encontrar un archivo con la ruta de acceso home/About. Los archivos no existen. ¿Cómo es esto posible?

## <a name="a-url-does-not-equal-a-page"></a>Una dirección URL no es igual a una página

Al compilar una aplicación de formularios Web Forms tradicional de ASP.NET o una aplicación de Active Server Pages, hay una correspondencia uno a uno entre una dirección URL y una página. Si solicita una página denominada SomePage. aspx del servidor, habrá mejor una página en disco denominada SomePage. aspx. Si el archivo SomePage. aspx no existe, obtendrá un error feo **404-página no encontrada** .

Al compilar una aplicación ASP.NET MVC, en cambio, no hay ninguna correspondencia entre la dirección URL que se escribe en la barra de direcciones del explorador y los archivos que se encuentran en la aplicación. En una aplicación ASP.NET MVC, una dirección URL corresponde a una acción de controlador en lugar de a una página del disco.

En una aplicación ASP.NET o ASP tradicional, las solicitudes del explorador se asignan a las páginas. En una aplicación ASP.NET MVC, en cambio, las solicitudes del explorador se asignan a las acciones del controlador. Una aplicación de formularios Web Forms ASP.NET está centrada en el contenido. Por el contrario, una aplicación ASP.NET MVC es centrada en la lógica de la aplicación.

## <a name="understanding-aspnet-routing"></a>Descripción del enrutamiento de ASP.NET

Una solicitud de explorador se asigna a una acción de controlador a través de una característica del marco ASP.NET denominada *enrutamiento ASP.net*. El marco de MVC de ASP.NET usa el enrutamiento ASP.NET para *enrutar* las solicitudes entrantes a las acciones de controlador.

El enrutamiento de ASP.NET usa una tabla de rutas para controlar las solicitudes entrantes. Esta tabla de rutas se crea cuando se inicia la aplicación web por primera vez. La tabla de rutas se configura en el archivo global. asax. El archivo global. asax de MVC predeterminado se incluye en la lista 1.

**Lista 1: global. asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Cuando se inicia una aplicación ASP.NET por primera vez, se llama al método Start () de la aplicación\_. En la lista 1, este método llama al método RegisterRoutes () y el método RegisterRoutes () crea la tabla de ruta predeterminada.

La tabla de rutas predeterminada consta de una ruta. Esta ruta predeterminada interrumpe todas las solicitudes entrantes en tres segmentos (un segmento de dirección URL es cualquier cosa entre barras diagonales). El primer segmento se asigna a un nombre de controlador, el segundo segmento se asigna a un nombre de acción y el segmento final se asigna a un parámetro pasado a la acción denominada ID.

Pongamos como ejemplo esta URL:

/Product/Details/3

Esta dirección URL se analiza en tres parámetros como este:

Controller = producto

Acción = detalles

ID. = 3

La ruta predeterminada definida en el archivo global. asax incluye los valores predeterminados para los tres parámetros. El controlador predeterminado es Home, la acción predeterminada es index y el ID. predeterminado es una cadena vacía. Teniendo en cuenta estos valores predeterminados, tenga en cuenta cómo se analiza la siguiente dirección URL:

/Employee

Esta dirección URL se analiza en tres parámetros como este:

Controller = empleado

Action = índice

ID. =

Por último, si abre una aplicación ASP.NET MVC sin proporcionar ninguna dirección URL (por ejemplo, `http://localhost`), la dirección URL se analiza de la siguiente manera:

controlador = Inicio

Action = índice

ID. =

La solicitud se enruta a la acción index () en la clase HomeController.

## <a name="understanding-controllers"></a>Descripción de los controladores

Un controlador es responsable de controlar el modo en que un usuario interactúa con una aplicación MVC. Un controlador contiene la lógica de control de flujo para una aplicación ASP.NET MVC. Un controlador determina qué respuesta devolver a un usuario cuando un usuario realiza una solicitud del explorador.

Un controlador es simplemente una clase (por ejemplo, un Visual Basic o C# una clase). La aplicación ASP.NET MVC de ejemplo incluye un controlador denominado HomeController.cs ubicado en la carpeta Controllers. El contenido del archivo HomeController.cs se reproduce en la lista 2.

**Lista 2-HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Observe que HomeController tiene dos métodos denominados index () y about (). Estos dos métodos se corresponden con las dos acciones expuestas por el controlador. La dirección URL/Home/Index invoca el método HomeController. index () y la dirección URL/Home/About invoca el método HomeController. about ().

Cualquier método público de un controlador se expone como una acción de controlador. Debe tener cuidado con esto. Esto significa que cualquier método público incluido en un controlador puede ser invocado por cualquier persona que tenga acceso a Internet; para ello, escriba la dirección URL correcta en un explorador.

## <a name="understanding-views"></a>Descripción de vistas

Las dos acciones de controlador que expone la clase HomeController, index () y about (), devuelven una vista. Una vista contiene el marcado HTML y el contenido que se envía al explorador. Una vista es el equivalente de una página cuando se trabaja con una aplicación ASP.NET MVC.

Debe crear las vistas en la ubicación correcta. La acción HomeController. index () devuelve una vista ubicada en la siguiente ruta de acceso:

\Views\Home\Index.aspx

La acción HomeController. about () devuelve una vista ubicada en la siguiente ruta de acceso:

\Views\Home\About.aspx

En general, si desea que se devuelva una vista para una acción de controlador, deberá crear una subcarpeta en la carpeta views con el mismo nombre que el controlador. Dentro de la subcarpeta, debe crear un archivo. aspx con el mismo nombre que la acción de controlador.

El archivo de la lista 3 contiene la vista about. aspx.

**Lista 3-acerca de. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Si omite la primera línea de la lista 3, la mayoría del resto de la vista se compone de HTML estándar. Puede modificar el contenido de la vista escribiendo el código HTML que desee aquí.

Una vista es muy similar a una página de Active Server páginas o formularios Web Forms de ASP.NET. Una vista puede contener contenido HTML y scripts. Puede escribir los scripts en su lenguaje de programación .NET favorito (por ejemplo C# , o Visual Basic .net). Los scripts se usan para mostrar contenido dinámico como datos de la base de datos.

## <a name="understanding-models"></a>Descripción de los modelos

Hemos analizado los controladores y hemos analizado las vistas. El último tema que necesitamos discutir es modelos. ¿Qué es un modelo de MVC?

Un modelo de MVC contiene toda la lógica de la aplicación que no está incluida en una vista o un controlador. El modelo debe contener toda la lógica de negocios de la aplicación, la lógica de validación y la lógica de acceso a la base de datos. Por ejemplo, si usa Microsoft Entity Framework para tener acceso a la base de datos, debe crear las clases de Entity Framework (el archivo. edmx) en la carpeta models.

Una vista solo debe contener lógica relacionada con la generación de la interfaz de usuario. Un controlador solo debe contener la lógica mínima necesaria para devolver la vista correcta o redirigir al usuario a otra acción (control de flujo). Todo lo demás debe estar incluido en el modelo.

En general, debe procurar los modelos FAT y los controladores de aspectos. Los métodos de controlador deben contener solo unas pocas líneas de código. Si una acción de controlador es demasiado grande, considere la posibilidad de mover la lógica a una nueva clase en la carpeta models.

## <a name="summary"></a>Resumen

En este tutorial se proporciona información general de alto nivel sobre las diferentes partes de una aplicación web MVC de ASP.NET. Ha aprendido cómo el enrutamiento de ASP.NET asigna solicitudes del explorador entrante a acciones de controlador concretas. Ha aprendido cómo los controladores organizan cómo se devuelven las vistas al explorador. Por último, ha aprendido cómo los modelos contienen lógica de negocios, validación y acceso a bases de datos de la aplicación.
