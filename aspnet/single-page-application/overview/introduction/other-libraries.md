---
uid: single-page-application/overview/introduction/other-libraries
title: ¿Conoce una biblioteca distinta de Knockout? | Microsoft Docs
author: madskristensen
description: ¿Conoce una biblioteca distinta de Knockout?
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 64a4ad1fb411f7291a5cba634afdf4d2fdb16d55
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467197"
---
# <a name="know-a-library-other-than-knockout"></a>¿Conoce una biblioteca distinta de Knockout?

por [Mads Kristensen](https://github.com/madskristensen)

La [plantilla de aplicación de una sola página (Spa)](knockoutjs-template.md) es una excelente manera de empezar a escribir aplicaciones de una sola página. La plantilla usa [KnockoutJS](http://knockoutjs.com/) para enlazar los datos de la aplicación a los elementos DOM.

Pero el knockout no es la única biblioteca de JavaScript para crear aplicaciones cliente enriquecidas. Otras bibliotecas resuelven desafíos similares de maneras diferentes. Podría preferir una biblioteca por encima de otra, por lo que hemos hecho que varias plantillas creadas por la comunidad estén disponibles para su descarga. Cada una de estas plantillas utiliza una combinación diferente de bibliotecas de JavaScript de cliente.

Para instalar una plantilla creada por la comunidad, visite una de las páginas de la plantilla que se enumeran a continuación y haga clic en el botón Descargar. Las plantillas se proporcionan como archivos VSIX.

## <a name="backbonejs"></a>BackboneJS

[Plantilla de red troncal. js Spa](../templates/backbonejs-template.md). Esta plantilla proporciona un esqueleto inicial para desarrollar una aplicación [backbone. js](http://backbonejs.org/) en ASP.NET MVC. De forma integrada, proporciona funcionalidad básica de inicio de sesión de usuario, como registro de usuario, Inicio de sesión, restablecimiento de contraseña y confirmación de usuario con plantillas de correo electrónico básicas.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) es una biblioteca de código abierto para administrar datos enriquecidos en un cliente de JavaScript. Controla con rapidez las consultas, el almacenamiento en caché, el seguimiento de cambios, la validación, etc. Dos plantillas de características con rapidez:

- La plantilla de gran [o cobertura](../templates/breezeknockout-template.md) amplía la plantilla de Spa para mostrar la facilidad con la que puede compilar una aplicación de una sola página con rapidez para la administración de datos y KnockoutJS para el enlace de datos.
- La plantilla de más [o menos angular](../templates/breezeangular-template.md) también amplía la plantilla de spa con rapidez, pero el uso de la biblioteca [AngularJS](http://angularjs.org) para el enlace de datos, la inserción de dependencias y la administración de la pantalla.

Además, la [plantilla de Hot paño Spa](../templates/hottowel-template.md) usa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Plantilla EMBERJS Spa](../templates/emberjs-template.md). Esta plantilla usa [Ember](http://emberjs.com/), una eficaz biblioteca de JavaScript para MVC que resuelve una amplia gama de desafíos para la creación de aplicaciones cliente enriquecidas.

La plantilla Ember SPA es una reimplementación de la plantilla de SPA de la cobertura, con plantillas de EmberJS y manillar.

## <a name="hot-towel"></a>Paño de acceso frecuente

[Plantilla](../templates/hottowel-template.md)de el paño de Hot-Spa. Esta plantilla incorpora varias bibliotecas de JavaScript, como pan, Knockout, RequireJS y Twitter bootstrap.

En comparación con las otras plantillas que se muestran aquí, la plantilla de paños calientes proporciona una aplicación más completa desde la que puede crear la suya propia. Hay más conceptos que se deben tener en cuenta, pero una vez que los comprenda, es posible que esta plantilla solo sea lo que busca. Si desea crear un SPA pero no puede decidir por dónde empezar, use el paño de acceso frecuente y, en segundos, tendrá un SPA y todas las herramientas que necesita para compilar en él.

## <a name="feature-table"></a>Tabla de características

Estas son las características proporcionadas por cada plantilla de SPA:

|                        | ASP.NET SPA | Red troncal | Pan/angular | Breeze/KO |  Ember   | Paño de acceso frecuente |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Ejemplo ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Plantilla sin sistema operativo      |             | &#10003; |                |           |          | &#10003;  |
| Navegación e historial |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotecas       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Red troncal     |             | &#10003; |                |           |          |           |
|         Enorme         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Cobertura        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |
