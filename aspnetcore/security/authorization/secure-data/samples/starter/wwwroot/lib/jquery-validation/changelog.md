---
ms.openlocfilehash: 56ee26dd77bb1b678b54b6776741f6547889fa08
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57040422"
---
<a name="1140--2015-06-30"></a>1.14.0 / 2015-06-30
==================

## <a name="core"></a>Principal
  * Quitar el método removeAttrs sin utilizar
  * Reemplazar regex para el método URL
  * Quitar el parámetro de URL incorrecta en $.ajax, sobrescrito por $.extend
  * Controlar correctamente el botón de envío de cancelación anidado
  * Corregir sangría
  * Refactorizar attributeRules y dataRules para compartir el normalizador
  * Método dataRules para convertir el valor al número de entradas numéricas
  * Actualizar método de URL para permitir direcciones URL relativas al protocolo
  * Quitar el marcador de posición $.format en desuso
  * Usar jQuery 1.7+ on/off y agregar método destroy
  * La compatibilidad de IE8 cambió .indexOf por $.inArray
  * Convertir atributos de valor NaN en valores sin definir para Opera Mini
  * Detener el valor de recorte dentro del método requerido
  * Usar el selector :disabled para que coincida con los elementos deshabilitados
  * Excluir algunas teclas del teclado para evitar la revalidación del campo
  * No buscar en el DOM completo los elementos de radio y casillas
  * Generar errores mejorados para métodos de regla incorrecta
  * Error de validación de número corregido
  * Corregir referencia a la especificación whatwg
  * Centrarse en el elemento no válido al validar un conjunto personalizado de entradas
  * Restablecer los estilos de elemento cuando se usan métodos de resaltado personalizados
  * Ignorar símbolo del dólar en el Id. de error
  * Revertir "Ignorar campos de solo lectura y deshabilitados"
  * Actualizar vínculo del comentario para algoritmo de Luhn

## <a name="additionals"></a>Otros
  * Actualizar dateITA para solucionar problemas de zona horaria
  * Corregir método de extensión solo con el período del método
  * Corregir el método de aceptación solo para coincidir con el período
  * Corregir el método de tiempo para permitir horas de un solo dígito
  * Anular prueba incorrecta del método notEqualTo
  * Agregar el método notEqualTo
  * Usar la referencia de jQuery correcta a través de `$`
  * Quitar marca de regex sin usar en el método iban
  * Número de CPF de Brasil

## <a name="localization"></a>Localización
  * Actualizar messages_tr.js
  * Actualizar messages_sr_lat.js
  * Agregar español de Perú (ES PE)
  * Agregar georgiano (ქართული, ge)
  * Error de escritura corregido en la traducción de catalán
  * Mejorar la traducción de finés (fi)
  * Agregar configuración regional armenia (hy_AM)
  * Extender traducción de italiano (it) con el método de moneda
  * Agregar configuración regional de bn_BD
  * Actualizar configuración regional de zh
  * Quitar el punto al final de los mensajes italianos

<a name="1131--2014-10-14"></a>1.13.1 / 2014-10-14
==================

## <a name="core"></a>Principal
  * Permitir 0 como valor de autoCreateRanges
  * Aplicar Omitir configuración a todos los elementos de validationTargetFor
  * No recortar el valor de los métodos min/max/rangelength
  * Ignorar el identificador o nombre antes de usarlos como un selector en errorsFor
  * Explicitar valor predeterminado para la opción focusCleanup
  * Corregir regexp incorrecto para el buscador de coincidencias describedby
  * Ignorar campos de solo lectura y deshabilitados
  * Mejorar el escape del identificador; almacenar identificador ignorado en describedby
  * Usar el valor devuelto de submitHandler para permitir o evitar el envío del formulario

## <a name="additionals"></a>Otros
  * Agregar método postalcodeBR
  * Corregir método del patrón si el parámetro es una cadena


<a name="1130--2014-07-01"></a>1.13.0 / 2014-07-01
==================

## <a name="all"></a>Todas
* Agregar contenedor UMD como complemento

## <a name="core"></a>Principal
* Respetar aria-describedby sin error y errores ocultos vacíos
* Mejorar dateISO RegExp
* Radio o casilla agregados para delegar el evento de clic
* Usar aria-describedby para elementos no etiquetados
* Registrar focusin, focusout y keyup también en radio o casilla
* Corregir normalización para el valor de atributo rangelength
* Actualizar método elementValue para abordar los campos type="number"
* Usar charAt en lugar de una notación de matriz de cadenas, para admitir IE8(?)

## <a name="localization"></a>Localización
* Corregir traducción sk del método rangelength
* Agregar métodos fineses
* Mensaje de validación de número de GL corregido
* Mensaje de validación de método numérico ES corregido
* Gallego (GL) agregado
* Mensajes de francés corregidos para métodos min y max

## <a name="additionals"></a>Otros
* Agregar método statesUS
* Corregir método dateITA para abordar errores DST
* Agregar método de fecha persa
* Agregar método postalCodeCA
* Agregar método postalcodeIT

<a name="1120--2014-04-01"></a>1.12.0 / 2014-04-01
==================

