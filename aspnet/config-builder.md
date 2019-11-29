---
uid: config-builder
title: Generadores de configuración para ASP.NET
author: rick-anderson
description: Obtenga información sobre cómo obtener datos de configuración de orígenes distintos de los valores de Web. config de orígenes externos.
ms.author: riande
ms.date: 10/29/2018
msc.type: content
ms.openlocfilehash: 5299d9ab057c3096773955a7461e77a80673ebfe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586766"
---
# <a name="configuration-builders-for-aspnet"></a>Generadores de configuración para ASP.NET

Por [Stephen Molloy](https://github.com/StephenMolloy) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Los generadores de configuración proporcionan un mecanismo moderno y ágil para que las aplicaciones de ASP.NET obtengan valores de configuración de orígenes externos.

Generadores de configuración:

* Están disponibles en .NET Framework 4.7.1 y versiones posteriores.
* Proporcionar un mecanismo flexible para leer los valores de configuración.
* Solucione algunas de las necesidades básicas de las aplicaciones cuando se mueven a un contenedor y a un entorno centrado en la nube.
* Se puede usar para mejorar la protección de los datos de configuración mediante el dibujo de orígenes no disponibles anteriormente (por ejemplo, Azure Key Vault y variables de entorno) en el sistema de configuración de .NET.

## <a name="keyvalue-configuration-builders"></a>Generadores de configuración de clave/valor

Un escenario común que pueden controlar los generadores de configuraciones es proporcionar un mecanismo básico de sustitución de clave-valor para las secciones de configuración que siguen un patrón de clave y valor. El concepto .NET Framework de ConfigurationBuilders no se limita a secciones o patrones de configuración específicos. Sin embargo, muchos de los generadores de configuración en `Microsoft.Configuration.ConfigurationBuilders` ([GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionan dentro del patrón de clave-valor.

## <a name="keyvalue-configuration-builders-settings"></a>Configuración de los generadores de configuración de clave/valor

La siguiente configuración se aplica a todos los generadores de configuración de clave y valor de `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modo

Los generadores de configuración usan un origen externo de la información de clave y valor para rellenar los elementos de clave y valor seleccionados del sistema de configuración. En concreto, las secciones `<appSettings/>` y `<connectionStrings/>` reciben un tratamiento especial de los generadores de configuración. Los generadores funcionan en tres modos:

* `Strict`: el modo predeterminado. En este modo, el generador de configuración solo funciona en secciones de configuración con clave y valor bien conocidas. el modo `Strict` enumera cada clave de la sección. Si se encuentra una clave coincidente en el origen externo:

   * Los generadores de configuración reemplazan el valor de la sección de configuración resultante por el valor del origen externo.
* `Greedy`: este modo está estrechamente relacionado con el modo de `Strict`. En lugar de limitarse a las claves que ya existen en la configuración original:

  * Los generadores de configuración agregan todos los pares clave-valor del origen externo a la sección de configuración resultante.

* `Expand`: opera en el XML sin formato antes de analizarlo en un objeto de sección de configuración. Puede considerarse como una expansión de tokens en una cadena. Cualquier parte de la cadena XML sin formato que coincida con el patrón `${token}` es candidata para la expansión de tokens. Si no se encuentra ningún valor correspondiente en el origen externo, no se cambia el token. Los generadores de este modo no se limitan a las secciones `<appSettings/>` y `<connectionStrings/>`.

El siguiente marcado de *Web. config* habilita [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en modo de `Strict`:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

En el código siguiente se lee el `<appSettings/>` y `<connectionStrings/>` que se muestran en el archivo *Web. config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

El código anterior establecerá los valores de propiedad en:

* Valores del archivo *Web. config* si las claves no se establecen en variables de entorno.
* Los valores de la variable de entorno, si se establece.

Por ejemplo, `ServiceID` contendrá:

* "Valor de ServiceID desde web. config", si no se establece la variable de entorno `ServiceID`.
* El valor de la variable de entorno `ServiceID`, si se establece.

En la imagen siguiente se muestran las claves y los valores de `<appSettings/>` del conjunto de archivos *Web. config* anterior en el editor de entorno:

![Editor de entorno](config-builder/static/env.png)

Nota: es posible que tenga que salir y reiniciar Visual Studio para ver los cambios en las variables de entorno.

### <a name="prefix-handling"></a>Control de prefijos

Los prefijos de clave pueden simplificar la configuración de claves porque:

* La configuración .NET Framework es compleja y está anidada.
* Los orígenes de clave/valor externos suelen ser básicos y planos por naturaleza. Por ejemplo, las variables de entorno no están anidadas.

Use cualquiera de los métodos siguientes para insertar `<appSettings/>` y `<connectionStrings/>` en la configuración mediante variables de entorno:

* Con el `EnvironmentConfigBuilder` en el modo de `Strict` predeterminado y los nombres de clave adecuados en el archivo de configuración. El código y el marcado anteriores tienen este enfoque. Con este enfoque **no** puede tener claves con el mismo nombre en `<appSettings/>` y `<connectionStrings/>`.
* Use dos `EnvironmentConfigBuilder`s en modo de `Greedy` con prefijos y `stripPrefix`distintos. Con este enfoque, la aplicación puede leer `<appSettings/>` y `<connectionStrings/>` sin necesidad de actualizar el archivo de configuración. En la sección siguiente, [stripPrefix](#stripprefix), se muestra cómo hacerlo.
* Use dos `EnvironmentConfigBuilder`s en modo de `Greedy` con prefijos distintos. Con este enfoque no puede tener nombres de clave duplicados, ya que los nombres de clave deben ser diferentes en el prefijo.  Por ejemplo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Con el marcado anterior, se puede usar el mismo origen de clave y valor sin formato para rellenar la configuración de dos secciones diferentes.

En la imagen siguiente se muestran las claves y los valores de `<appSettings/>` y `<connectionStrings/>` del conjunto de archivos *Web. config* anterior en el editor de entorno:

![Editor de entorno](config-builder/static/prefix.png)

El código siguiente lee el `<appSettings/>` y `<connectionStrings/>` claves/valores contenidos en el archivo *Web. config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

El código anterior establecerá los valores de propiedad en:

* Valores del archivo *Web. config* si las claves no se establecen en variables de entorno.
* Los valores de la variable de entorno, si se establece.

Por ejemplo, con el archivo *Web. config* anterior, las claves/valores de la imagen del editor del entorno anterior y el código anterior, se establecen los valores siguientes:

|  Key              | {2&gt;Value&lt;2} |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID de variables de env|
|    AppSetting_default            | AppSetting_default valor de env |
|       ConnStr_default         | ConnStr_default Val from env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: Boolean, el valor predeterminado es `false`. 

El marcado XML anterior separa la configuración de la aplicación de las cadenas de conexión, pero requiere todas las claves del archivo *Web. config* para usar el prefijo especificado. Por ejemplo, el prefijo `AppSetting` debe agregarse a la clave `ServiceID` ("AppSetting_ServiceID"). Con `stripPrefix`, el prefijo no se utiliza en el archivo *Web. config* . El prefijo es necesario en el origen del generador de configuraciones (por ejemplo, en el entorno). Se prevé que la mayoría de los desarrolladores utilizarán `stripPrefix`.

Normalmente, las aplicaciones quitan el prefijo. El siguiente *archivo Web. config* quita el prefijo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

En el archivo *Web. config* anterior, la clave de `default` está en el `<appSettings/>` y `<connectionStrings/>`.

En la imagen siguiente se muestran las claves y los valores de `<appSettings/>` y `<connectionStrings/>` del conjunto de archivos *Web. config* anterior en el editor de entorno:

![Editor de entorno](config-builder/static/prefix.png)

El código siguiente lee el `<appSettings/>` y `<connectionStrings/>` claves/valores contenidos en el archivo *Web. config* anterior:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

El código anterior establecerá los valores de propiedad en:

* Valores del archivo *Web. config* si las claves no se establecen en variables de entorno.
* Los valores de la variable de entorno, si se establece.

Por ejemplo, con el archivo *Web. config* anterior, las claves/valores de la imagen del editor del entorno anterior y el código anterior, se establecen los valores siguientes:

|  Key              | {2&gt;Value&lt;2} |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID de variables de env|
|    default            | AppSetting_default valor de env |
|    default         | ConnStr_default Val from env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: String, el valor predeterminado es `@"\$\{(\w+)\}"`

El `Expand` comportamiento de los generadores busca en el XML sin formato los tokens que parecen `${token}`. La búsqueda se realiza con la expresión regular predeterminada `@"\$\{(\w+)\}"`. El conjunto de caracteres que coincide con `\w` es más estricto que XML y muchos orígenes de configuración permiten. Use `tokenPattern` cuando se requieran más caracteres que `@"\$\{(\w+)\}"` en el nombre del token.

`tokenPattern`: cadena:

* Permite a los desarrolladores cambiar la expresión regular que se usa para la coincidencia de tokens.
* No se realiza ninguna validación para asegurarse de que se trata de una expresión regular correcta y no peligrosa.
* Debe contener un grupo de capturas. La expresión regular completa debe coincidir con el token completo. La primera captura debe ser el nombre del token que se va a buscar en el origen de configuración.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Generadores de configuración de Microsoft. Configuration. ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Es la más sencilla de los generadores de configuración.
* Lee los valores del entorno.
* No tiene ninguna opción de configuración adicional.
* El valor del atributo `name` es arbitrario.

**Nota:** En un entorno de contenedor de Windows, las variables que se establecen en tiempo de ejecución solo se insertan en el entorno de proceso EntryPoint. Las aplicaciones que se ejecutan como un servicio o un proceso sin punto de entrada no recogen estas variables a menos que se inserten de otro modo a través de un mecanismo del contenedor. En el caso de los contenedores basados en [ASP.net](https://github.com/Microsoft/aspnet-docker)de [IIS](https://github.com/Microsoft/iis-docker/pull/41)/, la versión actual de [ServiceMonitor. exe](https://github.com/Microsoft/iis-docker/pull/41) solo lo controla en *DefaultAppPool* . Otras variantes de contenedor basadas en Windows pueden necesitar desarrollar su propio mecanismo de inyección para los procesos que no son de punto de entrada.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nunca almacene contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente. Los secretos de producción no deben usarse para el desarrollo o las pruebas.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Este generador de configuración proporciona una característica similar a [ASP.net Core administrador de secretos](/aspnet/core/security/app-secrets).

[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) se puede usar en proyectos de .NET Framework, pero debe especificarse un archivo de secretos. Como alternativa, puede definir la propiedad `UserSecretsId` en el archivo de proyecto y crear el archivo de secretos sin formato en la ubicación correcta para la lectura. Para mantener las dependencias externas fuera del proyecto, el archivo secreto tiene un formato XML. El formato XML es un detalle de implementación y no se debe confiar en el formato. Si necesita compartir un archivo *Secrets. JSON* con proyectos de .net Core, considere la posibilidad de usar [SimpleJsonConfigBuilder](#simplejsonconfigbuilder). El formato de `SimpleJsonConfigBuilder` de .NET Core también debe considerarse un detalle de implementación sujeto a cambios.

Atributos de configuración para `UserSecretsConfigBuilder`:

* `userSecretsId`: este es el método preferido para identificar un archivo de secretos XML. Funciona de forma similar a .NET Core, que usa una propiedad de proyecto `UserSecretsId` para almacenar este identificador. La cadena debe ser única, por lo que no es necesario que sea un GUID. Con este atributo, el `UserSecretsConfigBuilder` buscar en una ubicación local conocida (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) para un archivo de secretos que pertenezca a este identificador.
* `userSecretsFile`: un atributo opcional que especifica el archivo que contiene los secretos. El carácter `~` se puede utilizar en el inicio para hacer referencia a la raíz de la aplicación. Este atributo o el atributo `userSecretsId` es obligatorio. Si se especifican ambos, la `userSecretsFile` tiene prioridad.
* `optional`: booleano, valor predeterminado `true`: impide una excepción si no se encuentra el archivo de secretos. 
* El valor del atributo `name` es arbitrario.

El archivo de secretos tiene el siguiente formato:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) Lee los valores almacenados en el [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` es obligatorio (ya sea el nombre del almacén o un URI del almacén). Los otros atributos permiten controlar el almacén al que se va a conectar, pero solo son necesarios si la aplicación no se está ejecutando en un entorno que funcione con `Microsoft.Azure.Services.AppAuthentication`. La biblioteca de autenticación de servicios de Azure se usa para seleccionar automáticamente la información de conexión del entorno de ejecución si es posible. Puede invalidar la recogida automática de información de conexión proporcionando una cadena de conexión.

* `vaultName`: es necesario si no se proporciona `uri`. Especifica el nombre del almacén en la suscripción de Azure desde el que se van a leer pares de clave y valor.
* `connectionString`: una cadena de conexión que puede usar [AzureServiceTokenProvider](https://docs.microsoft.com/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri`: se conecta a otros proveedores de Key Vault con el valor de `uri` especificado. Si no se especifica, Azure (`vaultName`) es el proveedor de almacén.
* `version`-Azure Key Vault proporciona una característica de control de versiones para secretos. Si se especifica `version`, el generador solo recupera los secretos que coinciden con esta versión.
* `preloadSecretNames`: de forma predeterminada, este generador consulta **todos** los nombres de clave en el almacén de claves cuando se inicializa. Para evitar la lectura de todos los valores de clave, establezca este atributo en `false`. Si se establece en `false` se leen los secretos de uno en uno. Leer secretos de uno en uno puede ser útil si el almacén permite el acceso "Get" pero no "list". **Nota:** Al usar el modo de `Greedy`, `preloadSecretNames` debe ser `true` (el valor predeterminado).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) es un generador de configuración básico que usa los archivos de un directorio como origen de valores. El nombre de un archivo es la clave y el contenido es el valor. Este generador de configuración puede ser útil cuando se ejecuta en un entorno de contenedor organizado. Los sistemas como Docker Swarm y Kubernetes proporcionan `secrets` a sus contenedores de Windows organizados de forma clave por archivo.

Detalles del atributo:

* `directoryPath` - Requerido. Especifica una ruta de acceso en la que buscar valores. Docker para Windows secretos se almacenan de forma predeterminada en el directorio *C:\ProgramData\Docker\secrets*
* `ignorePrefix`: se excluyen los archivos que comienzan con este prefijo. Se establece en "ignore." de forma predeterminada.
* `keyDelimiter`: el valor predeterminado es `null`. Si se especifica, el generador de configuración atraviesa varios niveles del directorio y crea nombres de clave con este delimitador. Si este valor se `null`, el generador de configuración solo busca en el nivel superior del directorio.
* `optional`: el valor predeterminado es `false`. Especifica si el generador de configuración debe producir errores si el directorio de origen no existe.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nunca almacene contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente. Los secretos de producción no deben usarse para el desarrollo o las pruebas.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Los proyectos de .NET Core usan frecuentemente archivos JSON para la configuración. El generador de [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) permite usar archivos JSON de .net Core en el .NET Framework. Este generador de configuración proporciona una asignación básica desde un origen de valor o clave sin formato en áreas de clave y valor específicas de la configuración de .NET Framework. Este generador de configuración **no** proporciona configuraciones jerárquicas. El archivo de copia de seguridad JSON es similar a un diccionario, no a un objeto jerárquico complejo. Se puede usar un archivo jerárquico de varios niveles. Este proveedor `flatten`la profundidad anexando el nombre de la propiedad en cada nivel mediante `:` como un delimitador.

Detalles del atributo:

* `jsonFile` - Requerido. Especifica el archivo JSON del que se va a leer. El carácter `~` se puede usar en el inicio para hacer referencia a la raíz de la aplicación.
* `optional`: booleano, el valor predeterminado es `true`. Evita producir excepciones si no se encuentra el archivo JSON.
* `jsonMode` - `[Flat|Sectional]`. `Flat` es el valor predeterminado. Cuando se `Flat``jsonMode`, el archivo JSON es un origen de clave/valor sin formato. Los `EnvironmentConfigBuilder` y `AzureKeyVaultConfigBuilder` son también orígenes de clave/valor sin formato. Cuando el `SimpleJsonConfigBuilder` está configurado en el modo de `Sectional`:

  * El archivo JSON se divide conceptualmente solo en el nivel superior en varios diccionarios.
  * Cada uno de los diccionarios solo se aplica a la sección de configuración que coincide con el nombre de la propiedad de nivel superior asociado a ellos. Por ejemplo:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementar un generador de configuración de clave/valor personalizado

Si los generadores de configuración no satisfacen sus necesidades, puede escribir uno personalizado. La clase base `KeyValueConfigBuilder` controla los modos de sustitución y la mayoría de los prefijos. Un proyecto de implementación solo necesita:

* Herede de la clase base e implemente un origen básico de pares clave-valor a través de la `GetValue` y `GetAllValues`:
* Agregue [Microsoft. Configuration. ConfigurationBuilders. base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al proyecto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

La clase base `KeyValueConfigBuilder` proporciona gran parte del comportamiento coherente y de trabajo en los generadores de configuración de clave/valor.

## <a name="additional-resources"></a>Recursos adicionales

* [Repositorio de los generadores de configuración de GitHub](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticación de servicio a servicio en Azure Key Vault mediante .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
