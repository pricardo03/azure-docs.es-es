---
title: Preguntas frecuentes y problemas conocidos con identidades administradas para recursos de Azure
description: Problemas conocidos con identidades administradas para recursos de Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 2097381a-a7ec-4e3b-b4ff-5d2fb17403b6
ms.service: active-directory
ms.component: msi
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 12/12/2017
ms.author: daveba
ms.openlocfilehash: b535939e200b533c06c97686897e283fb6cf57bc
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720191"
---
# <a name="faqs-and-known-issues-with-managed-identities-for-azure-resources"></a>Preguntas frecuentes y problemas conocidos con identidades administradas para recursos de Azure

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

## <a name="frequently-asked-questions-faqs"></a>Preguntas más frecuentes (P+F)

> [!NOTE]
> Identidades administradas para recursos de Azure es el nombre con el que ahora se conoce al servicio Managed Service Identity (MSI).

### <a name="does-managed-identities-for-azure-resources-work-with-azure-cloud-services"></a>¿Funcionan las identidades administradas para recursos de Azure con Azure Cloud Services?

No, no hay planes que admitan las identidades administradas para recursos de Azure en Azure Cloud Services.

### <a name="does-managed-identities-for-azure-resources-work-with-the-active-directory-authentication-library-adal-or-the-microsoft-authentication-library-msal"></a>¿Funcionan las identidades administradas para recursos de Azure con la Biblioteca de autenticación de Active Directory (ADAL) o la Biblioteca de autenticación de Microsoft (MSAL)?

No, las identidades administradas para recursos de Azure aún no están integradas con ADAL o MSAL. Para obtener más información sobre la adquisición de un token para las identidades administradas para recursos de Azure mediante el punto de conexión de REST, vea [Cómo usar identidades administradas de recursos de Azure en una máquina virtual de Azure para adquirir un token de acceso](how-to-use-vm-token.md).

### <a name="what-is-the-security-boundary-of-managed-identities-for-azure-resources"></a>¿Cuál es el límite de seguridad de las identidades administradas para recursos de Azure?

El límite de seguridad de la identidad es el recurso al que está asociada. Por ejemplo, el límite de seguridad para una máquina virtual que tenga las identidades administradas para recursos de Azure habilitadas, es la Máquina virtual. Cualquier código que se ejecute en esa máquina virtual puede llamar a las identidades administradas para los tokens de punto de conexión y solicitud de recursos de Azure. Esta experiencia es similar con otros recursos que admiten las identidades administradas para recursos de Azure.

### <a name="what-identity-will-imds-default-to-if-dont-specify-the-identity-in-the-request"></a>¿A qué identidad se asignará el IMDS de manera predeterminada si no se especifica la identidad en la solicitud?

- Si se habilita la identidad administrada asignada por el sistema y no se especifica ninguna identidad en la solicitud, el IMDS utilizará de manera predeterminada la esta identidad administrada.
- Si la identidad administrada asignada por el sistema no está habilitada, y solo existe una identidad administrada asignada por el usuario, IMDS será la identidad administrada asignada de manera predeterminada a ese único usuario. 
- Si la identidad administrada asignada por el sistema no está habilitada y existen varias identidades administradas asignadas por el usuario, es necesario especificar una identidad administrada en la solicitud.

### <a name="should-i-use-the-managed-identities-for-azure-resources-vm-imds-endpoint-or-the-vm-extension-endpoint"></a>¿Debo usar el punto de conexión de IMDS de la máquina virtual de las identidades administradas para recursos de Azure, o bien el punto de conexión de la extensión de la máquina virtual?

Al usar las identidades administradas para recursos de Azure con máquinas virtuales, se recomienda el punto de conexión de IMDS de las identidades administradas para recursos de Azure. Azure Instance Metadata Service es un punto de conexión REST al que pueden acceder todas las máquinas virtuales IaaS creadas a través de Azure Resource Manager. Algunas de las ventajas de usar las identidades administradas para recursos de Azure sobre IMDS son las siguientes:
    - Todos los sistemas operativos admitidos en IaaS de Azure pueden usar identidades administradas para recursos de Azure en lugar de IMDS.
    - Ya no es necesario instalar una extensión en la máquina virtual para habilitar las identidades administradas para recursos de Azure. 
    - Los certificados que usan las identidades administradas para recursos de Azure ya no están presentes en la máquina virtual.
    - El punto de conexión de IMDS es una dirección IP no enrutable conocida, que solo está disponible desde dentro de la máquina virtual.

