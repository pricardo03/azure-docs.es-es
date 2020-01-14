---
title: Conexión de datos de F5 con Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos F5 a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: rkarlin
ms.openlocfilehash: cf6dc6977ff066b646beac4db5562ae8d97ab066
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610563"
---
# <a name="connect-f5-to-azure-sentinel"></a>Conexión de F5 a Azure Sentinel

En este artículo se explica cómo conectar el dispositivo F5 a Azure Sentinel. El conector de datos de F5 permite conectar fácilmente los registros de F5 con Azure Sentinel para ver paneles, crear alertas personalizadas y mejorar la investigación. El uso de F5 en Azure Sentinel le proporcionará más información sobre el uso de Internet de su organización y mejorará sus capacidades de operación de seguridad. 

## <a name="configure-your-f5-to-send-cef-messages"></a>Configurar F5 para enviar mensajes de CEF

1. Vaya a [Configuración del registro de eventos de seguridad de la aplicación](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-11-5-0/12.html) de F5 y siga las instrucciones para configurar el registro remoto mediante estas directrices:
   - Establezca **Remote storage type** (Tipo de almacenamiento remoto) en **CEF**.
   - Configure el **protocolo** como **TCP**.
   - Establezca la **IP address** (Dirección IP) en la dirección IP del servidor de Syslog.
   - Establezca el valor de **port number** (número de puerto) en **514** o en el puerto que estableció para que lo usara el agente.
   - Puede establecer el valor de **Maximum Query String Size** (Tamaño máximo de la cadena de consulta) en el tamaño que estableció en el agente.

1. Para usar el esquema correspondiente en Log Analytics para los eventos CEF, busque `CommonSecurityLog`.

1. Diríjase al [PASO 3: Validación de conectividad](connect-cef-verify.md).


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar F5 a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
- [Use libros](tutorial-monitor-your-data.md) para supervisar sus datos.

