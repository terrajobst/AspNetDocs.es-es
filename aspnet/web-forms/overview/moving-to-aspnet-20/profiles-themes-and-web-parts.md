---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Perfiles, temas y elementos web | Microsoft Docs
author: microsoft
description: Hay cambios importantes en la configuración e instrumentación en ASP.NET 2,0. La nueva API de configuración de ASP.NET permite que los cambios de configuración se hagan PR...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: cf5c45781be6d003d28c6aa27efa08032579a6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474643"
---
# <a name="profiles-themes-and-web-parts"></a>Perfiles, temas y elementos web

por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración e instrumentación en ASP.NET 2,0. La nueva API de configuración de ASP.NET permite realizar cambios de configuración mediante programación. Además, existen muchas opciones de configuración nuevas que permiten nuevas configuraciones e instrumentación.

ASP.NET 2,0 representa una mejora sustancial en el área de sitios web personalizados. Además de las características de pertenencia que ya hemos tratado, los perfiles, los temas y los elementos Web de ASP.NET mejoran considerablemente la personalización de los sitios Web.

## <a name="aspnet-profiles"></a>Perfiles de ASP.NET

Los perfiles de ASP.NET son similares a las sesiones. La diferencia es que un perfil es persistente, mientras que una sesión se pierde cuando se cierra el explorador. Otra diferencia importante entre las sesiones y los perfiles es que los perfiles están fuertemente tipados, lo que le proporciona IntelliSense durante el proceso de desarrollo.

Un perfil se define en el archivo de configuración de los equipos o en el archivo Web. config de la aplicación. (No se puede definir un perfil en un archivo Web. config de subcarpetas). El código siguiente define un perfil para almacenar los visitantes del sitio web primero y el apellido.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

El tipo de datos predeterminado para una propiedad de perfil es System. String. En el ejemplo anterior, no se ha especificado ningún tipo de datos. Por lo tanto, las propiedades FirstName y LastName son de tipo cadena. Como se mencionó anteriormente, las propiedades de perfil están fuertemente tipadas. El código siguiente agrega una nueva propiedad para Age que es de tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Los perfiles suelen usarse con la autenticación de formularios ASP.NET. Cuando se usa en combinación con la autenticación de formularios, cada usuario tiene un perfil independiente asociado a su identificador de usuario. Sin embargo, también es posible permitir el uso de perfiles en una aplicación anónima mediante el elemento &lt;anonymousIdentification&gt; en el archivo de configuración junto con el atributo **allowAnonymous** , como se muestra a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Cuando un usuario anónimo examina el sitio, ASP.NET crea una instancia de **ProfileCommon** para el usuario. Este perfil usa un identificador único almacenado en una cookie en el explorador para identificar al usuario como visitante único. De esta manera, puede almacenar la información de Perfil de los usuarios que están explorando de forma anónima.

## <a name="profile-groups"></a>Grupos de perfiles

Es posible agrupar las propiedades de los perfiles. Al agrupar las propiedades, es posible simular varios perfiles para una aplicación específica.

La configuración siguiente configura una propiedad FirstName y LastName para dos grupos; Compradores y oportunidades.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Después, es posible establecer las propiedades en un grupo determinado como se indica a continuación:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Almacenar objetos complejos

Hasta ahora, los ejemplos que hemos descrito han almacenado tipos de datos simples en un perfil. También es posible almacenar tipos de datos complejos en un perfil especificando el método de serialización mediante el atributo **Serialize** de la manera siguiente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

En este caso, el tipo es PurchaseInvoice. La clase PurchaseInvoice debe marcarse como Serializable y puede contener cualquier número de propiedades. Por ejemplo, si PurchaseInvoice tiene una propiedad denominada **NumItemsPurchased**, puede hacer referencia a esa propiedad en el código como se indica a continuación:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Herencia de perfiles

Es posible crear un perfil para usarlo en varias aplicaciones. Al crear una clase de perfil que se deriva de ProfileBase, puede volver a usar un perfil en varias aplicaciones mediante el atributo **Inherits** , como se muestra a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

En este caso, la clase **PurchasingProfile** tendría el siguiente aspecto:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Proveedores de perfiles

Los perfiles de ASP.NET usan el modelo de proveedor. El proveedor predeterminado almacena la información en una base de datos SQL Server Express en la carpeta app\_data de la aplicación Web mediante el proveedor SqlProfileProvider. Si la base de datos no existe, ASP.NET la creará automáticamente cuando el perfil intente almacenar información.

Sin embargo, en algunos casos, puede que desee desarrollar su propio proveedor de perfiles. La característica de perfil ASP.NET le permite usar fácilmente diferentes proveedores.

Puede crear un proveedor de perfiles personalizado cuando:

- Debe almacenar la información de perfil en un origen de datos, como en una base de datos de FoxPro o en una base de datos de Oracle, que no es compatible con los proveedores de perfiles incluidos en el .NET Framework.
- Debe administrar la información de perfil con un esquema de base de datos diferente del esquema de base de datos utilizado por los proveedores incluidos con el .NET Framework. Un ejemplo común es que desea integrar la información de perfil con los datos de usuario en una base de datos de SQL Server existente.

### <a name="required-classes"></a>Clases necesarias

Para implementar un proveedor de perfiles, cree una clase que herede la clase abstracta System. Web. Profile. ProfileProvider. A su vez, la clase abstracta **ProfileProvider** hereda la clase abstracta System. Configuration. SettingsProvider, que hereda la clase abstracta System. Configuration. Provider. ProviderBase. Debido a esta cadena de herencia, además de los miembros necesarios de la clase **ProfileProvider** , debe implementar los miembros necesarios de las clases **SettingsProvider** y **ProviderBase** .

En las tablas siguientes se describen las propiedades y los métodos que debe implementar desde las clases abstractas **ProviderBase**, **SettingsProvider**y **ProfileProvider** .

### <a name="providerbase-members"></a>Miembros de ProviderBase

| **Miembro** | **Descripción** |
| --- | --- |
| Initialize (método) | Toma como entrada el nombre de la instancia del proveedor y un NameValueCollection de valores de configuración. Se usa para establecer opciones y valores de propiedad para la instancia del proveedor, incluidos los valores específicos de la implementación y las opciones especificadas en la configuración del equipo o en el archivo Web. config. |

### <a name="settingsprovider-members"></a>Miembros de SettingsProvider

