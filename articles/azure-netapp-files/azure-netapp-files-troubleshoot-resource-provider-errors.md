---
title: Solución de errores del proveedor de recursos de Azure NetApp Files | Microsoft Docs
description: Se describen las causas, soluciones y soluciones alternativas para los errores comunes del proveedor de recursos de Azure NetApp Files.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63769442"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Solución de errores del proveedor de recursos de Azure NetApp Files
En este artículo se describen los errores comunes del proveedor de recursos de Azure NetApp Files, así como sus causas, soluciones y soluciones alternativas. 

<a name="error_01"></a>***Azure Key Vault no configurado.***   
Azure Key Vault almacena las credenciales necesarias para acceder a la API subyacente. Este error indica que Azure Key Vault no recibió credenciales completas acceder a la API subyacente.

* Causa  
Azure Key Vault no recibió las credenciales correctas o las credenciales están incompletas.  

* Solución   
El servicio de Azure NetApp Files usa Azure Key Vault. Azure Key Vault se autentica mediante un token de Azure Active Directory. Por lo tanto, el propietario de la aplicación debe registrar esta en Azure Active Directory.

* Solución alternativa   
Ninguno.  Azure Key Vault debe configurarse correctamente para el uso de Azure NetApp Files.  

<a name="error_02"></a>***No se puede cambiar el token de creación.***   
Este error se produce al intentar cambiar el token de creación después de que el volumen se haya creado.
El token de creación se debe establecer cuando el volumen se crea y no se puede cambiar más adelante.

* Causa   
Está intentando cambiar el token de creación después de la creación del volumen, que no es una operación admitida.

* Solución   
Una vez creado el volumen, considere la posibilidad de quitar el parámetro de la solicitud para descartar el mensaje de error.

* Solución alternativa   
Si necesita cambiar el token de creación, puede crear un nuevo volumen con un nuevo token de creación y luego migrar los datos al nuevo volumen.


<a name="error_03"></a>***El token de creación debe tener una longitud de al menos 16 caracteres.***   
Este error se produce cuando el token de creación no cumple el requisito de longitud. La longitud del token de creación debe ser de al menos 16 caracteres.

* Causa   
El token de creación no cumple el requisito de longitud.  Cuando se crea un volumen mediante la API, se requiere un token de creación. Si usa el portal, el token se puede generar automáticamente.

* Solución   
Aumente la longitud del token de creación. Por ejemplo, puede agregar otra palabra al principio o al final del token de creación.

* Solución alternativa   
La longitud mínima requerida del token de creación no se puede omitir.  Puede usar un prefijo o sufijo para aumentar la longitud del token de creación.


<a name="error_04"></a>***Error al eliminar un volumen que no se encontró en Azure NetApp Files.***   
Este error se produce porque el registro interno de recursos no está sincronizado.

* Causa   
El volumen se puede seguir mostrado en el portal durante un tiempo después de su eliminación. Si elimina el volumen mediante la API, es posible que el volumen no se haya especificado correctamente. El error también se puede deber a una memoria caché de explorador obsoleta.

* Solución   
Si usa el portal, borre la memoria caché del explorador. También hay una memoria caché interna que se actualiza cada 10 minutos.  Puede intentar borrar la memoria caché de nuevo.  Si el problema persiste después de 10 minutos, puede crear una incidencia de soporte técnico.

* Solución alternativa   
Mientras tanto, use un volumen diferente durante y haga caso omiso del existente.


<a name="error_05"></a>***Error al insertar un nuevo volumen que se encuentra en Azure NetApp Files.***   
Este error se produce porque el registro interno de recursos no está sincronizado.

* Causa   
El volumen se puede seguir mostrado en el portal durante un tiempo después de su eliminación. Si elimina el volumen mediante la API, es posible que el volumen no se haya especificado correctamente.

* Solución   
Si usa el portal, el volumen ya se ha creado.  El volumen debería aparecer automáticamente. Si el problema persiste, puede crear una incidencia de soporte técnico.

* Solución alternativa   
Puede crear un volumen con un nombre diferente y un token de creación diferente.


<a name="error_06"></a>***El nombre de la ruta de acceso de archivo solo puede contener letras, números y guiones ("-").***   
Este error se produce cuando la ruta de acceso contiene caracteres no admitidos, por ejemplo, un punto ("."), coma (","), carácter de subrayado ("\_") o signo de dólar ("$").

* Causa   
La ruta de acceso contiene caracteres no admitidos, por ejemplo, un punto ("."), coma (","), carácter de subrayado ("\_") o signo de dólar ("$").

* Solución   
Quite los caracteres que no son letras alfabéticas, números o guiones ("-") de la ruta de acceso de archivo especificado.

* Solución alternativa   
Puede reemplazar un carácter de subrayado con un guion o usar mayúsculas y minúsculas en lugar de espacios para indicar el comienzo de nuevas palabras (por ejemplo, usar "NuevoVolumen" en lugar de "nuevo volumen").


