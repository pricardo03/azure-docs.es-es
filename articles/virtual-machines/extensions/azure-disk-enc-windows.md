---
title: Azure Disk Encryption para Windows | Microsoft Docs
description: Implementa Azure Disk Encryption en una máquina virtual Windows mediante una extensión de máquina virtual.
services: virtual-machines-windows
documentationcenter: ''
author: ejarvi
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 06/12/2018
ms.author: ejarvi
ms.openlocfilehash: 355fa90113e931fa3e21df1ccca5736622475bb3
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810387"
---
# <a name="azure-disk-encryption-for-windows-microsoftazuresecurityazurediskencryption"></a>Azure Disk Encryption para Windows (Microsoft.Azure.Security.AzureDiskEncryption)

## <a name="overview"></a>Información general

Azure Disk Encryption aprovecha BitLocker para ofrecer cifrado de disco completo en las máquinas virtuales de Azure que ejecutan Windows.  Esta solución se integra con Azure Key Vault para administrar los secretos y las claves de cifrado de los discos de la suscripción de Key Vault. 

## <a name="prerequisites"></a>Requisitos previos

Para obtener una lista completa de los requisitos previos, consulte [Requisitos previos de Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="operating-system"></a>Sistema operativo

Para obtener una lista de las versiones actuales de Windows, consulte [Requisitos previos de Azure Disk Encryption](../../security/azure-security-disk-encryption-prerequisites.md).

### <a name="internet-connectivity"></a>Conectividad de Internet

Azure Disk Encryption requiere conectividad a Internet para acceder a Active Directory, Azure Key Vault, Azure Storage y a los puntos de conexión de administración de paquetes.  Para más información acerca de la configuración de la seguridad de red, consulte [Requisitos previos de Azure Disk Encryption](
../../security/azure-security-disk-encryption-prerequisites.md).

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
    },
    "publisher": "Microsoft.Azure.Security",
    "settings": {
      "AADClientID": "[aadClientID]",
      "EncryptionOperation": "[encryptionOperation]",
      "KeyEncryptionAlgorithm": "[keyEncryptionAlgorithm]",
      "KeyEncryptionKeyURL": "[keyEncryptionKeyURL]",
      "KeyVaultURL": "[keyVaultURL]",
      "SequenceVersion": "sequenceVersion]",
      "VolumeType": "[volumeType]"
    },
    "type": "AzureDiskEncryption",
    "typeHandlerVersion": "[extensionVersion]"
  }
}
```

### <a name="property-values"></a>Valores de propiedad

| NOMBRE | Valor / ejemplo | Tipo de datos |
| ---- | ---- | ---- |
| apiVersion | 2015-06-15 | fecha |
| publisher | Microsoft.Azure.Security | string |
| Tipo | AzureDiskEncryptionForWindows| string |
| typeHandlerVersion | 1.0, 2.2 (VMSS) | int |
| (opcional) AADClientID | xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | GUID | 
| (opcional) AADClientSecret | contraseña | string |
| (opcional) AADClientCertificate | thumbprint | string |
| EncryptionOperation | EnableEncryption | string | 
| KeyEncryptionAlgorithm | RSA-OAEP | string |
| KeyEncryptionKeyURL | URL | string |
| KeyVaultURL | URL | string |
| SequenceVersion | uniqueidentifier | string |
| VolumeType | OS, Data, All | string |

## <a name="template-deployment"></a>Implementación de plantilla
Para ver un ejemplo de implementación de una plantilla, consulte [Create a new encrypted Windows VM from gallery image](https://github.com/Azure/azure-quickstart-templates/tree/master/201-encrypt-create-new-vm-gallery-image) (Creación de una nueva VM Windows cifrada a partir de una image de la galería).

## <a name="azure-cli-deployment"></a>Implementación de la CLI de Azure

Puede encontrar instrucciones en la versión más reciente de la [documentación de la CLI de Azure](/cli/azure/vm/encryption?view=azure-cli-latest). 

## <a name="troubleshoot-and-support"></a>Solución de problemas y asistencia

### <a name="troubleshoot"></a>Solución de problemas

Consulte la [Guía de solución de problemas de Azure Disk Encryption](../../security/azure-security-disk-encryption-tsg.md).

### <a name="support"></a>Soporte técnico

Si necesita más ayuda con cualquier aspecto de este artículo, puede ponerse en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/community/). Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione Obtener soporte. Para obtener información sobre el uso del soporte técnico, lea las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/).

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre las extensiones, consulte el artículo [Características y extensiones de las máquinas virtuales para Windows](features-windows.md).