La extensión de máquina virtual de identidades administradas para recursos de Azure todavía se puede usar actualmente; pero de aquí en adelante solo se usará el punto de conexión de IMDS de forma predeterminada. En enero de 2019 dejará de utilizarse la extensión de máquina virtual de identidades administradas para recursos de Azure. 

Para obtener más información sobre Azure Instance Metadata Service, vea la [documentación de IMDS](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service).

### <a name="will-managed-identities-be-recreated-automatically-if-i-move-a-subscription-to-another-directory"></a>¿Se van a volver a crear automáticamente las identidades administradas si muevo una suscripción a otro directorio?

 No. Si mueve una suscripción a otro directorio, tendrá que volver a crearlas manualmente y volver a asignar los roles de RBAC de Azure.
    - Para las identidades administradas asignadas por el sistema, tiene que desactivarlas y volver a activarlas.
    - Para las identidades administradas asignadas por el usuario, tiene que eliminarlas, volver a crearlas y adjuntarlas de nuevo a los recursos necesarios (por ejemplo, máquinas virtuales).

### <a name="can-i-use-a-managed-identity-to-access-a-resource-in-a-different-directorytenant"></a>¿Puedo usar una identidad administrada para acceder a un recurso en un directorio o inquilino diferente?

 No. Las identidades administradas no admiten actualmente escenarios entre directorios. 

### <a name="what-are-the-supported-linux-distributions"></a>¿Qué distribuciones de Linux son compatibles?

Todas las distribuciones de Linux compatibles con IaaS de Azure se pueden usar con las identidades administradas para recursos de Azure mediante el punto de conexión de IMDS. 

La extensión de máquina virtual de identidades administradas para recursos de Azure (que dejará de utilizarse en enero de 2019) solamente admite las distribuciones de Linux siguientes:
- CoreOS Stable
- CentOS 7.1
- Red Hat 7.2
- Ubuntu 15.04
- Ubuntu 16.04

Actualmente, otras distribuciones de Linux no son compatibles, por lo que existe la posibilidad de que la extensión produzca errores en ellas.

