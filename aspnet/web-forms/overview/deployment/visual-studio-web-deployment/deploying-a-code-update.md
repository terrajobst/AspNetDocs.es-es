---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implementación web de ASP.NET con Visual Studio: implementación de una actualización de código | Microsoft Docs'
author: tdykstra
description: En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros, por usa...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626777"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Implementación web de ASP.NET con Visual Studio: implementación de una actualización de código

por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> En esta serie de tutoriales se muestra cómo implementar (publicar) una aplicación Web de ASP.NET en Azure App Service Web Apps o en un proveedor de hospedaje de terceros mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información sobre la serie, vea [el primer tutorial de la serie](introduction.md).

## <a name="overview"></a>Información general del

Después de la implementación inicial, el trabajo de mantenimiento y desarrollo del sitio web continúa y antes de que se desee implementar una actualización. Este tutorial le guiará por el proceso de implementación de una actualización en el código de la aplicación. La actualización que implementa e implementa en este tutorial no implica un cambio en la base de datos; en el siguiente tutorial, verá qué es diferente de la implementación de un cambio en la base de datos.

Aviso: Si recibe un mensaje de error o algo no funciona a medida que avanza en el tutorial, asegúrese de consultar la [Página de solución de problemas](troubleshooting.md).

## <a name="make-a-code-change"></a>Hacer un cambio de código

Como ejemplo sencillo de una actualización de la aplicación, agregará a la página de **instructores** una lista de cursos impartidos por el instructor seleccionado.

Si ejecuta la página **instructores** , observará que hay vínculos **seleccionados** en la cuadrícula, pero que no hacen nada más que hacer que el fondo de la fila se convierta en gris.

![Página de instructores con selección](deploying-a-code-update/_static/image1.png)

Ahora agregará el código que se ejecuta cuando se hace clic en el vínculo **seleccionar** y muestra una lista de los cursos impartidos por el instructor seleccionado.

1. En *instructors. aspx*, agregue el siguiente marcado inmediatamente después del control **ErrorMessageLabel** `Label`:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Ejecute la página y seleccione un instructor. Verá una lista de los cursos impartidos por ese instructor.

    ![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image2.png)
3. Cierre el explorador.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Implementar la actualización del código en el entorno de prueba

Antes de poder usar los perfiles de publicación para implementar en pruebas, ensayo y producción, debe cambiar las opciones de publicación de bases de datos. Ya no es necesario ejecutar los scripts de implementación de datos y de concesión para la base de datos de pertenencia.

1. Para abrir el Asistente para **publicación web** , haga clic con el botón secundario en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Haga clic en el perfil de **prueba** en la lista desplegable **perfil** .
3. Haga clic en la pestaña **configuración** .
4. En **DefaultConnection** en la sección **bases** de datos, desactive la casilla **Actualizar base de datos** .
5. Haga clic en la pestaña **perfil** y, a continuación, haga clic en el perfil de **almacenamiento provisional** en la lista desplegable **perfil** .
6. Cuando se le pregunte si desea guardar los cambios realizados en el perfil de **prueba** , haga clic en **sí**.
7. Realice el mismo cambio en el perfil de almacenamiento provisional.
8. Repita el proceso para hacer el mismo cambio en el perfil de producción.
9. Cierre el Asistente para **publicación web** .

La implementación en el entorno de prueba es ahora una cuestión sencilla para ejecutar de nuevo la publicación con un solo clic. Para que este proceso sea más rápido, puede usar la barra de herramientas de **publicación de un solo clic** .

1. En el menú **Ver** , elija **barras de herramientas** y, después, **haga clic en publicación web**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. En **Explorador de soluciones**, seleccione el proyecto ContosoUniversity.
3. en la barra de herramientas de **publicación de un solo clic** , elija el perfil de publicación de **prueba** y, a continuación, haga clic en **publicar web** (el icono con flechas que apuntan a la izquierda y a la derecha)

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio implementa la aplicación actualizada y el explorador se abre automáticamente en la Página principal.
5. Ejecute la página Instructors y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

Normalmente también realizaría pruebas de regresión (es decir, probar el resto del sitio para asegurarse de que el nuevo cambio no interrumpió ninguna funcionalidad existente). Pero para este tutorial omitirá ese paso y continuará con la implementación de la actualización en el entorno de ensayo y la producción.

Al volver a implementar, Web Deploy determina automáticamente qué archivos han cambiado y solo copia los archivos modificados en el servidor. De forma predeterminada, Web Deploy utiliza fechas de última modificación en los archivos para determinar cuáles han cambiado. Algunos sistemas de control de código fuente cambian las fechas del archivo incluso cuando no se cambia el contenido del archivo. En ese caso, es posible que desee configurar Web Deploy para usar sumas de comprobación de archivos con el fin de determinar qué archivos han cambiado. Para obtener más información, consulte [¿por qué se vuelve a implementar todos los archivos, aunque no los he cambiado?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) en las preguntas más frecuentes sobre la implementación de ASP.net.

