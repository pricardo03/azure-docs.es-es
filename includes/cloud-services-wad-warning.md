---
author: jpconnock
ms.service: cloud-services
ms.topic: include
ms.date: 11/25/2018
ms.author: jeconnoc
ms.openlocfilehash: f23d5246401f23694755e63a51c52da5dbf5ac1b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "67186657"
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
