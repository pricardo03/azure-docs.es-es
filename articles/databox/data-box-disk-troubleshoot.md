---
title: Solución de problemas de Azure Data Box Disk | Microsoft Docs
description: Se describe cómo solucionar los problemas observados en Azure Data Box Disk.
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/12/2018
ms.author: alkohli
ms.openlocfilehash: f63eadc9a6ab834e02d62be9a209b504d76d3c8e
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090782"
---
# <a name="troubleshoot-issues-in-azure-data-box-disk-preview"></a>Solución de problemas de Azure Data Box Disk (versión preliminar)

Este artículo se aplica a la ejecución de Microsoft Azure Data Box en versión preliminar. En este artículo se describen algunos de los flujos de trabajo complejos y tareas de administración que se pueden realizar en Data Box y Data Box Disk. 

Puede administrar Data Box mediante la interfaz de usuario del servicio Data Box (conocida como la interfaz de usuario del portal) y la interfaz de usuario web local del dispositivo. Data Box Disk solo se puede administrar mediante Azure Portal. Este artículo se centra en las tareas que se pueden realizar mediante Azure Portal. Use Azure Portal para administrar pedidos, administrar dispositivos y realizar un seguimiento del estado del pedido mientras continúa hacia la fase final.

Este artículo incluye los siguientes tutoriales:

- Descarga de los registros de diagnóstico
- Consulta de los registros de actividad


> [!IMPORTANT]
> Data Box se encuentra en versión preliminar. Antes de implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="download-diagnostic-logs"></a>Descarga de los registros de diagnóstico

Si existen errores durante el proceso de copia de los datos, en el portal se muestra la ruta de acceso a la carpeta donde se encuentran los registros de diagnóstico. 

Los registros de diagnóstico pueden ser:
- Registros de error
- Registros detallados  

Para navegar a la ruta de acceso del registro de copia, vaya a la cuenta de almacenamiento asociada con el pedido de Data Box. 

1.  Vaya a **General > Detalles del pedido** y tome nota de la cuenta de almacenamiento asociada con su pedido.
 

2.  Vaya a **Todos los recursos** y busque la cuenta de almacenamiento identificada en el paso anterior. Seleccione la cuenta de almacenamiento y haga clic en ella.

    ![Registros de copia 1](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs1.png)

3.  Vaya a **Blob service > Examinar blobs** y busque el blob correspondiente a la cuenta de almacenamiento. Vaya a **diagnosticslogcontainer > waies**. 

    ![Registros de copia 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

    Verá los registros de errores y los registros detallados de la copia de los datos. Seleccione cada archivo y haga clic en él y luego descargue una copia local.

## <a name="query-activity-logs"></a>Consulta de los registros de actividad

Use los registros de actividad para encontrar errores al solucionar problemas o para supervisar cómo un usuario de su organización modificó un recurso. Mediante los registros de actividad, puede determinar:

- Qué operaciones se realizaron en los recursos de la suscripción.
- La persona que inicia la operación. 
- Cuándo tuvo lugar la operación.
- Estado de la operación.
- Los valores de otras propiedades que podrían ayudarle en la investigación de la operación.

El registro de actividad contiene todas las operaciones de escritura (por ejemplo, PUT, POST, DELETE) realizadas en los recursos, pero no las operaciones de lectura (por ejemplo, GET). 

Los registros de actividad se conservan 90 días. Puede consultar cualquier intervalo de fechas, siempre que no hayan transcurrido más de 90 días desde la fecha inicial. También puede filtrar por una de las consultas integradas en Insights. Por ejemplo, haga clic en el error y, luego, seleccione y haga clic en los errores específicos para entender la causa.

## <a name="data-box-disk-unlock-tool-errors"></a>Errores de la herramienta de desbloqueo de Data Box Disk


| Comportamiento de la herramienta o mensaje de error      | Recomendaciones                                                                                               |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| None<br><br>La herramienta de desbloqueo de Data Box Disk se bloquea.                                                                            | BitLocker no está instalado. Asegúrese de que el equipo host en el que se ejecuta la herramienta de desbloqueo de Data Box Disk tenga instalado BitLocker.                                                                            |
| No se admite la versión actual de .NET Framework. Las versiones compatibles son 4.5 y posteriores.<br><br>La herramienta se cierra con un mensaje.  | .NET 4.5 no está instalado. Instale .NET 4.5 o posterior en el equipo host donde se ejecuta la herramienta de desbloqueo de Data Box Disk.                                                                            |
| No se ha podido desbloquear ni verificar ningún volumen. Póngase en contacto con el soporte técnico de Microsoft  <br><br>La herramienta no puede desbloquear ni verificar ninguna unidad bloqueada. | La herramienta no ha podido desbloquear ninguna de las unidades de disco desbloqueadas con la clave de paso proporcionada. Póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes.                                                |
| Los siguientes volúmenes están desbloqueados y comprobados. <br>Letras de unidad de volumen: E:<br>No se ha podido desbloquear ningún volumen con las siguientes claves de paso: werwerqomnf, qwerwerqwdfda. <br><br>La herramienta desbloquea algunas unidades y enumera las letras de unidad correctas e incorrectas.| Parcialmente correcto. No se han podido desbloquear algunas de las unidades con la clave de paso proporcionada. Póngase en contacto con el servicio de soporte técnico de Microsoft para conocer los pasos siguientes. |
| No se han podido encontrar los volúmenes bloqueados. Compruebe que el disco recibido de Microsoft está conectado correctamente y se encuentra en estado bloqueado.          | La herramienta no puede encontrar ninguna unidad bloqueada. O las unidades están ya desbloqueadas o no se han detectado. Asegúrese de que las unidades están conectadas y bloqueadas.                                                           |
| Error grave: parámetro no válido<br>Nombre del parámetro: invalid_arg<br>USO:<br>DataBoxDiskUnlock /PassKeys:<passkey_list_separated_by_semicolon><br><br>Ejemplo: DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>Ejemplo: DataBoxDiskUnlock /SystemCheck<br>Ejemplo: DataBoxDiskUnlock /Help<br><br>/PassKeys: obtenga esta clave de paso del pedido de Azure Data Box Disk. La clave de paso desbloquea los discos.<br>/Help: esta opción proporciona ayuda sobre el uso del cmdlet y ejemplos.<br>/SystemCheck: esta opción comprueba si el sistema cumple los requisitos para ejecutar la herramienta.<br><br>Presione cualquier tecla para salir. | Parámetro no válido especificado. Los únicos parámetros permitidos son /SystemCheck, /PassKey y /Help.                                                                            |
## <a name="next-steps"></a>Pasos siguientes

- Aprenda a [administrar Data Box Disk mediante Azure Portal](data-box-portal-ui-admin.md).
