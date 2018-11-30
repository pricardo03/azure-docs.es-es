---
author: jpconnock
ms.service: cloud-services
ms.topic: include
ms.date: 11/25/2018
ms.author: jeconnoc
ms.openlocfilehash: f23d5246401f23694755e63a51c52da5dbf5ac1b
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52331477"
---
> [!WARNING]
> Cuando se habilita el diagnóstico para un rol existente, todas las extensiones que se hayan establecido ya se deshabilitan al implementar el paquete. Entre ellas se incluyen las siguientes:
>
> * Diagnósticos de Microsoft Monitoring Agent
> * Supervisión de la seguridad de Microsoft Azure
> * Microsoft Antimalware                 
> * Microsoft Monitoring Agent
> * Agente del generador de perfiles del servicio de Microsoft      
> * Extensión de dominio de Microsoft Azure        
> * Extensión de Microsoft Azure Diagnostics   
> * Extensión de escritorio remoto de Microsoft Azure
> * Recopilador de registros de Microsoft Azure
>
> Puede volver a establecer sus extensiones a través de Azure Portal o de PowerShell después de haber implementado el rol actualizado.
>
