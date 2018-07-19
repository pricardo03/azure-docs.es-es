---
title: Azure Disk Encryption para Linux | Microsoft Docs
description: Implementa Azure Disk Encryption para Linux en una máquina virtual mediante una extensión de máquina virtual.
services: virtual-machines-linux
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: danis
ms.openlocfilehash: e7926fa488ad38784a3db96035055a80f841aed8
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449269"
---
# <a name="azure-disk-encryption-for-linux-microsoftazuresecurityazurediskencryptionforlinux"></a>Azure Disk Encryption para Linux (Microsoft.Azure.Security.AzureDiskEncryptionForLinux)

## <a name="overview"></a>Información general

Azure Disk Encryption aprovecha el subsistema de dm-crypt de Linux para proporciona un cifrado completo de disco en [determinadas distribuciones de Azure Linux](https://aka.ms/adelinux).  Esta solución se integra con Azure Key Vault para administrar los secretos y las claves de cifrado del disco.

## <a name="prerequisites"></a>Requisitos previos

Para obtener una lista completa de los requisitos previos, consulte [Requisitos previos de Azure Disk Encryption](
../../security/azure-security-disk-encryption.md#prerequisites).

### <a name="operating-system"></a>Sistema operativo

Actualmente, Azure Disk Encryption es compatible con determinadas distribuciones y versiones.  Para ver la lista de distribuciones de Linux que se admiten, consulte las [preguntas más frecuentes acerca de Azure Disk Encryption](../../security/azure-security-disk-encryption-faq.md#what-linux-distributions-does-azure-disk-encryption-support).

### <a name="internet-connectivity"></a>Conectividad de Internet

Azure Disk Encryption para Linux requiere conectividad a Internet para acceder a Active Directory, Azure Key Vault, Azure Storage y a los puntos de conexión de administración de paquetes.  Para más información, consulte los [requisitos previos de Azure Disk Encryption](
../../security/azure-security-disk-encryption.md#prerequisites).

## <a name="extension-schema"></a>Esquema de extensión

```json
{
  "type": "extensions",
  "name": "[name]",
  "apiVersion": "2015-06-15",
  "location": "[location]",
  "properties": {
    "protectedSettings": {
      "AADClientSecret": "[aadClientSecret]",
      "Passphrase": "[passphrase]"
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "DiskFormatQuery": "[diskFormatQuery]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryptionForLinux",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>Valores de propiedad

| NOMBRE | Valor / ejemplo | Tipo de datos |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | fecha |
| publisher | Microsoft.Azure.Security | string |
| Tipo | AzureDiskEncryptionForLinux | string |
| typeHandlerVersion | 0.1, 1.1 (VMSS) | int |
| AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| AADClientSecret | contraseña | string |
| AADClientCertificate | thumbprint | string |
| DiskFormatQuery | {"dev_path":"","name":"","file_system":""} | Diccionario de JSON |
| EncryptionOperation | EnableEncryption, EnableEncryptionFormatAll | string | 
| KeyEncryptionAlgorithm | 'RSA-OAEP', 'RSA-OAEP-256', 'RSA1_5' | string |
| KeyEncryptionKeyURL | URL | string |
| KeyVaultURL | URL | string |
| Frase de contraseña | contraseña | string | 
| SequenceVersion | uniqueidentifier | string |
| VolumeType | OS, Data, All | string |

## <a name="template-deployment"></a>Implementación de plantilla

Para ver un ejemplo de implementación de plantilla, consulte [Enable Encryption on a running Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-running-linux-vm) (Habilitación de cifrado en una máquina virtual en que se ejecute Linux)///.

## <a name="azure-cli-deployment"></a>Implementación de la CLI de Azure

Puede encontrar instrucciones en la versión más reciente de la [documentación de la CLI de Azure](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Para solucionar el problema, consulte [Guía de solución de problemas de Azure Disk Encryption](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/community/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las extensiones de máquina virtual, consulte [Características y extensiones de las máquinas virtuales para Linux](features-linux.md).