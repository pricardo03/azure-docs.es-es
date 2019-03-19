---
title: Solución de problemas de BizTalk Services mediante registros de operaciones | Microsoft Docs
description: Solucione los problemas de BizTalk Services con el uso de registros de operaciones. MABS, WABS
services: biztalk-services
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
ms.assetid: 1081a9c6-58cc-4a76-8ac8-11e5e7a6ea27
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: e58a62761284e0671c0083d41f5dde4c13b32fe2
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58108263"
---
# <a name="biztalk-services-troubleshoot-using-operation-logs"></a>BizTalk Services: Solución de problemas con la utilización de los registros de operaciones

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]
> 
> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="what-are-the-operation-logs"></a>Qué son los registros de operaciones
Registros de operaciones es una característica de Servicios de administración, que permite consultar registros históricos de operaciones realizadas realizados en servicios de Azure, como BizTalk Services. Esta característica le permite consultar datos históricos relacionados con las operaciones de administración en su suscripción a Servicios de BizTalk de hasta 180 días de antigüedad.

> [!NOTE]
> Esta característica únicamente captura registros para operaciones de administración de BizTalk Services, como cuándo se ha iniciado el servicio, una copia de seguridad, y así sucesivamente. El seguimiento de estas operaciones se realiza mediante las [API de REST de BizTalk Services](https://msdn.microsoft.com/library/azure/dn232347.aspx). Para obtener una lista completa de las operaciones que se siguen mediante los Servicios de administración, consulte [Seguimiento de operaciones mediante Servicios de administración de Azure](#bizops).<br/><br/>
>  No captura los registros de actividades relacionadas con el tiempo de ejecución de los Servicios de BizTalk (como mensajes procesados por puentes, etc.). Para consultar estos registros, debe utilizar la vista de seguimiento desde el portal de BizTalk Services. Para obtener más información, consulte [Mensajes de seguimiento](https://msdn.microsoft.com/library/azure/hh949805.aspx).
> 
> 

## <a name="view-biztalk-services-operation-logs"></a>Consulta de los registros de operaciones de BizTalk Services
1. En el portal, seleccione **Servicios de administración** y, después, seleccione la pestaña **Registros de operaciones**.
2. Puede filtrar los registros en función de diferentes parámetros, como suscripción, intervalo de fechas, tipo de servicio (por ejemplo, BizTalk Services), nombre de servicio o estado (de la operación, por ejemplo, Succeeded, Failed).
3. Seleccione la marca de verificación para consultar la lista filtrada. La siguiente imagen muestra las actividades relacionadas con testbiztalkservice: ![Ver registros de operación][ViewLogs] 
4. Para obtener más información sobre una operación específica, seleccione la fila y haga clic en **Detalles** en la barra de tareas situada en la parte inferior.

## <a name="bizops"></a>Seguimiento de operaciones mediante Servicios de administración de Azure
En la tabla siguiente se enumeran las operaciones cuyo seguimiento se realiza utilizando Servicios de administración de Azure.

| Nombre de la operación | Tarea |
| --- | --- |
| CreateBizTalkService |Operación para crear un nuevo Servicio BizTalk |
| DeleteBizTalkService |Operación para eliminar un Servicio BizTalk |
| RestartBizTalkService |Operación para reiniciar un Servicio BizTalk |
| StartBizTalkService |Operación para iniciar un Servicio BizTalk |
| StopBizTalkService |Operación para detener un Servicio BizTalk |
| DisableBizTalkService |Operación para deshabilitar un Servicio BizTalk |
| EnableBizTalkService |Operación para habilitar un Servicio BizTalk |
| BackupBizTalkService |Operación para hacer una copia de seguridad de un Servicio BizTalk |
| RestoreBizTalkService |Operación para restaurar un Servicio BizTalk a partir de una copia de seguridad especificada |
| SuspendBizTalkService |Operación para suspender un Servicio BizTalk |
| ResumeBizTalkService |Operación para reanudar un Servicio BizTalk |
| ScaleBizTalkService |Operación para escalar o reducir verticalmente un Servicio BizTalk |
| ConfigUpdateBizTalkService |Operación para actualizar la configuración de un Servicio BizTalk |
| ServiceUpdateBizTalkService |Operación para actualizar o degradar un Servicio BizTalk a una versión diferente |
| PurgeBackupBizTalkService |Operación para purgar copias de seguridad del Servicio BizTalk fuera el período de retención |

## <a name="see-also"></a>Vea también
* [Copia de seguridad del servicio de BizTalk](https://go.microsoft.com/fwlink/p/?LinkID=325584)
* [Restauración del servicio de BizTalk a partir de una copia de seguridad](https://go.microsoft.com/fwlink/p/?LinkID=325582)
* [BizTalk Services: Developer, Basic, Standard y Premium gráfico de ediciones](https://go.microsoft.com/fwlink/p/?LinkID=302279)
* [BizTalk Services: Aprovisionamiento](https://go.microsoft.com/fwlink/p/?LinkID=302280)
* [BizTalk Services: Gráfico del estado de aprovisionamiento](https://go.microsoft.com/fwlink/p/?LinkID=329870)
* [BizTalk Services: Pestañas panel, Monitor y escala](https://go.microsoft.com/fwlink/p/?LinkID=302281)
* [BizTalk Services: limitación](https://go.microsoft.com/fwlink/p/?LinkID=302282)
* [BizTalk Services: Nombre del emisor y clave del emisor](https://go.microsoft.com/fwlink/p/?LinkID=303941)
* [¿Cómo puedo comenzar a utilizar el SDK de Azure BizTalk Services?](https://go.microsoft.com/fwlink/p/?LinkID=302335)

[ViewLogs]: ./media/biztalk-troubleshoot-using-ops-logs/Operation-Logs.png

