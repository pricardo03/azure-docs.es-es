---
title: Conectar datos de actividad de Azure a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de actividad de Azure a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: e329c8efd9b0e89f5f5eae41952cda9a45a95969
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620651"
---
# <a name="connect-data-from-azure-activity-log"></a>Conectar datos del registro de actividad de Azure

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se pueden transmitir registros desde el [registro de actividad de Azure](../azure-monitor/platform/activity-logs-overview.md) a Azure Sentinel con tan solo un clic. El registro de actividad es un registro de suscripción que proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. Esta incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Estado del servicio. Con el registro de actividades, se pueden determinar los interrogantes “qué, quién y cuándo” de las operaciones de escritura (PUT, POST, DELETE) en los recursos de la suscripción. También puede conocer el estado de la operación y otras propiedades relevantes. El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo Clásico/"RDFE". 


## <a name="prerequisites"></a>Requisitos previos

- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad


## <a name="connect-to-azure-activity-log"></a>Conectar al registro de actividad de Azure

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Registro de actividad de Azure**.

2. En el panel Registro de actividad de Azure, seleccione las suscripciones que quiera transmitir a Azure Sentinel. 

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de actividad de Azure, busque **AzureActivity**.


 

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar el registro de actividad de Azure a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
