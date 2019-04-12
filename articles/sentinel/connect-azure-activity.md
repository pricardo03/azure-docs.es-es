---
title: Conecte los datos de actividad de Azure a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de actividad de Azure a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 8c25baa8-b93b-41da-9e6c-15bb7b5c5511
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: d0cc13227bfe02594a57a7fb0ba8ee1cb3383d56
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59494685"
---
# <a name="connect-data-from-azure-activity-log"></a>Conectar los datos de registro de actividad de Azure

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de [registro de actividad de Azure](../azure-monitor/platform/activity-logs-overview.md) en Azure Sentinel con un solo clic. El registro de actividad es un registro de suscripción que proporciona información sobre los eventos de nivel de suscripción que se produjeron en Azure. Esta incluye una serie de datos, desde datos operativos de Azure Resource Manager hasta actualizaciones en eventos de Estado del servicio. Con el registro de actividad, se puede determinar el "qué, quién y cuándo ' para cualquier operación (PUT, POST, DELETE) realizada en los recursos en su suscripción de escritura. También puede conocer el estado de la operación y otras propiedades relevantes. El registro de actividad no incluye las operaciones de lectura (GET) ni las operaciones de los recursos que usan el modelo clásico / modelo "RDFE". 


## <a name="prerequisites"></a>Requisitos previos

- Usuario con permisos de administrador de seguridad o de administrador global


## <a name="connect-to-azure-activity-log"></a>Conectarse al registro de actividad de Azure

1. En Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **registro de actividad de Azure** icono.

2. En el panel de registro de actividad de Azure, seleccione las suscripciones que desea transmitir en Centinela de Azure. 

3. Haga clic en **Conectar**.

4. Para usar el esquema correspondiente en Log Analytics para las alertas de actividad de Azure, busque **AzureActivity**.


 

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar el registro de actividad de Azure a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