| **Miembro** | **Descripción** |
| --- | --- |
| Propiedad ApplicationName | El nombre de la aplicación que se almacena con cada perfil. El proveedor de perfiles usa el nombre de la aplicación para almacenar información de perfil por separado para cada aplicación. Esto permite que varias aplicaciones ASP.NET usen el mismo origen de datos sin un conflicto si el mismo nombre de usuario se crea en aplicaciones diferentes. Como alternativa, varias aplicaciones ASP.NET pueden compartir un origen de datos de perfil especificando el mismo nombre de aplicación. |
| Método GetPropertyValues | Toma como entrada un objeto SettingsContext y un objeto SettingsPropertyCollection. **SettingsContext** proporciona información sobre el usuario. Puede usar la información como clave principal para recuperar la información de la propiedad de perfil del usuario. Use el objeto **SettingsContext** para obtener el nombre de usuario y si el usuario está autenticado o es anónimo. La **SettingsPropertyCollection** contiene una colección de objetos SettingsProperty. Cada objeto **SettingsProperty** proporciona el nombre y el tipo de la propiedad, así como información adicional, como el valor predeterminado de la propiedad y si la propiedad es de solo lectura. El método **GetPropertyValues** rellena un objeto SettingsPropertyValueCollection con objetos SettingsPropertyValue basándose en los objetos **SettingsProperty** proporcionados como entrada. Los valores del origen de datos para el usuario especificado se asignan a las propiedades PropertyValue para cada objeto **SettingsPropertyValue** y se devuelve toda la colección. Al llamar al método, también se actualiza el valor de LastActivityDate para el perfil de usuario especificado a la fecha y hora actuales. |
| Método SetPropertyValues | Toma como entrada un objeto **SettingsContext** y un objeto **SettingsPropertyValueCollection** . **SettingsContext** proporciona información sobre el usuario. Puede usar la información como clave principal para recuperar la información de la propiedad de perfil del usuario. Use el objeto **SettingsContext** para obtener el nombre de usuario y si el usuario está autenticado o es anónimo. El objeto **SettingsPropertyValueCollection** contiene una colección de objetos **SettingsPropertyValue** . Cada objeto **SettingsPropertyValue** proporciona el nombre, el tipo y el valor de la propiedad, así como información adicional, como el valor predeterminado de la propiedad y si la propiedad es de solo lectura. El método **SetPropertyValues** actualiza los valores de propiedad de perfil en el origen de datos para el usuario especificado. Al llamar al método, también se actualizan los valores **LastActivityDate** y LastUpdatedDate para el perfil de usuario especificado a la fecha y hora actuales. |

### <a name="profileprovider-members"></a>Miembros de ProfileProvider

| **Miembro** | **Descripción** |
| --- | --- |
| Método DeleteProfiles | Toma como entrada una matriz de cadenas de nombres de usuario y elimina del origen de datos toda la información de perfil y los valores de propiedad de los nombres especificados en los que el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| Método DeleteProfiles | Toma como entrada una colección de objetos ProfileInfo y elimina del origen de datos toda la información de perfil y los valores de propiedad de cada perfil en el que el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| Método DeleteInactiveProfiles | Toma como entrada un valor de ProfileAuthenticationOption y un objeto DateTime y elimina del origen de datos toda la información de perfil y los valores de propiedad en los que la fecha de la última actividad es menor o igual que la fecha y hora especificadas y el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . El parámetro **ProfileAuthenticationOption** especifica si solo se van a eliminar los perfiles anónimos, solo los perfiles autenticados o todos los perfiles. Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| Método GetAllProfiles | Toma como entrada un valor de **ProfileAuthenticationOption** , un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un objeto ProfileInfoCollection que contiene objetos **ProfileInfo** para todos los perfiles en el origen de datos donde el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . El parámetro **ProfileAuthenticationOption** especifica si solo se devuelven perfiles anónimos, solo perfiles autenticados o todos los perfiles. Los resultados devueltos por el método **GetAllProfiles** están restringidos por los valores de índice de página y tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se van a devolver en **ProfileInfoCollection**. El valor de índice de página especifica la página de resultados que se va a devolver, donde 1 identifica la primera página. El parámetro para el total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de la página es 2 con un tamaño de página de 5, el valor de **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando el método devuelve. |
| Método GetAllInactiveProfiles | Toma como entrada un valor de **ProfileAuthenticationOption** , un objeto **DateTime** , un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un objeto **ProfileInfoCollection** que contiene objetos **ProfileInfo** para todos los perfiles en el origen de datos donde la fecha de la última actividad es menor o igual que el valor **DateTime** especificado y donde el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . El parámetro **ProfileAuthenticationOption** especifica si solo se devuelven perfiles anónimos, solo perfiles autenticados o todos los perfiles. Los resultados devueltos por el método **GetAllInactiveProfiles** están restringidos por los valores de índice de página y tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se van a devolver en **ProfileInfoCollection**. El valor de índice de página especifica la página de resultados que se va a devolver, donde 1 identifica la primera página. El parámetro para el total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de la página es 2 con un tamaño de página de 5, el valor de **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando el método devuelve. |
| Método FindProfilesByUserName | Toma como entrada un valor de **ProfileAuthenticationOption** , una cadena que contiene un nombre de usuario, un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un objeto **ProfileInfoCollection** que contiene objetos **ProfileInfo** para todos los perfiles en el origen de datos donde el nombre de usuario coincide con el nombre de usuario especificado y donde el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . El parámetro **ProfileAuthenticationOption** especifica si solo se devuelven perfiles anónimos, solo perfiles autenticados o todos los perfiles. Si el origen de datos admite capacidades de búsqueda adicionales, como caracteres comodín, puede proporcionar capacidades de búsqueda más amplias para los nombres de usuario. Los resultados devueltos por el método **FindProfilesByUserName** están restringidos por los valores de índice de página y tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se van a devolver en **ProfileInfoCollection**. El valor de índice de página especifica la página de resultados que se va a devolver, donde 1 identifica la primera página. El parámetro para el total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de la página es 2 con un tamaño de página de 5, el valor de **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando el método devuelve. |
| Método FindInactiveProfilesByUserName | Toma como entrada un valor de **ProfileAuthenticationOption** , una cadena que contiene un nombre de usuario, un objeto **DateTime** , un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el recuento total de perfiles. Devuelve un objeto **ProfileInfoCollection** que contiene objetos **ProfileInfo** para todos los perfiles en el origen de datos donde el nombre de usuario coincide con el nombre de usuario especificado, donde la última fecha de actividad es menor o igual que el valor **DateTime**especificado y el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . El parámetro **ProfileAuthenticationOption** especifica si solo se devuelven perfiles anónimos, solo perfiles autenticados o todos los perfiles. Si el origen de datos admite capacidades de búsqueda adicionales, como caracteres comodín, puede proporcionar capacidades de búsqueda más amplias para los nombres de usuario. Los resultados devueltos por el método **FindInactiveProfilesByUserName** están restringidos por los valores de índice de página y tamaño de página. El valor de tamaño de página especifica el número máximo de objetos **ProfileInfo** que se van a devolver en **ProfileInfoCollection**. El valor de índice de página especifica la página de resultados que se va a devolver, donde 1 identifica la primera página. El parámetro para el total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles para la aplicación y el valor de índice de la página es 2 con un tamaño de página de 5, el valor de **ProfileInfoCollection** devuelto contiene los perfiles del sexto al décimo. El valor total de registros se establece en 13 cuando el método devuelve. |
| Método GetNumberOfInActiveProfiles | Toma como entrada un valor de **ProfileAuthenticationOption** y un objeto **DateTime** y devuelve un recuento de todos los perfiles en el origen de datos donde la fecha de la última actividad es menor o igual que el valor **DateTime** especificado y el nombre de la aplicación coincide con el valor de la propiedad **applicationName** . El parámetro **ProfileAuthenticationOption** especifica si solo se van a contar perfiles anónimos, solo perfiles autenticados o todos los perfiles. |

