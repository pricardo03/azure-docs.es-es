---
title: Recopilación de datos de Azure AD Identity Protection en versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo recopilar datos de Azure AD Identity Protection en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 91c870e5-2669-437f-9896-ee6c7fe1d51d
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 609aced38b7e30f78d81934867196c568dcc85ca
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57240005"
---
# <a name="collect-data-from-azure-ad-identity-protection"></a>Recopilar datos de Azure AD Identity Protection

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de [Azure AD Identity Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) en Azure Sentinel a las alertas de flujo en Azure Centinela para ver los paneles, crear alertas personalizadas y mejorará la investigación. Azure Active Directory Identity Protection proporciona una vista consolidada en usuarios de riesgo, eventos de riesgo y vulnerabilidades, con la capacidad de corregir el riesgo de inmediato y establecer directivas para resolver automáticamente los eventos futuros. El servicio se basa en la experiencia de Microsoft para proteger las identidades de consumidor y logra una de más de 13 millones inicios de sesión día. 


## <a name="prerequisites"></a>Requisitos previos

- Debe tener un [licencia Azure Active Directory Premium P1 o P2](https://azure.microsoft.com/pricing/details/active-directory/)
- Usuario con permisos de administrador de seguridad o de administrador global


## <a name="connect-to-azure-ad-identity-protection"></a>Conectarse a Azure AD Identity Protection

Si ya tiene Azure AD Identity Protection, asegúrese de que es [habilitado en la red](../active-directory/identity-protection/enable.md).
Si se implementa Azure AD Identity Protection y obtención de datos, los datos de alerta fácilmente se pueden transmitir a Azure Sentinel.


1. En Azure Sentinel, seleccione **la recopilación de datos** y, a continuación, haga clic en el **Azure AD Identity Protection** icono.

2. Haga clic en **Connect** para iniciar el streaming de eventos de Azure AD Identity Protection en Azure Sentinel.


6. Para usar el esquema correspondiente en Log Analytics para las alertas de Azure AD Identity Protection, busque **IdentityProtectionLogs_CL**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar Azure AD Identity Protection con Azure Sentinel. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
