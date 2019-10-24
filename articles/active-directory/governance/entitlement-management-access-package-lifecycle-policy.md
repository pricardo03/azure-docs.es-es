---
title: 'Modificación de la configuración del ciclo de vida para un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Aprenda a modificar la configuración del ciclo de vida de un paquete de acceso en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 10/15/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46d63a12443edf04db3570fa43fbd8f20619122e
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391975"
---
# <a name="change-lifecycle-settings-for-an-access-package-in-azure-ad-entitlement-management-preview"></a>Modificación de la configuración del ciclo de vida para un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Como administrador de paquetes de acceso, puede cambiar la configuración del ciclo de vida de un paquete de acceso en cualquier momento si edita una directiva existente. Si cambia la fecha de expiración para una directiva, la fecha de expiración para las solicitudes que ya están aprobadas o en un estado de aprobación pendiente no cambiará.

En este artículo, se explica cómo se puede modificar la configuración del ciclo de vida de un paquete de acceso existente.

## <a name="open-lifecycle-settings"></a>Apertura de la configuración del ciclo de vida

Para cambiar la configuración del ciclo de vida de un paquete de acceso, debe abrir la directiva correspondiente. Siga estos pasos para abrir la configuración del ciclo de vida de un paquete de acceso.

**Rol necesario:** Administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú de la izquierda, haga clic en **Paquetes de acceso** y luego abra el paquete de acceso.

1. Haga clic en **Directivas** y, a continuación, haga clic en la directiva que contenga la configuración del ciclo de vida que desee editar.

    Se abre el panel Detalles de directiva en la parte inferior de la página.

    ![Paquete de acceso - Panel Detalles de directiva](./media/entitlement-management-shared/policy-details.png)

1. Haga clic en **Editar** para editar la directiva.

    ![Paquete de acceso - Editar directiva](./media/entitlement-management-shared/policy-edit.png)

1. Haga clic en la pestaña **Ciclo de vida** para abrir la configuración del ciclo de vida.

[!INCLUDE [Entitlement management lifecycle policy](../../../includes/active-directory-entitlement-management-lifecycle-policy.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Modificación de la configuración de solicitudes y aprobación de un paquete de acceso](entitlement-management-access-package-request-policy.md)