---
title: Configuración de Managed Service Identity en un conjunto de escalado de máquinas virtuales de Azure mediante Azure Portal
description: Instrucciones paso a paso para configurar una identidad de Managed Service Identity en un conjunto de escalado de máquinas virtuales de Azure mediante Azure Portal.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/20/2018
ms.author: daveba
ms.openlocfilehash: cbe2e3d9f60ced5c707ce5a701a5aac937ccc072
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42887996"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-using-the-azure-portal"></a>Configuración de una identidad de Managed Service Identity en un conjunto de escalado de máquinas virtuales mediante Azure Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo aprenderá a habilitar y deshabilitar la identidad asignada por el sistema y por el usuario para un conjunto de escalado de máquinas virtuales mediante Azure Portal.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración en este artículo, su cuenta debe tener la siguiente asignación de roles:
    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para habilitar y quitar la identidad administrada asignada por el sistema desde un conjunto de escalado de máquinas virtuales.

## <a name="system-assigned-identity"></a>Identidad asignada por el sistema 

En esta sección aprenderá a habilitar y deshabilitar la identidad asignada por el sistema en un conjunto de escalado de máquinas virtuales mediante Azure Portal.

### <a name="enable-system-assigned-identity-during-creation-of-a-virtual-machine-scale-set"></a>Habilitación de la identidad asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales

En la actualidad Azure Portal no permite habilitar una identidad asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales. En su lugar, consulte la siguiente guía de inicio rápido sobre la creación de conjuntos de escalado de máquinas virtuales para crear uno primero y, después, continúe a la siguiente sección para detalles sobre la habilitación de la identidad asignada por el sistema en un conjunto de escalado de máquinas virtuales:

- [Creación de un conjunto de escalado de máquinas virtuales en Azure Portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

### <a name="enable-system-assigned-identity-on-an-existing-virtual-machine-scale-set"></a>Habilitación de la identidad asignada en un conjunto de escalado de máquinas virtuales existente

Para habilitar la identidad asignada por el sistema en un conjunto de escalado de máquinas virtuales que se aprovisionó originalmente sin ella, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales.

2. Vaya al conjunto de escalado de máquinas virtuales de Azure que desee.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Activado** y haga clic en **Guardar**:

   [![Captura de pantalla de la página de configuración](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

### <a name="remove-system-assigned-identity-from-a-virtual-machine-scale-set"></a>Eliminación de una identidad asignada por el sistema desde un conjunto de escalado de máquinas virtuales

Si tiene un conjunto de escalado de máquinas virtuales que ya no necesita una identidad de servicio administrada:asignada por el sistema, haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le concede permisos de escritura en el conjunto de escalado de máquinas virtuales.

2. Vaya al conjunto de escalado de máquinas virtuales de Azure que desee.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Desactivado** y haga clic en **Guardar**:

   ![Captura de pantalla de la página de configuración](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

En esta sección aprenderá a agregar una identidad asignada por el usuario a un conjunto de escalado de máquinas virtuales y a quitarla mediante Azure Portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-virtual-machine-scale-set"></a>Asignación de una identidad asignada por el usuario durante la creación de un conjunto de escalado de máquinas virtuales

En la actualidad Azure Portal no permite las identidades asignadas por el usuario durante la creación de un conjunto de escalado de máquinas virtuales. En su lugar, consulte la siguiente guía de inicio rápido sobre la creación de conjuntos de escalado de máquinas virtuales para crear uno primero y, después, continúe a la siguiente sección para detalles sobre cómo el usuario puede asignarle una identidad:

- [Creación de un conjunto de escalado de máquinas virtuales en Azure Portal](../../virtual-machine-scale-sets/quick-create-portal.md)

### <a name="assign-a-user-assigned-identity-to-an-existing-virtual-machine-scale-set"></a>Asignar una identidad que asignó el usuario a un conjunto de escalado de máquinas virtuales existente

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales.
2. Vaya al conjunto de escalado de máquinas virtuales que desee y haga clic en **Identidad**, **Asignado por el usuario** y en **\+Agregar**.

   ![Incorporación de una identidad asignada por el usuario a una máquina virtual conjunto de escalado de máquinas virtuales](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vmss-screenshot1.png)

3. Haga clic en la identidad asignada por el usuario que desea agregar al conjunto de escalado de máquinas virtuales y en **Agregar**.
   
   ![Incorporación de una identidad asignada por el usuario a una máquina virtual conjunto de escalado de máquinas virtuales](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-virtual-machine-scale-set"></a>Eliminación de una identidad asignada por un usuario desde un conjunto de escalado de máquinas virtuales

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.
2. Vaya al conjunto de escalado de máquinas virtuales que desee y haga clic en **Identidad**, en **Asignado por el usuario**, en el nombre de la identidad asignada por el usuario que desea eliminar y en **Quitar** (haga clic en **Sí** en el panel de confirmación).

   ![Eliminación de una identidad asignada por el usuario de un conjunto de escalado de máquinas virtuales](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vmss-screenshot.png)


## <a name="related-content"></a>Contenido relacionado

- Para obtener información sobre Managed Service Identity, consulte [Información general](overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Desde Azure Portal, asigne a una identidad de Managed Service Identity de un conjunto de escalado de máquinas virtuales de Azure [acceso a otro recurso de Azure](howto-assign-access-portal.md).

Use la siguiente sección de comentarios para proporcionar sus opiniones y ayudarnos a afinar y remodelar el contenido.
