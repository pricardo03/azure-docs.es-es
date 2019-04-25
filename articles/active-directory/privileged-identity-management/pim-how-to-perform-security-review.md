---
title: Realizar una revisión de acceso de Mis roles de Azure AD en PIM - Azure Active Directory | Microsoft Docs
description: Obtenga información sobre cómo realizar una revisión de acceso de los roles de Azure AD en Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/21/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66f16e02716ceb94d2c8b10bb246a13dc566229c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60287819"
---
# <a name="perform-an-access-review-of-my-azure-ad-roles-in-pim"></a>Realizar una revisión de acceso de Mis roles de Azure AD en PIM
Azure Active Directory (AD) Privileged Identity Management (PIM) simplifica cómo las empresas administran el acceso con privilegios a los recursos de Azure AD y otros servicios en línea de Microsoft como Office 365 o Microsoft Intune.  

Si se le ha asignado un rol administrativo, el administrador de roles con privilegios de su organización le puede pedir que revise y confirme periódicamente que aún necesita ese rol para su trabajo. Puede que reciba un correo electrónico con un vínculo o que vaya directamente al [Portal de Azure](https://portal.azure.com). Siga los pasos de este artículo para realizar una autorrevisión de los roles asignados.

Si es un administrador de roles con privilegios o un administrador global y está interesado en las revisiones de acceso, puede obtener más información en [How to start an access review](pim-how-to-start-security-review.md) (Cómo iniciar una revisión de acceso).

## <a name="add-the-privileged-identity-management-application"></a>Incorporación de la aplicación Privileged Identity Management
Puede usar la aplicación Privileged Identity Management (PIM) de Azure AD en el [Portal de Azure](https://portal.azure.com/) para realizar la revisión.  Si no tiene la aplicación en el portal, siga estos pasos para comenzar:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. Seleccione su nombre de usuario en la esquina superior derecha del Portal de Azure y elija el directorio donde va a trabajar.
3. Seleccione **Todos los servicios** y use el cuadro de texto Filtro para buscar **Azure AD Privileged Identity Management**.
4. Active **Anclar al panel** y haga clic en **Crear**. Se abrirá la aplicación Privileged Identity Management.

## <a name="approve-or-deny-access"></a>Aprobación o denegación de acceso
Al aprobar o denegar el acceso, lo que hace es indicar al revisor si aún usa o no este rol. Elija **Aprobar** si desea permanecer en el rol o **Denegar** si no va a volver a necesitar el acceso. Su estado no cambiará inmediatamente, hay que esperar a que el revisor aplique los resultados.
Para buscar y completar la revisión de acceso, siga estos pasos:

1. En la aplicación PIM, seleccione **Revisar acceso con privilegios**. Si tienes revisiones de acceso pendientes, aparecerán en la hoja de revisiones de acceso de Azure AD.
2. Seleccione la revisión que quiere completar.
3. A no ser que haya creado la revisión, aparecerá como el único usuario en ella. Seleccione la marca de verificación junto a su nombre.
4. Elija **Aprobar** o **Denegar**. Puede que necesite incluir un motivo de su decisión en el cuadro de texto **Proporcionar una razón** .  
5. Cierre la hoja **Revisar roles de Azure AD** .

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Pasos siguientes

- [Realización de una revisión de acceso para los roles de recurso de Azure en PIM](pim-resource-roles-perform-access-review.md)
