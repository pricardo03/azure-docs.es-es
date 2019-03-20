---
title: Dejar de usar la extensión de máquina virtual de identidad administrada y empezar a usar el punto de conexión de Azure Instance Metadata Service
description: Instrucciones paso a paso para dejar de usar la extensión de máquina virtual y empezar a usar el servicio de metadatos de instancia (IMDS) de Azure para la autenticación.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/25/2018
ms.author: priyamo
ms.openlocfilehash: c29eb22f2606fc44cfd7d948f2c363cb51c811dd
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227690"
---
# <a name="how-to-stop-using-the-virtual-machine-managed-identities-extension-and-start-using-the-azure-instance-metadata-service"></a>Cómo detener el uso de la máquina virtual administrado extensión de identidades y comenzar a usar el servicio de metadatos de instancia de Azure

## <a name="virtual-machine-extension-for-managed-identities"></a>Extensión de máquina virtual para las identidades administradas

La extensión de máquina virtual para las identidades administradas se usa para solicitar tokens de una identidad administrada dentro de la máquina virtual. El flujo de trabajo consta de los pasos siguientes:

1. En primer lugar, llama a la carga de trabajo en el recurso de punto de conexión local `http://localhost/oauth2/token` para solicitar un token de acceso.
2. La extensión de máquina virtual, a continuación, usa las credenciales para la identidad administrada, para solicitar un token de acceso de Azure AD... 
3. El token de acceso se devuelve al llamador y se puede usar para autenticar a los servicios que admiten la autenticación de Azure AD, como Azure Key Vault o Azure Storage.

Debido a varias limitaciones que se describen en la sección siguiente, la extensión de máquina virtual de identidad administrada en desuso en favor de usar el punto de conexión equivalente en el servicio de metadatos de instancia (IMDS) de Azure

### <a name="provision-the-extension"></a>Aprovisionamiento de la extensión 

