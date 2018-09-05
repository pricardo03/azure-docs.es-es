---
title: Configuración de Managed Service Identity en una máquina virtual de Azure mediante Azure Portal
description: Instrucciones paso a paso para configurar una identidad de servicio administrada en una máquina virtual de Azure, mediante Azure Portal.
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
ms.date: 09/19/2017
ms.author: daveba
ms.openlocfilehash: 552fce2ffd8b6bd786010da82e702ee98c3f8647
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888563"
---
# <a name="configure-a-vm-managed-service-identity-using-the-azure-portal"></a>Configuración de una identidad de servicio administrada en una máquina virtual con Azure Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a habilitar y deshabilitar la identidad asignada por el usuario y el sistema para una máquina virtual de Azure mediante Azure Portal. 

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración en este artículo, su cuenta debe tener la siguiente asignación de roles:
    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para habilitar y quitar la identidad asignada por el sistema de una máquina virtual de Azure.

## <a name="system-assigned-identity"></a>Identidad asignada por el sistema

En esta sección, aprenderá a habilitar y deshabilitar la identidad asignada por el sistema para una máquina virtual mediante Azure Portal.

### <a name="enable-system-assigned-identity-during-creation-of-a-vm"></a>Habilitación de una identidad asignada por el sistema durante la creación de una máquina virtual

En la actualidad, Azure Portal no permite habilitar una identidad asignada por el sistema durante la creación de una máquina virtual. En su lugar, consulte uno de los artículos de inicio rápido sobre la creación de máquinas virtuales para crear primero una máquina virtual y continúe en la siguiente sección donde encontrará información acerca de cómo habilitar la identidad asignada por el sistema en la máquina virtual:

- [Creación de una máquina virtual Windows desde Azure Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Creación de una máquina virtuales Linux desde Azure Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)  

### <a name="enable-system-assigned-identity-on-an-existing-vm"></a>Habilitación de una identidad asignada por el sistema en una máquina virtual existente

Para habilitar la identidad asignada por el sistema en una máquina virtual que se aprovisionó originalmente sin ella, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.

2. Vaya a la máquina virtual que desee y seleccione la página **Identidad**.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Activado** y haga clic en **Guardar**:

   ![Captura de pantalla de la página de configuración](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-identity-from-a-vm"></a>Eliminación de una identidad asignada por el sistema desde una máquina virtual

Si tiene una máquina virtual que ya no necesita la identidad asignada por el sistema:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual. 

2. Vaya a la máquina virtual que desee y seleccione la página **Identidad**.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Desactivado** y haga clic en **Guardar**:

   ![Captura de pantalla de la página de configuración](../managed-service-identity/media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

 En esta sección, aprenderá a agregar y quitar una identidad asignada por el usuario desde una máquina virtual mediante Azure Portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Asignación de una identidad asignada por el usuario durante la creación de una máquina virtual

En la actualidad, Azure Portal no permite asignar una identidad asignada por el usuario durante la creación de una máquina virtual. En su lugar, consulte uno de los artículos de inicio rápido sobre la creación de máquinas virtuales para crear primero una máquina virtual y continúe en la siguiente sección, donde encontrará información acerca de cómo asignar la identidad asignada por el usuario a la máquina virtual:

- [Creación de una máquina virtual Windows desde Azure Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Creación de una máquina virtuales Linux desde Azure Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-identity-to-an-existing-vm"></a>Asignación de una identidad asignada por el usuario a una máquina virtual existente

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.
2. Vaya a la máquina virtual que desee y haga clic en **Identidad**, **Asignado por el usuario** y en **\+Agregar**.

   ![Incorporación de una identidad asignada por el usuario a una máquina virtual](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Haga clic en la identidad asignada por el usuario que desea agregar a la máquina virtual y en **Agregar**.

    ![Incorporación de una identidad asignada por el usuario a una máquina virtual](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-identity-from-a-vm"></a>Eliminación de una identidad asignada por el usuario de una máquina virtual

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.
2. Vaya a la máquina virtual que desee y haga clic en **Identidad**, en **Asignado por el usuario**, en el nombre de la identidad asignada por el usuario que desea eliminar y en **Quitar** (haga clic en **Sí** en el panel de confirmación).

   ![Eliminación de una identidad asignada por el usuario de una máquina virtual](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="related-content"></a>Contenido relacionado

- Para obtener información sobre Managed Service Identity, consulte [Información general](overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Con Azure Portal, dé a la identidad de servicio administrada de una máquina virtual de Azure [acceso a otro recurso de Azure](howto-assign-access-portal.md).

