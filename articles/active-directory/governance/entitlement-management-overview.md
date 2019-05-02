---
title: '¿Qué es la administración de derechos de Azure AD? (Versión preliminar): Azure Active Directory'
description: Obtenga información general de administración de derechos de Azure Active Directory y cómo puede utilizarlo para administrar el acceso a grupos, aplicaciones y sitios de SharePoint Online para usuarios internos y externos.
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/27/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3abe2f7deef2a1dbe82f4702fd3477303891ab2e
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64873559"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>¿Qué es la administración de derechos de Azure AD?  (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Los empleados de las organizaciones necesitan acceso a varios grupos, aplicaciones y sitios para realizar su trabajo. Es difícil administrar este acceso. En la mayoría de los casos, no hay ninguna lista organizada de todos los recursos que necesita un usuario para un proyecto. El jefe de proyecto tiene una buena comprensión de los recursos que necesita, las personas implicadas y cuánto tiempo durará el proyecto. Sin embargo, el jefe de proyecto normalmente no tiene permisos para aprobar o conceder acceso a otros usuarios. Este escenario se complica cuando intenta trabajar con usuarios externos o empresas.

Administración de derechos de Azure Active Directory (Azure AD) puede ayudarle a administrar el acceso a grupos, aplicaciones y sitios de SharePoint Online para usuarios internos y también a los usuarios ajenos a su organización.

## <a name="why-use-entitlement-management"></a>¿Por qué usar Administración de derechos?

Las organizaciones empresariales suelen enfrentan desafíos al administrar el acceso a recursos como:

- Los usuarios pueden no saber qué deben disponer de acceso
- Los usuarios pueden tener dificultades para encontrar las personas adecuadas o los recursos adecuados
- Una vez que los usuarios buscar y obtener acceso a un recurso, puede mantener para tener acceso a más de lo que es necesario para fines empresariales

Estos problemas se agravan para los usuarios que necesitan tener acceso desde otro directorio, por ejemplo, los usuarios externos que provienen de las organizaciones de cadena de suministro o de otro socio comercial. Por ejemplo: 

- Las organizaciones no pueden conocer todos los individuos específicos en otros directorios que puedan invitarlos
- Incluso si las organizaciones pueden invitar a estos usuarios, las organizaciones no es posible que recuerde a administrar todo el acceso del usuario coherente

Administración de derechos de Azure AD puede ayudar a enfrentar estos desafíos.

## <a name="what-can-i-do-with-entitlement-management"></a>¿Qué puedo hacer con la administración de derechos?

Estas son algunas de las capacidades de administración de derechos:

- Crear paquetes de recursos relacionados que los usuarios pueden solicitar
- Definir reglas acerca de cómo solicitar recursos y cuándo expira el acceso
- Controlar el ciclo de vida de acceso para usuarios internos y externos
- Delegar la administración de recursos
- Designar los aprobadores para aprobar solicitudes
- Crear informes para realizar un seguimiento del historial

Para obtener información general del control de identidad y administración de derechos, vea el siguiente vídeo de la conferencia de Ignite de 2018:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>¿Qué recursos se debe administrar?

Estos son los tipos de recursos que puede administrar el acceso a con la administración de derechos:

- Grupos de seguridad de Azure AD
- Grupos de Office 365
- Aplicaciones empresariales de Azure AD
- Aplicaciones SaaS
- Aplicaciones integradas personalizadas
- Colecciones de sitios de SharePoint Online
- Sitios de SharePoint Online

## <a name="prerequisites"></a>Requisitos previos

Para usar la administración de derechos de Azure AD (versión preliminar), debe tener una de las licencias siguientes:

- Azure AD Premium P2
- Licencia de Enterprise Mobility + Security (EMS) E5