Al configurar una máquina virtual o el escalado de máquinas virtuales que se define para tener una identidad administrada, puede opcional elegir, puede optar a aprovisionar las identidades administradas de extensión de máquina virtual de los recursos de Azure mediante el `-Type` parámetro en el [ Conjunto AzVMExtension](https://docs.microsoft.com/powershell/module/az.compute/set-azvmextension) cmdlet. Puede pasar `ManagedIdentityExtensionForWindows` o `ManagedIdentityExtensionForLinux`, según el tipo de máquina virtual y asígnele un nombre que el `-Name` parámetro. El parámetro `-Settings` especifica el puerto utilizado por el punto de conexión del token de OAuth para la adquisición de tokens:

```powershell
   $settings = @{ "port" = 50342 }
   Set-AzVMExtension -ResourceGroupName myResourceGroup -Location WestUS -VMName myVM -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Settings $settings 
```

También puede usar la plantilla de implementación de Azure Resource Manager para aprovisionar la extensión de VM, agregando el siguiente JSON para el `resources` sección a la plantilla (usar `ManagedIdentityExtensionForLinux` para los elementos de nombre y tipo para la versión de Linux).

    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2018-06-01",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
    
Si trabaja con conjuntos de escalado de máquinas virtuales, también puede aprovisionar las identidades administradas para utilizar la extensión del conjunto de escalado de máquinas virtuales de los recursos de Azure la [agregar AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) cmdlet. Puede pasar `ManagedIdentityExtensionForWindows` o `ManagedIdentityExtensionForLinux`, según el tipo de escalado de máquinas virtuales establecido y asígnele un nombre que el `-Name` parámetro. El parámetro `-Settings` especifica el puerto utilizado por el punto de conexión del token de OAuth para la adquisición de tokens:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzVmss
   Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```
Para aprovisionar el escalado de máquinas virtuales, establezca la extensión con la plantilla de implementación de Azure Resource Manager, agregue el siguiente JSON para el `extensionpProfile` sección a la plantilla (usar `ManagedIdentityExtensionForLinux` para los elementos de nombre y tipo para la versión de Linux).

    ```json
    "extensionProfile": {
        "extensions": [
            {
                "name": "ManagedIdentityWindowsExtension",
                "properties": {
                    "publisher": "Microsoft.ManagedIdentity",
                    "type": "ManagedIdentityExtensionForWindows",
                    "typeHandlerVersion": "1.0",
                    "autoUpgradeMinorVersion": true,
                    "settings": {
                        "port": 50342
                    },
                    "protectedSettings": {}
                }
            }
    ```

Aprovisionamiento de la extensión de máquina virtual puede producir un error debido a errores de búsqueda DNS. Si esto ocurre, reinicie la máquina virtual e inténtelo de nuevo. 

### <a name="remove-the-extension"></a>Eliminación de la extensión 
Para quitar la extensión, use `-n ManagedIdentityExtensionForWindows` o `-n ManagedIdentityExtensionForLinux` switch (según el tipo de máquina virtual) con [delete de extensión de máquina virtual de az](https://docs.microsoft.com/cli/azure/vm/), o [eliminación de az vmss extensión](https://docs.microsoft.com/cli/azure/vmss) para escalado de máquinas virtuales establece mediante la CLI de Azure, o `Remove-AzVMExtension` para Powershell:

```azurecli-interactive
az vm identity --resource-group myResourceGroup --vm-name myVm -n ManagedIdentityExtensionForWindows
```

```azurecli-interactive
az vmss extension delete -n ManagedIdentityExtensionForWindows -g myResourceGroup -vmss-name myVMSS
```

```powershell
Remove-AzVMExtension -ResourceGroupName myResourceGroup -Name "ManagedIdentityExtensionForWindows" -VMName myVM
```

### <a name="acquire-a-token-using-the-virtual-machine-extension"></a>Adquirir un token mediante la extensión de máquina virtual

Este es un ejemplo de solicitud con las identidades administradas para el punto de conexión de máquina virtual de extensión de recursos de Azure:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Elemento | DESCRIPCIÓN |
| ------- | ----------- |
| `GET` | El verbo HTTP, indicando que se van a recuperar datos desde el punto de conexión. En este caso, el token de acceso de OAuth. | 
| `http://localhost:50342/oauth2/token` | Punto de conexión de identidades administradas de recursos de Azure, donde 50342 es el puerto predeterminado y configurable. |
| `resource` | Un parámetro de cadena de consulta, que indica el URI del identificador de aplicación del recurso de destino. También aparece en la notificación `aud` (audiencia) del token emitido. En este ejemplo, se solicita un token para acceder a Azure Resource Manager, que tiene un URI de identificador de aplicación de https://management.azure.com/. |
| `Metadata` | Un campo de encabezado de la solicitud HTTP, requerido por las identidades administradas de recursos de Azure como mitigación frente a ataques de falsificación de solicitud de lado del servidor (SSRF). Este valor debe establecerse en "true", todo en minúsculas.|
| `object_id` | (Opcional) Un parámetro de cadena de consulta, que indica el valor de object_id de la identidad administrada para la que quiere que sea el token. Es obligatorio si la máquina virtual tiene varias identidades administradas asignadas por el usuario.|
| `client_id` | (Opcional) Un parámetro de cadena de consulta, que indica el valor de client_id de la identidad administrada para la que quiere que sea el token. Es obligatorio si la máquina virtual tiene varias identidades administradas asignadas por el usuario.|


Respuesta de ejemplo:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Elemento | DESCRIPCIÓN |
| ------- | ----------- |
| `access_token` | El token de acceso solicitado. Al llamar a una API de REST protegida, el token se inserta en el campo `Authorization` del encabezado de la solicitud como un token de "portador", lo que permite a la API autenticar el llamador. | 
| `refresh_token` | Las identidades administradas de recursos de Azure no lo usan. |
| `expires_in` | El número de segundos que el token de acceso sigue siendo válido, antes de expirar, desde la hora de emisión. La hora de emisión puede encontrarse en la notificación `iat` del token. |
| `expires_on` | La hora a la que expira el token de acceso. La fecha se representa como el número de segundos desde "1970-01-01T0:0:0Z UTC" (se corresponde con la notificación `exp` del token). |
| `not_before` | El intervalo de tiempo cuando el token de acceso tiene efecto y se puede aceptar. La fecha se representa como el número de segundos desde "1970-01-01T0:0:0Z UTC" (se corresponde con la notificación `nbf` del token). |
| `resource` | El recurso para el que se solicitó el token de acceso, que coincide con el parámetro de la cadena de consulta `resource` de la solicitud. |
| `token_type` | El tipo de token, que es un token de acceso al "Portador", lo que significa que el recurso puede permitir el acceso al portador del token. |


### <a name="troubleshoot-the-virtual-machine-extension"></a>Solución de problemas de la extensión de máquina virtual 

#### <a name="restart-the-virtual-machine-extension-after-a-failure"></a>Reinicie la extensión de máquina virtual después de un error

Tanto en Windows como en algunas versiones de Linux, si la extensión se detiene, se puede utilizar el siguiente cmdlet para reiniciarla manualmente:

```powershell
Set-AzVMExtension -Name <extension name>  -Type <extension Type>  -Location <location> -Publisher Microsoft.ManagedIdentity -VMName <vm name> -ResourceGroupName <resource group name> -ForceRerun <Any string different from any last value used>
```

Donde: 
- Nombre de la extensión y tipo para Windows es: `ManagedIdentityExtensionForWindows`
- Nombre de la extensión y tipo para Linux es: `ManagedIdentityExtensionForLinux`

#### <a name="automation-script-fails-when-attempting-schema-export-for-managed-identities-for-azure-resources-extension"></a>Se produce un error en el "script de automatización" al intentar exportar los esquemas de la extensión de identidades administradas para recursos de Azure.

Cuando las identidades administradas para los recursos de Azure está habilitada en una máquina virtual, se muestra el siguiente error al intentar utilizar la característica "Script de automatización" para la máquina virtual o su grupo de recursos:

![Error de exportación del script de automatización de identidades administradas para recursos de Azure](./media/howto-migrate-vm-extension/automation-script-export-error.png)

Las identidades administradas de extensión de máquina virtual de los recursos de Azure no admite actualmente la capacidad de exportar su esquema a una plantilla de grupo de recursos. Como resultado, en la plantilla generada no se muestran los parámetros de configuración para habilitar las identidades administradas para recursos de Azure en el recurso. Estas secciones pueden agregarse manualmente siguiendo los ejemplos de [configurar administradas las identidades de los recursos de Azure en una máquina virtual de Azure mediante una de las plantillas](qs-configure-template-windows-vm.md).

Cuando la funcionalidad de exportación de esquema está disponible para las identidades administradas de extensión de máquina virtual de los recursos de Azure (prevista para desuso en enero de 2019), se enumerarán en [exportación de grupos de recursos que contienen extensiones de máquina virtual ](../../virtual-machines/extensions/export-templates.md#supported-virtual-machine-extensions).

## <a name="limitations-of-the-virtual-machine-extension"></a>Limitaciones de la extensión de máquina virtual 

Existen varias limitaciones más importantes para el uso de la extensión de máquina virtual. 

 * La limitación más grave es el hecho de que las credenciales usadas para solicitar tokens se almacenan en la máquina virtual. Un atacante que infracciones correctamente la máquina virtual puede extraer las credenciales. 
 * Además, la extensión de máquina virtual no es aún compatible con varias distribuciones de Linux, con un costo para modificar, crear y probar la extensión en cada una de las distribuciones de desarrollo enorme. Actualmente, se admiten las siguientes distribuciones de Linux: 
    * CoreOS Stable
    * CentOS 7.1 
    * Red Hat 7.2 
    * Ubuntu 15.04 
    * Ubuntu 16.04
 * Hay un impacto en el rendimiento a la implementación de máquinas virtuales con identidades administradas, como la extensión de máquina virtual también tiene que estar aprovisionado. 
 * Por último, la extensión de máquina virtual solo puede admitir tener 32 asignada por el usuario las identidades administradas por máquina virtual. 

## <a name="azure-instance-metadata-service"></a>Servicio de metadatos de instancia de Azure

El [servicio de metadatos de instancia de Azure (IMDS)](https://docs.microsoft.com/azure/virtual-machines/instance-metadata-service) es un punto de conexión REST que proporciona información sobre la ejecución de las instancias de máquina virtual que pueden usarse para administrar y configurar las máquinas virtuales. El punto de conexión está disponible en una dirección IP no enrutable conocida (`169.254.169.254`) que puede tener acceso solo desde dentro de la máquina virtual.

Hay varias ventajas al uso de IMDS de Azure para solicitar tokens. 

1. El servicio es externo a la máquina virtual, por lo tanto, las credenciales utilizadas por las identidades administradas ya no están presentes en la máquina virtual. En su lugar, se hospedan y se protegen en el equipo host de máquina virtual de Azure.   
2. Todos los sistemas operativos Windows y Linux compatibles en IaaS de Azure puede usar las identidades administradas.
3. Implementación es más rápido y sencillo, puesto que ya no necesita la extensión de máquina virtual se aprovisione.
4. Con el IMDS el punto de conexión, hasta 1.000 identidades administradas asignada por el usuario puede asignarse a una sola máquina virtual.
5. No hay ningún cambio significativo en las solicitudes mediante IMDS en contraposición a aquellas que usan la extensión de máquina virtual, por lo tanto, es bastante fácil de puerto a través de las implementaciones existentes que actualmente utilizan la extensión de máquina virtual.

Por estas razones, el servicio de Azure IMDS será la forma de facto para solicitar tokens, una vez que la extensión de máquina virtual está en desuso. 


## <a name="next-steps"></a>Pasos siguientes

* [Cómo usar las identidades administradas para los recursos de Azure en una máquina virtual de Azure para adquirir un token de acceso](how-to-use-vm-token.md)
* [Servicio de metadatos de instancia de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service)