## <a name="take-the-application-offline-during-deployment"></a>Poner la aplicación sin conexión durante la implementación

El cambio que está implementando ahora es un cambio sencillo en una sola página. Pero a veces se implementan cambios mayores, o se implementan cambios en el código y en la base de datos, y es posible que el sitio se comporte de forma incorrecta si un usuario solicita una página antes de que finalice la implementación. Para evitar que los usuarios accedan al sitio mientras la implementación está en curso, puede usar una *aplicación\_archivo. htm sin conexión* . Cuando se coloca un archivo con el nombre *app\_offline. htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación. Por lo tanto, para evitar el acceso durante la implementación, coloque *app\_offline. htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quite *App\_offline. htm* después de la implementación correcta.

Puede configurar Web Deploy para colocar automáticamente una aplicación predeterminada *\_archivo. htm sin conexión* en el servidor cuando comience a implementarse y quitarla cuando finalice. Para ello, agregue el siguiente elemento XML al archivo de Perfil de publicación (. pubxml):

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

En este tutorial, verá cómo crear y usar una *aplicación personalizada\_archivo. htm sin conexión* .

No es necesario usar *app\_offline. htm* en el sitio de ensayo, ya que no tiene usuarios que accedan al sitio de ensayo. Pero es recomendable usar el almacenamiento provisional para probar todo el modo en que planea implementarlo en producción.

### <a name="create-app_offlinehtm"></a>Crear aplicación\_sin conexión. htm

1. En **Explorador de soluciones**, haga clic con el botón secundario en la solución, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.
2. Cree una **página HTML** denominada *App\_offline. htm* (elimine la "l" final en la extensión *. html* que Visual Studio crea de forma predeterminada).
3. Reemplace el marcado de la plantilla por el marcado siguiente:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Guarde y cierre el archivo.

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a>Copie la aplicación\_offline. htm en la carpeta raíz del sitio Web.

