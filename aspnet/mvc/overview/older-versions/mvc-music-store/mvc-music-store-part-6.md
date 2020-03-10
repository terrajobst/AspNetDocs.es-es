---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: uso de anotaciones de datos para la validación del modelo | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 6 cubre el uso de anotaciones de datos para el modelo V...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: bc031dd5be61cc6707c522f85f6af77a420c8b31
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433537"
---
# <a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: uso de anotaciones de datos para la validación del modelo

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 6 cubre el uso de anotaciones de datos para la validación del modelo.

Tenemos un problema importante con nuestros formularios de creación y edición: no están realizando ninguna validación. Podemos hacer cosas como dejar en blanco los campos obligatorios o escribir letras en el campo Precio, y el primer error que veremos en la base de datos.

Podemos agregar fácilmente la validación a nuestra aplicación agregando anotaciones de datos a nuestras clases de modelo. Las anotaciones de datos nos permiten describir las reglas que queremos aplicar a nuestras propiedades del modelo, y ASP.NET MVC se encargará de aplicarlas y mostrar los mensajes adecuados a los usuarios.

## <a name="adding-validation-to-our-album-forms"></a>Agregar validación a los formularios de álbumes

Usaremos los siguientes atributos de anotación de datos:

- **Requerido** : indica que la propiedad es un campo obligatorio.
- **DisplayName** : define el texto que se desea usar en los campos de formulario y en los mensajes de validación.
- **StringLength** : define una longitud máxima para un campo de cadena.
- **Intervalo** : proporciona un valor máximo y mínimo para un campo numérico.
- **BIND** : enumera los campos que se van a excluir o incluir al enlazar valores de parámetro o formulario a las propiedades del modelo
- **ScaffoldColumn** : permite ocultar campos de los formularios del editor

*Nota: para obtener más información sobre la validación de modelos mediante atributos de anotación de datos, consulte la documentación de MSDN en* [`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Abra la clase album y agregue las siguientes instrucciones *using* a la parte superior.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

A continuación, actualice las propiedades para agregar atributos de presentación y validación como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Aunque estamos ahí, también hemos cambiado el género y el artista a propiedades virtuales. Esto permite Entity Framework para cargarlos de forma diferida según sea necesario.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Después de agregar estos atributos a nuestro modelo de álbum, nuestra pantalla de creación y edición comienza inmediatamente a validar los campos y usando los nombres para mostrar que hemos elegido (por ejemplo, dirección URL de carátula de álbum en lugar de AlbumArtUrl). Ejecute la aplicación y vaya a/StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

A continuación, se interrumpirán algunas reglas de validación. Escriba un precio de 0 y deje el título en blanco. Cuando hacemos clic en el botón crear, veremos que el formulario se muestra con mensajes de error de validación que muestran qué campos no cumplían las reglas de validación que hemos definido.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Probar la validación del lado cliente

La validación del lado servidor es muy importante desde el punto de vista de la aplicación, ya que los usuarios pueden eludir la validación del lado cliente. Sin embargo, los formularios de página web que solo implementan la validación del servidor presentan tres problemas importantes.

1. El usuario tiene que esperar a que el formulario se exponga, se valide en el servidor y para que la respuesta se envíe a su explorador.
2. El usuario no recibe comentarios inmediatos cuando corrige un campo para que ahora pase las reglas de validación.
3. Estamos desperdiciando recursos del servidor para realizar la lógica de validación en lugar de aprovechar el explorador del usuario.

Afortunadamente, las plantillas scaffolding de ASP.NET MVC 3 tienen la validación del lado cliente integrada y no requieren ningún trabajo adicional.

Al escribir una sola letra en el campo título, se cumplen los requisitos de validación, por lo que el mensaje de validación se quita inmediatamente.

![](mvc-music-store-part-6/_static/image3.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-5.md)
> [Siguiente](mvc-music-store-part-7.md)
