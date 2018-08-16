---
title: Azure Storage Service Encryption para datos en reposo | Microsoft Docs
description: Use la característica Azure Storage Service Encryption para cifrar Azure Managed Disks, Azure Blob Storage, Azure Files, Azure Queue Storage y Azure Table Storage en el servicio cuando se almacenan los datos y descifrarlos cuando se recuperan.
services: storage
author: lakasa
ms.service: storage
ms.topic: article
ms.date: 08/01/2018
ms.author: lakasa
ms.component: common
ms.openlocfilehash: 5b676bbc764cb5689a6c80e81f597776fe80413e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520731"
---
# <a name="azure-storage-service-encryption-for-data-at-rest"></a>Azure Storage Service Encryption para datos en reposo
Azure Storage Service Encryption para datos en reposo le ayuda a proteger sus datos con el fin de cumplir con los compromisos de cumplimiento y seguridad de su organización. Con esta característica, la plataforma de almacenamiento de Azure cifra automáticamente los datos antes de almacenarlos en Azure Managed Disks, Azure Blob Storage, Azure Files o Azure Queue Storage y los descifra antes de recuperarlos. El control del cifrado, el cifrado en reposo, el descifrado y la administración de claves en Cifrado del servicio Storage es transparente para los usuarios. Todos los datos escritos en la plataforma de almacenamiento de Azure se cifran mediante [cifrado AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) de 256 bits, uno de los cifrados de bloques más fuertes disponibles.

El Cifrado del servicio Storage está habilitado para todas las cuentas de almacenamiento nuevas y existentes y no se puede deshabilitar. Dado que los datos están protegidos de forma predeterminada, no es necesario modificar el código o las aplicaciones para aprovechar el Cifrado del servicio Storage.

La característica cifra los datos automáticamente en:

- Servicios de Azure Storage:
    - Azure Managed Disks
    - Azure Blob Storage
    - Archivos de Azure
    - Azure Queue Storage
    - Azure Table Storage  
- Ambos niveles de rendimiento (ediciones Estándar y Premium).
- Ambos modelos de implementación (Azure Resource Manager y clásica).

Storage Service Encryption no afecta al rendimiento del almacenamiento de Azure.

Puede usar las claves de cifrado administradas por Microsoft con el Cifrado del servicio Storage, o puede usar sus propias claves de cifrado. Para más información sobre el uso de sus propias claves, consulte [Cifrado del servicio Storage mediante claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

## <a name="view-encryption-settings-in-the-azure-portal"></a>Visualización de la configuración de cifrado en Azure Portal
Para ver la configuración de Cifrado del servicio Storage, inicie sesión en [Azure Portal](https://portal.azure.com) y seleccione una cuenta de almacenamiento. En el panel **Configuración**, seleccione la opción **Cifrado**.

![Captura de pantalla del portal con la configuración de Cifrado](./media/storage-service-encryption/image1.png)

## <a name="faq-for-storage-service-encryption"></a>Preguntas frecuentes sobre el Cifrado del servicio Storage
**¿Cómo cifro los datos de una cuenta de almacenamiento de Resource Manager?**  
Storage Service Encryption está habilitado para todas las cuentas de almacenamiento (clásicas y de Resource Manager) y para los archivos existentes en la cuenta de almacenamiento que creó antes de habilitar el cifrado se cifrarán con carácter retroactivo mediante un proceso de cifrado en segundo plano.

**¿Se habilita Storage Service Encryption de forma predeterminada al crear una cuenta de almacenamiento?**  
Sí, la característica Storage Service Encryption está habilitada para todas las cuentas de almacenamiento y para todos los servicios de almacenamiento de Azure.

**Tengo una cuenta de almacenamiento de Resource Manager. ¿Puedo habilitar en ella el Cifrado del servicio Storage?**  
La característica Storage Service Encryption está habilitada de forma predeterminada en todas las cuentas de almacenamiento existentes de Resource Manager. Esto se aplica a Azure Blob Storage, Azure Files, Azure Queue Storage y Table Storage. 

**¿Puedo deshabilitar el cifrado en mi cuenta de almacenamiento?**  
El cifrado está habilitado de forma predeterminada y no hay ninguna forma de deshabilitarlo en la cuenta de almacenamiento. 

**¿Cuánto aumenta el costo de Azure Storage si se habilita Storage Service Encryption?**  
No hay costo adicional.

**¿Puedo usar mis propias claves de cifrado?**  
Sí, para Azure Blob Storage y Azure Files, puede usar sus propias claves de cifrado. Las claves administradas por el cliente no se admiten actualmente en Azure Managed Disks. Para más información, consulte [Cifrado del servicio Storage mediante claves administradas por el cliente en Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

**¿Puedo revocar el acceso a las claves de cifrado?**  
Sí, si [utiliza sus propias claves de cifrado](storage-service-encryption-customer-managed-keys.md) en Azure Key Vault.

**¿En qué se diferencia Storage Service Encryption de Azure Disk Encryption?**  
Azure Disk Encryption proporciona integración entre soluciones basadas en el sistema operativo, como BitLocker y DM-Crypt, y Azure Key Vault. Storage Service Encryption proporciona cifrado de forma nativa en la capa de la plataforma de almacenamiento de Azure, debajo de la máquina virtual.

**Tengo una cuenta de almacenamiento clásica. ¿Puedo habilitar en ella el Cifrado del servicio Storage?**  
La característica Storage Service Encryption está habilitada para todas las cuentas de almacenamiento (clásicas y de Resource Manager).

**¿Cómo puedo cifrar datos en mi cuenta de almacenamiento clásico?**  
Con el cifrado habilitado de forma predeterminada, cualquier dato almacenado en los servicios de almacenamiento de Azure se cifra automáticamente. 

**¿Puedo crear cuentas de almacenamiento con la característica Storage Service Encryption habilitada mediante Azure PowerShell y la CLI de Azure?**  
La característica Storage Service Encryption se habilita de forma predeterminada en el momento de crear cualquier cuenta de almacenamiento (clásica o de Resource Manager). Puede comprobar las propiedades de la cuenta con Azure PowerShell y la CLI de Azure.

**Mi cuenta de almacenamiento está configurada para la replicación con redundancia geográfica. Con el Cifrado del servicio Storage, ¿también se cifra mi copia redundante?**  
Sí, se cifran todas las copias de la cuenta de almacenamiento. Se admiten todas las opciones de redundancia: almacenamiento con redundancia local, almacenamiento con redundancia geográfica y almacenamiento con redundancia geográfica con acceso de lectura.

**¿Se permite Storage Service Encryption únicamente en determinadas regiones?**  
La característica Storage Service Encryption está disponible en todas las regiones.

**¿Es Storage Service Encryption compatible con FIPS 140-2?**  
Sí, la característica Storage Service Encryption es compatible con FIPS 140-2.

**¿Con quién me puedo comunicar si tengo problemas o quiero proporcionar algunos comentarios?**  
Si tiene algún problema o quiere realizar algún comentario en relación con la característica Storage Service Encryption, envíe un correo electrónico a [ssediscussions@microsoft.com](mailto:ssediscussions@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes
Azure Storage proporciona un completo conjunto de funcionalidades de seguridad que, juntas, ayudan a los desarrolladores a crear aplicaciones seguras. Para más información, consulte la [Guía de seguridad de Azure Storage](../storage-security-guide.md).