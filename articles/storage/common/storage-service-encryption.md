---
title: Cifrado de Azure Storage para datos en reposo | Microsoft Docs
description: Azure Storage protege los datos cifrándolos automáticamente antes de guardarlos en la nube. Puede confiar en las claves administradas por Microsoft para el cifrado de la cuenta de almacenamiento, o puede administrar el cifrado con sus propias claves.
services: storage
author: tamram
ms.service: storage
ms.date: 10/02/2019
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: cfac7fdbbdbf06ae74385fbc33e61d11cb99ff87
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74066315"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Cifrado de Azure Storage para datos en reposo

Azure Storage cifra automáticamente los datos al guardarlos en la nube. Mediante el cifrado, se protegen los datos y es más fácil cumplir los compromisos de cumplimiento y seguridad de la organización. Los datos de Azure Storage se cifran y descifran de forma transparente mediante el [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles, y son compatibles con FIPS 140-2. El cifrado de Azure Storage es similar al cifrado de BitLocker en Windows.

El cifrado de Azure Storage está habilitado en todas las cuentas de almacenamiento nuevas y no se puede deshabilitar. Como los datos están protegidos de forma predeterminada, no es necesario modificar el código o las aplicaciones para aprovechar el cifrado de Azure Storage.

Las cuentas de almacenamiento se cifran independientemente de su nivel de rendimiento (Estándar o Premium) o del modelo de implementación (Azure Resource Manager o clásico). Todas las opciones de redundancia de Azure Storage admiten el cifrado y se cifran todas las copias de una cuenta de almacenamiento. Se cifran todos los recursos de Azure Storage, incluidos los blobs, los discos, los archivos, las colas y las tablas. También se cifran todos los metadatos de objetos.

El cifrado no afecta al rendimiento de Azure Storage. No hay ningún costo adicional para el cifrado de Azure Storage.

Para más información sobre de los módulos criptográficos subyacentes al cifrado de Azure Storage, vea [API de criptografía: última generación](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

Puede confiar en las claves administradas por Microsoft para el cifrado de la cuenta de almacenamiento, o puede administrar el cifrado con sus propias claves. Si opta por administrar el cifrado con sus propias claves, tiene dos opciones:

- Puede especificar una *clave administrada por el cliente* para utilizarla para el cifrado y descifrado de todos los datos de la cuenta de almacenamiento. Una clave administrada por el cliente se usa para cifrar todos los datos de todos los servicios de la cuenta de almacenamiento.
- En las operaciones de almacenamiento de blobs, puede especificar una *clave proporcionada por el cliente*. Un cliente que realiza una solicitud de lectura o escritura en el almacenamiento de blobs puede incluir una clave de cifrado en la solicitud para tener un control detallado sobre el cifrado y el descifrado de los datos de blob.

En la tabla siguiente se comparan las opciones de administración de claves para el cifrado de Azure Storage.

|                                        |    Claves administradas por Microsoft                             |    Claves administradas por el cliente                                                                                                                        |    Claves proporcionadas por el cliente                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Operaciones de cifrado y descifrado    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Servicios de Azure Storage admitidos    |    All                                                |    Blob Storage, Azure Files                                                                                                               |    Almacenamiento de blobs                                                                  |
|    Almacenamiento de claves                         |    Almacén de claves de Microsoft    |    Azure Key Vault                                                                                                                              |    Azure Key Vault o cualquier otro almacén de claves                                                                 |
|    Responsabilidad de la rotación de claves         |    Microsoft                                          |    Cliente                                                                                                                                     |    Cliente                                                                      |
|    Uso de las claves                           |    Microsoft                                          |    Azure Portal, API REST del proveedor de recursos de almacenamiento, bibliotecas de administración de Azure Storage, PowerShell, CLI        |    API REST de Azure Storage (Blob Storage), bibliotecas de cliente de Azure Storage    |
|    Acceso a la clave                          |    Solo Microsoft                                     |    Microsoft, cliente                                                                                                                    |    Solo el cliente                                                                 |

En las secciones siguientes se describe cada una de las opciones de administración de claves con mayor detalle.

## <a name="microsoft-managed-keys"></a>Claves administradas por Microsoft

De forma predeterminada, la cuenta de almacenamiento usa claves de cifrado administradas por Microsoft. Puede ver la configuración de cifrado para la cuenta de almacenamiento en la sección **Cifrado** de [Azure Portal](https://portal.azure.com), tal y como se muestra en esta imagen.

![Ver cuenta cifrada con claves administradas por Microsoft](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

## <a name="customer-managed-keys"></a>Claves administradas por el cliente

Puede administrar el cifrado de Azure Storage en el nivel de la cuenta de almacenamiento con sus propias claves. Cuando especifica una clave administrada por el cliente en el nivel de la cuenta de almacenamiento, esa clave se usa para cifrar y descifrar todos los datos de la cuenta de almacenamiento, incluidos los datos de blobs, colas, archivos y tablas.  Las claves administradas por el cliente ofrecen más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. También permite auditar las claves de cifrado que se usan para proteger los datos.

Debe usar Azure Key Vault para almacenar las claves administradas por el cliente. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región, pero pueden estar en distintas suscripciones. Para más información sobre Azure Key Vault, vea [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md).

En este diagrama se muestra cómo Azure Storage usa Azure Active Directory y Azure Key Vault para hacer solicitudes mediante la clave administrada por el cliente:

![Diagrama que muestra cómo funcionan las claves administradas por el cliente en Azure Storage](media/storage-service-encryption/encryption-customer-managed-keys-diagram.png)

En la lista siguiente se explican los pasos numerados del diagrama:

1. Un administrador de Azure Key Vault concede permisos sobre las claves de cifrado a la identidad administrada que está asociada a la cuenta de almacenamiento.
2. Un administrador de Azure Storage configura el cifrado con una clave administrada por el cliente para la cuenta de almacenamiento.
3. Azure Storage usa la identidad administrada que está asociada a la cuenta de almacenamiento para autenticar el acceso a Azure Key Vault mediante Azure Active Directory.
4. Azure Storage encapsula la clave de cifrado de la cuenta con la clave de cliente en Azure Key Vault.
5. En operaciones de lectura y escritura, Azure Storage envía solicitudes a Azure Key Vault para encapsular y desencapsular la clave de cifrado de la cuenta para realizar operaciones de cifrado y descifrado.

Para revocar el acceso a las claves administradas por el cliente en la cuenta de almacenamiento, consulte [PowerShell de Azure Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/) y [CLI de Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los datos de la cuenta de almacenamiento, ya que Azure Storage no puede acceder a la clave de cifrado.

Las claves administradas por el cliente también están disponibles para Azure Managed Disks como versión preliminar pública; las claves administradas por el cliente funcionan de forma un poco diferente en los discos administrados que en el resto de opciones de almacenamiento. Para obtener más información, consulte nuestro [artículo al respecto](../../virtual-machines/linux/disk-encryption.md#customer-managed-keys-public-preview).

Para aprender a usar claves administradas por el cliente con Azure Storage, vea uno de estos artículos:

- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde Azure Portal](storage-encryption-keys-portal.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell](storage-encryption-keys-powershell.md)
- [Usar las claves administradas por el cliente con el cifrado de Azure Storage desde la CLI de Azure](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Las claves administradas por el cliente dependen de identidades administradas para los recursos de Azure, una característica de Azure Active Directory (Azure AD). Al configurar las claves administradas por el cliente en Azure Portal, se asigna automáticamente una identidad administrada a la cuenta de almacenamiento en segundo plano. Si posteriormente mueve la suscripción, el grupo de recursos o la cuenta de almacenamiento de un directorio de Azure AD a otro, la identidad administrada asociada a la cuenta de almacenamiento no se transfiere al nuevo inquilino, por lo que es posible que las claves administradas por el cliente dejen de funcionar. Para más información, vea **Transferencia de una suscripción entre directorios de Azure AD** en [Preguntas frecuentes y problemas conocidos con identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

## <a name="customer-provided-keys-preview"></a>Claves proporcionadas por el cliente (versión preliminar)

Los clientes que realizan solicitudes en Azure Blob Storage tienen la opción de proporcionar una clave de cifrado en una solicitud individual. La inclusión de la clave de cifrado en la solicitud proporciona un control detallado sobre la configuración de cifrado para las operaciones de almacenamiento de blobs. Las claves proporcionadas por el cliente (versión preliminar) se pueden almacenar en Azure Key Vault o en otro almacén de claves.

### <a name="encrypting-read-and-write-operations"></a>Cifrado de operaciones de lectura y escritura

Cuando una aplicación cliente proporciona una clave de cifrado en la solicitud, Azure Storage realiza el cifrado y el descifrado de forma transparente mientras lee y escribe datos de blobs. Se escribe un hash SHA-256 de la clave de cifrado junto con el contenido de un blob y se usa para comprobar que todas las operaciones posteriores en el blob utilizan la misma clave de cifrado. Azure Storage no almacena ni administra la clave de cifrado que el cliente envía con la solicitud. La clave se descarta de forma segura en cuanto se completa el proceso de cifrado o descifrado.

Cuando un cliente crea o actualiza un blob con una clave proporcionada por el cliente, las solicitudes de lectura y escritura posteriores para ese blob también deben proporcionar la clave. Si no se proporciona la clave en una solicitud para un blob que ya se ha cifrado con una clave proporcionada por el cliente, se produce un error en la solicitud con el código de error 409 (conflicto).

Si la aplicación cliente envía una clave de cifrado en la solicitud, y la cuenta de almacenamiento también está cifrada mediante una clave administrada por Microsoft o una clave administrada por el cliente, Azure Storage usa la clave proporcionada en la solicitud para el cifrado y el descifrado.

Para enviar la clave de cifrado como parte de la solicitud, un cliente debe establecer una conexión segura con Azure Storage mediante HTTPS.

Cada instantánea de blob puede tener su propia clave de cifrado.

### <a name="request-headers-for-specifying-customer-provided-keys"></a>Encabezados de solicitud para especificar las claves proporcionadas por el cliente

En el caso de las llamadas REST, los clientes pueden usar los siguientes encabezados para pasar de forma segura la información de clave de cifrado de una solicitud a Blob Storage:

|Encabezado de solicitud | DESCRIPCIÓN |
|---------------|-------------|
|`x-ms-encryption-key` |Se requiere para solicitudes de lectura y escritura. Valor de clave de cifrado AES-256 con codificación Base64. |
|`x-ms-encryption-key-sha256`| Se requiere para solicitudes de lectura y escritura. SHA256 con codificación Base64 de la clave de cifrado. |
|`x-ms-encryption-algorithm` | Se requiere para solicitudes de escritura, opcional para solicitudes de lectura. Especifica el algoritmo que se va a usar al cifrar datos con la clave especificada. Debe ser AES256. |

La especificación de claves de cifrado en la solicitud es opcional. Sin embargo, si especifica uno de los encabezados enumerados anteriormente para una operación de escritura, deberá especificarlos todos.

### <a name="blob-storage-operations-supporting-customer-provided-keys"></a>Operaciones de almacenamiento de blobs que admiten claves proporcionadas por el cliente

Las siguientes operaciones de almacenamiento de blobs admiten el envío de claves de cifrado proporcionadas por el cliente en una solicitud:

- [Put Blob](/rest/api/storageservices/put-blob)
- [Put Block List](/rest/api/storageservices/put-block-list)
- [Put Block](/rest/api/storageservices/put-block)
- [Put Block from URL](/rest/api/storageservices/put-block-from-url)
- [Put Page](/rest/api/storageservices/put-page)
- [Put Page from URL](/rest/api/storageservices/put-page-from-url) (Poner página de dirección URL)
- [Append Block](/rest/api/storageservices/append-block)
- [Set Blob Properties](/rest/api/storageservices/set-blob-properties)
- [Set Blob Metadata](/rest/api/storageservices/set-blob-metadata)
- [Get Blob](/rest/api/storageservices/get-blob)
- [Get Blob Properties](/rest/api/storageservices/get-blob-properties)
- [Get Blob Metadata](/rest/api/storageservices/get-blob-metadata)
- [Snapshot Blob](/rest/api/storageservices/snapshot-blob)

### <a name="rotate-customer-provided-keys"></a>Rotación de claves proporcionadas por el cliente

Para rotar una clave de cifrado pasada en la solicitud, descargue el blob y vuelva a cargarlo con la nueva clave de cifrado.

> [!IMPORTANT]
> Azure Portal no se puede usar para leer o escribir en un contenedor o blob que esté cifrado con una clave proporcionada en la solicitud.
>
> Asegúrese de proteger la clave de cifrado que proporcione en una solicitud a Blob Storage en un almacén de claves seguro como Azure Key Vault. Si intenta realizar una operación de escritura en un contenedor o blob sin la clave de cifrado, se producirá un error en la operación y se perderá el acceso al objeto.

### <a name="example-use-a-customer-provided-key-to-upload-a-blob-in-net"></a>Ejemplo: Uso de una clave proporcionada por el cliente para cargar un blob en .NET

En el ejemplo siguiente, se crea una clave proporcionada por el cliente y se usa esa clave para cargar un blob. El código carga un bloque y luego confirma la lista de bloques para escribir el blob en Azure Storage. La clave se proporciona en el objeto [BlobRequestOptions](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions) estableciendo la propiedad [CustomerProvidedKey](/dotnet/api/microsoft.azure.storage.blob.blobrequestoptions.customerprovidedkey).

La clave se crea con la clase [AesCryptoServiceProvider](/dotnet/api/system.security.cryptography.aescryptoserviceprovider). Para crear una instancia de esta clase en el código, agregue una instrucción `using` que haga referencia al espacio de nombres `System.Security.Cryptography`:

```csharp
public static void UploadBlobWithClientKey(CloudBlobContainer container)
{
    // Create a new key using the Advanced Encryption Standard (AES) algorithm.
    AesCryptoServiceProvider keyAes = new AesCryptoServiceProvider();

    // Specify the key as an option on the request.
    BlobCustomerProvidedKey customerProvidedKey = new BlobCustomerProvidedKey(keyAes.Key);
    var options = new BlobRequestOptions
    {
        CustomerProvidedKey = customerProvidedKey
    };

    string blobName = "sample-blob-" + Guid.NewGuid();
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(blobName);

    try
    {
        // Create an array of random bytes.
        byte[] buffer = new byte[1024];
        Random rnd = new Random();
        rnd.NextBytes(buffer);

        using (MemoryStream sourceStream = new MemoryStream(buffer))
        {
            // Write the array of random bytes to a block.
            int blockNumber = 1;
            string blockId = Convert.ToBase64String(Encoding.ASCII.GetBytes(string.Format("BlockId{0}",
                blockNumber.ToString("0000000"))));

            // Write the block to Azure Storage.
            blockBlob.PutBlock(blockId, sourceStream, null, null, options, null);

            // Commit the block list to write the blob.
            blockBlob.PutBlockList(new List<string>() { blockId }, null, options, null);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Cifrado de Azure Storage frente a cifrado de disco

El cifrado de Azure Storage cifra los blobs en páginas que respaldan los discos de máquinas virtuales de Azure. Además, existe la posibilidad de cifrar todos los discos de máquinas virtuales de Azure, incluidos los discos temporales locales, con [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md). Azure Disk Encryption usa el estándar del sector [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) en Windows y [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) en Linux para proporcionar soluciones de cifrado basado en el sistema que se integran con Azure Key Vault.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde Azure Portal](storage-encryption-keys-portal.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell](storage-encryption-keys-powershell.md)
- [Configuración de las claves administradas por el cliente para el cifrado de Azure Storage desde la CLI de Azure](storage-encryption-keys-cli.md)
