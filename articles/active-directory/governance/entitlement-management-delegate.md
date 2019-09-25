---
title: Delegación de tareas en Administración de derechos de Azure AD (versión preliminar) - Azure Active Directory
description: Conozca los roles que puede asignar para delegar tareas de administración de derechos en Azure Active Directory.
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
ms.openlocfilehash: 6857697423e494c515bd052cb42af3ad1d9fe188
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71057790"
---
# <a name="delegate-tasks-in-azure-ad-entitlement-management-preview"></a>Delegación de tareas en Administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

De forma predeterminada, los administradores globales y los administradores de usuarios pueden crear y gestionar todos los aspectos de Administración de derechos de Azure AD. No obstante, es posible que los usuarios de estos roles no conozcan todos los escenarios en los que se requieren paquetes de acceso. Normalmente, son los usuarios de los departamentos los que saben quiénes necesitan colaborar. 

En lugar de conceder permisos sin restricciones a los usuarios que no son administradores, puede conceder a los usuarios los permisos mínimos que necesitan para realizar su trabajo y evitar la creación de derechos de acceso conflictivos o inadecuados. En este artículo se describen los roles que se pueden asignar para delegar diversas tareas de administración de derechos. 

## <a name="delegate-example-for-departmental-adoption"></a>Ejemplo de delegación para la adopción por departamento

Para comprender cómo puede delegar tareas de administración de derechos, resulta útil plantearse un ejemplo. 

Supongamos que su organización tiene los cinco usuarios siguientes:

| Usuario | department | Notas |
| --- | --- | --- |
| Alice | IT | Administrador global |
| Bob | Investigación | Bob también es propietario de un grupo de investigación |
| Carol | Investigación |  |
| Dave | Marketing |  |
| Elisa | Marketing | Elisa también es propietaria de una aplicación de marketing |

Tanto el departamento de investigación como el de marketing quieren utilizar la administración de derechos para sus usuarios. Alice aún no está preparada para que otros departamentos utilicen la administración de derechos. He aquí una forma en que Alice podría delegar tareas a los departamentos de investigación y de marketing.

1. Alice crea un grupo de seguridad de Azure AD para creadores de catálogos y agrega a Bob, Carol, Dave y Elisa como miembros de ese grupo.

1. Alice utiliza la configuración de administración de derechos para añadir ese grupo al rol de creadores de catálogos.

1. Carol crea un catálogo de **investigación** y agrega a Bob como copropietario de dicho catálogo. Bob agrega el grupo de investigación que le pertenece al catálogo como recurso, para que puede usarse en un paquete de acceso para la colaboración en investigación.

1. Dave crea un catálogo de **marketing** y agrega a Elisa como copropietaria de dicho catálogo. Elisa agrega la aplicación de marketing que le pertenece al catálogo como recurso, para que puede usarse en un paquete de acceso para la colaboración en marketing.

Ahora los departamentos de investigación y marketing pueden utilizar la administración de derechos. Bob, Carol, Dave y Elisa pueden crear y administrar paquetes de acceso en sus respectivos catálogos.

![Ejemplo de delegación de administración de derechos](./media/entitlement-management-delegate/elm-delegate.png)


## <a name="entitlement-management-roles"></a>Roles de administración de derechos

La administración de derechos incluye los siguientes roles específicos de administración de derechos.

| Role | DESCRIPCIÓN |
| --- | --- |
| Creador de catálogos | Crear y administrar catálogos. Normalmente, un administrador de TI que no es administrador global o un propietario de recursos en una colección de recursos. La persona que crea un catálogo se convierte automáticamente en el primer propietario del catálogo y puede agregar otros propietarios del catálogo. Un creador de catálogos no puede administrar o ver los catálogos que no son de su propiedad y no puede agregar recursos que no son de su propiedad a un catálogo. Si el creador del catálogo tiene que administrar otro catálogo o agregar recursos que no son de su propiedad, puede solicitar ser copropietario de ese catálogo o recurso. |
| Propietario del catálogo | Editar y administrar catálogos existentes. Normalmente, un administrador de TI o propietarios de recursos, o un usuario que el propietario del catálogo ha designado. |
| Administrador de paquetes de acceso | Editar y administrar todos los paquetes de acceso existentes en un catálogo. |

