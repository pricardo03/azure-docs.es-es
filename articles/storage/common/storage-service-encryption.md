---
title: Cifrado de Azure Storage para datos en reposo | Microsoft Docs
description: Azure Storage protege los datos cifrándolos automáticamente antes de guardarlos en la nube. Todos los datos de Azure Storage se cifran y descifran de forma transparente mediante el cifrado AES de 256 bits y son compatibles con FIPS 140-2.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/15/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 1e95adbd1a564fb34d3f0506ac1cc25bc5a63c62
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65790052"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Cifrado de Azure Storage para datos en reposo

Azure Storage cifra automáticamente los datos al guardarlos en la nube. Mediante el cifrado, se protegen los datos y es más fácil cumplir los compromisos de cumplimiento y seguridad de la organización. Los datos de Azure Storage se cifran y descifran de forma transparente mediante el [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles, y son compatibles con FIPS 140-2. El cifrado de Azure Storage es similar al cifrado de BitLocker en Windows.

El cifrado de Azure Storage está habilitado para todas las cuentas de almacenamiento nuevas y existentes y no se puede deshabilitar. Como los datos están protegidos de forma predeterminada, no es necesario modificar el código o las aplicaciones para aprovechar el cifrado de Azure Storage. 

Las cuentas de almacenamiento se cifran independientemente de su nivel de rendimiento (Estándar o Premium) o del modelo de implementación (Azure Resource Manager o clásico). Todas las opciones de redundancia de Azure Storage admiten el cifrado y se cifran todas las copias de una cuenta de almacenamiento. Se cifran todos los recursos de Azure Storage, incluidos los blobs, los discos, los archivos, las colas y las tablas. También se cifran todos los metadatos de objetos.

El cifrado no afecta al rendimiento de Azure Storage. No hay ningún costo adicional para el cifrado de Azure Storage.

Para más información sobre de los módulos criptográficos subyacentes al cifrado de Azure Storage, vea [API de criptografía: última generación](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="key-management"></a>Administración de claves

Puede confiar en las claves administradas por Microsoft para el cifrado de la cuenta de almacenamiento, o puede administrar el cifrado con sus propias claves, junto con Azure Key Vault.

### <a name="microsoft-managed-keys"></a>Claves administradas por Microsoft

De forma predeterminada, la cuenta de almacenamiento usa claves de cifrado administradas por Microsoft. Puede ver la configuración de cifrado para la cuenta de almacenamiento en la sección **Cifrado** de [Azure Portal](https://portal.azure.com), tal y como se muestra en esta imagen.

![Ver cuenta cifrada con claves administradas por Microsoft](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

### <a name="customer-managed-keys"></a>Claves administradas por el cliente

Puede administrar el cifrado de Azure Storage con claves administradas por el cliente. Las claves administradas por el cliente ofrece más flexibilidad para crear, girar, deshabilitar y revocar controles de acceso. También permite auditar las claves de cifrado que se usan para proteger los datos. 

Use Azure Key Vault para administrar las claves y auditar su uso. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. Para más información sobre Azure Key Vault, vea [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md).

Para revocar el acceso a las claves administradas por el cliente, vea [PowerShell de Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) y [CLI de Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los datos de la cuenta de almacenamiento, ya que Azure Storage no puede acceder a la clave de cifrado.

Para aprender a usar claves administradas por el cliente con Azure Storage, vea uno de estos artículos:

- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde Azure Portal](storage-encryption-keys-portal.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell](storage-encryption-keys-powershell.md)
- [Usar las claves administradas por el cliente con el cifrado de Azure Storage desde la CLI de Azure](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Las claves administradas por el cliente dependen de identidades administradas para los recursos de Azure, una característica de Azure Active Directory (Azure AD). Cuando se transfiere una suscripción de un directorio de Azure AD a otro, las identidades administradas no se actualizan y es posible que las claves administradas por el cliente dejen de funcionar. Para más información, vea **Transferencia de una suscripción entre directorios de Azure AD** en [Preguntas frecuentes y problemas conocidos con identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

> [!NOTE]  
> Las claves administradas por el cliente no se admiten actualmente para [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Cifrado de Azure Storage frente a cifrado de disco

Con el cifrado de Azure Storage, se cifran todas las cuentas de Azure Storage y los recursos que contienen, incluidos los blobs en páginas que guardan los discos de máquinas virtuales de Azure. Además, se pueden cifrar los discos de máquinas virtuales de Azure con [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md). Azure Disk Encryption usa el estándar del sector [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) en Windows y [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) en Linux para proporcionar soluciones de cifrado basado en el sistema que se integran con Azure Key Vault.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md)