Puede usar cualquier herramienta FTP para copiar archivos en el sitio Web. [FileZilla](http://filezilla-project.org/) es una conocida herramienta de FTP y se muestra en las capturas de pantalla.

Para usar una herramienta de FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña.

La dirección URL se muestra en la página panel del sitio web de Azure Portal de administración, y el nombre de usuario y la contraseña para FTP se pueden encontrar en el archivo *. publishsettings* que descargó anteriormente. En los pasos siguientes se muestra cómo obtener estos valores.

1. En el Portal de administración de Azure, haga clic en la pestaña **sitios web** y, a continuación, haga clic en el sitio web de ensayo.
2. En la página **Panel** , desplácese hacia abajo para buscar el nombre de host FTP en la sección **vista rápida** .

    ![Nombre de host FTP](deploying-a-code-update/_static/image5.png)
3. Abra el archivo staging *. publishsettings* en el Bloc de notas o en otro editor de texto.
4. Busque el elemento `publishProfile` del perfil FTP.
5. Copie los valores de `userName` y `userPWD`.

    ![Credenciales de FTP](deploying-a-code-update/_static/image6.png)
6. Abra la herramienta FTP e inicie sesión en la dirección URL de FTP.
7. Copie *app\_offline. htm* de la carpeta de la solución en la carpeta */site/wwwroot* del sitio de ensayo.

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. Vaya a la dirección URL del sitio de ensayo. Verá que la *aplicación\_página offline. htm* aparece ahora en lugar de la Página principal.

    ![app_offline. htm en la ventana del explorador](deploying-a-code-update/_static/image8.png)

Ahora está listo para realizar la implementación en el almacenamiento provisional.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Implementación de la actualización del código en ensayo y producción

1. En la barra de herramientas de **publicación en Web** , elija el perfil de publicación de **ensayo** y, a continuación, haga clic en **publicar web**.

    Visual Studio implementa la aplicación actualizada y abre el explorador en la Página principal del sitio. Se muestra la *aplicación\_archivo. htm sin conexión* . Antes de que pueda realizar pruebas para comprobar que la implementación se ha realizado correctamente, debe quitar la *aplicación\_archivo. htm sin conexión* .
2. Vuelva a la herramienta FTP y elimine la **aplicación\_offline. htm** del sitio de ensayo.
3. En el explorador, abra la página de instructores en el sitio de ensayo y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.
4. Siga el mismo procedimiento para producción que para el almacenamiento provisional.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisar los cambios e implementar archivos específicos

Visual Studio 2012 también ofrece la posibilidad de implementar archivos individuales. En el caso de un archivo seleccionado, puede ver las diferencias entre la versión local y la versión implementada, implementar el archivo en el entorno de destino o copiar el archivo del entorno de destino en el proyecto local. En esta sección del tutorial, verá cómo usar estas características.

### <a name="make-a-change-to-deploy"></a>Realizar un cambio en la implementación

1. Abra *Content/site. CSS*y busque el bloque de la etiqueta `body`.
2. Cambie el valor de `background-color` de `#fff` a `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Ver el cambio en la ventana de vista previa de publicación

Al usar el Asistente para **publicación web** para publicar el proyecto, puede ver los cambios que se van a publicar haciendo doble clic en el archivo en la ventana de **vista previa** .

1. Haga clic con el botón derecho en el proyecto ContosoUniversity y haga clic en **publicar**.
2. En la lista desplegable **perfil** , seleccione el perfil de publicación de **prueba** .
3. Haga clic en **vista previa**y, a continuación, en **iniciar vista previa**.
4. En el panel de **vista previa** , haga doble clic en **site. CSS**.

    ![Haga doble clic en site. CSS](deploying-a-code-update/_static/image9.png)

    El cuadro de diálogo **vista previa** de los cambios muestra una vista previa de los cambios que se van a implementar.

    ![Vista previa de los cambios en site. CSS](deploying-a-code-update/_static/image10.png)

    Si hace doble clic en el archivo *Web. config* , el cuadro de diálogo **vista previa de los cambios** muestra el efecto de las transformaciones de configuración de compilación y las transformaciones de Perfil de publicación. En este momento, no ha hecho nada que haría que el archivo *Web. config* en el servidor cambiara, por lo que espera no ver ningún cambio. Sin embargo, la ventana **vista previa de cambios** muestra incorrectamente dos cambios. Parece que se quitarán dos elementos XML. Estos elementos se agregan mediante el proceso de publicación cuando se selecciona **ejecutar migraciones de Code First en el inicio** de la aplicación para una clase de contexto Code First. La comparación se realiza antes de que el proceso de publicación Agregue esos elementos, por lo que parece que se quitan, aunque no se quitarán. Este error se corregirá en una versión futura.
5. Haga clic en **Cerrar**.
6. Haga clic en **Publicar**.
7. Cuando el explorador se abra en la Página principal del sitio de prueba, presione CTRL + F5 para que se produzca una actualización de hardware con el fin de ver el efecto del cambio de CSS.

    ![Efecto del cambio de CSS](deploying-a-code-update/_static/image11.png)
8. Cierre el explorador.

### <a name="publish-specific-files-from-solution-explorer"></a>Publicar archivos específicos desde Explorador de soluciones

Supongamos que no le gusta el fondo azul y desea revertir al color original. En esta sección se restaurará la configuración original mediante la publicación de un archivo específico directamente desde **Explorador de soluciones**.

1. Abra *contenido/sitio. CSS* y restaure el valor de `background-color` en `#fff`.
2. En **Explorador de soluciones**, haga clic con el botón secundario en el archivo *Content/site. CSS* .

    En el menú contextual se muestran tres opciones de publicación.

    ![Opciones de publicación de Explorador de soluciones](deploying-a-code-update/_static/image12.png)
3. Haga clic en **vista previa de cambios en site. CSS**.

    Se abre una ventana para mostrar las diferencias entre el archivo local y la versión del mismo en el entorno de destino.

    ![Diff-Content/site. CSS](deploying-a-code-update/_static/image13.png)
4. En **Explorador de soluciones**, haga clic con el botón derecho en **site. CSS** de nuevo y haga clic en **Publicar sitio. CSS**.

    La ventana **actividad de publicación web** muestra que el archivo se ha publicado.

    ![Ventana actividad de publicación Web](deploying-a-code-update/_static/image14.png)
5. Abra un explorador en la dirección URL de `http://localhost/contosouniversity` y, a continuación, presione CTRL + F5 para producir una actualización de hardware con el fin de ver el efecto del cambio de CSS.

    ![Página principal con CSS normal](deploying-a-code-update/_static/image15.png)
6. Cierre el explorador.

## <a name="summary"></a>Resumen

Ahora ha visto varias maneras de implementar una actualización de la aplicación que no implica un cambio en la base de datos y ha visto cómo obtener una vista previa de los cambios para comprobar que lo que se va a actualizar es lo que espera. La página Instructors tiene ahora una sección de **cursos impartidos** .

![Página de instructores con cursos impartidos](deploying-a-code-update/_static/image16.png)

En el siguiente tutorial se muestra cómo implementar un cambio en la base de datos: agregará un campo BIRTHDATE a la base de datos y a la página instructores.

> [!div class="step-by-step"]
> [Anterior](deploying-to-production.md)
> [Siguiente](deploying-a-database-update.md)
