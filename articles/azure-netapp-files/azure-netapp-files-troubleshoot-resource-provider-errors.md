---
title: Solucionar errores de proveedor de recursos de archivos de NetApp de Azure | Microsoft Docs
description: Describe las causas, soluciones y soluciones alternativas para los errores de proveedor de recursos de Azure NetApp archivos comunes.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: b-juche
ms.openlocfilehash: d4e06429aa1efec7c3301c7d0f0e7e17800fd520
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63769442"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Solución de errores del proveedor de recursos de Azure NetApp Files
Este artículo describe comunes proveedor de recursos de archivos de Azure NetApp errores, sus causas, soluciones y soluciones alternativas. 

<a name="error_01"></a>***Azure Key Vault no configurado.***   
Azure Key Vault almacena las credenciales necesarias para tener acceso a la API subyacente. Este error indica que Azure Key Vault no ha recibido las credenciales para tener acceso a la API subyacente completadas.

* Causa  
Azure Key Vault no ha recibido las credenciales correctas o las credenciales están incompletas.  

* Solución   
El servicio de Azure Files de NetApp utiliza Azure Key Vault. Azure Key Vault se autentica mediante un token de Azure Active Directory. Por lo tanto, el propietario de la aplicación debe registrar la aplicación en Azure Active Directory.

* Solución alternativa   
Ninguno.  Azure Key Vault debe configurarse correctamente para el uso de Azure Files de NetApp.  

<a name="error_02"></a>***No se puede cambiar el testigo de creación.***   
Este error se produce al intentar cambiar el testigo de creación de una vez creado el volumen.
Testigo de creación debe establecerse cuando el volumen se crea y no se puede cambiar más adelante.

* Causa   
Está intentando cambiar el testigo de creación de una vez creado el volumen, que no es una operación admitida.

* Solución   
Una vez creado el volumen, considere la posibilidad de quitar el parámetro de la solicitud para descartar el mensaje de error.

* Solución alternativa   
Si necesita cambiar el testigo de creación, puede crear un nuevo volumen con un nuevo token de creación y, a continuación, migrar los datos al nuevo volumen.


<a name="error_03"></a>***Creación del Token debe ser al menos 16 caracteres.***   
Este error se produce cuando el testigo de creación no cumple el requisito de longitud. La longitud del token de creación debe ser al menos 16 caracteres.

* Causa   
El testigo de creación no cumple el requisito de longitud.  Cuando se crea un volumen mediante la API, se requiere un token de creación. Si está usando el portal, el token se puede generar automáticamente.

* Solución   
Aumente la longitud del token de creación. Por ejemplo, puede agregar otra palabra al principio o al final del token de creación.

* Solución alternativa   
El mínimo requerido no se pueden omitir la longitud del token de creación.  Puede usar un prefijo o sufijo para aumentar la duración del token de creación.


<a name="error_04"></a>***Error al eliminar un volumen que no se encontró en Azure Files de NetApp.***   
Este error se produjo porque el registro de recursos interno no está sincronizado.

* Causa   
El volumen puede permanecer mostrado en el portal durante algún tiempo después de que se ha eliminado. Si elimina el volumen mediante la API, es posible que el volumen no se especificó correctamente. El error también puede deberse a la caché del explorador obsoleto.

* Solución   
Memoria caché del explorador claro si utilizas el portal. También hay una memoria caché interna que se actualiza cada 10 minutos.  Puede intentar borrar caché de nuevo.  Si el problema persiste después de 10 minutos, puede crear una incidencia de soporte técnico.

* Solución alternativa   
Usar un volumen diferente durante el proceso y omitir uno existente.


<a name="error_05"></a>***Error al insertar un nuevo volumen que se encuentra en Azure Files de NetApp.***   
Este error se produce porque el registro de recursos interno no está sincronizado.

* Causa   
El volumen es posible que se siguen mostrando en el portal durante algún tiempo después de que se ha eliminado. Si elimina el volumen mediante la API, es posible que el volumen no se especificó correctamente.

* Solución   
Si está usando el portal, ya se ha creado el volumen.  Debería aparecer el volumen automáticamente. Si el problema persiste, puede crear una incidencia de soporte técnico.

* Solución alternativa   
Puede crear un volumen con un nombre diferente y un token de creación diferentes.


<a name="error_06"></a>***El nombre de ruta de acceso de archivo puede contener letras, números y guiones (""-"") solo.***   
Este error se produce cuando la ruta de acceso contiene caracteres no admitidos, por ejemplo, un punto ("."), coma (","), un carácter de subrayado ("\_"), o signo de dólar ("$").

* Causa   
La ruta de acceso contiene caracteres no admitidos, por ejemplo, un punto ("."), coma (","), un carácter de subrayado ("\_"), o signo de dólar ("$").

* Solución   
Quitar los caracteres que no son letras en orden alfabético, números o guiones ("-") de la ruta de acceso de archivo especificado.