Además, un aprobador designado y un solicitante de un paquete de acceso también disponen de derechos, aunque no son roles.
 
* Aprobador: Usuario autorizado por una directiva para aprobar o rechazar solicitudes de acceso a paquetes, aunque no puede cambiar las definiciones de paquetes de acceso.
* Solicitante: Usuario autorizado por una directiva de un paquete de acceso para solicitar ese paquete de acceso.

En la tabla siguiente se muestran las tareas que pueden realizar estos roles.

| Tarea | Creador de catálogos | Propietario del catálogo | Administrador de paquetes de acceso | Aprobador |
| --- | :---: | :---: | :---: | :---: |
| [Creación de un catálogo](entitlement-management-catalog-create.md) | :heavy_check_mark: |  |  |  |
| [Adición de un recurso a un catálogo](entitlement-management-catalog-create.md#add-resources-to-a-catalog) | | :heavy_check_mark: | | |
| [Edición de un catálogo](entitlement-management-catalog-create.md#edit-a-catalog) |  | :heavy_check_mark: |  |  |
| [Eliminación de un catálogo](entitlement-management-catalog-create.md#delete-a-catalog) |  | :heavy_check_mark: |  |  |
| [Adición de un propietario del catálogo o de un administrador de paquetes de acceso a un catálogo](#add-a-catalog-owner-or-an-access-package-manager) |  | :heavy_check_mark: |  |  |
| [Creación de un paquete de acceso en un catálogo](entitlement-management-access-package-create.md) |  | :heavy_check_mark:  | :heavy_check_mark:  |  |
| [Administración de roles de recurso en un paquete de acceso](entitlement-management-access-package-edit.md) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Creación y edición de directivas](entitlement-management-access-package-edit.md#add-a-new-policy) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Asignación directa de un usuario a un paquete de acceso](entitlement-management-access-package-edit.md#directly-assign-a-user) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Visualización de quién tiene una asignación en un paquete de acceso](entitlement-management-access-package-edit.md#view-who-has-an-assignment) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Visualización de las solicitudes de un paquete de acceso](entitlement-management-access-package-edit.md#view-requests) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Visualización de errores de entrega de una solicitud](entitlement-management-access-package-edit.md#view-a-requests-delivery-errors) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Cancelación de una solicitud pendiente](entitlement-management-access-package-edit.md#cancel-a-pending-request) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Ocultación de un paquete de acceso](entitlement-management-access-package-edit.md#change-the-hidden-setting) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Eliminación de un paquete de acceso](entitlement-management-access-package-edit.md#delete) |  | :heavy_check_mark: | :heavy_check_mark: |  |
| [Aprobación de una solicitud de acceso](entitlement-management-request-approve.md) |  |  |  | :heavy_check_mark: |

## <a name="required-roles-to-add-resources-to-a-catalog"></a>Roles necesarios para agregar recursos a un catálogo

Un administrador global puede agregar o quitar cualquier grupo (grupos de seguridad creados en la nube o grupos de Office 365 creados en la nube), aplicación o sitio de SharePoint Online en un catálogo. Un usuario administrador puede agregar o quitar cualquier grupo o aplicación en un catálogo.

Para que un usuario que no sea administrador global o administrador de usuarios pueda agregar grupos, aplicaciones o sitios de SharePoint Online a un catálogo, ese usuario debe tener *tanto* el rol de directorio de Azure AD como el rol de administración de derechos de propietario del catálogo que se requieren. En la tabla siguiente se muestran las combinaciones de roles necesarios para agregar recursos a un catálogo. Para quitar recursos de un catálogo, hay que tener los mismos roles.

| Rol de directorio de Azure AD | Rol de administración de derechos | Puede agregar grupos de seguridad | Puede agregar grupos de Office 365 | Puede agregar aplicaciones | Puede agregar sitios de SharePoint Online |
| --- | :---: | :---: | :---: | :---: | :---: |
| [Administrador global](../users-groups-roles/directory-assign-admin-roles.md) | N/D |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| [Administrador de usuarios](../users-groups-roles/directory-assign-admin-roles.md) | N/D |  :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |  |
| [Administrador de Intune](../users-groups-roles/directory-assign-admin-roles.md) | Propietario del catálogo | :heavy_check_mark: | :heavy_check_mark: |  |  |
| [Administrador de Exchange](../users-groups-roles/directory-assign-admin-roles.md) | Propietario del catálogo |  | :heavy_check_mark: |  |  |
| [Administrador del servicio Teams](../users-groups-roles/directory-assign-admin-roles.md) | Propietario del catálogo |  | :heavy_check_mark: |  |  |
| [Administrador de SharePoint](../users-groups-roles/directory-assign-admin-roles.md) | Propietario del catálogo |  | :heavy_check_mark: |  | :heavy_check_mark: |
| [Administrador de aplicaciones](../users-groups-roles/directory-assign-admin-roles.md) | Propietario del catálogo |  |  | :heavy_check_mark: |  |
| [Administrador de aplicaciones en la nube](../users-groups-roles/directory-assign-admin-roles.md) | Propietario del catálogo |  |  | :heavy_check_mark: |  |
| Usuario | Propietario del catálogo | Solo si es propietario del grupo | Solo si es propietario del grupo | Solo si es propietario de la aplicación |  |

Para determinar el rol con menos privilegios de una tarea, también puede hacer referencia a los [roles de administrador por tarea de administrador en Azure Active Directory](../users-groups-roles/roles-delegate-by-task.md#entitlement-management).

## <a name="add-a-catalog-creator"></a>Adición de un creador de catálogos

Si desea delegar la creación del catálogo, agregue usuarios al rol de creador de catálogos.  Puede agregar usuarios individuales o, para mayor comodidad, puede agregar un grupo, cuyos miembros pueden luego crear catálogos. Siga estos pasos para asignar un usuario al rol de creador de catálogos.

**Rol necesario:** administrador global o administrador de usuarios.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú izquierdo, en la sección **Administración de derechos**, haga clic en **Configuración**.

1. Haga clic en **Editar**.

1. En la sección **Delegate entitlement management** (Delegar la administración de derechos), haga clic en **Agregar creador de catálogos** para seleccionar los usuarios o grupos que serán los miembros de este rol de administración de derechos.

1. Haga clic en **Seleccionar**.

1. Haga clic en **Save**(Guardar).

## <a name="add-a-catalog-owner-or-an-access-package-manager"></a>Adición de un propietario del catálogo o de un administrador de paquetes de acceso

Si desea delegar la administración de un catálogo o de paquetes de acceso del catálogo, agregue usuarios a los roles de propietario del catálogo o de administrador de paquetes de acceso. Quien crea un catálogo se convierte en el primer propietario del catálogo. 

El propietario del catálogo asignado o el administrador de paquetes de acceso tienen que estar familiarizados con el proyecto. El creador del catálogo debe crear el paquete de acceso si participa en las operaciones cotidianas del proyecto y conoce la información siguiente:
- Qué recursos se necesitan
- Quién necesitará acceso
- Quién necesita aprobar el acceso
- Cuánto tiempo durará el proyecto

Si no participa en las operaciones diarias del proyecto, el creador del catálogo debe delegar la tarea en el responsable del proyecto, que creará y administrará el paquete de acceso. Siga estos pasos para asignar un usuario al rol de propietario del catálogo o de administrador de paquetes de acceso:

**Rol necesario:** Administrador global, administrador de usuarios o propietario del catálogo.

1. En Azure Portal, haga clic en **Azure Active Directory** y, luego, en **Gobernanza de identidades**.

1. En el menú izquierdo, haga clic en **Catálogos** y abra el catálogo al que quiere agregar administradores.

1. En el menú izquierdo, haga clic en **Roles y administradores**.

1. Haga clic en **Agregar propietarios** o en **Agregar administradores del paquete de acceso** para seleccionar los miembros de estos roles.

1. Haga clic en **Seleccionar** para agregar estos miembros.

## <a name="next-steps"></a>Pasos siguientes

- [Adición de aprobadores](entitlement-management-access-package-edit.md#policy-request)
- [Adición de recursos a un catálogo](entitlement-management-catalog-create.md#add-resources-to-a-catalog)
