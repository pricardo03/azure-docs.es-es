---
title: 'Solución de problemas: faltan datos en los registros de actividad de Azure Active Directory descargados | Microsoft Docs'
description: Proporciona una solución para los datos que faltan en los registros de actividad de Azure Active Directory descargados.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 01/15/2018
ms.author: rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 6878ff8175514b97fbeab70b932349ce400394dd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589872"
---
# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>No encuentro datos en los registros de actividad de Azure Active Directory que he descargado


## <a name="symptoms"></a>Síntomas

Descargue los registros de actividad (auditoría o inicios de sesión) y no veo todos los registros del tiempo que elegí. ¿Por qué? 

 ![Informes](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Causa

Al descargar los registros de actividad en Azure Portal, limitamos la escala a 120 000 registros, ordenados por antigüedad. 

## <a name="resolution"></a>Resolución

Puede sacar provecho de la [API de creación de informes de Azure AD](active-directory-reporting-api-getting-started.md) para capturar hasta un millones de registros en cualquier momento dado. Nuestro enfoque recomendado es ejecutar de forma programada un script que llame a las API de creación de informes para capturar registros de forma incremental durante un período de tiempo (p. ej., a diario o semanalmente).

## <a name="next-steps"></a>Pasos siguientes
Consulte [Preguntas más frecuentes sobre informes de Azure Active Directory](active-directory-reporting-faq.md).

