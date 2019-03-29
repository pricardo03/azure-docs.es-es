---
title: Recopilar datos de eventos de seguridad de Windows en Azure Sentinel Preview | Microsoft Docs
description: Obtenga información sobre cómo recopilar datos de eventos de seguridad de Windows en Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 90bff4cd98046a42fa31d98ff00e76d7fc9ac307
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576095"
---
# <a name="connect-windows-security-events"></a>Conexión de eventos de seguridad de Windows 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir todos los eventos de seguridad de los servidores de Windows conectados al área de trabajo Centinela de Azure. Esta conexión le permite ver los paneles, crear alertas personalizadas y mejorará la investigación. Esto le ofrece más información sobre la red de su organización y mejora las capacidades de seguridad.  Puede seleccionar qué eventos de streaming:

- **Todos los eventos** -seguridad de todos los Windows y los eventos de AppLocker.
- **Common** -un conjunto estándar de eventos con fines de auditoría.
- **Mínimo** -un pequeño conjunto de eventos que pueden indicar posibles amenazas. Si habilita esta opción, no podrá tener una pista de auditoría completa.
- **Ninguno** -sin seguridad o los eventos de AppLocker.

> [!NOTE]
> 
> - Datos se almacenarán en la ubicación geográfica del área de trabajo en el que se ejecuta Azure Sentinel.

## <a name="set-up-the-windows-security-events-connector"></a>Configurar el conector de eventos de seguridad de Windows

Para integrar totalmente los eventos de seguridad de Windows con Azure centinela:

1. En el portal de Azure Sentinel, seleccione **la recopilación de datos** y, a continuación, haga clic en el **eventos de seguridad de Windows** icono. 
1. Seleccione los tipos de datos que desea transmitir.
1. Haga clic en **Update**(Actualizar).
6. Para usar el esquema correspondiente en Log Analytics para los eventos de seguridad de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Puede tardar unos 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar los eventos de seguridad de Windows con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

