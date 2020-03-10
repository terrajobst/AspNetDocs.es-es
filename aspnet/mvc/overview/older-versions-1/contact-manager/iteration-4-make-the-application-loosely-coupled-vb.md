---
uid: mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
title: '#4 de iteración: hacer que la aplicación esté acoplada de forma flexible (VB) | Microsoft Docs'
author: microsoft
description: En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Para...
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 92c70297-4430-4e4e-919a-9c2333a8d09a
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-4-make-the-application-loosely-coupled-vb
msc.type: authoredcontent
ms.openlocfilehash: 422c75406d9c08279d0c2224ee4b6db3a71eb1b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78470227"
---
# <a name="iteration-4--make-the-application-loosely-coupled-vb"></a>#4 de iteración: hacer que la aplicación esté acoplada de forma flexible (VB)

por [Microsoft](https://github.com/microsoft)

[Descargar código](iteration-4-make-the-application-loosely-coupled-vb/_static/contactmanager_4_vb1.zip)

> En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y el patrón de inserción de dependencias.

## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Creación de una aplicación ASP.NET MVC de administración de contactos (VB)

En esta serie de tutoriales, creamos una aplicación de administración de contactos completa de principio a fin. La aplicación Contact Manager le permite almacenar información de contacto: nombres, números de teléfono y direcciones de correo electrónico, para obtener una lista de personas.

Compilamos la aplicación en varias iteraciones. Con cada iteración, mejoramos gradualmente la aplicación. El objetivo de este enfoque de varias iteraciones es permitirle comprender la razón de cada cambio.

- #1 de iteración: cree la aplicación. En la primera iteración, se crea el administrador de contactos de la manera más sencilla posible. Agregamos compatibilidad con las operaciones básicas de base de datos: crear, leer, actualizar y eliminar (CRUD).

- #2 de iteración: haga que la aplicación tenga un buen aspecto. En esta iteración, mejoramos la apariencia de la aplicación modificando la página maestra de vista de MVC predeterminada de ASP.NET y la hoja de estilos en cascada.

- #3 de iteración: agregar validación de formulario. En la tercera iteración, agregamos la validación de formulario básica. Impedimos que los usuarios envíen un formulario sin completar los campos de formulario necesarios. También validamos las direcciones de correo electrónico y los números de teléfono.

- #4 de iteración: hacer que la aplicación esté acoplada de forma flexible. En esta cuarta iteración, se aprovechan varios patrones de diseño de software para facilitar el mantenimiento y la modificación de la aplicación Contact Manager. Por ejemplo, se refactoriza nuestra aplicación para usar el patrón de repositorio y el patrón de inserción de dependencias.

- #5 de iteración: crear pruebas unitarias. En la quinta iteración, se facilita el mantenimiento y la modificación de la aplicación mediante la adición de pruebas unitarias. Hemos simulado nuestras clases de modelo de datos y compilamos pruebas unitarias para nuestros controladores y la lógica de validación.

- #6 de iteración: Use el desarrollo controlado por pruebas. En esta sexta iteración, agregamos una nueva funcionalidad a nuestra aplicación escribiendo primero las pruebas unitarias y escribiendo código en las pruebas unitarias. En esta iteración, se agregan grupos de contactos.

- #7 de iteración: agregar funcionalidad de Ajax. En la séptima iteración, se mejora la capacidad de respuesta y el rendimiento de la aplicación mediante la adición de compatibilidad con Ajax.

## <a name="this-iteration"></a>Esta iteración

En esta cuarta iteración de la aplicación Contact Manager, se refactoriza la aplicación para que la aplicación se acople de manera más flexible. Cuando una aplicación está acoplada de forma flexible, puede modificar el código en una parte de la aplicación sin necesidad de modificar el código en otras partes de la aplicación. Las aplicaciones de acoplamiento flexible son más resistentes a los cambios.

Actualmente, todo el acceso a datos y la lógica de validación utilizados por la aplicación Contact Manager se encuentran en las clases de controlador. Esta es una buena idea. Siempre que necesite modificar una parte de la aplicación, corre el riesgo de introducir errores en otra parte de la aplicación. Por ejemplo, si modifica la lógica de validación, se arriesga a introducir nuevos errores en el acceso a datos o la lógica del controlador.

> [!NOTE] 
> 
> (SRP), una clase nunca debe tener más de un motivo para cambiar. La combinación de controlador, validación y lógica de base de datos es una infracción masiva del principio de responsabilidad única.

Hay varias razones por las que puede que tenga que modificar la aplicación. Es posible que tenga que agregar una nueva característica a la aplicación, es posible que necesite corregir un error en la aplicación o que tenga que modificar el modo en que se implementa una característica de la aplicación. Las aplicaciones rara vez son estáticas. Tienden a crecer y mutar con el tiempo.

Imagine, por ejemplo, que decide cambiar el modo en que implementa la capa de acceso a datos. En este momento, la aplicación Contact Manager utiliza Microsoft Entity Framework para tener acceso a la base de datos. Sin embargo, puede decidir migrar a una tecnología de acceso a datos nueva o alternativa, como ADO.NET Data Services o NHibernate. Sin embargo, dado que el código de acceso a datos no está aislado del código de la validación y del controlador, no hay ninguna manera de modificar el código de acceso a datos en la aplicación sin modificar otro código que no esté directamente relacionado con el acceso a los datos.

Por otro lado, cuando una aplicación se acopla de manera flexible, puede realizar cambios en una parte de una aplicación sin tocar otras partes de una aplicación. Por ejemplo, puede cambiar las tecnologías de acceso a datos sin modificar la lógica de validación o del controlador.

En esta iteración, se aprovechan varios patrones de diseño de software que nos permiten refactorizar nuestra aplicación Contact Manager en una aplicación más acoplada. Cuando hayamos terminado, el administrador de contactos ganará todo lo que no hacía antes. Sin embargo, podremos cambiar la aplicación más fácilmente en el futuro.

> [!NOTE] 
> 
> La refactorización es el proceso de reescritura de una aplicación de tal manera que no pierde ninguna funcionalidad existente.

## <a name="using-the-repository-software-design-pattern"></a>Uso del modelo de diseño de software del repositorio

Nuestro primer cambio es aprovechar un patrón de diseño de software denominado patrón de repositorio. Usaremos el patrón de repositorio para aislar el código de acceso a datos del resto de la aplicación.

Para implementar el patrón de repositorio, es necesario completar los dos pasos siguientes:

1. Creación de una interfaz
2. Cree una clase concreta que implemente la interfaz.

En primer lugar, es necesario crear una interfaz que describa todos los métodos de acceso a datos que es necesario realizar. La interfaz IContactManagerRepository se encuentra en la lista 1. En esta interfaz se describen cinco métodos: CreateContact (), DeleteContact (), EditContact (), GetContact y ListContacts ().

**Lista 1-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample1.vb)]

