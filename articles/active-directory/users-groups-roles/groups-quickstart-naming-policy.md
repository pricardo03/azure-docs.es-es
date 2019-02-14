---
title: Inicio rápido para agregar la directiva de nomenclatura para grupos de Office 365 en Azure Active Directory | Microsoft Docs
description: Describe como agregar usuarios nuevos o eliminar usuarios existentes en Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9905e3d8b2e0760fca82e9d2c0bfb0164d8fc9fa
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210431"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Guía de inicio rápido: Directiva de nomenclatura para grupos de Azure Active Directory

En esta guía de inicio rápido, configurará una directiva de nomenclatura en el inquilino de Azure Active Directory (Azure AD) para los grupos creados por el usuario de Office 365, para ayudarle a ordenar y buscar los grupos del inquilino. Por ejemplo, podría usar la directiva de nomenclatura para:

* Comunicar la función de un grupo, la pertenencia, la región geográfica o quién creó el grupo.
* Ayudar a clasificar los grupos en la libreta de direcciones.
* Impedir que determinadas palabras se usen en los nombres y alias de grupo.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="install-powershell-cmdlets"></a>Instalación de los cmdlets de PowerShell

Asegúrese de desinstalar cualquier versión anterior del módulo Azure Active Directory PowerShell for Graph para Windows PowerShell y de instalar [Azure Active Directory PowerShell for Graph - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137) antes de ejecutar los comandos de PowerShell. 

1. Abra la aplicación Windows PowerShell como administrador.
2. Desinstale cualquier versión anterior de AzureADPreview.
  
  ```
  Uninstall-Module AzureADPreview
  ```
3. Instale la versión más reciente de AzureADPreview.
  
  ```
  Install-Module AzureADPreview
  ```
Si se le pregunta sobre si desea acceder a un repositorio en el que no se confía, escriba **S**. El nuevo módulo puede tardar varios minutos en instalarse.

## <a name="set-up-naming-policy"></a>Configuración de la directiva de nomenclatura

### <a name="step-1-sign-in-using-powershell-cmdlets"></a>Paso 1: Inicio de sesión con los cmdlets de PowerShell

1. Abra la aplicación Windows PowerShell. No necesita privilegios elevados.

2. Ejecute los comandos siguientes para prepararse para ejecutar los cmdlets.
  
  ```
  Import-Module AzureADPreview
  Connect-AzureAD
  ```
  En la pantalla **Sign in to your Account** (Iniciar sesión en su cuenta) que se abre, escriba la cuenta y la contraseña de administrador para conectarse al servicio y seleccione **Sign in** (Iniciar sesión).

3. Siga los pasos de [Cmdlets de Azure Active Directory para configurar las opciones de grupo](groups-settings-cmdlets.md) para crear una configuración de grupo para este inquilino.

### <a name="step-2-view-the-current-settings"></a>Paso 2: Visualización de la configuración actual

1. Visualice la configuración de la directiva de nomenclatura actual.
  
  ```
  $Setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
  ```
  
2. Muestre la configuración actual del grupo.
  
  ```
  $Setting.Values
  ```
  
### <a name="step-3-set-the-naming-policy-and-any-custom-blocked-words"></a>Paso 3: Establecimiento de la directiva de nomenclatura y de las palabras bloqueadas personalizadas

1. Establezca los prefijos y sufijos de nombre de grupo en Azure AD PowerShell. Para que la característica funcione correctamente, [nombre de grupo] debe estar incluido en la configuración.
  
  ```
  $Setting["PrefixSuffixNamingRequirement"] =“GRP_[GroupName]_[Department]"
  ```
  
2. Establezca las palabras bloqueadas personalizadas que quiere restringir. En el ejemplo siguiente se muestra cómo puede agregar sus propias palabras personalizadas.
  
  ```
  $Setting["CustomBlockedWordsList"]=“Payroll,CEO,HR"
  ```
  
3. Guarde la configuración de la nueva directiva para que se aplique, como en el ejemplo siguiente.
  
  ```
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ```
  
Eso es todo. Ha establecido la directiva de nomenclatura y ha agregado las palabras bloqueadas personalizadas.

## <a name="clean-up-resources"></a>Limpieza de recursos

1. Limpie los prefijos y sufijos de nombre de grupo en Azure AD PowerShell.
  
  ```
  $Setting["PrefixSuffixNamingRequirement"] =""
  ```
  
2. Vacíe las palabras bloqueadas personalizadas.
  
  ```
  $Setting["CustomBlockedWordsList"]=""
  ```
  
3. Guarde la configuración
  
  ```
  Set-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id -DirectorySetting $Setting
  ```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha aprendido cómo usar los cmdlets de PowerShell para establecer la directiva de nomenclatura para el inquilino de Azure AD.

Para más información acerca de las limitaciones técnicas, sobre cómo agregar una lista de palabras bloqueadas personalizadas o las experiencias de usuario final en las aplicaciones de Office 365, avance al siguiente artículo para más información sobre los detalles de la directiva de nomenclatura.
> [!div class="nextstepaction"]
> [Todos los detalles de la directiva de nomenclatura](groups-naming-policy.md)