<a name="error_07"></a>***El id. de volumen no se puede cambiar.***   
Este error se produce cuando intenta cambiar el identificador de volumen.  El cambio del id. de volumen no es una operación admitida.

* Causa   
El id. del sistema de archivos se establece en el momento de creación del volumen. El id. de volumen no se puede cambiar posteriormente.

* Solución   
Ninguno.

* Solución alternativa   
Ninguno.  El id. de volumen se genera cuando el volumen se crea y no se puede cambiar posteriormente.


<a name="error_08"></a>***Se recibió un valor no válido '{0}' para {1}.***   
Este mensaje indica un error en los campos para RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Causa   
Hubo un error en la solicitud de validación de entrada para al menos uno de los siguientes campos: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Solución   
Asegúrese de establecer todos los parámetros obligatorios y no conflictivos en la línea de comandos. Por ejemplo, no se pueden establecer los parámetros UnixReadOnly y UnixReadWrite al mismo tiempo.

* Solución alternativa   
Consulte la sección Solución.  


<a name="error_09"></a>***Falta el valor de '{0}'.***   
Este error indica que falta un atributo obligatorio en la solicitud para al menos uno de los parámetros siguientes: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Causa   
Hubo un error en la solicitud de validación de entrada para al menos uno de los siguientes campos: RuleIndex, AllowedClients, UnixReadOnly, UnixReadWrite, Nfsv3 y Nfsv4.

* Solución   
Asegúrese de establecer todos los parámetros obligatorios y no conflictivos en la línea de comandos. Por ejemplo, no se pueden establecer los parámetros UnixReadOnly y UnixReadWrite al mismo tiempo.

* Solución alternativa   
Consulte la sección Solución.  


<a name="error_10"></a> ***{0} ya está en uso.***   
Este error indica que se nombre del recurso ya se ha usado.

* Causa   
Está intentando crear un volumen con un nombre que es igual al nombre de un volumen existente.

* Solución   
Use un nombre único al crear un volumen.

* Solución alternativa   
Si fuera necesario, puede cambiar el nombre del volumen existente para que el nuevo volumen pueda usar el nombre deseado.


<a name="error_11"></a> ***{0} demasiado corto.***   
Este error indica que el nombre del volumen no cumple el requisito de longitud mínima.

* Causa   
El nombre del volumen es demasiado corto.

* Solución   
Aumente la longitud del nombre del volumen.  

* Solución alternativa   
Puede agregar un prefijo o sufijo común al nombre del volumen.


<a name="error_12"></a>***API de Azure NetApp Files inaccesible.***   
La API de Azure depende de la API de Azure NetApp Files para administrar volúmenes.  Este error indica un problema con la conexión de la API.

* Causa   
La API subyacente no responde, lo que resulta en un error interno. Es probable que el error sea temporal.

* Solución   
Es probable que el problema sea temporal.  La solicitud debería realizarse correctamente después de un tiempo.

* Solución alternativa   
Ninguno. La API subyacente es esencial para la administración de volúmenes.  


<a name="error_13"></a>***Se encontraron credenciales para la suscripción '{0}'.***   
Este error indica que las credenciales proporcionadas no son válidas o no se han establecido correctamente en la suscripción.

* Causa   
Las credenciales que no son válidas o se establecieron de manera incorrecta impiden el acceso al servicio para la administración de volúmenes.

* Solución   
Asegúrese de que las credenciales se establecen y especifican correctamente en la línea de comandos.

* Solución alternativa   
Ninguno.  Establecer las credenciales correctamente es fundamental para usar Azure NetApp Files.  


<a name="error_14"></a>***Se encontró ningún id. de resultado de operación para '{0}'.***   
Este error indica que un error interno impide la finalización de la operación.

* Causa   
Hubo un error interno que impidió la finalización de la operación.

* Solución   
Es probable que el error sea temporal.  Espere unos minutos y pruebe otra vez. Si el problema persiste, cree una incidencia para que el soporte técnico investigue el problema.

* Solución alternativa   
Espere unos minutos y compruebe si el problema persiste.


<a name="error_15"></a>***Operación '{0}' no admitida.***   
Este error indica que el comando no está disponible para la suscripción o recurso activo.

* Causa   
La operación no está disponible para la suscripción o recurso.

* Solución   
Asegúrese de que escribió correctamente el comando y que este esté disponible para el recursos y suscripción que esté usando.

* Solución alternativa   
Consulte la sección Solución.  


<a name="error_16"></a>***No se admite la operación de revisión para este tipo de recurso.***   
Este error se produce al intentar cambiar el destino de montaje o instantánea.

* Causa   
El destino de montaje se define en el momento de su creación y no se puede modificar posteriormente.

* Solución   
Ninguno.  El destino de montaje no se puede modificar después de que se haya creado el volumen.

* Solución alternativa   
Ninguno.


<a name="error_17"></a>***Recibió un valor para propiedad de solo lectura '{0}'.***   
Este error se produce cuando se define un valor para una propiedad que no se puede modificar. Por ejemplo, no se puede cambiar el id. del volumen.

