---
title: Conecte los datos de ATP de Azure a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de ATP de Azure a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 5bf3cc44-ecda-4c78-8a63-31ab42f43605
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: f0d86a62c59df5bebd34137d0903fcaa7014573d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204270"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Conectarse a datos desde Azure Advanced Threat Protection (ATP)

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Puede transmitir los registros de [protección contra amenazas avanzada de Azure](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) en Azure Sentinel con un solo clic.

## <a name="prerequisites"></a>Requisitos previos

- Usuario con permisos de administrador de seguridad o de administrador global
- Debe ser un cliente de versión preliminar privada de ATP de Azure

## <a name="connect-to-azure-atp"></a>Conectarse a ATP de Azure

Asegúrese de que la versión de vista previa privada de ATP de Azure es [habilitado en la red](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1).
Si está implementado ATP de Azure y consumir los datos, las alertas sospechosas fácilmente se pueden transmitir a Azure Sentinel. Puede tardar hasta 24 horas para las alertas que desea iniciar la transmisión en Centinela de Azure.



1. En Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **ATP de Azure** icono.

2. Haga clic en **Conectar**.

6. Para usar el esquema correspondiente en Log Analytics para las alertas de ATP de Azure, busque **SecurityAlert**.

## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar la protección contra amenazas avanzada de Azure a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).

