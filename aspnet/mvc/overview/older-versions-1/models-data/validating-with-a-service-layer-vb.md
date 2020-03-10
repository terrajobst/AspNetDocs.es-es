---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
title: Validación con un nivel de servicio (VB) | Microsoft Docs
author: StephenWalther
description: Obtenga información acerca de cómo trasladar la lógica de validación de las acciones del controlador y a un nivel de servicio independiente. En este tutorial, Stephen Walther explica cómo...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 344bb38e-4965-4c47-bda1-f6d29ae5b83a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 704657ffe6f50eaf3eb0d91d0d334567003ab7f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78436579"
---
# <a name="validating-with-a-service-layer-vb"></a>Validación con un nivel de servicio (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> Obtenga información acerca de cómo trasladar la lógica de validación de las acciones del controlador y a un nivel de servicio independiente. En este tutorial, Stephen Walther explica cómo puede mantener una separación nítida de problemas aislando la capa de servicio de la capa de controlador.

El objetivo de este tutorial es describir un método para realizar la validación en una aplicación ASP.NET MVC. En este tutorial, aprenderá a trasladar la lógica de validación de los controladores y a un nivel de servicio independiente.

## <a name="separating-concerns"></a>Descomponer problemas

Al compilar una aplicación ASP.NET MVC, no debe colocar la lógica de base de datos dentro de las acciones del controlador. La combinación de la lógica de base de datos y del controlador hace que la aplicación sea más difícil de mantener con el tiempo. Se recomienda colocar toda la lógica de base de datos en una capa de repositorio independiente.

Por ejemplo, la lista 1 contiene un repositorio simple denominado ProductRepository. El repositorio del producto contiene todo el código de acceso a los datos de la aplicación. La lista también incluye la interfaz IProductRepository que implementa el repositorio del producto.

**Lista 1-Models\ProductRepository.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample1.vb)]

El controlador de la lista 2 usa la capa de repositorio en las acciones de índice () y Create (). Tenga en cuenta que este controlador no contiene ninguna lógica de base de datos. La creación de una capa de repositorio le permite mantener una separación limpia de los problemas. Los controladores son responsables de la lógica de control de flujo de aplicación y el repositorio es responsable de la lógica de acceso a datos.

**Lista 2-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample2.vb)]

## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Por lo tanto, la lógica de control de flujo de la aplicación pertenece a un controlador y la lógica de acceso a datos pertenece a un repositorio. En ese caso, ¿dónde se coloca la lógica de validación? Una opción consiste en colocar la lógica de validación en un *nivel de servicio*.

Una capa de servicio es una capa adicional en una aplicación ASP.NET MVC que media la comunicación entre un controlador y un nivel de repositorio. El nivel de servicio contiene la lógica de negocios. En concreto, contiene la lógica de validación.

Por ejemplo, la capa de servicio de producto de la lista 3 tiene un método CreateProduct (). El método CreateProduct () llama al método ValidateProduct () para validar un nuevo producto antes de pasar el producto al repositorio del producto.

**Lista 3: Models\ProductService.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample3.vb)]

El controlador de producto se ha actualizado en la lista 4 para usar el nivel de servicio en lugar de la capa de repositorio. El nivel de controlador se comunica con el nivel de servicio. El nivel de servicio se comunica con la capa de repositorio. Cada capa tiene una responsabilidad independiente.

**Lista 4: Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample4.vb)]

Observe que el servicio de producto se crea en el constructor del controlador del producto. Cuando se crea el servicio de producto, el Diccionario de estado del modelo se pasa al servicio. El servicio de producto utiliza el estado del modelo para devolver los mensajes de error de validación al controlador.

## <a name="decoupling-the-service-layer"></a>Desacoplamiento del nivel de servicio

No se ha podido aislar el controlador y las capas de servicio en un sentido. El controlador y los niveles de servicio se comunican a través del estado del modelo. En otras palabras, el nivel de servicio tiene una dependencia en una característica determinada del marco ASP.NET MVC.

Queremos aislar el nivel de servicio de la capa de controlador tanto como sea posible. En teoría, deberíamos poder usar el nivel de servicio con cualquier tipo de aplicación y no solo una aplicación ASP.NET MVC. Por ejemplo, en el futuro, es posible que deseemos compilar un front-end de WPF para nuestra aplicación. Deberíamos encontrar una manera de quitar la dependencia en el estado del modelo MVC de ASP.NET de nuestro nivel de servicio.

En la lista 5, el nivel de servicio se ha actualizado para que ya no use el estado del modelo. En su lugar, utiliza cualquier clase que implemente la interfaz IValidationDictionary.

**Lista 5-Models\ProductService.vb (desacoplado)**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample5.vb)]

La interfaz IValidationDictionary se define en la lista 6. Esta interfaz simple tiene un único método y una propiedad única.

**Listado 6-Models\IValidationDictionary.cs**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample6.vb)]

La clase de la lista 7, denominada la clase ModelStateWrapper, implementa la interfaz IValidationDictionary. Puede crear una instancia de la clase ModelStateWrapper pasando un diccionario de estado del modelo al constructor.

**Listado 7-Models\ModelStateWrapper.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample7.vb)]

Por último, el controlador actualizado en la lista 8 usa ModelStateWrapper al crear el nivel de servicio en su constructor.

**Lista 8-Controllers\ProductController.vb**

[!code-vb[Main](validating-with-a-service-layer-vb/samples/sample8.vb)]

El uso de la interfaz IValidationDictionary y la clase ModelStateWrapper nos permite aislar completamente nuestro nivel de servicio de nuestra capa de controlador. El nivel de servicio ya no depende del estado del modelo. Puede pasar cualquier clase que implemente la interfaz IValidationDictionary a la capa de servicio. Por ejemplo, una aplicación de WPF podría implementar la interfaz IValidationDictionary con una clase de colección simple.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era analizar un enfoque para realizar la validación en una aplicación ASP.NET MVC. En este tutorial, ha aprendido a trasladar toda la lógica de validación de los controladores y a un nivel de servicio independiente. También ha aprendido a aislar el nivel de servicio de la capa de controlador mediante la creación de una clase ModelStateWrapper.

> [!div class="step-by-step"]
> [Anterior](validating-with-the-idataerrorinfo-interface-vb.md)
> [Siguiente](validation-with-the-data-annotation-validators-vb.md)
