---
title: 'Delegación de la gobernanza del acceso en los creadores de catálogos desde la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Aprenda a delegar la gobernanza del acceso de los administradores de TI en los creadores de catálogos y los jefes de proyecto para que ellos mismos puedan encargarse del acceso.
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
ms.openlocfilehash: da6183599f2dacd1a98d8fe359edda741b903b50
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170882"
---
# <a name="delegate-access-governance-to-catalog-creators-in-azure-ad-entitlement-management-preview"></a>Delegación de la gobernanza del acceso en los creadores de catálogos desde la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Para delegar en usuarios que no son administradores de modo que puedan crear sus propios catálogos, puede agregarlos al rol de creador de catálogos definido en la administración de derechos de Azure AD. Puede agregar usuarios individuales o bien agregar un grupo, cuyos miembros pueden luego crear catálogos.

## <a name="as-an-it-administrator-delegate-to-a-catalog-creator"></a>Como administrador de TI, delegación en un creador de catálogos

Siga estos pasos para asignar un usuario al rol de creador de catálogos.

**Rol necesario:** administrador global o administrador de usuarios.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú izquierdo, en la sección **Administración de derechos**, haga clic en **Configuración**.

1. Haga clic en **Editar**.

    ![Configuración para agregar creadores de catálogos](./media/entitlement-management-delegate/settings-delegate.png)

1. En la sección **Delegate entitlement management** (Delegar la administración de derechos), haga clic en **Agregar creador de catálogos** para seleccionar los usuarios o grupos en los que desea delegar este rol de administración de derechos.

1. Haga clic en **Seleccionar**.

1. Haga clic en **Save**(Guardar).

## <a name="next-steps"></a>Pasos siguientes

- [Creación y administración de un catálogo de recursos](entitlement-management-catalog-create.md)
- [Delegación de la gobernanza del acceso en administradores de paquetes de acceso](entitlement-management-delegate-managers.md)

