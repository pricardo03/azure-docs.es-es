---
title: 'Creación de un paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo crear un paquete de acceso de recursos que quiere compartir en la administración de derechos de Azure Active Directory (versión preliminar).
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
ms.date: 09/24/2019
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1283549ce78535757b7ba42eaf7054538f0c9784
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326340"
---
# <a name="create-a-new-access-package-in-azure-ad-entitlement-management-preview"></a>Creación de un paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> La administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Un paquete de acceso le permite realizar una instalación única de los recursos y las directivas que administran automáticamente el acceso durante toda la vida del paquete de acceso. En este artículo se describe cómo crear un paquete de acceso.

## <a name="overview"></a>Información general

Todos los paquetes de acceso deben colocarse en un contenedor que se conoce como catálogo. Un catálogo define qué recursos puede agregar al paquete acceso. Si no especifica un catálogo, el paquete de acceso se colocará en el catálogo General. Actualmente, no puede mover un paquete de acceso existente a otro catálogo.

Todos los paquetes de acceso deben tener al menos una directiva. Las directivas especifican quién puede solicitar el paquete de acceso y también la configuración de aprobación y expiración. Al crear un paquete de acceso, puede crear una directiva inicial para los usuarios del directorio, para los usuarios que no están en el directorio, solo para asignaciones directas del administrador o puede decidir crear la directiva más adelante.

En el siguiente diagrama se muestra el proceso general para crear un paquete de acceso.

![Proceso de creación de un paquete de acceso](./media/entitlement-management-access-package-create/access-package-process.png)

## <a name="start-new-access-package"></a>Inicio de un nuevo paquete de acceso

**Rol necesario:** administrador global, administrador de usuarios o propietario del catálogo.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Haga clic en **Azure Active Directory** y, luego, haga clic en **Gobernanza de identidades**.

1. En el menú izquierdo, haga clic en **Paquetes de acceso**.

    ![Administración de derechos en Azure Portal](./media/entitlement-management-shared/elm-access-packages.png)

1. Haga clic en **New access package** (Nuevo paquete acceso).

## <a name="basics"></a>Aspectos básicos

En la pestaña **Aspectos básicos**, asigne un nombre al paquete de acceso y especifique en qué catálogo quiere crear el paquete de acceso.

1. Escriba un nombre para mostrar y una descripción para el paquete de acceso. Los usuarios verán esta información al enviar una solicitud al paquete de acceso.

1. En la lista desplegable **Catálogo**, seleccione el catálogo en el que quiera crear el paquete de acceso. Por ejemplo, puede que tenga un propietario de catálogo que administra todos los recursos de marketing que se pueden solicitar. En este caso, puede seleccionar el catálogo de marketing.

    Solo verá los catálogos en los que tenga permiso para crear paquetes de acceso. Para crear el paquete de acceso en un catálogo existente, debe ser al menos administrador global, administrador de usuarios, propietario del catálogo o administrador de paquetes de acceso en ese catálogo.

    ![Paquete de acceso: aspectos básicos](./media/entitlement-management-access-package-create/basics.png)

    Si es administrador global o administrador de usuarios y quiere crear el paquete de acceso en un catálogo nuevo que no aparece en la lista, haga clic en **Crear nuevo**. Escriba el nombre del catálogo y la descripción y después haga clic en **Crear**.

    El paquete de acceso que está creando y los recursos incluidos en él se agregarán al nuevo catálogo. También puede agregar a otros propietarios del catálogo.


1. Haga clic en **Next**.

## <a name="resource-roles"></a>Roles de recursos

En la pestaña **Roles de recurso**, se seleccionan los recursos que se incluirán en el paquete de acceso.  Los usuarios que soliciten y reciban el paquete de acceso recibirán todos los roles de recursos del paquete de acceso.

1. Haga clic en el tipo de recurso que quiera agregar (**Grupos y equipos**, **Aplicaciones** o **Sitios de SharePoint**).

1. En el panel de selección que aparece, seleccione uno o varios recursos de la lista.

    ![Paquete de acceso: roles de recurso](./media/entitlement-management-access-package-create/resource-roles.png)

    Si está creando el paquete de acceso en el catálogo General o un catálogo nuevo, podrá elegir cualquier recurso del directorio que posee. Debe ser al menos administrador global, administrador de usuarios o creador de catálogos.

    Si está creando el paquete de acceso en un catálogo existente, puede seleccionar cualquier recurso que ya esté en el catálogo sin que sea su propietario.

    Si es administrador global, administrador de usuarios o el propietario del catálogo, tiene la opción adicional de seleccionar los recursos que posee y que aún no están en el catálogo. Si selecciona los recursos que no están actualmente en el catálogo seleccionado, estos también se agregarán al catálogo para que otros administradores de catálogos compilen paquetes de acceso con él. Si solo quiere seleccionar recursos que estén actualmente en el catálogo seleccionado, marque la casilla **Only see** (Solo ver) situada en la parte superior del panel de selección.

1. Una vez que haya seleccionado los recursos, en la lista **Roles**, seleccione el rol que quiere que se les asigne a los usuarios para el recurso.

    ![Paquete de acceso: selección del rol del recurso](./media/entitlement-management-access-package-create/resource-roles-role.png)

1. Haga clic en **Next**.

## <a name="policy"></a>Directiva

En la pestaña **Directiva**, creará la primera directiva para especificar quién puede solicitar el paquete de acceso y también la configuración de aprobación y expiración. Más adelante, puede crear más directivas para permitir a otros grupos de usuarios que soliciten el paquete de acceso con su propia configuración de aprobación y expiración. También puede decidir crear la directiva más adelante.

1. Establezca el botón de alternancia **Crear la primera directiva** en **Ahora** o **Más tarde**.

    ![Paquete de acceso: directiva](./media/entitlement-management-access-package-create/policy.png)

1. Si selecciona **Más tarde**, pase a la sección [Revisar y crear](#review--create) para crear el paquete de acceso.

1. Si selecciona **Ahora**, realice los pasos en una de las siguientes secciones de la directiva.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="review--create"></a>Revisar y crear

En la pestaña **Revisar y crear**, puede revisar la configuración y comprobar si hay errores de validación.

1. Revise la configuración del paquete de acceso.

    ![Paquete de acceso: directiva; configuración de habilitación de la directiva](./media/entitlement-management-access-package-create/review-create.png)

1. Haga clic en **Crear** para crear el paquete de acceso.

    El nuevo paquete de acceso aparece en la lista de paquetes de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Edición y administración de un paquete de acceso existente](entitlement-management-access-package-edit.md)
- [Adición de un propietario del catálogo o de un administrador de paquetes de acceso](entitlement-management-delegate.md#add-a-catalog-owner-or-an-access-package-manager)
- [Creación y administración de un catálogo](entitlement-management-catalog-create.md)