A continuación, es necesario crear una clase concreta que implementa la interfaz IContactManagerRepository. Como usamos Microsoft Entity Framework para tener acceso a la base de datos, crearemos una nueva clase denominada EntityContactManagerRepository. Esta clase se incluye en la lista 2.

**Lista 2-Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample2.vb)]

Observe que la clase EntityContactManagerRepository implementa la interfaz IContactManagerRepository. La clase implementa los cinco métodos descritos por esa interfaz.

Es posible que se pregunte por qué debemos molestarnos con una interfaz. ¿Por qué es necesario crear una interfaz y una clase que la implemente?

Con una excepción, el resto de la aplicación interactuará con la interfaz y no con la clase concreta. En lugar de llamar a los métodos expuestos por la clase EntityContactManagerRepository, llamaremos a los métodos expuestos por la interfaz IContactManagerRepository.

De este modo, podemos implementar la interfaz con una clase nueva sin necesidad de modificar el resto de la aplicación. Por ejemplo, en una fecha futura, es posible que deseemos implementar una clase DataServicesContactManagerRepository que implemente la interfaz IContactManagerRepository. La clase DataServicesContactManagerRepository puede usar ADO.NET Data Services para tener acceso a una base de datos en lugar de a la Entity Framework de Microsoft.

Si el código de aplicación se programa para la interfaz IContactManagerRepository en lugar de la clase EntityContactManagerRepository concreta, podemos cambiar las clases concretas sin modificar el resto del código. Por ejemplo, podemos cambiar de la clase EntityContactManagerRepository a la clase DataServicesContactManagerRepository sin modificar la lógica de validación o de acceso a los datos.

