---
title: Conectar los datos de Azure Information Protection a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectarse a datos de Azure Information Protection en Azure Sentinel.
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
ms.openlocfilehash: 4b73edd10521b23fb4befbe4fe7d9f0c7b496de3
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204299"
---
# <a name="connect-data-from-azure-information-protection"></a>Conectar los datos de Azure Information Protection

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Puede transmitir los registros de [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) en Azure Sentinel con un solo clic. Azure Information Protection ayuda a proteger los datos si se almacena en la nube o en las infraestructuras locales y control y ayudarle a proteger el correo electrónico, documentos y datos confidenciales que comparte fuera de su empresa. Desde una clasificación sencilla con grupos y permisos de etiquetas incrustadas, mejorar la protección de datos en todo momento con Azure Information Protection. Cuando se conectan Azure Information Protection a Centinela de Azure, stream todas las alertas de Azure Information Protection en Azure Sentinel.


## <a name="prerequisites"></a>Requisitos previos

- Usuario con el administrador global, Administrador de seguridad o permisos de protección de información


## <a name="connect-to-azure-information-protection"></a>Conectarse a Azure Information Protection

Si ya tiene Azure Information Protection, asegúrese de que es [habilitado en la red](https://docs.microsoft.com/azure/information-protection/activate-service).
Si se implementa Azure Information Protection y obtención de datos, los datos de alerta fácilmente se pueden transmitir a Azure Sentinel.


1. En Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **Azure Information Protection** icono.

2. Vaya a la [portal de Azure Information Protection](https://portal.azure.com/?ScannerConfiguration=true&EndpointDiscovery=true#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/quickstartBlade) 

3. En **conexión**, configurar streaming de registros de Azure Information Protection para Azure Sentinel haciendo [configurar análisis](https://portal.azure.com/#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/analyticsOnboardBlade)

4. Seleccione el área de trabajo en la que ha implementado Azure Sentinel. 

5. Haga clic en **OK**.

6. Para usar el esquema correspondiente en Log Analytics para las alertas de Azure Information Protection, busque **InformationProtectionLogs_CL**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar Azure Information Protection para Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
