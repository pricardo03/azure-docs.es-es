---
title: Administración de unidades administrativas (versión preliminar) en Azure Active Directory | Microsoft Docs
description: Uso de unidades administrativas para una delegación más detallada de permisos en Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: article
ms.subservice: users-groups-roles
ms.workload: identity
ms.date: 08/01/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b4bdced50f806367a53881d5ef0abd0a3710496
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2019
ms.locfileid: "68736778"
---
# <a name="administrative-units-management-in-azure-active-directory-public-preview"></a>Administración de unidades administrativas en Azure Active Directory (versión preliminar pública)

En este artículo, se describen las unidades administrativas de Azure Active Directory (Azure AD), que son un contenedor de recursos que se puede usar para delegar permisos administrativos en subconjuntos de usuarios y aplicar directivas a un subconjunto de usuarios. En Azure AD, las unidades administrativas habilitan a los administradores centrales para delegar permisos a administradores regionales o para establecer directivas en un nivel detallado.

Esto es útil en organizaciones con divisiones independientes, por ejemplo, una universidad grande que se compone de varias escuelas autónomas (escuela de negocios, escuela de ingenieros, etc.) que son independientes entre ellas. Estas divisiones tienen sus propios administradores de TI que controlan el acceso, administran usuarios y establecen directivas específicamente para su división. Los administradores centrales desean poder otorgar a estos administradores de divisiones para los usuarios en sus divisiones determinadas. Más específicamente, con este ejemplo, un administrador central puede, por ejemplo, crear una unidad administrativa para una determinada escuela (escuela de negocios) y rellenarla solo con usuarios de la escuela de negocios. Luego, un administrador central puede agregar al personal de TI de la escuela de negocios a un rol de ámbito, en otras palabras, conceder al personal de TI permisos administrativos de la escuela de negocios sobre la unidad administrativa de la misma escuela.

> [!IMPORTANT]
> El uso de unidades administrativas requiere que el administrador de la unidad administrativa tenga una licencia Azure Active Directory Premium. Para obtener más información, consulte [Introducción a Azure AD Premium](../fundamentals/active-directory-get-started-premium.md).
>

Desde el punto de vista del administrador central, una unidad administrativa es un objeto de directorio que se puede crear y rellenar con recursos. **En esta versión preliminar, estos recursos solo pueden ser usuarios.** Una vez creada y rellenada, la unidad administrativa puede usarse como ámbito para restringir el permiso otorgado para los recursos que contiene la unidad administrativa.

## <a name="managing-administrative-units"></a>Administración de unidades administrativas

En esta versión de vista previa, puede crear y administrar unidades administrativas con los cmdlets del módulo de Azure Active Directory para Windows PowerShell. Para más información sobre cómo hacerlo, vea [Trabajo con unidades administrativas](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)

Para más información sobre los requisitos de software y la instalación del módulo de Azure AD, además de información sobre los cmdlets del módulo de Azure AD para administrar las unidades administrativas, incluida sintaxis, descripciones de parámetros y ejemplos, vea [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).

## <a name="next-steps"></a>Pasos siguientes

[Ediciones de Azure Active Directory](../fundamentals/active-directory-whatis.md)
