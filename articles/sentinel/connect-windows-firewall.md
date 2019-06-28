---
title: Conectar datos de Firewall de Windows a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Firewall de Windows a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 9388e267c52ef53b59aacad844e964d3cfeb13d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233828"
---
# <a name="connect-windows-firewall"></a>Conexión del firewall de Windows

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

El conector de Firewall de Windows le permite conectarse fácilmente a los registros de los Firewall de Windows, si están conectados al área de trabajo Azure Sentinel. Esta conexión le permite ver paneles, crear alertas personalizadas y mejorar la investigación, de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad.  


> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="enable-the-connector"></a>Habilitar el conector 

1. En el portal de Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Firewall de Windows**. 
1. Seleccione los tipos de datos que quiera transmitir.
1. Haga clic en **Instalar**.
6. Para usar el esquema correspondiente en Log Analytics para encontrar Firewall de Windows, busque **SecurityEvent**.

## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Firewall de Windows a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

