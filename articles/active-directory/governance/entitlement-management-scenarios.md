---
title: Escenarios comunes en Administración de derechos de Azure AD (versión preliminar) - Azure Active Directory
description: Obtenga información sobre los pasos de alto nivel que debe seguir para escenarios comunes en Administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 04/23/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81382ebecdff6c7b146386b3ae2b0768a7c834bf
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72389063"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Escenarios comunes en Administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Hay varias formas para configurar la administración de derechos para su organización. Pero, si acaba de empezar, es útil comprender los escenarios comunes para los administradores, aprobadores y solicitantes.

## <a name="administrators"></a>Administradores

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Soy nuevo en la administración de derechos y quiero obtener ayuda para empezar.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | [Seguir el tutorial para crear el primer paquete de acceso](entitlement-management-access-package-first.md) | [![icono de Azure Portal](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Quiero permitir que los usuarios de mi directorio soliciten acceso a grupos, aplicaciones o sitios de SharePoint.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Crear un paquete de acceso en un catálogo](entitlement-management-access-package-create.md#start-new-access-package) | ![Creación de un paquete de acceso](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Agregar roles de recursos al paquete de acceso](entitlement-management-access-package-resources.md#add-resource-roles)<ul><li>Grupos</li><li>APLICACIONES</li><li>Sitios de SharePoint</li></ul> | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Agregar una directiva](entitlement-management-access-package-request-policy.md#for-users-in-your-directory)<ul><li>Para los usuarios del directorio</li><li>Requerir aprobación</li><li>Configuración del ciclo de vida</li></ul> | ![Add policy](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-directory-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Quiero permitir que los usuarios del directorio de socios comerciales (incluidos los usuarios que aún no están en mi directorio) soliciten acceso a grupos, aplicaciones o sitios de SharePoint.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Crear un paquete de acceso en un catálogo](entitlement-management-access-package-create.md#start-new-access-package) | ![Creación de un paquete de acceso](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Agregar roles de recursos al paquete de acceso](entitlement-management-access-package-resources.md#add-resource-roles) | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Agregar una directiva para los usuarios externos](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)<ul><li>Para los usuarios que no están en el directorio</li><li>Requerir aprobación</li><li>Configuración del ciclo de vida</li></ul> | ![Agregar una directiva para los usuarios externos](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [Enviar el vínculo para solicitar un paquete de acceso a su socio comercial](entitlement-management-access-package-settings.md)<ul><li>El socio comercial puede compartir el vínculo con sus usuarios</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Quiero cambiar los grupos, aplicaciones o sitios de SharePoint en un paquete de acceso.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** Abrir el paquete de acceso | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Agregar o quitar roles de recursos](entitlement-management-access-package-resources.md#add-resource-roles) | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Quiero ver quién tiene una asignación a grupos, aplicaciones o sitios de SharePoint.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** Abrir un paquete de acceso | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Ver asignaciones](entitlement-management-access-package-assignments.md#view-who-has-an-assignment)<ul><li>Ver qué usuarios tienen acceso a un paquete de acceso</li><li>Ver qué acceso del usuario ha expirado</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Quiero ver los grupos, aplicaciones o sitios de SharePoint a los que tiene acceso un usuario.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | [Ver el informe de asignaciones de usuario](entitlement-management-reports.md)<ul><li>Ver cuándo hicieron la solicitud y quién la aprobó</li></ul> |  |

## <a name="approvers"></a>Aprobadores

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Quiero aprobar las solicitudes de acceso a grupos, aplicaciones o sitios de SharePoint.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Abrir la solicitud en el portal Mi acceso](entitlement-management-request-approve.md#open-request) | [![Icono del portal Mi acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Aprobar la solicitud de acceso](entitlement-management-request-approve.md#approve-or-deny-request) | ![Aprobar el acceso](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>Solicitantes

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Quiero ver los grupos, aplicaciones o sitios de SharePoint que tengo disponibles y solicitar acceso.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Iniciar sesión en el portal Mi acceso](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Icono del portal Mi acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Buscar el paquete de acceso |  |
> | **3.** [Solicitar acceso](entitlement-management-request-access.md#request-an-access-package) | ![Solicitar acceso](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Soy un usuario externo y quiero solicitar acceso a grupos, aplicaciones o sitios de SharePoint con un vínculo directo.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Busque el vínculo del paquete de acceso que ha recibido](entitlement-management-access-package-settings.md) |  |
> | **2.** [Iniciar sesión en el portal Mi acceso](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Icono del portal Mi acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Solicitar acceso](entitlement-management-request-access.md#request-an-access-package) | ![Solicitud de acceso de usuario externo](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Quiero ver los grupos, aplicaciones o sitios de SharePoint a los que ya tengo acceso.

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Iniciar sesión en el portal Mi acceso](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Icono del portal Mi acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Ver los paquetes de acceso activos |  |

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Creación del primer paquete de acceso](entitlement-management-access-package-first.md)
- [Delegación y roles](entitlement-management-delegate.md)
