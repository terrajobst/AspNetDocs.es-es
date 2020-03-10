---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Referencia rápida de la API de ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: Esta página contiene una lista con ejemplos breves de los objetos, propiedades y métodos más utilizados para programar ASP.NET Web Pages con sintaxis Razor.
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: e010307fc0576e8b003fbfe665cae77618d9c9a5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78463585"
---
# <a name="aspnet-web-pages-razor-api-quick-reference"></a>Referencia rápida de la API de ASP.NET Web Pages (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta página contiene una lista con ejemplos breves de los objetos, propiedades y métodos más utilizados para programar ASP.NET Web Pages con sintaxis Razor.
> 
> Las descripciones marcadas con "(V2)" se introdujeron en ASP.NET Web Pages versión 2.
> 
> Para obtener documentación de referencia de API, consulte la [documentación de referencia de ASP.NET Web pages](https://go.microsoft.com/fwlink/?LinkId=208659) en MSDN.
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y ASP.NET Web Pages 1,0 (excepto para las características marcadas como V2).

Esta página contiene información de referencia para lo siguiente:

- [Clases](#Classes)
- [Data](#Data)
- [Aplicaciones auxiliares](#Helpers)
- [Validación](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Clases

### `AppState[key], AppState[index],App`

Contiene datos que se pueden compartir con cualquier página de la aplicación. Puede usar la propiedad `App` dinámica para tener acceso a los mismos datos, como en el ejemplo siguiente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Convierte un valor de cadena en un valor booleano (true/false). Devuelve false o el valor especificado si la cadena no representa true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Convierte un valor de cadena en una fecha y hora. Devuelve `DateTime.MinValue` o el valor especificado si la cadena no representa una fecha y hora.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Convierte un valor de cadena en un valor decimal. Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Convierte un valor de cadena en un valor de tipo float. Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Convierte un valor de cadena en un entero. Devuelve 0 o el valor especificado si la cadena no representa un entero.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crea una dirección URL compatible con el explorador a partir de una ruta de acceso de archivo local, con partes de ruta de acceso adicionales opcionales.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Representa el *valor* como marcado HTML en lugar de representarlo como salida codificada en HTML.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Devuelve true si el valor se puede convertir de una cadena al tipo especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Devuelve true si el objeto o la variable no tiene ningún valor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Devuelve true si la solicitud es una publicación. (Las solicitudes iniciales suelen ser GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Especifica la ruta de acceso de una página de diseño que se va a aplicar a esta página.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contiene los datos compartidos entre la página, las páginas de diseño y las páginas parciales de la solicitud actual. Puede usar la propiedad `Page` dinámica para tener acceso a los mismos datos, como en el ejemplo siguiente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Páginas de diseño) Representa el contenido de una página de contenido que no está en ninguna sección con nombre.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Representa una página de contenido mediante la ruta de acceso especificada y datos adicionales opcionales. Puede obtener los valores de los parámetros adicionales de `PageData` por posición (ejemplo 1) o clave (ejemplo 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Páginas de diseño) Representa una sección de contenido que tiene un nombre. Establezca *requerido* en false para que una sección sea opcional.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Obtiene o establece el valor de una cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Obtiene los archivos que se cargaron en la solicitud actual.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Obtiene los datos que se publicaron en un formulario (como cadenas). `Request[key]` comprueba las colecciones `Request.Form` y `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Obtiene los datos que se especificaron en la cadena de consulta de la dirección URL. `Request[key]` comprueba las colecciones `Request.Form` y `Request.QueryString`.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Deshabilita de forma selectiva la validación de solicitudes para un elemento de formulario, un valor de cadena de consulta, una cookie o un valor de encabezado. La validación de solicitudes está habilitada de forma predeterminada e impide que los usuarios publiquen marcado u otro contenido potencialmente peligroso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Agrega un encabezado de servidor HTTP a la respuesta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Almacena en caché el resultado de la página durante un tiempo especificado. Opcionalmente, establezca la *variable* para restablecer el tiempo de espera en cada acceso de página y *VaryByParams* para almacenar en caché diferentes versiones de la página para cada cadena de consulta diferente en la solicitud de la página.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Redirige la solicitud del explorador a una nueva ubicación.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Establece el código de Estado HTTP que se envía al explorador.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Escribe el contenido de los *datos* en la respuesta con un tipo MIME opcional.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Escribe el contenido de un archivo en la respuesta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Páginas de diseño) Define una sección de contenido que tiene un nombre.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Descodifica una cadena codificada en HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codifica una cadena para la representación en formato HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Devuelve la ruta de acceso física del servidor para la ruta de acceso virtual especificada.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Descodifica texto desde una dirección URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codifica el texto que se va a colocar en una dirección URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Obtiene o establece un valor que existe hasta que el usuario cierra el explorador.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Muestra una representación de cadena del valor del objeto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Obtiene datos adicionales de la dirección URL (por ejemplo, */MYPAGE/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Cambia la contraseña del usuario especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Confirma una cuenta mediante el token de confirmación de la cuenta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crea una nueva cuenta de usuario con el nombre de usuario y la contraseña especificados. Para requerir un token de confirmación, pase true para *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Obtiene el identificador entero para el usuario que ha iniciado sesión actualmente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Obtiene el nombre del usuario que ha iniciado sesión actualmente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Genera un token de restablecimiento de contraseña que se puede enviar por correo electrónico a un usuario para que el usuario pueda restablecer la contraseña.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Devuelve el ID. de usuario del nombre de usuario.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Devuelve true si el usuario actual ha iniciado sesión.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Devuelve true si el usuario se ha confirmado (por ejemplo, a través de un correo electrónico de confirmación).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Devuelve true si el nombre del usuario actual coincide con el nombre de usuario especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Inicia la sesión del usuario mediante el establecimiento de un token de autenticación en la cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Cierra la sesión del usuario quitando la cookie del token de autenticación.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Si el usuario no está autenticado, establece el estado de HTTP en 401 (No autorizado).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Si el usuario actual no es miembro de uno de los roles especificados, establece el estado de HTTP en 401 (no autorizado).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Si el usuario actual no es el usuario especificado por el *nombre*de usuario, establece el estado de HTTP en 401 (no autorizado).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Si el token de restablecimiento de contraseña es válido, cambia la contraseña del usuario a la nueva contraseña.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Datos

### `Database.Execute(SQLstatement [,parameters]`

Ejecuta *SQLstatement* (con parámetros opcionales) como INSERT, delete o Update y devuelve un recuento de los registros afectados.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Devuelve la columna de identidad a partir de la fila insertada más recientemente.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Abre el archivo de base de datos especificado o la base de datos especificada mediante una cadena de conexión con nombre del archivo *Web. config* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Abre una base de datos con la cadena de conexión. (Esto contrasta con `Database.Open`, que usa un nombre de cadena de conexión).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Consulta la base de datos mediante *SQLstatement* (opcionalmente, pasando parámetros) y devuelve los resultados como una colección.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un solo registro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un valor único.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Asistentes

### `Analytics.GetGoogleHtml(webPropertyId)`

Representa el código JavaScript de Google Analytics para el ID. especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Representa el código JavaScript de StatCounter Analytics para el proyecto especificado.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Representa el código JavaScript de Yahoo Analytics de la cuenta especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Pasa una búsqueda a Bing. Para especificar el sitio en el que se va a buscar y un título para el cuadro de búsqueda, puede establecer las propiedades `Bing.SiteUrl` y `Bing.SiteTitle`. Normalmente, estas propiedades se establecen en la página *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inicializa un gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Agrega una leyenda a un gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Agrega una serie de valores al gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Devuelve un valor hash para los datos especificados. El algoritmo predeterminado es `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Permite que los usuarios de Facebook realicen una conexión a las páginas.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Representa la interfaz de usuario para cargar archivos.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Representa la etiqueta de jugador de Xbox especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Representa la imagen de Gravatar para la dirección de correo electrónico especificada.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Convierte un objeto de datos en una cadena en el formato notación de objetos JavaScript (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Convierte una cadena de entrada codificada en JSON en un objeto de datos que se puede iterar o insertar en una base de datos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Representa los vínculos de redes sociales mediante el título especificado y la dirección URL opcional.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Asocia un mensaje de error a un campo de formulario. Utilice la aplicación auxiliar `ModelState` para tener acceso a este miembro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Asocia un mensaje de error a un formulario. Utilice la aplicación auxiliar `ModelState` para tener acceso a este miembro.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Devuelve true si no hay ningún error de validación. Utilice la aplicación auxiliar `ModelState` para tener acceso a este miembro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Representa las propiedades y los valores de un objeto y los objetos secundarios.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Representa la prueba de comprobación reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Establece claves públicas y privadas para el servicio reCAPTCHA. Normalmente, estas propiedades se establecen en la página *\_AppStart* .

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Devuelve el resultado de la prueba de reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Representa información de estado sobre ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Representa una secuencia de Twitter para el usuario especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Representa una secuencia de Twitter para el texto de búsqueda especificado.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Representa un reproductor de vídeo Flash para el archivo especificado con un ancho y un alto opcionales.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Representa un reproductor de Windows Media para el archivo especificado con el ancho y el alto opcionales.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Representa un reproductor de Silverlight para el archivo *. xap* especificado con el ancho y el alto requeridos.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Devuelve el objeto especificado por la *clave*o null si no se encuentra el objeto.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Quita el objeto especificado por la *clave* de la memoria caché.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Coloca el *valor* en la memoria caché con el nombre especificado por *key*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crea un nuevo objeto de `WebGrid` con los datos de una consulta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Representa el marcado para mostrar los datos en una tabla HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Representa un buscapersonas para el objeto `WebGrid`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Carga una imagen desde la ruta de acceso especificada.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Agrega la imagen especificada como una marca de agua.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Agrega el texto especificado a la imagen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Voltea la imagen horizontal o verticalmente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Carga una imagen cuando se envía una imagen a una página durante una carga de archivos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Cambia el tamaño de una imagen.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Gira la imagen a la izquierda o a la derecha.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Guarda la imagen en la ruta de acceso especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Establece la contraseña para el servidor SMTP. Normalmente, esta propiedad se establece en la página *\_AppStart* .

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Envía un mensaje de correo electrónico.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Establece el nombre del servidor SMTP. Normalmente, esta propiedad se establece en la página *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Establece el nombre de usuario del servidor SMTP. Normalmente, debe establecer esta propiedad en la página *\_AppStart* .

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validación

### `Html.ValidationMessage(field)`

2 Representa un mensaje de error de validación para el campo especificado.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

2 Muestra una lista de todos los errores de validación.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

2 Registra un elemento de entrada de usuario para el tipo de validación especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

2 Representa dinámicamente los atributos de clase CSS para la validación del lado cliente para que pueda dar formato a los mensajes de error de validación. (Requiere que se haga referencia a las bibliotecas de scripts de cliente adecuadas y que se definan las clases CSS).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

2 Habilita la validación del lado cliente para el campo de entrada del usuario. (Requiere que haga referencia a las bibliotecas de scripts de cliente adecuadas).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

2 Devuelve true si todos los elementos de entrada de usuario que se registran para la validación contienen valores válidos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

2 Especifica que los usuarios deben proporcionar un valor para el elemento de entrada del usuario.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

2 Especifica que los usuarios deben proporcionar valores para cada uno de los elementos de entrada del usuario. Este método no permite especificar un mensaje de error personalizado.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

2 Especifica una prueba de validación cuando se usa el método `Validation.Add`.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
