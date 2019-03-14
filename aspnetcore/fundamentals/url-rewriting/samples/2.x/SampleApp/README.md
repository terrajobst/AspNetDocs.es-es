---
ms.openlocfilehash: 4a4ad30846d1993b3e561c96c00fbf9a0c7f3929
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040812"
---
# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>Ejemplo de reescritura de dirección URL de ASP.NET Core (ASP.NET Core 2.x)

En este ejemplo se muestra el uso del software intermedio de reescritura de dirección URL de ASP.NET Core 2.x. En la aplicación se muestran las opciones de redireccionamiento y reescritura de dirección URL.

Al ejecutar el ejemplo, las respuestas que no son archivos devuelven la dirección URL reescrita o redirigida al aplicar una de las reglas a una dirección URL de solicitud. Para los ejemplos XML y de archivo de texto, el middleware de archivos estáticos sirve el archivo después de reescribir la dirección URL de solicitud.

## <a name="examples-in-this-sample"></a>Ejemplos que se incluyen

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Código de estado correcto: 302 (encontrado)
  - Ejemplo (redireccionamiento): **/redirect-rule/{capture_group}** a **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Código de estado correcto: 200 (CORRECTO)
  - Ejemplo (reescritura): **/rewrite-rule/{capture_group_1}/{capture_group_2}** a **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Código de estado correcto: 302 (encontrado)
  - Ejemplo (redireccionamiento): **/apache-mod-rules-redirect/{capture_group}** a **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Código de estado correcto: 200 (CORRECTO)
  - Ejemplo (reescritura): **/iis-rules-rewrite/{capture_group}** a **/rewritten?id={capture_group}**
* `Add(RedirectXmlFileRequests)`
  - Código de estado correcto: 301 (movido definitivamente)
  - Ejemplo (redireccionamiento): **/file.xml** a **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - Código de estado correcto: 200 (CORRECTO)
  - Ejemplo (reescritura): **/un_archivo.txt** por **/archivo.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Código de estado correcto: 301 (movido definitivamente)
  - Ejemplo (redireccionamiento): **/image.png** a **/png-images/image.png**
  - Ejemplo (redireccionamiento): **/image.jpg** a **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>Uso de PhysicalFileProvider

También puede obtener `IFileProvider` mediante la creación de un `PhysicalFileProvider` que se pasará a los métodos `AddApacheModRewrite()` y `AddIISUrlRewrite()`:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Extensiones de redireccionamiento seguro

En este ejemplo se incluye la configuración de `WebHostBuilder` para que la aplicación use direcciones URL (`https://localhost:5001`, `https://localhost`) y un certificado de prueba (*testCert.pfx*) para ayudar a explorar los métodos de redireccionamiento seguro. Si el servidor ya tiene el puerto 443 asignado o está en uso, el ejemplo `https://localhost` no funciona; quite el elemento `ListenOptions` para el puerto 443 en el método `CreateWebHostBuilder` del archivo *Program.cs* o desenlace el puerto 443 en el servidor para que Kestrel pueda usarlo.

| Método                           | Código de estado |    Puerto    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
