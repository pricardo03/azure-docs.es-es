---
title: 'Agregar una organización conectada en la administración de derechos de Azure AD: Azure Active Directory'
description: Obtenga información acerca de cómo permitir que los usuarios ajenos a la organización soliciten los paquetes de acceso para que puedan colaborar en proyectos.
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/24/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2dec950f5475a8a64cfecfac1fb25246d6a7aa29
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561916"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Agregar una organización conectada en la administración de derechos de Azure AD

La administración de derechos de Azure AD le permite colaborar con personas ajenas a la organización. Si colabora con frecuencia con usuarios en un dominio o directorio externo de Azure AD, puede agregarlos como una organización conectada. En este artículo se describe cómo agregar una organización conectada para que pueda permitir que los usuarios ajenos a la organización soliciten recursos en el directorio.

## <a name="what-is-a-connected-organization"></a>¿Qué es una organización conectada?

Una organización conectada es un dominio o directorio externo de Azure AD con el que tiene relación.

Por ejemplo, supongamos que trabaja en Woodgrove Bank y desea colaborar con dos organizaciones externas: Instituto de diseño gráfico y Contoso. Le ha indicado su contacto en el Instituto de diseño gráfico que usan Azure AD y que los usuarios del Instituto de diseño gráfico tienen un nombre principal de usuario que termina con `graphicdesigninstitute.com`. Además, le ha indicado su contacto en Contoso que todavía no usan Azure AD, pero que los usuarios de Contoso tienen un nombre principal de usuario que termina con `contoso.com`.

Puede configurar dos organizaciones conectadas: una para el Instituto de diseño gráfico con el dominio `graphicdesigninstitute.com` y otra para Contoso con el dominio `contoso.com`. Si después agrega esas dos organizaciones conectadas a una directiva, los usuarios de cada organización que tengan un nombre principal de usuario que coincida con la directiva podrán solicitar paquetes de acceso. Además, dado que el Instituto de diseño gráfico se identificó como usuario de Azure AD, y si el Instituto de diseño gráfico tiene subdominios más adelante, como `graphicdesigninstitute.example`, los usuarios con ese nombre principal de usuario también podrán solicitar los paquetes de acceso con la misma directiva.

![Ejemplo de una organización conectada](./media/entitlement-management-organization/connected-organization-example.png)

La forma en que los usuarios del directorio Azure AD o el dominio se autenticarán depende del tipo de autenticación. Los tipos de autenticación para las organizaciones conectadas son los siguientes:

- Azure AD
- [Federación directa](../b2b/direct-federation.md)
- [Código de acceso de un solo uso](../b2b/one-time-passcode.md) (dominio)

Para ver una demostración de cómo agregar una organización conectada, vea el vídeo siguiente:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Adición de una organización conectada

Siga estos pasos para agregar un directorio o dominio externo de Azure AD como organización conectada.

**Rol necesario:** Administrador global, administrador de usuarios o invitador de usuarios invitados

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Organizaciones conectadas**, luego en **Agregar una organización conectada**.

    ![Gobernanza de identidades: organizaciones conectadas: agregar organización conectada](./media/entitlement-management-organization/connected-organization.png)

1. Escriba en la pestaña **Básicos** un nombre para mostrar y una descripción para la organización.

    ![Agregar organización conectada: pestaña de básicos](./media/entitlement-management-organization/organization-basics.png)

1. En la pestaña **Directorio + dominio**, haga clic en **Agregar directorio + dominio** para abrir el panel Seleccionar directorios y dominios.

1. Escriba un nombre de dominio para buscar el directorio o el dominio de Azure AD. Debe escribir el nombre de dominio completo.

1. Compruebe que es la organización correcta por el nombre y el tipo de autenticación proporcionados. El modo en que los usuarios iniciarán sesión depende del tipo de autenticación.

    ![Agregar organización conectada: seleccionar directorios y dominios](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Haga clic en **Agregar** para agregar el directorio de Azure AD o el dominio. Actualmente, solo puede agregar un directorio o dominio de Azure AD por organización conectada.

    > [!NOTE]
    > Todos los usuarios del directorio o dominio de Azure AD podrán solicitar este paquete de acceso. Esto incluye a los usuarios de Azure AD de todos los subdominios asociados con el directorio, a menos que los dominios estén bloqueados por la lista de permitidos o denegados de Azure B2B. Para obtener más información, consulte [Allow or block invitations to B2B users from specific organizations](../b2b/allow-deny-list.md) (Permitir o bloquear invitaciones a usuarios de B2B procedentes de determinadas organizaciones).

1. Una vez que haya agregado el directorio o dominio de Azure AD, haga clic en **Seleccionar**.

    La organización aparece en la lista.

    ![Agregar organización conectada: pestaña de directorios](./media/entitlement-management-organization/organization-directory-domain.png)

1. En la pestaña **Patrocinadores**, agregue patrocinadores opcionales para esta organización conectada.

    Los patrocinadores son usuarios internos o externos ya existentes en el directorio que son el punto de contacto para la relación con esta organización conectada. Los patrocinadores internos son usuarios miembros de su directorio. Los patrocinadores externos son usuarios invitados de la organización conectada a los que se ha invitado previamente y que ya están en el directorio. Los patrocinadores se pueden usar como aprobadores cuando los usuarios de esta organización conectada soliciten acceso a este paquete de acceso. Para obtener información sobre cómo traer a un usuario invitado a su directorio, consulte [Agregar usuarios de colaboración de Azure Active Directory B2B en el Azure Portal](../b2b/add-users-administrator.md).

    Al hacer clic en **Agregar o quitar**, aparece un panel para seleccionar los patrocinadores internos o externos. El panel muestra una lista sin filtrar de usuarios y grupos en el directorio.

    ![Paquete de acceso: directiva: agregar organización conectada: pestaña patrocinadores](./media/entitlement-management-organization/organization-sponsors.png)

1. En la pestaña **Revisar + crear**, revise la configuración de la organización y luego haga clic en **Crear**.

    ![Paquete de acceso: directiva: agregar organización conectada: pestaña Revisar + crear](./media/entitlement-management-organization/organization-review-create.png)

## <a name="delete-a-connected-organization"></a>Eliminar una organización conectada

Si ya no tiene una relación con un dominio o directorio externo de Azure AD, puede eliminar la organización conectada.

**Rol necesario:** Administrador global, administrador de usuarios o invitador de usuarios invitados

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Organizaciones conectadas**, luego haga clic en una organización conectada para abrirla.

1. En la página de Información general, haga clic en **Eliminar** para quitar la organización conectada.

    Actualmente, solo se puede eliminar una organización conectada si no hay usuarios conectados.

    ![Gobernanza de identidades: organizaciones conectadas: eliminar organización conectada](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Pasos siguientes

- [Administración del acceso para usuarios externos](entitlement-management-organization.md)
- [Para los usuarios que no están en el directorio](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