La programación de interfaces (abstracciones) en lugar de clases concretas hace que la aplicación sea más resistente a los cambios.

> [!NOTE] 
> 
> Puede crear rápidamente una interfaz desde una clase concreta dentro de Visual Studio seleccionando la opción de menú refactorizar, extraer interfaz. Por ejemplo, puede crear la clase EntityContactManagerRepository en primer lugar y después usar Extract interface para generar la interfaz IContactManagerRepository automáticamente.

## <a name="using-the-dependency-injection-software-design-pattern"></a>Usar el modelo de diseño de software de inserción de dependencias

Ahora que hemos migrado nuestro código de acceso a datos a una clase de repositorio independiente, es necesario modificar nuestro controlador de contacto para usar esta clase. Sacaremos provecho de un patrón de diseño de software denominado inyección de dependencias para usar la clase de repositorio en nuestro controlador.

El controlador de contactos modificado está incluido en la lista 3.

**Lista 3: Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample3.vb)]

Observe que el controlador de contactos de la lista 3 tiene dos constructores. El primer constructor pasa una instancia concreta de la interfaz IContactManagerRepository al segundo constructor. La clase de controlador de contacto utiliza la *inserción de dependencias de constructor*.

El primero es el lugar en el que se usa la clase EntityContactManagerRepository. El resto de la clase usa la interfaz IContactManagerRepository en lugar de la clase EntityContactManagerRepository concreta.

Esto facilita el cambio de las implementaciones de la clase IContactManagerRepository en el futuro. Si desea utilizar la clase DataServicesContactRepository en lugar de la clase EntityContactManagerRepository, solo tiene que modificar el primer constructor.

La inserción de dependencias de constructor también hace que la clase de controlador de contactos sea muy comprobable. En las pruebas unitarias, puede crear una instancia del controlador de contactos pasando una implementación simulada de la clase IContactManagerRepository. Esta característica de la inserción de dependencias será muy importante para nosotros en la siguiente iteración cuando se compilan pruebas unitarias para la aplicación Contact Manager.

> [!NOTE] 
> 
> Si desea desacoplar por completo la clase de controlador de contactos de una implementación concreta de la interfaz IContactManagerRepository, puede aprovechar las ventajas de un marco de trabajo que admite la inserción de dependencias, como StructureMap o Microsoft. Entity Framework (MEF). Al aprovechar el marco de inserción de dependencias, nunca es necesario hacer referencia a una clase concreta en el código.

## <a name="creating-a-service-layer"></a>Creación de una capa de servicio

Es posible que haya observado que la lógica de validación se sigue mezclando con nuestra lógica del controlador en la clase de controlador modificada en la lista 3. Por la misma razón que es una buena idea aislar nuestra lógica de acceso a datos, es una buena idea aislar la lógica de validación.

Para corregir este problema, podemos crear un nivel de [servicio](http://martinfowler.com/eaaCatalog/serviceLayer.html)independiente. El nivel de servicio es una capa independiente que se puede insertar entre las clases del controlador y del repositorio. El nivel de servicio contiene nuestra lógica de negocios, incluida toda la lógica de validación.

El ContactManagerService se encuentra en la lista 4. Contiene la lógica de validación de la clase de controlador de contactos.

**Lista 4: Models\ContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample4.vb)]

Tenga en cuenta que el constructor de ContactManagerService requiere un ValidationDictionary. El nivel de servicio se comunica con la capa de controlador a través de este ValidationDictionary. En la sección siguiente se describe el ValidationDictionary en la sección siguiente cuando se describe el patrón de Decorator.

Tenga en cuenta, además, que el ContactManagerService implementa la interfaz IContactManagerService. Siempre debe procurar programar con interfaces en lugar de clases concretas. Otras clases de la aplicación Contact Manager no interactúan directamente con la clase ContactManagerService. En su lugar, con una excepción, el resto de la aplicación Contact Manager se programa en la interfaz IContactManagerService.

La interfaz IContactManagerService se encuentra en la lista 5.

**Lista 5-Models\IContactManagerService.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample5.vb)]

La clase de controlador de contacto modificada se incluye en la lista 6. Tenga en cuenta que el controlador de contacto ya no interactúa con el repositorio de ContactManager. En su lugar, el controlador de contactos interactúa con el servicio ContactManager. Cada capa está aislada tanto como sea posible de otras capas.

