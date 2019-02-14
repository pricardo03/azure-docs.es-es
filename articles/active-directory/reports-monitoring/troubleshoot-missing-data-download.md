---
title: 'Solución de problemas: Faltan datos en los registros de actividad de Azure Active Directory descargados | Microsoft Docs'
description: Proporciona una solución para los datos que faltan en los registros de actividad de Azure Active Directory descargados.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1ba5f803f59263f9bfebfd4ec8635d5cdd6d90a0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56171781"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>No encuentro todos los datos en los registros de actividad de Azure Active Directory que he descargado

## <a name="symptoms"></a>Síntomas

Descargue los registros de actividad (auditoría o inicios de sesión) y no veo todos los registros del tiempo que elegí. ¿Por qué? 

 ![Informes](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Causa

Al descargar los registros de actividad en Azure Portal, limitamos la escala a 5000 registros, con el más reciente primero. 

## <a name="resolution"></a>Resolución

Puede sacar provecho de la [API de creación de informes de Azure AD](concept-reporting-api.md) para capturar hasta un millones de registros en cualquier momento dado. Nuestro enfoque recomendado es [ejecutar de forma programada un script](tutorial-signin-logs-download-script.md) que llame a las API de creación de informes para capturar registros de forma incremental durante un período de tiempo (por ejemplo, a diario o semanalmente). 

## <a name="next-steps"></a>Pasos siguientes

* [P+F sobre los informes de Azure Active Directory](reports-faq.md)

