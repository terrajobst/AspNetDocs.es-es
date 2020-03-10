---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: pertenencia y autorización | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 7 cubre la pertenencia y la autorización.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433471"
---
# <a name="part-7-membership-and-authorization"></a>Parte 7: pertenencia y autorización

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 7 cubre la pertenencia y la autorización.

Nuestro controlador del administrador de la tienda es actualmente accesible para cualquier persona que visite nuestro sitio. Vamos a cambiar esto para restringir el permiso a los administradores del sitio.

## <a name="adding-the-accountcontroller-and-views"></a>Agregar AccountController y vistas

Una diferencia entre la plantilla de aplicación Web ASP.NET MVC 3 completa y la plantilla de aplicación Web vacía de ASP.NET MVC 3 es que la plantilla vacía no incluye un controlador de cuentas. Vamos a agregar un controlador de cuenta mediante la copia de algunos archivos de una nueva aplicación de ASP.NET MVC creada a partir de la plantilla de aplicación web completa ASP.NET MVC 3.

Cree una nueva aplicación de ASP.NET MVC con la plantilla de aplicación Web ASP.NET MVC 3 completa y copie los siguientes archivos en los mismos directorios de nuestro proyecto:

1. Copiar AccountController.cs en el directorio Controllers
2. Copiar AccountModels en el directorio Models
3. Cree un directorio de cuentas dentro del directorio views y copie las cuatro vistas en

Cambie el espacio de nombres de las clases del controlador y del modelo para que comiencen con MvcMusicStore. La clase AccountController debe usar el espacio de nombres MvcMusicStore. Controllers y la clase AccountModels debe usar el espacio de nombres MvcMusicStore. Models.

*Nota: estos archivos también están disponibles en la descarga de MvcMusicStore-Assets. zip desde la que se copiaron los archivos de diseño del sitio al principio del tutorial. Los archivos de pertenencia se encuentran en el directorio de código.*

La solución actualizada debe ser similar a la siguiente:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Agregar un usuario administrativo con el sitio de configuración de ASP.NET

Antes de que se requiera la autorización en nuestro sitio web, deberá crear un usuario con acceso. La forma más fácil de crear un usuario es usar el sitio web de configuración de ASP.NET integrado.

Para iniciar el sitio web de configuración de ASP.NET, haga clic en el icono del Explorador de soluciones.

![](mvc-music-store-part-7/_static/image2.png)

Esto inicia un sitio web de configuración. Haga clic en la pestaña seguridad de la pantalla de inicio y, a continuación, haga clic en el vínculo "habilitar roles" en el centro de la pantalla.

![](mvc-music-store-part-7/_static/image3.png)

Haga clic en el vínculo "crear o administrar roles".

![](mvc-music-store-part-7/_static/image4.png)

Escriba "administrador" como nombre del rol y presione el botón Agregar rol.

![](mvc-music-store-part-7/_static/image5.png)

Haga clic en el botón atrás y, a continuación, haga clic en el vínculo crear usuario en el lado izquierdo.

![](mvc-music-store-part-7/_static/image6.png)

Rellene los campos de información de usuario de la izquierda con la siguiente información:

| **Campo** | **Valor** |
| --- | --- |
| **Nombre de usuario** | Administrador |
| **Contraseña** | password123! |
| **Confirmar contraseña** | password123! |
| **Correo electrónico** | (cualquier dirección de correo electrónico funcionará) |
| **Pregunta de seguridad** | (lo que quiera) |
| **Respuesta de seguridad** | (lo que quiera) |

*Nota: puede usar cualquier contraseña que quiera. La configuración de seguridad de contraseña predeterminada requiere una contraseña que tenga una longitud de 7 caracteres y que contenga un carácter no alfanumérico.*

Seleccione el rol de administrador para este usuario y haga clic en el botón crear usuario.

![](mvc-music-store-part-7/_static/image7.png)

En este punto, debería ver un mensaje que indica que el usuario se ha creado correctamente.

![](mvc-music-store-part-7/_static/image8.png)

Ahora puede cerrar la ventana del explorador.

## <a name="role-based-authorization"></a>Autorización basada en roles

Ahora se puede restringir el acceso a StoreManagerController mediante el atributo [Authorize], especificando que el usuario debe tener el rol de administrador para tener acceso a cualquier acción del controlador en la clase.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Nota: el atributo [Authorize] se puede colocar en métodos de acción específicos, así como en el nivel de clase del controlador.*

Ahora, al ir a/StoreManager, se abre un cuadro de diálogo de inicio de sesión:

![](mvc-music-store-part-7/_static/image9.png)

Después de iniciar sesión con la nueva cuenta de administrador, podemos ir a la pantalla de edición del álbum como antes.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-6.md)
> [Siguiente](mvc-music-store-part-8.md)