**Listado 6-Controllers\ContactController.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample6.vb)]

Nuestra aplicación ya no ejecuta afoul del principio de responsabilidad única (SRP). El controlador de contactos de la lista 6 se ha quitado de cada responsabilidad distinta de controlar el flujo de ejecución de la aplicación. Toda la lógica de validación se ha quitado del controlador de contacto y se ha insertado en el nivel de servicio. Toda la lógica de la base de datos se ha insertado en la capa del repositorio.

## <a name="using-the-decorator-pattern"></a>Usar el patrón de Decorator

Queremos poder desacoplar completamente nuestro nivel de servicio de nuestra capa de controlador. En principio, deberíamos poder compilar la capa de servicio en un ensamblado independiente de la capa del controlador sin necesidad de agregar una referencia a nuestra aplicación MVC.

Sin embargo, nuestro nivel de servicio debe ser capaz de devolver mensajes de error de validación a la capa de controlador. ¿Cómo podemos habilitar el nivel de servicio para comunicar mensajes de error de validación sin necesidad de acoplar el controlador y el nivel de servicio? Podemos aprovechar un patrón de diseño de software denominado patrón de [decorador](http://en.wikipedia.org/wiki/Decorator_pattern).

Un controlador usa un ModelStateDictionary denominado ModelState para representar los errores de validación. Por lo tanto, es posible que se sienta tentado a pasar ModelState desde la capa del controlador a la capa de servicio. Sin embargo, el uso de ModelState en el nivel de servicio haría que el nivel de servicio dependía de una característica del marco ASP.NET MVC. Esto sería incorrecto porque, en algún día, podría querer usar el nivel de servicio con una aplicación WPF en lugar de una aplicación ASP.NET MVC. En ese caso, no desearía hacer referencia al marco de MVC de ASP.NET para usar la clase ModelStateDictionary.

El patrón Decorator permite ajustar una clase existente en una nueva clase para implementar una interfaz. Nuestro proyecto de Contact Manager incluye la clase ModelStateWrapper incluida en la lista 7. La clase ModelStateWrapper implementa la interfaz en la lista 8.

**Listado 7-Models\Validation\ModelStateWrapper.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample7.vb)]

**Lista 8-Models\Validation\IValidationDictionary.vb**

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample8.vb)]

Si echa un vistazo a la lista 5, verá que el nivel de servicio ContactManager usa la interfaz IValidationDictionary exclusivamente. El servicio ContactManager no depende de la clase ModelStateDictionary. Cuando el controlador de contactos crea el servicio ContactManager, el controlador ajusta su ModelState de la siguiente manera:

[!code-vb[Main](iteration-4-make-the-application-loosely-coupled-vb/samples/sample9.vb)]

## <a name="summary"></a>Resumen

En esta iteración, no se ha agregado ninguna funcionalidad nueva a la aplicación Contact Manager. El objetivo de esta iteración era refactorizar la aplicación Contact Manager para que sea más fácil de mantener y modificar.

En primer lugar, hemos implementado el modelo de diseño de software de repositorio. Hemos migrado todo el código de acceso a datos a una clase de repositorio de ContactManager independiente.

También hemos aislado nuestra lógica de validación de nuestra lógica del controlador. Hemos creado un nivel de servicio independiente que contiene todo el código de validación. La capa de controlador interactúa con el nivel de servicio y el nivel de servicio interactúa con la capa de repositorio.

Cuando creamos el nivel de servicio, aprovechamos el patrón de decorador para aislar la ModelState de nuestro nivel de servicio. En nuestro nivel de servicio, programamos en la interfaz IValidationDictionary en lugar de ModelState.

Por último, aprovechamos un patrón de diseño de software denominado patrón de inserción de dependencias. Este patrón nos permite programar contra interfaces (abstracciones) en lugar de clases concretas. La implementación del modelo de diseño de inyección de dependencia también hace que nuestro código sea más comprobable. En la iteración siguiente, se agregan pruebas unitarias a nuestro proyecto.

> [!div class="step-by-step"]
> [Anterior](iteration-3-add-form-validation-vb.md)
> [Siguiente](iteration-5-create-unit-tests-vb.md)