La extensión funciona en CentOS 6.9. Sin embargo, dada a la falta de compatibilidad el sistema en la 6.9, si la extensión se bloquea o se detiene, no se reiniciará automáticamente. Se reinicia cuando se reinicia la máquina virtual. Para reiniciar la extensión de forma manual, vea [¿Cómo se reinicia la extensión de identidades administradas para los recursos de Azure?](#how-do-you-restart-the-managed-identities-for-Azure-resources-extension)

### <a name="how-do-you-restart-the-managed-identities-for-azure-resources-extension"></a>¿Cómo se reinicia la extensión de identidades administradas para recursos de Azure?
Tanto en Windows como en algunas versiones de Linux, si la extensión se detiene, se puede utilizar el siguiente cmdlet para reiniciarla manualmente:

```powershell
Set-AzureRmVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Donde: 
- El tipo y el nombre de la extensión para Windows es: ManagedIdentityExtensionForWindows
- El tipo y el nombre de la extensión para Linux es: ManagedIdentityExtensionForLinux

## <a name="known-issues"></a>Problemas conocidos

### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>Se produce un error en el "script de automatización" al intentar exportar los esquemas de la extensión de identidades administradas para recursos de Azure.

Cuando se habilitan las identidades administradas para recursos de Azure en una máquina virtual, se muestra el siguiente error al intentar usar la característica de "Script de automatización" para la máquina virtual o su grupo de recursos:

![Error de exportación del script de automatización de identidades administradas para recursos de Azure](./media/msi-known-issues/automation-script-export-error.png)

La extensión de máquina virtual de identidades administradas para recursos de Azure (que dejará de utilizarse en enero de 2019) no admite actualmente la posibilidad de exportar su esquema a una plantilla de grupo de recursos. Como resultado, en la plantilla generada no se muestran los parámetros de configuración para habilitar las identidades administradas para recursos de Azure en el recurso. Estas secciones se pueden agregar de forma manual siguiendo los ejemplos de [Configuración de identidades administradas para recursos de Azure en una máquina virtual de Azure mediante una plantilla](qs-configure-template-windows-vm.md).

Cuando la funcionalidad de exportación de esquema está disponible para la extensión de máquina virtual de identidades administradas para recursos de Azure (que dejará de utilizarse en enero de 2019), se enumerará en [Exportación de grupos de recursos que contienen extensiones de máquina virtual](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

### <a name="configuration-blade-does-not-appear-in-the-azure-portal"></a>La hoja de configuración no aparece en Azure Portal

Si la hoja de configuración de la máquina virtual no aparece en la máquina virtual, quiere decir que las identidades administradas para recursos de Azure todavía no se han habilitado en el portal de su región.  Vuelva a comprobarlo más tarde.  También puede habilitar las identidades administradas para recursos de Azure para la máquina virtual con [PowerShell](qs-configure-powershell-windows-vm.md) o la [CLI de Azure](qs-configure-cli-windows-vm.md).

### <a name="cannot-assign-access-to-virtual-machines-in-the-access-control-iam-blade"></a>No se puede asignar el acceso a las máquinas virtuales en la hoja Control de acceso (IAM)

Si **Virtual Machine** no aparece en Azure Portal como una opción para **Asignar acceso a** en **Control de acceso (IAM)** > **Agregar asignación de roles**, las identidades administradas para recursos de Azure todavía no se han habilitado en el portal en su región. Vuelva a comprobarlo más tarde.  Aún puede seleccionar la identidad de la máquina virtual para la asignación de roles; para ello, busque la entidad de servicio de las identidades administradas para recursos de Azure.  Escriba el nombre de la máquina virtual en el campo **Seleccionar** y la entidad de servicio aparece en el resultado de la búsqueda.

### <a name="vm-fails-to-start-after-being-moved-from-resource-group-or-subscription"></a>La máquina virtual no puede iniciarse después de moverse del grupo de recursos o de la suscripción

Si mueve una máquina virtual en estado de ejecución, continúa ejecutándose durante el desplazamiento. Sin embargo, después, si la máquina virtual se detiene y se reinicia, no se podrá iniciar. Este problema se produce porque la máquina virtual no está actualizando la referencia a la identidad referente a las identidades administradas para recursos de Azure, y continúa apuntando a ella en el grupo de recursos anterior.

**Solución alternativa** 
 
Desencadene una actualización en la máquina virtual para que pueda obtener los valores correctos para las identidades administradas para recursos de Azure. Puede hacer un cambio de propiedad de máquina virtual para actualizar la referencia a la identidad referente a las identidades administradas para recursos de Azure. Por ejemplo, puede establecer un nuevo valor de etiqueta en la máquina virtual con el siguiente comando:

```azurecli-interactive
 az  vm update -n <VM Name> -g <Resource Group> --set tags.fixVM=1
```
 
Este comando establece una nueva etiqueta "fixVM" con un valor de 1 en la máquina virtual. 
 
Al establecer esta propiedad, la máquina virtual se actualiza con el URI del recurso correcto de las identidades administradas para recursos de Azure y, después, debería poder iniciar la máquina virtual. 
 
Una vez que se inicia la máquina virtual, la etiqueta puede quitarse con el comando siguiente:

```azurecli-interactive
az vm update -n <VM Name> -g <Resource Group> --remove tags.fixVM
```

### <a name="vm-extension-provisioning-fails"></a>Se produce un error de aprovisionamiento de la extensión de máquina virtual

Se puede producir un error en el aprovisionamiento de la extensión de máquina virtual debido a errores de búsqueda DNS. Reinicie la máquina virtual y vuelva a intentarlo.
 
> [!NOTE]
> Está previsto que la extensión de máquina virtual quede en desuso en enero de 2019. Le recomendamos que pase al uso del punto de conexión IMDS.

### <a name="transferring-a-subscription-between-azure-ad-directories"></a>Transferencia de una suscripción entre directorios de Azure AD

Las identidades administradas no se actualizan cuando una suscripción se mueve o transfiere a otro directorio. Como resultado, se interrumpe cualquier identidad administrada asignada por el sistema o por el usuario. 

Como solución alternativa, una vez que se ha movido la suscripción, puede deshabilitar las identidades administradas asignadas por el sistema y volver a habilitarlas. Igualmente, puede eliminar y volver a crear cualquier identidad administrada asignada por el usuario. 

## <a name="known-issues-with-user-assigned-managed-identities"></a>Problemas conocidos de las identidades asignadas por el usuario

- No se admite la creación de una identidad administrada asignada por el usuario con caracteres especiales (por ejemplo, un guion bajo) en el nombre.
- Los nombres de identidades asignadas por el usuario están restringidos a 24 caracteres. Si se supera este número, la identidad no se podrá asignar a un recurso (por ejemplo, Virtual Machine).
- Si se usa la extensión de máquina virtual de identidades administradas (que dejará de utilizarse en enero de 2019), el límite admitido es de 32 identidades administradas asignadas por el usuario. Sin la extensión de máquina virtual de identidades administradas, el límite admitido es de 512.  
- Mover una identidad administrada asignada por el usuario a otro grupo de recursos hará que la identidad se interrumpa. Como resultado, no podrá solicitar tokens para esa identidad. 
- La transferencia de una suscripción a otro directorio interrumpirá las identidades administradas asignadas por el usuario existentes. 