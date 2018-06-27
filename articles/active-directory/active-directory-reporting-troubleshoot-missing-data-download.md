---
title: 'Solución de problemas: faltan datos en los registros de actividad de Azure Active Directory descargados | Microsoft Docs'
description: Proporciona una solución para los datos que faltan en los registros de actividad de Azure Active Directory descargados.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 01/15/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d0638404ec6f5b6d13aa207ef54913c1bd3ecc1a
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36232232"
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>No encuentro datos en los registros de actividad de Azure Active Directory que descargué


## <a name="symptoms"></a>Síntomas

Descargue los registros de actividad (auditoría o inicios de sesión) y no veo todos los registros del tiempo que elegí. ¿Por qué? 

 ![Informes](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Causa

Al descargar los registros de actividad en Azure Portal, limitamos la escala a 5000 registros, con el más reciente primero. 

## <a name="resolution"></a>Resolución

Puede sacar provecho de la [API de creación de informes de Azure AD](active-directory-reporting-api-getting-started.md) para capturar hasta un millones de registros en cualquier momento dado. Nuestro enfoque recomendado es ejecutar de forma programada un script que llame a las API de creación de informes para capturar registros de forma incremental durante un período de tiempo (por ejemplo, a diario o semanalmente).

## <a name="next-steps"></a>Pasos siguientes
Consulte [Preguntas más frecuentes sobre informes de Azure Active Directory](active-directory-reporting-faq.md).

