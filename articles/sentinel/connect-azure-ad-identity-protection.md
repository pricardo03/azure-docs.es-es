---
title: Conectar datos de Azure AD Identity Protection a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Azure AD Identity Protection a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 5dae8c4da46c750f69057f33d593f5bb7396a99e
ms.sourcegitcommit: 7efb2a638153c22c93a5053c3c6db8b15d072949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72882253"
---
# <a name="connect-data-from-azure-ad-identity-protection"></a>Conectar datos de Azure AD Identity Protection



Puede transmitir registros de [Azure AD Identity Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) a Azure Sentinel para transmitir alertas a Azure Sentinel que permitan ver paneles, crear alertas personalizadas y mejorar la investigación. Azure Active Directory Identity Protection proporciona una vista consolidada de los usuarios en riesgo, las detecciones de riesgo y las vulnerabilidades, con la posibilidad de corregir el riesgo de inmediato y de establecer directivas para resolver eventos futuros de forma automática. Este servicio se basa en la experiencia de Microsoft a la hora de proteger las identidades de los consumidores, y logra una precisión increíble a partir de más 13 mil millones de inicios de sesión día. 


## <a name="prerequisites"></a>Requisitos previos

- Tener una [licencia de Azure Active Directory Premium P1 o P2](https://azure.microsoft.com/pricing/details/active-directory/)
- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad


## <a name="connect-to-azure-ad-identity-protection"></a>Conexión a Azure AD Identity Protection

Si ya tiene Azure AD Identity Protection, asegúrese de que está [habilitado en la red](../active-directory/identity-protection/overview-identity-protection.md).
Si Azure AD Identity Protection ya está implementado y obteniendo datos, los datos de alertas se pueden transmitir a Azure Sentinel muy fácilmente.


1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure AD Identity Protection**.

2. Haga clic en **Connect** (Conectar) para iniciar la transmisión de eventos de Azure AD Identity Protection a Azure Sentinel.


6. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure AD Identity Protection, busque **IdentityProtectionLogs_CL**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure AD Identity Protection a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