* Solución alternativa   
Puede reemplazar un carácter de subrayado con un guión o usar mayúsculas y minúsculas en lugar de espacios para indicar el comienzo de nuevas palabras (por ejemplo, mediante "NewVolume" en lugar de "nuevo volumen").


<a name="error_07"></a>***Id. de volumen no se puede cambiar.***   
Este error se produce al intentar cambiar el identificador de volumen.  Cambiar el Id. de volumen no es una operación admitida.

* Causa   
El identificador del sistema de archivos se establece cuando se crea el volumen. El Id. de volumen no se puede cambiar posteriormente.

* Solución   
Ninguno.

* Solución alternativa   
Ninguno.  El Id. de volumen se genera cuando el volumen se crea y no se puede cambiar posteriormente.


<a name="error_08"></a>***Un valor no válido '{0}' se ha recibido para {1}.***   
Este mensaje indica un error en los campos para RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Causa   
Error en la solicitud de validación de entrada para al menos uno de los siguientes campos: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Solución   
Asegúrese de establecer todos los parámetros obligatorios y no conflictivos en la línea de comandos. Por ejemplo, no se puede establecer los UnixReadOnly UnixReadWrite parámetros y al mismo tiempo.

* Solución alternativa   
Consulte la sección de solución.  


<a name="error_09"></a>***Falta el valor de '{0}'.***   
Este error indica que falta un atributo obligatorio de la solicitud para al menos uno de los parámetros siguientes: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Causa   
Error en la solicitud de validación de entrada para al menos uno de los siguientes campos: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Solución   
Asegúrese de establecer todos los parámetros obligatorios y no conflictivos en la línea de comandos. Por ejemplo, no se puede establecer los UnixReadOnly UnixReadWrite parámetros y al mismo tiempo

* Solución alternativa   
Consulte la sección de solución.  


<a name="error_10"></a> ***{0} ya está en uso.***   
Este error indica que se ha utilizado el nombre del recurso.

* Causa   
Está intentando crear un volumen con un nombre que es el mismo que un volumen existente.

* Solución   
Use un nombre único al crear un volumen.

* Solución alternativa   
Si es necesario, puede cambiar el nombre del volumen existente para que el nuevo volumen puede usar el nombre deseado.


<a name="error_11"></a> ***{0} es demasiado corta.***   
Este error indica que el nombre del volumen no cumple el requisito de longitud mínima.

* Causa   
El nombre del volumen es demasiado corto.

* Solución   
Aumente la longitud del nombre del volumen.  

* Solución alternativa   
Puede agregar un prefijo común o un sufijo al nombre de volumen.


<a name="error_12"></a>***API de archivos de NetApp Azure inaccesible.***   
La API de Azure se basa en la API de archivos de NetApp de Azure para administrar volúmenes.  Este error indica un problema con la conexión de API.

* Causa   
La API subyacente no responde, que da como resultado un error interno. Este error es probable que sea temporal.

* Solución   
El problema es probable que sea temporal.  Debería realizarse correctamente la solicitud más tarde.

* Solución alternativa   
Ninguno. La API subyacente es esencial para la administración de volúmenes.  


<a name="error_13"></a>***Se encontró ninguna credencial para la suscripción '{0}'.***   
Este error indica que las credenciales proporcionadas no son válidas o no se han establecido correctamente en la suscripción.

* Causa   
Las credenciales que no son válidas o establecida incorrectamente impiden el acceso al servicio de administración de volúmenes.

* Solución   
Asegúrese de que se establecen las credenciales y se escribió correctamente en la línea de comandos.

* Solución alternativa   
Ninguno.  Establecer las credenciales correctamente es esencial para usar Azure Files de NetApp.  


<a name="error_14"></a>***Se encontró ningún identificador de resultado de la operación para '{0}'.***   
Este error indica un error interno que impide la operación de finalización.

* Causa   
Error interno e impidió que completar la operación.

* Solución   
Este error es probable que sea temporal.  Espere unos minutos e inténtelo de nuevo. Si el problema persiste, cree una incidencia para que soporte técnico a investigar el problema.

* Solución alternativa   
Espere unos minutos y compruebe si el problema persiste.


<a name="error_15"></a>***Operación '{0}' no se admite.***   
Este error indica que el comando no está disponible para la suscripción activa o el recurso.

* Causa   
La operación no está disponible para la suscripción o el recurso.

* Solución   
Asegúrese de que ha escrito correctamente el comando y disponibles para los recursos y suscripción que está usando.

* Solución alternativa   
Consulte la sección de solución.  


<a name="error_16"></a>***No se admite la operación de revisión para este tipo de recurso.***   
Este error se produce al intentar cambiar el destino de montaje o instantánea.

* Causa   
El destino de montaje se define cuando se crea, y no se puede cambiar posteriormente.

* Solución   
Ninguno.  No se puede cambiar el destino de montaje después de crea el volumen.

* Solución alternativa   
Ninguno.


<a name="error_17"></a>***Recibió un valor de propiedad de solo lectura '{0}'.***   
Este error se produce cuando se define un valor para una propiedad que no se puede cambiar. Por ejemplo, no se puede cambiar el identificador del volumen.

