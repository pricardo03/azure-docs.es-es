---
title: Conexión de datos de Azure Security Center con Azure Sentinel, versión preliminar | Microsoft Docs
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
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: a9c210531f2c4cab1c3c023eab795023c3ad9f0c
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240219"
---
# <a name="connect-data-from-azure-security-center"></a>Conectar datos de Azure Security Center





Azure Sentinel permite conectar alertas de [Azure Security Center](../security-center/security-center-intro.md) y transmitirlos a Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Si quiere exportar alertas de Azure Security Center, debe ser colaborador de la suscripción cuyos registros se van a transmitir.

- En la suscripción debe ejecutarse el [nivel estándar de Azure Security Center](../security-center/security-center-pricing.md). Si no es así, [actualícela al nivel estándar](https://azure.microsoft.com/pricing/details/security-center/).

- Debe iniciar sesión con un usuario que tenga permisos de administrador de seguridad o de administrador global en cada suscripción que quiera conectar.


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
