---
title: Cifrado de Azure Storage para datos en reposo
description: Azure Storage protege los datos cifrándolos automáticamente antes de guardarlos en la nube. Puede confiar en las claves administradas por Microsoft para el cifrado de la cuenta de almacenamiento, o puede administrar el cifrado con sus propias claves.
services: storage
author: tamram
ms.service: storage
ms.date: 02/05/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 86d6a63601036abdde4ee7ae73114566d749feca
ms.sourcegitcommit: cfbea479cc065c6343e10c8b5f09424e9809092e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2020
ms.locfileid: "77082838"
---
# <a name="azure-storage-encryption-for-data-at-rest"></a>Cifrado de Azure Storage para datos en reposo

Azure Storage cifra automáticamente los datos al guardarlos en la nube. El cifrado de Azure Storage protege los datos y le ayuda a satisfacer los requisitos de cumplimiento normativo y seguridad de la organización.

## <a name="about-azure-storage-encryption"></a>Acerca del cifrado de Azure Storage

Los datos de Azure Storage se cifran y descifran de forma transparente mediante el [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más sólidos que hay disponibles, y son compatibles con FIPS 140-2. El cifrado de Azure Storage es similar al cifrado de BitLocker en Windows.

El cifrado de Azure Storage está habilitado para todas las cuentas de almacenamiento nuevas, incluidas las cuentas de almacenamiento clásicas y las de Resource Manager. El cifrado de Azure Storage no se puede deshabilitar. Como los datos están protegidos de forma predeterminada, no es necesario modificar el código o las aplicaciones para aprovechar el cifrado de Azure Storage.

Las cuentas de almacenamiento se cifran independientemente de su nivel de rendimiento (Estándar o Premium) o del modelo de implementación (Azure Resource Manager o clásico). Todas las opciones de redundancia de Azure Storage admiten el cifrado y se cifran todas las copias de una cuenta de almacenamiento. Se cifran todos los recursos de Azure Storage, incluidos los blobs, los discos, los archivos, las colas y las tablas. También se cifran todos los metadatos de objetos.

El cifrado no afecta al rendimiento de Azure Storage. No hay ningún costo adicional para el cifrado de Azure Storage.

Todos los blobs en bloques, blobs en anexos o blobs en páginas que se escribieron en Azure Storage después del 20 de octubre de 2017 está cifrados. Los blobs creados antes de esta fecha se siguen cifrando mediante un proceso en segundo plano. Para forzar el cifrado de un blob creado antes del 20 de octubre de 2017, puede volver a escribir el blob. Para más información sobre cómo comprobar el estado de cifrado de un blob, consulte [Comprobación del estado de cifrado de un blob](../blobs/storage-blob-encryption-status.md).

Para más información sobre de los módulos criptográficos subyacentes al cifrado de Azure Storage, vea [API de criptografía: última generación](https://docs.microsoft.com/windows/desktop/seccng/cng-portal).

## <a name="about-encryption-key-management"></a>Información sobre la administración de claves de cifrado

Puede confiar en las claves administradas por Microsoft para el cifrado de la cuenta de almacenamiento, o puede administrar el cifrado con sus propias claves. Si opta por administrar el cifrado con sus propias claves, tiene dos opciones:

- Puede especificar una *clave administrada por el cliente* con Azure Key Vault para utilizarla para el cifrado y descifrado de datos en Blob Storage y en Azure Files.<sup>1,2</sup>
- En las operaciones de almacenamiento de blobs, puede especificar una *clave proporcionada por el cliente*. Un cliente que realiza una solicitud de lectura o escritura en el almacenamiento de blobs puede incluir una clave de cifrado en la solicitud para tener un control detallado sobre el cifrado y el descifrado de los datos de blob.

En la tabla siguiente se comparan las opciones de administración de claves para el cifrado de Azure Storage.

|                                        |    Claves administradas por Microsoft                             |    Claves administradas por el cliente                                                                                                                        |    Claves proporcionadas por el cliente                                                          |
|----------------------------------------|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|    Operaciones de cifrado y descifrado    |    Azure                                              |    Azure                                                                                                                                        |    Azure                                                                         |
|    Servicios de Azure Storage admitidos    |    All                                                |    Blob Storage, Azure Files<sup>1,2</sup>                                                                                                               |    Almacenamiento de blobs                                                                  |
|    Almacenamiento de claves                         |    Almacén de claves de Microsoft    |    Azure Key Vault                                                                                                                              |    Azure Key Vault o cualquier otro almacén de claves                                                                 |
|    Responsabilidad de la rotación de claves         |    Microsoft                                          |    Customer                                                                                                                                     |    Customer                                                                      |
|    Uso de las claves                           |    Microsoft                                          |    Azure Portal, API REST del proveedor de recursos de almacenamiento, bibliotecas de administración de Azure Storage, PowerShell, CLI        |    API REST de Azure Storage (Blob Storage), bibliotecas de cliente de Azure Storage    |
|    Acceso a la clave                          |    Solo Microsoft                                     |    Microsoft, cliente                                                                                                                    |    Solo el cliente                                                                 |

<sup>1</sup> Para obtener información sobre cómo crear una cuenta que admita el uso de claves administradas por el cliente con Queue Storage, consulte [Creación de una cuenta que admita las claves administradas por el cliente para colas](account-encryption-key-create.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).<br />
<sup>2</sup> Para obtener información sobre cómo crear una cuenta que admita el uso de claves administradas por el cliente con Table Storage, consulte [Creación de una cuenta que admita las claves administradas por el cliente para tablas](account-encryption-key-create.md?toc=%2fazure%2fstorage%2ftables%2ftoc.json).

En las secciones siguientes se describe cada una de las opciones de administración de claves con mayor detalle.

## <a name="microsoft-managed-keys"></a>Claves administradas por Microsoft

De forma predeterminada, la cuenta de almacenamiento usa claves de cifrado administradas por Microsoft. Puede ver la configuración de cifrado para la cuenta de almacenamiento en la sección **Cifrado** de [Azure Portal](https://portal.azure.com), tal y como se muestra en esta imagen.

![Ver cuenta cifrada con claves administradas por Microsoft](media/storage-service-encryption/encryption-microsoft-managed-keys.png)

## <a name="customer-managed-keys-with-azure-key-vault"></a>Claves administradas por el cliente con Azure Key Vault

El cifrado de Azure Storage se puede administrar en el nivel de la cuenta de almacenamiento con sus propias claves. Cuando especifica una clave administrada por el cliente en el nivel de la cuenta de almacenamiento, esa clave se usa para proteger y controlar el acceso a la clave de cifrado raíz para la cuenta de almacenamiento que, a su vez, se usa para cifrar y descifrar todos los datos de archivos y de blob. Las claves administradas por el cliente ofrecen más flexibilidad para crear, rotar, deshabilitar y revocar controles de acceso. También permite auditar las claves de cifrado que se usan para proteger los datos.

Debe usar Azure Key Vault para almacenar las claves administradas por el cliente. Puede crear sus propias claves y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generarlas. La cuenta de almacenamiento y el almacén de claves deben estar en la misma región y en el mismo inquilino de Azure Active Directory (Azure AD), pero pueden estar en distintas suscripciones. Para más información sobre Azure Key Vault, vea [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md).

En este diagrama se muestra cómo Azure Storage usa Azure Active Directory y Azure Key Vault para hacer solicitudes mediante la clave administrada por el cliente:

![Diagrama que muestra cómo funcionan las claves administradas por el cliente en Azure Storage](media/storage-service-encryption/encryption-customer-managed-keys-diagram.png)

En la lista siguiente se explican los pasos numerados del diagrama:

1. Un administrador de Azure Key Vault concede permisos sobre las claves de cifrado a la identidad administrada que está asociada a la cuenta de almacenamiento.
2. Un administrador de Azure Storage configura el cifrado con una clave administrada por el cliente para la cuenta de almacenamiento.
3. Azure Storage usa la identidad administrada que está asociada a la cuenta de almacenamiento para autenticar el acceso a Azure Key Vault mediante Azure Active Directory.
4. Azure Storage encapsula la clave de cifrado de la cuenta con la clave de cliente en Azure Key Vault.
5. En operaciones de lectura y escritura, Azure Storage envía solicitudes a Azure Key Vault para encapsular y desencapsular la clave de cifrado de la cuenta para realizar operaciones de cifrado y descifrado.

### <a name="enable-customer-managed-keys-for-a-storage-account"></a>Habilitación de claves administradas por el cliente para una cuenta de almacenamiento

Al habilitar el cifrado con claves administradas por el cliente para una cuenta de almacenamiento, Azure Storage encapsula la clave de cifrado de la cuenta con la clave administrada por el cliente en el almacén de claves asociado. La habilitación de claves administradas por el cliente no afecta al rendimiento, y la cuenta se cifra con la nueva clave inmediatamente sin retraso alguno.

Las cuentas de almacenamiento nuevas siempre se cifran mediante claves administradas por Microsoft. No es posible habilitar claves administradas por el cliente en el momento en que se crea la cuenta. Las claves administradas por el cliente se almacenan en Azure Key Vault, y el almacén de claves se debe aprovisionar con directivas de acceso que concedan permisos de clave a la identidad administrada que está asociada a la cuenta de almacenamiento. La identidad administrada solo está disponible después de crear la cuenta de almacenamiento.

Al modificar la clave que se usa para el cifrado de Azure Storage habilitando o deshabilitando claves administradas por el cliente, actualizando la versión de clave o especificando una clave diferente, el cifrado de la clave raíz cambia, pero los datos de su cuenta de Azure Storage no tienen que volver a cifrarse.

Para aprender a usar claves administradas por el cliente con Azure Key Vault para el cifrado de Azure Storage, consulte uno de estos artículos:

- [Configuración de claves administradas por el cliente con Key Vault para el cifrado de Azure Storage mediante Azure Portal](storage-encryption-keys-portal.md)
- [Configuración de claves administradas por el cliente con Key Vault para el cifrado de Azure Storage mediante PowerShell](storage-encryption-keys-powershell.md)
- [Configuración de claves administradas por el cliente con Key Vault para el cifrado de Azure Storage mediante la CLI de Azure](storage-encryption-keys-cli.md)

> [!IMPORTANT]
> Las claves administradas por el cliente dependen de identidades administradas para los recursos de Azure, una característica de Azure AD. Las identidades administradas no admiten actualmente escenarios entre directorios. Al configurar las claves administradas por el cliente en Azure Portal, se asigna automáticamente una identidad administrada a la cuenta de almacenamiento en segundo plano. Si posteriormente mueve la suscripción, el grupo de recursos o la cuenta de almacenamiento de un directorio de Azure AD a otro, la identidad administrada asociada a la cuenta de almacenamiento no se transfiere al nuevo inquilino, por lo que es posible que las claves administradas por el cliente dejen de funcionar. Para más información, vea **Transferencia de una suscripción entre directorios de Azure AD** en [Preguntas frecuentes y problemas conocidos con identidades administradas para recursos de Azure](../../active-directory/managed-identities-azure-resources/known-issues.md#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Almacenamiento de claves administradas por el cliente en Azure Key Vault

Para habilitar las claves administradas por el cliente en una cuenta de almacenamiento, debe usar un almacén de Azure Key Vault para almacenar las claves. Debe habilitar las propiedades **Eliminación temporal** y **No purgar** en el almacén de claves.

Solo se admiten claves RSA de tamaño 2048 con el cifrado de Azure Storage. Para más información acerca de las claves, consulte la sección **Claves en Key Vault** en [Información acerca de claves, secretos y certificados de Azure Key Vault](../../key-vault/about-keys-secrets-and-certificates.md#key-vault-keys).

### <a name="rotate-customer-managed-keys"></a>Rotación de claves administradas por el cliente

Las claves administradas por el cliente se pueden rotar en Azure Key Vault según las directivas de cumplimiento. Cuando la clave rota, hay que actualizar la cuenta de almacenamiento para usar el nuevo identificador URI de la clave. Para aprender a actualizar la cuenta de almacenamiento para usar una nueva versión de la clave en Azure Portal, consulte la sección **Actualización de la versión de la clave** en [Configuración de claves administradas por el cliente para Azure Storage mediante Azure Portal](storage-encryption-keys-portal.md).

La rotación de la clave no desencadena un nuevo cifrado de los datos en la cuenta de almacenamiento. No es preciso que el usuario realice ninguna otra acción.

### <a name="revoke-access-to-customer-managed-keys"></a>Revocación del acceso a las claves administradas por el cliente

Para revocar el acceso a las claves administradas por el cliente, use PowerShell o la CLI de Azure. Para más información, consulte la referencia de [PowerShell para Azure Key Vault](/powershell/module/az.keyvault//) o la referencia de la [CLI para Azure Key Vault](/cli/azure/keyvault). La revocación del acceso bloquea de manera eficaz el acceso a todos los datos de la cuenta de almacenamiento, ya que Azure Storage no puede acceder a la clave de cifrado.

### <a name="customer-managed-keys-for-azure-managed-disks-preview"></a>Claves administradas por el cliente para discos administrados por Azure (versión preliminar)

Las claves administradas por el cliente también están disponibles para administrar el cifrado de discos administrados por Azure (versión preliminar). Las claves administradas por el cliente se comportan de forma diferente en los discos administrados que en los recursos de Azure Storage. Para más información, consulte [Cifrado del lado servidor de Azure Managed Disks](../../virtual-machines/windows/disk-encryption.md) para Windows o [Cifrado del lado servidor de Azure Managed Disks](../../virtual-machines/linux/disk-encryption.md) para Linux.

## <a name="customer-provided-keys-preview"></a>Claves proporcionadas por el cliente (versión preliminar)

Los clientes que realizan solicitudes en Azure Blob Storage tienen la opción de proporcionar una clave de cifrado en una solicitud individual. La inclusión de la clave de cifrado en la solicitud proporciona un control detallado sobre la configuración de cifrado para las operaciones de almacenamiento de blobs. Las claves proporcionadas por el cliente (versión preliminar) se pueden almacenar en Azure Key Vault o en otro almacén de claves.

Si desea ver un ejemplo en el que se muestra cómo especificar una clave proporcionada por el cliente en una solicitud para Blob Storage, consulte [Especificación de una clave proporcionada por el cliente en una solicitud para Blob Storage con .NET](../blobs/storage-blob-customer-provided-key.md). 

### <a name="encrypting-read-and-write-operations"></a>Cifrado de operaciones de lectura y escritura

Cuando una aplicación cliente proporciona una clave de cifrado en la solicitud, Azure Storage realiza el cifrado y el descifrado de forma transparente mientras lee y escribe datos de blobs. Azure Storage escribe un hash SHA-256 de la clave de cifrado junto con el contenido del blob. El hash se utiliza para comprobar que todas las operaciones posteriores en el blob usan la misma clave de cifrado. 

Azure Storage no almacena ni administra la clave de cifrado que el cliente envía con la solicitud. La clave se descarta de forma segura en cuanto se completa el proceso de cifrado o descifrado.

Cuando un cliente crea o actualiza un blob con una clave proporcionada por el cliente, las solicitudes de lectura y escritura posteriores para ese blob también deben proporcionar la clave. Si no se proporciona la clave en una solicitud para un blob que ya se ha cifrado con una clave proporcionada por el cliente, se produce un error en la solicitud con el código de error 409 (conflicto).

Si la aplicación cliente envía una clave de cifrado en la solicitud, y la cuenta de almacenamiento también está cifrada mediante una clave administrada por Microsoft o una clave administrada por el cliente, Azure Storage usa la clave proporcionada en la solicitud para el cifrado y el descifrado.

Para enviar la clave de cifrado como parte de la solicitud, un cliente debe establecer una conexión segura con Azure Storage mediante HTTPS.

Cada instantánea de blob puede tener su propia clave de cifrado.

### <a name="request-headers-for-specifying-customer-provided-keys"></a>Encabezados de solicitud para especificar las claves proporcionadas por el cliente

En el caso de las llamadas REST, los clientes pueden usar los siguientes encabezados para pasar de forma segura la información de clave de cifrado de una solicitud a Blob Storage:

|Encabezado de solicitud | Descripción |
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

## <a name="azure-storage-encryption-versus-disk-encryption"></a>Cifrado de Azure Storage frente a cifrado de disco

El cifrado de Azure Storage cifra los blobs en páginas que respaldan los discos de máquinas virtuales de Azure. Además, existe la posibilidad de cifrar todos los discos de máquinas virtuales de Azure, incluidos los discos temporales locales, con [Azure Disk Encryption](../../security/azure-security-disk-encryption-overview.md). Azure Disk Encryption usa el estándar del sector [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) en Windows y [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) en Linux para proporcionar soluciones de cifrado basado en el sistema que se integran con Azure Key Vault.

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es Azure Key Vault?](../../key-vault/key-vault-overview.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde Azure Portal](storage-encryption-keys-portal.md)
- [Configurar las claves administradas por el cliente para el cifrado de Azure Storage desde PowerShell](storage-encryption-keys-powershell.md)
- [Configuración de las claves administradas por el cliente para el cifrado de Azure Storage desde la CLI de Azure](storage-encryption-keys-cli.md)
