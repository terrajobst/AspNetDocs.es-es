---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Solución de problemas de aplicaciones web API2 que funcionan en Visual Studio y producen errores en un servidor IIS de producción
author: rmcmurray
description: Solución de problemas de aplicaciones web API2 que funcionan en Visual Studio y producen errores en un servidor IIS de producción
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 1b47f1ade3619cfd010260352f6a96985ab3598b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447031"
---
# <a name="troubleshoot-web-api2-apps-that-work-in-visual-studio-and-fail-on-a-production-iis-server"></a>Solución de problemas de aplicaciones web API2 que funcionan en Visual Studio y producen errores en un servidor IIS de producción

> Este documento explica cómo solucionar problemas de las aplicaciones web API2 que se implementan en un servidor IIS de producción. Trata los errores comunes HTTP 405 y 501.
> 
> ## <a name="software-used-in-this-tutorial"></a>Software usado en este tutorial
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (versión 7 o posterior)
> - [API Web](../../index.md) 

Las aplicaciones de API Web suelen usar varios verbos HTTP: GET, POST, PUT, DELETE y, en ocasiones, PATCH. Dicho esto, los desarrolladores pueden encontrarse en situaciones en las que otro módulo de IIS implementa esos verbos en el servidor IIS de producción, lo que conduce a una situación en la que un controlador de API Web que funciona correctamente en Visual Studio o en un servidor de desarrollo Devuelve un error HTTP 405 cuando se implementa en un servidor IIS de producción.

## <a name="what-causes-http-405-errors"></a>Qué provoca errores HTTP 405

El primer paso para aprender a solucionar los errores de HTTP 405 es entender qué significa un error HTTP 405 en realidad. El documento principal para HTTP es [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), que define el código de estado http 405 como ***método no permitido***y describe con mayor profundidad este código de estado como una situación en la que no se permite &quot;el método especificado en la línea de solicitud para el recurso identificado por el URI de solicitud.&quot; en otras palabras, no se permite el verbo HTTP para la dirección URL específica que un cliente HTTP ha solicitado.

Como una breve revisión, estos son algunos de los métodos HTTP más usados, tal como se define en RFC 2616, RFC 4918 y RFC 5789:

| Método HTTP | Description |
| --- | --- |
| **GET** | Este método se usa para recuperar datos de un URI y probablemente es el método HTTP más utilizado. |
| **HEAD** | Este método es muy similar al método GET, salvo que realmente no recupera los datos del URI de solicitud; simplemente recupera el Estado HTTP. |
| **POST** | Este método se utiliza normalmente para enviar datos nuevos al URI; POST se suele usar para enviar datos de formulario. |
| **PUT** | Este método se utiliza normalmente para enviar datos sin procesar al URI; PUT se usa a menudo para enviar datos JSON o XML a aplicaciones de API Web. |
| **DELETE** | Este método se usa para quitar datos de un URI. |
| **OPTIONS** | Este método se usa normalmente para recuperar la lista de métodos HTTP que se admiten para un URI. |
| **COPIAR MOVIMIENTO** | Estos dos métodos se usan con WebDAV y su finalidad es autoexplicativo. |
| **MKCOL** | Este método se utiliza con WebDAV y se usa para crear una colección (por ejemplo, un directorio) en el URI especificado. |
| **PROPFIND PROPPATCH** | Estos dos métodos se usan con WebDAV y se usan para consultar o establecer las propiedades de un URI. |
| **DESBLOQUEO DE BLOQUEOS** | Estos dos métodos se usan con WebDAV y se usan para bloquear o desbloquear el recurso identificado por el URI de solicitud al crear. |
| **PATCH** | Este método se utiliza para modificar un recurso HTTP existente. |

Cuando uno de estos métodos HTTP está configurado para su uso en el servidor, el servidor responderá con el Estado HTTP y otros datos adecuados para la solicitud. (Por ejemplo, un método GET podría recibir una respuesta HTTP 200 ***OK*** y un método put podría recibir una respuesta ***http 201)*** .

Si el método HTTP no está configurado para su uso en el servidor, el servidor responderá con un error HTTP 501 ***no implementado*** .

Sin embargo, cuando se configura un método HTTP para su uso en el servidor, pero se ha deshabilitado para un URI determinado, el servidor responderá con un error del método HTTP 405 ***no permitido*** .

## <a name="example-http-405-error"></a>Error HTTP 405 de ejemplo

En el siguiente ejemplo de solicitud y respuesta HTTP se ilustra una situación en la que un cliente HTTP está intentando poner valor en una aplicación de API Web en un servidor Web y el servidor devuelve un error HTTP que indica que no se permite el método PUT:

Solicitud HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]

Respuesta HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]

En este ejemplo, el cliente HTTP envió una solicitud JSON válida a la dirección URL de una aplicación de API Web en un servidor Web, pero el servidor devolvió un mensaje de error HTTP 405 que indica que no se permitió el método PUT para la dirección URL. Por el contrario, si el URI de solicitud no coincidía con una ruta para la aplicación de API Web, el servidor devolvería un error HTTP 404 ***no encontrado*** .

