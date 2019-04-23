---
title: Conectarse a datos de firewall de Windows a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectarse a datos de firewall de Windows Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 27a82545f77fe15541ac598abadbfc8ffb6d3405
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790967"
---
# <a name="connect-windows-firewall"></a>Conexión del firewall de Windows

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El conector de firewall de Windows le permite conectarse fácilmente a los registros de Firewall de Windows, si están conectados al área de trabajo Centinela de Azure. Esta conexión le permite ver los paneles, crear alertas personalizadas y mejorará la investigación. Esto le ofrece más información sobre la red de su organización y mejora las capacidades de seguridad.  


> [!NOTE]
> Datos se almacenarán en la ubicación geográfica del área de trabajo en el que se ejecuta Azure Sentinel.

## <a name="enable-the-connector"></a>Habilitar el conector 

1. En el portal de Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **firewall de Windows** icono. 
1. Seleccione los tipos de datos que desea transmitir.
1. Haga clic en **Instalar**.
6. Para usar el esquema correspondiente en Log Analytics para el firewall de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Puede tardar más de 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar el firewall de Windows con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