### <a name="applicationname"></a>ApplicationName

Dado que los proveedores de perfiles almacenan información de perfil por separado para cada aplicación, debe asegurarse de que el esquema de datos incluye el nombre de la aplicación y que las consultas y actualizaciones también incluyen el nombre de la aplicación. Por ejemplo, el comando siguiente se usa para recuperar un valor de propiedad de una base de datos basada en el nombre de usuario y si el perfil es anónimo, y garantiza que el valor **applicationName** está incluido en la consulta.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temas de ASP.NET

## <a name="what-are-aspnet-20-themes"></a>¿Qué son los temas de ASP.NET 2,0?

Uno de los aspectos más importantes de una aplicación web es una apariencia coherente en todo el sitio. Los desarrolladores de ASP.NET 1. x suelen usar Hojas de estilo CSS (CSS) para implementar una apariencia coherente. Los temas de ASP.NET 2,0 mejoran significativamente en CSS porque proporcionan al desarrollador de ASP.NET la capacidad de definir la apariencia de los controles de servidor de ASP.NET, así como los elementos HTML. Los temas de ASP.NET se pueden aplicar a controles individuales, a una página web específica o a una aplicación web completa. Los temas usan una combinación de archivos CSS, un archivo de máscara opcional y un directorio de imágenes opcional si se necesitan imágenes. El archivo de máscara controla la apariencia visual de los controles de servidor ASP.NET.

## <a name="where-are-themes-stored"></a>¿Dónde se almacenan los temas?

La ubicación donde se almacenan los temas varía en función de su ámbito. Los temas que se pueden aplicar a cualquier aplicación se almacenan en la siguiente carpeta:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema específico de una aplicación determinada se almacena en un directorio de `App\_Themes\<Theme\_Name>` en la raíz del sitio Web.

> [!NOTE]
> Un archivo de máscara solo debe modificar las propiedades del control de servidor que afectan a la apariencia.

Un tema global es un tema que se puede aplicar a cualquier aplicación o sitio web que se ejecute en el servidor Web. Estos temas se almacenan de forma predeterminada en el directorio ASP. NETClientfiles\Themes que se encuentra dentro del directorio V2. x. xxxxx. Como alternativa, puede trasladar los archivos de tema a la carpeta ASPNET\_Client/System\_web/[versión]/themes/[Theme\_Name] en la raíz de su sitio Web.

Los temas específicos de la aplicación solo se pueden aplicar a la aplicación en la que residen los archivos. Estos archivos se almacenan en el directorio `App\_Themes/<theme\_name>` en la raíz del sitio Web.

## <a name="the-components-of-a-theme"></a>Los componentes de un tema

Un tema se compone de uno o varios archivos CSS, un archivo de máscara opcional y una carpeta de imágenes opcional. Los archivos CSS pueden ser cualquier nombre que desee (es decir, default. CSS o Theme. CSS, etc.) y deben estar en la raíz de la carpeta themes. Los archivos CSS se utilizan para definir clases y atributos CSS normales para selectores específicos. Para aplicar una de las clases CSS a un elemento Page, se usa la propiedad **CssClass** .

El archivo de máscara es un archivo XML que contiene definiciones de propiedad para controles de servidor ASP.NET. El código que se muestra a continuación es un archivo de máscara de ejemplo.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

En la **figura 1** se muestra una pequeña página de ASP.net examinada sin aplicar un tema. En la **ilustración 2** se muestra el mismo archivo con un tema aplicado. El color de fondo y el color del texto se configuran mediante un archivo CSS. La apariencia del botón y del cuadro de texto se configura mediante el archivo de máscara indicado anteriormente.