Para obtener más información, consulte [registrarse para las ediciones de Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) o [Enterprise Mobility + Security E5 evaluación](https://aka.ms/emse5trial).

Nubes especializadas, como Azure Government, Azure Alemania y Azure China 21Vianet, no están disponibles actualmente para su uso en esta versión preliminar.

## <a name="what-are-access-packages-and-policies"></a>¿Cuáles son los paquetes de acceso y directivas?

Administración de derechos introduce el concepto de un *paquete acceso*. Un paquete de acceso es una agrupación de todos los recursos que un usuario necesita para trabajar en un proyecto o realizar su trabajo. Los recursos incluyen el acceso a grupos, aplicaciones o sitios. Paquetes de acceso se utilizan para controlar el acceso para sus empleados internos y también usuarios ajenos a su organización. Paquetes de acceso se definen en contenedores llamados *catálogos*.

Paquetes de acceso también incluyen uno o varios *directivas*. Una directiva define las reglas o barreras de seguridad para tener acceso a un paquete de acceso. Al habilitar una directiva exige que solo los usuarios adecuados se conceden acceso a los recursos adecuados y para la cantidad adecuada de tiempo.

![Paquete de acceso y directivas](./media/entitlement-management-overview/elm-overview-access-package.png)

Con un paquete de acceso y sus directivas, el Administrador de paquetes de acceso define:

- Recursos
- Roles de los usuarios necesitan para los recursos
- Los usuarios internos y externos a los usuarios que son aptos solicitar acceso
- Proceso de aprobación y los usuarios que pueden aprobar o denegar el acceso
- Duración del acceso del usuario

El siguiente diagrama muestra un ejemplo de los distintos elementos en la administración de derechos. Muestra dos paquetes de ejemplo de acceso.

- **Paquete de acceso 1** incluye un único grupo como un recurso. Acceso se define con una directiva que habilita un conjunto de usuarios en el directorio para solicitar acceso.
- **Paquete de acceso 2** incluye un grupo, una aplicación y un sitio de SharePoint Online como recursos. Acceso se define con dos directivas diferentes. La primera directiva habilita un conjunto de usuarios en el directorio para solicitar acceso. La segunda directiva permite a los usuarios en un directorio externo solicitar acceso.

![Información general sobre la administración de derechos](./media/entitlement-management-overview/elm-overview.png)

## <a name="external-users"></a>Usuarios externos

Cuando se usa el [Azure AD negocio a negocio (B2B)](../b2b/what-is-b2b.md) invitar experiencia, ya debe conocer las direcciones de correo electrónico de los usuarios invitados externos que desea incorporar a su directorio de recursos y trabajar con. Esto funciona muy bien cuando está trabajando en un proyecto más pequeño o a corto plazo y que ya conoce a todos los participantes, pero esto es más difícil de administrar si tiene una gran cantidad de usuarios que desea trabajar con o si los participantes cambian con el tiempo.  Por ejemplo, puede trabajar con otra organización y tener un punto de contacto con dicha organización, pero con el tiempo adicional a los usuarios de dicha organización también necesitará acceso.

Con la administración de derechos, puede definir una directiva que permite a los usuarios de organizaciones que especifique, que también usan Azure AD, para poder solicitar un paquete de acceso. Puede especificar una fecha de expiración para el acceso y si es necesaria la aprobación. Si se requiere aprobación, también puede designar como un aprobador de uno o varios usuarios de organizaciones externas que anteriormente invitado - dado que es probables que saber qué usuarios externos de su organización necesitan tener acceso. Una vez haya configurado el paquete de acceso, puede enviar un vínculo al paquete acceso a la persona de contacto en la organización externa. Dicho contacto puede compartir con otros usuarios de la organización externa, y puede usar este vínculo para solicitar el paquete de acceso.  A los usuarios que ya han sido invitados a su directorio de la organización que también pueden usar ese vínculo.

Cuando se aprueba una solicitud, administración de derechos proporcionará al usuario con el acceso necesario, que puede incluir a invitar al usuario si no están ya en el directorio. Azure AD creará automáticamente una cuenta de B2B para ellos.  Tenga en cuenta que un administrador puede haber limitado antes que las organizaciones se permiten para la colaboración, estableciendo una [B2B permitir o denegar lista](../b2b/allow-deny-list.md) permitir o bloquear invitaciones a otras organizaciones.  Si el usuario no está autorizado por la lista de permitidos o bloqueados, a continuación, no se les invitará.

Puesto que no se desea acceso del usuario externo a infinita, especifique una fecha de expiración de la directiva, por ejemplo, 180 días. Después de 180 días, si no se renueva su acceso, administración de derechos quitará todo el acceso asociado con ese paquete de acceso.  Si el usuario que se invitaron a través de la administración de derechos no tenga ningún otras asignaciones de paquete de acceso, a continuación, cuando pierde su última asignación, su cuenta de B2B se se impide el inicio de sesión durante 30 días y posteriormente quitar.  Esto evita la proliferación de cuentas innecesarias.  

## <a name="terminology"></a>Terminología

Para entender mejor la administración de derechos y su documentación, debe revisar los términos siguientes.

| Término o concepto | DESCRIPCIÓN |
| --- | --- |
| administración de derechos | Un servicio que se asigna, se revoca y administra los paquetes de acceso. |
| paquete de acceso | Una colección de permisos y directivas a los recursos que los usuarios pueden solicitar. Un paquete de acceso siempre se encuentra en un catálogo. |
| solicitud de acceso | Una solicitud para obtener acceso a un paquete de acceso. Normalmente, una solicitud pasa por un flujo de trabajo. |
| policy | Un conjunto de reglas que define el ciclo de vida de acceso, como cómo los usuarios obtienen acceso, quién puede aprobar y cuánto tiempo los usuarios tienen acceso. Las directivas de ejemplo incluyen el acceso de los empleados y el acceso externo. |
| catálogo | Un contenedor de paquetes de acceso y recursos relacionados. |
| Catálogo general | Un catálogo integrado que siempre está disponible. Para agregar recursos al catálogo General, requiere determinados permisos. |
| resource | Un recurso o servicio (por ejemplo, un grupo, aplicación o sitio) que se puede conceder permisos para un usuario. |
| tipo de recurso | El tipo de recurso, que incluye grupos, aplicaciones y sitios de SharePoint Online. |
| roles de recursos | Una colección de permisos asociados a un recurso. |
| directorio de recursos | Un directorio que tiene uno o más recursos para compartirla. |
| usuarios asignados | Asignación de un paquete de acceso a un usuario o grupo. |
| enable | El proceso de poner a disposición de los usuarios solicitar un paquete de acceso. |

## <a name="roles-and-permissions"></a>Roles y permisos

Administración de derechos tiene funciones diferentes en función de la función de trabajo.

| Rol | DESCRIPCIÓN |
| --- | --- |
| [Administrador de usuarios](../users-groups-roles/directory-assign-admin-roles.md#user-administrator) | Administrar todos los aspectos de administración de derechos.<br/>Crear usuarios y grupos. |
| Creador del catálogo | Crear y administrar catálogos. Normalmente, un administrador de TI o el propietario del recurso. La persona que crea un catálogo automáticamente se convierte en propietario del catálogo primera del catálogo. |
| Propietario del catálogo | Editar y administrar catálogos existentes. Normalmente, un administrador de TI o el propietario del recurso. |
| Administrador de paquetes de acceso | Editar y administrar todos los paquetes existentes de acceso dentro de un catálogo. |
| Aprobador | Aprobar solicitudes de acceso a los paquetes. |
| Solicitante | Solicitar acceso. |

En la tabla siguiente se enumera los permisos para cada uno de estos roles.

| Tarea | Administrador de usuarios | Creador del catálogo | Propietario del catálogo | Administrador de paquetes de acceso | Aprobador |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Crear un nuevo paquete de acceso en el catálogo General](entitlement-management-access-package-create.md) | :heavy_check_mark: |  :heavy_check_mark: |  |  |  |
| [Crear un nuevo paquete de acceso en un catálogo](entitlement-management-access-package-create.md) | :heavy_check_mark: |   | :heavy_check_mark: |  |  |
| [Agregar o quitar roles de recursos de un paquete de acceso](entitlement-management-access-package-edit.md) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Especificar quién puede solicitar un paquete de acceso](entitlement-management-access-package-edit.md#add-a-new-policy) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Asignar a un usuario directamente a un paquete de acceso](entitlement-management-access-package-edit.md#directly-assign-a-user) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ver quién tiene una asignación a un paquete de acceso](entitlement-management-access-package-edit.md#view-who-has-an-assignment) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ver las solicitudes de un paquete de acceso](entitlement-management-access-package-edit.md#view-requests) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ver errores de entrega de una solicitud](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Cancelar una solicitud pendiente](entitlement-management-access-package-edit.md#cancel-a-pending-request) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ocultar un paquete de acceso](entitlement-management-access-package-edit.md#change-the-hidden-setting) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Eliminar un paquete de acceso](entitlement-management-access-package-edit.md#delete) | :heavy_check_mark: |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Aprobar una solicitud de acceso](entitlement-management-request-approve.md) |  |  |  |  | :heavy_check_mark: |
| [Crear un catálogo](entitlement-management-catalog-create.md) | :heavy_check_mark: | :heavy_check_mark: |  |  |  |
| [Agregar o quitar recursos desde el catálogo General](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  |  |  |  |
| [Agregar o quitar recursos de un catálogo](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Agregar propietarios de catálogo o tener acceso a los administradores de paquetes](entitlement-management-catalog-create.md#add-catalog-owners-or-access-package-managers) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |
| [Editar o eliminar un catálogo](entitlement-management-catalog-create.md#edit-a-catalog) | :heavy_check_mark: |  | :heavy_check_mark: |  |  |

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Crear el primer paquete de acceso](entitlement-management-access-package-first.md)
- [Escenarios comunes](entitlement-management-scenarios.md)
