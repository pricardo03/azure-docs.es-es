---
title: Visualización de la entidad de servicio de una identidad administrada mediante PowerShell
description: Instrucciones detalladas para ver la entidad de servicio de una identidad administrada mediante PowerShell.
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
ms.date: 11/29/2018
ms.author: daveba
ms.openlocfilehash: 0ad3a52b837a5f79c9976c4c509e0a8516de1e7d
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714124"
---
# <a name="view-the-service-principal-of-a-managed-identity-using-powershell"></a>Visualización de la entidad de servicio de una identidad administrada mediante PowerShell

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, obtendrá información sobre cómo visualizar la entidad de servicio de una identidad administrada mediante PowerShell.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md).
- Si aún no tiene una cuenta de Azure, [regístrese para obtener una cuenta gratuita](https://azure.microsoft.com/free/).
- Habilite la [identidad asignada por el sistema en una máquina virtual](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) o [aplicación](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).
- Si decide instalar y usar PowerShell de forma local, en este tutorial necesitará la versión 5.7.0 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps). 
- Si ejecuta PowerShell localmente, también necesita realizar las siguientes operaciones: 
    - Ejecute `Login-AzureRmAccount` para crear una conexión con Azure.
    - Instalar la [versión más reciente de PowerShellGet](/powershell/gallery/installing-psget#for-systems-with-powershell-50-or-newer-you-can-install-the-latest-powershellget).
    - Ejecutar `Install-Module -Name PowerShellGet -AllowPrerelease` para obtener la versión preliminar del módulo `PowerShellGet` (es posible que deba usar `Exit` para salir de la sesión actual de PowerShell después de ejecutar este comando para instalar el módulo `AzureRM.ManagedServiceIdentity`).
    - Ejecute `Install-Module -Name AzureRM.ManagedServiceIdentity -AllowPrerelease` para instalar la versión preliminar del módulo `AzureRM.ManagedServiceIdentity` con el fin de realizar las operaciones de identidad administrada que haya asignado el usuario, y que se indican en este artículo.

## <a name="view-the-service-principal"></a>Visualización de la entidad de servicio

El comando siguiente muestra cómo ver la entidad de servicio de una aplicación o máquina virtual con la identidad asignada por el sistema habilitada. Reemplazar `<VM or application name>` por sus propios valores.

```PowerShell
Get-AzureRmADServicePrincipal -DisplayName <VM or application name>
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo ver las entidades de servicio de Azure AD mediante PowerShell, vea [Get-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/get-azurermadserviceprincipal).


