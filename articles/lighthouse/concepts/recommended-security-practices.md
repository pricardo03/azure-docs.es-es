---
title: Procedimientos de seguridad recomendados
description: Al usar la administración de recursos delegados de Azure, es importante tener en cuenta la seguridad y el control de acceso.
ms.date: 07/11/2019
ms.topic: conceptual
ms.openlocfilehash: 8972d6548eccb1006d90bfcbb4dba8c01b05a981
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456886"
---
# <a name="recommended-security-practices"></a>Procedimientos de seguridad recomendados

Al usar la administración de recursos delegados de Azure, es importante tener en cuenta la seguridad y el control de acceso. Los usuarios de su inquilino tendrán acceso directo a las suscripciones y los grupos de recursos del cliente, por lo que es recomendable tomar medidas para proteger la seguridad del inquilino. También es recomendable asegurarse de que solo se permite el acceso necesario para administrar de forma eficaz los recursos de los clientes. En este tema se proporcionan recomendaciones para ayudarle a hacerlo.

## <a name="require-azure-multi-factor-authentication"></a>Requerir Azure Multi-Factor Authentication

[Azure Multi-Factor Authentication](../../active-directory/authentication/concept-mfa-howitworks.md) (también conocido como verificación en dos pasos) ayuda a evitar que los atacantes obtengan acceso a una cuenta, ya que requiere varios pasos de autenticación. Debe requerir Multi-Factor Authentication a todos los usuarios del inquilino del proveedor de servicios, incluidos los usuarios que tendrán acceso a los recursos del cliente.

Le recomendamos que pida a sus clientes que implementen también Azure Multi-Factor Authentication en sus inquilinos.

## <a name="assign-permissions-to-groups-using-the-principle-of-least-privilege"></a>Asignar permisos a grupos mediante el principio de privilegios mínimos

Para facilitar la administración, se recomienda usar grupos de usuarios de Azure AD para cada rol necesario para administrar los recursos de los clientes. Esto le permite agregar o quitar usuarios individuales del grupo según sea necesario, en lugar de asignar permisos directamente a ese usuario.

Al crear la estructura de permisos, asegúrese de seguir el principio de privilegios mínimos para que los usuarios solo tengan los permisos necesarios para completar su trabajo, lo que ayuda a reducir la posibilidad de errores involuntarios.

Por ejemplo, puede que quiera usar una estructura como la siguiente:

|Nombre del grupo  |Tipo  |principalId  |Definición de roles  |Id. de definición de roles  |
|---------|---------|---------|---------|---------|
|Arquitectos     |Grupo de usuarios         |\<principalId\>         |Colaborador         |b24988ac-6180-42a0-ab88-20f7382dd24c  |
|Evaluación     |Grupo de usuarios         |\<principalId\>         |Lector         |acdd72a7-3385-48ef-bd42-f606fba81ae7  |
|Especialistas en VM     |Grupo de usuarios         |\<principalId\>         |Colaborador de VM         |9980e02c-c2be-4d73-94e8-173b1dc7cf3c  |
|Automation     |Nombre de entidad de seguridad de servicio (SPN)         |\<principalId\>         |Colaborador         |b24988ac-6180-42a0-ab88-20f7382dd24c  |

Una vez que haya creado estos grupos, puede asignarles usuarios según sea necesario. Agregue solo los usuarios que realmente necesitan tener acceso. Asegúrese de revisar la pertenencia a grupos con regularidad y quite los usuarios que ya no sean adecuados o que no sea necesario incluir.

Tenga en cuenta que al [incorporar clientes a través de una oferta pública de servicios administrados](../how-to/publish-managed-services-offers.md), cualquier grupo (o entidad de servicio o usuario) que incluya tendrá los mismos permisos para todos los clientes que compren el plan. Para asignar grupos diferentes para que trabajen con cada cliente, tendrá que publicar un plan privado independiente exclusivo para cada cliente o incorporar los clientes individualmente mediante plantillas de Azure Resource Manager. Por ejemplo, podría publicar un plan público que tenga un acceso muy limitado y, después, trabajar con el cliente directamente para incorporar sus recursos para obtener acceso adicional mediante una plantilla de recursos de Azure personalizada que concede acceso adicional según sea necesario.


## <a name="next-steps"></a>Pasos siguientes

- [Planificación de una implementación de Azure Multi-Factor Authentication basada en la nube](../../active-directory/authentication/howto-mfa-getstarted.md).
- Más información sobre las [experiencias de administración entre inquilinos](cross-tenant-management-experience.md).