![Sin tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: ningún tema

![Tema aplicado](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema aplicado

El archivo de máscara indicado anteriormente define una máscara predeterminada para todos los controles de cuadro de texto y controles de botón. Esto significa que todos los controles de cuadro de texto y de botón insertados en una página tendrán este aspecto. También puede definir una máscara que se puede aplicar a instancias específicas de estos controles mediante la propiedad **SkinID** del control.

El código siguiente define una máscara para un control de botón. Solo los controles de botón con una propiedad **SkinID** de **goButton** adoptarán la apariencia de la máscara.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Solo puede tener una máscara predeterminada por cada tipo de control de servidor. Si necesita máscaras adicionales, debe utilizar la propiedad SkinID.

## <a name="applying-themes-to-pages"></a>Aplicar temas a páginas

Un tema puede aplicarse mediante uno de los métodos siguientes:

- En el &lt;páginas&gt; elemento del archivo Web. config
- En la Directiva de @Page de una página
- De manera programática

## <a name="applying-a-theme-in-the-configuration-file"></a>Aplicar un tema en el archivo de configuración

Para aplicar un tema en el archivo de configuración de aplicaciones, use la siguiente sintaxis:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

El nombre de tema especificado aquí debe coincidir con el nombre de la carpeta themes. Esta carpeta puede existir en cualquiera de las ubicaciones mencionadas anteriormente en este curso. Si intenta aplicar un tema que no existe, se producirá un error de configuración.

## <a name="applying-a-theme-in-the-page-directive"></a>Aplicar un tema en la Directiva de página

También puede aplicar un tema en la Directiva @ page. Este método permite usar un tema para una página específica.

Para aplicar un tema en la Directiva @Page, use la siguiente sintaxis:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Una vez más, el tema que se especifica aquí debe coincidir con la carpeta de temas, como se mencionó anteriormente. Si intenta aplicar un tema que no existe, se producirá un error de compilación. Visual Studio también resaltará el atributo y le informará de que no existe ese tema.

## <a name="applying-a-theme-programmatically"></a>Aplicar un tema mediante programación

Para aplicar un tema mediante programación, debe especificar la propiedad **Theme** de la página en la **Página\_método PreInit** .

Para aplicar un tema mediante programación, use la sintaxis siguiente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Es necesario aplicar el tema en el método PreInit debido al ciclo de vida de la página. Si la aplica después de ese punto, el tiempo de ejecución ya habrá aplicado el tema de las páginas y un cambio en ese momento se retrasará en el ciclo de vida. Si aplica un tema que no existe, se produce una **HttpException** . Cuando se aplica un tema mediante programación, se produce una advertencia de compilación si algún control de servidor tiene una propiedad SkinID especificada. Esta advertencia está pensada para informarle de que no se aplica un tema mediante declaración y se puede omitir.

## <a name="exercise-1--applying-a-theme"></a>Ejercicio 1: aplicar un tema

En este ejercicio, se aplicará un tema ASP.NET a un sitio Web.

> [!IMPORTANT]
> Si usa Microsoft Word para escribir información en un archivo de máscara, asegúrese de que no está reemplazando las comillas regulares por Comillas inteligentes. Las comillas inteligentes producirán problemas con los archivos de máscara.

1. Cree un nuevo sitio web ASP.NET.
2. Haga clic con el botón derecho en el proyecto en Explorador de soluciones y elija Agregar nuevo elemento.
3. Elija archivo de configuración Web en la lista de archivos y haga clic en Agregar.
4. Haga clic con el botón derecho en el proyecto en Explorador de soluciones y elija Agregar nuevo elemento.
5. Elija archivo de máscara y haga clic en Agregar.
6. Haga clic en sí cuando se le pregunte si desea colocar el archivo dentro de la carpeta de temas del\_de la aplicación.
7. Haga clic con el botón derecho en la carpeta SkinFile dentro de la carpeta app\_themes en Explorador de soluciones y elija Agregar nuevo elemento.
8. Elija hoja de estilos en la lista de archivos y haga clic en Agregar. Ahora tiene todos los archivos necesarios para implementar el nuevo tema. Sin embargo, Visual Studio ha llamado a la carpeta themes SkinFile. Haga clic con el botón secundario en esa carpeta y cambie el nombre a CoolTheme.
9. Abra el archivo SkinFile. Skin y agregue el siguiente código al final del archivo: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Guarde el archivo SkinFile. Skin.
11. Abra StyleSheet. CSS.
12. Reemplace todo el texto que contiene por lo siguiente: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Guarde el archivo StyleSheet. CSS.
14. Abra la página default. aspx.
15. Agregue un control de cuadro de texto y un control de botón.
16. Guarde la página. Ahora, examine la página default. aspx. Debe mostrarse como un formulario web normal.
17. Abra el archivo Web. config.
18. Agregue lo siguiente directamente debajo de la etiqueta de apertura `<system.web>`: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Guarde el archivo Web. config. Ahora, examine la página default. aspx. Debe mostrarse con el tema aplicado.
20. Si aún no está abierto, abra la página default. aspx en Visual Studio.
21. Seleccione el botón.
22. Cambie la propiedad **SkinID** a goButton. Tenga en cuenta que Visual Studio proporciona una lista desplegable con valores SkinID válidos para un control de botón.
23. Guarde la página. Ahora vuelva a obtener una vista previa de la página en el explorador. El botón ahora debería decir "Go" y debe ser más ancho.

Con la propiedad **SkinID** , puede configurar fácilmente diferentes máscaras para diferentes instancias de un tipo determinado de control de servidor.

## <a name="the-stylesheettheme-property"></a>Propiedad StyleSheetTheme

Hasta ahora, hemos hablado solo sobre cómo aplicar temas con la propiedad Theme. Cuando se usa la propiedad Theme, el archivo de máscara invalidará cualquier configuración declarativa para los controles de servidor. Por ejemplo, en el ejercicio 1, ha especificado un SkinID de "goButton" para el control de botón y ha cambiado el texto del botón a "Go". Es posible que haya observado que la propiedad text del botón en el diseñador se ha establecido en "Button", pero el tema reemplazó. El tema siempre invalidará cualquier valor de propiedad en el diseñador.

Si desea poder invalidar las propiedades definidas en el archivo de máscara del tema con las propiedades especificadas en el diseñador, puede utilizar la propiedad **StyleSheetTheme** en lugar de la propiedad Theme. La propiedad StyleSheetTheme es igual que la propiedad Theme, salvo que no invalida todos los valores de propiedad explícitos, como sucede con la propiedad Theme.

Para ver esto en acción, abra el archivo Web. config del proyecto en el ejercicio 1 y cambie el elemento `<pages>` a lo siguiente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Ahora, examine la página default. aspx y verá que el control botón tiene una propiedad Text de "Button" de nuevo. Esto se debe a que el valor de la propiedad Explicit en el diseñador está invalidando la propiedad Text establecida por goButton SkinID.

## <a name="overriding-themes"></a>Reemplazar temas

Un tema global puede invalidarse aplicando un tema con el mismo nombre en la carpeta app\_themes de la aplicación. Sin embargo, el tema no se aplica en un escenario de invalidación real. Si el tiempo de ejecución encuentra archivos de tema en la carpeta de temas del\_de la aplicación, aplicará el tema con esos archivos y omitirá el tema global.

La propiedad StyleSheetTheme es reemplazable y se puede invalidar en el código como se indica a continuación:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Elementos Web

ASP.NET elementos web es un conjunto integrado de controles para crear sitios web que permiten a los usuarios finales modificar el contenido, la apariencia y el comportamiento de las páginas web directamente desde un explorador. Las modificaciones se pueden aplicar a todos los usuarios del sitio o a usuarios individuales. Cuando los usuarios modifican páginas y controles, se puede guardar la configuración para conservar las preferencias personales de un usuario en futuras sesiones del explorador, una característica denominada personalización. Estas funciones de elementos web indican que los desarrolladores pueden hacer que los usuarios finales personalicen una aplicación Web de forma dinámica, sin intervención del desarrollador o del administrador.

Con el conjunto de controles elementos web, usted puede permitir a los usuarios finales:

- Personalizar el contenido de la página. Los usuarios pueden agregar nuevos controles de elementos web a una página, quitarlos, ocultarlos o minimizarlos como las ventanas normales.
- Personalizar el diseño de página. Los usuarios pueden arrastrar un control elementos web a otra zona de una página o cambiar su apariencia, sus propiedades y su comportamiento.
- Exportar e importar controles. Los usuarios pueden importar o exportar la configuración del control elementos web para su uso en otras páginas o sitios, conservando las propiedades, la apariencia e, incluso, los datos de los controles. Esto reduce la entrada de datos y las demandas de configuración de los usuarios finales.
- Crear conexiones. Los usuarios pueden establecer conexiones entre los controles para que, por ejemplo, un control de gráfico muestre un gráfico para los datos en un control de tablero de cotizaciones. Los usuarios podrían personalizar no solo la propia conexión, sino la apariencia y los detalles de cómo el control Chart muestra los datos.
- Administrar y personalizar la configuración de nivel de sitio. Los usuarios autorizados pueden configurar los valores de nivel de sitio, determinar quién puede tener acceso a un sitio o una página, establecer el acceso basado en roles a los controles, etc. Por ejemplo, un usuario de un rol administrativo podría establecer un control elementos web que compartirán todos los usuarios y evitar que los usuarios que no son administradores personalizaran el control compartido.

Normalmente, trabajará con elementos web de una de estas tres maneras: crear páginas que utilicen controles de elementos web, crear controles de elementos web individuales o crear aplicaciones web completas y personalizables, como un portal.

## <a name="page-development"></a>Desarrollo de páginas

Los desarrolladores de páginas pueden usar herramientas de diseño visual como Microsoft Visual Studio 2005 para crear páginas que utilicen elementos web. Una ventaja del uso de una herramienta como Visual Studio es que el conjunto de controles elementos web proporciona características para la creación y la configuración de arrastrar y colocar de elementos web controles en un diseñador visual. Por ejemplo, puede utilizar el diseñador para arrastrar una elementos web zona, o un control de editor elementos web, a la superficie de diseño y, a continuación, configurar el control directamente en el diseñador mediante la interfaz de usuario proporcionada por el conjunto de controles elementos web. Esto puede acelerar el desarrollo de aplicaciones elementos web y reducir la cantidad de código que se debe escribir.

## <a name="control-development"></a>Desarrollo de controles

Puede usar cualquier control ASP.NET existente como control elementos web, incluidos controles de servidor web estándar, controles de servidor personalizados y controles de usuario. Para obtener el máximo control de programación de su entorno, también puede crear controles de elementos web personalizados que se derivan de la clase WebPart. Para el desarrollo de controles de elementos web individuales, normalmente creará un control de usuario y lo utilizará como un control de elementos web, o bien desarrollará un control de elementos web personalizado.

Como ejemplo de desarrollo de un control de elementos web personalizado, puede crear un control que proporcione cualquiera de las características proporcionadas por otros controles de servidor ASP.NET que pueden ser útiles para empaquetar como un control de elementos web personalizable: calendarios, listas, información financiera, Noticias, calculadoras, controles de texto enriquecido para actualizar contenido, cuadrículas editables que se conectan a bases de datos, gráficos que actualizan dinámicamente sus presentaciones o información meteorológica y de viaje. Si proporciona un diseñador visual con el control, cualquier desarrollador de páginas que use Visual Studio simplemente puede arrastrar el control a una zona elementos web y configurarlo en tiempo de diseño sin tener que escribir código adicional.

La personalización es la base de la característica elementos web. Permite a los usuarios modificar, o personalizar, el diseño, la apariencia y el comportamiento de los controles de elementos web en una página. La configuración personalizada es de larga duración: se conservan no solo durante la sesión actual del explorador (como con el estado de vista), sino también en el almacenamiento a largo plazo, de modo que la configuración de un usuario se guarda para futuras sesiones del explorador. La personalización está habilitada de forma predeterminada para elementos web páginas.

Los componentes estructurales de la interfaz de usuario se basan en la personalización y proporcionan la estructura básica y los servicios que necesitan todos los controles de elementos web. Un componente estructural de la interfaz de usuario necesario en cada elementos web página es el control WebPartManager. Aunque nunca es visible, este control tiene la tarea crítica de coordinar todos los controles de elementos web en una página. Por ejemplo, realiza un seguimiento de todos los controles de elementos web individuales. Administra elementos web zonas (regiones que contienen elementos web controles en una página) y qué controles son en qué zonas. También realiza un seguimiento de los distintos modos de presentación en los que puede encontrarse una página, como el modo de exploración, conexión, edición o catálogo, y si los cambios de personalización se aplican a todos los usuarios o a usuarios individuales. Por último, inicia y realiza un seguimiento de las conexiones y la comunicación entre los controles de elementos web.

El segundo tipo de componente estructural de la interfaz de usuario es la zona. Las zonas actúan como administradores de diseño en una página de elementos web. Contienen y organizan los controles que derivan de la clase de elemento (controles de elementos) y proporcionan la capacidad de realizar un diseño de página modular en orientación horizontal o vertical. Las zonas también ofrecen elementos de interfaz de usuario comunes y coherentes (como estilo de encabezado y pie de página, título, estilo de borde, botones de acción, etc.) para cada control que contengan; estos elementos comunes se conocen como el cromo de un control. Se usan varios tipos especializados de zonas en los diferentes modos de presentación y con varios controles. Los diferentes tipos de zonas se describen en la sección elementos web controles esenciales.

Los controles de interfaz de usuario de elementos web, que se derivan de la clase del **elemento** , conforman la interfaz de usuario principal en una página de elementos Web. El conjunto de controles elementos web es flexible y está incluido en las opciones que ofrece para crear controles de elementos. Además de crear sus propios controles de elementos web personalizados, también puede usar controles de servidor de ASP.NET, controles de usuario o controles de servidor personalizados existentes como elementos web controles. Los controles esenciales que se usan con más frecuencia para crear elementos web páginas se describen en la sección siguiente.

## <a name="web-parts-essential-controls"></a>elementos web controles esenciales

El conjunto de controles elementos web es amplio, pero algunos controles son esenciales porque son necesarios para que elementos web funcionen, o porque son los controles que se usan con más frecuencia en elementos web páginas. Al empezar a usar elementos web y crear páginas de elementos web básicas, resulta útil estar familiarizado con los controles de elementos web esenciales que se describen en la tabla siguiente.

| **Control de elementos web** | **Descripción** |
| --- | --- |
| WebPartManager | Administra todos los controles de elementos web de una página. Se requiere un control **WebPartManager** (y solo uno) para cada página de elementos Web. |
| CatalogZone | Contiene los controles CatalogPart. Use esta zona para crear un catálogo de elementos web controles desde los que los usuarios pueden seleccionar los controles que se van a agregar a una página. |
| EditorZone | Contiene controles EditorPart. Use esta zona para permitir que los usuarios editen y personalicen los controles de elementos web en una página. |
| WebPartZone | Contiene y proporciona el diseño general de los controles WebPart que componen la interfaz de usuario principal de una página. Use esta zona siempre que cree páginas con controles de elementos web. Las páginas pueden contener una o más zonas. |
| ConnectionsZone | Contiene los controles WebPartConnection y proporciona una interfaz de usuario para administrar las conexiones. |
| Elemento Web (GenericWebPart) | Representa la interfaz de usuario principal; la mayoría de los controles de interfaz de usuario elementos web entran dentro de esta categoría. Para obtener el máximo control de programación, puede crear controles de elementos web personalizados que se derivan del control de **elementos Web** base. También puede usar controles de servidor, controles de usuario o controles personalizados existentes como elementos web controles. Siempre que cualquiera de estos controles se coloca en una zona, el control **WebPartManager** los ajusta automáticamente con controles de **GenericWebPart** en tiempo de ejecución para que pueda usarlos con elementos Web funcionalidad. |
| CatalogPart | Contiene una lista de controles de elementos web disponibles que los usuarios pueden agregar a la página. |
| WebPartConnection | Crea una conexión entre dos controles elementos web de una página. La conexión define uno de los controles elementos web como un proveedor (de datos) y el otro como consumidor. |
| EditorPart | Actúa como la clase base para los controles de editor especializados. |
| Controles EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart y PropertyGridEditorPart) | Permitir a los usuarios personalizar distintos aspectos de elementos web controles de interfaz de usuario en una página |

## <a name="lab-create-a-web-part-page"></a>Laboratorio: creación de una página de elementos Web

En este laboratorio, creará una página de elementos Web que conservará la información a través de perfiles de ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Crear una página simple con elementos web

En esta parte del tutorial, creará una página que usa elementos web controles para mostrar el contenido estático. El primer paso para trabajar con elementos web es crear una página con dos elementos estructurales necesarios. En primer lugar, una página de elementos web necesita un control WebPartManager para realizar el seguimiento de todos los controles de elementos web y coordinarlos. En segundo lugar, una página elementos web necesita una o más zonas, que son controles compuestos que contienen WebPart u otros controles de servidor y ocupan una región especificada de una página.

> [!NOTE]
> No es necesario hacer nada para habilitar la personalización de elementos web; está habilitado de forma predeterminada para el conjunto de controles elementos web. La primera vez que se ejecuta una página de elementos web en un sitio, ASP.NET configura un proveedor de personalización predeterminado para almacenar la configuración de personalización del usuario. Para obtener más información sobre la personalización, consulte información general sobre la personalización de elementos web.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Para crear una página que contenga elementos web controles

1. Cierre la página predeterminada y agregue una nueva página al sitio denominado WebPartsDemo. aspx.
2. Cambie a la vista **diseño** .
3. En el menú **Ver** , asegúrese de que las opciones controles y **detalles** **no visuales** estén seleccionadas, de modo que pueda ver etiquetas y controles de diseño que no tienen una interfaz de usuario.
4. Coloque el punto de inserción antes de la `<div>` etiquetas en la superficie de diseño y presione Entrar para agregar una nueva línea. Coloque el punto de inserción delante del carácter de nueva línea, haga clic en el control de la lista desplegable **formato de bloqueo** en el menú y seleccione la opción **encabezado 1** . En el encabezado, agregue el texto **elementos Web Página de demostración**.
5. En la pestaña **elementos Web** del cuadro de herramientas, arrastre un control **WebPartManager** hasta la página, colocándolo justo después del carácter de nueva línea y antes de las etiquetas de `<div>`.   
  
   El control **WebPartManager** no representa ningún resultado, por lo que aparece como un cuadro gris en la superficie del diseñador.
6. Coloque el punto de inserción dentro de las etiquetas de `<div>`.
7. En el menú **diseño** , haga clic en **Insertar tabla**y cree una nueva tabla que tenga una fila y tres columnas. Haga clic en el botón **propiedades de celda** , seleccione **superior** en la lista desplegable **alineación vertical** , haga clic en **Aceptar**y vuelva a hacer clic en **Aceptar** para crear la tabla.
8. Arrastre un control WebPartZone a la columna de la tabla izquierda. Haga clic con el botón derecho en el control **WebPartZone** , elija **propiedades**y establezca las siguientes propiedades:   
  
   ID.: SidebarZone   
  
   HeaderText: Sidebar
9. Arrastre un segundo control **WebPartZone** a la columna de la tabla intermedia y establezca las siguientes propiedades:   
  
   ID.: MainZone   
  
   HeaderText: Main
10. Guarde el archivo.

La página tiene ahora dos zonas distintas que se pueden controlar por separado. Sin embargo, ninguna de las zonas tiene contenido, por lo que la creación de contenido es el paso siguiente. En este tutorial, trabajará con elementos web controles que solo muestran contenido estático.

El diseño de una zona elementos web se especifica mediante un elemento&gt; zonetemplate &lt;. Dentro de la plantilla de zona, puede agregar cualquier control ASP.NET, ya sea un control de elementos web personalizado, un control de usuario o un control de servidor existente. Tenga en cuenta que aquí se usa el control etiqueta y que simplemente se está agregando texto estático. Al colocar un control de servidor normal en una zona **WebPartZone** , ASP.net trata el control como un control elementos Web en tiempo de ejecución, lo que habilita elementos Web características en el control.

**Para crear contenido para la zona principal**

1. En la vista **diseño** , arrastre un control **etiqueta** desde la pestaña **estándar** del cuadro de herramientas hasta el área de contenido de la zona cuya propiedad **ID** esté establecida en MainZone.
2. Cambie a la vista **código fuente** . Observe que se ha agregado un elemento &lt;zonetemplate&gt; para ajustar el control **etiqueta** en MainZone.
3. Agregue un atributo denominado **title** al elemento &lt;ASP: Label&gt; y establezca su valor en content. Quite el atributo Text = "etiqueta" del elemento &lt;ASP: Label&gt;. Entre las etiquetas de apertura y cierre del elemento &lt;ASP: Label&gt;, agregue texto como **Bienvenido a mi página principal** dentro de un par de etiquetas de elementos de&gt; H2 &lt;. El código debe tener el siguiente aspecto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Guarde el archivo.

A continuación, cree un control de usuario que también se pueda agregar a la página como un control de elementos web.

### <a name="to-create-a-user-control"></a>Crear un control de usuario

1. Agregue un nuevo control de usuario Web al sitio para que sirva como control de búsqueda. Anule la selección de la opción para **colocar el código fuente en un archivo independiente**. Agréguelo en el mismo directorio que la página WebPartsDemo. aspx y asígnele el nombre SearchUserControl. ascx.   
  
    > [!NOTE]
    > El control de usuario de este tutorial no implementa la funcionalidad de búsqueda real; solo se usa para mostrar elementos web características.
2. Cambie a la vista **diseño** . En la pestaña **estándar** del cuadro de herramientas, arrastre un control TextBox hasta la página.
3. Coloque el punto de inserción después del cuadro de texto que acaba de agregar y presione Entrar para agregar una nueva línea.
4. Arrastre un control de botón a la página de la nueva línea debajo del cuadro de texto que acaba de agregar.
5. Cambie a la vista **código fuente** . Asegúrese de que el código fuente del control de usuario es similar al ejemplo siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Guarde y cierre el archivo.

Ahora puede Agregar controles elementos web a la zona de Sidebar. Agrega dos controles a la zona de la barra lateral, uno que contiene una lista de vínculos y otro que es el control de usuario que creó en el procedimiento anterior. Los vínculos se agregan como un control de servidor de **etiqueta** estándar, similar a la forma en que se creó el texto estático para la zona principal. Sin embargo, aunque los controles de servidor individuales contenidos en el control de usuario pueden estar contenidos directamente en la zona (por ejemplo, el control de etiqueta), en este caso no lo son. En su lugar, forman parte del control de usuario que creó en el procedimiento anterior. Esto muestra una manera común de empaquetar los controles y la funcionalidad adicional que desee en un control de usuario y, a continuación, hacer referencia a ese control en una zona como un control de elementos web.

En tiempo de ejecución, el conjunto de controles elementos web contiene ambos controles con controles de GenericWebPart. Cuando un control **GenericWebPart** ajusta un control de servidor Web, el control de elemento genérico es el control principal y puede tener acceso al control de servidor a través de la propiedad ChildControl del control primario. Este uso de los controles de elementos genéricos permite que los controles de servidor web estándar tengan el mismo comportamiento básico y los mismos atributos que los elementos web controles que derivan de la clase **WebPart** .

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Para agregar controles elementos web a la zona de Sidebar

1. Abra la página WebPartsDemo. aspx.
2. Cambie a la vista **diseño** .
3. Arrastre la página de control de usuario que ha creado, SearchUserControl. ascx, desde **Explorador de soluciones** a la zona cuya propiedad **ID** está establecida en SidebarZone y colóquela ahí.
4. Guarde la página WebPartsDemo. aspx.
5. Cambie a la vista **código fuente** .
6. Dentro del elemento &lt;ASP: webpartzone&gt; para SidebarZone, justo encima de la referencia al control de usuario, agregue un elemento &lt;ASP: Label&gt; con vínculos contenidos, tal como se muestra en el ejemplo siguiente. Además, agregue un atributo **title** a la etiqueta de control de usuario, con un valor de **Search**, como se muestra. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Guarde y cierre el archivo.

Ahora puede probar la página desplazándose hasta ella en el explorador. En la página se muestran las dos zonas. En la captura de pantalla siguiente se muestra la página.

**elementos web página de demostración con dos zonas**

![Captura de pantalla de elementos web Tutorial 1 de VS](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: captura de pantalla de elementos Web tutorial 1 de vs

En la barra de título de cada control hay una flecha hacia abajo que proporciona acceso a un menú de verbos de las acciones disponibles que se pueden realizar en un control. Haga clic en el menú de verbos de uno de los controles y, a continuación, haga clic en el verbo **minimizar** y observe que el control está minimizado. En el menú de verbos, haga clic en **restaurar**y el control vuelve a su tamaño normal.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permitir a los usuarios editar páginas y cambiar el diseño

Elementos web proporciona la capacidad de los usuarios de cambiar el diseño de los controles de elementos web arrastrándolos de una zona a otra. Además de permitir a los usuarios trasladar controles de **elementos Web** de una zona a otra, puede permitir que los usuarios editen diversas características de los controles, incluida la apariencia, el diseño y el comportamiento. El conjunto de controles de elementos web proporciona funcionalidad de edición básica para los controles de **elementos Web** . Aunque no lo hará en este tutorial, también puede crear controles de editor personalizados que permitan a los usuarios editar las características de los controles de **elementos Web** . Al igual que cuando se cambia la ubicación de un control **WebPart** , la edición de las propiedades de un control se basa en la personalización de ASP.net para guardar los cambios que realizan los usuarios.

En esta parte del tutorial, agregará la capacidad de los usuarios de modificar las características básicas de cualquier control **WebPart** en la página. Para habilitar estas características, agregue otro control de usuario personalizado a la página, junto con un elemento &lt;ASP: EditorZone&gt; y dos controles de edición.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Para crear un control de usuario que permita cambiar el diseño de página

1. En Visual Studio, en el menú **archivo** , seleccione el submenú **nuevo** y haga clic en la opción **archivo** .
2. En el cuadro de diálogo **Agregar nuevo elemento** , seleccione **control de usuario Web**. Asigne al nuevo archivo el nombre DisplayModeMenu. ascx. Anule la selección de la opción para **colocar el código fuente en un archivo independiente**.
3. Haga clic en Agregar para crear el nuevo control.
4. Cambie a la vista **código fuente** .
5. Quite todo el código existente en el nuevo archivo y pegue el código siguiente. Este código de control de usuario utiliza características del conjunto de controles elementos web que permiten a una página cambiar su vista o modo de visualización, y también permite cambiar el aspecto físico y el diseño de la página mientras se encuentre en determinados modos de presentación. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Guarde el archivo haciendo clic en el icono guardar de la barra de herramientas o seleccionando **Guardar** en el menú **archivo** .

### <a name="to-enable-users-to-change-the-layout"></a>Para permitir que los usuarios cambien el diseño

1. Abra la página WebPartsDemo. aspx y cambie a la vista **diseño** .
2. Coloque el punto de inserción en la vista de **diseño** justo después del control **WebPartManager** que agregó anteriormente. Agregue una devolución fuerte después del texto para que haya una línea en blanco después del control **WebPartManager** . Coloque el punto de inserción en la línea en blanco.
3. Arrastre el control de usuario que acaba de crear (el archivo se denomina DisplayModeMenu. ascx) a la página WebPartsDemo. aspx y colóquelo en la línea en blanco.
4. Arrastre un control EditorZone desde la sección **elementos Web** del cuadro de herramientas hasta la celda de tabla abierta restante en la página WebPartsDemo. aspx.
5. En la sección **elementos Web** del cuadro de herramientas, arrastre un control AppearanceEditorPart y un control LayoutEditorPart al control **EditorZone** .
6. Cambie a la vista **código fuente** . El código resultante de la celda de la tabla debe ser similar al código siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Guarde el archivo WebPartsDemo. aspx. Ha creado un control de usuario que le permite cambiar los modos de presentación y cambiar el diseño de página, y ha hecho referencia al control en la página web principal.

Ahora puede probar la capacidad de editar páginas y cambiar el diseño.

### <a name="to-test-layout-changes"></a>Para probar los cambios de diseño

1. Cargue la página en un explorador.
2. Haga clic en el menú desplegable **modo de presentación** y seleccione **Editar**. Se muestran los títulos de zona.
3. Arrastre el control **mis vínculos** por la barra de título desde la zona de la barra lateral hasta la parte inferior de la zona principal. La página debe ser similar a la siguiente captura de pantalla.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>elementos web página de demostración con el control mis vínculos descartado

![elementos web captura de pantalla del tutorial 2 de VS](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: captura de pantalla de elementos Web tutorial de vs 2

1. Haga clic en el menú desplegable **modo de presentación** y seleccione **examinar**. La página se actualiza, los nombres de zona desaparecen y el control **mis vínculos** permanece en el lugar donde se colocó.
2. Para demostrar que la personalización funciona, cierre el explorador y, a continuación, vuelva a cargar la página. Los cambios realizados se guardan para futuras sesiones del explorador.
3. En el menú **modo de presentación** , seleccione **Editar**.   
  
   Cada control de la página se muestra ahora con una flecha hacia abajo en la barra de título, que contiene el menú desplegable de verbos.
4. Haga clic en la flecha para mostrar el menú de verbos en el control **mis vínculos** . Haga clic en el verbo **Editar** .   
  
   Aparece el control **EditorZone** y muestra los controles EditorPart que ha agregado.
5. En la sección **apariencia** del control de edición, cambie el **título** a mis favoritos, use la lista desplegable **tipo de cromo** para seleccionar **solo el título**y, a continuación, haga clic en **aplicar**. La siguiente captura de pantalla muestra la página en modo de edición.

### <a name="web-parts-demo-page-in-edit-mode"></a>elementos web página de demostración en modo de edición

![Captura de pantalla del tutorial de VS 3 de elementos web](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: captura de pantalla de elementos Web tutorial de vs 3

1. Haga clic en el menú **modo de presentación** y seleccione **examinar** para volver al modo de exploración.
2. Ahora el control tiene un título actualizado y ningún borde, como se muestra en la siguiente captura de pantalla.

### <a name="edited-web-parts-demo-page"></a>Página de demostración de elementos web editada

![Captura de pantalla del tutorial 4 de elementos web VS](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: captura de pantalla de elementos Web tutorial 4 de vs

### <a name="adding-web-parts-at-run-time"></a>Agregar elementos web en tiempo de ejecución

También puede permitir que los usuarios agreguen elementos web controles a su página en tiempo de ejecución. Para ello, configure la página con un catálogo de elementos web, que contiene una lista de los controles de elementos web que desea poner a disposición de los usuarios.

**Para permitir que los usuarios agreguen elementos web en tiempo de ejecución**

1. Abra la página WebPartsDemo. aspx y cambie a la vista **diseño** .
2. En la pestaña **elementos Web** del cuadro de herramientas, arrastre un control CatalogZone a la columna derecha de la tabla, debajo del control **EditorZone** .   
  
   Ambos controles pueden estar en la misma celda de la tabla porque no se mostrarán al mismo tiempo.
3. En el panel Propiedades, asigne la cadena **Add elementos Web** a la propiedad HeaderText del control **CatalogZone** .
4. En la sección **elementos Web** del cuadro de herramientas, arrastre un control DeclarativeCatalogPart hasta el área de contenido del control **CatalogZone** .
5. Haga clic en la flecha situada en la esquina superior derecha del control **DeclarativeCatalogPart** para exponer su menú tareas y, a continuación, seleccione **editar plantillas**.
6. En la sección **estándar** del cuadro de herramientas, arrastre un control **FileUpload** y un control **Calendar** a la sección **webpartstemplate** del control **DeclarativeCatalogPart** .
7. Cambie a la vista **código fuente** . Inspeccione el código fuente del elemento &lt;ASP: CatalogZone&gt;. Observe que el control **DeclarativeCatalogPart** contiene un elemento &lt;webpartstemplate&gt; con los dos controles de servidor delimitados que se pueden agregar a la página desde el catálogo.
8. Agregue una propiedad **title** a cada uno de los controles que agregó al catálogo mediante el valor de cadena que se muestra para cada título en el ejemplo de código siguiente. Aunque el título no es una propiedad que normalmente se puede establecer en estos dos controles de servidor en tiempo de diseño, cuando un usuario agrega estos controles a una zona de **WebPartZone** desde el catálogo en tiempo de ejecución, cada uno se encapsula con un control **GenericWebPart** . Esto les permite actuar como controles elementos web, por lo que podrán mostrar los títulos.   
  
   El código de los dos controles contenidos en el control **DeclarativeCatalogPart** debe tener el aspecto siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Guarde la página.

Ahora puede probar el catálogo.

### <a name="to-test-the-web-parts-catalog"></a>Para probar el catálogo de elementos web

1. Cargue la página en un explorador.
2. Haga clic en el menú desplegable **modo de presentación** y seleccione **Catálogo**.   
  
   Se muestra el catálogo titulado **agregar elementos Web** .
3. Arrastre el control **Mis Favoritos** desde la zona principal hacia la parte superior de la zona de la barra lateral y colóquelo ahí.
4. En agregar catálogo de **elementos Web** , Active ambas casillas y, a continuación, seleccione **Main** en la lista desplegable que contiene las zonas disponibles.
5. Haga clic en **Agregar** en el catálogo. Los controles se agregan a la zona principal. Si lo desea, puede agregar varias instancias de controles desde el catálogo a la página.   
  
   En la captura de pantalla siguiente se muestra la página con el control de carga de archivos y el calendario en la zona principal. 

![Controles agregados a la zona principal del catálogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Haga clic en el menú desplegable **modo de presentación** y seleccione **examinar**. El catálogo desaparece y se actualiza la página.
7. Cierre el explorador. Vuelva a cargar la página. Los cambios realizados se conservan.
