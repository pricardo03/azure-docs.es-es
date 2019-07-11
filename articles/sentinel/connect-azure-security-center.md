---
title: Conectar datos de Azure Security Center a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Azure Security Center a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 697983000f84f9e4efe7e2c8ef9dbb2f636e0735
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620577"
---
# <a name="connect-data-from-azure-security-center"></a>Conectar datos de Azure Security Center

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Azure Sentinel permite conectar alertas de [Azure Security Center](../security-center/security-center-intro.md) y transmitirlos a Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Si quiere exportar alertas de Azure Security Center, debe ser colaborador de la suscripción cuyos registros se van a transmitir.

- En la suscripción debe ejecutarse el [nivel estándar de Azure Security Center](../security-center/security-center-pricing.md). Si no es así, [actualícela al nivel estándar](https://azure.microsoft.com/pricing/details/security-center/).

- Debe iniciar sesión con un usuario que tenga permisos de administrador de seguridad o de administrador global en cada suscripción que quiera conectar.


## <a name="connect-to-azure-security-center"></a>Conectar a Azure Security Center

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Security Center**.
1. A la derecha, haga clic en **Connect** (Conectar) junto a cada suscripción cuyas alertas quiera transmitir a Azure Sentinel. Asegúrese de actualizar cada suscripción al nivel estándar de Azure Security Center para poder transmitir alertas a Azure Sentinel.

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure Security Center, busque **SecurityEvent**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Security Center a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
