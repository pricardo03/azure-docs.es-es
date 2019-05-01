---
title: 'Escenarios comunes de administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre los pasos generales que debe seguir para escenarios comunes de administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 04/23/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96442a9d49581da6841fa7acb8329354ec727f60
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918471"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Escenarios comunes de administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Hay varias formas que puede configurar la administración de derechos para su organización. Sin embargo, si acaba de empezar, es útil comprender los escenarios comunes para los administradores, los aprobadores y los solicitantes.

## <a name="administrators"></a>Administradores

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Soy nuevo en la administración de derechos y quiero obtener ayuda

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | [Siga el tutorial para crear el primer paquete de acceso](entitlement-management-access-package-first.md) | [![Icono de portal de Azure](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Quiero permitir que a los usuarios en mi directorio para solicitar acceso a grupos, aplicaciones o sitios de SharePoint

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Crear un nuevo paquete de acceso en un catálogo](entitlement-management-access-package-create.md#start-new-access-package) | ![Crear un paquete de acceso](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Agregar roles de recursos para acceder al paquete](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>Grupos</li><li>APLICACIONES</li><li>Sitios de SharePoint</li></ul> | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Agregar una directiva](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>Para los usuarios del directorio</li><li>Requerir aprobación</li><li>Configuración de expiración</li></ul> | ![Add policy](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Quiero permitir que a los usuarios de mi socios comerciales (incluidos los usuarios aún no en mi directorio) para solicitar acceso a grupos, aplicaciones o sitios de SharePoint

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Crear un nuevo paquete de acceso en un catálogo](entitlement-management-access-package-create.md#start-new-access-package) | ![Crear un paquete de acceso](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Agregar roles de recursos para acceder al paquete](entitlement-management-access-package-edit.md#add-resource-roles) | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Agregar una directiva para los usuarios externos](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>Para los usuarios no en el directorio</li><li>Requerir aprobación</li><li>Configuración de expiración</li></ul> | ![Agregar directiva para los usuarios externos](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [Envíe el vínculo del portal de mi acceso para solicitar el paquete de acceso a su socio comercial](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>Socio de negocio puede compartir el vínculo con sus usuarios</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Deseo cambiar los grupos, aplicaciones o sitios de SharePoint en un paquete de acceso

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** Abra el paquete de acceso | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Agregar o quitar roles de recursos](entitlement-management-access-package-edit.md#add-resource-roles) | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Deseo ver quién tiene una asignación a grupos, aplicaciones o sitios de SharePoint

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** Abrir un paquete de acceso | ![Agregar roles de recursos](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Visualización de asignaciones](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>Ver qué usuarios tienen acceso a un paquete de acceso</li><li>Vista de acceso del usuario que ha expirado</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Deseo ver grupos, aplicaciones o un usuario tiene acceso a los sitios de SharePoint

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | [Ver informe de asignaciones de usuario](entitlement-management-reports.md)<ul><li>Vista cuando soliciten y quién aprueba</li></ul> |  |

## <a name="approvers"></a>Aprobadores

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Deseo aprobar solicitudes de acceso a grupos, aplicaciones o sitios de SharePoint

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Solicitud de apertura en el portal de mi acceso](entitlement-management-request-approve.md#open-request) | [![Icono Mi portal de acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Aprobar solicitud de acceso](entitlement-management-request-approve.md#approve-or-deny-request) | ![Aprobar acceso](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>Solicitantes

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Deseo ver los grupos, aplicaciones o sitios Web de SharePoint disponibles para mí y solicitar acceso

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Inicie sesión en el portal de mi acceso](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Icono Mi portal de acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Busque el paquete de acceso |  |
> | **3.** [Solicitar acceso](entitlement-management-request-access.md#request-an-access-package) | ![Solicitar acceso](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Soy un usuario externo y deseo solicitar acceso a grupos, aplicaciones o sitios de SharePoint con un vínculo directo

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Busque el vínculo de acceso a Mi portal recibió](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [Inicie sesión en el portal de mi acceso](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Icono Mi portal de acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Solicitar acceso](entitlement-management-request-access.md#request-an-access-package) | ![Solicitud de acceso de usuario externo](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Deseo ver los grupos, aplicaciones o sitios de SharePoint a que ya tienen acceso

> [!div class="mx-tableFixed"]
> | Pasos | Ejemplo |
> | --- | --- |
> | **1.** [Inicie sesión en el portal de mi acceso](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Icono Mi portal de acceso](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Ver los paquetes de acceso activa |  |

## <a name="next-steps"></a>Pasos siguientes

- [Tutorial: Crear el primer paquete de acceso](entitlement-management-access-package-first.md)
- [Editar y administrar un paquete de acceso existente](entitlement-management-access-package-edit.md)