* Causa   
Intentó modificar un parámetro (por ejemplo, el Id. de volumen) que no se puede modificar.

* Solución   
Ninguno. No se puede modificar el parámetro para el id. de volumen.

* Solución alternativa   
No debería ser necesario cambiar el id. de volumen.  Por lo tanto, no se necesita una solución alternativa.

<a name="error_18"></a>***Objeto {0} solicitado no encontrado.***   
Este error se produce cuando se intenta hacer referencia a un recurso que no existe, por ejemplo, un volumen o una instantánea. Es posible que el recurso se haya eliminado o tenga un nombre de recurso con errores ortográficos.

* Causa   
Está intentando hacer referencia a un recurso que no existe (por ejemplo, un volumen o instantánea) que ya se ha eliminado o que tiene un nombre de recurso mal escrito.

* Solución   
Compruebe la solicitud para ver si tiene errores de ortografía y asegurarse de que se hace referencia a ella correctamente.

* Solución alternativa   
Consulte la sección Solución.

<a name="error_19"></a>***No se pueden obtener las credenciales para la suscripción '{0}'.***   
Este error indica que las credenciales proporcionadas no son válidas o no se establecieron de manera incorrecta en la suscripción.

* Causa   
Las credenciales que no son válidas o que se establecieron de manera incorrecta en la suscripción impiden el acceso al servicio para la administración de volúmenes.

* Solución   
Asegúrese de que las credenciales se establecen y especifican correctamente en la línea de comandos.

* Solución alternativa   
Ninguno.  El correcto establecimiento de credenciales es fundamental para el uso de Azure NetApp Files.

<a name="error_20"></a>***Error desconocido de Azure NetApp Files.***   
La API de Azure depende de la API de Azure NetApp Files para administrar volúmenes. El error indica que hay un problema en la comunicación con la API.

* Causa   
La API subyacente está enviando un error desconocido.  Es probable que el error sea temporal.

* Solución   
Es probable que el problema sea temporal y la solicitud debería realizarse correctamente después de un tiempo. Si el problema persiste, cree una incidencia de soporte técnico para que el problema se investigue.

* Solución alternativa   
Ninguno.  La API subyacente es esencial para la administración de volúmenes.

<a name="error_21"></a>***Valor que recibió para una propiedad '{0}' desconocida.***   
Este error se produce cuando se proporcionan propiedades no existentes para un recurso, como el volumen, instantánea o destino de montaje.

* Causa   
La solicitud tiene un conjunto de propiedades que se pueden usar con cada recurso.  No se pueden incluir propiedades inexistentes en la solicitud.

* Solución   
Asegúrese de que todos los nombres de propiedad se hayan escrito correctamente y que las propiedades estén disponibles para la suscripción y recurso.

* Solución alternativa   
Reduzca el número de propiedades definidas en la solicitud para eliminar la propiedad que provoca el error.


<a name="error_22"></a>***No se admite la operación de actualización para este tipo de recurso.***   
Solo los volúmenes se pueden actualizar. Este error se produce cuando se intenta realizar una operación de actualización no admitida, por ejemplo, actualizar una instantánea.

* Causa   
El recurso que intenta actualizar no admite la operación de actualización.  Solo los volúmenes permiten la modificación de sus propiedades.

* Solución   
Ninguno.  El recurso que intenta actualizar no admite la operación de actualización. Por lo tanto, no se puede modificar.

* Solución alternativa   
Para un volumen, cree un nuevo recurso con la actualización en contexto y migre los datos.


<a name="error_23"></a>***Número de elementos: {0} para el objeto: {1} está fuera del rango mínimo-máximo.***   
Este error se produce cuando las reglas de directivas de exportación no cumplen el requisito de rango mínimo o máximo.  Si define la directiva de exportación, esta debe tener una regla de directiva de exportación como mínimo y cinco reglas de directiva de exportación como máximo.

* Causa   
La directiva de exportación definida no cumple el rango necesario.  

* Solución   
Asegúrese de que el índice no esté en uso y que se encuentre en el rango de 1 a 5.

* Solución alternativa   
No es obligatorio usar directivas de exportación en los volúmenes. Por lo tanto, puede omitir la directiva de exportación por completo si no necesita tener reglas de directiva de exportación.


<a name="error_24"></a>***Error de valor duplicado para el objeto {0}.***   
Este error se produce cuando la directiva de exportación no está definida con un índice único.  Al definir directivas de exportación, todas las reglas de directiva de exportación deben tener un índice único entre 1 y 5.

* Causa   
La directiva de exportación definida no cumple el requisito de reglas de directivas de exportación. Debe tener como mínimo una regla de directiva de exportación y como máximo cinco reglas de directiva de exportación.  

* Solución   
Asegúrese de que el índice no esté en uso y que se encuentre en el rango de 1 a 5.

* Solución alternativa   
Use un índice diferente para la regla que está intentando establecer.


