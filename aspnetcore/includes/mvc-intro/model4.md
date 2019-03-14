---
ms.openlocfilehash: 568fa161b27e554fd8b474670b8f9a7ec2f39f45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047132"
---
En la tabla siguiente se incluyen los detalles de los parámetros del generador de código de ASP.NET Core:

| Parámetro               | Descripción|
| ----------------- | ------------ |
| -m  | Nombre del modelo |
| -dc  | Contexto de datos |
| -udl | Usa el diseño predeterminado. |
| --relativeFolderPath | Ruta de acceso relativa de la carpeta de salida para crear las vistas |
| --useDefaultLayout | Se debe usar el diseño predeterminado para las vistas. |
| --referenceScriptLibraries | Agrega `_ValidationScriptsPartial` a las páginas Editar y Crear. |

Active o desactive `h` para obtener ayuda con el comando `aspnet-codegenerator controller`:

```console
dotnet aspnet-codegenerator controller -h
```