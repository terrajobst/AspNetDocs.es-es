---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: información general y creación del proyecto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447937"
---
# <a name="part-1-overview-and-creating-the-project"></a>Parte 1: información general y creación del proyecto

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework es un marco de trabajo de asignación relacional/objeto. Asigna los objetos de dominio del código a las entidades de una base de datos relacional. En su mayor parte, no tiene que preocuparse por el nivel de base de datos, porque Entity Framework se encarga de ello. El código manipula los objetos y los cambios se conservan en una base de datos.

## <a name="about-the-tutorial"></a>Acerca del tutorial

En este tutorial, creará una aplicación de almacenamiento simple. Hay dos partes principales en la aplicación. Los usuarios normales pueden ver los productos y crear pedidos:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Los administradores pueden crear, eliminar o editar productos:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aprenderá lo siguiente:

- Cómo usar Entity Framework con ASP.NET Web API.
- Cómo usar Knockout. js para crear una interfaz de usuario de cliente dinámica.
- Cómo usar la autenticación de formularios con la API Web para autenticar a los usuarios.

Aunque este tutorial es independiente, puede que desee leer los siguientes tutoriales en primer lugar:

- [Su primer ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Creación de una API Web que admita operaciones CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Algunos conocimientos de [ASP.NET MVC](../../../../mvc/index.md) también son útiles.

## <a name="overview"></a>Información general

En un nivel alto, esta es la arquitectura de la aplicación:

- ASP.NET MVC genera las páginas HTML para el cliente.
- ASP.NET Web API expone operaciones CRUD en los datos (productos y pedidos).
- Entity Framework traduce los C# modelos utilizados por Web API en entidades de base de datos.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

En el diagrama siguiente se muestra cómo se representan los objetos de dominio en varias capas de la aplicación: la capa de base de datos, el modelo de objetos y, por último, el formato de conexión, que se usa para transmitir los datos al cliente a través de HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

Puede crear el proyecto tutorial mediante Visual Web Developer Express o la versión completa de Visual Studio.

En la página de **Inicio** , haga clic en **nuevo proyecto**.

En el panel **plantillas** , seleccione **plantillas instaladas** y expanda el nodo **Visual C#**  . En **Visual C#** , seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Asigne al proyecto el nombre "ProductStore" y haga clic en **Aceptar**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de Internet** y haga clic en **Aceptar**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

La plantilla "aplicación de Internet" crea una aplicación de ASP.NET MVC que admite la autenticación de formularios. Si ejecuta la aplicación ahora, ya tiene algunas características:

- Los usuarios nuevos pueden registrarse haciendo clic en el vínculo "Register" en la esquina superior derecha.
- Los usuarios registrados pueden iniciar sesión haciendo clic en el vínculo "iniciar sesión".

La información de pertenencia se conserva en una base de datos que se crea automáticamente. Para obtener más información sobre la autenticación de formularios en ASP.NET MVC, vea [Tutorial: usar la autenticación de formularios en ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Actualizar el archivo CSS

Este paso es cosmético, pero hará que las páginas se representen como las capturas de pantalla anteriores.

En Explorador de soluciones, expanda la carpeta Content y abra el archivo denominado site. CSS. Agregue los siguientes estilos CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Siguiente](using-web-api-with-entity-framework-part-2.md)
