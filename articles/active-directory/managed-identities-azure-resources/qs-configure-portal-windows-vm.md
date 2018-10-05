---
title: Cómo configurar identidades administradas para recursos de Azure en una VM de Azure mediante Azure Portal
description: Instrucciones detalladas para configurar identidades administradas para recursos de Azure en una VM de Azure mediante Azure Portal.
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
ms.openlocfilehash: b73a79676be559ad491bd7bb16691369dd8fa271
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47158638"
---
# <a name="configure-managed-identities-for-azure-resources-on-a-vm-using-the-azure-portal"></a>Configurar identidades administradas para recursos de Azure en una VM mediante Azure Portal

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a habilitar y deshabilitar las identidades administradas asignadas por el usuario y el sistema para una máquina virtual (VM) de Azure mediante Azure Portal. 

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para realizar las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de control de acceso basado en rol:

    > [!NOTE]
    > No se requieren asignaciones de roles del directorio de Azure AD.

    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para habilitar y quitar la identidad administrada asignada por el sistema de una VM de Azure.

## <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

En esta sección, aprenderá a habilitar y deshabilitar la identidad administrada asignada por el sistema para una VM mediante Azure Portal.

### <a name="enable-system-assigned-managed-identity-during-creation-of-a-vm"></a>Habilitación de una identidad administrada asignada por el sistema durante la creación de una VM

Para habilitar la identidad administrada asignada por el sistema durante la creación de una máquina virtual, en la pestaña **Administración**, en la sección **Identidad** cambie **Identidad de servicio administrada** a **Activada**.  

![Habilitación de una identidad asignada por el sistema durante la creación de una máquina virtual](./media/msi-qs-configure-portal-windows-vm/enable-system-assigned-identity-vm-creation.png)

Consulte las siguientes guías de inicio rápido para crear una máquina virtual: 

- [Creación de una máquina virtual Windows desde Azure Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine) 
- [Creación de una máquina virtuales Linux desde Azure Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)


### <a name="enable-system-assigned-managed-identity-on-an-existing-vm"></a>Habilitación de una identidad administrada asignada por el sistema en una VM existente

Para habilitar la identidad administrada asignada por el sistema en una VM que se aprovisionó originalmente sin ella, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.

2. Vaya a la máquina virtual que desee y seleccione la página **Identidad**.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Activado** y haga clic en **Guardar**:

   ![Captura de pantalla de la página de configuración](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade.png)  

### <a name="remove-system-assigned-managed-identity-from-a-vm"></a>Quitar una identidad administrada asignada por el sistema de una VM

Si tiene una máquina virtual que ya no necesita la identidad administrada asignada por el sistema:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual. 

2. Vaya a la máquina virtual que desee y seleccione la página **Identidad**.

3. En **Asignado por el sistema**, en **Estado**, seleccione **Desactivado** y haga clic en **Guardar**:

   ![Captura de pantalla de la página de configuración](./media/msi-qs-configure-portal-windows-vm/create-windows-vm-portal-configuration-blade-disable.png)

## <a name="user-assigned-managed-identity"></a>Identidad administrada asignada por el usuario

 En esta sección, aprenderá a agregar y quitar una identidad administrada asignada por el usuario desde una VM mediante Azure Portal.

### <a name="assign-a-user-assigned-identity-during-the-creation-of-a-vm"></a>Asignación de una identidad asignada por el usuario durante la creación de una VM

En la actualidad, Azure Portal no permite asignar una identidad administrada asignada por el usuario durante la creación de una VM. En su lugar, consulte uno de los artículos de inicio rápido sobre la creación de VM para crear primero una VM y continúe en la siguiente sección, donde encontrará información acerca de cómo asignar la identidad administrada asignada por el usuario a la VM:

- [Creación de una máquina virtual Windows desde Azure Portal](../../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Creación de una máquina virtuales Linux desde Azure Portal](../../virtual-machines/linux/quick-create-portal.md#create-virtual-machine)

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-vm"></a>Asignación de una identidad administrada asignada por el usuario a una VM existente

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.
2. Vaya a la máquina virtual que desee y haga clic en **Identidad**, **Asignado por el usuario** y en **\+Agregar**.

   ![Agregar identidad administrada asignada por el usuario a una VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot1.png)

3. Haga clic en la identidad asignada por el usuario que quiera agregar a la VM y, a continuación, en **Agregar**.

    ![Agregar identidad administrada asignada por el usuario a una VM](./media/msi-qs-configure-portal-windows-vm/add-user-assigned-identity-vm-screenshot2.png)

### <a name="remove-a-user-assigned-managed-identity-from-a-vm"></a>Eliminación de una identidad administrada asignada por el usuario de una VM

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene la máquina virtual.
2. Vaya a la VM que quiera y haga clic en **Identidad**, en **Asignado por el usuario**, en el nombre de la identidad administrada asignada por el usuario que quiera eliminar y en **Quitar** (haga clic en **Sí** en el panel de confirmación).

   ![Eliminación de una identidad asignada por el usuario de una VM](./media/msi-qs-configure-portal-windows-vm/remove-user-assigned-identity-vm-screenshot.png)

## <a name="next-steps"></a>Pasos siguientes

- Con Azure Portal, dé a la identidad administrada de una VM de Azure [acceso a otro recurso de Azure](howto-assign-access-portal.md).

