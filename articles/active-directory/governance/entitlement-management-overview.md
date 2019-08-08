---
title: '¿Qué es la administración de derechos de Azure AD? (Versión preliminar): Azure Active Directory'
description: Obtenga información general de la administración de derechos de Azure Active Directory y cómo puede utilizarla para administrar el acceso a grupos, aplicaciones y sitios de SharePoint Online para usuarios internos y externos.
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
ms.date: 07/10/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 12452b4e9cc6caa64d4c81a310fbccb5d1717817
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68678155"
---
# <a name="what-is-azure-ad-entitlement-management-preview"></a>¿Qué es la administración de derechos de Azure AD? (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Los empleados de las organizaciones necesitan tener acceso a varios grupos, aplicaciones y sitios para realizar su trabajo. La administración de este acceso es un reto. En la mayoría de los casos, no existe una lista organizada de todos los recursos que un usuario necesita para un proyecto. El jefe de proyecto tiene un buen entendimiento de los recursos necesarios, de las personas implicadas y de cuánto tiempo durará el proyecto. Sin embargo, el jefe de proyecto normalmente no tiene permisos para aprobar o conceder acceso a otros. Este escenario se complica cuando se intenta trabajar con personas o compañías externas.

La administración de derechos de Azure Active Directory (Azure AD) puede ayudarle a administrar el acceso a grupos, aplicaciones y sitios de SharePoint Online para usuarios internos también para usuarios que están fuera de la organización.

## <a name="why-use-entitlement-management"></a>¿Qué es la administración de derechos?

Las organizaciones empresariales a menudo se enfrentan a desafíos a la hora de administrar el acceso a recursos tales como:

- Los usuarios pueden no saber qué acceso deberían tener
- Los usuarios pueden tener dificultades para localizar a las personas o los recursos adecuados.
- Una vez que los usuarios buscan y reciben acceso a un recurso, pueden retener el acceso por más tiempo del necesario para fines empresariales.

Estos problemas se agravan para los usuarios que necesitan acceso desde otro directorio, como los usuarios externos que pertenecen a organizaciones de la cadena de suministro u otros asociados comerciales. Por ejemplo:

- Es posible que las organizaciones no conozcan a todos los individuos específicos de otros directorios para poder invitarlos.
- Incluso si las organizaciones pudieran invitar a estos usuarios, es posible que no recuerden administrar todo el acceso de los usuarios de forma coherente.

La administración de derechos de Azure AD puede ayudar a abordar estos desafíos.

## <a name="what-can-i-do-with-entitlement-management"></a>¿Qué se puede hacer con la administración de derechos?

Estas son algunas de las funcionalidades de la administración de derechos:

- Crear paquetes de recursos relacionados que los usuarios pueden solicitar
- Definir reglas sobre cómo solicitar recursos y cuándo expira el acceso
- Controlar el ciclo de vida del acceso tanto para usuarios internos como externos
- Delegar la administración de los recursos
- Designar los aprobadores para autorizar las solicitudes
- Crear informes para realizar el seguimiento del historial

Para obtener información general de la gobernanza de identidades y de la administración de derechos, vea el siguiente vídeo de la conferencia Ignite 2018:

>[!VIDEO https://www.youtube.com/embed/aY7A0Br8u5M]

## <a name="what-resources-can-i-manage"></a>¿Qué recursos se deben administrar?

Estos son los tipos de recursos a los que puede administrar el acceso con la administración de derechos:

- Grupos de seguridad de Azure AD
- Grupos de Office 365
- Aplicaciones empresariales Azure AD, incluidas las aplicaciones SaaS y aplicaciones integradas personalizadas que admiten la federación o el aprovisionamiento.
- Sitios y colecciones de sitios de SharePoint Online

También puede controlar el acceso a otros recursos que dependen de los grupos de seguridad de Azure AD o de Office 365.  Por ejemplo:

- Puede conceder licencias a los usuarios para Microsoft Office 365 mediante un grupo de seguridad de Azure AD en un paquete de acceso y la configuración de [licencias basadas en grupos](../users-groups-roles/licensing-groups-assign.md) para ese grupo.
- Puede dar acceso a los usuarios para administrar los recursos de Azure mediante un grupo de seguridad de Azure AD en un paquete de acceso y la creación de una [asignación de roles de Azure](../../role-based-access-control/role-assignments-portal.md) para ese grupo.

## <a name="what-are-access-packages-and-policies"></a>¿Qué son los paquetes de acceso y las directivas?

La administración de derechos introduce el concepto de un *paquete de acceso*. Un paquete de acceso es una agrupación de todos los recursos que necesita un usuario para trabajar en un proyecto o realizar su trabajo. Los recursos incluyen el acceso a grupos, aplicaciones o sitios. Los paquetes de acceso se utilizan para controlar el acceso de los empleados internos y también de los usuarios de fuera de la organización. Los paquetes de acceso se definen en contenedores llamados *catálogos*.

Los paquetes de acceso también incluyen una o varias *directivas*. Una directiva define las reglas o barreras para acceder a un paquete de acceso. Al habilitar una directiva, se garantiza que solo los usuarios adecuados tengan acceso a los recursos adecuados y durante el período de tiempo adecuado.

![Paquete de acceso y directivas](./media/entitlement-management-overview/elm-overview-access-package.png)

Con un paquete de acceso y sus directivas, el administrador de paquetes de acceso define:

- Recursos
- Roles que los usuarios necesitan para los recursos
- Usuarios internos y organizaciones de partners de usuarios externos que pueden solicitar acceso
- Proceso de aprobación y los usuarios que pueden aprobar o denegar el acceso
- Duración del acceso del usuario

El siguiente diagrama muestra un ejemplo de los diferentes elementos de la administración de derechos. Muestra dos paquetes de acceso de ejemplo.

- El **paquete de acceso 1** incluye un único grupo como un recurso. El acceso se define con una directiva que permite a un conjunto de usuarios del directorio solicitar acceso.
- El **paquete de acceso 2** incluye un grupo, una aplicación y un sitio de SharePoint Online como recursos. El acceso se define con dos directivas diferentes. La primera directiva permite a un conjunto de usuarios del directorio solicitar acceso. La segunda directiva permite a los usuarios de un directorio externo solicitar acceso.

![Introducción a la administración de derechos](./media/entitlement-management-overview/elm-overview.png)

## <a name="external-users"></a>Usuarios externos

Cuando utilice la experiencia de invitación de [Azure AD de empresa a empresa (B2B)](../b2b/what-is-b2b.md), ya debe conocer las direcciones de correo electrónico de los usuarios invitados externos que desea incorporar al directorio de recursos y con los que desea trabajar. Esto funciona muy bien cuando está trabajando en un proyecto más pequeño o a corto plazo y ya conoce a todos los participantes, pero es más difícil de administrar si tiene muchos usuarios con los que quiere trabajar o si los participantes cambian con el tiempo.  Por ejemplo, puede estar trabajando con otra organización y tener un punto de contacto con ella, pero con el tiempo otros usuarios de esa organización también necesitarán acceso.

Con la administración de derechos, puede definir una directiva que permita a los usuarios de las organizaciones que especifique, que también estén usando Azure AD, poder solicitar un paquete de acceso. Puede especificar si se requiere aprobación y una fecha de expiración para el acceso. Si se requiere autorización, también puede designar como aprobador a uno o varios usuarios de la empresa externa a la que ha invitado previamente, ya que es probable que sepan qué usuarios externos de su empresa necesitan acceso. Una vez configurado el paquete de acceso, puede enviar un vínculo al paquete de acceso a la persona de contacto de la organización externa. Ese contacto puede compartirlo con otros usuarios de la organización externa y pueden usar este vínculo para solicitar el paquete de acceso.  Los usuarios de esa organización que ya han recibido la invitación a su directorio también pueden usar ese vínculo.

Cuando se aprueba una solicitud, la administración de derechos proporciona al usuario el acceso necesario, que puede incluir invitar al usuario si aún no está en el directorio. Azure AD creará automáticamente una cuenta de B2B para ellos.  Tenga en cuenta que un administrador puede haber limitado previamente qué organizaciones están permitidas para la colaboración, mediante el establecimiento de una [lista de permitidos y denegados de B2B](../b2b/allow-deny-list.md) para permitir o bloquear las invitaciones a otras organizaciones.  Si el usuario no está autorizado por la lista de permitidos o bloqueados, no se le invitará.

Puesto que no desea que el acceso del usuario externo dure para siempre, especifique una fecha de expiración en la directiva, por ejemplo 180 días. Después de 180 días, si no se renueva el acceso, la administración de derechos eliminará todo el acceso asociado con ese paquete de acceso.  Si el usuario invitado a través de la administración de derechos no tiene ninguna otra asignación de paquetes de acceso, cuando pierda la última asignación, la cuenta de B2B se bloqueará durante 30 días y posteriormente se eliminará.  Esto evita la proliferación de cuentas innecesarias.  

## <a name="terminology"></a>Terminología

Para conocer mejor la administración de derechos y su documentación, debe revisar los términos siguientes.

| Término o concepto | DESCRIPCIÓN |
| --- | --- |
| administración de derechos | Un servicio que asigna, revoca y administra los paquetes de acceso. |
| paquete de acceso | Una colección de permisos y directivas de recursos que los usuarios pueden solicitar. Un paquete de acceso siempre se encuentra en un catálogo. |
| solicitud de acceso | Una solicitud para acceder a un paquete de acceso. Normalmente, una solicitud pasa por un flujo de trabajo. |
| policy | Un conjunto de reglas que define el ciclo de vida del acceso, como por ejemplo, cómo los usuarios obtienen acceso, quién puede aprobarlo y cuánto tiempo tienen acceso. Algunos ejemplos de directivas incluyen el acceso de los empleados y el acceso externo. |
| catálogo | Un contenedor de recursos relacionados y paquetes de acceso. |
| Catálogo general | Un catálogo integrado que siempre está disponible. Para agregar los recursos al catálogo general, se requieren determinados permisos. |
| resource | Un recurso o servicio (por ejemplo, un grupo de Office, un grupo de seguridad, una aplicación o un sitio de SharePoint Online) al que se puede conceder permisos a un usuario. |
| tipo de recurso | El tipo de recurso, que incluye grupos, aplicaciones y sitios de SharePoint Online. |
| rol de recurso | Una colección de permisos asociados a un recurso. |
| directorio de recursos | Un directorio que tiene uno o más recursos para compartir. |
| usuarios asignados | Una asignación de un paquete de acceso a un usuario, de modo que el usuario tenga todos los roles de recursos de ese paquete de acceso. |
| enable | El proceso de poner un paquete de acceso a disposición de los usuarios para que lo soliciten. |

## <a name="license-requirements"></a>Requisitos de licencia

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

Las nubes especializadas, como Azure Government, Azure Alemania y Azure China 21Vianet, no están actualmente disponibles para su uso en esta versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación del primer paquete de acceso](entitlement-management-access-package-first.md)
- [Escenarios comunes](entitlement-management-scenarios.md)
