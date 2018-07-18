---
title: ¿En qué consisten las licencias basadas en grupos de Azure Active Directory? | Microsoft Docs
description: Descripción de las licencias basadas en grupo de Azure Active Directory, cómo funcionan y procedimientos recomendados
services: active-directory
keywords: Licencias de Azure AD
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: fundamentals
ms.topic: overview
ms.workload: identity
ms.date: 06/13/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: c6800e5a00d2fb837538141571c105a1d21e4374
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37860460"
---
# <a name="group-based-licensing-basics-in-azure-active-directory"></a>Aspectos básicos de las licencias basadas en grupos en Azure Active Directory

Los servicios en la nube de pago de Microsoft, como Office 365, Enterprise Mobility + Security, Dynamics 365 y otros productos similares, requieren licencias. Estas licencias se asignan a cada usuario que necesita acceso a estos servicios. Para administrar las licencias, los administradores usan uno de los portales de administración (ya sea Office o Azure) y los cmdlets de PowerShell. Azure Active Directory (Azure AD) es la infraestructura subyacente que admite la administración de identidades para todos los servicios en la nube de Microsoft. Azure AD almacena información sobre los estados de asignación de licencias para los usuarios.

Hasta ahora, las licencias solo podían asignarse a nivel de cada usuario, lo que puede dificultar la administración a gran escala. Por ejemplo, para agregar o quitar licencias de usuario en función de los cambios que se producen en la organización, por ejemplo, la incorporación o la baja de un usuario en la organización o en un departamento, un administrador a menudo debe escribir un script de PowerShell complejo. Este script hace llamadas individuales al servicio en la nube.

Para abordar esos desafíos, Azure AD incluye ahora las licencias basadas en grupo. Puede asignar una o varias licencias de producto a un grupo. Azure AD garantiza que las licencias se asignen a todos los miembros del grupo. A todos los miembros nuevos que se unan al grupo se les asignarán las licencias correspondientes. Cuando salen del grupo, se quitan esas licencias. De esta forma, ya no es necesario automatizar la administración de licencias a través de PowerShell para reflejar los cambios que se producen en la organización y en la estructura de departamento por cada usuario.

>[!NOTE]
>Esta función actualmente se encuentra en versión preliminar pública. Debe estar preparado para deshacer o eliminar los cambios. La característica está disponible en cualquier plan de licencias de Azure Active Directory (Azure AD) de pago durante el período de versión preliminar pública. Sin embargo, cuando ya esté disponible con carácter general, algunos aspectos de ella podrían requerir una o varias licencias de Azure Active Directory Premium.

## <a name="features"></a>Características

A continuación se indican las características principales de las licencias basadas en grupos:

- Se pueden asignar licencias a todos los grupos de seguridad en Azure AD. Los grupos de seguridad se pueden sincronizar en el entorno local mediante Azure AD Connect. También puede crear grupos de seguridad directamente en Azure AD (también denominados grupos solo de nube) o de forma automática, a través de la característica de grupo dinámico de Azure AD.

- Cuando se asigna una licencia de producto a un grupo, el administrador puede deshabilitar uno o varios planes de servicio del producto. Habitualmente, esto se hace cuando la organización todavía no está preparada para comenzar a usar un servicio incluido en un producto. Por ejemplo, el administrador podría asignar Office 365 a un departamento y deshabilitar temporalmente el servicio Yammer.

- Se admiten todos los Servicios en la nube de Microsoft que requieren licencias a nivel de usuario. Se incluyen todos los productos de Office 365, Enterprise Mobility + Security y Dynamics 365.

- Las licencias basadas en grupos actualmente solo están disponibles a través de [Azure Portal](https://portal.azure.com). Si usa principalmente otros portales de administración para la administración de usuarios y grupos, como el portal de Office 365, puede seguir haciéndolo. Sin embargo, debe usar Azure Portal para administrar las licencias en el nivel de grupo.

- Azure AD administra automáticamente las modificaciones de licencia resultantes de los cambios de pertenencia a grupos. Habitualmente, las modificaciones de licencia entran en vigor minutos después de un cambio en la pertenencia.

- Un usuario puede ser miembro de varios grupos con directivas de licencia especificadas. Un usuario también puede tener algunas licencias que se asignaron directamente, fuera de cualquier grupo. El estado de usuario resultante es una combinación de todas las licencias de producto y servicio asignadas.

- En algunos casos, las licencias no se pueden asignar a un usuario. Por ejemplo, es posible que no haya licencias disponibles suficientes en el inquilino o puede que se hayan asignado servicios en conflicto al mismo tiempo. Los administradores tienen acceso a información sobre usuarios para los que Azure AD no pudo procesar íntegramente las licencias de grupo. Pueden realizar acciones correctivas según esa información.

- Durante la versión preliminar pública, se requiere una suscripción de pago o de prueba a las ediciones básica o premium de Azure AD en el inquilino para usar la administración de licencias basadas en grupos.

## <a name="your-feedback-is-welcome"></a>Agradecemos sus comentarios.

Si tiene comentarios o solicitudes de características, compártalos con nosotros a través del [foro de administradores de Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=162510).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre otros escenarios de administración de licencias basadas en grupos, consulte:

* [Asignación de licencias a un grupo en Azure Active Directory](../users-groups-roles/licensing-groups-assign.md)
* [Identificación y resolución de problemas de licencias de un grupo en Azure Active Directory](../users-groups-roles/licensing-groups-resolve-problems.md)
* [Migración de usuarios individuales con licencia a licencias basadas en grupos en Azure Active Directory](../users-groups-roles/licensing-groups-migrate-users.md)
* [Escenarios adicionales de licencias basadas en grupos de Azure Active Directory](../users-groups-roles/licensing-group-advanced.md)
