---
title: Recopilación de datos de Azure Security Center en versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo recopilar datos de Azure Security Center en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: f59c5f6a9f497a6420172996f9f327f16ffd26f9
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242282"
---
# <a name="collect-data-from-azure-security-center"></a>Recopilar datos de Azure Security Center

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Sentinel Azure le permite recopilar las alertas de [Azure Security Center](../security-center/security-center-intro.md) y su transmisión a través de Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Si desea exportar las alertas de Azure Security Center, debe ser un colaborador de la suscripción cuyos registros se transmite.

- Debe tener la [nivel estándar de Azure Security Center](../security-center/security-center-pricing.md) que se ejecutan en la suscripción. Si no es así, [actualizar su suscripción a estándar](https://azure.microsoft.com/pricing/details/security-center/).

- Debe iniciar sesión con un usuario que tenga permisos de administrador de seguridad o de administrador global en cada suscripción que desea conectarse.


## <a name="connect-to-azure-security-center"></a>Conectarse a Azure Security Center

1. En Azure Sentinel, seleccione **la recopilación de datos** y, a continuación, haga clic en el **Azure Security Center** icono.
1. En la parte derecha, haga clic en **Connect** situada junto a cada suscripción cuyas alertas desea transmitir en Centinela de Azure. Asegúrese de actualizar cada suscripción al nivel estándar de Azure Security Center a las alertas de flujo Centinela de Azure.

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para las alertas de Azure Security Center, busque **SecurityEvent**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar Azure Security Center con Azure Sentinel. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
