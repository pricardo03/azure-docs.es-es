---
title: 'Administración del acceso de su organización a aplicaciones y grupos: Azure AD'
description: Obtenga información sobre cómo realizar una revisión de acceso para administrar el acceso de seguridad para las aplicaciones y grupos de su organización desde el portal Aplicaciones.
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
ms.openlocfilehash: e739024686bdac497b9b7dd450c5ed46e3cf9a63
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76705027"
---
# <a name="perform-an-access-review-from-the-my-apps-portal"></a>Realización de una revisión de acceso desde el portal Aplicaciones

Puede usar su cuenta profesional o educativa con el portal **Aplicaciones** basado en web para ver e iniciar muchas de las aplicaciones de su organización basadas en la nube, actualizar información de su perfil y cuenta, ver su información de **Grupos** y realizar **revisiones de acceso** para sus aplicaciones y grupos. Si no tiene acceso al portal **Aplicaciones**, debe ponerse en contacto con su departamento de soporte técnico para obtener permiso.

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-my-apps-portal.md)]

>[!Important]
>Este contenido está diseñado para usuarios. Si es administrador, puede encontrar más información acerca de cómo configurar y administrar aplicaciones basadas en la nube en la [Documentación sobre la administración de aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps).

## <a name="manage-access-reviews"></a>Administración de revisiones del acceso

Si el administrador le ha dado permiso para realizar sus propias revisiones de acceso, puede administrar el acceso de grupos o aplicaciones desde el icono **Revisiones de acceso** de la página del portal **Aplicaciones**.

>[!Note]
>Si no ve el icono **Revisiones de acceso** significa que no tienen permiso para realizar las revisiones de acceso o que no tiene ninguna revisión pendiente que esté esperando su aprobación. Si cree que debería tener acceso al icono, póngase en contacto con el departamento de soporte técnico para obtener ayuda.

### <a name="to-perform-your-access-reviews"></a>Para realizar sus revisiones de acceso

1. Inicie sesión en su cuenta profesional o educativa.

2. Abra el explorador web y vaya a https://myapps.microsoft.com o use el vínculo proporcionado por su organización. Por ejemplo, quizá se le redirija a una página personalizada para la organización, como https://myapps.microsoft.com/contoso.com.

    Aparecerá la página **Aplicaciones**, que muestra todas las aplicaciones basadas en la nube disponibles para su uso y propiedad de su organización.

    ![Página de aplicaciones en el portal Mis aplicaciones](media/my-apps-portal/my-apps-portal-apps-page-access-review-tile.png)

3. Seleccione el icono **Revisiones de acceso** para ver una lista de revisiones de acceso que esperan su aprobación.

    ![Página Revisiones de acceso con revisiones de acceso pendientes para la organización](media/my-apps-portal/my-apps-portal-access-reviews-page.png)

4. Seleccione **Comenzar revisión** para iniciar la revisión de acceso.

5. Revise el acceso y determine si sigue siendo necesario.

    ![Página Revisión de acceso que muestra los detalles de revisión](media/my-apps-portal/my-apps-portal-perform-access-reviews-page.png)

    >[!Note]
    >Si es administrador y tiene permiso para revisar el acceso de su organización a grupos y aplicaciones, verá una página diferente. Para más información sobre la revisión de grupos o aplicaciones para su organización, consulte [Revisión del acceso a grupos o aplicaciones en las revisiones de acceso de Azure AD](https://docs.microsoft.com/azure/active-directory/governance/perform-access-review).

6. Seleccione **Sí** para mantener el acceso o **No** para quitarlo.

    Si selecciona **Sí**, es posible que necesite especificar una justificación en el cuadro **Motivo**.

    ![Página Revisión de acceso que se muestra el cuadro Motivo con texto de ejemplo](media/my-apps-portal/my-apps-portal-perform-access-reviews-reason-box.png)

7. Seleccione **Submit** (Enviar).

    La revisión de acceso se ha completado y vuelve al portal **Aplicaciones**.

    >[!Note]
    >Puede cambiar el acceso en cualquier momento hasta que el período de revisión de acceso termine. Si quita el acceso a una aplicación o un grupo, no se quita inmediatamente. La eliminación se produce cuando el período de revisión de acceso termina o cuando un administrador cierra la revisión.

## <a name="next-steps"></a>Pasos siguientes

- [Acceder y usar las aplicaciones del portal Aplicaciones](my-apps-portal-end-user-access.md).

- [Modificación de información de perfil](my-apps-portal-end-user-update-profile.md).

- [Ver y actualizar la información relacionada con grupos](my-apps-portal-end-user-groups.md).
