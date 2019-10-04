---
title: Conexión de datos de Cloud App Security a Azure Sentinel | Microsoft Docs
description: Aprenda a conectar datos de Cloud App Security a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 6e71028855344d083dedf3493682e1e27685de48
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240117"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Conectar datos de Microsoft Cloud App Security 



Se pueden transmitir registros desde [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) a Azure Sentinel con tan solo un clic. Esta conexión permite transmitir las alertas de Cloud App Security a Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad

## <a name="connect-to-cloud-app-security"></a>Conectar a Cloud App Security

Si ya tiene Cloud App Security, asegúrese de que está [habilitado en la red](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Si Cloud App Security ya está implementado e ingiriendo datos, los datos de alerta se pueden transmitir a Azure Sentinel muy fácilmente.


1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Cloud App Security**.

1. Seleccione qué registros desea transmitir a Azure Sentinel. Aquí puede elegir **Alertas**. 

1. Puede seleccionar si desea que las alertas de Microsoft Cloud App Security generen incidentes automáticamente en Azure Sentinel. En **Creación de incidentes** seleccione **Habilitar** para habilitar la regla analítica predeterminada que crea incidentes automáticamente a partir de alertas generadas en el servicio de seguridad conectado. Luego, puede editar esta regla en **Analytics** (Análisis) y **Active rules** (Reglas activas).

1. Haga clic en **Conectar**.

1. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Cloud App Security, busque **SecurityAlert**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Microsoft Cloud App Security a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats-built-in.md).
