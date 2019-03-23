---
title: Introducción a la cuenta de Azure Storage | Microsoft Docs
description: Compruebe qué opciones tiene para crear y usar una cuenta de Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/06/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: ca71fdc8074e56adc8595ee905d5b1db3b60cef1
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58371819"
---
# <a name="azure-storage-account-overview"></a>Información general acerca de la cuenta de Azure Storage

Una cuenta de Azure Storage contiene todos los objetos de datos de Azure Storage: blobs, archivos, colas, tablas y discos. Los datos en su cuenta de Azure Storage son duraderos, seguros, están completamente disponibles, puede escalarlos de forma masiva y puede acceder a ellos desde cualquier lugar del mundo a través de HTTP o HTTPS. 

Para obtener información sobre cómo crear una cuenta de Azure Storage, consulte [Crear una cuenta de almacenamiento](storage-quickstart-create-account.md).

## <a name="types-of-storage-accounts"></a>Tipos de cuentas de almacenamiento

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

### <a name="general-purpose-v2-accounts"></a>Cuentas de uso general v2.

Las cuentas de almacenamiento de uso general v2 son compatibles con las últimas características de Azure Storage, e incorporan todas las funcionalidades de las cuentas de Blob Storage y de uso general v1. Las cuentas de uso general v2 ofrecen los precios de capacidad por gigabyte más bajos para Azure Storage, así como los precios de transacción más competitivos del sector. Las cuentas de almacenamiento de uso general v2 son compatibles con estos servicios de Azure Storage:

- Blobs (todos los tipos: en bloques, en páginas y en anexos).
- Archivos
- Discos
- Colas
- Tablas

> [!NOTE]
> Microsoft recomienda usar cuentas de almacenamiento de uso general v2 en la mayoría de los escenarios. Puede actualizar fácilmente una cuenta de Blob Storage o de uso general v1 a una cuenta de uso general v2 sin tiempo de inactividad y sin la necesidad de copiar datos.
>
> Para obtener más información sobre la actualización a una cuenta de uso general v2, consulte [Upgrade to a general-purpose v2 storage account](storage-account-upgrade.md) (Actualizar a una cuenta de almacenamiento de uso general v2).

