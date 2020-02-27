---
title: Conectar datos de actividad de Azure a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de actividad de Azure a Azure Sentinel.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 28d5acd80deef193c7d2fea46f682f26abd756aa
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588610"
---
# <a name="connect-data-from-azure-activity-log"></a>Conectar datos del registro de actividad de Azure



Se pueden transmitir registros desde el [registro de actividad de Azure](../azure-monitor/platform/platform-logs-overview.md) a Azure Sentinel con tan solo un clic. El registro de actividad es un registro de suscripción que proporciona información sobre los eventos de nivel de suscripción que se han producido en Azure. Esta incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Estado del servicio. Con el registro de actividades, se pueden determinar los interrogantes “qué, quién y cuándo” de las operaciones de escritura (PUT, POST, DELETE) en los recursos de la suscripción. También puede conocer el estado de la operación y otras propiedades relevantes. El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo Clásico/"RDFE". 


## <a name="prerequisites"></a>Prerrequisitos

- Usuario con permisos de colaborador en un área de trabajo de Log Analytics 


## <a name="connect-to-azure-activity-log"></a>Conectar al registro de actividad de Azure

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Registro de actividad de Azure**.

2. En el panel Registro de actividad de Azure, seleccione las suscripciones que quiera transmitir a Azure Sentinel. 

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de actividad de Azure, busque **AzureActivity**.


 

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar el registro de actividad de Azure a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
