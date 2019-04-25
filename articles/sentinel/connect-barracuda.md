---
title: Conecte los datos de Barracuda para la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectarse a datos Barracuda Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: d8e92fd3918230b48449926dcbb7528d919fd96f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60443932"
---
# <a name="connect-your-barracuda-appliance"></a>Conecte el dispositivo de Barracuda 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Conector de Firewall de aplicaciones Web (WAF) de Barracuda le permite conectar fácilmente los registros de Barracuda con su Centinela de Azure, para ver los paneles, crear alertas personalizadas y mejorará la investigación. Esto le ofrece más información sobre la red de su organización y mejora las capacidades de seguridad. Sentinel Azure aprovecha las ventajas de la integración nativa entre **Barracuda** y Microsoft Azure OMS para proporcionar una integración sin problemas. 


> [!NOTE]
> Datos se almacenarán en la ubicación geográfica del área de trabajo en el que se ejecuta Azure Sentinel.

## <a name="configure-and-connect-barracuda-waf"></a>Configurar y conectar Barracuda WAF
Barracuda Web Application Firewall puede integrar y exportar registros directamente a Azure Sentinel a través del servidor de OMS de Azure.
1. Vaya a [flujo de la configuración de Barracuda WAF](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)y siga las instrucciones para configurar la conexión, mediante estos parámetros:
    - **Id. de área de trabajo**: copie el valor de su identificador de área de trabajo de la página del conector Azure Sentinel Barracuda.
    - **Clave principal**: copie el valor de la clave principal de la página del conector Azure Sentinel Barracuda.
2. En el portal de Azure Sentinel, vaya al área de trabajo en el que ha implementado Azure Sentinel y seleccione el botón de puntos suspensivos (...) al final de la fila y seleccione **configuración avanzada**. 
1. Seleccione **datos** y, a continuación, **Syslog**.
1. Asegúrese de que la instalación se establecen en Barracuda existe y establece la gravedad y haga clic en **guardar**.
6. Para usar el esquema correspondiente en Log Analytics para los eventos de Barracuda, busque **CommonSecurityLog**.


## <a name="validate-connectivity"></a>Validar conectividad

Puede tardar más de 20 minutos hasta que los registros se empiecen a aparecer en Log Analytics. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar dispositivos Barracuda a Centinela de Azure. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

