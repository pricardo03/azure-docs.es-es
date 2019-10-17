---
title: 'Delegación de la gobernanza del acceso en administradores de paquetes de acceso desde la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Aprenda a delegar la gobernanza del acceso de los administradores de TI en los administradores de paquetes de acceso y los jefes de proyecto para que ellos mismos puedan encargarse del acceso.
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
ms.date: 10/07/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: c7a2b6bfdb4904e11ffba3a9fe1097c7f5cfe9d6
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170874"
---
# <a name="delegate-access-governance-to-access-package-managers-in-azure-ad-entitlement-management-preview"></a>Delegación de la gobernanza del acceso en administradores de paquetes de acceso desde la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para delegar la creación y administración de paquetes de acceso en un catálogo, se agregan usuarios al rol de administrador de paquetes de acceso. Estos administradores deben estar familiarizados con la necesidad de que los usuarios soliciten acceso a los recursos de un catálogo. Por ejemplo, si se utiliza un catálogo en un proyecto, el administrador de paquetes de acceso para ese catálogo podría ser un responsable del proyecto.  Los administradores de paquetes de acceso no pueden agregar recursos a un catálogo, pero sí pueden administrar los paquetes de acceso y las directivas del catálogo.  Cuando se delegar en un administrador de paquetes de acceso, esta persona puede responsabilizarse después de los siguientes aspectos:

- Qué roles tendrá un usuario para los recursos de un catálogo
- Quién necesitará acceso
- Quién necesita aprobar las solicitudes de acceso
- Cuánto tiempo durará el proyecto

En este vídeo se ofrece información general sobre el modo en que el propietario del catálogo delega la gobernanza del acceso en un administrador de paquetes de acceso.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE3Lq08]

## <a name="as-a-catalog-owner-delegate-to-an-access-package-manager"></a>Como propietario del catálogo, delegación en un administrador de paquetes de acceso

Siga estos pasos para asignar un usuario al rol de administrador de paquetes de acceso:

**Rol necesario:** administrador global, administrador de usuarios o propietario del catálogo.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo al que quiere agregar administradores.

1. En el menú izquierdo, haga clic en **Roles y administradores**.

    ![Roles y administradores de catálogos](./media/entitlement-management-shared/catalog-roles-administrators.png)

1. Haga clic en **Agregar administradores del paquete de acceso** para seleccionar los miembros de este rol.

1. Haga clic en **Seleccionar** para agregar estos miembros.

## <a name="remove-an-access-package-manager"></a>Eliminación de un administrador de paquetes de acceso

Siga estos pasos para quitar a un usuario del rol de administrador de paquetes de acceso:

**Rol necesario:** administrador global, administrador de usuarios o propietario del catálogo.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo al que quiere agregar administradores.

1. En el menú izquierdo, haga clic en **Roles y administradores**.

1. Agregue una marca de verificación junto al administrador de paquetes de acceso que desea quitar.

1. Haga clic en **Quitar**.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de un paquete de acceso](entitlement-management-access-package-create.md)
