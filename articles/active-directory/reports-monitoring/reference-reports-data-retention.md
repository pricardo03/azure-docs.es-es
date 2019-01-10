---
title: Directivas de retención de informes de Azure Active Directory | Microsoft Docs
description: Directivas de retención de datos de informes en Azure Active Directory
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 183e53b0-0647-42e7-8abe-3e9ff424de12
ms.service: active-directory
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 6188b141ec1a514d999f290366cd9ffbafc1d96c
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753139"
---
# <a name="azure-active-directory-report-retention-policies"></a>Directivas de retención de informes de Azure Active Directory

En este artículo, aprenderá sobre las directivas de retención de datos para los diferentes informes de actividad en Azure Active Directory. 

### <a name="when-does-azure-ad-start-collecting-data"></a>¿Cuándo Azure AD comienza la recopilación de datos?

| Edición de Azure AD | Inicio de la recopilación |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Cuándo suscribirse |
| Azure AD Free <br /> Azure AD Basic | La primera vez que abra la [hoja de Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) o use las [API de informes](https://aka.ms/aadreports)  |

---

### <a name="when-is-the-activity-data-available-in-the-azure-portal"></a>¿Cuándo estarán disponibles los datos de actividad en Azure Portal?

- **Inmediatamente**: si ya ha estado trabajando con informes en Azure Portal.
- **Dentro de 2 horas**: si no ha activado la generación de informes en Azure Portal.

---

### <a name="when-does-azure-ad-start-collecting-security-signal-data"></a>¿Cuándo Azure AD comienza a recopilar datos de señales de seguridad?  

En el caso de las señales de seguridad, el proceso de recopilación se inicia cuando decide usar **Identity Protection Center**. 

---

### <a name="how-long-does-azure-ad-store-the-data"></a>¿Durante cuánto tiempo Azure AD almacena los datos?

**Informes de actividad**    

| Informe                 | Azure AD Free | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--            | :--                 | :--                 |
| Auditoría de directorio        | 7 días        |  7 días        | 30 días             | 30 días             |
| Actividad de inicio de sesión       | N/D           |  N/D           | 30 días             | 30 días             |
| Uso de Azure MFA        | 30 días       |  30 días       | 30 días             | 30 días             |

Puede conservar los datos de actividad de auditoría e inicio de sesión durante un tiempo mayor que el período de retención predeterminado que se ha descrito anteriormente si los enruta a una cuenta de almacenamiento de Azure con Azure Monitor. Para obtener más información, consulte [Archivado de registros de Azure AD en una cuenta de Azure Storage](quickstart-azure-monitor-route-logs-to-storage-account.md).

**Señales de seguridad**

| Informe         | Azure AD Free | Azure AD Basic | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--            | :--                 | :--                 |
| Usuarios en riesgo  | 7 días        | 7 días         | 30 días             | 90 días             |
| Inicios de sesión no seguros | 7 días        | 7 días         |  30 días            | 90 días             |

---
