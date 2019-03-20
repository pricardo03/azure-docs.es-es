---
title: Administración de programas y controles para las revisiones de acceso de Azure AD | Microsoft Docs
description: Aprenda a crear programas adicionales para cada control, administración de riesgos e iniciativa de cumplimiento de su organización para recopilar y organizar las revisiones de acceso de Active Directory de Azure como controles.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3056976ada6c0a0287eec608dbad230e8dbb2625
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57895189"
---
# <a name="manage-programs-and-controls-for-azure-ad-access-reviews"></a>Administración de programas y controles para las revisiones de acceso de Azure AD

Azure Active Directory (Azure AD) incluye revisiones de acceso de los miembros del grupo y acceso a la aplicación. Estos ejemplos de controles garantizan la vigilancia de quién dispone de acceso a sus aplicaciones y pertenencias a grupos de la organización. Las organizaciones pueden usar estos controles para abordar eficazmente sus requisitos de cumplimiento de normas, administración de riesgos y regulación.

## <a name="create-and-manage-programs-and-their-controls"></a>Creación y administración de programas y de sus controles
Puede simplificar el seguimiento y la recopilación de revisiones de acceso para distintos fines organizándolos en programas. Cada revisión de acceso se puede vincular a un programa. Por lo tanto, cuando prepare los informes para un auditor, puede centrarse en las revisiones de acceso en el ámbito de una iniciativa determinada.  Programas y los resultados de la revisión de acceso son visibles para los usuarios en el administrador Global, Administrador de usuarios, Administrador de seguridad o rol de lector de seguridad.

Para ver una lista de programas, vaya a la [página de revisiones de acceso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/) y seleccione **Programas**.

El **programa predeterminado** siempre está presente. Si está en un administrador global o un rol de usuario administrador, puede crear programas adicionales. Por ejemplo, puede elegir tener un programa cada para iniciativa de cumplimiento de normas u objetivo empresarial.

Si ya no necesita un programa y no tiene ningún control vinculado a él, puede eliminarlo.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una revisión de acceso de grupos o aplicaciones](create-access-review.md)
- [Recuperar los resultados de la revisión de acceso para grupos o aplicaciones](retrieve-access-review.md)
