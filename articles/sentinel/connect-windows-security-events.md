---
title: Conectar datos de eventos de seguridad de Windows a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de eventos de seguridad de Windows a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: d51d2e09-a073-41c8-b396-91d60b057e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 88b066818d53fd92e8238e270b9bc785d4275186
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233968"
---
# <a name="connect-windows-security-events"></a>Conexión de eventos de seguridad de Windows 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir todos los eventos de seguridad de los servidores de Windows conectados al área de trabajo Azure Sentinel. Esta conexión le permite ver paneles, crear alertas personalizadas y mejorar la investigación, de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad.  Puede seleccionar qué eventos transmitir:

- **Todos los eventos**: todos los eventos de seguridad de Windows y AppLocker.
- **Común**: conjunto estándar de eventos con fines de auditoría.
- **Mínimo**: conjunto reducido de eventos que pueden señalar posibles amenazas. Si habilita esta opción, no podrá tener una pista de auditoría completa.
- **Ninguno**: no se transmite ningún evento de seguridad o de AppLocker.

> [!NOTE]
> 
> - Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="set-up-the-windows-security-events-connector"></a>Configurar el conector de eventos de seguridad de Windows

Para integrar totalmente los eventos de seguridad de Windows con Azure Sentinel:

1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **eventos de seguridad de Windows**. 
1. Seleccione los tipos de datos que quiera transmitir.
1. Haga clic en **Update**(Actualizar).
6. Para usar el esquema correspondiente en Log Analytics para encontrar eventos de seguridad de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir unos 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar eventos de seguridad de Windows a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

