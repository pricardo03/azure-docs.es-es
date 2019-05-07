---
title: Conecte los datos de Azure AD a la versión preliminar de Azure Sentinel | Microsoft Docs
description: Obtenga información sobre cómo conectar los datos de Azure Active Directory a Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2019
ms.author: rkarlin
ms.openlocfilehash: 357435b8a4ac396c1548c89206f269730e871f6b
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204499"
---
# <a name="connect-data-from-azure-active-directory"></a>Conectar los datos de Azure Active Directory

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Sentinel Azure le permite recopilar datos de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) y hacer streaming en Azure Sentinel. Puede elegir en secuencia [los registros de inicio de sesión](../active-directory/reports-monitoring/concept-sign-ins.md) y [registros de auditoría](../active-directory/reports-monitoring/concept-audit-logs.md) .

## <a name="prerequisites"></a>Requisitos previos

- Si desea exportar los datos de inicio de sesión de Active Directory, debe tener una licencia de Azure AD P1 o P2.

- Usuario con permisos de administrador globales de seguridad o administrador en el inquilino que se va a transmitir los registros de.


## <a name="connect-to-azure-ad"></a>Conectarse a Azure

1. En Azure Sentinel, seleccione **conectores de datos** y, a continuación, haga clic en el **Azure Active Directory** icono.

2. Junto a los registros que desea transmitir en Centinela de Azure, haga clic en **Connect**.

6. Para usar el esquema correspondiente en Log Analytics para las alertas de Azure AD, busque **SigninLogs** y **AuditLogs**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido cómo conectar Azure AD a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Obtenga información sobre cómo [Obtenga visibilidad sobre sus datos y las posibles amenazas](quickstart-get-visibility.md).
- Introducción a [detección de amenazas con Azure Sentinel](tutorial-detect-threats.md).
