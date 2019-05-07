---
title: Conectarse a datos de firewall de aplicación web de Microsoft a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectarse a datos de firewall de aplicación web de Microsoft Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: bfa2eca4-abdc-49ce-b11a-0ee229770cdd
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 99c553145f0a600826e48bec437449f278b74fbf
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65205644"
---
# <a name="connect-data-from-microsoft-web-application-firewall"></a>Conectar los datos de firewall de aplicaciones web de Microsoft

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de firewall de aplicaciones de Azure Application Gateway Microsoft web (WAF). Este WAF protege las aplicaciones de vulnerabilidades web comunes, como inyección de código SQL y el scripting entre sitios y le permite personalizar reglas para reducir los falsos positivos. Siga estas instrucciones para transmitir los registros de firewall de aplicaciones Web de Microsoft a Azure Sentinel.


## <a name="prerequisites"></a>Requisitos previos

- Un recurso de puerta de enlace de aplicaciones existente

## <a name="connect-to-microsoft-web-application-firewall"></a>Conectarse al servidor de aplicaciones web de Microsoft

Si ya dispone de firewall de aplicaciones web de Microsoft, asegúrese de que tiene un recurso de puerta de enlace existente.
Una vez que se implementa el firewall de aplicaciones web de Microsoft y obtención de datos, los datos de alerta fácilmente se pueden transmitir a Azure Sentinel.
    
1. En el portal de Azure Sentinel, seleccione **conectores de datos**.
1. En la página de conectores de datos, seleccione el **WAF** icono.
1. Vaya a [recurso de Application Gateway](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAllResourcesBlade/resourceType/Microsoft.Network%2FapplicationGateways) y elija el WAF.
    1. Seleccione **configuración de diagnóstico**.
    1. Seleccione **+ Agregar configuración de diagnóstico** bajo la tabla.
    1. En el **configuración de diagnóstico** página, escriba un **nombre** y seleccione **enviar a Log Analytics**.
    1. En **el área de trabajo de Log Analytics** seleccione el área de trabajo Centinela de Azure.
    1. Seleccione los tipos de registro que desea analizar. Se recomienda: ApplicationGatewayAccessLog y ApplicationGatewayFirewallLog.
1. Para usar el esquema correspondiente en Log Analytics para las alertas de firewall de aplicaciones de web de Microsoft, busque **AzureDiagnostics**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar el firewall de aplicaciones web de Microsoft para Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