* Agregar pruebas ARIA ([3d5658e](https://github.com/jzaefferer/jquery-validation/commit/3d5658e9e4825fab27198c256beed86f0bd12577))
* Agregar mensajes de localización es-AR. ([7b30beb](https://github.com/jzaefferer/jquery-validation/commit/7b30beb8ebd218c38a55d26a63d529e16035c7a2))
* Agregar los puntos que faltan en los mensajes de "es" y "es_AR". ([a2a653c](https://github.com/jzaefferer/jquery-validation/commit/a2a653cb68926ca034b4b09d742d275db934d040))
* Localización indonesia (ID) agregada ([1d348bd](https://github.com/jzaefferer/jquery-validation/commit/1d348bdcb65807c71da8d0bfc13a97663631cd3a))
* Validación agregada de números de documentos españoles NIF, NIE y CIF ([#830](https://github.com/jzaefferer/jquery-validation/issues/830), [317c20f](https://github.com/jzaefferer/jquery-validation/commit/317c20fa9bb772770bb9b70d46c7081d7cfc6545))
* Formulario actual agregado al contexto de la solicitud ajax remota ([0a18ae6](https://github.com/jzaefferer/jquery-validation/commit/0a18ae65b9b6d877e3d15650a5c2617a9d2b11d5))
* Otros: Actualizar método IBAN; recortar espacios en blanco finales ([#970](https://github.com/jzaefferer/jquery-validation/issues/970), [347b04a](https://github.com/jzaefferer/jquery-validation/commit/347b04a7d4e798227405246a5de3fc57451d52e1))
* {Método BIC: Mejorar RegEx, {1} siempre es redundante. Cierra gh-744 ([5cad6b4](https://github.com/jzaefferer/jquery-validation/commit/5cad6b493575e8a9a82470d17e0900c881130873))
* Bower: Agregar Bower.json para registro de paquetes ([e86ccb0](https://github.com/jzaefferer/jquery-validation/commit/e86ccb06e301613172d472cf15dd4011ff71b398))
* Cambia referencias de dólar a "jQuery", para compatibilidad con jQuery.noConflict. Cierra gh-754 ([2049afe](https://github.com/jzaefferer/jquery-validation/commit/2049afe46c1be7b3b89b1d9f0690f5bebf4fbf68))
* Principal: Agregue el campo "method" a la entrada de la lista de errores ([89a15c7](https://github.com/jzaefferer/jquery-validation/commit/89a15c7a4b17fa2caaf4ff817f09b04c094c3884))
* Principal: Agrega compatibilidad para mensajes genéricos a través del atributo data-msg ([5bebaa5](https://github.com/jzaefferer/jquery-validation/commit/5bebaa5c55c73f457c0e0181ec4e3b0c409e2a9d))
* Principal: Permitir que los atributos tienen un valor de cero (eg min = '0') ([#854](https://github.com/jzaefferer/jquery-validation/issues/854), [9dc0d1d](https://github.com/jzaefferer/jquery-validation/commit/9dc0d1dd946b2c6178991fb16df0223c76162579))
* Principal: Deshabilitar $.format en desuso ([#755](https://github.com/jzaefferer/jquery-validation/issues/755), [bf3b350](https://github.com/jzaefferer/jquery-validation/commit/bf3b3509140ea8ab5d83d3ec58fd9f1d7822efc5))
* Principal: Corregir compatibilidad para varias clases de error ([c1f0baf](https://github.com/jzaefferer/jquery-validation/commit/c1f0baf36c21ca175bbc05fb9345e5b44b094821))
* Principal: Omita algunos eventos en elementos ignorados ([#700](https://github.com/jzaefferer/jquery-validation/issues/700), [a864211](https://github.com/jzaefferer/jquery-validation/commit/a86421131ea69786ee9e0d23a68a54a7658ccdbf))
* Principal: Mejorar el método elementValue ([6c041ed](https://github.com/jzaefferer/jquery-validation/commit/6c041edd21af1425d12d06cdd1e6e32a78263e82))
* Principal: Que element() administre los elementos ignorados correctamente. ([3f464a8](https://github.com/jzaefferer/jquery-validation/commit/3f464a8da49dbb0e4881ada04165668e4a63cecb))
* Principal: Cambie el análisis de dataRules al estilo de especificaciones W3C HTML5 ([460fd22](https://github.com/jzaefferer/jquery-validation/commit/460fd22b6c84a74c825ce1fa860c0a9da20b56bb))
* Principal: Desencadenar opcionales, pero tener otros validadores correctos ([#851](https://github.com/jzaefferer/jquery-validation/issues/851), [f93e1de](https://github.com/jzaefferer/jquery-validation/commit/f93e1deb48ec8b3a8a54e946a37db2de42d3aa2a))
* Principal: Usar elemento sin formato en lugar de ajuste sin el elemento nuevo ([03cd4c9](https://github.com/jzaefferer/jquery-validation/commit/03cd4c93069674db5415a0bf174a5870da47e5d2))
* Principal: asegurarse de que remote se ejecuta en último lugar ([#711](https://github.com/jzaefferer/jquery-validation/issues/711), [ad91b6f](https://github.com/jzaefferer/jquery-validation/commit/ad91b6f388b7fdfb03b74e78554cbab9fd8fca6f))
* Demostración: Usar opción correcta en demostración de varias partes. ([#1025](https://github.com/jzaefferer/jquery-validation/issues/1025), [070edc7](https://github.com/jzaefferer/jquery-validation/commit/070edc7be4de564cb74cfa9ee4e3f40b6b70b76f))
* Corregir uso de $/jQuery en métodos adicionales. Corrige #839 ([#839](https://github.com/jzaefferer/jquery-validation/issues/839), [59bc899](https://github.com/jzaefferer/jquery-validation/commit/59bc899e4586255a4251903712e813c21d25b3e1))
* Mejorar las traducciones de chino ([1a0bfe3](https://github.com/jzaefferer/jquery-validation/commit/1a0bfe32b16f8912ddb57388882aa880fab04ffe))
* Implementación inicial de ARIA-Required ([bf3cfb2](https://github.com/jzaefferer/jquery-validation/commit/bf3cfb234ede2891d3f7e19df02894797dd7ba5e))
* Localización: cambiar valores aceptados de la extensión. Corrige #771, cierra gh-793. ([#771](https://github.com/jzaefferer/jquery-validation/issues/771), [12edec6](https://github.com/jzaefferer/jquery-validation/commit/12edec66eb30dc7e86756222d455d49b34016f65))
* Mensajes: Agregar localización de Islandés ([dc88575](https://github.com/jzaefferer/jquery-validation/commit/dc885753c8872044b0eaa1713cecd94c19d4c73d))
* Mensajes: Agregue los puntos que faltan a los mensajes de "bg", "fr" y "sr". ([adbc636](https://github.com/jzaefferer/jquery-validation/commit/adbc6361c377bf6b74c35df9782479b1115fbad7))
* Mensajes: Create messages_sr_lat.js ([f2f9007](https://github.com/jzaefferer/jquery-validation/commit/f2f90076518014d98495c2a9afb9a35d45d184e6))
* Mensajes: Create messages_tj.js ([de830b3](https://github.com/jzaefferer/jquery-validation/commit/de830b3fd8689a7384656c17565ee92c2878d8a5))
* Mensajes: Corregir traducción de sr_lat; agregue el espacio que falta ([880ba1c](https://github.com/jzaefferer/jquery-validation/commit/880ba1ca545903a41d8c5332fc4038a7e9a580bd))
* Mensajes: Actualizar messages_sr.js; corregir espacio que falta ([10313f4](https://github.com/jzaefferer/jquery-validation/commit/10313f418c18ea75f385248468c2d3600f136cfb))
* Métodos: Agregar método adicional para moneda ([1a981b4](https://github.com/jzaefferer/jquery-validation/commit/1a981b440346620964c87ebdd0fa03246348390e))
* Métodos: Agregar comillas tipográficas a la eliminación de puntuación de stripHTML ([aa0d624](https://github.com/jzaefferer/jquery-validation/commit/aa0d6241c3ea04663edc1e45ed6e6134630bdd2f))
* Métodos: Corregir método dateITA, evitar errores de horario de verano ([279b932](https://github.com/jzaefferer/jquery-validation/commit/279b932c1267b7238e6652880b7846ba3bbd2084))
* Métodos: Métodos localizados para cultura Chilena (es-CL) ([cf36b93](https://github.com/jzaefferer/jquery-validation/commit/cf36b933499e435196d951401221d533a4811810))
* Métodos: Actualizar el correo electrónico para usar HTML5 regex, quitar el método email2 ([#828](https://github.com/jzaefferer/jquery-validation/issues/828), [dd162ae](https://github.com/jzaefferer/jquery-validation/commit/dd162ae360639f73edd2dcf7a256710b2f5a4e64))
* Método de patrón: Quitar los delimitadores, puesto que las implementaciones de HTML5 no los incluyen. ([37992c1](https://github.com/jzaefferer/jquery-validation/commit/37992c1c9e2e0be8b315ccccc2acb74863439d3e))
* Restricción de validador de tarjeta de crédito para incluir comprobación de longitud. Cierra gh-772 ([f5f47c5](https://github.com/jzaefferer/jquery-validation/commit/f5f47c5c661da5b0c0c6d59d169e82230928a804))
* Actualizar messages_ko.js; cierra gh-715 ([5da3085](https://github.com/jzaefferer/jquery-validation/commit/5da3085ff02e0e6ecc955a8bfc3bb9a8d220581b))
* Actualizar messages_pt_BR.js. Cierra gh-782 ([4bf813b](https://github.com/jzaefferer/jquery-validation/commit/4bf813b751ce34fac3c04eaa2e80f75da3461124))
* Actualizar phonesUK y mobileUK para aceptar nuevos prefijos. Cierra gh-750 ([d447b41](https://github.com/jzaefferer/jquery-validation/commit/d447b41b830dee984be21d8281ec7b87a852001d))
* Verificar códigos postales de nueve dígitos. Cierra gh-726 ([165005d](https://github.com/jzaefferer/jquery-validation/commit/165005d4b5780e22d13d13189d107940c622a76f))
* phoneUS: Agregar exclusiones N11. Cierra gh-861 ([519bbc6](https://github.com/jzaefferer/jquery-validation/commit/519bbc656bcb26e8aae5166d7b2e000014e0d12a))
* resetForm debe borrar cualquier valor aria-invalid ([4f8a631](https://github.com/jzaefferer/jquery-validation/commit/4f8a631cbe84f496ec66260ada52db2aa0bb3733))
* valid(): Compruebe todos los elementos. Corrige #791: valid() valida solo el primer elemento (no válido) ([#791](https://github.com/jzaefferer/jquery-validation/issues/791), [6f26803](https://github.com/jzaefferer/jquery-validation/commit/6f268031afaf4e155424ee74dd11f6c47fbb8553))

<a name="1111--2013-03-22"></a>1.11.1 / 2013-03-22
==================

  * Revertir para convertir también los parámetros del método range a números. Cierra gh-702
  * Reemplazar la mayoría del uso de PHP con controladores mockjax. Realizar también alguna limpieza de demostración; actualizar al último complemento de entrada enmascarada. Mantener la demostración de CAPTCHA en PHP. Corrige #662
  * Eliminar el resaltado del código insertado de la demostración de milk. Ver si el código fuente funciona bien.
  * Corregir demostración de totales dinámicos con el recorte de espacios en blanco del contenido de plantilla antes de pasar al constructor de jQuery
  * Corregir validación de min/max. Cierra gh-666. Corrige #648
  * Se han corregido los "mensajes" generados como una regla y que causan una excepción después de actualizarse a través de rules("add"). Cierra gh-670, corrige #624
  * Agregar localización de coreano (ko). Cierra gh-671
  * Método de código postal de Reino Unido mejorado para ignorar más códigos postales no válidos. Cierra #682
  * Actualizar messages_sv.js. Cierra #683
  * Cambiar vínculo Grunt al sitio web del proyecto. Cierra #684
  * Bajar el método remote en la lista para ejecutarlo el último, después de que todos los demás métodos se apliquen a un campo. Corrige #679
  * Actualizar descripción de plugin.json; debe incluir la palabra "validate"
  * Corregir errores tipográficos
  * Corregir el cargador de jQuery para usar su propia ruta de acceso. Corrige demostraciones anidadas.
  * Actualizar grunt-contrib-qunit para usar PhantomJS 1.8, cuando se instala a través del módulo de nodo "phantomjs"
  * Hacer que valid() devuelva un booleano en lugar de 0 o 1. Corrige #109; valid() no devuelve un valor booleano

<a name="1110--2013-02-04"></a>1.11.0 / 2013-02-04
==================

  * Quitar compensación como números de las reglas `min`, `max` y `range`. Corrige #455. Cierra gh-528.
  * Actualizar etiquetas preexistentes; corrige #430; cierra gh-436
  * Corregir $.validator.format para evitar la interpolación de grupo, donde al menos IE8/9 reemplaza -bash con la coincidencia. Corrige #614
  * Corregir mimetype regex
  * Agregar manifiesto del complemento y actualizar los encabezados a simplemente licencia MIT; quitar licencias dobles no necesarias (por ejemplo, jQuery).
  * Mensajes hebreos: Puntos quitados al final de las frases; corrige gh-568
  * Traducción al francés para la validación de require_from_group. Corrige gh-573.
  * Permitir que los grupos sean una matriz o una cadena; corrige #479
  * Espacios quitados con múltiples tipos MIME
  * Corregir algunas validaciones de fecha y errores de sintaxis JS.
  * Quitar la compatibilidad de complemento de metadatos, reemplazar con las propiedades data-rule- y data-msg- (agregadas en 907467e8).
  * Sftp agregado como patrón de URL válido
  * Agregar localización de malayo (my)
  * Actualizar localization/messages_hu.js
  * Quitar focusin/focusout polyfill. Corrige #542: la inclusión de jquery.validate interfiere con los eventos focusin y focusout de IE9
  * Localización: Error de escritura corregido en la traducción de finés
  * Corregir demostración de RTM para mostrar icono no válido al pasar de nuevo de válido a no válido
  * Devolución prematura corregida en la función remote que impide realizar la llamada ajax en caso que se haya escrito una entrada demasiado rápido. Garantiza que la validación remota siempre valida el valor más reciente.
  * Deshacer la corrección de #244. Corrige #521: se activa la validación de correo electrónico inmediatamente cuando el texto están en el campo.

<a name="1100--2012-09-07"></a>1.10.0 / 2012-09-07
===================

  * Se han corregido las cadenas de francés para nowhitespace, phoneUS, phoneUK y mobileUK en función de los comentarios de la comunidad.
  * Se ha cambiado el nombre de los archivos para idioma_REGIÓN según la norma ISO_3166-1 (http://en.wikipedia.org/wiki/ISO_3166-1); para Taiwán, el idioma es chino (zh) y la región es Taiwán (TW)
  * Optimizar patrones de RegEx, especialmente para números de teléfono de Reino Unido.
  * Se ha agregado el nombre de idioma en cada archivo y se ha cambiado el nombre del código de idioma según la norma ISO 639 para estonio, georgiano, ucraniano y chino (http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
  * Localización de croata (HR) añadida
  * Se editaron las traducciones existentes de francés y se agregaron las traducciones de francés para los métodos adicionales.
  * Combinación de cambios para especificar mensajes de error personalizados en atributos de datos
  * Regex de número de teléfono móvil de Reino Unido actualizado para números nuevos. Corrige #154
  * Agregar elemento a llamada correcta con prueba. Corrige #60
  * Regex corregido para método adicional de tiempo. Corrige #131
  * resetForm ahora borra los valores anteriores previousValue de elementos de formulario. Corrige #312
  * Prueba de casilla agregada a require_from_group y require_from_group modificado para usar elementValue. Corrige #359
  * Errores de respuesta dataFilter corregidos en jQuery 1.5.2+. Corrige #405
  * Demostración agregada de jQuery Mobile. Corrige #249
  * Desoptimizar findByName para exactitud. Corrige #82: Interrupciones de $.validator.prototype.findByName en IE7
  * Compatibilidad y pruebas de código postal de Estados Unido agregadas. Corrige #90
  * lastElement cambiado a lastActive en keyup; omitir validación de elemento de pestaña o vacío. Corrige #244
  * Número quitado eliminado de stripHtml. Corrige #2
  * Recuento no válido corregido en la validación remota de no válido a válido. Corrige #286
  * Agregar vínculo a file_input para el índice de la demostración
  * Método anterior aceptado movido al método adicional de extensión; método aceptado nuevo agregado para administrar el filtrado de tipo MIME del explorador estándar. Corrige #287 y reemplaza #369
  * Deshabilita el evento de desenfoque si onfocusout está establecido en false. Prueba agregada.
  * Problema de valor corregido para botones de radio y casillas. Corrige #363
  * Prueba agregada para rangeWords y regex y límites corregidos en el método. Corrige #308
  * Demostración de TinyMCE corregida y vínculo agregado en la página de la demostración. Corrige #382
  * Mensaje de localización modificado para min/max. Corrige #273
  * Selector seudo agregado para tipos de entrada de texto para corregir problemas con el atributo de tipo vacío predeterminado. Pruebas y algún marcado de prueba agregados. Corrige #217
  * Error de delegado corregido para demostración de totales dinámicos. Corrige #51
  * Corregir mensaje incorrecto para el validador alfanumérico
  * Valor false incorrecto quitado en la comprobación del atributo requerido
  * Corrección de atributo requerido para exploradores que no son html5. Corrige #301
  * Métodos agregados "require_from_group" y "skip_or_fill_minimum"
  * Utilizar código ISO correcto para sueco
  * Archivos HTML de demostración actualizados para usar el tipo de documento HTML5
  * Problema regex corregido para decimales sin ceros iniciales. Nueva prueba de métodos agregada. Corrige #41
  * Introducir un método elementValue que normaliza solo los valores de cadena (no tocar valores de matriz de selección múltiple). Corrige #116
  * Compatibilidad para botones de envío agregados de forma dinámica y caso de prueba actualizado. Usa validateDelegate. Código de PR #9
  * Corregir comillas dobles incorrectas en accesorios de prueba
  * Corregir método maxWords para incluir el límite superior, no para excluirlo. Corrige #284
  * Error gramatical corregido en el mensaje del validador de intervalos de alemán. Corrige #315
  * Procesamiento corregido de varios nombres de clave para la opción errorClass. Prueba de Max Lynch. Corrige #280
  * Corregir uso de jQuery.format, debe ser $.validator.format. Corrige #329
  * Métodos para "todos" los números de teléfono de Reino Unido + códigos postales de Reino Unido
  * Método de patrón: Convertir el parámetro de cadena en RegExp. Corrige el problema #223
  * Error gramatical en el archivo de localización de alemán
  * Localización de estonio agregada para los mensajes
  * Mejorar la administración de información sobre herramientas en la demostración de themerollered
  * Agregar type="text" a los campos de entrada sin el atributo type para please qSA
  * Actualizar demostración de themerollered para usar la información sobre herramientas para mostrar errores como superposición.
  * Actualizar demostración de themerollered para utilizar la última UI de jQuery (junto con la última versión de jQuery). Mover código para acelerar la carga de la página.
  * Mensaje de error min roto en japonés corregido.
  * Actualizar el complemento de formulario a la última versión. Mejorar la demostración de ajaxSubmit.
  * Quitar métodos dateDE y numberDE de classRuleSettings; sobrante de moverlos a métodos localizados
  * Pasar evento de envío a la devolución de llamada de submitHandler
  * #219 corregido: Corregir valid() en elementos con dependency-callback o dependency-expression.
  * Mejorar compilación para quitar el directorio dist para garantizar que solo la versión actual se comprima

<a name="190"></a>1.9.0
---
* Localización de euskera (EU) agregada
* Localización de esloveno (SL) agregada
* Problema #127 corregido: las traducciones de finés tienen : en lugar de ;
* Localización de ruso corregida; problema de sintaxis leve
* Compatibilidad agregada para tipos de entrada HTML5; corrige #97
* Compatibilidad de HTML5 mejorada con la definición del atributo novalidate en el formulario y con la lectura del atributo type.
* showLabel() corregido tras eliminar todas las clases del elemento de error. Quitar solo settings.validClass. Corrige #151.
* "pattern" agregado a métodos adicionales para validar según las expresiones regulares arbitrarias.
* Método de correo electrónico mejorado para no permitir el punto al final (válido para RFC, pero no deseado aquí). Corrige #143
* Traducciones de sueco y noruego corregidas, con mensajes min/max cambiados. Corrige #181
* #184 corregido; resetForm: se debe anular lastElement
* #71 corregido: Mejorar método de tiempo existente y agregar el método time12h para un formato de hora 12h am/pm
* #177 corregido: Corregir validación de una única entrada de radio o casilla
* #189 corregido: los elementos ocultos se ignoran ahora de forma predeterminada
* #194 corregido: Error en el atributo requerido si jQuery>=1.6; usar .prop en lugar de .attr
* #47, #39 y #32 corregidos: Se permite que los números de tarjeta de crédito contengan espacios y guiones (los usuarios incluyen espacios con frecuencia).

<a name="181"></a>1.8.1
---
* Localización de tailandés (TH) agregada; corrige #85
* Localización de vietnamita (VI) agregada; gracias a Ngoc
* Problema #78 corregido. El estilo error/válido se aplica a todos los botones de radio del mismo grupo para la validación requerida.
* No usar form.elements, porque ha dejado de ser compatible en jQuery 1.6. Es una cuestión muy complicada de todas formas (IE6-8: form.elements === form).

<a name="180"></a>1.8.0
---
* Se ha mejorado la localización de NL (http://plugins.jquery.com/node/14120)
* Localización de georgiano (GE) agregada, gracias a Avtandil Kikabidze
* Localización de serbio (SR) agregada, gracias a Aleksandar Milovac
* ipv4 y ipv6 agregados a métodos adicionales, gracias a Natal Ngétal
* Localización de japonés (JA) agregada, gracias a Bryan Meyerovich
* Localización de catalán (CA) agregada, gracias a Xavier de Pedro
* Instrucciones var ausentes corregidas en los bucles for-in
* Se ha corregido la validación remota, donde un mensaje con formato generaba errores (https://github.com/jzaefferer/jquery-validation/issues/11)
* Corrige errores de compatibilidad con jQuery 1.5.1, manteniendo la compatibilidad con versiones anteriores

<a name="17"></a>1.7
---
* Localización de lituano (LT) agregada
* Localización de griego (EL) agregada (http://plugins.jquery.com/node/12319)
* Localización de letón (LV) agregada (http://plugins.jquery.com/node/12349)
* Localización de hebreo (HE) agregada (http://plugins.jquery.com/node/12039)
* Localización de español (ES) agregada (http://plugins.jquery.com/node/12696)
* Demostración de themerolled de interfaz de usuario de jQuery agregada
* cmxform.js quitado
* Se han corregido cuatro signos de punto y coma que faltaban (http://plugins.jquery.com/node/12639)
* Cambiar el nombre de phone-method en additional-methods.js a phoneUS
* Se han agregado métodos phoneUK y mobileUK a additional-methods.js (http://plugins.jquery.com/node/12359)
* Se han incluido opciones de extensión profunda para no tener que modificar varios formularios al usar rules-method en un único elemento (http://plugins.jquery.com/node/12411)
* Corrige errores de compatibilidad con jQuery 1.4.2, manteniendo la compatibilidad con versiones anteriores

<a name="16"></a>1.6
---
* Localización de árabe (AR), portugués (PTPT), persa (FA), finés (FI) y búlgaro (Brasil) agregada
* Localización de sueco (SE) actualizada (faltan algunos caracteres ISO HTML)
* $.validator.addMethod para administrar correctamente la cadena vacía frente a la no definida para el argumento de mensaje
* Dos variables globales accidentales corregidas
* min/max/rangeWords mejorados (en additional-methods.js) para quitar HTML antes del recuento; correcto cuando se cuentan las palabras en un editor de texto enriquecido
* Métodos localizados agregados para DE, NL y PT, quitando los métodos dateDE y numberDE (en su lugar, usar messages_de.js y methods_de.js con los métodos de fecha y número)
* Sincronización de envío de formulario remota corregida, gracias a Matas Petrikas
* Se ha mejorado la validación de selección interactiva, que ahora valida también al hacer clic (a través de opciones o selecciones, incoherente según el explorador); no funciona con Safari, que no activa un evento de clic en los elementos seleccionados; corrige http://plugins.jquery.com/node/11520
* Se ha actualizado al último complemento de formulario (2.36), lo que corrige http://plugins.jquery.com/node/11487
* Enlace a evento de desenfoque para que el destino equalTo se revalide cuando el destino cambie; corrige http://plugins.jquery.com/node/11450
* Validación de selección simplificada, delegando al método val() de jQuery para obtener el valor seleccionado; debe corregir http://plugins.jquery.com/node/11239
* Mensaje predeterminado corregido para los dígitos (http://plugins.jquery.com/node/9853)
* Se ha corregido un problema con mensajes remotos almacenados en caché (http://plugins.jquery.com/node/11029 y http://plugins.jquery.com/node/9351)
* Punto y coma que falta corregido en additional-methods.js (http://plugins.jquery.com/node/9233)
* Detección automática de parámetros de sustitución agregada en los mensajes, lo que acaba con la necesidad de proporcionar funciones de formato (http://plugins.jquery.com/node/11195)
* Problema corregido con :filled/:blank causado por Sizzle (http://plugins.jquery.com/node/11144)
* Se ha agregado un método entero a additional-methods.js (http://plugins.jquery.com/node/9612)
* Método errorsFor corregido, donde el atributo for-attribute contiene caracteres que deben ignorarse para ser válidos en un selector (http://plugins.jquery.com/node/9611)

<a name="155"></a>1.5.5
---
* Corrección de http://plugins.jquery.com/node/8659
* Coma final corregida en messages_cs.js

<a name="154"></a>1.5.4
---
* Se corrigió un error de método remoto (http://plugins.jquery.com/node/8658)

<a name="153"></a>1.5.3
---
* Error corregido relacionado con la opción de contenedor, donde se seleccionaron todos los elementos antecesores que coincidían con la opción de contenedor (http://plugins.jquery.com/node/7624)
* Demostración de varias partes actualizada para usar la última versión de jQuery UI Accordion
* Métodos dateNL y time agregados a additionalMethods.js
* Localización de chino tradicional (Taiwán, tw) y Kazajistán (KK) agregada
* Cambio de jQuery.format (anteriormente, String.format) a jQuery.validator.format, jQuery.format está en desuso y se quitará en 1.6 (vea http://code.google.com/p/jquery-utils/issues/detail?id=15 para más información)
* Mensajes messages_pl.js y messages_ptbr.js eliminados (aún mensajes definidos para max/min/rangeValue, que se quitaron en 1.4)
* Lógica booleana con errores corregida en el método valid-plugin-method para varios elementos; ahora todos los elementos necesitan ser válidos para un resultado boolean-true (http://plugins.jquery.com/node/8481)
* Mejora de $. validator.addMethod: Un tercer argumento mensaje indefinido no sobrescribirá un mensaje existente)http://plugins.jquery.com/node/8443)
* Mejora de la opción submitHandler: Cuando se utiliza, haga clic en los eventos de envío se capturan los botones y el botón de envío se inserta en el formulario antes de llamar a submitHandler y quita posteriormente; mantiene (intacta de botones Enviar http://plugins.jquery.com/node/7183#comment-3585)
* Opción validClass agregada, cuyo valor predeterminado es "valid", que agrega dicha clase a todos los elementos válidos, después de la validación (http://dev.jquery.com/ticket/2205)
* Método creditcardtypes agregado a additionalMethods.js, incluidas las pruebas (a través de http://dev.jquery.com/ticket/3635)
* Método remoto mejorado para permitir mensajes de lado servidor como una cadena, true para valid o false para invalid con el mensaje definido en el lado cliente (http://dev.jquery.com/ticket/3807)
* Método de aceptación mejorado para aceptar también una lista de valores separados por comas con estilo Drupal (http://plugins.jquery.com/node/8580)

<a name="152"></a>1.5.2
---
* Mensajes corregidos en additional-methods.js para maxWords, minWords y rangeWords para incluir la llamada a $.format
* Valor corregido pasado a los métodos para excluir el retorno de carro (\r), lo mismo que hace val() en jQuery
* Localización de eslovaco (sk) agregada
* Demostración agregada para la integración con pestañas de la UI de jQuery
* Ejemplo agregado de agrupación de instrucciones SELECT a la demostración de pestañas (vea la segunda pestaña en el campo de la fecha de nacimiento)

<a name="151"></a>1.5.1
---
* Demostración de marketo actualizada para usar la opción invalidHandler en lugar del evento invalid-form de enlace
* Ejemplo de integración de TinyMCE agregado
* Localización de ucraniano (ua) agregada
* Validación de longitud corregida para trabajar con valores recortados (regresión de 1.5, donde se quitó el recorte general antes de la validación)
* Varias correcciones pequeñas de compatibilidad con 1.2.6 y 1.3

<a name="15"></a>1.5
---
* Demostración básica mejorada, con validación del campo de confirmación de contraseña después del cambio de contraseña
* Validación básica corregida para pasar el valor de entrada sin recortar como el primer parámetro a los métodos de validación; se requieren cambios; interrumpe el método personalizado existente basado en el recorte
* Localización de noruego (no), italiano (it), húngaro (hu) y rumano (ro)
* #3195 corregido: Dos errores en la localización sueco
* #3503 corregido: Extendidos rules("add") para aceptar la propiedad de mensajes: usar para especificar agregar mensajes personalizados a un elemento a través de las reglas ("add", {mensajes: {necesarios: "¡Required! " } });
* #3356 corregido: Regresión desde #2908 al usar la opción de metadatos
* #3370 corregido: Opción ignoreTitle agregada, establecida para omitir la lectura de mensajes desde el atributo title; ayuda a evitar problemas con la barra de herramientas de Google; el valor predeterminado es false para la compatibilidad
* Fijo #3516: Evento de desencadenador invalid-form incluso cuando está implicada en validación remota
* Opción invalidHandler agregada como un acceso directo a bind("invalid-form", function() {})
* Problema de Safari corregido para cargar el indicador en ajaxSubmit-integration-demo (anexar primero al cuerpo y después ocultar)
* Prueba agregada para validación de tarjeta de crédito y mensaje predeterminado mejorado
* Validación remota mejorada, con la aceptación de opciones para crear un acceso directo a $.ajax como parámetro (opciones o cadena URL, incluida la propiedad URL y todo lo que $.ajax admite)

<a name="14"></a>1.4
---
* #2931 corregido: Validar elementos en el orden del documento e ignorar las entradas type=image
* Uso corregido de las variables $ y jQuery, que ahora son totalmente compatibles con todas las variaciones de uso de noConflict
* #2908 implementado: Habilitar mensajes personalizados a través de metadatos ala class="{required:true,messages:{required:'required field'}}"; demo/custom-messages-metadata-demo.html agregado
* Métodos en desuso minValue (min), maxValue (max), rangeValue (rangevalue), minLength (minlength), maxLength (maxlength) y rangeLength (rangelength) eliminados
* Ha corregido la regresión de #2215: Llamada de anulación de resaltado solo para los elementos actuales, no todo
* #2989 implementado: Habilitar el botón de imagen para cancelar la validación
* Problema corregido, donde IE valida incorrectamente en maxlength=0
* Localización de checo (cs) agregada
* Restablecer validator.submitted en validator.resetForm(), lo que permite un reinicio completo cuando sea necesario
* #3035 corregido: Se omiten todos los atributos false al leer reglas (0, sin definir, cadena vacía); se quita parte de la solución alternativa maxlength (para 0)
* Localización de neerlandés (nl) agregada (#3201)

<a name="13"></a>1.3
---
* Evento invalid-form corregido; ahora solo se inicia cuando el formulario no es válido
* Localización de español (es), ruso (ru), portugués de Brasil (ptbr), turco (tr) y polaco (pl) agregada
* Complemento removeAttrs agregado para facilitar la adición y eliminación de varios atributos
* Opción de grupos agregada para mostrar un único mensaje para varios elementos, a través de grupos: { arbitraryGroupName: "fieldName1 fieldName2[, fieldNameN" }
* Parámetro rules() mejorado para agregar y quitar reglas (estáticas): rules("add", "method1[, methodN]"/{method1:param[, method_n:param]}) y rules("remove"[, "method1[, method_n]")
* Opción de reglas mejorada; acepta lista de cadenas de métodos separados por espacios; por ejemplo, {birthdate: "required date"}
* Se ha corregido la casilla de verificación validación de grupo con reglas insertadas: Las reglas se especifican en el primer elemento, siempre y cuando el grupo se valida correctamente en haga clic en
* #2473 corregido: Ignorar todas las reglas con un parámetro explícito de boolean-false; por ejemplo, required:false es lo mismo que no especificar required (hasta ahora, se ha administrado como required:true)
* Fijo 2424 #, con una revisión modificada de #2473: Los métodos que devuelven un error de coincidencia de dependencia no detienen otras reglas de evaluarse; No obstante, success no se aplica para campos opcionales
* Validación de URL y correo electrónico corregida para no usar valores recortados
* Validación de tarjeta de crédito corregida para aceptar solo dígitos y guiones ("asdf" no es un número de tarjeta de crédito válido)
* Permitir botones y elementos de entrada para los botones de cancelación (a través de class="cancel")
* #2215 corregido: Muestra el mensaje corregido para llamar a la anulación de resaltado como parte de mostrar y ocultar los mensajes, ya no tiene efectos secundarios visuales durante la comprobación de un elemento y extrae validator.checkForm para validar un formulario sin la UI
* Se reescribieron los selectores personalizados (:blank, :filled, :unchecked) con funciones para la compatibilidad con AIR

<a name="121"></a>1.2.1
-----

* Complemento de delegado agrupado con complemento de validación; siempre es necesario
* Validación remota mejorada para incluir partes del complemento ajaxQueue para una correcta sincronización (no se necesita ningún complemento adicional)
* stopRequest corregido para evitar pendingRequest < 0
* Propiedad jQuery.validator.autoCreateRanges agregada, con valor predeterminado false, que permite habilitar la conversión de min/max a range y de minlength/maxlength a rangelength; básicamente corrige el problema generado con la creación automática de intervalos en 1.2
* Métodos opcionales corregidos para no resaltar nada si el campo está vacío, es decir, no desencadenar success
* Permitir false/null para opciones de resaltado o anulación de resaltado, en lugar de forzar una devolución de llamada do-nothing-callback incluso cuando no es necesario resaltar nada
* Llamada validate() corregida con ningún elemento seleccionado; devuelve undefined en lugar de emitir un error
* Demostración mejorada, mediante la sustitución de metadatos con clases/atributos para especificar las reglas
* Error corregido cuando no se usa ningún mensaje personalizado para la validación remota
* Validación de correo electrónico y URL modificada para requerir la etiqueta de dominio y la etiqueta superior
* Validación de URL y correo electrónico corregida para requerir TLD (realmente, para requerir la etiqueta de dominio); la versión 1.2 (TLD es opcional) cambia a adiciones como as url2 y email2
* Demostración de totales dinámicos corregida en IE6/7 y plantillas mejoradas, con el uso de textarea para almacenar la interpolación de cadena y plantilla multilínea
* Ejemplo de formulario de inicio de sesión agregado con el vínculo "Contraseña de correo electrónico" que hace que el campo de contraseña sea opcional
* Demostración de totales dinámicos mejorada con un ejemplo de un único mensaje para dos campos

<a name="12"></a>1.2
---

* Ejemplo de validación con CAPTCHA de AJAX agregado (basado en http://psyrens.com/captcha/)
* Demostración remember-the-milk-demo agregada (gracias al equipo de RTM por el permiso)
* Demostración marketo-demo agregada (gracias a Glen Lipka)
* Compatibilidad agregada para validación AJAX; véase el método "remote"; serverside devuelve JSON, true para elementos valid, false o String para invalid y String se usa como mensaje
* Opciones de resaltado y anulación de resaltado agregadas; de forma predeterminada, alterna errorClass en el elemento y permite el resaltado personalizado
* Método de complemento valid() agregado para una comprobación sencilla mediante programación de los formularios y campos sin necesidad de usar la API del validador
* Método de complemento rules() agregado para leer y escribir reglas para un elemento (actualmente solo lectura)
* Regex reemplazado para método de correo electrónico, gracias a la contribución de Scott Gonzalez; vea http://projects.scottsplayground.com/email_address_validation/
* Arquitectura de eventos reestructurada para depender solo de la delegación, con una mejora del rendimiento y facilidad de uso para el desarrollador (requiere jquery.delegate.js)
* Documentación en línea movida a http://docs.jquery.com/Plugins/Validation, incluidos los ejemplos interactivos de todos los métodos
* Método validator.refresh() eliminado; la validación ahora es totalmente dinámica
* Nombre de minValue cambiado a min, maxValue a max y rangeValue a range, dejando en desuso los nombres anteriores (que se quitarán en la versión 1.3)
* Nombre de minLength cambiado a minlength, maxLength a maxlength y rangeLength a rangelength, dejando en desuso los nombres anteriores (que se quitarán en la versión 1.3)
* Característica agregada para combinar min + max en range y minlength + maxlength en rangelength
* Compatibilidad agregada para los parámetros de regla dinámica, que permite especificar una función como un parámetro; por ejemplo, para minlength, la llamada se realiza al validar el elemento
* Permitir especificar null o una cadena vacía como un mensaje para mostrar nothing (ver la demostración de marketo)
* Revisión de reglas: Ahora admite la combinación de opción de reglas, metadatos, clases (nuevo) y atributos (nuevos); ver rules() para obtener más información

<a name="112"></a>1.1.2
---

* Regex reemplazado para método de URL, gracias a la contribución de Scott Gonzalez; vea http://projects.scottsplayground.com/iri/
* Método de correo electrónico mejorado para administrar mejor los caracteres Unicode
* Contenedor de errores corregido para ocultarse cuando todos los elementos son válidos, no solo al enviar el formulario
* String.format corregido para jQuery.format (transferencia al espacio de nombres de jQuery)
* Método de aceptación corregido para aceptar extensiones en mayúscula y minúscula
* Método de complemento validate() corregido para crear solo una instancia de validador para un formulario determinado y siempre devolver dicha instancia (evita enlazar eventos varias veces)
* Registro de consola en modo de depuración cambiado de nivel de "error" a "advertencia"

<a name="111"></a>1.1.1
-----

* XHTML no válido corregido, que impide la creación de etiquetas de error en IE desde jQuery 1.1.4
* String.format fijo y mejorado: Búsqueda global y reemplazar, una administración mejorada de los argumentos de matriz
* Control del botón de cancelación corregido para usar el objeto de validador para almacenar el estado en lugar del elemento de formulario
* Selectores de nombres corregidos para administrar nombres "complejos"; por ejemplo, los que contengan corchetes ("list[]")
* Botón agregado y elementos deshabilitados para excluirlos de la validación
* Controladores de eventos de elementos movidos para actualizarlos, a fin de agregar controladores a los nuevos elementos
* Validación de correo electrónico corregida para permitir dominios largos de nivel superior (por ejemplo, ".travel")
* Método showErrors() movido de valid() a form()
* Método validator.size() agregado: devuelve el número de errores actuales
* Llamar a submitHandler con validador para facilitar el acceso de sus métodos; por ejemplo, para buscar las etiquetas de error mediante errorsFor(Element)
* Compatible con jQuery 1.1.x y 1.2.x

<a name="11"></a>1.1
---

* Validación agregada en desenfoque, keyup y clic (para casillas y botones de radio). Reemplaza la opción de eventos.
* resetForm corregido
* Demostración custom-methods-demo corregida

<a name="10"></a>1.0
---

* Métodos number y numberDE mejorados para buscar los números decimales correctos con delimitadores
* Solo se comprueban los elementos que tienen reglas (de lo contrario, se aplica success-option a todos los elementos)
* Método de número de tarjeta de crédito agregado (gracias a Brian Klug)
* Opción ignore-option agregada; por ejemplo, ignore: "[@type=hidden]", usando dicha expresión para excluir elementos de la validación. Valor predeterminado: none, aunque los botones de envío y restablecimiento siempre se ignoran
* Funciones como mensajes muy mejoradas proporcionando un asistente flexible para String.format
* Aceptar funciones como mensajes, proporcionando mensajes personalizados en tiempo de ejecución
* Exclusión de elementos corregida sin reglas de successList
* Demostración custom-method-demo corregida, donde se reemplazó la alerta con un mensaje que muestra el número de errores
* form-submit-prevention corregido al usar submitHandler
* Dependencia de identificadores de elementos totalmente eliminada, aunque aún se usan (si existen) para vincular etiquetas de errores con entradas. Se consigue con el uso de una matriz con {name, message, element}, en lugar de un objeto con pares id:message para la lista de errores internos.
* Compatibilidad agregada para especificar reglas sencillas como cadenas sencillas; por ejemplo, "required" es equivalente a {required: true}
* Característica agregada: Agregue errorClass al elemento primario de campo no válidos s, lo que facilita el contenedor o el campo de etiqueta o la etiqueta para el campo de estilo.
* Característica agregada: focusCleanup; si está habilitada, quita errorClass de los elementos no válidos y oculta todos los mensajes de error cada vez que se centra el elemento.
* Opción correcta agregada para mostrar un campo cuya validación es correcta
* Problema select-issue de Opera corregido (evitando un conflicto de atributos)
* Problemas corregidos con un enfoque en elementos ocultos en IE
* Característica agregada para omitir la validación de los botones de envío con la clase "cancel"
* Problemas potenciales corregidos con la barra de herramientas de Google, al establecer la preferencia de mensajes con opción de complemento con respecto al atributo de título
* Solo se llama a submitHandler cuando se administró un evento de envío real; validator.form() devuelve false solo para los formularios no válidos
* Los elementos no válidos ahora se centran solo en el envío o a través de validator.focusInvalid(), para evitar todos los problemas con focus-on-blur
* Se ha resuelto el problema de diseño de contenedor con error de IE6
* Personalizar elemento de error con la opción errorElement
* Método validator.refresh() agregado para encontrar nuevas entradas en el formulario
* Método de validación aceptado agregado; comprueba las extensiones de archivo
* Característica de dependencia mejorada con la adición de dos expresiones personalizadas: ":blank" para seleccionar elementos con un valor vacío y :filled para seleccionar elementos con un valor, en ambos casos, excluyendo los espacios en blanco
* Método resetForm() agregado al validador: Restablece cada elemento de formulario (mediante el complemento de formulario, si está disponible), elimina las clases de elementos no válidos y oculta todos los mensajes de error
* Documentos corregidos para validator.showErrors()
* Creación de etiquetas de error mejorada para usar siempre html() en lugar de text(), para permitir que el HTML arbitrario se transfiera como mensajes
* Creación de etiqueta de error corregida para usar una clase de error específica
* Característica de dependencia agregada: El método requires acepta String (expresiones de jQuery) y funciones como argumento
* Personalización muy mejorada de presentación del mensaje de error: Usar mensajes normales y mostrar u ocultar un contenedor adicional; Reemplazar completamente la presentación de mensajes con un mecanismo propio (mientras se pueden delegar en el controlador predeterminado; Personalizar la colocación de etiquetas generadas (en lugar de por debajo-elemento predeterminado)
* Dos errores importantes corregidos en IE (contenedores con error) y Opera (metadatos)
* Métodos de validación modificados para aceptar campos vacíos como válidos (excepción: los métodos required y equalTo)
* Nombre de "min" cambiado a "minLength", "max" a "maxLength" y "length" a "rangeLength"
* "minValue", "maxValue" y "rangeValue" agregados
* API simplificada para la compatibilidad con diversos eventos. El valor predeterminado, submit, se puede deshabilitar. Si se especifica cualquier evento, este se aplica a cada elemento (en lugar de todo el formulario). La combinación de keyup-validation con submit-validation ahora es muy fácil de configurar
* Compatibilidad agregada para one-message-per-rule al definir mensajes a través de la configuración del complemento
* Compatibilidad agregada para encapsular metadatos en algunos elementos principales. Resulta útil si los metadatos también se usan para otros complementos.
* Refactorizado pruebas y demostraciones: Menos archivos demostraciones mejoradas
* Documentación mejorada: Más ejemplos de métodos y más textos explicar algunos conceptos básicos de referencia
