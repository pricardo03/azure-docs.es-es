---
title: Conecte los datos de Cloud App Security para la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de Cloud App Security con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: cd9e5e27-fdd4-4717-8924-be4c1c430f23
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 5a7dfeed7b52453b38720c21c7d213679b8d2854
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59789456"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Conectar los datos de Microsoft Cloud App Security 

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de [Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) en Azure Sentinel con un solo clic. Esta conexión le permite hacer streaming de las alertas de Cloud App Security en Azure Sentinel. 

## <a name="prerequisites"></a>Requisitos previos

- Usuario con permisos de administrador de seguridad o de administrador global

## <a name="connect-to-cloud-app-security"></a>Conectar cloud App Security

Si ya dispone de Cloud App Security, asegúrese de que es [habilitado en la red](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security).
Si se implementa Cloud App Security y consumir los datos, los datos de alerta fácilmente se pueden transmitir a Azure Sentinel.


1. En Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **Cloud App Security** icono.

2. Haga clic en **Conectar**.

3. Para usar el esquema correspondiente en Log Analytics para las alertas de Cloud App Security, busque **SecurityAlert**.


## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Microsoft Cloud App Security con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
