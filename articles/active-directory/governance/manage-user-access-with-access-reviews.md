---
title: Administración del acceso de los usuarios con las revisiones de acceso de Azure AD | Microsoft Docs
description: Aprenda a administrar el acceso de los usuarios, como la pertenencia a un grupo o la asignación a una aplicación con revisiones de acceso de Azure Active Directory
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
ms.component: compliance
ms.date: 06/21/2018
ms.author: rolyon
ms.reviewer: mwahl
ms.openlocfilehash: 5af1d91a51b090821b5bbb314834db0d5cbe6a26
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45606865"
---
# <a name="manage-user-access-with-azure-ad-access-reviews"></a>Administración del acceso de los usuarios con las revisiones de acceso de Azure AD

Con Azure Active Directory (Azure AD), puede asegurarse de que los usuarios tienen el acceso adecuado. Puede pedir a los propios usuarios o a quien decida en su lugar que participen en una revisión de acceso y vuelvan a certificar (o atestiguar) el acceso de los usuarios. Los revisores pueden dar su aprobación para cada necesidad de acceso continuado de los usuarios, en función de las sugerencias de Azure AD. Cuando una revisión de acceso haya terminado, es posible hacer cambios y retirar la concesión de acceso a los usuarios que ya no lo necesitan.

> [!NOTE]
> Si desea revisar solo el acceso de los usuarios invitados y no el de todos los tipos de usuarios, consulte [Administración del acceso de los invitados con las revisiones de acceso de Azure AD](manage-guest-access-with-access-reviews.md). Si desea revisar la pertenencia de los usuarios a roles administrativos tales como administrador global, consulte [Inicio de una revisión de acceso en Azure AD Privileged Identity Management](../privileged-identity-management/pim-how-to-start-security-review.md). 
>
>

## <a name="prerequisites"></a>Requisitos previos 


Las revisiones de acceso están disponibles con la edición Premium P2 de Azure AD, que se incluye en Microsoft Enterprise Mobility + Security, E5. Para obtener más información, consulte [Ediciones de Azure Active Directory](../fundamentals/active-directory-whatis.md). Cada usuario que interactúa con esta característica, ya sea para crear una revisión, rellenar una revisión o confirmar su acceso, etc., requiere una licencia. 

## <a name="create-and-perform-an-access-review"></a>Creación y realización de una revisión de acceso

Puede tener uno o más usuarios como revisores en una revisión de acceso.  

1. Seleccione un grupo de Azure AD con uno o más miembros. O bien, seleccione una aplicación conectada a Azure AD con uno o más usuarios asignados a ella. 

2. Decida si cada usuario revisará su propio acceso o bien si uno o más usuarios revisarán el acceso de todos.

3. Habilite las revisiones de acceso para que aparezcan en los paneles de acceso de los revisores. Como administrador global o administrador de cuentas de usuario, vaya a la [página de revisiones de acceso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

4. Inicie la revisión de acceso. Para más información, consulte el artículo sobre la [creación de una revisión de acceso](create-access-review.md).

5. Pida a los revisores que proporcionen sus datos de entrada. De forma predeterminada, cada uno recibe un correo electrónico de Azure AD con un vínculo al panel de acceso, en el que podrán [realizar la revisión de acceso](perform-access-review.md).

6. Si los revisores no han proporcionado información, puede pedir a Azure AD que les envíe un recordatorio. De forma predeterminada, Azure AD envía automáticamente un recordatorio hacia la mitad del plazo de finalización a los revisores que no hayan respondido.

7. Cuando los revisores hayan proporcionado la información, detenga la revisión de acceso y aplique los cambios. Para más información, consulte el artículo sobre la [realización de una revisión de acceso](complete-access-review.md).


## <a name="next-steps"></a>Pasos siguientes

[Crear una revisión de acceso para los miembros de un grupo o el acceso a una aplicación](create-access-review.md)