## <a name="resolve-http-405-errors"></a>Resolver errores HTTP 405

Hay varios motivos por los que no se permite un verbo HTTP específico, pero hay un escenario principal que es la causa inicial de este error en IIS: se han definido varios controladores para el mismo verbo o método, y uno de los controladores está bloqueando el controlador esperado para procesar la solicitud. A modo de explicación, IIS procesa los controladores de primero a último en función de las entradas del controlador de pedidos en los archivos *ApplicationHost. config* y *Web. config* , donde se usará la primera combinación de ruta de acceso, verbo, recurso, etc., para controlar la solicitud.

El ejemplo siguiente es un extracto de un archivo *ApplicationHost. config* para un servidor IIS que devolvió un error http 405 al usar el método put para enviar datos a una aplicación de API Web. En este extracto, se definen varios controladores HTTP y cada controlador tiene un conjunto diferente de métodos HTTP para los que está configurado: la última entrada de la lista es el controlador de contenido estático, que es el controlador predeterminado que se usa después de que los otros controladores hayan tenido un chanc e para examinar la solicitud:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

En el ejemplo anterior, el controlador WebDAV y el controlador de URL sin extensión para ASP.NET (que se usa para la API Web) se definen claramente para listas independientes de métodos HTTP. Tenga en cuenta que el controlador de DLL ISAPI está configurado para todos los métodos HTTP, aunque esta configuración no producirá necesariamente un error. Sin embargo, las opciones de configuración como esta deben tenerse en cuenta al solucionar los errores HTTP 405.

En el ejemplo anterior, el controlador de DLL de ISAPI no era el problema; de hecho, el problema no se definió en el archivo *ApplicationHost. config* para el servidor IIS: el problema se debe a una entrada realizada en el archivo *Web. config* cuando se creó la aplicación de API Web en Visual Studio. En el siguiente fragmento del archivo *Web. config* de la aplicación se muestra la ubicación del problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

En este extracto, el controlador de URL sin extensión para ASP.NET se ha redefinido para incluir métodos HTTP adicionales que se usarán con la aplicación de API Web. Sin embargo, puesto que se define un conjunto similar de métodos HTTP para el controlador WebDAV, se produce un conflicto. En este caso concreto, IIS define y carga el controlador WebDAV, aunque WebDAV está deshabilitado para el sitio web que incluye la aplicación de API Web. Durante el procesamiento de una solicitud HTTP PUT, IIS llama al módulo WebDAV, ya que se define para el verbo PUT. Cuando se llama al módulo WebDAV, comprueba su configuración y ve que está deshabilitado, por lo que devolverá un error de método HTTP 405 ***no permitido*** para cualquier solicitud que se parezca a una solicitud de WebDAV. Para resolver este problema, debe quitar WebDAV de la lista de módulos HTTP para el sitio web en el que se define la aplicación de API Web. En el ejemplo siguiente se muestra lo que podría ser similar a:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Este escenario se suele encontrar después de publicar una aplicación desde un entorno de desarrollo a un entorno de producción de IIS y esto se debe a que la lista de controladores o módulos es diferente entre los entornos de desarrollo y de producción. Por ejemplo, si usa Visual Studio 2012 o una versión posterior para desarrollar una aplicación de API Web, IIS Express es el servidor Web predeterminado para las pruebas. Este servidor Web de desarrollo es una versión reducida de la funcionalidad de IIS completa que se incluye en un producto de servidor, y este servidor Web de desarrollo contiene algunos cambios que se agregaron para los escenarios de desarrollo. Por ejemplo, el módulo WebDAV suele instalarse en un servidor Web de producción que ejecuta la versión completa de IIS, aunque es posible que no esté en uso. La versión de desarrollo de IIS (IIS Express) instala el módulo WebDAV, pero las entradas del módulo WebDAV se marcan como comentario intencionadamente, por lo que el módulo WebDAV nunca se carga en IIS Express a menos que se modifique específicamente la configuración de IIS Express configuración para agregar funcionalidad de WebDAV a la instalación de IIS Express. Como resultado, la aplicación web puede funcionar correctamente en el equipo de desarrollo, pero puede encontrar errores HTTP 405 al publicar la aplicación de API Web en el servidor Web de producción de IIS.

## <a name="http-501-errors"></a>Errores HTTP 501

* Indica que la funcionalidad específica no se ha implementado en el servidor.
* Normalmente significa que no hay ningún controlador definido en la configuración de IIS que coincida con la solicitud HTTP:
  * Probablemente indica que algo no se instaló correctamente en IIS o
  * Algo ha modificado la configuración de IIS para que no se hayan definido controladores que admitan el método HTTP específico.

Para resolver ese problema, debe volver a instalar cualquier aplicación que esté intentando usar un método HTTP para el que no tiene definiciones de módulo o de controlador correspondientes.

## <a name="summary"></a>Resumen

Los errores HTTP 405 se producen cuando un servidor Web no permite un método HTTP para una dirección URL solicitada. Esta condición suele considerarse cuando se ha definido un controlador determinado para un verbo específico y dicho controlador está invalidando el controlador que se espera que procese la solicitud.
