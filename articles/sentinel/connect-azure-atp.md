---
title: Conectar datos de Azure ATP a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Azure ATP a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/28/2019
ms.author: rkarlin
ms.openlocfilehash: 6f41a5704c783ba8aeab7bc2e82ef731c6a257ac
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599156"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Conectar datos de Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Se pueden transmitir registros desde [Azure Advanced Threat Protection](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) a Azure Sentinel con tan solo un clic.

## <a name="prerequisites"></a>Requisitos previos

- Un usuario que sea administrador global o que tenga permisos de administrador de seguridad
- Debe ser un cliente de la versión preliminar de Azure ATP.

## <a name="connect-to-azure-atp"></a>Conectar a Azure ATP

Asegúrese de que la versión preliminar de Azure ATP está [habilitada en la red](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Si Azure ATP ya está implementado e ingiriendo datos, las alertas sospechosas se pueden transmitir a Azure Sentinel muy fácilmente. Las alertas pueden tardar hasta 24 horas en empezar a transmitirse a Azure Sentinel.


1. Para conectar Azure ATP a Azure Sentinel, primero debe habilitar la integración entre Azure ATP y Microsoft Cloud App Security. Para obtener información sobre cómo hacerlo, consulte [Integración de Azure Advanced Threat Protection](https://docs.microsoft.com/cloud-app-security/aatp-integration).

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure ATP**.

2. Haga clic en **Conectar**.

6. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure ATP, busque **SecurityAlert**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure Advanced Threat Protection a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).

