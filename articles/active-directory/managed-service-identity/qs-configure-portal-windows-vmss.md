---
title: Configuración de MSI en un conjunto de escalado de máquinas virtuales de Azure con Azure Portal
description: Instrucciones paso a paso para configurar una identidad de servicio administrada (MSI) en un conjunto de escalado de máquinas virtuales de Azure mediante Azure Portal.
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
ms.openlocfilehash: 8779600f2c85a8bb309f7b2a8874608170de8877
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39035248"
---
# <a name="configure-a-virtual-machine-scale-set-managed-service-identity-msi-using-the-azure-portal"></a>Configuración de una instancia de Managed Service Identity (MSI) en un conjunto de escalado de máquinas virtuales mediante Azure Portal

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a habilitar y deshabilitar la identidad asignada por el sistema para un conjunto de escalado de máquinas virtuales, mediante Azure Portal. La asignación y eliminación de identidades asignadas por el usuario desde un conjunto de escalado de máquinas virtuales de Azure no se admiten actualmente mediante Azure Portal.

> [!NOTE]
> Actualmente, las operaciones de identidades asignadas por el usuario no se admiten mediante Azure Portal. Compruebe si hay actualizaciones.

## <a name="prerequisites"></a>Requisitos previos


- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.

## <a name="managed-service-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Managed Service Identity durante la creación de un conjunto de escalado de máquinas virtuales de Azure

Actualmente, la creación de máquinas virtuales mediante Azure Portal no admite operaciones de Managed Service Identity. En su lugar, consulte la siguiente guía de inicio rápido sobre creación de un conjunto de escalado de máquinas virtuales de Azure para crear primero uno de ellos:

- [Creación de un conjunto de escalado de máquinas virtuales en Azure Portal](../../virtual-machine-scale-sets/quick-create-portal.md)  

A continuación, pase a la siguiente sección, donde encontrará información detallada acerca de la habilitación de MSI en el conjunto de escalado de máquinas virtuales.

## <a name="enable-managed-service-identity-on-an-existing-azure-vmms"></a>Habilitación de Managed Service Identity en un VMMS existente de Azure

Para habilitar la identidad asignada por el sistema en una máquina virtual que se aprovisionó originalmente sin ella, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales.

2. Vaya al conjunto de escalado de máquinas virtuales de Azure que desee.

3. Para habilitar la identidad asignada por el sistema en la máquina virtual, seleccione "Sí" en "Managed Service Identity" y, luego, haga clic en **Guardar**. Esta operación puede tardar 60 segundos o más en completarse:

   [![Captura de pantalla de la página de configuración](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png)](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/create-windows-vmss-portal-configuration-blade.png#lightbox)  

## <a name="remove-managed-service-identity-from-an-azure-virtual-machine-scale-set"></a>Eliminación de Managed Service Identity de un conjunto de escalado de máquinas virtuales de Azure

Si tiene un conjunto de escalado de máquinas virtuales de Azure que ya no necesita una identidad de servicio administrada:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta asociada a la suscripción de Azure que contiene el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le concede permisos de escritura en el conjunto de escalado de máquinas virtuales.

2. Vaya al conjunto de escalado de máquinas virtuales de Azure que desee.

3. Para deshabilitar la identidad asignada por el sistema en la máquina virtual, seleccione "No" en "Managed Service Identity" y, luego, haga clic en Guardar. Esta operación puede tardar 60 segundos o más en completarse:

   ![Captura de pantalla de la página de configuración](../managed-service-identity/media/msi-qs-configure-portal-windows-vmss/disable-windows-vmss-portal-configuration-blade.png)  

## <a name="related-content"></a>Contenido relacionado

- Para obtener información sobre Managed Service Identity, consulte [Información general](overview.md).

## <a name="next-steps"></a>Pasos siguientes

- Mediante Azure Portal, asigne a una identidad de servicio administrada de un conjunto de escalado de máquinas virtuales de Azure [acceso a otro recurso de Azure](howto-assign-access-portal.md).

Use la siguiente sección de comentarios para proporcionar sus opiniones y ayudarnos a afinar y remodelar el contenido.
