---
title: Conectar datos de Azure Information Protection a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Azure Information Protection a Azure Sentinel.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65204299"
---
# <a name="connect-data-from-azure-information-protection"></a>Conectar datos de Azure Information Protection

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Se pueden transmitir registros desde [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/reports-aip) a Azure Sentinel con tan solo un clic. Azure Information Protection ayuda a proteger los datos (estén almacenados en la nube o en infraestructuras locales) y a controlar e intentar proteger los correos electrónicos, documentos y datos confidenciales que comparte fuera de la empresa. Con Azure Information Protection, puede usar desde una clasificación sencilla hasta etiquetas y permisos insertados para mejorar la protección de los datos en todo momento. Cuando Azure Information Protection se conecta a Azure Sentinel, todas las alertas de Azure Information Protection se transmiten a Azure Sentinel.


## <a name="prerequisites"></a>Requisitos previos

- Usuario que sea administrador global, administrador de seguridad o que tenga permisos de protección de información


## <a name="connect-to-azure-information-protection"></a>Conectar a Azure Information Protection

Si ya tiene Azure Information Protection, asegúrese de que está [habilitado en la red](https://docs.microsoft.com/azure/information-protection/activate-service).
Si Azure Information Protection ya está implementado y obteniendo datos, los datos de alertas se pueden transmitir a Azure Sentinel muy fácilmente.


1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Information Protection**.

2. Vaya al [portal de Azure Information Protection](https://portal.azure.com/?ScannerConfiguration=true&EndpointDiscovery=true#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/quickstartBlade). 

3. En **Conexión**, configure la transmisión de registros de Azure Information Protection a Azure Sentinel haciendo clic en [Configurar análisis](https://portal.azure.com/#blade/Microsoft_Azure_InformationProtection/DataClassGroupEditBlade/analyticsOnboardBlade).

4. Seleccione el área de trabajo en la que ha implementado Azure Sentinel. 

5. Haga clic en **OK**.

6. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure Information Protection, busque **InformationProtectionLogs_CL**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Information Protection a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
