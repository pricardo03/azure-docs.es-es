---
title: Conexión de datos de Azure Security Center a Azure Sentinel
description: Aprenda a conectar datos de Azure Security Center a Azure Sentinel.
author: rkarlin
manager: rkarlin
ms.assetid: d28c2264-2dce-42e1-b096-b5a234ff858a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 4c75d1870ca3b966acdd658c91c0af8fd7465bfb
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190875"
---
# <a name="connect-data-from-azure-security-center"></a>Conectar datos de Azure Security Center





Azure Sentinel permite conectar alertas de [Azure Security Center](../security-center/security-center-intro.md) y transmitirlos a Azure Sentinel. 

## <a name="prerequisites"></a>Prerrequisitos

- Para exportar alertas desde Azure Security Center, debe tener el rol de lector de seguridad en la suscripción de los registros que transmita.

- En la suscripción debe ejecutarse el [nivel estándar de Azure Security Center](../security-center/security-center-pricing.md). Si no es así, [actualícela al nivel estándar](https://azure.microsoft.com/pricing/details/security-center/).



## <a name="connect-to-azure-security-center"></a>Conectar a Azure Security Center

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Security Center**.

1. A la derecha, haga clic en **Connect** (Conectar) junto a cada suscripción cuyas alertas quiera transmitir a Azure Sentinel. Asegúrese de actualizar cada suscripción al nivel estándar de Azure Security Center para poder transmitir alertas a Azure Sentinel.

1. Puede seleccionar si desea que las alertas de Azure Security Center generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes** seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Luego, puede editar esta regla en **Analytics** (Análisis) y **Active rules** (Reglas activas).

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure Security Center, busque **SecurityAlert**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Security Center a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
