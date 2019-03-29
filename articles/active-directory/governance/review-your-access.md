---
title: Revisar el acceso por sí mismo a grupos o aplicaciones en las revisiones de acceso - Azure Active Directory | Microsoft Docs
description: Aprenda a revisar su propio acceso a grupos o aplicaciones en las revisiones de acceso de Azure Active Directory.
services: active-directory
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 02/20/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3fe2013ff84dd0451fed7d108539606520cb9403
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2019
ms.locfileid: "58576316"
---
# <a name="review-access-for-yourself-to-groups-or-applications-in-azure-ad-access-reviews"></a>Revisar el acceso por sí mismo a grupos o las revisiones de acceso de las aplicaciones de Azure AD

Azure Active Directory (Azure AD) simplifica el modo en que las empresas administran el acceso a grupos o aplicaciones de Azure AD y revisiones de otros servicios en línea de Microsoft con una característica denominada acceso de Azure AD.

En este artículo se describe cómo revisar su propio acceso a un grupo o una aplicación.

## <a name="open-the-access-review"></a>Abra la revisión de acceso

Es el primer paso para realizar una revisión de acceso buscar y abrir la revisión de acceso.

1. Busque un correo electrónico de Microsoft que le pide que revise el acceso. Este es un correo electrónico de ejemplo para revisar su acceso a un grupo.

    ![Correo electrónico de revisión de acceso](./media/review-your-access/access-review-email.png)

1. Haga clic en el **revisar acceso** vínculo para abrir la revisión de acceso.

Si no tiene el correo electrónico, puede encontrar que revisa su acceso pendiente siguiendo estos pasos.

1. Inicie sesión en el portal de MyApps en [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

    ![Portal de MyApps](./media/review-your-access/myapps-access-panel.png)

1. En la esquina superior derecha de la página, haga clic en el símbolo de usuario que muestra la organización predeterminada y el nombre de usuario. Si aparece más de una organización, seleccione la que haya solicitado una revisión de acceso.

1. En el lado derecho de la página, haga clic en el **las revisiones de acceso** icono para ver una lista de las revisiones de acceso pendientes.

    Si el icono no está visible, no hay ninguna revisión de acceso para esa organización y no se requiere ninguna acción en este momento.

    ![Lista de revisiones de acceso](./media/review-your-access/access-reviews-list.png)

1. Haga clic en el **comenzar revisión** vínculo para la revisión de acceso que desea realizar.

## <a name="perform-the-access-review"></a>Realizar la revisión de acceso

Una vez que ha abierto la revisión de acceso, puede ver los derechos de acceso.

1. Revisión del acceso y decida si todavía tiene acceso.

    Si la solicitud es para revisar el acceso para que otros usuarios, la página tendrá un aspecto diferente. Para obtener más información, consulte [revisen el acceso a grupos o aplicaciones](perform-access-review.md).

    ![Realizar la revisión de acceso](./media/review-your-access/perform-access-review.png)

1. Haga clic en **Sí** para mantener el acceso o haga clic en **No** para quitar el acceso.

1. Si hace clic en **Sí**, es posible que necesite especificar una justificación en el **motivo** cuadro.

    ![Realizar la revisión de acceso](./media/review-your-access/perform-access-review-submit.png)

1. Haga clic en **Enviar**.

    La selección se envía y devuelve en el portal de MyApps.

    Si desea cambiar la respuesta, vuelva a abrir la página de revisiones de acceso y actualizar su respuesta. Puede cambiar su respuesta en cualquier momento hasta que haya finalizado la revisión de acceso.

    > [!NOTE]
    > Si se ha indicado que ya no necesitan tener acceso, no se quitan inmediatamente. Se quitan cuando la revisión ha finalizado o cuando un administrador detiene la revisión.

## <a name="next-steps"></a>Pasos siguientes

- [Revisión de acceso de grupos o aplicaciones](complete-access-review.md)
