---
title: Controlador de extensiones de configuración de estado deseado de Azure | Microsoft Docs
description: Cargue y aplique una configuración de DSC de PowerShell en una máquina virtual de Azure con la extensión DSC
services: virtual-machines-windows
documentationcenter: ''
author: bobbytreed
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: windows
ms.workload: ''
ms.date: 03/26/2018
ms.author: robreed
ms.openlocfilehash: 9f81e2b7537a5ecc6778baa93a1bab23dd30ff8a
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57451916"
---
# <a name="powershell-dsc-extension"></a>Extensión DSC de PowerShell

## <a name="overview"></a>Información general

Microsoft, como editor de la extensión DSC de PowerShell, es quien presta los servicios de soporte técnico para esta solución. La extensión carga y aplica una configuración de DSC de PowerShell en una máquina virtual de Azure. La extensión DSC llama a DSC de PowerShell para aplicar la configuración de DSC recibida en la máquina virtual. En este documento se especifican las plataformas compatibles, configuraciones y opciones de implementación de la extensión de máquina virtual de DSC para Windows.

## <a name="prerequisites"></a>Requisitos previos

### <a name="operating-system"></a>Sistema operativo

La extensión DSC es compatible con los sistemas operativos siguientes:

Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, el cliente de Windows 7/8.1/10

### <a name="internet-connectivity"></a>Conectividad de Internet

La extensión DSC para Windows requiere que la máquina virtual de destino sea capaz de comunicarse con Azure y la ubicación del paquete de configuración (archivo .zip) si se almacena en una ubicación fuera de Azure. 

## <a name="extension-schema"></a>Esquema de extensión