Las cuentas de almacenamiento de uso general v2 ofrecen varios niveles de acceso para almacenar datos según sus patrones de uso. Para obtener más información, consulte [Access tiers for block blob data](#access-tiers-for-block-blob-data) (Niveles de acceso para los datos del blob en bloques).

### <a name="general-purpose-v1-accounts"></a>Cuentas de uso general v1

Las cuentas de uso general v1 proporcionan acceso a todos los servicios de Azure Storage, pero puede que no incluyan las características más recientes o que no tengan los precios más bajos por gigabyte. Las cuentas de almacenamiento de uso general v1 son compatibles con estos servicios de Azure Storage:

- Blobs (todos los tipos)
- Archivos
- Discos
- Colas
- Tablas

Si bien se recomienda usar las cuentas de uso general v2 en la mayoría de los casos, las cuentas de uso general v1 se adaptan mejor a estos escenarios: 

* Sus aplicaciones requieren el modelo de implementación clásico de Azure. Las cuentas de uso general v2 y las cuentas de Blob Storage solo admiten el modelo de implementación de Azure Resource Manager.

* Sus aplicaciones realizan una gran cantidad de transacciones o utilizan un ancho de banda significativo de replicación geográfica, pero no es necesario que tengan una gran capacidad. En este caso, las cuentas de uso general v1 pueden ser la opción más económica.

* Usa una versión de la [API de REST de servicios de almacenamiento](https://msdn.microsoft.com/library/azure/dd894041.aspx) anterior al 14-02-2014 o una biblioteca de cliente con una versión inferior a la 4.x y no puede actualizar la aplicación.

### <a name="block-blob-storage-accounts"></a>Cuentas de almacenamiento de blobs de bloque

Una cuenta de almacenamiento de blobs de bloque es una cuenta de almacenamiento especializada para almacenar datos de objetos no estructurados como blobs en bloques o blobs en anexos. Las cuentas de almacenamiento de blobs de bloque ofrecen varios niveles de acceso para almacenar los datos según los patrones de uso. Para obtener más información, consulte [Access tiers for block blob data](#access-tiers-for-block-blob-data) (Niveles de acceso para los datos del blob en bloques).

## <a name="naming-storage-accounts"></a>Nomenclatura de las cuentas de almacenamiento

Cuando especifique un nombre para la cuenta de almacenamiento, tenga en cuenta estas reglas:

- Los nombres de las cuentas de almacenamiento deben tener entre 3 y 24 caracteres y solo pueden incluir números y letras en minúscula.
- El nombre de la cuenta de almacenamiento debe ser único dentro de Azure. No puede haber dos cuentas de almacenamiento con el mismo nombre.

## <a name="performance-tiers"></a>Niveles de rendimiento

Las cuentas de almacenamiento de uso general pueden configurarse para cada uno de los niveles de rendimiento siguientes:

* Un nivel de rendimiento estándar para almacenar blobs, archivos, tablas, colas y discos de máquina virtual de Azure.
* Un nivel de rendimiento prémium para almacenar solo los discos de máquina virtual no administrados.

## <a name="access-tiers-for-block-blob-data"></a>Niveles de acceso a datos de blob en bloques

Azure Storage ofrece diferentes opciones para obtener acceso a los datos de blob en bloques, en función de los patrones de uso. Cada nivel de acceso en Azure Storage está optimizado para un patrón particular de uso de datos. Al seleccionar el nivel de acceso adecuado a sus necesidades, puede almacenar sus datos de blob en bloques de la manera más rentable.

Los niveles de acceso disponibles son:

* El nivel de acceso **frecuente**, que está optimizado para el acceso frecuente de objetos en la cuenta de almacenamiento. Obtener acceso a los datos en el nivel de acceso frecuente es más rentable, aunque los costos de almacenamiento son algo más altos. Las nuevas cuentas de almacenamiento se crean en el nivel de acceso frecuente de forma predeterminada.
* El nivel de acceso **esporádico** está optimizado para almacenar grandes cantidades de datos a los que se accede con poca frecuencia y que llevan guardados al menos 30 días. El almacenamiento de datos en el nivel de acceso esporádico es más rentable, pero el acceso a esos datos puede ser algo más costoso que acceder a los datos del nivel de acceso frecuente.
* El nivel de almacenamiento de **archivo** está disponible solo para blobs en bloques individuales. Este nivel está destinado a los datos que pueden tolerar varias horas de latencia de recuperación y que permanecerán en el nivel de almacenamiento de archivo durante un mínimo de 180 días. El nivel de almacenamiento de archivo es el más rentable de todos para almacenar datos, pero el acceso a esos datos es más costoso que acceder a los datos del nivel de acceso frecuente o esporádico. 

Si hay un cambio en el patrón de uso de datos, también se puede cambiar de nivel de acceso en cualquier momento. Para obtener más información acerca de los niveles de acceso, consulte [Azure Blob storage: niveles de acceso de archivo y frecuente, esporádico](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Si cambia el nivel de acceso de una cuenta de almacenamiento o blob existente es posible que deba pagar cargos adicionales. Para más información, consulte la sección [Facturación de la cuenta de almacenamiento](#storage-account-billing).

## <a name="replication"></a>Replicación

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Para más información sobre la replicación del almacenamiento, consulte [Replicación en Azure Storage](storage-redundancy.md).

## <a name="encryption"></a>Cifrado

Todos los datos de su cuenta de almacenamiento se cifran en el mismo servicio. Para obtener más información, consulte [Azure Storage Service Encryption para datos en reposo](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Puntos de conexión de cuenta de almacenamiento

Una cuenta de almacenamiento le proporciona un espacio de nombres único en Azure para sus datos. Cada objeto que almacena en Azure Storage tiene una dirección que incluye su nombre de cuenta único. La combinación del nombre de la cuenta y el punto de conexión del servicio de Azure Storage forma los puntos de conexión de su cuenta de almacenamiento.

Por ejemplo, si la cuenta de almacenamiento de uso general se llama *mystorageaccount*, los puntos de conexión predeterminados de la cuenta de almacenamiento serán:

* Blob Storage: http://*mystorageaccount*.blob.core.windows.net
* Table storage: http://*mystorageaccount*.table.core.windows.net
* Queue storage: http://*mystorageaccount*.queue.core.windows.net
* Azure Files: http://*mystorageaccount*.file.core.windows.net

> [!NOTE]
> Una cuenta de Blob Storage solo expone el punto de conexión de Blob service.

La dirección URL para obtener acceso a un objeto en una cuenta de almacenamiento se crea anexando la ubicación del objeto en la cuenta de almacenamiento al punto de conexión. Por ejemplo, una dirección de blob podría tener este formato: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

También puede configurar una cuenta de almacenamiento para usar un dominio personalizado para los blobs. Para obtener más información, consulte [Configuración de un nombre de dominio personalizado para una cuenta de Azure Storage](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Controlar el acceso a los datos de la cuenta

De forma predeterminada, los datos de su cuenta están disponibles solo para usted, el propietario de la cuenta. Tiene control sobre quién puede acceder a sus datos y qué permisos otorgarle.

Toda solicitud realizada en la cuenta de almacenamiento debe estar autorizada. En el nivel del servicio, la solicitud debe incluir un encabezado de *Autorización* válido, que incluya toda la información necesaria para que el servicio valide la solicitud antes de ejecutarla.

Puede otorgar acceso a los datos de la cuenta de almacenamiento si usa cualquiera de los siguientes métodos:

- **Azure Active Directory:** Usar credenciales de Azure Active Directory (Azure AD) para autenticar un usuario, grupo u otra identidad para el acceso a datos blob y cola. Si la autenticación de una identidad se realiza con éxito, entonces Azure AD devuelve un token para autorizar la solicitud a Azure Blob Storage o a Queue Storage. Para obtener más información, consulte [Autenticación del acceso a Azure Storage con Azure Active Directory](storage-auth-aad.md).
- **Autorización de clave compartida:** use la clave de acceso de la cuenta de almacenamiento para crear una cadena de conexión que la aplicación pueda usar en el tiempo de ejecución para obtener acceso a Azure Storage. Los valores en la cadena de conexión se usan para construir el encabezado *Autorización* que se pasa a Azure Storage. Para obtener más información sobre las cadenas de conexión, consulte [Configuración de las cadenas de conexión de Azure Storage](storage-configure-connection-string.md).
- **Firma de acceso compartido:** use una Firma de acceso compartido para delegar el acceso a los recursos de la cuenta de almacenamiento, si no usa autenticación de Azure AD. Una Firma de acceso compartido es un token que encapsula toda la información necesaria para autorizar una solicitud a Azure Storage en la dirección URL. Puede especificar el recurso de almacenamiento, los permisos concedidos y el intervalo durante el cual los permisos serán válidos como parte de la firma de acceso compartido. Para obtener más información, consulte [Uso de firmas de acceso compartido (SAS)](storage-dotnet-shared-access-signature-part-1.md).

> [!NOTE]
> Si autentica a los usuarios o a las aplicaciones que usan las credenciales de Azure AD, mejorará la seguridad y le será más fácil usar esta opción en lugar de otros medios de autorización. Aunque puede seguir utilizando la autorización con clave compartida con las aplicaciones, el uso de Azure AD evita la necesidad de almacenar su clave de acceso de cuenta con el código. Asimismo, aún puede usar firmas de acceso compartido (SAS) para conceder acceso pormenorizado a los recursos de su cuenta de almacenamiento, pero Azure AD ofrece funcionalidades similares sin necesidad de administrar tokens de SAS ni de preocuparse sobre cómo revocar una SAS en peligro. 
>
> Microsoft recomienda usar autenticación de Azure AD para las aplicaciones de Azure Storage Blob y Queue cuando sea posible.

## <a name="copying-data-into-a-storage-account"></a>Copiar los datos en una cuenta de almacenamiento

Microsoft proporciona herramientas y bibliotecas para importar datos desde dispositivos de almacenamiento local o proveedores de almacenamiento en la nube de terceros. La solución que use dependerá de la cantidad de datos que vaya a transferir. 

Al actualizar a una cuenta de uso general v1 a otra de uso general v2 o a una cuenta Blob Storage, los datos se migran automáticamente. Microsoft recomienda que se usen estos pasos para actualizar la cuenta. Sin embargo, si decide mover datos desde una cuenta de uso general v1 a una cuenta de Blob Storage, deberá migrar manualmente los datos, utilizando las herramientas y bibliotecas que se describen a continuación. 

### <a name="azcopy"></a>AzCopy

AzCopy es una utilidad de línea de comandos de Windows diseñada para la copia de datos de alto rendimiento a y desde Azure Storage. AzCopy se puede usar para copiar datos en una cuenta de Blob Storage desde cuentas de almacenamiento de uso general ya existentes, o para cargar datos desde dispositivos de almacenamiento local. Para más información, vea [Transferencia de datos con la utilidad en línea de comandos AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Data Movement Library

Azure Storage Data Movement Library for .NET se basa en el marco de movimiento de datos principal que se utiliza con AzCopy. La biblioteca está diseñada para operaciones de transferencia de datos de alto rendimiento, confiables y fáciles similares a AzCopy. Se puede usar para sacar partido a las características que proporciona AzCopy en cualquier aplicación de forma nativa, sin tener que ejecutar y supervisar instancias externas de AzCopy. Para más información, consulte [Azure Storage Data Movement Library for .Net](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>API de REST o biblioteca de cliente

Puede crear una aplicación personalizada para migrar sus datos a una cuenta de Almacenamiento de blobs con una de las bibliotecas de cliente de Azure o la API de REST de servicios de almacenamiento de Azure. Azure Storage proporciona bibliotecas de cliente enriquecidas para varios lenguajes y aplicaciones como .NET, Java, C++, Node.JS, PHP, Ruby y Python. Las bibliotecas de cliente ofrecen capacidades avanzadas, como lógica de reintentos, registro y cargas paralelas. También se puede desarrollar directamente en la API de REST, a la que se puede llamar con cualquier lenguaje que genere solicitudes HTTP/HTTPS.

Para obtener más información sobre la API REST de Azure Storage, consulte [Azure Storage Services REST API Reference](https://docs.microsoft.com/rest/api/storageservices/) (Referencia de la API REST de Azure Storage). 

> [!IMPORTANT]
> Los blobs que usan el cifrado de cliente almacenan metadatos relacionados con el cifrado junto con el blob. Si copia un blob en el que se usa un cifrado de cliente, asegúrese de que la operación de copia conserva los metadatos del blob y, especialmente, los metadatos relacionados con el cifrado. Si copia un blob sin los metadatos de cifrado, su contenido no puede volver a recuperarse. Para más información acerca de los metadatos relacionados con el cifrado, consulte [Cifrado del lado de cliente y Almacén de claves de Azure para Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="azure-importexport-service"></a>Servicio Azure Import/Export

Si tiene una gran cantidad de datos para importar a la cuenta de almacenamiento, tenga en cuenta el servicio Azure Import/Export. El servicio Import/Export se usa para importar de forma segura grandes cantidades de datos a Azure Blob Storage y Azure Files mediante el envío de unidades de disco a un centro de datos de Azure. 

También se puede usar este servicio Import/Export para transferir datos desde Azure Blob Storage hasta las unidades de disco y enviarlas a los sitios locales. Se pueden importar los datos de una o varias unidades a Azure Blob Storage o Azure Files. Para obtener más información, vaya a [What is Azure Import/Export service?](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) (¿Qué es el servicio Azure Import/Export?).

## <a name="storage-account-billing"></a>Facturación de la cuenta de almacenamiento

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para obtener información sobre cómo crear una cuenta de Azure Storage, consulte [Crear una cuenta de almacenamiento](storage-quickstart-create-account.md).
* Para administrar o eliminar una cuenta de almacenamiento, consulte [Manage Azure storage accounts](storage-account-manage.md) (Administrar cuentas de Azure Storage).
