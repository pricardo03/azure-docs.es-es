---
author: tanmaygore
ms.author: tagore
ms.service: cloud-services
ms.topic: include
ms.date: 11/25/2018
ms.openlocfilehash: 5deba9d8968f71ef1b21517e74e6af4c39aa9271
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279424"
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
