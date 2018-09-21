---
title: 'Azure AD Connect Health: Alerta sobre que los datos del servicio de mantenimiento no están actualizados | Documentos de Microsoft'
description: Este documento describe la causa de la alerta "Los datos del servicio de mantenimiento no están actualizados" y cómo solucionar el problema.
services: active-directory
documentationcenter: ''
author: zhiweiw
manager: maheshu
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: zhiweiw
ms.openlocfilehash: 430ea5f0a6f737d7632a4352c24d893368b80558
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46310384"
---
# <a name="health-service-data-is-not-up-to-date-alert"></a>Alerta sobre que los datos del servicio de mantenimiento no están actualizados

## <a name="overview"></a>Información general
<li>Azure AD Connect Health genera una alerta de datos actualizada si no recibe todos los puntos de datos del servidor durante dos horas. El título de la alerta es **Los datos del servicio de mantenimiento no están actualizados**. </li>
<li>La alerta con el estado **Advertencia** se desencadena si Connect Health no recibe elementos de datos parciales enviados desde el servidor durante dos horas. La alerta con el estado de advertencia no desencadena notificaciones de correo electrónico para el administrador de inquilinos. </li>
<li>La alerta con el estado **Error** se desencadena si Connect Health no recibe ningún elemento de datos enviado desde el servidor durante dos horas. La alerta con el estado de error desencadena notificaciones de correo electrónico para el administrador de inquilinos. </li>

>[!IMPORTANT] 
> Esta alerta sigue la [directiva de retención de datos](reference-connect-health-user-privacy.md#data-retention-policy) de Connect Health

## <a name="troubleshooting-steps"></a>Pasos para solucionar problemas 
* Asegúrese de repasar y cumplir la [sección de los requisitos](how-to-connect-health-agent-install.md#requirements).
* Utilice la [herramienta de pruebas de conectividad](how-to-connect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service) para detectar problemas de conectividad.


## <a name="next-steps"></a>Pasos siguientes
* [Preguntas más frecuentes de Azure AD Connect Health](reference-connect-health-faq.md)
