---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57063992"
---
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="67d4f-101">Prueba de la aplicación</span><span class="sxs-lookup"><span data-stu-id="67d4f-101">Test the app</span></span>

* <span data-ttu-id="67d4f-102">Ejecute la aplicación y anexe `/Movies` a la dirección URL en el explorador (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="67d4f-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="67d4f-103">Pruebe el vínculo **Crear**.</span><span class="sxs-lookup"><span data-stu-id="67d4f-103">Test the **Create** link.</span></span>

  ![Página Crear](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="67d4f-105">Pruebe los vínculos **Editar**, **Detalles** y **Eliminar**.</span><span class="sxs-lookup"><span data-stu-id="67d4f-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="67d4f-106">Si recibe el error siguiente, compruebe que haya realizado las migraciones y que la base de datos esté actualizada:</span><span class="sxs-lookup"><span data-stu-id="67d4f-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```
