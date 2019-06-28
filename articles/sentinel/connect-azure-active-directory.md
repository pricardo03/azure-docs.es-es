---
title: Conectar datos de Azure AD a Azure Sentinel, versión preliminar | Microsoft Docs
description: Aprenda a conectar datos de Azure Active Directory a Azure Sentinel.
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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65204499"
---
# <a name="connect-data-from-azure-active-directory"></a>Conectar datos desde Azure Active Directory

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel permite recopilar datos de [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) y transmitirlos a Azure Sentinel. Puede elegir si transmitir [registros de inicio de sesión](../active-directory/reports-monitoring/concept-sign-ins.md) y [registros de auditoría](../active-directory/reports-monitoring/concept-audit-logs.md).

## <a name="prerequisites"></a>Requisitos previos

- Si quiere exportar datos de inicio de sesión desde Active Directory, debe tener una licencia de Azure AD P1 o P2.

- Usuario con permisos de administrador global o administrador de seguridad en el inquilino desde el que se van a transmitir los registros.


## <a name="connect-to-azure-ad"></a>Conectarse a Azure

1. En Azure Sentinel, seleccione **Data connectors** (Conectores de datos) y, después, haga clic en el icono de **Azure Active Directory**.

2. Haga clic en **Connect** (Conectar) junto a los registros que quiere transmitir a Azure Sentinel.

6. Para usar el esquema correspondiente en Log Analytics para encontrar alertas de Azure AD, busque **SigninLogs** y **AuditLogs**.




## <a name="next-steps"></a>Pasos siguientes
En este documento, ha aprendido a conectar Azure AD a Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:
- Aprenda a [obtener visibilidad de los datos y de posibles amenazas](quickstart-get-visibility.md).
- Empiece a [detectar amenazas con Azure Sentinel](tutorial-detect-threats.md).
