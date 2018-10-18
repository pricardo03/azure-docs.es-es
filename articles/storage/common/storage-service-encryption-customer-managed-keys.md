---
title: Cifrado del servicio Storage de Azure mediante claves administradas por el cliente en Azure Key Vault | Microsoft Docs
description: Use la característica Azure Storage Service Encryption para cifrar Azure Blob Storage, Azure Files en el servicio al almacenar los datos y descifrarlos al recuperarlos mediante claves administradas por el cliente.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 10/11/2018
ms.author: lakasa
ms.component: common
ms.openlocfilehash: 0ed05cab774360c4165e89399ba16f7443debb85
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165168"
---
# <a name="storage-service-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Cifrado del servicio Storage mediante claves administradas por el cliente en Azure Key Vault
Microsoft Azure está comprometido a ayudarle a asegurar y proteger sus datos con el fin de satisfacer los compromisos de cumplimiento y seguridad de su organización. Una manera en que la plataforma de Azure Storage protege los datos es mediante Storage Service Encryption (SSE), que cifra los datos al escribirlos en el almacenamiento y los descifra al recuperarlos. El cifrado y descifrado son automáticos, transparentes y usan [cifrado AES](https://wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más seguros disponibles.

Puede usar claves de cifrado administradas por Microsoft con SSE o puede utilizar sus propias claves de cifrado. En este artículo se describe cómo usar sus propias claves de cifrado. Para más información sobre el uso de claves administradas por Microsoft, o sobre SSE en general, consulte [Storage Service Encryption para datos en reposo](storage-service-encryption.md).

SSE para Azure Blob Storage y Azure Files se integra con Azure Key Vault, de modo que puede usar un almacén de claves para administrar sus claves de cifrado. Puede crear sus propias claves de cifrado y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generar las claves de cifrado. Con Azure Key Vault, puede administrar y controlar las claves y auditar su uso.

> [!Note]  
> Storage Service Encryption mediante claves administradas por el cliente no está disponible para [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md). [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md) usa el estándar del sector [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) en Windows y [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) en Linux para proporcionar cifrado integrado con KeyVault.

¿Por qué crear sus propias claves? Las claves personalizadas proporcionan mayor flexibilidad, de manera que puede crear, girar, deshabilitar y definir controles de acceso. También le permiten auditar las claves de cifrado que se usan para proteger los datos.

## <a name="get-started-with-customer-managed-keys"></a>Introducción a las claves administradas por el cliente
Para usar las claves administradas por el cliente con SSE, puede crear un almacén de claves y una clave nuevos, o bien puede usar un almacén de claves y una clave existentes. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. 

### <a name="step-1-create-a-storage-account"></a>Paso 1: Creación de una cuenta de almacenamiento
En primer lugar, cree una cuenta de almacenamiento si todavía no tiene una. Para obtener más información, consulte [Creación de una cuenta de almacenamiento](storage-quickstart-create-account.md).

### <a name="step-2-enable-sse-for-blob-and-file-storage"></a>Paso 2: Habilitación de SSE para Blob y File Storage
Para habilitar SSE con claves administradas por el cliente, también se deben habilitar dos características de protección de claves en Azure Key Vault: Eliminación temporal y No purgar. Estos valores garantizan que las claves no se puedan eliminar de manera accidental ni intencional. El período de retención máximo de las claves se establece en 90 días, lo que protege a los usuarios frente a actores malintencionados o ataques de ransomware.

Si desea habilitar las claves administradas por el cliente para SSE mediante programación, puede usar la [API de REST de proveedor de recursos de Azure Storage](https://docs.microsoft.com/rest/api/storagerp), la [biblioteca de cliente de proveedor de recursos de almacenamiento para .NET](https://docs.microsoft.com/dotnet/api), [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) o la [CLI de Azure](https://docs.microsoft.com/azure/storage/storage-azure-cli).

Para usar las claves administradas por el cliente con SSE, debe asignar una identidad de cuenta de almacenamiento a la cuenta de almacenamiento. Para establecer la identidad, ejecuta el comando de PowerShell siguiente:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName \$resourceGroup -Name \$accountName -AssignIdentity
```

Para habilitar las características Eliminación temporal y No purgar, ejecute los comandos de PowerShell siguientes:

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enableSoftDelete -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties

($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName
$vaultName).ResourceId).Properties | Add-Member -MemberType NoteProperty -Name
enablePurgeProtection -Value 'True'

Set-AzureRmResource -resourceid $resource.ResourceId -Properties
$resource.Properties
```

### <a name="step-3-enable-encryption-with-customer-managed-keys"></a>Paso 3: Habilitación del cifrado con claves administradas por el cliente
De manera predeterminada, SSE usa claves administradas por Microsoft. Puede habilitar SSE con claves administradas por el cliente para la cuenta de almacenamiento mediante [Azure Portal](https://portal.azure.com/). En la hoja **Configuración** de la cuenta de almacenamiento, haga clic en **Cifrado**. Seleccione la opción **Use su propia clave**, tal como se muestra en la imagen siguiente.

![Captura de pantalla del Portal que muestra la opción de la característica Cifrado](./media/storage-service-encryption-customer-managed-keys/ssecmk1.png)

### <a name="step-4-select-your-key"></a>Paso 4: Selección de la clave
Puede especificar la clave como un identificador URI, o bien selecciónela en un almacén de claves.

#### <a name="specify-a-key-as-a-uri"></a>Especificación de una clave como URI
Para especificar la clave a partir de un URI, siga estos pasos:

1. Elija la opción **Enter key URI** (Escribir el URI de la clave).  
2. En el campo **Key URI** (URI de la clave), especifique el URI.

    ![Captura de pantalla del portal que muestra el cifrado con la opción de especificación del UIR de la clave](./media/storage-service-encryption-customer-managed-keys/ssecmk2.png)


#### <a name="specify-a-key-from-a-key-vault"></a>Especificación de una clave a partir de un almacén de claves 
Para especificar la clave a partir de un almacén de claves, siga estos pasos:

1. Elija la opción **Select from Key Vault** (Seleccionar desde almacén de claves).  
2. Elija el almacén de claves que contiene la clave que desea usar.
3. Elija la clave en el almacén de claves.

    ![Captura de pantalla del portal que muestra la opción de cifrado con su propia clave](./media/storage-service-encryption-customer-managed-keys/ssecmk3.png)

Si la cuenta de almacenamiento no tiene acceso al almacén de claves, puede ejecutar el comando de Azure PowerShell que aparece en la imagen siguiente para conceder acceso.

![Captura de pantalla de portal que muestra el acceso denegado para el almacén de claves](./media/storage-service-encryption-customer-managed-keys/ssecmk4.png)

También puede conceder acceso a través de Azure Portal yendo a Azure Key Vault en Azure Portal y concediendo acceso a la cuenta de almacenamiento.


Puede asociar la clave anterior con una cuenta de almacenamiento existente mediante los siguientes comandos de PowerShell:
```powershell
$storageAccount = Get-AzureRmStorageAccount -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount"
$keyVault = Get-AzureRmKeyVault -VaultName "mykeyvault"
$key = Get-AzureKeyVaultKey -VaultName $keyVault.VaultName -Name "keytoencrypt"
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVault.VaultName -ObjectId $storageAccount.Identity.PrincipalId -PermissionsToKeys wrapkey,unwrapkey,get
Set-AzureRmStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName -AccountName $storageAccount.StorageAccountName -KeyvaultEncryption -KeyName $key.Name -KeyVersion $key.Version -KeyVaultUri $keyVault.VaultUri
```

### <a name="step-5-copy-data-to-storage-account"></a>Paso 5: Copia de los datos en una cuenta de almacenamiento
Para transferir datos a la nueva cuenta de almacenamiento de modo que estén cifrados. Para obtener más información, consulte [Preguntas frecuentes sobre el Cifrado del servicio Storage](storage-service-encryption.md#faq-for-storage-service-encryption).

### <a name="step-6-query-the-status-of-the-encrypted-data"></a>Paso 6: Consulta del estado de los datos cifrados
Consulte el estado de los datos cifrados.

## <a name="faq-for-sse-with-customer-managed-keys"></a>Preguntas más frecuentes sobre SSE con claves administradas por el cliente
**Uso Premium Storage. ¿Puedo usar claves administradas por el cliente con SSE?**  
Sí, SSE con claves administradas por Microsoft y claves administradas el cliente es compatible tanto con almacenamiento estándar como con almacenamiento premium.

**¿Puedo crear cuentas de almacenamiento con SSE con claves administradas por el cliente habilitado a través de Azure PowerShell y la CLI de Azure?**  
Sí.

**¿Cuánto aumenta el costo de Azure Storage si uso las claves administradas por el cliente con SSE?**  
Hay un costo asociado al uso de Azure Key Vault. Para más información, visite [Precios de Key Vault](https://azure.microsoft.com/pricing/details/key-vault/). No hay ningún costo adicional por SSE, que está habilitado para todas las cuentas de almacenamiento.

**¿Está disponible Storage Service Encryption en Azure Managed Disks?**  
Storage Service Encryption está disponible para Azure Managed Disks con claves administradas por Microsoft, pero no con claves administradas por el cliente. En lugar de Managed Disks con soporte SSE con claves administradas por el cliente, se recomienda [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md), que usa el estándar del sector [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) en Windows y [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) en Linux para proporcionar cifrado integrado con KeyVault.

**¿En qué se diferencia Storage Service Encryption de Azure Disk Encryption?**  
Azure Disk Encryption proporciona integración entre soluciones basadas en el sistema operativo, como BitLocker y DM-Crypt, y Azure Key Vault. Storage Service Encryption proporciona cifrado de forma nativa en la capa de la plataforma de almacenamiento de Azure, debajo de la máquina virtual.

**¿Puedo revocar el acceso a las claves de cifrado?**
Sí, puede revocar el acceso en cualquier momento. Hay varias maneras para revocar el acceso a las claves. Para más información, consulte [PowerShell de Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) y la [CLI de Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). La revocación del acceso bloqueará eficazmente el acceso a todos los blobs de la cuenta de almacenamiento, ya que Azure Storage no podrá acceder a la clave de cifrado de la cuenta.

**¿Puedo crear una cuenta de almacenamiento y un clave en una región diferente?**  
No, la cuenta de almacenamiento, Azure Key Vault y la clave deben estar en la misma región.

**¿Puedo habilitar claves administradas por el cliente para SSE durante la creación de la cuenta de almacenamiento?**  
No. Cuando crea la cuenta de almacenamiento, solo las claves administradas por Microsoft están disponibles para SSE. Para usar claves administradas por el cliente, debe actualizar las propiedades de la cuenta de almacenamiento. Puede usar REST o una de las bibliotecas de cliente de almacenamiento para actualizar mediante programación la cuenta de almacenamiento, o actualice las propiedades de la cuenta de almacenamiento mediante Azure Portal después de crear la cuenta.

**¿Puedo deshabilitar el cifrado mientras claves administradas por el cliente con SSE?**  
No, no puede deshabilitar el cifrado. El cifrado está habilitado de forma predeterminada para Azure Blob Storage, Azure Files, Azure Queue y Azure Table Storage. De manera opcional, puede pasar de usar las claves administradas por Microsoft a usar las claves administradas por el cliente y viceversa.

**¿Está habilitado SSE cuando creo una cuenta de almacenamiento?**  
SSE está habilitado para todas las cuentas de almacenamiento y para Azure Blob Storage, Azure Files, Azure Queue Storage y Azure Table Storage.

**No puedo habilitar SSE mediante las claves administradas por el cliente en mi cuenta de almacenamiento.**  
¿Es una cuenta de almacenamiento de Azure Resource Manager? Las cuentas de almacenamiento clásicas no son compatibles con las claves administradas por el cliente. SSE con claves administradas por el cliente solo se pueden habilitar en las cuentas de almacenamiento de Resource Manager.

**¿Qué son las características Eliminación temporal y No purgar? ¿Tengo que habilitarlas para usar SSE con claves administradas por el cliente?**  
Las características Eliminación temporal y No purgar se deben habilitar para usar SSE con claves administradas por el cliente. Estos valores garantizan que la clave no se elimine de manera accidental ni intencional. El período de retención máximo de las claves se establece en 90 días, lo que protege a los usuarios frente a actores malintencionados y ataques de ransomware. Esta configuración no se puede deshabilitar.

**¿Solo se permite SSE con claves administradas por el cliente en determinadas regiones?**  
SSE con claves administradas por el cliente está disponible en todas las regiones para Azure Blob Storage y Azure Files.

**¿Cómo me puedo comunicar con alguna persona si tengo problemas o si deseo enviar comentarios?**  
Envíe un mensaje de correo electrónico a [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com) si tiene algún problema relacionado con Storage Service Encryption.

## <a name="next-steps"></a>Pasos siguientes
- Para más información sobre el conjunto completo de funcionalidades de seguridad que ayudan a los desarrolladores a crear aplicaciones seguras, consulte la [Guía de seguridad de Azure Storage](storage-security-guide.md).
- Para información general sobre Azure Key Vault, consulte [¿Qué es Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-whatis)
- Para una introducción sobre Azure Key Vault, consulte [Introducción al Almacén de claves de Azure](https://docs.microsoft.com/azure/key-vault/key-vault-get-started).
