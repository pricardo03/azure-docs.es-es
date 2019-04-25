---
title: 'Realizar una revisión de acceso desde el portal mis aplicaciones: Azure Active Directory | Microsoft Docs'
description: Obtenga información sobre cómo ver y administrar el acceso de seguridad para las aplicaciones y los grupos de su organización.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 3/21/2019
ms.author: lizross
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 551bbe4e994e15688c59fd395ab762e14927dfda
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60482887"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Realizar una revisión de acceso desde el portal mis aplicaciones
Puede usar su cuenta profesional o educativa con el portal **Aplicaciones** basado en web para ver e iniciar muchas de las aplicaciones de su organización basadas en la nube, actualizar información de su perfil y cuenta, ver su información de **Grupos** y realizar **revisiones de acceso** para sus aplicaciones y grupos. Si no tiene acceso al portal **Aplicaciones**, debe ponerse en contacto con su departamento de soporte técnico para obtener permiso.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Este contenido está diseñado para usuarios. Si es administrador, puede encontrar más información acerca de cómo configurar y administrar aplicaciones basadas en la nube en la [Documentación sobre la administración de aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps).

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso
Si el administrador le ha dado permiso para realizar sus propias revisiones de acceso, puede administrar el acceso de grupos o aplicaciones desde el **las revisiones de acceso** icono en el **mis aplicaciones** página del portal.

>[!Note]
>Si no ve el **las revisiones de acceso** segmentarla cualquiera de estas formas que no tienen permiso para realizar las revisiones de acceso o que no tenga ninguna pendiente revisiones pendientes de aprobación. Si cree que debería tener acceso al icono, póngase en contacto con el soporte técnico.

### <a name="to-perform-your-access-reviews"></a>Para realizar sus revisiones de acceso

1.  Inicie sesión en su cuenta profesional o educativa.

2.  Abra el explorador web y vaya a https://myapps.microsoft.com, o use el vínculo proporcionado por su organización. Por ejemplo, puede es posible que sea dirigido a una página personalizada para su organización, como https://myapps.microsoft.com/contoso.com.

    El **aplicaciones** aparece en la página, que muestra todas las aplicaciones basadas en la nube disponibles para su uso y propiedad de su organización.

    ![Página de aplicaciones en el portal mis aplicaciones](media/my-apps-portal/my-apps-portal-apps-page-access-review-tile.png)

3. Seleccione el **las revisiones de acceso** revisiones de icono para ver una lista de acceso esperando su aprobación.

    ![Revisiones de acceso de página con las revisiones de acceso para la organización](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

4. Seleccione **comenzar revisión** para iniciar la revisión de acceso.

5. Revisión del acceso y determinar si sigue siendo necesario.

    ![Página de revisión de acceso, que muestra los detalles de revisión](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Si es administrador y permite que revisen el acceso de su organización a grupos y aplicaciones, verá una página diferente. Para obtener más información acerca de la revisión de grupos o aplicaciones para su organización, consulte [revisen el acceso a grupos o aplicaciones en las revisiones de acceso de Azure AD](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review).

6. Seleccione **Sí** para mantener el acceso o **No** para quitar el acceso.

    Si selecciona **Sí**, es posible que necesite especificar una justificación en el **motivo** cuadro.

    ![Revisión de acceso de página para mostrar el cuadro de motivo con texto de ejemplo](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. Seleccione **Submit** (Enviar).

    La revisión de acceso está completa y se devuelven a la **mis aplicaciones** portal.

    >[!Note]
    >Puede cambiar el acceso en cualquier momento hasta que el periodo de revisión de su acceso. Si quita el acceso a una aplicación o un grupo, no se quita inmediatamente. La eliminación se produce cuando el acceso a revisar periodo o cuando un administrador cierra la revisión. 

## <a name="next-steps"></a>Pasos siguientes

- [Obtener acceso y usar aplicaciones en el portal mis aplicaciones](my-apps-portal-end-user-access.md).

- [Cambiar la información del perfil](my-apps-portal-end-user-update-profile.md).

- [Ver y actualizar la información de grupos](my-apps-portal-end-user-groups.md).