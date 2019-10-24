---
title: Conexión de datos de ATP de Microsoft Defender con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar los datos de Protección contra amenazas avanzada de Microsoft Defender con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2019
ms.author: rkarlin
ms.openlocfilehash: 19d496ebb61a3ceb47f69f661e30ab529dc64f3d
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72257309"
---
# <a name="connect-alerts-from-microsoft-defender-advanced-threat-protection"></a>Conexión de alertas desde Protección contra amenazas avanzada de Microsoft Defender 


> [!IMPORTANT]
> La ingesta de los registros de Protección contra amenazas avanzada de Microsoft Defender se encuentra actualmente en versión preliminar pública.
> Esta característica se ofrece sin contrato de nivel de servicio y no se recomienda para cargas de trabajo de producción.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 

Se pueden transmitir registros desde [Protección contra amenazas avanzada de Microsoft Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) a Azure Sentinel con tan solo un clic. Esta conexión permite transmitir las alertas de Protección contra amenazas avanzada de Microsoft Defender a Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Una licencia válida para Protección contra amenazas avanzada de Microsoft Defender habilitada, tal como se describe en [Validación del aprovisionamiento de licencias y configuración completa para Protección contra amenazas avanzada de Microsoft Defender](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing). 
- Debe ser un administrador o un administrador de seguridad en el inquilino de Azure Sentinel.


## <a name="connect-to-microsoft-defender-advanced-threat-protection"></a>Conexión a Protección contra amenazas avanzada de Microsoft Defender

Si Protección contra amenazas avanzada de Microsoft Defender ya está implementada e ingiriendo datos, las alertas se pueden transmitir a Azure Sentinel muy fácilmente.


1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos), haga clic en el icono **Protección contra amenazas avanzada de Microsoft Defender** y seleccione **Open connector page** (Abrir la página del conector).
1. Haga clic en **Conectar**. 
1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de ATP de Defender, busque **SecurityAlert** y que el **nombre del proveedor** sea **MDATP**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar ATP de Microsoft Defender a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
