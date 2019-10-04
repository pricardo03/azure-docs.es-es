---
title: Conexión de datos de Barracuda a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Barracuda a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: b3ca93d9e70456d25d5f78b2ca1fde8e4ea24f8d
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240198"
---
# <a name="connect-your-barracuda-appliance"></a>Conexión del dispositivo Barracuda 



El conector del firewall de aplicaciones web (WAF) de Barracuda permite conectar fácilmente los registros de Barracuda con Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación, de modo que dispondrá de más información sobre la red de la organización y de mejores capacidades de seguridad. Azure Sentinel aprovecha las ventajas de la integración nativa entre **Barracuda** y el agente de Log Analytics para proporcionar una integración sin problemas. 


> [!NOTE]
> Los datos se almacenarán en la ubicación geográfica del área de trabajo en la que Azure Sentinel se ejecute.

## <a name="configure-and-connect-barracuda-waf"></a>Configuración y conexión del WAF de Barracuda
El firewall de aplicaciones web de Barracuda puede integrar y exportar registros directamente a Azure Sentinel a través del agente de Log Analytics.
1. Vaya al [flujo de configuración del WAF de Barracuda](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/) y siga las instrucciones para configurar la conexión con estos parámetros:
    - **Id. del área de trabajo**: copie el valor del identificador del área de trabajo que se indica en la página del conector de Barracuda de Azure Sentinel.
    - **Clave principal**: copie el valor de la clave principal que se indica en la página del conector de Barracuda de Azure Sentinel.
2. En el portal de Azure Sentinel, vaya al área de trabajo en la que ha implementado Azure Sentinel, haga clic en el botón de puntos suspensivos (…) al final de la fila y seleccione **Configuración avanzada**. 
1. Seleccione **Datos** y, después, **Syslog**.
1. Asegúrese de que la instalación que establece en Barracuda existe, defina la gravedad y haga clic en **Guardar**.
6. Para usar el esquema pertinente en Log Analytics para los eventos de Barracuda, busque **CommonSecurityLog** y **barracuda_CL**.


## <a name="validate-connectivity"></a>Validar conectividad

Hasta que los registros empiecen a aparecer en Log Analytics, pueden transcurrir más de 20 minutos. 



## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos de Barracuda a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

