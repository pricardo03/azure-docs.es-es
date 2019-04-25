---
title: Configuración de grupos mediante PowerShell en Azure Active Directory | Microsoft Docs
description: Administración de la configuración de grupos mediante cmdlets de Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 02/26/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5773924e98b7ea13c180979dba1325eb8919ff3a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60469902"
---
# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Cmdlets de Azure Active Directory para configurar las opciones de grupo
Este artículo contiene instrucciones para usar cmdlets de PowerShell de Azure Active Directory (Azure AD) para crear y actualizar grupos. Este contenido se aplica únicamente a grupos de Office 365, también conocidos como grupos unificados. 

> [!IMPORTANT]
> Algunas configuraciones requieren una licencia de Azure Active Directory Premium P1. Para más información, consulte la tabla [Configuración de plantillas](#template-settings).

Para más información sobre cómo evitar que los usuarios no administradores creen grupos de seguridad, establezca  `Set-MsolCompanySettings -UsersPermissionToCreateGroupsEnabled $False` como se describe en [Set-MSOLCompanySettings](https://docs.microsoft.com/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0). 

Los grupos de Office 365 se configuran mediante un objeto Settings y un objeto SettingsTemplate. Al principio no ve ningún objeto de configuración en el directorio porque este se ha configurado de forma predeterminada. Para cambiar la configuración predeterminada, debe crear un nuevo objeto Settings utilizando una plantilla SettingsTemplate. Las plantillas de configuración las define Microsoft. Hay varias plantillas de configuración diferentes. Para configurar los valores del grupo de Office 365 para su directorio, utiliza la plantilla denominada "Group.Unified". Para configurar los valores del grupo de Office 365 en un único grupo, utilice la plantilla denominada "Group.Unified.Guest". Esta plantilla se usa para administrar el acceso de invitado a un grupo de Office 365. 

Los cmdlets forman parte del módulo Azure Active Directory PowerShell V2. Para obtener instrucciones sobre cómo descargar e instalar el módulo en el equipo, consulte el artículo [Azure Active Directory PowerShell versión 2](https://docs.microsoft.com/powershell/azuread/). Puede instalar la versión 2 del módulo desde [la Galería de PowerShell](https://www.powershellgallery.com/packages/AzureAD/).

## <a name="retrieve-a-specific-settings-value"></a>Recuperación de un valor de configuración específico
Si conoce el nombre de la configuración que desea recuperar, puede usar el siguiente cmdlet para recuperar el valor de configuración actual. En este ejemplo, se recuperará el valor de una configuración denominada "UsageGuidelinesUrl". Puede obtener información adicional sobre la configuración de directorio y sus nombres en este artículo.

```powershell
(Get-AzureADDirectorySetting).Values | Where-Object -Property Name -Value UsageGuidelinesUrl -EQ
```

## <a name="create-settings-at-the-directory-level"></a>Creación de una configuración en el nivel de directorio
Con estos pasos se crean configuraciones en el nivel de directorio que se aplican a todos los grupos de Office 365 del directorio. El cmdlet Get-AzureADDirectorySettingTemplate solo está disponible en el [módulo de versión preliminar de Azure AD PowerShell for Graph](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).

1. En los cmdlets DirectorySettings, debe especificar el identificador de la plantilla SettingsTemplate que desea usar. Si no conoce este identificador, este cmdlet devuelve la lista de plantillas de configuración:
  
   ```powershell
   Get-AzureADDirectorySettingTemplate
   ```
   Esta llamada al cmdlet devuelve todas las plantillas que están disponibles:
  
   ```powershell
   Id                                   DisplayName         Description
   --                                   -----------         -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified       ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest Settings for a specific Office 365 group
   16933506-8a8d-4f0d-ad58-e1db05a5b929 Company.BuiltIn     Setting templates define the different settings that can be used for the associ...
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy       Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule       Settings ...
   ```
2. Para agregar una dirección URL de instrucciones de uso, primero debe obtener el objeto SettingsTemplate que define el valor pertinente, es decir, la plantilla Group.Unified:
  
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 62375ab9-6b52-47ed-826b-58e47e0e304b
   ```
3. A continuación, cree un nuevo objeto de configuración basado en esa plantilla:
  
   ```powershell
   $Setting = $template.CreateDirectorySetting()
   ```  
4. Luego, actualice el valor de instrucciones de uso:
  
   ```powershell
   $setting["UsageGuidelinesUrl"] = "https://guideline.example.com"
   ```  
5. Por último, aplique la configuración:
  
   ```powershell
   New-AzureADDirectorySetting -DirectorySetting $setting
   ```

Tras completarse correctamente, el cmdlet devuelve el identificador del nuevo objeto de configuración:

  ```powershell
  Id                                   DisplayName TemplateId                           Values
  --                                   ----------- ----------                           ------
  c391b57d-5783-4c53-9236-cefb5c6ef323             62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
  ```

## <a name="template-settings"></a>Configuración de plantillas
Esta es la configuración definida en el objeto SettingsTemplate Group.Unified. Salvo que se indique lo contrario, estas características requieren una licencia Azure Active Directory Premium P1. 

| **Configuración** | **Descripción** |
| --- | --- |
|  <ul><li>EnableGroupCreation<li>Escriba:  Boolean<li>Valor predeterminado: True |Marca que indica si se permite la creación de grupos de Office 365 en el directorio por parte de usuarios que no sean administradores. Este valor no requiere una licencia Azure Active Directory Premium P1.|
|  <ul><li>GroupCreationAllowedGroupId<li>Escriba:  string<li>Valor predeterminado: “” |El GUID del grupo de seguridad para el que se permite a los miembros crear grupos de Office 365 incluso cuando EnableGroupCreation == false. |
|  <ul><li>UsageGuidelinesUrl<li>Escriba:  string<li>Valor predeterminado: “” |Un vínculo a las instrucciones de uso de grupos. |
|  <ul><li>ClassificationDescriptions<li>Escriba:  string<li>Valor predeterminado: “” | Una lista delimitada por comas de las descripciones de clasificación. El valor de ClassificationDescriptions solo es válido en este formato:<br>$setting ["ClassificationDescriptions"] = "Clasificación: Description, descripción de clasificación:"<br>donde clasificación coincide con las cadenas en el ClassificationList.|
|  <ul><li>DefaultClassification<li>Escriba:  string<li>Valor predeterminado: “” | La clasificación que se va a utilizar como la clasificación predeterminada para un grupo si no se ha especificado ninguno.|
|  <ul><li>PrefixSuffixNamingRequirement<li>Escriba:  string<li>Valor predeterminado: “” | Cadena de una longitud máxima de 64 caracteres que define la convención de nomenclatura configurada para los grupos de Office 365. Para más información, consulte [Aplicación de una directiva de nomenclatura para grupos de Office 365](groups-naming-policy.md). |
| <ul><li>CustomBlockedWordsList<li>Escriba:  string<li>Valor predeterminado: “” | Cadena de frases separadas por comas que los usuarios no tendrán permiso para usar en los nombres de grupos o alias. Para más información, consulte [Aplicación de una directiva de nomenclatura para grupos de Office 365](groups-naming-policy.md). |
| <ul><li>EnableMSStandardBlockedWords<li>Escriba:  Boolean<li>Valor predeterminado: “False” | No usar
|  <ul><li>AllowGuestsToBeGroupOwner<li>Escriba:  Boolean<li>Valor predeterminado: False | Valor booleano que indica si un usuario invitado puede ser o no un propietario de grupos. |
|  <ul><li>AllowGuestsToAccessGroups<li>Escriba:  Boolean<li>Valor predeterminado: True | Valor booleano que indica si un usuario invitado puede tener o no acceso al contenido de grupos de Office 365.  Este valor no requiere una licencia Azure Active Directory Premium P1.|
|  <ul><li>GuestUsageGuidelinesUrl<li>Escriba:  string<li>Valor predeterminado: “” | La dirección URL de un vínculo a las instrucciones de uso del invitado. |
|  <ul><li>AllowToAddGuests<li>Escriba:  Boolean<li>Valor predeterminado: True | Un valor booleano que indica si está permitido o no agregar invitados a este directorio.|
|  <ul><li>ClassificationList<li>Escriba:  string<li>Valor predeterminado: “” |Lista de valores de clasificación delimitados por coma que se puede aplicar a grupos de Office 365. |

## <a name="read-settings-at-the-directory-level"></a>Lectura de la configuración en el nivel de directorio
Con estos pasos se lee la configuración en el nivel de directorio, la cual se aplica a todos los grupos de Office del directorio.

1. Lea toda la configuración de directorio existente:
   ```powershell
   Get-AzureADDirectorySetting -All $True
   ```
   Este cmdlet devuelve la lista de las opciones de configuración del directorio:
   ```powershell
   Id                                   DisplayName   TemplateId                           Values
   --                                   -----------   ----------                           ------
   c391b57d-5783-4c53-9236-cefb5c6ef323 Group.Unified 62375ab9-6b52-47ed-826b-58e47e0e304b {class SettingValue {...
   ```

2. Lea toda la configuración de un grupo específico:
   ```powershell
   Get-AzureADObjectSetting -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -TargetType Groups
   ```

3. Lea todos los valores de configuración de directorio de un objeto de configuración del directorio específico, con el GUID de identificador de configuración:
   ```powershell
   (Get-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323).values
   ```
   Este cmdlet devuelve los nombres y valores de este objeto de configuración para este grupo en particular:
   ```powershell
   Name                          Value
   ----                          -----
   ClassificationDescriptions
   DefaultClassification
   PrefixSuffixNamingRequirement
   CustomBlockedWordsList        
   AllowGuestsToBeGroupOwner     False 
   AllowGuestsToAccessGroups     True
   GuestUsageGuidelinesUrl
   GroupCreationAllowedGroupId
   AllowToAddGuests              True
   UsageGuidelinesUrl            https://guideline.example.com
   ClassificationList
   EnableGroupCreation           True
   ```

## <a name="update-settings-for-a-specific-group"></a>Actualización de la configuración de un grupo específico

1. Busque la plantilla de configuración denominada "Groups.Unified.Guest"
   ```powershell
   Get-AzureADDirectorySettingTemplate
  
   Id                                   DisplayName            Description
   --                                   -----------            -----------
   62375ab9-6b52-47ed-826b-58e47e0e304b Group.Unified          ...
   08d542b9-071f-4e16-94b0-74abb372e3d9 Group.Unified.Guest    Settings for a specific Office 365 group
   4bc7f740-180e-4586-adb6-38b2e9024e6b Application            ...
   898f1161-d651-43d1-805c-3b0b388a9fc2 Custom Policy Settings ...
   5cf42378-d67d-4f36-ba46-e8b86229381d Password Rule Settings ...
   ```
2. Recupere el objeto de plantilla para la plantilla Groups.Unified.Guest:
   ```powershell
   $Template = Get-AzureADDirectorySettingTemplate -Id 08d542b9-071f-4e16-94b0-74abb372e3d9
   ```
3. Cree un nuevo objeto de configuración a partir de la plantilla:
   ```powershell
   $Setting = $Template.CreateDirectorySetting()
   ```

4. Establezca la configuración al valor requerido:
   ```powershell
   $Setting["AllowToAddGuests"]=$False
   ```
5. Cree la nueva configuración para el grupo necesario en el directorio:
   ```powershell
   New-AzureADObjectSetting -TargetType Groups -TargetObjectId ab6a3887-776a-4db7-9da4-ea2b0d63c504 -DirectorySetting $Setting
  
   Id                                   DisplayName TemplateId                           Values
   --                                   ----------- ----------                           ------
   25651479-a26e-4181-afce-ce24111b2cb5             08d542b9-071f-4e16-94b0-74abb372e3d9 {class SettingValue {...
   ```

## <a name="update-settings-at-the-directory-level"></a>Actualización de la configuración en el nivel de directorio

Con estos pasos se actualiza la configuración en el nivel de directorio que se aplica a todos los grupos de Office 365 del directorio. En estos ejemplos se asume que ya hay un objeto de configuración en el directorio.

1. Busque el objeto de configuración existente:
   ```powershell
   $setting = Get-AzureADDirectorySetting -Id (Get-AzureADDirectorySetting | where -Property DisplayName -Value "Group.Unified" -EQ).id
   ```
2. Actualice el valor:
  
   ```powershell
   $Setting["AllowToAddGuests"] = "false"
   ```
3. Actualice la configuración:
  
   ```powershell
   Set-AzureADDirectorySetting -Id c391b57d-5783-4c53-9236-cefb5c6ef323 -DirectorySetting $Setting
   ```

## <a name="remove-settings-at-the-directory-level"></a>Eliminación de la configuración en el nivel de directorio
Con estos pasos se elimina la configuración en el nivel de directorio, lo cual se aplica a todos los grupos de Office del directorio.
  ```powershell
  Remove-AzureADDirectorySetting –Id c391b57d-5783-4c53-9236-cefb5c6ef323c
  ```

## <a name="cmdlet-syntax-reference"></a>Referencia de sintaxis de cmdlet
Puede encontrar más documentación de Azure Active Directory PowerShell en el artículo sobre los [cmdlets de Azure Active Directory](/powershell/azure/install-adv2?view=azureadps-2.0).

## <a name="additional-reading"></a>Lecturas adicionales

* [Administración del acceso a los recursos con grupos de Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Integración de las identidades locales con Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
