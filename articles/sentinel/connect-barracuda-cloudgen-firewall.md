---
title: Conectar el Firewall de Barracuda CloudGen a Azure Sentinel | Microsoft Docs
description: Más información sobre cómo conectar el Firewall de Barracuda CloudGen a Azure Sentinel.
services: sentinel
documentationcenter: na
author: cabailey
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: cabailey
ms.openlocfilehash: 3d467026022a7bc90babd94827d64828f59ea714
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73511913"
---
# <a name="connect-barracuda-cloudgen-firewall"></a>Conexión del Firewall de Barracuda CloudGen

El conector del Firewall de Barracuda CloudGen (CGFW) le permite conectar fácilmente los registros de Barracuda CGFW con Azure Sentinel para ver los paneles, crear alertas personalizadas y mejorar la investigación. Esto le ofrece más información sobre la red de su organización y mejora las capacidades de las operaciones de seguridad.




## <a name="prerequisites"></a>Requisitos previos

- Permisos de lectura y escritura en el área de trabajo de Azure Sentinel.

- El Firewall de Barracuda CloudGen debe estar configurado para exportar registros a través de Syslog.

## <a name="connect-azure-sentinel-to-barracuda-cloudgen-firewall"></a>Conexión de Azure Sentinel con el Firewall de Barracuda CloudGen

1. En el Azure Portal, vaya a **Azure Sentinel** > **Conectores de datos** y, después, seleccione el conector de **Firewall de Barracuda C4**.

2. Seleccione **Open connector page** (Abrir página del conector).

3. Siga las instrucciones que aparecen en la página del **Firewall de Barracuda CloudGen**.


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar dispositivos del Firewall de Barracuda a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).