* Causa   
Ha intentado modificar un parámetro (por ejemplo, el Id. de volumen) que no se puede cambiar.

* Solución   
Ninguno. No se puede modificar el parámetro para el Id. de volumen.

* Solución alternativa   
El Id. de volumen no debería requerir la modificación.  Por lo tanto, una solución alternativa no es necesaria.

<a name="error_18"></a>***Solicitado {0} no se encontró.***   
Este error se produce cuando se intenta hacer referencia a un recurso que no existe, por ejemplo, un volumen o una instantánea. Puede que haya eliminado el recurso o tiene un nombre de recurso con errores ortográficos.

* Causa   
Está intentando hacer referencia a un recurso que no existe (por ejemplo, un volumen o instantánea) que ya se ha eliminado o tiene un nombre de recurso mal escritas.

* Solución   
Compruebe la solicitud para errores de ortografía para asegurarse de que correctamente hace referencia.

* Solución alternativa   
Consulte la sección de solución.

<a name="error_19"></a>***No se puede obtener las credenciales para la suscripción '{0}'.***   
Este error indica que las credenciales proporcionadas no son válidas o configurado incorrectamente en la suscripción.

* Causa   
Las credenciales que no son válidas o incorrectamente conjunto en la suscripción impedir el acceso al servicio de administración de volúmenes.

* Solución   
Asegúrese de que se establecen las credenciales y se escribió correctamente en la línea de comandos.

* Solución alternativa   
Ninguno.  Correctamente, establezca las credenciales son esenciales para el uso de Azure Files de NetApp.

<a name="error_20"></a>***Error de los archivos de NetApp Azure desconocido.***   
La API de Azure se basa en la API de archivos de NetApp de Azure para administrar volúmenes. El error indica un problema en la comunicación con la API.

* Causa   
La API subyacente está enviando un error desconocido.  Este error es probable que sea temporal.

* Solución   
El problema es probable que sea temporal, y debería realizarse correctamente la solicitud más tarde. Si el problema persiste, cree una incidencia de soporte técnico para que el problema investigar.

* Solución alternativa   
Ninguno.  La API subyacente es esencial para la administración de volúmenes.

<a name="error_21"></a>***Valor que recibió para una propiedad desconocida '{0}'.***   
Este error se produce cuando se proporcionan las propiedades que no existentes para un recurso, como el volumen, la instantánea o el destino de montaje.

* Causa   
La solicitud tiene un conjunto de propiedades que se pueden usar con cada recurso.  No se puede incluir cualquier propiedad que no existe en la solicitud.

* Solución   
Asegúrese de que todos los nombres de propiedad estén escritos correctamente y las propiedades están disponibles para la suscripción y recursos.

* Solución alternativa   
Reduzca el número de propiedades definidas en la solicitud para eliminar la propiedad que está provocando el error.


<a name="error_22"></a>***No se admite la operación de actualización para este tipo de recurso.***   
Solo los volúmenes se pueden actualizar. Este error se produce cuando se intenta realizar una operación de actualización no admitida, por ejemplo, una instantánea de la actualización.

* Causa   
El recurso que está intentando actualizar no se admite la operación de actualización.  Volúmenes solo pueden tener sus propiedades modificadas.

* Solución   
Ninguno.  El recurso que está intentando actualizar no se admite la operación de actualización. Por lo tanto, no se puede cambiar.

* Solución alternativa   
Para un volumen, cree un nuevo recurso con la actualización en contexto y migrar los datos.


<a name="error_23"></a>***Número de elementos: {0} objeto: {1} está fuera del intervalo mínimo-máximo.***   
Este error se produce cuando las reglas de directivas de exportación no cumplen el requisito de intervalo mínimo o máximo.  Si define la directiva de exportación, debe tener una exportación regla de directiva como mínimo y cinco reglas de directiva de exportación como máximo.

* Causa   
Ha definido la directiva de exportación no cumple el intervalo.  

* Solución   
Asegúrese de que no se utiliza ya el índice y que está en el intervalo de 1 a 5.

* Solución alternativa   
No es obligatorio utilizar Directiva de exportación en los volúmenes. Por lo tanto, puede omitir la directiva de exportación por completo si no necesita tener reglas de directiva de exportación.


<a name="error_24"></a>***Error de valor de objeto duplicado {0}.***   
Este error se produce cuando la directiva de exportación no está definida con un índice único.  Al definir las directivas de exportación, todas las reglas de directiva de exportación deben tener un índice único entre 1 y 5.

* Causa   
La directiva de exportación definido no cumple el requisito de reglas de directivas de exportación. Debe tener la regla de directiva de una exportación como mínimo y cinco reglas de directiva de exportación como máximo.  

* Solución   
Asegúrese de que el índice no ya se usa y que se encuentra en el rango de 1 a 5.

* Solución alternativa   
Utilice un índice diferente para la regla que está intentando establecer.