El siguiente JSON muestra el esquema para la parte de configuración de la extensión DSC en una plantilla de Azure Resource Manager. 

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "Microsoft.Powershell.DSC",
  "apiVersion": "2018-10-01",
  "location": "<location>",
  "properties": {
    "publisher": "Microsoft.Powershell",
    "type": "DSC",
    "typeHandlerVersion": "2.77",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "wmfVersion": "latest",
        "configuration": {
            "url": "http://validURLToConfigLocation",
            "script": "ConfigurationScript.ps1",
            "function": "ConfigurationFunction"
        },
        "configurationArguments": {
            "argument1": "Value1",
            "argument2": "Value2"
        },
        "configurationData": {
            "url": "https://foo.psd1"
        },
        "privacy": {
            "dataCollection": "enable"
        },
        "advancedOptions": {
            "forcePullAndApply": false
            "downloadMappings": {
                "specificDependencyKey": "https://myCustomDependencyLocation"
            }
        } 
    },
    "protectedSettings": {
        "configurationArguments": {
            "parameterOfTypePSCredential1": {
                "userName": "UsernameValue1",
                "password": "PasswordValue1"
            },
            "parameterOfTypePSCredential2": {
                "userName": "UsernameValue2",
                "password": "PasswordValue2"
            }
        },
        "configurationUrlSasToken": "?g!bber1sht0k3n",
        "configurationDataUrlSasToken": "?dataAcC355T0k3N"
    }
  }
}
```

### <a name="property-values"></a>Valores de propiedad

| NOMBRE | Valor / ejemplo | Tipo de datos |
| ---- | ---- | ---- |
| apiVersion | 01 de octubre de 2018 | fecha |
| publisher | Microsoft.Powershell.DSC | string |
| Tipo | DSC | string |
| typeHandlerVersion | 2.77 | int |

### <a name="settings-property-values"></a>Valores de la propiedad settings

| NOMBRE | Tipo de datos | DESCRIPCIÓN
| ---- | ---- | ---- |
| settings.wmfVersion | string | Especifica la versión de Windows Management Framework que debe instalarse en la máquina virtual. Si se establece esta propiedad en "latest", se instalará la versión más reciente de WMF. Los únicos valores posibles actualmente para esta propiedad son "4.0", "5.0" y "latest". Estos valores posibles están sujetos a actualizaciones. El valor predeterminado es "latest". |
| settings.configuration.url | string | Especifica la ubicación de la dirección URL desde la que descargar el archivo zip de la configuración de DSC. Si la dirección URL proporcionada requiere un token de SAS para el acceso, deberá establecer la propiedad protectedSettings.configurationUrlSasToken en el valor de su token de SAS. Esta propiedad es necesaria si se definen settings.configuration.script o settings.configuration.function.
| settings.configuration.script | string | Especifica el nombre de archivo del script que contiene la definición de la configuración de DSC. Este script debe estar en la carpeta raíz del archivo zip descargado de la dirección URL especificada en la propiedad configuration.url. Esta propiedad es necesaria si se definen settings.configuration.url o settings.configuration.script.
| settings.configuration.function | string | Especifica el nombre de la configuración de DSC. La configuración con nombre debe incluirse en el script definido en configuration.script. Esta propiedad es necesaria si se definen settings.configuration.url o settings.configuration.function.
| settings.configurationArguments | Colección | Define los parámetros que desea pasar a la configuración de DSC. Esta propiedad no se cifrará.
| settings.configurationData.url | string | Especifica la dirección URL desde la que descargar el archivo de datos de configuración (.pds1) que se usará como entrada para la configuración de DSC. Si la dirección URL proporcionada requiere un token de SAS para el acceso, deberá establecer la propiedad protectedSettings.configurationDataUrlSasToken en el valor de su token de SAS.
| settings.privacy.dataEnabled | string | Habilita o deshabilita la recopilación de telemetría. Los únicos valores posibles para esta propiedad son "Enable", "Disable" o "$null". Si se deja esta propiedad en blanco o como null, se habilitará la telemetría.
| settings.advancedOptions.forcePullAndApply | Bool | Esta configuración está diseñada para mejorar la experiencia de trabajar con la extensión para registrar los nodos con DSC de automatización de Azure.  Si el valor es `$true`, la extensión esperará a que la primera ejecución de la configuración que se extraen desde el servicio antes de devolver correcto o con errores.  Si el valor se establece en $false, el estado devuelto por la extensión solo se hará referencia a si el nodo se registró correctamente con la configuración de estado de automatización de Azure y la configuración de nodo no se ejecutará durante el registro.
| settings.advancedOptions.downloadMappings | Colección | Define ubicaciones alternativas para descargar dependencias como WMF y .NET

### <a name="protected-settings-property-values"></a>Valores protegidos de la propiedad settings

| NOMBRE | Tipo de datos | DESCRIPCIÓN
| ---- | ---- | ---- |
| protectedSettings.configurationArguments | string | Define los parámetros que desea pasar a la configuración de DSC. Esta propiedad se cifrará. |
| protectedSettings.configurationUrlSasToken | string | Especifica el token de SAS para acceder a la dirección URL definida en configuration.url. Esta propiedad se cifrará. |
| protectedSettings.configurationDataUrlSasToken | string | Especifica el token de SAS para acceder a la dirección URL definida en configurationData.url. Esta propiedad se cifrará. |


## <a name="template-deployment"></a>Implementación de plantilla

Las extensiones de VM de Azure pueden implementarse con plantillas de Azure Resource Manager.
Las plantillas resultan ideales al implementar una o varias máquinas virtuales que requieren configurarse tras la implementación.
Una plantilla de Resource Manager de ejemplo que incluye la extensión DSC para Windows puede encontrarse en el [Galería de inicio rápido de Azure](https://github.com/Azure/azure-quickstart-templates/blob/master/101-automation-configuration/nested/provisionServer.json#L91).

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Los datos sobre el estado de las implementaciones de extensiones pueden recuperarse desde Azure Portal y mediante la CLI de Azure. Para ver el estado de implementación de las extensiones de una máquina virtual determinada, ejecute el comando siguiente con la CLI de Azure.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

El paquete de extensión se descarga y se implementa en esta ubicación en la máquina virtual de Azure.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}
```

Archivo de extensión de estado contiene el subestado y códigos de estado de éxito o error junto con el error detallado y una descripción para cada ejecución de la extensión.
```
C:\Packages\Plugins\{Extension_Name}\{Extension_Version}\Status\{0}.Status  -> {0} being the sequence number
```

Los registros de salida de la extensión se registran en el directorio siguiente:

```
C:\WindowsAzure\Logs\Plugins\{Extension_Name}\{Extension_Version}
```

### <a name="error-codes-and-their-meanings"></a>Códigos de error y su significado

| Código de error | Significado | Acción posible |
| :---: | --- | --- |
| 1000 | Error genérico | El mensaje de este error lo proporciona la excepción específica en los registros de extensión |
| 52 | Error de instalación de extensión | El mensaje de este error lo proporciona la excepción específica |
| 1002 | Error de instalación de WMF | Error al instalar WMF |
| 1004 | Paquete comprimido no válido | Archivo ZIP no válido; error al desempaquetar el archivo ZIP |
| 1100 | Error de argumento | Indica un problema en la entrada proporcionada por el usuario. El mensaje del error lo proporciona la excepción específica|


### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).
