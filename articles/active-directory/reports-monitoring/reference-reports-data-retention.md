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
ms.date: 05/10/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 68028fd1ba116251860e5c370e9e9ce61fd314bb
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42146535"
---
# <a name="azure-active-directory-report-retention-policies"></a>Directivas de retención de informes de Azure Active Directory


En este artículo se proporcionan respuestas a las preguntas más frecuentes en relación a la retención de datos de los distintos informes de actividad de Azure Active Directory. 

### <a name="q-how-can-you-get-the-collection-of-activity-data-started"></a>P: ¿Cómo puede empezar la recopilación de datos de actividad?

**R:**

| Edición de Azure AD | Inicio de la recopilación |
| :--              | :--   |
| Azure AD Premium P1 <br /> Azure AD Premium P2 | Cuándo suscribirse |
| Azure AD Free | La primera vez que abra la [hoja de Azure Active Directory](https://ms.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) o use las [API de informes](https://aka.ms/aadreports)  |

---
### <a name="q-when-is-your-activity-data-available-in-the-azure-portal"></a>P: ¿Cuándo estarán disponibles los datos de actividad en Azure Portal?

**R:**

- **Inmediatamente**: si ya ha estado trabajando con informes en Azure Portal.
- **Dentro de 2 horas**: si no ha activado la generación de informes en Azure Portal.

---

### <a name="q-how-can-you-get-the-collection-of-security-signals-started"></a>P: ¿Cómo puede empezar la recopilación de señales de seguridad?  

**R:** En el caso de las señales de seguridad, el proceso de recopilación se inicia cuando decide usar Identity Protection Center. 


---

### <a name="q-for-how-long-is-the-collected-data-stored"></a>P: ¿Durante cuánto tiempo se almacenan los datos recopilados?

**R:**

**Informes de actividad**    

| Informe                 | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--                    | :--           | :--                 | :--                 |
| Auditoría de directorio        | 7 días        | 30 días             | 30 días             |
| Actividad de inicio de sesión       | N/D           | 30 días             | 30 días             |
| Uso de Azure MFA        | 30 días       | 30 días             | 30 días             |

**Señales de seguridad**

| Informe         | Azure AD Free | Azure AD Premium P1 | Azure AD Premium P2 |
| :--            | :--           | :--                 | :--                 |
| Usuarios en riesgo  | 7 días        | 30 días             | 90 días             |
| Inicios de sesión no seguros | 7 días        | 30 días             | 90 días             |

---
