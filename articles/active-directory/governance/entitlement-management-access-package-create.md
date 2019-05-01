---
title: 'Crear un nuevo paquete de acceso en la administración de derechos de Azure AD (versión preliminar): Azure Active Directory'
description: Obtenga información sobre cómo crear un nuevo paquete de acceso de recursos que desea compartir en la administración de derechos de Azure Active Directory (versión preliminar).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/24/2019
ms.author: rolyon
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4ad6570a3f30e40e4074502a8ce85bf739f58d3f
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866447"
---
# <a name="create-a-new-access-package-in-azure-ad-entitlement-management-preview"></a>Crear un nuevo paquete de acceso en la administración de derechos de Azure AD (versión preliminar)

> [!IMPORTANT]
> Administración de derechos de Azure Active Directory (Azure AD) está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Un paquete de acceso le permite realizar una instalación única de los recursos y las directivas que administra automáticamente acceso durante la vida del paquete de acceso. En este artículo se describe cómo crear un nuevo paquete de acceso.

## <a name="overview"></a>Información general

Todos los paquetes de acceso deben colocarse en un contenedor denominado catálogo. Un catálogo define qué recursos puede agregar al paquete acceso. Si no se especifica un catálogo, se colocará el paquete de acceso en el catálogo General. Actualmente, no se puede mover un paquete existente de acceso a un catálogo distinto.

Todos los paquetes de acceso deben tener al menos una directiva. Las directivas especifican que pueden solicitar también el paquete de acceso del sistema y la configuración de aprobación y expiración. Cuando se crea un nuevo paquete de acceso, puede crear una directiva inicial para los usuarios en el directorio, para los usuarios de su directorio, por sólo, asignaciones directas de administrador o puede elegir crear la directiva más adelante.

El siguiente diagrama muestra el proceso de alto nivel para crear un nuevo paquete de acceso.

![Crear un proceso de acceso de paquetes](./media/entitlement-management-access-package-create/access-package-process.png)

## <a name="start-new-access-package"></a>Iniciar el nuevo paquete de acceso

**Función de requisitos previos:** Usuario administrador o propietario del catálogo

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Haga clic en **Azure Active Directory** y, a continuación, haga clic en **gobierno de identidades**.

1. En el menú izquierdo, haga clic en **tener acceso a los paquetes**.

    ![Administración de derechos en el portal de Azure](./media/entitlement-management-shared/elm-access-packages.png)

1. Haga clic en **nuevo paquete acceso**.

## <a name="basics"></a>Aspectos básicos

En el **Fundamentos** ficha, asigne un nombre a los paquetes de acceso y especificar qué catálogo para crear el paquete de acceso en.

1. Escriba un nombre para mostrar y una descripción para el paquete de acceso. Los usuarios verán esta información al enviar una solicitud para el paquete de acceso.

1. En el **catálogo** menú desplegable, seleccione el catálogo que desea crear el acceso de paquetes en. Por ejemplo, podría tener un propietario del catálogo que administra todos los recursos de marketing que se pueden solicitar. En este caso, puede seleccionar el catálogo de marketing.

    Solo verá los catálogos tiene permiso para crear paquetes de acceso en. Para crear el paquete de acceso en un catálogo existente, debe ser al menos un usuario administrador, el propietario del catálogo o el Administrador de paquetes de acceso.

    ![Paquete de acceso: conceptos básicos](./media/entitlement-management-access-package-create/basics.png)

    Si desea crear el paquete de acceso en un nuevo catálogo, haga clic en **crear nuevo**. Escriba el nombre del catálogo y la descripción y, a continuación, haga clic en **crear**.

    El paquete de acceso que está creando y los recursos incluidos en el se agregarán al nuevo catálogo. Además, automáticamente se convertirá en el primer propietario del catálogo. Puede agregar los propietarios de catálogo adicionales.

    Para crear un nuevo catálogo, debe ser al menos un usuario administrador o creador del catálogo.

1. Haga clic en **Next**.

## <a name="resource-roles"></a>Roles de recursos

En el **roles de recursos** ficha, seleccione los recursos que se va a incluir en el paquete de acceso.

1. Haga clic en el tipo de recurso que desea agregar (**grupos**, **aplicaciones**, o **sitios de SharePoint**).

1. En el panel de selección que aparece, seleccione uno o varios recursos de la lista.

    ![Paquete de acceso - roles de recursos](./media/entitlement-management-access-package-create/resource-roles.png)

    Si va a crear el paquete de acceso en el catálogo General o un nuevo catálogo, podrá elegir cualquier recurso desde el directorio que posee. Debe ser al menos un usuario administrador o creador de catálogo.

    Si va a crear el paquete de acceso en un catálogo existente, puede seleccionar cualquier recurso que ya está en el catálogo sin que sea su propietaria.

    Si es un usuario administrador o propietario del catálogo, tiene la opción adicional de los recursos que posee y que aún no están en el catálogo. Si selecciona los recursos no actualmente en el catálogo seleccionado, estos recursos también se agregarán al catálogo de otros administradores de catálogo generar paquetes de acceso con. Si desea seleccionar los recursos que están actualmente en el catálogo seleccionado, compruebe la **solo ver** casilla de verificación en la parte superior de la panorámica Select.

1. Una vez haya seleccionado los recursos, en el **rol** lista, seleccione el rol que desea que los usuarios que se asignará para el recurso.

    ![Paquete de acceso - selección de rol de recursos](./media/entitlement-management-access-package-create/resource-roles-role.png)

1. Haga clic en **Next**.

## <a name="policy"></a>Directiva

En el **directiva** ficha, crea la primera directiva para especificar quién puede solicitar también el paquete de acceso del sistema y la configuración de aprobación y expiración. Más adelante, puede crear directivas más para permitir a otros grupos de usuarios para solicitar el paquete de acceso con su propia configuración de aprobación y expiración. También puede crear la directiva más adelante.

1. Establecer el **crear la primera directiva** alternar a **ahora** o **Later**.

    ![Package - directiva de acceso](./media/entitlement-management-access-package-create/policy.png)

1. Si selecciona **Later**, pasar a la [revisión + crear](#review--create) sección para crear el paquete de acceso.

1. Si selecciona **ahora**, realice los pasos de una de las siguientes secciones de la directiva.

[!INCLUDE [Entitlement management policy](../../../includes/active-directory-entitlement-management-policy.md)]

## <a name="review--create"></a>Revisar y crear

En el **revisar + crear** ficha, puede revisar la configuración y compruebe si hay errores de validación.

1. Revise la configuración del paquete de acceso

    ![Paquete de acceso - habilitar Directiva de configuración de directiva](./media/entitlement-management-access-package-create/review-create.png)

1. Haga clic en **crear** para crear el paquete de acceso.

    El nuevo paquete de acceso aparece en la lista de paquetes de acceso.

## <a name="next-steps"></a>Pasos siguientes

- [Editar y administrar un paquete de acceso existente](entitlement-management-access-package-edit.md)
- [Crear y administrar un catálogo](entitlement-management-catalog-create.md)
